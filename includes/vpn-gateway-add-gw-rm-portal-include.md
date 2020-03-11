---
title: include file
description: include file
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/04/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 9606221184adf5f787485c1d47b9d6c4ed13429b
ms.sourcegitcommit: f915d8b43a3cefe532062ca7d7dbbf569d2583d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2020
ms.locfileid: "78301942"
---
1. Wählen Sie im [Azure-Portal](https://portal.azure.com)-Menü die Option **Ressource erstellen** aus. 

   ![Erstellen einer Ressource im Azure-Portal](./media/vpn-gateway-add-gw-rm-portal-include/azure-portal-create-resource.png)
2. Geben Sie im Feld **Marketplace durchsuchen** den Text „Gateway für virtuelle Netzwerke“ ein. Suchen Sie in der Ausgabe nach **Gateway für virtuelle Netzwerke**, und wählen Sie diesen Eintrag aus. Wählen Sie auf der Seite **Gateway für virtuelle Netzwerke** die Option **Erstellen** aus. Die Seite **Gateway für virtuelle Netzwerke erstellen** wird geöffnet.

   ![Felder der Seite „Gateway für virtuelle Netzwerke erstellen“](./media/vpn-gateway-add-gw-rm-portal-include/gateway1.png "Felder der Seite „Gateway für virtuelle Netzwerke erstellen“")

   ![Felder der Seite „Gateway für virtuelle Netzwerke erstellen“](./media/vpn-gateway-add-gw-rm-portal-include/gateway2.png "Felder der Seite „Gateway für virtuelle Netzwerke erstellen“")
3. Geben Sie auf der Registerkarte **Grundlagen** die Werte für Ihr Gateway für virtuelle Netzwerke an.

   **Projektdetails**

   - **Abonnement**: Wählen Sie in der Dropdownliste das zu verwendende Abonnement aus.
   - **Ressourcengruppe**: Diese Einstellung wird automatisch angegeben, wenn Sie auf dieser Seite Ihr virtuelles Netzwerk auswählen.

   **Instanzendetails**

   - **Name**: Benennen Sie Ihr Gateway. Das Benennen eines Gateways ist nicht das Gleiche wie das Benennen eines Gatewaysubnetzes. Hierbei handelt es sich um den Namen des Gatewayobjekts, das Sie erstellen.
   - **Region**: Wählen Sie die Region aus, in der Sie diese Ressource erstellen möchten. Die Region für das Gateway muss der des virtuellen Netzwerks entsprechen.
   - **Gatewaytyp**: Wählen Sie **VPN** aus. Bei VPN-Gateways wird ein virtuelles Netzwerkgateway vom Typ **VPN** verwendet.
   - **VPN-Typ:** Wählen Sie den für Ihre Konfiguration angegebenen VPN-Typ aus. Bei den meisten Konfigurationen wird ein routenbasierter VPN-Typ benötigt.
   - **SKU**: Wählen Sie in der Dropdownliste die Gateway-SKU aus. Welche SKUs in der Dropdownliste aufgeführt werden, hängt vom ausgewählten VPN-Typ ab. Weitere Informationen zu Gateway-SKUs finden Sie unter [Gateway-SKUs](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku).
   - **Generation:** Informationen zur Generierung von VPN-Gateways finden Sie unter [Gateway-SKUs](../articles/vpn-gateway/vpn-gateway-about-vpngateways.md#gwsku).
   - **Virtuelles Netzwerk:** Wählen Sie in der Dropdownliste das virtuelle Netzwerk aus, dem Sie dieses Gateway hinzufügen möchten.
   - **Adressbereich für Gatewaysubnetz**: Dieses Feld wird nur angezeigt, wenn Ihr VNet nicht über ein Gatewaysubnetz verfügt. Legen Sie den Bereich möglichst als /27 oder größer (/26, /25 usw.) fest. Es empfiehlt sich nicht, einen Bereich zu erstellen, der kleiner als /28 ist. Wenn Sie bereits über ein Gatewaysubnetz verfügen, können Sie die Gatewaysubnetzdetails anzeigen, indem Sie zu Ihrem virtuellen Netzwerk navigieren. Klicken Sie auf **Subnetze**, um den Bereich anzuzeigen. Wenn Sie den Bereich ändern möchten, können Sie das GatewaySubnet löschen und neu erstellen.

   **Öffentliche IP-Adresse**
   
   Mit diesen Einstellungen wird das Objekt für die öffentliche IP-Adresse angegeben, das dem VPN-Gateway zugeordnet wird. Die öffentliche IP-Adresse wird bei der Erstellung des VPN-Gateways diesem Objekt dynamisch zugewiesen. Die öffentliche IP-Adresse ändert sich nur, wenn das Gateway gelöscht und neu erstellt wird. Sie ändert sich nicht, wenn die Größe geändert wird, das VPN-Gateway zurückgesetzt wird oder andere interne Wartungs-/Upgradevorgänge für das VPN-Gateway durchgeführt werden.

     - **Öffentliche IP-Adresse:** Lassen Sie **Neu erstellen** aktiviert.
     - **Öffentliche IP-Adresse**: Geben Sie im Textfeld einen Namen für die öffentliche IP-Adressinstanz ein.
     - **Zuweisung**: VPN Gateway unterstützt nur die dynamische Zuweisung.
   
   Zusätzliche Einstellungen

     - **Aktiv/Aktiv-Modus:** Aktivieren Sie **Aktiv/Aktiv-Modus aktivieren** nur, wenn Sie eine Aktiv/Aktiv-Gatewaykonfiguration erstellen. Lassen Sie diese Einstellung andernfalls deaktiviert.

     - Aktivieren Sie **BGP-ASN konfigurieren** nur, wenn diese Einstellung für Ihre Konfiguration speziell erforderlich ist. Sollte diese Einstellung erforderlich sein, lautet die Standard-ASN 65515. (Dieser Wert kann jedoch geändert werden.)

4. Wählen Sie zum Ausführen der Validierung **Bewerten + erstellen** aus. Wählen Sie nach Abschluss der Validierung **Erstellen** aus, um das VPN-Gateway bereitzustellen. Die gesamte Erstellung und Bereitstellung eines Gateways kann bis zu 45 Minuten dauern. Der Bereitstellungsstatus wird auf der Übersichtsseite für Ihr Gateway angezeigt.

Nach der Erstellung des Gateways können Sie die zugewiesene IP-Adresse unter dem Virtual Network im Portal anzeigen. Das Gateway wird als verbundenes Gerät angezeigt.
