---
title: 'Schnellstart: Erstellen eines internen Lastenausgleichs mit einer Vorlage'
description: In dieser Schnellstartanleitung wird veranschaulicht, wie Sie mit einer ARM-Vorlage (Azure Resource Manager) einen Azure-Lastenausgleich erstellen.
services: load-balancer
author: asudbring
ms.service: load-balancer
ms.topic: quickstart
ms.custom: subject-armqs
ms.author: allensu
ms.date: 09/14/2020
ms.openlocfilehash: aa68dad2f8f018a9f3f70f2f02fd5e989ccbad4e
ms.sourcegitcommit: 2e72661f4853cd42bb4f0b2ded4271b22dc10a52
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2020
ms.locfileid: "92047759"
---
# <a name="quickstart-create-an-internal-load-balancer-to-load-balance-vms-by-using-an-arm-template"></a>Schnellstart: Erstellen eines internen Lastenausgleichs für VMs mithilfe einer ARM-Vorlage

In dieser Schnellstartanleitung wird beschrieben, wie Sie mithilfe einer Azure Resource Manager-Vorlage (ARM-Vorlage) einen internen Azure-Lastenausgleich erstellen.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="review-the-template"></a>Überprüfen der Vorlage

Die in dieser Schnellstartanleitung verwendete Vorlage stammt aus den [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/201-2-vms-internal-load-balancer).

:::code language="json" source="~/quickstart-templates/201-2-vms-internal-load-balancer/azuredeploy.json":::

In der Vorlage wurden mehrere Azure-Ressourcen definiert:

- [**Microsoft.Storage/storageAccounts:** ](/azure/templates/microsoft.storage/storageaccounts) VM-Speicherkonten für die Startdiagnose
- [**Microsoft.Compute/availabilitySets:** ](/azure/templates/microsoft.compute/availabilitySets) Verfügbarkeitsgruppe für VMs
- [**Microsoft.Network/virtualNetworks**](/azure/templates/microsoft.network/virtualNetworks): Virtuelles Netzwerk für Lastenausgleich und VMs
- [**Microsoft.Network/networkInterfaces**](/azure/templates/microsoft.network/networkInterfaces): Netzwerkschnittstellen für VMs
- [**Microsoft.Network/loadBalancers**](/azure/templates/microsoft.network/loadBalancers): Interner Lastenausgleich
- [**Microsoft.Compute/virtualMachines**](/azure/templates/microsoft.compute/virtualMachines): Virtuelle Computer

Weitere Vorlagen zum Azure Load Balancer finden Sie unter [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Network&pageNumber=1&sort=Popular).

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

**Azure-Befehlszeilenschnittstelle**

```azurecli-interactive
read -p "Enter the location (i.e. westcentralus): " location
resourceGroupName="myResourceGroupLB"
templateUri="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json" 

az group create \
--name $resourceGroupName \
--location $location

az deployment group create \
--resource-group $resourceGroupName \
--template-uri  $templateUri
```

## <a name="review-deployed-resources"></a>Überprüfen der bereitgestellten Ressourcen

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Wählen Sie im linken Bereich **Ressourcengruppen** aus.

3. Wählen Sie die Ressourcengruppe aus, die Sie im vorherigen Abschnitt erstellt haben. Der Ressourcengruppenname lautet standardmäßig **myResourceGroupLB**.

4. Vergewissern Sie sich, dass die folgenden Ressourcen in der Ressourcengruppe erstellt wurden:

:::image type="content" source="media/quickstart-load-balancer-standard-internal-template/verify-creation.png" alt-text="Verwenden des Azure-Portals zum Überprüfen der Erstellung der Ressourcen" border="true":::

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn die Ressourcengruppe und alle darin enthaltenen Ressourcen nicht mehr benötigt werden, können Sie sie mit dem Befehl [az group delete](/cli/azure/group#az-group-delete) entfernen.

```azurecli-interactive 
  az group delete \
    --name myResourceGroupLB
```

## <a name="next-steps"></a>Nächste Schritte

Ein Schritt-für-Schritt-Tutorial mit Anleitungen zum Erstellen einer Vorlage finden Sie unter folgendem Link:

> [!div class="nextstepaction"]
> [Tutorial: Erstellen und Bereitstellen Ihrer ersten ARM-Vorlage](/azure/azure-resource-manager/templates/template-tutorial-create-first-template)