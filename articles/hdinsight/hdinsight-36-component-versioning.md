---
title: 'Apache Hadoop-Komponenten und -Versionen: Azure HDInsight 3.6'
description: Lernen Sie die Apache Hadoop-Komponenten und -Versionen in Azure HDInsight 3.6 kennen.
ms.service: hdinsight
ms.topic: conceptual
author: deshriva
ms.author: deshriva
ms.date: 02/08/2021
ms.openlocfilehash: 32b287c3d7b1974db5a079d1ee84aaafad3faed7
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105727687"
---
# <a name="hdinsight-36-component-versions"></a>HDInsight 3.6-Komponentenversionen

In diesem Artikel erfahren Sie mehr über die Komponenten und Versionen der Apache Hadoop-Umgebung in Azure HDInsight 3.6.

## <a name="support-for-hdinsight-36"></a>Support für HDInsight 3.6

Ab dem 1. Juli 2021 bietet Microsoft Basic-Support für bestimmte HDI 3.6-Clustertypen an.
In der folgenden Tabelle ist der Supportzeitraum für HDInsight 3.6-Clustertypen aufgeführt.

| Clustertyp                    | Frameworkversion | Ablaufdatum für Standard-Support       | Ablaufdatum für Basic-Support | Deaktivierungstermin |
|---------------------------------|-------------------|-----------------------------------|------------------------------|-----------------|
| HDInsight 3.6 Hadoop            | 2.7.3             | 30. Juni 2021                     | 3\. April 2022                | 4\. April 2022 |
| HDInsight 3.6 Spark             | 2.3               | 30. Juni 2021                     | 3\. April 2022                | 4\. April 2022 |
| HDInsight 3.6 Kafka             | 1.1               | 30. Juni 2021                     | 3\. April 2022                | 4\. April 2022 |
| HDInsight 3.6 HBase             | 1.1               | 30. Juni 2021                     | 3\. April 2022                | 4\. April 2022 |
| HDInsight 3.6 Interactive Query | 2.1               | 30. Juni 2021                     | 3\. April 2022                | 4\. April 2022 |
| HDInsight 3.6 Storm             | 1.1               | 30. Juni 2021                     | 3\. April 2022                | 4\. April 2022 |
| HDInsight 3.6 ML Services      | 9.3               | -                                 | -                            | 31. Dezember 2020 |
| HDInsight 3.6 Spark             | 2.2               | -                                 | -                            | 30. Juni 2020 |
| HDInsight 3.6 Spark             | 2.1               | -                                 | -                            | 30. Juni 2020 |
| HDInsight 3.6 Kafka             | 1.0               | -                                 | -                            | 30. Juni 2020 |

## <a name="apache-components-available-with-hdinsight-version-36"></a>Mit HDInsight Version 3.6 verfügbare Apache-Komponenten

Die HDInsight 3.6 zugeordneten OSS-Komponentenversionen sind in der folgenden Tabelle aufgeführt.

| Komponente              | HDInsight 3.6 (Standard)     |
|------------------------|-----------------------------|
| Apache Hadoop und YARN | 2.7.3                       |
| Apache Tez             | 0.7.0                       |
| Apache Pig             | 0.16.0                      |
| Apache Hive            | (2.1.0 bei ESP Interactive Query) |
| Apache Tez Hive2       | 0.8.4                       |
| Apache Ranger          | 0.7.0                       |
| Apache HBase           | 1.1.2                       |
| Apache Sqoop           | 1.4.6                       |
| Apache Oozie           | 4.2.0                       |
| Apache Zookeeper       | 3.4.6                       |
| Apache Storm           | 1.1.0                       |
| Apache Mahout          | 0.9.0+                      |
| Apache Phoenix         | 4.7.0                       |
| Apache Spark           | 2.3.2.                      |
| Apache Livy            | 0.4.                        |
| Apache Kafka           | 1.1                         |
| Apache Ambari          | 2.6.0                       |
| Apache Zeppelin        | 0.7.3                       |
| Mono                   | 4.2.1                       |

## <a name="next-steps"></a>Nächste Schritte

- [Einrichten von Clustern in HDInsight mit Apache Hadoop, Spark, Kafka usw.](hdinsight-hadoop-provision-linux-clusters.md)
- [Enterprise-Sicherheitspaket](./enterprise-security-package.md)
- [Verwenden eines Windows-Computers mit Apache Hadoop in HDInsight](hdinsight-hadoop-windows-tools.md)
