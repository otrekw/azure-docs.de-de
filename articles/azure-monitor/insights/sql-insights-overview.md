---
title: Überwachen Ihrer SQL-Bereitstellungen mit SQL Insights (Vorschau)
description: Übersicht über SQL Insights in Azure Monitor
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/15/2021
ms.openlocfilehash: d0bb5c55d3f7ba0573dfe9b511f4d31dcc64ed85
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105567830"
---
# <a name="monitor-your-sql-deployments-with-sql-insights-preview"></a>Überwachen Ihrer SQL-Bereitstellungen mit SQL Insights (Vorschau)
Bei SQL Insights handelt es sich um eine umfassende Lösung zur Überwachung jeglichem Produkt der [Azure SQ- Familie](../../azure-sql/index.yml). SQL Insights verwendet [dynamische Managementansichten](../../azure-sql/database/monitoring-with-dmvs.md), um die Daten offenzulegen, die Sie zur Überwachung des Zustands, zur Diagnose von Problemen und zur Leistungsoptimierung benötigen.  

SQL Insights führt die gesamte Überwachung aus der Ferne durch. Überwachungsagenten auf dedizierten virtuellen Maschinen stellen eine Verbindung zu Ihren SQL-Ressourcen her und sammeln Daten aus der Ferne. Die gesammelten Daten werden in [Azure Monitor Protokollen](../logs/data-platform-logs.md)gespeichert, sodass eine einfache Aggregation, Filterung und Trendanalyse ermöglicht wird. Sie können die gesammelten Daten aus der SQL Insights-[Arbeitsmappenvorlage](../visualize/workbooks-overview.md) anzeigen, oder Sie können direkt in die Daten eintauchen, indem Sie [Protokollabfragen verwenden](../logs/get-started-queries.md).

## <a name="pricing"></a>Preise
Es gibt keine direkten Kosten für SQL Insights. Alle Kosten werden durch die virtuellen Computer, von denen die Daten gesammelt werden, durch die Log Analytics Arbeitsbereiche, in denen die Daten gespeichert werden, sowie für alle für die Daten konfigurierten Warnungs Regeln anfallen. 

**Virtuelle Computer**

Im Falle virtueller Maschinen werden Sie auf der Grundlage der auf der Preisseite für [virtuelle Maschinen](https://azure.microsoft.com/en-us/pricing/details/virtual-machines/linux/) veröffentlichten Preise belastet. Die Anzahl erforderlicher virtueller Computer variiert je nach der Anzahl der Verbindungs-Zeichenketten, die Sie überwachen möchten. Es wird empfohlen, für jeweils 100 Verbindungs-Zeichenketten 1 virtuelle Gerät der Größe Standard_B2s zuzuweisen. Weitere Informationen finden Sie unter [Anforderungen an virtuelle Azure-Computer](sql-insights-enable.md#azure-virtual-machine-requirements).

**Log Analytics-Arbeitsbereiche**

Für die Log Analytics-Arbeitsbereiche werden Sie auf der Grundlage der auf der Azure Monitor-Preisseite veröffentlichten Preise belastet, die auf der [Seite Azure Monitor Preise](https://azure.microsoft.com/pricing/details/monitor/)veröffentlicht werden. Die von SQL Insights verwendeten Log Analytics Arbeitsbereiche verursachen Kosten für die Datenerfassung, Daten-Aufbewahrung und (fakultativ) den Datenexport. Die genauen Kosten variieren je nach der erfassten, beibehaltenen und exportierten Datenmenge. Die Menge dieser Daten hängt anschließend von Ihrer Datenbankaktivität und den in Ihren [Überwachungsprofilen](sql-insights-enable.md#create-sql-monitoring-profile) definierten Erfassungseinstellungen ab.

**Warnregeln**

Für die Log Analytics-Arbeitsbereiche werden Sie auf der Grundlage der auf der Azure Monitor-Preisseite veröffentlichten Preise belastet, die auf der [Seite Azure Monitor-Preisgestaltung](https://azure.microsoft.com/pricing/details/monitor/) veröffentlicht werden. Wenn Sie [Warnungen mit SQL Insights erstellen](sql-insights-alerts.md), werden Ihnen alle erstellten Warnungsregeln und gesendeten Benachrichtigungen in Rechnung gestellt.

## <a name="supported-versions"></a>Unterstützte Versionen 
SQL Insights unterstützt die folgenden Versionen von SQL Server:
- SQL Server 2012 und höher

SQL Insights unterstützt SQL Server in den folgenden Umgebungen:
- Azure SQL-Datenbank
- Verwaltete Azure SQL-Instanz
- SQL Server auf virtuellen Azure-Maschinen (SQL Server wird auf virtuellen Maschinen ausgeführt, die beim Anbieter von [virtuellen SQL-Maschinen](../../azure-sql/virtual-machines/windows/sql-agent-extension-manually-register-single-vm.md) registriert sind)
- Azure-VMS (SQL Server, die auf virtuellen Computern ausgeführt werden, die nicht beim [SQL](../../azure-sql/virtual-machines/windows/sql-agent-extension-manually-register-single-vm.md) -VM-Anbieter registriert sind)

SQL Insights bietet keine oder nur eingeschränkte Unterstützung für Folgendes:
- **Nicht-Azure-Instanzen**: SQL Server, die auf virtuellen Maschinen außerhalb von Azure läuft, werden nicht unterstützt
- **Azure SQL Database Elastic Pools**: Für Elastic Pools können keine Metriken erfasst werden. Metriken können nicht für Datenbanken in Elastic Pools gesammelt werden.
- **Azure SQL Database niedrige Service-Stufen**: Metriken können für Datenbanken auf den [Service-Stufen](../../azure-sql/database/resource-limits-dtu-single-databases.md) Basic, S0, S1 und S2 nicht erfasst werden
- **Server loser Azure SQL-Datenbank-Stufe**: Metriken können für Datenbanken mit der serverlosen Compute-Stufe erfasst werden. Der Prozess der Erfassung von Metriken setzt jedoch den Verzögerungs-Timer für die automatische Pause zurück, wodurch verhindert wird, dass die Datenbank in einen automatischen Pausenzustand übergeht
- **Sekundäre Replikate**: Metriken können nur für ein einzelnes sekundäres Replikat pro Datenbank gesammelt werden. Wenn eine Datenbank über mehr als 1 sekundäres Replikat verfügt, kann nur 1 überwacht werden.
- **Authentifizierung mit Azure Active Directory**: Die einzige unterstützte [Authentifizierungs-](../../azure-sql/database/logins-create-manage.md#authentication-and-authorization)Methode für die Überwachung ist die SQL-Authentifizierung. Für SQL Server auf Azure VM wird die Authentifizierung mit Active Directory auf einem benutzerdefinierten Domänencontroller nicht unterstützt.  

## <a name="open-sql-insights"></a>Öffnen von SQL Insights
Sie können SQL Insights durch Auswählen von **SQL (Vorschau)** im Bereich **Insights** des Menüs **Azure Monitor** im Azure-Portal öffnen. Klicken Sie auf eine Kachel, um die Darstellung für den überwachten SQL-Typ zu laden.

:::image type="content" source="media/sql-insights/portal.png" alt-text="SQL Insights im Azure-Portal":::

## <a name="enable-sql-insights"></a>Aktivieren von SQL Insights 
Anweisungen zum Aktivieren von SQL-Insights finden Sie unter [Aktivieren von SQL-Insights](sql-insights-enable.md).

## <a name="troubleshoot-sql-insights"></a>Fehlerbehebung bei SQL-Insights 
Anweisungen zur Fehlerbehebung von SQL Insights finden Sie unter [Fehlerbehebung von SQL Insights](sql-insights-troubleshoot.md).

## <a name="data-collected-by-sql-insights"></a>In SQL Insights erfasste Daten
SQL Insights führt die gesamte Überwachung aus der Ferne durch. Wir installieren keine Agenten auf den virtuellen Maschinen, auf denen SQL Server ausgeführt wird. 

SQL Insights verwendet dedizierte virtuelle Maschinen zur Überwachung, um Daten von Ihren SQL-Ressourcen aus der Ferne zu sammeln. Auf jeder überwachenden virtuellen Maschine werden der [Azure Monitor-Agent](https://docs.microsoft.com/azure/azure-monitor/agents/azure-monitor-agent-overview) und die Erweiterung Workload Insights (WLI) installiert. Die WLI-Erweiterung beinhaltet den Open-Source [Telegraf-Agenten](https://www.influxdata.com/time-series-platform/telegraf/). SQL insights verwendet [Datensammlungsregeln](https://docs.microsoft.com/azure/azure-monitor/agents/data-collection-rule-overview), um die Datensammlungseinstellungen für das [SQL Server-Plugin](https://www.influxdata.com/integration/microsoft-sql-server/) von Telegraf festzulegen.

Für Azure SQL Database, Azure SQL Managed Instance und den SQL Server sind unterschiedliche Datensätze verfügbar. Die folgenden Tabellen beschreiben die verfügbaren Daten. Bei der [Erstellung eines Überwachungsprofils](sql-insights-enable.md#create-sql-monitoring-profile) können Sie festlegen, welche Datensätze gesammelt werden sollen und wie häufig dies geschehen soll.

Die folgenden Tabellen weisen die folgenden Spalten auf:
- **Spitznamen**: Namen der Abfrage, wie sie im Azure-Portal beim Erstellen eines Überwachungsprofils angezeigt wird
- **Konfigurationsname**: Namen der Abfrage, wie im Azure-Portal beim Bearbeiten eines Überwachungsprofils angezeigt
- **Namespace**: Namen der Abfrage, wie er in einem Log Analytics-Arbeitsbereich zu finden ist. Dieser Identifizierer erscheint in der Tabelle **InsighstMetrics** auf der `Namespace`Eigenschaft in der `Tags` Spalte
- **DMVs**: Die dynamisch verwalteten Ansichten, die zur Erzeugung des Datensatzes verwendet werden
- **Standardmäßig aktiviert**: Ob die Daten standardmäßig gesammelt werden
- **Standard-Erfassungshäufigkeit**: Wie oft die Daten standardmäßig erfasst werden

### <a name="data-for-azure-sql-database"></a>Daten für die Azure SQL Datenbank 
| Anzeigename | Konfigurationsname | Namespace | DMVs | Standardmäßig aktiviert | DB-Wartestatistiken |
|:---|:---|:---|:---|:---|:---|
| Standard-Sammelhäufigkeit | AzureSQLDBWaitStats | sqlserver_azuredb_waitstats | sys.dm_db_wait_stats | Nein | Nicht verfügbar |
| DBO-Wartestatistiken | AzureSQLDBOsWaitstats | sqlserver_waitstats |sys.dm_os_wait_stats | Ja | 60 Sekunden |
| Arbeitsspeicherclerks | AzureSQLDBMemoryClerks | sqlserver_memory_clerks | sys.dm_os_memory_clerks | Ja | 60 Sekunden |
| Datenbank-E/A | AzureSQLDBDatabaseIO | sqlserver_database_io | sys.dm_io_virtual_file_stats<br>sys.database_files<br>tempdb.sys.database_files | Ja | 60 Sekunden |
| Servereigenschaften | AzureSQLDBServerProperties | sqlserver_server_properties | sys.dm_os_job_object<br>sys.database_files<br>sys.[databases]<br>sys.[database_service_objectives] | Ja | 60 Sekunden |
| Leistungsindikatoren | AzureSQLDBPerformanceCounters | sqlserver_performance | sys.dm_os_performance_counters<br>sys.databases | Ja | 60 Sekunden |
| Ressourcenstatistiken | AzureSQLDBResourceStats | sqlserver_azure_db_resource_stats | sys.dm_db_resource_stats | Ja | 60 Sekunden |
| Ressourcengovernance | AzureSQLDBResourceGovernance | sqlserver_db_resource_governance | sys.dm_user_db_resource_governance | Ja | 60 Sekunden |
| Requests | AzureSQLDBRequests | sqlserver_requests | sys.dm_exec_sessions<br>sys.dm_exec_requests<br>sys.dm_exec_sql_text | Nein | Nicht verfügbar |
| Zeitplaner| AzureSQLDBSchedulers | sqlserver_schedulers | sys.dm_os_schedulers | Nein | Nicht verfügbar  |

### <a name="data-for-azure-sql-managed-instance"></a>Daten für die Azure SQL Verwaltete Instanz 

| Anzeigename | Konfigurationsname | Namespace | DMVs | Standardmäßig aktiviert | DB-Wartestatistiken |
|:---|:---|:---|:---|:---|:---|
| Wartestatistik | AzureSQLMIOsWaitstats | sqlserver_waitstats | sys.dm_os_wait_stats | Ja | 60 Sekunden |
| Arbeitsspeicherclerks | AzureSQLMIMemoryClerks | sqlserver_memory_clerks | sys.dm_os_memory_clerks | Ja | 60 Sekunden |
| Datenbank-E/A | AzureSQLMIDatabaseIO | sqlserver_database_io | sys.dm_io_virtual_file_stats<br>sys.master_files | Ja | 60 Sekunden |
| Servereigenschaften | AzureSQLMIServerProperties | sqlserver_server_properties | sys.server_resource_stats | Ja | 60 Sekunden |
| Leistungsindikatoren | AzureSQLMIPerformanceCounters | sqlserver_performance | sys.dm_os_performance_counters<br>sys.databases| Ja | 60 Sekunden |
| Ressourcenstatistiken | AzureSQLMIResourceStats | sqlserver_azure_db_resource_stats | sys.server_resource_stats | Ja | 60 Sekunden |
| Ressourcengovernance | AzureSQLMIResourceGovernance | sqlserver_instance_resource_governance | sys.dm_instance_resource_governance | Ja | 60 Sekunden |
| Requests | AzureSQLMIRequests | sqlserver_requests | sys.dm_exec_sessions<br>sys.dm_exec_requests<br>sys.dm_exec_sql_text | Nein | Nicht verfügbar |
| Zeitplaner | AzureSQLMISchedulers | sqlserver_schedulers | sys.dm_os_schedulers | Nein | Nicht verfügbar |

### <a name="data-for-sql-server"></a>Daten für den SQL Server

| Anzeigename | Konfigurationsname | Namespace | DMVs | Standardmäßig aktiviert | DB-Wartestatistiken |
|:---|:---|:---|:---|:---|:---|
| Wartestatistik | SQLServerWaitStatsCategorized | sqlserver_waitstats | sys.dm_os_wait_stats | Ja | 60 Sekunden | 
| Arbeitsspeicherclerks | SQLServerMemoryClerks | sqlserver_memory_clerks | sys.dm_os_memory_clerks | Ja | 60 Sekunden |
| Datenbank-E/A | SQLServerDatabaseIO | sqlserver_database_io | sys.dm_io_virtual_file_stats<br>sys.master_files | Ja | 60 Sekunden |
| Servereigenschaften | SQLServerProperties | sqlserver_server_properties | sys.dm_os_sys_info | Ja | 60 Sekunden |
| Leistungsindikatoren | SQLServerPerformanceCounters | sqlserver_performance | sys.dm_os_performance_counters | Ja | 60 Sekunden |
| Volumenraum | SQLServerVolumeSpace | sqlserver_volume_space | sys.master_files | Ja | 60 Sekunden |
| SQL-Server-CPU | SQLServerCpu | sqlserver_cpu | sys.dm_os_ring_buffers | Ja | 60 Sekunden |
| Zeitplaner | SQLServerSchedulers | sqlserver_schedulers | sys.dm_os_schedulers | Nein | Nicht verfügbar |
| Requests | SQLServerRequests | sqlserver_requests | sys.dm_exec_sessions<br>sys.dm_exec_requests<br>sys.dm_exec_sql_text | Nein | Nicht verfügbar |
| Verfügbarkeit Replikat-Stati | SQLServerAvailabilityReplicaStates | sqlserver_hadr_replica_states | sys.dm_hadr_availability_replica_states<br>sys.availability_replicas<br>sys.availability_groups<br>sys.dm_hadr_availability_group_states | Nein | 60 Sekunden |
| Verfügbarkeit Datenbankreplikate | SQLServerDatabaseReplicaStates | sqlserver_hadr_dbreplica_states | sys.dm_hadr_database_replica_states<br>sys.availability_replicas | Nein | 60 Sekunden |

## <a name="next-steps"></a>Nächste Schritte

- Anweisungen zum Aktivieren von SQL-Insights finden Sie unter [SQL-Insights aktivieren](sql-insights-enable.md)
- Siehe [Häufig gestellte Fragen](../faq.md#sql-insights-preview) für häufig gestellte Fragen zu SQL insights
