---
title: Verwendung von Apache Ambari in Azure HDInsight
description: In diesem Artikel wird die Verwendung von Apache Ambari in Azure HDInsight erläutert.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 02/05/2020
ms.openlocfilehash: 466c170985715be52a90d579c19ca23aefefe2e5
ms.sourcegitcommit: db2d402883035150f4f89d94ef79219b1604c5ba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2020
ms.locfileid: "77066001"
---
# <a name="apache-ambari-usage-in-azure-hdinsight"></a>Verwendung von Apache Ambari in Azure HDInsight

HDInsight verwendet Apache Ambari für die Bereitstellung und Verwaltung von Clustern. Ambari-Agents werden auf jedem Knoten ausgeführt: Hauptknoten, Workerknoten, Zookeeper-Knoten und Edgeknoten, sofern vorhanden. Der Ambari-Server wird nur auf dem Hauptknoten („hn0“ oder „hn1“) ausgeführt. Es kann immer nur eine Instanz des Ambari-Servers ausgeführt werden. Dies wird durch den HDInsight-Failovercontroller gesteuert. Wenn einer der Hauptknoten zum Zweck eines Neustarts oder für Wartungsmaßnamen heruntergefahren wurde, wird der andere Hauptknoten aktiv, und der Ambari-Server auf dem zweiten Hauptknoten wird gestartet.

Die gesamte Clusterkonfiguration sollte über die [Ambari-Benutzeroberfläche](./hdinsight-hadoop-manage-ambari.md) erfolgen, alle lokalen Änderungen werden beim Neustart des Knotens überschrieben.

## <a name="failover-controller-services"></a>Failovercontrollerdienste

Der HDInsight-Failovercontroller ist auch für die Aktualisierung der IP-Adresse des Hauptknotenhosts zuständig, die auf den aktuellen aktiven Hauptknoten zeigt. Alle Ambari-Agents sind so konfiguriert, dass sie den Zustand und Takt des Controllers an den Hauptknotenhost melden. Der Failovercontroller besteht aus einer Reihe von Diensten, die auf jedem Knoten im Cluster ausgeführt werden. Wenn diese Dienste nicht ausgeführt werden, funktioniert das Hauptknotenfailover möglicherweise nicht ordnungsgemäß, und Sie erhalten einen HTTP 502-Fehler, wenn Sie versuchen, auf den Ambari-Server zuzugreifen.

Eine Möglichkeit zum Überprüfen, welcher Hauptknoten aktiv ist, besteht darin, eine SSH-Verbindung mit einem der Knoten im Cluster herzustellen, dann `ping headnodehost` auszuführen und die IP-Adresse mit den Adressen der beiden Hauptknoten zu vergleichen.

Wenn die Failovercontrollerdienste nicht ausgeführt werden, erfolgt das Hauptknotenfailover möglicherweise nicht ordnungsgemäß, was dazu führen kann, dass der Ambari-Server nicht ausgeführt wird. Um zu überprüfen, ob die Failovercontrollerdienste ausgeführt werden, führen Sie Folgendes aus:

```bash
ps -ef | grep failover
```

## <a name="logs"></a>Protokolle

Auf dem aktiven Hauptknoten können Sie die Ambari-Serverprotokolle hier überprüfen:

```
/var/log/ambari-server/ambari-server.log
/var/log/ambari-server/ambari-server-check-database.log
```

Auf jedem anderen Knoten im Cluster können Sie die Ambari-Agent-Protokolle hier überprüfen:

```bash
/var/log/ambari-agent/ambari-agent.log
```

## <a name="service-start-sequences"></a>Dienststartsequenzen

Zum Zeitpunkt des Systemstarts werden die Dienste in dieser Reihenfolge gestartet:

1. Der HDInsight-Agent startet die Failovercontrollerdienste.
1. Die Failovercontrollerdienste starten die Ambari-Agents auf allen Knoten und den Ambari-Server auf dem aktiven Hauptknoten.

## <a name="ambari-database"></a>Ambari-Datenbank

HDInsight erstellt hinter den Kulissen eine SQL Azure-Datenbank als Datenbank für den Ambari-Server. Die standardmäßige [Dienstebene ist S0](../sql-database/sql-database-elastic-pool-scale.md).

Bei allen Clustern, die bei der Erstellung mehr als 16 Workerknoten umfassen, ist S2 die Dienstebene für die Datenbank.

## <a name="takeaway-points"></a>Die wichtigsten Punkte

Starten oder beenden Sie die Dienste für Ambari-Server oder Ambari-Agents niemals manuell, es sei denn, Sie versuchen, die Dienste zur Umgehung eines Problems neu zu starten. Um ein Failover zu erzwingen, können Sie den aktiven Hauptknoten neu starten.

Ändern Sie die Konfigurationsdateien auf den Clusterknoten niemals manuell – das wird auf der Ambari-Benutzeroberfläche erledigt.

## <a name="next-steps"></a>Nächste Schritte

* [Verwalten von HDInsight-Clustern mithilfe der Apache Ambari-Webbenutzeroberfläche](hdinsight-hadoop-manage-ambari.md)
* [Verwalten von HDInsight-Clustern mithilfe der Apache Ambari-REST-API](hdinsight-hadoop-manage-ambari-rest-api.md)

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Nutzen Sie den [Azure-Communitysupport](https://azure.microsoft.com/support/community/), um Antworten von Azure-Experten zu erhalten.

* Herstellen einer Verbindung mit [@AzureSupport](https://twitter.com/azuresupport), dem offiziellen Microsoft Azure-Konto zum Verbessern der Kundenfreundlichkeit. Verbinden der Azure-Community mit den richtigen Ressourcen: Antworten, Support und Experten.

* Sollten Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support**. Ausführlichere Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.
