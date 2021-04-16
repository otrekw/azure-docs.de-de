---
title: Abfragen von Azure Monitor-Protokollen zum Überwachen von Azure HDInsight-Clustern
description: Erfahren Sie, wie Sie in Azure Monitor Abfragen zum Überwachen von Aufträgen ausführen, die in einem HDInsight-Cluster ausgeführt werden.
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive
ms.date: 12/02/2019
ms.openlocfilehash: 3cf97039983ecec44a7c3a32e178fdcf9f9c45ff
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104872182"
---
# <a name="query-azure-monitor-logs-to-monitor-hdinsight-clusters"></a>Abfragen von Azure Monitor-Protokollen zum Überwachen von HDInsight-Clustern

Lernen Sie einige grundlegende Szenarien zur Verwendung von Azure Monitor-Protokollen für die Überwachung von Azure HDInsight-Clustern kennen:

* [Analysieren von HDInsight-Clustermetriken](#analyze-hdinsight-cluster-metrics)
* [Erstellen von Ereigniswarnungen](#create-alerts-for-tracking-events)

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites"></a>Voraussetzungen

Sie müssen einen HDInsight-Cluster konfiguriert haben, um Azure Monitor-Protokolle verwenden zu können, und dem Arbeitsbereich die spezifischen Azure Monitor-Protokollverwaltungslösungen für den HDInsight-Cluster hinzugefügt haben. Anweisungen finden Sie unter [Verwenden von Azure Monitor-Protokollen mit HDInsight-Clustern](hdinsight-hadoop-oms-log-analytics-tutorial.md).

## <a name="analyze-hdinsight-cluster-metrics"></a>Analysieren von HDInsight-Clustermetriken

Erfahren Sie, wie Sie nach bestimmten Metriken für Ihren HDInsight-Cluster suchen.

1. Öffnen Sie den Log Analytics-Arbeitsbereich, der Ihrem HDInsight-Cluster zugeordnet ist, im Azure-Portal.
1. Wählen Sie unter **Allgemein** die Option **Protokolle** aus.
1. Geben Sie im Suchfeld die folgende Abfrage ein, um nach allen verfügbaren Metriken für alle HDInsight-Cluster zu suchen, die für die Verwendung von Azure Monitor-Protokollen konfiguriert sind, und klicken Sie dann auf **Ausführen**. Überprüfen Sie die Ergebnisse.

    ```kusto
    search *
    ```

    :::image type="content" source="./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-all-metrics.png" alt-text="Apache Ambari-Analysen – Suchen aller Metriken":::

1. Klicken Sie im Menü links auf die Registerkarte **Filter**.

1. Wählen Sie unter **Typ** die Option **Heartbeat** aus. Klicken Sie anschließend auf **Apply & Run** (Anwenden und ausführen).

    :::image type="content" source="./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-metrics.png" alt-text="Spezifische Metriken der Log Analytics-Suche":::

1. Beachten Sie, dass die Abfrage im Textfeld geändert wird:

    ```kusto
    search *
    | where Type == "Heartbeat"
    ```

1. Sie können diese Fehlerliste mithilfe der im linken Bereich des Menüs verfügbaren Optionen näher untersuchen. Beispiel:

   - So zeigen Sie Protokolle von einem bestimmten Knoten an:

     :::image type="content" source="./media/hdinsight-hadoop-oms-log-analytics-use-queries/log-analytics-specific-node.png" alt-text="Suchen nach bestimmten Fehlern (Ausgabe 1)":::

   - So zeigen Sie Protokolle zu bestimmten Zeiten an:

     :::image type="content" source="./media/hdinsight-hadoop-oms-log-analytics-use-queries/log-analytics-specific-time.png" alt-text="Nach bestimmten Fehlern suchen (Ausgabe 2)":::

1. Klicken Sie auf **Apply & Run** (Anwenden und ausführen), und überprüfen Sie die Ergebnisse. Beachten Sie auch, dass die Abfrage folgendermaßen aktualisiert wurde:

    ```kusto
    search *
    | where Type == "Heartbeat"
    | where (Computer == "zk2-myhado") and (TimeGenerated == "2019-12-02T23:15:02.69Z" or TimeGenerated == "2019-12-02T23:15:08.07Z" or TimeGenerated == "2019-12-02T21:09:34.787Z")
    ```

### <a name="additional-sample-queries"></a>Zusätzliche Beispielabfragen

Dies ist eine weitere Beispielabfrage, die auf dem Mittelwert der in einem Intervall von 10 Minuten verwendeten Ressourcen basiert und nach Clusternamen kategorisiert wird:

```kusto
search in (metrics_resourcemanager_queue_root_default_CL) * 
| summarize AggregatedValue = avg(UsedAMResourceMB_d) by ClusterName_s, bin(TimeGenerated, 10m)
```

Anstelle der auf dem Mittelwert der verwendeten Ressourcen basierenden Optimierung können Sie die Ergebnisse mit der folgenden Abfrage basierend auf dem Zeitpunkt in einem 10-Minuten-Fenster optimieren, an dem die meisten Ressourcen verwendet wurden (sowie den Zeitpunkten beim 90. und 95. Perzentil):

```kusto
search in (metrics_resourcemanager_queue_root_default_CL) * 
| summarize ["max(UsedAMResourceMB_d)"] = max(UsedAMResourceMB_d), ["pct95(UsedAMResourceMB_d)"] = percentile(UsedAMResourceMB_d, 95), ["pct90(UsedAMResourceMB_d)"] = percentile(UsedAMResourceMB_d, 90) by ClusterName_s, bin(TimeGenerated, 10m)
```

## <a name="create-alerts-for-tracking-events"></a>Erstellen von Warnungen zum Nachverfolgen von Ereignissen

Damit eine Warnung erstellt werden kann, muss eine Abfrage vorhanden sein, auf deren Basis die Warnung ausgelöst wird. Sie können jede beliebige Abfrage verwenden, um eine Warnung zu erstellen.

1. Öffnen Sie den Log Analytics-Arbeitsbereich, der Ihrem HDInsight-Cluster zugeordnet ist, im Azure-Portal.
1. Wählen Sie unter **Allgemein** die Option **Protokolle** aus.
1. Führen Sie die folgende Abfrage aus, zu der Sie eine Warnung erstellen möchten, und klicken Sie dann auf **Ausführen**.

    ```kusto
    metrics_resourcemanager_queue_root_default_CL | where AppsFailed_d > 0
    ```

    Die Abfrage stellt eine Liste mit fehlerhaften Anwendungen bereit, die in HDInsight-Clustern ausgeführt werden.

1. Klicken Sie oben auf der Seite auf **New alert rule** (Neue Warnungsregel).

    :::image type="content" source="./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-create-alert-query.png" alt-text="Neue Warnungsregel":::

1. Geben Sie im Fenster **Regel erstellen** die Abfrage und andere Details zum Erstellen einer Warnung ein, und wählen Sie dann **Warnungsregel erstellen**.

    :::image type="content" source="./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-create-alert.png" alt-text="Definieren einer Warnungsbedingung":::

### <a name="edit-or-delete-an-existing-alert"></a>Bearbeiten oder Löschen einer vorhandenen Warnung

1. Öffnen Sie im Azure-Portal den Log Analytics-Arbeitsbereich.

1. Klicken Sie links im Menü unter **Monitor** auf **Warnungen**.

1. Klicken Sie oben auf **Warnungsregeln verwalten**.

1. Wählen Sie die Warnung aus, die Sie bearbeiten oder löschen möchten.

1. Sie haben folgende Optionen: **Speichern**, **Verwerfen**, **Deaktivieren** und **Löschen**.

    :::image type="content" source="media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-edit-alert.png" alt-text="Lösch/Bearbeitungswarnung für HDInsight Azure Monitor-Protokolle":::

Weitere Informationen finden Sie unter [Erstellen, Anzeigen und Verwalten von Metrikwarnungen mit Azure Monitor](../azure-monitor/alerts/alerts-metric.md).

## <a name="see-also"></a>Siehe auch

* [Erste Schritte mit Protokollabfragen in Azure Monitor](../azure-monitor/logs/get-started-queries.md)
* [Erstellen benutzerdefinierter Ansichten mithilfe des Ansicht-Designers in Azure Monitor](../azure-monitor/visualize/view-designer.md)
