---
title: Informationen zum Hochladen von Dateien in Azure IoT Hub | Microsoft-Dokumentation
description: 'Entwicklerhandbuch: Verwenden des Dateiupload-Features von IoT Hub zum Verwalten der Uploads von Dateien von einem Gerät in einen Azure Storage-Blobcontainer.'
author: robinsh
ms.author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 11/07/2018
ms.custom:
- mqtt
- 'Role: Cloud Development'
- 'Role: IoT Device'
ms.openlocfilehash: dd7adc3a1fdb45255bd13ee279d66e64bdf4c1d1
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122355003"
---
# <a name="upload-files-with-iot-hub"></a>Hochladen von Dateien mit IoT Hub

In einigen Szenarien können Sie allerdings nicht einfach die Daten, die Ihre Geräte senden, den relativ kleinen Gerät-zu-Cloud-Nachrichten zuordnen, die IoT Hub bereitwillig akzeptiert. Beispiel:
* Große Image-Dateien
* Videodateien
* Vibrationsdaten, die mit hoher Häufigkeit als Stichproben erfasst werden
* Eine Form vorverarbeiteter Daten

Wenn Sie derartige Dateien von einem Gerät hochladen müssen, können Sie weiterhin die Sicherheit und Zuverlässigkeit des IoT Hub nutzen. Statt Nachrichten über IoT Hub selbst zu übertragen, fungiert IoT Hub stattdessen als Verteiler für ein zugeordnetes Azure Storage-Konto. Ein Gerät fordert ein Speichertoken von IoT Hub an, das spezifisch für die vom Gerät hochgeladene Datei gilt. Das Gerät verwendet den SAS-URI zum Hochladen der Datei in den Speicher. Wenn der Upload abgeschlossen ist, sendet das Gerät eine Benachrichtigung über den Abschluss an IoT Hub. IoT Hub überprüft, ob der Dateiupload abgeschlossen ist.

[!INCLUDE [iot-hub-include-x509-ca-signed-file-upload-support-note](../../includes/iot-hub-include-x509-ca-signed-file-upload-support-note.md)]

### <a name="when-to-use"></a>Verwendung

Verwenden Sie Dateiuploads zum Senden von Mediendateien und umfangreichen Telemetriebatches, die von zeitweise verbundenen Geräten hochgeladen oder komprimiert werden, um Bandbreite zu sparen. Weitere Informationen zur Verwendung von gemeldeten Eigenschaften, D2C-Nachrichten oder Dateiuploads finden Sie im [Leitfaden zur D2C-Kommunikation](iot-hub-devguide-d2c-guidance.md).

## <a name="associate-an-azure-storage-account-with-iot-hub"></a>Zuordnen eines Azure Storage-Kontos zu IoT Hub

Sie müssen über ein Azure Storage-Konto verfügen, das mit Ihrem IoT-Hub verbunden ist. 

Um zu erfahren, wie Sie ein eines mithilfe des Portals erstellen, lesen Sie den Artikel [Erstellen eines Speicherkontos](../storage/common/storage-account-create.md). 

Sie können auch eines programmgesteuert mithilfe der [REST-APIs des IoT Hub Ressourcenanbieters ](/rest/api/iothub/iothubresource) erstellen. 

Wenn Sie Azure Storage-Konto einem IoT Hub zuordnen, generiert der IoT Hub einen SAS-URI. Ein Gerät kann diesen SAS-URI für das sichere Hochladen einer Datei zu einem Blobcontainer verwenden.

## <a name="create-a-container"></a>Erstellen eines Containers

 So erstellen Sie einen Blobcontainer über das Portal:

1. Wählen Sie im linken Bereich Ihres Speicherkontos unter **Datenspeicherung** die Option **Container** aus.
1. Wählen Sie auf dem Container-Blatt die Option **+ Container** aus.
1. Geben Sie im Bereich **Neuer Container**, der geöffnet wird, einen Namen für Ihren Container ein, und wählen Sie **Erstellen** aus.

Nach der Erstellung des Containers folgen Sie den Anleitungen in [Konfigurieren des Dateiuploads im Azure-Portal](iot-hub-configure-file-upload.md). Stellen Sie sicher, dass Ihrem IoT Hub ein Blobcontainer zugeordnet ist und die Dateibenachrichtigungen aktiviert sind.

Sie können auch die [REST-APIs IoT Hub Ressourcenanbieter](/rest/api/iothub/iothubresource) verwenden, um einen Container zu erstellen, der dem Speicher für Ihre IoT Hub zugeordnet ist.

## <a name="file-upload-using-an-sdk"></a>Dateiupload mithilfe eines SDK

Die folgenden Anleitungen bieten vollständige exemplarische Vorgehensweisen für den Dateiupload-Prozess in einer Vielzahl von SDK-Sprachen. Diese Leitfäden zeigen, wie Sie mit dem Azure-Portal ein Speicherkonto einem IoT-Hub zuordnen. Sie enthalten auch Codeausschnitte oder verweisen auf Beispiele, die Sie durch den Uploadprozess führen.

* [.NET](iot-hub-csharp-csharp-file-upload.md)
* [Java](iot-hub-java-java-file-upload.md)
* [Node.js](iot-hub-node-node-file-upload.md)
* [Python](iot-hub-python-python-file-upload.md)

> [!NOTE]
> Die [Azure IoT-SDKs](iot-hub-devguide-sdks.md) verarbeiten automatisch das Abrufen des SAS-URIs (Shared Access Signature), das Hochladen der Datei und das Benachrichtigen von IoT Hub über einen abgeschlossenen Upload. Wenn eine Firewall den Zugriff auf den BLOB Storage-Endpunkt blockiert, aber den Zugriff auf den IOT Hub-Endpunkt zulässt, schlägt der Dateiupload fehl und zeigt folgenden Fehler für das IOT c#-Geräte-SDK an:
>
> `---> System.Net.Http.HttpRequestException: A connection attempt failed because the connected party did not properly respond after a period of time, or established connection failed because connected host has failed to respond`
>
> Damit das Feature zum Hochladen von Dateien funktioniert, muss das Gerät Zugriff auf den IOT Hub-Endpunkt als auch auf den BLOB Storage -Endpunkt haben.
> 


## <a name="initialize-a-file-upload-rest"></a>Initialisieren eines Dateiuploads (REST)

Sie können REST-APIs anstelle eines der SDKs verwenden, um eine Datei hochzuladen. IoT Hub verfügt über einen Endpunkt speziell für Geräte zum Anfordern eines SAS-URI für die Speicherung zum Hochladen einer Datei. Zum Beginnen des Dateiuploads sendet das Gerät eine POST-Anforderung mit dem folgenden JSON-Code an `{iot hub}.azure-devices.net/devices/{deviceId}/files`:

```json
{
    "blobName": "{name of the file for which a SAS URI will be generated}"
}
```

IoT Hub gibt die folgenden Daten zurück, die vom Gerät zum Hochladen der Datei genutzt werden:

```json
{
    "correlationId": "somecorrelationid",
    "hostName": "yourstorageaccount.blob.core.windows.net",
    "containerName": "testcontainer",
    "blobName": "test-device1/image.jpg",
    "sasToken": "1234asdfSAStoken"
}
```

### <a name="deprecated-initialize-a-file-upload-with-a-get"></a>Veraltet: Initialisieren des Uploads einer Datei mit einer GET-Anweisung

> [!NOTE]
> In diesem Abschnitt wird veraltete Funktionalität zum Empfangen eines SAS-URI von IoT Hub beschrieben. Verwenden Sie die zuvor beschriebene POST-Methode.

IoT Hub verfügt über zwei REST-Endpunkte, um den Dateiupload zu unterstützen: einen zum Abrufen des SAS-URIs für den Speicher und einen zum Benachrichtigen von IoT Hub über einen abgeschlossenen Upload. Das Gerät beginnt mit dem Dateiupload, indem es eine GET-Anforderung an den IoT-Hub unter `{iot hub}.azure-devices.net/devices/{deviceId}/files/{filename}`sendet. IoT Hub gibt Folgendes zurück:

* Einen spezifischen SAS-URI für die hochzuladende Datei

* Eine Korrelations-ID zur Verwendung nach Abschluss des Uploadvorgangs

## <a name="notify-iot-hub-of-a-completed-file-upload-rest"></a>Benachrichtigen von IoT Hub über einen abgeschlossenen Dateiupload (REST)

Das Gerät lädt die Datei mithilfe der Azure Storage-SDKs in den Speicher hoch. Nach Abschluss des Uploadvorgangs sendet das Gerät eine POST-Anforderung mit dem folgenden JSON-Code an `{iot hub}.azure-devices.net/devices/{deviceId}/files/notifications`:

```json
{
    "correlationId": "{correlation ID received from the initial request}",
    "isSuccess": bool,
    "statusCode": XXX,
    "statusDescription": "Description of status"
}
```

Der Wert `isSuccess` ist ein boolescher Wert, der angibt, ob die Datei erfolgreich hochgeladen wurde. Der Statuscode `statusCode` ist der Status des Uploads der Datei in Speicher, und `statusDescription` entspricht `statusCode`.

## <a name="reference-topics"></a>Referenzthemen:

Die folgenden Referenzthemen enthalten weitere Informationen zum Hochladen von Dateien von einem Gerät.

### <a name="file-upload-notifications"></a>Benachrichtigungen zum Dateiupload

Optional: Wenn ein Gerät IoT Hub benachrichtigt, dass ein Upload abgeschlossen ist, generiert IoT Hub eine Benachrichtigungsmeldung. Diese Meldung enthält den Namen und den Speicherort der Datei.

Wie im Abschnitt [Endpunkte](iot-hub-devguide-endpoints.md) erläutert, übermittelt IoT Hub Dateiuploadbenachrichtigungen als Nachrichten über einen dienstseitigen Endpunkt (**/messages/servicebound/fileuploadnotifications**). Die Empfangssemantik für Dateiuploadbenachrichtigungen entspricht der Empfangssemantik für C2D-Nachrichten und weist den gleichen [Nachrichtenlebenszyklus](iot-hub-devguide-messages-c2d.md#the-cloud-to-device-message-life-cycle) auf. Jede Nachricht, die vom Endpunkt für Dateiuploadbenachrichtigungen abgerufen wird, ist ein JSON-Datensatz mit den folgenden Eigenschaften:

| Eigenschaft | BESCHREIBUNG |
| --- | --- |
| EnqueuedTimeUtc |Zeitstempel, der die Erstellung der Benachrichtigung angibt. |
| deviceId |**DeviceId** des Geräts, das die Datei hochgeladen hat. |
| BlobUri |Der URI der hochgeladenen Datei. |
| BlobName |Der Name der hochgeladenen Datei. |
| LastUpdatedTime |Zeitstempel, der die letzte Aktualisierung der Datei angibt. |
| BlobSizeInBytes |Die Größe der hochgeladenen Datei. |

**Beispiel**. Dieses Beispiel zeigt den Text einer Dateiuploadbenachrichtigung.

```json
{
    "deviceId":"mydevice",
    "blobUri":"https://{storage account}.blob.core.windows.net/{container name}/mydevice/myfile.jpg",
    "blobName":"mydevice/myfile.jpg",
    "lastUpdatedTime":"2016-06-01T21:22:41+00:00",
    "blobSizeInBytes":1234,
    "enqueuedTimeUtc":"2016-06-01T21:22:43.7996883Z"
}
```

### <a name="file-upload-notification-configuration-options"></a>Konfigurationsoptionen für Dateiuploadbenachrichtigungen

Jeder IoT-Hub weist die folgenden Konfigurationsoptionen für Dateiuploadbenachrichtigungen auf:

| Eigenschaft | BESCHREIBUNG | Bereich und Standardwert |
| --- | --- | --- |
| **enableFileUploadNotifications** |Steuert, ob Dateiuploadbenachrichtigungen in den Endpunkt für Dateibenachrichtigungen geschrieben werden. |Bool. Standardwert: TRUE. |
| **fileNotifications.ttlAsIso8601** |Standardgültigkeitsdauer für Dateiuploadbenachrichtigungen. |ISO_8601-Intervall bis zu 48 Stunden (mindestens 1 Minute). Standardwert: 1 Stunde. |
| **fileNotifications.lockDuration** |Sperrdauer für die Warteschlange der Dateiuploadbenachrichtigungen. |5 bis 300 Sekunden (mindestens 5 Sekunden). Standardwert: 60 Sekunden. |
| **fileNotifications.maxDeliveryCount** |Maximale Übermittlungsanzahl für die Warteschlange der Dateiuploadbenachrichtigungen. |1 bis 100. Standardwert: 100. |

Sie können diese Eigenschaften für Ihren IoT-Hub über das Azure-Portal, Azure CLI oder PowerShell festlegen. Entsprechende Informationen finden Sie in den Themen unter [Konfigurieren des Dateiuploads](iot-hub-configure-file-upload.md).

## <a name="additional-reference-material"></a>Weiteres Referenzmaterial

Weitere Referenzthemen im IoT Hub-Entwicklerhandbuch:

* Unter [IoT Hub-Endpunkte](iot-hub-devguide-endpoints.md) werden die verschiedenen IoT Hub-Endpunkte für Laufzeit- und Verwaltungsvorgänge beschrieben.

* Unter [Drosselung und Kontingente](iot-hub-devguide-quotas-throttling.md) werden die Kontingente und das Drosselungsverhalten des IoT Hub-Diensts beschrieben.

* Unter [Verstehen und Verwenden von Azure IoT Hub SDKs](iot-hub-devguide-sdks.md) werden die verschiedenen Sprach-SDKs aufgelistet, die Sie bei der Entwicklung von Geräte- und Dienst-Apps für die Interaktion mit IoT Hub verwenden können.

* Unter [IoT Hub-Abfragesprache](iot-hub-devguide-query-language.md) wird die Abfragesprache beschrieben, mit der Sie Informationen zu Gerätezwillingen und Aufträgen aus IoT Hub abrufen können.

* [Kommunikation mit Ihrem IoT Hub mithilfe des Protokolls MQTT](iot-hub-mqtt-support.md) enthält weitere Informationen zur IoT Hub-Unterstützung für das MQTT-Protokoll.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie gelernt haben, wie Sie Dateien mithilfe von IoT Hub von Geräten hochladen, sind möglicherweise die folgenden Themen im IoT Hub-Entwicklerhandbuch für Sie interessant:

* [Verwalten von Geräteidentitäten in IoT Hub](iot-hub-devguide-identity-registry.md)

* [Verwalten des Zugriffs auf IoT Hub](iot-hub-devguide-security.md)

* [Verwenden von Gerätezwillingen zum Synchronisieren von Status und Konfigurationen](iot-hub-devguide-device-twins.md)

* [Aufrufen einer direkten Methode auf einem Gerät](iot-hub-devguide-direct-methods.md)

* [Planen von Aufträgen auf mehreren Geräten](iot-hub-devguide-jobs.md)

Um einige der in diesem Artikel beschriebenen Konzepte auszuprobieren, sehen Sie sich das folgende IoT Hub-Tutorial an:

* [Hochladen von Dateien von Geräten in die Cloud mit IoT Hub](iot-hub-csharp-csharp-file-upload.md)
