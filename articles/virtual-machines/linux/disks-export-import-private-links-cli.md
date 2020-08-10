---
title: 'Azure CLI: Einschränken des Import-/Exportzugriffs auf verwaltete Datenträger mit privaten Links (Vorschauversion)'
description: Aktivieren Sie mit der Azure CLI private Links (Vorschauversion) für Ihre verwalteten Datenträger. Dadurch können Sie Datenträger nur innerhalb Ihres virtuellen Netzwerks sicher exportieren und importieren.
author: roygara
ms.service: virtual-machines
ms.topic: overview
ms.date: 07/15/2020
ms.author: rogarana
ms.subservice: disks
ms.custom: references_regions
ms.openlocfilehash: 5df11e704987098d61ced7afbff5e6234d4d5f04
ms.sourcegitcommit: e71da24cc108efc2c194007f976f74dd596ab013
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2020
ms.locfileid: "87420282"
---
# <a name="azure-cli---restrict-importexport-access-for-managed-disks-with-private-links-preview"></a>Azure CLI: Einschränken des Import-/Exportzugriffs für verwaltete Datenträger mit privaten Links (Vorschauversion)

Sie können [private Endpunkte](../../private-link/private-endpoint-overview.md) (Vorschauversion) verwenden, um den Export und Import verwalteter Datenträger einzuschränken und sicheren Zugriff auf Daten über einen [privaten Link](../../private-link/private-link-overview.md) von Clients in Ihrem virtuellen Azure-Netzwerk zu ermöglichen. Der private Endpunkt verwendet eine IP-Adresse aus dem Adressraum des virtuellen Netzwerks für Ihren Dienst für verwaltete Datenträger. Der Netzwerkdatenverkehr zwischen den Clients im virtuellen Netzwerk und den verwalteten Datenträgern wird über das virtuelle Netzwerk und eine private Verbindung im Microsoft-Backbonenetzwerk geleitet und somit nicht dem öffentlichen Internet ausgesetzt. 

Wenn Sie private Links zum Exportieren und Importieren verwalteter Datenträger verwenden möchten, erstellen Sie zunächst eine Datenträgerzugriffsressource und verknüpfen sie mit einem virtuellen Netzwerk im gleichen Abonnement, indem Sie einen privaten Endpunkt erstellen. Ordnen Sie dann einer Instanz des Datenträgerzugriffs einen Datenträger oder eine Momentaufnahme zu. Legen Sie abschließend die NetworkAccessPolicy-Eigenschaft des Datenträgers oder der Momentaufnahme auf `AllowPrivate` fest. Dadurch wird der Zugriff auf das virtuelle Netzwerk beschränkt. 

Sie können die NetworkAccessPolicy-Eigenschaft auf `DenyAll` festlegen, um zu verhindern, dass Daten auf einem Datenträger oder in einer Momentaufnahme exportiert werden. Der Standardwert für die NetworkAccessPolicy-Eigenschaft ist `AllowAll`.

## <a name="limitations"></a>Einschränkungen

[!INCLUDE [virtual-machines-disks-private-links-limitations](../../../includes/virtual-machines-disks-private-links-limitations.md)]

## <a name="regional-availability"></a>Regionale Verfügbarkeit

[!INCLUDE [virtual-machines-disks-private-links-regions](../../../includes/virtual-machines-disks-private-links-regions.md)]

## <a name="log-in-into-your-subscription-and-set-your-variables"></a>Anmelden bei Ihrem Abonnement und Festlegen Ihrer Variablen

```azurecli-interactive

subscriptionId=yourSubscriptionId
resourceGroupName=yourResourceGroupName
region=northcentralus
diskAccessName=yourDiskAccessForPrivateLinks
vnetName=yourVNETForPrivateLinks
subnetName=yourSubnetForPrivateLinks
privateEndPointName=yourPrivateLinkForSecureMDExportImport
privateEndPointConnectionName=yourPrivateLinkConnection

#The name of an existing disk which is the source of the snapshot
sourceDiskName=yourSourceDiskForSnapshot

#The name of the new snapshot which will be secured via Private Links
snapshotNameSecuredWithPL=yourSnapshotNameSecuredWithPL

az login

az account set --subscription $subscriptionId

```

## <a name="create-a-disk-access-using-azure-cli"></a>Erstellen eines Datenträgerzugriffs mithilfe der Azure CLI
```azurecli-interactive
az group deployment create -g $resourceGroupName \
--template-uri "https://raw.githubusercontent.com/Azure-Samples/managed-disks-powershell-getting-started/master/privatelinks/CreateDiskAccess.json" \
--parameters "region=$region" "diskAccessName=$diskAccessName"

diskAccessId=$(az resource show -n $diskAccessName -g $resourceGroupName --namespace Microsoft.Compute --resource-type diskAccesses --query [id] -o tsv)
```

## <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

Netzwerkrichtlinien wie Netzwerksicherheitsgruppen (NSG) werden für private Endpunkte nicht unterstützt. Zum Bereitstellen eines privaten Endpunkts in einem bestimmten Subnetz ist eine explizite Einstellung zum Deaktivieren in diesem Subnetz erforderlich. 

```azurecli-interactive
az network vnet create --resource-group $resourceGroupName \
    --name $vnetName \
    --subnet-name $subnetName
```
## <a name="disable-subnet-private-endpoint-policies"></a>Deaktivieren von Richtlinien für den privaten Endpunkt im Subnetz

Azure stellt Ressourcen für ein Subnetz innerhalb eines virtuellen Netzwerks bereit. Daher müssen Sie das Subnetz aktualisieren, um die Netzwerkrichtlinien für den privaten Endpunkt zu deaktivieren. 

```azurecli-interactive
az network vnet subnet update --resource-group $resourceGroupName \
    --name $subnetName  \
    --vnet-name $vnetName \
    --disable-private-endpoint-network-policies true
```
## <a name="create-a-private-endpoint-for-the-disk-access-object"></a>Erstellen eines privaten Endpunkts für das Datenträgerzugriffsobjekt

```azurecli-interactive
az network private-endpoint create --resource-group $resourceGroupName \
    --name $privateEndPointName \
    --vnet-name $vnetName  \
    --subnet $subnetName \
    --private-connection-resource-id $diskAccessId \
    --group-ids disks \
    --connection-name $privateEndPointConnectionName
```

## <a name="configure-the-private-dns-zone"></a>Konfigurieren der privaten DNS-Zone

Erstellen Sie eine private DNS-Zone für die Storage Blob-Domäne, eine Zuordnungsverknüpfung mit dem virtuellen Netzwerk und eine DNS-Zonengruppe, um den privaten Endpunkt der privaten DNS-Zone zuzuordnen. 

```azurecli-interactive
az network private-dns zone create --resource-group $resourceGroupName \ 
    --name "privatelink.blob.core.windows.net"

az network private-dns link vnet create --resource-group $resourceGroupName \
    --zone-name "privatelink.blob.core.windows.net" \
    --name yourDNSLink \
    --virtual-network $vnetName \
    --registration-enabled false 

az network private-endpoint dns-zone-group create \
   --resource-group $resourceGroupName \
   --endpoint-name $privateEndPointName \
   --name yourZoneGroup \
   --private-dns-zone "privatelink.blob.core.windows.net" \
   --zone-name disks
```

## <a name="create-a-disk-protected-with-private-links"></a>Erstellen eines Datenträgers, der durch private Links geschützt ist
  ```cli
    resourceGroupName=yourResourceGroupName
    region=northcentralus
    diskAccessName=yourDiskAccessName
    diskName=yourDiskName
    diskSkuName=Standard_LRS
    diskSizeGB=128

    diskAccessId=$(az resource show -n $diskAccessName -g $resourceGroupName --namespace Microsoft.Compute --resource-type diskAccesses --query [id] -o tsv)

    az group deployment create -g $resourceGroupName \
       --template-uri "https://raw.githubusercontent.com/Azure-Samples/managed-disks-powershell-getting-started/master/privatelinks/CreateDisksWithExportViaPrivateLink.json" \
       --parameters "diskName=$diskName" \
       "diskSkuName=$diskSkuName" \
       "diskSizeGB=$diskSizeGB" \
       "diskAccessId=$diskAccessId" \
       "region=$region" \
       "networkAccessPolicy=AllowPrivate"
```

## <a name="create-a-snapshot-of-a-disk-protected-with-private-links"></a>Erstellen einer Momentaufnahme eines Datenträgers, der durch private Links geschützt ist
   ```cli
    resourceGroupName=yourResourceGroupName
    region=northcentralus
    diskAccessName=yourDiskAccessName
    sourceDiskName=yourSourceDiskForSnapshot
    snapshotNameSecuredWithPL=yourSnapshotName

    diskId=$(az disk show -n $sourceDiskName -g $resourceGroupName --query [id] -o tsv)
   
    diskAccessId=$(az resource show -n $diskAccessName -g $resourceGroupName --namespace Microsoft.Compute --resource-type diskAccesses --query [id] -o tsv)
   
    az group deployment create -g $resourceGroupName \
      --template-uri "https://raw.githubusercontent.com/Azure-Samples/managed-disks-powershell-getting-started/master/privatelinks/CreateSnapshotWithExportViaPrivateLink.json" \
      --parameters "snapshotName=$snapshotNameSecuredWithPL" \
      "sourceResourceId=$diskId" \
      "diskAccessId=$diskAccessId" \
      "region=$region" \
      "networkAccessPolicy=AllowPrivate" 
```

## <a name="next-steps"></a>Nächste Schritte

- [Häufig gestellte Fragen zu privaten Links](faq-for-disks.md#private-links-for-securely-exporting-and-importing-managed-disks)
- [Exportieren/Kopieren verwalteter Momentaufnahmen als VHD in ein Speicherkonto in einer anderen Region mit der Befehlszeilenschnittstelle](../scripts/virtual-machines-linux-cli-sample-copy-managed-disks-vhd.md)
