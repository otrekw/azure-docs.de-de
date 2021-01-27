---
title: Fehler aufgrund nicht verfügbarer SKU
description: Hier wird beschrieben, wie Sie den Fehler aufgrund einer nicht verfügbaren SKU beim Bereitstellen von Ressourcen mit Azure Resource Manager beheben.
ms.topic: troubleshooting
ms.date: 02/18/2020
ms.openlocfilehash: 5b0bbd653907c109eca526af86979013b3137cfa
ms.sourcegitcommit: 78ecfbc831405e8d0f932c9aafcdf59589f81978
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2021
ms.locfileid: "98737149"
---
# <a name="resolve-errors-for-sku-not-available"></a>Beheben des Fehlers vom Typ „SKU nicht verfügbar“

In diesem Artikel wird beschrieben, wie Sie den Fehler **SkuNotAvailable** beheben. Wenn Sie in dieser Region/Zone oder einer anderen Region/Zone keine geeignete SKU finden, die Ihre Geschäftsanforderungen erfüllt, übermitteln Sie eine [SKU-Anforderung](/troubleshoot/azure/general/region-access-request-process) an den Azure-Support.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="symptom"></a>Symptom

Beim Bereitstellen einer Ressource (in der Regel ein virtueller Computer) werden der folgende Fehlercode und die folgende Fehlermeldung angezeigt:

```
Code: SkuNotAvailable
Message: The requested tier for resource '<resource>' is currently not available in location '<location>'
for subscription '<subscriptionID>'. Please try another tier or deploy to a different location.
```

## <a name="cause"></a>Ursache

Sie erhalten diesen Fehler, wenn die ausgewählte Ressourcen-SKU (z.B. die Größe des virtuellen Computers) für den ausgewählten Standort nicht verfügbar ist.

Wenn Sie eine Azure Spot-VM oder eine Instanz einer Spot-Skalierungsgruppe bereitstellen, ist an diesem Standort keine Kapazität für Azure Spot verfügbar. Weitere Informationen finden Sie unter [Spot-Fehlermeldungen](../../virtual-machines/error-codes-spot.md).

## <a name="solution-1---powershell"></a>Lösung 1: PowerShell

Verwenden Sie den Befehl [Get AzComputeResourceSku](/powershell/module/az.compute/get-azcomputeresourcesku), um zu ermitteln, welche SKUs in einer Region/Zone verfügbar sind. Filtern Sie die Ergebnisse nach Standort. Für diesen Befehl benötigen Sie die aktuelle Version von PowerShell.

```azurepowershell-interactive
Get-AzComputeResourceSku | where {$_.Locations -icontains "centralus"}
```

Die Ergebnisse enthalten eine Liste von SKUs für den Standort und alle Einschränkungen für diese SKU. Beachten Sie, dass die SKU möglicherweise als `NotAvailableForSubscription` aufgeführt wird.

```output
ResourceType          Name           Locations   Zone      Restriction                      Capability           Value
------------          ----           ---------   ----      -----------                      ----------           -----
virtualMachines       Standard_A0    centralus             NotAvailableForSubscription      MaxResourceVolumeMB   20480
virtualMachines       Standard_A1    centralus             NotAvailableForSubscription      MaxResourceVolumeMB   71680
virtualMachines       Standard_A2    centralus             NotAvailableForSubscription      MaxResourceVolumeMB  138240
virtualMachines       Standard_D1_v2 centralus   {2, 1, 3}                                  MaxResourceVolumeMB
```

Einige weitere Beispiele:

```azurepowershell-interactive
Get-AzComputeResourceSku | where {$_.Locations.Contains("centralus") -and $_.ResourceType.Contains("virtualMachines") -and $_.Name.Contains("Standard_DS14_v2")}
Get-AzComputeResourceSku | where {$_.Locations.Contains("centralus") -and $_.ResourceType.Contains("virtualMachines") -and $_.Name.Contains("v3")} | fc
```

Bei Anfügen von „fc“ am Ende werden weitere Details zurückgegeben.

## <a name="solution-2---azure-cli"></a>Lösung 2: Azure CLI

Verwenden Sie den Befehl `az vm list-skus`, um zu ermitteln, welche SKUs in einer Region verfügbar sind. Verwenden Sie den Parameter `--location`, um die Ausgabe auf den verwendeten Standort zu filtern. Verwenden Sie den Parameter `--size`, um nach dem Teil eines Namens für die Größe zu suchen.

```azurecli-interactive
az vm list-skus --location southcentralus --size Standard_F --output table
```

Der Befehl gibt Ergebnisse ähnlich den folgenden zurück:

```output
ResourceType     Locations       Name              Zones    Capabilities    Restrictions
---------------  --------------  ----------------  -------  --------------  --------------
virtualMachines  southcentralus  Standard_F1                ...             None
virtualMachines  southcentralus  Standard_F2                ...             None
virtualMachines  southcentralus  Standard_F4                ...             None
...
```

## <a name="solution-3---azure-portal"></a>Lösung 3: Azure-Portal

Verwenden Sie das [Portal](https://portal.azure.com), um zu ermitteln, welche SKUs in einer Region verfügbar sind. Melden Sie sich beim Portal an, und fügen Sie über die Oberfläche eine Ressource hinzu. Beim Festlegen der Werte werden die verfügbaren SKUs für die Ressource angezeigt. Sie müssen die Bereitstellung nicht abschließen.

Starten Sie beispielsweise den Prozess zum Erstellen eines virtuellen Computers. Um andere verfügbare Größen anzuzeigen, wählen Sie **Größe ändern** aus.

![Erstellen eines virtuellen Computers](./media/error-sku-not-available/create-vm.png)

Sie können die verfügbaren Größen filtern und durch sie scrollen.

![Verfügbare SKUs](./media/error-sku-not-available/available-sizes.png)

## <a name="solution-4---rest"></a>Lösung 4: REST

Verwenden Sie den Vorgang [Ressourcen-SKUs – Auflisten](/rest/api/compute/resourceskus/list), um zu ermitteln, welche SKUs in einer Region verfügbar sind.

Verfügbare SKUs und Regionen werden im folgenden Format zurückgegeben:

```json
{
  "value": [
    {
      "resourceType": "virtualMachines",
      "name": "Standard_A0",
      "tier": "Standard",
      "size": "A0",
      "locations": [
        "eastus"
      ],
      "restrictions": []
    },
    {
      "resourceType": "virtualMachines",
      "name": "Standard_A1",
      "tier": "Standard",
      "size": "A1",
      "locations": [
        "eastus"
      ],
      "restrictions": []
    },
    ...
  ]
}
```