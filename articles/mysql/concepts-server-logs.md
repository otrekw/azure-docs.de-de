---
title: Protokolle für langsame Abfragen – Azure Database for MySQL
description: Beschreibt die Protokolle für langsame Abfragen, die in Azure Database for MySQL verfügbar sind, sowie die verfügbaren Parameter zum Aktivieren verschiedener Protokolliergrade.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 01/28/2020
ms.openlocfilehash: 9a3a58cab2d9673a4660967e3a11d7f88900e718
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79232686"
---
# <a name="slow-query-logs-in-azure-database-for-mysql"></a>Protokolle für langsame Abfragen in Azure Database for MySQL
In Azure Database for MySQL ist das Protokoll für langsame Abfragen für Benutzer verfügbar. Der Zugriff auf das Transaktionsprotokoll wird jedoch nicht unterstützt. Das Protokoll für langsame Abfragen kann verwendet werden, um Leistungsengpässe für die Problembehandlung zu erkennen.

Weitere Informationen zum MySQL-Protokoll für langsame Abfragen finden Sie im [Abschnitt zu langsamen Abfragen](https://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html) im MySQL-Referenzhandbuch.

## <a name="access-slow-query-logs"></a>Zugreifen auf Protokolle für langsame Abfragen
Sie können Protokolle für langsame Abfragen von Azure Database for MySQL über das Azure-Portal und die Azure CLI auflisten und herunterladen.

Wählen Sie im Azure-Portal Ihren Azure Database for MySQL-Server aus. Wählen Sie unter der Überschrift **Überwachung** die Seite **Serverprotokolle** aus.

Weitere Informationen zur Azure-Befehlszeilenschnittstelle finden Sie unter [Konfigurieren der und Zugreifen auf die Serverprotokolle mithilfe der Azure-Befehlszeilenschnittstelle](howto-configure-server-logs-in-cli.md).

Auf ähnliche Weise können Sie die Protokolle mithilfe von Diagnoseprotokollen an Azure Monitor weiterreichen. Weitere Informationen finden Sie [unten](concepts-server-logs.md#diagnostic-logs).

## <a name="log-retention"></a>Protokollaufbewahrung
Protokolle sind für bis zu sieben Tage nach ihrer Erstellung verfügbar. Wenn die Gesamtgröße der verfügbaren Protokolle 7 GB überschreitet, werden die ältesten Dateien gelöscht, bis Speicherplatz verfügbar ist. 

Die Protokolle werden alle 24 Stunden oder bei Erreichen einer Größe von 7 GB rotiert, je nachdem, welches Ereignis früher eintritt.

## <a name="configure-slow-query-logging"></a>Konfigurieren der Protokollierung für langsame Abfragen 
Das Protokoll für langsame Abfragen ist standardmäßig deaktiviert. Legen Sie „slow_query_log“ auf „ON“ fest, um dieses Feature zu aktivieren.

Weitere Parameter, die Sie anpassen können:

- **long_query_time**: Die Abfrage wird protokolliert, wenn sie länger als „long_query_time“ (in Sekunden) dauert. Der Standardwert ist 10 Sekunden.
- **log_slow_admin_statements**: Wenn „ON“, sind administrative Anweisungen wie ALTER_TABLE und ANALYZE_TABLE in den Anweisungen enthalten, die in „slow_query_log“ geschrieben werden.
- **log_queries_not_using_indexes**: Bestimmt, ob Abfragen, die keine Indizes verwenden, in „slow_query_log“ protokolliert werden.
- **log_throttle_queries_not_using_indexes**: Dieser Parameter schränkt die Anzahl der Nichtindexabfragen ein, die in das Protokoll für langsame Abfragen geschrieben werden können. Dieser Parameter wird wirksam, wenn „log_queries_not_using_indexes“ auf „ON“ festgelegt ist.
- **log_output**: Wenn „file“, kann das Protokoll für langsame Abfragen sowohl in den lokalen Serverspeicher als auch in Azure Monitor-Diagnoseprotokolle geschrieben werden. Wenn der Parameter „None“ lautet, wird das Protokoll für langsame Abfragen nur in Azure Monitor-Diagnoseprotokolle geschrieben. 

> [!IMPORTANT]
> Wenn Ihre Tabellen nicht indiziert sind, kann das Festlegen der Parameter `log_queries_not_using_indexes` und `log_throttle_queries_not_using_indexes` auf ON (EIN) die Leistung von MySQL beeinträchtigen, da alle Anforderungen, die für diese nicht indizierten Tabellen ausgeführt werden, in das Protokoll für langsame Abfragen geschrieben werden.<br><br>
> Wenn Sie langsame Abfragen für längere Zeit protokollieren möchten, wird empfohlen, `log_output` auf „None“ (Kein) festzulegen. Wenn „File“ (Datei) festgelegt ist, werden diese Protokolle in den Speicher des lokalen Servers geschrieben und können sich negativ auf die MySQL-Leistung auswirken. 

Vollständige Beschreibungen der Parameter des Protokolls für langsame Abfragen finden Sie in der [MySQL-Dokumentation zum Protokoll für langsame Abfragen](https://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html).

## <a name="diagnostic-logs"></a>Diagnoseprotokolle
Azure Database for MySQL ist in Azure Monitor-Diagnoseprotokolle integriert. Nachdem Sie die Protokolle zu langsamen Abfragen auf Ihrem MySQL-Server aktiviert haben, können Sie diese an Azure Monitor-Protokolle, Event Hubs oder Azure Storage ausgeben. Weitere Informationen zum Aktivieren von Diagnoseprotokollen finden Sie im Gewusst-wie-Abschnitt der [Dokumentation zu Diagnoseprotokollen](../azure-monitor/platform/platform-logs-overview.md).

In der folgenden Tabelle wird der Inhalt der einzelnen Protokolle beschrieben. Je nach Ausgabemethode können die enthaltenen Felder und ihre Reihenfolge variieren.

| **Eigenschaft** | **Beschreibung** |
|---|---|
| `TenantId` | Ihre Mandanten-ID |
| `SourceSystem` | `Azure` |
| `TimeGenerated` [UTC] | Zeitstempel für den Aufzeichnungsbeginn des Protokolls in UTC |
| `Type` | Typ des Protokolls Immer `AzureDiagnostics` |
| `SubscriptionId` | GUID für das Abonnement, zu dem der Server gehört |
| `ResourceGroup` | Name der Ressourcengruppe, zu der der Server gehört |
| `ResourceProvider` | Name des Ressourcenanbieters Immer `MICROSOFT.DBFORMYSQL` |
| `ResourceType` | `Servers` |
| `ResourceId` | Ressourcen-URI |
| `Resource` | Name des Servers |
| `Category` | `MySqlSlowLogs` |
| `OperationName` | `LogEvent` |
| `Logical_server_name_s` | Name des Servers |
| `start_time_t` [UTC] | Uhrzeit, zu der die Abfrage begann |
| `query_time_s` | Gesamtzeit (in Sekunden), die die Ausführung der Abfrage gedauert hat |
| `lock_time_s` | Gesamtzeit in Sekunden, während der die Abfrage gesperrt war |
| `user_host_s` | Username |
| `rows_sent_s` | Anzahl gesendeter Zeilen |
| `rows_examined_s` | Anzahl untersuchter Zeilen |
| `last_insert_id_s` | [last_insert_id](https://dev.mysql.com/doc/refman/8.0/en/information-functions.html#function_last-insert-id) |
| `insert_id_s` | Einfüge-ID |
| `sql_text_s` | Vollständige Abfrage |
| `server_id_s` | Die ID des Servers |
| `thread_id_s` | Thread-ID |
| `\_ResourceId` | Ressourcen-URI |

> [!Note]
> Bei `sql_text` wird das Protokoll abgeschnitten, wenn es länger als 2.048 Zeichen ist.

## <a name="analyze-logs-in-azure-monitor-logs"></a>Analysieren von Protokollen in Azure Monitor-Protokollen

Sobald die Protokolle für langsamen Abfragen an Azure Monitor-Protokolle über Diagnoseprotokolle weitergeleitet werden, können Sie weitere Analysen Ihrer langsamen Abfragen durchführen. Im Folgenden finden Sie einige Beispielabfragen, die Ihnen beim Einstieg helfen. Stellen Sie sicher, dass Sie die Abfragen unten mit Ihrem Servernamen aktualisieren.

- Abfragen auf einem bestimmten Server, die länger als 10 Sekunden dauern

    ```Kusto
    AzureDiagnostics
    | where LogicalServerName_s == '<your server name>'
    | where Category == 'MySqlSlowLogs'
    | project TimeGenerated, LogicalServerName_s, event_class_s, start_time_t , query_time_d, sql_text_s 
    | where query_time_d > 10
    ```

- Auflisten der 5 längsten Abfragen auf einem bestimmten Server

    ```Kusto
    AzureDiagnostics
    | where LogicalServerName_s == '<your server name>'
    | where Category == 'MySqlSlowLogs'
    | project TimeGenerated, LogicalServerName_s, event_class_s, start_time_t , query_time_d, sql_text_s 
    | order by query_time_d desc
    | take 5
    ```

- Zusammenfassen langsamer Abfragen nach minimaler, maximaler, durchschnittlicher Dauer und standardmäßigen Abweichungen der Abfragezeit auf einem bestimmten Server

    ```Kusto
    AzureDiagnostics
    | where LogicalServerName_s == '<your server name>'
    | where Category == 'MySqlSlowLogs'
    | project TimeGenerated, LogicalServerName_s, event_class_s, start_time_t , query_time_d, sql_text_s 
    | summarize count(), min(query_time_d), max(query_time_d), avg(query_time_d), stdev(query_time_d), percentile(query_time_d, 95) by LogicalServerName_s
    ```

- Diagramm der Verteilung langsamer Abfragen auf einem bestimmten Server

    ```Kusto
    AzureDiagnostics
    | where LogicalServerName_s == '<your server name>'
    | where Category == 'MySqlSlowLogs'
    | project TimeGenerated, LogicalServerName_s, event_class_s, start_time_t , query_time_d, sql_text_s 
    | summarize count() by LogicalServerName_s, bin(TimeGenerated, 5m)
    | render timechart
    ```

- Anzeigen von Abfragen, die länger als 10 Sekunden für alle MySQL-Server mit aktivierten Diagnoseprotokollen dauern

    ```Kusto
    AzureDiagnostics
    | where Category == 'MySqlSlowLogs'
    | project TimeGenerated, LogicalServerName_s, event_class_s, start_time_t , query_time_d, sql_text_s 
    | where query_time_d > 10
    ```    
    
## <a name="next-steps"></a>Nächste Schritte
- [Vorgehensweise: Konfigurieren von Protokollen für langsame Abfragen im Azure-Portal](howto-configure-server-logs-in-portal.md)
- [Vorgehensweise: Konfigurieren von Protokollen für langsame Abfragen über die Azure CLI](howto-configure-server-logs-in-cli.md).
