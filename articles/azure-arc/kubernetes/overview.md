---
title: Übersicht über Kubernetes mit Azure Arc-Aktivierung
services: azure-arc
ms.service: azure-arc
ms.date: 05/19/2020
ms.topic: overview
author: mlearned
ms.author: mlearned
description: ''
keywords: Kubernetes, Arc, Azure, Container
ms.openlocfilehash: 3b3b184ff7b18e05dfeb1052c0d6b4aa406a8787
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83665275"
---
# <a name="what-is-azure-arc-enabled-kubernetes-preview"></a>Was ist Kubernetes mit Azure Arc-Aktivierung (Vorschau)?

Sie können Kubernetes-Cluster innerhalb oder außerhalb von Azure mit Azure Arc-Aktivierung für Kubernetes (Vorschau) anfügen und konfigurieren. Wenn ein Kubernetes-Cluster an Azure Arc angefügt wird, wird es im Azure-Portal angezeigt und weist eine Azure Resource Manager-ID sowie eine verwaltete Identität auf. Cluster sind an Azure-Standardabonnements angefügt, befinden sich in einer Ressourcengruppe und können wie jede andere Azure-Ressource Tags erhalten. 


Zum Herstellen einer Verbindung zwischen einem Kubernetes-Cluster und Azure muss ein Clusteradministrator entsprechende Agents bereitstellen. Diese Agents werden in einem Kubernetes-Namespace namens `azure-arc` ausgeführt und sind Kubernetes-Standardbereitstellungen. Die Agents sind verantwortlich für die Konnektivität mit Azure, die Erfassung von Azure Arc-Protokollen und -Metriken sowie die Überwachung von Konfigurationsanforderungen.  
 
 > [!NOTE]
> Kubernetes mit Azure Arc-Aktivierung befindet sich in der Vorschauversion und wird für Workloads in Produktionsumgebungen nicht empfohlen. 


## <a name="supported-scenarios"></a>Unterstützte Szenarien 

Kubernetes mit Azure Arc-Aktivierung unterstützt die folgenden Szenarien: 

* Verbinden von außerhalb von Azure ausgeführtem Kubernetes für Bestand, Gruppierung und Kennzeichnung 

* Bereitstellen von Anwendungen und Anwenden der Konfiguration mithilfe der GitOps-basierten Konfigurationsverwaltung 

* Verwenden von Azure Monitor für Container zum Anzeigen und Überwachen Ihrer Cluster 

* Anwenden von Richtlinien mit Azure Policy für Kubernetes 

 
## <a name="supported-regions"></a>Unterstützte Regionen 

Kubernetes mit Azure Arc-Aktivierung wird derzeit in den folgenden Regionen unterstützt: 

* East US 
* Europa, Westen 


## <a name="next-steps"></a>Nächste Schritte

* [Verbinden eines Clusters](./connect-cluster.md)
