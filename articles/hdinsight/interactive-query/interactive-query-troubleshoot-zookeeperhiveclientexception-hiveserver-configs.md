---
title: Fehler durch Apache Hive Zeppelin-Interpreter – Azure HDInsight
description: Der Apache Zeppelin Hive-JDBC-Interpreter zeigt auf die falsche URL in Azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 07/30/2019
ms.openlocfilehash: 04288fdde74da705f6127c2444b0dc7ac7a2a8dd
ms.sourcegitcommit: 91fdedcb190c0753180be8dc7db4b1d6da9854a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/17/2021
ms.locfileid: "112290537"
---
# <a name="scenario-apache-hive-zeppelin-interpreter-gives-a-zookeeper-error-in-azure-hdinsight"></a>Szenario: Apache Hive Zeppelin-Interpreter löst einen ZooKeeper-Fehler in Azure HDInsight aus

Dieser Artikel beschreibt Schritte zur Problembehandlung sowie mögliche Lösungen für Probleme bei der Verwendung von Interactive Query-Komponenten in Azure HDInsight-Clustern.

## <a name="issue"></a>Problem

In einem Apache Hive LLAP-Cluster gibt der Zeppelin-Interpreter beim Versuch, eine Abfrage auszuführen, die folgende Fehlermeldung aus:

```
java.sql.SQLException: org.apache.hive.jdbc.ZooKeeperHiveClientException: Unable to read HiveServer2 configs from ZooKeeper
```

## <a name="cause"></a>Ursache

Der Zeppelin Hive-JDBC-Interpreter zeigt auf die falsche URL.

## <a name="resolution"></a>Lösung

1. Navigieren Sie zur Hive-Komponentenzusammenfassung, und kopieren Sie die „Hive JDBC Url“ in die Zwischenablage.

1. Navigieren Sie zu `https://clustername.azurehdinsight.net/zeppelin/#/interpreter`.

1. Bearbeiten Sie die JDBC-Einstellungen: Aktualisieren Sie den Wert von hive.url in die in Schritt 1 kopierte Hive JDBC URL.

1. Speichern Sie die URL, und wiederholen Sie die Abfrage.

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [troubleshooting next steps](../includes/hdinsight-troubleshooting-next-steps.md)]