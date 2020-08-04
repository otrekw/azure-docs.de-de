---
title: 'Tutorial: Verbinden einer generischen Node.js-Client-App mit Azure IoT Central | Microsoft-Dokumentation'
description: In diesem Tutorial für Geräteentwickler erfahren Sie, wie Sie ein Gerät, auf dem eine Node.js-Client-App ausgeführt wird, mit Ihrer Azure IoT Central-Anwendung verbinden. Sie erstellen eine Gerätevorlage, indem Sie ein Gerätefunktionsmodell importieren, und fügen Ansichten hinzu, die es Ihnen ermöglichen, mit einem verbundenen Gerät zu interagieren.
author: dominicbetts
ms.author: dobett
ms.date: 07/07/2020
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mqtt
ms.openlocfilehash: e20ab44f309fd9ff7f2d6d9b1ad2a4ca0bfa3223
ms.sourcegitcommit: 46f8457ccb224eb000799ec81ed5b3ea93a6f06f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87336088"
---
# <a name="tutorial-create-and-connect-a-client-application-to-your-azure-iot-central-application-nodejs"></a>Tutorial: Erstellen einer Clientanwendung und Verbinden der Anwendung mit Ihrer Azure IoT Central-Anwendung (Node.js)

[!INCLUDE [iot-central-selector-tutorial-connect](../../../includes/iot-central-selector-tutorial-connect.md)]

*Dieser Artikel richtet sich an Lösungs- und Geräteentwickler.*

In diesem Tutorial für Geräteentwickler erfahren Sie, wie Sie eine Node.js-Clientanwendung mit Ihrer Azure IoT Central-Anwendung verbinden. Diese Node.js-Anwendung simuliert das Verhalten eines Umgebungssensorgeräts. Sie verwenden ein exemplarisches _Gerätefunktionsmodell_, um in IoT Central eine _Gerätevorlage_ zu erstellen. Sie fügen der Gerätevorlage Ansichten hinzu, um einem Bediener die Interaktion mit einem Gerät zu ermöglichen.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Importieren eines Gerätefunktionsmodells zum Erstellen einer Gerätevorlage
> * Hinzufügen von Standardansichten und benutzerdefinierten Ansichten zu einer Gerätevorlage
> * Veröffentlichen einer Gerätevorlage und Hinzufügen eines echten Geräts zu Ihrer IoT Central-Anwendung
> * Erstellen und Ausführen des Node.js-Gerätecodes und Verfolgen der Verbindungsherstellung mit Ihrer IoT Central-Anwendung
> * Zeigen Sie die simulierten Telemetriedaten an, die vom Gerät gesendet werden.
> * Verwalten von Geräteeigenschaften mithilfe einer Ansicht
> * Rufen Sie synchrone und asynchrone Befehle auf, um das Gerät zu steuern.

## <a name="prerequisites"></a>Voraussetzungen

Damit Sie die in diesem Artikel aufgeführten Schritte ausführen können, benötigen Sie Folgendes:

* Eine Azure IoT Central-Anwendung, die mit der Vorlage **Benutzerdefinierte Anwendung** erstellt wurde. Weitere Informationen finden Sie unter [Schnellstart: Erstellen einer Anwendung](quick-deploy-iot-central.md). Die Anwendung muss am 14.07.2020 oder nach diesem Datum erstellt worden sein.
* Einen Entwicklungscomputer, auf dem mindestens die Version 10.0.0 von [Node.js](https://nodejs.org/) installiert ist. Sie können `node --version` in der Befehlszeile ausführen, um Ihre Version zu überprüfen. Bei den Anweisungen in diesem Tutorial wird davon ausgegangen, dass Sie den Befehl **node** an der Windows-Eingabeaufforderung ausführen. Node.js kann jedoch unter einer Reihe anderer Betriebssysteme verwendet werden.

[!INCLUDE [iot-central-add-environmental-sensor](../../../includes/iot-central-add-environmental-sensor.md)]

### <a name="create-a-nodejs-application"></a>Erstellen einer Node.js-Anwendung

In den folgenden Schritten wird eine Node.js-Clientanwendung erstellt, die sich mit dem echten Gerät verbindet, das Sie der Anwendung hinzugefügt haben. Diese Node.js-Anwendung simuliert das Verhalten eines echten Geräts.

1. Navigieren Sie in Ihrer Befehlszeilenumgebung zum zuvor erstellten Ordner `environmental-sensor`.

1. Führen Sie die folgenden Befehle aus, um das Node.js-Projekt zu initialisieren und die erforderlichen Abhängigkeiten zu installieren. Übernehmen Sie beim Ausführen von `npm init` alle Standardoptionen:

    ```cmd/sh
    npm init
    npm install azure-iot-device azure-iot-device-mqtt azure-iot-provisioning-device-mqtt azure-iot-security-symmetric-key --save
    ```

1. Erstellen Sie im Ordner `environmental-sensor` eine Datei mit dem Namen **environmentalSensor.js**.

1. Fügen Sie am Anfang der Datei **environmentalSensor.js** die folgenden `require`-Anweisungen hinzu:

    ```javascript
    "use strict";

    // Use the Azure IoT device SDK for devices that connect to Azure IoT Central.
    var iotHubTransport = require('azure-iot-device-mqtt').Mqtt;
    var Client = require('azure-iot-device').Client;
    var Message = require('azure-iot-device').Message;
    var ProvisioningTransport = require('azure-iot-provisioning-device-mqtt').Mqtt;
    var SymmetricKeySecurityClient = require('azure-iot-security-symmetric-key').SymmetricKeySecurityClient;
    var ProvisioningDeviceClient = require('azure-iot-provisioning-device').ProvisioningDeviceClient;
    ```

1. Fügen Sie der Datei folgende Variablendeklarationen hinzu:

    ```javascript
    var provisioningHost = 'global.azure-devices-provisioning.net';
    var idScope = '{your Scope ID}';
    var registrationId = '{your Device ID}';
    var symmetricKey = '{your Primary Key}';
    var provisioningSecurityClient = new SymmetricKeySecurityClient(registrationId, symmetricKey);
    var provisioningClient = ProvisioningDeviceClient.create(provisioningHost, idScope, new ProvisioningTransport(), provisioningSecurityClient);
    var hubClient;

    var targetTemperature = 0;
    var ledOn = true;
    ```

    Aktualisieren Sie die Platzhalter `{your Scope ID}`, `{your Device ID}` und `{your Primary Key}` auf die Werte, die Sie sich zuvor notiert haben. In diesem Beispiel wird `targetTemperature` mit null initialisiert, wobei Sie auch die aktuelle Ablesung vom Gerät oder den Wert vom Gerätezwilling übernehmen können.

1. Fügen Sie der Datei die folgende Funktion hinzu, um simulierte Telemetriedaten an Ihre Azure IoT Central-Anwendung zu senden:

    ```javascript
    // Send simulated device telemetry.
    function sendTelemetry() {
      var temp = targetTemperature + (Math.random() * 15);
      var humid = 70 + (Math.random() * 10);
      var data = JSON.stringify({
        temp: temp,
        humid: humid,
        });
      var message = new Message(data);
      hubClient.sendEvent(message, (err, res) => console.log(`Sent message: ${message.getData()}` +
        (err ? `; error: ${err.toString()}` : '') +
        (res ? `; status: ${res.constructor.name}` : '')));
    }
    ```

    Die Namen der Telemetrieelemente (`temp` und `humid`) müssen mit den in der Gerätevorlage verwendeten Namen übereinstimmen.

1. Fügen Sie zum Senden von Eigenschaften eines Gerätezwillings an Ihre Azure IoT Central-Anwendung folgende Funktion zu Ihrer Datei hinzu:

    ```javascript
    // Send device twin reported properties.
    function sendDeviceProperties(twin, properties) {
      twin.properties.reported.update(properties, (err) => console.log(`Sent device properties: ${JSON.stringify(properties)}; ` +
        (err ? `error: ${err.toString()}` : `status: success`)));
    }
    ```

    IoT Central verwendet Gerätezwillinge, um Eigenschaftswerte zwischen dem Gerät und der IoT Central-Anwendung zu synchronisieren. Die gemeldeten Eigenschaften von Gerätezwillingen werden für die Eigenschaftswerte eines Geräts verwendet. Für schreibbare Eigenschaften werden sowohl die gemeldeten als auch die gewünschten Eigenschaften eines Gerätezwillings verwendet.

1. Fügen Sie den folgenden Code hinzu, um die schreibbaren Eigenschaften zu definieren und zu behandeln, auf die Ihr Gerät reagiert. Die Meldung, die das Gerät als Reaktion auf die [schreibbare Eigenschaftsaktualisierung](concepts-telemetry-properties-commands.md#writeable-property-types) sendet, muss die Felder `av` und `ac` enthalten. Das Feld `ad` ist optional:

    ```javascript
    // Add any writeable properties your device supports,
    // mapped to a function that's called when the writeable property
    // is updated in the IoT Central application.
    var writeableProperties = {
      'name': (newValue, callback) => {
          setTimeout(() => {
            callback(newValue, 'completed', 200);
          }, 1000);
      },
      'brightness': (newValue, callback) => {
        setTimeout(() => {
            callback(newValue, 'completed', 200);
        }, 5000);
      }
    };

    // Handle writeable property updates that come from IoT Central via the device twin.
    function handleWriteablePropertyUpdates(twin) {
      twin.on('properties.desired', function (desiredChange) {
        for (let setting in desiredChange) {
          if (writeableProperties[setting]) {
            console.log(`Received setting: ${setting}: ${desiredChange[setting]}`);
            writeableProperties[setting](desiredChange[setting], (newValue, status, code) => {
              var patch = {
                [setting]: {
                  value: newValue,
                  ad: status,
                  ac: code,
                  av: desiredChange.$version
                }
              }
              sendDeviceProperties(twin, patch);
            });
          }
        }
      });
    }
    ```

    Wenn der Bediener eine beschreibbare Eigenschaft in der IoT Central-Anwendung festlegt, verwendet die Anwendung eine vom Gerätezwilling gewünschte Eigenschaft, um den Wert an das Gerät zu senden. Das Gerät antwortet dann mit einer gemeldeten Eigenschaft des Gerätezwillings. Wenn IoT Central den gemeldeten Eigenschaftswert empfängt, wird die Eigenschaftsansicht mit dem Status **Synchronisiert** aktualisiert.

    Die Namen der Eigenschaften (`name` und `brightness`) müssen mit den in der Gerätevorlage verwendeten Namen übereinstimmen.

1. Fügen Sie den folgenden Code hinzu, um die von der IoT Central-Anwendung gesendeten Befehle zu behandeln:

    ```javascript
    // Setup command handlers
    function setupCommandHandlers(twin) {

      // Handle synchronous LED blink command with request and response payload.
      function onBlink(request, response) {
        console.log('Received synchronous call to blink');
        var responsePayload = {
          status: 'Blinking LED every ' + request.payload  + ' seconds'
        }
        response.send(200, responsePayload, (err) => {
          if (err) {
            console.error('Unable to send method response: ' + err.toString());
          } else {
            console.log('Blinking LED every ' + request.payload  + ' seconds');
          }
        });
      }

      // Handle synchronous LED turn on command
      function turnOn(request, response) {
        console.log('Received synchronous call to turn on LED');
        if(!ledOn){
          console.log('Turning on the LED');
          ledOn = true;
        }
        response.send(200, (err) => {
          if (err) {
            console.error('Unable to send method response: ' + err.toString());
          }
        });
      }

      // Handle synchronous LED turn off command
      function turnOff(request, response) {
        console.log('Received synchronous call to turn off LED');
        if(ledOn){
          console.log('Turning off the LED');
          ledOn = false;
        }
        response.send(200, (err) => {
          if (err) {
            console.error('Unable to send method response: ' + err.toString());
          }
        });
      }

      // Handle asynchronous sensor diagnostics command with response payload.
      function diagnostics(request, response) {
        console.log('Starting asynchronous diagnostics run...');
        response.send(202, (err) => {
          if (err) {
            console.error('Unable to send method response: ' + err.toString());
          } else {
            var repetitions = 3;
            var intervalID = setInterval(() => {
              console.log('Generating diagnostics...');
              if (--repetitions === 0) {
                clearInterval(intervalID);
                var properties = {
                  rundiagnostics: {
                    value: 'Diagnostics run complete at ' + new Date().toLocaleString()
                  }
                };
                sendDeviceProperties(twin, properties);
              }
            }, 2000);
          }
        });
      }

      hubClient.onDeviceMethod('blink', onBlink);
      hubClient.onDeviceMethod('turnon', turnOn);
      hubClient.onDeviceMethod('turnoff', turnOff);
      hubClient.onDeviceMethod('rundiagnostics', diagnostics);
    }
    ```

    Die Namen der Befehle (`blink`, `turnon`, `turnoff` und `rundiagnostics`) müssen mit den in der Gerätevorlage verwendeten Namen übereinstimmen.

    Derzeit verwendet IoT Central nicht das im Gerätefunktionsmodell definierte Antwortschema. Bei einem synchronen Befehl kann die Antwortnutzlast beliebiger gültiger JSON-Code sein. Bei einem asynchronen Befehl muss das Gerät sofort eine Antwort des Typs 202 zurückgeben, gefolgt von einer gemeldeten Eigenschaftsaktualisierung, sobald die Aufgabe erledigt ist. Das Format der gemeldeten Eigenschaftsaktualisierung ist wie folgt:

    ```json
    {
      [command name] : {
        value: 'response message'
      }
    }
    ```

    Ein Bediener kann die Antwortnutzlast im Befehlsverlauf einsehen.

1. Fügen Sie den folgenden Code hinzu, um die Verbindungsherstellung mit Azure IoT Central abzuschließen und die Funktionen im Clientcode einzubinden:

    ```javascript
    // Handle device connection to Azure IoT Central.
    var connectCallback = (err) => {
      if (err) {
        console.log(`Device could not connect to Azure IoT Central: ${err.toString()}`);
      } else {
        console.log('Device successfully connected to Azure IoT Central');

        // Send telemetry to Azure IoT Central every 1 second.
        setInterval(sendTelemetry, 1000);

        // Get device twin from Azure IoT Central.
        hubClient.getTwin((err, twin) => {
          if (err) {
            console.log(`Error getting device twin: ${err.toString()}`);
          } else {
            // Send device properties once on device start up.
            var properties = {
              state: 'true',
              processorArchitecture: 'ARM',
              swVersion: '1.0.0'
            };
            sendDeviceProperties(twin, properties);

            handleWriteablePropertyUpdates(twin);

            setupCommandHandlers(twin);
          }
        });
      }
    };

    // Start the device (register and connect to Azure IoT Central).
    provisioningClient.register((err, result) => {
      if (err) {
        console.log('Error registering device: ' + err);
      } else {
        console.log('Registration succeeded');
        console.log('Assigned hub=' + result.assignedHub);
        console.log('DeviceId=' + result.deviceId);
        var connectionString = 'HostName=' + result.assignedHub + ';DeviceId=' + result.deviceId + ';SharedAccessKey=' + symmetricKey;
        hubClient = Client.fromConnectionString(connectionString, iotHubTransport);

        hubClient.open(connectCallback);
      }
    });
    ```

## <a name="run-your-nodejs-application"></a>Ausführen der Node.js-Anwendung

Geben Sie in Ihrer Befehlszeilenumgebung den folgenden Befehl ein, um die Geräteclientanwendung zu starten:

```cmd/sh
node environmentalSensor.js
```

Wie Sie sehen, stellt das Gerät eine Verbindung mit Ihrer Azure IoT Central-Anwendung her und beginnt mit dem Senden von Telemetriedaten:

![Ausführen der Clientanwendung](media/tutorial-connect-device-nodejs/run-application.png)

[!INCLUDE [iot-central-monitor-environmental-sensor](../../../includes/iot-central-monitor-environmental-sensor.md)]

Sie können sehen, wie das Gerät auf Befehle und Eigenschaftsaktualisierungen reagiert:

![Beobachten der Clientanwendung](media/tutorial-connect-device-nodejs/run-application-2.png)

## <a name="view-raw-data"></a>Anzeigen von Rohdaten

[!INCLUDE [iot-central-monitor-environmental-sensor-raw-data](../../../includes/iot-central-monitor-environmental-sensor-raw-data.md)]

## <a name="next-steps"></a>Nächste Schritte

Als Geräteentwickler haben Sie hier die Grundlagen zur Erstellung eines Geräts mithilfe von Node.js kennengelernt. Vorschläge für die nächsten Schritte:

* Unter [Was sind Gerätevorlagen?](./concepts-device-templates.md) erfahren Sie mehr über die Rolle von Gerätevorlagen beim Implementieren Ihres Gerätecodes.
* Weitere Informationen dazu, wie Sie Geräte bei IoT Central registrieren und wie IoT Central Geräteverbindungen schützt, finden Sie unter [Herstellen einer Verbindung mit Azure IoT Central](./concepts-get-connected.md).

Wenn Sie mit den Tutorials zu IoT Central fortfahren und mehr über das Erstellen einer IoT Central-Lösung erfahren möchten, lesen Sie den folgenden Artikel:

> [!div class="nextstepaction"]
> [Erstellen einer Gatewaygerätevorlage](./tutorial-define-gateway-device-type.md)
