---
title: 'Szenario: Isolieren von virtuellen Netzwerken (VNETs)'
titleSuffix: Azure Virtual WAN
description: 'Szenarien für Routing: Isolieren von virtuellen Netzwerken (VNETs)'
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 08/03/2020
ms.author: cherylmc
ms.custom: fasttrack-edit
ms.openlocfilehash: b8a0a8093ab5f4d6c5e528bce592d5c029de30a7
ms.sourcegitcommit: 5a3b9f35d47355d026ee39d398c614ca4dae51c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89400040"
---
# <a name="scenario-isolating-vnets"></a>Szenario: Isolieren von virtuellen Netzwerken (VNETs)

Wenn Sie mit Virtual WAN-Routing für virtuelle Hubs arbeiten, stehen Ihnen eine ganze Reihe von Szenarien zur Verfügung. In diesem Szenario besteht das Ziel darin zu verhindern, dass VNETs andere VNETs erreichen können. Dies wird als Isolieren von VNETs bezeichnet. Weitere Informationen zum Routing für virtuelle Hubs finden Sie unter [Informationen zum Routing virtueller Hubs](about-virtual-hub-routing.md).

## <a name="design"></a><a name="design"></a>Entwurf

In diesem Szenario bleibt die Workload in einem bestimmten VNET isoliert und kann nicht mit anderen VNETs kommunizieren. Allerdings müssen die VNETs alle Zweige (VPN, ER und Benutzer-VPN) erreichen können. Um herauszufinden, wie viele Routingtabellen erforderlich sind, können Sie eine Verbindungsmatrix erstellen. In diesem Szenario sieht diese wie die folgende Tabelle aus, in der jede Zelle angibt, ob eine Quelle (Zeile) mit einem Ziel (Spalte) kommunizieren kann:

| From |   To |  *VNETs* | *Branches* |
| -------------- | -------- | ---------- | ---|
| VNETs     | &#8594;|           |     X    |
| Branches   | &#8594;|    X     |     X    |

Jede Zelle in der vorstehenden Tabelle beschreibt, ob eine Virtual WAN-Verbindung (die Seite „From“ des Flows, die Zeilenheader) ein Zielpräfix (die Seite „To“ des Flows, die kursiv formatierten Spaltenheader) für einen bestimmten Datenverkehrsfluss lernt. Dabei bedeutet ein „X“, dass die Konnektivität durch eine Virtual WAN-Verbindung bereitgestellt wird.

Diese Verbindungsmatrix gibt zwei verschiedene Zeilenmuster an, die in zwei Routingtabellen übersetzt werden. Virtual WAN verfügt bereits über eine Standardroutingtabelle, daher ist eine weitere Routingtabelle erforderlich. In diesem Beispiel nennen wir die Routingtabelle **RT_VNET**.

VNETs werden dieser Routingtabelle **RT_VNET** zugeordnet. Da die Branches Konnektivität zu Branches benötigen, müssen sie an **RT_VNET** weitergegeben werden (andernfalls werden die Branchpräfixe nicht von den VNETs erlernt). Da die Branches immer der Standardroutingtabelle zugeordnet sind, müssen die VNETs an die Standardroutingtabelle weitergegeben werden. Dies ist somit der endgültige Entwurf:

* Virtuelle Netzwerke:
  * Zugeordnete Routingtabelle: **RT_VNET**
  * Weitergabe an Routingtabellen: **Standard**
* Branches:
  * Zugeordnete Routingtabelle: **Standard**
  * Weitergabe an Routingtabellen: **RT_VNET** und **Standard**

Da nur Branches an die Routingtabelle **RT_VNET** weitergegeben werden, sind dies die einzigen Präfixe, die VNETs erlernen, nicht die anderer VNETs.

Weitere Informationen zum Routing für virtuelle Hubs finden Sie unter [Informationen zum Routing virtueller Hubs](about-virtual-hub-routing.md).

## <a name="workflow"></a><a name="workflow"></a>Workflow

Führen Sie zur Konfiguration dieses Szenarios die folgenden Schritte aus:

1. Erstellen Sie eine benutzerdefinierte Routingtabelle in jedem Hub. Im Beispiel ist **RT_VNet** die Routingtabelle. Informationen, wie eine Routingtabelle erstellt wird, finden Sie unter [Konfigurieren des Routings für virtuelle Hubs](how-to-virtual-hub-routing.md). Weitere Informationen über Routingtabellen finden Sie unter [Informationen zum Routing virtueller Hubs](about-virtual-hub-routing.md).
2. Wenn Sie die **RT_VNet**-Routingtabelle erstellen, konfigurieren Sie die folgenden Einstellungen:

   * **Zuordnung:** Wählen Sie die VNETs aus, die Sie isolieren möchten.
   * **Weitergabe**: Wählen Sie die Option für Zweige aus, was impliziert, dass Zweigverbindungen (VPN/ER/P2S-Verbindungen) Routen an diese Routingtabelle weitergeben.

:::image type="content" source="./media/routing-scenarios/isolated/isolated-vnets.png" alt-text="Isolierte VNETs":::

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu Virtual WAN finden Sie unter [Häufig gestellte Fragen](virtual-wan-faq.md).
* Weitere Informationen zum Routing für virtuelle Hubs finden Sie unter [Informationen zum Routing virtueller Hubs](about-virtual-hub-routing.md).
