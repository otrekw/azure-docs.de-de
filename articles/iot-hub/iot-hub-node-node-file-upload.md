---
title: Hochladen von Dateien von Geräten nach Azure IoT Hub mit Node | Microsoft-Dokumentation
description: Informationen zum Hochladen von Dateien von einem Gerät in die Cloud mithilfe des Azure IoT-Geräte-SDK für Node.js Hochgeladene Dateien werden in einem Azure Storage-Blobcontainer gespeichert.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 06/28/2017
ms.custom: mqtt, devx-track-js
ms.openlocfilehash: b94cd4cc5c76495cb08f743b1a6849b6eb6c77b2
ms.sourcegitcommit: 190658142b592db528c631a672fdde4692872fd8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2021
ms.locfileid: "112005931"
---
# <a name="upload-files-from-your-device-to-the-cloud-with-iot-hub-nodejs"></a>Hochladen von Dateien von Ihrem Gerät in die Cloud mit IoT Hub (Node.js)

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

Das Tutorial veranschaulicht folgende Vorgehensweisen:

* Sicheres Bereitstellen eines Geräts mit einem Azure-Blob-URI für das Hochladen einer Datei.

* Verwenden der IoT Hub-Dateihochlade-Benachrichtigungen zum Auslösen der Dateiverarbeitung in Ihrem App-Back-End.

Der Schnellstart [Senden von Telemetriedaten von einem Gerät an einen IoT-Hub](quickstart-send-telemetry-node.md) veranschaulicht die grundlegenden Gerät-zu-Cloud-Messagingfunktionen von IoT Hub. In einigen Szenarien können Sie allerdings nicht einfach die Daten, die Ihre Geräte senden, den relativ kleinen D2C-Nachrichten zuordnen, die IoT Hub akzeptiert. Beispiel:

* Große Dateien, die Bilder enthalten
* Videos
* Vibrationsdaten, die mit hoher Häufigkeit als Stichproben erfasst werden
* Eine Form vorverarbeiteter Daten.

Diese Dateien werden normalerweise als Batch in der Cloud mit Tools wie [Azure Data Factory](../data-factory/introduction.md) oder dem [Hadoop](../hdinsight/index.yml)-Stapel verarbeitet. Wenn Sie Dateien von einem Gerät hochladen müssen, können Sie weiterhin die Sicherheit und Zuverlässigkeit des IoT Hub nutzen.

Am Ende dieses Tutorials führen Sie zwei Node.js-Konsolen-Apps aus:

* **FileUpload.js** lädt mithilfe eines SAS-URI, den Ihr IoT-Hub bereitstellt, eine Datei in den Speicher hoch.

* **ReadFileUploadNotification.js** empfängt Benachrichtigungen zum Hochladen von Dateien von Ihrem IoT-Hub.

> [!NOTE]
> IoT Hub bietet über Azure IoT-Geräte-SDKs Unterstützung für zahlreiche Geräteplattformen und Sprachen (u.a. C, .NET, Javascript, Python und Java). Im [Azure IoT Developer Center] finden Sie Schritt-für-Schritt-Anweisungen zum Verbinden eines Geräts mit Azure IoT Hub.

[!INCLUDE [iot-hub-include-x509-ca-signed-file-upload-support-note](../../includes/iot-hub-include-x509-ca-signed-file-upload-support-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

* Node.js, Version 10.0.x oder höher. Unter [Prepare your development environment](https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md) (Vorbereiten Ihrer Entwicklungsumgebung) wird beschrieben, wie Sie Node.js für dieses Tutorial unter Windows oder Linux installieren.

* Ein aktives Azure-Konto. (Wenn Sie nicht über ein Konto verfügen, können Sie in nur wenigen Minuten ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen.)

* Stellen Sie sicher, dass der Port 8883 in Ihrer Firewall geöffnet ist. Das Beispielgerät in diesem Artikel verwendet das MQTT-Protokoll, das über Port 8883 kommuniziert. In einigen Netzwerkumgebungen von Unternehmen oder Bildungseinrichtungen ist dieser Port unter Umständen blockiert. Weitere Informationen und Problemumgehungen finden Sie unter [Herstellen einer Verbindung mit IoT Hub (MQTT)](iot-hub-mqtt-support.md#connecting-to-iot-hub).

## <a name="create-an-iot-hub"></a>Erstellen eines IoT-Hubs

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]

## <a name="upload-a-file-from-a-device-app"></a>Hochladen einer Datei von einer Geräte-App

In diesem Abschnitt kopieren Sie die Geräte-App von GitHub, um eine Datei in den IoT-Hub hochzuladen.

1. Auf GitHub stehen zwei Beispiele für Dateiuploads für Node.js zur Verfügung: ein einfaches und ein komplexeres Beispiel. Kopieren Sie das einfache Beispiel aus [diesem Repository](https://github.com/Azure/azure-iot-sdk-node/blob/master/device/samples/upload_to_blob.js). Das komplexere Beispiel finden Sie [hier](https://github.com/Azure/azure-iot-sdk-node/blob/master/device/samples/upload_to_blob_advanced.js).   

2. Erstellen Sie einen leeren Ordner mit dem Namen ```fileupload```.  Erstellen Sie im Ordner ```fileupload``` die Datei „package.json“, indem Sie an der Eingabeaufforderung den unten angegebenen Befehl ausführen.  Übernehmen Sie alle Standardeinstellungen:

    ```cmd/sh
    npm init
    ```

3. Führen Sie an der Eingabeaufforderung im Ordner ```fileupload``` den folgenden Befehl aus, um das Geräte-SDK-Paket **azure-iot-device** und das Paket **azure-iot-device-mqtt** zu installieren:

    ```cmd/sh
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

4. Erstellen Sie im Ordner ```fileupload``` mithilfe eines Text-Editors eine Datei vom Typ **FileUpload.js**, und kopieren Sie das einfache Beispiel in den Ordner.

    ```javascript
    // Copyright (c) Microsoft. All rights reserved.
    // Licensed under the MIT license. See LICENSE file in the project root for full license information.

    'use strict';

    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    var Client = require('azure-iot-device').Client;
    var fs = require('fs');

    var deviceConnectionString = process.env.ConnectionString;
    var filePath = process.env.FilePath;

    var client = Client.fromConnectionString(deviceConnectionString, Protocol);
    fs.stat(filePath, function (err, fileStats) {
      var fileStream = fs.createReadStream(filePath);

      client.uploadToBlob('testblob.txt', fileStream, fileStats.size, function (err, result) {
        if (err) {
          console.error('error uploading file: ' + err.constructor.name + ': ' + err.message);
        } else {
          console.log('Upload successful - ' + result);
        }
        fileStream.destroy();
      });
    });
    ```

5. Fügen Sie Umgebungsvariablen für Ihre Geräteverbindungszeichenfolge und den Pfad zur Datei hinzu, die Sie hochladen möchten.

6. Speichern und schließen Sie die Datei **FileUpload.js**.

7. Kopieren Sie eine Bilddatei in den Ordner `fileupload`, und geben Sie ihr einen Namen, etwa `myimage.png`. Platzieren Sie den Pfad zur Datei in der Umgebungsvariablen `FilePath`.

## <a name="get-the-iot-hub-connection-string"></a>Abrufen der IoT-Hub-Verbindungszeichenfolge

In diesem Artikel erstellen Sie einen Back-End-Dienst, um Dateiuploadbenachrichtigungen von dem IoT-Hub zu erhalten, den Sie erstellt haben. Damit Ihr Dienst Dateiuploadbenachrichtigungen empfangen kann, muss er über die Berechtigung **Dienstverbindung** verfügen. Standardmäßig wird jeder IoT-Hub mit einer SAS-Richtlinie namens **service** erstellt, die diese Berechtigung erteilt.

[!INCLUDE [iot-hub-include-find-service-connection-string](../../includes/iot-hub-include-find-service-connection-string.md)]

## <a name="receive-a-file-upload-notification"></a>Erhalten einer Benachrichtigung zum Dateiupload

In diesem Abschnitt erstellen Sie eine Node.js-Konsolen-App, die Dateiuploadbenachrichtigungen von IoT Hub empfängt.

Sie können diesen Abschnitt mit der Verbindungszeichenfolge **iothubowner** aus Ihrer IoT Hub-Instanz abschließen. Die Verbindungszeichenfolge finden Sie im [Azure-Portal](https://portal.azure.com/) auf dem Blatt **SAS-Richtlinie**.

1. Erstellen Sie einen leeren Ordner mit dem Namen ```fileuploadnotification```.  Erstellen Sie im Ordner ```fileuploadnotification``` die Datei „package.json“, indem Sie an der Eingabeaufforderung den unten angegebenen Befehl ausführen.  Übernehmen Sie alle Standardeinstellungen:

    ```cmd/sh
    npm init
    ```

2. Führen Sie an der Eingabeaufforderung im Ordner ```fileuploadnotification``` den folgenden Befehl aus, um das SDK-Paket **azure-iothub** zu installieren:

    ```cmd/sh
    npm install azure-iothub --save
    ```

3. Erstellen Sie mit einem Text-Editor eine **FileUploadNotification.js**-Datei im Ordner `fileuploadnotification`.

4. Fügen Sie am Anfang der Datei **FileUploadNotification.js** die folgenden `require`-Anweisungen hinzu:

    ```javascript
    'use strict';

    var Client = require('azure-iothub').Client;
    ```

5. Fügen Sie die Variable `iothubconnectionstring` hinzu, und verwenden Sie sie zum Erstellen einer **Client**-Instanz.  Ersetzen Sie den Platzhalterwert `{iothubconnectionstring}` durch die IoT-Hub-Verbindungszeichenfolge, die Sie zuvor unter [Abrufen der IoT-Hub-Verbindungszeichenfolge](#get-the-iot-hub-connection-string) kopiert haben:

    ```javascript
    var connectionString = '{iothubconnectionstring}';
    ```

    > [!NOTE]
    > Der Einfachheit halber ist die Verbindungszeichenfolge im Code enthalten: Dies ist jedoch kein empfohlenes Verfahren. Abhängig von Ihrem Anwendungsfall und der Architektur sollten sie unter Umständen sicherere Methoden zum Speichern dieses Geheimnisses in Betracht ziehen.

6. Fügen Sie den folgenden Code hinzu, um eine Verbindung mit dem Client herzustellen:

    ```javascript
    var serviceClient = Client.fromConnectionString(connectionString);
    ```

7. Öffnen Sie den Client, und verwenden Sie die **getFileNotificationReceiver**-Funktion zum Empfangen von Statusaktualisierungen.

    ```javascript
    serviceClient.open(function (err) {
      if (err) {
        console.error('Could not connect: ' + err.message);
      } else {
        console.log('Service client connected');
        serviceClient.getFileNotificationReceiver(function receiveFileUploadNotification(err, receiver){
          if (err) {
            console.error('error getting the file notification receiver: ' + err.toString());
          } else {
            receiver.on('message', function (msg) {
              console.log('File upload from device:')
              console.log(msg.getData().toString('utf-8'));
            });
          }
        });
      }
    });
    ```

8. Speichern und schließen Sie die Datei **FileUploadNotification.js**.

## <a name="run-the-applications"></a>Ausführen der Anwendungen

Sie können jetzt die Anwendung ausführen.

Führen Sie an der Eingabeaufforderung im `fileuploadnotification`-Ordner folgenden Befehl aus:

```cmd/sh
node FileUploadNotification.js
```

Führen Sie an der Eingabeaufforderung im `fileupload`-Ordner folgenden Befehl aus:

```cmd/sh
node FileUpload.js
```

Der folgende Screenshot zeigt die Ausgabe der App **FileUpload**:

![Ausgabe der simulated-device-App](./media/iot-hub-node-node-file-upload/simulated-device.png)

Der folgende Screenshot zeigt die Ausgabe der **FileUploadNotification**-App:

![Ausgabe der read-file-upload-notification-App](./media/iot-hub-node-node-file-upload/read-file-upload-notification.png)

Sie können das Verwaltungsportal verwenden, um die hochgeladene Datei im Speichercontainer anzuzeigen, den Sie konfiguriert haben:

![Hochgeladene Datei](./media/iot-hub-node-node-file-upload/uploaded-file.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie die IoT Hub-Funktionen zum Hochladen von Dateien nutzen, um Dateiuploads zu vereinfachen. In den folgenden Artikeln werden weitere IoT Hub-Features und -Szenarien vorgestellt:

* [Erstellen einer IoT Hub-Instanz mithilfe einer Azure Resource Manager-Vorlage (PowerShell)](iot-hub-rm-template-powershell.md)

* [Azure IoT-Geräte-SDK für C](iot-hub-device-sdk-c-intro.md)

* [Azure IoT SDKs](iot-hub-devguide-sdks.md)
