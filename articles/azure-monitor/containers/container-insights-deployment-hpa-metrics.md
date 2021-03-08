---
title: Bereitstellung und HPA-Metriken mit Container Insights | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, welche Bereitstellungs- und HPA-Metriken (horizontale automatische Podskalierung) mit Container Insights gesammelt werden.
ms.topic: conceptual
ms.date: 08/09/2020
ms.openlocfilehash: c8bb100b756ea92d73e1c3a698f119b4f8157930
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101717705"
---
# <a name="deployment--hpa-metrics-with-container-insights"></a>Bereitstellungs- und HPA-Metriken mit Container Insights

Ab Agent-Version *ciprod08072020* werden von dem in Container Insights integrierten Agent jetzt Metriken für Bereitstellungen und HPAs erfasst.

## <a name="deployment-metrics"></a>Bereitstellungsmetriken

Container Insights beginnt automatisch mit der Überwachung von Bereitstellungen, indem die folgenden Metriken in 60-Sekunden-Intervallen gesammelt und in der **InsightMetrics**-Tabelle gespeichert werden:

|Metrikname |Metrikdimension (Tags) |Beschreibung |
|------------|------------------------|------------|
|kube_deployment_status_replicas_ready |container.azm.ms/clusterId, container.azm.ms/clusterName, creationTime, deployment, deploymentStrategy, k8sNamespace, spec_replicas, status_replicas_available, status_replicas_updated (status.updatedReplicas) | Die Gesamtzahl einsatzbereiter Pods, die diese Bereitstellung zum Ziel hat (status.readyReplicas). Nachfolgend sind Dimensionen dieser Metrik aufgeführt. <ul> <li> deployment: Name der Bereitstellung </li> <li> k8sNamespace: Kubernetes-Namespace für die Bereitstellung </li> <li> deploymentStrategy: Bereitstellungsstrategie zum Ersetzen von Pods durch neue (spec.strategy.type)</li><li> creationTime: Zeitstempel der Bereitstellungserstellung </li> <li> spec_replicas: Anzahl gewünschter Pods (spec.replicas) </li> <li>status_replicas_available: Gesamtzahl verfügbarer Pods (bereit über mindestens minReadySeconds), die diese Bereitstellung zum Ziel hat (status.availableReplicas)</li><li>status_replicas_updated: Gesamtzahl nicht beendeter Pods, die diese Bereitstellung zum Ziel hat und die gewünschte Vorlagenspezifikation aufweisen (status.updatedReplicas) </li></ul>|

## <a name="hpa-metrics"></a>HPA-Metriken

Container Insights beginnt automatisch mit der Überwachung von HPAs, indem die folgenden Metriken in 60-Sekunden-Intervallen gesammelt und in der **InsightMetrics**-Tabelle gespeichert werden:

|Metrikname |Metrikdimension (Tags) |Beschreibung |
|------------|------------------------|------------|
|kube_hpa_status_current_replicas |container.azm.ms/clusterId, container.azm.ms/clusterName, creationTime, hpa, k8sNamespace, lastScaleTime, spec_max_replicas, spec_min_replicas, status_desired_replicas, targetKind, targetName | Aktuelle Anzahl der Replikate von Pods, die von dieser automatischen Skalierungsfunktion verwaltet werden (status.currentReplicas). Nachfolgend sind Dimensionen dieser Metrik aufgeführt. <ul> <li> hpa: Name der HPA </li> <li> k8sNamespace: Kubernetes-Namespace für die HPA </li> <li> lastScaleTime: Letzter Zeitpunkt, zu dem die HPA die Anzahl von Pods skaliert hat (status.lastScaleTime)</li><li> creationTime: Zeitstempel der HPA-Erstellung </li> <li> spec_max_replicas: Obergrenze für die Anzahl von Pods, die von der automatischen Skalierungsfunktion festgelegt werden können (spec.maxReplicas) </li> <li> spec_min_replicas: Untergrenze für die Anzahl von Replikaten, auf die die automatische Skalierungsfunktion horizontal herunterskalieren kann (spec.minReplicas) </li><li>status_desired_replicas: Gewünschte Anzahl der Replikate von Pods, die von dieser automatischen Skalierungsfunktion verwaltet werden (status.desiredReplicas)</li><li>targetKind: Art des HPA-Ziels (spec.scaleTargetRef.kind) </li><li>targetName: Name des HPA-Ziels (spec.scaleTargetRef.name) </li></ul>|

## <a name="deployment--hpa-charts"></a>Bereitstellungs- und HPA-Diagramme 

Container Insights enthält vorkonfigurierte Diagramme für die Metriken, die weiter oben in der Tabelle als Arbeitsmappe für jeden Cluster aufgeführt sind. Sie finden die Bereitstellungs- und HPA-Arbeitsmappe **Deployments & HPA** direkt von einem AKS-Cluster aus, indem Sie **Arbeitsmappen** im linken Bereich und in der Dropdownliste in Insight **Arbeitsmappen anzeigen** auswählen.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu kube-state-metrics finden Sie unter [kube-state-metrics in Kubernetes](https://github.com/kubernetes/kube-state-metrics/tree/master/docs).