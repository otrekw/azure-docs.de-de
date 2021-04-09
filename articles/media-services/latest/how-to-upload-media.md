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

[!INCLUDE [Upload files with the CLI](./includes/task-upload-file-to-asset-cli.md)]

## <a name="python"></a>[Python](#tab/python)

Wenn für Ihren Code bereits eine Authentifizierung eingerichtet ist und Sie bereits eine Eingaberessource erstellt haben, verwenden Sie den folgenden Codeausschnitt, um lokale Dateien in diese Ressource (in_container) hochzuladen.

```python
#The storage objects
from azure.storage.blob import BlobServiceClient, BlobClient

#Establish storage variables
storage_account_name = '<your storage account name'
storage_account_key = '<your storage account key'
storage_blob_url = 'https://<your storage account name>.blob.core.windows.net/'

in_container = 'asset-' + inputAsset.asset_id

#The file path of local file you want to upload
source_file = "ignite.mp4"

# Use the Storage SDK to upload the video
blob_service_client = BlobServiceClient(account_url=storage_blob_url, credential=storage_account_key)
blob_client = blob_service_client.get_blob_client(in_container,source_file)

# Upload the video to storage as a block blob
with open(source_file, "rb") as data:
    blob_client.upload_blob(data, blob_type="BlockBlob")
```

---
<!-- add these to the tabs when available -->
Informationen zu weiteren Methoden finden Sie in der [Azure Storage-Dokumentation](../../storage/blobs/index.yml) zum Arbeiten mit Blobs in [.NET](../../storage/blobs/storage-quickstart-blobs-dotnet.md), [Java](../../storage/blobs/storage-quickstart-blobs-java.md), [Python](../../storage/blobs/storage-quickstart-blobs-python.md) und [JavaScript (Node.js)](../../storage/blobs/storage-quickstart-blobs-nodejs.md).

## <a name="next-steps"></a>Nächste Schritte

> [Media Services: Übersicht](media-services-overview.md)
