---
title: Advisor-Daten in Azure Resource Graph
description: Abfragen von Advisor-Daten in Azure Resource Graph
ms.topic: article
ms.date: 03/12/2020
ms.author: sagupt
ms.openlocfilehash: 66bb500d419d1f5537afafd7a2df543ded8cc7ce
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87057773"
---
# <a name="query-for-advisor-data-in-resource-graph-explorer-azure-resource-graph"></a>Abfragen von Advisor-Daten im Resource Graph-Explorer (Azure Resource Graph)

Advisor-Ressourcen sind nun in [Azure Resource Graph](https://azure.microsoft.com/features/resource-graph/) integriert. Dadurch wird eine Grundlage für viele bedarfsgerechte Kundenszenarien mit Advisor-Empfehlungen geschaffen. Im Anschluss finden Sie einige Szenarien, die zuvor nicht im großen Stil möglich waren, nun aber mithilfe von Resource Graph umgesetzt werden können:
* Gives-Funktion, um komplexe Abfragen für alle Ihre Abonnements im Azure-Portal zu ermöglichen
* Nach Kategorietyp (z. B. Zuverlässigkeit, Leistung) und Auswirkungstyp (hoch, mittel, niedrig) zusammengefasste Empfehlungen
* Alle Empfehlungen für einen bestimmten Empfehlungstyp
* Anzahl betroffener Ressourcen nach Empfehlungskategorie

![Advisor im Azure Resource Graph-Explorer](./media/azure-resource-graph-1.png)  


## <a name="advisor-resource-types-in-azure-graph"></a>Advisor-Ressourcentypen in Azure Graph

Verfügbare Advisor-Ressourcentypen in [Resource Graph](../governance/resource-graph/index.yml): Unter Advisor-Ressourcen stehen drei Ressourcentypen für Abfragen zur Verfügung. Die folgenden Ressourcen sind jetzt für Abfragen in Resource Graph verfügbar:
* Microsoft.Advisor/configurations
* Microsoft.Advisor/recommendations
* Microsoft.Advisor/suppressions

Diese Ressourcentypen werden unter einer neuen Tabelle namens „AdvisorResources“ aufgelistet, die Sie auch im Resource Graph-Explorer im Azure-Portal abfragen können.


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Advisor-Empfehlungen finden Sie unter:
* [Einführung in Azure Advisor](advisor-overview.md)
* [Erste Schritte mit Advisor](advisor-get-started.md)
* [Reduzieren der Dienstkosten mithilfe des Azure Advisors](advisor-cost-recommendations.md)
* [Verbessern der Verfügbarkeit Ihrer Anwendung mit dem Azure Advisor](advisor-high-availability-recommendations.md)
* [Verbessern der Leistung von Azure-Anwendungen mit dem Azure Advisor](advisor-performance-recommendations.md)
* [Erhöhen der Sicherheit von Ressourcen mit dem Azure Advisor](advisor-security-recommendations.md)
* [Sicherstellen des optimalen Betriebs mit dem Azure Advisor](advisor-operational-excellence-recommendations.md)
* [Advisor-REST-API](/rest/api/advisor/)
