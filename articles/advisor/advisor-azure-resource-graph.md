---
title: Advisor-Daten in Azure Resource Graph
description: Abfragen von Advisor-Daten in Azure Resource Graph
ms.topic: article
ms.date: 03/12/2020
ms.author: sagupt
ms.openlocfilehash: f8ad8fd450bc004d9caa2699922717f38d38b482
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79503914"
---
# <a name="query-for-advisor-data-in-resource-graph-explorer-azure-resource-graph"></a>Abfragen von Advisor-Daten im Resource Graph-Explorer (Azure Resource Graph)

Advisor-Ressourcen sind nun in [Azure Resource Graph](https://azure.microsoft.com/features/resource-graph/) integriert. Dadurch wird eine Grundlage für viele bedarfsgerechte Kundenszenarien mit Advisor-Empfehlungen geschaffen. Im Anschluss finden Sie einige Szenarien, die zuvor nicht im großen Stil möglich waren, nun aber mithilfe von Resource Graph umgesetzt werden können:
* Gives-Funktion, um komplexe Abfragen für alle Ihre Abonnements im Azure-Portal zu ermöglichen
* Nach Kategorietyp (z. B. Hochverfügbarkeit, Leistung) und Auswirkungstyp (hoch, mittel, niedrig) zusammengefasste Empfehlungen
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
* [Advisor-Empfehlungen zu Kosten](advisor-cost-recommendations.md)
* [Advisor-Empfehlungen zur Leistung](advisor-performance-recommendations.md)
* [Advisor-Empfehlungen zur Sicherheit](advisor-security-recommendations.md)
* [Advisor-Empfehlungen für einen optimalen Betrieb](advisor-operational-excellence-recommendations.md)
* [Advisor-REST-API](https://docs.microsoft.com/rest/api/advisor/)
