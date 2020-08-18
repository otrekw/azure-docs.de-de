---
title: Verbinden von exemplarischem IoT Plug & Play Preview-Komponentengerätecode (Python) mit IoT Hub | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie IoT Plug & Play Preview-Beispielgerätecode (Python) erstellen und ausführen, von dem mehrere Komponenten verwendet werden und eine Verbindung mit einem IoT-Hub hergestellt wird. Verwenden Sie das Tool Azure IoT-Explorer, um die vom Gerät an den Hub gesendeten Informationen anzuzeigen.
author: ericmitt
ms.author: ericmitt
ms.date: 7/14/2020
ms.topic: tutorial
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: 571f0e0ceff0adfbf1814abc627fcab6b23acbe1
ms.sourcegitcommit: 4e5560887b8f10539d7564eedaff4316adb27e2c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2020
ms.locfileid: "87905855"
---
# <a name="tutorial-connect-a-sample-iot-plug-and-play-preview-multiple-component-device-application-to-iot-hub-python"></a>Tutorial: Herstellen einer Verbindung zwischen einer exemplarischen IoT Plug & Play Preview-Geräteanwendung mit mehreren Komponenten und IoT Hub (Python)

[!INCLUDE [iot-pnp-tutorials-device-selector.md](../../includes/iot-pnp-tutorials-device-selector.md)]

In diesem Tutorial erfahren Sie, wie Sie eine exemplarische IoT Plug & Play-Geräteanwendung mit Komponenten und Stammschnittstelle erstellen, mit Ihrem IoT-Hub verbinden und mithilfe von Azure IoT-Explorer die an den Hub gesendeten Informationen anzeigen. Die Beispielanwendung wurde in Python geschrieben und ist im Azure IoT-Geräte-SDK für Python enthalten. Ein Lösungsentwickler kann mithilfe von Azure IoT-Explorer mehr über die Funktionen eines IoT Plug & Play-Geräts erfahren, ohne Gerätecode anzeigen zu müssen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial muss sich Python 3.7 auf Ihrem Entwicklungscomputer befinden. Die neueste empfohlene Version kann für mehrere Plattformen von [python.org](https://www.python.org/) heruntergeladen werden. Sie können Ihre Python-Version mit dem folgenden Befehl überprüfen:  

```cmd/sh
python --version
```

Die neueste empfohlene Version kann für mehrere Plattformen von [python.org](https://www.python.org/) heruntergeladen werden.

### <a name="azure-iot-explorer"></a>Azure IoT-Explorer

Wenn Sie im zweiten Teil dieses Tutorials mit dem Beispielgerät interagieren möchten, verwenden Sie das Tool **Azure IoT-Explorer**. Dazu müssen Sie [die neueste Version von Azure IoT-Explorer für Ihr Betriebssystem herunterladen und installieren](./howto-use-iot-explorer.md).

[!INCLUDE [iot-pnp-prepare-iot-hub.md](../../includes/iot-pnp-prepare-iot-hub.md)]

Führen Sie den folgenden Befehl aus, um die _IoT-Hub-Verbindungszeichenfolge_ für Ihren Hub abzurufen. Notieren Sie sich diese Verbindungszeichenfolge zur späteren Verwendung in diesem Tutorial:

```azurecli-interactive
az iot hub show-connection-string --hub-name <YourIoTHubName> --output table
```

> [!TIP]
> Die IoT-Hub-Verbindungszeichenfolge kann auch per Azure IoT-Explorer ermittelt werden.

Führen Sie den folgenden Befehl aus, um die _Geräteverbindungszeichenfolge_ für das dem Hub hinzugefügte Gerät abzurufen. Notieren Sie sich diese Verbindungszeichenfolge zur späteren Verwendung in diesem Tutorial:

```azurecli-interactive
az iot hub device-identity show-connection-string --hub-name <YourIoTHubName> --device-id <YourDeviceID> --output table
```

[!INCLUDE [iot-pnp-download-models.md](../../includes/iot-pnp-download-models.md)]

## <a name="set-up-your-environment"></a>Einrichten Ihrer Umgebung

Dieses Paket wird als PIP zur Aktualisierung der Public Preview-Version veröffentlicht. Bei der Paketversion muss es sich mindestens um die Version `2.1.4` handeln.

Installieren Sie die Datei wie folgt in Ihrer lokalen Python-Umgebung:

```cmd/sh
pip install azure-iot-device
```

Klonen Sie das Python SDK-IoT-Repository, und checken Sie **pnp-preview-refresh** aus:

```cmd/sh
git clone https://github.com/Azure/azure-iot-sdk-python
```

## <a name="review-the-code"></a>Überprüfen des Codes

In diesem Beispiel wird ein IoT Plug & Play-Temperaturregler implementiert. Das in diesem Beispiel implementierte Modell umfasst [mehrere Komponenten](concepts-components.md). In der [DTDL-Modelldatei (Digital Twins Definition Language) für das Temperaturgerät](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/samples/TemperatureController.json) werden die vom Gerät implementierten Telemetriedaten, Eigenschaften und Befehle definiert.

Der Ordner *azure-iot-sdk-python\azure-iot-device\samples\pnp* enthält den Beispielcode für das IoT Plug & Play-Gerät. Das Beispiel mit dem Temperaturregler umfasst folgende Dateien:

- pnp_temp_controller_with_thermostats.py
- pnp_helper_preview_refresh.py

Der Temperaturregler verfügt über mehrere Komponenten sowie über eine Stammschnittstelle, die auf dem DTDL-Modell des Temperaturreglers basiert.

Öffnen Sie die Datei *pnp_temp_controller_with_thermostats.py* in einem Editor Ihrer Wahl. Der Code in dieser Datei bewirkt Folgendes:

1. Er importiert `pnp_helper_preview_refresh.py`, um Zugriff auf Hilfsmethoden zu erhalten.

2. Er definiert zwei Modellbezeichner für digitale Zwillinge (Digital Twin Model Identifiers, DTMIs), um zwei Schnittstellen, die im DTDL-Modell definiert sind, eindeutig darzustellen. Von den Komponenten in einem echten Temperaturregler müssen die beiden folgenden Schnittstellen implementiert werden. Die beiden Schnittstellen sind bereits in einem zentralen Repository veröffentlicht. Die DTMIs müssen dem Benutzer bekannt sein und unterscheiden sich je nach Geräteimplementierungsszenario. Im vorliegenden Beispiel stellen die beiden Schnittstellen Folgendes dar:

  - Einen Thermostat
  - Von Azure entwickelte Geräteinformationen

3. Er definiert die DTMI-Modell-ID (`model_id`) für das zu implementierende Gerät. Der DTMI ist benutzerdefiniert und muss dem DTMI in der DTDL-Modelldatei entsprechen.

4. Er definiert die Namen, die für die Komponenten in der DTDL-Datei vergeben wurden. Die DTDL enthält zwei Thermostate und eine Geräteinformationskomponente. In der Stammschnittstelle wird außerdem eine Konstante namens `serial_number` definiert. Die Seriennummer (`serial_number`) eines Geräts ist unveränderlich.

5. Er definiert Befehlshandlerimplementierungen. Dadurch wird definiert, wie das Gerät auf eingehende Befehlsanforderungen reagiert.

6. Er definiert Funktionen zum Erstellen einer Befehlsantwort. Dadurch wird definiert, wie das Gerät auf Befehlsanforderungen antwortet. Sie erstellen Befehlsantwortfunktionen, wenn von einem Befehl eine benutzerdefinierte Antwort an den IoT-Hub zurückgesendet werden muss. Wurde für einen Befehl keine Antwortfunktion bereitgestellt, wird eine generische Antwort gesendet. In diesem Beispiel ist nur für den Befehl **getMaxMinReport** eine benutzerdefinierte Antwort vorhanden.

7. Er definiert eine Funktion, um Telemetriedaten von diesem Gerät zu senden. Telemetriedaten werden von beiden Thermostaten sowie von der Stammschnittstelle gesendet. Von dieser Funktion wird ein optionaler Komponentennamenparameter akzeptiert, um die Komponente zu identifizieren, von der die Telemetriedaten gesendet wurden.

8. Er definiert einen Listener für Befehlsanforderungen.

9. Er definiert einen Listener für die Aktualisierung gewünschter Eigenschaften.

10. Er enthält eine Funktion vom Typ `main`, für die Folgendes gilt:

    1. Sie verwendet das Geräte-SDK, um einen Geräteclient zu erstellen und eine Verbindung mit Ihrem IoT-Hub herzustellen. Vom Gerät wird die Modell-ID (`model_id`) gesendet, damit es vom IoT-Hub als IoT Plug & Play-Gerät identifiziert werden kann.

    1. Sie verwendet die Funktion `create_reported_properties` in der Hilfsdatei, um die Eigenschaften zu erstellen. Übergeben Sie den Komponentennamen und die Eigenschaften als Schlüssel-Wert-Paare an diese Funktion.

    1. Sie aktualisiert die lesbaren Eigenschaften für die zugehörigen Komponenten durch Aufrufen von `patch_twin_reported_properties`.

    1. Sie beginnt mit dem Lauschen auf Befehlsanforderungen unter Verwendung der Funktion `execute_command_listener`. Von der Funktion wird ein Listener eingerichtet, um auf Anforderungen des Diensts zu lauschen. Beim Einrichten des Listeners wird Folgendes als Parameter angegeben: `method_name`, `user_command_handler` und `create_user_response_handler`.
        - `method_name` dient zum Definieren der Befehlsanforderung. In diesem Beispiel werden durch das Modell die Befehle **reboot** und **getMaxMinReport** definiert.
        - Die Funktion `user_command_handler` definiert, was das Gerät tun soll, wenn es einen Befehl empfängt.
        - Die Funktion `create_user_response_handler` erstellt eine Antwort, die an Ihren IoT-Hub gesendet wird, wenn ein Befehl erfolgreich ausgeführt wird. Sie können diese Antwort im Portal anzeigen. Ohne Angabe dieser Funktion wird eine generische Antwort an den Dienst gesendet.

    1. Sie verwendet `execute_property_listener`, um auf Eigenschaftsaktualisierungen zu lauschen.

    1. Sie beginnt mit dem Senden von Telemetriedaten unter Verwendung von `send_telemetry`. Im Beispielcode wird eine Schleife verwendet, um drei Funktionen zum Senden von Telemetriedaten aufzurufen. Sie werden jeweils alle acht Sekunden aufgerufen.

    1. Sie deaktiviert alle Listener und Aufgaben und beendet die Schleife, wenn Sie **Q** oder **q** drücken.

Nachdem Sie nun mit dem Code vertraut sind, erstellen Sie als Nächstes eine Umgebungsvariable namens **IOTHUB_DEVICE_CONNECTION_STRING**, um die zuvor notierte Geräteverbindungszeichenfolge zu speichern. Verwenden Sie den folgenden Befehl, um das Beispiel auszuführen:

```cmd/sh
python pnp_temp_controller_with_thermostats.py
```

Das Beispielgerät sendet im Abstand von wenigen Sekunden Telemetrienachrichten an Ihren IoT-Hub.

Die Ausgabe sieht wie folgt aus und gibt an, dass das Gerät Telemetriedaten an den Hub sendet und nun zum Empfangen von Befehlen und Eigenschaftenaktualisierungen bereit ist:

![Bestätigungsmeldungen des Geräts](media/tutorial-multiple-components-python/multiple-component.png)

Behalten Sie die Ausführung des Beispiels während der nächsten Schritte bei.

## <a name="use-azure-iot-explorer-to-validate-the-code"></a>Überprüfen des Codes mithilfe von Azure IoT-Explorer

Überprüfen Sie nach dem Start des Geräteclientbeispiels mithilfe von Azure IoT-Explorer, ob das Beispiel funktioniert.

[!INCLUDE [iot-pnp-iot-explorer.md](../../includes/iot-pnp-iot-explorer.md)]

[!INCLUDE [iot-pnp-clean-resources.md](../../includes/iot-pnp-clean-resources.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie für ein IoT Plug & Play-Gerät mit Komponenten eine Verbindung mit einem IoT-Hub herstellen. Weitere Informationen zu IoT Plug & Play-Gerätemodellen finden Sie hier:

> [!div class="nextstepaction"]
> [Anleitung für Entwickler: Modellierung mit IoT Plug & Play (Vorschau)](concepts-developer-guide.md)
