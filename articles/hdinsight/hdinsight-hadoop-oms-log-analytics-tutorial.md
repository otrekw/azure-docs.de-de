---
title: Verwenden von Azure Monitor-Protokollen zum Überwachen von Azure HDInsight-Clustern
description: Erfahren Sie, wie Sie Azure Monitor-Protokolle zum Überwachen von Aufträgen verwenden, die in einem HDInsight-Cluster ausgeführt werden.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: seoapr2020
ms.date: 04/24/2020
ms.openlocfilehash: 41688792330214943eeb116dc4b5aaf7eebfeebf
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82192041"
---
# <a name="use-azure-monitor-logs-to-monitor-hdinsight-clusters"></a>Verwenden von Azure Monitor-Protokollen zum Überwachen von HDInsight-Clustern

In diesem Artikel erfahren Sie, wie Sie Azure Monitor-Protokolle zum Überwachen von Hadoop-Clustervorgängen in HDInsight verwenden. Außerdem erfahren Sie, wie Sie eine HDInsight-Überwachungslösung hinzufügen.

Bei [Azure Monitor-Protokollen](../log-analytics/log-analytics-overview.md) handelt es sich um einen Azure Monitor-Dienst, der Ihre Cloud- und lokalen Umgebungen überwacht. Die Überwachung dient der Aufrechterhaltung der Verfügbarkeit und Leistung Ihrer Umgebungen. Der Dienst sammelt Daten, die von Ressourcen in Ihrer Cloud- und in Ihrer lokalen Umgebung generiert werden, sowie von anderen Überwachungstools. Die Daten werden verwendet, um Analysen für mehrere Quellen bereitstellen zu können.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

* Einen Log Analytics-Arbeitsbereich Sie können sich diesen Arbeitsbereich als einzigartige Azure Monitor-Protokollumgebung mit eigenem Datenrepository, eigenen Datenquellen und eigenen Lösungen vorstellen. Eine Anleitung hierzu finden Sie unter [Erste Schritte mit einem Log Analytics-Arbeitsbereich](../azure-monitor/learn/quick-collect-azurevm.md#create-a-workspace).

* Ein Azure HDInsight-Cluster. Derzeit können Sie Azure Monitor-Protokolle mit den folgenden HDInsight-Clustertypen verwenden:

  * Hadoop
  * hbase
  * Interactive Query
  * Kafka
  * Spark
  * Storm

  Anweisungen zum Erstellen von HDInsight-Clustern finden Sie unter [Hadoop-Tutorial: Erste Schritte bei der Verwendung von Hadoop in HDInsight](hadoop/apache-hadoop-linux-tutorial-get-started.md).  

* Azure PowerShell Az-Modul.  Siehe [Einführung in das neue Azure PowerShell Az-Modul](https://docs.microsoft.com/powershell/azure/new-azureps-module-az). Stellen Sie sicher, dass Sie über die aktuelle Version verfügen. Führen Sie gegebenenfalls `Update-Module -Name Az` aus.

> [!NOTE]  
> Es wird empfohlen, sowohl den HDInsight-Cluster als auch den Log Analytics-Arbeitsbereich in derselben Region anzuordnen, um die Leistung zu erhöhen. Azure Monitor-Protokolle sind nicht in allen Azure-Regionen verfügbar.

## <a name="enable-azure-monitor-logs-by-using-the-portal"></a>Aktivieren von Azure Monitor-Protokollen im Azure-Portal

In diesem Abschnitt konfigurieren Sie einen vorhandenen HDInsight Hadoop-Cluster zur Verwendung eines Azure Log Analytics-Arbeitsbereichs, z. B. zum Überwachen von Aufträgen und Debugprotokollen.

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) Ihren Cluster aus. Der Cluster wird auf einer neuen Portalseite geöffnet.

1. Wählen Sie links unter **Überwachung** die Option **Azure Monitor** aus.

1. Wählen Sie in der Hauptansicht unter **Azure Monitor-Integration** die Option **Aktivieren** aus.

1. Wählen Sie in der Dropdownliste **Arbeitsbereich auswählen** einen vorhandenen Log Analytics-Arbeitsbereich aus.

1. Wählen Sie **Speichern** aus.  Das Speichern der Einstellung dauert einige Zeit.

    ![Aktivieren der Überwachung für HDInsight-Cluster](./media/hdinsight-hadoop-oms-log-analytics-tutorial/azure-portal-monitoring.png "Aktivieren der Überwachung für HDInsight-Cluster")

## <a name="enable-azure-monitor-logs-by-using-azure-powershell"></a>Aktivieren von Azure Monitor-Protokollen mit der Azure PowerShell

Sie können die Azure Monitor-Protokolle im Azure PowerShell Az-Modul mit dem Cmdlet [Enable-AzHDInsightMonitoring](https://docs.microsoft.com/powershell/module/az.hdinsight/enable-azhdinsightmonitoring) aktivieren.

```powershell
# Enter user information
$resourceGroup = "<your-resource-group>"
$cluster = "<your-cluster>"
$LAW = "<your-Log-Analytics-workspace>"
# End of user input

# obtain workspace id for defined Log Analytics workspace
$WorkspaceId = (Get-AzOperationalInsightsWorkspace `
                    -ResourceGroupName $resourceGroup `
                    -Name $LAW).CustomerId

# obtain primary key for defined Log Analytics workspace
$PrimaryKey = (Get-AzOperationalInsightsWorkspace `
                    -ResourceGroupName $resourceGroup `
                    -Name $LAW | Get-AzOperationalInsightsWorkspaceSharedKeys).PrimarySharedKey

# Enables monitoring and relevant logs will be sent to the specified workspace.
Enable-AzHDInsightMonitoring `
    -ResourceGroupName $resourceGroup `
    -Name $cluster `
    -WorkspaceId $WorkspaceId `
    -PrimaryKey $PrimaryKey

# Gets the status of monitoring installation on the cluster.
Get-AzHDInsightMonitoring `
    -ResourceGroupName $resourceGroup `
    -Name $cluster
```

Zum Deaktivieren verwenden Sie das Cmdlet [Disable-AzHDInsightMonitoring](https://docs.microsoft.com/powershell/module/az.hdinsight/disable-azhdinsightmonitoring):

```powershell
Disable-AzHDInsightMonitoring -Name "<your-cluster>"
```

## <a name="install-hdinsight-cluster-management-solutions"></a>Installieren von HDInsight-Clusterverwaltungslösungen

HDInsight bietet clusterspezifische Verwaltungslösungen, die Sie zu Azure Monitor-Protokollen hinzufügen können. [Verwaltungslösungen](../log-analytics/log-analytics-add-solutions.md) erweitern den Funktionsumfang von Azure Monitor-Protokollen und stellen zusätzliche Daten und Analysetools bereit. Diese Lösungen sammeln wichtige Leistungsmetriken aus Ihren HDInsight-Clustern. Außerdem stellen sie die Tools bereit, um die Metriken durchsuchen zu können. Außerdem bieten diese Lösungen Visualisierungen und Dashboards für die meisten in HDInsight unterstützten Clustertypen. Anhand der mit der Lösung erfassten Kennzahlen können Sie benutzerdefinierte Überwachungsregeln und -warnungen erstellen.

Verfügbare HDInsight-Lösungen:

* HDInsight Hadoop-Überwachung
* HDInsight HBase-Überwachung
* HDInsight Interactive Query-Überwachung
* HDInsight Kafka-Überwachung
* HDInsight Spark-Überwachung
* HDInsight Storm-Überwachung

Eine Anleitung für eine Verwaltungslösung finden Sie unter [Installieren einer Überwachungslösung](../azure-monitor/insights/solutions.md#install-a-monitoring-solution). Installieren Sie zum Experimentieren eine HDInsight Hadoop-Überwachungslösung. Wenn der Vorgang abgeschlossen ist, wird unter **Zusammenfassung** die Kachel **HDInsightHadoop** angezeigt. Wählen Sie die Kachel **HDInsightHadoop** aus. Die HDInsightHadoop-Lösung sieht wie folgt aus:

![HDInsight-Überwachungslösung – Ansicht](media/hdinsight-hadoop-oms-log-analytics-tutorial/hdinsight-oms-hdinsight-hadoop-monitoring-solution.png)

Im Bericht werden keine Aktivitäten angezeigt, da es sich um einen brandneuen Cluster handelt.

## <a name="configuring-performance-counters"></a>Konfigurieren von Leistungsindikatoren

Azure Monitor unterstützt das Sammeln und Analysieren von Leistungsmetriken für die Knoten in Ihrem Cluster. Weitere Informationen finden Sie unter [Leistungsindikatoren von Linux](https://docs.microsoft.com/azure/azure-monitor/platform/data-sources-performance-counters#linux-performance-counters).

## <a name="cluster-auditing"></a>Clusterüberwachung

HDInsight unterstützt die Clusterüberwachung mit Azure Monitor-Protokollen, indem die folgenden Protokolltypen importiert werden:

* `log_gateway_audit_CL`: Diese Tabelle enthält Überwachungsprotokolle von Clustergatewayknoten, in denen erfolgreiche und fehlerhafte Anmeldeversuche aufgezeichnet sind.
* `log_auth_CL`: Diese Tabelle enthält SSH-Protokolle mit erfolgreichen und fehlerhaften Anmeldeversuchen.
* `log_ambari_audit_CL`: Diese Tabelle enthält Überwachungsprotokolle von Ambari.
* `log_ranger_audti_CL`: Diese Tabelle enthält Überwachungsprotokolle von Apache Ranger auf Clustern mit Enterprise-Sicherheitspaket.

## <a name="next-steps"></a>Nächste Schritte

* [Abfragen von Azure Monitor-Protokollen zum Überwachen von HDInsight-Clustern](hdinsight-hadoop-oms-log-analytics-use-queries.md)
* [Überwachen der Clusterverfügbarkeit mit Apache Ambari und Azure Monitor-Protokollen](./hdinsight-cluster-availability.md)
