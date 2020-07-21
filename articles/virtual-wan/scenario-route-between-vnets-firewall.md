---
title: 'Szenario: Benutzerdefiniertes Routing über Azure Firewall für Virtual WAN'
titleSuffix: Azure Virtual WAN
description: Routingszenarios – direktes Routing von Datenverkehr zwischen VNets, jedoch Verwendung von Azure Firewall für die Datenverkehrsflüsse VNet > Internet/Branch und Branch > VNet
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 06/29/2020
ms.author: cherylmc
ms.openlocfilehash: cdaa594a87d960688638591e6edd525aa3b048f4
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85567942"
---
# <a name="scenario-azure-firewall---custom"></a>Szenario: Azure Firewall: Benutzerdefiniert

Wenn Sie mit Virtual WAN-Routing für virtuelle Hubs arbeiten, steht Ihnen eine ganze Reihe von Szenarios zur Verfügung. In diesem Szenario besteht das Ziel darin, Datenverkehr zwischen VNets direkt weiterzuleiten, für die Datenverkehrsflüsse vom VNet zum Internet/Branch und vom Branch zum VNet hingegen Azure Firewall zu verwenden. Weitere Informationen zum Routing in virtuellen Hubs finden Sie unter [Routing in virtuellen Hubs](about-virtual-hub-routing.md).

## <a name="scenario-workflow"></a><a name="workflow"></a>Workflow des Szenarios

In diesem Szenario möchten Sie Datenverkehr von einem VNet zum Internet oder Branch und von einem Branch zu einem VNet über Azure Firewall weiterleiten, während der Datenverkehr zwischen VNets direkt weitergeleitet werden soll. Wenn Sie Azure Firewall Manager verwendet haben, werden die Routeneinstellungen automatisch in die **Standardroutingtabelle** eingetragen. Privater Datenverkehr gilt für VNet und Branches, Internetdatenverkehr gilt für 0.0.0.0/0.

VPN-, ExpressRoute- und Benutzer-VPN-Verbindungen werden zusammen als Branches bezeichnet und derselben (Standard-)Routingtabelle zugeordnet. Alle VPN-, ExpressRoute- und Benutzer-VPN-Verbindungen geben Routen an dieselben Routingtabellen weiter. Führen Sie zur Konfiguration dieses Szenarios die folgenden Schritte aus:

1. Erstellen Sie eine benutzerdefinierte Routingtabelle **RT_VNET**.
1. Erstellen Sie eine Route, um VNet zu Internet und VNet zu Branch: 0.0.0.0/0 mit dem nächsten Hop zu aktivieren, der auf Azure Firewall zeigt. Stellen Sie im Abschnitt „Verteilung“ sicher, dass VNets ausgewählt werden, sodass spezifischere Routen gewährleistet werden und dadurch ein direkter Datenverkehrsfluss von VNet zu VNet möglich wird.

   * Unter **Zuordnung**: Wählen Sie VNets aus, sodass VNets das Ziel gemäß den Routen dieser Routingtabelle erreichen.
   * Unter **Weitergabe**: Wählen Sie VNets aus, sodass die Weitergabe der VNets an diese Routingtabelle erfolgt. Die Weitergabe an diese Routingtabelle erfolgt also mit spezifischeren Routen, was einen direkten Datenverkehrsfluss zwischen den VNets sicherstellt.

1. Fügen Sie eine aggregierte statische Route für VNets zur **Standardroutingtabelle** hinzu, um den Datenverkehrsfluss vom Branch zum VNet über Azure Firewall zu aktivieren. 

   * Beachten Sie, dass Branches verknüpft sind und bei ihnen die Weitergabe an die Standardroutingtabelle erfolgt.
   * Branches führen keine Weitergabe an die Routingtabelle RT_VNET durch. Hierdurch wird sichergestellt, dass der Datenverkehrsfluss vom VNet zum Branch über Azure Firewall erfolgt.

Dies führt, wie in **Abbildung 1** gezeigt, zu Änderungen an der Routingkonfiguration.

**Abbildung 1**

:::image type="content" source="./media/routing-scenarios/between-vnets-firewall/routing.png" alt-text="Abbildung 1":::

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu Virtual WAN finden Sie unter [Häufig gestellte Fragen](virtual-wan-faq.md).
* Weitere Informationen zum Routing in virtuellen Hubs finden Sie unter [Routing in virtuellen Hubs](about-virtual-hub-routing.md).
* Weitere Informationen zum Konfigurieren des Routings in virtuellen Hubs finden Sie unter [Konfigurieren des Routings in virtuellen Hubs](how-to-virtual-hub-routing.md).
