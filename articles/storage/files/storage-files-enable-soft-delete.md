---
title: 'Aktivieren des vorläufigen Löschens: Azure-Dateifreigaben'
description: Hier erfahren Sie, wie Sie das vorläufige Löschen (Vorschau) für Azure-Dateifreigaben aktivieren, um die Wiederherstellung von Daten zu ermöglichen und versehentliches Löschen zu verhindern.
author: roygara
ms.service: storage
ms.topic: how-to
ms.date: 05/28/2020
ms.author: rogarana
ms.subservice: files
services: storage
ms.openlocfilehash: f432c544d8632a548c397b63ffa8066f63424f67
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86528382"
---
# <a name="enable-soft-delete-on-azure-file-shares"></a>Aktivieren des vorläufigen Löschens für Azure-Dateifreigaben

Azure Storage bietet vorläufiges Löschen für Dateifreigaben (Vorschau), damit Sie Ihre Daten leichter wiederherstellen können, wenn sie von einer Anwendung oder einem anderen Benutzer des Speicherkontos irrtümlich gelöscht wurden. Weitere Informationen zum vorläufigen Löschen finden Sie unter [Verhindern des versehentlichen Löschens von Azure-Dateifreigaben](storage-files-prevent-file-share-deletion.md).

In den folgenden Abschnitten erfahren Sie, wie Sie vorläufiges Löschen für Azure-Dateifreigaben eines bereits vorhandenen Speicherkontos aktivieren und verwenden:

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
1. Navigieren Sie zu Ihrem Speicherkonto, und wählen Sie unter **Dateidienst** die Option **Vorläufiges Löschen** aus.
1. Wählen Sie für **Vorläufiges Löschen von Dateifreigaben** die Option **Aktiviert** aus.
1. Wählen Sie **Aufbewahrungsdauer für Dateifreigabe in Tagen** aus, und geben Sie eine Zahl Ihrer Wahl ein.
1. Wählen Sie **Speichern** aus, um die Einstellungen für die Datenaufbewahrung zu bestätigen.

:::image type="content" source="media/storage-how-to-recover-deleted-account/enable-soft-delete-files.png" alt-text="Screenshot: Speicherkontobereich mit den Einstellungen für vorläufiges Löschen. Hervorgehoben sind der Dateifreigabebereich, die Aktivierungsoption, das Festlegen eines Aufbewahrungszeitraums, und die Speicheroption. Dadurch wird vorläufiges löschen für alle Dateifreigaben in Ihrem Speicherkonto aktiviert.":::

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Cmdlets für vorläufiges Löschen sind in der Vorschauversion 2.1.1 des Az.Storage-Moduls verfügbar. Zum Aktivieren des vorläufigen Löschens müssen die Diensteigenschaften eines Dateiclients aktualisiert werden. Im folgenden Beispiel wird das vorläufige Löschen für alle Dateifreigaben in einem Speicherkonto aktiviert:

```azurepowershell-interactive
$rgName = "yourResourceGroupName"
$accountName = "yourStorageAccountName"

Update-AzStorageFileServiceProperty -ResourceGroupName $rgName -StorageAccountName $accountName -EnableShareDeleteRetentionPolicy $true -ShareRetentionDays 7
```

Mit dem folgenden Befehl können Sie überprüfen, ob vorläufiges Löschen aktiviert ist, und die zugehörige Aufbewahrungsrichtlinie anzeigen:

```azurepowershell-interactive
Get-AzStorageFileServiceProperty -ResourceGroupName $rgName -StorageAccountName $accountName
```
---

## <a name="restore-soft-deleted-file-share"></a>Wiederherstellen einer vorläufig gelöschten Dateifreigabe

# <a name="portal"></a>[Portal](#tab/azure-portal)

So stellen Sie eine vorläufig gelöschte Dateifreigabe wieder her:

1. Navigieren Sie zu Ihrem Speicherkonto, und wählen Sie **Dateifreigaben** aus.
1. Aktivieren Sie auf dem Dateifreigabeblatt die Option **Gelöschte Freigaben anzeigen**, um vorläufig gelöschte Freigaben anzuzeigen.

    Dadurch werden alle Freigaben angezeigt, die sich aktuell im Zustand **Gelöscht** befinden.

    :::image type="content" source="media/storage-how-to-recover-deleted-account/undelete-file-share.png" alt-text="Ist die Statusspalte (die Spalte neben der Namensspalte) auf „Gelöscht“ festgelegt, befindet sich die Dateifreigabe in einem vorläufig gelöschten Zustand. In diesem Fall wird sie nach Ablauf des angegebenen Aufbewahrungszeitraums endgültig gelöscht.":::

1. Wählen Sie die Freigabe und anschließend **Wiederherstellen** aus, um sie wiederherzustellen.

    Wenn die Freigabe den Status **Aktiv** hat, wurde sie wiederhergestellt.

    :::image type="content" source="media/storage-how-to-recover-deleted-account/restored-file-share.png" alt-text="Ist die Statusspalte (die Spalte neben der Namensspalte) auf „Aktiv“ festgelegt, wurde Ihre Dateifreigabe wiederhergestellt.":::

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Cmdlets für vorläufiges Löschen sind in der Vorschauversion 2.1.1 des Az.Storage-Moduls verfügbar. Eine vorläufig gelöschte Dateifreigabe kann mithilfe des folgenden Befehls wiederhergestellt werden:

```azurepowershell-interactive
Restore-AzRmStorageShare -ResourceGroupName $rgname -StorageAccountName $accountName -DeletedShareVersion 01D5E2783BDCDA97
```
---

## <a name="disable-soft-delete"></a>Deaktivieren des vorläufigen Löschens

Wenn Sie das vorläufige Löschen nicht mehr verwenden oder eine Dateifreigabe endgültig löschen möchten, gehen Sie wie folgt vor:

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. Navigieren Sie zu Ihrem Speicherkonto, und wählen Sie unter **Einstellungen** die Option **Vorläufiges Löschen** aus.
1. Wählen Sie unter **Dateifreigaben** für **Vorläufiges Löschen von Dateifreigaben** die Option **Deaktiviert** aus.
1. Wählen Sie **Speichern** aus, um die Einstellungen für die Datenaufbewahrung zu bestätigen.

    :::image type="content" source="media/storage-how-to-recover-deleted-account/disable-soft-delete-files.png" alt-text="Wenn Sie das vorläufige Löschen deaktivieren, können Sie nach Belieben alle Dateifreigaben in Ihrem Speicherkonto sofort und endgültig löschen.":::

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Cmdlets für vorläufiges Löschen sind in der Vorschauversion 2.1.1 des Az.Storage-Moduls verfügbar. Das vorläufige Löschen für Ihr Speicherkonto kann mithilfe des folgenden Befehls deaktiviert werden:

```azurepowershell-interactive
Update-AzStorageFileServiceProperty -ResourceGroupName $rgName -StorageAccountName $accountName -EnableShareDeleteRetentionPolicy $false
```
---

## <a name="next-steps"></a>Nächste Schritte

Informationen zu einer anderen Form von Datenschutz und -wiederherstellung finden Sie in unserem Artikel [Übersicht über Freigabemomentaufnahmen für Azure Files](storage-snapshots-files.md).
