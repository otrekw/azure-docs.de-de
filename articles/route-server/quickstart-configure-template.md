---
title: 'Schnellstart: Erstellen eines Azure Route Servers mithilfe einer Azure Resource Manager-Vorlage (ARM-Vorlage)'
description: In dieser Schnellstartanleitung wird gezeigt, wie Sie einen Azure Route Server mit einer Azure Resource Manager-Vorlage (ARM-Vorlage) erstellen.
services: route-server
author: duongau
ms.service: route-server
ms.topic: quickstart
ms.custom: subject-armqs
ms.date: 04/05/2021
ms.author: duau
ms.openlocfilehash: 6f56b9fb1f6a1f5a1fe0811617fb20412c52fd72
ms.sourcegitcommit: 56b0c7923d67f96da21653b4bb37d943c36a81d6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2021
ms.locfileid: "106450839"
---
# <a name="quickstart-create-an-azure-route-server-using-an-arm-template"></a>Schnellstart: Erstellen eines Azure Route Servers mit einer ARM-Vorlage

In dieser Schnellstartanleitung wird beschrieben, wie Sie eine Azure Resource Manager-Vorlage (ARM-Vorlage) verwenden, um einen Azure Route Server in einem neuen oder vorhandenen virtuellen Netzwerk bereitzustellen.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Wenn Ihre Umgebung die Voraussetzungen erfüllt und Sie mit der Verwendung von ARM-Vorlagen vertraut sind, klicken Sie auf die Schaltfläche **In Azure bereitstellen**. Die Vorlage wird im Azure-Portal geöffnet.

[![In Azure bereitstellen](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-route-server%2Fazuredeploy.json)

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="review-the-template"></a>Überprüfen der Vorlage

Die in dieser Schnellstartanleitung verwendete Vorlage stammt von der Seite mit den [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/101-route-server).

In dieser Schnellstartanleitung stellen Sie einen Azure Route Server in einem neuen oder vorhandenen virtuellen Netzwerk bereit. Ein dediziertes Subnetz mit dem Namen `RouteServerSubnet` wird erstellt, um den Route Server zu hosten. Um BGP-Peering einzurichten, wird der Route Server zudem mit der Peer-ASN und der Peer-IP-Adresse konfiguriert.

:::code language="json" source="~/quickstart-templates/101-route-server/azuredeploy.json" range="001-145" highlight="105-142":::

In der Vorlage wurden mehrere Azure-Ressourcen definiert:

* [**Microsoft.Network/virtualNetworks**](/azure/templates/microsoft.network/virtualNetworks)
* [**Microsoft.Network/virtualNetworks/subnets**](/azure/templates/microsoft.network/virtualNetworks/subnets): (zwei Subnetze, eines mit dem Name `routeserversubnet`)
* [**Microsoft. Network/virtualhubs**](/azure.templates/microsoft.network/virtualhubs) (Bereitstellung von Route Server)
* [**Microsoft.Network/virtualHubs/ipConfigurations**](/azure.templates/microsoft.network/virtualhubs/ipConfigurations)
* [**Microsoft. Network/virtualhubs/bgpConnections**](/azure.templates/microsoft.network/virtualhubs/bgpConnections) (Peer-ASN und Peer-IP-Konfiguration)


Weitere Vorlagen zu ExpressRoute finden Sie unter [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Network&pageNumber=1&sort=Popular).

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

1. Wählen Sie **Try it** (Ausprobieren) im folgenden Codeblock aus, um Azure Cloud Shell zu öffnen. Folgen Sie dann den Anweisungen, um sich bei Azure anzumelden.

    ```azurepowershell-interactive
    $projectName = Read-Host -Prompt "Enter a project name that is used for generating resource names"
    $location = Read-Host -Prompt "Enter the location (i.e. centralus)"
    $templateUri = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-route-server/azuredeploy.json"

    $resourceGroupName = "${projectName}rg"

    New-AzResourceGroup -Name $resourceGroupName -Location "$location"
    New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri

    Read-Host -Prompt "Press [ENTER] to continue ..."
    ```

    Warten Sie, bis die Aufforderung in der Konsole angezeigt wird.

1. Wählen Sie **Copy** (Kopieren) im vorherigen Codeblock aus, um das PowerShell-Skript zu kopieren.

1. Klicken Sie mit der rechten Maustaste auf den Shellkonsolenbereich, und wählen Sie **Einfügen** aus.

1. Gehen Sie die Werte ein.

    Der Ressourcengruppenname ist der Projektname mit dem Zusatz **rg**.

    Das Bereitstellen der Vorlage dauert ungefähr 20 Minuten. Nach Abschluss des Vorgangs sieht die Ausgabe in etwa wie folgt aus:

    :::image type="content" source="./media/quickstart-configure-template/powershell-output.png" alt-text="Ausgabe der PowerShell-Bereitstellung für die Route Server Resource Manager-Vorlage.":::

Azure PowerShell wird verwendet, um die Vorlage bereitzustellen. Neben Azure PowerShell können Sie auch das Azure-Portal, die Azure-CLI und die REST-API verwenden. Informationen zu anderen Bereitstellungsmethoden finden Sie unter [Bereitstellen von Vorlagen](../azure-resource-manager/templates/deploy-portal.md).

## <a name="validate-the-deployment"></a>Überprüfen der Bereitstellung

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Wählen Sie im linken Bereich **Ressourcengruppen** aus.

1. Wählen Sie die Ressourcengruppe aus, die Sie im vorherigen Abschnitt erstellt haben. Der Ressourcengruppenname entspricht standardmäßig dem Projektnamen mit dem Zusatz **rg**.

1. Die Ressourcengruppe darf nur das virtuelle Netzwerk enthalten:

     :::image type="content" source="./media/quickstart-configure-template/resource-group.png" alt-text="Ressourcengruppe mit virtuellem Netzwerk der Route Server-Bereitstellung.":::

1. Gehe zu https://aka.ms/routeserver.

1. Wählen Sie den Route Server mit dem Namen **routeserver** aus, um zu verifizieren, dass die Bereitstellung erfolgreich war.

    :::image type="content" source="./media/quickstart-configure-template/deployment.png" alt-text="Screenshot: Übersichtsseite von Route Server":::

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcengruppe, wenn Sie die mit dem Route Server erstellten Ressourcen nicht mehr benötigen. Dadurch werden der Route Server und alle zugehörigen Ressourcen gelöscht.

Rufen Sie zum Löschen der Ressourcengruppe das Cmdlet `Remove-AzResourceGroup` auf:

```azurepowershell-interactive
Remove-AzResourceGroup -Name <your resource group name>
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie Folgendes erstellt:

* Route Server
* Virtual Network
* Subnet

Informieren Sie sich nach dem Erstellen der Azure Route Server-Instanz darüber, wie Azure Route Server mit ExpressRoute- und VPN-Gateways interagiert: 

> [!div class="nextstepaction"]
> [Unterstützung von Azure ExpressRoute und Azure VPN](expressroute-vpn-support.md)
