---
title: Erhöhen der Skalierung für Apache Kafka – Azure HDInsight
description: Erfahren Sie, wie Sie verwaltete Datenträger für Apache Kafka-Cluster in Azure HDInsight konfigurieren, um die Skalierbarkeit zu erhöhen.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 12/09/2019
ms.openlocfilehash: 586094f9e6ee696487064dcfea68af0bfc26c1ae
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88649107"
---
# <a name="configure-storage-and-scalability-for-apache-kafka-on-hdinsight"></a>Konfigurieren von Speicher und Skalierbarkeit für Apache Kafka in HDInsight

Erfahren Sie, wie Sie die Anzahl der von [Apache Kafka](https://kafka.apache.org/) in HDInsight verwendeten verwalteten Datenträgern konfigurieren.

Kafka in HDInsight verwendet den lokalen Datenträger der virtuellen Computer im HDInsight-Cluster. Da Kafka sehr E/A-intensiv ist, wird [Azure Managed Disks](../../virtual-machines/managed-disks-overview.md) verwendet, um einen hohen Durchsatz zu ermöglichen und mehr Speicher pro Knoten bereitzustellen. Wenn herkömmliche virtuelle Festplatten (VHD) für Kafka verwendet werden, ist jeder Knoten auf 1 TB beschränkt. Mit verwalteten Datenträgern können Sie mehrere Datenträger verwenden, um für jeden Knoten im Cluster 16 TB zu erzielen.

Das folgende Diagramm zeigt einen Vergleich zwischen Kafka in HDInsight vor verwalteten Datenträgern und Kafka in HDInsight mit verwalteten Datenträgern:

![Kafka mit Architektur für verwaltete Datenträger](./media/apache-kafka-scalability/kafka-with-managed-disks-architecture.png)

## <a name="configure-managed-disks-azure-portal"></a>Konfigurieren von verwalteten Datenträgern: Azure-Portal

1. Führen Sie die Schritte unter [Erstellen eines HDInsight-Clusters](../hdinsight-hadoop-create-linux-clusters-portal.md) aus, um die übliche Vorgehensweise zum Erstellen eines Clusters mit dem Portal zu verstehen. Führen Sie den Vorgang zum Erstellen eines Portals nicht aus.

2. Verwenden Sie im Abschnitt **Konfiguration und Preise** das Feld __Knotenanzahl__, um die Anzahl der Datenträger zu konfigurieren.

    > [!NOTE]  
    > Der Typ des verwalteten Datenträgers kann entweder __Standard__ (HDD) oder __Premium__ (SSD) sein. Premium-Datenträger werden mit virtuellen Computern der DS- und GS-Serie verwendet. Alle anderen virtuellen Computertypen verwenden den Standardtyp.

    ![Abschnitt „Clustergröße“ mit hervorgehobenen Datenträgern pro Workerknoten](./media/apache-kafka-scalability/azure-portal-cluster-configuration-pricing-kafka-disks.png)

## <a name="configure-managed-disks-resource-manager-template"></a>Konfigurieren von verwalteten Datenträgern: Resource Manager-Vorlage

Um die Anzahl der von den Workerknoten in einem Kafka-Cluster verwendeten Datenträger zu steuern, verwenden Sie den folgenden Abschnitt der Vorlage:

```json
"dataDisksGroups": [
    {
        "disksPerNode": "[variables('disksPerWorkerNode')]"
    }
    ],
```

Sie finden eine vollständige Vorlage, die das Konfigurieren von verwalteten Datenträgern veranschaulicht, unter [https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json](https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Verwendung von Apache Kafka in HDInsight finden Sie in den folgenden Dokumenten:

* [Verwenden von MirrorMaker zum Replizieren von Apache Kafka in HDInsight](apache-kafka-mirroring.md)
* [Verwenden von Apache Storm mit Apache Kafka in HDInsight](../hdinsight-apache-storm-with-kafka.md)
* [Verwenden von Apache Spark mit Apache Kafka in HDInsight](../hdinsight-apache-spark-with-kafka.md)
* [Herstellen einer Verbindung mit Apache Kafka über ein virtuelles Azure-Netzwerk](apache-kafka-connect-vpn-gateway.md)

* [HDInsight-Blog über verwaltete Datenträger mit Apache Kafka](https://azure.microsoft.com/blog/announcing-public-preview-of-apache-kafka-on-hdinsight-with-azure-managed-disks/)
