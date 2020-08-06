---
title: Übersicht zu Azure Monitor für Container | Microsoft-Dokumentation
description: Dieser Artikel beschreibt Azure Monitor für Container, das die AKS Container Insights-Lösung überwacht, und den Wert, den es durch die Überwachung der Integrität Ihrer AKS-Cluster und Containerinstanzen in Azure bietet.
ms.topic: conceptual
ms.date: 05/21/2020
ms.openlocfilehash: 85adae1b79609f13be6bb73eb4e57000beda6d77
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87320271"
---
# <a name="azure-monitor-for-containers-overview"></a>Azure Monitor für Container – Übersicht

Azure Monitor für Container ist ein Feature zur Überwachung der Leistung von Containerworkloads, die für folgende Komponenten bereitgestellt werden:

- Managed Kubernetes-Cluster, die in [Azure Kubernetes Service (AKS)](../../aks/intro-kubernetes.md) gehostet werden
- Selbstverwaltete Kubernetes-Cluster, die in Azure mit der [AKS-Engine](https://github.com/Azure/aks-engine) gehostet werden
- [Azure Container Instances](../../container-instances/container-instances-overview.md)
- Selbstverwaltete Kubernetes-Cluster, die in [Azure Stack](/azure-stack/user/azure-stack-kubernetes-aks-engine-overview?view=azs-1910) oder lokal gehostet werden
- [Azure Red Hat OpenShift](../../openshift/intro-openshift.md)
- [Kubernetes mit Azure Arc-Aktivierung](../../azure-arc/kubernetes/overview.md) (Vorschauversion)

Azure Monitor für Container unterstützt Cluster, auf denen das Betriebssystem Linux und Windows Server 2019 ausgeführt wird.

Die Überwachung Ihrer Container ist vor allem dann entscheidend, wenn Sie einen umfangreichen Produktionscluster mit mehreren Anwendungen ausführen.

Azure Monitor für Container visualisiert die Leistung, indem anhand der Metrik-API die in Kubernetes verfügbaren Speicher- und Prozessormetriken von Controllern, Knoten und Containern erfasst werden. Auch Containerprotokolle werden erfasst.  Nach der Aktivierung der Überwachung auf Kubernetes-Clustern werden Metriken und Protokolle für Sie automatisch mittels einer Containerversion des Log Analytics-Agents für Linux erfasst. Metriken werden in den Metrikspeicher geschrieben, und Protokolldaten werden in den Protokollspeicher geschrieben, der Ihrem [Log Analytics](../log-query/log-query-overview.md)-Arbeitsbereich zugeordnet ist.

![Architektur von Azure Monitor für Container](./media/container-insights-overview/azmon-containers-architecture-01.png)

## <a name="what-does-azure-monitor-for-containers-provide"></a>Was bietet Azure Monitor für Container?

Azure Monitor für Container stellt eine umfassende Überwachungsumgebung auf Grundlage verschiedener Features von Azure Monitor bereit. Mithilfe dieser Features können Sie die Leistung und Integrität Ihres Kubernetes-Clusters, auf dem das Betriebssystem Linux und Windows Server 2019 ausgeführt wird, sowie die Containerworkloads nachvollziehen. Azure Monitor für Container bietet Ihnen folgende Möglichkeiten:

* Sie können auf dem Knoten ausgeführte AKS-Container sowie deren durchschnittliche Prozessor- und Speicherauslastung ermitteln. Mit diesem Wissen können Sie Ressourcenengpässe erkennen.
* Ermitteln Sie die Prozessor- und Arbeitsspeichernutzung von Containergruppen und den zugehörigen Containern, die in Azure Container Instances gehostet werden.
* Sie können feststellen, wo im Controller bzw. im Pod sich der Container befindet. Dadurch können Sie die Gesamtleistung des Controllers bzw. Pods anzeigen.
* Sie können die Ressourcenauslastung von Workloads überprüfen, die unabhängig von den Standardprozessen, die den Pod unterstützen, im Host ausgeführt werden.
* Sie bekommen Einblicke in das Verhalten des Clusters bei durchschnittlichen und schwersten Lasten. So können Sie benötigte Kapazitäten ermitteln und die maximale Last bestimmen, die der Cluster toleriert.
* Konfigurieren Sie Warnungen so, dass Sie proaktiv benachrichtigt werden oder aufgezeichnet wird, wenn die CPU- und Arbeitsspeicherauslastung auf Knoten oder in Containern die Schwellenwerte überschreitet oder wenn eine Änderung des Integritätszustands im Cluster beim Integritätsrollup für Infrastruktur oder Knoten erfolgt.
* Über die Integration mit [Prometheus](https://prometheus.io/docs/introduction/overview/) können Sie Anwendungs- und Workloadmetriken anzeigen, die von Knoten und Kubernetes mithilfe von [Abfragen](container-insights-log-search.md) gesammelt werden, um benutzerdefinierte Warnungen und Dashboards zu erstellen und ausführliche Analysen durchzuführen.
* Überwachen Sie Containerworkloads, [die auf der AKS-Engine](https://github.com/Azure/aks-engine) lokal und der [AKS-Engine in Azure Stack](/azure-stack/user/azure-stack-kubernetes-aks-engine-overview?view=azs-1908) bereitgestellt werden.
* Überwachen von Containerworkloads, [die in Azure Red Hat OpenShift](../../openshift/intro-openshift.md) bereitgestellt sind.

    >[!NOTE]
    >Die Unterstützung für Azure Red Hat OpenShift ist zurzeit als Feature in der öffentlichen Vorschauversion verfügbar.
    >

* Überwachen von Containerworkloads, die in [Kubernetes mit Azure Arc-Aktivierung (Vorschau) ](../../azure-arc/kubernetes/overview.md) bereitgestellt sind.

Nachfolgend sind die wichtigsten Unterschiede bei der Überwachung eines Windows Server-Clusters im Vergleich zu einem Linux-Cluster aufgeführt:

- Windows weist keine Arbeitsspeicher-RSS-Metrik auf, daher ist diese für Windows-Knoten und -Container nicht verfügbar. Die Metrik [Arbeitssatz](/windows/win32/memory/working-set) ist verfügbar.
- Für Windows-Knoten sind keine Informationen zur Speicherkapazität des Datenträgers verfügbar.
- Nur Pod-Umgebungen werden überwacht, nicht aber Docker-Umgebungen.
- Mit der Vorschauversion werden maximal 30 Windows Server-Container unterstützt. Diese Einschränkung gilt nicht für Linux-Container.

Anhand des folgenden Videos für fortgeschrittene Benutzer können Sie sich ausführlicher mit der Überwachung Ihres AKS-Clusters mit Azure Monitor für Container vertraut machen.

> [!VIDEO https://www.youtube.com/embed/RjsNmapggPU]

## <a name="how-do-i-access-this-feature"></a>Wie greife ich auf dieses Feature zu?

Sie können auf zwei Arten auf Azure Monitor für Container zugreifen, aus Azure Monitor oder direkt aus dem ausgewählten AKS-Cluster. In Azure Monitor haben Sie eine globale Perspektive aller bereitgestellten Container, der überwachten wie auch der nicht überwachten, was es Ihnen ermöglicht, über Ihre Abonnements und Ressourcengruppen zu suchen und zu filtern und dann Drilldowns aus einem ausgewählten Container in Azure Monitor für Container auszuführen.  Andernfalls können Sie von der AKS-Seite aus einem ausgewählten AKS-Container auf das Feature direkt zugreifen.

![Übersicht der Methoden für den Zugriff auf Azure Monitor für Container](./media/container-insights-overview/azmon-containers-experience.png)

Wenn Sie Ihre außerhalb von AKS ausgeführten Docker- und Windows-Containerhosts überwachen und verwalten möchten, um Informationen zur Konfiguration, Überwachung und Ressourcenverwendung anzuzeigen, lesen Sie den Artikel zur [Containerüberwachungslösung](./containers.md).

## <a name="next-steps"></a>Nächste Schritte

Um in die Überwachung Ihrer Kubernetes-Cluster einzusteigen, lesen Sie [Aktivieren von Azure Monitor für Container](container-insights-onboard.md), um ein Verständnis für die Anforderungen und verfügbaren Methoden zum Aktivieren von Überwachung zu entwickeln.

