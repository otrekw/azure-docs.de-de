---
title: Übersicht über Kubernetes mit Azure Arc-Aktivierung
services: azure-arc
ms.service: azure-arc
ms.date: 05/19/2020
ms.topic: overview
author: mlearned
ms.author: mlearned
description: Dieser Artikel enthält eine Übersicht zu Kubernetes mit Azure Arc-Aktivierung.
keywords: Kubernetes, Arc, Azure, Container
ms.custom: references_regions
ms.openlocfilehash: 3960a0f0b6866a0a5d3476e841d29c6e5a89a238
ms.sourcegitcommit: 69156ae3c1e22cc570dda7f7234145c8226cc162
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/03/2020
ms.locfileid: "84309933"
---
# <a name="what-is-azure-arc-enabled-kubernetes-preview"></a>Was ist die Vorschauversion von Kubernetes mit Azure Arc-Aktivierung?

Sie können Kubernetes-Cluster innerhalb oder außerhalb von Azure mit der Vorschauversion von Kubernetes mit Azure Arc-Aktivierung anfügen und konfigurieren. Wenn ein Kubernetes-Cluster an Azure Arc angefügt ist, wird er im Azure-Portal angezeigt. Er verfügt dann über eine Azure Resource Manager-ID und eine verwaltete Identität. Cluster sind an Azure-Standardabonnements angefügt, befinden sich in einer Ressourcengruppe und können wie jede andere Azure-Ressource Tags erhalten. 

Zum Herstellen einer Verbindung zwischen einem Kubernetes-Cluster und Azure muss ein Clusteradministrator entsprechende Agents bereitstellen. Diese Agents werden in einem Kubernetes-Namespace namens `azure-arc` ausgeführt und sind Kubernetes-Standardbereitstellungen. Die Agents sind verantwortlich für die Konnektivität mit Azure, die Erfassung von Azure Arc-Protokollen und -Metriken sowie die Überwachung von Konfigurationsanforderungen. 

Kubernetes mit Azure Arc-Aktivierung unterstützt branchenübliches SSL zum Absichern von Daten während der Übertragung. Die Daten werden außerdem im Ruhezustand verschlüsselt in einer Azure Cosmos DB-Datenbank gespeichert, um ihre Vertraulichkeit zu gewährleisten.
 
 > [!NOTE]
> Kubernetes mit Azure Arc-Aktivierung befindet sich in der Vorschauphase. Sie sollte nicht für Produktionsworkloads verwendet werden. 


## <a name="supported-scenarios"></a>Unterstützte Szenarios 

Kubernetes mit Azure Arc-Aktivierung unterstützt diese Szenarien: 

* Verbinden von außerhalb von Azure ausgeführtem Kubernetes für Bestand, Gruppierung und Kennzeichnung

* Bereitstellen von Anwendungen und Anwenden der Konfiguration mithilfe der GitOps-basierten Konfigurationsverwaltung 

* Verwenden von Azure Monitor für Container zum Anzeigen und Überwachen Ihrer Cluster 

* Anwenden von Richtlinien mit Azure Policy für Kubernetes 

 
## <a name="supported-regions"></a>Unterstützte Regionen 

Kubernetes mit Azure Arc-Aktivierung wird derzeit in diesen Regionen unterstützt: 

* East US 
* Europa, Westen 

## <a name="next-steps"></a>Nächste Schritte

* [Herstellen einer Verbindung mit einem Cluster](./connect-cluster.md)
