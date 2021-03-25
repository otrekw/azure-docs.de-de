---
title: Skalieren von HiveServer2 in Azure HDInsight
description: Skalieren Sie HiveServer2 horizontal in Azure HDInsight-Clustern mithilfe von Edgeknoten zum Erhöhen der Fehlertoleranz und Verfügbarkeit.
services: hdinsight
ms.service: hdinsight
ms.topic: conceptual
ms.reviewer: hrasheed-msft
ms.author: kecheung
author: kcheeeung
ms.date: 08/12/2020
ms.openlocfilehash: d0a0df4791492c1c9f0d600630d723024c46c1b8
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "88226910"
---
# <a name="scale-hiveserver2-on-azure-hdinsight-clusters-for-high-availability"></a>Skalieren von HiveServer2 in Azure HDInsight für Hochverfügbarkeit

Erfahren Sie, wie Sie zusätzliche HiveServer2 in Ihrem Cluster bereitstellen, um die Verfügbarkeit und Lastverteilung zu erhöhen. Wenn eine Erhöhung der Hauptknotengröße nicht erwünscht ist, können Sie auch Edgeknoten zum Bereitstellen von HiveServer2 verwenden. 

> [!NOTE]
> Abhängig von Ihrer Nutzung kann eine Erhöhung der Anzahl der HiveServer2 die Anzahl der Verbindungen zum Hive-Metastore erhöhen. Stellen Sie auch sicher, dass Ihre Azure SQL-Datenbank die richtige Größe aufweist.

## <a name="prerequisites"></a>Voraussetzungen

Sie müssen den folgenden Artikel verstehen, um diesen Leitfaden verwenden zu können:
- [Verwenden leerer Edgeknoten in Apache Hadoop-Clustern in HDInsight](hdinsight-apps-use-edge-node.md)

## <a name="install-hiveserver2"></a>Installieren von HiveServer2

In diesem Abschnitt installieren Sie einen zusätzlichen HiveServer2 auf Ihren Zielhosts.

1. Öffnen Sie Ambari in Ihrem Browser, und klicken Sie auf den Zielhost.

:::image type="content" source="media/hdinsight-apps-install-hiveserver2/hdinsight-install-hiveserver2-a.png" alt-text="Hostmenü von Ambari.":::

2. Klicken Sie auf die Schaltfläche „Hinzufügen“ und dann auf „HiveServer2“.

:::image type="content" source="media/hdinsight-apps-install-hiveserver2/hdinsight-install-hiveserver2-b.png" alt-text="Bereich des Hosts zum Hinzufügen von HiveServer2.":::

3. Bestätigen Sie den Vorgang und der Prozess wird ausgeführt. Wiederholen Sie 1-3 für alle gewünschten Hosts.

4. Wenn Sie die Installation beendet haben, starten Sie alle Dienste mit veralteten Konfigurationen neu, und starten Sie HiveServer2.

:::image type="content" source="media/hdinsight-apps-install-hiveserver2/hdinsight-install-hiveserver2-c.png" alt-text="Bereich zum Starten von HiveServer2.":::

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie Sie HiveServer2 auf Ihrem Cluster installieren. Weitere Informationen zu Edgeknoten und Anwendungen finden Sie in den folgenden Artikeln:

* [Installieren von Edgeknoten](hdinsight-apps-use-edge-node.md): Erfahren Sie, wie Sie einen Edgeknoten in Ihrem HDInsight-Cluster installieren.
* [Installieren von HDInsight-Anwendungen](hdinsight-apps-install-applications.md): Hier erfahren Sie, wie Sie eine HDInsight-Anwendung in Ihren Clustern installieren.
* [Azure SQL DTU-Verbindungsbeschränkungen](../azure-sql/database/resource-limits-dtu-single-databases.md): Erfahren Sie mehr über Grenzwerte für Azure SQL-Datenbanken mithilfe der DTU.
* [Azure SQL vCore-Verbindungsbeschränkungen](../azure-sql/database/resource-limits-vcore-elastic-pools.md): Erfahren Sie mehr über Grenzwerte für Azure SQL-Datenbanken mithilfe von vCores.
