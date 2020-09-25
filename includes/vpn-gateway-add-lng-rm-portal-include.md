---
title: include file
description: include file
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 09/17/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: f708052e6c1b474d1447a352d10eb1defe311fc4
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "91025004"
---
1. Klicken Sie im Portal auf **Ressource erstellen**.
2. Geben Sie im Suchfeld den Text **Lokales Netzwerkgateway** ein, und drücken Sie die **EINGABETASTE**. Daraufhin wird eine Liste mit Ergebnissen zurückgegeben. Klicken Sie auf **Lokales Netzwerkgateway** und anschließend auf die Schaltfläche **Erstellen**, um die Seite **Lokales Netzwerkgateway erstellen** zu öffnen.

   :::image type="content" source="./media/vpn-gateway-add-lng-rm-portal-include/create-local-gateway-ip.png" alt-text="Erstellen des Gateways des lokalen Netzwerks":::

3. Geben Sie auf der Seite **Lokales Netzwerkgateway erstellen** die Werte für das lokale Netzwerkgateway ein.

   - **Name:** Geben Sie einen Namen für das lokale Netzwerkgatewayobjekt ein.
   - **Endpunkt:** Wählen Sie den Endpunkttyp für das lokale VPN-Gerät aus: **IP-Adresse** oder **FQDN (vollqualifizierter Domänen Name)** .
      - **IP-Adresse**: Wenn Sie über eine statische öffentliche IP-Adresse verfügen, die Ihrem VPN-Gerät von Ihrem Internetdienstanbieter zugeordnet wurde, wählen Sie die Option „IP-Adresse“ aus, und geben Sie die IP-Adresse wie im Beispiel gezeigt ein. Dies ist die öffentliche IP-Adresse des VPN-Geräts, mit dem Azure VPN Gateway eine Verbindung herstellen soll. Falls Sie die IP-Adresse gerade nicht zur Hand haben, können Sie die Werte aus dem Beispiel verwenden. In diesem Fall müssen Sie allerdings später die Platzhalter-IP-Adresse durch die öffentliche IP-Adresse Ihres VPN-Geräts ersetzen. Andernfalls kann Azure keine Verbindung herstellen.
      - **FQDN**: Wenn Sie über eine dynamische öffentliche IP-Adresse verfügen, die sich nach einem bestimmten Zeitraum ändern könnte, der normalerweise von Ihrem Internetdienstanbieter festgelegt wird, können Sie einen konstanten DNS-Namen mit einem dynamischen DNS-Dienst verwenden, um auf die aktuelle öffentliche IP-Adresse Ihres VPN-Geräts zu verweisen. Ihr Azure VPN Gateway löst den FQDN dann auf, um die öffentliche IP-Adresse für die Verbindungsherstellung zu ermitteln. Der folgende Screenshot zeigt ein Beispiel für die Verwendung eines FQDN anstelle der IP-Adresse.
   - **Adressraum** bezieht sich auf die Adressbereiche für das Netzwerk, das dieses lokale Netzwerk darstellt. Sie können mehrere Adressraumbereiche hinzufügen. Achten Sie darauf, dass sich die hier angegebenen Bereiche nicht mit den Bereichen anderer Netzwerke überschneiden, mit denen Sie eine Verbindung herstellen möchten. Azure leitet den Adressbereich, den Sie angeben, an die lokale IP-Adresse des VPN-Geräts weiter. *Verwenden Sie hier anstelle der Werte aus dem Beispiel Ihre eigenen Werte, wenn Sie eine Verbindung mit Ihrem lokalen Standort herstellen möchten.*
   - **BGP-Einstellungen konfigurieren:** Nur beim Konfigurieren von BGP verwenden. Lassen Sie sie andernfalls deaktiviert.
   - **Abonnement:** Vergewissern Sie sich, dass das richtige Abonnement angezeigt wird.
   - **Ressourcengruppe:** Wählen Sie die Ressourcengruppe aus, die Sie verwenden möchten. Sie können entweder eine neue Ressourcengruppe erstellen oder eine auswählen, die Sie bereits erstellt haben.
   - **Standort:** Wählen Sie den Standort aus, an dem dieses Objekt erstellt wird. Es empfiehlt sich unter Umständen, den gleichen Ort auszuwählen, an dem sich auch Ihr VNet befindet, dies ist aber nicht zwingend erforderlich.

    Dies ist dieselbe Seite, nur mit hervorgehobenem FQDN:
   
   :::image type="content" source="./media/vpn-gateway-add-lng-rm-portal-include/create-local-gateway-fqdn.png" alt-text="Erstellen des lokalen Netzwerkgateways: FQDN":::
   
   > [!NOTE]
   >
   > * Azure VPN unterstützt nur eine IPv4-Adresse für jeden FQDN. Wenn der Domänenname zu mehreren IP-Adressen aufgelöst wird, verwendet Azure VPN Gateway die erste von den DNS-Servern zurückgegebene IP-Adresse. Um die Ungewissheit zu beseitigen, wird empfohlen, dass Ihr FQDN immer zu einer einzelnen IPv4-Adresse aufgelöst wird. IPv6 wird nicht unterstützt.
   > * Azure VPN Gateway verwaltet einen DNS-Cache, der alle 5 Minuten aktualisiert wird. Das Gateway versucht, die vollqualifizierten Domänennamen nur für getrennte Tunnel aufzulösen. Durch das Zurücksetzen des Gateways wird auch die Auflösung eines FQDN ausgelöst.
   >

4. Wenn Sie die Werte angegeben haben, wählen Sie die Schaltfläche **Erstellen**, um das Gateway zu erstellen.
