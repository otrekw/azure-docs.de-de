---
title: Migrieren von Clustern zu einer neueren Version
titleSuffix: Azure HDInsight
description: Erfahren Sie mehr über Richtlinien für das Migrieren Ihres Azure HDInsight-Clusters zu einer neueren Version.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 01/31/2020
ms.openlocfilehash: f7198aeff5e9ef6d37e29c2336dc38e4eec0dda1
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77023972"
---
# <a name="migrate-hdinsight-cluster-to-a-newer-version"></a>Migrieren eines HDInsight-Clusters zu einer neueren Version

Um die neuesten HDInsight-Funktionen nutzen zu können, wird empfohlen, HDInsight-Cluster regelmäßig zur neuesten Version zu migrieren. HDInsight unterstützt keine direkten Upgrades, bei denen ein vorhandener Cluster auf eine neuere Komponentenversion aktualisiert wird. Sie müssen einen neuen Cluster mit der gewünschten Komponenten- und Plattformversion erstellen und dann die Anwendungen migrieren, um den neuen Cluster zu verwenden. Befolgen Sie die nachstehenden Leitlinien zum Migrieren Ihrer HDInsight-Clusterversionen.

> [!NOTE]  
> Informationen zu unterstützten HDInsight-Versionen finden Sie unter [HDInsight-Komponentenversionen](hdinsight-component-versioning.md#supported-hdinsight-versions).

## <a name="migration-tasks"></a>Migrationsaufgaben

Der Workflow für ein Upgrade eines HDInsight-Clusters ist wie folgt.
![HDInsight-Upgradeworkflow – Diagramm](./media/hdinsight-upgrade-cluster/upgrade-workflow-diagram.png)

1. Lesen Sie jeden Abschnitt dieses Dokuments, um zu verstehen, welche Änderungen möglicherweise erforderlich sind, wenn Sie Ihren HDInsight-Cluster aktualisieren.
2. Erstellen Sie einen Cluster als Test-/Qualitätssicherungsumgebung. Weitere Informationen zur Erstellung eines Clusters finden Sie unter [Erstellen von Linux-basierten Hadoop-Clustern in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).
3. Kopieren Sie vorhandene Aufträge, Datenquellen und Senken in die neue Umgebung.
4. Führen Sie Überprüfungstests durch, um sicherzustellen, dass Ihre Aufträge im neuen Cluster wie erwartet funktionieren.

Nachdem Sie überprüft haben, dass alles wie erwartet funktioniert, planen Sie für die Migration eine Downtime. Führen Sie während dieser Ausfallzeit die folgenden Aktionen durch:

1. Sichern Sie alle Daten, die vorübergehend lokal auf dem Clusterknoten gespeichert sind. zum Beispiel direkt auf einem Hauptknoten gespeicherte Daten.
1. [Löschen Sie den vorhandenen Cluster.](./hdinsight-delete-cluster.md)
1. Erstellen Sie einen Cluster im gleichen VNET-Subnetz mit der aktuellen (oder einer unterstützten) HDI-Version, die den gleichen Standarddatenspeicher wie der vorherige Cluster verwendet. Dadurch wird dem neuen Cluster ermöglicht, weiterhin mit Ihren vorhandenen Produktionsdaten zu arbeiten.
1. Importieren Sie alle vorübergehenden Daten, die Sie gesichert haben.
1. Starten Sie Aufträge/Verarbeiten Sie weiterhin mithilfe des neuen Clusters.

## <a name="workload-specific-guidance"></a>Workloadspezifischer Leitfaden

Die folgenden Dokumente enthalten Anleitungen zum Migrieren bestimmter Workloads:

* [Migrieren von HBase](./hbase/apache-hbase-migrate-new-version.md)
* [Migrieren von Kafka](./kafka/migrate-versions.md)
* [Migrieren von Hive/Interactive Query](./interactive-query/apache-hive-migrate-workloads.md)

## <a name="backup-and-restore"></a>Sichern und Wiederherstellen

Weitere Informationen zur Datenbanksicherung und -wiederherstellung finden Sie unter [Wiederherstellen einer Azure SQL-Datenbank-Instanz mit automatisierten Datenbanksicherungen](../sql-database/sql-database-recovery-using-backups.md).

## <a name="next-steps"></a>Nächste Schritte

* [Erfahren Sie, wie Sie Linux-basierte HDInsight-Cluster erstellen](hdinsight-hadoop-provision-linux-clusters.md)
* [Herstellen von Verbindungen mit HDInsight per SSH](hdinsight-hadoop-linux-use-ssh-unix.md)
* [Verwalten eines Linux-basierten Clusters mithilfe von Apache Ambari](hdinsight-hadoop-manage-ambari.md)
* [Versionshinweise zu HDInsight](./hdinsight-version-release.md)
