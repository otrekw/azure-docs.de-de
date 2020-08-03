---
title: Interagieren mit einem IoT Plug & Play Preview-Gerät, das mit Ihrer Azure IoT-Lösung verbunden ist (Python) | Microsoft-Dokumentation
description: Verwenden Sie Python zum Herstellen einer Verbindung und zum Interagieren mit einem IoT Plug & Play Preview-Gerät, das mit Ihrer Azure IoT-Lösung verbunden ist.
author: elhorton
ms.author: elhorton
ms.date: 7/13/2020
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: e3f00bb601cce17721c5247941588be1c2de788d
ms.sourcegitcommit: 46f8457ccb224eb000799ec81ed5b3ea93a6f06f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87352653"
---
# <a name="quickstart-interact-with-an-iot-plug-and-play-preview-device-thats-connected-to-your-solution-python"></a>Schnellstart: Interagieren mit einem IoT Plug & Play Preview-Gerät, das mit Ihrer Lösung verbunden ist (Python)

[!INCLUDE [iot-pnp-quickstarts-service-selector.md](../../includes/iot-pnp-quickstarts-service-selector.md)]

IoT Plug & Play Preview vereinfacht das IoT, indem es Ihnen die Interaktion mit dem Modell eines Geräts ohne Kenntnisse der vorliegenden Geräteimplementierung ermöglicht. In dieser Schnellstartanleitung erfahren Sie, wie Sie mit Python eine Verbindung mit einem IoT Plug & Play-Gerät herstellen, das mit Ihrer Lösung verbunden ist, und wie Sie dieses Gerät steuern.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Voraussetzungen

Um diese Schnellstartanleitung abzuschließen, benötigen Sie Python 3.7 auf Ihrem Entwicklungscomputer. Sie können die neueste empfohlene Version für mehrere Plattformen von [python.org](https://www.python.org/) herunterladen. Sie können Ihre Python-Version mit dem folgenden Befehl überprüfen:  

```cmd/sh
python --version
```

Installieren Sie das [Vorschaupaket für das Python-Dienst-SDK](https://pypi.org/project/azure-iot-hub/2.2.1rc0/), indem Sie den folgenden Befehl ausführen:

```cmd/sh
pip3 install azure-iot-hub==2.2.1rc0
```

[!INCLUDE [iot-pnp-prepare-iot-hub.md](../../includes/iot-pnp-prepare-iot-hub.md)]

Führen Sie den folgenden Befehl aus, um die _IoT-Hub-Verbindungszeichenfolge_ für Ihren Hub abzurufen. Notieren Sie sich diese Verbindungszeichenfolge, die Sie später in diesem Schnellstart verwenden werden:

```azurecli-interactive
az iot hub show-connection-string --hub-name <YourIoTHubName> --output table
```

Führen Sie den folgenden Befehl aus, um die _Geräteverbindungszeichenfolge_ für das dem Hub hinzugefügte Gerät abzurufen. Notieren Sie sich diese Verbindungszeichenfolge, die Sie später in diesem Schnellstart verwenden werden:

```azurecli-interactive
az iot hub device-identity show-connection-string --hub-name <YourIoTHubName> --device-id <YourDeviceID> --output
```

## <a name="run-the-sample-device"></a>Ausführen des Beispielgeräts

In dieser Schnellstartanleitung verwenden Sie einen in Python geschriebenen Beispielthermostat als IoT Plug & Play-Gerät. So führen Sie das Beispielgerät aus:

1. Öffnen Sie ein Terminalfenster in einem Ordner Ihrer Wahl. Führen Sie den folgenden Befehl aus, um das GitHub-Repository [Azure IoT Python SDK](https://github.com/Azure/azure-iot-sdk-python) an diesem Speicherort zu klonen:

    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-sdk-python
    ```

1. Dieses Terminalfenster wird als Ihr **Geräteterminal** verwendet. Wechseln Sie zum Ordner Ihres geklonten Repositorys, und navigieren Sie zum Ordner */azure-iot-sdk-python/azure-iot-device/samples/pnp*.

1. Konfigurieren Sie die _Geräte-Verbindungszeichenfolge_:

    ```cmd/sh
    set IOTHUB_DEVICE_CONNECTION_STRING=<YourDeviceConnectionString>
    ```

1. Führen Sie den Beispielthermostat mit dem folgenden Befehl aus:

    ```cmd/sh
    python pnp_thermostat.py
    ```

1. Es werden Nachrichten darüber angezeigt, dass das Gerät Informationen gesendet und sich selbst als „online“ gemeldet hat. Diese Nachrichten weisen darauf hin, dass das Gerät damit begonnen hat, Telemetriedaten an den Hub zu senden und jetzt bereit zum Empfangen von Befehlen und Eigenschaftsaktualisierungen ist. Schließen Sie dieses Terminal nicht. Sie benötigen es zur Prüfung, ob das Dienstbeispiel funktioniert.

## <a name="run-the-sample-solution"></a>Ausführen der Beispiellösung

In dieser Schnellstartanleitung verwenden Sie eine IoT-Beispiellösung in Python, um mit dem gerade eingerichteten Beispielgerät zu interagieren.

1. Öffnen Sie ein weiteres Terminalfenster zur Verwendung als Ihr **Dienstterminal**. Weil sich das Dienst-SDK in der Vorschau befindet, müssen Sie die Beispiele aus einem [Vorschaubranch des Python SDKs](https://github.com/Azure/azure-iot-sdk-python/tree/pnp-preview-refresh) klonen:

    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-sdk-python -b pnp-preview-refresh
    ```

1. Wechseln Sie zum Ordner dieses geklonten Repositorybranches, und navigieren Sie zum Ordner */azure-iot-sdk-python/azure-iot-hub/samples*.

1. Konfigurieren Sie Umgebungsvariablen für Ihre Geräte-ID und Ihre _IoT Hub-Verbindungszeichenfolge_:

    ```cmd/sh
    set IOTHUB_CONNECTION_STRING=<YourIOTHubConnectionString>
    set IOTHUB_DEVICE_ID=<Your device ID>
    ```

1. Der Beispielordner enthält vier Beispieldateien mit dem Präfix `pnp`. Diese Beispiele veranschaulichen, wie die einzelnen APIs zur Interaktion mit IoT Plug & Play-Geräten verwendet werden:

### <a name="get-digital-twin"></a>Abrufen des digitalen Zwillings

Verwenden Sie den folgenden Befehl im **Dienstterminal**, um dieses Beispiel auszuführen:

```cmd/sh
python pnp_get_digital_twin_sample.py
```

Die Ausgabe zeigt den digitalen Zwilling des Geräts und druckt seine Modell-ID:

```cmd/sh
{'$dtId': 'mySimpleThermostat', '$metadata': {'$model': 'dtmi:com:example:Thermostat;1'}}
Model Id: dtmi:com:example:Thermostat;1
```

Der folgende Codeausschnitt zeigt den Beispielcode aus *pnp_get_digital_twin_sample.py*:

```python
    # Get digital twin and retrieve the modelId from it
    digital_twin = iothub_digital_twin_manager.get_digital_twin(device_id)
    if digital_twin:
        print(digital_twin)
        print("Model Id: " + digital_twin["$metadata"]["$model"])
    else:
        print("No digital_twin found")
```

### <a name="update-a-digital-twin"></a>Aktualisieren eines digitalen Zwillings

In diesem Beispiel wird gezeigt, wie Sie einen *Patch* verwenden, um Eigenschaften über den digitalen Zwilling Ihres Geräts zu aktualisieren. Der folgende Codeausschnitt aus *pnp_update_digital_twin_sample.py* zeigt, wie der Patch erstellt wird:

```python
# If you already have a component thermostat1:
# patch = [{"op": "replace", "path": "/thermostat1/targetTemperature", "value": 42}]
patch = [{"op": "add", "path": "/targetTemperature", "value": 42}]
iothub_digital_twin_manager.update_digital_twin(device_id, patch)
print("Patch has been succesfully applied")
```

Verwenden Sie den folgenden Befehl im **Dienstterminal**, um dieses Beispiel auszuführen:

```cmd/sh
python pnp_update_digital_twin_sample.py
```

Sie können die Aktualisierung im **Geräteterminal** überprüfen, das die folgende Ausgabe anzeigt:

```cmd/sh
the data in the desired properties patch was: {'targetTemperature': 42, '$version': 2}
previous values
42
```

Das **Dienstterminal** bestätigt, dass der Patch erfolgreich war:

```cmd/sh
Patch has been successfully applied
```

### <a name="invoke-a-command"></a>Aufrufen eines Befehls

Zum Aufrufen eines Befehls führen Sie das Beispiel *pnp_invoke_command_sample.py* aus. Dieses Beispiel zeigt, wie Sie einen Befehl in einem einfachen Thermostat aufrufen. Bevor Sie dieses Beispiel ausführen, legen Sie die Umgebungsvariablen `IOTHUB_COMMAND_NAME` und `IOTHUB_COMMAND_PAYLOAD` im **Dienstterminal** fest:

```cmd/sh
set IOTHUB_COMMAND_NAME="getMaxMinReport" # this is the relevant command for the thermostat sample
set IOTHUB_COMMAND_PAYLOAD="hello world" # this payload doesn't matter for this sample
```

Verwenden Sie im **Dienstterminal** den folgenden Befehl, um das Beispiel auszuführen:
  
```cmd/sh
python pnp_invoke_command_sample.py
```

Das **Dienstterminal** zeigt eine Bestätigungsmeldung vom Gerät an:

```cmd/sh
{"tempReport": {"avgTemp": 34.5, "endTime": "13/07/2020 16:03:38", "maxTemp": 49, "minTemp": 11, "startTime": "13/07/2020 16:02:18"}}
```

Im **Geräteterminal** sehen Sie, dass das Gerät den Befehl empfängt:

```cmd/sh
Command request received with payload
hello world
Will return the max, min and average temperature from the specified time hello to the current time
Done generating
{"tempReport": {"avgTemp": 34.2, "endTime": "09/07/2020 09:58:11", "maxTemp": 49, "minTemp": 10, "startTime": "09/07/2020 09:56:51"}}
Sent message
```

[!INCLUDE [iot-pnp-clean-resources.md](../../includes/iot-pnp-clean-resources.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie erfahren, wie Sie ein IoT Plug & Play-Gerät mit einer IoT-Lösung verbinden. Weitere Informationen zu IoT Plug & Play-Gerätemodellen finden Sie unter:

> [!div class="nextstepaction"]
> [Anleitung für Entwickler: Modellierung mit IoT Plug & Play (Vorschau)](concepts-developer-guide.md)
