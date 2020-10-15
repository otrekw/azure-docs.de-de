---
title: 'Szenario: Any-to-Any'
titleSuffix: Azure Virtual WAN
description: 'Szenarien für das Routing: Any-to-Any'
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 09/22/2020
ms.author: cherylmc
ms.custom: fasttrack-edit
ms.openlocfilehash: 6115ca375c3e5bf2be3335fe2231628ec7bf309f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91267736"
---
# <a name="scenario-any-to-any"></a>Szenario: Any-to-Any

Wenn Sie mit Virtual WAN-Routing für virtuelle Hubs arbeiten, stehen Ihnen eine ganze Reihe von Szenarien zur Verfügung. In einem Any-to-Any-Szenario kann jeder Spoke einen anderen Spoke erreichen. Wenn mehrere Hubs vorhanden sind, ist das Hub-zu-Hub-Routing (auch als Routing zwischen Hubs bezeichnet) im virtuellen Standard-WAN standardmäßig aktiviert. Weitere Informationen zum Routing für virtuelle Hubs finden Sie unter [Informationen zum Routing virtueller Hubs](about-virtual-hub-routing.md).

## <a name="design"></a><a name="design"></a>Entwurf

Um herauszufinden, wie viele Routingtabellen in einem Virtual WAN-Szenario erforderlich sind, können Sie eine Verbindungsmatrix erstellen, in der jede Zelle angibt, ob eine Quelle (Zeile) mit einem Ziel (Spalte) kommunizieren kann. Die Verbindungsmatrix in diesem Szenario ist trivial, aber wir haben sie eingeschlossen, um mit anderen Szenarien konsistent zu sein.

| From |   To |  *VNETs* | *Branches* |
| -------------- | -------- | ---------- | ---|
| VNETs     | &#8594;|      X     |     X    |
| Branches   | &#8594;|    X     |     X    |

Jede Zelle in der vorstehenden Tabelle beschreibt, ob eine Virtual WAN-Verbindung (die Seite „From“ des Flows, die Zeilenheader in der Tabelle) ein Zielpräfix (die Seite „To“ des Flows, die kursiv formatierten Spaltenheader in der Tabelle) für einen bestimmten Datenverkehrsfluss lernt. Dabei bedeutet ein „X“, dass die Konnektivität durch eine Virtual WAN-Verbindung bereitgestellt wird.

Da für alle Verbindungen von VNETs und Branches (VPN, ExpressRoute und Benutzer-VPN) die gleichen Konnektivitätsanforderungen gelten, ist eine einzelne Routingtabelle erforderlich. Dadurch werden alle Verbindungen verknüpft und an dieselbe Routingtabelle weitergegeben, nämlich die Standardroutingtabelle:

* Virtuelle Netzwerke:
  * Zugeordnete Routingtabelle: **Standard**
  * Weitergabe an Routingtabellen: **Standard**
* Branches:
  * Zugeordnete Routingtabelle: **Standard**
  * Weitergabe an Routingtabellen: **Standard**

Weitere Informationen zum Routing für virtuelle Hubs finden Sie unter [Informationen zum Routing virtueller Hubs](about-virtual-hub-routing.md).

## <a name="architecture"></a><a name="architecture"></a>Architektur

In **Abbildung 1** können alle VNETs und Branches (VPN, ExpressRoute, P2S) einander erreichen. In einem virtuellen Hub funktionieren die Verbindungen wie folgt:

* Eine VPN-Verbindung verbindet einen VPN-Standort mit einem VPN-Gateway.
* Eine VNET-Verbindung verbindet ein virtuelles Netzwerk mit einem virtuellen Hub. Der Router des virtuellen Hubs bietet die Transitfunktionalität zwischen VNETs.
* Eine ExpressRoute-Verbindung verbindet eine ExpressRoute-Leitung mit einem ExpressRoute-Gateway.

Diese Verbindungen werden (standardmäßig bei Erstellung) der Standardroutingtabelle zugeordnet, sofern Sie die Routingkonfiguration der Verbindung nicht als **Keine** oder eine benutzerdefinierte Routingtabelle einrichten. Diese Verbindungen übergeben Routen standardmäßig an die Standardroutingtabelle. Dadurch wird ein Any-to-Any-Szenario ermöglicht, in dem alle Spokes (VNET, VPN, ER, P2S) einander erreichen können.

**Abbildung 1**

:::image type="content" source="./media/routing-scenarios/any-any/figure-1.png" alt-text="Abbildung 1":::

## <a name="workflow"></a><a name="workflow"></a>Workflow

Dieses Szenario ist für das virtuelle Standard-WAN standardmäßig aktiviert. Wenn die Einstellung für Branch-to-Branch in der WAN-Konfiguration deaktiviert ist, sind keine Verbindungen zwischen Branchspokes möglich. VPN/ExpressRoute/Benutzer-VPN werden in Virtual WAN als Branchspokes angesehen.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu Virtual WAN finden Sie unter [Häufig gestellte Fragen](virtual-wan-faq.md).
* Weitere Informationen zum Routing für virtuelle Hubs finden Sie unter [Informationen zum Routing virtueller Hubs](about-virtual-hub-routing.md).
