---
title: Aktivieren und Erstellen großer Dateifreigaben – Azure Files
description: In diesem Artikel erfahren Sie, wie Sie große Dateifreigaben aktivieren und erstellen.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 11/20/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 34b8af56a8f2f108b96ca07fa73f90bb9eb5bf13
ms.sourcegitcommit: 4c831e768bb43e232de9738b363063590faa0472
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/23/2019
ms.locfileid: "74422729"
---
# <a name="how-to-enable-and-create-large-file-shares"></a>Aktivieren und Erstellen großer Dateifreigaben

Ursprünglich konnten standardmäßige Dateifreigaben nur auf maximal 5 TiB skaliert werden. Ab jetzt lassen sie sich dank großer Dateifreigaben auf bis zu 100 TiB skalieren. Premium-Dateifreigaben können standardmäßig auf bis zu 100 TiB skaliert werden. 

Um unter Verwendung von Standarddateifreigaben eine Skalierung auf bis zu 100 TiB vorzunehmen, müssen Sie Ihr Speicherkonto für die Verwendung großer Dateifreigaben aktivieren. Sie können entweder ein vorhandenes Konto aktivieren oder ein neues Konto erstellen, um große Dateifreigaben zu verwenden.

## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.
- Falls Sie die Azure-Befehlszeilenschnittstelle verwenden möchten, [installieren Sie die neueste Version](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).
- Falls Sie PowerShell verwenden möchten, [installieren Sie die neueste Version](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.0.0).

## <a name="restrictions"></a>Einschränkungen

Konten, die für große Dateifreigaben aktiviert sind, unterstützen LRS oder ZRS. GZRS, GRS oder RA-GRS werden von Konten, die für große Dateifreigaben aktiviert sind, derzeit nicht unterstützt. Die Aktivierung großer Dateifreigaben für ein Konto kann nicht rückgängig gemacht werden. Danach kann Ihr Konto nicht mehr für GZRS, GRS oder RA-GRS konvertiert werden.

## <a name="create-a-new-storage-account"></a>Erstellen eines neuen Speicherkontos

### <a name="portal"></a>Portal

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Wählen Sie im Azure-Portal **Alle Dienste** aus. Geben Sie in der Liste mit den Ressourcen **Speicherkonten** ein. Sobald Sie mit der Eingabe beginnen, wird die Liste auf der Grundlage Ihrer Eingabe gefiltert. Wählen Sie **Speicherkonten**.
1. Klicken Sie im angezeigten Fenster **Speicherkonten** auf **Hinzufügen**.
1. Wählen Sie das Abonnement aus, in dem das Speicherkonto erstellt werden soll.
1. Wählen Sie unter dem Feld **Ressourcengruppe** die Option **Neu erstellen**. Geben Sie einen Namen für die neue Ressourcengruppe an, wie in der folgenden Abbildung dargestellt.

    ![Screenshot: Erstellen einer Ressourcengruppe im Portal](media/storage-files-how-to-create-large-file-share/create-large-file-share.png)

1. Geben Sie als Nächstes einen Namen für Ihr Speicherkonto ein. Der gewählte Name muss innerhalb von Azure eindeutig sein. Der Name muss ebenfalls zwischen 3 und 24 Zeichen lang sein und darf nur Zahlen und Kleinbuchstaben enthalten.
1. Wählen Sie einen Speicherort für Ihr Speicherkonto aus, und vergewissern Sie sich, dass es sich um eine der [für LFS unterstützten Regionen](storage-files-planning.md#regional-availability) handelt.
1. Legen Sie die Replikation auf **lokal redundanten Speicher** oder **zonenredundanten Speicher** fest.
1. Behalten Sie die Standardwerte für diese Felder bei:

   |Feld  |Wert  |
   |---------|---------|
   |Bereitstellungsmodell     |Ressourcen-Manager         |
   |Leistung     |Standard         |
   |Kontoart     |StorageV2 (universell v2)         |
   |Zugriffsebene     |Heiß         |

1. Wählen Sie erst **Erweitert** und dann für **große Dateifreigaben** die Option **Aktiviert** aus.
1. Wählen Sie **Überprüfen + erstellen**, um die Speicherkontoeinstellungen zu überprüfen und das Konto zu erstellen.

    ![large-file-shares-advanced-enable.png](media/storage-files-how-to-create-large-file-share/large-file-shares-advanced-enable.png)

1. Klicken Sie auf **Erstellen**.


### <a name="cli"></a>Befehlszeilenschnittstelle (CLI)

[Installieren Sie zunächst die neueste Version](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest), um dieses Feature aktivieren zu können.

Um ein Speicherkonto mit aktivierten großen Dateifreigaben zu erstellen, ersetzen Sie `<yourStorageAccountName>`, `<yourResourceGroup>`und `<yourDesiredRegion>` durch Ihre Werte, und verwenden Sie anschließend den folgenden Befehl:

```azurecli-interactive
## This command creates a large file share enabled account, it will not support GZRS, GRS, or RA-GRS
az storage account create –name <yourStorageAccountName> -g <yourResourceGroup> -l <yourDesiredRegion> –sku Standard_LRS --kind StorageV2 –enable-large-file-share
```

### <a name="powershell"></a>PowerShell

[Installieren Sie zunächst die neueste Version](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.0.0), um dieses Feature aktivieren zu können.

Um ein Speicherkonto mit aktivierten großen Dateifreigaben zu erstellen, ersetzen Sie `<yourStorageAccountName>`, `<yourResourceGroup>`und `<yourDesiredRegion>` durch Ihre Werte, und verwenden Sie anschließend den folgenden Befehl:

```PowerShell
## This command creates a large file share enabled account, it will not support GZRS, GRS, or RA-GRS
New-AzStorageAccount -ResourceGroupName <yourResourceGroup> -Name <yourStorageAccountName> -Location <yourDesiredRegion> -SkuName Standard_LRS -EnableLargeFileShare;
```

## <a name="enable-on-existing-account"></a>Aktivieren für ein vorhandenes Konto

### <a name="portal"></a>Portal

Sie können große Dateifreigaben auch für vorhandene Konten aktivieren. In diesem Fall kann für das Konto keine Konvertierung mehr in GZRS, GRS oder RA-GRS durchgeführt werden. Die Auswahl kann für dieses Konto nicht rückgängig gemacht werden.

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com), und navigieren Sie zu dem Speicherkonto, für das Sie große Dateifreigaben aktivieren möchten.
1. Öffnen Sie das Speicherkonto, und wählen Sie **Konfiguration** aus.
1. Wählen Sie für große Dateifreigaben die Option **Aktiviert** aus, und wählen Sie dann **Speichern** aus.
1. Wählen Sie **Übersicht** und dann **Aktualisieren** aus.

![enable-large-file-shares-on-existing.png](media/storage-files-how-to-create-large-file-share/enable-large-file-shares-on-existing.png)

Sie haben jetzt große Dateifreigaben für Ihr Speicherkonto aktiviert.

Falls Sie die Fehlermeldung „Große Dateifreigaben sind für das Konto noch nicht verfügbar“ erhalten, können Sie eine Weile warten, da sich Ihre Region wahrscheinlich gerade mitten im Rollout befindet. In dringenden Fällen können Sie sich an den Support wenden.

### <a name="cli"></a>Befehlszeilenschnittstelle (CLI)

Sie können große Dateifreigaben für Ihre vorhandenen Konten aktivieren. In diesem Fall kann für das Konto keine Konvertierung mehr in GZRS, GRS oder RA-GRS durchgeführt werden. Die Auswahl kann für dieses Konto nicht rückgängig gemacht werden.

Ersetzen Sie `<yourStorageAccountName>` und `<yourResourceGroup>` im folgenden Befehl, und führen Sie ihn anschließend aus, um große Dateifreigaben für Ihr vorhandenes Konto zu aktivieren:

```azurecli-interactive
az storage account update –name <yourStorageAccountName> -g <yourResourceGroup> –enable-large-file-share
```

### <a name="powershell"></a>PowerShell

Sie können große Dateifreigaben für Ihre vorhandenen Konten aktivieren. In diesem Fall kann für das Konto keine Konvertierung mehr in GZRS, GRS oder RA-GRS durchgeführt werden. Die Auswahl kann für dieses Konto nicht rückgängig gemacht werden.

Ersetzen Sie `<yourStorageAccountName>` und `<yourResourceGroup>` im folgenden Befehl, und führen Sie ihn anschließend aus, um große Dateifreigaben für Ihr vorhandenes Konto zu aktivieren:

```PowerShell
Set-AzStorageAccount -ResourceGroupName <yourResourceGroup> -Name <yourStorageAccountName> -EnableLargeFileShare
```

## <a name="create-a-large-file-share"></a>Erstellen einer großen Dateifreigabe

### <a name="portal"></a>Portal

Das Erstellen einer großen Dateifreigabe ist nahezu identisch mit dem Erstellen einer standardmäßigen Dateifreigabe. Der Hauptunterschied besteht darin, dass Sie ein Kontingent von bis zu 100 TiB festlegen können.

1. Wählen Sie in Ihrem Speicherkonto die Option **Dateifreigaben** aus.
1. Klicken Sie auf **+ Dateifreigabe**.
1. Geben Sie einen Namen für die Dateifreigabe und (optional) die gewünschte Kontingentgröße (bis zu 100 TiB) ein, und wählen Sie dann **Erstellen** aus. 

![large-file-shares-create-share.png](media/storage-files-how-to-create-large-file-share/large-file-shares-create-share.png)

### <a name="cli"></a>Befehlszeilenschnittstelle (CLI)

Nachdem Sie große Dateifreigaben für Ihr Speicherkonto aktiviert haben, können Sie darin Dateifreigaben mit höheren Kontingenten erstellen. Ersetzen Sie `<yourStorageAccountName>`, `<yourStorageAccountKey>` und `<yourFileShareName>` im folgenden Befehl durch Ihre Werte, und führen Sie ihn anschließend aus, um eine große Dateifreigabe zu erstellen:

```azurecli-interactive
az storage share create --account-name <yourStorageAccountName> --account-key <yourStorageAccountKey> --name <yourFileShareName>
```

### <a name="powershell"></a>PowerShell

Nachdem Sie große Dateifreigaben für Ihr Speicherkonto aktiviert haben, können Sie darin Dateifreigaben mit höheren Kontingenten erstellen. Ersetzen Sie `<YourStorageAccountName>`, `<YourStorageAccountKey>` und `<YourStorageAccountFileShareName>` im folgenden Befehl durch Ihre Werte, und führen Sie ihn anschließend aus, um eine große Dateifreigabe zu erstellen:

```PowerShell
##Config
$storageAccountName = "<YourStorageAccountName>"
$storageAccountKey = "<YourStorageAccountKey>"
$shareName="<YourStorageAccountFileShareName>"
$ctx = New-AzStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
New-AzStorageShare -Name $shareName -Context $ctx
```

## <a name="expand-existing-file-shares"></a>Erweitern vorhandener Dateifreigaben

### <a name="portal"></a>Portal

Sobald Sie große Dateifreigaben in Ihrem Speicherkonto aktiviert haben, können Sie vorhandene Freigaben auf das höhere Kontingent erweitern.

1. Wählen Sie in Ihrem Speicherkonto die Option **Dateifreigaben** aus.
1. Klicken Sie mit der rechten Maustaste auf die Dateifreigabe, und wählen Sie **Kontingent** aus.
1. Geben Sie die gewünschte neue Größe ein, und wählen Sie dann **OK** aus.

![update-large-file-share-quota.png](media/storage-files-how-to-create-large-file-share/update-large-file-share-quota.png)

### <a name="cli"></a>Befehlszeilenschnittstelle (CLI)

Nachdem Sie große Dateifreigaben für Ihr Speicherkonto aktiviert haben, können Sie vorhandene Dateifreigaben in diesem Konto auf die höheren Kontingente erweitern. Ersetzen Sie `<yourStorageAccountName>`, `<yourStorageAccountKey>` und `<yourFileShareName>` im folgenden Befehl durch Ihre Werte, und führen Sie ihn anschließend aus, um das Kontingent auf die maximale Größe festzulegen:

```azurecli-interactive
az storage share update --account-name <yourStorageAccountName> --account-key <yourStorageAccountKey> --name <yourFileShareName> --quota 102400
```

### <a name="powershell"></a>PowerShell

Nachdem Sie große Dateifreigaben für Ihr Speicherkonto aktiviert haben, können Sie vorhandene Dateifreigaben in diesem Konto auf die höheren Kontingente erweitern. Ersetzen Sie `<YourStorageAccountName>`, `<YourStorageAccountKey>` und `<YourStorageAccountFileShareName>` im folgenden Befehl durch Ihre Werte, und führen Sie ihn anschließend aus, um das Kontingent auf die maximale Größe festzulegen:

```PowerShell
##Config
$storageAccountName = "<YourStorageAccountName>"
$storageAccountKey = "<YourStorageAccountKey>"
$shareName="<YourStorageAccountFileShareName>"
$ctx = New-AzStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
# update quota
Set-AzStorageShareQuota -ShareName $shareName -Context $ctx -Quota 102400
```

## <a name="next-steps"></a>Nächste Schritte

* [Einbinden einer Azure-Dateifreigabe und Zugreifen auf die Freigabe unter Windows](storage-how-to-use-files-windows.md)
* [Verwenden des Azure-Dateispeichers unter Linux](../storage-how-to-use-files-linux.md)
* [Einbinden einer Azure-Dateifreigabe über SMB mit macOS](storage-how-to-use-files-mac.md)