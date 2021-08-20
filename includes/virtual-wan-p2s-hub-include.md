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
ms.openlocfilehash: a1abf76ee8e6c44eaa61d9de38b163f230327edd
ms.sourcegitcommit: 47ac63339ca645096bd3a1ac96b5192852fc7fb7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/16/2021
ms.locfileid: "114362308"
---
1. Wählen Sie auf der Seite für Ihr virtual WAN im linken Bereich **Hubs** aus. Wählen Sie auf der Seite **Hubs** die Option **+Neuer Hub** aus.

   :::image type="content" source="media/virtual-wan-p2s-hub/new-hub.png" alt-text="Screenshot des neuen Hubs":::

1. Füllen Sie auf der Seite **Virtuellen Hub erstellen** die folgenden Felder aus:

   * **Region**: Wählen Sie die Region aus, in der Sie den virtuellen Hub bereitstellen möchten.
   * **Name**: Geben Sie den Namen ein, den Sie Ihrem virtuellen Hub zuweisen möchten.
   * **Privater Adressraum des Hubs**: Der Adressbereich des Hubs in CIDR-Notation.

   :::image type="content" source="media/virtual-wan-p2s-hub/create-hub.png" alt-text="Screenshot: Erstellen eines virtuellen Hubs":::

1. Füllen Sie auf der Registerkarte **Point-to-Site** die folgenden Felder aus:

   * **Gatewayskalierungseinheiten**: Steht für die Aggregatkapazität des Benutzer-VPN-Gateways.
   * **Point-to-Site-Konfiguration**: Diese Konfiguration haben Sie im vorherigen Schritt erstellt.
   * **Clientadresspool**: Für die Remotebenutzer bestimmt.
   * **IP-Adresse des benutzerdefinierten DNS-Servers**.
   * **Routingpräferenz**: Wählen Sie die entsprechende Routingpräferenz aus. Die Azure-Routingpräferenz ermöglicht es Ihnen zu wählen, wie Ihr Datenverkehr zwischen Azure und dem Internet geleitet wird. Sie können auswählen, ob der Datenverkehr entweder über das Microsoft-Netzwerk oder über das ISP-Netzwerk (öffentliches Internet) geleitet werden soll. Diese Optionen werden auch als Cold Potato-Routing bzw. Hot Potato-Routing bezeichnet. Die öffentliche IP-Adresse in Virtual WAN wird vom Dienst basierend auf der ausgewählten Routingoption zugewiesen. Weitere Informationen zur Routingpräferenz über das Microsoft-Netzwerk oder ISP finden Sie im Artikel [Routingpräferenz](../articles/virtual-network/routing-preference-overview.md).

   :::image type="content" source="media/virtual-wan-p2s-hub/create-point-to-site.png" alt-text="Screenshot der Konfiguration des virtuellen Hubs mit ausgewählter Point-to-Site-Konfiguration":::

1. Klicken Sie auf **Überprüfen + erstellen**.
1. Wählen Sie auf der Seite **Überprüfung erfolgreich** die Option **Erstellen** aus.