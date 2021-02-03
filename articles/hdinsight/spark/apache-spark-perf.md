---
title: Optimieren von Spark-Aufträgen im Hinblick auf die Leistung – Azure HDInsight
description: Dieser Artikel zeigt allgemeine Strategien zum Optimieren der Leistung von Apache Spark-Clustern in Azure HDInsight.
ms.service: hdinsight
ms.topic: conceptual
ms.date: 08/21/2020
ms.custom: contperf-fy21q1
ms.openlocfilehash: 567fc2637538408d9727d07d3185a5b0e3cf20c5
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2021
ms.locfileid: "98929928"
---
# <a name="optimize-apache-spark-jobs-in-hdinsight"></a>Optimieren von Apache Spark-Aufträgen in HDInsight

Dieser Artikel bietet eine Übersicht über die Strategien zur Optimierung der Apache Spark-Arbeitsplätze für Azure HDInsight.

## <a name="overview"></a>Übersicht

Die Leistung Ihrer Apache Spark-Aufträge hängt von mehreren Faktoren ab. Zu diesen Leistungsfaktoren gehören: wie Ihre Daten gespeichert werden, wie der Cluster konfiguriert ist und welche Operationen bei der Verarbeitung der Daten verwendet werden.

Zu den häufigen Herausforderungen, mit denen Sie möglicherweise konfrontiert werden, gehören Speicherbeschränkungen aufgrund von nicht ordnungsgemäß dimensionierten Executors, zeitintensiven Vorgängen und Tasks, die zu kartesischen Operationen führen.

Es gibt allerdings zahlreiche Strategien, die Ihnen dabei helfen können, diese Herausforderungen zu meistern. Hierzu zählen beispielsweise das Zwischenspeichern und das Zulassen von Datenschiefe.

Die folgenden Artikel enthalten jeweils Informationen zu verschiedenen Aspekten der Spark-Optimierung:

* [Optimieren der Datenspeicherung für Apache Spark](optimize-data-storage.md)
* [Optimieren der Datenverarbeitung für Apache Spark](optimize-data-processing.md)
* [Optimieren der Speicherauslastung für Apache Spark](optimize-memory-usage.md)
* [Optimierung der Clusterkonfiguration](optimize-cluster-configuration.md)

## <a name="next-steps"></a>Nächste Schritte

* [Debuggen von Apache Spark-Aufträgen, die in HDInsight ausgeführt werden](apache-spark-job-debugging.md)
* [Verwalten von Ressourcen für den Apache Spark-Cluster unter HDInsight](apache-spark-resource-manager.md)
* [Konfigurieren von Apache Spark-Einstellungen](apache-spark-settings.md)
