---
title: 'Azure ExpressRoute: Hinzufügen der IPv6-Unterstützung mithilfe des Azure-Portals'
description: Hier erfahren Sie, wie Sie die IPv6-Unterstützung zum Herstellen einer Verbindung mit Azure-Bereitstellungen mithilfe des Azure-Portals hinzufügen.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: how-to
ms.date: 2/9/2021
ms.author: duau
ms.openlocfilehash: ef6ea9017a9aaa98e153df0d67f0b54fe5a2b64d
ms.sourcegitcommit: dac05f662ac353c1c7c5294399fca2a99b4f89c8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102124158"
---
# <a name="add-ipv6-support-for-private-peering-using-the-azure-portal-preview"></a>Hinzufügen der IPv6-Unterstützung für privates Peering mithilfe des Azure-Portal (Vorschau)

In diesem Artikel wird beschrieben, wie Sie die IPv6-Unterstützung zum Herstellen einer Verbindung mit Ihren Ressourcen in Azure über ExpressRoute mithilfe des Azure-Portal hinzufügen. 

> [!Note]
> Dieses Feature ist derzeit als Vorschauversion in den [Azure-Regionen mit Verfügbarkeitszonen](https://docs.microsoft.com/azure/availability-zones/az-region#azure-regions-with-availability-zones) verfügbar. Die ExpressRoute-Verbindung kann daher über einen beliebigen Peeringstandort erstellt werden, aber die IPv6-basierten Bereitstellungen, mit denen sie verknüpft ist, müssen sich in einer Region mit Verfügbarkeitszonen befinden.

## <a name="register-for-public-preview"></a>Registrieren für die Public Preview
Bevor Sie IPv6-Unterstützung hinzufügen können, müssen Sie Ihr Abonnement registrieren. Führen Sie zu Ihrer Registrierung folgende Schritte über Azure PowerShell aus:
1.  Melden Sie sich bei Azure an, und wählen Sie das Abonnement aus. Führen Sie diesen Schritt für das Abonnement durch, das Ihre ExpressRoute-Verbindung enthält, und für das Abonnement, das Ihre Azure-Bereitstellungen enthält (falls diese sich unterscheiden).

    ```azurepowershell-interactive
    Connect-AzAccount 

    Select-AzSubscription -Subscription "<SubscriptionID or SubscriptionName>"
    ```

2. Registrieren Sie Ihr Abonnement für die Public Preview mit dem folgenden Befehl:
    ```azurepowershell-interactive
    Register-AzProviderFeature -FeatureName AllowIpv6PrivatePeering -ProviderNamespace Microsoft.Network
    ```

Ihre Anforderung wird dann innerhalb von 2–3 Werktagen vom ExpressRoute-Team genehmigt.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Navigieren Sie in einem Browser zum [Azure-Portal](https://portal.azure.com), und melden Sie sich mit Ihrem Azure-Konto an.

## <a name="add-ipv6-private-peering-to-your-expressroute-circuit"></a>Hinzufügen von privatem IPv6-Peering zu Ihrer ExpressRoute-Verbindung

1. [Erstellen Sie eine ExpressRoute-Verbindung](https://docs.microsoft.com/azure/expressroute/expressroute-howto-circuit-portal-resource-manager), oder navigieren Sie zur vorhandenen Verbindung, die Sie ändern möchten.

    :::image type="content" source="./media/expressroute-howto-add-ipv6-portal/navigate-to-circuit.png" alt-text="Navigieren zur Verbindung":::

2. Wählen Sie als Peeringkonfiguration **Azure, privat** aus.

    :::image type="content" source="./media/expressroute-howto-add-ipv6-portal/navigate-to-peering.png" alt-text="Navigieren zum Peering":::

3. Fügen Sie ein privates IPv6-Peering zu Ihrer vorhandenen privaten IPv4-Peeringkonfiguration hinzu, indem Sie „Both“ (Beide) für **Subnets** (Subnetze) auswählen oder das private IPv6-Peering für eine neue Verbindung durch Auswählen von „IPv6“ aktivieren. Geben Sie zwei /126 IPv6-Subnetze an, die Sie für Ihre primäre bzw. sekundäre Verknüpfung besitzen. Über jedes dieser Subnetze weisen Sie die erste verwendbare IP-Adresse für Ihren Router zu, da die zweite verwendbare IP-Adresse von Microsoft für den eigenen Router genutzt wird. **Speichern** Sie Ihre Peeringkonfiguration, sobald Sie alle Parameter angegeben haben.

    :::image type="content" source="./media/expressroute-howto-add-ipv6-portal/add-ipv6-peering.png" alt-text="Hinzufügen des privaten IPv6-Peerings":::

4. Nachdem die Konfiguration akzeptiert wurde, sieht die Anzeige in etwa wie im folgenden Beispiel aus:

    :::image type="content" source="./media/expressroute-howto-add-ipv6-portal/view-ipv6-peering.png" alt-text="Ansicht des privaten IPv6-Peerings":::

## <a name="update-your-connection-to-an-existing-virtual-network"></a>Aktualisieren Sie die Verbindung mit einem vorhandenen virtuellen Netzwerk.

Führen Sie die folgenden Schritte aus, wenn Sie über eine vorhandene Umgebung von Azure-Ressourcen in einer Region mit Verfügbarkeitszonen verfügen, für die Sie das private IPv6-Peering verwenden möchten.

1. Navigieren Sie zu dem virtuellen Netzwerk, mit dem Ihre ExpressRoute-Verbindung verknüpft ist.

    :::image type="content" source="./media/expressroute-howto-add-ipv6-portal/navigate-to-vnet.png" alt-text="Navigation zum virtuellen Netzwerk":::

2. Navigieren Sie zur Registerkarte **Adressraum**, und fügen Sie einen IPv6-Adressraum zu Ihrem virtuellen Netzwerk hinzu. **Speichern** Sie Ihren Adressraum.

    :::image type="content" source="./media/expressroute-howto-add-ipv6-portal/add-ipv6-space.png" alt-text="Hinzufügen des IPv6-Adressraums":::

3. Navigieren Sie zur Registerkarte **Subnetze**, und wählen Sie das **GatewaySubnet** aus. Aktivieren Sie das Kontrollkästchen **IPv6-Adresse hinzufügen**, und geben Sie einen IPv6-Adressraum für Ihr Subnetz an. Das Gateway-IPv6-Subnetz sollte /64 oder größer sein. **Speichern** Sie Ihre Konfiguration, nachdem Sie alle Parameter angegeben haben.

    :::image type="content" source="./media/expressroute-howto-add-ipv6-portal/add-ipv6-gateway-space.png" alt-text="Hinzufügen des IPv6-Adressraums zum Subnetz":::

4. Wenn Sie über ein vorhandenes zonenredundantes Gateway verfügen, navigieren Sie zu Ihrem ExpressRoute-Gateway, und aktualisieren Sie die Ressource, um IPv6-Konnektivität zu aktivieren. Sie können alternativ [das Gateway des virtuellen Netzwerks erstellen](https://docs.microsoft.com/azure/expressroute/expressroute-howto-add-gateway-portal-resource-manager), indem Sie eine zonenredundante SKU (ErGw1AZ, ErGw2AZ, ErGw3AZ) verwenden. Wenn Sie FastPath verwenden möchten, verwenden Sie ErGw3AZ.

    :::image type="content" source="./media/expressroute-howto-add-ipv6-portal/refresh-gateway.png" alt-text="Aktualisieren des Gateways":::

## <a name="create-a-connection-to-a-new-virtual-network"></a>Herstellen einer Verbindung mit einem neuen virtuellen Netzwerk

Führen Sie die folgenden Schritte aus, wenn Sie vorhaben, eine Verbindung zu neuen Azure-Ressourcen in einer Region mit Verfügbarkeitszonen über Ihr privates IPv6-Peering herzustellen.

1. Erstellen Sie ein virtuelles Netzwerk mit dualem Stapel mit dem IPv4- und dem IPv6-Adressraum. Weitere Informationen finden Sie unter [Erstellen eines virtuellen Netzwerks](https://docs.microsoft.com/azure/virtual-network/quick-create-portal#create-a-virtual-network).

2. [Erstellen Sie das Gatewaysubnetz mit dualem Stapel](https://docs.microsoft.com/azure/expressroute/expressroute-howto-add-gateway-portal-resource-manager#create-the-gateway-subnet).

3. [Erstellen Sie das Gateway des virtuellen Netzwerks](https://docs.microsoft.com/azure/expressroute/expressroute-howto-add-gateway-portal-resource-manager#create-the-virtual-network-gateway) mithilfe einer zonenredundanten SKU (ErGw1AZ, ErGw2AZ, ErGw3AZ). Wenn Sie FastPath verwenden möchten, verwenden Sie ErGw3AZ (beachten Sie, dass dies nur für Verbindungen mit ExpressRoute Direct verfügbar ist).

4. [Verknüpfen Sie Ihr virtuelles Netzwerks mit Ihrer ExpressRoute-Verbindung](https://docs.microsoft.com/azure/expressroute/expressroute-howto-linkvnet-portal-resource-manager).

## <a name="limitations"></a>Einschränkungen
Obwohl IPv6-Unterstützung für Verbindungen zu Bereitstellungen in Regionen mit Verfügbarkeitszonen zur Verfügung steht, werden die folgenden Anwendungsfälle nicht unterstützt:

* Verbindungen mit Bereitstellungen in Azure über eine ExpressRoute-Gateway-SKU ohne Verfügbarkeitszone
* Verbindungen mit Bereitstellungen in Regionen ohne Verfügbarkeitszone
* Global Reach-Verbindungen zwischen ExpressRoute-Verbindungen
* Verwendung von ExpressRoute mit virtuellem WAN
* FastPath mit Verbindungen, die nicht ExpressRoute Direct-Verbindungen sind
* Koexistenz mit VPN Gateway

## <a name="next-steps"></a>Nächste Schritte

Informationen zur Problembehandlung bei ExpressRoute-Verbindungen finden Sie in den folgenden Artikeln:

* [Überprüfen der ExpressRoute-Konnektivität](expressroute-troubleshooting-expressroute-overview.md)
* [Beheben von Problemen bei der Netzwerkleistung](expressroute-troubleshooting-network-performance.md)
