---
title: Übersicht über Azure Arc
description: Hier erfahren Sie, was Azure Arc ist und wie es Kunden bei der Verwaltung und Governance ihrer Hybridressourcen mit anderen Azure-Diensten und -Features unterstützt.
ms.date: 08/25/2020
ms.topic: overview
ms.openlocfilehash: 46bd2089dbc2794d0fb98ceb9a5e97011e36d6ea
ms.sourcegitcommit: 8a7b82de18d8cba5c2cec078bc921da783a4710e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2020
ms.locfileid: "89052027"
---
# <a name="azure-arc-overview"></a>Übersicht über Azure Arc

Aufgrund der stetig zunehmenden Komplexität von Umgebungen tun sich Unternehmen heutzutage schwer damit, diese zu steuern und zu kontrollieren. Diese Umgebungen umfassen Rechenzentren, mehreren Clouds und den Edgebereich. Jede Umgebung und Cloud hat ihre eigenen separaten Verwaltungstools, mit denen Sie sich vertraut machen müssen, um sie verwenden zu können.

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

* Verwenden einer GitOps-basierten Konfiguration als Codeverwaltung, um Anwendungen und Konfigurationen direkt über eine Quellcodeverwaltung (etwa GitHub) in einem einzelnen Cluster oder clusterübergreifend bereitzustellen

* Zero Touch-Compliance und -Konfiguration für Ihre Kubernetes-Cluster mit Azure Policy

* Ausführen von Azure Data Services in einer beliebigen Kubernetes-Umgebung (insbesondere Azure SQL Managed Instance und Azure Database for PostgreSQL Hyperscale) mit Vorteilen wie Upgrades/Updates, Sicherheit und Überwachung wie bei der Ausführung in Azure. Nutzen von elastischer Skalierung und Anwenden von Updates ganz ohne Anwendungsdowntime, selbst wenn keine kontinuierliche Verbindung mit Azure besteht

* Einheitliche Betrachtung Ihrer Azure Arc-fähigen Ressourcen bei Verwendung von Azure-Portal, Azure CLI, Azure PowerShell oder Azure-REST-API

## <a name="how-much-does-azure-arc-cost"></a>Wie viel kostet Azure Arc?

Im Anschluss finden Sie Preisinformationen für die Features, die aktuell mit Azure Arc verfügbar sind:

### <a name="arc-enabled-servers"></a>Server mit Arc-Aktivierung

In der aktuellen Vorschauphase fallen für Azure Arc-fähige Server keine zusätzlichen Kosten an.

Auf Arc-fähigen Servern verwendete Azure-Dienste wie etwa Azure Security Center oder Azure Monitor werden zu den Preisen für den jeweiligen Dienst abgerechnet. Weitere Informationen finden Sie auf der Seite [Azure-Preise](https://azure.microsoft.com/pricing/).

### <a name="azure-arc-enabled-kubernetes"></a>Kubernetes mit Azure Arc-Aktivierung

In der aktuellen Vorschauphase fallen für Azure Arc-fähige Kubernetes-Instanzen keine zusätzlichen Kosten an.

### <a name="azure-arc-enabled-data-services"></a>Azure Arc-fähige Datendienste

In der aktuellen Vorschauphase fallen für Azure Arc-fähige Datendienste keine zusätzlichen Kosten an.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu Arc-fähigen Servern finden Sie in [dieser Übersicht](./servers/overview.md).

* Weitere Informationen zu Arc-fähigen Kubernetes-Instanzen finden Sie in [dieser Übersicht](./kubernetes/overview.md).

* Weitere Informationen zu Arc-fähigen Datendiensten finden Sie in [dieser Übersicht](https://azure.microsoft.com/services/azure-arc/hybrid-data-services/).
