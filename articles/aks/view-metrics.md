---
title: Anzeigen von Clustermetriken für Azure Kubernetes Service (AKS)
description: Anzeigen von Clustermetriken für Azure Kubernetes Service (AKS).
services: container-service
ms.topic: article
ms.date: 03/30/2021
ms.openlocfilehash: 32d8745e0ea780b5f86588fabdc25b244cf2cd8e
ms.sourcegitcommit: f5448fe5b24c67e24aea769e1ab438a465dfe037
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105975813"
---
# <a name="view-cluster-metrics-for-azure-kubernetes-service-aks"></a>Anzeigen von Clustermetriken für Azure Kubernetes Service (AKS)

AKS stellt einen Satz von Metriken für die Steuerungsebene bereit, einschließlich API-Server, Clusterautoskalierung und Clusterknoten. Diese Metriken ermöglichen es Ihnen, die Integrität Ihres Clusters zu überwachen und Probleme zu behandeln. Sie können die Metriken für Ihren Cluster über das Azure-Portal anzeigen.

> [!NOTE]
> Diese AKS-Clustermetriken überlappen sich mit einer Teilmenge der [von Kubernetes bereitgestellten Metriken][kubernetes-metrics].

## <a name="view-metrics-for-your-aks-cluster-using-the-azure-portal"></a>Anzeigen von Metriken für Ihren AKS-Cluster mithilfe des Azure-Portals

So zeigen Sie die Metriken für Ihren AKS-Cluster an

1. Melden Sie sich beim [Azure-Portal][azure-portal] an, und navigieren Sie zu Ihrem AKS-Cluster.
1. Wählen Sie links unter *Überwachung* die Option *Metriken* aus.
1. Erstellen Sie ein Diagramm für die Metriken, die Sie anzeigen möchten. Erstellen Sie zum Beispiel ein Diagramm:
    1. Wählen Sie für *Bereich* Ihren Cluster aus.
    1. Wählen Sie für *Metriknamespace* die Option *Containerdienst (verwaltet) Standardmetriken* aus.
    1. Wählen Sie für *Metrik* unter *Pods* die *Anzahl von Pods nach Phase* aus.
    1. Wählen Sie für *Aggregation* die Option *Durchschn.* aus.

:::image type="content" source="media/metrics/metrics-chart.png" alt-text="{alt-text}":::

Das obige Beispiel zeigt die Metriken für die durchschnittliche Anzahl von Pods für *myAKSCluster* an.

## <a name="available-metrics"></a>Verfügbare Metriken

Die folgenden Clustermetriken sind verfügbar:

| Name | Gruppieren | id | BESCHREIBUNG |
| --- | --- | --- | ---- |
| In-Flight-Anforderungen | API-Server (Vorschau) |apiserver_current_inflight_requests | Maximale Anzahl der aktuell aktiven In-Flight-Anforderungen auf dem API-Server pro Anforderungstyp. |
| Clusterintegrität | Clusterautoskalierung (Vorschauversion) | cluster_autoscaler_cluster_safe_to_autoscale | Bestimmt, ob die Autoskalierung Aktionen für den Cluster durchführt oder nicht. |
| Herunterskalieren im Abkühlen | Clusterautoskalierung (Vorschauversion) | cluster_autoscaler_scale_down_in_cooldown | Bestimmt, ob sich das Herunterskalieren im Abkühlen befindet: Während dieses Zeitraums werden keine Knoten entfernt. |
| Nicht benötigte Knoten | Clusterautoskalierung (Vorschauversion) | cluster_autoscaler_unneeded_nodes_count | Die Clusterautoskalierung markiert diese Knoten als Kandidaten zum Löschen, die auch schließlich gelöscht werden. |
| Nicht planbare Pods | Clusterautoskalierung (Vorschauversion) | cluster_autoscaler_unschedulable_pods_count | Anzahl von Pods, die derzeit im Cluster nicht planbar sind. |
| Gesamtzahl der verfügbaren CPU-Kerne in einem verwalteten Cluster | Nodes | kube_node_status_allocatable_cpu_cores | Gesamtzahl der verfügbaren CPU-Kerne in einem verwalteten Cluster. |
| Gesamtzahl des verfügbaren Speicherplatzes in einem verwalteten Cluster | Nodes | kube_node_status_allocatable_memory_bytes | Gesamtzahl des verfügbaren Speicherplatzes in einem verwalteten Cluster. |
| Status für verschiedene Knotenbedingungen | Nodes | kube_node_status_condition | Status für verschiedene Knotenbedingungen |
| CPU-Auslastung (Millicore) | Knoten (Vorschau) | node_cpu_usage_millicores | Aggregierte Messung der CPU-Auslastung im gesamten Cluster in Millicore. |
| Prozentuale CPU-Auslastung | Knoten (Vorschau) | node_cpu_usage_percentage | Aggregierte durchschnittliche CPU-Auslastung in Prozent für den gesamten Cluster. |
| Arbeitsspeicher-RSS (Bytes) | Knoten (Vorschau) | node_memory_rss_bytes | Verwendeter RSS-Arbeitsspeicher des Containers in Byte. |
| Arbeitsspeicher-RSS (Prozent) | Knoten (Vorschau) | node_memory_rss_percentage | Verwendeter RSS-Arbeitsspeicher des Containers in Prozent. |
| Arbeitssatz für Arbeitsspeicher (Bytes) | Knoten (Vorschau) | node_memory_working_set_bytes | Verwendeter Arbeitssatz-Arbeitsspeicher des Containers in Bytes. |
| Arbeitssatz für Arbeitsspeicher (Prozent) | Knoten (Vorschau) | node_memory_working_set_percentage | Verwendeter Arbeitssatz-Arbeitsspeicher des Containers in Prozent. |
| Datenträgerverwendung (Bytes) | Knoten (Vorschau) | node_disk_usage_bytes | Verwendeter Datenträgerspeicherplatz in Bytes nach Gerät. |
| Datenträgerverwendung (Prozent) | Knoten (Vorschau) | node_disk_usage_percentage | Verwendeter Datenträgerspeicherplatz in Prozent nach Gerät. |
| Eingehender Netzwerkverkehr (Bytes) | Knoten (Vorschau) | node_network_in_bytes | Empfangene Netzwerk-Bytes. |
| Ausgehender Netzwerkverkehr (Bytes) | Knoten (Vorschau) | node_network_out_bytes | Übertragene Netzwerk-Bytes. |
| Anzahl der Pods mit dem Status „Bereit“ | Pods | kube_pod_status_ready | Anzahl der Pods mit dem Status *Bereit*. |
| Anzahl der Pods nach Phase | Pods | kube_pod_status_phase | Anzahl der Pods nach Phase. |

> [!IMPORTANT]
> Metriken in der Vorschau können aktualisiert oder geändert werden, einschließlich ihrer Namen und Beschreibungen, während Sie sich in der Vorschauphase befinden.

## <a name="next-steps"></a>Nächste Schritte

Zusätzlich zu den Clustermetriken für AKS können Sie auch Azure Monitor mit Ihrem AKS-Cluster verwenden. Weitere Informationen zur Verwendung von Azure Monitor mit AKS finden Sie unter [Azure Monitor für Container][aks-azure-monitory].

[aks-azure-monitory]: ../azure-monitor/containers/container-insights-overview.md
[azure-portal]: https://portal.azure.com/
[kubernetes-metrics]: https://kubernetes.io/docs/concepts/cluster-administration/system-metrics/