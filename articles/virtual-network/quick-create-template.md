---
title: 'Schnellstart: Erstellen eines virtuellen Netzwerks mithilfe einer Resource Manager-Vorlage'
titleSuffix: Azure Virtual Network
description: Hier erfahren Sie, wie Sie mithilfe einer Resource Manager-Vorlage ein virtuelles Azure-Netzwerk erstellen.
services: virtual-network
author: KumudD
ms.service: virtual-network
ms.topic: quickstart
ms.date: 06/23/2020
ms.author: kumud
ms.custom: ''
ms.openlocfilehash: c9e9ea2e59537a1c0c8e372a766fba3aa9a1b9a0
ms.sourcegitcommit: b8702065338fc1ed81bfed082650b5b58234a702
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2020
ms.locfileid: "88122154"
---
# <a name="quickstart-create-a-virtual-network---resource-manager-template"></a>Schnellstart: Erstellen eines virtuellen Netzwerks – Resource Manager-Vorlage

In dieser Schnellstartanleitung erfahren Sie, wie Sie mithilfe der Azure Resource Manager-Vorlage ein virtuelles Netzwerk mit zwei Subnetzen erstellen. Ein virtuelles Netzwerk ist der grundlegende Baustein für Ihr privates Netzwerk in Azure. Über dieses Netzwerk können Azure-Ressourcen wie etwa virtuelle Computer sicher miteinander und mit dem Internet kommunizieren.


[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Diese Schnellstartanleitung steht auch für das [Azure-Portal](quick-create-portal.md) sowie für [Azure PowerShell](quick-create-powershell.md) und die [Azure-Befehlszeilenschnittstelle](quick-create-cli.md) zur Verfügung.

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="review-the-template"></a>Überprüfen der Vorlage

Die in dieser Schnellstartanleitung verwendete Vorlage stammt aus den [Azure-Schnellstartvorlagen](https://github.com/Azure/azure-quickstart-templates/blob/master/101-vnet-two-subnets/azuredeploy.json).

:::code language="json" source="~/quickstart-templates/101-vnet-two-subnets/azuredeploy.json" range="001-96" highlight="56-92":::

In der Vorlage wurden die folgenden Azure-Ressourcen definiert:
- [**Microsoft.Network/virtualNetworks**](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks): Dient zum Erstellen eines virtuellen Azure-Netzwerks.
-  [**Microsoft.Network/virtualNetworks/subnets**](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks/subnets): Dient zum Erstellen eines Subnetzes.

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

Stellen Sie die Resource Manager-Vorlage in Azure bereit:

1. Wählen Sie **Bereitstellung in Azure** aus, um sich bei Azure anzumelden und die Vorlage zu öffnen. Mit der Vorlage wird ein virtuelles Netzwerk mit zwei Subnetzen erstellt.

   [![In Azure bereitstellen](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-virtual-network-2vms-create%2Fazuredeploy.json)

2. Geben Sie im Portal auf der Seite **Erstellen eines virtuellen Netzwerks mit zwei Subnetzen** die folgenden Werte ein, oder wählen Sie sie aus:
   - **Ressourcengruppe**: Wählen Sie **Neu erstellen** aus, und geben Sie einen Namen für die Ressourcengruppe an. Klicken Sie auf **OK**.
   - **Name des virtuellen Netzwerks**: Geben Sie einen Namen für das neue virtuelle Netzwerk ein.
3. Klicken Sie auf **Überprüfen und erstellen** und dann auf **Erstellen**.

## <a name="review-deployed-resources"></a>Überprüfen der bereitgestellten Ressourcen

Untersuchen Sie zuerst die Ressourcen, die mit dem virtuellen Netzwerk erstellt wurden.

Weitere Informationen zur JSON-Syntax und zu den Eigenschaften für ein virtuelles Netzwerk in einer Vorlage finden Sie unter [Microsoft.Network/virtualNetworks](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks).

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die mit dem virtuellen Netzwerk erstellten Ressourcen nicht mehr benötigen, löschen Sie die Ressourcengruppe. Dadurch werden das virtuelle Netzwerk und alle zugehörigen Ressourcen entfernt.

Rufen Sie zum Löschen der Ressourcengruppe das Cmdlet `Remove-AzResourceGroup` auf:

```azurepowershell-interactive
Remove-AzResourceGroup -Name <your resource group name>
```

## <a name="next-steps"></a>Nächste Schritte
In dieser Schnellstartanleitung haben Sie ein virtuelles Azure-Netzwerk mit zwei Subnetzen bereitgestellt. Fahren Sie mit dem Tutorial für virtuelle Netzwerke fort, um weitere Informationen zu virtuellen Azure-Netzwerke zu erhalten.

> [!div class="nextstepaction"]
> [Erstellen von Netzwerksicherheitsgruppen über das Azure-Portal](tutorial-filter-network-traffic.md)
