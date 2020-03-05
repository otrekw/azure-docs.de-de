---
title: 'Schnellstart: Erstellen einer Load Balancer-Instanz – Azure-Vorlage'
titleSuffix: Azure Load Balancer
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie einen Lastenausgleich per Azure Resource Manager-Vorlage erstellen.
services: load-balancer
documentationcenter: na
author: asudbring
manager: twooley
Customer intent: I want to create a load balancer by using an Azure Resource Manager template so that I can load balance internet traffic to VMs.
ms.service: load-balancer
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/26/2020
ms.author: allensu
ms.custom: mvc,subject-armqs
ms.openlocfilehash: 1f681a9a8d8547589873cff055d9f59973c27355
ms.sourcegitcommit: f915d8b43a3cefe532062ca7d7dbbf569d2583d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2020
ms.locfileid: "78295784"
---
# <a name="quickstart-create-a-load-balancer-to-load-balance-vms-by-using-azure-resource-manager-template"></a>Schnellstart: Erstellen einer Load Balancer-Instanz für den Lastenausgleich virtueller Computer mithilfe einer Azure Resource Manager-Vorlage

Durch die Verteilung der eingehenden Anforderungen auf mehrere virtuelle Computer (VMs) bietet ein Lastenausgleich ein höheres Maß an Verfügbarkeit und Skalierbarkeit. Dieser Schnellstart zeigt Ihnen, wie Sie eine Azure Resource Manager-Vorlage bereitstellen, die einen Lastenausgleich im Tarif „Standard“ für VMs erstellt. Bei Verwendung der Resource Manager-Vorlage werden im Vergleich zu anderen Bereitstellungsmethoden weniger Schritte benötigt.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="create-a-load-balancer"></a>Erstellen einer Load Balancer-Instanz

Die Load Balancer-SKU und die SKU für öffentliche IP-Adressen müssen übereinstimmen. Wenn Sie eine Load Balancer Standard-Instanz erstellen, müssen Sie auch eine neue öffentliche Standard-IP-Adresse erstellen, die als Front-End für diese Load Balancer Standard-Instanz konfiguriert ist. Wenn Sie eine Load Balancer Basic-Instanz erstellen möchten, verwenden Sie [diese Vorlage](https://azure.microsoft.com/resources/templates/201-2-vms-loadbalancer-natrules/). Microsoft empfiehlt für Produktionsworkloads die Verwendung der Standard-SKU.

### <a name="review-the-template"></a>Überprüfen der Vorlage

Die in dieser Schnellstartanleitung verwendete Vorlage stammt von der Seite mit den [Azure-Schnellstartvorlagen](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-load-balancer-standard-create/azuredeploy.json).

:::code language="json" source="~/quickstart-templates/101-load-balancer-standard-create/azuredeploy.json" range="1-150" highlight="58-122":::

In der Vorlage wurden mehrere Azure-Ressourcen definiert:

- [**Microsoft.Network/loadBalancers**](/azure/templates/microsoft.network/loadbalancers)
- [**Microsoft.Network/publicIPAddresses**](/azure/templates/microsoft.network/publicipaddresses): Für den Lastenausgleich und jeweils die drei virtuellen Computer.
- [**Microsoft.Network/networkSecurityGroups**](/azure/templates/microsoft.network/networksecuritygroups)
- [**Microsoft.Network/virtualNetworks**](/azure/templates/microsoft.network/virtualnetworks)
- [**Microsoft.Compute/virutalMachines**](/azure/templates/microsoft.compute/virtualmachines) (3)
- [**Microsoft.Network/networkInterfaces**](/azure/templates/microsoft.network/networkinterfaces) (3)
- [**Microsoft.Compute/virtualMachine/extensions**](/azure/templates/microsoft.compute/virtualmachines/extensions) (3): Für die Konfiguration von IIS und der Webseiten.

Weitere Vorlagen zum Azure Load Balancer finden Sie unter [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Network&pageNumber=1&sort=Popular).

### <a name="deploy-the-template"></a>Bereitstellen der Vorlage

1. Wählen Sie **Try it** (Ausprobieren) im folgenden Codeblock aus, um Azure Cloud Shell zu öffnen. Folgen Sie dann den Anweisungen, um sich bei Azure anzumelden.

   ```azurepowershell-interactive
   $projectName = Read-Host -Prompt "Enter a project name with 12 or less letters or numbers that is used to generate Azure resource names"
   $location = Read-Host -Prompt "Enter the location (i.e. centralus)"
   $adminUserName = Read-Host -Prompt "Enter the virtual machine administrator account name"
   $adminPassword = Read-Host -Prompt "Enter the virtual machine administrator password" -AsSecureString

   $resourceGroupName = "${projectName}rg"
   $templateUri = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-load-balancer-standard-create/azuredeploy.json"

   New-AzResourceGroup -Name $resourceGroupName -Location $location
   New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri -projectName $projectName -location $location -adminUsername $adminUsername -adminPassword $adminPassword

   Write-Host "Press [ENTER] to continue."
   ```

   Warten Sie, bis die Aufforderung in der Konsole angezeigt wird.

1. Wählen Sie **Copy** (Kopieren) im vorherigen Codeblock aus, um das PowerShell-Skript zu kopieren.

1. Klicken Sie mit der rechten Maustaste auf den Shellkonsolenbereich, und wählen Sie **Einfügen** aus.

1. Gehen Sie die Werte ein.

   Die Vorlagenbereitstellung erstellt drei Verfügbarkeitszonen. Verfügbarkeitszonen werden nur in [bestimmten Regionen](../availability-zones/az-overview.md) unterstützt. Verwenden Sie eine der unterstützten Regionen. Wenn Sie nicht sicher sind, geben Sie **centralus** ein.

   Der Ressourcengruppenname ist der Projektname mit dem Zusatz **rg**. Sie benötigen den Ressourcengruppennamen im nächsten Abschnitt.

Das Bereitstellen der Vorlage dauert ungefähr 10 Minuten. Nach Abschluss des Vorgangs sieht die Ausgabe in etwa wie folgt aus:

![Azure Load Balancer Standard – Resource Manager-Vorlage: PowerShell-Bereitstellungsausgabe](./media/quickstart-load-balancer-standard-public-template/azure-standard-load-balancer-resource-manager-template-powershell-output.png)

Azure PowerShell wird verwendet, um die Vorlage bereitzustellen. Neben Azure PowerShell können Sie auch das Azure-Portal, die Azure-CLI und die REST-API verwenden. Informationen zu anderen Bereitstellungsmethoden finden Sie unter [Bereitstellen von Vorlagen](../azure-resource-manager/templates/deploy-portal.md).

## <a name="test-the-load-balancer"></a>Testen des Lastenausgleichs

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Wählen Sie im linken Bereich **Ressourcengruppen** aus.

1. Wählen Sie die Ressourcengruppe aus, die Sie im vorherigen Abschnitt erstellt haben. Der Ressourcengruppenname entspricht standardmäßig dem Projektnamen mit dem Zusatz **rg**.

1. Wählen Sie den Load Balancer aus. Sein Standardname entspricht dem Projektnamen mit dem Zusatz **-lb**.

1. Kopieren Sie nur den IP-Adressteil der öffentlichen IP-Adresse, und fügen Sie ihn in die Adressleiste des Browsers ein.

   ![Azure Load Balancer Standard – Resource Manager-Vorlage: Öffentliche IP-Adresse](./media/quickstart-load-balancer-standard-public-template/azure-standard-load-balancer-resource-manager-template-deployment-public-ip.png)

    Der Browser zeigt die Standardseite des Internetinformationsdienste-Webservers (Internet Information Services, IIS) an.

   ![IIS-Webserver](./media/quickstart-load-balancer-standard-public-template/load-balancer-test-web-page.png)

Sie können vom Clientcomputer aus eine erzwungene Aktualisierung Ihres Webbrowsers durchführen, um zu verfolgen, wie der Lastenausgleich den Datenverkehr auf alle drei virtuellen Computer verteilt.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Sie können die Ressourcengruppe, den Lastenausgleich und alle dazugehörigen Ressourcen löschen, wenn Sie sie nicht mehr benötigen. Wählen Sie hierzu im Azure-Portal die Ressourcengruppe aus, die den Lastenausgleich enthält, und wählen Sie anschließend **Ressourcengruppe löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie einen Lastenausgleich im Tarif „Standard“ erstellt, virtuelle Computer angefügt, die Datenverkehrsregel für den Lastenausgleich konfiguriert, einen Integritätstest durchgeführt und den Lastenausgleich getestet.

Weitere Informationen zum Lastenausgleich finden Sie in den Tutorials.

> [!div class="nextstepaction"]
> [Azure Load Balancer-Tutorials](tutorial-load-balancer-standard-public-zone-redundant-portal.md)
