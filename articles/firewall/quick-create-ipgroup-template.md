---
title: 'Schnellstart: Erstellen einer Azure Firewall-Instanz sowie von IP-Adressgruppen: Resource Manager-Vorlage'
description: Hier erfahren Sie, wie Sie mit einer Resource Manager-Vorlage eine Azure Firewall-Instanz und IP-Adressgruppen erstellen.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: quickstart
ms.date: 04/06/2020
ms.author: victorh
ms.openlocfilehash: 767e91af7fb5ff3de517a79ac4df55af5057c76f
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2020
ms.locfileid: "80677755"
---
# <a name="quickstart-create-an-azure-firewall-and-ip-groups---resource-manager-template"></a>Schnellstart: Erstellen einer Azure Firewall-Instanz sowie von IP-Adressgruppen: Resource Manager-Vorlage

In dieser Schnellstartanleitung wird mithilfe einer Resource Manager-Vorlage eine Azure Firewall-Instanz mit exemplarischen IP-Adressgruppen erstellt, die in einer Netzwerk- und in einer Anwendungsregel verwendet werden.

Eine IP-Adressgruppe ist eine Ressource der obersten Ebene und ermöglicht es, IP-Adressen, Bereiche und Subnetze in einem einzelnen Objekt zu definieren und zu gruppieren. Dies ist bei der Verwaltung von IP-Adressen in Azure Firewall-Regeln hilfreich. IP-Adressen können entweder manuell eingegeben oder aus einer Datei importiert werden.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="create-an-azure-firewall-and-ip-groups"></a>Erstellen einer Azure Firewall-Instanz sowie von IP-Adressgruppen

Mit dieser Vorlage werden eine Azure Firewall-Instanz sowie IP-Adressgruppen und die erforderlichen unterstützenden Ressourcen für die Azure Firewall-Instanz erstellt.

### <a name="review-the-template"></a>Überprüfen der Vorlage

Die in dieser Schnellstartanleitung verwendete Vorlage stammt aus den [Azure-Schnellstartvorlagen](https://github.com/Azure/azure-quickstart-templates/blob/master/101-azurefirewall-create-with-ipgroups-and-linux-jumpbox/azuredeploy.json).

:::code language="json" source="~/quickstart-templates/101-azurefirewall-create-with-ipgroups-and-linux-jumpbox/azuredeploy.json" range="001-512" highlight="118-141":::

In der Vorlage sind mehrere Azure-Ressourcen definiert:

- [**Microsoft.Network/ipGroups**](/azure/templates/microsoft.network/ipGroups)
- [**Microsoft.Network/publicIPAddresses**](/azure/templates/microsoft.network/publicipaddresses)
- [**Microsoft.Network/networkSecurityGroups**](/azure/templates/microsoft.network/networksecuritygroups)
- [**Microsoft.Network/virtualNetworks**](/azure/templates/microsoft.network/virtualnetworks)
- [**Microsoft.Compute/virtualMachines**](/azure/templates/microsoft.compute/virtualmachines)
- [**Microsoft.Network/networkInterfaces**](/azure/templates/microsoft.network/networkinterfaces)
- [**Microsoft.Storage/storageAccounts**](/azure/templates/microsoft.storage/storageAccounts)
- [**Microsoft.Network/azureFirewalls**](/azure/templates/microsoft.network/azureFirewalls)
- [**Microsoft.Network/routeTables**](/azure/templates/microsoft.network/routeTables)


### <a name="deploy-the-template"></a>Bereitstellen der Vorlage

Stellen Sie die Resource Manager-Vorlage in Azure bereit:

1. Wählen Sie **Bereitstellung in Azure** aus, um sich bei Azure anzumelden und die Vorlage zu öffnen. Durch die Vorlage werden eine Azure Firewall-Instanz, die Netzwerkinfrastruktur und zwei virtuelle Computer erstellt.

   <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-azurefirewall-create-with-ipgroups-and-linux-jumpbox%2Fazuredeploy.json"><img src="./media/quick-create-ipgroup-template/deploy-to-azure.png" alt="deploy to azure"/></a>

2. Geben Sie im Portal auf der Seite **Create an Azure Firewall with IpGroups** (Erstellen einer Azure Firewall-Instanz mit IP-Adressgruppen) die folgenden Werte ein, oder wählen Sie sie aus:
   - Abonnement: Wählen Sie aus den vorhandenen Abonnements aus. 
   - Ressourcengruppe:  Wählen Sie aus den vorhandenen Ressourcengruppen aus, oder wählen Sie **Neu erstellen** und anschließend **OK** aus.
   - Standort: Wählen Sie einen Standort aus.
   - Name des virtuellen Netzwerks: Geben Sie einen Namen für das neue virtuelle Netzwerk (VNET) ein. 
   - Name der IP-Adressgruppe 1: Geben Sie einen Namen für die IP-Adressgruppe 1 ein. 
   - Name der IP-Adressgruppe 2: Geben Sie einen Namen für die IP-Adressgruppe 2 ein. 
   - Administratorbenutzername: Geben Sie den Benutzernamen für das Administratorbenutzerkonto ein. 
   - Authentifizierung: Wählen Sie „sshPublicKey“ oder „Kennwort“ aus. 
   - Administratorkennwort: Geben Sie ein Administratorkennwort oder einen Administratorschlüssel ein.

3. Wählen Sie **Ich stimme den oben genannten Geschäftsbedingungen zu** und anschließend **Kaufen** aus. Die Bereitstellung kann zehn Minuten oder länger dauern.

## <a name="review-deployed-resources"></a>Überprüfen der bereitgestellten Ressourcen

Überprüfen Sie im Azure-Portal die bereitgestellten Ressourcen (insbesondere die Firewallregeln mit IP-Adressgruppen).

:::image type="content" source="media/quick-create-ipgroup-template/ipgroups.png" alt-text="IP-Adressgruppen":::

:::image type="content" source="media/quick-create-ipgroup-template/network-rule.png" alt-text="Netzwerkregeln":::

Informationen zur JSON-Syntax und zu den Eigenschaften für eine Firewall in einer Vorlage finden Sie in der Vorlagenreferenz für [Microsoft.Network/azureFirewalls](https://docs.microsoft.com/azure/templates/Microsoft.Network/2019-11-01/azureFirewalls).

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die mit der Firewall erstellten Ressourcen nicht mehr benötigen, löschen Sie die Ressourcengruppe. Dadurch werden die Firewall und alle zugehörigen Ressourcen entfernt.

Rufen Sie zum Löschen der Ressourcengruppe das Cmdlet `Remove-AzResourceGroup` auf:

```azurepowershell-interactive
Remove-AzResourceGroup -Name "<your resource group name>"
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Tutorial: Bereitstellen und Konfigurieren von Azure Firewall in einem Hybridnetzwerk über das Azure-Portal](tutorial-hybrid-portal.md)