---
title: Überprüfen des Erfolgs und Problembehandlung nach der Installation und Bereitstellung
description: Includedatei
services: iot-edge
author: kgremban
ms.service: iot-edge
ms.topic: include
ms.date: 10/06/2020
ms.author: kgremban
ms.custom: include file
ms.openlocfilehash: 295769e5d4924e529b296dbb0b9d405ee197c1db
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2020
ms.locfileid: "91979263"
---
## <a name="verify-successful-setup"></a>Überprüfen der erfolgreichen Installation

Überprüfen Sie den Status des IoT Edge-Diensts. Er sollte als „Wird ausgeführt“ aufgelistet werden.  

# <a name="linux"></a>[Linux](#tab/linux)

```bash
systemctl status iotedge
```

# <a name="windows"></a>[Windows](#tab/windows)

```powershell
Get-Service iotedge
```

---

Untersuchen Sie die Dienstprotokolle.

# <a name="linux"></a>[Linux](#tab/linux)

```bash
journalctl -u iotedge --no-pager --no-full
```

# <a name="windows"></a>[Windows](#tab/windows)

Wenn Sie die Installation der IoT Edge-Runtime gerade beendet haben, wird möglicherweise eine Liste von Fehlern aus der Zeit zwischen den Ausführungen von **Deploy-IoTEdge** und **Initialize-IoTEdge** angezeigt. Diese Fehler werden erwartet, da der Dienst versucht, zu starten, bevor er konfiguriert ist.

```powershell
. {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; Get-IoTEdgeLog
```

---

Führen Sie das [Tool zur Problembehandlung](../articles/iot-edge/troubleshoot.md#run-the-check-command) aus, um nach den häufigsten Konfigurations-und Netzwerkfehlern zu suchen.

```powershell
iotedge check
```

Das Systemmodul **$edgeHub** wird erst auf dem Gerät bereitgestellt, wenn Sie Ihr erstes Modul in IoT Edge auf dem Gerät bereitstellen. Infolgedessen wird bei der automatisierten Überprüfung ein Fehler für die Konnektivitätsprüfung `Edge Hub can bind to ports on host` zurückgegeben. Dieser Fehler kann ignoriert werden – es sei denn, er tritt nach der Bereitstellung eines Moduls auf dem Gerät auf.

Listen Sie abschließend die derzeit ausgeführten Module auf:

```powershell
iotedge list
```

Nach einer Neuinstallation sollte als einziges Modul **edgeAgent** ausgeführt werden.

## <a name="tips-and-troubleshooting"></a>Tipps und Problembehandlung

Auf Geräten mit Ressourceneinschränkungen wird dringend empfohlen, die Umgebungsvariable *OptimizeForPerformance* auf *false* festzulegen, wie im [Leitfaden zur Problembehandlung](../articles/iot-edge/troubleshoot.md) beschrieben.

Wenn Ihr Gerät keine Verbindung mit IoT Hub herstellen kann und Ihr Netzwerk einen Proxyserver enthält, führen Sie die Schritte in [Konfigurieren Ihres IoT Edge-Geräts für die Kommunikation über einen Proxyserver](../articles/iot-edge/how-to-configure-proxy-support.md) aus.

# <a name="linux"></a>[Linux](#tab/linux)

Auf Linux-Geräten benötigen Sie erhöhte Rechte zum Ausführen von `iotedge`-Befehlen. Melden Sie sich der Installation der Runtime von Ihrem Computer ab und anschließend wieder an, um Ihres Berechtigungen automatisch zu aktualisieren. Verwenden Sie bis dahin `sudo`, um Befehle mit erhöhten Rechten auszuführen.

# <a name="windows"></a>[Windows](#tab/windows)

Auf Windows-Geräten, auf denen Windows-Container ausgeführt werden, wurde die Moby-Containerengine als Teil von IoT Edge installiert. Die Moby-Engine wurde für die parallele Ausführung mit Docker Desktop entwickelt. Wenn Sie direkt mit den Containern auf Ihrem Gerät interagieren möchten, können Sie `docker`-Befehle verwenden. Wenn Docker Desktop ebenfalls auf dem Gerät installiert ist, müssen Sie allerdings speziell die Moby-Engine als Ziel auswählen.

Verwenden Sie z. B. den folgenden Befehl, um alle Docker-Images aufzulisten:

```powershell
docker images
```

Ändern Sie denselben Befehl mit einem Zeiger auf die Moby-Engine, um alle Moby-Images aufzulisten:

```powershell
docker -H npipe:////./pipe/iotedge_moby_engine images
```

Die Engine-URI ist in der Ausgabe des Installationsskripts aufgeführt, oder Sie finden sie im Abschnitt für die Einstellungen der Containerruntime der Datei „config.yaml“.

![moby_runtime uri in config.yaml](./media/iot-edge-verify-troubleshoot-install/moby-runtime-uri.png)

---
