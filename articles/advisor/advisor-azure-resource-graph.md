---
title: Advisor-Daten in Azure Resource Graph
description: Abfragen von Advisor-Daten in Azure Resource Graph
ms.topic: article
ms.date: 03/12/2020
ms.author: sagupt
ms.openlocfilehash: c0786d1d09ff61ddd9c375c68b7199521e319a4f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85117833"
---
# <a name="query-for-advisor-data-in-resource-graph-explorer-azure-resource-graph"></a>Abfragen von Advisor-Daten im Resource Graph-Explorer (Azure Resource Graph)

Advisor-Ressourcen sind nun in [Azure Resource Graph](https://azure.microsoft.com/features/resource-graph/) integriert. Dadurch wird eine Grundlage für viele bedarfsgerechte Kundenszenarien mit Advisor-Empfehlungen geschaffen. Im Anschluss finden Sie einige Szenarien, die zuvor nicht im großen Stil möglich waren, nun aber mithilfe von Resource Graph umgesetzt werden können:
* Gives-Funktion, um komplexe Abfragen für alle Ihre Abonnements im Azure-Portal zu ermöglichen
* Nach Kategorietyp (z. B. Zuverlässigkeit, Leistung) und Auswirkungstyp (hoch, mittel, niedrig) zusammengefasste Empfehlungen
* Alle Empfehlungen für einen bestimmten Empfehlungstyp
* Anzahl betroffener Ressourcen nach Empfehlungskategorie

![Advisor im Azure Resource Graph-Explorer](./media/azure-resource-graph-1.png)  


## <a name="advisor-resource-types-in-azure-graph"></a>Advisor-Ressourcentypen in Azure Graph

Verfügbare Advisor-Ressourcentypen in [Resource Graph](https://docs.microsoft.com/azure/governance/resource-graph/): Unter Advisor-Ressourcen stehen drei Ressourcentypen für Abfragen zur Verfügung. Die folgenden Ressourcen sind jetzt für Abfragen in Resource Graph verfügbar:
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
* [Advisor-REST-API](https://docs.microsoft.com/rest/api/advisor/)
