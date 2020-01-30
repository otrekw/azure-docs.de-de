---
title: Überwachen der Integrität von Kubernetes-Clustern mit Azure Monitor für Container | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie mit Azure Monitor für Container die Integrität Ihrer AKS- und Nicht-AKS-Cluster anzeigen und analysieren können.
ms.topic: conceptual
ms.date: 12/01/2019
ms.openlocfilehash: f50ef13efca78bbb5285b99759b8111dc1915ad0
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2020
ms.locfileid: "76843989"
---
# <a name="understand-kubernetes-cluster-health-with-azure-monitor-for-containers"></a>Verstehen der Integrität von Kubernetes-Clustern mit Azure Monitor für Container

Mit Azure Monitor für Container wird der Integritätsstatus der verwalteten Infrastrukturkomponenten und aller Knoten, die auf einem von Azure Monitor für Container unterstützten Kubernetes-Cluster ausgeführt werden, überwacht und gemeldet. Dies ist noch weitreichender als der Integritätsstatus des Clusters, der in der [Multi-Cluster-Ansicht](container-insights-analyze.md#multi-cluster-view-from-azure-monitor) berechnet und gemeldet wird. Sie können nun erkennen, ob ein oder mehrere Knoten im Cluster eingeschränkte Ressourcen aufweisen oder ein Knoten oder Pod nicht verfügbar ist, was sich basierend auf bereitgestellten Metriken auf eine aktive Anwendung im Cluster auswirken kann.

>[!NOTE]
>Die Integritätsfunktion befindet sich derzeit in der öffentlichen Vorschauphase.
>

Weitere Informationen zum Aktivieren von Azure Monitor für Container finden Sie unter [Onboarding von Azure Monitor für Container](container-insights-onboard.md).

>[!NOTE]
>Vergewissern Sie sich zur Unterstützung der AKS-Engine-Cluster, dass sie Folgendes erfüllen:
>- Es werden die neuesten Versionen des [HTML-Clients](https://helm.sh/docs/using_helm/) verwendet.
>- Die Version des containerisierten Agents ist *microsoft/oms:ciprod11012019*. Informationen zum Upgraden des Agents finden Sie unter [Upgraden des Agents im Kubernetes-Cluster](container-insights-manage-agent.md#upgrade-agent-on-monitored-kubernetes-cluster).
>

## <a name="overview"></a>Übersicht

Das Integritätsfeature (Preview) in Azure Monitor für Container bietet eine proaktive Integritätsüberwachung Ihres Kubernetes-Clusters als Hilfe beim Identifizieren und Diagnostizieren von Problemen. Sie haben dadurch die Möglichkeit, wichtige erkannte Probleme anzuzeigen. Monitore zur Auswertung der Integrität Ihres Clusters, werden im Container-Agent in Ihrem Cluster ausgeführt, und die Integritätsdaten werden in die Tabelle **KubeHealth** in Ihrem Log Analytics-Arbeitsbereich geschrieben. 

Die Integrität des Kubernetes-Clusters basiert auf einer Reihe von Überwachungsszenarien, die nach folgenden Kubernetes-Objekten und -Abstraktionen organisiert sind:

- Kubernetes-Infrastruktur: Bietet einen Rollup für den Kubernetes-API-Server, ReplicaSets und DaemonSets, die auf bereitgestellten Knoten in Ihrem Cluster ausgeführt werden, durch Auswertung der CPU- und Arbeitsspeicherauslastung und der Verfügbarkeit von Pods.

    ![Ansicht des Integritätsrollups für Kubernetes-Infrastruktur](./media/container-insights-health/health-view-kube-infra-01.png)

- Knoten: Bietet einen Rollup für Knotenpools und den Status einzelner Knoten in jedem Pool durch Auswertung der CPU- und Arbeitsspeicherauslastung und den Status eines Knotens, wie er von Kubernetes gemeldet wird.

    ![Ansicht des Integritätsrollups für Knoten](./media/container-insights-health/health-view-nodes-01.png)

Derzeit wird nur der Status eines virtuellen Kubelets unterstützt. Der Integritätsstatus für die CPU- und Arbeitsspeicherauslastung von Knoten eines virtuellen Kublets wird als **Unbekannt** gemeldet, da von diesen kein Signal empfangen wird.

Alle Monitore werden in einem hierarchischen Layout im Bereich „Integritätshierarchie“ angezeigt, in dem ein Aggregatmonitor, der das Kubernetes-Objekt oder die Abstraktion (d.h. Kubernetes-Infrastruktur oder Knoten) darstellt, der Monitor auf oberster Ebene ist, der die kombinierte Integrität aller abhängigen untergeordneten Monitore darstellt. Die wichtigsten Überwachungsszenarien zum Ableiten der Integrität sind:

* Auswerten der CPU-Auslastung von Knoten und Container
* Auswerten der Arbeitsspeicherauslastung von Knoten und Container
* Status von Pods und Knoten basierend auf der Berechnung des Bereitschaftszustands, der von Kubernetes gemeldet wird

Folgende Symbole dienen zur Angabe des Status:

|Symbol|Bedeutung|  
|--------|-----------|  
|![Ein grünes Häkchensymbol bedeutet fehlerfrei](./media/container-insights-health/healthyicon.png)|Erfolgreich, Integrität ist in Ordnung (grün)|  
|![Ein gelbes Dreieck mit Ausrufezeichen kennzeichnet eine Warnung](./media/container-insights-health/warningicon.png)|Warnung (gelb)|  
|![Ein roter Kreis mit weißem X zeigt einen kritischen Zustand an](./media/container-insights-health/criticalicon.png)|Kritisch (rot)|  
|![Ausgegrautes Symbol](./media/container-insights-health/grayicon.png)|Unbekannt (grau)|  

## <a name="monitor-configuration"></a>Monitorkonfiguration

Informationen zum Verhalten und zur Konfiguration der einzelnen Monitore, die das Integritätsfeature von Azure Monitor für Container unterstützen, finden Sie im [Konfigurationsleitfaden für Integritätsmonitore](container-insights-health-monitors-config.md).

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. 

## <a name="view-health-of-an-aks-or-non-aks-cluster"></a>Anzeigen der Integrität eines AKS- oder Nicht-AKS-Clusters

Der Zugriff auf das Integritätsfeature (Preview) von Azure Monitor für Container ist direkt aus einem AKS-Cluster möglich, indem Sie im Azure-Portal im linken Bereich die Option **Insights** auswählen. Wählen Sie im Abschnitt **Insights** die Option **Container** aus. 

Zum Anzeigen der Integrität für einen Nicht-AKS-Cluster, bei dem es sich um einen lokal oder auf Azure Stack gehosteten AKS-Engine-Cluster handelt, wählen Sie im Azure-Portal im linken Bereich die Option **Azure Monitor** aus. Wählen Sie im Abschnitt **Insights** die Option **Container** aus.  Wählen Sie auf der Multi-Cluster-Seite den Nicht-AKS-Cluster aus der Liste aus.

Wählen Sie in Azure Monitor für Container auf der Seite **Cluster** die Option **Integrität** aus.

![Beispiel des Dashboards für Clusterintegrität](./media/container-insights-health/container-insights-health-page.png)

## <a name="review-cluster-health"></a>Überprüfen der Clusterintegrität

Wenn die Seite „Integrität“ geöffnet wird, ist im Raster **Integritätsaspekt** standardmäßig die Option **Kubernetes-Infrastruktur** ausgewählt.  Das Raster stellt eine Übersicht über den aktuellen Status des Integritätsrollups von Kubernetes-Infrastruktur und Clusterknoten bereit. Bei Auswahl eines der Integritätsaspekte werden die Ergebnisse im Bereich „Integritätshierarchie“ (d.h. im mittleren Bereich) aktualisiert und alle untergeordneten Monitore in einem hierarchischen Layout mit dem aktuellen Integritätsstatus angezeigt. Wenn Sie weitere Informationen zu einem abhängigen Monitor anzeigen möchten, wählen Sie einen Monitor aus. Daraufhin wird automatisch rechts auf der Seite ein Eigenschaftenbereich geöffnet. 

![Eigenschaftenbereich für Clusterintegrität](./media/container-insights-health/health-view-property-pane.png)

Im Eigenschaftenbereich stehen folgende Informationen bereit:

- Auf der Registerkarte **Übersicht** werden der aktuelle Status des ausgewählten Monitors, der Zeitpunkt der letzten Berechnung des Monitors und der Zeitpunkt der letzten Statusänderung angezeigt. Je nach Typ des in der Hierarchie ausgewählten Monitors werden zusätzliche Informationen angezeigt.

    Wenn Sie im Bereich „Integritätshierarchie“ einen Aggregatmonitor auswählen, werden auf der Registerkarte **Übersicht** im Eigenschaftenbereich ein Rollup der Gesamtzahl der untergeordneten Monitore in der Hierarchie und die Anzahl der Aggregatmonitore mit dem Status „Kritisch“, „Warnung“ und „Fehlerfrei“ angezeigt. 

    ![Integritätseigenschaftenbereich auf der Registerkarte „Übersicht“ für einen Aggregatmonitor](./media/container-insights-health/health-overview-aggregate-monitor.png)

    Wenn Sie im Bereich „Integritätshierarchie“ einen Einheitenmonitor auswählen, werden unter **Letzte Statusänderung** außerdem die letzten Stichproben angezeigt, die vom Container-Agent in den letzten vier Stunden berechnet und gemeldet wurden. Dies basiert auf der Berechnung des Einheitenmonitors für den Vergleich mehrerer aufeinander folgender Werte zur Bestimmung des Status. Wenn Sie z.B. den Einheitenmonitor *Pods im Zustand „Bereit“* ausgewählt haben, werden die letzten beiden Stichproben angezeigt, die durch den Parameter *ConsecutiveSamplesForStateTransition* gesteuert werden. Weitere Informationen finden Sie in der ausführlichen Beschreibung der [Einheitenmonitore](container-insights-health-monitors-config.md#unit-monitors).
    
    ![Integritätseigenschaftenbereich auf der Registerkarte „Übersicht“](./media/container-insights-health/health-overview-unit-monitor.png)

    Wenn der für **Letzte Statusänderung** angegebene Zeitpunkt einen Tag oder länger zurückliegt, haben keine Statusänderungen für den Monitor stattgefunden. Wenn jedoch die zuletzt für einen Einheitenmonitor empfangene Stichprobe älter als vier Stunden ist, weist dies wahrscheinlich darauf hin, dass der Container-Agent keine Daten gesendet hat. Wenn der Agent weiß, dass eine bestimmte Ressource vorhanden ist (z.B. ein Knoten), aber von den Monitoren für CPU- oder Arbeitsspeicherauslastung des Knotens keine Daten empfangen wurden, wird der Integritätsstatus des Monitors auf **Unbekannt** gesetzt.  

- Auf der Registerkarte **Konfiguration** werden die Standardeinstellungen der Konfigurationsparameter (nur für Einheitenmonitore, nicht für Aggregatmonitore) und deren Werte angezeigt.
- Die Registerkarte **Wissen** enthält Informationen, die das Verhalten des Monitors und dessen Auswertung für eine fehlerhafte Bedingung erläutern.

Überwachungsdaten auf dieser Seite werden nicht automatisch aktualisiert, und Sie müssen oben auf der Seite **Aktualisieren** auswählen, um den aktuellen Integritätsstatus anzuzeigen, der vom Cluster empfangen wurde.

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich die [Beispiele zu Protokollabfragen](container-insights-log-search.md#search-logs-to-analyze-data) an, die auch vordefinierte Abfragen enthalten. Mit diesen Materialien können Sie Auswertungen bzw. Anpassungen für Warnungen, Visualisierungen und Analysen von Clustern vornehmen.
