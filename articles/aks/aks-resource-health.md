---
title: Überprüfung auf Resource Health-Ereignisse, die sich auf Ihren AKS-Cluster auswirken (Vorschau)
description: Überprüfen Sie den Integritätsstatus Ihres AKS-Clusters mithilfe von Azure Resource Health.
services: container-service
author: yunjchoi
ms.topic: troubleshooting
ms.date: 08/18/2020
ms.author: yunjchoi
ms.openlocfilehash: a409ac944c51d893fc344f82ae83d97559e055ef
ms.sourcegitcommit: a92fbc09b859941ed64128db6ff72b7a7bcec6ab
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2020
ms.locfileid: "92070655"
---
# <a name="check-for-resource-health-events-impacting-your-aks-cluster-preview"></a>Überprüfung auf Resource Health-Ereignisse, die sich auf Ihren AKS-Cluster auswirken (Vorschau)


Wenn Sie Ihre Containerworkloads in AKS ausführen, sollten Sie sicherstellen, dass Sie Probleme ermitteln und behandeln können, sobald diese auftreten. Dadurch lassen sich die Auswirkungen auf die Verfügbarkeit Ihrer Workloads minimieren. Mit [Azure Resource Health](../service-health/resource-health-overview.md) erhalten Sie wertvolle Einblicke in verschiedene Integritätsereignisse, aufgrund derer Ihr AKS-Cluster möglicherweise nicht mehr verfügbar ist.

[!INCLUDE [preview features callout](./includes/preview/preview-callout.md)]

## <a name="open-resource-health"></a>Öffnen von Ressource Health

### <a name="to-access-resource-health-for-your-aks-cluster"></a>So greifen Sie auf Resource Health für Ihren AKS-Cluster zu:

- Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem AKS-Cluster.
- Wählen Sie im linken Navigationsbereich den Eintrag **Resource Health** aus.

### <a name="to-access-resource-health-for-all-clusters-on-your-subscription"></a>So greifen Sie auf Resource Health für alle Cluster innerhalb Ihres Abonnements zu:

- Suchen Sie im [Azure-Portal](https://portal.azure.com) nach **Service Health**, und navigieren Sie zu diesem Dienst.
- Wählen Sie im linken Navigationsbereich den Eintrag **Resource Health** aus.
- Wählen Sie Ihr Abonnement aus, und legen Sie als Ressourcentyp Azure Kubernetes Service (AKS) fest.

![Screenshot, der die Ressourcenintegrität für AKS-Cluster zeigt.](./media/aks-resource-health/resource-health-check.png)

## <a name="check-the-health-status"></a>Überprüfen des Integritätsstatus

Azure Resource Health unterstützt Sie bei der Diagnose und bei Supportanfragen, wenn Dienstprobleme Auswirkungen auf Ihre Azure-Ressourcen haben. Resource Health liefert Informationen zum aktuellen und vergangenen Integritätsstatus Ihrer Ressourcen und hilft Ihnen zu bestimmen, ob das Problem durch eine benutzerseitig initiierte Aktion oder durch ein Plattformereignis ausgelöst wurde.

Resource Health empfängt Signale für Ihren verwalteten Cluster, die Auskunft über den Integritätszustand des Clusters geben. Der Dienst untersucht den Integritätszustand Ihres AKS-Clusters und zeigt die erforderlichen Aktionen für die einzelnen Integritätssignale an. Diese Signale reichen von Problemen, die automatisch behoben werden, über geplante Updates und ungeplante Integritätsereignisse bis hin zur Nichtverfügbarkeit, die durch benutzerinitiierte Aktionen ausgelöst wurde. Diese Signale werden anhand des Azure Resource Health-Integritätsstatus klassifiziert: *Verfügbar*, *Nicht verfügbar*, *Unbekannt* und *Beeinträchtigt*.

- **Verfügbar**: Wenn keine bekannten Probleme vorliegen, die sich auf die Integrität Ihres Clusters auswirken, lautet der Resource Health-Status für Ihren Cluster *Verfügbar*.

- **Nicht verfügbar**: Wenn ein Plattformereignis oder anderes Ereignis aufgetreten ist, das sich auf die Integrität Ihres Clusters auswirkt, lautet der Resource Health-Status für Ihren Cluster *Nicht verfügbar*.

- **Unbekannt:** Wenn die Verbindung mit den Integritätsmetriken Ihres Clusters vorübergehend unterbrochen ist, lautet der Resource Health-Status für Ihren Cluster *Unbekannt*.

- **Beeinträchtigt**: Wenn ein Integritätsproblem vorliegt, das Sie behandeln müssen, lautet der Resource Health-Status für Ihren Cluster *Beeinträchtigt*.

Weitere Informationen zu den einzelnen Integritätsstatus finden Sie unter [Übersicht über Resource Health](../service-health/resource-health-overview.md#health-status).

### <a name="view-historical-data"></a>Anzeigen von Verlaufsdaten

Im Abschnitt **Integritätsverlauf** können Sie Resource Health-Verlaufsdaten der letzten 30 Tage anzeigen.

## <a name="next-steps"></a>Nächste Schritte

Verwenden Sie die [AKS-Diagnose](./concepts-diagnostics.md), um weitere Überprüfungen für Ihren Cluster durchzuführen und Clusterprobleme zu behandeln.