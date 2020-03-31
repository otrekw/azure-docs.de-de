---
title: Protokolle für langsame Abfragen – Azure Database for MariaDB
description: Beschreibt die Protokolle, die in Azure Database for MariaDB verfügbar sind, sowie die verfügbaren Parameter zum Aktivieren verschiedener Protokolliergrade.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 2c07e5eeedd2e4f42ec7b165bf161e142421df58
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79527893"
---
# <a name="slow-query-logs-in-azure-database-for-mariadb"></a>Protokolle für langsame Abfragen in Azure Database for MariaDB
In Azure Database for MariaDB ist das Protokoll für langsame Abfragen für Benutzer verfügbar. Der Zugriff auf das Transaktionsprotokoll wird jedoch nicht unterstützt. Das Protokoll für langsame Abfragen kann verwendet werden, um Leistungsengpässe für die Problembehandlung zu erkennen.

Weitere Informationen zum Protokoll für langsame Abfragen finden Sie in der MariaDB-Dokumentation zum [Protokoll für langsame Abfragen](https://mariadb.com/kb/en/library/slow-query-log-overview/).

## <a name="access-slow-query-logs"></a>Zugreifen auf Protokolle für langsame Abfragen
Sie können Protokolle für langsame Abfragen von Azure Database for MariaDB über das Azure-Portal und die Azure CLI auflisten und herunterladen.

Wählen Sie im Azure-Portal Ihren Azure Database for MariaDB-Server aus. Wählen Sie unter der Überschrift **Überwachung** die Seite **Serverprotokolle** aus.

Weitere Informationen zur Azure CLI finden Sie unter [Konfigurieren der und Zugreifen auf die Serverprotokolle mithilfe der Azure CLI](howto-configure-server-logs-cli.md).

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
> Wenn Ihre Tabellen nicht indiziert sind, kann das Festlegen der Parameter `log_queries_not_using_indexes` und `log_throttle_queries_not_using_indexes` auf ON (EIN) die Leistung von MariaDB beeinträchtigen, da alle Anforderungen, die für diese nicht indizierten Tabellen ausgeführt werden, in das Protokoll für langsame Abfragen geschrieben werden.<br><br>
> Wenn Sie langsame Abfragen für längere Zeit protokollieren möchten, wird empfohlen, `log_output` auf „None“ (Kein) festzulegen. Wenn „File“ (Datei) festgelegt ist, werden diese Protokolle in den Speicher des lokalen Servers geschrieben und können sich negativ auf die MariaDB-Leistung auswirken. 

Vollständige Beschreibungen der Parameter des Protokolls für langsame Abfragen finden Sie in der [MariaDB-Dokumentation zum Protokoll für langsame Abfragen](https://mariadb.com/kb/en/library/slow-query-log-overview/).

## <a name="diagnostic-logs"></a>Diagnoseprotokolle
Azure Database for MariaDB ist in Azure Monitor-Diagnoseprotokolle integriert. Nachdem Sie die Protokolle zu langsamen Abfragen auf Ihrem MariaDB-Server aktiviert haben, können Sie sie an Azure Monitor-Protokolle, Event Hubs oder Azure Storage ausgeben. Weitere Informationen zum Aktivieren von Diagnoseprotokollen finden Sie im Gewusst-wie-Abschnitt der [Dokumentation zu Diagnoseprotokollen](../azure-monitor/platform/platform-logs-overview.md).

In der folgenden Tabelle wird der Inhalt der einzelnen Protokolle beschrieben. Je nach Ausgabemethode können die enthaltenen Felder und ihre Reihenfolge variieren.

| **Eigenschaft** | **Beschreibung** |
|---|---|
| `TenantId` | Ihre Mandanten-ID |
| `SourceSystem` | `Azure` |
| `TimeGenerated` [UTC] | Zeitstempel für den Aufzeichnungsbeginn des Protokolls in UTC |
| `Type` | Typ des Protokolls Immer `AzureDiagnostics` |
| `SubscriptionId` | GUID für das Abonnement, zu dem der Server gehört |
| `ResourceGroup` | Name der Ressourcengruppe, zu der der Server gehört |
| `ResourceProvider` | Name des Ressourcenanbieters Immer `MICROSOFT.DBFORMARIADB` |
| `ResourceType` | `Servers` |
| `ResourceId` | Ressourcen-URI |
| `Resource` | Name des Servers |
| `Category` | `MySqlSlowLogs` |
| `OperationName` | `LogEvent` |
| `Logical_server_name_s` | Name des Servers |
| `start_time_t` [UTC] | Uhrzeit, zu der die Abfrage begann |
| `query_time_s` | Gesamtzeit, die die Abfrage zur Ausführung benötigte |
| `lock_time_s` | Gesamtzeit, über die die Abfrage gesperrt war |
| `user_host_s` | Username |
| `rows_sent_s` | Anzahl gesendeter Zeilen |
| `rows_examined_s` | Anzahl untersuchter Zeilen |
| `last_insert_id_s` | [last_insert_id](https://mariadb.com/kb/en/library/last_insert_id/) |
| `insert_id_s` | Einfüge-ID |
| `sql_text_s` | Vollständige Abfrage |
| `server_id_s` | Server-ID |
| `thread_id_s` | Thread-ID |
| `\_ResourceId` | Ressourcen-URI |

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

- Anzeigen von Abfragen, die länger als 10 Sekunden für alle MariaDB-Server mit aktivierten Diagnoseprotokollen dauern

    ```Kusto
    AzureDiagnostics
    | where Category == 'MySqlSlowLogs'
    | project TimeGenerated, LogicalServerName_s, event_class_s, start_time_t , query_time_d, sql_text_s 
    | where query_time_d > 10
    ```    
    
## <a name="next-steps"></a>Nächste Schritte
- [Vorgehensweise: Konfigurieren von Protokollen für langsame Abfragen im Azure-Portal](howto-configure-server-logs-portal.md)
- [Vorgehensweise: Konfigurieren von Protokollen für langsame Abfragen über die Azure CLI](howto-configure-server-logs-cli.md).
