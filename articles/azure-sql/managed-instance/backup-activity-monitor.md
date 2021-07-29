---
title: Überwachen der Sicherungsaktivität
titleSuffix: Azure SQL Managed Instance
description: Erfahren Sie, wie Sie die Sicherungsaktivität von Azure SQL Managed Instance mit erweiterten Ereignissen überwachen.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: backup-restore
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: Misliplavo
ms.author: mlazic
ms.reviewer: mathoma
ms.date: 12/14/2018
ms.openlocfilehash: 8c38b9d77baee05fe14c07dc5e6bc185bcb46538
ms.sourcegitcommit: 34feb2a5bdba1351d9fc375c46e62aa40bbd5a1f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111897179"
---
# <a name="monitor-backup-activity-for-azure-sql-managed-instance"></a>Überwachen der Sicherungsaktivität für Azure SQL Managed Instance 
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

In diesem Artikel erfahren Sie, wie Sie Sitzungen mit erweiterten Ereignissen (XEvent) konfigurieren, um die Sicherungsaktivität für [Azure SQL Managed Instance](sql-managed-instance-paas-overview.md) zu überwachen. 

## <a name="overview"></a>Übersicht

Azure SQL Managed Instance gibt während der Sicherungsaktivität Ereignisse (sogenannte [erweiterte Ereignisse oder XEvents](../database/xevent-db-diff-from-svr.md)) für die Berichterstellung aus. Konfigurieren Sie eine XEvent-Sitzung, um Informationen wie Sicherungsstatus, Sicherungstyp, Größe, Uhrzeit und Speicherort in der msdb-Datenbank nachzuverfolgen. Diese Informationen können in die Sicherungsüberwachungssoftware integriert und auch für die Unternehmensaudits verwendet werden. 

Bei Unternehmensaudits ist möglicherweise der Nachweis erfolgreicher Sicherungen mit dem Zeitpunkt und der Dauer der Sicherung erforderlich.

## <a name="configure-xevent-session"></a>Konfigurieren einer XEvent-Sitzung

Verwenden Sie das erweiterte `backup_restore_progress_trace`-Ereignis, um den Status Ihrer SQL Managed Instance-Sicherung aufzuzeichnen. Ändern Sie die XEvent-Sitzungen nach Bedarf, um die Informationen nachzuverfolgen, die Sie für Ihr Unternehmen benötigen. Diese T-SQL-Codeausschnitte speichern die XEvent-Sitzungen im Ringpuffer, es ist aber auch möglich, in [Azure Blob Storage](../database/xevent-code-event-file.md) zu schreiben. XEvent-Sitzungen, die Daten im Ringpuffer speichern, sind auf ca. 1.000 Nachrichten beschränkt und sollten daher nur zum Nachverfolgen aktueller Aktivitäten verwendet werden. Darüber hinaus gehen Daten im Ringpuffer bei einem Failover verloren. Schreiben Sie daher für die Speicherung von Verlaufsdaten zu Sicherungen stattdessen in eine Ereignisdatei. 

### <a name="simple-tracking"></a>Einfaches Nachverfolgen

Konfigurieren Sie eine einfache XEvent-Sitzung, um einfache Ereignisse zu vollständig abgeschlossenen Sicherungen zu erfassen. Dieses Skript sammelt den Namen der Datenbank, die Gesamtanzahl der verarbeiteten Bytes und die Uhrzeit, zu der die Sicherung abgeschlossen wurde.

Verwenden Sie Transact-SQL (T-SQL), um die einfache XEvent-Sitzung zu konfigurieren: 


```sql
CREATE EVENT SESSION [Simple backup trace] ON SERVER
ADD EVENT sqlserver.backup_restore_progress_trace(
WHERE operation_type = 0
AND trace_message LIKE '%100 percent%')
ADD TARGET package0.ring_buffer
WITH(STARTUP_STATE=ON)
GO
ALTER EVENT SESSION [Simple backup trace] ON SERVER
STATE = start;
```



### <a name="verbose-tracking"></a>Ausführliche Nachverfolgung

Konfigurieren Sie eine ausführliche XEvent-Sitzung, um mehr Details zur Sicherungsaktivität nachzuverfolgen. Dieses Skript erfasst Start und Ende von vollständigen, differenziellen und Protokollsicherungen. Da dieses Skript ausführlicher ist, füllt es den Ringpuffer schneller auf, sodass Einträge möglicherweise schneller bereinigt werden als beim einfachen Skript. 

Verwenden Sie Transact-SQL (T-SQL), um die ausführliche XEvent-Sitzung zu konfigurieren: 

```sql
CREATE EVENT SESSION [Verbose backup trace] ON SERVER 
ADD EVENT sqlserver.backup_restore_progress_trace(
    WHERE (
              [operation_type]=(0) AND (
              [trace_message] like '%100 percent%' OR 
              [trace_message] like '%BACKUP DATABASE%' OR [trace_message] like '%BACKUP LOG%'))
       )
ADD TARGET package0.ring_buffer
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,
       MAX_DISPATCH_LATENCY=30 SECONDS,MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,
       TRACK_CAUSALITY=OFF,STARTUP_STATE=ON)

ALTER EVENT SESSION [Verbose backup trace] ON SERVER
STATE = start;

```

## <a name="monitor-backup-progress"></a>Überwachen des Sicherungsstatus 

Nachdem die XEvent-Sitzung erstellt wurde, können Sie Transact-SQL (T-SQL) verwenden, um Ringpufferergebnisse abzufragen und den Status der Sicherung zu überwachen. Nachdem das XEvent gestartet wurde, werden alle Sicherungsereignisse gesammelt, sodass der Sitzung ungefähr alle 5 bis 10 Minuten Einträge hinzugefügt werden.  

### <a name="simple-tracking"></a>Einfaches Nachverfolgen

Der folgende Transact-SQL-Code (T-SQL) fragt die einfache XEvent-Sitzung ab und gibt den Namen der Datenbank, die Gesamtanzahl der verarbeiteten Bytes und die Uhrzeit zurück, zu der die Sicherung abgeschlossen wurde: 

```sql 
WITH
a AS (SELECT xed = CAST(xet.target_data AS xml)
FROM sys.dm_xe_session_targets AS xet
JOIN sys.dm_xe_sessions AS xe
ON (xe.address = xet.event_session_address)
WHERE xe.name = 'Backup trace'),
b AS(SELECT
d.n.value('(@timestamp)[1]', 'datetime2') AS [timestamp],
ISNULL(db.name, d.n.value('(data[@name="database_name"]/value)[1]', 'varchar(200)')) AS database_name,
d.n.value('(data[@name="trace_message"]/value)[1]', 'varchar(4000)') AS trace_message
FROM a
CROSS APPLY  xed.nodes('/RingBufferTarget/event') d(n)
LEFT JOIN master.sys.databases db
ON db.physical_database_name = d.n.value('(data[@name="database_name"]/value)[1]', 'varchar(200)'))
SELECT * FROM b
```

Der folgende Screenshot zeigt ein Beispiel für die Ausgabe des obigen Skripts: 

![Screenshot der XEvent-Ausgabe](./media/backup-activity-monitor/present-xevents-output.png)

In diesem Beispiel wurden fünf Datenbanken innerhalb von 2 Stunden und 30 Minuten automatisch gesichert, und die XEvent-Sitzung umfasst 130 Einträge. 

### <a name="verbose-tracking"></a>Ausführliche Nachverfolgung 

Der folgende Transact-SQL-Code (T-SQL) fragt die ausführliche XEvent-Sitzung ab und gibt den Namen der Datenbank sowie den Start und das Ende von vollständigen, differenziellen und Protokollsicherungen zurück. 


```sql
WITH
a AS (SELECT xed = CAST(xet.target_data AS xml)
FROM sys.dm_xe_session_targets AS xet
JOIN sys.dm_xe_sessions AS xe
ON (xe.address = xet.event_session_address)
WHERE xe.name = 'Verbose backup trace'),
b AS(SELECT
d.n.value('(@timestamp)[1]', 'datetime2') AS [timestamp],
ISNULL(db.name, d.n.value('(data[@name="database_name"]/value)[1]', 'varchar(200)')) AS database_name,
d.n.value('(data[@name="trace_message"]/value)[1]', 'varchar(4000)') AS trace_message
FROM a
CROSS APPLY  xed.nodes('/RingBufferTarget/event') d(n)
LEFT JOIN master.sys.databases db
ON db.physical_database_name = d.n.value('(data[@name="database_name"]/value)[1]', 'varchar(200)'))
SELECT * FROM b
```

Der folgende Screenshot zeigt ein Beispiel für eine vollständige Sicherung in der XEvent-Sitzung:

:::image type="content" source="media/backup-activity-monitor/output-with-full.png" alt-text="XEvent-Ausgabe bei vollständigen Sicherungen":::

Der folgende Screenshot zeigt ein Beispiel für die Ausgabe einer differenziellen Sicherung in der XEvent-Sitzung:

:::image type="content" source="media/backup-activity-monitor/output-with-differential.png" alt-text="XEvent-Ausgabe bei differenziellen Sicherungen":::


## <a name="next-steps"></a>Nächste Schritte

Nachdem die Sicherung abgeschlossen ist, können Sie eine [Zeitpunktwiederherstellung durchführen](point-in-time-restore.md) oder eine [Richtlinie für die Langzeitaufbewahrung konfigurieren](long-term-backup-retention-configure.md). 

Weitere Informationen finden Sie unter [Automatisierte Sicherungen](../database/automated-backups-overview.md). 
