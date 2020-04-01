---
title: 'Schnellstart: Erstellen eines NAT-Gateways: Resource Manager-Vorlage'
titleSuffix: Azure Virtual Network NAT
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie ein NAT-Gateway per Azure Resource Manager-Vorlage erstellen.
services: load-balancer
documentationcenter: na
author: asudbring
manager: KumudD
Customer intent: I want to create a NAT gateway by using an Azure Resource Manager template so that I can provide outbound connectivity for my virtual machines.
ms.service: virtual-network
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/09/2020
ms.author: allensu
ms.custom: subject-armqs
ms.openlocfilehash: 6b3dee698e63ed2b0a44f05b593b8840d1cd23ee
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/26/2020
ms.locfileid: "80066401"
---
# <a name="quickstart-create-a-nat-gateway---resource-manager-template"></a>Schnellstart: Erstellen eines NAT-Gateways: Resource Manager-Vorlage

Hier finden Sie Informationen zu den ersten Schritten mit Virtual Network NAT unter Verwendung einer Azure Resource Manager-Vorlage.  Mit dieser Vorlage werden ein virtuelles Netzwerk, eine NAT-Gatewayressource und ein virtueller Ubuntu-Computer bereitgestellt. Der virtuelle Ubuntu-Computer wird in einem Subnetz bereitgestellt, das der NAT-Gatewayressource zugeordnet ist.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="create-a-nat-gateway-and-supporting-resources"></a>Erstellen eines NAT-Gateways und unterstützender Ressourcen

Diese Vorlage ist für die Erstellung von Folgendem konfiguriert: 

* Virtuelles Netzwerk 
* NAT-Gatewayressource
* Virtueller Ubuntu-Computer

Der virtuelle Ubuntu-Computer wird in einem Subnetz bereitgestellt, das der NAT-Gatewayressource zugeordnet ist.

### <a name="review-the-template"></a>Überprüfen der Vorlage

Die in dieser Schnellstartanleitung verwendete Vorlage stammt aus den [Azure-Schnellstartvorlagen](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-nat-gateway-1-vm/azuredeploy.json).

:::code language="json" source="~/quickstart-templates/101-nat-gateway-1-vm/azuredeploy.json" range="1-335" highlight="256-282":::

In der Vorlage sind neun Azure-Ressourcen definiert:

**Microsoft.Network**

* **[Microsoft.Network/natGateways](https://docs.microsoft.com/azure/templates/microsoft.network/natgateways)** : Erstellt eine NAT-Gatewayressource.

* **[Microsoft.Network/networkSecurityGroups](https://docs.microsoft.com/azure/templates/microsoft.network/networksecuritygroups)** : Erstellt eine Netzwerksicherheitsgruppe.

    * **[Microsoft.Network/networkSecurityGroups/securityRules](https://docs.microsoft.com/azure/templates/microsoft.network/networksecuritygroups/securityrules)** : Erstellt eine Sicherheitsregel.

* **[Microsoft.Network/publicIPAddresses](https://docs.microsoft.com/azure/templates/microsoft.network/publicipaddresses)** : Erstellt eine öffentliche IP-Adresse.

* **[Microsoft.Network/publicIPPrefixes](https://docs.microsoft.com/azure/templates/microsoft.network/publicipprefixes)** : Erstellt ein Präfix für öffentliche IP-Adressen.

* **[Microsoft.Network/virtualNetworks](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks)** : Erstellen Sie ein virtuelles Netzwerk.

    * **[Microsoft.Network/virtualNetworks/subnets](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks/subnets)** : Erstellt ein Subnetz für virtuelle Netzwerke.

* **[Microsoft.Network/networkinterfaces](https://docs.microsoft.com/azure/templates/microsoft.network/networkinterfaces)** : Erstellt eine Netzwerkschnittstelle.

**Microsoft.Compute**

* **[Microsoft.Compute/virtualMachines](https://docs.microsoft.com/azure/templates/Microsoft.Compute/virtualMachines)** : Erstellt einen virtuellen Computer.

### <a name="deploy-the-template"></a>Bereitstellen der Vorlage

**Azure-Befehlszeilenschnittstelle**

```azurecli-interactive
read -p "Enter the location (i.e. westcentralus): " location
resourceGroupName="myResourceGroupNAT"
templateUri="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-nat-gateway-1-vm/azuredeploy.json" 

az group create \
--name $resourceGroupName \
--location $location

az group deployment create \
--resource-group $resourceGroupName \
--template-uri  $templateUri
```

**Azure PowerShell**

```azurepowershell-interactive
$location = Read-Host -Prompt "Enter the location (i.e. westcentralus)"
$templateUri = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-nat-gateway-1-vm/azuredeploy.json"

$resourceGroupName = "myResourceGroupNAT"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri
```

**Azure portal**

[![In Azure bereitstellen](./media/quick-create-template/deploy-to-azure.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-nat-gateway-1-vm%2Fazuredeploy.json)

## <a name="review-deployed-resources"></a>Überprüfen der bereitgestellten Ressourcen

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Wählen Sie im linken Bereich **Ressourcengruppen** aus.

3. Wählen Sie die Ressourcengruppe aus, die Sie im vorherigen Abschnitt erstellt haben. Der Ressourcengruppenname lautet standardmäßig **myResourceGroupNAT**.

4. Vergewissern Sie sich, dass die folgenden Ressourcen in der Ressourcengruppe erstellt wurden:

    ![Ressourcengruppe für Virtual Network NAT](./media/quick-create-template/nat-gateway-template-rg.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

**Azure-Befehlszeilenschnittstelle**

Wenn die Ressourcengruppe und alle darin enthaltenen Ressourcen nicht mehr benötigt werden, können Sie sie mit dem Befehl [az group delete](/cli/azure/group#az-group-delete) entfernen.

```azurecli-interactive 
  az group delete \
    --name myResourceGroupNAT
```

**Azure PowerShell**

Wenn die Ressourcengruppe und alle darin enthaltenen Ressourcen nicht mehr benötigt werden, können Sie sie mit dem Befehl [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup?view=latest) entfernen.

```azurepowershell-interactive 
Remove-AzResourceGroup -Name myResourceGroupNAT
```

**Azure portal**

Löschen Sie die Ressourcengruppe, das NAT-Gateway und alle dazugehörigen Ressourcen, wenn Sie sie nicht mehr benötigen. Wählen Sie die Ressourcengruppe **myResourceGroupNAT** aus, die das NAT-Gateway enthält, und wählen Sie anschließend **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie Folgendes erstellt:

* NAT-Gatewayressource
* Virtuelles Netzwerk
* Virtueller Ubuntu-Computer

Der virtuelle Computer wird in einem VNET-Subnetz bereitgestellt, das dem NAT-Gateway zugeordnet ist. 

Weitere Informationen zu Virtual Network NAT und Azure Resource Manager finden Sie in den folgenden Artikeln:

* [Was ist Virtual Network NAT?](nat-overview.md)
* [Entwerfen von virtuellen Netzwerken mit NAT-Gatewayressourcen](nat-gateway-resource.md)
* Lesen Sie weitere Informationen zu [Azure Resource Manager](../azure-resource-manager/management/overview.md).
