---
title: Bereitstellen und Konfigurieren von Azure VMware Solution
description: Hier erfahren Sie, wie Sie die in der Planungsphase gesammelten Informationen verwenden, um die private Azure VMware Solution-Cloud bereitzustellen und zu konfigurieren.
ms.topic: tutorial
ms.custom: contperf-fy21q4
ms.date: 04/23/2021
ms.openlocfilehash: fe5e34ecb0ed3d8984432d44bbed5678c4592553
ms.sourcegitcommit: 516eb79d62b8dbb2c324dff2048d01ea50715aa1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108175854"
---
# <a name="deploy-and-configure-azure-vmware-solution"></a>Bereitstellen und Konfigurieren von Azure VMware Solution

In diesem Artikel verwenden Sie die Informationen aus dem [Planungsprozess](production-ready-deployment-steps.md), um Azure VMware Solution bereitzustellen und zu konfigurieren. 

>[!IMPORTANT]
>Es handelt sich dabei um Informationen, die Sie im [Planungsabschnitt](production-ready-deployment-steps.md) durchgegangen sind, bevor Sie fortfahren.

## <a name="step-1-register-the-microsoftavs-resource-provider"></a>Schritt 1: Registrieren des **Microsoft.AVS**-Ressourcenanbieters

[!INCLUDE [register-resource-provider-steps](includes/register-resource-provider-steps.md)]

## <a name="step-2-create-an-azure-vmware-solution-private-cloud"></a>Schritt 2: Erstellen einer privaten Cloud von Azure VMware Solution

[!INCLUDE [create-private-cloud-azure-portal-steps](includes/create-private-cloud-azure-portal-steps.md)]

>[!NOTE]
>Eine umfassende Übersicht über diesen Schritt vermittelt das Video [Azure VMware Solution: Bereitstellen](https://www.youtube.com/embed/gng7JjxgayI).


## <a name="step-3-connect-to-azure-virtual-network-with-expressroute"></a>Schritt 3: Herstellen einer Verbindung mit Azure Virtual Network über ExpressRoute

In der Planungsphase haben Sie definiert, ob Sie ein *vorhandenes* oder *neues* ExpressRoute-VNET-Gateway verwenden möchten.  

### <a name="use-a-new-expressroute-virtual-network-gateway"></a>Verwenden eines neuen ExpressRoute-VNET-Gateways

>[!IMPORTANT]
>Sie benötigen ein virtuelles Netzwerk mit einem Gatewaysubnetz, das noch **nicht** über ein VNET-Gateway verfügt.

| Wenn | Then  |
| --- | --- |
| Sie noch kein virtuelles Netzwerk besitzen...     |  Erstellen Sie Folgendes:<ul><li><a href="tutorial-configure-networking.md#create-a-virtual-network">Virtuelles Netzwerk</a></li><li><a href="../expressroute/expressroute-howto-add-gateway-portal-resource-manager.md#create-the-gateway-subnet">GatewaySubnet</a></li><li><a href="tutorial-configure-networking.md#create-a-virtual-network-gateway">Gateway für virtuelle Netzwerke</a></li></ul>        |
| Sie bereits ein virtuelles Netzwerk **ohne** Gatewaysubnetz besitzen...   | Erstellen Sie Folgendes: <ul><li><a href="../expressroute/expressroute-howto-add-gateway-portal-resource-manager.md#create-the-gateway-subnet">GatewaySubnet</a></li><li><a href="tutorial-configure-networking.md#create-a-virtual-network-gateway">Gateway für virtuelle Netzwerke</a></li></ul>          |
| Sie bereits ein virtuelles Netzwerk **mit** Gatewaysubnetz besitzen... | [Erstellen eines Gateways für das virtuelle Netzwerk](tutorial-configure-networking.md#create-a-virtual-network-gateway)   |


### <a name="use-an-existing-virtual-network-gateway"></a>Verwenden eines vorhandenen VNET-Gateways

[!INCLUDE [connect-expressroute-to-vnet](includes/connect-expressroute-vnet.md)]


## <a name="step-4-validate-the-connection"></a>Schritt 4. Überprüfen der Verbindung

Zwischen der Azure Virtual Network-Instanz, in der die ExpressRoute-Leitung endet, und der privaten Azure VMware Solution-Cloud muss eine Verbindung bestehen. 

1. Verwenden Sie einen [virtuellen Computer](../virtual-machines/windows/quick-create-portal.md#create-virtual-machine) in der Azure Virtual Network-Instanz, in der die ExpressRoute-Leitung von Azure VMware Solution endet (siehe [Schritt 3: Herstellen einer Verbindung mit Azure Virtual Network über ExpressRoute](#step-3-connect-to-azure-virtual-network-with-expressroute)).  

   1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
   2. Navigieren Sie zu einem ausgeführten virtuellen Computer, und wählen Sie unter **Einstellungen** die Option **Netzwerk** und dann die Netzwerkschnittstellenressource aus.
      ![Anzeigen von Netzwerkschnittstellen](../virtual-network/media/diagnose-network-routing-problem/view-nics.png)
   4. Wählen Sie auf der linken Seite **Effektive Routen** aus. Eine Liste der Adresspräfixe wird angezeigt, die in dem CIDR-Block vom Typ `/22` enthalten sind, den Sie während der Bereitstellungsphase eingegeben haben.

1. Wenn Sie sich sowohl bei vCenter als auch bei NSX-T Manager anmelden möchten, öffnen Sie einen Webbrowser, und melden Sie sich bei demselben virtuellen Computer an, der auch für die Netzwerkroutenüberprüfung verwendet wird.  

   Sie können die IP-Adressen und Anmeldeinformationen von vCenter und NSX-T Manager im Azure-Portal ermitteln.  Wählen Sie Ihre private Cloud und dann **Verwalten** > **Identität** aus.

   :::image type="content" source="media/tutorial-access-private-cloud/ss4-display-identity.png" alt-text="Screenshot: URLs und Anmeldeinformationen für vCenter und NSX Manager der privaten Cloud" border="true":::


## <a name="next-steps"></a>Nächste Schritte

Im nächsten Abschnitt verbinden Sie Azure VMware Solution über ExpressRoute mit Ihrem lokalen Netzwerk.

> [!div class="nextstepaction"]
> [Tutorial: Zusammenfassen lokaler Umgebungen mittels Peering zu einer privaten Cloud](tutorial-expressroute-global-reach-private-cloud.md)