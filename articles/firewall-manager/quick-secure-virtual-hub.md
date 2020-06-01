---
title: 'Schnellstart: Schützen eines virtuellen Hubs mit Azure Firewall Manager (Vorschauversion) mithilfe einer Resource Manager-Vorlage'
description: Hier erfahren Sie, wie Sie Ihren virtuellen Hub mithilfe von Azure Firewall Manager (Vorschauversion) schützen.
services: firewall-manager
author: vhorne
ms.service: firewall
ms.topic: quickstart
ms.date: 05/19/2020
ms.author: victorh
ms.openlocfilehash: b9839e51fcea1e8fe4adc4760e16ae2d73b163ee
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2020
ms.locfileid: "83694134"
---
# <a name="quickstart-secure-your-virtual-hub-using-azure-firewall-manager---resource-manager-template"></a>Schnellstart: Schützen Ihres virtuellen Hubs mit Azure Firewall Manager mithilfe einer Resource Manager-Vorlage

In dieser Schnellstartanleitung verwenden Sie eine Resource Manager-Vorlage, um Ihren virtuellen Hub mithilfe von Azure Firewall Manager (Vorschauversion) zu schützen.

Die bereitgestellte Firewall verfügt über eine Anwendungsregel, die Verbindungen mit `www.microsoft.com` zulässt. Zum Testen der Firewall werden zwei virtuelle Computer unter Windows Server 2019 bereitgestellt. Ein Jumpserver wird zum Herstellen einer Verbindung mit dem Workloadserver verwendet. Auf dem Workloadserver können Sie nur eine Verbindung mit `www.microsoft.com` herstellen.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Unter [Was ist Azure Firewall Manager (Vorschau)?](overview.md) finden Sie weitere Informationen zur Vorschauversion von Azure Firewall Manager.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="create-a-secured-virtual-hub"></a>Erstellen eines geschützten virtuellen Hubs

Mithilfe dieser Vorlage erstellen Sie einen geschützten virtuellen Hub mit Azure Firewall Manager (Vorschauversion) sowie die erforderlichen Ressourcen zur Unterstützung des Szenarios.

### <a name="review-the-template"></a>Überprüfen der Vorlage

Die in dieser Schnellstartanleitung verwendete Vorlage stammt von der Seite mit den [Azure-Schnellstartvorlagen](https://github.com/Azure/azure-quickstart-templates/blob/master/fwm-docs-qs/azuredeploy.json).

:::code language="json" source="~/quickstart-templates/fwm-docs-qs/azuredeploy.json" range="001-477" highlight="47-76":::

In der Vorlage sind mehrere Azure-Ressourcen definiert:

- [**Microsoft.Network/publicIPAddresses**](/azure/templates/microsoft.network/publicipaddresses)
- [**Microsoft.Network/networkSecurityGroups**](/azure/templates/microsoft.network/networksecuritygroups)
- [**Microsoft.Network/virtualNetworks**](/azure/templates/microsoft.network/virtualnetworks)
- [**Microsoft.Compute/virtualMachines**](/azure/templates/microsoft.compute/virtualmachines)
- [**Microsoft.Network/networkInterfaces**](/azure/templates/microsoft.network/networkinterfaces)
- [**Microsoft.Storage/storageAccounts**](/azure/templates/microsoft.storage/storageAccounts)
- [**Microsoft.Network/azureFirewalls**](/azure/templates/microsoft.network/azureFirewalls)
- [**Microsoft.Network/firewallPolicies**](/azure/templates/microsoft.network/firewallPolicies)
- [**Microsoft.Network/routeTables**](/azure/templates/microsoft.network/routeTables)
- [**Microsoft.Network/virtualWans**](/azure/templates/microsoft.network/virtualWans)
- [**Microsoft.Network/virtualHubs**](/azure/templates/microsoft.network/virtualHubs)

### <a name="deploy-the-template"></a>Bereitstellen der Vorlage

Stellen Sie die Resource Manager-Vorlage in Azure bereit:

1. Wählen Sie **Bereitstellung in Azure** aus, um sich bei Azure anzumelden und die Vorlage zu öffnen. Mit der Vorlage werden eine Azure Firewall-Instanz, ein virtuelles WAN und ein virtueller Hub, die Netzwerkinfrastruktur sowie zwei virtuelle Computer erstellt.

   [![In Azure bereitstellen](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Ffwm-docs-qs%2Fazuredeploy.json)

2. Geben Sie im Portal auf der Seite **Geschützte virtuelle Hubs** die folgenden Werte ein, oder wählen Sie diese aus:
   - Abonnement: Wählen Sie aus den vorhandenen Abonnements aus. 
   - Ressourcengruppe:  Wählen Sie aus den vorhandenen Ressourcengruppen aus, oder wählen Sie **Neu erstellen** und anschließend **OK** aus.
   - Standort: Standort auswählen
   - Administratorbenutzername: Geben Sie den Benutzernamen für das Administratorbenutzerkonto ein. 
   - Administratorkennwort: Geben Sie ein Administratorkennwort oder einen Administratorschlüssel ein.

3. Wählen Sie **Überprüfen und erstellen** und anschließend **Erstellen** aus. Die Bereitstellung kann zehn Minuten oder länger dauern.

## <a name="validate-the-deployment"></a>Überprüfen der Bereitstellung

Testen Sie nun die Firewallregeln, um sicherzustellen, dass sie wie erwartet funktionieren.

1. Überprüfen Sie im Azure-Portal die Netzwerkeinstellungen für den virtuellen Computer **Workload-Srv**, und notieren Sie sich die private IP-Adresse.
2. Stellen Sie eine Remotedesktopverbindung mit dem virtuellen Computer **Jump-Srv** her, und melden Sie sich an. Stellen Sie von dort aus eine Remotedesktopverbindung mit der privaten IP-Adresse von **Workload-Srv** her.

3. Navigieren Sie in Internet Explorer zu `www.microsoft.com`.
4. Klicken Sie in den Sicherheitswarnungen von Internet Explorer auf **OK** > **Schließen**.

   Daraufhin sollte die Microsoft-Homepage angezeigt werden.

5. Navigieren Sie zu `www.google.com`.

   Sie sollten durch die Firewall blockiert werden.

Damit haben Sie sich vergewissert, dass die Firewallregeln funktionieren:

* Sie können zum einzigen zulässigen FQDN navigieren, aber nicht zu anderen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die mit der Firewall erstellten Ressourcen nicht mehr benötigen, löschen Sie die Ressourcengruppe. Dadurch werden die Firewall und alle zugehörigen Ressourcen entfernt.

Rufen Sie zum Löschen der Ressourcengruppe das Cmdlet `Remove-AzResourceGroup` auf:

```azurepowershell-interactive
Remove-AzResourceGroup -Name "<your resource group name>"
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Was sind vertrauenswürdige Sicherheitspartner (Vorschau)?](trusted-security-partners.md)
