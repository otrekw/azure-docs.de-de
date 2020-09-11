---
title: Kopieren eines Blobs mit .NET – Azure Storage
description: Hier erfahren Sie, wie Sie ein Blob in Ihrem Azure Storage-Konto mithilfe der .NET-Clientbibliothek kopieren.
services: storage
author: mhopkins-msft
ms.author: mhopkins
ms.date: 09/01/2020
ms.service: storage
ms.subservice: blobs
ms.topic: how-to
ms.custom: devx-track-csharp
ms.openlocfilehash: a7ca195bdfb05baff6100b3481903f9ca0841dc6
ms.sourcegitcommit: 5ed504a9ddfbd69d4f2d256ec431e634eb38813e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89320663"
---
# <a name="copy-a-blob-with-net"></a>Kopieren eines Blobs mit .NET

In diesem Artikel wird beschrieben, wie Sie ein Blob mit einem Azure Storage-Konto kopieren. Außerdem wird gezeigt, wie ein asynchroner Kopiervorgang abgebrochen wird. Im Beispielcode wird die [Azure Storage-Clientbibliothek für .NET](/dotnet/api/overview/azure/storage) verwendet.

## <a name="about-copying-blobs"></a>Informationen zum Kopieren von Blobs

Wenn Sie ein Blob innerhalb desselben Speicherkontos kopieren, ist dies ein synchroner Vorgang. Wenn Sie zwischen Konten kopieren, ist es ein asynchroner Vorgang.

Das Quellblob für einen Kopiervorgang kann ein Blockblob, ein Anfügeblob, ein Seitenblob oder eine Momentaufnahme sein. Wenn das Zielblob bereits vorhanden ist, muss es von demselben Blobtyp wie das Quellblob sein. Ein eventuell vorhandenes Zielblob wird überschrieben.

Das Zielblob kann während der Ausführung eines Kopiervorgangs nicht geändert werden. Bei einem Zielblob kann es nur einen ausstehenden Vorgang zum Kopieren des Blobs geben. Anders ausgedrückt: Ein Blob kann nicht das Ziel für mehrere ausstehende Kopiervorgänge sein.

Es wird immer das gesamte Quellblob oder die gesamte Quelldatei kopiert. Das Kopieren eines Bytebereichs oder einer Gruppe von Blöcken wird nicht unterstützt.

Wenn ein Blob kopiert wird, werden dessen Systemeigenschaften mit denselben Werten in das Zielblob kopiert.

Ein Kopiervorgang kann eine der folgenden Formen haben:

- Sie können ein Quellblob in ein Zielblob mit einem anderen Namen kopieren. Das Zielblob kann ein vorhandenes Blob desselben Blobtyps sein (Blockblob, Anfügeblob oder Seitenblob), oder es kann sich um ein neues Blob handeln, das durch den Kopiervorgang erstellt wurde.
- Sie können ein Quellblob in ein Zielblob mit demselben Namen kopieren und so das Zielblob effektiv ersetzen. Ein solcher Kopiervorgang entfernt alle Blöcke ohne Commit und überschreibt die Metadaten des Zielblobs.
- Sie können eine Quelldatei im Azure-Dateidienst in ein Zielblob kopieren. Das Zielblob kann ein vorhandenes Blockblob oder ein neues Blockblob sein, das durch den Kopiervorgang erstellt wurde. Das Kopieren von Dateien in Seitenblobs oder Anfügeblobs wird nicht unterstützt.
- Sie können eine Momentaufnahme über das zugehörige Basis-Blob kopieren. Indem Sie eine Momentaufnahme zu einem Basis-Blob heraufstufen, können Sie eine frühere Version eines Blobs wiederherstellen.
- Sie können eine Momentaufnahme in ein Ziel-Blob mit einem anderen Namen kopieren. Das resultierende Zielblob ist ein beschreibbares Blob und keine Momentaufnahme.

## <a name="copy-a-blob"></a>Kopieren eines Blobs

# <a name="net-v12"></a>[.NET v12](#tab/dotnet)

Zum Kopieren eines Blobs rufen Sie eine der folgenden Methoden auf:

- [StartCopyFromUri](/dotnet/api/azure.storage.blobs.specialized.blobbaseclient.startcopyfromuri)
- [StartCopyFromUriAsync](/dotnet/api/azure.storage.blobs.specialized.blobbaseclient.startcopyfromuriasync)

Die Methoden **StartCopyFromUri** und **StartCopyFromUriAsync** geben ein [CopyFromUriOperation](/dotnet/api/azure.storage.blobs.models.copyfromurioperation)-Objekt zurück, das Informationen über den Kopiervorgang enthält.

Im folgenden Codebeispiel wird ein [BlobClient](/dotnet/api/azure.storage.blobs.blobclient) abgerufen, der ein zuvor erstelltes Blob darstellt und in demselben Container in ein neues Blob kopiert:

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/CopyBlob.cs" id="Snippet_CopyBlob":::

# <a name="net-v11"></a>[.NET v11](#tab/dotnet11)

Zum Kopieren eines Blobs rufen Sie eine der folgenden Methoden auf:

- [StartCopy](/dotnet/api/microsoft.azure.storage.blob.cloudblob.startcopy)
- [StartCopyAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblob.startcopyasync)

Die Methoden **StartCopy** und **StartCopyAsync** geben einen Wert für die Kopier-ID zurück, der zum Überprüfen des Status oder zum Abbrechen des Kopiervorgangs verwendet wird.

Im folgenden Codebeispiel wird ein Verweis auf ein zuvor erstelltes Blob abgerufen und in ein neues Blob in demselben Container kopiert:

```csharp
private static async Task CopyBlockBlobAsync(CloudBlobContainer container)
{
    CloudBlockBlob sourceBlob = null;
    CloudBlockBlob destBlob = null;
    string leaseId = null;

    try
    {
        // Get a block blob from the container to use as the source.
        sourceBlob = container.ListBlobs().OfType<CloudBlockBlob>().FirstOrDefault();

        // Lease the source blob for the copy operation 
        // to prevent another client from modifying it.
        // Specifying null for the lease interval creates an infinite lease.
        leaseId = await sourceBlob.AcquireLeaseAsync(null);

        // Get a reference to a destination blob (in this case, a new blob).
        destBlob = container.GetBlockBlobReference("copy of " + sourceBlob.Name);

        // Ensure that the source blob exists.
        if (await sourceBlob.ExistsAsync())
        {
            // Get the ID of the copy operation.
            string copyId = await destBlob.StartCopyAsync(sourceBlob);

            // Fetch the destination blob's properties before checking the copy state.
            await destBlob.FetchAttributesAsync();

            Console.WriteLine("Status of copy operation: {0}", destBlob.CopyState.Status);
            Console.WriteLine("Completion time: {0}", destBlob.CopyState.CompletionTime);
            Console.WriteLine("Bytes copied: {0}", destBlob.CopyState.BytesCopied.ToString());
            Console.WriteLine("Total bytes: {0}", destBlob.CopyState.TotalBytes.ToString());
        }
    }
    catch (StorageException e)
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
    finally
    {
        // Break the lease on the source blob.
        if (sourceBlob != null)
        {
            await sourceBlob.FetchAttributesAsync();

            if (sourceBlob.Properties.LeaseState != LeaseState.Available)
            {
                await sourceBlob.BreakLeaseAsync(new TimeSpan(0));
            }
        }
    }
}
```

---

## <a name="abort-a-blob-copy-operation"></a>Abbrechen eines Vorgangs zum Kopieren des Blobs

Das Abbrechen eines Kopiervorgangs führt zu einem Zielblob der Länge 0 (null) für Blockblobs, Anfügeblobs und Seitenblobs. Die Metadaten für das Zielblob weisen jedoch die neuen Werte auf, die aus dem Quellblob kopiert oder explizit während des Kopiervorgangs festgelegt wurden. Zur Beibehaltung der ursprünglichen, vor dem Kopiervorgang vorliegenden Metadaten erstellen Sie eine Momentaufnahme des Zielblobs, bevor Sie eine der Kopiermethoden aufrufen.

# <a name="net-v12"></a>[.NET v12](#tab/dotnet)

Sie können die Eigenschaft [BlobProperties.CopyStatus](/dotnet/api/azure.storage.blobs.models.blobproperties.copystatus) im Zielblob überprüfen, um den Status des Kopiervorgangs zu erfahren. Das endgültige Blob wird bei Abschluss des Kopiervorgangs committet.

Wenn Sie einen Vorgang zum Kopieren des Blobs abbrechen, wird der Kopierstatus des Zielblobs auf [CopyStatus.Aborted](/dotnet/api/microsoft.azure.storage.blob.copystatus) festgelegt.

Die Methoden [AbortCopyFromUri](/dotnet/api/azure.storage.blobs.specialized.blobbaseclient.abortcopyfromuri) und [AbortCopyFromUriAsync](/dotnet/api/azure.storage.blobs.specialized.blobbaseclient.abortcopyfromuriasync) brechen einen laufenden Blobkopiervorgang ab.

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/CopyBlob.cs" id="Snippet_StopBlobCopy":::

# <a name="net-v11"></a>[.NET v11](#tab/dotnet11)

Sie können die Eigenschaft [CopyState.Status](/dotnet/api/microsoft.azure.storage.blob.copystate.status) im Zielblob überprüfen, um den Status des Kopiervorgangs zu erfahren. Das endgültige Blob wird bei Abschluss des Kopiervorgangs committet.

Wenn Sie einen Vorgang zum Kopieren des Blobs abbrechen, wird der Kopierstatus des Zielblobs auf [CopyStatus.Aborted](/dotnet/api/microsoft.azure.storage.blob.copystatus) festgelegt.

Die Methoden [AbortCopy](/dotnet/api/microsoft.azure.storage.blob.cloudblob.abortcopy) und [AbortCopyAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblob.abortcopyasync) brechen einen laufenden Blobkopiervorgang ab.

```csharp
// Fetch the destination blob's properties before checking the copy state.
await destBlob.FetchAttributesAsync();

// Check the copy status. If it is still pending, abort the copy operation.
if (destBlob.CopyState.Status == CopyStatus.Pending)
{
    await destBlob.AbortCopyAsync(copyId);
    Console.WriteLine("Copy operation {0} has been aborted.", copyId);
}
```

---

[!INCLUDE [storage-blob-dotnet-resources-include](../../../includes/storage-blob-dotnet-resources-include.md)]

## <a name="next-steps"></a>Nächste Schritte

Die folgenden Themen enthalten Informationen zum Kopieren von Blobs und zum Abbrechen laufender Kopiervorgänge mithilfe der Azure-REST-APIs.

- [Copy Blob](/rest/api/storageservices/copy-blob)
- [Abort Copy Blob](/rest/api/storageservices/abort-copy-blob)
