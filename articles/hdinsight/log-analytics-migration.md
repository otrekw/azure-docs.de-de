---
title: Migrieren von Log Analytics-Daten für Azure HDInsight
description: Erfahren Sie mehr über die Änderungen an der Azure Monitor-Integration und bewährte Methoden für die Verwendung der neuen Tabellen.
ms.service: hdinsight
ms.topic: how-to
ms.author: ali
author: AliciaLiMicrosoft
ms.date: 04/19/2021
ms.openlocfilehash: 6659b515ee2d25a4b9136ccfac4cc3444e491438
ms.sourcegitcommit: 425420fe14cf5265d3e7ff31d596be62542837fb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/20/2021
ms.locfileid: "107741174"
---
# <a name="log-analytics-migration-guide-for-azure-hdinsight-clusters"></a>Log Analytics-Migrationshandbuch für Azure HDInsight-Cluster

 Azure HDInsight ist ein unternehmensfähiger, verwalteter Clusterdienst. Dieser Dienst führt Open-Source-Analyseframeworks wie Apache Spark, Hadoop, HBase und Kafka in Azure aus. Azure HDInsight wurde in andere Azure-Dienste integriert, damit Kunden ihre Big Data-Analyseanwendungen besser verwalten können.

Log Analytics bietet ein Tool im Azure-Portal, mit dem Protokollabfragen bearbeitet und ausgeführt werden können. Die Abfragen stammen aus Daten, die von Azure Monitor-Protokollen gesammelt wurden, und analysieren ihre Ergebnisse interaktiv. Kunden können Log Analytics-Abfragen verwenden, um Datensätze abzurufen, die bestimmten Kriterien entsprechen. Sie können auch Abfragen verwenden, um Trends zu identifizieren, Muster zu analysieren und Einblicke in ihre Daten zu gewinnen.

Azure HDInsight hat die Integration in Log Analytics im Jahr 2017 aktiviert. HDInsight-Kunden haben dieses Feature schnell eingeführt, um ihre HDInsight-Cluster zu überwachen und die Protokolle in den Clustern abzufragen. Während dieses Feature immer häufiger eingeführt wurde, haben Kunden ihr Feedback zur Integration gegeben:

- Kunden können nicht entscheiden, welche Protokolle gespeichert werden sollen, und das Speichern aller Protokolle kann teuer werden.
- Aktuelle HDInsight-Schemaprotokolle folgen nicht konsistenten Benennungskonventionen, und einige Tabellen wiederholen sich.
- Kunden wünschen sich ein sofort einsatzbereites Dashboard, um den KPI ihrer HDInsight-Cluster einfach zu überwachen.
- Kunden müssen zu Log Analytics springen, um einfache Abfragen auszuführen.

## <a name="solution-overview"></a>Lösungsübersicht

Unter Berücksichtigung des Kundenfeedbacks hat das Azure HDInsight-Team in die Integration in Azure Monitor investiert. Diese Integration ermöglicht:

- Eine neue Gruppe von Tabellen im Log Analytics-Arbeitsbereich von Kunden. Die neuen Tabellen werden über eine neue Log Analytics-Pipeline bereitgestellt.
- Höhere Zuverlässigkeit
- Schnellere Übermittlung der Clusterprotokolldatei
- Ressourcenbasierte Tabellengruppierung und Standardabfragen

## <a name="benefits-of-the-new-azure-monitor-integration"></a>Vorteile der neuen Azure Monitor Integration

Dieses Dokument beschreibt die Änderungen an der Azure Monitor Integration und enthält bewährte Methoden für die Verwendung der neuen Tabellen.

**Neu gestaltete Schemata**: Die Schemaformatierung für die neue Azure Monitor-Integration ist besser organisiert und leicht verständlich. Es gibt zwei Drittel weniger Schemata, um so viel Mehrdeutigkeit in den Legacy-Schemata wie möglich zu beseitigen.

**Selektive Protokollierung (wird in Kürze veröffentlicht)** : Über Log Analytics sind Protokolle und Metriken verfügbar. Damit Sie Kosten bei der Überwachung einsparen, veröffentlichen wir ein neues Feature für die selektive Protokollierung. Verwenden Sie dieses Feature, um verschiedene Protokolle und Metrikquellen zu aktivieren und zu deaktivieren. Bei diesem Feature müssen Sie nur für das bezahlen, was Sie verwenden.

**Integration des Clusterportals für Protokolle**: Der Bereich **Protokolle** ist neu im HDInsight-Clusterportal. Jeder Benutzer mit Zugriff auf den Cluster kann in diesem Bereich eine beliebige Tabelle abfragen, an welche die Clusterressource Datensätze sendet. Benutzer benötigen keinen Zugriff mehr auf den Log Analytics-Arbeitsbereich, um die Datensätze für eine bestimmte Clusterressource anzeigen zu können.

**Integration des Insights-Clusterportals**: Der Bereich **Erkenntnisse** ist ebenfalls neu im HDInsight-Clusterportal. Nachdem Sie die neue Azure Monitor-Integration aktivieren, können Sie den Bereich **Erkenntnisse** auswählen und ein für den Clustertyp spezifisches Dashboard für Out-of-Box-Protokolle und Metriken wird automatisch aufgefüllt. Diese Dashboards wurden aus unseren vorherigen Azure-Lösungen überarbeitet. Sie bieten Ihnen tiefe Einblicke in die Leistung und Integrität Ihres Clusters.

**Einblicke in großem Umfang**: Sie können die neue Arbeitsmappe **At-Scale Insights** im **Azure Monitor-Portal** verwenden, um die Integrität und Leistung Ihrer Cluster über verschiedene Abonnements hinweg zu überwachen.

## <a name="customer-scenarios"></a>Kundenszenarien

In den folgenden Abschnitten wird beschrieben, wie Kunden das neue Azure Monitor in verschiedenen Szenarien verwenden können. Im Abschnitt [Neue Azure Monitor-Integration aktivieren](#activate-a-new-azure-monitor-integration) wird beschrieben, wie Sie die neue Azure Monitor-Integration aktivieren und verwenden. Der Abschnitt [Migrieren von Azure Monitor Classic zum neuen Azure Monitor-Integration](#migrate-to-the-new-azure-monitor-integration) enthält zusätzliche Informationen für Benutzer, die von der alten Azure Monitor-Integration abhängen.

> [!NOTE]
> Nur Cluster, die Ende September 2020 und danach erstellt wurden, sind für die neue Azure Monitoring-Integration berechtigt.

## <a name="activate-a-new-azure-monitor-integration"></a>Aktivieren einer neuen Azure Monitor-Integration 

> [!NOTE]
> Sie benötigen einen Log Analytics-Arbeitsbereich, der in einem Abonnement erstellt wurde, auf das Sie Zugriff haben, bevor Sie die neue Integration aktivieren. Weitere Informationen zum Erstellen eines Log Analytics-Arbeitsbereichs finden Sie unter [Erstellen eines Log Analytics-Arbeitsbereichs im Azure-Portal](../azure-monitor/learn/quick-create-workspace.md).

Aktivieren Sie die neue Integration, indem Sie zur Portalseite Ihres Clusters navigieren und im Menü auf der linken Seite nach unten scrollen, bis Sie den Abschnitt **Überwachung** erreichen. Wählen Sie im Abschnitt **Überwachung** die Option **Integration überwachen** aus. Wählen Sie dann **Aktivieren** aus, dann Sie können den Log Analytics-Arbeitsbereich auswählen, an den Ihre Protokolle gesendet werden sollen. Wählen Sie **Speichern** aus, nachdem Sie Ihren Arbeitsbereich ausgewählt haben. 

### <a name="access-the-new-tables"></a>Zugriff auf die neuen Tabellen

Es gibt zwei Möglichkeiten, auf die neuen Tabellen zuzugreifen. 

### <a name="approach-1"></a>Ansatz 1:
Die erste Möglichkeit, auf die neuen Tabellen zuzugreifen, ist der Log Analytics-Arbeitsbereich. 

1. Wechseln Sie zum Log Analytics-Arbeitsbereich, den Sie beim Aktivieren der Integration ausgewählt haben. 
2. Scrollen Sie im Menü auf der linken Seite des Bildschirms nach unten und wählen Sie **Protokolle** aus. Ein Abfrage-Editor für Protokolle wird mit einer Liste aller Tabellen im Arbeitsbereich angezeigt. 
3. Wenn die Tabellen nach **Lösung** gruppiert sind, befinden sich die neuen HDI-Tabellen im Abschnitt **Protokollverwaltung**. 
4. Wenn Sie die Tabellen nach **Ressourcentyp** gruppiert sind, befinden sich die Tabellen im Abschnitt **HDInsight-Cluster**, wie in der folgenden Abbildung dargestellt. 

> [!NOTE]
> Dieser Prozess beschreibt, wie in der alten Integration auf die Protokolle zugegriffen wurde. Dies erfordert, dass der Benutzer Zugriff auf den Arbeitsbereich hat.

### <a name="approach-2"></a>Ansatz 2:

Die zweite Möglichkeit, auf die neuen Tabellen zuzugreifen, ist der Zugriff auf das Clusterportal.
 
1. Navigieren Sie zur Portalseite Ihres Clusters, und scrollen Sie im Menü auf der linken Seite nach unten, bis der Abschnitt **Überwachung** angezeigt wird. In diesem Abschnitt wird der Bereich **Protokolle** angezeigt. 
2. Wählen Sie **Protokolle** aus, dann wird ein Abfrage-Editor für Protokolle angezeigt. Der Editor enthält alle Protokolle, die der Clusterressource zugeordnet sind. Sie haben die Protokolle an den Log Analytics-Arbeitsbereich gesendet, als Sie die Integration aktiviert haben. Diese Protokolle bieten rollenbasierte Zugriffskontrolle (RBAC). Mit RBAC können Benutzer, die Zugriff auf den Cluster haben (aber nicht auf den Arbeitsbereich), die Protokolle sehen, die dem Cluster zugeordnet sind.

Zum Vergleich zeigen die folgenden Screenshots die Ansicht des Legacy-Integrationsarbeitsbereichs und die neue Ansicht des Integrationsarbeitsbereichs:

**Arbeitsbereichsansicht der Legacyintegration**

  :::image type="content" source="./media/log-analytics-migration/legacy-integration-workspace-view.png"  lightbox="./media/log-analytics-migration/legacy-integration-workspace-view.png" alt-text="Screenshot: Ansicht des Legacy-Integrationsarbeitsbereichs." border="false":::

**Arbeitsbereichsansicht der neuen Integration**

  :::image type="content" source="./media/log-analytics-migration/new-integration-workspace-view.png" lightbox="./media/log-analytics-migration/new-integration-workspace-view.png" alt-text="Screenshot: Ansicht des neuen Integrationsarbeitsbereichs." border="false":::

### <a name="use-the-new-tables"></a>Verwenden der neuen Tabellen

Diese Integrationen können Ihnen helfen, die neuen Tabellen zu verwenden:

#### <a name="default-queries-to-use-with-new-tables"></a>Standardabfragen für neue Tabellen

Legen Sie im Abfrage-Editor für Protokolle den Umschalter auf **Abfragen** oberhalb der Tabellenliste fest. Stellen Sie sicher, dass Sie die Abfragen nach **Ressourcentyp** gruppieren und dass kein Filter für einen anderen Ressourcentyp als **HDInsight-Cluster** festgelegt ist. Die folgende Abbildung zeigt, wie die Ergebnisse aussehen, wenn sie nach **Ressourcentyp** und nach **HDInsight-Clustern** gefiltert sind. Wählen Sie einfach eine Option aus, die im Abfrageprotokoll-Editor angezeigt wird. Lesen Sie unbedingt die Kommentare, die in den Abfragen enthalten sind, da sie einige Informationen wie Ihren Clusternamen eingeben müssen, damit die Abfrage erfolgreich ausgeführt werden kann.

:::image type="content" source="./media/log-analytics-migration/default-query-results-grouped-resource-type.png" alt-text="Screenshot: Standardabfrageergebnisse, gruppiert nach Ressourcentyp" border="true":::.


#### <a name="create-your-own-queries"></a>Erstellen Ihrer eigenen Abfragen

Sie können Ihre eigenen Abfragen im Abfrageprotokoll-Editor eingeben. Abfragen, die für die alten Tabellen verwendet werden, sind für die neuen Tabellen nicht gültig, da viele der neuen Tabellen über neue, verbesserte Schemata verfügen. Die Standardabfragen sind hervorragende Verweise zum Strukturieren von Abfragen für die neuen Tabellen.

#### <a name="insights"></a>Einblicke

Erkenntnisse sind clusterspezifische Visualisierungsdashboards, die mithilfe von [Azure-Arbeitsmappen](../azure-monitor/platform/workbooks-overview.md) erstellt wurden. Diese Dashboards bieten Ihnen detaillierte Diagramme und Visualisierungen zur Ausführung Ihres Clusters. Die Dashboards verfügen über Abschnitte für jeden Clustertyp, YARN, Systemmetriken und Komponentenprotokolle. Sie können auf das Dashboard Ihres Clusters zugreifen, indem Sie die Seite Ihres Clusters im Portal aufrufen, nach unten zum Abschnitt **Überwachung** scrollen und den Bereich **Erkenntnisse** auswählen. Das Dashboard wird automatisch geladen, wenn Sie die neue Integration aktiviert haben. Warten Sie einige Sekunden, bis die Diagramme geladen werden, während sie die Protokolle abfragen.

:::image type="content" source="./media/log-analytics-migration/visualization-dashboard.png" lightbox="./media/log-analytics-migration/visualization-dashboard.png" alt-text="Screenshot des Dashboards des Visualisierungsdashboards.":::

#### <a name="custom-azure-workbooks"></a>Benutzerdefinierte Azure-Arbeitsmappen

Sie können eigene Azure-Arbeitsmappen mit benutzerdefinierten Diagrammen und Visualisierungen erstellen. Scrollen Sie auf der Portalseite Ihres Clusters nach unten zum Abschnitt **Überwachung** und wählen Sie im Menü auf der linken Seite den Bereich **Arbeitsmappen** aus. Sie können entweder eine leere Vorlage verwenden oder eine der Vorlagen im Abschnitt **HDInsight-Cluster** verwenden. Es gibt eine Vorlage für jeden Clustertyp. Vorlagen sind nützlich, wenn Sie bestimmte Anpassungen speichern möchten, die von HDInsight Insights standardmäßig nicht zur Verfügung stehen. Sie können gerne Anforderungen für neue Features in HDInsight senden, wenn Sie der Meinung sind, dass ihnen etwas fehlt.

#### <a name="at-scale-workbooks-for-new-azure-monitor-integrations"></a>Arbeitsmappen im großen Stil für neue Azure Monitor-Integrationen

Verwenden Sie unsere neue Arbeitsmappe im großen Stil, um eine Überwachung mit mehreren Clustern für Ihre Cluster zu erhalten. In unserer Arbeitsmappen-basierten Version wird gezeigt, für welche Ihrer Cluster die Überwachungspipeline aktiviert ist. Die Arbeitsmappe bietet Ihnen auch eine einfache Möglichkeit, die Integrität mehrerer Cluster gleichzeitig zu überprüfen. So zeigen Sie diese Arbeitsmappe an:

1. Wechseln Sie von der Startseite des Azure-Portals auf die Seite **Azure Monitor**
2. Wählen Sie auf der Seite **Azure Monitor** die Option **Insights Hub** im Bereich **Erkenntnisse** aus.
3. Wählen Sie im Abschnitt **Analyse** die Option **HDInsight-Cluster** aus.

   :::image type="content" source="./media/log-analytics-migration/at-scale-workbook.png" lightbox="./media/log-analytics-migration/at-scale-workbook.png" alt-text="Screenshot, der den Bildschirm „Arbeitsmappe im großen Stil“ zeigt." border="false":::

#### <a name="alerts"></a>Alerts

Sie können Ihren Clustern und Arbeitsbereichen im Protokollabfrage-Editor benutzerdefinierte Warnungen hinzufügen. Wechseln Sie zum Protokollabfrage-Editor, indem Sie im Cluster- oder Arbeitsbereichsportal auf den Bereich **Protokolle** klicken. Führen Sie eine Abfrage aus und wählen Sie dann **Neue Warnungsregel** aus, wie im folgenden Screenshot gezeigt. Weitere Informationen finden Sie unter [Konfigurieren von Warnungen](../azure-monitor/platform/alerts-log.md).

:::image type="content" source="./media/log-analytics-migration/new-rule-alert.png" alt-text="Screenshot: Fenster „Neue Warnungsregel“." border="false":::

## <a name="migrate-to-the-new-azure-monitor-integration"></a>Vorteile der neuen Azure Monitor-Integration

Wenn Sie die klassische Azure Monitor-Integration verwenden, müssen Sie einige Anpassungen an den neuen Tabellenformaten vornehmen, nachdem Sie zum neuen Azure Monitor wechseln.

Um die neue Azure Monitor-Integration zu aktivieren, führen Sie die Schritte aus, die im Abschnitt [Aktivieren einer neuen Azure Monitor-Integration](#activate-a-new-azure-monitor-integration) beschrieben werden.

### <a name="run-queries-in-log-analytics"></a>Abfragen in Log Analytics ausführen

Da sich das neue Tabellenformat vom vorherigen unterscheidet, müssen Ihre Abfragen überarbeitet werden, damit Sie unsere neuen Tabellen verwenden können. Nachdem Sie die neue Azure Monitor-Integration aktiviert haben, können Sie die Tabellen und Schemata durchsuchen, um die Felder zu identifizieren, die in Ihren alten Abfragen verwendet wurden.

Wir stellen eine [Mapping-Tabelle](#appendix-table-mapping) zwischen der alten Tabelle und der neuen Tabelle zur Verfügung, damit Sie schnell die neuen Felder finden können, die Sie zum Migrieren Ihrer Dashboards und Abfragen verwenden müssen.

**Standardabfragen**: Wir haben Standardabfragen erstellt, die zeigen, wie die neuen Tabellen für häufige Situationen verwendet werden. Die Standardabfragen zeigen auch, welche Informationen in den einzelnen Tabellen verfügbar sind. Sie können auf die Standardabfragen zugreifen, indem Sie die Anweisungen im Abschnitt [Standardabfragen für neue Tabellen](#default-queries-to-use-with-new-tables) in diesem Artikel befolgen.

### <a name="update-dashboards-for-hdinsight-clusters"></a>Aktualisieren von Dashboards für HDInsight-Cluster

Wenn Sie mehrere Dashboards erstellt haben, um Ihre HDInsight-Cluster zu überwachen, müssen Sie die Abfrage hinter der Tabelle anpassen, sobald Sie die neue Azure Monitor-Integration aktivieren. Der Tabellen- oder Feldname kann sich in der neuen Integration ändern, aber alle Informationen, die Sie in der alten Integration haben, sind enthalten.

Informationen zum Aktualisieren der Abfrage hinter den Dashboards finden Sie in der [Mapping-Tabelle](#appendix-table-mapping) zwischen der alten Tabelle/dem alten Schema und der neuen Tabelle bzw. dem neuen Schema.

#### <a name="out-of-box-dashboards"></a>Out-of-Box-Dashboards 

Wir haben außerdem die sofort einsatzbereiten Dashboards auf Clusterebene verbessert. Rechts oben in jedem Diagramm befindet sich eine Schaltfläche, mit der Sie die zugrundeliegende Abfrage anzeigen können, die die Informationen erzeugt. Das Diagramm ist eine hervorragende Möglichkeit, sich mit der effektiven Abfrage der neuen Tabellen vertraut zu machen. Sie können auf die standardmäßigen Dashboards zugreifen, indem Sie die Anweisungen befolgen, die Sie in den Abschnitten [Erkenntnisse](#insights) und [Arbeitsmappen im großen Stil für neue Azure Monitor-Integrationen](#at-scale-workbooks-for-new-azure-monitor-integrations) finden.

### <a name="use-an-hdinsight-at-scale-monitoring-dashboard"></a>Verwenden eines HDInsight-Dashboards für die Überwachung im großen Stil

Wenn Sie das Out-of-Box-Überwachungsdashboard für HDInsight-Cluster wie HDInsight Spark Monitoring und HDInsight Interactive Monitoring verwenden, arbeiten wir daran, Ihnen die gleichen Funktionen im Azure Monitor-Portal bereitzustellen.

Sie werden feststellen, dass es eine HDInsight-Clusteroption in Azure Monitor gibt.

   :::image type="content" source="./media/log-analytics-migration/hdinsight-azure-monitor.png" lightbox="./media/log-analytics-migration/hdinsight-azure-monitor.png" alt-text="Screenshot: HDInsight-Option in Azure Monitor." border="false":::

Der Insights Hub des Azure Monitor-Portals bietet Ihnen die Möglichkeit, mehrere HDInsight-Cluster an einem Ort zu überwachen. Wir organisieren die Cluster basierend auf dem Workloadtyp, sodass Sie Typen wie Spark, HBase und Hive sehen. Anstatt mehrere Dashboards zu verwenden, können Sie jetzt alle Ihre HDInsight-Cluster in dieser Ansicht überwachen.

> [!NOTE]
> Weitere Informationen finden Sie in den Abschnitten [Insights](#insights) und [Arbeitsmappen im großen Stil für neue Azure Monitor-Integrationen](#at-scale-workbooks-for-new-azure-monitor-integrations) in diesem Artikel.

## <a name="enable-both-integrations-to-accelerate-the-migration"></a>Aktivieren beider Integrationen zur Beschleunigung der Migration

Sie können sowohl die klassische als auch die neue Azure Monitor-Integration gleichzeitig in einem Cluster aktivieren, der für beide Integrationen geeignet ist, um schnell zur neuen Azure Monitor-Integration zu migrieren. Die neue Integration ist für alle Cluster verfügbar, die nach Mitte September 2020 erstellt wurden.

Auf diese Weise können Sie problemlos einen direkten Vergleich für die von Ihnen verwendeten Abfragen ausführen.

### <a name="enabling-the-classic-integration"></a>Aktivieren der klassischen Integration

Wenn Sie einen Cluster verwenden, der nach Mitte September 2020 erstellt wurde, wird die neue Portalumgebung im Portal Ihres Clusters angezeigt. Um die neue Pipeline zu aktivieren, führen Sie die Schritte aus, die im Abschnitt [Aktivieren einer neuen Azure Monitor-Integration](#activate-a-new-azure-monitor-integration) beschrieben werden. Um die klassische Integration in diesem Cluster zu aktivieren, wechseln Sie zur Portalseite Ihres Clusters. Wählen Sie den Abschnitt **Monitor-Integration** im Bereich **Überwachung** auf der linken Seite ihrer Clusterportalseite aus. Wählen Sie **Konfigurieren von Clustern fürHDInsight-Clusterintegration (klassisch)** aus. Ein Seitenkontext wird mit einem Umschalter angezeigt, den Sie verwenden können, um die klassische Azure Monitor-Integration zu aktivieren und zu deaktivieren. 

> [!NOTE]
> Es werden keine Protokolle oder Metriken der klassischen Integration über die Protokoll- und Insights-Seite Ihres Clusterportals angezeigt. Nur die neuen Integrationsprotokolle und -metriken sind an diesen Speicherorten vorhanden.

   :::image type="content" source="./media/log-analytics-migration/hdinsight-classic-integration.png" alt-text="Screenshot: Link für den Zugriff auf die klassische Integration." border="false":::

Das Erstellen neuer Cluster mit der klassischen Azure Monitor-Integration ist nach dem 1. Juli 2021 nicht mehr verfügbar.

## <a name="release-and-support-timeline"></a>Veröffentlichungs- und Supportzeitachse

- Die klassische Azure Monitoring-Integration ist nach dem 15. Oktober 2021 nicht mehr verfügbar. Sie können die klassische Azure Monitoring-Integration nach diesem Datum nicht mehr aktivieren.
- Vorhandene klassische Azure Monitoring-Integrationen funktionieren weiterhin. Die klassische Azure Monitoring-Integration wird nur eingeschränkt unterstützt. 
  - Probleme werden untersucht, sobald Kunden das Supportticket übermitteln.
  - Wenn die Lösung eine Imageänderung erfordert, sollten Kunden zur neuen Integration wechseln.
  - Die klassischen Azure Monitoring-Integrationscluster werden mit Ausnahme kritischer Sicherheitsprobleme nicht gepatcht.

## <a name="appendix-table-mapping"></a>Anhang: Tabellenzuordnung

Die folgenden Diagramme zeigen die Tabellenzuordnungen von der klassischen Azure Monitoring-Integration zu unserer neuen. In der Spalte **Workload** wird beschrieben, welcher Workload jeder Tabelle zugeordnet ist. In der Zeile **Neue Tabelle** wird der Name der neuen Tabelle angezeigt. In der Zeile **Beschreibung** wird der Typ der Protokolle/Metriken beschrieben, die in dieser Tabelle verfügbar sein werden. Die Zeile **Alte Tabelle** ist eine Liste aller Tabellen aus der klassischen Azure Monitor-Integration, deren Daten jetzt in der Tabelle vorhanden sind, die in der Zeile **Neue Tabelle** aufgeführt ist.

> [!NOTE]
> Einige Tabellen sind neu und basieren nicht auf alten Tabellen.

## <a name="general-workload-tables"></a>Allgemeine Workloadtabellen

| Neue Tabelle | Details |
| --- | --- |
| HDInsightAmbariSystemMetrics | <ul><li>**Beschreibung**: Diese Tabelle enthält die von Ambari gesammelten Systemmetriken. Die Metriken stammen jetzt von jedem Knoten im Cluster (mit Ausnahme von Edgeknoten) anstelle der beiden Hauptknoten. Jede Metrik ist jetzt eine Spalte, und jede Metrik wird einmal pro Datensatz gemeldet.</li><li>**Alte Tabelle**: Metriken\_CPU\_nice\_cl, Metriken\_CPU\_System\_cl, Metriken\_CPU\_Benutzer\_cl, Metriken\_Arbeitsspeicher\_Cache\_CL, Metriken\_Arbeitsspeicher\_Swap\_CL, Metriken\_Arbeitsspeicher\_gesamt\_CLMetriken\_Arbeitsspeicher\_Buffer\_CL, Metriken\_Laden\_1min\_CL, Metriken\_Laden\_CPU\_CL, Metriken\_Laden\_Knoten\_CL, Metriken\_Laden\_Prozessoren\_CL, Metriken\_Netzwerk\_in\_CL, Metriken\_Netzwerk\_aus\_CL</li></ul>|
| HDInsightAmbariClusterAlerts | <ul><li>**Beschreibung**: Diese Tabelle enthält Ambari-Clusterwarnungen von jedem Knoten im Cluster (mit Ausnahme von Edgeknoten). Jede Warnung ist ein Datensatz in dieser Tabelle.</li><li>**Alte Tabelle**: Metrik\_cluster\_warnungen\_CL</li></ul>|
| HDInsightSecurityLogs | <ul><li>**Beschreibung**: Diese Tabelle enthält Datensätze aus den Ambari-Überwachungsprotokollen und -Authentifizierungsprotokollen.</li><li>**Alte Tabelle**: Protokoll\_ambari\_audit\_CL, Protokoll\_auth\_CL</li></ul>|
| HDInsightRangerAuditLogs | <ul><li>**Beschreibung**: Diese Tabelle enthält alle Datensätze aus dem Ranger-Überwachungsprotokoll für ESP-Cluster.</li><li>**Alte Tabelle**: Ranger\_audit\_Protokolle\_CL</li></ul>|
| HDInsightGatewayAuditLogs\_CL | <ul><li>**Beschreibung**: Diese Tabelle enthält die Überwachungsinformationen für Gateway-Knoten. Sie hat das gleiche Format wie die Tabelle in der Spalte „Alte Tabellen“. **Sie befindet sich weiterhin im Abschnitt „Benutzerdefinierte Protokolle“.**</li><li>**Alte Tabelle**: Protokoll\_Gateway\_Audit\_CL</li></ul>|

## <a name="spark-workload"></a>Spark-Workload

> [!NOTE]
> Spark-Anwendungstabellen wurden durch 11 neue Spark-Tabellen (ab HDInsightSpark*) ersetzt, die detailliertere Informationen zu Ihren Spark-Workloads enthalten.


| Neue Tabelle | Details |
| --- | --- |
| HDInsightSparkLogs | <ul><li>**Beschreibung**: Diese Tabelle enthält alle Protokolle im Zusammenhang mit Spark und der zugehörigen Komponente: Livy und Jupyter.</li><li>**Alte Tabelle**: Protokoll\_livy,\_CL, Protokoll\_jupyter\_CL, Protokoll\_spark\_CL, Protokoll\_sparkappsexecutors\_CL, Protokoll\_sparkappsdrivers\_CL</li></ul>|
| HDInsightSparkApplicationEvents | <ul><li>**Beschreibung**: Diese Tabelle enthält Ereignisinformationen für Spark-Anwendungen, einschließlich Übermittlungs- und Abschlusszeit, App-ID und AppName. Dies ist hilfreich, um den Start- und Abschluss von Anwendungen nach verfolgen zu können. </li></ul>|
| HDInsightSparkBlockManagerEvents | <ul><li>**Beschreibung**: Diese Tabelle enthält Ereignisinformationen im Zusammenhang mit dem Block-Manager von Spark. Sie enthält Informationen wie die Executor-Speicherauslastung.</li></ul>|
| HDInsightSparkEnvironmentEvents | <ul><li>**Beschreibung**: Diese Tabelle enthält Ereignisinformationen im Zusammenhang mit der Umgebung, in der eine Anwendung ausgeführt wird, einschließlich Spark-Bereitstellungsmodus, Master und Informationen zum Executor.</li></ul>|
| HDInsightSparkExecutorEvents | <ul><li>**Beschreibung**: Diese Tabelle enthält Ereignisinformationen zur Spark Executor-Verwendung für durch eine Anwendung.</li></ul>|
| HDInsightSparkExtraEvents | <ul><li>**Beschreibung**: Diese Tabelle enthält Ereignisinformationen, die nicht in eine andere Spark-Tabelle passen. </li></ul>|
| HDInsightSparkJobEvents | <ul><li>**Beschreibung**: Diese Tabelle enthält Informationen zu Spark-Aufträgen, einschließlich der Start- und Endzeit, des Ergebnisses und der zugehörigen Phasen.</li></ul>|
| HDInsightSparkSqlExecutionEvents | <ul><li>**Beschreibung**: Diese Tabelle enthält Ereignisinformationen zu Spark SQL-Abfragen, einschließlich der Planinformationen und Beschreibung sowie Start- und Endzeiten.</li></ul>|
| HDInsightSparkStageEvents | <ul><li>**Beschreibung**: Diese Tabelle enthält Ereignisinformationen für Spark-Phasen, einschließlich der Start- und Abschlusszeiten, des Fehlerstatus und detaillierter Ausführungsinformationen.</li></ul>|
| HDInsightSparkStageTaskAccumulables | <ul><li>**Beschreibung**: Diese Tabelle enthält Leistungsmetriken für Phasen und Aufgaben.</li></ul>|
| HDInsightTaskEvents | <ul><li>**Beschreibung**: Diese Tabelle enthält Ereignisinformationen für Spark-Tasks, einschließlich Start- und Abschlusszeit, zugeordneter Phasen, Ausführungsstatus und Aufgabentyp.</li></ul>|
| HDInsightJupyterNotebookEvents | <ul><li>**Beschreibung**: Diese Tabelle enthält Ereignisinformationen für Jupyter Notebooks.</li></ul>|

## <a name="hadoopyarn-workload"></a>Hadoop/YARN-Workload

| Neue Tabelle | Details |
| --- | --- |
| HDInsightHadoopAndYarnMetrics | <ul><li>**Beschreibung**: Diese Tabelle enthält JMX-Metriken aus den Hadoop- und YARN-Frameworks. Sie enthält die gleichen JMX-Metriken wie die alten benutzerdefinierten Protokolltabellen sowie weitere Metriken, die wir als wichtig betrachtet haben. Wir haben Metriken für Timeline Server, Node Manager und Job History Server hinzugefügt. Sie enthält eine Metrik pro Datensatz.</li><li>**Alte Tabelle**: Metriken\_Ressourcemanager\_Clustermetriken\_CL, Metriken\_Ressourcemanager\_jvm\_CL, Metriken\_Ressourcemanager\_Warteschlange\_Root\_CL, Metriken\_Ressourcemanager\_Warteschlange\_Root\_Joblauncher\_CL, Metriken\_Ressourcemanager\_Warteschlange\_Root\_Standard\_CL, Metriken\_Ressourcemanager\_Warteschlange\_Root\_thriftsvr\_CL</li></ul>|
| HDInsightHadoopAndYarnLogs | <ul><li>**Beschreibung**: Diese Tabelle enthält alle generierten Protokolle aus den Hadoop- und YARN-Frameworks.</li><li>**Alte Tabelle**: Protokoll\_mrjobsummary\_CL, Protokoll\_Ressourcemanager\_CL, Protokoll\_timelineserver\_CL, Protokoll\_nodemanager\_CL</li></ul>|

 
## <a name="hivellap-workload"></a>Hive/LLAP-Workload 

| Neue Tabelle | Details |
| --- | --- |
| HDInsightHiveAndLLAPMetrics | <ul><li>**Beschreibung**: Diese Tabelle enthält JMX-Metriken aus den Hive- und LLAP-Frameworks. Sie enthält alle gleichen JMX-Metriken wie die alten Benutzerdefinierten Protokolltabellen. Sie enthält eine Metrik pro Datensatz.</li><li>**Alte Tabelle**: llap\_Metriken\_hiveserver2\_CL, llap\_Metriken\_hs2\_Metriken\_subsystemllap\_Metriken\_jvm\_CL, llap\_Metriken\_llap\_daemon\_info\_CL, llap\_Metriken\_buddy\_allocator\_info\_CL, llap\_Metriken\_deamon\_jvm\_CL, llap\_Metriken\_io\_CL, llap\_Metriken\_executor\_Metriken\_CL, llap\_Metriken\_metrisches System\_stats\_CL, llap\_Metriken\_Cache\_CL</li></ul>|
| HDInsightHiveAndLLAPLogs | <ul><li>**Beschreibung**: Diese Tabelle enthält Protokolle, die von Hive, LLAP und den zugehörigen Komponenten generiert wurden: WebHCat und Zeppelin.</li><li>**Alte Tabelle**: Protokoll\_hivemetastore\_CL Protokoll\_hiveserver2\_CL, Protokoll\_hiveserve2interactive\_CL, Protokoll\_webhcat\_CL, Protokoll\_zeppelin\_zeppelin\_CL</li></ul>|


## <a name="kafka-workload"></a>Kafka-Workload

| Neue Tabelle | Details |
| --- | --- |
| HDInsightKafkaMetrics | <ul><li>**Beschreibung**: Diese Tabelle enthält JMX-Metriken aus Kafka. Sie enthält die gleichen JMX-Metriken wie die alten benutzerdefinierten Protokolltabellen sowie weitere Metriken, die wir als wichtig betrachtet haben. Sie enthält eine Metrik pro Datensatz.</li><li>**Alte Tabelle**: Metriken\_kafka\_CL</li></ul>|
| HDInsightKafkaLogs | <ul><li>**Beschreibung**: Diese Tabelle enthält alle Protokolle, die von den Kafka-Brokern generiert wurden.</li><li>**Alte Tabelle**: Protokoll\_kafkaserver\_CL, Protokoll\_kafkacontroller\_CL</li></ul>|

## <a name="hbase-workload"></a>HBase-Workload

| Neue Tabelle | Details |
| --- | --- |
| HDInsightHBaseMetrics | <ul><li>**Beschreibung**: Diese Tabelle enthält JMX-Metriken aus HBase. Sie enthält dieselben JMX-Metriken aus den Tabellen, die in der Spalte „Altes Schema“ aufgeführt sind. Im Gegensatz zu den alten Tabellen enthält jede Zeile eine Metrik.</li><li>**Alte Tabelle**: Metriken\_regionserver\_CL, Metriken\_regionserver\_wal\_CL, Metriken\_regionserver\_ipc\_CL, Metriken\_regionserver\_os\_CL, Metriken\_regionserver\_replication\_CL, Metriken\_restserver\_CL, Metriken\_restserver\_jvm\_CL, Metriken\_hmaster\_assignmentmanager\_CL, Metriken\_hmaster\_ipc\_CL, Metriken\_hmaser\_os\_CL, Metriken\_hmaster\_balancer\_CL, Metriken\_hmaster\_jvm\_CL, Metriken\_hmaster\_CL, Metriken\_hmaster\_fs\_CL</li></ul>|
| HDInsightHBaseLogs | <ul><li>**Beschreibung**: Diese Tabelle enthält Protokolle von HBase und den zugehörigen Komponenten: Phoenix und HDFS.</li><li>**Alte Tabelle**: Protokoll\_regionserver\_CL, Protokoll\_restserver\_CL, Protokoll\_phoenixserver\_CL, Protokoll\_hmaster\_CL, Protokoll\_hdfsnamenode\_CL, Protokoll\_garbage\_collector\_CL</li></ul>|

## <a name="storm-workload"></a>Storm-Workload

| Neue Tabelle | Details |
| --- | --- |
| HDInsightStormMetrics | <ul><li>**Beschreibung**: Diese Tabelle enthält die gleichen JMX-Metriken wie die Tabellen im Abschnitt Alte Tabellen. Die Zeilen enthalten eine Metrik pro Datensatz.</li><li>**Alte Tabelle**: Metriken\_stormnimbus\_CL, Metriken\_stormsupervisor\_CL</li></ul>|
| HDInsightStormTopologyMetrics | <ul><li>**Beschreibung**: Diese Tabelle enthält Metriken auf Topologieebene von Storm. Es ist die gleiche Form wie die Tabelle, die im Abschnitt „Alte Tabellen“ aufgeführt ist.</li><li>**Alte Tabelle**: Metriken\_stormrest\_CL</li></ul>|
| HDInsightStormLogs | <ul><li>**Beschreibung**: Diese Tabelle enthält alle Protokolle, die von Storm generiert wurden.</li><li>**Alte Tabelle**: Protokoll\_supervisor\_CL, Protokoll\_nimbus\_CL</li></ul>|

## <a name="oozie-workload"></a>Oozie-Workload

| Neue Tabelle | Details |
| --- | --- |
| HDInsightOozieLogs | <ul><li>**Beschreibung**: Diese Tabelle enthält alle Protokolle, die vom Oozie-Framework generiert wurden.</li><li>**Alte Tabelle**: Protokoll\_oozie\_CL</li></ul>|

## <a name="next-steps"></a>Nächste Schritte
[Abfragen von Azure Monitor-Protokollen zum Überwachen von HDInsight-Clustern](hdinsight-hadoop-oms-log-analytics-use-queries.md)
