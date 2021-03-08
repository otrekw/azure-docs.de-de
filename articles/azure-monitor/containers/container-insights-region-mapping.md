---
title: Regionszuordnungen in Container Insights
description: In diesem Artikel werden die von Container Insights, dem Log Analytics-Arbeitsbereich und von benutzerdefinierten Metriken unterstützten Regionszuordnungen beschrieben.
ms.topic: conceptual
ms.date: 09/22/2020
ms.custom: references_regions
ms.openlocfilehash: f9e910b1352109608becb82609e85e26d27d2cd1
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101728874"
---
# <a name="region-mappings-supported-by-container-insights"></a>Von Container Insights unterstützte Regionszuordnungen

 Wenn Sie Container Insights aktivieren, werden nur bestimmte Regionen für das Verknüpfen eines Log Analytics-Arbeitsbereichs und eines AKS-Clusters sowie für das Erfassen benutzerdefinierter Metriken unterstützt, die an Azure Monitor übermittelt werden.

## <a name="log-analytics-workspace-supported-mappings"></a>Log Analytics-Arbeitsbereich unterstützt Zuordnungen

Unterstützte AKS-Regionen sind unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service) aufgeführt. Der Log Analytics-Arbeitsbereich muss sich in derselben Region befinden. Ausgenommen sind davon die in der folgenden Tabelle aufgeführten Regionen. Achten Sie auf Aktualisierungen in den [AKS-Versionshinweisen](https://github.com/Azure/AKS/releases).


|**AKS-Clusterregion** | **Log Analytics-Arbeitsbereichsregion** |
|-----------------------|------------------------------------|
|**Afrika** | |
|Südafrika, Nord |Europa, Westen |
|Südafrika, West |Europa, Westen |
|**Australien** | |
|Australien, Mitte 2 |Australien, Mitte |
|**Brasilien** | |
|Brasilien, Süden | USA, Süden-Mitte |
|**Kanada** ||
|CanadaEast |CanadaCentral |
|**Europa** | |
|Frankreich, Süden |Frankreich, Mitte |
|UK, Westen |UKSouth |
|**Indien** | |
|SouthIndia |CentralIndia |
|WestIndia |CentralIndia |
|**Japan** | |
|Japan, Westen |JapanEast |
|**Korea** | |
|KoreaSouth |KoreaCentral |
|**USA** | |
|USA, Westen-Mitte<sup>1</sup>|EastUS<sup>1</sup>|


<sup>1</sup> Aufgrund von Kapazitätseinschränkungen ist die Region nicht verfügbar, wenn neue Ressourcen erstellt werden. Dies beinhaltet einen Log Analytics-Arbeitsbereich. Allerdings sollten bereits in der Region vorhandene verknüpfte Ressourcen weiterhin funktionieren.

## <a name="custom-metrics-supported-regions"></a>Regionen, die benutzerdefinierte Metriken unterstützen

Das Sammeln von Metriken von Knoten und Pods von AKS-Clustern (Azure Kubernetes Services) wird für die Veröffentlichung als benutzerdefinierte Metriken nur in den folgenden [Azure-Regionen](../essentials/metrics-custom-overview.md#supported-regions) unterstützt.

## <a name="next-steps"></a>Nächste Schritte

Informationen für die Voraussetzungen und verfügbaren Methoden zum Aktivieren der Überwachung, damit Sie mit der Überwachung Ihres AKS-Clusters beginnen können, finden Sie unter [Aktivieren von Container Insights](container-insights-onboard.md).  
