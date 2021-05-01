---
title: Verwalten und Wiederherstellen vorläufig gelöschter Blobs
titleSuffix: Azure Storage
description: Verwalten und Wiederherstellen vorläufig gelöschter Blobs und Momentaufnahmen mit dem Azure-Portal oder mithilfe der Azure Storage-Clientbibliotheken.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 03/27/2021
ms.author: tamram
ms.subservice: blobs
ms.custom: devx-track-csharp
ms.openlocfilehash: 9d5ef85d947ae999fd94ba5a6e9cdb00baec9786
ms.sourcegitcommit: b0557848d0ad9b74bf293217862525d08fe0fc1d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/07/2021
ms.locfileid: "106555899"
---
# <a name="manage-and-restore-soft-deleted-blobs"></a>Verwalten und Wiederherstellen vorläufig gelöschter Blobs

Das Feature für das vorläufige Löschen von Blobs schützt einzelne Blobs und die zugehörigen Versionen, Momentaufnahmen und Metadaten vor einer versehentlichen Löschung oder Überschreibung, weil die gelöschten Daten für einen angegebenen Zeitraum im System beibehalten werden. Während der Beibehaltungsdauer kann das Blob in dem Zustand wiederhergestellt werden, in dem es sich zum Zeitpunkt der Löschung befand. Nach Ablauf der Beibehaltungsdauer wird das Blob endgültig gelöscht. Weitere Informationen zum vorläufigen Löschen von Blobs finden Sie unter [Vorläufiges Löschen für Blobs](soft-delete-blob-overview.md).

Das vorläufige Löschen von Blobs ist Teil einer umfassenden Datenschutzstrategie für Blobdaten. Weitere Informationen zu den Datenschutzempfehlungen von Microsoft finden Sie in der [Übersicht zum Datenschutz](data-protection-overview.md).

## <a name="manage-soft-deleted-blobs-with-the-azure-portal"></a>Verwalten von vorläufig gelöschten Blobs mit dem Azure-Portal

Sie können das Azure-Portal verwenden, um vorläufig gelöschte Blobs und Momentaufnahmen anzuzeigen und wiederherzustellen.

### <a name="view-deleted-blobs"></a>Anzeigen gelöschter Blobs

Wenn Blobs vorläufig gelöscht werden, sind sie im Azure-Portal standardmäßig nicht sichtbar. Um vorläufig gelöschte Blobs anzuzeigen, navigieren Sie zur Seite **Übersicht** für den Container, und aktivieren Sie die Einstellung **Gelöschte Blobs anzeigen**. Vorläufig gelöschte Blobs werden mit dem Status **Gelöscht** angezeigt.

:::image type="content" source="media/soft-delete-blob-manage/soft-deleted-blobs-list-portal.png" alt-text="Screenshot: Vorgehensweise zum Auflisten vorläufig gelöschter Blobs im Azure-Portal":::

Wählen Sie als Nächstes das gelöschte Blob in der Liste aus, um seine Eigenschaften anzuzeigen. Beachten Sie, dass auf der Registerkarte **Übersicht** der Blobstatus auf **Gelöscht** festgelegt ist. Das Portal zeigt außerdem an, in wie viel Tagen das Blob endgültig gelöscht wird.

:::image type="content" source="media/soft-delete-blob-manage/soft-deleted-blob-properties-portal.png" alt-text="Screenshot: Eigenschaften eines vorläufig gelöschten Blobs im Azure-Portal":::

### <a name="view-deleted-snapshots"></a>Anzeigen gelöschter Momentaufnahmen

Beim Löschen eines Blobs werden auch alle dem Blob zugeordnete Momentaufnahmen gelöscht. Wenn ein vorläufig gelöschtes Blob über Momentaufnahmen verfügt, können die gelöschten Momentaufnahmen ebenfalls im Portal angezeigt werden. Zeigen Sie die Eigenschaften des vorläufig gelöschten Blobs an, und navigieren Sie dann zur Registerkarte **Momentaufnahmen**. Aktivieren Sie dort die Option **Gelöschte Momentaufnahmen anzeigen**.

:::image type="content" source="media/soft-delete-blob-manage/soft-deleted-blob-snapshots-portal.png" alt-text="Screenshot":::

### <a name="restore-soft-deleted-objects-when-versioning-is-disabled"></a>Wiederherstellen vorläufig gelöschter Objekte bei deaktivierter Versionsverwaltung

Um ein vorläufig gelöschtes Blob im Azure-Portal wiederherzustellen, wenn die Blobversionsverwaltung deaktiviert ist, zeigen Sie zunächst die Eigenschaften des Blobs an und klicken dann auf der Registerkarte **Übersicht** auf die Schaltfläche **Wiederherstellen**. Durch das Wiederherstellen eines Blobs werden auch alle Momentaufnahmen wiederhergestellt, die während der Beibehaltungsdauer für vorläufiges Löschen gelöscht wurden.

:::image type="content" source="media/soft-delete-blob-manage/undelete-soft-deleted-blob-portal.png" alt-text="Screenshot: Wiederherstellen eines vorläufig gelöschten Blobs im Azure-Portal":::

Um eine vorläufig gelöschte Momentaufnahme zum Basisblob hochzustufen, müssen Sie zunächst sicherstellen, dass die vorläufig gelöschten Momentaufnahmen des Blobs wiederhergestellt wurden. Klicken Sie auf die Schaltfläche **Wiederherstellen**, um die vorläufig gelöschten Momentaufnahmen des Blobs wiederherzustellen. Dies gilt selbst dann, wenn das Basisblob selbst nicht vorläufig gelöscht wurde. Als Nächstes wählen Sie die Momentaufnahme auf, die höher gestuft werden soll. Klicken Sie auf die Schaltfläche **Momentaufnahme höher stufen**, um das Basisblob mit den Inhalten der Momentaufnahme zu überschreiben.

:::image type="content" source="media/soft-delete-blob-manage/promote-snapshot.png" alt-text="Screenshot: Vorgehensweise zum Höherstufen einer Momentaufnahme zum Basisblob":::

### <a name="restore-soft-deleted-blobs-when-versioning-is-enabled"></a>Wiederherstellen vorläufig gelöschter Objekte bei aktivierter Versionsverwaltung

Um ein vorläufig gelöschtes Blob im Azure-Portal wiederherzustellen, wenn die Versionsverwaltung aktiviert ist, zeigen Sie zunächst die Eigenschaften des vorläufig gelöschten Blobs an und wählen dann die Registerkarte **Versionen** aus. Wählen Sie die Version aus, die Sie zur aktuellen Version hochstufen möchten, und klicken Sie dann auf **Als aktuelle Version festlegen**.  

:::image type="content" source="media/soft-delete-blob-manage/soft-deleted-blob-promote-version-portal.png" alt-text="Screenshot: Vorgehensweise zum Höherstufen einer Version zum Wiederherstellen eines Blobs im Azure-Portal":::

Um vorläufig gelöschte Versionen oder Momentaufnahmen wiederherzustellen, wenn die Versionsverwaltung aktiviert ist, zeigen Sie die Eigenschaften des Blobs an und klicken auf der Registerkarte **Übersicht** auf die Schaltfläche **Wiederherstellen**.

> [!NOTE]
> Bei aktivierter Versionsverwaltung werden durch das Klicken auf die Schaltfläche **Wiederherstellen** für ein gelöschtes Blob alle vorläufig gelöschten Versionen oder Momentaufnahmen wiederhergestellt, nicht jedoch das Basisblob. Um das Basisblob wiederherzustellen, müssen Sie eine vorherige Version höher stufen.

## <a name="manage-soft-deleted-blobs-with-code"></a>Verwalten vorläufig gelöschter Blobs mithilfe von Code

Sie können die Azure Storage-Clientbibliotheken verwenden, um vorläufig gelöschte Blobs oder Momentaufnahmen wiederherzustellen. Die folgenden Beispiele zeigen die Verwendung der .NET-Clientbibliothek.

### <a name="restore-soft-deleted-objects-when-versioning-is-disabled"></a>Wiederherstellen vorläufig gelöschter Objekte bei deaktivierter Versionsverwaltung

# <a name="net-v12"></a>[.NET v12](#tab/dotnet)

Für das Wiederherstellen gelöschter Blobs bei aktivierter Versionsverwaltung rufen Sie den Vorgang [Undelete Blobs](/rest/api/storageservices/undelete-blob) für diese Blobs auf. Der Vorgang **Undelete Blobs** stellt vorläufig gelöschte Blobs und alle diesem Blob zugeordneten vorläufig gelöschten Momentaufnahmen wieder her.

Der Aufruf von **Undelete Blob** für einen Blob, der nicht gelöscht wurde, hat keinerlei Auswirkung. Das folgende Beispiel ruft **Undelete Blob** für alle Blobs in einem Container auf und stellt die vorläufig gelöschten Blobs und die zugehörigen Momentaufnahmen wieder her:

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/DataProtection.cs" id="Snippet_RecoverDeletedBlobs":::

Wenn Sie eine spezifische Version wiederherstellen möchten, rufen Sie den Vorgang **Undelete Blob** zunächst für das Basisblob oder die Basisversion auf und kopieren die gewünschte Version über das Basisblob. Das folgende Beispiel stellt ein Blockblob in der zuletzt gespeicherten Version wieder her:

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/DataProtection.cs" id="Snippet_RecoverSpecificBlobSnapshot":::

# <a name="net-v11"></a>[.NET v11](#tab/dotnet11)

Für das Wiederherstellen gelöschter Blobs bei aktivierter Versionsverwaltung rufen Sie den Vorgang [Undelete Blobs](/rest/api/storageservices/undelete-blob) für diese Blobs auf. Der Vorgang **Undelete Blobs** stellt vorläufig gelöschte Blobs und alle diesem Blob zugeordneten vorläufig gelöschten Momentaufnahmen wieder her.

Der Aufruf von **Undelete Blob** für einen Blob, der nicht gelöscht wurde, hat keinerlei Auswirkung. Das folgende Beispiel ruft **Undelete Blob** für alle Blobs in einem Container auf und stellt die vorläufig gelöschten Blobs und die zugehörigen Momentaufnahmen wieder her:

```csharp
// Restore all blobs in a container.
foreach (CloudBlob blob in container.ListBlobs(useFlatBlobListing: true, blobListingDetails: BlobListingDetails.Deleted))
{
       await blob.UndeleteAsync();
}
```

Wenn Sie eine spezifische Momentaufnahme wiederherstellen möchten, rufen Sie den Vorgang **Undelete Blob** zunächst für das Basisblob auf und kopieren die gewünschte Momentaufnahme über das Basisblob. Im folgenden Beispiel wird die zuletzt generierte Momentaufnahme eines Blockblobs wiederhergestellt:

```csharp
// Restore the block blob.
await blockBlob.UndeleteAsync();

// List all blobs and snapshots in the container, prefixed by the blob name.
IEnumerable<IListBlobItem> allBlobSnapshots = container.ListBlobs(
    prefix: blockBlob.Name, useFlatBlobListing: true, blobListingDetails: BlobListingDetails.Snapshots);

// Copy the most recently generated snapshot to the base blob.
CloudBlockBlob copySource = allBlobSnapshots.First(snapshot => ((CloudBlockBlob)version).IsSnapshot &&
    ((CloudBlockBlob)snapshot).Name == blockBlob.Name) as CloudBlockBlob;
blockBlob.StartCopy(copySource);
```  

---

### <a name="restore-soft-deleted-blobs-when-versioning-is-enabled"></a>Wiederherstellen vorläufig gelöschter Objekte bei aktivierter Versionsverwaltung

Um ein vorläufig gelöschtes Blob bei aktivierter Versionsverwaltung wiederherzustellen, kopieren Sie mithilfe des Vorgangs [Copy Blob](/rest/api/storageservices/copy-blob) oder [Copy Blob From URL](/rest/api/storageservices/copy-blob-from-url) eine vorherige Version über das Basisblob.  

# <a name="net-v12"></a>[.NET v12](#tab/dotnet)

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/DataProtection.cs" id="Snippet_RestorePreviousVersion":::

# <a name="net-v11"></a>[.NET v11](#tab/dotnet11)

Nicht zutreffend Die Blobversionsverwaltung wird nur in den Azure Storage-Clientbibliotheken der Version 12.x und höher unterstützt.

---

## <a name="next-steps"></a>Nächste Schritte

- [Vorläufiges Löschen für Blobspeicher](./soft-delete-blob-overview.md)
- [Aktivieren von „Vorläufiges Löschen“ für Blobs](soft-delete-blob-enable.md)
- [Blobversionsverwaltung](versioning-overview.md)
