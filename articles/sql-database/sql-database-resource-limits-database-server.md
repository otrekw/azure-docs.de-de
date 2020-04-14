---
title: Azure SQL-Datenbank-Ressourcenlimits | Microsoft-Dokumentation
description: Dieser Artikel bietet eine Übersicht über die Ressourcenlimits von Azure SQL-Datenbank für Einzeldatenbanken und Pools für elastische Datenbanken. Darüber hinaus bietet er Informationen darüber, was geschieht, wenn diese Ressourcenlimits erreicht oder überschritten werden.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: sashan,moslake,josack
ms.date: 11/19/2019
ms.openlocfilehash: afb30a17d7a1450f169402c18f41ce249415e89d
ms.sourcegitcommit: 6397c1774a1358c79138976071989287f4a81a83
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/07/2020
ms.locfileid: "80804825"
---
# <a name="sql-database-resource-limits-and-resource-governance"></a>SQL-Datenbank-Ressourcenlimits und -Ressourcenkontrolle

Dieser Artikel bietet eine Übersicht über die SQL-Datenbank-Ressourcenlimits für einen SQL-Datenbank-Server, der Einzeldatenbanken und Pools für elastische Datenbanken verwaltet. Er enthält Informationen darüber, was geschieht, wenn diese Ressourcenlimits erreicht oder überschritten werden. Außerdem werden die zum Erzwingen dieser Grenzwerte verwendeten Ressourcenkontrollmechanismen beschrieben.

> [!NOTE]
> Informationen zu den Grenzwerten für verwaltete Instanzen finden Sie unter [Ressourcenlimits bei SQL-Datenbank für verwaltete Instanzen](sql-database-managed-instance-resource-limits.md).

## <a name="maximum-resource-limits"></a>Maximale Ressourcenlimits

| Resource | Begrenzung |
| :--- | :--- |
| Datenbanken pro Server | 5\.000 |
| Standardanzahl von Servern pro Abonnement in beliebiger Region | 20 |
| Maximale Anzahl von Servern pro Abonnement in beliebiger Region | 200 |  
| DTU/eDTU-Kontingent pro Server | 54.000 |  
| Virtuelle Kerne pro Server/Instanz | 540 |
| Maximale Anzahl von Pools pro Server | Begrenzt durch die Anzahl von DTUs oder virtuellen Kernen. Beispiel: Wenn jeder Pool 1.000 DTUs umfasst, kann ein Server 54 Pools unterstützen.|
|||

> [!IMPORTANT]
> Wenn sich die Anzahl von Datenbanken dem Grenzwert pro SQL-Datenbank-Server nähert, kann Folgendes geschehen:
>
> - Höhere Latenz bei der Ausführung von Abfragen, die die Masterdatenbank betreffen.  Dies bezieht sich auch auf die Ansichten der Ressourcennutzungsstatistiken (z. B. sys.resource_stats) ein.
> - Höhere Latenz bei Verwaltungsvorgängen und dem Rendern von Portalblickpunkten. Dazu gehört auch das Aufzählen von Datenbanken auf dem Server.

> [!NOTE]
> Wenn Sie ein höheres DTU-/eDTU-Kontingent oder eine höhere Anzahl von virtuellen Kernen oder Servern als die Standardanzahl erhalten möchten, übermitteln Sie im Azure-Portal eine neue Supportanfrage. Weitere Informationen finden Sie unter [Anfordern von Kontingenterhöhungen für Azure SQL-Datenbank](quota-increase-request.md).

### <a name="storage-size"></a>Speichergröße

Informationen zu den Grenzwerten bei der Speichergröße pro Tarif für Ressourcenspeichergrößen von Einzeldatenbanken finden Sie unter [DTU-Ressourcenlimits](sql-database-dtu-resource-limits-single-databases.md) und [V-Kern-Ressourcenlimits](sql-database-vcore-resource-limits-single-databases.md).

## <a name="what-happens-when-database-resource-limits-are-reached"></a>Was geschieht, wenn die Datenbankressourcen erreicht werden?

### <a name="compute-dtus-and-edtus--vcores"></a>Compute (DTUs und eDTUs / vCores)

Bei zunehmender Datenbank-Computenutzung (gemessen an DTUs und eDTUs oder virtuellen Kernen) erhöht sich die Abfragewartezeit, und bei Abfragen können sogar Timeouts auftreten. Unter diesen Bedingungen können Abfragen vom Dienst in die Warteschlange eingereiht werden, und dann werden Ressourcen für die Ausführung bereitgestellt, wenn Ressourcen frei werden.
Wenn eine hohe Computenutzung festgestellt wird, stehen folgende Optionen als Gegenmaßnahmen zur Verfügung:

- Erhöhen der Computegröße der Datenbank oder des Pools für elastische Datenbanken, um mehr Computeressourcen für die Datenbank bereitzustellen. Siehe [Skalieren der Ressourcen für einzelne Datenbanken](sql-database-single-database-scale.md) und [Skalieren der Ressourcen für elastische Pools in Azure SQL-Datenbank](sql-database-elastic-pool-scale.md).
- Optimieren von Abfragen, um die Ressourcenverwendung durch die einzelnen Abfragen zu verringern. Weitere Informationen finden Sie unter [Abfrageoptimierung/Abfragehinweise](sql-database-performance-guidance.md#query-tuning-and-hinting).

### <a name="storage"></a>Storage

Wenn der verwendete Datenbankspeicherplatz die maximale Größe erreicht, treten bei Einfügungen in die Datenbank und Updates, die die Datenmenge erhöhen, Fehler auf, sodass Clients eine [Fehlermeldung](troubleshoot-connectivity-issues-microsoft-azure-sql-database.md) erhalten. SELECT- und DELETE-Anweisungen werden weiterhin erfolgreich ausgeführt.

Wenn eine hohe Speicherplatznutzung festgestellt wird, stehen folgende Optionen als Gegenmaßnahmen zur Verfügung:

- Erhöhen der maximalen Größe der Datenbank oder des Pools für elastische Datenbanken oder Hinzufügen von zusätzlichem Speicher. Siehe [Skalieren der Ressourcen für einzelne Datenbanken](sql-database-single-database-scale.md) und [Skalieren der Ressourcen für elastische Pools in Azure SQL-Datenbank](sql-database-elastic-pool-scale.md).
- Wenn sich die Datenbank in einem Pool für elastische Datenbanken befindet, kann sie auch aus dem Pool heraus verschoben werden, damit ihr Speicherplatz nicht mit anderen Datenbanken gemeinsam genutzt wird.
- Verkleinern Sie eine Datenbank, um ungenutzten Speicherplatz freizugeben. Weitere Informationen finden Sie unter [Verwalten von Dateispeicherplatz in Azure SQL-Datenbank](sql-database-file-space-management.md).

### <a name="sessions-and-workers-requests"></a>Sitzungen und Worker (Anforderungen)

Die maximale Anzahl von Sitzungen und Workern wird durch die Dienstebene und Computegröße (DTUs/eDTUs oder V-Kerne) bestimmt. Neue Anforderungen werden abgelehnt, wenn Sitzung oder Worker Grenzwerte erreicht haben, und Clients erhalten eine Fehlermeldung. Während die Anzahl der verfügbaren Verbindungen von der Anwendung gesteuert werden kann, ist die Anzahl paralleler Worker oft schwieriger zu schätzen und zu steuern. Dies gilt insbesondere während der Spitzenlastzeiten, wenn Datenbankressourcenlimits erreicht werden und sich Worker aufgrund längerer Ausführungszeiten von Abfragen, langer Blockierungsketten oder einer übermäßigen Abfrageparallelität anhäufen.

Wenn eine hohe Sitzungs- oder Workernutzung festgestellt wird, stehen folgende Optionen als Gegenmaßnahmen zur Verfügung:

- Erhöhen der Dienstebene oder Computegröße der Datenbank oder des Pools für elastische Datenbanken. Siehe [Skalieren der Ressourcen für einzelne Datenbanken](sql-database-single-database-scale.md) und [Skalieren der Ressourcen für elastische Pools in Azure SQL-Datenbank](sql-database-elastic-pool-scale.md).
- Optimieren von Abfragen, um die Ressourcenverwendung durch die einzelnen Abfragen zu verringern, wenn die Ursache der zunehmenden Auslastung durch Worker Konflikte bezüglich der Computeressourcen sind. Weitere Informationen finden Sie unter [Abfrageoptimierung/Abfragehinweise](sql-database-performance-guidance.md#query-tuning-and-hinting).
- Verringern der Einstellung [MAXDOP](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option#Guidelines) (maximaler Grad an Parallelität).
- Optimieren der Abfragearbeitsauslastung, um die Anzahl von Vorkommen und die Dauer der Abfrageblockierung zu verringern.

### <a name="resource-consumption-by-user-workloads-and-internal-processes"></a>Ressourcenauslastung durch Benutzerarbeitsauslastungen und interne Prozesse

Die CPU- und Arbeitsspeicherauslastung durch Benutzerarbeitsauslastungen in den einzelnen Datenbanken wird in den Spalten `avg_cpu_percent` und `avg_memory_usage_percent` der Sichten [sys.dm_db_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database?view=azuresqldb-current) und [sys.resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database?view=azuresqldb-current) gemeldet. Bei Pools für elastische Datenbanken wird die Ressourcenauslastung auf Poolebene in der Sicht [sys.elastic_pool_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database) gemeldet. Die CPU-Auslastung durch Benutzerarbeitsauslastungen wird auch über die Azure Monitor-Metrik `cpu_percent` gemeldet, und zwar für [Einzeldatenbanken](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported#microsoftsqlserversdatabases) und für [Pools für elastische Datenbanken](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported#microsoftsqlserverselasticpools) auf Poolebene.

Azure SQL-Datenbank benötigt Computeressourcen zur Implementierung zentraler Dienstfeatures wie Hochverfügbarkeit und Notfallwiederherstellung, Datenbanksicherung und -wiederherstellung, Überwachung, Abfragespeicher, automatische Optimierung und Ähnliches. Ein gewisser Teil der Gesamtressourcen wird mithilfe von Mechanismen der [Ressourcengovernance](#resource-governance) für diese internen Prozesse reserviert. Die übrigen Ressourcen stehen für Benutzerarbeitsauslastungen zur Verfügung. Wenn von internen Prozessen keine Computeressourcen beansprucht werden, werden die Ressourcen für Benutzerarbeitsauslastungen bereitgestellt.

Die gesamte CPU- und Arbeitsspeicherauslastung durch Benutzerarbeitsauslastungen und interne Prozesse der SQL Server-Instanz, die eine Einzeldatenbank oder einen Pool für elastische Datenbanken hostet, wird in den Spalten `avg_instance_cpu_percent` und `avg_instance_memory_percent` der Sichten [sys.dm_db_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database?view=azuresqldb-current) und [sys.resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database?view=azuresqldb-current) gemeldet. Diese Daten werden auch über die Azure Monitor-Metriken `sqlserver_process_core_percent` und `sqlserver_process_memory_percent` gemeldet, und zwar für [Einzeldatenbanken](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported#microsoftsqlserversdatabases) und für [Pools für elastische Datenbanken](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported#microsoftsqlserverselasticpools) auf Poolebene.

Eine detailliertere Aufschlüsselung der aktuellen Ressourcenauslastung durch Benutzerarbeitsauslastungen und interne Prozesse finden Sie in den Sichten [sys.dm_resource_governor_resource_pools_history_ex](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pools-history-ex-azure-sql-database) und [sys.dm_resource_governor_workload_groups_history_ex](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-resource-governor-workload-groups-history-ex-azure-sql-database). Ausführliche Informationen zu Ressourcenpools und Arbeitsauslastungsgruppen, auf die in diesen Sichten verwiesen wird, finden Sie unter [Ressourcengovernance](#resource-governance). In diesen Sichten wird die Ressourcenverwendung durch Benutzerarbeitsauslastungen und bestimmte interne Prozesse in den zugeordneten Ressourcenpools und Arbeitsauslastungsgruppen gemeldet.

Bei der Leistungsüberwachung und Problembehandlung muss sowohl die **benutzerspezifische CPU-Auslastung** (`avg_cpu_percent`, `cpu_percent`) als auch die **CPU-Gesamtauslastung** nach Benutzerarbeitsauslastungen und internen Prozessen (`avg_instance_cpu_percent`, `sqlserver_process_core_percent`) berücksichtigt werden.

Die **benutzerspezifische CPU-Auslastung** wird als Prozentsatz der Grenzwerte für Benutzerarbeitsauslastungen in den einzelnen Dienstzielen berechnet. Eine **benutzerspezifische CPU-Auslastung** von 100 Prozent gibt an, dass für die Benutzerarbeitsauslastung der Grenzwert des Dienstziels erreicht wurde. Wenn sich jedoch die **CPU-Gesamtauslastung** im Bereich zwischen 70 und 100 Prozent bewegt, kann es zu einer Abflachung des Durchsatzes für Benutzerarbeitsauslastungen sowie zu einer Erhöhung der Abfragewartezeit kommen, auch wenn die gemeldete **benutzerspezifische CPU-Auslastung** deutlich unter 100 Prozent liegt. Dieses Verhalten wird durch die Verwendung kleinerer Dienstziele mit einer moderaten Zuordnung von Computeressourcen, aber relativ intensiven Benutzerarbeitsauslastungen (etwa in [umfangreichen Pools für elastische Datenbanken](sql-database-elastic-pool-resource-management.md)) begünstigt. Es kann aber auch bei kleineren Dienstzielen auftreten, wenn interne Prozesse vorübergehend zusätzliche Ressourcen benötigen (etwa beim Erstellen eines neuen Replikats der Datenbank).

Bei einer hohen **CPU-Gesamtauslastung** stehen die gleichen Abhilfeoptionen zur Verfügung, die weiter oben bereits erwähnt wurden. Hierzu zählen unter anderem die Erhöhung des Dienstziels und/oder die Optimierung der Benutzerarbeitsauslastung.

## <a name="resource-governance"></a>Ressourcengovernance

Zur Erzwingung von Ressourcenlimits verwendet Azure SQL-Datenbank eine auf SQL Server [Resource Governor](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor) basierende Ressourcengovernanceimplementierung, die geändert und erweitert wurde, um einen SQL Server-Datenbankdienst in Azure auszuführen. Jede SQL Server-Instanz im Dienst umfasst mehrere [Ressourcenpools](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor-resource-pool) und [Arbeitsauslastungsgruppen](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor-workload-group), und die Ressourcenlimits werden auf Pool- und Gruppenebene festgelegt, um eine [ausgeglichene Database-as-a-Service-Lösung](https://azure.microsoft.com/blog/resource-governance-in-azure-sql-database/) bereitzustellen. Die Benutzerarbeitsauslastung und interne Workloads werden in separate Ressourcenpools und Arbeitsauslastungsgruppen eingeteilt. Die Benutzerarbeitsauslastung auf den primären und lesbaren sekundären Replikaten, einschließlich Georeplikaten, wird als Ressourcenpool `SloSharedPool1` und Arbeitsauslastungsgruppe `UserPrimaryGroup.DBId[N]` klassifiziert, wobei `N` für den Datenbank-ID-Wert steht. Außerdem gibt es mehrere Ressourcenpools und Arbeitsauslastungsgruppen für verschiedene interne Workloads.

Zusätzlich zur Verwendung von Resource Governor zum Steuern von Ressourcen innerhalb des SQL Server-Prozesses verwendet Azure SQL-Datenbank auch Windows-[Auftragsobjekte](https://docs.microsoft.com/windows/win32/procthread/job-objects) für die Ressourcenkontrolle auf Prozessebene und den [Ressourcen-Manager für Dateiserver (File Server Resource Manager, FSRM)](https://docs.microsoft.com/windows-server/storage/fsrm/fsrm-overview) von Windows für die Verwaltung von Speicherkontingenten.

Die Azure SQL-Datenbank-Ressourcenkontrolle ist hierarchisch strukturiert. Grenzwerte werden von oben nach unten auf Betriebssystemebene und auf Speichervolumeebene mithilfe von Betriebssystem-Ressourcenkontrollmechanismen und Resource Governor, dann auf Ressourcenpoolebene mithilfe von Resource Governor und dann auf Arbeitsauslastungsgruppenebene mithilfe von Resource Governor erzwungen. Die für die aktuelle Datenbank oder den Pool für elastische Datenbanken geltenden Grenzwerte der Ressourcenkontrolle sind in der Ansicht [sys.dm_user_db_resource_governance](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-user-db-resource-governor-azure-sql-database) angegeben. 

### <a name="data-io-governance"></a>Daten-E/A-Governance

In Azure SQL-Datenbank ist die Daten-E/A-Governance ein Prozess zum Begrenzen der physischen E/A-Lese- und -Schreibvorgänge für Datendateien einer Datenbank. IOPS-Grenzwerte werden für jeden Servicelevel festgelegt, um den „Noisy Neighbor“-Effekt zu minimieren, eine gerechte Ressourcenzuordnung im mehrinstanzenfähigen Dienst zu gewährleisten und die Funktionen der zugrunde liegenden Hardware und Speicher zu unterstützen.

Bei Einzeldatenbanken gelten die Grenzwerte für Arbeitsauslastungsgruppen für alle Speicher-E/A-Vorgänge für die Datenbank, während die Grenzwerte für Ressourcenpools für alle Speicher-E/A-Vorgänge für alle Datenbanken in derselben SQL Server-Instanz (einschließlich der Datenbank `tempdb`) gelten. Bei Pools für elastische Datenbanken gelten die Grenzwerte für Arbeitsauslastungsgruppen für jede Datenbank im Pool, während die Grenzwerte für Ressourcenpools für den gesamten Pool für elastische Datenbanken gelten, einschließlich der Datenbank `tempdb`, die von allen Datenbanken im Pool gemeinsam genutzt wird. Im Allgemeinen können die Grenzwerte für Ressourcenpools nicht von der Workload einer Datenbank (entweder einzeln oder in einem Pool) erreicht werden, weil die Grenzwerte für Arbeitsauslastungsgruppen niedriger sind als die Grenzwerte für Ressourcenpools und IOPS/den Durchsatz früher begrenzen. Poolgrenzwerte können jedoch von der kombinierten Workload mehrerer Datenbanken auf derselben SQL Server-Instanz erreicht werden.

Beispiel: Wenn eine Abfrage 1.000 IOPS ohne E/A-Ressourcenkontrolle generiert, aber der maximale IOPS-Grenzwert für die Arbeitsauslastungsgruppe auf 900 IOPS festgelegt ist, kann die Abfrage nicht mehr als 900 IOPS generieren. Wenn der maximale IOPS-Grenzwert für den Ressourcenpool jedoch auf 1.500 IOPS festgelegt ist und die Gesamtanzahl der E/A-Vorgänge aller dem Ressourcenpool zugeordneten Arbeitsauslastungsgruppen 1.500 IOPS überschreitet, kann die Anzahl der E/A-Vorgänge der gleichen Abfrage so reduziert werden, dass sie unter dem Grenzwert von 900 IOPS liegt.

Die von der Ansicht [sys.dm_user_db_resource_governance](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-user-db-resource-governor-azure-sql-database) zurückgegebenen Mindest- und Höchstwerte für IOPS und Durchsatz gelten als Grenzwerte/Obergrenzen, nicht als Garantien. Darüber hinaus garantiert die Ressourcenkontrolle keine bestimmten Speicherlatenzen. Die bestmöglichen erreichbaren Latenzen, IOPS und Durchsatzwerte für eine bestimmte Benutzerarbeitsauslastung hängen nicht nur von den Grenzwerten der E/A-Ressourcenkontrolle, sondern auch von der Mischung der verwendeten E/A-Größen und den Funktionen des zugrunde liegenden Speichers ab. SQL Server verwendet E/As in Größenordnungen zwischen 512 KB und 4 MB. Zur Erzwingung der IOPS-Grenzwerte wird jede E/A unabhängig von der Größe berücksichtigt, mit Ausnahme von Datenbanken mit Datendateien in Azure Storage. In diesem Fall werden E/As, die größer als 256 KB sind, als mehrere E/As mit 256 KB eingestuft, um sie mit der E/A-Auslastung von Azure Storage abzustimmen.

Bei Basic-, Standard- und universellen Datenbanken, die Datendateien in Azure Storage verwenden, kann der Wert `primary_group_max_io` möglicherweise nicht erreicht werden, wenn eine Datenbank nicht über genügend Datendateien verfügt, um diese Anzahl von IOPS kumulativ bereitzustellen, oder wenn die Daten nicht gleichmäßig auf die Dateien verteilt sind oder die Leistungsstufe der zugrunde liegenden BLOBs die IOPS/den Durchsatz auf einen Wert begrenzt, der unter dem Grenzwert der Ressourcenkontrolle liegt. Ebenso kann der Wert `primary_max_log_rate` bei kleinen Protokoll-E/As, die durch häufige Transaktionscommits generiert werden, aufgrund des IOPS-Grenzwerts des zugrunde liegenden Azure Storage-Blobs nicht von einer Workload erreicht werden.

In den Ansichten [sys.dm_db_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database), [sys.resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database) und [sys.elastic_pool_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database) erfasste Ressourcennutzungswerte wie `avg_data_io_percent` und `avg_log_write_percent` werden als Prozentsätze der maximalen Grenzwerte der Ressourcenkontrolle berechnet. Wenn andere Faktoren als die Ressourcenkontrolle die IOPS/den Durchsatz einschränken, ist es daher möglich, eine Verflachung der IOPS/des Durchsatzes und steigende Latenzen bei zunehmender Arbeitsauslastung zu beobachten, auch wenn die gemeldete Ressourcenauslastung unter 100% bleibt. 

Wenn Sie Werte für Lese- und Schreib-IOPS, Durchsatz und Latenz pro Datenbankdatei anzeigen möchten, verwenden Sie die Funktion [sys.dm_io_virtual_file_stats()](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-io-virtual-file-stats-transact-sql). Diese Funktion zeigt alle E/A-Vorgänge für die Datenbank an, einschließlich Hintergrund-E/As, die nicht für `avg_data_io_percent` berücksichtigt werden, aber IOPS und Durchsatz des zugrunde liegenden Speichers verwenden und sich auf die beobachtete Speicherlatenz auswirken können. Die Funktion zeigt auch zusätzliche Latenz, die möglicherweise von der E/A-Ressourcenkontrolle für Lese- und Schreibvorgänge in den Spalten `io_stall_queued_read_ms` und `io_stall_queued_write_ms` eingeführt wird.

### <a name="transaction-log-rate-governance"></a>Transaktionsprotokollratengovernance

Als Transaktionsprotokollratengovernance wird der Prozess in Azure SQL-Datenbank bezeichnet, der verwendet wird, um hohe Datenerfassungsraten für Workloads zu begrenzen, z. B. für Masseneinfügung (BULK INSERT), SELECT INTO und für die Indizierung. Diese Grenzwerte werden für die Rate, in der Protokolleinträge generiert werden, in Sekundenbruchteilen verfolgt und erzwungen. Dadurch wird der Durchsatz unabhängig von den E/A-Größen der Datendateien eingeschränkt.  Die Generierungsrate von Transaktionsprotokollen wird momentan bis zu einem hardwareabhängigen Punkt linear skaliert. Die maximal zulässige Protokollrate beträgt dabei für das vCore-basierte Kaufmodell 96 MB/s. 

> [!NOTE]
> Die tatsächlichen physischen E/A-Größen für Transaktionsprotokolldateien sind nicht gesteuert oder beschränkt.

Protokollraten werden so festgelegt, dass sie in einer Vielzahl von Szenarios erreicht und erhalten werden können, während das gesamte System seine Funktionalität mit minimalen Einbußen bei der Benutzerauslastung beibehalten kann. Die Protokollratengovernance sorgt dafür, dass sich Transaktionsprotokollsicherungen innerhalb veröffentlichter Vereinbarungen zum Servicelevel für die Wiederherstellbarkeit bewegen.  Diese Governance verhindert außerdem ein exzessives Backlog bei sekundären Replikaten.

Während die Protokolleinträge generiert werden, wird jeder Vorgang ausgewertet und es wird eingeschätzt, ob er verzögert werden soll, um eine maximal erwünschte Protokollrate aufrechtzuerhalten (MB/s). Die Verzögerungen werden nicht hinzugefügt, wenn die Protokolleinträge in den Speicher geleert werden. Die Protokollratengovernance wird eher während der Generierung der Protokollrate selbst angewendet.

Die tatsächlichen Protokollgenerierungsraten, die während der Laufzeit durchgesetzt werden, werden möglicherweise auch von Feedbackmechanismen beeinflusst. Dadurch wird die zulässige Protokollrate zeitweise reduziert, damit sich das System stabilisieren kann. Die Speicherplatzverwaltung für die Protokolldatei, mithilfe derer also vermieden wird, dass nicht mehr genug Protokollspeicherplatz sowie Verfügbarkeitsgruppenreplikationsmechanismen vorhanden sind, kann zeitweise zu einer Verringerung der gesamten Begrenzungen für das System führen.

Die Datenverkehrsmodellierung der Protokollratenbegrenzung erfolgt über die folgenden Wartetypen (über die Sichten [sys.dm_exec_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql) und [sys.dm_os_wait_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql) bereitgestellt):

| Wartetyp | Notizen |
| :--- | :--- |
| LOG_RATE_GOVERNOR | Datenbankbegrenzung |
| POOL_LOG_RATE_GOVERNOR | Poolbegrenzung |
| INSTANCE_LOG_RATE_GOVERNOR | Instanzebenenbegrenzung |  
| HADR_THROTTLE_LOG_RATE_SEND_RECV_QUEUE_SIZE | Feedbackkontrolle; physische Replikation von Verfügbarkeitsgruppen in den Tarifen „Premium“/„Unternehmenskritisch“ zu langsam |  
| HADR_THROTTLE_LOG_RATE_LOG_SIZE | Feedbacksteuerung; Raten werden beschränkt, um eine Situation zu vermeiden, in der der Speicherplatz für Protokolle ausgeht |
| HADR_THROTTLE_LOG_RATE_MISMATCHED_SLO | Feedbacksteuerung für Georeplikation, Begrenzen der Protokollrate, um hohe Datenlatenz und Nichtverfügbarkeit von sekundären Georeplikaten zu vermeiden.|
|||

Wenn es zu einer Begrenzung der Protokollrate zu kommen droht, die die gewünschte Skalierbarkeit beeinträchtigt, können Sie die folgenden Optionen in Erwägung ziehen:
- Skalieren Sie zentral auf einen höheren Servicelevel hoch, um die maximale Protokollrate von 96 MB/s zu erhalten, oder wechseln Sie zu einer anderen Dienstebene. Die Dienstebene [Hyperscale](sql-database-service-tier-hyperscale.md) bietet unabhängig vom gewählten Servicelevel eine Protokollrate von 100 MB/s.
- Wenn die Daten, die geladen werden, kurzlebig sind, wie z. B. Stagingdaten in einem ETL-Prozess, können sie in eine tmpdb geladen werden (für die nur die minimal notwendigen Protokolle erstellt werden). 
- In Analyseszenarios kann das Laden in eine gruppierte von einem Columnstore abgedeckte Tabelle erfolgen. Dadurch wird die erforderliche Protokollrate über Komprimierung reduziert. Dieses Vorgehen erhöht jedoch die CPU-Auslastung und eignet sich nur für Datasets, die von gruppierten Columnstore-Indizes profitieren. 

## <a name="next-steps"></a>Nächste Schritte

- Informationen zu allgemeinen Azure-Einschränkungen finden Sie unter [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../azure-resource-manager/management/azure-subscription-service-limits.md).
- Informationen zu DTUs und eDTUs finden Sie unter [Datenbanktransaktionseinheiten (DTUs) und elastische Datenbanktransaktionseinheiten (eDTUs)](sql-database-purchase-models.md#dtu-based-purchasing-model).
- Informationen zu Größenbeschränkungen für tempdb finden Sie unter [Tempdb-Datenbank in SQL-Datenbank](https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database#tempdb-database-in-sql-database).
