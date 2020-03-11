---
title: Verwalten von Medienobjekten in Azure Media Services
titleSuffix: Azure Media Services
description: Ein Medienobjekt, bei dem Sie Medien eingeben (z. B. durch Hochladen oder Liveerfassung), Medien ausgeben (von einer Auftragsausgabe) und Medien veröffentlichen (für Streaming). Dieses Thema bietet eine Übersicht darüber, wie Sie ein neues Medienobjekt erstellen und Dateien hochladen können.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 08/29/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: b1ec4ee3d7a51c2a21a5bbd8888ea4662cf78bf5
ms.sourcegitcommit: f915d8b43a3cefe532062ca7d7dbbf569d2583d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2020
ms.locfileid: "78304156"
---
# <a name="manage-assets"></a>Verwalten von Objekten

In Azure Media Services bietet ein [Medienobjekt](https://docs.microsoft.com/rest/api/media/assets) folgende Möglichkeiten: 

* Hochladen von Mediendateien in ein Medienobjekt
* Erfassen und Archivieren von Livestreams in einem Medienobjekt
* Ausgabe der Ergebnisse einer Codierung eines Analyseauftrags in ein Medienobjekt
* Veröffentlichen von Medien für das Streaming 
* Herunterladen von Dateien aus einem Medienobjekt

Dieses Thema enthält eine Übersicht über das Hochladen von Dateien in ein Medienobjekt und das Ausführen einiger anderer allgemeiner Vorgänge. Außerdem finden Sie hier Links zu Codebeispielen und verwandten Themen.

## <a name="prerequisite"></a>Voraussetzung 

Machen Sie sich zunächst mit Folgendem vertraut, bevor Sie mit der Entwicklung beginnen:

* [Konzepte](concepts-overview.md)
* [Entwickeln mit Media Services v3-APIs](media-services-apis-overview.md) (Informationen zum Zugreifen auf APIs, Namenskonventionen usw.) 

## <a name="upload-media-files-into-an-asset"></a>Hochladen von Mediendateien in ein Medienobjekt

Nachdem die digitalen Dateien in den Speicher hochgeladen und einem Medienobjekt zugeordnet wurden, können sie in den Media Services-Workflows zum Codieren, Streamen und Analysieren von Inhalten verwendet werden. Einer der gängigsten Media Services-Workflows besteht aus dem Hochladen, Codieren und Streamen einer Datei. Dieser Abschnitt erläutert die allgemeinen Schritte.

1. Verwenden Sie die Media Services v3-API, um ein neues Medienobjekt für die Eingabe zu erstellen. Dieser Vorgang erstellt einen Container in dem Speicherkonto, das Ihrem Media Services-Konto zugeordnet ist. Die API gibt den Containernamen zurück (z.B. `"container": "asset-b8d8b68a-2d7f-4d8c-81bb-8c7bbbe67ee4"`).

    Wenn Sie bereits über einen Blobcontainer verfügen, den Sie einem Medienobjekt zuordnen möchten, können Sie den Containernamen beim Erstellen des Medienobjekts angeben. Media Services unterstützt derzeit nur Blobs im Containerstamm, nicht mit Pfaden im Dateinamen. Daher funktioniert ein Container mit dem Dateinamen „input.mp4“. Ein Container mit dem Dateinamen „videos/inputs/input.mp4“ funktioniert hingegen nicht.

    Sie können die Azure CLI verwenden, um Dateien direkt in ein Speicherkonto und einen Container hochzuladen, für das bzw. den Sie in Ihrem Abonnement Berechtigungen besitzen.

    Der Containername muss eindeutig sein und Benennungsrichtlinien für Speicher befolgen. Der Name muss nicht der Formatierung für Containernamen für Media Services-Medienobjekte folgen (Asset-GUID).

    ```azurecli
    az storage blob upload -f /path/to/file -c MyContainer -n MyBlob
    ```
2. Rufen Sie eine SAS-URL mit Lese-/Schreibberechtigungen ab, die verwendet wird, um digitale Dateien in den Medienobjektcontainer hochzuladen. Sie können die Media Services-API verwenden, um [die Medienobjektcontainer-URLs aufzulisten](https://docs.microsoft.com/rest/api/media/assets/listcontainersas).
3. Verwenden Sie die Azure Storage-APIs oder SDKs (z.B. [Storage-REST-API](../../storage/common/storage-rest-api-auth.md) oder [.NET SDK](../../storage/blobs/storage-quickstart-blobs-dotnet.md)), um Dateien in den Medienobjektcontainer hochzuladen.
4. Verwenden Sie Media Services v3-APIs, um eine Transformation und einen Auftrag zur Verarbeitung Ihres Eingabemedienobjekts zu erstellen. Weitere Informationen finden Sie unter [Transformationen und Aufträge](transform-concept.md).
5. Streamen Sie die Inhalte des Ausgabemedienobjekts.

### <a name="create-a-new-asset"></a>Erstellen des neuen Medienobjekts

> [!NOTE]
> Eigenschaften von Medienobjekten im Datetime-Typ liegen immer im UTC-Format vor.

#### <a name="rest"></a>REST

```
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Media/mediaServices/{amsAccountName}/assets/{assetName}?api-version=2018-07-01
```

Ein REST-Beispiel finden Sie unter [Erstellen eines Medienobjekts mit REST](https://docs.microsoft.com/rest/api/media/assets/createorupdate#examples).

Dieses Beispiel zeigt, wie Sie den **Anforderungstext** erstellen, in dem Sie Beschreibung, Containername, Speicherkonto und weitere hilfreiche Informationen angeben können.

#### <a name="curl"></a>cURL

```cURL
curl -X PUT \
  'https://management.azure.com/subscriptions/00000000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Media/mediaServices/amsAccountName/assets/myOutputAsset?api-version=2018-07-01' \
  -H 'Accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "properties": {
    "description": "",
  }
}'
```

#### <a name="net"></a>.NET

```csharp
 Asset asset = await client.Assets.CreateOrUpdateAsync(resourceGroupName, accountName, assetName, new Asset());
```

### <a name="see-also"></a>Weitere Informationen

* [Erstellen einer Auftragseingabe aus einer lokalen Datei](job-input-from-local-file-how-to.md)
* [Erstellen einer Auftragseingabe aus einer HTTPS-URL](job-input-from-http-how-to.md)

## <a name="ingest-and-archive-live-streams-into-an-asset"></a>Erfassen und Archivieren von Livestreams in einem Medienobjekt

In Media Services verhält sich ein [Liveausgabe](https://docs.microsoft.com/rest/api/media/liveoutputs)-Objekt wie ein digitaler Videorekorder, der Ihren Livestream erfasst und in einem Medienobjekt (Asset) in Ihrem Media Services-Konto aufzeichnet. Der aufgezeichnete Inhalt wird in dem durch die Ressource [Medienobjekt](https://docs.microsoft.com/rest/api/media/assets) definierten Container beibehalten.

Weitere Informationen finden Sie unter

* [Verwenden eines Cloud-DVR](live-event-cloud-dvr.md)
* [Tutorial zum Livestreaming](stream-live-tutorial-with-api.md)

## <a name="output-the-results-of-a-job-to-an-asset"></a>Ausgeben der Ergebnisse eines Auftrags in ein Medienobjekt

In Media Services müssen Sie bei der Verarbeitung von Videos (z. B. Codierung oder Analyse) eine [Ausgaberessource](assets-concept.md) erstellen, um das Ergebnis Ihrer [Auftrags](transforms-jobs-concept.md) zu speichern.

Weitere Informationen finden Sie unter

* [Videocodierung](encoding-concept.md)
* [Erstellen einer Auftragseingabe aus einer lokalen Datei](job-input-from-local-file-how-to.md)

## <a name="publish-an-asset-for-streaming"></a>Veröffentlichen eines Medienobjekts für das Streaming

Um ein Medienobjekt für das Streaming zu veröffentlichen, müssen Sie einen [Streaminglocator](streaming-locators-concept.md) erstellen. Der Streaminglocator muss den Namen des Objekts kennen, das Sie veröffentlichen möchten. 

Weitere Informationen finden Sie unter

[Tutorial: Hochladen, Codieren und Streamen von Videos mit Media Services v3](stream-files-tutorial-with-api.md)

## <a name="download-results-of-a-job-from-an-output-asset"></a>Herunterladen der Ergebnisse eines Auftrags von einem Ausgabemedienobjekt

Anschließend können Sie diese Ergebnisse Ihres Auftrags mithilfe von Media Service- und Storage-APIs in einen lokalen Ordner herunterladen. 

Weitere Informationen finden Sie im Beispiel zum [Herunterladen von Dateien](download-results-howto.md).

## <a name="filtering-ordering-paging"></a>Filterung, Sortierung, Paging

Informationen finden Sie unter [Filterung, Sortierung, Paginierung von Media Services-Entitäten](entities-overview.md).

## <a name="next-steps"></a>Nächste Schritte

Betrachten Sie die vollständigen Codebeispiele, die zeigen, wie Sie hochladen, codieren, analysieren sowie live und bedarfsgesteuert streamen können: 

* [Java](https://docs.microsoft.com/samples/azure-samples/media-services-v3-java/azure-media-services-v3-samples-using-java/), 
* [.NET](https://docs.microsoft.com/samples/azure-samples/media-services-v3-dotnet/azure-media-services-v3-samples-using-net/), 
* [REST](https://docs.microsoft.com/samples/azure-samples/media-services-v3-rest-postman/azure-media-services-postman-collection/).
