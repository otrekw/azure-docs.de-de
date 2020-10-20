---
title: Interagieren mit einem IoT Plug & Play-Gerät, das mit Ihrer Azure IoT-Lösung verbunden ist (Python) | Microsoft-Dokumentation
description: Mithilfe von Python können Sie eine Verbindung mit einem IoT Plug & Play-Gerät herstellen, das mit ihrer Azure IoT-Lösung verbunden ist, und mit ihm interagieren.
author: elhorton
ms.author: elhorton
ms.date: 10/05/2020
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: d04a1eda7dc414233075f5d70e29c967c8bdfc35
ms.sourcegitcommit: ba7fafe5b3f84b053ecbeeddfb0d3ff07e509e40
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/12/2020
ms.locfileid: "91946075"
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

1. Öffnen Sie die Datei *registry_manager_pnp_sample.py*, und sehen Sie sich den Code an. In diesem Beispiel wird veranschaulicht, wie Sie die **IoTHubRegistryManager**-Klasse zum Interagieren mit Ihrem IoT Plug & Play-Gerät verwenden.

> [!NOTE]
> In diesen Dienstbeispielen wird die **IoTHubRegistryManager**-Klasse aus dem **IoT Hub-Dienstclient** verwendet. Weitere Informationen zu den APIs, einschließlich der Digital Twins-API, finden Sie im [Leitfaden für Dienstentwickler](concepts-developer-guide-service.md).

### <a name="get-the-device-twin"></a>Abrufen des Gerätezwillings

In [Einrichten Ihrer Umgebung für die IoT Plug & Play-Schnellstarts und -Tutorials](set-up-environment.md) haben Sie zwei Umgebungsvariablen erstellt, um das Beispiel so zu konfigurieren, dass eine Verbindung mit Ihrem IoT-Hub und -Gerät hergestellt wird:

* **IOTHUB_CONNECTION_STRING**: die IoT-Hub-Verbindungszeichenfolge, die Sie sich zuvor notiert haben.
* **IOTHUB_DEVICE_ID**: `"my-pnp-device"`.

Verwenden Sie den folgenden Befehl im **Dienstterminal**, um dieses Beispiel auszuführen:

```cmd/sh
set IOTHUB_METHOD_NAME="getMaxMinReport"
set IOTHUB_METHOD_PAYLOAD="hello world"
python registry_manager_pnp_sample.py
```

> [!NOTE]
> Verwenden Sie `export` anstelle von `set`, wenn Sie dieses Beispiel unter Linux ausführen.

In der Ausgabe sind der Gerätezwilling und die zugehörige Modell-ID enthalten:

```cmd/sh
The Model ID for this device is:
dtmi:com:example:Thermostat;1
```

Im folgenden Codeausschnitt ist der Beispielcode aus *registry_manager_pnp_sample.py* enthalten:

```python
    # Create IoTHubRegistryManager
    iothub_registry_manager = IoTHubRegistryManager(iothub_connection_str)

    # Get device twin
    twin = iothub_registry_manager.get_twin(device_id)
    print("The device twin is: ")
    print("")
    print(twin)
    print("")

    # Print the device's model ID
    additional_props = twin.additional_properties
    if "modelId" in additional_props:
        print("The Model ID for this device is:")
        print(additional_props["modelId"])
        print("")
```

### <a name="update-a-device-twin"></a>Aktualisieren eines Gerätezwillings

In diesem Beispiel wird veranschaulicht, wie Sie die schreibbare Eigenschaft `targetTemperature` auf dem Gerät aktualisieren:

```python
    # Update twin
    twin_patch = Twin()
    twin_patch.properties = TwinProperties(
        desired={"targetTemperature": 42}
    )  # this is relevant for the thermostat device sample
    updated_twin = iothub_registry_manager.update_twin(device_id, twin_patch, twin.etag)
    print("The twin patch has been successfully applied")
    print("")
```

Sie können die Aktualisierung im **Geräteterminal** überprüfen, das die folgende Ausgabe anzeigt:

```cmd/sh
the data in the desired properties patch was: {'targetTemperature': 42, '$version': 2}
```

Das **Dienstterminal** bestätigt, dass der Patch erfolgreich war:

```cmd/sh
The twin patch has been successfully applied
```

### <a name="invoke-a-command"></a>Aufrufen eines Befehls

Im Beispiel wird dann ein Befehl aufgerufen:

Das **Dienstterminal** zeigt eine Bestätigungsmeldung vom Gerät an:

```cmd/sh
The device method has been successfully invoked
```

Im **Geräteterminal** sehen Sie, dass das Gerät den Befehl empfängt:

```cmd/sh
Command request received with payload
hello world
Will return the max, min and average temperature from the specified time hello to the current time
Done generating
{"tempReport": {"avgTemp": 34.2, "endTime": "09/07/2020 09:58:11", "maxTemp": 49, "minTemp": 10, "startTime": "09/07/2020 09:56:51"}}
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie erfahren, wie Sie ein IoT Plug & Play-Gerät mit einer IoT-Lösung verbinden. Weitere Informationen zu IoT Plug & Play-Gerätemodellen finden Sie unter:

> [!div class="nextstepaction"]
> [Anleitung für Entwickler: Modellierung mit IoT Plug & Play](concepts-developer-guide-device-csharp.md)
