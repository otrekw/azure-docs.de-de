---
title: Automatische, georedundante Sicherungen
titleSuffix: Azure SQL Database & Azure SQL Managed Instance
description: Azure SQL-Datenbank und Azure SQL Managed Instance erstellen alle paar Minuten automatisch eine lokale Datenbanksicherung und verwenden georedundanten Azure-Speicher mit Lesezugriff (Read-Access Geo-Redundant Storage, RA-GRS), um für Georedundanz zu sorgen.
services: sql-database
ms.service: sql-database
ms.subservice: backup-restore
ms.custom: sqldbrb=2
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab, danil
manager: craigg
ms.date: 12/13/2019
ms.openlocfilehash: a99045359d9d6c6a327ef646ba15457c3e4fbbda
ms.sourcegitcommit: d118ad4fb2b66c759b70d4d8a18e6368760da3ad
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/02/2020
ms.locfileid: "84299807"
---
# <a name="automated-backups---azure-sql-database--sql-managed-instance"></a>Automatisierte Sicherungen – Azure SQL-Datenbank und SQL Managed Instance

[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

Sowohl Azure SQL-Datenbank als auch Azure SQL Managed Instance erstellen Datenbanksicherungen, die für die Dauer des konfigurierten Aufbewahrungszeitraums aufbewahrt werden. Sie verwenden dabei [georedundanten Azure-Speicher mit Lesezugriff (Read-Access Geo-Redundant Storage, RA-GRS)](../../storage/common/storage-redundancy.md), um sicherzustellen, dass die Sicherungen auch dann erhalten bleiben, wenn das Rechenzentrum nicht erreichbar ist.

Datenbanksicherungen sind ein wesentlicher Bestandteil jeder Strategie für Geschäftskontinuität und Notfallwiederherstellung, da Ihre Daten vor versehentlichen Beschädigungen und Löschungen geschützt werden. Wenn es gemäß Ihren Sicherheitsregeln erforderlich ist, dass Ihre Sicherungen über einen längeren Zeitraum verfügbar sind (bis zu 10 Jahre), können Sie sowohl für Einzel- als auch für Pooldatenbanken [Langzeitaufbewahrung](long-term-retention-overview.md) konfigurieren.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="what-is-a-sql-database-backup"></a>Was ist die Sicherung einer SQL-Datenbank-Instanz?

Sowohl SQL-Datenbank als auch SQL Managed Instance nutzen SQL Server-Technologie, um wöchentlich [vollständige Sicherungen](https://docs.microsoft.com/sql/relational-databases/backup-restore/full-database-backups-sql-server), alle 12 Stunden [differenzielle Sicherungen](https://docs.microsoft.com/sql/relational-databases/backup-restore/differential-backups-sql-server) und alle 5 bis 10 Minuten [Transaktionsprotokollsicherungen](https://docs.microsoft.com/sql/relational-databases/backup-restore/transaction-log-backups-sql-server) zu erstellen. Die Sicherungen werden in [RA-GRS-Speicherblobs](../../storage/common/storage-redundancy.md) gespeichert, die in einem [gekoppelten Rechenzentrum](../../best-practices-availability-paired-regions.md) repliziert werden, um Schutz vor Rechenzentrumsausfällen zu bieten. Wenn Sie eine Datenbank wiederherstellen, bestimmt der Dienst, welche vollständigen und differenziellen Sicherungen bzw. Transaktionsprotokollsicherungen wiederhergestellt werden müssen.

Sie können diese Sicherungen für Folgendes verwenden:

- **Stellen Sie für eine vorhandene Datenbank den Stand zu einem vergangenen Zeitpunkt wieder her**, der innerhalb des Aufbewahrungszeitraums liegt, indem Sie das Azure-Portal, Azure PowerShell, die Azure CLI oder die REST-API verwenden. Bei Einzel- und Pooldatenbanken wird bei diesem Vorgang auf demselben Server und im selben Abonnement wie die ursprüngliche Datenbank eine neue Datenbank erstellt. Bei einer verwalteten Instanz kann bei diesem Vorgang eine Kopie der Datenbank oder die gleiche oder eine andere verwaltete Instanz im selben Abonnement erstellt werden.
- **Stellen Sie für eine gelöschte Datenbank den Stand zum Zeitpunkt des Löschvorgangs wieder her** oder den Stand zu einem beliebigen anderen Zeitpunkt innerhalb des Aufbewahrungszeitraums. Die gelöschte Datenbank kann nur auf dem Server bzw. in der verwalteten Instanz und in dem Abonnement wiederhergestellt werden, auf dem bzw. in der und in dem die ursprüngliche Datenbank erstellt wurde.
- **Stellen Sie eine Datenbank in einer anderen geografischen Region wieder her**. Die Geowiederherstellung ermöglicht die Wiederherstellung nach dem Ausfall einer geografischen Region, wenn Sie keinen Zugriff auf Ihren Server und Ihre Datenbank haben. Dabei wird eine neue Datenbank auf einem beliebigen Server an einem beliebigen Ort der Welt erstellt.
- **Führen Sie die Wiederherstellung einer Datenbank aus einer bestimmten langfristigen Sicherung einer Einzel- oder Pooldatenbank durch**, wenn die Datenbank mit einer Richtlinie zur Langzeitaufbewahrung (Long-Term Retention, LTR) konfiguriert wurde. Mit LTR können Sie eine alte Version der Datenbank wiederherstellen, indem Sie [das Azure-Portal](long-term-backup-retention-configure.md#using-azure-portal) oder [Azure PowerShell](long-term-backup-retention-configure.md#using-powershell) verwenden, um eine Konformitätsanforderung zu erfüllen oder eine alte Version der Anwendung auszuführen. Weitere Informationen finden Sie unter [Langfristige Aufbewahrung](long-term-retention-overview.md).

Informationen zum Durchführen einer Wiederherstellung finden Sie unter [Wiederherstellen einer Azure SQL-Datenbank mit automatisierten Datenbanksicherungen](recovery-using-backups.md).

> [!NOTE]
> In Azure Storage bezieht sich der Begriff *Replikation* auf das Kopieren von Dateien von einem Speicherort an einen anderen. In Azure SQL-Datenbank und SQL Managed Instance bedeutet *Datenbankreplikation*, dass mehrere sekundäre Datenbanken mit einer primären Datenbank synchron gehalten werden.

Sie können einige dieser Vorgänge ausprobieren, indem Sie die folgenden Beispiele verwenden:

| | Das Azure-Portal | Azure PowerShell |
|---|---|---|
| Ändern der Sicherungsaufbewahrung | [Einzeldatenbank](automated-backups-overview.md?tabs=managed-instance#change-the-pitr-backup-retention-period-by-using-the-azure-portal) <br/> [Verwaltete Instanz](automated-backups-overview.md?tabs=managed-instance#change-the-pitr-backup-retention-period-by-using-the-azure-portal) | [Einzeldatenbank](automated-backups-overview.md#change-the-pitr-backup-retention-period-by-using-powershell) <br/>[Verwaltete Instanz](https://docs.microsoft.com/powershell/module/az.sql/set-azsqlinstancedatabasebackupshorttermretentionpolicy) |
| Ändern der Langzeitaufbewahrung von Sicherungen | [Einzeldatenbank](long-term-backup-retention-configure.md#configure-long-term-retention-policies)<br/>Verwaltete Instanz: nicht verfügbar  | [Einzeldatenbank](long-term-backup-retention-configure.md)<br/>Verwaltete Instanz: nicht verfügbar  |
| Wiederherstellen einer Datenbank bis zu einem Zeitpunkt | [Einzeldatenbank](recovery-using-backups.md#point-in-time-restore) | [Einzeldatenbank](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqldatabase) <br/> [Verwaltete Instanz](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqlinstancedatabase) |
| Wiederherstellung einer gelöschten Datenbank | [Einzeldatenbank](recovery-using-backups.md) | [Einzeldatenbank](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldeleteddatabasebackup) <br/> [Verwaltete Instanz](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldeletedinstancedatabasebackup)|
| Wiederherstellen der Datenbank aus Azure Blob Storage | Einzeldatenbank: Nicht verfügbar <br/>Verwaltete Instanz: nicht verfügbar  | Einzeldatenbank: Nicht verfügbar <br/>[Verwaltete Instanz](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-get-started-restore) |

## <a name="backup-frequency"></a>Sicherungshäufigkeit

### <a name="point-in-time-restore"></a>Wiederherstellung bis zu einem bestimmten Zeitpunkt

SQL-Datenbank und SQL Managed Instance unterstützen Self-Service für die Point-in-Time-Wiederherstellung (Point-in-Time Restore, PITR) durch automatisches Erstellen von vollständigen Sicherungen, differenziellen Sicherungen und Transaktionsprotokollsicherungen. Vollständige Datenbanksicherungen werden wöchentlich, differenzielle Datenbanksicherungen im Allgemeinen alle 12 Stunden erstellt. Transaktionsprotokollsicherungen werden meist alle 5 bis 10 Minuten erstellt. Die Häufigkeit von Transaktionsprotokollsicherungen basiert auf der Computegröße und dem Umfang der Datenbankaktivität.

Die erste vollständige Sicherung wird unmittelbar nach der Datenbankerstellung geplant. Diese Sicherung wird normalerweise innerhalb von 30 Minuten abgeschlossen, kann aber länger dauern, wenn die Datenbank groß ist. Die erste Sicherung kann bei einer wiederhergestellten Datenbank oder einer Datenbankkopie beispielsweise länger dauern. Nach der ersten vollständigen Sicherung werden alle weiteren Sicherungen automatisch geplant und im Hintergrund verwaltet. Der genaue Zeitpunkt für alle Datenbanksicherungen wird von SQL-Datenbank oder SQL Managed Instance festgelegt, da dort die gesamte Systemarbeitsauslastung verwaltet wird. Sie können die Sicherungsaufträge nicht ändern oder deaktivieren.

### <a name="default-backup-retention-period"></a>Standardaufbewahrungsdauer für Sicherungen

PITR-Sicherungen sind durch georedundanten Speicher geschützt. Weitere Informationen finden Sie unter [Azure Storage-Redundanz](../../storage/common/storage-redundancy.md).

Weitere Informationen zur PITR finden Sie unter [Point-in-Time-Wiederherstellung](recovery-using-backups.md#point-in-time-restore).

### <a name="long-term-retention"></a>Langfristige Aufbewahrung

Für Einzel- und Pooldatenbanken können Sie die Langzeitaufbewahrung (LTR) vollständiger Sicherungen für eine Dauer von bis zu 10 Jahren in Azure Blob Storage konfigurieren. Wenn Sie die LTR-Richtlinie aktivieren, werden die wöchentlichen vollständigen Sicherungen automatisch in einen anderen RA-GRS-Speichercontainer kopiert. Zur Einhaltung verschiedener Konformitätsanforderungen können Sie verschiedene Aufbewahrungszeiträume für wöchentliche, monatliche oder jährliche Sicherungen auswählen. Der Speicherbedarf hängt von der ausgewählten Häufigkeit der Sicherungen und Aufbewahrungsdauer ab. Sie können den [LTR-Preisrechner](https://azure.microsoft.com/pricing/calculator/?service=sql-database) verwenden, um die Kosten für den LTR-Speicher zu schätzen.

Wie bei PITR-Sicherungen sind LTR-Sicherungen durch georedundanten Speicher geschützt. Weitere Informationen finden Sie unter [Azure Storage-Redundanz](../../storage/common/storage-redundancy.md).

Weitere Informationen zu LTR finden Sie unter [Langfristiges Aufbewahren von Sicherungen](long-term-retention-overview.md).

## <a name="backup-storage-consumption"></a>Sicherungsspeicherverbrauch

Bei Einzeldatenbanken und verwalteten Instanzen wird der insgesamt genutzte Sicherungsspeicher wie folgt berechnet:

`Total backup storage size = (size of full backups + size of differential backups + size of log backups) – database size`

Bei Pooldatenbanken wird die Gesamtgröße des Sicherungsspeichers auf Poolebene aggregiert und wie folgt berechnet:

`Total backup storage size = (total size of all full backups + total size of all differential backups + total size of all log backups) - allocated pool data storage`

Sicherungen, die vor dem Aufbewahrungszeitraum erfolgt sind, werden basierend auf ihrem Zeitstempel automatisch bereinigt. Da differenzielle Sicherungen und Protokollsicherungen erst von Nutzen sind, wenn zuvor eine vollständige Sicherung erfolgt ist, werden sie zusammen in wöchentlichen Blöcken bereinigt.

SQL-Datenbank und SQL Managed Instance berechnen den gesamten Sicherungsspeicher für die Aufbewahrung als kumulativen Wert. Jede Stunde wird dieser Wert an die Azure-Abrechnungspipeline gemeldet, die für die Aggregierung dieser stündlichen Nutzung verantwortlich ist, um die Nutzung am Ende jedes Monats zu berechnen. Nach dem Löschen der Datenbank sinkt der Verbrauch mit zunehmendem Alter der Sicherungen. Sobald das Alter der Sicherungen den Aufbewahrungszeitraum überschreitet, wird die Abrechnung eingestellt.
   
   > [!IMPORTANT]
   > Sicherungen einer Datenbank werden für die angegebene Beibehaltungsdauer beibehalten, auch wenn die Datenbank gelöscht wurde. Mit häufigem Löschen und Neuerstellen einer Datenbank können zwar Speicher- und Computekosten eingespart werden, aber die Sicherungsspeicherkosten können steigen, da Microsoft für jede gelöschte Datenbank bei jeder Löschung eine Sicherung für die angegebene Beibehaltungsdauer beibehält. 

### <a name="monitor-consumption"></a>Überwachen des Verbrauchs

Jeder Sicherungstyp (vollständig, differenziell und Protokoll) wird auf dem Blatt für die Datenbanküberwachung als separate Metrik ausgewiesen. Im folgenden Diagramm wird gezeigt, wie Sie den Sicherungsspeicherverbrauch für eine Einzeldatenbank überwachen. Dieses Feature ist derzeit für verwaltete Instanzen nicht verfügbar.

![Überwachen des Sicherungsspeicherverbrauchs von Datenbanken im Azure-Portal](./media/automated-backups-overview/backup-metrics.png)

### <a name="fine-tune-backup-storage-consumption"></a>Optimieren des Sicherungsspeicherverbrauchs

Der zusätzliche Sicherungsspeicherverbrauch hängt von der Workload und der Größe der einzelnen Datenbanken ab. Ziehen Sie einige der folgenden Optimierungstechniken in Betracht, um den Sicherungsspeicherverbrauch zu reduzieren:

- Reduzieren Sie den [Aufbewahrungszeitraum für Sicherungen](#change-the-pitr-backup-retention-period-by-using-the-azure-portal) auf die Mindestanforderungen für Ihre Zwecke.
- Vermeiden Sie es, große Schreibvorgänge, wie z.B. die Neuerstellung von Indizes, öfter als nötig durchzuführen.
- Bei umfangreichen Datenladevorgängen sollten Sie [gruppierte Columnstore-Indizes](https://docs.microsoft.com/sql/database-engine/using-clustered-columnstore-indexes) verwenden, die Anzahl der nicht gruppierten Indizes reduzieren und auch Massenladevorgänge mit einer Zeilenanzahl von rund 1 Mio. in Erwägung ziehen.
- Auf der Dienstebene „Universell“ ist der bereitgestellte Datenspeicher günstiger als die Kosten für den zusätzlichen Sicherungsspeicher. Wenn ständig hohe Kosten durch zusätzlichen Sicherungsspeicher anfallen, können Sie eine Vergrößerung des Datenspeichers in Betracht ziehen, um den Sicherungsspeicher einzusparen.
- Verwenden Sie TempDB anstelle permanenter Tabellen in Ihrer ETL-Logik zum Speichern temporärer Ergebnisse. (Gilt nur für SQL Managed Instance.)
- Deaktivieren Sie ggf. die TDE-Verschlüsselung für Datenbanken, die keine sensiblen Daten enthalten (z. B. Entwicklungs- oder Testdatenbanken). Sicherungen für nicht verschlüsselte Datenbanken werden in der Regel mit einem höheren Komprimierungsverhältnis komprimiert.

> [!IMPORTANT]
> Für analytische Data Mart-\Data Warehouse-Workloads sollten Sie unbedingt [gruppierte Columnstore-Indizes](https://docs.microsoft.com/sql/database-engine/using-clustered-columnstore-indexes) verwenden, die Anzahl der nicht gruppierten Indizes reduzieren und Massenladevorgänge mit einer Zeilenanzahl von rund 1 Mio. in Erwägung ziehen, um den zusätzlichen Verbrauch von Sicherungsspeicher zu verringern.

## <a name="storage-costs"></a>Speicherkosten

Der Preis für den Speicher variiert abhängig davon, ob Sie das Modell „DTU“ oder „Virtueller Kern“ verwenden.

### <a name="dtu-model"></a>DTU-Modell

Beim DTU-Modell fallen keine zusätzlichen Kosten für den Sicherungsspeicher für Datenbanken und Pools für elastische Datenbanken an.

### <a name="vcore-model"></a>V-Kern-Modell

Bei Einzeldatenbanken in SQL-Datenbank wird eine Mindestmenge an Sicherungsspeicher, die der Größe der Datenbank entspricht, ohne zusätzliche Kosten zur Verfügung gestellt. Bei Pools für elastische Datenbanken in SQL-Datenbank sowie Einzelinstanzen und Instanzpools in SQL Managed Instance wird eine Mindestmenge an Sicherungsspeicher, die der Größe des dem Pool zugeordneten Datenspeichers bzw. der Instanzgröße entspricht, ohne zusätzliche Kosten bereitgestellt. Zusätzlich verbrauchter Sicherungsspeicher wird pro GB und Monat abgerechnet. Dieser zusätzliche Verbrauch hängt von der Workload und der Größe der einzelnen Datenbanken ab.

SQL-Datenbank und SQL Managed Instance berechnen den gesamten Sicherungsspeicher für die Aufbewahrung als kumulativen Wert. Jede Stunde wird dieser Wert der Azure-Abrechnungspipeline gemeldet, die für die Aggregierung dieser stündlichen Nutzung verantwortlich ist, um den Verbrauch am Ende jedes Monats zu berechnen. Nach dem Löschen der Datenbank senkt Microsoft den Verbrauch mit zunehmendem Alter der Sicherungen. Sobald das Alter der Sicherungen den Aufbewahrungszeitraum überschreitet, wird die Abrechnung eingestellt. Da alle Protokoll- und differenziellen Sicherungen für den gesamten Aufbewahrungszeitraum beibehalten werden, fallen für häufig geänderte Datenbanken höhere Sicherungskosten an.

Angenommen, die Datenbank hat 744 GB Sicherungsspeicher angesammelt, und diese Menge bleibt während eines ganzen Monats konstant. Um diesen kumulativen Speicherverbrauch in eine stündliche Nutzung umzurechnen, dividieren wir ihn durch 744,0 (31 Tage pro Monat x 24 Stunden pro Tag). So meldet SQL-Datenbank, dass die Datenbank pro Stunde 1 GB an PITR-Sicherungen verbraucht hat. Die Azure-Abrechnung aggregiert dies und zeigt eine Nutzung von 744 GB für den gesamten Monat. Die Kosten basierend auf dem EUR/GB/Monat-Satz in Ihrer Region an. Die Kosten basierend auf den monatlichen Kosten pro GB in Ihrer Region.

Jetzt folgt ein komplexeres Beispiel. Angenommen, der Aufbewahrungszeitraum der Datenbank wird Mitte des Monats auf 14 Tage verlängert. Nehmen wir an, dass diese Zunahme (hypothetisch) dazu führt, dass sich der gesamte Sicherungsspeicher auf 1.488 GB verdoppelt. SQL-Datenbank meldet eine Nutzung von 1 GB für die Stunden 1 bis 372. Die Nutzung wird als 2 GB für die Stunden 373 bis 744 gemeldet. Die monatliche Schlussrechnung basiert dann auf dem aggregierten Wert von 1.116 GB.

### <a name="monitor-costs"></a>Überwachen der Kosten

Um die Kosten für Sicherungsspeicher zu verstehen, wechseln Sie im Azure-Portal zu **Kostenverwaltung + Abrechnung**. Wählen Sie **Kostenverwaltung** und dann **Kostenanalyse** aus. Wählen Sie das gewünschte Abonnement als **Bereich** aus, und filtern Sie dann nach dem gewünschten Zeitraum und Dienst.

Fügen Sie einen Filter für **Dienstname** hinzu, und wählen Sie dann in der Dropdownliste **SQL-Datenbank** aus. Verwenden Sie den Filter **Unterkategorie der Verbrauchseinheit**, um den Abrechnungszähler für Ihren Dienst auszuwählen. Wählen Sie für eine einzelne Datenbank oder einen Pool für elastische Datenbanken den **PITR-Sicherungsspeicher für eine einzelne Datenbank/einen Pool für elastische Datenbanken** aus. Wählen Sie für eine verwaltete Instanz **MI-PITR-Sicherungsspeicher** aus. Die Unterkategorien **Speicher** und **Compute** können für Sie auch von Interesse sein, obwohl sie nicht im Zusammenhang mit den Sicherungsspeicherkosten stehen.

![Analyse der Kosten für Sicherungsspeicher](./media/automated-backups-overview/check-backup-storage-cost-sql-mi.png)

## <a name="backup-retention"></a>Sicherungsaufbewahrung

Bei allen Datenbanken in SQL-Datenbank und SQL Managed Instance gilt für Sicherungen ein Standardaufbewahrungszeitraum von 7 Tagen. Sie können die [Aufbewahrungsdauer für Sicherungen in bis zu 35 Tage ändern](#change-the-pitr-backup-retention-period).

Wenn Sie eine Datenbank löschen, bewahrt Azure die Sicherungen auf die gleiche Weise auf wie bei Onlinedatenbanken. Beim Löschen einer Datenbank des Typs „Basic“, für die eine Aufbewahrungsdauer von sieben Tagen gilt, wird eine vier Tage alte Sicherung weitere drei Tage aufbewahrt.

Falls Sie die Sicherungen länger als die maximale Aufbewahrungsdauer beibehalten müssen, können Sie die Sicherungseigenschaften so ändern, dass Ihrer Datenbank eine oder mehrere Zeiträume für die langfristige Aufbewahrung hinzugefügt werden. Weitere Informationen finden Sie unter [Langfristige Aufbewahrung](long-term-retention-overview.md).

> [!IMPORTANT]
> Das Festlegen des Aufbewahrungszeitraums für Sicherungen auf einen Tag (oder auf einen beliebigen Wert zwischen 1 und 7) wird derzeit nur über PowerShell oder die REST-API unterstützt. Die mindestens erforderliche Version des Moduls Az.SQL ist v2.6.0. Alternativ kann das Modul über Cloud Shell ausgeführt werden, wo immer die jeweils aktuelle Version von Az.SQL verwendet wird.

> [!IMPORTANT]
> Wenn Sie den Server oder die verwaltete Instanz löschen, werden auch alle Datenbanken gelöscht, die von diesem Server oder dieser verwalteten Instanz verwaltet werden. Sie können nicht wiederhergestellt werden. Ein gelöschter Server oder eine gelöschte verwaltete Instanz kann nicht wiederhergestellt werden. Wenn Sie allerdings Langzeitaufbewahrung (LTR) für SQL-Datenbank oder SQL Managed Instance konfiguriert haben, werden die Sicherungen für die Datenbanken mit LTR nicht gelöscht, und diese Datenbanken können wiederhergestellt werden.

## <a name="encrypted-backups"></a>Verschlüsselte Sicherungen

Wenn Ihre Datenbank mit TDE verschlüsselt ist, werden Sicherungen im Ruhezustand, einschließlich LTR-Sicherungen, automatisch verschlüsselt. Wenn Transparent Data Encryption (TDE) für SQL-Datenbank oder SQL Managed Instance aktiviert ist, werden Sicherungen ebenfalls verschlüsselt. Bei allen neuen Datenbanken in SQL-Datenbank und SQL Managed Instance ist TDE standardmäßig aktiviert. Weitere Informationen finden Sie unter [Transparent Data Encryption für SQL-Datenbank, SQL Managed Instance und Azure Synapse Analytics](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql).

## <a name="backup-integrity"></a>Sicherungsintegrität

Das Azure SQL-Entwicklungsteam testet fortlaufend automatisch die Wiederherstellung von automatischen Datenbanksicherungen in SQL-Datenbank. (Für SQL Managed Instance sind diese Tests nicht verfügbar.) Bei der Point-in-Time-Wiederherstellung werden die Datenbanken außerdem mithilfe von DBCC CHECKDB Integritätsprüfungen unterzogen.

Nach Abschluss der Migration führt SQL Managed Instance mit `CHECKSUM` eine automatische Erstsicherung von Datenbanken durch, die über den nativen Befehl `RESTORE` oder Azure Data Migration Service wiederhergestellt wurden.

Mögliche Probleme, die bei der Integritätsprüfung gefunden werden, führen zu einer Warnung des Entwicklungsteams. Weitere Informationen finden Sie unter [Datenintegrität in Azure SQL-Datenbank](https://azure.microsoft.com/blog/data-integrity-in-azure-sql-database/).

## <a name="compliance"></a>Kompatibilität

Wenn Sie Ihre Datenbank von einer DTU-basierten Dienstebene zu einer Dienstebene auf Basis virtueller Kerne migrieren, wird die PITR-Aufbewahrung beibehalten. So soll sichergestellt werden, dass die Datenwiederherstellungsrichtlinie Ihrer Anwendung nicht kompromittiert wird. Falls die Standardaufbewahrung Ihre Konformitätsanforderungen nicht erfüllt, können Sie die PITR-Aufbewahrungsdauer per PowerShell oder REST-API ändern. Weitere Informationen finden Sie unter [Ändern des PITR-Aufbewahrungszeitraums von Sicherungen](#change-the-pitr-backup-retention-period).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="change-the-pitr-backup-retention-period"></a>Ändern des PITR-Aufbewahrungszeitraums von Sicherungen

Sie können den Standardzeitraum für die Aufbewahrung von PITR-Sicherungen im Azure-Portal, mit PowerShell oder der REST-API ändern. In den folgenden Beispielen wird veranschaulicht, wie Sie die PITR-Aufbewahrungsdauer in 28 Tage ändern.

> [!WARNING]
> Wenn Sie die aktuelle Aufbewahrungsdauer reduzieren, sind alle vorhandenen Sicherungen, die älter als die neue Aufbewahrungsdauer sind, nicht mehr verfügbar. Wenn Sie den aktuellen Aufbewahrungszeitraum verlängern, bewahrt Azure die vorhandenen Sicherungen bis zum Ende des längeren Aufbewahrungszeitraums auf.

> [!NOTE]
> Diese APIs wirken sich nur auf die PITR-Aufbewahrungsdauer aus. Falls Sie für Ihre Datenbank LTR konfiguriert haben, ist sie nicht betroffen. Informationen zum Ändern von LTR-Aufbewahrungsdauern finden Sie unter [Langfristige Aufbewahrung](long-term-retention-overview.md).

### <a name="change-the-pitr-backup-retention-period-by-using-the-azure-portal"></a>Ändern der PITR-Aufbewahrungsdauer im Azure-Portal

Um die Aufbewahrungsdauer der PITR-Sicherung im Azure-Portal zu ändern, navigieren Sie im Portal zum Serverobjekt, dessen Aufbewahrungsdauer Sie ändern möchten. Wählen Sie dann die entsprechende Option basierend auf dem Serverobjekt aus, das Sie ändern.

#### <a name="sql-database"></a>[SQL-Datenbank](#tab/single-database)

Bei SQL-Datenbank wird die Aufbewahrung für Sicherungen für Zeitpunktwiederherstellung auf Serverebene geändert. Die auf Serverebene vorgenommene Änderung betrifft die Datenbanken auf dem jeweiligen Server. Wenn Sie die Aufbewahrung in Bezug auf Zeitpunktwiederherstellung für einen Server im Azure-Portal ändern möchten, navigieren Sie zum Blatt mit der Übersicht für diesen Server. Wählen Sie im linken Bereich **Sicherungen verwalten** und dann oben auf dem Bildschirm **Aufbewahrung konfigurieren** aus:

![Ändern der PITR-Aufbewahrung, Serverebene](./media/automated-backups-overview/configure-backup-retention-sqldb.png)

#### <a name="sql-managed-instance"></a>[SQL Managed Instance](#tab/managed-instance)

Bei SQL Managed Instance wird die Aufbewahrung für Sicherungen für Zeitpunktwiederherstellung auf Ebene der einzelnen Datenbanken geändert. Um die Aufbewahrung von PITR-Sicherungen für eine Instanzdatenbank im Azure-Portal zu ändern, navigieren Sie zum Übersichtsblatt der jeweiligen Datenbank. Wählen Sie dann oben auf dem Bildschirm **Aufbewahrungsdauer für Sicherungen konfigurieren** aus.

![Ändern der PITR-Aufbewahrung, verwaltete Instanz](./media/automated-backups-overview/configure-backup-retention-sqlmi.png)

---

### <a name="change-the-pitr-backup-retention-period-by-using-powershell"></a>Ändern der PITR-Aufbewahrungsdauer mit PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]
> [!IMPORTANT]
> Das PowerShell-Modul AzureRM wird weiterhin von SQL-Datenbank und SQL Managed Instance unterstützt, in Zukunft wird jedoch nur das Modul Az.Sql weiterentwickelt. Weitere Informationen finden Sie unter [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Die Argumente für die Befehle im Az-Modul und den AzureRm-Modulen sind im Wesentlichen identisch.

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

- Datenbanksicherungen sind ein wesentlicher Bestandteil jeder Strategie für Geschäftskontinuität und Notfallwiederherstellung, da Ihre Daten vor versehentlichen Beschädigungen und Löschungen geschützt werden. Informationen zu den anderen Geschäftskontinuitätslösungen von SQL-Datenbank finden Sie unter [Übersicht über Geschäftskontinuität mit Azure SQL-Datenbank](business-continuity-high-availability-disaster-recover-hadr-overview.md).
- Erfahren Sie mehr zum [Wiederherstellen einer Datenbank bis zu einem bestimmten Zeitpunkt mithilfe des Azure-Portals](recovery-using-backups.md).
- Erfahren Sie mehr zum [Wiederherstellen einer Datenbank bis zu einem bestimmten Zeitpunkt mit PowerShell](scripts/restore-database-powershell.md).
- Informationen zum Konfigurieren, Verwalten und Wiederherstellen aus der Langzeitaufbewahrung automatisierter Sicherungen in Azure Blob Storage im Azure-Portal finden Sie im Artikel [Verwalten der langfristigen Aufbewahrung von Sicherungen im Azure-Portal](long-term-backup-retention-configure.md).
- Informationen zum Konfigurieren, Verwalten und Wiederherstellen aus der langfristigen Aufbewahrung automatisierter Sicherungen in Azure Blob Storage mit PowerShell finden Sie unter [Verwalten der langfristigen Aufbewahrung von Sicherungen mit PowerShell](long-term-backup-retention-configure.md).
