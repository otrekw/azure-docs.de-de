---
title: 'Tutorial: Verbinden einer generischen Node.js-Client-App mit Azure IoT Central | Microsoft-Dokumentation'
description: In diesem Tutorial für Geräteentwickler erfahren Sie, wie Sie ein Gerät, auf dem eine Node.js-Client-App ausgeführt wird, mit Ihrer Azure IoT Central-Anwendung verbinden. Sie erstellen eine Gerätevorlage, indem Sie ein Gerätefunktionsmodell importieren, und fügen Ansichten hinzu, die es Ihnen ermöglichen, mit einem verbundenen Gerät zu interagieren.
author: dominicbetts
ms.author: dobett
ms.date: 02/26/2020
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.openlocfilehash: 1bcfc949eff0639dd1b4a063687e2c198f480ea3
ms.sourcegitcommit: 5a71ec1a28da2d6ede03b3128126e0531ce4387d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/26/2020
ms.locfileid: "77624336"
---
# <a name="tutorial-create-and-connect-a-nodejs-client-application-to-your-azure-iot-central-application-nodejs"></a>Tutorial: Erstellen einer Node.js-Clientanwendung und Verbinden der Anwendung mit Ihrer Azure IoT Central-Anwendung (Node.js)

In diesem Tutorial für Geräteentwickler erfahren Sie, wie Sie eine Node.js-Clientanwendung mit Ihrer Azure IoT Central-Anwendung verbinden. Die Node.js-Anwendung simuliert das Verhalten eines echten Geräts. Sie verwenden ein exemplarisches _Gerätefunktionsmodell_ für ein Umgebungssensorgerät, um eine _Gerätevorlage_ in IoT Central zu erstellen. Sie fügen der Gerätevorlage Ansichten hinzu, um Gerätetelemetriedaten visualisieren, Geräteeigenschaften verwalten und Befehle zum Steuern Ihrer Geräte verwenden zu können.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Importieren eines Gerätefunktionsmodells zum Erstellen einer Gerätevorlage
> * Hinzufügen von Standardansichten und benutzerdefinierten Ansichten zu einer Gerätevorlage
> * Veröffentlichen einer Gerätevorlage und Hinzufügen eines echten Geräts zu Ihrer IoT Central-Anwendung
> * Erstellen und Ausführen des Node.js-Gerätecodes und Verfolgen der Verbindungsherstellung mit Ihrer IoT Central-Anwendung
> * Anzeigen der simulierten Telemetriedaten, die vom Gerät gesendet werden
> * Verwalten von Geräteeigenschaften mithilfe einer Ansicht
> * Aufrufen von Befehlen zum Steuern des Geräts

## <a name="prerequisites"></a>Voraussetzungen

Damit Sie die in diesem Artikel aufgeführten Schritte ausführen können, benötigen Sie Folgendes:

* Eine Azure IoT Central-Anwendung, die mit der Vorlage **Benutzerdefinierte Anwendung** erstellt wurde. Weitere Informationen finden Sie unter [Schnellstart: Erstellen einer Anwendung](quick-deploy-iot-central.md).
* Einen Entwicklungscomputer, auf dem mindestens die Version 10.0.0 von [Node.js](https://nodejs.org/) installiert ist. Sie können `node --version` in der Befehlszeile ausführen, um Ihre Version zu überprüfen. Node.js ist für eine Vielzahl von Betriebssystemen verfügbar. Bei den Anweisungen in diesem Tutorial wird davon ausgegangen, dass Sie den Befehl **node** an der Windows-Eingabeaufforderung ausführen. Node.js kann unter verschiedenen Betriebssystemen verwendet werden.

## <a name="create-a-device-template"></a>Erstellen einer Gerätevorlage

Erstellen Sie auf Ihrem lokalen Computer einen Ordner namens `environmental-sensor`.

Laden Sie die [JSON-Datei für das Umgebungssensor-Funktionsmodell](https://raw.githubusercontent.com/Azure/IoTPlugandPlay/master/samples/EnvironmentalSensorInline.capabilitymodel.json) herunter, und speichern Sie sie im Ordner `environmental-sensor`.

Öffnen Sie die heruntergeladene Datei `EnvironmentalSensorInline.capabilitymodel.json` in einem Text-Editor, und ersetzen Sie die beiden Instanzen von `{YOUR_COMPANY_NAME_HERE}` durch den Namen Ihres Unternehmens.

Erstellen Sie in Ihrer Azure IoT Central-Anwendung eine Gerätevorlage mit dem Namen *Environmental sensor* (Umgebungssensor), indem Sie die Gerätefunktionsmodell-Datei `EnvironmentalSensorInline.capabilitymodel.json` importieren:

![Gerätevorlage mit importiertem Gerätefunktionsmodell](./media/tutorial-connect-device/device-template.png)

Das Gerätefunktionsmodell enthält zwei Schnittstellen: die Standardschnittstelle **Device Information** (Geräteinformationen) und die benutzerdefinierte Schnittstelle **Environmental Sensor** (Umgebungssensor). Die Schnittstelle **Environmental Sensor** (Umgebungssensor) definiert folgende Funktionen:

| Typ | Anzeigename | Beschreibung |
| ---- | ------------ | ----------- |
| Eigenschaft | Device State (Gerätezustand)     | Der Zustand des Geräts. Mögliche Zustände: online und offline. |
| Eigenschaft | Customer Name (Kundenname)    | Der Name des Kunden, der das Gerät momentan verwendet. |
| Eigenschaft | Brightness Level (Helligkeitsstufe) | Die Helligkeitsstufe für das Licht des Geräts. Mögliche Werte: 1 (hoch), 2 (mittel), 3 (niedrig). |
| Telemetrie | Temperature (Temperatur) | Die aktuelle Temperatur des Geräts. |
| Telemetrie | Humidity (Luftfeuchtigkeit)    | Die aktuelle Luftfeuchtigkeit des Geräts. |
| Befehl | blink          | Lässt die LED für das angegebene Zeitintervall blinken. |
| Befehl | turnon         | Schaltet das LED-Licht des Geräts ein. |
| Befehl | turnoff        | Schaltet das LED-Licht des Geräts aus. |
| Befehl | rundiagnostics | Startet eine Diagnose des Geräts. |

Wählen Sie in der Gerätevorlage die Option **Anpassen** aus, um die Darstellung der Eigenschaft **Device State** (Gerätezustand) in Ihrer IoT Central-Anwendung anzupassen. Erweitern Sie den Eintrag **Device State** (Gerätezustand), und geben Sie unter **Name: TRUE** den Wert _Online_ und unter **Name: FALSE** den Wert _Offline_ ein. Speichern Sie die Änderungen:

![Anpassen der Gerätevorlage](media/tutorial-connect-device/customize-template.png)

## <a name="create-views"></a>Erstellen von Ansichten

Über Ansichten können Sie mit Geräten interagieren, die mit Ihrer IoT Central-Anwendung verbunden sind. So können Sie beispielsweise Ansichten verwenden, die Telemetriedaten oder Eigenschaften anzeigen oder in denen Sie schreibbare und cloudbasierte Eigenschaften bearbeiten können. Ansichten sind Teil einer Gerätevorlage.

Gehen Sie wie folgt vor, um Ihrer Gerätevorlage **Environmental sensor** (Umgebungssensor) einige Standardansichten hinzuzufügen: Navigieren Sie zu Ihrer Gerätevorlage, und wählen Sie **Ansichten** und anschließend die Kachel **Standardansichten generieren** aus. Vergewissern Sie sich, dass **Übersicht** und **Info** auf **Ein** festgelegt sind, und wählen Sie anschließend **Standarddashboardansichten generieren** aus. In Ihrer Vorlage sind nun zwei Standardansichten definiert.

Die Schnittstelle **Environmental Sensor** (Umgebungssensor) enthält zwei schreibbare Eigenschaften: **Customer Name** (Kundenname) und **Brightness Level** (Helligkeitsstufe). Diese Eigenschaften können bearbeitet werden, um eine Ansicht zu erstellen:

1. Wählen Sie **Ansichten** und anschließend die Kachel **Geräte- und Clouddaten bearbeiten** aus.

1. Geben Sie _Properties_ (Eigenschaften) als Formularname ein.

1. Wählen Sie die Eigenschaften **Brightness Level** (Helligkeitsstufe) und **Customer Name** (Kundenname) aus. Wählen Sie anschließend **Abschnitt hinzufügen** aus.

1. Speichern Sie die Änderungen.

![Hinzufügen einer Ansicht, um die Bearbeitung von Eigenschaften zu ermöglichen](media/tutorial-connect-device/properties-view.png)

## <a name="publish-the-template"></a>Veröffentlichen der Vorlage

Bevor Sie Ihrer IoT Central-Anwendung ein Gerät mit der Gerätevorlage **Environmental sensor** (Umgebungssensor) hinzufügen, müssen Sie die Vorlage veröffentlichen.

Wählen Sie in der Gerätevorlage die Option **Veröffentlichen** aus. Wählen Sie im Bereich mit den zu veröffentlichenden Änderungen die Option **Veröffentlichen** aus.

Navigieren Sie in Ihrer IoT Central-Anwendung zur Seite **Geräte**, um zu überprüfen, ob die Vorlage verwendet werden kann. Der Abschnitt **Geräte** enthält eine Liste der veröffentlichten Geräte in der Anwendung:

![Veröffentlichte Vorlagen auf der Seite „Geräte“](media/tutorial-connect-device/published-templates.png)

## <a name="add-a-real-device"></a>Hinzufügen eines echten Geräts

Fügen Sie der Gerätevorlage, die Sie im vorherigen Abschnitt erstellt haben, in Ihrer Azure IoT Central-Anwendung ein echtes Gerät hinzu:

1. Wählen Sie auf der Seite **Geräte** die Gerätevorlage **Environmental Sensor** (Umgebungssensor) aus.

1. Wählen Sie **+ Neu** aus.

1. Stellen Sie sicher, dass **Simuliert** auf **Aus** festgelegt ist. Klicken Sie anschließend auf **Erstellen**.

Klicken Sie auf den Gerätenamen, und wählen Sie anschließend die Option **Verbinden** aus. Notieren Sie sich die auf der Seite **Geräteverbindung** angegebenen Geräteverbindungsinformationen: **ID-Bereich**, **Geräte-ID** und **Primärschlüssel**. Diese Werte werden beim Erstellen des Gerätecodes benötigt:

![Geräteverbindungsinformationen](./media/tutorial-connect-device/device-connection.png)

### <a name="create-a-nodejs-application"></a>Erstellen einer Node.js-Anwendung

In den folgenden Schritten wird eine Node.js-Clientanwendung erstellt, die das echte Gerät implementiert, das Sie der Anwendung hinzugefügt haben. Diese Node.js-Anwendung simuliert das Verhalten eines echten Geräts.

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

1. Fügen Sie der Datei die folgende Funktion hinzu, um Telemetriedaten an Ihre Azure IoT Central-Anwendung zu senden:

    ```javascript
    // Send device measurements.
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

1. Fügen Sie zum Senden von Eigenschaften an Ihre Azure IoT Central-Anwendung folgende Funktion zu Ihrer Datei hinzu:

    ```javascript
    // Send device reported properties.
    function sendDeviceProperties(twin, properties) {
      twin.properties.reported.update(properties, (err) => console.log(`Sent device properties: ${JSON.stringify(properties)}; ` +
        (err ? `error: ${err.toString()}` : `status: success`)));
    }
    ```

1. Fügen Sie den folgenden Code hinzu, um die schreibbaren Eigenschaften zu definieren und zu behandeln, auf die Ihr Gerät reagiert:

    ```javascript
    // Add any writeable properties your device supports,
    // mapped to a function that is called when the setting is changed.
    var settings = {
      'name': (newValue, callback) => {
          setTimeout(() => {
            callback(newValue, 'completed');
          }, 1000);
      },
      'brightness': (newValue, callback) => {
        setTimeout(() => {
            callback(newValue, 'completed');
      }, 5000);
      }
    };

    // Handle writeable property changes that come from Azure IoT Central via the device twin.
    function handleSettings(twin) {
      twin.on('properties.desired', function (desiredChange) {
        for (let setting in desiredChange) {
          if (settings[setting]) {
            console.log(`Received setting: ${setting}: ${desiredChange[setting].value}`);
            settings[setting](desiredChange[setting].value, (newValue, status, message) => {
              var patch = {
                [setting]: {
                  value: newValue,
                  status: status,
                  desiredVersion: desiredChange.$version,
                  message: message
                }
              }
              sendDeviceProperties(twin, patch);
            });
          }
        }
      });
    }
    ```

1. Fügen Sie den folgenden Code hinzu, um die von der IoT Central-Anwendung gesendeten Befehle zu behandeln:

    ```javascript
    // Handle blink command
    function onBlink(request, response) {
      console.log('Received asynchronous call to blink');
      response.send(200, (err) => {
        if (err) {
          console.error('Unable to send method response: ' + err.toString());
        } else {
          console.log('Blinking LED every ' + request.payload  + ' seconds');
        }
      });
    }

    // Handle LED turn on command
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

    // Handle LED turn off command
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

    // Handle sensor diagnostics command
    function diagnostics(request, response) {
      console.log('Received asynchronous call to run diagnostics');
      response.send(200, (err) => {
        if (err) {
          console.error('Unable to send method response: ' + err.toString());
        } else {
          console.log('Running diagnostics...');
        }
      });
    }
    ```

1. Fügen Sie den folgenden Code hinzu, um die Verbindungsherstellung mit Azure IoT Central abzuschließen und die Funktionen im Clientcode einzubinden:

    ```javascript
    // Handle device connection to Azure IoT Central.
    var connectCallback = (err) => {
      if (err) {
        console.log(`Device could not connect to Azure IoT Central: ${err.toString()}`);
      } else {
        console.log('Device successfully connected to Azure IoT Central');
        // Create handlers for commands
        hubClient.onDeviceMethod('blink', onBlink);
        hubClient.onDeviceMethod('turnon', turnOn);
        hubClient.onDeviceMethod('turnoff', turnOff);
        hubClient.onDeviceMethod('rundiagnostics', diagnostics);
        // Send telemetry measurements to Azure IoT Central every 1 second.
        setInterval(sendTelemetry, 1000);
        // Get device twin from Azure IoT Central.
        hubClient.getTwin((err, twin) => {
          if (err) {
            console.log(`Error getting device twin: ${err.toString()}`);
          } else {
            // Send Environmental Sensor device properties once on device start up.
            var properties = {
              state: 'true'
            };
            sendDeviceProperties(twin, properties);
            handleSettings(twin);
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

![Ausführen der Clientanwendung](media/tutorial-connect-device/run-application.png)

Als Operator in Ihrer Azure IoT Central-Anwendung haben Sie folgende Möglichkeiten:

* Anzeigen der vom Gerät gesendeten Telemetriedaten auf der Seite **Übersicht**:

    ![Anzeigen von Telemetriedaten](media/tutorial-connect-device/view-telemetry.png)

* Aktualisieren schreibbarer Eigenschaftswerte auf der Seite **Properties** (Eigenschaften):

    ![Aktualisieren von Eigenschaften](media/tutorial-connect-device/update-properties.png)

    ![Aktualisieren von Geräteeigenschaften](media/tutorial-connect-device/update-properties-device.png)

* Aufrufen der Befehle auf der Seite **Befehle**:

    ![Aufrufen des Befehls „blink“](media/tutorial-connect-device/call-command.png)

    ![Aufrufen des Befehls „blink“ für das Gerät](media/tutorial-connect-device/call-command-device.png)

* Anzeigen der Geräteeigenschaften auf der Seite **Info**:

    ![Anzeigen von Eigenschaften](media/tutorial-connect-device/about-properties.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Gerätefunktionsmodellen und zur Erstellung Ihrer eigenen Gerätevorlagen finden Sie in der folgenden Schrittanleitung:

> [!div class="nextstepaction"]
> [Definieren eines neuen IoT-Gerätetyps](./howto-set-up-template.md)
