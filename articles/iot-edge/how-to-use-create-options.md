---
title: 'Schreiben von createOptions für Module: Azure IoT Edge | Microsoft-Dokumentation'
description: Verwenden von createOptions im Bereitstellungsmanifest zum Konfigurieren von Modulen zur Laufzeit
keywords: ''
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 04/01/2020
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 055b89858fde901ab014e409fbe30c3438efce12
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101732988"
---
# <a name="how-to-configure-container-create-options-for-iot-edge-modules"></a>Konfigurieren von Erstellungsoptionen für Container für IoT Edge-Module

Mit dem Parameter **createOptions** im Bereitstellungsmanifest können Sie die Modulcontainer zur Laufzeit konfigurieren. Dieser Parameter erweitert Ihre Kontrolle über die Module und ermöglicht Aufgaben wie das Gewähren oder Einschränken des Zugriffs auf die Ressourcen des Hostgeräts durch das Modul oder das Konfigurieren des Netzwerks.

IoT Edge-Module werden als Docker-kompatible Container auf Ihrem IoT Edge-Gerät implementiert. Docker bietet viele Optionen zum Erstellen von Containern, und diese Optionen treffen auch für IoT Edge-Module zu. Weitere Informationen finden Sie unter [Optionen für die Docker-Containererstellung](https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate).

## <a name="format-create-options"></a>Formatieren der Erstellungsoptionen

Das IoT Edge-Bereitstellungsmanifest akzeptiert Erstellungsoptionen, die als JSON formatiert sind. Beispiel: die Erstellungsoptionen, die für jedes edgeHub-Modul automatisch enthalten sind:

```json
"createOptions": {
  "HostConfig": {
    "PortBindings": {
      "5671/tcp": [
        {
          "HostPort": "5671"
        }
      ],
      "8883/tcp": [
        {
          "HostPort": "8883"
        }
      ],
      "443/tcp": [
        {
          "HostPort": "443"
        }
      ]
    }
  }
}
```

In diesem edgeHub-Beispiel wird der Parameter **HostConfig.PortBindings** verwendet, um verfügbar gemachte Ports im Container einem Port auf dem Hostgerät zuzuordnen.

Wenn Sie die Azure IoT Tools-Erweiterungen für Visual Studio oder Visual Studio Code verwenden, können Sie die Erstellungsoptionen im JSON-Format in die Datei **deployment.template.json** schreiben. Wenn Sie dann mithilfe der Erweiterung die IoT Edge-Lösung erstellen oder das Bereitstellungsmanifest generieren, wird der JSON-Code als Zeichenfolgen in dem von der IoT Edge-Runtime erwarteten Format ausgegeben. Beispiel:

```json
"createOptions": "{\"HostConfig\":{\"PortBindings\":{\"5671/tcp\":[{\"HostPort\":\"5671\"}],\"8883/tcp\":[{\"HostPort\":\"8883\"}],\"443/tcp\":[{\"HostPort\":\"443\"}]}}}"
```

Ein Tipp zum Schreiben von Erstellungsoptionen ist die Verwendung des Befehls `docker inspect`. Führen Sie das Modul im Rahmen Ihres Entwicklungsprozesses lokal mithilfe von `docker run <container name>` aus. Wenn das Modul auf die gewünschte Weise funktioniert, führen Sie `docker inspect <container name>` aus. Dieser Befehl gibt die Moduldetails im JSON-Format aus. Suchen Sie die konfigurierten Parameter, und kopieren Sie den JSON-Code. Beispiel:

[![Ergebnisse von „docker inspect edgeHub“](./media/how-to-use-create-options/docker-inspect-edgehub-inline-and-expanded.png)](./media/how-to-use-create-options/docker-inspect-edgehub-inline-and-expanded.png#lightbox)

## <a name="common-scenarios"></a>Häufige Szenarios

Containererstellungsoptionen ermöglichen viele Szenarien, im Folgenden finden Sie nur einige der häufigsten beim Erstellen von IoT Edge-Lösungen:

* [Gewähren des Zugriffs auf den Hostspeicher für Module](how-to-access-host-storage-from-module.md)
* [Zuordnen eines Hostports zu einem Modulport](#map-host-port-to-module-port)
* [Einschränken des Modularbeitsspeichers und der CPU-Nutzung](#restrict-module-memory-and-cpu-usage)

### <a name="map-host-port-to-module-port"></a>Zuordnen eines Hostports zu einem Modulport

Wenn Ihr Modul mit einem Dienst außerhalb der IoT Edge-Lösung kommunizieren muss und dazu kein Nachrichtenrouting verwendet, müssen Sie einem Modulport einen Hostport zuordnen.

>[!TIP]
>Diese Portzuordnung ist für die Kommunikation zwischen Modulen auf demselben Gerät nicht erforderlich. Wenn Modul A eine API abfragen muss, die in Modul B gehostet wird, kann dies ohne Portzuordnung erfolgen. Modul B muss einen Port in seinem Dockerfile verfügbar machen, z. B. `EXPOSE 8080`. Anschließend kann Modul A die API über den Namen von Modul B abfragen, z. B. `http://ModuleB:8080/api`.

Stellen Sie zunächst sicher, dass im Modul ein Port verfügbar ist, um auf Verbindungen zu lauschen. Hierzu können Sie eine [EXPOSE](https://docs.docker.com/engine/reference/builder/#expose)-Anweisung im Dockerfile verwenden. Beispiel: `EXPOSE 8080`. Die EXPOSE-Anweisung verwendet standardmäßig das TCP-Protokoll, wenn keines angegeben ist, oder Sie können UDP angeben.

Verwenden Sie dann die Einstellung **PortBindings** in der Gruppe **HostConfig** der [Erstellungsoptionen für Docker-Container](https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate), um den verfügbar gemachten Port im Modul einem Port auf dem Hostgerät zuzuordnen. Wenn Sie z. B. Port 8080 im Modul verfügbar gemacht haben und diesen Port 80 des Hostgeräts zuordnen möchten, ähneln die Erstellungsoptionen in der Datei „template.json“ dem folgenden Beispiel:

```json
"createOptions": {
  "HostConfig": {
    "PortBindings": {
      "8080/tcp": [
        {
          "HostPort": "80"
        }
      ]
    }
  }
}
```

In Form von Zeichenfolgen für das Bereitstellungsmanifest sieht die gleiche Konfiguration wie das folgende Beispiel aus:

```json
"createOptions": "{\"HostConfig\":{\"PortBindings\":{\"8080/tcp\":[{\"HostPort\":\"80\"}]}}}"
```

### <a name="restrict-module-memory-and-cpu-usage"></a>Einschränken von Modularbeitsspeicher und CPU-Nutzung

Sie können deklarieren, welchen Anteil der Hostressourcen ein Modul verwenden kann. Diese Kontrolle ist hilfreich, um sicherzustellen, dass ein Modul nicht zu viel Arbeitsspeicher oder CPU-Nutzung beanspruchen und die Ausführung anderer Prozesse auf dem Gerät verhindern kann. Sie können diese Einstellungen mit den [Erstellungsoptionen für Docker-Container](https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate) in der Gruppe **HostConfig** verwalten, einschließlich:

* **Arbeitsspeicher**: Arbeitsspeicherlimit in Byte. Beispiel: 268.435.456 Byte = 256 MB
* **MemorySwap:** Gesamtspeicherlimit (Arbeitsspeicher + Swap). Beispiel: 536.870.912 Byte = 512 MB
* **CpuPeriod:** Die Länge eines CPU-Zeitraums in Mikrosekunden. Der Standardwert ist 100.000, sodass beispielsweise ein Wert von 25.000 einen Container auf 25 % der CPU-Ressourcen beschränkt.

Im Format für „template.json“ sehen diese Werte wie im folgenden Beispiel aus:

```json
"createOptions": {
  "HostConfig": {
    "Memory": 268435456,
    "MemorySwap": 536870912,
    "CpuPeriod": 25000
  }
}
```

In Form von Zeichenfolgen für das abschließende Bereitstellungsmanifest sehen diese Werte wie das folgende Beispiel aus:

```json
"createOptions":"{\"HostConfig\":{\"Memory\":268435456,\"MemorySwap\":536870912,\"CpuPeriod\":25000}}"
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Beispiele für angewandte Erstellungsoptionen finden Sie in den folgenden IoT Edge-Beispielen:

* [Custom Vision und Azure IoT Edge auf einem Raspberry Pi 3](https://github.com/Azure-Samples/custom-vision-service-iot-edge-raspberry-pi)
* [Azure IoT Edge-Blobspeicherbeispiel](https://github.com/Azure-Samples/azure-iotedge-blobstorage-sample)
