---
title: Aktivieren und Verwalten von Blobversionsverwaltung
titleSuffix: Azure Storage
description: Erfahren Sie, wie Sie Blobversionsverwaltung im Azure-Portal und mit einer Azure Resource Manager-Vorlage aktivieren können.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 08/27/2020
ms.author: tamram
ms.subservice: blobs
ms.custom: devx-track-csharp
ms.openlocfilehash: 1df7afb5a029ff7770a64d6bf698a462c8ab9735
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89230669"
---
# <a name="enable-and-manage-blob-versioning"></a>Aktivieren und Verwalten von Blobversionsverwaltung

Sie können die Blob Storage-Versionsverwaltung aktivieren, um frühere Versionen eines Objekts automatisch zu verwalten.  Wenn Blobversionsverwaltung aktiviert ist, können Sie eine frühere Version eines Blobs wiederherstellen, um Daten wiederherzustellen, wenn diese irrtümlich geändert oder gelöscht wurden.

In diesem Artikel wird gezeigt, wie Sie die Blobversionsverwaltung für das Speicherkonto über das Azure-Portal oder mithilfe einer Azure Resource Manager-Vorlage aktivieren oder deaktivieren können. Informationen zur Blobversionsverwaltung finden Sie unter [Blobversionsverwaltung](versioning-overview.md).

[!INCLUDE [storage-data-lake-gen2-support](../../../includes/storage-data-lake-gen2-support.md)]

## <a name="enable-blob-versioning"></a>Aktivieren der Blobversionsverwaltung

# <a name="azure-portal"></a>[Azure portal](#tab/portal)

So aktivieren Sie Blobversionsverwaltung im Azure-Portal:

1. Navigieren Sie im Portal zu Ihrem Speicherkonto.
1. Wählen Sie unter **Blob-Dienst** die Option **Datenschutz** aus.
1. Wählen Sie im Abschnitt **Versionsverwaltung** die Option **Aktiviert** aus.

:::image type="content" source="media/versioning-enable/portal-enable-versioning.png" alt-text="Screenshot: Aktivieren von Blobversionsverwaltung im Azure-Portal":::

# <a name="template"></a>[Vorlage](#tab/template)

Um Blobversionsverwaltung mit einer Vorlage zu aktivieren, erstellen Sie eine Vorlage, in der die **IsVersioningEnabled**-Eigenschaft auf **TRUE** festgelegt ist. Die folgenden Schritte beschreiben, wie eine Vorlage im Azure-Portal erstellt wird.

1. Klicken Sie im Azure-Portal auf **Ressource erstellen**.
1. Geben Sie in **Marketplace durchsuchen** den Begriff **Vorlagenbereitstellung** ein, und drücken Sie dann die **EINGABETASTE**.
1. Wählen Sie **Vorlagenbereitstellung** aus, klicken Sie auf **Erstellen**, und wählen Sie dann **Eigene Vorlage im Editor erstellen** aus.
1. Fügen Sie folgenden JSON-Code im Vorlagen-Editor ein. Ersetzen Sie den Platzhalter `<accountName>` durch den Namen Ihres Speicherkontos.
1. Speichern Sie die Vorlage.
1. Geben Sie die Ressourcengruppe des Kontos an, und wählen Sie dann die Schaltfläche **Kaufen** aus, um die Vorlage bereitzustellen und die Blobversionsverwaltung zu aktivieren.

    ```json
    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {},
        "variables": {},
        "resources": [
            {
                "type": "Microsoft.Storage/storageAccounts/blobServices",
                "apiVersion": "2019-06-01",
                "name": "<accountName>/default",
                "properties": {
                    "IsVersioningEnabled": true
                }
            }
        ]
    }
    ```

Weitere Informationen zum Bereitstellen von Ressourcen mit Vorlagen im Azure-Portal finden Sie unter [Bereitstellen von Ressourcen mit dem Azure-Portal](../../azure-resource-manager/templates/deploy-portal.md).

---

## <a name="modify-a-blob-to-trigger-a-new-version"></a>Ändern eines Blobs zum Auslösen einer neuen Version

Im folgenden Codebeispiel wird veranschaulicht, wie Sie die Erstellung einer neuen Version mit der Azure Storage-Clientbibliothek für .NET ab Version [12.5.1](https://www.nuget.org/packages/Azure.Storage.Blobs/12.5.1) auslösen. Stellen Sie vor dem Ausführen dieses Beispiels sicher, dass Sie Versionsverwaltung für Ihr Speicherkonto aktiviert haben.

Im Beispiel wird ein Blockblob erstellt, und anschließend werden die Metadaten des Blobs aktualisiert. Durch Aktualisieren der Metadaten des Blobs wird die Erstellung einer neuen Version ausgelöst. Das Beispiel ruft die anfängliche Version und die aktuelle Version ab und zeigt, dass nur die aktuelle Version die Metadaten enthält.

```csharp
public static async Task UpdateVersionedBlobMetadata(string containerName, string blobName)
{
    // Create a new service client from the connection string.
    BlobServiceClient blobServiceClient = new BlobServiceClient(ConnectionString);

    // Create a new container client.
    BlobContainerClient containerClient = blobServiceClient.GetBlobContainerClient(containerName);

    try
    {
        // Create the container.
        await containerClient.CreateIfNotExistsAsync();

        // Upload a block blob.
        BlockBlobClient blockBlobClient = containerClient.GetBlockBlobClient(blobName);

        string blobContents = string.Format("Block blob created at {0}.", DateTime.Now);
        byte[] byteArray = Encoding.ASCII.GetBytes(blobContents);

        string initalVersionId;
        using (MemoryStream stream = new MemoryStream(byteArray))
        {
            Response<BlobContentInfo> uploadResponse = await blockBlobClient.UploadAsync(stream, null, default);

            // Get the version ID for the current version.
            initalVersionId = uploadResponse.Value.VersionId;
        }

        // Update the blob's metadata to trigger the creation of a new version.
        Dictionary<string, string> metadata = new Dictionary<string, string>
        {
            { "key", "value" },
            { "key1", "value1" }
        };

        Response<BlobInfo> metadataResponse = await blockBlobClient.SetMetadataAsync(metadata);

        // Get the version ID for the new current version.
        string newVersionId = metadataResponse.Value.VersionId;

        // Request metadata on the previous version.
        BlockBlobClient initalVersionBlob = blockBlobClient.WithVersion(initalVersionId);
        Response<BlobProperties> propertiesResponse = await initalVersionBlob.GetPropertiesAsync();
        PrintMetadata(propertiesResponse);

        // Request metadata on the current version.
        BlockBlobClient newVersionBlob = blockBlobClient.WithVersion(newVersionId);
        Response<BlobProperties> newPropertiesResponse = await newVersionBlob.GetPropertiesAsync();
        PrintMetadata(newPropertiesResponse);
    }
    catch (RequestFailedException e)
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
    finally
    {
        await containerClient.DeleteAsync();
    }
}

static void PrintMetadata(Response<BlobProperties> propertiesResponse)
{
    if (propertiesResponse.Value.Metadata.Count > 0)
    {
        Console.WriteLine("Metadata values for version {0}:", propertiesResponse.Value.VersionId);
        foreach (var item in propertiesResponse.Value.Metadata)
        {
            Console.WriteLine("Key:{0}  Value:{1}", item.Key, item.Value);
        }
    }
    else
    {
        Console.WriteLine("Version {0} has no metadata.", propertiesResponse.Value.VersionId);
    }
}
```

## <a name="next-steps"></a>Nächste Schritte

- [Blobversionsverwaltung](versioning-overview.md)
- [Vorläufiges Löschen für Azure Storage-Blobs](soft-delete-overview.md)
