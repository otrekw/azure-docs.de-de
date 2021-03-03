---
title: Bereitstellungs- und HPA-Metriken bei Azure Monitor für Container | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, welche Bereitstellungs- und HPA-Metriken (horizontale automatische Podskalierung) mit Azure Monitor für Container gesammelt werden.
ms.topic: conceptual
ms.date: 08/09/2020
ms.openlocfilehash: ee1f0d4849a8382a898aaca84956ff78166e138f
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/17/2021
ms.locfileid: "100602223"
---
# <a name="deployment--hpa-metrics-with-azure-monitor-for-containers"></a>Bereitstellungs- und HPA-Metriken bei Azure Monitor für Container

Ab Agent-Version *ciprod08072020* werden von dem in Azure Monitor für Container integrierten Agent nun Metriken für Bereitstellungen und HPAs erfasst.

## <a name="deployment-metrics"></a>Bereitstellungsmetriken

Azure Monitor für Container beginnt automatisch mit der Überwachung von Bereitstellungen, indem die folgenden Metriken in 60-Sekunden-Intervallen gesammelt und in der **InsightMetrics**-Tabelle gespeichert werden:

|Metrikname |Metrikdimension (Tags) |Beschreibung |
|------------|------------------------|------------|
|kube_deployment_status_replicas_ready |container.azm.ms/clusterId, container.azm.ms/clusterName, creationTime, deployment, deploymentStrategy, k8sNamespace, spec_replicas, status_replicas_available, status_replicas_updated (status.updatedReplicas) | Die Gesamtzahl einsatzbereiter Pods, die diese Bereitstellung zum Ziel hat (status.readyReplicas). Nachfolgend sind Dimensionen dieser Metrik aufgeführt. <ul> <li> deployment: Name der Bereitstellung </li> <li> k8sNamespace: Kubernetes-Namespace für die Bereitstellung </li> <li> deploymentStrategy: Bereitstellungsstrategie zum Ersetzen von Pods durch neue (spec.strategy.type)</li><li> creationTime: Zeitstempel der Bereitstellungserstellung </li> <li> spec_replicas: Anzahl gewünschter Pods (spec.replicas) </li> <li>status_replicas_available: Gesamtzahl verfügbarer Pods (bereit über mindestens minReadySeconds), die diese Bereitstellung zum Ziel hat (status.availableReplicas)</li><li>status_replicas_updated: Gesamtzahl nicht beendeter Pods, die diese Bereitstellung zum Ziel hat und die gewünschte Vorlagenspezifikation aufweisen (status.updatedReplicas) </li></ul>|

## <a name="hpa-metrics"></a>HPA-Metriken

Azure Monitor für Container beginnt automatisch mit der Überwachung von HPAs, indem die folgenden Metriken in 60-Sekunden-Intervallen gesammelt und in der **InsightMetrics**-Tabelle gespeichert werden:

|Metrikname |Metrikdimension (Tags) |Beschreibung |
|------------|------------------------|------------|
|kube_hpa_status_current_replicas |container.azm.ms/clusterId, container.azm.ms/clusterName, creationTime, hpa, k8sNamespace, lastScaleTime, spec_max_replicas, spec_min_replicas, status_desired_replicas, targetKind, targetName | Aktuelle Anzahl der Replikate von Pods, die von dieser automatischen Skalierungsfunktion verwaltet werden (status.currentReplicas). Nachfolgend sind Dimensionen dieser Metrik aufgeführt. <ul> <li> hpa: Name der HPA </li> <li> k8sNamespace: Kubernetes-Namespace für die HPA </li> <li> lastScaleTime: Letzter Zeitpunkt, zu dem die HPA die Anzahl von Pods skaliert hat (status.lastScaleTime)</li><li> creationTime: Zeitstempel der HPA-Erstellung </li> <li> spec_max_replicas: Obergrenze für die Anzahl von Pods, die von der automatischen Skalierungsfunktion festgelegt werden können (spec.maxReplicas) </li> <li> spec_min_replicas: Untergrenze für die Anzahl von Replikaten, auf die die automatische Skalierungsfunktion horizontal herunterskalieren kann (spec.minReplicas) </li><li>status_desired_replicas: Gewünschte Anzahl der Replikate von Pods, die von dieser automatischen Skalierungsfunktion verwaltet werden (status.desiredReplicas)</li><li>targetKind: Art des HPA-Ziels (spec.scaleTargetRef.kind) </li><li>targetName: Name des HPA-Ziels (spec.scaleTargetRef.name) </li></ul>|

## <a name="deployment--hpa-charts"></a>Bereitstellungs- und HPA-Diagramme 

Azure Monitor für Container enthält vorkonfigurierte Diagramme für die Metriken, die weiter oben in der Tabelle als eine Arbeitsmappe für jeden Cluster aufgeführt sind. Sie finden die Bereitstellungs- und HPA-Arbeitsmappe **Deployments & HPA** direkt von einem AKS-Cluster aus, indem Sie **Arbeitsmappen** im linken Bereich und in der Dropdownliste in Insight **Arbeitsmappen anzeigen** auswählen.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu kube-state-metrics finden Sie unter [kube-state-metrics in Kubernetes](https://github.com/kubernetes/kube-state-metrics/tree/master/docs).