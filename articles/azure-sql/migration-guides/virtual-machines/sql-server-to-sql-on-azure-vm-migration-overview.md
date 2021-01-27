---
title: SQL Server zu SQL Server auf Azure-VMs (Migrationsübersicht)
description: Erfahren Sie mehr über die verschiedenen Migrationsstrategien, wenn Sie Ihre SQL Server-Instanz zu SQL Server auf Azure-VMs migrieren möchten.
ms.custom: ''
ms.service: virtual-machines-sql
ms.subservice: migration-guide
ms.devlang: ''
ms.topic: how-to
author: markjones-msft
ms.author: markjon
ms.reviewer: mathoma
ms.date: 11/06/2020
ms.openlocfilehash: 0eabb48aabcb50557b342385068807eb67a9b165
ms.sourcegitcommit: 95c2cbdd2582fa81d0bfe55edd32778ed31e0fe8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/26/2021
ms.locfileid: "98797847"
---
# <a name="migration-overview-sql-server-to-sql-server-on-azure-vms"></a>Migrationsübersicht: SQL Server zu SQL Server auf Azure-VMs
[!INCLUDE[appliesto--sqlmi](../../includes/appliesto-sqlvm.md)]

Erfahren Sie mehr über die verschiedenen Migrationsstrategien, um Ihre SQL Server-Instanz zu SQL Server auf Azure Virtual Machines (VMs) zu migrieren. 

Sie können SQL Server-Instanzen migrieren, die lokal oder auf folgenden Plattformen ausgeführt werden:

- SQL Server auf virtuellen Computern  
- Amazon Web Services (AWS) EC2 
- Amazon Relational Database Service (AWS RDS) 
- Compute Engine (Google Cloud Platform – GCP)

Informationen zu anderen Szenarios finden Sie im [Leitfaden zur Datenbankmigration](https://datamigration.microsoft.com/). 

## <a name="overview"></a>Übersicht

Migrieren Sie zu [SQL Server auf Azure Virtual Machines (VMs)](../../virtual-machines/windows/sql-server-on-azure-vm-iaas-what-is-overview.md), wenn Sie die vertraute SQL Server-Umgebung mit Betriebssystemkontrolle verwenden möchten und die Vorteile von Cloudfeatures wie integrierte Hochverfügbarkeit von virtuellen Computern, [automatisierte Sicherungen](../../virtual-machines/windows/automated-backup.md) und [automatisiertes Patchen](../../virtual-machines/windows/automated-patching.md) nutzen möchten. 

Sparen Sie Kosten, indem Sie Ihre eigene Lizenz mit dem [Lizenzmodell mit Azure-Hybridvorteil](../../virtual-machines/windows/licensing-model-azure-hybrid-benefit-ahb-change.md) bereitstellen, oder erweitern Sie die Unterstützung für SQL Server 2008 und SQL Server 2008 R2, indem Sie [kostenlose Sicherheitsupdates](../../virtual-machines/windows/sql-server-2008-extend-end-of-support.md) erhalten. 


## <a name="choose-appropriate-target"></a>Auswählen des richtigen Ziels

Azure Virtual Machines kann in vielen verschiedenen Azure-Regionen ausgeführt werden und bietet außerdem eine Vielzahl von [Größen virtueller Computer](../../../virtual-machines/sizes.md) und [Speicheroptionen](../../../virtual-machines/disks-types.md). Bei der Bestimmung der richtigen Größe von VM und Speicher für Ihre SQL Server-Workload beziehen Sie sich auf die [Leistungsrichtlinien für SQL Server auf Azure Virtual Machines](../../virtual-machines/windows/performance-guidelines-best-practices.md#vm-size-guidance). So ermitteln Sie VM-Größe und Speicheranforderungen für Ihre Workload Es wird empfohlen, dass deren Größe durch eine leistungsbasierte [Azure Migrate-Bewertung](../../../migrate/concepts-assessment-calculation.md#types-of-assessments) angepasst wird. Wenn dies keine verfügbare Option ist, lesen Sie den folgenden Artikel über das Erstellen einer eigenen [Baseline für die Leistung](https://azure.microsoft.com/services/virtual-machines/sql-server/).

Es sollten auch Überlegungen über die ordnungsgemäße Installation und Konfiguration von SQL Server auf einem virtuellen Computer angestellt werden. Es wird empfohlen, den [Imagekatalog für virtuelle Azure SQL-Computer](../../virtual-machines/windows/create-sql-vm-portal.md) zu verwenden, da Sie auf diese Weise eine SQL Server-VM mit der richtigen Version, Edition und dem richtigen Betriebssystem erstellen können. Dadurch wird auch die Azure-VM automatisch beim SQL Server-[Ressourcenanbieter](../../virtual-machines/windows/create-sql-vm-portal.md) registriert, wodurch Features wie automatisierte Sicherungen und automatisiertes Patchen aktiviert werden.

## <a name="migration-strategies"></a>Migrationsstrategien

Es gibt zwei Migrationsstrategien, um Ihre Benutzerdatenbanken auf eine Instanz von SQL Server auf Azure-VMs zu migrieren: **Migrieren** und **Lift & Shift**. 

Der geeignete Ansatz für Ihr Unternehmen hängt in der Regel von den folgenden Faktoren ab: 

- Größe und Skalierung der Migration
- Geschwindigkeit der Migration
- Anwendungsunterstützung für Codeänderungen
- Die Anforderung zum Ändern der SQL Server-Version, des Betriebssystems oder beider Optionen.
- Lebenszyklus der Unterstützbarkeit Ihrer vorhandenen Produkte
- Fenster für Anwendungsdowntime während der Migration

:::image type="content" source="media/sql-server-to-sql-on-azure-vm-individual-databases-guide/virtual-machine-migration-downtime.png" alt-text="Downtime während der Migration virtueller Computer":::

In der folgenden Tabelle werden die Unterschiede zwischen den beiden Migrationsstrategien beschrieben:
<br />

| **Migrationsstrategie** | **Beschreibung** | **Einsatzgebiete** |
| --- | --- | --- |
| **Lift & Shift** | Verwenden Sie die Lift & Shift-Migrationsstrategie, um den gesamten physischen oder virtuellen SQL Server von seinem aktuellen Standort auf eine Instanz von SQL Server auf Azure-VMs zu verschieben, ohne Änderungen am Betriebssystem oder der SQL Server-Version vorzunehmen. Informationen zum Durchführen einer Lift & Shift-Migration finden Sie unter [Azure Migrate](../../../migrate/migrate-services-overview.md). <br /><br /> Der Quellserver bleibt online und bearbeitet Anforderungen, während Quell- und Zielserver die Daten synchronisieren, was eine nahezu nahtlose Migration ermöglicht. | Verwendung für einzelne bis sehr umfangreiche Migrationen, sogar anwendbar für Szenarien wie den Ausstieg aus einem Rechenzentrum. <br /><br /> Es sind nur minimale bis keine Codeänderungen an den SQL-Datenbanken oder Anwendungen der Benutzer erforderlich, sodass insgesamt schnellere Migrationen möglich sind. <br /><br />Es sind keine zusätzlichen Schritte für die Migration der Business Intelligence-Dienste wie [SSIS](/sql/integration-services/sql-server-integration-services), [SSRS](/sql/reporting-services/create-deploy-and-manage-mobile-and-paginated-reports) und [SSAS](/analysis-services/analysis-services-overview) erforderlich. |
|**Migrieren** | Verwenden Sie eine Migrationsstrategie, wenn Sie die SQL Server- und/oder Betriebssystemzielversion aktualisieren möchten. <br /> <br /> Wählen Sie eine Azure-VM aus dem Azure Marketplace oder ein vorbereitetes SQL Server-Image aus, das mit der SQL Server-Quellversion übereinstimmt. | Verwenden Sie diese Option, wenn die Anforderung oder der Wunsch besteht, in neueren Versionen von SQL Server verfügbare Features zu verwenden, oder wenn es erforderlich ist, ein Upgrade von Legacy-SQL Server- und/oder -Betriebssystemversionen durchzuführen, die nicht mehr unterstützt werden.  <br /> <br /> Möglicherweise sind zur Unterstützung des SQL Server-Upgrades einige Änderungen an Anwendungen oder Benutzerdatenbanken erforderlich. <br /><br />Es kann weitere Überlegungen zur Migration von [Business Intelligence](#business-intelligence)-Diensten geben, wenn diese im Rahmen der Migration erfolgen. |


## <a name="lift-and-shift"></a>Lift & Shift  

In der folgenden Tabelle wird die verfügbare Methode für die **Lift & Shift**-Migrationsstrategie zum Migrieren Ihrer SQL Server-Datenbank zu SQL Server auf Azure-VMs beschrieben:
<br />

|**Methode** | **Minimale Quellversion** | **Minimale Zielversion** | **Größenbeschränkung für Quellsicherung** |  **Hinweise** |
| --- | --- | --- | --- | --- |
| [Azure Migrate](../../../migrate/index.yml) | SQL Server 2008 SP4| SQL Server 2008 SP4| [Azure VM-Speichergrenze](../../../index.yml) |  Vorhandene SQL Server-Instanz, die unverändert in eine Instanz von SQL Server auf einer Azure-VM verschoben werden soll. Kann Migrationsworkloads von bis zu 35.000 VMs skalieren. <br /><br /> Quellserver bleiben online und bearbeiten Anforderungen während der Synchronisierung der Serverdaten, wodurch die Downtime minimiert wird. <br /><br /> **Automatisierung und Skripterstellung**: [Azure Site Recovery-Skripts](../../../migrate/how-to-migrate-at-scale.md) und [Beispiel für skalierte Migration und Planung für Azure](/azure/cloud-adoption-framework/migrate/azure-best-practices/contoso-migration-scale)|

## <a name="migrate"></a>Migrieren  

Aufgrund der einfachen Einrichtung wird als Migrationsansatz empfohlen, lokal eine native SQL Server-[Sicherung](/sql/t-sql/statements/backup-transact-sql) zu verwenden und dann die Datei nach Azure zu kopieren. Diese Methode unterstützt größere Datenbanken (>1 TB) für alle Versionen von SQL Server ab 2008 und größere Datenbanksicherungen (>1 TB). Für Datenbanken ab SQL Server 2014, die kleiner als 1 TB sind und über eine gute Konnektivität zu Azure verfügen, ist jedoch die [SQL Server-Sicherung über URLs](/sql/relational-databases/backup-restore/sql-server-backup-to-url) der bessere Ansatz. 

Bei der Migration von SQL Server-Datenbanken zu einer Instanz von SQL Server auf Azure-VMs ist es wichtig, einen geeigneten Ansatz zu wählen, wenn eine Übernahme auf den Zielserver erforderlich ist, da dies das Fenster für die Downtime der Anwendung beeinflusst.

In der folgenden Tabelle sind alle verfügbaren Methoden zur Migration Ihrer SQL Server-Datenbank zu SQL Server auf Azure-VMs aufgeführt:
<br />

|**Methode** | **Minimale Quellversion** | **Minimale Zielversion** | **Größenbeschränkung für Quellsicherung** | **Hinweise** |
| --- | --- | --- | --- | --- |
| **[Sicherung in einer Datei](sql-server-to-sql-on-azure-vm-individual-databases-guide.md#migrate)** | SQL Server 2008 SP4 | SQL Server 2008 SP4| [Azure VM-Speichergrenze](../../../index.yml) |  Dies ist eine einfache und ausführlich getestete Methode zum Verschieben von Datenbanken auf verschiedene Computer. Verwenden Sie die Komprimierung, um die Größe der Sicherung für die Übertragung zu minimieren. <br /><br /> **Automatisierung und Skripterstellung**: [Transact-SQL (T-SQL)](/sql/t-sql/statements/backup-transact-sql) und [AzCopy in Blobspeicher](../../../storage/common/storage-use-azcopy-v10.md)  |
| **[Erstellen von Sicherungen über URLs](/sql/relational-databases/backup-restore/sql-server-backup-to-url)** | SQL Server 2012 SP1 CU2 | SQL Server 2012 SP1 CU2| 12,8 TB für SQL Server 2016, andernfalls 1 TB | Eine alternative Möglichkeit, die Sicherungsdatei mithilfe von Azure-Speicher auf die VM zu verschieben. Verwenden Sie die Komprimierung, um die Größe der Sicherung für die Übertragung zu minimieren. <br /><br /> **Automatisierung und Skripterstellung**:  [T-SQL oder Wartungsplan](/sql/relational-databases/backup-restore/sql-server-backup-to-url) |
| **[Datenbankmigrations-Assistent (DMA)](/sql/dma/dma-overview)** | SQL Server 2005| SQL Server 2008 SP4| [Azure VM-Speichergrenze](../../../index.yml) |  Der [DMA](/sql/dma/dma-overview) bewertet SQL Server lokal und führt dann nahtlos ein Upgrade auf spätere Versionen von SQL Server durch oder migriert zu SQL Server auf Azure-VMs, Azure SQL-Datenbank oder Azure SQL Managed Instance. <br /><br /> Sollte nicht für Filestream-fähige Benutzerdatenbanken verwendet werden.<br /><br /> Der DMA umfasst auch die Möglichkeit, [SQL- und Windows-Anmeldungen](/sql/dma/dma-migrateserverlogins) zu migrieren und [SSIS-Pakete](/sql/dma/dma-assess-ssis) zu bewerten. <br /><br /> **Automatisierung und Skripterstellung**: [Befehlszeilenschnittstelle](/sql/dma/dma-commandline) |
| **[Trennen und Anfügen](../../virtual-machines/windows/migrate-to-vm-from-sql-server.md#detach-and-attach-from-a-url)** | SQL Server 2008 SP4 | SQL Server 2014 | [Azure VM-Speichergrenze](../../../index.yml) | Verwenden Sie diese Methode, wenn Sie planen, [diese Dateien mithilfe des Azure Blob Storage-Diensts](/sql/relational-databases/databases/sql-server-data-files-in-microsoft-azure) zu speichern und sie an eine Instanz von SQL Server auf einer Azure-VM anzufügen, was besonders bei sehr großen Datenbanken nützlich ist oder wenn die Zeit für Sicherung und Wiederherstellung zu lang ist. <br /><br /> **Automatisierung und Skripterstellung**:  [T-SQL](/sql/relational-databases/databases/detach-a-database#TsqlProcedure) und [AzCopy in Blobspeicher](../../../storage/common/storage-use-azcopy-v10.md)|
|**[Protokollversand](sql-server-to-sql-on-azure-vm-individual-databases-guide.md#migrate)** | SQL Server 2008 SP4 (nur Windows) | SQL Server 2008 SP4 (nur Windows) | [Azure VM-Speichergrenze](../../../index.yml) | Der Protokollversand repliziert Transaktionsprotokolldateien von lokalen Speicherorten auf eine Instanz von SQL Server auf einer Azure-VM. <br /><br /> Dies ermöglicht eine minimale Downtime während des Failovers und erfordert einen geringeren Konfigurationsaufwand als die Einrichtung einer Always On-Verfügbarkeitsgruppe. <br /><br /> **Automatisierung und Skripterstellung**: [T-SQL](/sql/database-engine/log-shipping/log-shipping-tables-and-stored-procedures)  |
| **[Verteilte Verfügbarkeitsgruppe](../../virtual-machines/windows/business-continuity-high-availability-disaster-recovery-hadr-overview.md#hybrid-it-disaster-recovery-solutions)** | SQL Server 2016| SQL Server 2016 | [Azure VM-Speichergrenze](../../../index.yml) |  Eine [verteilte Verfügbarkeitsgruppe](/sql/database-engine/availability-groups/windows/distributed-availability-groups) ist ein spezieller Typ einer Verfügbarkeitsgruppe, der zwei separate Verfügbarkeitsgruppen umfasst. Die Verfügbarkeitsgruppen, die Teil einer verteilten Verfügbarkeitsgruppe sind, müssen sich weder am selben Ort befinden noch die domänenübergreifende Unterstützung umfassen. <br /><br /> Diese Methode minimiert die Downtime und wird verwendet, wenn Sie eine Verfügbarkeitsgruppe lokal konfiguriert haben. <br /><br /> **Automatisierung und Skripterstellung**: [T-SQL](/sql/t-sql/statements/alter-availability-group-transact-sql)  |
| | | | | |

&nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp;
 
> [!TIP]
> Informationen zu umfangreichen Datenübertragungen mit eingeschränkten oder keinen Netzwerkoptionen finden Sie unter [Umfangreiche Datenübertragungen mit eingeschränkter Konnektivität](../../../storage/common/storage-solution-large-dataset-low-network.md).
> 

### <a name="considerations"></a>Überlegungen

Im Folgenden finden Sie eine Liste mit den wichtigsten Punkten, die bei der Überprüfung von Migrationsmethoden zu berücksichtigen sind:

- Für eine optimale Datenübertragungsleistung migrieren Sie Datenbanken und Dateien auf eine Instanz von SQL Server auf Azure-VM unter Verwendung einer komprimierten Sicherungsdatei. Bei größeren Datenbanken wird zusätzlich zur Komprimierung [die Sicherungsdatei in kleinere Dateien aufgeteilt](/sql/relational-databases/backup-restore/back-up-files-and-filegroups-sql-server), um die Leistung während der Sicherung und Übertragung zu erhöhen. 
- Wenn Sie von SQL Server 2014 oder höher migrieren, sollten Sie die [Verschlüsselung der Sicherungen](/sql/relational-databases/backup-restore/backup-encryption) in Betracht ziehen, um die Daten während der Netzwerkübertragung zu schützen.
- Um die Downtime während der Datenbankmigration zu minimieren, verwenden Sie die Option für die Always On-Verfügbarkeitsgruppe. 
- Verwenden Sie die Option für den Protokollversand, um die Downtime ohne den Aufwand der Konfiguration einer Verfügbarkeitsgruppe zu minimieren. 
- Wenn Sie über keine oder eingeschränkte Netzwerkoptionen verfügen, verwenden Sie Offlinemigrationsmethoden wie die Sicherung und Wiederherstellung oder die in Azure verfügbaren [Datenträgerübertragungsdienste](../../../storage/common/storage-solution-large-dataset-low-network.md).
- Informationen zum Ändern der Version von SQL Server für eine Instanz von SQL Server auf Azure-VM finden Sie unter [Ändern der SQL Server-Edition](../../virtual-machines/windows/change-sql-server-edition.md).

## <a name="business-intelligence"></a>Business Intelligence 

Bei der Migration von SQL Server Business Intelligence-Diensten außerhalb des Bereichs der Benutzerdatenbankmigrationen kann es zusätzliche Überlegungen geben. 

Zu diesen Diensten gehören:

- [**SQL Server Integration Services (SSIS)**](/sql/integration-services/install-windows/upgrade-integration-services)
- [**SQL Server Reporting Services (SSRS)**](/sql/reporting-services/install-windows/upgrade-and-migrate-reporting-services)
- [**SQL Server Analysis Services (SSAS)**](/sql/database-engine/install-windows/upgrade-analysis-services)

## <a name="supported-versions"></a>Unterstützte Versionen

Wenn Sie sich auf die Migration von SQL Server-Datenbanken zu SQL Server auf Azure-VMs vorbereiten, sollten Sie unbedingt die unterstützten Versionen von SQL Server berücksichtigen. Eine Liste der aktuell unterstützten SQL Server-Versionen auf Azure-VMs finden Sie unter [SQL Server auf Azure-VMs](../../virtual-machines/windows/sql-server-on-azure-vm-iaas-what-is-overview.md#get-started-with-sql-server-vms).

## <a name="migration-assets"></a>Migrationsressourcen 

Weitere Unterstützung finden Sie in den folgenden Ressourcen, die für reale Migrationsprojekte entwickelt wurden.

|Asset  |BESCHREIBUNG  |
|---------|---------|
|[Data Workload Assessment Model and Tool (Datenarbeitsauslastungs-Bewertungsmodell und -tool)](https://github.com/microsoft/DataMigrationTeam/tree/master/IP%20and%20Scripts/Data%20Workload%20Assessment%20Model%20and%20Tool)| Dieses Tool stellt für eine angegebene Arbeitsauslastung Informationen zu empfohlenen optimalen Zielplattformen, zur Cloudbereitschaft und zum Korrekturbedarf für Anwendungen/Datenbanken bereit. Es bietet eine einfache Berechnung und Berichterstellung mit nur einem Klick, die Ihnen durch Bereitstellung eines automatisierten und einheitlichen Zielplattform-Entscheidungsprozesses dabei helfen, umfangreiche Bewertungen zu beschleunigen.|
|[Automatisierung der Perfmon-Datensammlung mithilfe von Logman](https://github.com/microsoft/DataMigrationTeam/tree/master/IP%20and%20Scripts/Perfmon%20Data%20Collection%20Automation%20Using%20Logman)|Hierbei handelt es sich um ein Tool, das Perfmon-Daten erfasst, um die Baselineleistung zu verstehen, die die Empfehlung für das Migrationsziel unterstützt. Dieses Tool verwendet die ausführbare Datei „logman.exe“, um den Befehl zu erstellen, der die Leistungsindikatoren für eine SQL Server-Remoteinstanz erstellt, startet, beendet und löscht.|
|[SQL Server Deployment in Azure](https://github.com/microsoft/DataMigrationTeam/blob/master/Whitepapers/SQL%20Server%20Deployment%20in%20Azure%20.pdf) (SQL Server-Bereitstellung in Azure)|Dieses Whitepaper enthält einen Leitfaden, der Sie bei der Überprüfung verschiedener Optionen unterstützt, mit denen Sie Ihre SQL Server-Workloads nach Azure verschieben können. Dies schließt auch Featurevergleiche, Hochverfügbarkeit und Überlegungen zur Sicherung und Speicherung ein. |
|[On-Premise SQL Server to Azure virtual machine](https://github.com/microsoft/DataMigrationTeam/blob/master/Whitepapers/OnPremise%20SQL%20Server%20to%20Azure%20VM.pdf) (Migrieren von lokalem SQL Server zu Azure-VMs)|In diesem Whitepaper werden die Schritte zum Sichern und Wiederherstellen von Datenbanken aus lokalen SQL Server-Instanzen zu SQL Server auf Azure-VMs mithilfe von Beispielskripts beschrieben.|
|[Multiple-SQL-VM-VNet-ILB](https://github.com/microsoft/DataMigrationTeam/tree/master/IP%20and%20Scripts/ARM%20Templates/Multiple-SQL-VM-VNet-ILB)|In diesem Whitepaper werden die Schritte zum Einrichten mehrerer Azure-VMs in einer SQL Server-Konfiguration mit Always On-Verfügbarkeitsgruppen beschrieben.|
|[Azure virtual machines supporting Ultra SSD per Region](https://github.com/microsoft/DataMigrationTeam/tree/master/IP%20and%20Scripts/Find%20Azure%20VMs%20supporting%20Ultra%20SSD) (Azure-VMs mit Unterstützung für SSD Ultra nach Region)|Diese PowerShell-Skripts bieten eine programmgesteuerte Möglichkeit zum Abrufen der Liste der Regionen, die Azure-VMs mit SSD Ultra unterstützen.|

Diese Ressourcen wurden im Rahmen des Data SQL Ninja-Programms entwickelt, das vom Azure Data Group-Entwicklungsteam gesponsert wird. Der Hauptzweck des Data SQL Ninja-Programms besteht darin, Hindernisse für komplexe Modernisierung zu beseitigen und Letztere zu beschleunigen sowie den Wettbewerb in Bezug auf Möglichkeiten zur Migration von Datenplattformen zur Azure-Datenplattform von Microsoft zu fördern. Wenn Sie denken, dass Ihre Organisation an einer Teilnahme am Data SQL Ninja-Programm interessiert wäre, wenden Sie sich an Ihr Kundenteam, und bitten Sie es, eine Nominierung einzureichen.

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Starten der Migration Ihrer SQL Server-Datenbanken zu SQL Server auf Azure-VMs finden Sie im [Migrationsleitfaden für einzelne Datenbanken](sql-server-to-sql-on-azure-vm-individual-databases-guide.md). 

- Eine Übersicht über die Dienste und Tools von Microsoft und Drittanbietern, mit denen Sie verschiedene Datenbank- und Datenmigrationsszenarios und Spezialaufgaben ausführen können, finden Sie im Artikel [Dienste und Tools für die Datenmigration](../../../dms/dms-tools-matrix.md).

- Hier finden Sie weitere Informationen zu Azure SQL:
   - [Bereitstellungsoptionen](../../azure-sql-iaas-vs-paas-what-is-overview.md)
   - [SQL Server auf Azure-VMs](../../virtual-machines/windows/sql-server-on-azure-vm-iaas-what-is-overview.md)
   - [Azure-Gesamtkostenrechner](https://azure.microsoft.com/pricing/tco/calculator/) 


- Weitere Informationen zum Framework und zum Einführungszyklus von Cloudmigrationen finden Sie unter:
   -  [Cloud Adoption Framework für Azure](/azure/cloud-adoption-framework/migrate/azure-best-practices/contoso-migration-scale)
   -  [Bewährte Methoden für die Kostenermittlung und Größenanpassung von zu Azure migrierten Workloads](/azure/cloud-adoption-framework/migrate/azure-best-practices/migrate-best-practices-costs) 

- Hier finden Sie weitere Informationen zur Lizenzierung:
   - [Bring-Your-Own-License mit dem Azure-Hybridvorteil](../../virtual-machines/windows/licensing-model-azure-hybrid-benefit-ahb-change.md)
   - [Erweiterter Support für SQL Server 2008 und SQL Server 2008 R2](../../virtual-machines/windows/sql-server-2008-extend-end-of-support.md)


- Informationen zum Bewerten der Anwendungszugriffsebene finden Sie unter [Data Access Migration Toolkit (Vorschauversion)](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit).
- Ausführliche Informationen zum Ausführen von A/B-Tests auf Datenzugriffsebene finden Sie unter [Assistent für Datenbankexperimente](/sql/dea/database-experimentation-assistant-overview).