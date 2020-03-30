---
title: include file
description: include file
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 08/02/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 9c18a2c74d03a636a0865f3008eb421ab8d7412d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "68781452"
---
1. Klicken Sie im Portal auf **Ressource erstellen**.
2. Geben Sie im Suchfeld den Text **Lokales Netzwerkgateway** ein, und drücken Sie die **EINGABETASTE**. Daraufhin wird eine Liste mit Ergebnissen zurückgegeben. Klicken Sie auf **Lokales Netzwerkgateway** und anschließend auf die Schaltfläche **Erstellen**, um die Seite **Lokales Netzwerkgateway erstellen** zu öffnen.

   ![Erstellen des Gateways des lokalen Netzwerks](./media/vpn-gateway-add-lng-rm-portal-include/local-network-gateway.png "Erstellen des Gateways des lokalen Netzwerks")

3. Geben Sie auf der Seite **Lokales Netzwerkgateway erstellen** die Werte für das lokale Netzwerkgateway ein.

   - **Name:** Geben Sie einen Namen für das lokale Netzwerkgatewayobjekt ein.
   - **IP-Adresse:** Dies ist die öffentliche IP-Adresse des VPN-Geräts, mit dem Azure eine Verbindung herstellen soll. Geben Sie eine gültige öffentliche IP-Adresse ein. Falls Sie die IP-Adresse gerade nicht zur Hand haben, können Sie die Werte aus dem Beispiel verwenden. In diesem Fall müssen Sie allerdings später die Platzhalter-IP-Adresse durch die öffentliche IP-Adresse Ihres VPN-Geräts ersetzen. Andernfalls kann Azure keine Verbindung herstellen.
   - **Adressraum** bezieht sich auf die Adressbereiche für das Netzwerk, das dieses lokale Netzwerk darstellt. Sie können mehrere Adressraumbereiche hinzufügen. Achten Sie darauf, dass sich die hier angegebenen Bereiche nicht mit den Bereichen anderer Netzwerke überschneiden, mit denen Sie eine Verbindung herstellen möchten. Azure leitet den Adressbereich, den Sie angeben, an die lokale IP-Adresse des VPN-Geräts weiter. *Verwenden Sie hier anstelle der Werte aus dem Beispiel Ihre eigenen Werte, wenn Sie eine Verbindung mit Ihrem lokalen Standort herstellen möchten.*
   - **BGP-Einstellungen konfigurieren:** Aktivieren Sie diese Option nur, wenn Sie BGP konfigurieren. Lassen Sie sie andernfalls deaktiviert.
   - **Abonnement:** Stellen Sie sicher, dass das richtige Abonnement angezeigt wird.
   - **Ressourcengruppe:** Wählen Sie die Ressourcengruppe aus, die Sie verwenden möchten. Sie können entweder eine neue Ressourcengruppe erstellen oder eine auswählen, die Sie bereits erstellt haben.
   - **Speicherort:** Wählen Sie den Speicherort aus, an dem dieses Objekt erstellt wird. Es empfiehlt sich unter Umständen, den gleichen Ort auszuwählen, an dem sich auch Ihr VNet befindet, dies ist aber nicht zwingend erforderlich.

4. Wenn Sie die Werte angegeben haben, klicken Sie auf die Schaltfläche **Erstellen**, um das Gateway zu erstellen.
