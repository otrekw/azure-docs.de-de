---
title: 'SQL Server zu SQL Server auf Azure Virtual Machines: Migrationsleitfaden'
description: Befolgen Sie diese Anleitung, um einzelne SQL Server-Datenbank-Instanzen zu SQL Server auf Azure-VMs zu migrieren.
ms.custom: ''
ms.service: virtual-machines-sql
ms.subservice: migration-guide
ms.devlang: ''
ms.topic: how-to
author: markjones-msft
ms.author: markjon
ms.reviewer: mathoma
ms.date: 03/19/2021
ms.openlocfilehash: e7fc4bacd73cec0fdab3117ada190fb7964b4282
ms.sourcegitcommit: b0557848d0ad9b74bf293217862525d08fe0fc1d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/07/2021
ms.locfileid: "106550896"
---
# <a name="migration-guide-sql-server-to-sql-server-on-azure-virtual-machines"></a>Migrationsleitfaden: SQL Server zu SQL Server auf Azure Virtual Machines

[!INCLUDE[appliesto--sqlmi](../../includes/appliesto-sqlvm.md)]

In diesem Leitfaden wird behandelt, wie Sie Ihre Benutzerdatenbanken *ermitteln*, *bewerten* und von SQL Server zu einer SQL Server-Instanz auf einer Azure-VM *migrieren*, indem Sie die Sicherungs-, Wiederherstellungs- und Protokollversandoptionen nutzen. Die Bewertung erfolgt mit dem [Datenmigrations-Assistenten (DMA)](/sql/dma/dma-overview).

Sie können SQL Server-Instanzen migrieren, die lokal oder auf folgenden Plattformen ausgeführt werden:

- SQL Server auf virtuellen Computern (VMs)
- Amazon Web Services (AWS) EC2
- Amazon Relational Database Service (AWS RDS)
- Compute Engine (Google Cloud Platform – GCP)

Informationen zu anderen Migrationsstrategien finden Sie in der [Übersicht zur Migration von SQL Server-VMs](sql-server-to-sql-on-azure-vm-migration-overview.md). Weitere Migrationsleitfäden finden Sie in den [Leitfäden zur Azure-Datenbankmigration](https://docs.microsoft.com/data-migration).

:::image type="content" source="media/sql-server-to-sql-on-azure-vm-migration-overview/migration-process-flow-small.png" alt-text="Diagramm, das einen Migrationsprozessfluss anzeigt.":::

## <a name="prerequisites"></a>Voraussetzungen

Für die Migration zu SQL Server auf Azure-VMs benötigen Sie die folgenden Ressourcen:

- [Datenmigrations-Assistent](https://www.microsoft.com/download/details.aspx?id=53595).
- ein [Azure Migrate-Projekt](../../../migrate/create-manage-projects.md)
- eine vorbereitete Zielinstanz für [SQL Server auf Azure-VMs](../../virtual-machines/windows/create-sql-vm-portal.md), deren Version identisch mit oder höher als die der SQL Server-Quellinstanz ist
- [Konnektivität zwischen Azure und einer lokalen Umgebung](/azure/architecture/reference-architectures/hybrid-networking)
- eine [geeignete Migrationsstrategie](sql-server-to-sql-on-azure-vm-migration-overview.md#migrate)

## <a name="pre-migration"></a>Vor der Migration

Bevor Sie mit der Migration beginnen, müssen Sie die Topologie Ihrer SQL-Umgebung ermitteln und die Umsetzbarkeit der gewünschten Migrationsstrategie bewerten.

### <a name="discover"></a>Entdecken

Azure Migrate bewertet die Umsetzbarkeit der Migration auf lokalen Computern, führt eine leistungsbasierte Größenanpassung durch und erstellt eine Kostenschätzungen für die Ausführung in lokalen Umgebungen. Zur Planung der Migration können Sie Azure Migrate einsetzen, um [vorhandene Datenquellen und Details zu den Features](../../../migrate/concepts-assessment-calculation.md) zu ermitteln, die Ihre SQL Server-Instanzen verwenden. Bei diesem Prozess wird das Netzwerk gescannt, um alle SQL Server-Instanzen Ihrer Organisation sowie die verwendeten Versionen und Features zu ermitteln.

> [!IMPORTANT]
> Berücksichtigen Sie die [Leistungsrichtlinien für SQL Server auf Azure-VMs](../../virtual-machines/windows/performance-guidelines-best-practices.md), wenn Sie eine Azure-Ziel-VM für Ihre SQL Server-Instanz auswählen.

Weitere Informationen zu Ermittlungstools finden Sie unter [Dienste und Tools](../../../dms/dms-tools-matrix.md#business-justification-phase) für Datenmigrationsszenarien.

### <a name="assess"></a>Bewerten

[!INCLUDE [assess-estate-with-azure-migrate](../../../../includes/azure-migrate-to-assess-sql-data-estate.md)]

Nachdem Sie alle Datenquellen ermittelt haben, können Sie den [Datenmigrations-Assistenten (DMA)](/sql/dma/dma-overview) verwenden, um lokale SQL Server-Instanzen zu bewerten, die zu einer SQL Server-Instanz auf einer Azure-VM migriert werden, und so Diskrepanzen zwischen Quell- und Zielinstanzen festzustellen.

> [!NOTE]
> Wenn Sie Ihre SQL Server-Version _nicht_ upgraden, können Sie diesen Schritt überspringen und mit dem Abschnitt [Migrieren](#migrate) fortfahren.

#### <a name="assess-user-databases"></a>Bewerten von Benutzerdatenbanken

Der Datenmigrations-Assistent (DMA) unterstützt Sie bei der Migration zu einer modernen Datenplattform, indem er Kompatibilitätsprobleme erkennt, die die Datenbankfunktionalität auf der neuen Version von SQL Server beeinträchtigen können. DMA empfiehlt Leistungs- und Zuverlässigkeitsverbesserungen für Ihre Zielumgebung und ermöglicht es Ihnen zudem, Ihr Schema, Ihre Daten und Anmeldeobjekte vom Quellserver auf den Zielserver zu verschieben.

Weitere Informationen finden Sie unter [Bewertung](/sql/dma/dma-migrateonpremsql).

> [!IMPORTANT]
>Die Berechtigungen für den SQL Server-Quellserver unterscheiden sich je nach Art der Bewertung möglicherweise.
   > - Für den Advisor zur *Featureparität* müssen die Anmeldeinformationen für die Verbindungsherstellung mit der SQL Server-Quelldatenbank ein Mitglied der Serverrolle *sysadmin* sein.
   > - Für den Advisor zu *Kompatibilitätsproblemen* müssen die Anmeldeinformationen mindestens die Berechtigungen `CONNECT SQL`, `VIEW SERVER STATE`, und `VIEW ANY DEFINITION` besitzen.
   > - Datenmigrations-Assistent zeigt vor der Bewertung an, welche Berechtigungen für den ausgewählten Advisor benötigt werden.

#### <a name="assess-the-applications"></a>Bewerten der Anwendungen

In der Regel greift eine Anwendungsschicht auf Benutzerdatenbanken zu, um Daten zu speichern und zu ändern. Datenmigrations-Assistent kann die Datenzugriffsschicht einer Anwendung auf zwei Arten bewerten:

- Mithilfe von erfassten [erweiterten Ereignissen](/sql/relational-databases/extended-events/extended-events) oder [SQL Server Profiler-Ablaufverfolgungen](/sql/tools/sql-server-profiler/create-a-trace-sql-server-profiler) Ihrer Benutzerdatenbanken. Sie können auch mithilfe des [Assistenten für Datenbankexperimente](/sql/dea/database-experimentation-assistant-capture-trace) ein Ablaufverfolgungsprotokoll erstellen, das auch für A/B-Tests verwendet werden kann.
- Mithilfe des [Data Access Migration Toolkit (DAMT, Vorschauversion)](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit), das SQL-Abfragen im Code ermittelt und bewertet und zudem für die Migration von Anwendungsquellcode von einer Datenbankplattform zu einer anderen verwendet wird. Dieses Tool unterstützt beliebte Dateitypen wie C#, Java, XML und nur-Text. Eine Anleitung zum Ausführen einer Bewertung des Data Access Migration Toolkit finden Sie im Blogbeitrag [Datenmigrations-Assistent](https://techcommunity.microsoft.com/t5/microsoft-data-migration/using-data-migration-assistant-to-assess-an-application-s-data/ba-p/990430) .

Verwenden Sie während der Bewertung von Benutzerdatenbanken Datenmigrations-Assistent, um erfasste Ablaufverfolgungsdateien oder die Toolkit-Dateien für die Datenzugriffsmigration zu [importieren](/sql/dma/dma-assesssqlonprem#add-databases-and-extended-events-trace-to-assess).

#### <a name="assessments-at-scale"></a>Skalierbare Bewertungen

Wenn für mehrere Server eine DMA-Bewertung durchgeführt werden muss, können Sie den Prozess über die [Befehlszeilenschnittstelle](/sql/dma/dma-commandline) automatisieren. Mithilfe der Schnittstelle können Sie Bewertungsbefehle für alle SQL Server-Instanzen vorbereiten, die migriert werden sollen.

Wenn Sie zusammenfassende Berichte für große Bestände benötigen, können DMA-Bewertungen nun auch [in Azure Migrate konsolidiert](/sql/dma/dma-assess-sql-data-estate-to-sqldb) werden.

#### <a name="refactor-databases-with-data-migration-assistant"></a>Umgestalten von Datenbanken mit Datenmigrations-Assistent

Auf Grundlage der DMA-Bewertungsergebnisse werden einige Empfehlungen angezeigt, um sicherzustellen, dass Ihre Benutzerdatenbanken nach der Migration korrekt funktionieren. Datenmigrations-Assistent liefert Details zu den betroffenen Objekten und Ressourcen zur Behebung der einzelnen Probleme. Stellen Sie sicher, dass Sie alle wichtigen Änderungen und Verhaltensänderungen beheben, bevor Sie die Produktionsmigration starten.

Hinsichtlich veralteter Features können Sie sich dafür entscheiden, Ihre Benutzerdatenbanken im ursprünglichen [Kompatibilitätsmodus](/sql/t-sql/statements/alter-database-transact-sql-compatibility-level) auszuführen, um diese Änderungen zu vermeiden und die Migration zu beschleunigen. Sie können die [Datenbankkompatibilität jedoch nicht upgraden](/sql/database-engine/install-windows/compatibility-certification#compatibility-levels-and-database-engine-upgrades), bis die Probleme mit veralteten Elementen behoben wurden.

Sie müssen für alle Fixes des Datenmigrations-Assistenten Skripts erstellen und diese während der Phase [nach der Migration](#post-migration) auf die SQL Server-Zieldatenbank anwenden.

> [!CAUTION]
> Nicht alle SQL Server-Versionen unterstützen sämtliche Kompatibilitätsmodi. Vergewissern Sie sich, dass die [SQL Server-Zielversion](/sql/t-sql/statements/alter-database-transact-sql-compatibility-level) den ausgewählten Datenbank-Kompatibilitätsgrad unterstützt. SQL Server 2019 unterstützt beispielsweise keine Datenbanken mit dem Kompatibilitätsgrad 90 (entspricht SQL Server 2005). Für diese Datenbanken müsste mindestens ein Upgrade auf Kompatibilitätsgrad 100 durchgeführt werden.
>

## <a name="migrate"></a>Migrieren

Nachdem Sie die Schritte der Prämigrationsphase abgeschlossen haben, sind Sie bereit für die Migration der Benutzerdatenbanken und Komponenten. Migrieren Sie Ihre Datenbanken mithilfe Ihrer bevorzugten [Migrationsmethode](sql-server-to-sql-on-azure-vm-migration-overview.md#migrate).

Im Folgenden werden die Schritte für eine Migration mit Sicherung und Wiederherstellung oder eine Migration mit minimaler Downtime erläutert, bei der die Sicherung und Wiederherstellung in Kombination mit dem Protokollversand verwendet werden.

### <a name="backup-and-restore"></a>Sichern und Wiederherstellen

Befolgen Sie diese Schritte, wen Sie eine gewöhnliche Migration mit Sicherung und Wiederherstellung durchführen möchten:

1. Richten Sie die Konnektivität von SQL Server auf Azure-VMs Ihren Anforderungen entsprechend ein. Weitere Informationen finden Sie unter [Verbinden mit SQL Server-Instanzen auf virtuellen Azure-Maschinen (Ressourcen-Manager)](../../virtual-machines/windows/ways-to-connect-to-sql.md).
1. Halten Sie alle Anwendungen an, die für die Migration vorgesehene Datenbanken verwenden.
1. Sorgen Sie mit dem [Einzelbenutzermodus](/sql/relational-databases/databases/set-a-database-to-single-user-mode) dafür, dass Benutzerdatenbanken inaktiv sind.
1. Führen Sie eine vollständige Datenbanksicherung an einem lokalen Speicherort durch.
1. Kopieren Sie Ihre lokalen Sicherungsdateien auf Ihre VM, indem Sie einen Remotedesktop, [Azure Data Explorer](/azure/data-explorer/data-explorer-overview) oder das [Befehlszeilentool AzCopy](../../../storage/common/storage-use-azcopy-v10.md) verwenden. (Es werden mehr als 2 TB Sicherungen empfohlen.)
1. Stellen Sie die vollständigen Datenbanksicherungen in SQL Server auf der Azure-VM wieder her.

### <a name="log-shipping-minimize-downtime"></a>Protokollversand (Minimieren der Downtime)

Befolgen Sie diese Schritte, um eine Migration mit minimaler Downtime mithilfe von Sicherung, Wiederherstellung und Protokollversand durchzuführen:

1. Richten Sie die Konnektivität von SQL Server auf Azure-VMs Ihren Anforderungen entsprechend ein. Weitere Informationen finden Sie unter [Verbinden mit SQL Server-Instanzen auf virtuellen Azure-Maschinen (Ressourcen-Manager)](../../virtual-machines/windows/ways-to-connect-to-sql.md).
1. Stellen Sie sicher, dass die zu migrierenden Benutzerdatenbanken das vollständige oder massenprotokollierte Wiederherstellungsmodell verwenden.
1. Führen Sie eine vollständige Datenbanksicherung auf einen lokalen Speicherort durch und ändern Sie vorhandene Datenbanksicherungsaufträge so, dass das Schlüsselwort [COPY_ONLY](/sql/relational-databases/backup-restore/copy-only-backups-sql-server) verwendet wird, um die Protokollkette aufrechtzuerhalten.
1. Kopieren Sie Ihre lokalen Sicherungsdateien auf Ihre VM, indem Sie einen Remotedesktop, [Azure Data Explorer](/azure/data-explorer/data-explorer-overview) oder das [Befehlszeilentool AzCopy](../../../storage/common/storage-use-azcopy-v10.md) verwenden. (Es werden mehr als 1 TB Sicherungen empfohlen.)
1. Stellen Sie die vollständigen Datenbanksicherungen in SQL Server auf der Azure-VM wieder her.
1. Richten Sie den [Protokollversand](/sql/database-engine/log-shipping/configure-log-shipping-sql-server) zwischen der lokalen Datenbank und SQL Server auf Azure-VMs ein. Initialisieren Sie Ihre Datenbanken nicht erneut, da diese Aufgabe bereits in einem vorherigen Schritt durchgeführt wurde.
1. Führen Sie eine Übernahme auf den Zielserver durch.
   1. Halten Sie Anwendungen an, die die zu migrierenden Datenbanken verwenden.
   1. Sorgen Sie mit dem [Einzelbenutzermodus](/sql/relational-databases/databases/set-a-database-to-single-user-mode) dafür, dass Benutzerdatenbanken inaktiv sind.
   1. Wenn Sie bereit sind, führen Sie ein [gesteuertes Failover](/sql/database-engine/log-shipping/fail-over-to-a-log-shipping-secondary-sql-server) des Protokollversands auf lokalen Datenbanken für SQL Server auf Azure Virtual Machines aus.

### <a name="migrate-objects-outside-user-databases"></a>Migrieren von Objekten außerhalb von Benutzerdatenbanken

Möglicherweise werden zusätzliche SQL Server-Objekte für den reibungslosen Betrieb Ihrer Benutzerdatenbanken nach der Migration benötigt.

Die folgende Tabelle enthält Komponenten und empfohlene Migrationsmethoden, die vor oder nach der Migration Ihrer Benutzerdatenbanken anwendbar sind:

| **Feature** | **Komponente** | **Migrationsmethoden** |
| --- | --- | --- |
| **Datenbanken** | Modell | Skript mit SQL Server Management Studio |
|| TempDB | Verschieben Sie tempdb auf einen [temporären SSD-Datenträger einer Azure-VM](../../virtual-machines/windows/performance-guidelines-best-practices.md#temporary-disk), um die bestmögliche Leistung zu erzielen. Wählen Sie eine VM-Größe mit ausreichend lokalem SSD-Speicher für tempdb. |
|| Benutzerdatenbanken mit FileStream | Wenden Sie die [Sicherungs- und Wiederherstellungsmethode](../../virtual-machines/windows/migrate-to-vm-from-sql-server.md#back-up-and-restore) für die Migration an. Datenmigrations-Assistent unterstützt keine Datenbanken mit FileStream. |
| **Security** | SQL Server- und Windows-Anmeldungen | Verwenden Sie Datenmigrations-Assistent, um [Benutzeranmeldungen zu migrieren](/sql/dma/dma-migrateserverlogins). |
|| SQL Server-Rollen | Skript mit SQL Server Management Studio |
|| Kryptografieanbieter | Eine [Konvertierung](../../virtual-machines/windows/azure-key-vault-integration-configure.md) wird empfohlen, damit Sie Azure Key Vault verwenden können. Hierfür wird der Ressourcenanbieter [Microsoft.SqlVirtualMachine](../../virtual-machines/windows/sql-agent-extension-manually-register-single-vm.md) verwendet. |
| **Serverobjekte** | Sicherungsmedien | Ersetzen Sie die Datenbanksicherung durch [Azure Backup](../../../backup/backup-sql-server-database-azure-vms.md) oder schreiben Sie Sicherungen in [Azure Storage](../../virtual-machines/windows/azure-storage-sql-server-backup-restore-use.md) (SQL Server 2012 SP1 CU2 und höher). Hierfür wird der Ressourcenanbieter [Microsoft.SqlVirtualMachine](../../virtual-machines/windows/sql-agent-extension-manually-register-single-vm.md) verwendet.|
|| Verbindungsserver | Skript mit SQL Server Management Studio |
|| Servertrigger | Skript mit SQL Server Management Studio |
| **Replikation** | Lokale Veröffentlichungen | Skript mit SQL Server Management Studio |
|| Lokale Abonnenten | Skript mit SQL Server Management Studio |
| **PolyBase** | PolyBase | Skript mit SQL Server Management Studio |
| **Verwaltung** | Datenbank-E-Mail | Skript mit SQL Server Management Studio |
| **SQL Server-Agent** | Aufträge | Skript mit SQL Server Management Studio |
|| Alerts | Skript mit SQL Server Management Studio |
|| Operatoren | Skript mit SQL Server Management Studio |
|| Proxys | Skript mit SQL Server Management Studio |
| **Betriebssystem** | Dateien, Dateifreigaben | Notieren Sie alle zusätzlichen Dateien oder Dateifreigaben, die von Ihren SQL Server-Instanzen verwendet werden, und replizieren Sie diese auf der Azure-Ziel-VM. |

## <a name="post-migration"></a>Nach der Migration

Nach erfolgreichem Abschluss der Migrationsphase müssen Sie eine Reihe von Aufgaben ausführen, um sicherzustellen, dass alles so reibungslos und effizient wie möglich funktioniert.

### <a name="remediate-applications"></a>Korrigieren von Anwendungen

Wenn die Daten in die Zielumgebung migriert wurden, müssen alle Anwendungen, die zuvor die Quelle verwendet haben, beginnen das Ziel zu verwenden. Hierfür sind in einigen Fällen Änderungen an den Anwendungen erforderlich.

Wenden Sie alle von Datenmigrations-Assistent empfohlenen Fixes auf Benutzer Datenbanken an. Sie müssen eine Skripterstellung für diese Korrekturen durch Schreiben, um Konsistenz und Automatisierung zu gewährleisten.

### <a name="perform-tests"></a>Durchführen von Tests

Das Testvorgehen für die Datenbankmigration umfasst die folgenden Aktivitäten:

1. **Entwickeln von Validierungstests:** Für das Testen der Datenbankmigration müssen Sie SQL-Abfragen verwenden. Erstellen Sie Validierungsabfragen, die für die Quell- und die Zieldatenbanken ausgeführt werden sollen. Ihre Validierungsabfragen sollten den von Ihnen definierten Bereich abdecken.
1. **Einrichten der Testumgebung:** Die Testumgebung sollte eine Kopie der Quelldatenbank und der Zieldatenbank enthalten. Stellen Sie sicher, dass Sie die Testumgebung isolieren.
1. **Ausführen der Validierungstests:** Führen Sie die Validierungstests für die Quelle und das Ziel aus und analysieren Sie anschließend die Ergebnisse.
1. **Ausführen von Leistungstests:** Führen Sie Leistungstests für die Quelle und das Ziel aus, und analysieren und vergleichen Sie anschließend die Ergebnisse.

> [!TIP]
> Verwenden Sie den [Assistenten für Datenbankexperimente (DEA)](/sql/dea/database-experimentation-assistant-overview), um die Leistung der SQL Server-Zielinstanz zu bewerten.

### <a name="optimize"></a>Optimieren

Die Phase nach der Migration ist entscheidend für den Abgleich von Problemen mit der Datengenauigkeit, die Überprüfung der Vollständigkeit und das Beheben von Leistungsproblemen der Arbeitsauslastung.

Hier finden Sie weitere Informationen zu diesen Problemen und den Behebungsschritten:

- [Leitfaden für die Überprüfung und Optimierung nach der Migration](/sql/relational-databases/post-migration-validation-and-optimization-guide)
- [Optimieren der Leistung bei SQL Server auf Azure-VMs](../../virtual-machines/windows/performance-guidelines-best-practices.md)
- [Kostenoptimierung in Azure](https://azure.microsoft.com/overview/cost-optimization/)

## <a name="next-steps"></a>Nächste Schritte

- Die Verfügbarkeit der mit SQL Server kompatiblen Dienste finden Sie auf der Seite für die [globale Azure-Infrastruktur](https://azure.microsoft.com/global-infrastructure/services/?regions=all&amp;products=synapse-analytics,virtual-machines,sql-database).
- Eine Übersicht über die Dienste und Tools von Microsoft und Drittanbietern, mit denen Sie verschiedene Datenbank- und Datenmigrationsszenarien sowie Spezialaufgaben ausführen können, finden Sie unter [Dienste und Tools für die Datenmigration](../../../dms/dms-tools-matrix.md).
- Hier finden Sie weitere Informationen zu Azure SQL:
   - [Bereitstellungsoptionen](../../azure-sql-iaas-vs-paas-what-is-overview.md)
   - [SQL Server auf virtuellen Azure-Computern](../../virtual-machines/windows/sql-server-on-azure-vm-iaas-what-is-overview.md)
   - [Azure-Gesamtkostenrechner](https://azure.microsoft.com/pricing/tco/calculator/)

- Weitere Informationen zum Framework und zum Einführungszyklus von Cloudmigrationen finden Sie unter:
   - [Cloud Adoption Framework für Azure](/azure/cloud-adoption-framework/migrate/azure-best-practices/contoso-migration-scale)
   - [Bewährte Methoden für die Kostenermittlung und Größenanpassung von zu Azure migrierten Arbeitsauslastungen](/azure/cloud-adoption-framework/migrate/azure-best-practices/migrate-best-practices-costs)

- Hier finden Sie Informationen zur Lizenzierung:
   - [Bring-Your-Own-License mit dem Azure-Hybridvorteil](../../virtual-machines/windows/licensing-model-azure-hybrid-benefit-ahb-change.md)
   - [Erweiterter Support für SQL Server 2008 und SQL Server 2008 R2](../../virtual-machines/windows/sql-server-2008-extend-end-of-support.md)

- Informationen zum Bewerten der Anwendungszugriffsebene finden Sie unter [Data Access Migration Toolkit (Vorschauversion)](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit).
- Informationen zum Ausführen von A/B-Tests auf Datenzugriffsebene finden Sie unter [Überblick über den Assistenten für Datenbankexperimente](/sql/dea/database-experimentation-assistant-overview).
