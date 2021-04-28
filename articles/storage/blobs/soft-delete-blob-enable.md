---
title: Aktivieren von „Vorläufiges Löschen“ für Blobs
titleSuffix: Azure Storage
description: Hier erfahren Sie, wie Sie vorläufiges Löschen für Blobs aktivieren, um Blobdaten vor versehentlichem Löschen oder Überschreiben zu schützen.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 03/27/2021
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: a1698a56ad7e92a59b664ce8f8bca2355fb44fb1
ms.sourcegitcommit: 2e123f00b9bbfebe1a3f6e42196f328b50233fc5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/27/2021
ms.locfileid: "108074707"
---
# <a name="enable-soft-delete-for-blobs"></a>Aktivieren von „Vorläufiges Löschen“ für Blobs

Vorläufiges Löschen von Blobs schützt ein einzelnes Blob sowie seine Versionen, Momentaufnahmen und Metadaten vor versehentlichen Lösch- oder Überschreibungsvorgängen, da die Daten für einen gewissen Zeitraum im System verbleiben. Während der Beibehaltungsdauer kann das Blob in dem Zustand wiederhergestellt werden, in dem es sich zum Zeitpunkt der Löschung befand. Nach Ablauf der Beibehaltungsdauer wird das Blob endgültig gelöscht. Weitere Informationen zum vorläufigen Löschen von Blobs finden Sie unter [Vorläufiges Löschen für Blobs](soft-delete-blob-overview.md).

Das vorläufige Löschen von Blobs ist Teil einer umfassenden Datenschutzstrategie für Blobdaten. Weitere Informationen zu den Datenschutzempfehlungen von Microsoft finden Sie in der [Datenschutzübersicht](data-protection-overview.md).

## <a name="enable-blob-soft-delete"></a>Aktivieren des vorläufigen Löschens von Blobs

Das vorläufige Löschen von Blobs ist für neue Speicherkonten standardmäßig deaktiviert. Vorläufiges Löschen kann für ein Speicherkonto jederzeit über das Azure-Portal, mithilfe von PowerShell oder über die Azure CLI aktiviert oder deaktiviert werden.

# <a name="portal"></a>[Portal](#tab/azure-portal)

Über das Azure-Portal kann das vorläufige Löschen von Blobs für Ihr Speicherkonto wie folgt aktiviert werden:

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) zu Ihrem Speicherkonto.
1. Navigieren Sie unter **Blob-Dienst** zur Option **Datenschutz**.
1. Wählen Sie im Abschnitt **Wiederherstellung** die Option **Vorläufiges Löschen für Blobs aktivieren** aus.
1. Geben Sie eine Beibehaltungsdauer zwischen einem Tag und 365 Tagen an. Microsoft empfiehlt eine Mindestbeibehaltungsdauer von sieben Tagen.
1. Speichern Sie die Änderungen.

:::image type="content" source="media/soft-delete-blob-enable/blob-soft-delete-configuration-portal.png" alt-text="Screenshot: Aktivieren des vorläufigen Löschens über das Azure-Portal":::

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Wenn Sie das vorläufige Löschen von Blobs mithilfe von PowerShell aktivieren möchten, können Sie den Befehl [Enable-AzStorageBlobDeleteRetentionPolicy](/powershell/module/az.storage/enable-azstorageblobdeleteretentionpolicy) aufrufen und dabei die Beibehaltungsdauer in Tagen angeben.

Im folgenden Beispiel wird das vorläufige Löschen von Blobs aktiviert und die Beibehaltungsdauer auf sieben Tage festgelegt. Denken Sie daran, die Platzhalterwerte in Klammern durch Ihre eigenen Werte zu ersetzen:

```azurepowershell
Enable-AzStorageBlobDeleteRetentionPolicy -ResourceGroupName <resource-group> `
    -StorageAccountName <storage-account> `
    -RetentionDays 7
```

Die aktuellen Einstellungen für das vorläufige Löschen von Blobs können mithilfe des Befehls [Get-AzStorageBlobServiceProperty](/powershell/module/az.storage/get-azstorageblobserviceproperty) überprüft werden:

```azurepowershell
$properties = Get-AzStorageBlobServiceProperty -ResourceGroupName <resource-group> `
    -StorageAccountName <storage-account>
$properties.DeleteRetentionPolicy.Enabled
$properties.DeleteRetentionPolicy.Days
```

# <a name="cli"></a>[BEFEHLSZEILENSCHNITTSTELLE (CLI)](#tab/azure-CLI)

Wenn Sie das vorläufige Löschen von Blobs mithilfe der Azure CLI aktivieren möchten, können Sie den Befehl [az storage account blob-service-properties update](/cli/azure/storage/account/blob-service-properties#az_storage_account_blob_service_properties_update) aufrufen und dabei die Beibehaltungsdauer in Tagen angeben.

Im folgenden Beispiel wird das vorläufige Löschen von Blobs aktiviert und die Beibehaltungsdauer auf sieben Tage festgelegt. Denken Sie daran, die Platzhalterwerte in Klammern durch Ihre eigenen Werte zu ersetzen:

```azurecli-interactive
az storage account blob-service-properties update --account-name <storage-account> \
    --resource-group <resource-group> \
    --enable-delete-retention true \
    --delete-retention-days 7
```

Die aktuellen Einstellungen für das vorläufige Löschen von Blobs können mithilfe des Befehls [az storage account blob-service-properties show](/cli/azure/storage/account/blob-service-properties#az_storage_account_blob_service_properties_show) überprüft werden:

```azurecli-interactive
az storage account blob-service-properties show --account-name <storage-account> \
    --resource-group <resource-group>
```

---

## <a name="next-steps"></a>Nächste Schritte

- [Vorläufiges Löschen für Blobs](soft-delete-blob-overview.md)
- [Verwalten und Wiederherstellen vorläufig gelöschter Blobs](soft-delete-blob-manage.md)
