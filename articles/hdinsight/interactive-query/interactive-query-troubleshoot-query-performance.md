---
title: Schlechte Leistung bei Apache Hive LLAP-Abfragen in Azure HDInsight
description: Abfragen in Apache Hive LLAP werden in Azure HDInsight langsamer als erwartet ausgeführt.
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 07/30/2019
ms.openlocfilehash: 80fe51c2131da5a6fcb69eb5526b5d4f33a45e03
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98930905"
---
# <a name="scenario-poor-performance-in-apache-hive-llap-queries-in-azure-hdinsight"></a>Szenario: Schlechte Leistung bei Apache Hive LLAP-Abfragen in Azure HDInsight

Dieser Artikel beschreibt Schritte zur Problembehandlung sowie mögliche Lösungen für Probleme bei der Verwendung von Interactive Query-Komponenten in Azure HDInsight-Clustern.

## <a name="issue"></a>Problem

Die Standardclusterkonfigurationen sind für Ihre Workload nicht ausreichend optimiert. Abfragen in Hive LLAP werden langsamer als erwartet ausgeführt.

## <a name="cause"></a>Ursache

Dies kann aufgrund einer Vielzahl von Gründen geschehen.

## <a name="resolution"></a>Lösung

LLAP ist für Abfragen optimiert, die Joins und Aggregate einschließen. Abfragen wie die folgende funktionieren in einem interaktiven Hive-Cluster nicht besonders gut:

```
select * from table where column = "columnvalue"
```

Legen Sie die folgenden Konfigurationen fest, um die Leistung von Punktabfragen in Hive LLAP zu verbessern:

```
hive.llap.io.enabled=false; (disable LLAP IO)
hive.optimize.index.filter=false; (disable ORC row index)
hive.exec.orc.split.strategy=BI; (to avoid recombining splits)
```

Sie können mit der folgenden Konfigurationsänderung auch die Nutzung des LLAP-Caches erhöhen, um die Leistung zu verbessern:

```
hive.fetch.task.conversion=none
```

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [troubleshooting next steps](../../../includes/hdinsight-troubleshooting-next-steps.md)]