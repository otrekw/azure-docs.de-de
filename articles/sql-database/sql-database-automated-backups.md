---
title: Automatische, georedundante Sicherungen
description: SQL-Datenbank erstellt alle paar Minuten automatisch eine lokale Datenbanksicherung und verwendet georedundanten Azure-Speicher mit Lesezugriff (Read-Access Geographically Redundant Storage, RA-GRS), um für Georedundanz zu sorgen.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab, danil
manager: craigg
ms.date: 12/13/2019
ms.openlocfilehash: 7858d626282b3090e6c95af9c06b3a0a7e603462
ms.sourcegitcommit: 318d1bafa70510ea6cdcfa1c3d698b843385c0f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2020
ms.locfileid: "83773594"
---
# <a name="automated-backups"></a>Automatisierte Sicherungen

Azure SQL-Datenbank erstellt automatisch die Datenbanksicherungen, die für die Dauer des konfigurierten Aufbewahrungszeitraums aufbewahrt werden. Dazu wird [georedundanter Azure-Speicher mit Lesezugriff (RA-GRS)](../storage/common/storage-redundancy.md) verwendet, um sicherzustellen, dass die Sicherungen auch dann erhalten bleiben, wenn das Rechenzentrum nicht verfügbar ist.

Datenbanksicherungen sind ein wesentlicher Bestandteil jeder Strategie für Geschäftskontinuität und Notfallwiederherstellung, da Ihre Daten vor versehentlichen Beschädigungen und Löschungen geschützt werden. Falls es gemäß Ihren Sicherheitsregeln erforderlich ist, dass Ihre Sicherungen über einen längeren Zeitraum verfügbar sind (bis zu 10 Jahre), können Sie eine [Langzeitaufbewahrung](sql-database-long-term-retention.md) in Einzeldatenbanken und Pools für elastische Datenbanken konfigurieren.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="what-is-a-sql-database-backup"></a>Was ist die Sicherung einer SQL-Datenbank-Instanz?

SQL-Datenbank nutzt SQL Server-Technologie, um wöchentlich [vollständige Sicherungen](https://docs.microsoft.com/sql/relational-databases/backup-restore/full-database-backups-sql-server), alle 12 Stunden [differenzielle Sicherungen](https://docs.microsoft.com/sql/relational-databases/backup-restore/differential-backups-sql-server) und alle 5 bis 10 Minuten [Transaktionsprotokollsicherungen](https://docs.microsoft.com/sql/relational-databases/backup-restore/transaction-log-backups-sql-server) zu erstellen. Die Sicherungen werden in [RA-GRS-Speicherblobs](../storage/common/storage-redundancy.md) gespeichert, die in einem [gekoppelten Rechenzentrum](../best-practices-availability-paired-regions.md) repliziert werden, um Schutz vor Rechenzentrumsausfällen zu bieten. Wenn Sie eine Datenbank wiederherstellen, bestimmt der Dienst, welche vollständigen und differenziellen Sicherungen bzw. Transaktionsprotokollsicherungen wiederhergestellt werden müssen.

Sie können diese Sicherungen für Folgendes verwenden:

- **Stellen Sie für eine vorhandene Datenbank den Stand zu einem vergangenen Zeitpunkt wieder her**, der innerhalb des Aufbewahrungszeitraums liegt, indem Sie das Azure-Portal, Azure PowerShell, die Azure CLI oder die REST-API verwenden. Für Einzeldatenbanken und Pools für elastische Datenbanken wird bei diesem Vorgang eine neue Datenbank auf Server erstellt, auf dem sich die ursprüngliche Datenbank befindet. Bei einer verwalteten Instanz kann mit diesem Vorgang eine Kopie der Datenbank in derselben oder einer anderen verwalteten Instanz unter demselben Abonnement erstellt werden.
- **Stellen Sie für eine gelöschte Datenbank den Stand zum Zeitpunkt des Löschvorgangs wieder her** oder den Stand zu einem beliebigen anderen Zeitpunkt innerhalb des Aufbewahrungszeitraums. Die gelöschte Datenbank kann nur auf demselben logischen Server oder in der verwalteten Instanz wiederhergestellt werden, auf dem bzw. in der die ursprüngliche Datenbank erstellt wurde.
- **Stellen Sie eine Datenbank in einer anderen geografischen Region wieder her**. Die Geowiederherstellung ermöglicht die Wiederherstellung nach dem Ausfall einer geografischen Region, wenn Sie keinen Zugriff auf Ihren Server und Ihre Datenbank haben. Dabei wird eine neue Datenbank auf einem beliebigen Server an einem beliebigen Ort der Welt erstellt.
- **Führen Sie die Wiederherstellung einer Datenbank aus einer spezifischen langfristigen Sicherung durch** (in einer Einzeldatenbank oder einem Pool für elastische Datenbanken), wenn die Datenbank mit einer Richtlinie zur Langzeitaufbewahrung (Long-Term Retention, LTR) konfiguriert wurde. Mit LTR können Sie eine alte Version der Datenbank wiederherstellen, indem Sie [das Azure-Portal](sql-database-long-term-backup-retention-configure.md#using-azure-portal) oder [Azure PowerShell](sql-database-long-term-backup-retention-configure.md#using-powershell) verwenden, um eine Konformitätsanforderung zu erfüllen oder eine alte Version der Anwendung auszuführen. Weitere Informationen finden Sie unter [Langfristige Aufbewahrung](sql-database-long-term-retention.md).

Informationen zum Durchführen einer Wiederherstellung finden Sie unter [Wiederherstellen einer Azure SQL-Datenbank mit automatisierten Datenbanksicherungen](sql-database-recovery-using-backups.md).

> [!NOTE]
> In Azure Storage bezieht sich der Begriff *Replikation* auf das Kopieren von Dateien von einem Speicherort an einen anderen. In SQL Server bedeutet *Datenbankreplikation*, dass mehrere sekundäre Datenbanken mit einer primären Datenbank synchron bleiben.

Sie können einige dieser Vorgänge ausprobieren, indem Sie die folgenden Beispiele verwenden:

| | Das Azure-Portal | Azure PowerShell |
|---|---|---|
| Ändern der Sicherungsaufbewahrung | [Einzeldatenbank](sql-database-automated-backups.md?tabs=managed-instance#change-the-pitr-backup-retention-period-by-using-the-azure-portal) <br/> [Verwaltete Instanz](sql-database-automated-backups.md?tabs=managed-instance#change-the-pitr-backup-retention-period-by-using-the-azure-portal) | [Einzeldatenbank](sql-database-automated-backups.md#change-the-pitr-backup-retention-period-by-using-powershell) <br/>[Verwaltete Instanz](https://docs.microsoft.com/powershell/module/az.sql/set-azsqlinstancedatabasebackupshorttermretentionpolicy) |
| Ändern der Langzeitaufbewahrung von Sicherungen | [Einzeldatenbank](sql-database-long-term-backup-retention-configure.md#configure-long-term-retention-policies)<br/>Verwaltete Instanz: nicht verfügbar  | [Einzeldatenbank](sql-database-long-term-backup-retention-configure.md)<br/>Verwaltete Instanz: nicht verfügbar  |
| Wiederherstellen einer Datenbank bis zu einem Zeitpunkt | [Einzeldatenbank](sql-database-recovery-using-backups.md#point-in-time-restore) | [Einzeldatenbank](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqldatabase) <br/> [Verwaltete Instanz](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqlinstancedatabase) |
| Wiederherstellung einer gelöschten Datenbank | [Einzeldatenbank](sql-database-recovery-using-backups.md) | [Einzeldatenbank](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldeleteddatabasebackup) <br/> [Verwaltete Instanz](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldeletedinstancedatabasebackup)|
| Wiederherstellen der Datenbank aus Azure Blob Storage | Einzeldatenbank: Nicht verfügbar <br/>Verwaltete Instanz: nicht verfügbar  | Einzeldatenbank: Nicht verfügbar <br/>[Verwaltete Instanz](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-get-started-restore) |


## <a name="backup-frequency"></a>Sicherungshäufigkeit

### <a name="point-in-time-restore"></a>Wiederherstellung bis zu einem bestimmten Zeitpunkt

SQL-Datenbank unterstützt Self-Service für die Point-in-Time-Wiederherstellung (Point-in-Time Restore, PITR), indem automatisch vollständige Sicherungen, differenzielle Sicherungen und Transaktionsprotokollsicherungen erstellt werden. Vollständige Datenbanksicherungen werden wöchentlich, differenzielle Datenbanksicherungen im Allgemeinen alle 12 Stunden erstellt. Transaktionsprotokollsicherungen werden meist alle 5 bis 10 Minuten erstellt. Die Häufigkeit von Transaktionsprotokollsicherungen basiert auf der Computegröße und dem Umfang der Datenbankaktivität. 

Die erste vollständige Sicherung wird unmittelbar nach der Datenbankerstellung geplant. Diese Sicherung wird normalerweise innerhalb von 30 Minuten abgeschlossen, kann aber länger dauern, wenn die Datenbank groß ist. Die erste Sicherung kann bei einer wiederhergestellten Datenbank oder einer Datenbankkopie beispielsweise länger dauern. Nach der ersten vollständigen Sicherung werden alle weiteren Sicherungen automatisch geplant und im Hintergrund verwaltet. Die genaue Zeitplanung für alle Datenbanksicherungen wird vom SQL-Datenbank-Dienst je nach der gesamten Systemworkload bestimmt. Sie können die Sicherungsaufträge nicht ändern oder deaktivieren.

### <a name="default-backup-retention-period"></a>Standardaufbewahrungsdauer für Sicherungen

PITR-Sicherungen sind durch georedundanten Speicher geschützt. Weitere Informationen finden Sie unter [Azure Storage-Redundanz](../storage/common/storage-redundancy.md).

Weitere Informationen zur PITR finden Sie unter [Point-in-Time-Wiederherstellung](sql-database-recovery-using-backups.md#point-in-time-restore).

### <a name="long-term-retention"></a>Langfristige Aufbewahrung

Für Einzel- und Pooldatenbanken können Sie die Langzeitaufbewahrung (LTR) vollständiger Sicherungen für eine Dauer von bis zu 10 Jahren in Azure Blob Storage konfigurieren. Wenn Sie die LTR-Richtlinie aktivieren, werden die wöchentlichen vollständigen Sicherungen automatisch in einen anderen RA-GRS-Speichercontainer kopiert. Zur Einhaltung verschiedener Konformitätsanforderungen können Sie verschiedene Aufbewahrungszeiträume für wöchentliche, monatliche oder jährliche Sicherungen auswählen. Der Speicherbedarf hängt von der ausgewählten Häufigkeit der Sicherungen und Aufbewahrungsdauer ab. Sie können den [LTR-Preisrechner](https://azure.microsoft.com/pricing/calculator/?service=sql-database) verwenden, um die Kosten für den LTR-Speicher zu schätzen.

Wie bei PITR-Sicherungen sind LTR-Sicherungen durch georedundanten Speicher geschützt. Weitere Informationen finden Sie unter [Azure Storage-Redundanz](../storage/common/storage-redundancy.md).

Weitere Informationen zu LTR finden Sie unter [Langfristiges Aufbewahren von Sicherungen](sql-database-long-term-retention.md).

## <a name="backup-storage-consumption"></a>Sicherungsspeicherverbrauch

Bei Einzeldatenbanken wird der gesamte Sicherungsspeicherbedarf wie folgt berechnet:

`Total backup storage size = (size of full backups + size of differential backups + size of log backups) – database size`

Bei Pools für elastische Datenbanken wird die gesamte Sicherungsspeichergröße auf Poolebene aggregiert und wie folgt berechnet:

`Total backup storage size = (total size of all full backups + total size of all differential backups + total size of all log backups) - allocated pool data storage`

Sicherungen, die vor dem Aufbewahrungszeitraum erfolgt sind, werden basierend auf ihrem Zeitstempel automatisch bereinigt. Da differenzielle Sicherungen und Protokollsicherungen erst von Nutzen sind, wenn zuvor eine vollständige Sicherung erfolgt ist, werden sie zusammen in wöchentlichen Blöcken bereinigt.

Azure SQL-Datenbank berechnet den gesamten Sicherungsspeicher in Aufbewahrung als kumulativen Wert. Jede Stunde wird dieser Wert an die Azure-Abrechnungspipeline gemeldet, die für die Aggregierung dieser stündlichen Nutzung verantwortlich ist, um die Nutzung am Ende jedes Monats zu berechnen. Nach dem Löschen der Datenbank sinkt der Verbrauch mit zunehmendem Alter der Sicherungen. Sobald das Alter der Sicherungen den Aufbewahrungszeitraum überschreitet, wird die Abrechnung eingestellt.

   > [!IMPORTANT]
   > Sicherungen einer Datenbank werden für die angegebene Beibehaltungsdauer beibehalten, auch wenn die Datenbank gelöscht wurde. Mit häufigem Löschen und Neuerstellen einer Datenbank können zwar Speicher- und Computekosten eingespart werden, aber die Sicherungsspeicherkosten können steigen, da Microsoft für jede gelöschte Datenbank bei jeder Löschung eine Sicherung für die angegebene Beibehaltungsdauer beibehält.

### <a name="monitor-consumption"></a>Überwachen des Verbrauchs

Jeder Sicherungstyp (vollständig, differenziell und Protokoll) wird auf dem Blatt für die Datenbanküberwachung als separate Metrik ausgewiesen. Im folgenden Diagramm wird gezeigt, wie Sie den Sicherungsspeicherverbrauch für eine Einzeldatenbank überwachen. Dieses Feature ist derzeit für verwaltete Instanzen nicht verfügbar.

![Überwachen des Sicherungsspeicherverbrauchs von Datenbanken im Azure-Portal](media/sql-database-automated-backup/backup-metrics.png)

### <a name="fine-tune-backup-storage-consumption"></a>Optimieren des Sicherungsspeicherverbrauchs

Der zusätzliche Sicherungsspeicherverbrauch hängt von der Workload und der Größe der einzelnen Datenbanken ab. Ziehen Sie einige der folgenden Optimierungstechniken in Betracht, um den Sicherungsspeicherverbrauch zu reduzieren:

* Reduzieren Sie den [Aufbewahrungszeitraum für Sicherungen](#change-the-pitr-backup-retention-period-by-using-the-azure-portal) auf die Mindestanforderungen für Ihre Zwecke.
* Vermeiden Sie es, große Schreibvorgänge, wie z.B. die Neuerstellung von Indizes, öfter als nötig durchzuführen.
* Bei umfangreichen Datenladevorgängen sollten Sie [gruppierte Columnstore-Indizes](https://docs.microsoft.com/sql/database-engine/using-clustered-columnstore-indexes) verwenden, die Anzahl der nicht gruppierten Indizes reduzieren und auch Massenladevorgänge mit einer Zeilenanzahl von rund 1 Mio. in Erwägung ziehen.
* Auf der Dienstebene „Universell“ ist der bereitgestellte Datenspeicher günstiger als die Kosten für den zusätzlichen Sicherungsspeicher. Wenn ständig hohe Kosten durch zusätzlichen Sicherungsspeicher anfallen, können Sie eine Vergrößerung des Datenspeichers in Betracht ziehen, um den Sicherungsspeicher einzusparen.
* Verwenden Sie TempDB anstelle permanenter Tabellen in Ihrer ETL-Logik zum Speichern temporärer Ergebnisse. (Gilt nur für eine verwaltete Instanz.)
* Deaktivieren Sie ggf. die TDE-Verschlüsselung für Datenbanken, die keine sensiblen Daten enthalten (z. B. Entwicklungs- oder Testdatenbanken). Sicherungen für nicht verschlüsselte Datenbanken werden in der Regel mit einem höheren Komprimierungsverhältnis komprimiert.

> [!IMPORTANT]
> Für analytische Data Mart-\Data Warehouse-Workloads sollten Sie unbedingt [gruppierte Columnstore-Indizes](https://docs.microsoft.com/sql/database-engine/using-clustered-columnstore-indexes) verwenden, die Anzahl der nicht gruppierten Indizes reduzieren und Massenladevorgänge mit einer Zeilenanzahl von rund 1 Mio. in Erwägung ziehen, um den zusätzlichen Verbrauch von Sicherungsspeicher zu verringern.

## <a name="storage-costs"></a>Speicherkosten

Der Preis für den Speicher variiert abhängig davon, ob Sie das Modell „DTU“ oder „Virtueller Kern“ verwenden.

### <a name="dtu-model"></a>DTU-Modell

Beim DTU-Modell fallen keine zusätzlichen Kosten für den Sicherungsspeicher für Datenbanken und Pools für elastische Datenbanken an.

### <a name="vcore-model"></a>V-Kern-Modell

Für Einzeldatenbanken wird eine Mindestmenge an Sicherungsspeicher, die der Gesamtgröße der Datenbank entspricht, kostenlos zur Verfügung gestellt. Bei Pools für elastische Datenbanken und verwalteten Instanzen wird eine Mindestmenge an Sicherungsspeicher, die der Gesamtgröße des zugeordneten Datenspeichers für den Pool bzw. der Instanzgröße entspricht, ohne zusätzliche Kosten bereitgestellt. Zusätzlich verbrauchter Sicherungsspeicher wird pro GB und Monat abgerechnet. Dieser zusätzliche Verbrauch hängt von der Workload und der Größe der einzelnen Datenbanken ab.

Azure SQL-Datenbank berechnet den gesamten Sicherungsspeicher in der Aufbewahrung als kumulativen Wert. Jede Stunde wird dieser Wert der Azure-Abrechnungspipeline gemeldet, die für die Aggregierung dieser stündlichen Nutzung verantwortlich ist, um den Verbrauch am Ende jedes Monats zu berechnen. Nach dem Löschen der Datenbank senkt Microsoft den Verbrauch mit zunehmendem Alter der Sicherungen. Sobald das Alter der Sicherungen den Aufbewahrungszeitraum überschreitet, wird die Abrechnung eingestellt. Da alle Protokoll- und differenziellen Sicherungen für den gesamten Aufbewahrungszeitraum beibehalten werden, fallen für häufig geänderte Datenbanken höhere Sicherungskosten an.

Angenommen, die Datenbank hat 744 GB Sicherungsspeicher angesammelt, und diese Menge bleibt während eines ganzen Monats konstant. Um diesen kumulativen Speicherverbrauch in eine stündliche Nutzung umzurechnen, dividieren wir ihn durch 744,0 (31 Tage pro Monat x 24 Stunden pro Tag). So meldet SQL-Datenbank, dass die Datenbank pro Stunde 1 GB an PITR-Sicherungen verbraucht hat. Die Azure-Abrechnung aggregiert dies und zeigt eine Nutzung von 744 GB für den gesamten Monat. Die Kosten basierend auf dem EUR/GB/Monat-Satz in Ihrer Region an. Die Kosten basierend auf den monatlichen Kosten pro GB in Ihrer Region.

Jetzt folgt ein komplexeres Beispiel. Angenommen, der Aufbewahrungszeitraum der Datenbank wird Mitte des Monats auf 14 Tage verlängert. Nehmen wir an, dass diese Zunahme (hypothetisch) dazu führt, dass sich der gesamte Sicherungsspeicher auf 1.488 GB verdoppelt. SQL-Datenbank meldet eine Nutzung von 1 GB für die Stunden 1 bis 372. Die Nutzung wird als 2 GB für die Stunden 373 bis 744 gemeldet. Die monatliche Schlussrechnung basiert dann auf dem aggregierten Wert von 1.116 GB.

### <a name="monitor-costs"></a>Überwachen der Kosten

Um die Kosten für Sicherungsspeicher zu verstehen, wechseln Sie im Azure-Portal zu **Kostenverwaltung + Abrechnung**. Wählen Sie **Kostenverwaltung** und dann **Kostenanalyse** aus. Wählen Sie das gewünschte Abonnement als **Bereich** aus, und filtern Sie dann nach dem gewünschten Zeitraum und Dienst.

Fügen Sie einen Filter für **Dienstname** hinzu, und wählen Sie dann in der Dropdownliste **SQL-Datenbank** aus. Verwenden Sie den Filter **Unterkategorie der Verbrauchseinheit**, um den Abrechnungszähler für Ihren Dienst auszuwählen. Wählen Sie für eine einzelne Datenbank oder einen Pool für elastische Datenbanken den **PITR-Sicherungsspeicher für eine einzelne Datenbank/einen Pool für elastische Datenbanken** aus. Wählen Sie für eine verwaltete Instanz **MI-PITR-Sicherungsspeicher** aus. Die Unterkategorien **Speicher** und **Compute** können für Sie auch von Interesse sein, obwohl sie nicht im Zusammenhang mit den Sicherungsspeicherkosten stehen.

![Analyse der Kosten für Sicherungsspeicher](./media/sql-database-automated-backup/check-backup-storage-cost-sql-mi.png)

## <a name="backup-retention"></a>Sicherungsaufbewahrung

Bei allen Azure SQL-Datenbanken (Einzeldatenbanken, Pooldatenbanken und Datenbanken der verwalteten Instanz) gilt eine Standardaufbewahrungsdauer für Sicherungen von 7 Tagen. Sie können die [Aufbewahrungsdauer für Sicherungen in bis zu 35 Tage ändern](#change-the-pitr-backup-retention-period).

Wenn Sie eine Datenbank löschen, bewahrt SQL-Datenbank die Sicherungen auf die gleiche Weise wie für eine Onlinedatenbank auf. Beim Löschen einer Datenbank des Typs „Basic“, für die eine Aufbewahrungsdauer von sieben Tagen gilt, wird eine vier Tage alte Sicherung weitere drei Tage aufbewahrt.

Falls Sie die Sicherungen länger als die maximale Aufbewahrungsdauer beibehalten müssen, können Sie die Sicherungseigenschaften so ändern, dass Ihrer Datenbank eine oder mehrere Zeiträume für die langfristige Aufbewahrung hinzugefügt werden. Weitere Informationen finden Sie unter [Langfristige Aufbewahrung](sql-database-long-term-retention.md).

> [!IMPORTANT]
> Wenn Sie die Azure SQL Server-Instanz löschen, in der SQL-Datenbanken gehostet werden, werden alle Pools für elastische Datenbanken und Datenbanken, die zum Server gehören, ebenfalls gelöscht. Sie können nicht wiederhergestellt werden. Es ist nicht möglich, einen gelöschten Server wiederherzustellen. Wenn Sie aber die langfristige Aufbewahrung konfiguriert haben, werden die Sicherungen für die Datenbanken mit LTR nicht gelöscht, und diese Datenbanken können wiederhergestellt werden.

> [!NOTE]
> Die minimale PITR-Sicherungsaufbewahrung, die für einzelne, in einem Pool zusammengefasste Datenbanken und verwaltete Instanzen über das Azure-Portal konfiguriert werden kann, beträgt 7 Tage. Die minimale PITR-Sicherungsaufbewahrung von 1 Tag kann für eine verwaltete Instanz nur mithilfe von PowerShell mit dem As.SQL-Modul v2.6.0 oder höher konfiguriert werden.

## <a name="encrypted-backups"></a>Verschlüsselte Sicherungen

Wenn Ihre Datenbank mit TDE verschlüsselt ist, werden Sicherungen im Ruhezustand, einschließlich LTR-Sicherungen, automatisch verschlüsselt. Wenn Transparent Data Encryption (TDE) für eine Azure SQL-Datenbank aktiviert ist, werden die Sicherungen ebenfalls verschlüsselt. In allen neuen Azure SQL-Datenbanken ist TDE standardmäßig aktiviert. Weitere Informationen finden Sie unter [Transparent Data Encryption mit Azure SQL-Datenbank](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql).

## <a name="backup-integrity"></a>Sicherungsintegrität

Fortlaufend testet das Entwicklungsteam von Azure SQL-Datenbank automatisch die Wiederherstellung automatischer Sicherungen von Datenbanken auf logischen Servern und in Pools für elastische Datenbanken. (In einer verwalteten Instanz stehen dieses Tests nicht zur Verfügung.) Bei der Point-in-Time-Wiederherstellung werden die Datenbanken außerdem mithilfe von DBCC CHECKDB Integritätsprüfungen unterzogen.

Nach Abschluss der Migration führt die verwaltete Instanz mit `CHECKSUM` eine automatische Erstsicherung von Datenbanken durch, die mit dem nativen Befehl `RESTORE` oder Azure Data Migration Service wiederhergestellt wurden.

Mögliche Probleme, die bei der Integritätsprüfung gefunden werden, führen zu einer Warnung des Entwicklungsteams. Weitere Informationen finden Sie unter [Data Integrity in Azure SQL Database (Datenintegrität in Azure SQL-Datenbank)](https://azure.microsoft.com/blog/data-integrity-in-azure-sql-database/).

## <a name="compliance"></a>Kompatibilität

Wenn Sie Ihre Datenbank von einer DTU-basierten Dienstebene zu einer Dienstebene auf Basis virtueller Kerne migrieren, wird die PITR-Aufbewahrung beibehalten. So soll sichergestellt werden, dass die Datenwiederherstellungsrichtlinie Ihrer Anwendung nicht kompromittiert wird. Falls die Standardaufbewahrung Ihre Konformitätsanforderungen nicht erfüllt, können Sie die PITR-Aufbewahrungsdauer per PowerShell oder REST-API ändern. Weitere Informationen finden Sie unter [Ändern des PITR-Aufbewahrungszeitraums von Sicherungen](#change-the-pitr-backup-retention-period).

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="change-the-pitr-backup-retention-period"></a>Ändern des PITR-Aufbewahrungszeitraums von Sicherungen

Sie können den Standardzeitraum für die Aufbewahrung von PITR-Sicherungen im Azure-Portal, mit PowerShell oder der REST-API ändern. In den folgenden Beispielen wird veranschaulicht, wie Sie die PITR-Aufbewahrungsdauer in 28 Tage ändern.

> [!WARNING]
> Wenn Sie die aktuelle Aufbewahrungsdauer reduzieren, sind alle vorhandenen Sicherungen, die älter als die neue Aufbewahrungsdauer sind, nicht mehr verfügbar. Wenn Sie die aktuelle Aufbewahrungsdauer erhöhen, behält SQL-Datenbank die vorhandenen Sicherungen bei, bis das Ende der längeren Aufbewahrungsdauer erreicht ist.

> [!NOTE]
> Diese APIs wirken sich nur auf die PITR-Aufbewahrungsdauer aus. Falls Sie für Ihre Datenbank LTR konfiguriert haben, ist sie nicht betroffen. Informationen zum Ändern von LTR-Aufbewahrungsdauern finden Sie unter [Langfristige Aufbewahrung](sql-database-long-term-retention.md).

### <a name="change-the-pitr-backup-retention-period-by-using-the-azure-portal"></a>Ändern der PITR-Aufbewahrungsdauer im Azure-Portal

Um die Aufbewahrungsdauer der PITR-Sicherung im Azure-Portal zu ändern, navigieren Sie im Portal zum Serverobjekt, dessen Aufbewahrungsdauer Sie ändern möchten. Wählen Sie dann die entsprechende Option basierend auf dem Serverobjekt aus, das Sie ändern.

#### <a name="single-database-and-elastic-database-pools"></a>[Einzeldatenbank und Pools für elastische Datenbanken](#tab/single-database)

Das Ändern der Aufbewahrungsdauer für PITR-Sicherungen für einzelne Azure SQL-Datenbank-Instanzen erfolgt auf Serverebene. Die auf Serverebene vorgenommene Änderung betrifft die Datenbanken auf dem jeweiligen Server. Um die PITR-Aufbewahrung für eine Azure SQL-Datenbank-Serverinstanz im Azure-Portal zu ändern, wechseln Sie zum Blatt mit der Übersicht über den Server. Wählen Sie im linken Bereich **Sicherungen verwalten** und dann oben auf dem Bildschirm **Aufbewahrung konfigurieren** aus:

![Ändern der PITR-Aufbewahrung, Serverebene](./media/sql-database-automated-backup/configure-backup-retention-sqldb.png)

#### <a name="managed-instance"></a>[Verwaltete Instanz](#tab/managed-instance)

Die Änderung der Aufbewahrung von PITR-Sicherungen für verwaltete SQL-Datenbank-Instanzen erfolgt auf der Ebene der einzelnen Datenbanken. Um die Aufbewahrung von PITR-Sicherungen für eine Instanzdatenbank im Azure-Portal zu ändern, navigieren Sie zum Übersichtsblatt der jeweiligen Datenbank. Wählen Sie dann oben auf dem Bildschirm **Aufbewahrungsdauer für Sicherungen konfigurieren** aus.

![Ändern der PITR-Aufbewahrung, verwaltete Instanz](./media/sql-database-automated-backup/configure-backup-retention-sqlmi.png)

---

### <a name="change-the-pitr-backup-retention-period-by-using-powershell"></a>Ändern der PITR-Aufbewahrungsdauer mit PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> Das PowerShell-Modul AzureRM wird von Azure SQL-Datenbank weiterhin unterstützt, aber alle Weiterentwicklungen erfolgen für das Modul Az.Sql. Weitere Informationen finden Sie unter [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Die Argumente für die Befehle im Az-Modul und den AzureRm-Modulen sind im Wesentlichen identisch.

```powershell
Set-AzSqlDatabaseBackupShortTermRetentionPolicy -ResourceGroupName resourceGroup -ServerName testserver -DatabaseName testDatabase -RetentionDays 28
```

### <a name="change-the-pitr-backup-retention-period-by-using-the-rest-api"></a>Ändern der PITR-Aufbewahrungsdauer über die REST-API

#### <a name="sample-request"></a>Beispiel für eine Anforderung

```http
PUT https://management.azure.com/subscriptions/00000000-1111-2222-3333-444444444444/resourceGroups/resourceGroup/providers/Microsoft.Sql/servers/testserver/databases/testDatabase/backupShortTermRetentionPolicies/default?api-version=2017-10-01-preview
```

#### <a name="request-body"></a>Anforderungstext

```json
{
  "properties":{
    "retentionDays":28
  }
}
```

#### <a name="sample-response"></a>Beispiel für eine Antwort

Statuscode: 200

```json
{
  "id": "/subscriptions/00000000-1111-2222-3333-444444444444/providers/Microsoft.Sql/resourceGroups/resourceGroup/servers/testserver/databases/testDatabase/backupShortTermRetentionPolicies/default",
  "name": "default",
  "type": "Microsoft.Sql/resourceGroups/servers/databases/backupShortTermRetentionPolicies",
  "properties": {
    "retentionDays": 28
  }
}
```

Weitere Informationen finden Sie unter [REST-API für die Aufbewahrung von Sicherungen](https://docs.microsoft.com/rest/api/sql/backupshorttermretentionpolicies).

## <a name="next-steps"></a>Nächste Schritte

- Datenbanksicherungen sind ein wesentlicher Bestandteil jeder Strategie für Geschäftskontinuität und Notfallwiederherstellung, da Ihre Daten vor versehentlichen Beschädigungen und Löschungen geschützt werden. Weitere Informationen zu den anderen Geschäftskontinuitätslösungen von Azure SQL-Datenbank finden Sie unter [Übersicht über die Geschäftskontinuität mit Azure SQL-Datenbank](sql-database-business-continuity.md).
- Erfahren Sie mehr zum [Wiederherstellen einer Datenbank bis zu einem bestimmten Zeitpunkt mithilfe des Azure-Portals](sql-database-recovery-using-backups.md).
- Erfahren Sie mehr zum [Wiederherstellen einer Datenbank bis zu einem bestimmten Zeitpunkt mit PowerShell](scripts/sql-database-restore-database-powershell.md).
- Informationen zum Konfigurieren, Verwalten und Wiederherstellen aus der Langzeitaufbewahrung automatisierter Sicherungen in Azure Blob Storage im Azure-Portal finden Sie im Artikel [Verwalten der langfristigen Aufbewahrung von Sicherungen im Azure-Portal](sql-database-long-term-backup-retention-configure.md).
- Informationen zum Konfigurieren, Verwalten und Wiederherstellen aus der langfristigen Aufbewahrung automatisierter Sicherungen in Azure Blob Storage mit PowerShell finden Sie unter [Verwalten der langfristigen Aufbewahrung von Sicherungen mit PowerShell](sql-database-long-term-backup-retention-configure.md).
