---
title: Übersicht über Azure Arc
description: Hier erfahren Sie, was Azure Arc ist und wie es Kunden bei der Verwaltung und Governance ihrer Hybridressourcen mit anderen Azure-Diensten und -Features unterstützt.
ms.date: 03/02/2021
ms.topic: overview
ms.openlocfilehash: 33c9d6ca87c3d8d2d8920ff429902f5876bbdc59
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "101650191"
---
# <a name="azure-arc-overview"></a>Übersicht über Azure Arc

Heutzutage ist es schwierig, immer komplexere Umgebungen zu kontrollieren und zu steuern. Diese Umgebungen umfassen Rechenzentren, mehreren Clouds und den Edgebereich. Jede Umgebung und Cloud besitzt ihre eigenen separaten Verwaltungstools, mit denen Sie sich vertraut machen müssen, um sie verwenden zu können.

Hinzu kommt, dass sich neue DevOps- und ITOps-Betriebsmodelle nicht so ohne Weiteres implementieren lassen, da neue cloudnative Muster von den bereits vorhandenen Tools nicht unterstützt werden.

Azure Arc bietet eine konsistente Verwaltungsplattform für mehrere Clouds und die lokale Umgebung und vereinfacht so die Governance und Verwaltung. Mit Azure Arc können Sie Ihre gesamte Umgebung über eine zentralisierte Benutzeroberfläche verwalten, indem Sie Ihre vorhandenen Ressourcen in Azure Resource Manager projizieren. Virtuelle Computer, Kubernetes-Cluster und Datenbanken können nun so verwaltet werden, als würden sie in Azure ausgeführt. Sie können ortsunabhängig vertraute Azure-Dienste und Verwaltungsfunktionen verwenden. Azure Arc ermöglicht die Weiterverwendung herkömmlicher ITOps-Methoden und führt gleichzeitig DevOps-Methoden ein, um neue cloudnative Muster in Ihrer Umgebung zu unterstützen.

:::image type="content" source="./media/overview/azure-arc-control-plane.png" alt-text="Diagramm: Steuerungsebene der Azure Arc-Verwaltung" border="false":::

Aktuell können mit Azure Arc folgende außerhalb von Azure gehostete Ressourcentypen verwaltet werden:

* Server: Sowohl physische als auch virtuelle Computer unter Windows oder Linux
* Kubernetes-Cluster: Unterstützung mehrerer Kubernetes-Distributionen
* Azure Data Services: Azure SQL-Datenbank und PostgreSQL Hyperscale

## <a name="what-does-azure-arc-deliver"></a>Was bietet Azure Arc?

Zu den wichtigsten Features von Azure Arc zählen folgende:

* Implementieren einer konsistenten Bestands-, Verwaltungs-, Governance- und Sicherheitslösung für die Server in Ihrer gesamten Umgebung

* Konfigurieren von [Azure-VM-Erweiterungen](./servers/manage-vm-extensions.md) für die Verwendung von Azure-Verwaltungsdiensten zur Überwachung, zum Schutz und zur Aktualisierung Ihrer Server

* Verwalten und Steuern von Kubernetes-Clustern im großen Stil

* Verwenden von GitOps zur Bereitstellung von Konfigurationen in einem oder mehreren Clustern aus Git-Repositorys.

*  Zero Touch-Compliance und -Konfiguration für Ihre Kubernetes-Cluster mit Azure Policy.

* Ausführen von Azure Data Services in einer beliebigen Kubernetes-Umgebung wie bei der Ausführung in Azure (insbesondere Azure SQL Managed Instance und Azure Database for PostgreSQL Hyperscale mit Vorteilen wie Upgrades/Updates, Sicherheit und Überwachung). Nutzen von elastischer Skalierung und Anwenden von Updates ganz ohne Anwendungsdowntime, sogar ohne kontinuierliche Verbindung mit Azure

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
