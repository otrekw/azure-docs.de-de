---
title: 'Azure ExpressRoute-Vorlage: Erstellen Sie eine ExpressRoute-Verbindung.'
description: Erfahren Sie, wie Sie eine Azure ExpressRoute-Leitung erstellen, indem Sie eine Azure Resource Manager-Vorlage mit Azure PowerShell bereitstellen.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: how-to
ms.date: 11/13/2019
ms.author: duau
ms.openlocfilehash: 51bd54fddfdbd5978c3a262ffcd81ff204d7d156
ms.sourcegitcommit: 2e123f00b9bbfebe1a3f6e42196f328b50233fc5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/27/2021
ms.locfileid: "108073879"
---
# <a name="create-an-expressroute-circuit-by-using-azure-resource-manager-template"></a>Erstellen einer ExpressRoute-Verbindung mithilfe einer Azure Resource Manager-Vorlage

> [!div class="op_single_selector"]
> * [Azure portal](expressroute-howto-circuit-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-circuit-arm.md)
> * [Azure-Befehlszeilenschnittstelle](howto-circuit-cli.md)
> * [Azure Resource Manager-Vorlage](expressroute-howto-circuit-resource-manager-template.md)
> * [Video – Azure-Portal](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [PowerShell (klassisch)](expressroute-howto-circuit-classic.md)
>

Erfahren Sie, wie Sie eine ExpressRoute-Verbindung erstellen, indem Sie eine Azure Resource Manager-Vorlage mit Azure PowerShell bereitstellen. Weitere Informationen zur Entwicklung von Resource Manager-Vorlagen finden Sie in der [Resource Manager-Dokumentation](../azure-resource-manager/index.yml) und der [Vorlagenreferenz](/azure/templates/microsoft.network/expressroutecircuits).

## <a name="before-you-begin"></a>Voraussetzungen

* Lesen Sie vor Beginn der Konfiguration die Seiten zu den [Voraussetzungen](expressroute-prerequisites.md) und [Workflows](expressroute-workflows.md).
* Stellen Sie sicher, dass Sie über die notwendigen Berechtigungen verfügen, um neue Netzwerkressourcen zu erstellen. Wenden Sie sich an Ihren Kontoadministrator, wenn Sie nicht über die richtigen Berechtigungen verfügen.
* Sie können sich das [Video ansehen](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit), bevor Sie beginnen, um die Schritte besser zu verstehen.

## <a name="create-and-provision-an-expressroute-circuit"></a><a name="create"></a>Erstellen und Bereitstellen einer ExpressRoute-Verbindung

[Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/) bieten eine gute Sammlung von Resource Manager-Vorlagen. Verwenden Sie eine der [vorhandenen Vorlagen](https://azure.microsoft.com/resources/templates/101-expressroute-circuit-create/) zum Erstellen einer ExpressRoute-Verbindung.

[!code-json[create-azure-expressroute-circuit](~/quickstart-templates/quickstarts/microsoft.network/expressroute-circuit-create/azuredeploy.json)]

[Hier](https://azure.microsoft.com/resources/templates/?term=expressroute) finden Sie weitere verwandte Vorlagen.

So erstellen Sie eine ExpressRoute-Verbindung durch Bereitstellen einer Vorlage:

1. Wählen Sie im folgenden Codeblock **Ausprobieren** aus, und folgen Sie dann den Anweisungen, um sich bei Azure Cloud Shell anzumelden.

    ```azurepowershell-interactive
    $circuitName = Read-Host -Prompt "Enter a circuit name"
    $location = Read-Host -Prompt "Enter the location (i.e. centralus)"
    $resourceGroupName = "${circuitName}rg"
    $templateUri = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-expressroute-circuit-create/azuredeploy.json"

    $serviceProviderName = "Equinix"
    $peeringLocation = "Silicon Valley"
    $bandwidthInMbps = 500
    $sku_tier = "Premium"
    $sku_family = "MeteredData"

    New-AzResourceGroup -Name $resourceGroupName -Location $location
    New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri -circuitName $circuitName -serviceProviderName $serviceProviderName -peeringLocation $peeringLocation -bandwidthInMbps $bandwidthInMbps -sku_tier $sku_tier -sku_family $sku_family

    Write-Host "Press [ENTER] to continue ..."
    ```

   * Der **SKU-Tarif** bestimmt, ob eine ExpressRoute-Verbindung eine [lokale](expressroute-faqs.md#expressroute-local), Standard- oder [Premium](expressroute-faqs.md#expressroute-premium)-Verbindung ist. Sie können *Lokal*, *Standard oder *Premium* angeben. Es ist nicht möglich, die SKU von *Standard/Premium* in *Local* zu ändern.
   * Die **SKU-Familie** bestimmt den Abrechnungstyp. Sie können *Metereddata* für einen Volumentarif und *Unlimiteddata* für einen Plan mit Datenflatrate auswählen. Sie können den Abrechnungstyp von *Metereddata* in *Unlimiteddata* ändern, nicht jedoch umgekehrt (*Unlimiteddata* in *Metereddata*). Eine *lokale* Verbindung kann nur vom Typ *Unlimiteddata* sein.
   * Der **Peeringort** ist der physische Standort, an dem Ihr Peering mit Microsoft stattfindet.

     > [!IMPORTANT]
     > Der Peeringstandort entspricht dem [physischen Standort](expressroute-locations.md), an dem Ihr Peering mit Microsoft stattfindet. Dieser ist **nicht** mit der Eigenschaft „Standort“ verknüpft, die sich auf den geografischen Standort des Azure-Netzwerkressourcenanbieters befindet. Obgleich sie nicht miteinander in Zusammenhang stehen, sollten Sie einen Netzwerkressourcenanbieter in geografischer Nähe des Peeringstandorts der Verbindung wählen.

    Der Ressourcengruppenname entspricht dem Service Bus-Namespace mit dem Zusatz **rg**.

2. Wählen Sie **Kopieren**, um das PowerShell-Skript zu kopieren.
3. Klicken Sie mit der rechten Maustaste auf die Shellkonsole, und wählen Sie **Einfügen** aus.

Es dauert einen Augenblick, einen Event Hub zu erstellen.

In diesem Tutorial wird Azure PowerShell verwendet, um die Vorlage bereitzustellen. Weitere Methoden zum Bereitstellen von Vorlagen finden Sie unter:

* [Über das Azure-Portal](../azure-resource-manager/templates/deploy-portal.md).
* [Mit der Azure CLI](../azure-resource-manager/templates/deploy-cli.md).
* [Mit der REST-API](../azure-resource-manager/templates/deploy-rest.md).

## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a><a name="delete"></a>Aufheben der Bereitstellung und Löschen einer ExpressRoute-Verbindung

Sie können Ihre ExpressRoute-Verbindung löschen. Wählen Sie dazu das Symbol **Löschen** aus. Beachten Sie die folgenden Informationen:

* Sie müssen die Verknüpfung aller virtuellen Netzwerke mit der ExpressRoute-Verbindung aufheben. Falls dieser Vorgang nicht erfolgreich ist, überprüfen Sie, ob noch virtuelle Netzwerke mit der Verbindung verknüpft sind.
* Wenn der Bereitstellungsstatus des ExpressRoute-Verbindungsdienstanbieters **Bereitstellung** oder **Bereitgestellt** lautet, arbeiten Sie mit Ihrem Dienstanbieter zusammen, um die Verbindungsbereitstellung auf Anbieterseite aufzuheben. Microsoft reserviert weiterhin Ressourcen für Sie und stellt Ihnen dies in Rechnung, bis der Dienstanbieter die Aufhebung der Verbindungsbereitstellung abgeschlossen hat und uns benachrichtigt.
* Wenn der Dienstanbieter die Bereitstellung der Verbindung aufgehoben hat (Bereitstellungsstatus des Dienstanbieters lautet **Nicht bereitgestellt**), können Sie die Verbindung löschen. Damit wird die Abrechnung für die Verbindung beendet.

Sie können die ExpressRoute-Verbindung löschen, indem Sie den folgenden PowerShell-Befehl ausführen:

```azurepowershell-interactive
$circuitName = Read-Host -Prompt "Enter the same circuit name that you used earlier"
$resourceGroupName = "${circuitName}rg"

Remove-AzExpressRouteCircuit -ResourceGroupName $resourceGroupName -Name $circuitName
```

## <a name="next-steps"></a>Nächste Schritte

Führen Sie nach dem Erstellen Ihrer Verbindung folgende Aufgaben durch:

* [Erstellen und Ändern des Routings für Ihre ExpressRoute-Verbindung](expressroute-howto-routing-portal-resource-manager.md)
* [Verknüpfen Ihres virtuelles Netzwerks mit Ihrer ExpressRoute-Verbindung](expressroute-howto-linkvnet-arm.md)