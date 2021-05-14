---
title: Aktivieren und Erstellen großer Dateifreigaben – Azure Files
description: In diesem Artikel erfahren Sie, wie Sie große Dateifreigaben aktivieren und erstellen.
author: roygara
ms.service: storage
ms.topic: how-to
ms.date: 04/20/2021
ms.author: rogarana
ms.subservice: files
ms.custom: devx-track-azurecli, devx-track-azurepowershell
ms.openlocfilehash: a53f964020583c41e2400d97ad244bacd33813bc
ms.sourcegitcommit: 260a2541e5e0e7327a445e1ee1be3ad20122b37e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/21/2021
ms.locfileid: "107818258"
---
# <a name="enable-and-create-large-file-shares"></a>Aktivieren und Erstellen großer Dateifreigaben
Ihre Azure-Dateifreigaben können auf bis zu 100 TiB hochskaliert werden, wenn Sie große Dateifreigaben in Ihrem Speicherkonto aktivieren. Durch die Aktivierung großer Dateifreigaben erhöhen sich ggf. auch die IOPS- und Durchsatzgrenzwerte Ihrer Dateifreigabe. Sie können diese Skalierung auch in Ihren bereits vorhandenen Speicherkonten für bereits vorhandene und neue Dateifreigaben aktivieren. Ausführliche Informationen zu Leistungsunterschieden finden Sie unter [Skalierbarkeitsziele für Azure Files](storage-files-scale-targets.md#azure-files-scale-targets).

## <a name="prerequisites"></a>Voraussetzungen
- Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.
- Falls Sie die Azure CLI verwenden möchten, [installieren Sie die neueste Version](/cli/azure/install-azure-cli).
- Falls Sie das Azure PowerShell-Modul verwenden möchten, [installieren Sie die neueste Version](/powershell/azure/install-az-ps).

## <a name="restrictions"></a>Beschränkungen
Vorerst kann nur lokal redundanter Speicher (LRS) oder zonenredundanter Speicher (ZRS) für Speicherkonten mit aktivierten großen Dateifreigaben verwendet werden. Sie können keinen geozonenredundanten Speicher (GZRS), georedundanten Speicher (GRS), georedundanten Speicher mit Lesezugriff (RA-GRS) oder geozonenredundanten Speicher mit Lesezugriff (RA-GZRS) verwenden.

Die Aktivierung großer Dateifreigaben für ein Konto kann nicht rückgängig gemacht werden. Danach kann Ihr Konto nicht mehr in GZRS, GRS, RA-GRS oder RA-GZRS konvertiert werden.

## <a name="create-a-new-storage-account"></a>Erstellen eines neuen Speicherkontos

# <a name="portal"></a>[Portal](#tab/azure-portal)
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im Azure-Portal **Alle Dienste** aus. 
1. Geben Sie in der Liste der Ressourcen **Speicherkonten** ein. Die Liste wird während Ihrer Eingabe gefiltert. Wählen Sie **Speicherkonten**.
1. Wählen Sie auf dem daraufhin angezeigten Blatt **Speicherkonten** die Option **+ Neu** aus.
1. Geben Sie auf dem Blatt „Allgemeine Informationen“ die gewünschten Optionen an.
1. Achten Sie darauf, dass **Leistung** auf **Standard** festgelegt ist.
1. Legen Sie **Redundanz** entweder auf **Lokal redundanter Speicher** oder auf **Zonenredundanter Speicher** fest.
1. Wählen Sie das Blatt **Erweitert** und anschließend rechts neben **Große Dateifreigaben** das Optionsfeld **Aktiviert** aus.
1. Wählen Sie **Überprüfen + erstellen**, um die Speicherkontoeinstellungen zu überprüfen und das Konto zu erstellen.
1. Klicken Sie auf **Erstellen**.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
Um ein Speicherkonto mit aktivierten großen Dateifreigaben zu erstellen, verwenden Sie den folgenden Befehl. Ersetzen Sie `<yourStorageAccountName>`, `<yourResourceGroup>` und `<yourDesiredRegion>` durch Ihre Angaben.

```powershell
# This command creates a large file share–enabled account. It will not support changing the 
# redundancy to GRS, GZRS, RA-GRS, or RA-GZRS.
New-AzStorageAccount `
    -ResourceGroupName <yourResourceGroup> `
    -Name <yourStorageAccountName> `
    -Location <yourDesiredRegion> `
    -SkuName Standard_LRS `
    -EnableLargeFileShare
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)
Um ein Speicherkonto mit aktivierten großen Dateifreigaben zu erstellen, verwenden Sie den folgenden Befehl. Ersetzen Sie `<yourStorageAccountName>`, `<yourResourceGroup>` und `<yourDesiredRegion>` durch Ihre Angaben.

```azurecli-interactive
# This command creates a large file share–enabled account. It will not support changing the 
# redundancy to GRS, GZRS, RA-GRS, or RA-GZRS.
az storage account create \
    --name <yourStorageAccountName> \
    -g <yourResourceGroup> \
    -l <yourDesiredRegion> \
    --sku Standard_LRS \
    --kind StorageV2 \
    --enable-large-file-share
```

---

Nachdem Sie ein Speicherkonto mit aktivierter großer Dateifreigabe erstellt haben, können Sie eine Dateifreigabe erstellen, die die großen Kapazitäts- und Skalierungsgrenzen nutzt. Weitere Informationen zum Erstellen von Speicherkonten und Azure-Dateifreigaben finden Sie unter [Erstellen einer Azure-Dateifreigabe](storage-how-to-create-file-share.md).

## <a name="enable-large-files-shares-on-an-existing-account"></a>Aktivieren großer Dateifreigaben für ein vorhandenes Konto
Sie können große Dateifreigaben auch für Ihre vorhandenen LRS- und ZRS-Speicherkonten aktivieren. Wenn Sie ein GRS-, GZRS-, RA-GRS- oder RA-GZRS-Konto haben, müssen Sie es in ein LRS-Konto konvertieren, bevor Sie fortfahren.

# <a name="portal"></a>[Portal](#tab/azure-portal)
1. Öffnen Sie das [Azure-Portal](https://portal.azure.com), und navigieren Sie zu dem Speicherkonto, für das Sie große Dateifreigaben aktivieren möchten.
1. Öffnen Sie das Speicherkonto, und wählen Sie **Dateifreigaben** aus.
1. Wählen Sie für **Große Dateifreigaben** die Option **Aktiviert** und dann **Speichern** aus.
1. Wählen Sie **Übersicht** und dann **Aktualisieren** aus.
1. Wählen Sie unter **Freigabekapazität** den Wert **100 TiB** und anschließend **Speichern** aus.

    :::image type="content" source="media/storage-files-how-to-create-large-file-share/files-enable-large-file-share-existing-account.png" alt-text="Screenshot: Azure-Speicherkonto, Blatt „Dateifreigaben“ mit hervorgehobenen 100 TiB-Freigaben":::

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
Verwenden Sie den folgenden Befehl, um große Dateifreigaben für Ihr vorhandenes Konto zu aktivieren. Ersetzen Sie `<yourStorageAccountName>` und `<yourResourceGroup>` durch Ihre Angaben.

```powershell
Set-AzStorageAccount `
    -ResourceGroupName <yourResourceGroup> `
    -Name <yourStorageAccountName> `
    -EnableLargeFileShare
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)
Verwenden Sie den folgenden Befehl, um große Dateifreigaben für Ihr vorhandenes Konto zu aktivieren. Ersetzen Sie `<yourStorageAccountName>` und `<yourResourceGroup>` durch Ihre Angaben.

```azurecli-interactive
az storage account update --name <yourStorageAccountName> -g <yourResourceGroup> --enable-large-file-share
```

---

Sie haben jetzt große Dateifreigaben für Ihr Speicherkonto aktiviert. Als Nächstes müssen Sie das [Kontingent der vorhandenen Freigabe aktualisieren](#expand-existing-file-shares), um die erweiterte Kapazität und Skalierung nutzen zu können. 

> [!Important]  
> Vorhandene Dateifreigaben werden nur dann auf die angekündigten Grenzwerte für große Dateifreigaben hochskaliert, wenn das Kontingent geändert wurde.

## <a name="expand-existing-file-shares"></a>Erweitern vorhandener Dateifreigaben
Nach dem Aktivieren großer Dateifreigaben in Ihrem Speicherkonto, müssen Sie vorhandene Dateifreigaben in diesem Speicherkonto erweitern, um von der erhöhten Kapazität und Skalierung zu profitieren. 

# <a name="portal"></a>[Portal](#tab/azure-portal)
1. Wählen Sie in Ihrem Speicherkonto die Option **Dateifreigaben** aus.
1. Klicken Sie mit der rechten Maustaste auf die Dateifreigabe, und wählen Sie dann **Kontingent** aus.
1. Geben Sie die gewünschte neue Größe ein, und wählen Sie dann **OK** aus.

![Benutzeroberfläche des Azure-Portals mit „Kontingent“ für vorhandene Dateifreigaben](media/storage-files-how-to-create-large-file-share/update-large-file-share-quota.png)

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
Verwenden Sie den folgenden Befehl, um das Kontingent auf die maximale Größe festzulegen. Ersetzen Sie `<YourResourceGroupName>`, `<YourStorageAccountName>` und `<YourStorageAccountFileShareName>` durch Ihre Angaben.

```powershell
$resourceGroupName = "<YourResourceGroupName>"
$storageAccountName = "<YourStorageAccountName>"
$shareName="<YourStorageAccountFileShareName>"

# update quota
Set-AzRmStorageShare `
    -ResourceGroupName $resourceGroupName `
    -StorageAccountName $storageAccountName `
    -Name $shareName `
    -QuotaGiB 102400
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)
Verwenden Sie den folgenden Befehl, um das Kontingent auf die maximale Größe festzulegen. Ersetzen Sie `<yourResourceGroupName>`, `<yourStorageAccountName>` und `<yourFileShareName>` durch Ihre Angaben.

```azurecli-interactive
az storage share-rm update \
    --resource-group <yourResourceGroupName> \
    --storage-account <yourStorageAccountName> \
    --name <yourFileShareName> \
    --quota 102400
```

---

## <a name="next-steps"></a>Nächste Schritte
* [Einbinden einer Dateifreigabe unter Windows](storage-how-to-use-files-windows.md)
* [Einbinden einer Dateifreigabe unter Linux](storage-how-to-use-files-linux.md)
* [Einbinden einer Dateifreigabe unter macOS](storage-how-to-use-files-mac.md)