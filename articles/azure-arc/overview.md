---
title: Übersicht über Azure Arc
description: Hier erfahren Sie, was Azure Arc ist und wie es Kunden bei der Verwaltung und Governance ihrer Hybridressourcen mit anderen Azure-Diensten und -Features unterstützt.
ms.date: 05/25/2021
ms.topic: overview
ms.openlocfilehash: b67c01f666916508946b49b0ad45d7151ed179ef
ms.sourcegitcommit: 91fdedcb190c0753180be8dc7db4b1d6da9854a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/17/2021
ms.locfileid: "112287297"
---
# <a name="azure-arc-overview"></a>Übersicht über Azure Arc

Heutzutage ist es schwierig, immer komplexere Umgebungen zu kontrollieren und zu steuern. Diese Umgebungen umfassen Rechenzentren, mehreren Clouds und den Edgebereich. Jede Umgebung und Cloud besitzt ihre eigenen separaten Verwaltungstools, mit denen Sie sich vertraut machen müssen, um sie verwenden zu können.

Hinzu kommt, dass sich neue DevOps- und ITOps-Betriebsmodelle nicht so ohne Weiteres implementieren lassen, da neue cloudnative Muster von den bereits vorhandenen Tools nicht unterstützt werden.

Azure Arc bietet eine konsistente Verwaltungsplattform für mehrere Clouds und die lokale Umgebung und vereinfacht so die Governance und Verwaltung. Mit Azure Arc können Sie folgende Aktionen durchführen:
* Sie können Ihre gesamte Umgebung über eine zentralisierte Benutzeroberfläche verwalten, indem Sie Ihre vorhandenen Ressourcen (Ressourcen, die nicht aus Azure stammen, lokale Ressourcen oder jene aus anderen Clouds) in Azure Resource Manager projizieren. 
* Sie verwalten virtuelle Computer, Kubernetes-Cluster und Datenbanken so, als würden sie in Azure ausgeführt. 
* Sie können ortsunabhängig vertraute Azure-Dienste und Verwaltungsfunktionen verwenden. 
* Sie können herkömmliche ITOps-Methoden weiterverwenden und gleichzeitig DevOps-Methoden einführen, um neue cloudnative Muster in Ihrer Umgebung zu unterstützen.
* Sie können „Benutzerdefinierte Speicherorte“ als Abstraktionsschicht auf der Grundlage von Kubernetes-Clustern mit Azure Arc-Unterstützung, Cluster Connect und Clustererweiterungen konfigurieren.  

:::image type="content" source="./media/overview/azure-arc-control-plane.png" alt-text="Diagramm: Steuerungsebene der Azure Arc-Verwaltung" border="false":::

Aktuell können mit Azure Arc folgende außerhalb von Azure gehostete Ressourcentypen verwaltet werden:

* Server: Sowohl physische als auch virtuelle Computer unter Windows oder Linux
* Kubernetes-Cluster: Unterstützung mehrerer Kubernetes-Distributionen
* Azure-Datendienste: Azure SQL Managed Instance und PostgreSQL Hyperscale
* SQL Server: Registrieren von Instanzen von einem beliebigen Standort aus

## <a name="what-does-azure-arc-deliver"></a>Was bietet Azure Arc?

Zu den wichtigsten Features von Azure Arc zählen folgende:

* Implementieren einer konsistenten Bestands-, Verwaltungs-, Governance- und Sicherheitslösung für die Server in Ihrer gesamten Umgebung

* Konfigurieren von [Azure-VM-Erweiterungen](./servers/manage-vm-extensions.md) für die Verwendung von Azure-Verwaltungsdiensten zur Überwachung, zum Schutz und zur Aktualisierung Ihrer Server

* Verwalten und Steuern von Kubernetes-Clustern im großen Stil

* Verwenden von GitOps zur Bereitstellung von Konfigurationen in einem oder mehreren Clustern aus Git-Repositorys.

*  Zero Touch-Compliance und -Konfiguration für Ihre Kubernetes-Cluster mit Azure Policy.

* Die Ausführung von [Azure Data Services](../azure-arc/kubernetes/custom-locations.md) in einer beliebigen Kubernetes-Umgebung ist wie die Ausführung in Azure möglich (insbesondere Azure SQL Managed Instance und Azure Database for PostgreSQL Hyperscale mit Vorteilen wie Upgrades/Updates, Sicherheit und Überwachung). Nutzen Sie die elastische Skalierung, und wenden Sie Updates ganz ohne Anwendungsdowntime an, sogar ohne kontinuierliche Verbindung mit Azure.

* Erstellen Sie [benutzerdefinierter Standorte](./kubernetes/custom-locations.md) auf Grundlage Ihrer [Azure Arc-fähigen Kubernetes-Cluster](./kubernetes/overview.md), wobei diese als Zielstandorte für die Bereitstellung von Azure-Dienstinstanzen verwendet werden Sie können Ihre Azure-Dienstclustererweiterungen für [Azure Arc-fähige Data Services-Instanzen](./data/deploy-data-controller-direct-mode.md), [App Services auf Azure Arc](../app-service/overview-arc-integration.md) (einschließlich Web- Funktions- und Logik-Apps) und für [Event Grid in Kubernetes](../event-grid/kubernetes/overview.md) bereitstellen.

* Einheitliche Betrachtung Ihrer Azure Arc-fähigen Ressourcen bei Verwendung von Azure-Portal, Azure CLI, Azure PowerShell oder Azure-REST-API

## <a name="how-much-does-azure-arc-cost"></a>Wie viel kostet Azure Arc?

Im Anschluss finden Sie Preisinformationen für die Features, die aktuell mit Azure Arc verfügbar sind:

### <a name="arc-enabled-servers"></a>Server mit Arc-Aktivierung

Die folgende Funktionalität der Azure Arc-Steuerungsebene wird ohne Zusatzkosten angeboten:

* Ressourcenorganisation über Azure-Verwaltungsgruppen und -Tags.

* Suche und Indizierung über Azure Resource Graph.

* Zugriff und Sicherheit durch Azure RBAC und Abonnements.

* Umgebungen und Automatisierung über Vorlagen und Erweiterungen.

* Updateverwaltung

Auf Arc-fähigen Servern verwendete Azure-Dienste wie etwa Azure Security Center oder Azure Monitor werden zu den Preisen für den jeweiligen Dienst abgerechnet. Weitere Informationen finden Sie auf der Seite [Azure-Preise](https://azure.microsoft.com/pricing/).

### <a name="azure-arc-enabled-kubernetes"></a>Kubernetes mit Azure Arc-Aktivierung

Auf Arc-fähigem Kubernetes verwendete Azure-Dienste wie etwa Azure Security Center oder Azure Monitor werden zu den Preisen für den jeweiligen Dienst abgerechnet. Weitere Informationen zu Preisen für Konfigurationen auf Azure Arc-fähigem Kubernetes finden Sie auf der Seite [Azure-Preise](https://azure.microsoft.com/pricing/).

### <a name="azure-arc-enabled-data-services"></a>Azure Arc-fähige Datendienste

In der aktuellen Vorschauphase fallen für Azure Arc-fähige Datendienste keine zusätzlichen Kosten an.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu Arc-fähigen Servern finden Sie in [dieser Übersicht](./servers/overview.md).

* Weitere Informationen zu Arc-fähigen Kubernetes-Instanzen finden Sie in [dieser Übersicht](./kubernetes/overview.md).

* Weitere Informationen zu Arc-fähigen Datendiensten finden Sie in [dieser Übersicht](https://azure.microsoft.com/services/azure-arc/hybrid-data-services/).

* Informieren Sie sich über Arc-fähige Dienste unter [Schnelleinstieg; Proof of Concept](https://azurearcjumpstart.io/azure_arc_jumpstart/).
