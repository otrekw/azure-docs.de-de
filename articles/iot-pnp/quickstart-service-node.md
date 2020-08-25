---
title: Interagieren mit einem IoT Plug & Play Preview-Gerät, das mit ihrer Azure IoT-Lösung verbunden ist (Node.js) | Microsoft-Dokumentation
description: Verwenden Sie Node.js zum Herstellen einer Verbindung und zum Interagieren mit einem IoT Plug & Play Preview-Gerät, das mit ihrer Azure IoT-Lösung verbunden ist.
author: elhorton
ms.author: elhorton
ms.date: 08/11/2020
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc, devx-track-javascript
ms.openlocfilehash: fd65dcc9ce0be07daa5848a0ac583cf795150e47
ms.sourcegitcommit: faeabfc2fffc33be7de6e1e93271ae214099517f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2020
ms.locfileid: "88184753"
---
# <a name="quickstart-interact-with-an-iot-plug-and-play-preview-device-thats-connected-to-your-solution-nodejs"></a>Schnellstart: Interagieren mit einem IoT Plug & Play-Gerät (Vorschauversion), das mit Ihrer Lösung (Node.js) verbunden ist

[!INCLUDE [iot-pnp-quickstarts-service-selector.md](../../includes/iot-pnp-quickstarts-service-selector.md)]

IoT Plug & Play Preview vereinfacht das IoT, indem es Ihnen die Interaktion mit den Funktionen eines Geräts ohne Kenntnisse der vorliegenden Geräteimplementierung ermöglicht. In diesem Schnellstart erfahren Sie, wie Sie mit Node.js eine Verbindung mit einem IoT Plug & Play-Gerät, das mit Ihrer Lösung verbunden ist, herstellen und dieses steuern.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Voraussetzungen

Um diesen Schnellstart abzuschließen, benötigen Sie Node.js auf Ihrem Entwicklungscomputer. Sie können die neueste empfohlene Version für mehrere Plattformen von [nodejs.org](https://nodejs.org) herunterladen.

Mit dem folgenden Befehl können Sie die aktuelle Node.js-Version auf Ihrem Entwicklungscomputer überprüfen:

```cmd/sh
node --version
```

[!INCLUDE [iot-pnp-prepare-iot-hub.md](../../includes/iot-pnp-prepare-iot-hub.md)]

Führen Sie den folgenden Befehl aus, um die _IoT-Hub-Verbindungszeichenfolge_ für Ihren Hub abzurufen. Notieren Sie sich diese Verbindungszeichenfolge, die Sie später in diesem Schnellstart verwenden werden:

```azurecli-interactive
az iot hub show-connection-string --hub-name <YourIoTHubName> --output table
```

Führen Sie den folgenden Befehl aus, um die _Geräteverbindungszeichenfolge_ für das dem Hub hinzugefügte Gerät abzurufen. Notieren Sie sich diese Verbindungszeichenfolge zur späteren Verwendung in dieser Schnellstartanleitung:

```azurecli-interactive
az iot hub device-identity show-connection-string --hub-name <YourIoTHubName> --device-id <YourDeviceID> --output
```

### <a name="clone-the-sdk-repository-with-the-sample-code"></a>Klonen des SDK-Repositorys mit dem Beispielcode

Weil sich das Dienst-SDK in der Vorschauphase befindet, müssen Sie die Beispiele aus einem [Vorschaubranch des Node SDK](https://github.com/Azure/azure-iot-sdk-node/tree/pnp-preview-refresh) klonen. Öffnen Sie ein Terminalfenster in einem Ordner Ihrer Wahl. Führen Sie den folgenden Befehl aus, um den Branch **pnp-preview-refresh** des GitHub-Repositorys [Microsoft Azure IoT SDK for Node.js](https://github.com/Azure/azure-iot-sdk-node) zu klonen:

```cmd/sh
git clone https://github.com/Azure/azure-iot-sdk-node -b pnp-preview-refresh
```

## <a name="run-the-sample-device"></a>Ausführen des Beispielgeräts

In diesem Schnellstart können Sie ein Beispielthermostat verwenden, das in Node.js als IoT Plug & Play-Gerät geschrieben wird. So führen Sie das Beispielgerät aus:

1. Öffnen Sie ein Terminalfenster, und navigieren Sie zu dem lokalen Ordner mit dem Microsoft Azure IoT SDK for Node.js, das Sie auf GitHub geklont haben.

1. Dieses Terminalfenster wird als Ihr **Geräteterminal** verwendet. Wechseln Sie zum Ordner Ihres geklonten Repositorys, und navigieren Sie zum Ordner */azure-iot-sdk-node/device/samples/pnp*. Installieren Sie alle Abhängigkeiten, indem Sie den folgenden Befehl ausführen:

    ```cmd/sh
    npm install
    ```

1. Konfigurieren Sie die _Geräte-Verbindungszeichenfolge_:

    ```cmd/sh
    set IOTHUB_DEVICE_CONNECTION_STRING=<YourDeviceConnectionString>
    ```

1. Führen Sie den Beispielthermostat mit dem folgenden Befehl aus:

    ```cmd/sh
    node simple_thermostat.js
    ```

1. Es werden Nachrichten darüber angezeigt, dass das Gerät Informationen gesendet und sich selbst als „online“ gemeldet hat. Diese Nachrichten weisen darauf hin, dass das Gerät damit begonnen hat, Telemetriedaten an den Hub zu senden und jetzt bereit zum Empfangen von Befehlen und Eigenschaftsaktualisierungen ist. Schließen Sie dieses Terminal nicht. Sie benötigen es zur Prüfung, ob das Dienstbeispiel funktioniert.

## <a name="run-the-sample-solution"></a>Ausführen der Beispiellösung

In diesem Schnellstart verwenden Sie eine Beispiel-IoT-Lösung in Node.js für die Interaktion mit dem gerade eingerichteten Beispielgerät.

1. Öffnen Sie ein weiteres Terminalfenster zur Verwendung als Ihr **Dienstterminal**. Weil sich das Dienst-SDK in der Vorschau befindet, müssen Sie die Beispiele aus einem [Vorschaubranch des Knoten SDKs](https://github.com/Azure/azure-iot-sdk-node/tree/pnp-preview-refresh) klonen:

    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-sdk-node -b pnp-preview-refresh
    ```

1. Wechseln Sie zum Ordner dieses geklonten Repository-Branches, und navigieren Sie zum Ordner */azure-iot-sdk-node/digitaltwins/samples/service/javascript*. Installieren Sie alle Abhängigkeiten, indem Sie den folgenden Befehl ausführen:

    ```cmd/sh
    npm install
    ```

1. Konfigurieren Sie Umgebungsvariablen für Ihre Geräte-ID und Ihre _IoT Hub-Verbindungszeichenfolge_:

    ```cmd/sh
    set IOTHUB_CONNECTION_STRING=<YourIOTHubConnectionString>
    set IOTHUB_DEVICE_ID=<Your device ID>
    ```

### <a name="read-a-property"></a>Lesen einer Eigenschaft

1. Bei Ihrer Ausführung des Beispielthermostats im **Geräteterminal** wurden folgende Nachrichten zur Mitteilung von dessen Onlinestatus angezeigt:

    ```cmd/sh
    properties have been reported for component
    sending telemetry message 0...
    ```

1. Wechseln Sie zum **Dienstterminal**, und führen Sie den folgenden Befehl aus, um das Beispiel für das Lesen von Geräteinformationen auszuführen:

    ```cmd/sh
    node get_digital_twin.js
    ```

1. Beachten Sie in der Ausgabe des **Dienstterminals** die Antwort des digitalen Zwillings. Die Modell-ID des Geräts und die zugehörigen Eigenschaften werden angezeigt:

    ```json
    "$dtId": "mySimpleThermostat",
    "serialNumber": "123abc",
    "maxTempSinceLastReboot": 51.96167432818655,
    "$metadata": {
      "$model": "dtmi:com:example:Thermostat;1",
      "serialNumber": { "lastUpdateTime": "2020-07-09T14:04:00.6845182Z" },
      "maxTempSinceLastReboot": { "lastUpdateTime": "2020-07-09T14:04:00.6845182" }
    }
    ```

1. Der folgende Codeausschnitt zeigt den Code in *get_digital_twin.js*, mit dem die Modell-ID des Gerätezwillings abgerufen wird:

    ```javascript
    console.log("Model Id: " + inspect(digitalTwin.$metadata.$model))
    ```

In diesem Szenario wird `Model Id: dtmi:com:example:Thermostat;1` ausgegeben.

### <a name="update-a-writable-property"></a>Aktualisieren einer schreibbaren Eigenschaft

1. Öffnen Sie die Datei *update_digital_twin.js* in einem Code-Editor.

1. Überprüfen Sie den Beispielcode. Sie können sehen, wie ein JSON-Patch zum Aktualisieren des digitalen Zwillings Ihres Geräts erstellt wird. In diesem Beispiel ersetzt der Code die Temperatur des Thermostats durch den Wert „42“:

    ```javascript
    const patch = [{
        op: 'add',
        path: '/targetTemperature',
        value: '42'
      }]
    ```

1. Verwenden Sie im **Dienstterminal** den folgenden Befehl zur Ausführung des Beispiels zum Aktualisieren der Eigenschaft:

    ```cmd/sh
    node update_digital_twin.js
    ```

1. In Ihrem **Geräteterminal** sehen Sie, dass das Gerät die Aktualisierung erhalten hat:

    ```cmd/sh
    The following properties will be updated for root interface:
    {
      targetTemperature: {
        value: 42,
        ac: 200,
        ad: 'Successfully executed patch for targetTemperature',
        av: 2
      }
    }
    updated the property
    Properties have been reported for component
    ```

1. Führen Sie in Ihrem **Dienstterminal** den folgenden Befehl zur Bestätigung aus, dass die-Eigenschaft aktualisiert wurde:

    ```cmd/sh
    node get_digital_twin.js
    ```

1. In der Ausgabe des **Dienstterminals** wird in der Antwort des digitalen Zwillings unter der Komponente `thermostat1` die aktualisierte Zieltemperatur angezeigt. Es kann eine Weile dauern, bis das Gerät die Aktualisierung abgeschlossen hat. Wiederholen Sie diesen Schritt so oft, bis das Gerät die Eigenschaftsaktualisierung verarbeitet hat:

    ```json
    targetTemperature: 42,
    ```

### <a name="invoke-a-command"></a>Aufrufen eines Befehls

1. Öffnen Sie die Datei *invoke_command.js*, und überprüfen Sie den Code.

1. Wechseln Sie zum **Dienstterminal**. Verwenden Sie den folgenden Befehl, um das Beispiel für das Aufrufen des Befehls auszuführen:

    ```cmd/sh
    set IOTHUB_COMMAND_NAME=getMaxMinReport
    set IOTHUB_COMMAND_PAYLOAD=commandpayload
    node invoke_command.js
    ```

1. Die Ausgabe im **Dienstterminal** zeigt die folgende Bestätigung:

    ```cmd/sh
    {
        xMsCommandStatuscode: 200,  
        xMsRequestId: 'ee9dd3d7-4405-4983-8cee-48b4801fdce2',  
        connection: 'close',  'content-length': '18',  
        'content-type': 'application/json; charset=utf-8',  
        date: 'Thu, 09 Jul 2020 15:05:14 GMT',  
        server: 'Microsoft-HTTPAPI/2.0',  vary: 'Origin',  
        body: 'min/max response'
    }
    ```

1. Im **Geräteterminal** sehen Sie, dass der Befehl bestätigt wird:

    ```cmd/sh
    MaxMinReport commandpayload
    Response to method 'getMaxMinReport' sent successfully.
    ```

[!INCLUDE [iot-pnp-clean-resources.md](../../includes/iot-pnp-clean-resources.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie erfahren, wie Sie ein IoT Plug & Play-Gerät mit einer IoT-Lösung verbinden. Weitere Informationen zu IoT Plug & Play-Gerätemodellen finden Sie unter:

> [!div class="nextstepaction"]
> [Anleitung für Entwickler: Modellierung mit IoT Plug & Play (Vorschau)](concepts-developer-guide.md)
