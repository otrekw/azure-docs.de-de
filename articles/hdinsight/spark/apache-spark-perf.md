---
title: Optimieren von Spark-Aufträgen im Hinblick auf die Leistung – Azure HDInsight
description: Dieser Artikel zeigt allgemeine Strategien zum Optimieren der Leistung von Apache Spark-Clustern in Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/21/2020
ms.openlocfilehash: f92a351087670ce0b37921a496eabfa883a3b1fc
ms.sourcegitcommit: a9784a3fd208f19c8814fe22da9e70fcf1da9c93
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/22/2020
ms.locfileid: "83780105"
---
# <a name="optimize-apache-spark-jobs-in-hdinsight"></a>Optimieren von Apache Spark-Aufträgen in HDInsight

Dieser Artikel bietet eine Übersicht über die Strategien zur Optimierung der Apache Spark-Arbeitsplätze für Azure HDInsight.

## <a name="overview"></a>Übersicht

Die Leistung Ihrer Apache Spark-Aufträge hängt von mehreren Faktoren ab. Zu diesen Leistungsfaktoren gehören: wie Ihre Daten gespeichert werden, wie der Cluster konfiguriert ist und welche Operationen bei der Verarbeitung der Daten verwendet werden.

Zu den häufigen Herausforderungen, mit denen Sie möglicherweise konfrontiert werden, gehören Speicherbeschränkungen aufgrund von nicht ordnungsgemäß dimensionierten Executors, zeitintensive Vorgänge und Tasks, die zu kartesischen Operationen führen.

Es gibt auch verschiedene Strategien, die Ihnen helfen können, diese Herausforderungen zu bewältigen, z. B. das Zwischenspeichern und das Zulassen der Datenschiefe.

In jedem der folgenden Artikel können Sie allgemeine Herausforderungen und Lösungen für einen anderen Aspekt der Spark-Optimierung finden.

* [Optimieren des Datenspeichers](optimize-data-storage.md)
* [Optimieren der Datenverarbeitung](optimize-data-processing.md)
* [Optimieren der Arbeitsspeicherauslastung](optimize-memory-usage.md)
* [Optimieren der Clusterkonfiguration](optimize-cluster-configuration.md)

## <a name="next-steps"></a>Nächste Schritte

* [Debuggen von Apache Spark-Aufträgen, die in HDInsight ausgeführt werden](apache-spark-job-debugging.md)
* [Verwalten von Ressourcen für den Apache Spark-Cluster unter HDInsight](apache-spark-resource-manager.md)
* [Konfigurieren von Apache Spark-Einstellungen](apache-spark-settings.md)
