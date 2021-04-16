---
title: Konfigurieren des maximalen Grads an Parallelität (max degree of parallelism, MAXDOP)
titleSuffix: Azure SQL Database
description: Hier erfahren Sie etwas zum maximalen Grad an Parallelität (max degree of parallelism, MAXDOP).
ms.date: 03/29/2021
services: sql-database
dev_langs:
- TSQL
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: tsql
ms.topic: conceptual
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.reviewer: ''
ms.openlocfilehash: 31ddf15975abdce70ea02b5de64ea5611e7e72b3
ms.sourcegitcommit: 5fd1f72a96f4f343543072eadd7cdec52e86511e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/01/2021
ms.locfileid: "106111118"
---
# <a name="configure-the-max-degree-of-parallelism-maxdop-in-azure-sql-database"></a>Konfigurieren des maximalen Grads an Parallelität (max degree of parallelism, MAXDOP) in Azure SQL-Datenbank
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

  In diesem Artikel wird der **maximale Grad an Parallelität (max degree of parallelism, MAXDOP)** in der Azure SQL-Datenbank beschrieben und erläutert, wie er konfiguriert werden kann. 

> [!NOTE]
> **Der folgende Inhalt konzentriert sich auf Azure SQL-Datenbank.** Weil Azure SQL-Datenbank auf der letzten stabilen Version der Microsoft SQL Server-Datenbank-Engine basiert, sind die Inhalte größtenteils ähnlich. Allerdings kann es Unterschiede bei den Problembehandlungs- und Konfigurationsoptionen geben. Weitere Informationen zu MAXDOP in SQL Server finden Sie unter [Konfigurieren der Serverkonfigurationsoption „Max. Grad an Parallelität“](/sql/database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option).

## <a name="overview"></a>Übersicht
  In Azure SQL-Datenbank ist die Standardeinstellung „MAXDOP“ für jede neue Einzeldatenbank (Singleton) und jeden neuen Pool für elastische Datenbanken auf „8“ festgelegt. Dies bedeutet, dass die Datenbank-Engine Abfragen möglicherweise mithilfe mehrerer Threads ausführt. Anders als bei SQL Server, bei dem der standardmäßige serverweite MAXDOP-Wert gleich „0“ (unbegrenzt) ist, werden neue Datenbanken in Azure SQL-Datenbank standardmäßig auf „MAXDOP 8“ festgelegt. Diese Standardeinstellung verhindert unnötige Ressourcenverwendung und sorgt für eine einheitliche Benutzerfreundlichkeit. Normalerweise ist es nicht erforderlich, den MAXDOP-Wert in Azure SQL-Datenbank-Workloads weiter zu konfigurieren, doch dies bietet möglicherweise Vorteile als erweiterte Übung zur Leistungsoptimierung.

> [!Note]
>   Im September 2020 wurde auf der Grundlage von Jahren der Telemetrie im Azure SQL-Datenbankdienst [MAXDOP 8](https://techcommunity.microsoft.com/t5/azure-sql/changing-default-maxdop-in-azure-sql-database-and-azure-sql/ba-p/1538528) als Standardwert für neue Datenbanken ausgewählt – ein optimaler Wert für die größte Vielfalt von Kundenworkloads. Diese Standardeinstellung hat geholfen, Leistungsprobleme aufgrund von übermäßiger Parallelität zu vermeiden. Vorher war die Standardeinstellung für neue Datenbanken „MAXDOP 0“. Die datenbankbezogene Konfigurationsoption „MAXDOP“ wurde bei Datenbanken, die vor September 2020 erstellt worden waren, nicht geändert.

  Im Allgemeinen ist die Ausführungszeit schneller, wenn die Datenbank-Engine die Ausführung einer Abfrage mit Parallelität auswählt. Allerdings kann übermäßige Parallelität übermäßige Prozessorressourcen verbrauchen, ohne die Abfrageleistung zu verbessern. Bei der Skalierung kann sich übermäßige Parallelität negativ auf die Abfrageleistung für alle Abfragen auswirken, die auf derselben Datenbank-Engine-Instanz ausgeführt werden. Deshalb war das Festlegen einer oberen Grenze für Parallelität eine allgemeine Übung zur Leistungsoptimierung in SQL Server-Workloads.

  In der folgenden Tabelle wird das Verhalten der Datenbank-Engine bei der Ausführung von Abfragen mit verschiedenen MAXDOP-Werten beschrieben:

| MAXDOP | Verhalten | 
|--|--|
| = 1 | Die Datenbank-Engine führt keine Abfragen aus, die mehrere gleichzeitige Threads verwenden. | 
| > 1 | Die Datenbank-Engine legt eine obere Grenze für die Anzahl paralleler Threads fest. Die Datenbank-Engine wählt die Anzahl der zu verwendenden zusätzlichen Arbeitsthreads aus. Die Gesamtzahl der Arbeitsthreads, die zum Ausführen einer Abfrage verwendet werden, kann höher als der angegebene MAXDOP-Wert sein. |
| = 0 | Die Datenbank-Engine kann eine Reihe paralleler Threads mit einer oberen Grenze verwenden, die von der Gesamtzahl der logischen Prozessoren abhängig ist. Die Datenbank-Engine wählt die Anzahl von parallelen Threads aus, die verwendet werden sollen.| 
| | |
  
##  <a name="considerations"></a><a name="Considerations"></a> Weitere Überlegungen  

-   In Azure SQL-Datenbank können Sie den MAXDOP-Standardwert ändern:
    -   Verwenden Sie auf Abfrageebene hierfür den [Abfragehinweis](/sql/t-sql/queries/hints-transact-sql-query) für **MAXDOP**.     
    -   Auf Datenbankebene verwenden Sie die [auf die Datenbank beschränkte Konfiguration](/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) für **MAXDOP**.

-   Überlegungen und [Empfehlungen](/sql/database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option#Guidelines) für einen langfristigen SQL Server-MAXDOP-Wert gelten für Azure SQL-Datenbank. 

-   Der MAXDOP-Wert wird pro [Task](/sql/relational-databases/system-dynamic-management-views/sys-dm-os-tasks-transact-sql) erzwungen. Er wird nicht pro [Anforderung](/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql) oder pro „Abfrage“ erzwungen. Dies bedeutet, dass eine einzelne Anforderung während einer parallelen Abfrageausführung mehrere Tasks mit einer durch den MAXDOP-Wert festgelegten oberen Grenze erzeugen kann. Weitere Informationen finden Sie im [Handbuch zur Thread- und Taskarchitektur](/sql/relational-databases/thread-and-task-architecture-guide) im Abschnitt *Planen von parallelen Tasks*. 
  
-   Indizierungsoperationen, bei denen ein Index erstellt oder neu aufgebaut wird bzw. an deren Ende ein gruppierter Index steht, können ressourcenintensiv sein. Sie können den Wert „maximaler Grad an Parallelität“ der Datenbank für Indexvorgänge außer Kraft setzen, indem Sie die MAXDOP-Indexoption in der Anweisung `CREATE INDEX` oder `ALTER INDEX` angeben. Der Wert MAXDOP wird zur Ausführungszeit auf die Anweisung angewendet und wird nicht in den Metadaten für den Index gespeichert. Weitere Informationen finden Sie unter [Konfigurieren von Parallelindexvorgängen](/sql/relational-databases/indexes/configure-parallel-index-operations).  
  
-   Zusätzlich zu Abfragen und Indexvorgängen steuert die datenbankbezogene Konfigurationsoption auch die Parallelität von DBCC CHECKTABLE, DBCC CHECKDB und DBCC CHECKFILEGROUP. 

##  <a name="recommendations"></a><a name="Security"></a> Empfehlungen  

  Das Ändern von MAXDOP für die Datenbank kann sich auf die Abfrageleistung und die Ressourcenverwendung erheblich auswirken – sowohl positiv als auch negativ. Es gibt jedoch keinen einzelnen MAXDOP-Wert, der für alle Workloads optimal ist. Die Empfehlungen zum Festlegen von MAXDOP sind differenziert und abhängig von vielen Faktoren. 

  Einige gleichzeitige Spitzenworkloads funktionieren mit einem anderen MAXDOP-Wert möglicherweise besser als andere. Ein ordnungsgemäß konfigurierter MAXDOP-Wert sollte das Risiko von Leistungs- und Verfügbarkeitsvorfällen verringern und in einigen Fällen die Kosten senken, indem eine unnötige Ressourcenverwendung vermieden und so zu einem niedrigeren Dienstziel herunterskaliert werden kann.

### <a name="excessive-parallelism"></a>Übermäßige Parallelität

  Ein höherer MAXDOP-Wert verkürzt oft die Dauer für CPU-intensive Abfragen. Übermäßige Parallelität kann jedoch die Leistung anderer gleichzeitiger Workloads verschlechtern, indem andere Abfragen der CPU- und Arbeitsthreadressourcen „verhungern“. In Extremfällen kann eine übermäßige Parallelität sämtliche Ressourcen der Datenbank oder des Pools für elastische Datenbanken verbrauchen, wodurch Abfragetimeouts, Fehler und Anwendungsausfälle verursacht werden. 

  Wir empfehlen, dass Kunden den MAXDOP-Wert „0“ selbst dann vermeiden, wenn er zurzeit anscheinend keine Probleme verursacht. Übermäßige Parallelität wird sehr problematisch, wenn die CPU- und Arbeitsthreads mehr gleichzeitige Anforderungen empfangen, als vom Dienstziel unterstützt werden können. Vermeiden Sie „MAXDOP 0“, um das Risiko potenzieller zukünftiger Probleme aufgrund von übermäßiger Parallelität zu verringern, wenn eine Datenbank zentral hochskaliert wird oder wenn zukünftige Hardwaregenerationen in Azure SQL-Datenbank mehr Kerne für dasselbe Datenbankdienstziel bereitstellen.

### <a name="modifying-maxdop"></a>Ändern von MAXDOP 

  Wenn Sie feststellen, dass eine andere MAXDOP-Einstellung für Ihre Azure SQL-Datenbank-Workload optimal ist, können Sie die T-SQL-Anweisung `ALTER DATABASE SCOPED CONFIGURATION` verwenden. Beispiele dazu finden Sie unten im Abschnitt [Beispiele zur Verwendung von Transact-SQL](#examples). Fügen Sie diesen Schritt dem Bereitstellungsprozess hinzu, um MAXDOP nach Erstellung der Datenbank zu ändern.

  Wenn der Nicht-Standardwert für MAXDOP nur eine Teilmenge der Abfragen in der Workload nutzt, können Sie MAXDOP auf der Abfrageebene außer Kraft setzen, indem Sie den Hinweis „OPTION (MAXDOP)“ hinzufügen. Beispiele dazu finden Sie unten im Abschnitt [Beispiele zur Verwendung von Transact-SQL](#examples). 

  Testen Sie Ihre MAXDOP-Konfigurationsänderungen gründlich mit Auslastungstests, die realistische gleichzeitige Abfragelasten umfassen. 

  Der MAXDOP-Wert für das primäre und das sekundäre Replikat kann unabhängig konfiguriert werden, um unterschiedliche optimale MAXDOP-Einstellungen für Workloads mit Lese-/Schreibzugriff und schreibgeschützte Workloads zu nutzen. Dies gilt für Azure SQL-Datenbanken mit [horizontaler Leseskalierung](read-scale-out.md), [Georeplikation](active-geo-replication-overview.md) und [sekundären Replikaten für Hyperscale in Azure SQL-Datenbank](service-tier-hyperscale.md). Standardmäßig erben alle sekundären Replikate die MAXDOP-Konfiguration des primären Replikats.

## <a name="security"></a><a name="Security"></a> Sicherheit  
  
###  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
  Die Anweisung `ALTER DATABASE SCOPED CONFIGURATION` muss als Serveradministrator, als Mitglied der Datenbankrolle `db_owner` oder als Benutzer, dem die Berechtigung `ALTER ANY DATABASE SCOPED CONFIGURATION` erteilt wurde, ausgeführt werden.
 
## <a name="examples"></a>Beispiele   

  In diesen Beispielen wird die neueste **AdventureWorksLT**-Beispieldatenbank verwendet, wenn die Option `SAMPLE` für eine neue Einzeldatenbank (Singleton) von Azure SQL-Datenbank ausgewählt wird.

### <a name="powershell"></a>PowerShell

#### <a name="maxdop-database-scoped-configuration"></a>Datenbankbezogene MAXDOP-Konfiguration   

  In diesem Beispiel wird gezeigt, wie die Anweisung [ALTER DATABASE SCOPED CONFIGURATION](/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) verwendet wird, um für die Option `max degree of parallelism` den Wert `2` zu konfigurieren. Die Einstellung wird sofort wirksam. Das PowerShell-Cmdlet [Invoke-SqlCmd](/powershell/module/sqlserver/invoke-sqlcmd) führt die T-SQL-Abfragen aus, um die datenbankbezogene MAXDOP-Konfiguration festzulegen und dann zurückzugeben. 

```powershell
$dbName = "sample" 
$serverName = <server name here>
$serveradminLogin = <login here>
$serveradminPassword = <password here>
$desiredMAXDOP = 8

$params = @{
    'database' = $dbName
    'serverInstance' =  $serverName
    'username' = $serveradminLogin
    'password' = $serveradminPassword
    'outputSqlErrors' = $true
    'query' = 'ALTER DATABASE SCOPED CONFIGURATION SET MAXDOP = ' + $desiredMAXDOP + ';
     SELECT [value] FROM sys.database_scoped_configurations WHERE [name] = ''MAXDOP'';'
  }
  Invoke-SqlCmd @params
```

Dieses Beispiel ist zur Verwendung bei Azure SQL-Datenbanken mit [aktivierten Replikaten für horizontale Leseskalierung](read-scale-out.md), [Georeplikation](active-geo-replication-overview.md) und [sekundäre Replikate für Hyperscale in Azure SQL-Datenbank ](service-tier-hyperscale.md) vorgesehen. Beispielsweise wird das primäre Replikat auf einen anderen MAXDOP-Standardwert als das sekundäre Replikat festgelegt. Dabei wird erwartet, dass es Unterschiede zwischen einem Workload mit Lese-/Schreibzugriff und einem schreibgeschützten Workload gibt.

```powershell
$dbName = "sample" 
$serverName = <server name here>
$serveradminLogin = <login here>
$serveradminPassword = <password here>
$desiredMAXDOP_primary = 8
$desiredMAXDOP_secondary_readonly = 1
 
$params = @{
    'database' = $dbName
    'serverInstance' =  $serverName
    'username' = $serveradminLogin
    'password' = $serveradminPassword
    'outputSqlErrors' = $true
    'query' = 'ALTER DATABASE SCOPED CONFIGURATION SET MAXDOP = ' + $desiredMAXDOP + ';
    ALTER DATABASE SCOPED CONFIGURATION FOR SECONDARY SET MAXDOP = ' + $desiredMAXDOP_secondary_readonly + ';
    SELECT [value], value_for_secondary FROM sys.database_scoped_configurations WHERE [name] = ''MAXDOP'';'
  }
  Invoke-SqlCmd @params
```

### <a name="transact-sql"></a>Transact-SQL
  
  Sie können mit dem [Azure-Portal-Abfrage-Editor](connect-query-portal.md), [SQL Server Management Studio (SSMS)](/sql/ssms/download-sql-server-management-studio-ssms) oder [Azure Data Studio](/sql/azure-data-studio/download-azure-data-studio) T-SQL-Abfragen für Ihre Azure SQL-Datenbank ausführen.

1.  Stellen Sie eine Verbindung zur Azure SQL-Datenbank her. Sie können die datenbankbezogenen Konfigurationen in der Masterdatenbank nicht ändern.
  
2.  Klicken Sie in der Standardleiste auf **Neue Abfrage**.   
  
3.  Kopieren Sie das folgende Beispiel in das Abfragefenster, und klicken Sie dann auf **Ausführen**. 


#### <a name="maxdop-database-scoped-configuration"></a>Datenbankbezogene MAXDOP-Konfiguration

  In diesem Beispiel wird gezeigt, wie die aktuelle datenbankbezogene MAXDOP-Datenbankkonfiguration mithilfe der Systemkatalogsicht [sys.database_scoped_configurations](/sql/relational-databases/system-catalog-views/sys-database-scoped-configurations-transact-sql) ermittelt wird.

```sql
SELECT [value] FROM sys.database_scoped_configurations WHERE [name] = 'MAXDOP';
```

  In diesem Beispiel wird gezeigt, wie die Anweisung [ALTER DATABASE SCOPED CONFIGURATION](/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) verwendet wird, um für die Option `max degree of parallelism` den Wert `8` zu konfigurieren. Die Einstellung wird sofort wirksam.  
  
```sql  
ALTER DATABASE SCOPED CONFIGURATION SET MAXDOP = 8;
```  

Dieses Beispiel ist zur Verwendung bei Azure SQL-Datenbanken mit [aktivierten Replikaten für horizontale Leseskalierung](read-scale-out.md), [Georeplikation](active-geo-replication-overview.md) und [sekundären Replikaten für Hyperscale in Azure SQL-Datenbank ](service-tier-hyperscale.md) vorgesehen. Beispielsweise wird das primäre Replikat auf einen anderen MAXDOP-Standardwert als das sekundäre Replikat festgelegt. Dabei wird erwartet, dass es Unterschiede zwischen einem Workload mit Lese-/Schreibzugriff und einem schreibgeschützten Workload gibt. Die Spalte `value_for_secondary` von `sys.database_scoped_configurations` enthält Einstellungen für das sekundäre Replikat.

```sql
ALTER DATABASE SCOPED CONFIGURATION SET MAXDOP = 8;
ALTER DATABASE SCOPED CONFIGURATION FOR SECONDARY SET MAXDOP = 1;
SELECT [value], value_for_secondary FROM sys.database_scoped_configurations WHERE [name] = 'MAXDOP';
```

#### <a name="maxdop-query-hint"></a>MAXDOP-Abfragehinweis

  In diesem Beispiel wird gezeigt, wie eine Abfrage mithilfe des Abfragehinweises ausgeführt wird, um für `max degree of parallelism` den Wert `2` zu erzwingen.  

```sql 
SELECT ProductID, OrderQty, SUM(LineTotal) AS Total  
FROM SalesLT.SalesOrderDetail  
WHERE UnitPrice < 5  
GROUP BY ProductID, OrderQty  
ORDER BY ProductID, OrderQty  
OPTION (MAXDOP 2);    
GO
```
#### <a name="maxdop-index-option"></a>MAXDOP-Indexoption

  Dieses Beispiel zeigt, wie Sie einen Index mithilfe der Indexoption neu erstellen, um für `max degree of parallelism` den Wert `12` zu erzwingen.  

```sql 
ALTER INDEX ALL ON SalesLT.SalesOrderDetail 
REBUILD WITH 
   (     MAXDOP = 12
       , SORT_IN_TEMPDB = ON
       , ONLINE = ON);
```

## <a name="see-also"></a>Siehe auch  
* [ALTER DATABASE SCOPED CONFIGURATION (Transact-SQL)](/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql)        
* [sys.database_scoped_configurations (Transact-SQL)](/sql/relational-databases/system-catalog-views/sys-database-scoped-configurations-transact-sql)
* [Konfigurieren von Parallelindexvorgängen](/sql/relational-databases/indexes/configure-parallel-index-operations)    
* [Abfragehinweise &#40;Transact-SQL&#41;](/sql/t-sql/queries/hints-transact-sql-query)     
* [Festlegen von Indexoptionen](/sql/relational-databases/indexes/set-index-options)     
* [Verstehen und Beheben von Problemen durch Blockierungen in Azure SQL-Datenbank](understand-resolve-blocking.md)

## <a name="next-steps"></a>Nächste Schritte

* [Überwachen und Optimieren der Leistung](/sql/relational-databases/performance/monitor-and-tune-for-performance)
