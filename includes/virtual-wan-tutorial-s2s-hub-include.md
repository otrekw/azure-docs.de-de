---
title: Datei einfügen
description: include file
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 06/23/2021
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: aacd6a780562476ea338b4c08795e14b6ba39760
ms.sourcegitcommit: 54d8b979b7de84aa979327bdf251daf9a3b72964
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/24/2021
ms.locfileid: "112579165"
---
1. Suchen Sie das virtuelle WAN, das Sie erstellt haben. Wählen Sie auf der Seite „Virtual WAN“ im Abschnitt **Konnektivität** die Option **Hubs** aus.
1. Wählen Sie auf der Seite **Hubs** die Option **+ Neuer Hub** aus, um die Seite **Virtuellen Hub erstellen** zu öffnen.

   :::image type="content" source="./media/virtual-wan-tutorial-hub-include/basics.png" alt-text="Screenshot: Bereich „Virtuellen Hub erstellen“ mit ausgewählter Registerkarte „Grundlagen“" border="false":::
1. Füllen Sie auf der Seite **Virtuellen Hub erstellen** auf der Registerkarte **Grundlagen** die folgenden Felder aus:

   * **Region** (zuvor als „Standort“ bezeichnet)
   * **Name**
   * **Privater Adressraum des Hubs**: Der minimale Adressraum ist „/24“ zum Erstellen eines Hubs. Wenn Sie eine Adresse im Bereich von „/25“ bis „/32“ verwenden, tritt bei der Erstellung ein Fehler auf. Sie müssen den Adressraum des Subnetzes für die Dienste im virtuellen Hub nicht explizit planen. Azure Virtual WAN ist ein verwalteter Dienst und erstellt daher die geeigneten Subnetze im virtuellen Hub für die verschiedenen Gateways/Dienste (z. B. VPN-Gateways, ExpressRoute-Gateways, Benutzer-VPN-Point-to-Site-Gateways, Firewall, Routing usw.).
