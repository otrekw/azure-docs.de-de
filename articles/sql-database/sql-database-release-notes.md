---
title: Versionsinformationen
description: Weitere Informationen zu den neuen Features und Verbesserungen in Azure SQL-Datenbank und in der zugehörigen Dokumentation
services: sql-database
author: stevestein
ms.service: sql-database
ms.subservice: service
ms.devlang: ''
ms.topic: conceptual
ms.date: 05/13/2020
ms.author: sstein
ms.openlocfilehash: 3e5069c779cee0700bff6b2236f3cd36547fd623
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83659601"
---
# <a name="sql-database-release-notes"></a>Versionshinweise zu SQL-Datenbank

In diesem Artikel werden die SQL-Datenbank-Features aufgeführt, die sich derzeit in der Public Preview-Phase befinden. Informationen zu Updates und Verbesserungen für SQL-Datenbank finden Sie unter [Azure-Updates](https://azure.microsoft.com/updates/?product=sql-database). Informationen zu Updates und Verbesserungen für andere Azure-Dienste finden Sie unter [Dienstupdates](https://azure.microsoft.com/updates).

## <a name="features-in-public-preview"></a>Features in der öffentlichen Vorschau

### <a name="single-database"></a>[Einzeldatenbank](#tab/single-database)

| Funktion | Details |
| ---| --- |
| Neue Hardwaregenerationen der Serien Fsv2 und M| Weitere Informationen finden Sie unter [Hardwaregenerationen](sql-database-service-tiers-vcore.md#hardware-generations).|
| Beschleunigte Datenbankwiederherstellung bei Singletons und Pools für elastische Datenbanken | Weitere Informationen finden Sie unter [Schnellere Datenbankwiederherstellung](sql-database-accelerated-database-recovery.md).|
|Geschätzte eindeutige Anzahl|Weitere Informationen finden Sie unter [Geschätzte Abfrageverarbeitung](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing#approximate-query-processing).|
|Batchmodus für Rowstore (unter Kompatibilitätsgrad 150)|Weitere Informationen finden Sie unter [Batchmodus bei Rowstore](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing#batch-mode-on-rowstore).|
| Datenermittlung und -klassifizierung  |Weitere Informationen finden Sie unter [Azure SQL-Datenbank und SQL Data Warehouse: Datenermittlung und -klassifizierung](sql-database-data-discovery-and-classification.md).|
| Aufträge für die elastische Datenbank | Weitere Informationen finden Sie unter [Erstellen, Konfigurieren und Verwalten von Aufträgen für die elastische Datenbank](elastic-jobs-overview.md). |
| Elastische Abfragen | Weitere Informationen finden Sie unter [Übersicht über elastische Abfragen](sql-database-elastic-query-overview.md). |
| Elastische Transaktionen | [Verteilte Transaktionen über Clouddatenbanken](sql-database-elastic-transactions-overview.md). |
|Feedback zur Speicherzuweisung (Zeilenmodus) (unter Kompatibilitätsgrad 150)|Weitere Informationen finden Sie unter [Feedback zur Speicherzuweisung im Zeilenmodus](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing#row-mode-memory-grant-feedback).|
| Abfrage-Editor im Azure-Portal |Weitere Informationen finden Sie unter [Verwenden des SQL-Abfrage-Editors im Azure-Portal zum Verbinden und Abfragen von Daten](sql-database-connect-query-portal.md).|
| R-Dienste/maschinelles Lernen mit Singletons und Pools für elastische Datenbanken |Weitere Informationen finden Sie unter [Machine Learning Services in Azure SQL Database (Machine Learning Services in Azure SQL-Datenbank)](https://docs.microsoft.com/sql/advanced-analytics/what-s-new-in-sql-server-machine-learning-services?view=sql-server-2017#machine-learning-services-in-azure-sql-database).|
|Azure SQL-Analyse|Weitere Informationen finden Sie unter [Azure SQL-Analyse](../azure-monitor/insights/azure-sql.md).|
|Verzögerte Kompilierung von Tabellenvariablen (unter Kompatibilitätsgrad 150)|Weitere Informationen finden Sie unter [Verzögerte Kompilierung von Tabellenvariablen](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing#table-variable-deferred-compilation).|
| &nbsp; |

### <a name="managed-instance"></a>[Verwaltete Instanz](#tab/managed-instance)

| Funktion | Details |
| ---| --- |
| <a href="/azure/sql-database/sql-database-instance-pools">Instanzenpools</a> | Eine praktische und kostengünstige Möglichkeit, um kleinere SQL-Instanzen zur Cloud zu migrieren. |
| <a href="https://aka.ms/managed-instance-aadlogins">Azure AD-Serverprinzipale auf Instanzebene (Anmeldungen)</a> | Erstellen Sie Anmeldungen auf Serverebene mithilfe der Anweisung <a href="https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current">CREATE LOGIN FROM EXTERNAL PROVIDER</a>. |
| [Transaktionsreplikation](sql-database-managed-instance-transactional-replication.md) | Replizieren Sie die Änderungen aus Ihren Tabellen in andere Datenbanken, die in verwalteten Instanzen, Einzeldatenbanken oder SQL Server-Instanzen abgelegt wurden, oder aber aktualisieren Sie die Tabellen, wenn einige Zeilen in anderen verwalteten Instanzen oder einer SQL Server-Instanz geändert werden. Weitere Informationen finden Sie unter [Konfigurieren der Replikation in einer verwalteten Azure SQL-Datenbank-Instanzdatenbank](replication-with-sql-database-managed-instance.md). |
| Bedrohungserkennung |Weitere Informationen finden Sie unter [Konfigurieren der Bedrohungserkennung (Vorschau) für eine verwaltete Azure SQL-Datenbank-Instanz](sql-database-managed-instance-threat-detection.md).|
| Langfristiges Aufbewahren von Sicherungen | Weitere Informationen finden Sie unter [Konfigurieren der langfristigen Sicherungsaufbewahrung für verwaltete Azure SQL-Datenbank-Instanzen](sql-database-managed-instance-long-term-backup-retention-configure.md) (derzeit in der eingeschränkten Public Preview). | 

---

## <a name="managed-instance---new-features-and-known-issues"></a>Verwaltete Instanz – Neue Features und bekannte Probleme

### <a name="managed-instance-h2-2019-updates"></a>Verwaltete Instanz – H2 2019-Updates

- [Dienstgestützte Subnetzkonfiguration](https://azure.microsoft.com/updates/service-aided-subnet-configuration-for-managed-instance-in-azure-sql-database-available/) – Eine sichere und bequeme Möglichkeit zur Verwaltung der Subnetzkonfiguration, bei der Sie den Datenverkehr steuern, während die verwaltete Instanz den ununterbrochenen Fluss des Verwaltungsdatenverkehrs sicherstellt
- [Transparent Data Encryption (TDE) mit Bring Your Own Key (BYOK)](https://azure.microsoft.com/updates/general-avilability-transparent-data-encryption-with-customer-managed-keys-for-azure-sql-database-managed-instance/) – Ermöglicht ein „Bring Your Own Key (BYOK)“-Szenario für den Schutz von Daten im Ruhezustand, sodass Organisationen Verwaltungsaufgaben für Schlüssel und Daten trennen können
- [Gruppen für automatisches Failover](https://azure.microsoft.com/updates/azure-sql-database-auto-failover-groups-feature-now-available-in-all-regions/) ermöglichen es Ihnen, alle Datenbanken aus der primären Instanz in eine sekundäre Instanz in einer anderen Region zu replizieren.
- Konfigurieren Sie das Verhalten Ihrer verwalteten Instanz mit [globalen Ablaufverfolgungsflags](https://azure.microsoft.com/updates/global-trace-flags-are-now-available-in-azure-sql-database-managed-instance/).

### <a name="managed-instance-h1-2019-updates"></a>Verwaltete Instanz – H1 2019-Updates

Die folgenden Funktionen sind im Bereitstellungsmodell für verwaltete Instanzen in H1 2019 aktiviert:
  - Unterstützung für Abonnements mit <a href="https://aka.ms/sql-mi-visual-studio-subscribers">monatlicher Azure-Gutschrift für Visual Studio-Abonnenten</a> und erhöhte [regionale Grenzwerte](sql-database-managed-instance-resource-limits.md#regional-resource-limitations).
  - Unterstützung für <a href="https://docs.microsoft.com/sharepoint/administration/deploy-azure-sql-managed-instance-with-sharepoint-servers-2016-2019"> SharePoint 2016 und SharePoint 2019 </a> und <a href="https://docs.microsoft.com/business-applications-release-notes/october18/dynamics365-business-central/support-for-azure-sql-database-managed-instance"> Dynamics 365 Business Central </a>
  - Erstellen Sie Instanzen mit <a href="https://aka.ms/managed-instance-collation">Sortierung auf Serverebene</a> und <a href="https://azure.microsoft.com/updates/managed-instance-time-zone-ga/">Zeitzone</a> für Ihre Auswahl.
  - Verwaltete Instanzen sind jetzt durch eine <a href="sql-database-managed-instance-management-endpoint-verify-built-in-firewall.md">integrierte Firewall</a> geschützt.
  - Konfigurieren Sie Instanzen für die Verwendung von [öffentlichen Endpunkten](sql-database-managed-instance-public-endpoint-configure.md), einer Verbindung zur [Proxy-Außerkraftsetzung](sql-database-connectivity-architecture.md#connection-policy), um eine bessere Netzwerkleistung zu erzielen, <a href="https://aka.ms/four-cores-sql-mi-update">4 virtuellen Kernen bei der Gen5-Hardwaregenerierung</a>, oder <a href="https://aka.ms/managed-instance-configurable-backup-retention">konfigurieren Sie die Beibehaltung der Sicherung bis zu 35 Tage</a> für eine Point-in-Time-Wiederherstellung. [Die langfristige Sicherungsaufbewahrung](sql-database-long-term-retention.md#managed-instance-support) (bis zu 10 Jahre) ist zurzeit in der eingeschränkten Public Preview verfügbar.  
  - Neue Funktionen ermöglichen Ihnen die <a href="https://medium.com/@jocapc/geo-restore-your-databases-on-azure-sql-instances-1451480e90fa">Geowiederherstellung Ihrer Datenbank in einem anderen Rechenzentrum mithilfe von PowerShell </a>, das [Umbenennen einer Datenbank](https://azure.microsoft.com/updates/azure-sql-database-managed-instance-database-rename-is-supported/) und das [Löschen eines virtuellen Clusters](sql-database-managed-instance-delete-virtual-cluster.md).
  - Die neue integrierte [Rolle „Mitwirkender für Instanzen“](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#sql-managed-instance-contributor) ermöglicht SoD-Einhaltung (Separation of Duty, Aufgabentrennung) von Sicherheitsprinzipien und Einhaltung von Unternehmensstandards.
  - Eine verwaltete Instanz ist in den folgenden Azure Government-Regionen für GA (US Gov Texas, US Gov Arizona) sowie in China, Norden 2, und China, Osten 2, verfügbar. Außerdem ist sie in den folgenden öffentlichen Regionen verfügbar: Australien, Mitte; Australien, Mitte 2; Brasilien, Süden; Frankreich, Süden; VAE, Mitte; VAE, Norden; Südafrika, Norden; Südafrika, Westen.

### <a name="known-issues"></a>Bekannte Probleme

|Problem  |Entdeckt am  |Status  |Gelöst am  |
|---------|---------|---------|---------|
|[Wiederherstellen der manuellen Sicherung ohne CHECKSUM schlägt möglicherweise fehl](#restoring-manual-backup-without-checksum-might-fail)|Mai 2020|Mit Problemumgehung| |
|[Der Agent reagiert beim Ändern, Deaktivieren oder Aktivieren vorhandener Aufträge nicht mehr](#agent-becomes-unresponsive-upon-modifying-disabling-or-enabling-existing-jobs)|Mai 2020|Automatisch behoben| |
|[Berechtigungen für Ressourcengruppe werden nicht auf verwaltete Instanz angewendet](#permissions-on-resource-group-not-applied-to-managed-instance)|Februar 2020|Mit Problemumgehung| |
|[Einschränkung beim manuellen Failover über das Portal für Failovergruppen](#limitation-of-manual-failover-via-portal-for-failover-groups)|Januar 2020|Mit Problemumgehung| |
|[SQL-Agent-Rollen benötigen explizite EXECUTE-Berechtigungen für Anmeldungen, die keine Systemadministratoranmeldungen sind](#in-memory-oltp-memory-limits-are-not-applied)|Dezember 2019|Mit Problemumgehung| |
|[SQL Agent-Aufträge können durch den Neustart des Agent-Prozesses unterbrochen werden](#sql-agent-jobs-can-be-interrupted-by-agent-process-restart)|Dezember 2019|Gelöst|März 2020|
|[AAD-Anmeldungen und -Benutzer werden in SSDT nicht unterstützt](#aad-logins-and-users-are-not-supported-in-ssdt)|November 2019|Keine Problemumgehung| |
|[In-Memory-OLTP-Arbeitsspeicherlimits werden nicht angewendet](#in-memory-oltp-memory-limits-are-not-applied)|Oktober 2019|Mit Problemumgehung| |
|[Zurückgegebener Fehler beim Versuch, eine nicht leere Datei zu entfernen](#wrong-error-returned-while-trying-to-remove-a-file-that-is-not-empty)|Oktober 2019|Mit Problemumgehung| |
|[Das Ändern der Dienstebene und Erstellen von Instanzvorgängen werden durch die laufende Datenbankwiederherstellung blockiert](#change-service-tier-and-create-instance-operations-are-blocked-by-ongoing-database-restore)|September 2019|Mit Problemumgehung| |
|[Resource Governor auf Dienstebene „Unternehmenskritisch“ muss nach einem Failover möglicherweise neu konfiguriert werden](#resource-governor-on-business-critical-service-tier-might-need-to-be-reconfigured-after-failover)|September 2019|Mit Problemumgehung| |
|[Datenbankübergreifende Service Broker-Dialoge müssen nach einem Upgrade der Dienstebene erneut initialisiert werden](#cross-database-service-broker-dialogs-must-be-re-initialized-after-service-tier-upgrade)|August 2019|Mit Problemumgehung| |
|[Identitätswechsel für Azure AD-Anmeldetypen wird nicht unterstützt](#impersonification-of-azure-ad-login-types-is-not-supported)|Juli 2019|Keine Problemumgehung| |
|[Parameter @query wird in „sp_send_db_mail“ nicht unterstützt](#-parameter-not-supported-in-sp_send_db_mail)|April 2019|Keine Problemumgehung| |
|[Transaktionsreplikation muss nach einem geografischen Failover neu konfiguriert werden](#transactional-replication-must-be-reconfigured-after-geo-failover)|März 2019|Keine Problemumgehung| |
|[Temporäre Datenbank wird während des RESTORE-Vorgangs verwendet](#temporary-database-is-used-during-restore-operation)||Mit Problemumgehung| |
|[Struktur und Inhalt von TEMPDB werden neu erstellt](#tempdb-structure-and-content-is-re-created)| |Keine Problemumgehung| |
|[Überschreiten des Speicherplatzes mit kleinen Datenbankdateien](#exceeding-storage-space-with-small-database-files)| |Mit Problemumgehung| |
|[Statt Datenbanknamen werden GUID-Werte angezeigt](#guid-values-shown-instead-of-database-names) ||Mit Problemumgehung| |
|[Fehlerprotokolle werden nicht persistent gespeichert](#error-logs-arent-persisted)||Keine Problemumgehung| |
|[CLR-Module und Verbindungsserver können manchmal nicht auf eine lokale IP-Adresse verweisen](#clr-modules-and-linked-servers-sometimes-cant-reference-a-local-ip-address)| |Mit Problemumgehung| |
|[Transaktionsbereich in zwei Datenbanken innerhalb derselben Instanz wird nicht unterstützt](#transaction-scope-on-two-databases-within-the-same-instance-isnt-supported)| |Gelöst|März 2020|
|Nach dem Wiederherstellen der Datenbank aus Azure Blob Storage wird die Datenbankkonsistenz nicht mithilfe von DBCC CHECKDB überprüft.| |Gelöst|November 2019|
|Die Point-in-Time-Wiederherstellung einer Datenbank von der Dienstebene „Unternehmenskritisch“ in die Dienstebene „Universell“ wird nicht gelingen, wenn die Quelldatenbank In-Memory-OLTP-Objekte enthält.| |Gelöst|Oktober 2019|
|Feature „Datenbank-E-Mail“ bei externen (Nicht-Azure-) E-Mail-Servern über sichere Verbindung| |Gelöst|Oktober 2019|
|Eigenständige Datenbanken werden in verwalteter Instanz nicht unterstützt| |Gelöst|August 2019|

### <a name="restoring-manual-backup-without-checksum-might-fail"></a>Wiederherstellen der manuellen Sicherung ohne CHECKSUM schlägt möglicherweise fehl

Unter bestimmten Umständen kann die manuelle Sicherung von Datenbanken, die für eine verwalteten Instanz ohne CHECKSUM erstellt wurde, nicht wiederhergestellt werden. Versuchen Sie in diesem Fall, die Sicherung wiederherzustellen, bis der Vorgang erfolgreich war.

**Problemumgehung**: Erstellen Sie manuelle Sicherungen von Datenbanken für eine verwaltete Instanz mit aktivierter CHECKSUM.

### <a name="agent-becomes-unresponsive-upon-modifying-disabling-or-enabling-existing-jobs"></a>Der Agent reagiert beim Ändern, Deaktivieren oder Aktivieren vorhandener Aufträge nicht mehr

Unter bestimmten Umständen kann es vorkommen, dass der Agent nicht mehr reagiert, wenn ein vorhandener Auftrag geändert, deaktiviert oder aktiviert wird. Das Problem wird automatisch behoben, sobald es erkannt wird, indem ein Neustart des Agent-Prozesses veranlasst wird.

### <a name="permissions-on-resource-group-not-applied-to-managed-instance"></a>Berechtigungen für Ressourcengruppe werden nicht auf verwaltete Instanz angewendet

Auf eine Ressourcengruppe (RG) angewendete RBAC-Rolle „Mitwirkender“ der verwalteten Instanz wird nicht auf verwaltete Instanz angewendet und besitzt keine Auswirkungen.

**Problemumgehung**: Richten Sie die Rolle „Mitwirkender“ der verwalteten Instanz für Benutzer auf Abonnementebene ein.

### <a name="limitation-of-manual-failover-via-portal-for-failover-groups"></a>Einschränkung beim manuellen Failover über das Portal für Failovergruppen

Wenn sich die Failovergruppe über mehrere Instanzen in verschiedenen Azure-Abonnements oder Ressourcengruppen erstreckt, kann von der primären Instanz in der Failovergruppe kein manuelles Failover initiiert werden.

**Problemumgehung**: Initiieren Sie das Failover über das Portal von der geosekundären Instanz.

### <a name="sql-agent-roles-need-explicit-execute-permissions-for-non-sysadmin-logins"></a>SQL-Agent-Rollen benötigen explizite EXECUTE-Berechtigungen für Anmeldungen, die keine Systemadministratoranmeldungen sind

Wenn Nicht-Systemadministratoranmeldungen einer der [festen SQL-Agent-Datenbankrollen](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent-fixed-database-roles) hinzugefügt werden, gibt es ein Problem, bei dem den gespeicherten Masterprozeduren explizite EXECUTE-Berechtigungen gewährt werden müssen, damit diese Anmeldungen funktionieren. Wenn dieses Problem auftritt, wird die Fehlermeldung „The EXECUTE permission was denied on the object <object_name> (Microsoft SQL Server, Error: 229)“ (Die Berechtigung EXECUTE wurde für das Objekt <objekt_name> verweigert (Microsoft SQL Server, Fehler: 229) angezeigt.

**Problemumgehung**: Nachdem Sie einer der festen SQL-Agent-Datenbankrollen (SQLAgentUserRole, SQLAgentReaderRole oder SQLAgentOperatorRole) Anmeldungen hinzugefügt haben, führen Sie für jede Anmeldung, die diesen Rollen hinzugefügt wurde, das folgende T-SQL-Skript aus, um den aufgelisteten gespeicherten Prozeduren explizit EXECUTE-Berechtigungen zu erteilen.

```tsql
USE [master]
GO
CREATE USER [login_name] FOR LOGIN [login_name]
GO
GRANT EXECUTE ON master.dbo.xp_sqlagent_enum_jobs TO [login_name]
GRANT EXECUTE ON master.dbo.xp_sqlagent_is_starting TO [login_name]
GRANT EXECUTE ON master.dbo.xp_sqlagent_notify TO [login_name]
```

### <a name="sql-agent-jobs-can-be-interrupted-by-agent-process-restart"></a>SQL Agent-Aufträge können durch den Neustart des Agent-Prozesses unterbrochen werden

**(Gelöst im März 2020)** Der SQL-Agent erstellt jedes Mal, wenn der Auftrag gestartet wird, eine neue Sitzung und erhöht den Speicherverbrauch allmählich. Um zu vermeiden, dass die interne Arbeitsspeichergrenze erreicht wird, wodurch die Ausführung geplanter Aufträge blockiert würde, wird der Agent-Prozess neu gestartet, sobald der Arbeitsspeicherverbrauch den Schwellenwert erreicht. Dies kann dazu führen, dass die Ausführung von Aufträgen unterbrochen wird, die zum Zeitpunkt des Neustarts ausgeführt werden.

### <a name="in-memory-oltp-memory-limits-are-not-applied"></a>In-Memory-OLTP-Arbeitsspeicherlimits werden nicht angewendet.

In einigen Fällen wird die Dienstebene „Unternehmenskritisch“ [maximale Arbeitsspeicherlimits für speicheroptimierte Objekte](sql-database-managed-instance-resource-limits.md#in-memory-oltp-available-space) nicht ordnungsgemäß anwenden. Die verwaltete Instanz ermöglicht es vielleicht, dass die Workload mehr Arbeitsspeicher für In-Memory-OLTP-Vorgänge belegt. Dies kann sich auf die Verfügbarkeit und Stabilität der Instanz auswirken. In-Memory-OLTP-Abfragen, die die Grenzwerte erreichen, führen möglicherweise nicht sofort zu Fehlern. Dieses Problem wird bald behoben. Die Abfragen, die mehr In-Memory-OLTP-Arbeitsspeicher belegen, werden früher fehlschlagen, wenn sie die [Limits](sql-database-managed-instance-resource-limits.md#in-memory-oltp-available-space) erreichen.

**Problemumgehung:** [Überwachen der In-Memory-OLTP-Arbeitsspeichernutzung](https://docs.microsoft.com/azure/sql-database/sql-database-in-memory-oltp-monitoring) mithilfe von [SQL Server Management Studio ](/sql/relational-databases/in-memory-oltp/monitor-and-troubleshoot-memory-usage#bkmk_Monitoring), um sicherzustellen, dass die Workload nicht mehr als den verfügbaren Arbeitsspeicher belegt. Erhöhen Sie die Arbeitsspeicherlimits, die von der Anzahl von virtuellen Kernen abhängen, oder optimieren Sie Ihre Workload, damit sie weniger Arbeitsspeicher belegt.

### <a name="wrong-error-returned-while-trying-to-remove-a-file-that-is-not-empty"></a>Zurückgegebener Fehler bei dem Versuch, eine nicht leere Datei zu entfernen

SQL Server/Verwaltete Instanz [erlaubt Benutzern nicht das Löschen von Dateien, die nicht leer sind](/sql/relational-databases/databases/delete-data-or-log-files-from-a-database#Prerequisites). Wenn Sie versuchen, eine nicht leere Datendatei mithilfe der `ALTER DATABASE REMOVE FILE`-Anweisung zu entfernen, wird der Fehler `Msg 5042 – The file '<file_name>' cannot be removed because it is not empty` nicht sofort zurückgegeben. Die verwaltete Instanz versucht fortgesetzt, die Datei zu löschen, und der Vorgang schlägt nach 30 Minuten mit `Internal server error` fehl.

**Problemumgehung**: Entfernen Sie den Inhalt der Datei mit dem `DBCC SHRINKFILE (N'<file_name>', EMPTYFILE)`-Befehl. Wenn es sich um die einzige Datei in der Dateigruppe handelt, müssten Sie Daten aus der Tabelle oder Partition löschen, die dieser Dateigruppe zugeordnet ist, bevor Sie die Datei verkleinern, und diese Daten optional in eine andere Tabelle/Partition laden.

### <a name="change-service-tier-and-create-instance-operations-are-blocked-by-ongoing-database-restore"></a>Das Ändern der Dienstebene und Erstellen von Instanzvorgängen wird durch die laufende Datenbankwiederherstellung blockiert

Durch die laufende `RESTORE`-Anweisung blockieren der Migrationsprozess des Datenmigrationsdiensts und die integrierte Point-in-Time-Wiederherstellung das Aktualisieren der Dienstebene oder eine Größenänderung bei der vorhandenen Instanz sowie das Erstellen neuer Instanzen so lange, bis der Wiederherstellungsvorgang abgeschlossen ist. Der Wiederherstellungsvorgang blockiert diese Vorgänge für die verwalteten Instanzen und Instanzenpools in demselben Subnetz, in dem der Wiederherstellungsvorgang ausgeführt wird. Die Instanzen in Instanzenpools sind davon nicht betroffen. Die Vorgänge des Erstellens oder Änderns von Dienstebenen führen nicht zu einem Fehler oder Timeout. Sie werden nach dem Abschluss oder einem Abbrechen des Wiederherstellungsvorgangs fortgesetzt.

**Problemumgehung**: Warten Sie, bis der Wiederherstellungsvorgang abgeschlossen ist, oder brechen Sie ihn ab, wenn der Vorgang des Erstellens oder Aktualisierens der Dienstebene höhere Priorität hat.

### <a name="resource-governor-on-business-critical-service-tier-might-need-to-be-reconfigured-after-failover"></a>Resource Governor auf Dienstebene „Unternehmenskritisch“ muss möglicherweise nach einem Failover neu konfiguriert werden

Die Funktion [Resource Governor](/sql/relational-databases/resource-governor/resource-governor), die Ihnen ermöglicht, die der Benutzerworkload zugewiesenen Ressourcen einzuschränken, könnte eine andere Benutzerworkload nach einem Failover oder einer vom Benutzer initiierten Änderung der Dienstebene (z. B. Änderung der maximalen virtuellen Kerne oder maximalen Instanzspeichergröße) falsch klassifizieren.

**Problemumgehung**: Führen Sie `ALTER RESOURCE GOVERNOR RECONFIGURE` regelmäßig oder als Teil des SQL Agent-Auftrags aus, der den SQL-Task ausführt, wenn die Instanz gestartet wird, wenn Sie [Resource Governor](/sql/relational-databases/resource-governor/resource-governor) verwenden.

### <a name="cross-database-service-broker-dialogs-must-be-re-initialized-after-service-tier-upgrade"></a>Datenbankübergreifende Service Broker-Dialoge müssen nach dem Upgrade der Dienstebene erneut initialisiert werden.

Datenbankübergreifenden Service Broker-Dialoge stellen die Zustellung der Nachrichten an die Dienste in anderen Datenbanken nach Änderung der Dienstebene ein. Die Nachrichten sind **nicht verloren**, sondern befinden sich in der Absenderwarteschlange. Jegliche Änderung virtueller Kerne oder der Instanzspeichergröße in der verwalteten Instanz führt dazu, dass der Wert `service_broke_guid` in der Sicht [sys.databases](/sql/relational-databases/system-catalog-views/sys-databases-transact-sql) für alle Datenbanken geändert wird. Ein `DIALOG`, der mit der Anweisung [BEGIN DIALOG](/sql/t-sql/statements/begin-dialog-conversation-transact-sql) erstellt wurde und auf Service Broker in einer anderen Datenbank verweist, stellt die Zustellung von Nachrichten an den Zieldienst ein.

**Problemumgehung:** Beenden Sie alle Aktivitäten, die datenbankübergreifende Dialogkonversationen des Service Brokers verwenden, bevor Sie die Dienstebene aktualisieren, und initialisieren Sie sie danach neu. Wenn es noch Nachrichten gibt, die nach dem Ändern der Dienstebene nicht zugestellt werden, lesen Sie die Nachrichten aus der Quellwarteschlange, und senden Sie sie erneut an die Zielwarteschlange.

### <a name="impersonification-of-azure-ad-login-types-is-not-supported"></a>Identitätswechsel für Azure AD-Anmeldetypen werden nicht unterstützt.

Der Identitätswechsel mithilfe von `EXECUTE AS USER` oder `EXECUTE AS LOGIN` der folgenden AAD-Prinzipale wird nicht unterstützt:
-    AAD-Benutzer mit Alias. In diesem Fall wird der Fehler `15517` zurückgegeben.
- AAD-Anmeldungen und-Benutzer, die auf AAD-Anwendungen oder -Dienstprinzipalen basieren. In diesem Fall werden die Fehler `15517` und `15406` zurückgegeben.

### <a name="query-parameter-not-supported-in-sp_send_db_mail"></a>Der Parameter @query wird in sp_send_db_mail nicht unterstützt.

Der `@query`-Parameter in der Prozedur [sp_send_db_mail](/sql/relational-databases/system-stored-procedures/sp-send-dbmail-transact-sql) funktioniert nicht.

### <a name="transactional-replication-must-be-reconfigured-after-geo-failover"></a>Die Transaktionsreplikation muss nach einem geografischen Failover neu konfiguriert werden.

Wenn die Transaktionsreplikation für eine Datenbank in einer Autofailover-Gruppe aktiviert ist, muss der Administrator der verwalteten Instanz alle Veröffentlichungen für die alte primäre Instanz bereinigen und nach einem Failover in eine andere Region für die neue primäre Instanz erneut konfigurieren. Weitere Informationen finden Sie unter [Replikation](sql-database-managed-instance-transact-sql-information.md#replication).

### <a name="aad-logins-and-users-are-not-supported-in-ssdt"></a>AAD-Anmeldungen und -Benutzer werden in SSDT nicht unterstützt.

SQL Server Data Tools unterstützen Azure Active Directory-Anmeldungen und -Benutzer nicht vollständig.

### <a name="temporary-database-is-used-during-restore-operation"></a>Temporäre Datenbank wird während des RESTORE-Vorgangs verwendet

Wenn eine Datenbank auf einer verwalteten Instanz wiederhergestellt wird, erstellt der Wiederherstellungsdienst zunächst eine leere Datenbank mit dem gewünschten Namen, um den Namen auf der Instanz zuzuweisen. Nach einiger Zeit wird diese Datenbank gelöscht, und die Wiederherstellung der eigentlichen Datenbank wird gestartet. Die Datenbank, die sich im Status *Wird wiederhergestellt* befindet, weist temporär einen zufälligen GUID-Wert anstelle des Namens auf. Nachdem der Wiederherstellungsvorgang abgeschlossen ist, wird der temporäre Name in den in der `RESTORE`-Anweisung angegebenen gewünschten Namen geändert. In der Anfangsphase kann der Benutzer auf die leere Datenbank zugreifen und sogar Tabellen erstellen oder Daten in diese Datenbank laden. Diese temporäre Datenbank wird gelöscht, wenn der Wiederherstellungsdienst die zweite Phase startet.

**Problemumgehung**: Greifen Sie erst dann auf die Datenbank zu, die Sie wiederherstellen, wenn Sie feststellen, dass die Wiederherstellung abgeschlossen ist.

### <a name="tempdb-structure-and-content-is-re-created"></a>Struktur und Inhalt von TEMPDB werden neu erstellt.

Die Datenbank `tempdb` ist immer in 12 Datendateien aufgeteilt, und die Dateistruktur kann nicht geändert werden. Die maximale Größe pro Datei kann nicht geändert werden. Zudem können `tempdb` keine neuen Dateien hinzugefügt werden. `Tempdb` wird beim Start oder Failover einer Instanz immer als leere Datenbank neu erstellt. Änderungen an `tempdb` werden nicht beibehalten.

### <a name="exceeding-storage-space-with-small-database-files"></a>Überschreiten des Speicherplatzes mit kleinen Datenbankdateien

`CREATE DATABASE`-, `ALTER DATABASE ADD FILE`- und `RESTORE DATABASE`-Anweisungen schlagen möglicherweise fehl, weil die Instanz das Azure-Speicherlimit erreichen kann.

Jede verwaltete Instanz in der Dienstebene „Universell“ reserviert bis zu 35 TB Speicherplatz für Azure Premium-Datenträger. Jede Datenbankdatei wird auf einem separaten physischen Datenträger platziert. Mögliche Datenträgergrößen sind 128 GB, 256 GB, 512 GB, 1 TB oder 4 TB. Nicht verwendeter Speicherplatz auf dem Datenträger wird nicht berechnet, aber die Gesamtgröße der Azure Premium-Datenträger darf 35 TB nicht überschreiten. In einigen Fällen kann eine verwaltete Instanz, die nicht insgesamt 8 TB benötigt, aufgrund interner Fragmentierung das Azure-Limit von 35 TB überschreiten.

Ein Beispiel: In einer verwalteten Instanz der Dienstebene „Universell“ gibt es eine große Datei (1,2 TB), die auf einem 4-TB-Datenträger gespeichert ist. Es kann auch 248 Dateien mit einer Größe von jeweils 1 GB geben, die auf separaten 128-GB-Datenträgern gespeichert sind. In diesem Beispiel:

- Die Gesamtgröße des zugewiesenen Datenträgerspeichers beträgt 1 x 4 TB + 248 x 128 GB = 35 TB.
- Der reservierte Gesamtspeicherplatz für Datenbanken in der Instanz beträgt 1 x 1,2 TB + 248 x 1 GB = 1,4 TB.

Dieses Beispiel verdeutlicht, dass eine verwaltete Instanz unter bestimmten Umständen aufgrund einer spezifischen Verteilung von Dateien das für einen angefügten Azure Premium-Datenträger reservierte Limit von 35 TB erreichen kann, auch wenn Sie dies möglicherweise nicht erwarten.

In diesem Beispiel funktionieren vorhandene Datenbanken weiterhin und können ohne Probleme weiter wachsen, solange keine neuen Dateien hinzugefügt werden. Es könnten jedoch keine neuen Datenbanken erstellt oder wiederhergestellt werden, da für neue Datenträgerlaufwerke nicht genügend Speicherplatz vorhanden ist, selbst nicht dann, wenn die Gesamtgröße aller Datenbanken das Größenlimit der Instanz nicht überschreitet. Der Fehler, der in diesem Fall zurückgegeben wird, ist nicht klar.

Sie können mithilfe von Systemansichten [die Anzahl von verbleibenden Dateien identifizieren](https://medium.com/azure-sqldb-managed-instance/how-many-files-you-can-create-in-general-purpose-azure-sql-managed-instance-e1c7c32886c1). Wenn Sie dieses Limit erreichen, versuchen Sie, [einige der kleineren Dateien mithilfe der DBCC SHRINKFILE-Anweisung zu leeren und zu löschen](/sql/t-sql/database-console-commands/dbcc-shrinkfile-transact-sql#d-emptying-a-file), oder wechseln Sie zur [Dienstebene „Unternehmenskritisch“, für die dieses Limit nicht gilt](/azure/sql-database/sql-database-managed-instance-resource-limits#service-tier-characteristics).

### <a name="guid-values-shown-instead-of-database-names"></a>Anstelle von Datenbanknamen werden GUID-Werte gezeigt.

Mehrere Systemansichten, Leistungsindikatoren, Fehlermeldungen, XEvents und Fehlerprotokolleinträge zeigen GUID-Datenbankbezeichner anstelle der eigentlichen Datenbanknamen an. Verlassen Sie sich nicht auf diese GUIDs, da sie in Zukunft durch tatsächliche Datenbanknamen ersetzt werden.

**Problemumgehung**: Verwenden Sie die Ansicht „sys.databases“, um den tatsächlichen Datenbanknamen aus dem physischen Datenbanknamen aufzulösen, der in Form von GUID-Datenbankbezeichnern angegeben wurde.

```tsql
SELECT name as ActualDatabaseName, physical_database_name as GUIDDatabaseIdentifier 
FROM sys.databases
WHERE database_id > 4
```

### <a name="error-logs-arent-persisted"></a>Fehlerprotokolle werden nicht persistent gespeichert

Die Fehlerprotokolle in einer verwalteten Instanz werden nicht persistent gespeichert, und ihre Größe wird im Speicherlimit nicht berücksichtigt. Fehlerprotokolle werden im Falle eines Failovers möglicherweise automatisch gelöscht. Möglicherweise gibt es Lücken im Fehlerprotokollverlauf, weil die verwaltete Instanz auf mehreren virtuellen Computern mehrmals verschoben wurde.

### <a name="transaction-scope-on-two-databases-within-the-same-instance-isnt-supported"></a>Ein Transaktionsbereich in zwei Datenbanken in derselben Instanz wird nicht unterstützt

**(Gelöst im März 2020)** Die `TransactionScope`-Klasse in .NET funktioniert nicht, wenn zwei Abfragen an zwei Datenbanken in derselben Instanz im gleichen Transaktionsbereich gesendet werden:

```csharp
using (var scope = new TransactionScope())
{
    using (var conn1 = new SqlConnection("Server=quickstartbmi.neu15011648751ff.database.windows.net;Database=b;User ID=myuser;Password=mypassword;Encrypt=true"))
    {
        conn1.Open();
        SqlCommand cmd1 = conn1.CreateCommand();
        cmd1.CommandText = string.Format("insert into T1 values(1)");
        cmd1.ExecuteNonQuery();
    }

    using (var conn2 = new SqlConnection("Server=quickstartbmi.neu15011648751ff.database.windows.net;Database=b;User ID=myuser;Password=mypassword;Encrypt=true"))
    {
        conn2.Open();
        var cmd2 = conn2.CreateCommand();
        cmd2.CommandText = string.Format("insert into b.dbo.T2 values(2)");        cmd2.ExecuteNonQuery();
    }

    scope.Complete();
}

```

**Problemumgehung (seit März 2020 nicht mehr erforderlich):** Verwenden Sie [SqlConnection.ChangeDatabase(String)](/dotnet/api/system.data.sqlclient.sqlconnection.changedatabase), um anstelle von zwei Verbindungen eine andere Datenbank im Verbindungskontext zu verwenden.

### <a name="clr-modules-and-linked-servers-sometimes-cant-reference-a-local-ip-address"></a>CLR-Module und Verbindungsserver können manchmal nicht auf eine lokale IP-Adresse verweisen

CLR-Module, die in einer verwalteten Instanz bereitgestellt werden, und Verbindungsserver oder verteilte Abfragen, die auf eine aktuelle Instanz verweisen, können die IP-Adresse der lokalen Instanz manchmal nicht auflösen. Dieser Fehler ist ein vorübergehendes Problem.

**Problemumgehung:** Verwenden Sie nach Möglichkeit Kontextverbindungen im CLR-Modul.

## <a name="updates"></a>Aktualisierungen

Eine Liste mit Updates und Verbesserungen für SQL-Datenbank finden Sie unter [Azure-Updates](https://azure.microsoft.com/updates/?product=sql-database).

Informationen zu Updates und Verbesserungen für alle Azure-Dienste finden Sie unter [Dienstupdates](https://azure.microsoft.com/updates).

## <a name="contribute-to-content"></a>Mitwirkung am Inhalt

Informationen zur Mitwirkung an der Dokumentation für Azure SQL-Datenbank finden Sie unter [Leitfaden für Mitwirkende an der Microsoft-Dokumentation: Übersicht](https://docs.microsoft.com/contribute/).
