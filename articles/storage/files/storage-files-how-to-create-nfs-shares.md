---
title: Erstellen einer NFS-Freigabe – Azure Files (Vorschau)
description: In diesem Artikel erfahren Sie, wie Sie eine Azure-Dateifreigabe erstellen, die mithilfe des NFS-Protokolls (Network File System) eingebunden werden kann.
author: roygara
ms.service: storage
ms.topic: how-to
ms.date: 01/22/2021
ms.author: rogarana
ms.subservice: files
ms.custom: references_regions, devx-track-azurecli
ms.openlocfilehash: dc23dec8a8d59a7762e93cdfaa2a39d824506e7b
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2021
ms.locfileid: "100382122"
---
# <a name="how-to-create-an-nfs-share"></a>Erstellen einer NFS-Freigabe
Azure-Dateifreigaben sind vollständig verwaltete Dateifreigaben, die in der Cloud gespeichert werden. In diesem Artikel geht es um das Erstellen einer Dateifreigabe, für die das NFS-Protokoll verwendet wird. Weitere Informationen zu den beiden Protokollen finden Sie unter [Protokolle für Azure-Dateifreigaben](storage-files-compare-protocols.md).

## <a name="limitations"></a>Einschränkungen
[!INCLUDE [files-nfs-limitations](../../../includes/files-nfs-limitations.md)]

### <a name="regional-availability"></a>Regionale Verfügbarkeit
[!INCLUDE [files-nfs-regional-availability](../../../includes/files-nfs-regional-availability.md)]

## <a name="prerequisites"></a>Voraussetzungen
- Der Zugriff auf NFS-Freigaben ist nur über vertrauenswürdige Netzwerke möglich. Verbindungen zu Ihrer NFS-Freigabe müssen einer der folgenden Quellen entstammen:
    - Entweder [Erstellen eines privaten Endpunkts](storage-files-networking-endpoints.md#create-a-private-endpoint) (empfohlen) oder [Einschränken des Zugriffs auf den öffentlichen Endpunkt](storage-files-networking-endpoints.md#restrict-public-endpoint-access)
    - [Konfigurieren eines P2S-VPN (Point-to-Site) unter Linux zur Verwendung mit Azure Files](storage-files-configure-p2s-vpn-linux.md)
    - [Konfigurieren eines Site-to-Site-VPN zur Verwendung mit Azure Files](storage-files-configure-s2s-vpn.md)
    - Konfigurieren von [ExpressRoute](../../expressroute/expressroute-introduction.md)

- Falls Sie die Azure CLI verwenden möchten, [installieren Sie die neueste Version](/cli/azure/install-azure-cli?view=azure-cli-latest&preserve-view=true).

## <a name="register-the-nfs-41-protocol"></a>Registrieren des NFS 4.1-Protokolls
Wenn Sie das Azure PowerShell-Modul oder die Azure CLI nutzen, registrieren Sie Ihr Feature mithilfe der folgenden Befehle:

# <a name="portal"></a>[Portal](#tab/azure-portal)
Verwenden Sie Azure PowerShell oder die Azure-Befehlszeilenschnittstelle, um die NFS 4.1-Funktionen für Azure Files zu registrieren.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
```azurepowershell
# Connect your PowerShell session to your Azure account, if you have not already done so.
Connect-AzAccount

# Set the actively selected subscription, if you have not already done so.
$subscriptionId = "<yourSubscriptionIDHere>"
$context = Get-AzSubscription -SubscriptionId $subscriptionId
Set-AzContext $context

# Register the NFS 4.1 feature with Azure Files to enable the preview.
Register-AzProviderFeature `
    -ProviderNamespace Microsoft.Storage `
    -FeatureName AllowNfsFileShares 
    
Register-AzResourceProvider -ProviderNamespace Microsoft.Storage
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)
```azurecli
# Connect your Azure CLI to your Azure account, if you have not already done so.
az login

# Provide the subscription ID for the subscription where you would like to 
# register the feature
subscriptionId="<yourSubscriptionIDHere>"

az feature register \
    --name AllowNfsFileShares \
    --namespace Microsoft.Storage \
    --subscription $subscriptionId

az provider register \
    --namespace Microsoft.Storage
```

---

Die Registrierungsgenehmigung kann bis zu einer Stunde dauern. Verwenden Sie die folgenden Befehle, um zu überprüfen, ob die Registrierung abgeschlossen wurde:

# <a name="portal"></a>[Portal](#tab/azure-portal)
Verwenden Sie Azure PowerShell oder die Azure-Befehlszeilenschnittstelle, um die Registrierung der NFS 4.1-Funktionen für Azure Files zu überprüfen. 

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
```azurepowershell
Get-AzProviderFeature `
    -ProviderNamespace Microsoft.Storage `
    -FeatureName AllowNfsFileShares
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)
```azurecli
az feature show \
    --name AllowNfsFileShares \
    --namespace Microsoft.Storage \
    --subscription $subscriptionId
```

---

## <a name="create-a-filestorage-storage-account"></a>Erstellen eines FileStorage-Speicherkontos
Derzeit sind NFS 4.1-Freigaben nur als Premium-Dateifreigaben verfügbar. Wenn Sie eine Premium-Dateifreigabe mit Unterstützung für das Protokoll NFS 4.1 bereitstellen möchten, müssen Sie zunächst ein FileStorage-Speicherkonto erstellen. Ein Speicherkonto ist ein Objekt auf oberster Ebene in Azure, das einen freigegebenen Speicherpool darstellt, der zum Bereitstellen mehrerer Azure-Dateifreigaben verwendet werden kann.

# <a name="portal"></a>[Portal](#tab/azure-portal)
Navigieren Sie zum Erstellen eines FileStorage-Speicherkontos zum Azure-Portal.

1. Wählen Sie im Azure-Portal im linken Menü **Speicherkonten** aus.

    ![Azure-Portal, Hauptseite „Speicherkonto auswählen“](media/storage-how-to-create-premium-fileshare/azure-portal-storage-accounts.png)

2. Klicken Sie im angezeigten Fenster **Speicherkonten** auf **Hinzufügen**.
3. Wählen Sie das Abonnement aus, in dem das Speicherkonto erstellt werden soll.
4. Wählen Sie die Ressourcengruppe aus, in der das Speicherkonto erstellt werden soll.

5. Geben Sie als Nächstes einen Namen für Ihr Speicherkonto ein. Der gewählte Name muss innerhalb von Azure eindeutig sein. Der Name muss ebenfalls zwischen 3 und 24 Zeichen lang sein und darf nur Zahlen und Kleinbuchstaben enthalten.
6. Wählen Sie einen Standort für Ihr Speicherkonto aus, oder verwenden Sie den Standardstandort.
7. Wählen Sie für **Leistung** den Wert **Premium** aus.

    Sie müssen **Premium** auswählen, damit **FileStorage** in der Dropdownliste **Kontoart** verfügbar ist.

8. Wählen Sie **Kontoart** und dann **FileStorage** aus.
9. Behalten Sie für **Replikation** den Standardwert **Lokal redundanter Speicher (LRS)** bei.

    ![So erstellen ein Speicherkonto für eine Premium-Dateifreigabe](media/storage-how-to-create-premium-fileshare/create-filestorage-account.png)

10. Wählen Sie **Überprüfen + erstellen**, um die Speicherkontoeinstellungen zu überprüfen und das Konto zu erstellen.
11. Klicken Sie auf **Erstellen**.

Sobald Ihre Speicherkontoressource erstellt wurde, navigieren Sie dorthin.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
Um ein FileStorage-Speicherkonto zu erstellen, öffnen Sie eine PowerShell-Eingabeaufforderung, und führen Sie die folgenden Befehle aus. Ersetzen Sie dabei `<resource-group>` und `<storage-account>` durch die entsprechenden Werte für Ihre Umgebung.

```powershell
$resourceGroupName = "<resource-group>"
$storageAccountName = "<storage-account>"
$location = "westus2"

$storageAccount = New-AzStorageAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $storageAccountName `
    -SkuName Premium_LRS `
    -Location $location `
    -Kind FileStorage
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)
Um ein FileStorage-Speicherkonto zu erstellen, öffnen Sie Ihr Terminal, und führen Sie die folgenden Befehle aus. Ersetzen Sie dabei `<resource-group>` und `<storage-account>` durch die entsprechenden Werte für Ihre Umgebung.

```azurecli-interactive
resourceGroup="<resource-group>"
storageAccount="<storage-account>"
location="westus2"

az storage account create \
    --resource-group $resourceGroup \
    --name $storageAccount \
    --location $location \
    --sku Premium_LRS \
    --kind FileStorage
```
---

## <a name="create-an-nfs-share"></a>Erstellen einer NFS-Freigabe

# <a name="portal"></a>[Portal](#tab/azure-portal)

Nachdem Sie nun ein FileStorage-Konto erstellt und das Netzwerk konfiguriert haben, können Sie eine NFS-Dateifreigabe erstellen. Der Prozess ähnelt dem Erstellen einer SMB-Freigabe, Sie wählen jedoch anstelle von **SMB** beim Erstellen der Freigabe **NFS** aus.

1. Navigieren Sie zu Ihrem Speicherkonto, und wählen Sie **Dateifreigaben** aus.
1. Wählen Sie **+ Dateifreigabe** aus, um eine neue Dateifreigabe zu erstellen.
1. Benennen Sie Ihre Dateifreigabe, und wählen Sie eine bereitgestellte Kapazität aus.
1. Wählen Sie für **Protokoll** die Option **NFS (Vorschau)** aus.
1. Treffen Sie für **Root-Squash** eine Auswahl.

    - Root-Squash (Standardwert): Zugriff für den Remotesuperuser (Stamm) wird UID (65534) und GID (65534) zugeordnet.
    - Kein Root-Squash: Remotesuperuser (Stamm) erhält Zugriff als Stamm.
    - All Squash: Der gesamte Benutzerzugriff wird UID (65534) und GID (65534) zugeordnet.
    
1. Klicken Sie auf **Erstellen**.

    :::image type="content" source="media/storage-files-how-to-create-mount-nfs-shares/create-nfs-file-share.png" alt-text="Screenshots des Blatts für die Erstellung einer Dateifreigabe":::

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

1. Stellen Sie sicher, dass .NET Framework installiert ist. Siehe [Download .NET Framework](https://dotnet.microsoft.com/download/dotnet-framework) (Herunterladen von .NET Framework).
 
1. Überprüfen Sie mit dem folgenden Befehl, ob PowerShell Version `5.1` oder höher installiert ist.    

   ```powershell
   echo $PSVersionTable.PSVersion.ToString() 
   ```
    
   Informationen zum Aktualisieren der PowerShell-Version finden Sie unter [Aktualisieren einer vorhandenen Windows PowerShell-Version](/powershell/scripting/install/installing-windows-powershell?view=powershell-6&preserve-view=true#upgrading-existing-windows-powershell).
    
1. Installieren Sie die aktuelle Version des PowerShellGet-Moduls.

   ```powershell
   install-Module PowerShellGet –Repository PSGallery –Force  
   ```

1. Schließen Sie die PowerShell-Konsole, und öffnen Sie sie dann erneut.

1. Installieren Sie die Version **2.5.2-preview** des **Az.Storage**-Vorschaumoduls:

   ```powershell
   Install-Module Az.Storage -Repository PsGallery -RequiredVersion 2.5.2-preview -AllowClobber -AllowPrerelease -Force  
   ```

   Weitere Informationen zum Installieren von PowerShell-Modulen finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-az-ps?view=azps-3.0.0&preserve-view=true).
   
1. Im Azure PowerShell-Modul verwenden Sie das Cmdlet [New-AzRmStorageShare](/powershell/module/az.storage/new-azrmstorageshare) zum Erstellen einer Premium-Dateifreigabe.

    > [!NOTE]
    > Premium-Dateifreigaben werden über ein bereitgestelltes Modell abgerechnet. Die bereitgestellte Größe der Freigabe wird unten durch `QuotaGiB` angegeben. Weitere Informationen finden Sie unter [Grundlegendes zu dem bereitgestellten Modell](understanding-billing.md#provisioned-model) und auf der Seite [Azure Files – Preise](https://azure.microsoft.com/pricing/details/storage/files/).

    ```powershell
    New-AzRmStorageShare `
        -StorageAccount $storageAccount `
        -Name myshare `
        -EnabledProtocol NFS `
        -RootSquash RootSquash `
        -QuotaGiB 1024
    ```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)
In der Azure CLI verwenden Sie den Befehl [az storage share create](/cli/azure/storage/share-rm) zum Erstellen einer Premium-Dateifreigabe.

> [!NOTE]
> Premium-Dateifreigaben werden über ein bereitgestelltes Modell abgerechnet. Die bereitgestellte Größe der Freigabe wird unten durch `quota` angegeben. Weitere Informationen finden Sie unter [Grundlegendes zu dem bereitgestellten Modell](understanding-billing.md#provisioned-model) und auf der Seite [Azure Files – Preise](https://azure.microsoft.com/pricing/details/storage/files/).

```azurecli-interactive
az storage share-rm create \
    --resource-group $resourceGroup \
    --storage-account $storageAccount \
    --name "myshare" \
    --enabled-protocol NFS \
    --root-squash RootSquash \
    --quota 1024
```
---

## <a name="next-steps"></a>Nächste Schritte
Da Sie nun eine NFS-Freigabe erstellt haben, müssen Sie sie in Ihren Linux-Client einbinden, um sie verwenden zu können. Weitere Informationen dazu finden Sie unter [Einbinden einer NFS-Freigabe](storage-files-how-to-mount-nfs-shares.md).

Wenn Probleme auftreten, finden Sie unter [Fehlerbehebung für NFS-Dateifreigaben in Azure](storage-troubleshooting-files-nfs.md) weitere Informationen.