---
title: include file
description: include file
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 07/16/2021
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 5b47f21ba6597380ca79bfce03feb8edd62aa5ae
ms.sourcegitcommit: 7d63ce88bfe8188b1ae70c3d006a29068d066287
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2021
ms.locfileid: "114439661"
---
1. Geben Sie unter **Nach Ressourcen, Diensten und Dokumenten suchen (G+/)** den Begriff **VNET-Gateway** ein. Suchen Sie in den Suchergebnissen nach **VNET-Gateway**, und wählen Sie diesen Eintrag aus.

   :::image type="content" source="./media/vpn-gateway-add-gw-portal/search.png" alt-text="Screenshot: Suchfeld" lightbox="./media/vpn-gateway-add-gw-portal/search-expand.png":::

1. Wählen Sie auf der Seite **Gateways für virtuelle Netzwerke** die Option **+ Erstellen** aus. Die Seite **Gateway für virtuelle Netzwerke erstellen** wird geöffnet.

   :::image type="content" source="./media/vpn-gateway-add-gw-portal/create.png" alt-text="Screenshot: Seite „Gateways für virtuelle Netzwerke“ mit hervorgehobener Option zum Erstellen":::
1. Geben Sie auf der Registerkarte **Grundlagen** die Werte für **Projektdetails** und **Details zur Instanz** ein.

   :::image type="content" source="./media/vpn-gateway-add-gw-portal/instance-details.png" alt-text="Screenshot: Instanzfelder":::

   * **Abonnement**: Wählen Sie in der Dropdownliste das zu verwendende Abonnement aus.
   * **Ressourcengruppe**: Diese Einstellung wird automatisch angegeben, wenn Sie auf dieser Seite Ihr virtuelles Netzwerk auswählen.
   * **Name**: Benennen Sie Ihr Gateway. Das Benennen eines Gateways ist nicht das Gleiche wie das Benennen eines Gatewaysubnetzes. Hierbei handelt es sich um den Namen des Gatewayobjekts, das Sie erstellen.
   * **Region**: Wählen Sie die Region aus, in der Sie diese Ressource erstellen möchten. Die Region für das Gateway muss der des virtuellen Netzwerks entsprechen.
   * **Gatewaytyp**: Wählen Sie **VPN** aus. Bei VPN-Gateways wird ein virtuelles Netzwerkgateway vom Typ **VPN** verwendet.
   * **VPN-Typ:** Wählen Sie den für Ihre Konfiguration angegebenen VPN-Typ aus. Bei den meisten Konfigurationen wird ein routenbasierter VPN-Typ benötigt.
   * **SKU**: Wählen Sie in der Dropdownliste die zu verwendende Gateway-SKU aus. Welche SKUs in der Dropdownliste aufgeführt werden, hängt vom ausgewählten VPN-Typ ab. Wählen Sie unbedingt eine SKU aus, die die von Ihnen gewünschten Features unterstützt. Weitere Informationen zu Gateway-SKUs finden Sie unter [Gateway-SKUs](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku).
   * **Generation**: Wählen Sie die zu verwendende Generation aus. Weitere Informationen finden Sie unter [Gateway-SKUs](../articles/vpn-gateway/vpn-gateway-about-vpngateways.md#gwsku).
   * **Virtuelles Netzwerk:** Wählen Sie in der Dropdownliste das virtuelle Netzwerk aus, dem Sie dieses Gateway hinzufügen möchten.
   * **Adressbereich für Gatewaysubnetz**: Dieses Feld wird nur angezeigt, wenn Ihr VNet nicht über ein Gatewaysubnetz verfügt. Es wird empfohlen, mindestens „/27“ (/26, /25 usw.) festzulegen. Dadurch stehen genügend IP-Adressen für zukünftige Änderungen zur Verfügung, etwa das Hinzufügen eines ExpressRoute-Gateways. Es empfiehlt sich nicht, einen Bereich zu erstellen, der kleiner als /28 ist. Wenn Sie bereits über ein Gatewaysubnetz verfügen, können Sie die Gatewaysubnetzdetails anzeigen, indem Sie zu Ihrem virtuellen Netzwerk navigieren. Klicken Sie auf **Subnetze**, um den Bereich anzuzeigen. Wenn Sie den Bereich ändern möchten, können Sie das GatewaySubnet löschen und neu erstellen.