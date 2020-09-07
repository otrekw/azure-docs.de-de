---
title: 'Szenario: Benutzerdefinierte Isolation für VNETs'
titleSuffix: Azure Virtual WAN
description: 'Routingszenarien: Verhindern, dass ausgewählte VNETs einander erreichen können'
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 08/03/2020
ms.author: cherylmc
ms.openlocfilehash: 0a3665f1719c7a5f8ed9bd6acf518b642e06320d
ms.sourcegitcommit: 5a3b9f35d47355d026ee39d398c614ca4dae51c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89400057"
---
# <a name="scenario-custom-isolation-for-vnets"></a>Szenario: Benutzerdefinierte Isolation für VNETs

Wenn Sie mit Virtual WAN-Routing für virtuelle Hubs arbeiten, stehen Ihnen eine ganze Reihe von Szenarien zur Verfügung. In einem Szenario mit benutzerdefinierter Isolation für VNETs besteht das Ziel darin, zu verhindern, dass bestimmte VNETs eine andere bestimmte Gruppe von VNETs erreichen können. Allerdings müssen die VNETs alle Zweige (VPN, ExpressRoute und Benutzer-VPN) erreichen können. Weitere Informationen zum Routing für virtuelle Hubs finden Sie unter [Informationen zum Routing virtueller Hubs](about-virtual-hub-routing.md).

## <a name="design"></a><a name="design"></a>Entwurf

Um herauszufinden, wie viele Routingtabellen erforderlich sind, können Sie eine Verbindungsmatrix erstellen. Für dieses Szenario sieht diese wie die folgende Tabelle aus, in der jede Zelle angibt, ob eine Quelle (Zeile) mit einem Ziel (Spalte) kommunizieren kann:

| From | Nach:| *Blaue VNets* | *Rote VNets* | *Branches*|
|---|---|---|---|---|
| **Blaue VNets** |   &#8594;|      X        |               |       X      |
| **Rote VNets**  |   &#8594;|              |       X       |       X      |
| **Branches**   |   &#8594;|     X        |       X       |       X      |

Jede Zelle in der vorstehenden Tabelle beschreibt, ob eine Virtual WAN-Verbindung (die Seite „From“ des Flows, die Zeilenheader in der Tabelle) ein Zielpräfix (die Seite „To“ des Flows, die kursiv formatierten Spaltenheader in der Tabelle) für einen bestimmten Datenverkehrsfluss lernt. Dabei bedeutet ein „X“, dass die Konnektivität durch eine Virtual WAN-Verbindung bereitgestellt wird.

Die Anzahl von verschiedenen Zeilenmustern entspricht der Anzahl von Routingtabellen, die in diesem Szenario benötigt werden. In diesem Fall werden drei Routingtabellen verwendet, wobei die für die virtuellen Netzwerke **RT_BLUE** und **RT_RED** genannt werden und die für die Zweigstellen **Standard**. Beachten Sie, dass die Zweigstellen immer der Routingtabelle „Standard“ zugeordnet sein müssen.

Die Zweigstellen müssen sowohl die Präfixe aus den roten als auch die aus den blauen VNets kennen, sodass alle VNets Daten an „Standard“ weitergeben müssen (zusätzlich zu **RT_BLUE** oder **RT_RED**). Die blauen und roten VNets müssen die Präfixe der Zweigstellen kennen, sodass die Zweigstellen ebenfalls Daten an die beiden Routingtabellen **RT_BLUE** und **RT_RED** weitergeben. Dies ist somit der endgültige Entwurf:

* Blaue virtuelle Netzwerke:
  * Zugeordnete Routingtabelle: **RT_BLUE**
  * Weitergabe an Routingtabellen: **RT_BLUE** und **Standard**
* Rote virtuelle Netzwerke:
  * Zugeordnete Routingtabelle: **RT_RED**
  * Weitergabe an Routingtabellen: **RT_RED** und **Standard**
* Branches:
  * Zugeordnete Routingtabelle: **Standard**
  * Weitergabe an Routingtabellen: **RT_BLUE**, **RT_RED** und **Standard**

> [!NOTE]
> Da alle Zweigstellen der Routingtabelle „Standard“ zugeordnet sein und Daten an dieselben Routingtabellen weitergeben müssen, verfügen sie alle über das gleiche Konnektivitätsprofil. Anders ausgedrückt: Das Konzept mit Rot und Blau für VNets kann nicht auf Zweigstellen angewendet werden.

> [!NOTE]
> Wenn Ihre Virtual WAN-Instanz regionsübergreifend bereitgestellt wird, müssen Sie die Routingtabellen **RT_BLUE** und **RT_RED** in jedem Hub erstellen, und die Routen jeder VNet-Verbindung müssen mithilfe von Weitergabebezeichnungen an die Routingtabellen in jedem virtuellen Hub weitergegeben werden.

Weitere Informationen zum Routing für virtuelle Hubs finden Sie unter [Informationen zum Routing virtueller Hubs](about-virtual-hub-routing.md).

## <a name="workflow"></a><a name="architecture"></a>Workflow

In **Abbildung 1** gibt es blaue und rote VNET-Verbindungen.

* Blaue VNETs können sich gegenseitig sowie alle Zweigverbindungen (VPN/ExpressRoute/P2S) erreichen.
* Rote VNETs können sich gegenseitig sowie alle Zweigverbindungen (VPN/ExpressRoute/P2S) erreichen.

Führen Sie zum Einrichten des Routings die folgenden Schritte aus.

1. Erstellen Sie im Azure-Portal die beiden benutzerdefinierten Routingtabellen **RT_BLUE** und **RT_RED**.
2. Legen Sie für die Routingtabelle **RT_BLUE** die folgenden Einstellungen fest:
   * **Zuordnung:** Wählen Sie alle blauen VNets aus.
   * **Weitergabe**: Wählen Sie für Zweigstellen die Option für Zweigstellen aus, was impliziert, dass Zweigstellenverbindungen (VPN/ER/P2S) Routen an diese Routingtabelle weitergeben.
3. Wiederholen Sie diese Schritte für die Routingtabelle **RT_RED** für rote VNets und Zweigstellen (VPN/ER/P2S).

Dies führt zu Änderungen der Routingkonfiguration, wie in der folgenden Abbildung dargestellt.

**Abbildung 1**

:::image type="content" source="./media/routing-scenarios/custom-isolation/custom.png" alt-text="Abbildung 1":::

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu Virtual WAN finden Sie unter [Häufig gestellte Fragen](virtual-wan-faq.md).
* Weitere Informationen zum Routing für virtuelle Hubs finden Sie unter [Informationen zum Routing virtueller Hubs](about-virtual-hub-routing.md).
