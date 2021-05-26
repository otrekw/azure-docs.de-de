---
title: Apache Hive-Verbindungen mit Apache ZooKeeper – Azure HDInsight
description: Zugriff auf die Apache Hive-Ansicht aufgrund von Apache Zookeeper-Problemen in Azure HDInsight nicht möglich
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 07/30/2019
ms.openlocfilehash: 475f717413252b69e3861ca3a7210ec72b6ac581
ms.sourcegitcommit: a9f131fb59ac8dc2f7b5774de7aae9279d960d74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110191396"
---
# <a name="scenario-apache-hive-fails-to-establish-a-connection-to-apache-zookeeper-in-azure-hdinsight"></a>Szenario: Von Apache Hive kann keine Verbindung mit Apache ZooKeeper in Azure HDInsight hergestellt werden.

Dieser Artikel beschreibt Schritte zur Problembehandlung sowie mögliche Lösungen für Probleme bei der Verwendung von Interactive Query-Komponenten in Azure HDInsight-Clustern.

## <a name="issue"></a>Problem

Auf die Hive-Ansicht kann nicht zugegriffen werden, und die Protokolle in `/var/log/hive` enthalten einen Fehler wie den folgenden:

```
ERROR [Curator-Framework-0]: curator.ConnectionState (ConnectionState.java:checkTimeouts(200)) - Connection timed out for connection string (<zookeepername1>.cloud.wbmi.com:2181,<zookeepername2>.cloud.wbmi.com:2181,<zookeepername3>.cloud.wbmi.com:2181) and timeout (15000) / elapsed (21852)
```

## <a name="cause"></a>Ursache

Von Hive kann möglicherweise keine Verbindung mit ZooKeeper hergestellt werden, was dazu führt, dass die Hive-Ansicht nicht gestartet werden kann.

## <a name="resolution"></a>Lösung

1. Überprüfen Sie die Integrität des ZooKeeper-Diensts.

1. Überprüfen Sie, ob der ZooKeeper-Dienst über einen ZNode-Eintrag für „HiveServer2“ verfügt. Der Wert ist falsch oder nicht vorhanden.

    ```
    /usr/hdp/2.6.2.25-1/zookeeper/bin/zkCli.sh -server <zookeepername1>
    [zk: <zookeepername1>(CONNECTED) 0] ls /hiveserver2-hive2
    ```

1. Starten Sie zum Wiederherstellen der Konnektivität die ZooKeeper-Knoten und „HiveServer2“ neu.

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [troubleshooting next steps](../../../includes/hdinsight-troubleshooting-next-steps.md)]