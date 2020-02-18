---
title: Automatische, georedundante Sicherungen
description: SQL-Datenbank erstellt alle paar Minuten automatisch eine lokale Datenbanksicherung und verwendet georedundanten Azure-Speicher mit Lesezugriff (Read-Access Geographically Redundant Storage, RA-GRS), um für Georedundanz zu sorgen.
services: sql-database
ms.service: sql-database
ms.subservice: backup-restore
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab, danil
manager: craigg
ms.date: 12/13/2019
ms.openlocfilehash: f460bc3e4809b8a1cbabe1161c888255a7a484db
ms.sourcegitcommit: 76bc196464334a99510e33d836669d95d7f57643
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2020
ms.locfileid: "77157498"
---
# <a name="automated-backups"></a>Automatisierte Sicherungen

SQL-Datenbank erstellt automatisch die Datenbanksicherungen, die für die Dauer des konfigurierten Aufbewahrungszeitraums gespeichert werden. Es wird [georedundanter Azure-Speicher mit Lesezugriff (RA-GRS)](../storage/common/storage-redundancy.md) verwendet, um sicherzustellen, dass diese auch dann beibehalten werden, wenn das Rechenzentrum nicht verfügbar ist. Diese Sicherungskopien werden automatisch erstellt. Datenbanksicherungen sind ein wesentlicher Bestandteil jeder Strategie für Geschäftskontinuität und Notfallwiederherstellung, da Ihre Daten vor versehentlichen Beschädigungen und Löschungen geschützt werden. Falls es gemäß Ihren Sicherheitsregeln erforderlich ist, dass Ihre Sicherungen über einen längeren Zeitraum verfügbar sind (bis zu 10 Jahre), können Sie eine [Langzeitaufbewahrung](sql-database-long-term-retention.md) in Singleton-Datenbanken und Pools für elastische Datenbanken konfigurieren.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="what-is-a-sql-database-backup"></a>Was ist die Sicherung einer SQL-Datenbank-Instanz?

SQL-Datenbank nutzt SQL Server-Technologie, um wöchentlich [vollständige Sicherungen](https://docs.microsoft.com/sql/relational-databases/backup-restore/full-database-backups-sql-server), alle zwölf Stunden [differenzielle Sicherungen](https://docs.microsoft.com/sql/relational-databases/backup-restore/differential-backups-sql-server) und alle fünf bis zehn Minuten [Transaktionsprotokollsicherungen](https://docs.microsoft.com/sql/relational-databases/backup-restore/transaction-log-backups-sql-server) zu erstellen. Die Sicherungen werden in [RA-GRS-Speicherblobs](../storage/common/storage-redundancy.md) gespeichert, die in einem [gekoppelten Rechenzentrum](../best-practices-availability-paired-regions.md) repliziert werden, um Schutz vor Rechenzentrumsausfällen zu bieten. Wenn Sie eine Datenbank wiederherstellen, ermittelt der Dienst, welche vollständigen und differenziellen Sicherungen bzw. Transaktionsprotokollsicherungen wiederhergestellt werden müssen.

Sie können diese Sicherungen für Folgendes verwenden:

- **Stellen Sie für eine vorhandene Datenbank den Stand zu einem vergangenen Zeitpunkt wieder her**, der innerhalb des Aufbewahrungszeitraums liegt, indem Sie das Azure-Portal, Azure PowerShell, die Azure CLI oder die REST-API verwenden. In Einzeldatenbanken und Pools für elastische Datenbanken wird mit diesem Vorgang eine neue Datenbank auf demselben Server erstellt, auf dem sich auch die ursprüngliche Datenbank befindet. Bei einer verwalteten Instanz kann mit diesem Vorgang eine Kopie der Datenbank in derselben oder einer anderen verwalteten Instanz unter demselben Abonnement erstellt werden.
- **Stellen Sie für eine gelöschte Datenbank den Stand zum Zeitpunkt des Löschvorgangs wieder her**, oder den Stand zu einem beliebigen anderen Zeitpunkt innerhalb des Aufbewahrungszeitraums. Die gelöschte Datenbank kann nur auf demselben logischen Server oder der verwalteten Instanz wiederhergestellt werden, auf dem bzw. der die ursprüngliche Datenbank erstellt wurde.
- **Stellen Sie eine Datenbank in einer anderen geografischen Region wieder her**. Die Geowiederherstellung ermöglicht die Wiederherstellung nach dem Ausfall einer geografischen Region, wenn Sie keinen Zugriff auf Ihren Server und Ihre Datenbank haben. Dabei wird eine neue Datenbank auf einem beliebigen Server an einem beliebigen Ort der Welt erstellt.
- **Führen Sie die Wiederherstellung einer Datenbank aus einer spezifischen langfristigen Sicherung durch** (in einer Einzeldatenbank oder einem Pool für elastische Datenbanken), wenn die Datenbank mit einer Richtlinie zur Langzeitaufbewahrung (Long-Term Retention, LTR) konfiguriert wurde. Mit LTR können Sie eine alte Version der Datenbank wiederherstellen, indem Sie [das Azure-Portal](sql-database-long-term-backup-retention-configure.md#using-azure-portal) oder [Azure PowerShell](sql-database-long-term-backup-retention-configure.md#using-powershell) verwenden, um eine Konformitätsanforderung zu erfüllen oder eine alte Version der Anwendung auszuführen. Weitere Informationen finden Sie unter [Langfristige Aufbewahrung](sql-database-long-term-retention.md).
- Informationen zum Durchführen einer Wiederherstellung finden Sie unter [Wiederherstellen einer Azure SQL-Datenbank mit automatisierten Datenbanksicherungen](sql-database-recovery-using-backups.md).

> [!NOTE]
> In Azure Storage bezieht sich der Begriff *Replikation* auf das Kopieren von Dateien von einem Speicherort zum anderen. Die *Datenbankreplikation* der SQL bezieht sich auf das Beibehalten von mehreren sekundären Datenbanken, die mit einer primären Datenbank synchronisiert werden.

Sie können einige dieser Vorgänge ausprobieren, indem Sie die folgenden Beispiele verwenden:

| | Das Azure-Portal | Azure PowerShell |
|---|---|---|
| Ändern der Sicherungsaufbewahrung | [Einzeldatenbank](sql-database-automated-backups.md?tabs=managed-instance#change-pitr-backup-retention-period-using-azure-portal) <br/> [Verwaltete Instanz](sql-database-automated-backups.md?tabs=managed-instance#change-pitr-backup-retention-period-using-azure-portal) | [Einzeldatenbank](sql-database-automated-backups.md#change-pitr-backup-retention-period-using-powershell) <br/>[Verwaltete Instanz](https://docs.microsoft.com/powershell/module/az.sql/set-azsqlinstancedatabasebackupshorttermretentionpolicy) |
| Ändern der Langzeitaufbewahrung von Sicherungen | [Einzeldatenbank](sql-database-long-term-backup-retention-configure.md#configure-long-term-retention-policies)<br/>Verwaltete Instanz: Nicht verfügbar  | [Einzeldatenbank](sql-database-long-term-backup-retention-configure.md)<br/>Verwaltete Instanz: Nicht verfügbar  |
| Wiederherstellen des Stands einer Datenbank zu einem bestimmten Zeitpunkt | [Einzeldatenbank](sql-database-recovery-using-backups.md#point-in-time-restore) | [Einzeldatenbank](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqldatabase) <br/> [Verwaltete Instanz](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqlinstancedatabase) |
| Wiederherstellen einer gelöschten Datenbank | [Einzeldatenbank](sql-database-recovery-using-backups.md) | [Einzeldatenbank](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldeleteddatabasebackup) <br/> [Verwaltete Instanz](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldeletedinstancedatabasebackup)|
| Wiederherstellen der Datenbank aus Azure Blob Storage | Einzeldatenbank: Nicht verfügbar <br/>Verwaltete Instanz: Nicht verfügbar  | Einzeldatenbank: Nicht verfügbar <br/>[Verwaltete Instanz](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-get-started-restore) |


## <a name="backup-frequency"></a>Sicherungshäufigkeit

### <a name="point-in-time-restore"></a>Wiederherstellung bis zu einem bestimmten Zeitpunkt

SQL-Datenbank unterstützt den Self-Service für die Point-in-Time-Wiederherstellung (Point-in-Time Restore, PITR), indem automatisch vollständige Sicherungen, differenzielle Sicherungen und Transaktionsprotokollsicherungen erstellt werden. Vollständige Datensicherungen werden wöchentlich erstellt. Differenzielle Sicherungen erfolgen im Allgemeinen alle 12 Stunden und Transaktionsprotokollsicherungen alle 5-10 Minuten, wobei die Häufigkeit auf der Computegröße und dem Umfang der Datenbankaktivität basiert. Die erste vollständige Sicherung wird unmittelbar nach der Datenbankerstellung geplant. Sie wird normalerweise innerhalb von 30 Minuten abgeschlossen, aber der Vorgang kann auch länger dauern, wenn es sich um eine sehr große Datenbank handelt. Die erste Sicherung kann bei einer wiederhergestellten Datenbank oder einer Datenbankkopie beispielsweise länger dauern. Nach der ersten vollständigen Sicherung werden alle weiteren Sicherungen automatisch geplant und im Hintergrund verwaltet. Die genaue Zeitplanung für alle Datenbanksicherungen wird vom SQL-Datenbank-Dienst je nach der gesamten Systemworkload bestimmt. Sie können die Sicherungsaufträge nicht ändern oder deaktivieren.

Die PITR-Sicherungen werden durch georedundanten Speicher geschützt. Weitere Informationen finden Sie unter [Azure Storage-Redundanz](../storage/common/storage-redundancy.md).

Weitere Informationen finden Sie unter [Point-in-Time-Wiederherstellung](sql-database-recovery-using-backups.md#point-in-time-restore).

### <a name="long-term-retention"></a>Langfristige Aufbewahrung

Einzel- und Pooldatenbanken verfügen über eine Option zum Konfigurieren der Langzeitaufbewahrung (LTR) von vollständigen Sicherungen für eine Dauer von bis zu zehn Jahren in Azure Blob Storage. Wenn die LTR-Richtlinie aktiviert ist, werden die wöchentlichen vollständigen Sicherungen automatisch in einen anderen RA-GRS-Speichercontainer kopiert. Zur Erfüllung unterschiedlicher Konformitätsanforderungen können Sie verschiedene Aufbewahrungsdauern für wöchentliche, monatliche oder jährliche Sicherungen auswählen. Der Speicherverbrauch hängt von der ausgewählten Häufigkeit der Sicherungen und von den Aufbewahrungsdauern ab. Sie können den [LTR-Preisrechner](https://azure.microsoft.com/pricing/calculator/?service=sql-database) verwenden, um die Kosten für den LTR-Speicher zu schätzen.

Wie bei PITR werden die LTR-Sicherungen durch georedundanten Speicher geschützt. Weitere Informationen finden Sie unter [Azure Storage-Redundanz](../storage/common/storage-redundancy.md).

Weitere Informationen finden Sie unter [Langfristiges Aufbewahren von Sicherungen](sql-database-long-term-retention.md).

## <a name="backup-storage-consumption"></a>Sicherungsspeicherverbrauch 

Bei einzelnen Datenbanken wird der gesamte Sicherungsspeicherverbrauch wie folgt berechnet:   
`Total backup storage size = (size of full backups + size of differential backups + size of log backups) – database size`.

Bei Pools für elastische Datenbanken wird die gesamte Sicherungsspeichergröße auf Poolebene aggregiert und wie folgt berechnet:   
`Total backup storage size = (total size of all full backups + total size of all differential backups + total size of all log backups) - allocated pool data storage`. 

Sicherungen, deren Alter den Aufbewahrungszeitraum überschreitet, werden basierend auf ihrem Zeitstempel automatisch bereinigt. Da die differenziellen Sicherungen und Protokollsicherungen erst von Nutzen sind, wenn eine frühere vollständige Sicherung erfolgt, werden sie zusammen in wöchentlichen Blöcken bereinigt. 

Azure SQL-Datenbank berechnet den gesamten Sicherungsspeicher in der Aufbewahrung als kumulativen Wert. Jede Stunde wird dieser Wert an die Azure-Abrechnungspipeline gemeldet, die für die Aggregierung dieser stündlichen Nutzung verantwortlich ist, um den Verbrauch am Ende jedes Monats zu berechnen. Nach dem Löschen der Datenbank sinkt der Verbrauch mit zunehmendem Alter der Sicherungen. Sobald das Alter der Sicherungen den Aufbewahrungszeitraum überschreitet, wird die Abrechnung beendet. 


### <a name="monitoring-consumption"></a>Überwachen des Verbrauchs

Jeder Sicherungstyp (vollständig, differenziell und Protokoll) wird auf dem Blatt für die Datenbanküberwachung als separate Metrik angezeigt. Im folgenden Diagramm wird gezeigt, wie Sie den Speicherverbrauch der Sicherungen überwachen.  

![Überwachen des Verbrauchs der Datenbanksicherung auf dem Blatt für die Datenbanküberwachung des Azure-Portals](media/sql-database-automated-backup/backup-metrics.png)

### <a name="fine-tune-the-backup-storage-consumption"></a>Optimieren des Sicherungsspeicherverbrauchs

Der zusätzliche Sicherungsspeicherverbrauch hängt von der Workload und der Größe der einzelnen Datenbanken ab. Sie können einige der folgenden Optimierungstechniken implementieren, um den Sicherungsspeicherverbrauch weiter zu reduzieren:

* Reduzieren Sie den [Aufbewahrungszeitraum für Sicherungen](#change-pitr-backup-retention-period-using-azure-portal) auf die Mindestanforderungen für Ihre Zwecke.
* Vermeiden Sie so weit wie möglich große Schreibvorgänge wie z. B. Neuerstellungen von Indizes.
* Bei umfangreichen Datenladevorgängen sollten Sie [gruppierte Columnstore-Indizes](https://docs.microsoft.com/sql/database-engine/using-clustered-columnstore-indexes) verwenden, die Anzahl der nicht gruppierten Indizes reduzieren und auch Massenladevorgänge mit einer Zeilenzahl von rund einer Million in Erwägung ziehen.
* In der Dienstebene „Universell“ ist der bereitgestellte Datenspeicher kostengünstiger als zusätzlicher Sicherungsspeicher. Darum sollten Kunden, bei denen fortlaufend hohe Kosten für zusätzlichen Sicherungsspeicher anfallen, erwägen, den Datenspeicher zu erhöhen, um Sicherungsspeicher zu sparen.
* Verwenden Sie TempDB in Ihrer ETL-Logik zum Speichern temporärer Ergebnisse anstelle permanenter Tabellen (gilt nur für verwaltete Instanzen).
* Deaktivieren Sie ggf. die TDE-Verschlüsselung für Datenbanken, die keine sensiblen Daten (z. B. Entwicklungs- oder Testdatenbanken) enthalten. Sicherungen für nicht verschlüsselte Datenbanken werden in der Regel mit einem höheren Komprimierungsverhältnis komprimiert.

> [!IMPORTANT]
> Für analytische Data Mart-\Data Warehouse-Workloads sollten Sie unbedingt [gruppierte Columnstore-Indizes](https://docs.microsoft.com/sql/database-engine/using-clustered-columnstore-indexes) verwenden, die Anzahl der nicht gruppierten Indizes reduzieren und auch Massenladevorgänge mit einer Zeilenzahl von rund einer Million in Erwägung ziehen, um den zusätzlichen Sicherungsspeicherverbrauch zu verringern.


## <a name="storage-costs"></a>Speicherkosten


### <a name="dtu-model"></a>DTU-Modell

Mit dem DTU-Modell fallen keine zusätzlichen Kosten für den Sicherungsspeicher für Datenbanken und Pools für elastische Datenbanken an. 

### <a name="vcore-model"></a>V-Kern-Modell

Für Einzeldatenbanken wird eine Mindestspeichermenge für die Sicherung, die der Gesamtgröße der Datenbank entspricht, kostenlos zur Verfügung gestellt. Bei Pools für elastische Datenbanken und verwalteten Instanzen wird eine Mindestspeichermenge für die Sicherung, die der Gesamtgröße des zugeordneten Datenspeichers für den Pool bzw. der Instanzgröße entspricht, ohne zusätzliche Kosten bereitgestellt. Zusätzlich verbrauchter Sicherungsspeicher wird pro GB und Monat abgerechnet. Dieser zusätzliche Verbrauch hängt von der Workload und der Größe der einzelnen Datenbanken ab.

Azure SQL-Datenbank berechnet den gesamten Sicherungsspeicher in der Aufbewahrung als kumulativen Wert. Jede Stunde wird dieser Wert an die Azure-Abrechnungspipeline gemeldet, die für die Aggregierung dieser stündlichen Nutzung verantwortlich ist, um den Verbrauch am Ende jedes Monats zu berechnen. Nach dem Löschen der Datenbank senken wir den Verbrauch mit zunehmendem Alter der Sicherungen. Sobald ihr Alter den Aufbewahrungszeitraum überschreitet, wird die Abrechnung beendet. Da alle Protokollsicherungen und differenziellen Sicherungen für den gesamten Aufbewahrungszeitraum beibehalten werden, werden für Datenbanken, die stark geändert werden, höhere Sicherungsgebühren berechnet. 

Nehmen wir an, die Datenbank hat 744 GB Sicherungsspeicher angesammelt, und diese Menge bleibt während eines ganzen Monats konstant. Um diesen kumulativen Speicherverbrauch in eine stündliche Nutzung zu konvertieren, dividieren wir ihn durch 744,0 (31 Tage pro Monat * 24 Stunden pro Tag). Dann meldet SQL DB, dass die Datenbank jede Stunde 1 GB an PITR-Sicherungen verbraucht hat. Die Azure-Abrechnung aggregiert dies und zeigt eine Nutzung von 744 GB für den gesamten Monat und die Kosten basierend auf dem $/GB/Monat-Satz in Ihrer Region an. 

Jetzt folgt ein komplexeres Beispiel. Angenommen, der Aufbewahrungszeitraum der Datenbank wird in der Mitte des Monats auf 14 Tage verlängert, und dies führt (hypothetisch) zu einer Verdoppelung des gesamten Sicherungsspeichers auf 1.488 GB. Die SQL-Datenbank meldet dann 1 GB Nutzung für die Stunden 1-372 und dann 2 GB Nutzung für die Stunden 373-744. Dies würde zu einer abschließenden Rechnung von 1.116 GB/Monat aggregiert. 

Sie können die Kostenanalyse von Azure-Abonnements verwenden, um die aktuellen Ausgaben für den Sicherungsspeicher zu ermitteln.

![Analyse der Kosten für Sicherungsspeicher](./media/sql-database-automated-backup/check-backup-storage-cost-sql-mi.png)

Wenn Sie sich z. B. über die Sicherungsspeicherkosten für eine verwaltete Instanz informieren möchten, wechseln Sie in Azure-Portal zu Ihrem Abonnement, und öffnen Sie das Blatt „Kostenanalyse“. Wählen Sie die Unterkategorie **mi pitr backup storage** der Verbrauchseinheit aus, um die aktuellen Sicherungskosten und eine Gebührenvorhersage anzuzeigen. Sie können auch andere Unterkategorien der Verbrauchseinheit einbeziehen, wie etwa **managed instance general purpose - storage** oder **managed instance general purpose - compute gen5**, um die Kosten für Sicherungsspeicher mit anderen Kostenkategorien zu vergleichen.

## <a name="backup-retention"></a>Sicherungsaufbewahrung

Bei allen Azure SQL-Datenbanken (Einzeldatenbanken, Pooldatenbanken und Datenbanken der verwalteten Instanz) gibt es eine standardmäßige Aufbewahrungsdauer für Sicherungen von **7** Tagen. Sie können die [Aufbewahrungsdauer für Sicherungen in bis zu 35 Tage ändern](#change-pitr-backup-retention-period).

Wenn Sie eine Datenbank löschen, bewahrt SQL-Datenbank die Sicherungen auf die gleiche Weise wie für eine Onlinedatenbank auf. Beim Löschen einer Datenbank vom Typ „Basic“, für die eine Aufbewahrungsdauer von sieben Tagen gilt, wird eine vier Tage alte Sicherung weitere drei Tage lang aufbewahrt.

Falls Sie die Sicherungen länger als die maximale Aufbewahrungsdauer beibehalten müssen, können Sie die Sicherungseigenschaften so ändern, dass Ihrer Datenbank eine oder mehrere Zeiträume für die langfristige Aufbewahrung hinzugefügt werden. Weitere Informationen finden Sie unter [Langfristige Aufbewahrung](sql-database-long-term-retention.md).

> [!IMPORTANT]
> Wenn Sie die Azure SQL Server-Instanz löschen, auf der die SQL-Datenbanken gehostet werden, werden alle Pools für elastische Datenbanken und Datenbanken, die zum Server gehören, ebenfalls gelöscht und können nicht wiederhergestellt werden. Es ist nicht möglich, einen gelöschten Server wiederherzustellen. Wenn Sie aber die langfristige Aufbewahrung konfiguriert haben, werden die Sicherungen für die Datenbanken mit LTR nicht gelöscht, und diese Datenbanken können wiederhergestellt werden.

## <a name="encrypted-backups"></a>Verschlüsselte Sicherungen

Wenn Ihre Datenbank mit TDE verschlüsselt ist, werden die Sicherungen im Ruhezustand, einschließlich LTR-Sicherungen, automatisch verschlüsselt. Wenn Transparent Data Encryption (TDE) für eine Azure SQL-Datenbank aktiviert ist, werden die Sicherungen ebenfalls verschlüsselt. In allen neuen Azure SQL-Datenbanken ist TDE standardmäßig aktiviert. Weitere Informationen finden Sie unter [Transparent Data Encryption mit Azure SQL-Datenbank](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql).

## <a name="backup-integrity"></a>Sicherungsintegrität

Fortlaufend testet das Entwicklungsteam von Azure SQL-Datenbank automatisch die Wiederherstellung automatischer Sicherungen von Datenbanken auf logischen Servern und in Pools für elastische Datenbanken (steht in der verwalteten Instanz nicht zur Verfügung). Bei der Point-in-Time-Wiederherstellung werden die Datenbanken außerdem mithilfe von DBCC CHECKDB Integritätsprüfungen unterzogen.

Nach Abschluss der Migration führt die verwaltete Instanz automatisch die erste Sicherung der Datenbanken, die mit dem nativen Befehl `RESTORE` oder dem Data Migration Service wiederhergestellt wurden, mit `CHECKSUM` aus.

Mögliche Probleme, die bei der Integritätsprüfung gefunden werden, führen zu einer Warnung des Entwicklungsteams. Weitere Informationen zur Integrität der Daten in Azure SQL-Datenbank finden Sie unter [Datenintegrität in Azure SQL-Datenbank](https://azure.microsoft.com/blog/data-integrity-in-azure-sql-database/).

## <a name="compliance"></a>Kompatibilität

Wenn Sie Ihre Datenbank von einer DTU-basierten Dienstebene zu einer Dienstebene auf V-Kern-Basis migrieren, wird die PITR-Aufbewahrung beibehalten. So soll sichergestellt werden, dass die Datenwiederherstellungsrichtlinie Ihrer Anwendung nicht kompromittiert wird. Falls die Standardaufbewahrung Ihre Konformitätsanforderungen nicht erfüllt, können Sie die PITR-Aufbewahrungsdauer per PowerShell oder REST-API ändern. Weitere Informationen finden Sie unter [Ändern des Zeitraums für die Aufbewahrung von Sicherungen](#change-pitr-backup-retention-period).

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="change-pitr-backup-retention-period"></a>Ändern des PITR-Aufbewahrungszeitraums von Sicherungen

Sie können den Standardzeitraum für die Aufbewahrung von PITR-Sicherungen mit dem Azure-Portal, PowerShell oder der REST-API ändern. In den folgenden Beispielen wird veranschaulicht, wie Sie die PITR-Aufbewahrungsdauer in 28 Tage ändern.

> [!WARNING]
> Wenn Sie die aktuelle Aufbewahrungsdauer reduzieren, sind alle vorhandenen Sicherungen, die außerhalb der neuen Aufbewahrungsdauer liegen, nicht mehr verfügbar. Wenn Sie die aktuelle Aufbewahrungsdauer erhöhen, behält SQL-Datenbank die vorhandenen Sicherungen bei, bis die längere Aufbewahrungsdauer erreicht ist.

> [!NOTE]
> Diese APIs wirken sich nur auf die PITR-Aufbewahrungsdauer aus. Falls Sie für Ihre Datenbank LTR konfiguriert haben, ergeben sich keine Auswirkungen. Weitere Informationen zum Ändern von LTR-Aufbewahrungsdauern finden Sie unter [Langfristige Aufbewahrung](sql-database-long-term-retention.md).

### <a name="change-pitr-backup-retention-period-using-azure-portal"></a>Ändern der PITR-Aufbewahrungsdauer mithilfe des Azure-Portals

Um die Aufbewahrungsdauer der PITR-Sicherung über das Azure-Portal zu ändern, navigieren Sie zu dem Serverobjekt, dessen Aufbewahrungsdauer Sie innerhalb des Portals ändern möchten, und wählen Sie dann die entsprechende Option basierend auf dem Serverobjekt aus, das Sie ändern.

#### <a name="single-database--elastic-poolstabsingle-database"></a>[Einzeldatenbank und Pools für elastische Datenbanken](#tab/single-database)

Das Ändern der Aufbewahrungsdauer für PITR-Sicherungen für einzelne Azure SQL-Datenbanken erfolgt auf Serverebene. Die auf Serverebene vorgenommene Änderung betrifft die Datenbanken auf dem jeweiligen Server. Um PITR für Azure SQL-Datenbank-Server im Azure-Portal zu ändern, navigieren Sie zum Server-Übersichtsblatt, klicken Sie im Navigationsmenü auf „Sicherungen verwalten“, und klicken Sie dann in der Navigationsleiste auf „Aufbewahrung konfigurieren“.

![Ändern von PITR im Azure-Portal](./media/sql-database-automated-backup/configure-backup-retention-sqldb.png)

#### <a name="managed-instancetabmanaged-instance"></a>[Verwaltete Instanz](#tab/managed-instance)

Die Änderung der Aufbewahrung von PITR-Sicherungen für verwaltete SQL-Datenbank-Instanzen erfolgt auf der Ebene der einzelnen Datenbanken. Um die Aufbewahrung von PITR-Sicherungen für eine Instanzdatenbank im Azure-Portal zu ändern, navigieren Sie zum Übersichtsblatt der einzelnen Datenbank, und klicken Sie dann in der Navigationsleiste auf „Aufbewahrungsdauer für Sicherungen konfigurieren“.

![Ändern von PITR im Azure-Portal](./media/sql-database-automated-backup/configure-backup-retention-sqlmi.png)

---

### <a name="change-pitr-backup-retention-period-using-powershell"></a>Ändern der PITR-Aufbewahrungsdauer mit PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> Das PowerShell Azure Resource Manager-Modul wird von Azure SQL-Datenbank weiterhin unterstützt, aber alle zukünftigen Entwicklungen erfolgen für das Az.Sql-Modul. Informationen zu diesen Cmdlets finden Sie unter [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Die Argumente für die Befehle im Az-Modul und den AzureRm-Modulen sind im Wesentlichen identisch.

```powershell
Set-AzSqlDatabaseBackupShortTermRetentionPolicy -ResourceGroupName resourceGroup -ServerName testserver -DatabaseName testDatabase -RetentionDays 28
```

### <a name="change-pitr-retention-period-using-rest-api"></a>Ändern der PITR-Aufbewahrungsdauer mit der REST-API

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
- Informationen zur Wiederherstellung des Zustands zu einem bestimmten Zeitpunkt über das Azure-Portal finden Sie unter [Wiederherstellen des Zustands einer Azure SQL-Datenbank zu einem früheren Zeitpunkt über das Azure-Portal](sql-database-recovery-using-backups.md).
- Informationen zur Wiederherstellung des Zustands zu einem bestimmten Zeitpunkt mithilfe von PowerShell finden Sie unter [Wiederherstellen des Zustands einer Azure SQL-Datenbank zu einem früheren Zeitpunkt mit PowerShell](scripts/sql-database-restore-database-powershell.md).
- Informationen zum Konfigurieren, Verwalten und Wiederherstellen aus der Langzeitaufbewahrung automatisierter Sicherungen in Azure Blob Storage mit dem Azure-Portal finden Sie im Artikel [Verwalten der Langzeitaufbewahrung von Sicherungen mit dem Azure-Portal](sql-database-long-term-backup-retention-configure.md).
- Informationen zum Konfigurieren, Verwalten und Wiederherstellen aus der langfristigen Aufbewahrung automatisierter Sicherungen im Azure-Blobspeicher mit Azure PowerShell finden Sie unter [Verwalten der langfristigen Sicherungsaufbewahrung mit PowerShell](sql-database-long-term-backup-retention-configure.md).
