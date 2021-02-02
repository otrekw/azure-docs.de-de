---
Titel: Hochladen von Medien : Azure Media Services description: Hier erfahren Sie, wie Sie Medien für Streaming und Codierung hochladen.
services: media-services documentationcenter: '' author: IngridAtMicrosoft manager: femila editor: ''

ms.service: media-services ms.workload: ms.topic: how-to ms.date: 08/31/2020 ms.author: inhenkel
---

# <a name="upload-media-for-streaming-or-encoding"></a>Hochladen von Medien für Streaming und Codierung

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

In Media Services laden Sie Ihre digitalen Dateien (Medien) in einen Blobcontainer hoch, der einem Medienobjekt zugeordnet ist. Die [Asset](/rest/api/media/operations/asset)-Entität kann Videos, Audiodateien, Bilder, Miniaturansichtssammlungen, Texttitel und Untertiteldateien (und die Metadaten zu diesen Dateien) enthalten. Nachdem die Dateien in den Container des Medienobjekts hochgeladen wurden, werden Ihre Inhalte zur weiteren Verarbeitung und zum Streaming sicher in der Cloud gespeichert.

Bevor Sie beginnen, müssen Sie jedoch einige Werte erfassen bzw. berücksichtigen.

1. Lokaler Dateipfad zu der Datei, die Sie hochladen möchten
1. Asset-ID für das Medienobjekt (Container)
1. Name, den Sie für die hochgeladene Datei verwenden möchten, einschließlich der Erweiterung
1. Name des von Ihnen verwendeten Speicherkontos
1. Zugriffsschlüssel für Ihr Speicherkonto

## <a name="portal"></a>[Portal](#tab/portal/)

[!INCLUDE [Upload files with the portal](./includes/task-upload-file-to-asset-portal.md)]

## <a name="cli"></a>[BEFEHLSZEILENSCHNITTSTELLE (CLI)](#tab/cli/)

[!INCLUDE [Upload files with the portal](./includes/task-upload-file-to-asset-cli.md)]

## <a name="rest"></a>[REST](#tab/rest/)

Nachdem Sie [mithilfe von Postman oder einer anderen REST-Methode ein Medienobjekt erstellt und die SAS-URL für das Medienobjekt abgerufen haben](how-to-create-asset.md?tabs=rest), verwenden Sie die Azure Storage-APIs oder SDKs (etwa die [Storage-REST-API](../../storage/common/storage-rest-api-auth.md) oder das [.NET SDK](../../storage/blobs/storage-quickstart-blobs-dotnet.md)).

---
<!-- add these to the tabs when available -->
Informationen zu weiteren Methoden finden Sie in der [Azure Storage-Dokumentation](../../storage/blobs/index.yml) zum Arbeiten mit Blobs in [.NET](../../storage/blobs/storage-quickstart-blobs-dotnet.md), [Java](../../storage/blobs/storage-quickstart-blobs-java.md), [Python](../../storage/blobs/storage-quickstart-blobs-python.md) und [JavaScript (Node.js)](../../storage/blobs/storage-quickstart-blobs-nodejs.md).

## <a name="next-steps"></a>Nächste Schritte

> [Media Services: Übersicht](media-services-overview.md)