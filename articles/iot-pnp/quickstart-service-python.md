---
title: Interagieren mit einem IoT Plug & Play-Gerät, das mit Ihrer Azure IoT-Lösung verbunden ist (Python) | Microsoft-Dokumentation
description: Mithilfe von Python können Sie eine Verbindung mit einem IoT Plug & Play-Gerät herstellen, das mit ihrer Azure IoT-Lösung verbunden ist, und mit ihm interagieren.
author: elhorton
ms.author: elhorton
ms.date: 7/13/2020
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: be5ff3e863752dfc187bd91257425af5e8de85c4
ms.sourcegitcommit: a422b86148cba668c7332e15480c5995ad72fa76
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/30/2020
ms.locfileid: "91574964"
---
# <a name="quickstart-interact-with-an-iot-plug-and-play-device-thats-connected-to-your-solution-python"></a>Schnellstart: Interagieren mit einem IoT Plug & Play-Gerät, das mit Ihrer Lösung verbunden ist (Python)

[!INCLUDE [iot-pnp-quickstarts-service-selector.md](../../includes/iot-pnp-quickstarts-service-selector.md)]

IoT Plug & Play vereinfacht IoT, indem es Ihnen die Interaktion mit dem Modell eines Geräts – ohne Kenntnisse der zugrunde liegenden Geräteimplementierung – ermöglicht. In dieser Schnellstartanleitung erfahren Sie, wie Sie mit Python eine Verbindung mit einem IoT Plug & Play-Gerät herstellen, das mit Ihrer Lösung verbunden ist, und wie Sie dieses Gerät steuern.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [iot-pnp-prerequisites](../../includes/iot-pnp-prerequisites.md)]

Um diese Schnellstartanleitung abzuschließen, benötigen Sie Python 3.7 auf Ihrem Entwicklungscomputer. Sie können die neueste empfohlene Version für mehrere Plattformen von [python.org](https://www.python.org/) herunterladen. Sie können Ihre Python-Version mit dem folgenden Befehl überprüfen:  

```cmd/sh
python --version
```

Das **azure-iot-device**-Paket wird als PIP veröffentlicht.

Installieren Sie das Paket wie folgt in Ihrer lokalen Python-Umgebung:

```cmd/sh
pip install azure-iot-device
```

Installieren Sie das **azure-iot-hub**-Paket durch Ausführen des folgenden Befehls:

```cmd/sh
pip install azure-iot-hub
```

## <a name="run-the-sample-device"></a>Ausführen des Beispielgeräts

[!INCLUDE [iot-pnp-environment](../../includes/iot-pnp-environment.md)]

Weitere Informationen zur Beispielkonfiguration finden Sie in der [Beispiel-Infodatei](https://github.com/Azure/azure-iot-sdk-python/blob/master/azure-iot-device/samples/pnp/README.md).

In dieser Schnellstartanleitung verwenden Sie einen in Python geschriebenen Beispielthermostat als IoT Plug & Play-Gerät. So führen Sie das Beispielgerät aus:

1. Öffnen Sie ein Terminalfenster in einem Ordner Ihrer Wahl. Führen Sie den folgenden Befehl aus, um das GitHub-Repository [Azure IoT Python SDK](https://github.com/Azure/azure-iot-sdk-python) an diesem Speicherort zu klonen:

    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-sdk-python
    ```

1. Dieses Terminalfenster wird als Ihr **Geräteterminal** verwendet. Wechseln Sie zum Ordner Ihres geklonten Repositorys, und navigieren Sie zum Ordner */azure-iot-sdk-python/azure-iot-device/samples/pnp*.

1. Führen Sie den Beispielthermostat mit dem folgenden Befehl aus:

    ```cmd/sh
    python simple_thermostat.py
    ```

1. Es werden Nachrichten darüber angezeigt, dass das Gerät Informationen gesendet und sich selbst als „online“ gemeldet hat. Diese Nachrichten weisen darauf hin, dass das Gerät damit begonnen hat, Telemetriedaten an den Hub zu senden und jetzt bereit zum Empfangen von Befehlen und Eigenschaftsaktualisierungen ist. Schließen Sie dieses Terminal nicht. Sie benötigen es zur Prüfung, ob das Dienstbeispiel funktioniert.

## <a name="run-the-sample-solution"></a>Ausführen der Beispiellösung

In dieser Schnellstartanleitung verwenden Sie eine IoT-Beispiellösung in Python, um mit dem gerade eingerichteten Beispielgerät zu interagieren.

1. Öffnen Sie ein weiteres Terminalfenster zur Verwendung als Ihr **Dienstterminal**. 

1. Navigieren Sie zum Ordner */azure-iot-sdk-python/azure-iot-hub/samples* des geklonten Python SDK-Repositorys.

1. Der Ordner „Samples“ enthält vier Beispieldateien zur Veranschaulichung der Vorgänge bei der Digital Twin Manager-Klasse: *„get_digital_twin_sample.py“, „update_digitial_twin_sample.py“, „invoke_command_sample.py“ und „invoke_component_command_sample.py“* .  Diese Beispiele veranschaulichen, wie die einzelnen APIs zur Interaktion mit IoT Plug & Play-Geräten verwendet werden:

### <a name="get-digital-twin"></a>Abrufen des digitalen Zwillings

In [Einrichten Ihrer Umgebung für die IoT Plug & Play-Schnellstarts und -Tutorials](set-up-environment.md) haben Sie zwei Umgebungsvariablen erstellt, um das Beispiel so zu konfigurieren, dass eine Verbindung mit Ihrem IoT-Hub und -Gerät hergestellt wird:

* **IOTHUB_CONNECTION_STRING**: die IoT-Hub-Verbindungszeichenfolge, die Sie sich zuvor notiert haben.
* **IOTHUB_DEVICE_ID**: `"my-pnp-device"`.

Verwenden Sie den folgenden Befehl im **Dienstterminal**, um dieses Beispiel auszuführen:

```cmd/sh
python get_digital_twin_sample.py
```

Die Ausgabe zeigt den digitalen Zwilling des Geräts und druckt seine Modell-ID:

```cmd/sh
{'$dtId': 'mySimpleThermostat', '$metadata': {'$model': 'dtmi:com:example:Thermostat;1'}}
Model Id: dtmi:com:example:Thermostat;1
```

Der folgende Codeausschnitt zeigt den Beispielcode aus *get_digital_twin_sample.py*:

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

In diesem Beispiel wird gezeigt, wie Sie einen *Patch* verwenden, um Eigenschaften über den digitalen Zwilling Ihres Geräts zu aktualisieren. Der folgende Codeausschnitt aus *update_digital_twin_sample.py* zeigt, wie der Patch erstellt wird:

```python
# If you already have a component thermostat1:
# patch = [{"op": "replace", "path": "/thermostat1/targetTemperature", "value": 42}]
patch = [{"op": "add", "path": "/targetTemperature", "value": 42}]
iothub_digital_twin_manager.update_digital_twin(device_id, patch)
print("Patch has been succesfully applied")
```

Verwenden Sie den folgenden Befehl im **Dienstterminal**, um dieses Beispiel auszuführen:

```cmd/sh
python update_digital_twin_sample.py
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

Zum Aufrufen eines Befehls führen Sie das Beispiel *invoke_command_sample.py* aus. Dieses Beispiel zeigt, wie Sie einen Befehl in einem einfachen Thermostat aufrufen. Bevor Sie dieses Beispiel ausführen, legen Sie die Umgebungsvariablen `IOTHUB_COMMAND_NAME` und `IOTHUB_COMMAND_PAYLOAD` im **Dienstterminal** fest:

```cmd/sh
set IOTHUB_COMMAND_NAME="getMaxMinReport" # this is the relevant command for the thermostat sample
set IOTHUB_COMMAND_PAYLOAD="hello world" # this payload doesn't matter for this sample
```

Verwenden Sie im **Dienstterminal** den folgenden Befehl, um das Beispiel auszuführen:
  
```cmd/sh
python invoke_command_sample.py
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

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie erfahren, wie Sie ein IoT Plug & Play-Gerät mit einer IoT-Lösung verbinden. Weitere Informationen zu IoT Plug & Play-Gerätemodellen finden Sie unter:

> [!div class="nextstepaction"]
> [Anleitung für Entwickler: Modellierung mit IoT Plug & Play](concepts-developer-guide-device-csharp.md)
