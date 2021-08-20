---
title: Datei einfügen
description: Datei einfügen
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 07/15/2021
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 009b82188c52df6728aeef6c414c5f9ef0507ccc
ms.sourcegitcommit: 47ac63339ca645096bd3a1ac96b5192852fc7fb7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/16/2021
ms.locfileid: "114362285"
---
1. Klicken Sie auf der Seite **Virtuellen Hub erstellen** auf **Site to site**, um die Registerkarte **Site to site** (Standort zu Standort) zu öffnen.

   :::image type="content" source="./media/virtual-wan-tutorial-hub-include/site-to-site.png" alt-text="Screenshot: Bereich „Virtuellen Hub erstellen“ mit ausgewählter Option „Site-to-Site“":::

1. Füllen Sie auf der Registerkarte **Site to site** die folgenden Felder aus:

   * Wählen Sie **Ja** zum Erstellen eines Site-to-Site-VPN aus.
   * Das Feld „AS-Nummer“ kann nicht bearbeitet werden.
   * Wählen Sie den Wert **Gatewayskalierungseinheiten** in der Dropdownliste aus. Mit der Skalierungseinheit können Sie den aggregierten Durchsatz des VPN-Gateways auswählen, das im virtuellen Hub erstellt wird, um eine Verbindung zwischen Sites herzustellen. Wenn Sie 1 Skalierungseinheit = 500 Mbit/s auswählen, bedeutet dies, dass zwei Instanzen für Redundanz erstellt werden, von denen jede einen maximalen Durchsatz von 500 Mbit/s hat. Wenn Sie z. B. fünf Verzweigungen mit jeweils 10 Mbit/s an der Verzweigung hatten, benötigen Sie ein Aggregat von 50 Mbit/s am Headend. Die Planung der Aggregatkapazität des Azure-VPN-Gateways sollte nach der Bewertung der Kapazität erfolgen, die zur Unterstützung der Anzahl von Verzweigungen zum Hub benötigt wird.
   * Wählen Sie die entsprechende **Routingpräferenz** aus. Die Azure-Routingpräferenz ermöglicht es Ihnen zu wählen, wie Ihr Datenverkehr zwischen Azure und dem Internet geleitet wird. Sie können auswählen, ob der Datenverkehr entweder über das Microsoft-Netzwerk oder über das ISP-Netzwerk (öffentliches Internet) geleitet werden soll. Diese Optionen werden auch als Cold Potato-Routing bzw. Hot Potato-Routing bezeichnet. Die öffentliche IP-Adresse in Virtual WAN wird vom Dienst basierend auf der ausgewählten Routingoption zugewiesen. Weitere Informationen zur Routingpräferenz über das Microsoft-Netzwerk oder ISP finden Sie im Artikel [Routingpräferenz](../articles/virtual-network/routing-preference-overview.md).
1. Wählen Sie zum Überprüfen **Überprüfen + erstellen** aus.
1. Wählen Sie **Erstellen** aus, um den Hub zu erstellen. Die Erstellung eines VPN-Gateways kann bis zu 30 Minuten dauern. Klicken Sie nach 30 Minuten auf **Aktualisieren**, um den Hub auf der Seite **Hubs** anzuzeigen. Wählen Sie **Zu Ressource wechseln** aus, um zur Ressource zu navigieren.