---
title: Einführung zu Azure Managed Instance for Apache Cassandra
description: Erfahren Sie mehr über Azure Managed Instance for Apache Cassandra. Dieser Dienst verwaltet die Bereitstellung und Skalierung von nativen Open-Source-Instanzen von Apache Cassandra in Azure.
author: TheovanKraay
ms.author: thvankra
ms.service: managed-instance-apache-cassandra
ms.topic: overview
ms.date: 03/02/2021
ms.openlocfilehash: d99c62e7d88510c351f87d85b4f8c5c271767cb3
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101747625"
---
# <a name="what-is-azure-managed-instance-for-apache-cassandra-preview"></a>Was ist Azure Managed Instance for Apache Cassandra? (Vorschau)

> [!IMPORTANT]
> Azure Managed Instance for Apache Cassandra ist zurzeit als öffentliche Vorschauversion verfügbar.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Der Dienst Azure Managed Instance for Apache Cassandra bietet automatisierte Bereitstellungs- und Skalierungsvorgänge für verwaltete Open-Source-basierte Apache Cassandra-Rechenzentren. Dieser Dienst unterstützt Sie dabei, Hybridszenarien zu beschleunigen und die laufende Wartung zu verringern. Bei der allgemeinen Veröffentlichung wird er umfassende Integrations- und Datenverschiebungsfunktionen mit der [Azure Cosmos DB-Cassandra-API](../cosmos-db/cassandra-introduction.md) bieten.

:::image type="content" source="./media/introduction/icon.gif" alt-text="Azure Managed Instance for Apache Cassandra ist ein verwalteter Dienst für Apache Cassandra." border="false":::

## <a name="key-benefits"></a>Hauptvorteile

### <a name="hybrid-deployments"></a>Hybridbereitstellungen

Mit diesem Dienst können Sie problemlos verwaltete Instanzen von Apache Cassandra-Rechenzentren, die automatisch als VM-Skalierungsgruppen bereitgestellt werden, in einem neuen oder vorhandenen Azure-VNET platzieren. Diese Rechenzentren können Ihrem vorhandenen Apache Cassandra-Ring, der lokal über [Azure ExpressRoute](/azure/architecture/reference-architectures/hybrid-networking/expressroute) in Azure ausgeführt wird, oder einer anderen Cloudumgebung hinzugefügt werden.

- **Vereinfachte Bereitstellung:** Nach Herstellen der Hybridkonnektivität ist die Bereitstellung über das Gossipprotokoll einfach.
- **Gehostete Metriken:** Die Metriken werden in [Prometheus](https://prometheus.io/docs/introduction/overview/) gehostet, um Aktivitäten in Ihrem Cluster zu überwachen.

### <a name="simplified-scaling"></a>Vereinfachte Skalierung

In der verwalteten Instanz wird das Hoch- und Herunterskalieren von Knoten in einem Rechenzentrum vollständig verwaltet. Sie geben die Anzahl der benötigten Knoten ein, und der Skalierungsorchestrator übernimmt die Einrichtung des Vorgangs innerhalb des Cassandra-Rings.

### <a name="managed-and-cost-effective"></a>Verwaltet und kostengünstig

Der Dienst stellt Verwaltungsvorgänge für die folgenden allgemeinen Apache Cassandra-Aufgaben bereit:

- Bereitstellen eines Clusters
- Bereitstellen eines Rechenzentrums
- Skalieren eines Rechenzentrums
- Löschen eines Rechenzentrums
- Starten einer Reparaturaktion für einen Keyspace
- Ändern der Konfiguration eines Rechenzentrums
- Einrichten von Sicherungen

Das Preismodell ist flexibel, bedarfsgesteuert, instanzbasiert und frei von Lizenzierungsgebühren. Dieses Preismodell ermöglicht Ihnen die Anpassung an Ihre spezifischen Workloadanforderungen. Wählen Sie aus, wie viele Kerne, welche VM-SKU, Arbeitsspeichergröße und Datenträger-Speicherplatzgröße Sie benötigen.

## <a name="next-steps"></a>Nächste Schritte

Erste Schritte mit einem unserer Schnellstarts:

* [Erstellen einer Azure Managed Instance-Instanz für Apache Cassandra-Cluster im Azure-Portal (Vorschau)](create-cluster-portal.md)
* [Bereitstellen eines verwalteten Apache Spark-Clusters mit Azure Databricks](deploy-cluster-databricks.md)
* [Verwalten von Azure Managed Instance for Apache Cassandra-Ressourcen mit der Azure CLI](manage-resources-cli.md)
