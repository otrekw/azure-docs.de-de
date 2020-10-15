---
title: Überprüfen des Verschlüsselungsstatus eines Blobs – Azure Storage
description: Erfahren Sie, wie Sie über das Azure-Portal, PowerShell oder die Azure CLI prüfen, ob ein bestimmtes Blob verschlüsselt ist. Ist ein Blob nicht verschlüsselt, erfahren Sie, wie Sie die Verschlüsselung mit AzCopy erzwingen, indem Sie das Blob herunterladen und erneut hochladen.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 11/26/2019
ms.author: tamram
ms.reviewer: ozgun
ms.subservice: common
ms.openlocfilehash: 6d0392cd8e94ba8a9026f557b90e740fbed7f50c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "84809089"
---
# <a name="check-the-encryption-status-of-a-blob"></a>Überprüfen des Verschlüsselungsstatus eines Blobs

Alle Blockblobs, Anfügeblobs und Seitenblobs, die nach dem 20. Oktober 2017 in Azure Storage geschrieben wurden, sind mit Azure Storage-Verschlüsselung verschlüsselt. Blobs, die vor diesem Datum erstellt wurden, werden weiterhin durch einen Hintergrundprozess verschlüsselt.

In diesem Artikel wird erläutert, wie Sie feststellen, ob ein bestimmtes Blob verschlüsselt wurde.

## <a name="check-a-blobs-encryption-status"></a>Überprüfen des Verschlüsselungsstatus eines Blobs

Verwenden Sie das Azure-Portal, PowerShell oder die Azure CLI, um festzustellen, ob ein Blob ohne Code verschlüsselt wurde.

### <a name="azure-portal"></a>[Azure portal](#tab/portal)

Führen Sie die folgenden Schritte aus, um mithilfe des Azure-Portals zu überprüfen, ob ein Blob verschlüsselt wurde:

1. Navigieren Sie im Azure-Portal zu Ihrem Speicherkonto.
1. Wählen Sie **Container** aus, um zu einer Liste von Containern im Konto zu navigieren.
1. Suchen Sie das Blob, und zeigen Sie dessen Registerkarte **Übersicht** an.
1. Sehen Sie sich die Eigenschaft **Server verschlüsselt** an. Wenn **True** angegeben ist (siehe folgende Abbildung), ist das Blob verschlüsselt. Beachten Sie, dass die Eigenschaften des Blobs auch das Datum und die Uhrzeit der Bloberstellung umfassen.

    ![Screenshot zum Überprüfen der Eigenschaft „Server verschlüsselte“ im Azure-Portal](media/storage-blob-encryption-status/blob-encryption-property-portal.png)

### <a name="powershell"></a>[PowerShell](#tab/powershell)

Wenn Sie mithilfe von PowerShell überprüfen möchten, ob ein Blob verschlüsselt wurde, sehen Sie sich die **IsServerEncrypted**-Eigenschaft des Blobs an. Denken Sie daran, die Platzhalterwerte in eckigen Klammern durch Ihre eigenen Werte zu ersetzen:

```powershell
$account = Get-AzStorageAccount -ResourceGroupName <resource-group> `
    -Name <storage-account>
$blob = Get-AzStorageBlob -Context $account.Context `
    -Container <container> `
    -Blob <blob>
$blob.ICloudBlob.Properties.IsServerEncrypted
```

Um festzustellen, wann das Blob erstellt wurde, sehen Sie sich den Wert der Eigenschaft **Created** an:

```powershell
$blob.ICloudBlob.Properties.IsServerEncrypted
```

### <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/cli)

Wenn Sie mithilfe der Azure CLI überprüfen möchten, ob ein Blob verschlüsselt wurde, sehen Sie sich die **IsServerEncrypted**-Eigenschaft des Blobs an. Denken Sie daran, die Platzhalterwerte in eckigen Klammern durch Ihre eigenen Werte zu ersetzen:

```azurecli-interactive
az storage blob show \
    --account-name <storage-account> \
    --container-name <container> \
    --name <blob> \
    --query "properties.serverEncrypted"
```

Um festzustellen, wann das Blob erstellt wurde, sehen Sie sich den Wert der Eigenschaft **created** an.

---

## <a name="force-encryption-of-a-blob"></a>Erzwingen der Verschlüsselung eines Blobs

Wenn ein Blob, das vor dem 20. Oktober 2017 erstellt wurde, noch nicht durch den Hintergrundprozess verschlüsselt wurde, können Sie eine sofortige Verschlüsselung erzwingen, indem Sie das Blob herunterladen und erneut hochladen. Eine einfache Möglichkeit hierfür bietet AzCopy.

Verwenden Sie die folgende Syntax, um ein Blob mit AzCopy in das lokale Dateisystem herunterzuladen:

```
azcopy copy 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<blob-path>' '<local-file-path>'

Example:
azcopy copy 'https://storagesamples.blob.core.windows.net/sample-container/blob1.txt' 'C:\temp\blob1.txt'
```

Verwenden Sie die folgende Syntax, um das Blob mit AzCopy erneut in Azure Storage hochzuladen:

```
azcopy copy '<local-file-path>' 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<blob-name>'

Example:
azcopy copy 'C:\temp\blob1.txt' 'https://storagesamples.blob.core.windows.net/sample-container/blob1.txt'
```

Weitere Informationen zum Verwenden von AzCopy zum Kopieren von Blobdaten finden Sie unter [Übertragen von Daten mit AzCopy und Blob Storage](../common/storage-use-azcopy-blobs.md).

## <a name="next-steps"></a>Nächste Schritte

[Azure Storage encryption for data at rest (Azure Storage-Verschlüsselung für ruhende Daten)](../common/storage-service-encryption.md)
