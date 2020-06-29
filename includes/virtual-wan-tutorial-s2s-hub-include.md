---
title: include file
description: include file
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 11/04/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 105ab0c71d9e7e935842550ecdc4c8d2ff2a2d8c
ms.sourcegitcommit: 9bfd94307c21d5a0c08fe675b566b1f67d0c642d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/17/2020
ms.locfileid: "84977910"
---
1. Suchen Sie das virtuelle WAN, das Sie erstellt haben. Wählen Sie auf der Seite „Virtual WAN“ im Abschnitt **Konnektivität** die Option **Hubs** aus.
2. Wählen Sie auf der Seite „Hubs“ **+ Neuer Hub** aus, um die Seite **Virtuellen Hub erstellen** zu öffnen.

    ![Grundlagen](./media/virtual-wan-tutorial-hub-include/basics.png "Grundlagen")
3. Füllen Sie auf der Seite **Virtuellen Hub erstellen** auf der Registerkarte **Grundlagen** die folgenden Felder aus:

    **Projektdetails**

   * Region (zuvor als „Standort“ bezeichnet)
   * Name
   * Privater Adressraum des Hubs. Der minimale Adressraum ist „/24“ zum Erstellen eines Hubs. Dies beinhaltet, dass ein Bereich von „/25“ bis „/32“ bei der Erstellung einen Fehler erzeugt. Azure Virtual WAN ist ein verwalteter Dienst von Microsoft und erstellt die geeigneten Subnetze im virtuellen Hub für die verschiedenen Gateways/Dienste (z. B. VPN-Gateways, ExpressRoute-Gateways, Benutzer-VPN/Point-to-Site-Gateways, Firewall, Routing usw.). Dabei muss der Benutzer den Subnetzadressraum für die Dienste im virtuellen Hub nicht explizit planen, weil Microsoft dies im Rahmen des Diensts übernimmt.
4. Klicken Sie auf **Weiter: Site-to-Site**.

    ![Site-to-Site](./media/virtual-wan-tutorial-hub-include/site-to-site.png "Site-to-Site")

5. Füllen Sie auf der Registerkarte **Site-to-Site** die folgenden Felder aus:

   * Wählen Sie **Ja** zum Erstellen eines Site-to-Site-VPN aus.
   * Das Feld „AS-Nummer“ kann zurzeit nicht auf dem virtuellen Hub bearbeitet werden.
   * Wählen Sie den Wert **Gatewayskalierungseinheiten** in der Dropdownliste aus. Mit der Skalierungseinheit können Sie den aggregierten Durchsatz des VPN-Gateways auswählen, das im virtuellen Hub erstellt wird, um eine Verbindung zwischen Sites herzustellen. Wenn Sie 1 Skalierungseinheit = 500 Mbit/s auswählen, bedeutet dies, dass zwei Instanzen für Redundanz erstellt werden, von denen jede einen maximalen Durchsatz von 500 Mbit/s hat. Wenn Sie z. B. fünf Verzweigungen mit jeweils 10 Mbit/s an der Verzweigung hatten, benötigen Sie ein Aggregat von 50 Mbit/s am Headend. Die Planung der Aggregatkapazität des Azure-VPN-Gateways sollte nach der Bewertung der Kapazität erfolgen, die zur Unterstützung der Anzahl von Verzweigungen zum Hub benötigt wird.
6. Wählen Sie zum Überprüfen **Überprüfen + erstellen** aus.
7. Wählen Sie **Erstellen** aus, um den Hub zu erstellen. Klicken Sie nach 30 Minuten auf **Aktualisieren**, um den Hub auf der Seite **Hubs** anzuzeigen. Wählen Sie **Zu Ressource wechseln** aus, um zur Ressource zu navigieren.
