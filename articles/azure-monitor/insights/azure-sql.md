---
title: Azure SQL-Analyse-Lösung in Azure Monitor | Microsoft-Dokumentation
description: Die Azure SQL-Analyse-Lösung hilft Ihnen bei der Verwaltung Ihrer Azure SQL-Datenbanken.
ms.topic: conceptual
author: danimir
ms.author: danil
ms.date: 09/19/2020
ms.reviewer: carlrab
ms.openlocfilehash: 54ef88e65925ba9c7e9fe2e44ef0c76fbc9ceb04
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101717484"
---
# <a name="monitor-azure-sql-database-using-azure-sql-analytics-preview"></a>Überwachen von Azure SQL-Datenbank mithilfe von Azure SQL-Analyse (Vorschauversion)

![Symbol Azure SQL Analytics](./media/azure-sql/azure-sql-symbol.png)

Azure SQL-Analyse ist eine erweiterte Cloudüberwachungslösung zum bedarfsorientierten und abonnementübergreifenden Überwachen der Leistung Ihrer gesamten Azure SQL-Datenbankinstanzen in einer zentralen Ansicht. Azure SQL-Analyse erfasst und visualisiert wichtige Leistungsmetriken und umfasst integrierte Logik für die Problembehandlung in Bezug auf die Leistung.

Indem Sie diese erfassten Metriken verwenden, können Sie benutzerdefinierte Überwachungsregeln und Warnungen erstellen. Azure SQL-Analyse hilft Ihnen beim Erkennen von Problemen auf jeder Ebene Ihres Anwendungsstapels. Hierbei werden Azure-Diagnosemetriken zusammen mit Azure Monitor-Ansichten verwendet, um Daten zu Ihren gesamten Azure SQL-Datenbanken in einem zentralen Log Analytics-Arbeitsbereich darzustellen. Azure Monitor unterstützt Sie beim Erfassen, Korrelieren und Visualisieren strukturierter und nicht strukturierter Daten.

Einen Überblick über die praktische Verwendung der Azure SQL-Analyse und über typische Verwendungsszenarien finden Sie im eingebetteten Video:

>[!VIDEO https://www.youtube.com/embed/j-NDkN4GIzg]
>

## <a name="connected-sources"></a>Verbundene Quellen

Azure SQL-Analyse ist eine reine Cloudüberwachungslösung, die das Streaming von Diagnosetelemetriedaten für Ihre gesamten Azure SQL-Datenbanken unterstützt. Da von Azure SQL-Analyse für die Verbindungsherstellung mit Azure Monitor keine Agents verwendet werden, wird die Überwachung von SQL Server (lokal oder auf virtuellen Computern gehostet) nicht unterstützt.

| Verbundene Quelle | Unterstützt | BESCHREIBUNG |
| --- | --- | --- |
| [Diagnoseeinstellungen](../essentials/diagnostic-settings.md) | **Ja** | Azure-Metrik- und Protokolldaten werden direkt von Azure an Azure Monitor-Protokolle gesendet. |
| [Azure-Speicherkonto](../essentials/resource-logs.md#send-to-log-analytics-workspace) | Nein | Azure Monitor liest keine Daten aus einem Speicherkonto. |
| [Windows-Agents](../agents/agent-windows.md) | Nein | Von Azure SQL-Analyse werden keine direkten Windows-Agents verwendet. |
| [Linux-Agents](../vm/quick-collect-linux-computer.md) | Nein | Von Azure SQL-Analyse werden keine direkten Linux-Agents verwendet. |
| [System Center Operations Manager-Verwaltungsgruppe](../agents/om-agents.md) | Nein | Von Azure SQL-Analyse wird keine direkte Verbindung vom Operations Manager-Agent zu Azure Monitor verwendet. |

## <a name="azure-sql-analytics-options"></a>Azure SQL-Analyse – Optionen

In der folgenden Tabelle werden unterstützte Optionen für zwei Versionen des Dashboards der Azure SQL-Analyse beschrieben: eine für Azure SQL-Datenbank und die andere für Datenbanken einer verwalteten Azure SQL-Instanz.

| Option für Azure SQL-Analyse | BESCHREIBUNG | Unterstützung für SQL-Datenbank | Unterstützung für verwaltete SQL-Instanz |
| --- | ------- | ----- | ----- |
| Ressource nach Typ | Perspektive, die alle überwachten Ressourcen zählt. | Ja | Ja |
| Einblicke | Stellt einen hierarchischen Drilldown in die Leistung in Intelligent Insights bereit. | Ja | Ja |
| Errors | Stellt den hierarchischen Drilldown in SQL-Fehler bereit, die in den Datenbanken aufgetreten sind. | Ja | Ja |
| Zeitlimits | Stellt den hierarchischen Drilldown in SQL-Zeitlimits bereit, die in den Datenbanken aufgetreten sind. | Ja | Nein |
| Blockierungen | Stellt den hierarchischen Drilldown in SQL-Blockierungen bereit, die in den Datenbanken aufgetreten sind. | Ja | Nein |
| Datenbankwartevorgänge | Stellt den hierarchischen Drilldown in SQL-Wartestatistiken auf Datenbankebene bereit. Enthält Zusammenfassungen der gesamten Wartezeit sowie die Wartezeit pro Wartetyp. |Ja | Nein |
| Abfragedauer | Stellt den hierarchischen Drilldown in die Statistiken zur Abfrageausführung bereit, z.B. Abfragedauer, CPU-Auslastung, Daten-E/A-Auslastung und Protokoll-E/A-Auslastung. | Ja | Ja |
| Abfragewartevorgänge | Stellt den hierarchischen Drilldown in die Statistiken zu Abfragewartevorgängen nach Wartekategorie bereit. | Ja | Ja |

## <a name="configuration"></a>Konfiguration

Fügen Sie Azure SQL-Analyse (Vorschau) mithilfe des unter [Hinzufügen von Azure Monitor-Lösungen aus dem Lösungskatalog](./solutions.md) beschriebenen Prozesses Ihrem Log Analytics-Arbeitsbereich hinzu.

### <a name="configure-azure-sql-database-to-stream-diagnostics-telemetry"></a>Konfigurieren von Azure SQL-Datenbank zum Streamen von Diagnosetelemetriedaten

Nachdem Sie die Azure SQL-Analyse-Lösung in Ihrem Arbeitsbereich erstellt haben, müssen Sie **jede Ressource konfigurieren**, für die Sie zur Überwachung die zugehörigen Diagnosetelemetriedaten an Azure SQL-Analyse streamen möchten. Befolgen Sie dazu die detaillierten Anweisungen auf dieser Seite:

- Aktivieren Sie die Azure-Diagnose für Ihre Datenbank, um [Diagnosetelemetriedaten an die Azure SQL-Analyse zu streamen](../../azure-sql/database/metrics-diagnostic-telemetry-logging-streaming-export-configure.md).

Die oben abgebildete Seite enthält auch Anweisungen zum Aktivieren der Unterstützung für die Überwachung mehrerer Azure-Abonnements über einen einzelnen Azure SQL-Analysearbeitsbereich als zentrale Konsole.

## <a name="using-azure-sql-analytics"></a>Verwenden von Azure SQL-Analyse

Wenn Sie Azure SQL-Analyse Ihrem Arbeitsbereich hinzufügen, wird die Kachel „Azure SQL-Analyse“ Ihrem Arbeitsbereich hinzugefügt und erscheint in der Übersicht. Klicken Sie auf den Link „Zusammenfassung anzeigen“, um den Kachelinhalt zu laden.

![Zusammenfassungskachel „Azure SQL Analytics“](./media/azure-sql/azure-sql-sol-tile-01.png)

Nach dem Laden werden auf der Kachel die Anzahl von Datenbanken und Pools für elastische Datenbanken in SQL-Datenbank sowie Instanzen und Instanzdatenbanken in einer verwalteten SQL-Instanz angezeigt, von denen Diagnosetelemetriedaten für Azure SQL-Analyse empfangen werden.

![Kachel „Azure SQL Analytics“](./media/azure-sql/azure-sql-sol-tile-02.png)

Azure SQL-Analyse bietet zwei separate Ansichten: eine zur Überwachung der SQL-Datenbank und die andere zur Überwachung der verwalteten SQL-Instanz.

Zum Anzeigen des Überwachungsdashboards der Azure SQL-Analyse für die SQL-Datenbank klicken Sie auf den oberen Bereich der Kachel. Zum Anzeigen des Überwachungsdashboards der Azure SQL-Analyse für die verwaltete SQL-Instanz klicken Sie auf den unteren Bereich der Kachel.

### <a name="viewing-azure-sql-analytics-data"></a>Anzeigen von Azure SQL Analytics-Daten

Das Dashboard umfasst die Übersicht über alle Datenbanken, die aus verschiedenen Perspektiven überwacht werden. Damit verschiedene Perspektiven funktionieren, müssen Sie die richtigen Metriken oder Protokolle auf Ihren SQL-Ressourcen aktivieren, die dann an den Log Analytics-Arbeitsbereich gestreamt werden.

Wenn einige Metriken oder Protokolle nicht nach Azure Monitor gestreamt werden, werden die Kacheln in Azure SQL-Analyse nicht mit Überwachungsinformationen gefüllt.

### <a name="sql-database-view"></a>Ansicht der SQL-Datenbank

Nach Auswahl der Kachel „Azure SQL-Analyse“ für die Datenbank wird das Überwachungsdashboard angezeigt.

![Screenshot des Überwachungsdashboards](./media/azure-sql/azure-sql-sol-overview.png)

Die Auswahl einer Kachel öffnet einen Drilldown-Bericht zur entsprechenden Perspektive. Nach Auswahl der Perspektive wird der Drilldownbericht geöffnet.

![Screenshot des Drilldownberichts in eine bestimmte Perspektive](./media/azure-sql/azure-sql-sol-metrics.png)

Jede Perspektive in dieser Ansicht enthält Zusammenfassungen zu den Ebenen für Abonnement, Server, Pool für elastische Datenbanken und Datenbank. Darüber hinaus zeigt jede Perspektive auf der rechten Seite eine Perspektive an, die für den Bericht spezifisch ist. Durch die Auswahl eines Abonnements, Servers, Anwendungspools oder einer Datenbank aus der Liste wird der Drilldown fortgesetzt.

### <a name="sql-managed-instance-view"></a>Ansicht der verwalteten SQL-Instanz

Nach Auswahl der Kachel „Azure SQL-Analyse“ für die Datenbank wird das Überwachungsdashboard angezeigt.

![Azure SQL-Analyse – Übersicht](./media/azure-sql/azure-sql-sol-overview-mi.png)

Die Auswahl einer Kachel öffnet einen Drilldown-Bericht zur entsprechenden Perspektive. Nach Auswahl der Perspektive wird der Drilldownbericht geöffnet.

Nach dem Auswählen der Ansicht der verwalteten SQL-Instanz werden Details zur Instanznutzung, Instanzdatenbanken und Telemetriedaten zu den für die gesamte verwaltete Instanz ausgeführten Abfragen angezeigt.

![Azure SQL-Analyse – Zeitlimits](./media/azure-sql/azure-sql-sol-metrics-mi.png)

### <a name="intelligent-insights-report"></a>Intelligent Insights-Bericht

[Intelligent Insights](../../azure-sql/database/intelligent-insights-overview.md) von Azure SQL-Datenbank informiert Sie über die Leistung all Ihrer Azure SQL-Datenbanken. Alle erfassten Intelligent Insights-Daten können visuell dargestellt und über die Insights Perspektive aufgerufen werden.

![Azure SQL-Analyse – Insights](./media/azure-sql/azure-sql-sol-insights.png)

### <a name="elastic-pools-and-database-reports"></a>Berichte zu Pools für elastische Datenbanken und Datenbanken

Sowohl Pools für elastische Datenbanken als auch Datenbanken verfügen über eigene spezifische Berichte mit allen Daten, die für die Ressource im angegebenen Zeitraum gesammelt werden.

![Azure SQL-Analyse –Datenbank](./media/azure-sql/azure-sql-sol-database.png)

![Pool für elastische Azure SQL-Datenbanken](./media/azure-sql/azure-sql-sol-pool.png)

### <a name="query-reports"></a>Abfrageberichte

Anhand der Perspektive für Abfragedauer und Abfragewartevorgänge können Sie die Leistung von Abfragen über den Abfragebericht korrelieren. Dieser Bericht vergleicht die Abfrageleistung in verschiedenen Datenbanken und erleichtert das Ermitteln von Datenbanken, die die ausgewählte Abfrage im Vergleich zu langsamen Datenbanken ordnungsgemäß ausführen.

![Azure SQL-Analyse – Abfragen](./media/azure-sql/azure-sql-sol-queries.png)

## <a name="permissions"></a>Berechtigungen

Für die Verwendung von Azure SQL-Analyse benötigen Benutzer mindestens die Rolle „Leser“ in Azure. Diese Rolle erlaubt es Benutzern jedoch nicht, den Abfragetext anzuzeigen oder automatische Optimierungsaktionen durchzuführen. Rollen mit umfassenderen Berechtigungen in Azure, die eine Nutzung von Azure SQL-Analyse in vollem Umfang ermöglichen, sind „Besitzer“, „Mitwirkender“, „SQL-DB-Mitwirkender“ und „SQL Server-Mitwirkender“. Sie sollten auch erwägen, im Portal eine benutzerdefinierte Rolle mit spezifischen Berechtigungen zu erstellen, die nur erforderlich sind, um Azure SQL-Analyse zu verwenden, aber keinen Zugriff auf die Verwaltung anderer Ressourcen bieten.

### <a name="creating-a-custom-role-in-portal"></a>Erstellen einer benutzerdefinierten Rolle im Portal

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Da einige Organisationen eine strenge Berechtigungssteuerung in Azure erzwingen, können Sie das folgende PowerShell-Skript verwenden. Es ermöglicht das Erstellen der benutzerdefinierten Rolle „SQL Analytics Monitoring Operator“ im Azure-Portal. Sie umfasst nur die mindestens erforderlichen Lese- und Schreibberechtigungen, um Azure SQL-Analyse in vollem Umfang zu nutzen.

Ersetzen Sie „{SubscriptionId}“ im Skript unten durch Ihre Azure-Abonnement-ID, und führen Sie das Skript aus, während Sie mit der Rolle „Besitzer“ oder „Mitwirkender“ in Azure angemeldet sind.

   ```powershell
    Connect-AzAccount
    Select-AzSubscription {SubscriptionId}
    $role = Get-AzRoleDefinition -Name Reader
    $role.Name = "SQL Analytics Monitoring Operator"
    $role.Description = "Lets you monitor database performance with Azure SQL Analytics as a reader. Does not allow change of resources."
    $role.IsCustom = $true
    $role.Actions.Add("Microsoft.SQL/servers/databases/read");
    $role.Actions.Add("Microsoft.SQL/servers/databases/topQueries/queryText/*");
    $role.Actions.Add("Microsoft.Sql/servers/databases/advisors/read");
    $role.Actions.Add("Microsoft.Sql/servers/databases/advisors/write");
    $role.Actions.Add("Microsoft.Sql/servers/databases/advisors/recommendedActions/read");
    $role.Actions.Add("Microsoft.Sql/servers/databases/advisors/recommendedActions/write");
    $role.Actions.Add("Microsoft.Sql/servers/databases/automaticTuning/read");
    $role.Actions.Add("Microsoft.Sql/servers/databases/automaticTuning/write");
    $role.Actions.Add("Microsoft.Sql/servers/advisors/read");
    $role.Actions.Add("Microsoft.Sql/servers/advisors/write");
    $role.Actions.Add("Microsoft.Sql/servers/advisors/recommendedActions/read");
    $role.Actions.Add("Microsoft.Sql/servers/advisors/recommendedActions/write");
    $role.Actions.Add("Microsoft.Resources/deployments/write");
    $role.AssignableScopes = "/subscriptions/{SubscriptionId}"
    New-AzRoleDefinition $role
   ```

Nachdem die neue Rolle erstellt wurde, weisen Sie sie allen Benutzern zu, denen Sie benutzerdefinierte Berechtigungen zum Verwenden von Azure SQL-Analyse gewähren möchten.

## <a name="analyze-data-and-create-alerts"></a>Analysieren von Daten und Erstellen von Warnungen

Die Datenanalyse in Azure SQL-Analyse basiert auf der [Log Analytics-Sprache](../logs/get-started-queries.md) für Ihre benutzerdefinierten Abfragen und Berichte. Eine Beschreibung der von der Datenbankressource für benutzerdefinierte Abfragen gesammelten Daten finden Sie unter [Verfügbare Metriken und Protokolle](../../azure-sql/database/metrics-diagnostic-telemetry-logging-streaming-export-configure.md#metrics-and-logs-available).

Automatisierte Warnungen in Azure SQL-Analyse basieren auf dem Schreiben einer Log Analytics-Abfrage, die eine Warnung auslöst, wenn eine Bedingung erfüllt ist. Im Folgenden finden Sie mehrere Beispiele für Log Analytics-Abfragen, für die in Azure SQL-Analyse Warnungen eingerichtet werden können.

### <a name="creating-alerts-for-azure-sql-database"></a>Erstellen von Warnungen für Azure SQL-Datenbank

Sie können problemlos [Warnungen](../alerts/alerts-metric.md) mit den Daten erstellen, die aus Azure SQL-Datenbank-Ressourcen stammen. Im Folgenden finden Sie einige nützliche [Protokollabfragen](../logs/log-query-overview.md), die Sie mit einer Protokollwarnung verwenden können:

#### <a name="high-cpu"></a>Hohe CPU-Auslastung

```
AzureMetrics
| where ResourceProvider=="MICROSOFT.SQL"
| where ResourceId contains "/DATABASES/"
| where MetricName=="cpu_percent"
| summarize AggregatedValue = max(Maximum) by bin(TimeGenerated, 5m)
| render timechart
```

> [!NOTE]
>
> - Voraussetzung für die Einrichtung dieser Warnung ist, dass überwachte Datenbanken grundlegende Metriken an Azure SQL-Analyse streamen.
> - Ersetzen Sie den MetricName-Wert „cpu_percent“ durch „dtu_consumption_percent“ um stattdessen hohe DTU-Ergebnisse zu erhalten.

#### <a name="high-cpu-on-elastic-pools"></a>Hohe CPU-Auslastung der Pools für elastische Datenbanken

```
AzureMetrics
| where ResourceProvider=="MICROSOFT.SQL"
| where ResourceId contains "/ELASTICPOOLS/"
| where MetricName=="cpu_percent"
| summarize AggregatedValue = max(Maximum) by bin(TimeGenerated, 5m)
| render timechart
```

> [!NOTE]
>
> - Voraussetzung für die Einrichtung dieser Warnung ist, dass überwachte Datenbanken grundlegende Metriken an Azure SQL-Analyse streamen.
> - Ersetzen Sie den MetricName-Wert „cpu_percent“ durch „dtu_consumption_percent“ um stattdessen hohe DTU-Ergebnisse zu erhalten.

#### <a name="storage-in-average-above-95-in-the-last-1-hr"></a>Speicher im Durchschnitt über 95 % in der letzten Stunde

```
let time_range = 1h;
let storage_threshold = 95;
AzureMetrics
| where ResourceId contains "/DATABASES/"
| where MetricName == "storage_percent"
| summarize max_storage = max(Average) by ResourceId, bin(TimeGenerated, time_range)
| where max_storage > storage_threshold
| distinct ResourceId
```

> [!NOTE]
>
> - Voraussetzung für die Einrichtung dieser Warnung ist, dass überwachte Datenbanken grundlegende Metriken an Azure SQL-Analyse streamen.
> - Diese Abfrage erfordert die Einrichtung einer Warnungsregel, die eine Warnung auslöst, wenn Ergebnisse (> 0 Ergebnisse) von der Abfrage vorhanden sind, die angeben, dass die Bedingung in einigen Datenbanken vorhanden ist. Die Ausgabe ist eine Liste von Datenbankressourcen, die im definierten Zeitbereich den Speicherschwellenwert überschreiten.
> - Die Ausgabe ist eine Liste von Datenbankressourcen, die im definierten Zeitbereich den Speicherschwellenwert überschreiten.

#### <a name="alert-on-intelligent-insights"></a>Warnung in Intelligent Insights

> [!IMPORTANT]
> Wenn die Leistung einer Datenbank zufriedenstellend war und keine Intelligent Insights-Ergebnisse erzeugt werden konnten, wird für diese Abfrage folgende Fehlermeldung zurückgegeben: „Failed to resolve scalar expression named 'rootCauseAnalysis_s'“ (Der Skalarausdruck namens „rootCauseAnalysis_s“ konnte nicht aufgelöst werden). Dieses Verhalten ist für alle Fälle zu erwarten, in denen keine Intelligent Insights-Ergebnisse für die Datenbank vorhanden sind.

```
let alert_run_interval = 1h;
let insights_string = "hitting its CPU limits";
AzureDiagnostics
| where Category == "SQLInsights" and status_s == "Active"
| where TimeGenerated > ago(alert_run_interval)
| where rootCauseAnalysis_s contains insights_string
| distinct ResourceId
```

> [!NOTE]
>
> - Voraussetzung für die Einrichtung dieser Warnung ist, dass überwachte Datenbanken ein SQLInsights-Diagnoseprotokoll an Azure SQL-Analyse streamen.
> - Diese Abfrage erfordert die Einrichtung einer Warnungsregel, die mit der gleichen Häufigkeit wie „alert_run_interval“ ausgeführt wird, um Duplikate zu vermeiden. Die Regel muss für das Auslösen der Warnung eingerichtet werden, wenn Ergebnisse (> 0 Ergebnisse) von der Abfrage vorhanden sind.
> - Passen Sie den Wert „alert_run_interval“ an, um den Zeitbereich festzulegen, für den überprüft wird, ob die Bedingung in Datenbanken aufgetreten ist, die zum Streamen des SQLInsights-Protokolls an Azure SQL-Analyse konfiguriert sind.
> - Passen Sie „insights_string“ so an, dass die Ausgabe des Texts der Insights-Analyse der Grundursache erfasst wird. Der gleiche Text wird auf der Azure SQL-Analyse-Benutzeroberfläche angezeigt, die sie über die vorhandenen Einblicke nutzen können. Alternativ können Sie die folgende Abfrage verwenden, um den Text aller in Ihrem Abonnement generierten Einblicke anzuzeigen. Verwenden Sie die Ausgabe der Abfrage, um die unterschiedlichen Zeichenfolgen für die Einrichtung von Warnungen für Insights zu nutzen.

```
AzureDiagnostics
| where Category == "SQLInsights" and status_s == "Active"
| distinct rootCauseAnalysis_s
```

### <a name="creating-alerts-for-sql-managed-instance"></a>Erstellen von Warnungen für verwaltete SQL-Instanz

#### <a name="storage-is-above-90"></a>Speicher liegt über 90 %

```
let storage_percentage_threshold = 90;
AzureDiagnostics
| where Category =="ResourceUsageStats"
| summarize (TimeGenerated, calculated_storage_percentage) = arg_max(TimeGenerated, todouble(storage_space_used_mb_s) *100 / todouble (reserved_storage_mb_s))
   by ResourceId
| where calculated_storage_percentage > storage_percentage_threshold
```

> [!NOTE]
>
> - Voraussetzung für die Einrichtung dieser Warnung ist, dass für die überwachte verwaltete Instanz das Streaming des ResourceUsageStats-Protokolls an Azure SQL-Analyse aktiviert ist.
> - Diese Abfrage erfordert die Einrichtung einer Warnungsregel, die eine Warnung auslöst, wenn Ergebnisse (> 0 Ergebnisse) für die Abfrage vorhanden sind, die angeben, dass die Bedingung auf der verwalteten Instanz vorhanden ist. Die Ausgabe ist der prozentuale Speicherverbrauch auf der verwalteten Instanz.

#### <a name="cpu-average-consumption-is-above-95-in-the-last-1-hr"></a>Durchschnittliche CPU-Auslastung über 95 % in der letzten Stunde

```
let cpu_percentage_threshold = 95;
let time_threshold = ago(1h);
AzureDiagnostics
| where Category == "ResourceUsageStats" and TimeGenerated > time_threshold
| summarize avg_cpu = max(todouble(avg_cpu_percent_s)) by ResourceId
| where avg_cpu > cpu_percentage_threshold
```

> [!NOTE]
>
> - Voraussetzung für die Einrichtung dieser Warnung ist, dass für die überwachte verwaltete Instanz das Streaming des ResourceUsageStats-Protokolls an Azure SQL-Analyse aktiviert ist.
> - Diese Abfrage erfordert die Einrichtung einer Warnungsregel, die eine Warnung auslöst, wenn Ergebnisse (> 0 Ergebnisse) für die Abfrage vorhanden sind, die angeben, dass die Bedingung auf der verwalteten Instanz vorhanden ist. Die Ausgabe ist die durchschnittliche CPU-Auslastung der verwalteten Instanz im definierten Zeitraum in Prozent.

### <a name="pricing"></a>Preise

Obwohl Azure SQL-Analyse kostenlos verwendet werden kann, fallen für die Nutzung von Diagnosetelemetriedaten über die kostenlosen Einheiten hinaus, die für die Datenerfassung jeden Monat zugeteilt werden, Gebühren an. Weitere Informationen finden Sie unter [Log Analytics – Preise](https://azure.microsoft.com/pricing/details/monitor). Die kostenlosen Einheiten der bereitgestellten Datenerfassung ermöglichen die kostenlose Überwachung von mehreren Datenbanken pro Monat. Bei einer größeren Anzahl aktiver Datenbanken mit umfangreicheren Workloads können mehr Daten als bei Datenbanken im Leerlauf erfasst werden. Sie können problemlos Ihre Datenerfassungsnutzung in Azure SQL-Analyse überwachen, indem Sie den OMS-Arbeitsbereich im Navigationsmenü von Azure SQL-Analyse und dann „Nutzungs- und geschätzte Kosten“ auswählen.

## <a name="next-steps"></a>Nächste Schritte

- Verwenden Sie [Protokollabfragen](../logs/log-query-overview.md) in Azure Monitor, um detaillierte Azure SQL-Daten anzuzeigen.
- [Erstellen eigener Dashboards](../visualize/tutorial-logs-dashboards.md), die Azure SQL-Daten anzeigen
- [Erstellen von Warnungen](../alerts/alerts-overview.md), wenn bestimmte Azure SQL-Ereignisse auftreten

