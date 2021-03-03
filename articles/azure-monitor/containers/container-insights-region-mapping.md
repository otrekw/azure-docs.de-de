---
title: Regionszuordnungen für Azure Monitor für Container
description: Hier werden die zwischen Azure Monitor für Container, dem Log Analytics-Arbeitsbereich und benutzerdefinierten Metriken unterstützten Regionszuordnungen beschrieben.
ms.topic: conceptual
ms.date: 09/22/2020
ms.custom: references_regions
ms.openlocfilehash: 9b77242de3e7845a97874b663266103bf00a8e66
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/17/2021
ms.locfileid: "100602089"
---
# <a name="region-mappings-supported-by-azure-monitor-for-containers"></a>Von Azure Monitor für Container unterstützte Regionszuordnungen

 Wenn Sie Azure Monitor für Container aktivieren, werden nur bestimmte Regionen unterstützt, um einen Log Analytics-Arbeitsbereich und einen AKS-Cluster zu verbinden und benutzerdefinierte Metriken zu sammeln, die an Azure Monitor übermittelt werden.

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

Um in die Überwachung Ihrer AKS-Cluster einzusteigen, lesen Sie [Aktivieren von Azure Monitor für Container](container-insights-onboard.md), um ein Verständnis für die Anforderungen und verfügbaren Methoden zum Aktivieren von Überwachung zu entwickeln.  
