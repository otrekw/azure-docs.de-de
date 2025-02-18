---
title: Hochladen von Dateien von Geräten nach Azure IoT Hub mit .NET | Microsoft-Dokumentation
description: Informationen zum Hochladen von Dateien von einem Gerät in die Cloud mithilfe des Azure IoT-Geräte-SDK für .NET. Hochgeladene Dateien werden in einem Azure Storage-Blobcontainer gespeichert.
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: conceptual
ms.date: 07/04/2017
ms.author: robinsh
ms.custom: mqtt, devx-track-csharp
ms.openlocfilehash: df460ba4163b414ad954dce73125a6376f4cad6f
ms.sourcegitcommit: 190658142b592db528c631a672fdde4692872fd8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2021
ms.locfileid: "112004581"
---
# <a name="upload-files-from-your-device-to-the-cloud-with-iot-hub-net"></a>Hochladen von Dateien von Ihrem Gerät in die Cloud mit IoT Hub (.NET)

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

In diesem Tutorial erfahren Sie mithilfe des Beispiels zum Hochladen von .NET-Dateien, wie Sie die Dateiuploadfunktionen von IoT Hub verwenden. 

Die Schnellstartanleitung [Senden von Telemetriedaten von einem Gerät an eine IoT Hub-Instanz und Lesen der Telemetriedaten aus der IoT Hub-Instanz mit einer Back-End-Anwendung (C#)](quickstart-send-telemetry-dotnet.md) und das Tutorial [Senden von Nachrichten aus der Cloud an das Gerät mit IoT Hub (.NET)](iot-hub-csharp-csharp-c2d.md) veranschaulichen die grundlegenden Gerät-zu-Cloud- und Cloud-zu-Gerät-Messagingfunktionen von IoT Hub. Im [Tutorial: Konfigurieren der Nachrichtenweiterleitung mit IoT Hub](tutorial-routing.md) wird eine Möglichkeit für das zuverlässige Speichern von Gerät-zu-Cloud-Nachrichten in Microsoft Azure Blob Storage beschrieben. In einigen Szenarien können Sie allerdings nicht einfach die Daten, die Ihre Geräte senden, den relativ kleinen Gerät-zu-Cloud-Nachrichten zuordnen, die IoT Hub akzeptiert. Beispiel:

* Große Dateien, die Bilder enthalten

* Videos

* Vibrationsdaten, die mit hoher Häufigkeit als Stichproben erfasst werden

* Eine Form vorverarbeiteter Daten

Diese Dateien werden normalerweise als Batch in der Cloud mit Tools wie [Azure Data Factory](../data-factory/introduction.md) oder dem [Hadoop](../hdinsight/index.yml)-Stapel verarbeitet. Wenn Sie Dateien von einem Gerät hochladen müssen, können Sie aber weiterhin die Sicherheit und Zuverlässigkeit des IoT Hub nutzen. In diesem Tutorial erfahren Sie, wie.

> [!NOTE]
> IoT Hub bietet über Azure IoT-Geräte-SDKs Unterstützung für zahlreiche Geräteplattformen und Sprachen, darunter C, Java, Python und JavaScript. Im [Azure IoT Developer Center](https://azure.microsoft.com/develop/iot) finden Sie Schritt-für-Schritt-Anweisungen zum Verbinden eines Geräts mit Azure IoT Hub.

[!INCLUDE [iot-hub-include-x509-ca-signed-file-upload-support-note](../../includes/iot-hub-include-x509-ca-signed-file-upload-support-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

* Visual Studio Code

* Ein aktives Azure-Konto. Wenn Sie nicht über ein Konto verfügen, können Sie in nur wenigen Minuten ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen.

* Laden Sie unter [https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip](https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip) die Azure IoT-C#-Beispiele herunter, und extrahieren Sie das ZIP-Archiv.

* Öffnen Sie in Visual Studio Code den Ordner *FileUploadSample*, und öffnen Sie die Datei *FileUploadSample.cs*.

* Stellen Sie sicher, dass Port 8883 in Ihrer Firewall geöffnet ist. Für das Beispiel in diesem Artikel wird das MQTT-Protokoll verwendet, das über Port 8883 kommuniziert. In einigen Netzwerkumgebungen von Unternehmen oder Bildungseinrichtungen ist dieser Port unter Umständen blockiert. Weitere Informationen und Problemumgehungen finden Sie unter [Herstellen einer Verbindung mit IoT Hub (MQTT)](iot-hub-mqtt-support.md#connecting-to-iot-hub).

## <a name="create-an-iot-hub"></a>Erstellen eines IoT-Hubs

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]

## <a name="get-the-iot-hub-connection-string"></a>Abrufen der IoT-Hub-Verbindungszeichenfolge

[!INCLUDE [iot-hub-include-find-service-connection-string](../../includes/iot-hub-include-find-service-connection-string.md)]

## <a name="examine-the-application"></a>Untersuchen der Anwendung

Navigieren Sie in Ihrem .NET-Beispieldownload zum Ordner *FileUploadSample*. Öffnen Sie den Ordner in Visual Studio Code. Der Ordner enthält eine Datei mit dem Namen *parameters.cs*. Wenn Sie diese Datei öffnen, sehen Sie, dass der Parameter *p* erforderlich ist und die Verbindungszeichenfolge enthält. Der Parameter *t* kann angegeben werden, wenn Sie das Transportprotokoll ändern möchten. Das Standardprotokoll ist „mqtt“. Die Datei *program.cs* enthält die *main*-Funktion. Die Datei *FileUploadSample.cs* enthält die primäre Beispiellogik. *TestPayload.txt* ist die Datei, die in Ihren Blobcontainer hochgeladen werden soll.

## <a name="run-the-application"></a>Ausführen der Anwendung

Sie können die Anwendung jetzt ausführen.

1. Öffnen Sie in Visual Studio Code ein Terminalfenster.
1. Geben Sie die folgenden Befehle ein:
    ```cmd/sh
    dotnet restore
    dotnet run --p "{Your connection string}"
    ```

Die Ausgabe sollte wie folgt aussehen:

```cmd/sh
  Uploading file TestPayload.txt
  Getting SAS URI from IoT Hub to use when uploading the file...
  Successfully got SAS URI (https://contosostorage.blob.core.windows.net/contosocontainer/MyDevice%2FTestPayload.txt?sv=2018-03-28&sr=b&sig=x0G1Baf%2BAjR%2BTg3nW34zDNKs07p6dLzkxvZ3ZSmjIhw%3D&se=2021-05-04T16%3A40%3A52Z&sp=rw) from IoT Hub
  Uploading file TestPayload.txt using the Azure Storage SDK and the retrieved SAS URI for authentication
  Successfully uploaded the file to Azure Storage
  Notified IoT Hub that the file upload succeeded and that the SAS URI can be freed.
  Time to upload file: 00:00:01.5077954.
  Done.
```

## <a name="verify-the-file-upload"></a>Überprüfen des Dateiuploads

Führen Sie die folgenden Schritte aus, um sich zu vergewissern, dass  *TestPayload.txt* in Ihren Container hochgeladen wurde:

1. Wählen Sie im linken Bereich Ihres Speicherkontos unter **Datenspeicherung** die Option **Container** aus.
1. Wählen Sie den Container aus, in den Sie *TestPayload.txt* hochgeladen haben.
1. Wählen Sie den Ordner aus, der nach Ihrem Gerät benannt ist.
1. Wählen Sie *TestPayload.txt* aus.
1. Laden Sie die Datei herunter, um ihren Inhalt lokal anzuzeigen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie die IoT Hub-Funktionen zum Hochladen von Dateien nutzen, um Dateiuploads zu vereinfachen. In den folgenden Artikeln werden weitere IoT Hub-Features und -Szenarien vorgestellt:

* [Erstellen einer IoT Hub-Instanz mithilfe einer Azure Resource Manager-Vorlage (PowerShell)](iot-hub-rm-template-powershell.md)

* [Azure IoT-Geräte-SDK für C](iot-hub-device-sdk-c-intro.md)

* [Azure IoT SDKs](iot-hub-devguide-sdks.md)

Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter:

* [Bereitstellen von KI auf Edge-Geräten mit Azure IoT Edge](../iot-edge/quickstart-linux.md)