---
title: 'Schnellstart: Senden von Gerätetelemetriedaten an Azure IoT Hub (Node.js)'
description: In dieser Schnellstartanleitung verwenden Sie das Azure IoT Hub-Geräte-SDK für Node.js, um Telemetriedaten von einem Gerät an einen IoT Hub zu senden.
author: timlt
ms.author: timlt
ms.service: iot-develop
ms.devlang: node
ms.topic: quickstart
ms.date: 01/11/2021
ms.openlocfilehash: 97fcff4706d6da968c93426f85569fed9af95aac
ms.sourcegitcommit: dda0d51d3d0e34d07faf231033d744ca4f2bbf4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102197808"
---
# <a name="quickstart-send-telemetry-from-a-device-to-an-iot-hub-nodejs"></a>Schnellstart: Senden von Telemetriedaten von einem Gerät an eine IoT Hub-Instanz (Node.js)

**Gilt für**: [Geräteanwendungsentwicklung](about-iot-develop.md#device-application-development)

In dieser Schnellstartanleitung lernen Sie einen einfachen Anwendungsentwicklungsworkflow für IoT-Geräte kennen. Sie verwenden die Azure CLI, um einen Azure IoT Hub und ein simuliertes Gerät zu erstellen. Anschließend verwenden Sie das Node.js SDK von Azure IoT, um auf das Gerät zuzugreifen und Telemetriedaten an den Hub zu senden.

## <a name="prerequisites"></a>Voraussetzungen
- Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), bevor Sie beginnen.
- Azure-Befehlszeilenschnittstelle. Sie können alle Befehle dieser Schnellstartanleitung über die Azure Cloud Shell ausführen. Hierbei handelt es sich um eine interaktive CLI-Shell, die in Ihrem Browser ausgeführt wird. Bei Verwendung der Cloud Shell müssen Sie nichts installieren. Falls Sie die lokale Nutzung der CLI vorziehen, müssen Sie für diese Schnellstartanleitung mindestens Version 2.0.76 der Azure CLI verwenden. Führen Sie „az --version“ aus, um die Version zu ermitteln. Informationen zum Ausführen einer Installation oder eines Upgrades finden Sie unter [Installieren der Azure CLI]( /cli/azure/install-azure-cli).
- [Node.js 10 oder höher](https://nodejs.org). Wenn Sie die Azure Cloud Shell verwenden, aktualisieren Sie die installierte Version von „Node.js“ nicht. Die Azure Cloud Shell hat bereits die neueste Version von „Node.js“.

    Überprüfen Sie mit dem folgenden Befehl die aktuelle Node.js-Version auf Ihrem Entwicklungscomputer:

    ```cmd/sh
        node --version
    ```

[!INCLUDE [iot-hub-include-create-hub-cli](../../includes/iot-hub-include-create-hub-cli.md)]

## <a name="use-the-nodejs-sdk-to-send-messages"></a>Verwenden des Node.js SDK zum Senden von Nachrichten
In diesem Abschnitt verwenden Sie das Node.js SDK zum Senden von Nachrichten vom simulierten Gerät an den IoT Hub. 

1. Öffnen Sie ein neues Terminalfenster. Sie verwenden dieses Terminal, um das Node.js SDK zu installieren und mit Node.js-Beispielcode zu arbeiten. Jetzt sollten zwei Terminals geöffnet sein: das Terminal, das Sie gerade zum Arbeiten mit Node.js geöffnet haben, und die CLI-Shell, die Sie in den vorherigen Abschnitten für die Eingabe von Azure CLI-Befehlen verwendet haben. 

1. Kopieren Sie die [Beispiele für das Node.js-Geräte-SDK von Azure IoT Hub](https://github.com/Azure/azure-iot-sdk-node/tree/master/device/samples) auf Ihren lokalen Computer:

    ```console
    git clone https://github.com/Azure/azure-iot-sdk-node
    ```

1. Navigieren Sie zum Verzeichnis *azure-iot-sdk-node/device/samples*.

    ```console
    cd azure-iot-sdk-node/device/samples
    ```
1. Installieren Sie das Node.js SDK von Azure IoT sowie die erforderlichen Abhängigkeiten:

    ```console
    npm install
    ```
    Mit diesem Befehl werden die richtigen Abhängigkeiten installiert, die in der Datei *package.js* im Verzeichnis der Gerätebeispiele angegeben sind.

1. Legen Sie die Geräteverbindungszeichenfolge als Umgebungsvariable `DEVICE_CONNECTION_STRING` fest. Der zu verwendende Zeichenfolgenwert ist die Zeichenfolge, die Sie im vorherigen Abschnitt abgerufen haben, nachdem Sie das simulierte Node.js-Gerät erstellt haben 

    **Windows (CMD)**

    ```console
    set DEVICE_CONNECTION_STRING=<your connection string here>
    ```

    > [!NOTE]
    > In den Windows-CMD-Befehlen ist die Verbindungszeichenfolge nicht in Anführungszeichen eingeschlossen.

    **Linux (bash)**

    ```bash
    export DEVICE_CONNECTION_STRING="<your connection string here>"
    ```

1. Führen Sie in der geöffneten CLI-Shell den Befehl [az iot hub monitor-events](/cli/azure/ext/azure-iot/iot/hub#ext-azure-iot-az-iot-hub-monitor-events) aus, um mit der Überwachung von Ereignissen auf dem simulierten IoT-Gerät zu beginnen.  Ereignismeldungen werden im Terminal angezeigt, sobald sie eintreffen.

    ```azurecli
    az iot hub monitor-events --output table --hub-name {YourIoTHubName}
    ```

1. Führen Sie in Ihrem Node.js-Terminal den Code für die installierte Beispieldatei *simple_sample_device.js* aus. Dieser Code greift auf das simulierte IoT-Gerät zu und sendet eine Nachricht an den IoT Hub.

    So führen Sie das Node.js-Beispiel aus dem Terminal aus:
    ```console
    node ./simple_sample_device.js
    ```

    Optional können Sie den Node.js-Code aus dem Beispiel in der JavaScript-IDE ausführen:
    ```javascript
    'use strict';

    const Protocol = require('azure-iot-device-mqtt').Mqtt;
    // Uncomment one of these transports and then change it in fromConnectionString to test other transports
    // const Protocol = require('azure-iot-device-amqp').AmqpWs;
    // const Protocol = require('azure-iot-device-http').Http;
    // const Protocol = require('azure-iot-device-amqp').Amqp;
    // const Protocol = require('azure-iot-device-mqtt').MqttWs;
    const Client = require('azure-iot-device').Client;
    const Message = require('azure-iot-device').Message;

    // String containing Hostname, Device Id & Device Key in the following formats:
    //  "HostName=<iothub_host_name>;DeviceId=<device_id>;SharedAccessKey=<device_key>"
    const deviceConnectionString = process.env.DEVICE_CONNECTION_STRING;
    let sendInterval;

    function disconnectHandler () {
    clearInterval(sendInterval);
    client.open().catch((err) => {
        console.error(err.message);
    });
    }

    // The AMQP and HTTP transports have the notion of completing, rejecting or abandoning the message.
    // For example, this is only functional in AMQP and HTTP:
    // client.complete(msg, printResultFor('completed'));
    // If using MQTT calls to complete, reject, or abandon are no-ops.
    // When completing a message, the service that sent the C2D message is notified that the message has been processed.
    // When rejecting a message, the service that sent the C2D message is notified that the message won't be processed by the device. the method to use is client.reject(msg, callback).
    // When abandoning the message, IoT Hub will immediately try to resend it. The method to use is client.abandon(msg, callback).
    // MQTT is simpler: it accepts the message by default, and doesn't support rejecting or abandoning a message.
    function messageHandler (msg) {
    console.log('Id: ' + msg.messageId + ' Body: ' + msg.data);
    client.complete(msg, printResultFor('completed'));
    }

    function generateMessage () {
    const windSpeed = 10 + (Math.random() * 4); // range: [10, 14]
    const temperature = 20 + (Math.random() * 10); // range: [20, 30]
    const humidity = 60 + (Math.random() * 20); // range: [60, 80]
    const data = JSON.stringify({ deviceId: 'myFirstDevice', windSpeed: windSpeed, temperature: temperature, humidity: humidity });
    const message = new Message(data);
    message.properties.add('temperatureAlert', (temperature > 28) ? 'true' : 'false');
    return message;
    }

    function errorCallback (err) {
    console.error(err.message);
    }

    function connectCallback () {
    console.log('Client connected');
    // Create a message and send it to the IoT Hub every two seconds
    sendInterval = setInterval(() => {
        const message = generateMessage();
        console.log('Sending message: ' + message.getData());
        client.sendEvent(message, printResultFor('send'));
    }, 2000);

    }

    // fromConnectionString must specify a transport constructor, coming from any transport package.
    let client = Client.fromConnectionString(deviceConnectionString, Protocol);

    client.on('connect', connectCallback);
    client.on('error', errorCallback);
    client.on('disconnect', disconnectHandler);
    client.on('message', messageHandler);

    client.open()
    .catch(err => {
    console.error('Could not connect: ' + err.message);
    });

    // Helper function to print results in the console
    function printResultFor(op) {
    return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
    };
    }
    ```

Wenn der Node.js-Code eine simulierte Telemetrienachricht von Ihrem Gerät an den IoT Hub sendet, wird die Meldung in der CLI-Shell angezeigt, in der Ereignisse überwacht werden:

```output
event:
  component: ''
  interface: ''
  module: ''
  origin: <your device name>
  payload: '{"deviceId":"myFirstDevice","windSpeed":11.853592092144627,"temperature":22.62484121157508,"humidity":66.17960805575937}'
```

Ihr Gerät ist nun sicher verbunden und sendet Telemetriedaten an Azure IoT Hub.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Falls Sie die in dieser Schnellstartanleitung erstellten Azure-Ressourcen nicht mehr benötigen, können Sie sie mit der Azure CLI löschen.

> [!IMPORTANT]
> Das Löschen einer Ressourcengruppe kann nicht rückgängig gemacht werden. Die Ressourcengruppe und alle darin enthaltenen Ressourcen werden unwiderruflich gelöscht. Achten Sie daher darauf, dass Sie nicht versehentlich die falsche Ressourcengruppe oder die falschen Ressourcen löschen. 

So löschen Sie eine Ressourcengruppe anhand des Namens:
1. Führen Sie den Befehl [az group delete](/cli/azure/group#az-group-delete) aus. Die von Ihnen erstellte Ressourcengruppe, der IoT-Hub und die Geräteregistrierung werden mithilfe dieses Befehls entfernt.

    ```azurecli
    az group delete --name MyResourceGroup
    ```
1. Führen Sie den Befehl [az group list](/cli/azure/group#az-group-list) aus, um sich zu vergewissern, dass die Ressourcengruppe gelöscht wurde.  

    ```azurecli
    az group list
    ```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie einen grundlegenden Workflow für Azure IoT-Anwendungen zum sicheren Verbinden eines Geräts mit der Cloud und zum Senden von Telemetriedaten vom Gerät zur Cloud kennengelernt. Sie verwenden die Azure CLI, um einen IoT Hub und ein simuliertes Gerät zu erstellen. Anschließend verwenden Sie das Node.js SDK von Azure IoT, um auf das Gerät zuzugreifen und Telemetriedaten an den Hub zu senden. 

Im nächsten Schritt erkunden Sie das Azure IoT Node.js SDK mithilfe von Anwendungsbeispielen.

- [Weitere Node.js-Beispiele:](https://github.com/Azure/azure-iot-sdk-node/tree/master/device/samples) Dieses Verzeichnis enthält weitere Beispiele aus dem Node.js SDK-Repository für die Darstellung von IoT Hub-Szenarios.