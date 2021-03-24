---
title: 'Schnellstart: Erstellen einer Azure Firewall-Instanz mit mehreren öffentlichen IP-Adressen – Resource Manager-Vorlage'
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie mithilfe einer Azure Resource Manager-Vorlage (ARM-Vorlage) eine Azure Firewall-Instanz mit mehreren öffentlichen IP-Adressen erstellen.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: quickstart
ms.custom: subject-armqs
ms.date: 08/28/2020
ms.author: victorh
ms.openlocfilehash: 33d110f327a381241fd9dbf55996a6e3f9fd3a43
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "92057976"
---
# <a name="quickstart-create-an-azure-firewall-with-multiple-public-ip-addresses---arm-template"></a>Schnellstart: Erstellen einer Azure Firewall-Instanz mit mehreren öffentlichen IP-Adressen – ARM-Vorlage

In dieser Schnellstartanleitung verwenden Sie eine Azure Resource Manager-Vorlage (ARM-Vorlage), um eine Azure Firewall-Instanz mit mehreren öffentlichen IP-Adressen über ein Präfix für öffentliche IP-Adressen bereitzustellen. Die bereitgestellte Firewall verfügt über Regeln einer NAT-Regelsammlung, die RDP-Verbindungen mit zwei virtuellen Windows Server 2019-Computern ermöglichen.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Weitere Informationen zu Azure Firewall mit mehreren öffentlichen IP-Adressen finden Sie unter [Bereitstellen einer Azure Firewall mit mehreren öffentlichen IP-Adressen mithilfe von Azure PowerShell](deploy-multi-public-ip-powershell.md).

Wenn Ihre Umgebung die Voraussetzungen erfüllt und Sie mit der Verwendung von ARM-Vorlagen vertraut sind, klicken Sie auf die Schaltfläche **In Azure bereitstellen**. Die Vorlage wird im Azure-Portal geöffnet.

[![In Azure bereitstellen](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Ffw-docs-qs%2Fazuredeploy.json)

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="review-the-template"></a>Überprüfen der Vorlage

Mit dieser Vorlage werden eine Azure Firewall-Instanz mit zwei öffentlichen IP-Adressen und die erforderlichen unterstützenden Ressourcen für die Azure Firewall-Instanz erstellt.

Die in dieser Schnellstartanleitung verwendete Vorlage stammt von der Seite mit den [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/fw-docs-qs).

:::code language="json" source="~/quickstart-templates/fw-docs-qs/azuredeploy.json":::

In der Vorlage sind mehrere Azure-Ressourcen definiert:

- [**Microsoft.Network/networkSecurityGroups**](/azure/templates/microsoft.network/networksecuritygroups)
- [**Microsoft.Network/publicIPPrefix**](/azure/templates/microsoft.network/publicipprefixes)
- [**Microsoft.Network/publicIPAddresses**](/azure/templates/microsoft.network/publicipaddresses)
- [**Microsoft.Network/virtualNetworks**](/azure/templates/microsoft.network/virtualnetworks)
- [**Microsoft.Compute/virtualMachines**](/azure/templates/microsoft.compute/virtualmachines)
- [**Microsoft.Storage/storageAccounts**](/azure/templates/microsoft.storage/storageAccounts)
- [**Microsoft.Network/networkInterfaces**](/azure/templates/microsoft.network/networkinterfaces)
- [**Microsoft.Network/azureFirewalls**](/azure/templates/microsoft.network/azureFirewalls)
- [**Microsoft.Network/routeTables**](/azure/templates/microsoft.network/routeTables)

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

Stellen Sie die ARM-Vorlage in Azure bereit:

1. Wählen Sie **Bereitstellung in Azure** aus, um sich bei Azure anzumelden und die Vorlage zu öffnen. Durch die Vorlage werden eine Azure Firewall-Instanz, die Netzwerkinfrastruktur und zwei virtuelle Computer erstellt.

   [![In Azure bereitstellen](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Ffw-docs-qs%2Fazuredeploy.json)

2. Geben Sie im Portal auf der Seite **Erstellen einer Azure Firewall-Instanz mit mehreren öffentlichen IP-Adressen** die folgenden Werte ein, oder wählen Sie sie aus:
   - Abonnement: Wählen Sie aus den vorhandenen Abonnements aus. 
   - Ressourcengruppe:  Wählen Sie aus den vorhandenen Ressourcengruppen aus, oder wählen Sie **Neu erstellen** und anschließend **OK** aus.
   - Standort: Standort auswählen
   - Administratorbenutzername: Geben Sie den Benutzernamen für das Administratorbenutzerkonto ein. 
   - Administratorkennwort: Geben Sie ein Administratorkennwort oder einen Administratorschlüssel ein.

3. Wählen Sie **Ich stimme den oben genannten Geschäftsbedingungen zu** und anschließend **Kaufen** aus. Die Bereitstellung kann zehn Minuten oder länger dauern.

## <a name="validate-the-deployment"></a>Überprüfen der Bereitstellung

Überprüfen Sie im Azure-Portal die bereitgestellten Ressourcen. Achten Sie auf die öffentlichen IP-Adressen der Firewall.  

Verwenden Sie eine Remotedesktopverbindung, um eine Verbindung mit den öffentlichen IP-Adressen der Firewall herzustellen. Anhand von erfolgreichen Verbindungen erkennen Sie Firewall-NAT-Regeln, die die Verbindung mit den Back-End-Servern zulassen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die mit der Firewall erstellten Ressourcen nicht mehr benötigen, löschen Sie die Ressourcengruppe. Dadurch werden die Firewall und alle zugehörigen Ressourcen entfernt.

Rufen Sie zum Löschen der Ressourcengruppe das Cmdlet `Remove-AzResourceGroup` auf:

```azurepowershell-interactive
Remove-AzResourceGroup -Name "<your resource group name>"
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Tutorial: Bereitstellen und Konfigurieren von Azure Firewall in einem Hybridnetzwerk über das Azure-Portal](tutorial-hybrid-portal.md)
