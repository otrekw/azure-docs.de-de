---
title: Datei einfügen
description: include file
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 05/26/2021
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: ab341181db71a8df5dde27311e9169f9477c70f8
ms.sourcegitcommit: e1d5abd7b8ded7ff649a7e9a2c1a7b70fdc72440
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2021
ms.locfileid: "110578247"
---
1. Suchen Sie das virtuelle WAN, das Sie erstellt haben. Wählen Sie auf der Seite „Virtual WAN“ im Abschnitt **Konnektivität** die Option **Hubs** aus.
2. Wählen Sie auf der Seite **Hubs** die Option **+ Neuer Hub** aus, um die Seite **Virtuellen Hub erstellen** zu öffnen.

   :::image type="content" source="./media/virtual-wan-tutorial-hub-include/basics.png" alt-text="Screenshot: Bereich „Virtuellen Hub erstellen“ mit ausgewählter Registerkarte „Grundlagen“" border="false":::
3. Füllen Sie auf der Seite **Virtuellen Hub erstellen** auf der Registerkarte **Grundlagen** die folgenden Felder aus:

   * **Region** (zuvor als „Standort“ bezeichnet)
   * **Name**
   * **Privater Adressraum des Hubs**: Der minimale Adressraum ist „/24“ zum Erstellen eines Hubs. Wenn Sie eine Adresse im Bereich von „/25“ bis „/32“ verwenden, tritt bei der Erstellung ein Fehler auf. Sie müssen den Adressraum des Subnetzes für die Dienste im virtuellen Hub nicht explizit planen. Azure Virtual WAN ist ein verwalteter Dienst und erstellt daher die geeigneten Subnetze im virtuellen Hub für die verschiedenen Gateways/Dienste (z. B. VPN-Gateways, ExpressRoute-Gateways, Benutzer-VPN-Point-to-Site-Gateways, Firewall, Routing usw.).
4. Klicken Sie auf **Weiter: Site-to-Site**.

   :::image type="content" source="./media/virtual-wan-tutorial-hub-include/site-to-site.png" alt-text="Screenshot: Bereich „Virtuellen Hub erstellen“ mit ausgewählter Option „Site-to-Site“" border="false":::

5. Füllen Sie auf der Registerkarte **Site-to-Site** die folgenden Felder aus:

   * Wählen Sie **Ja** zum Erstellen eines Site-to-Site-VPN aus.
   * Das Feld „AS-Nummer“ kann nicht bearbeitet werden.
   * Wählen Sie den Wert **Gatewayskalierungseinheiten** in der Dropdownliste aus. Mit der Skalierungseinheit können Sie den aggregierten Durchsatz des VPN-Gateways auswählen, das im virtuellen Hub erstellt wird, um eine Verbindung zwischen Sites herzustellen. Wenn Sie 1 Skalierungseinheit = 500 Mbit/s auswählen, bedeutet dies, dass zwei Instanzen für Redundanz erstellt werden, von denen jede einen maximalen Durchsatz von 500 Mbit/s hat. Wenn Sie z. B. fünf Verzweigungen mit jeweils 10 Mbit/s an der Verzweigung hatten, benötigen Sie ein Aggregat von 50 Mbit/s am Headend. Die Planung der Aggregatkapazität des Azure-VPN-Gateways sollte nach der Bewertung der Kapazität erfolgen, die zur Unterstützung der Anzahl von Verzweigungen zum Hub benötigt wird.
6. Wählen Sie zum Überprüfen **Überprüfen + erstellen** aus.
7. Wählen Sie **Erstellen** aus, um den Hub zu erstellen. Klicken Sie nach 30 Minuten auf **Aktualisieren**, um den Hub auf der Seite **Hubs** anzuzeigen. Wählen Sie **Zu Ressource wechseln** aus, um zur Ressource zu navigieren.
