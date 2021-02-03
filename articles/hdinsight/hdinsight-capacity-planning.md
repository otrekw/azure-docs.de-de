---
title: Clusterkapazitätsplanung in Azure HDInsight
description: Identifizieren wichtiger Fragen zur Planung der Kapazität und Leistung eines Azure HDInsight-Clusters.
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 05/07/2020
ms.openlocfilehash: 45cfa90f2156dba87cbec2b6313bc24e5d030572
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2021
ms.locfileid: "98933222"
---
# <a name="capacity-planning-for-hdinsight-clusters"></a>Kapazitätsplanung für HDInsight-Cluster

Planen Sie vor der Bereitstellung eines HDInsight-Clusters die vorgesehene Clusterkapazität, indem Sie die erforderliche Leistung und Skalierung bestimmen. Diese Planung trägt zur Optimierung von Nutzbarkeit und Kosten bei. Einige Entscheidungen hinsichtlich der Clusterkapazität können nach der Bereitstellung nicht mehr geändert werden. Wenn sich die Leistungsparameter ändern, kann ein Cluster deinstalliert und ohne Verlust von gespeicherten Daten neu erstellt werden.

Die wichtigsten Fragen zur Kapazitätsplanung sind:

* In welcher geografischen Region sollten Sie Ihren Cluster bereitstellen?
* Wie viel Speicher benötigen Sie?
* Welchen Clustertyp sollten Sie bereitstellen?
* Welche VM-Größe und welchen VM-Typ sollten die Clusterknoten verwenden?
* Wie viele Workerknoten sollte Ihr Cluster besitzen?

## <a name="choose-an-azure-region"></a>Auswahl einer Azure-Region

Die Azure-Region bestimmt, wo Ihr Cluster physisch bereitgestellt wird. Um die Latenz der Lese- und Schreibvorgänge zu minimieren, sollte der Cluster sich in der Nähe Ihrer Daten befinden.

HDInsight ist in vielen Azure-Regionen verfügbar. Die nächstgelegene Region finden Sie unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/?products=hdinsight).

## <a name="choose-storage-location-and-size"></a>Auswahl von Speicherort und -größe

### <a name="location-of-default-storage"></a>Speicherort des Standardspeichers

Der Standardspeicher, ein Azure Storage-Konto oder Azure Data Lake Storage, muss sich am gleichen Standort wie der Cluster befinden. Azure Storage ist an allen Speicherorten verfügbar. Data Lake Storage Gen1 ist in einigen Regionen verfügbar. Welche das sind, erfahren Sie unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/?products=storage).

### <a name="location-of-existing-data"></a>Speicherort der vorhandenen Daten

Wenn Sie ein vorhandenes Speicherkonto oder Data Lake-Storage als Standardspeicher für Ihren Cluster verwenden möchten, müssen Sie Ihren Cluster am gleichen Standort bereitstellen.

### <a name="storage-size"></a>Speichergröße

Für einen bereitgestellten Cluster können Sie zusätzliche Azure Storage-Konten anfügen oder auf andere Data Lake Storages zugreifen. Alle Ihre Speicherkonten müssen sich an demselben Speicherort wie Ihr Cluster befinden. Ein Data Lake Storage kann sich an einem anderen Standort befinden, auch wenn große Entfernungen zu einer gewissen Wartezeit führen können.

Für Azure Storage gelten einige [Kapazitätsgrenzen](../azure-resource-manager/management/azure-subscription-service-limits.md#storage-limits), während Data Lake Storage Gen1 nahezu unbegrenzt ist.

Ein Cluster kann auf eine Kombination verschiedener Speicherkonten zugreifen. Typische Beispiele hierfür sind:

* Die Menge der Daten überschreitet wahrscheinlich die Speicherkapazität eines einzelnen Blobspeichercontainers.
* Die Zugriffsrate könnte beim Zugriff auf den Blobcontainer den Schwellenwert überschreiten, bei dem eine Drosselung auftritt.
* Sie möchten Daten bearbeiten, die Sie bereits in einen Blobcontainer hochgeladen haben, der für den Cluster verfügbar ist.
* Sie möchten verschiedene Teile des Speichers aus Sicherheitsgründen, oder um die Administration zu vereinfachen, isolieren.

Verwenden Sie nur einen Container pro Speicherkonto, um die Leistung zu verbessern.

## <a name="choose-a-cluster-type"></a>Auswahl eines Clustertyps

Der Clustertyp bestimmt die Workload, für deren Ausführung Ihr HDInsight-Cluster konfiguriert ist. Zu den Typen gehören [Apache Hadoop](./hadoop/apache-hadoop-introduction.md), [Apache Storm](./storm/apache-storm-overview.md), [Apache Kafka](./kafka/apache-kafka-introduction.md) oder [Apache Spark](./spark/apache-spark-overview.md). Eine detaillierte Beschreibung der verfügbaren Clustertypen finden Sie unter [Einführung in Azure HDInsight](hdinsight-overview.md#cluster-types-in-hdinsight). Jeder Clustertyp verfügt über eine bestimmte Bereitstellungstopologie, die Anforderungen an die Größe und Anzahl der Knoten enthält.

## <a name="choose-the-vm-size-and-type"></a>Auswahl von Größe und Typ des virtuellen Computers

Jeder Clustertyp hat einen Satz von Knotentypen, und jeder Knotentyp hat bestimmte Optionen für VM-Größe und -Typ.

Um die optimale Clustergröße für Ihre Anwendung zu ermitteln, können Sie einen Vergleichstest für die Clusterkapazität durchführen und die Größe wie angegeben heraufsetzen. Sie können z.B. eine simulierte Workload oder eine *Testabfrage* verwenden. Führen Sie die simulierten Workloads in Clustern mit unterschiedlicher Größe aus. Erhöhen Sie allmählich die Größe, bis die beabsichtigte Leistung erreicht ist. Eine Testabfrage kann in regelmäßigen Abständen zwischen anderen Produktionsabfragen eingefügt werden, um zu zeigen, ob der Cluster über genügend Ressourcen verfügt.

Weitere Informationen zum Auswählen der richtigen VM-Familie für ihre Workload finden Sie unter [Auswählen der richtigen VM-Größe für Ihren Azure HDInsight-Cluster](hdinsight-selecting-vm-size.md).

## <a name="choose-the-cluster-scale"></a>Auswahl der Clusterskalierung

Die Skalierung eines Clusters wird durch die Menge seiner VM-Knoten bestimmt. Für alle Clustertypen gibt es Knotentypen mit bestimmter Skalierung, und Knotentypen, die horizontales Hochskalieren unterstützen. Beispielsweise könnte ein Cluster genau drei [Apache ZooKeeper](https://zookeeper.apache.org/)-Knoten oder zwei Hauptknoten erfordern. Workerknoten, die eine verteilte Datenverarbeitung praktizieren, profitieren von den zusätzlichen Workerknoten.

Abhängig von Ihrem Clustertyp führt eine Erhöhung der Anzahl der Workerknoten zu zusätzlicher Rechenkapazität (z. B. mehr Kerne). Mehr Knoten erhöhen den Gesamtspeicherbedarf des gesamten Clusters, um die In-Memory-Speicherung der zu verarbeitenden Daten zu unterstützen. Wie bei der Wahl von VM-Größe und -Typ wird die Auswahl der richtigen Clustergröße in der Regel empirisch erreicht. Verwenden Sie simulierte Workloads oder Testabfragen.

Sie können Ihren Cluster aufskalieren, um Spitzenlastanforderungen zu erfüllen. Dann skalieren Sie ihn wieder herunter, wenn diese zusätzlichen Knoten nicht mehr benötigt werden. Das [Autoskalierungsfeature](hdinsight-autoscale-clusters.md) ermöglicht die automatische Skalierung Ihres Clusters auf der Grundlage vorab festgelegter Metriken und Zeitangaben. Weitere Informationen zur manuellen Clusterskalierung finden Sie unter [Skalieren von HDInsight-Clustern](hdinsight-scaling-best-practices.md).

### <a name="cluster-lifecycle"></a>Clusterlebenszyklus

Die Lebensdauer des Clusters wird Ihnen in Rechnung gestellt. Wenn Sie Ihren Cluster nur zu bestimmten Zeiten benötigen, können Sie [mithilfe von Azure Data Factory bedarfsgesteuerte Cluster erstellen](hdinsight-hadoop-create-linux-clusters-adf.md). Sie können auch PowerShell-Skripts erstellen, die Ihren Cluster bereitstellen und löschen, und diese Skripts mit [Azure Automation](https://azure.microsoft.com/services/automation/) planen.

> [!NOTE]  
> Wenn ein Cluster gelöscht wird, wird sein standardmäßiger Hive-Metastore ebenfalls gelöscht. Um den Metastore für die nächste Clusterneuerstellung beizubehalten, verwenden Sie einen externen Metadatenspeicher, z.B. Azure Database oder [Apache Oozie](https://oozie.apache.org/).

### <a name="isolate-cluster-job-errors"></a>Isolieren von Clusterauftragsfehlern

Manchmal können Fehler aufgrund paralleler Ausführung von Mehrfachzuordnungen auftreten und Komponenten auf einem Cluster mit mehreren Knoten reduzieren. Versuchen Sie es mit verteilten Tests, um das Problem einzugrenzen. Führen Sie mehrere Aufträge gleichzeitig auf einem einzelnen Workerknotencluster aus. Erweitern Sie dann diesen Ansatz, um mehrere Aufträge gleichzeitig auf Clustern mit mehr als einem Knoten auszuführen. Um einen HDInsight-Cluster mit einem einzelnen Knoten in Azure zu erstellen, verwenden Sie die Option *`Custom(size, settings, apps)`* , und verwenden Sie für die Bereitstellung eines neuen Clusters im Portal den Wert „1“ für *Anzahl von Workerknoten* im Abschnitt **Clustergröße**.

## <a name="quotas"></a>Kontingente

Weitere Informationen zur Verwaltung von Abonnementkontingenten finden Sie unter [Anfordern einer Erhöhung von Kontingenten](quota-increase-request.md).

## <a name="next-steps"></a>Nächste Schritte

* [Einrichten von Clustern in HDInsight mit Apache Hadoop, Spark, Kafka usw.](hdinsight-hadoop-provision-linux-clusters.md): Erfahren Sie, wie Cluster in HDInsight eingerichtet und konfiguriert werden können.
* [Überwachen der Clusterleistung](hdinsight-key-scenarios-to-monitor.md): Lernen Sie wichtige Szenarien zur Überwachung des HDInsight-Clusters kennen, die sich auf die Kapazität des Clusters auswirken können.
