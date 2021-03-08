---
title: Migrieren von Datenbanken zu SQL Managed Instance mithilfe des Protokollwiedergabediensts
description: Informationen zum Migrieren von Datenbanken aus SQL Server zu SQL Managed Instance mithilfe des Protokollwiedergabediensts
services: sql-database
ms.service: sql-managed-instance
ms.custom: seo-lt-2019, sqldbrb=1
ms.devlang: ''
ms.topic: how-to
author: danimir
ms.reviewer: sstein
ms.date: 03/01/2021
ms.openlocfilehash: bc0dc72c7547c8f74aec53b7153fc5384c6b634b
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101690786"
---
# <a name="migrate-databases-from-sql-server-to-sql-managed-instance-using-log-replay-service-preview"></a>Informationen zum Migrieren von Datenbanken aus SQL Server zu SQL Managed Instance mithilfe des Protokollwiedergabediensts (Vorschau)
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

In diesem Artikel wird erläutert, wie Sie die Datenbankmigration von SQL Server 2008-2019 zu SQL Managed Instance mithilfe des Protokollwiedergabediensts (Log Replay Service, LRS) konfigurieren, der sich derzeit in der öffentlichen Vorschau befindet. Dabei handelt es sich um einen für Managed Instance aktivierten Clouddienst, der auf der SQL Server-Protokollversandtechnologie basiert. LRS sollte verwendet werden, wenn komplexe benutzerdefinierte Migrationens- und Hybridarchitekturen vorhanden sind, wenn eine höhere Kontrolle erforderlich ist, wenn die Toleranz für Ausfallzeiten gering ist oder Azure Data Migration Service (DMS) nicht verwendet werden kann.

Sowohl DMS als auch LRS verwenden dieselbe zugrunde liegende Migrationstechnologie und dieselben APIs. Mit der Veröffentlichung von LRS vereinfachen wir komplexe benutzerdefinierte Migrationen und Hybridarchitekturen zwischen lokalen SQL Server-Instanzen und SQL Managed Instance.

## <a name="when-to-use-log-replay-service"></a>Wann sollte der Protokollwiedergabedienst verwendet werden?

In Fällen, in denen [Azure DMS](/azure/dms/tutorial-sql-server-to-managed-instance.md) nicht für die Migration verwendet werden kann, kann der LRS-Clouddienst direkt mit PowerShell, CLI-Cmdlets oder APIs zum manuellen Erstellen und Orchestrieren von Datenbankmigrationen zu SQL Managed Instance verwendet werden. 

In einigen der folgenden Fälle sollten Sie den LRS-Clouddienst verwenden:
- Das Datenbankmigrationsprojekt erfordert mehr Kontrolle.
- Die Toleranz für Ausfallzeiten bei der Migrationsübernahme ist gering.
- Die ausführbare DMS-Datei kann nicht in Ihrer Umgebung installiert werden.
- Die ausführbare DMS-Datei hat keinen Dateizugriff auf Datenbanksicherungen.
- Es ist kein Zugriff auf das Hostbetriebssystem möglich, oder es sind keine Administratorrechte vorhanden.
- Netzwerkports für Verbindungen von Ihrer Umgebung zu Azure können nicht geöffnet werden.
- Sicherungen werden mithilfe der TO URL-Option direkt in Azure Blob Storage gespeichert.
- Es müssen differenzielle Sicherungen verwendet werden.

> [!NOTE]
> Als automatisierte Methode zum Migrieren von Datenbanken von SQL Server zu SQL Managed Instance wird die Verwendung von Azure DMS empfohlen. Dieser Dienst verwendet im Back-End denselben LRS-Clouddienst mit Protokollversand im NORECOVERY-Modus. Sie sollten die manuelle Verwendung von LRS zum Orchestrieren von Migrationen in Fällen in Erwägung ziehen, in denen Azure-DMS Ihre Szenarios nicht vollständig unterstützt.

## <a name="how-does-it-work"></a>Funktionsweise

Zum Erstellen einer benutzerdefinierten Lösung zum Migrieren von Datenbanken zur Cloud mithilfe von LRS sind einige Orchestrierungsschritte erforderlich, die im Diagramm gezeigt und in der folgenden Tabelle skizziert werden.

Die Migration umfasst vollständige Datenbanksicherungen in SQL Server, wobei CHECKSUM aktiviert ist, und das Kopieren von Sicherungsdateien in Azure Blob Storage. LRS wird zum Wiederherstellen von Sicherungsdateien aus Azure Blob Storage in SQL Managed Instance verwendet. Azure Blob Storage wird als zwischengeschalteter Speicher zwischen SQL Server und SQL Managed Instance verwendet.

LRS überwacht Azure Blob Storage auf neue differenzielle oder Protokollsicherungen, die nach der Wiederherstellung der vollständigen Sicherung hinzugefügt wurden, und stellt automatisch neu hinzugefügte Dateien wieder her. Der Fortschritt der Sicherungsdateien, die in SQL Managed Instance wiederhergestellt werden, kann mithilfe des Diensts überwacht werden, und der Prozess kann bei Bedarf auch abgebrochen werden.

LRS erfordert keine bestimmte Benennungskonvention für Sicherungsdateien, da alle in Azure Blob Storage platzierten Dateien überprüft werden und die Sicherungskette nur aus dem Lesen der Dateiheader erstellt wird. Datenbanken befinden sich während des Migrationsvorgangs im Zustand „Wiederherstellung“, da sie im [NORECOVERY](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-transact-sql#comparison-of-recovery-and-norecovery)-Modus wiederhergestellt werden und nicht zum Lesen oder Schreiben verwendet werden können, bis der Migrationsvorgang vollständig abgeschlossen ist. 

Beim Migrieren mehrerer Datenbanken müssen Sicherungen für jede Datenbank in Azure Blob Storage in einem separaten Ordner abgelegt werden. LRS muss für jede Datenbank separat gestartet werden, und es müssen verschiedene Pfade für separate Azure Blob Storage-Ordner angegeben werden. 

LRS kann im AutoVervollständigen- oder kontinuierlichen Modus gestartet werden. Beim Starten im AutoVervollständigen-Modus wird die Migration automatisch abgeschlossen, wenn die zuletzt angegebene Sicherungsdatei wiederhergestellt wurde. Wenn der Dienst im kontinuierlichen Modus gestartet wird, werden alle neu hinzugefügten Sicherungsdateien fortlaufend wiederhergestellt, und die Migration wird nur bei manueller Übernahme abgeschlossen. Es wird empfohlen, die manuelle Übernahme erst auszuführen, nachdem die endgültige Log Tail-Sicherung erstellt und in SQL Managed Instance als wiederhergestellt angezeigt wurde. Mit dem abschließenden Übernahmeschritt wird die Datenbank online geschaltet und ist für die Lese- und Schreibverwendung in SQL Managed Instance verfügbar.

Nachdem LRS beendet wurde, entweder automatisch bei AutoVervollständigen oder manuell bei der Übernahme, kann der Wiederherstellungsvorgang für eine Datenbank, die in SQL Managed Instance online geschaltet wurde, nicht fortgesetzt werden. Wenn Sie zusätzliche Sicherungsdateien wiederherstellen möchten, nachdem die Migration durch AutoVervollständigen oder manuell bei der Übernahme abgeschlossen wurde, muss die Datenbank gelöscht werden, und die gesamte Sicherungskette muss von Grund auf neu erstellt werden, indem der LRS neu gestartet wird.

![Erklärung der LRS-Orchestrierungsschritte für SQL Managed Instance](./media/log-replay-service-migrate/log-replay-service-conceptual.png)
    
| Vorgang | Details |
| :----------------------------- | :------------------------- |
| **1. Kopieren von Datenbanksicherungen aus SQL Server in Azure Blob Storage**. | – Kopieren Sie vollständige, differenzielle und Protokollsicherungen aus SQL Server mithilfe von [Azcopy](/azure/storage/common/storage-use-azcopy-v10) oder [Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/) in den Azure Blob Storage-Container. <br />– Verwenden Sie beliebige Dateinamen, da LRS keine bestimmte Dateibenennungskonvention erfordert.<br />– Bei der Migration mehrerer Datenbanken ist für jede Datenbank ein separater Ordner erforderlich. |
| **2. Starten des LRS-Diensts in der Cloud**. | – Der Dienst kann mit einer Auswahl von Cmdlets gestartet werden: <br /> PowerShell [start-azsqlinstancedatabaselogreplay](/powershell/module/az.sql/start-azsqlinstancedatabaselogreplay) <br /> CLI [az_sql_midb_log_replay_start](/cli/azure/sql/midb/log-replay#az_sql_midb_log_replay_start)-Cmdlets. <br /> – Starten Sie LRS separat für jede andere Datenbank, die auf einen anderen Sicherungsordner in Azure Blob Storage verweist. <br />– Sobald der Dienst gestartet wurde, werden vom Azure Blob Storage-Container Sicherungen erstellt und deren Wiederherstellung in SQL Managed Instance gestartet.<br /> – Für den Fall, dass LRS im kontinuierlichen Modus gestartet wurde, achtet der Dienst nach dem Wiederherstellen aller zuerst hochgeladenen Sicherungen auf neue Dateien, die in den Ordner hochgeladen werden, und wendet fortlaufend auf Basis der LSN-Kette Protokolle an, bis der Dienst beendet wird. |
| **2.1. Überwachen des Fortschritt des Vorgangs**. | – Der Fortschritt des Wiederherstellungsvorgangs kann mit einer Auswahl von Cmdlets überwacht werden: <br /> PowerShell [get-azsqlinstancedatabaselogreplay](/powershell/module/az.sql/get-azsqlinstancedatabaselogreplay) <br /> CLI-[az_sql_midb_log_replay_start](/cli/azure/sql/midb/log-replay#az_sql_midb_log_replay_show)-Cmdlets. |
| **2.2. Stoppen\Abbrechen des Vorgangs, falls erforderlich**. | – Für den Fall, dass der Migrationsprozess abgebrochen werden muss, kann der Vorgang mit einer Auswahl von Cmdlets gestoppt werden: <br /> PowerShell [stop-azsqlinstancedatabaselogreplay]/powershell/module/az.sql/stop-azsqlinstancedatabaselogreplay) <br /> CLI-[az_sql_midb_log_replay_stop](/cli/azure/sql/midb/log-replay#az_sql_midb_log_replay_stop)-Cmdlets. <br /><br />– Dies führt dazu, dass die Datenbank, die in SQL Managed Instance wiederhergestellt wird, gelöscht wird. <br />– Nach dem Stoppen kann LRS nicht mehr für eine Datenbank fortgesetzt werden. Der Migrationsprozess muss von Grund auf neu gestartet werden. |
| **3. Übernahme der Cloud bei Bereitschaft**. | – Stoppen Sie die Anwendung und die Workload. Nehmen Sie die letzte Log Tail-Sicherung und das Hochladen in Azure Blob Storage vor.<br /> – Schließen Sie die Übernahme ab, indem Sie den Abschluss des LRS-Vorgangs mit einer Auswahl von Cmdlets initiieren: <br />PowerShell [complete-azsqlinstancedatabaselogreplay](/powershell/module/az.sql/complete-azsqlinstancedatabaselogreplay) <br /> CLI-[az_sql_midb_log_replay_complete](/cli/azure/sql/midb/log-replay#az_sql_midb_log_replay_complete)-Cmdlets. <br /><br />– Dies bewirkt, dass der LRS-Dienst gestoppt und die Datenbank für Lese- und Schreibvorgänge in SQL Managed Instance online geschaltet wird.<br /> – Setzen Sie die Anwendungs-Verbindungszeichenfolge von SQL Server auf SQL Managed Instance zurück. |

## <a name="requirements-for-getting-started"></a>Anforderungen für den Einstieg

### <a name="sql-server-side"></a>SQL Server-seitig
- SQL Server 2008-2019
- Vollständige Sicherung von Datenbanken (eine oder mehrere Dateien)
- Differenzielle Sicherung (eine oder mehrere Dateien)
- Protokollsicherung (keine Aufteilung für Transaktionsprotokolldatei)
- **CHECKSUM muss für Sicherungen aktiviert werden** (obligatorisch).

### <a name="azure-side"></a>Azure-seitig
- PowerShell Az.SQL-Modulversion 2.16.0 oder höher ([installieren](https://www.powershellgallery.com/packages/Az.Sql/) oder Azure [Cloud Shell](/azure/cloud-shell/) verwenden)
- CLI-Version 2.19.0 oder höher ([Installation](/cli/azure/install-azure-cli))
- Azure Blob Storage-Container bereitgestellt
- SAS-Sicherheitstoken mit Berechtigungen nur zum **Lesen** und **Auflisten**, die für den Blob Storage-Container generiert werden

### <a name="migrating-multiple-databases"></a>Migrieren mehrerer Datenbanken
- Sicherungsdateien für verschiedene Datenbanken müssen in Azure Blob Storage in separaten Ordnern abgelegt werden.
- LRS muss separat für jede Datenbank gestartet werden, die auf einen entsprechenden Ordner in Azure Blob Storage verweist.
- LRS kann bis zu 100 gleichzeitige Wiederherstellungsprozesse pro einzelner SQL Managed Instance-Instanz unterstützen.

### <a name="azure-rbac-permissions-required"></a>Erforderliche Azure RBAC-Berechtigungen
Die Ausführung von LRS über die bereitgestellten Clients erfordert eine der folgenden Azure-Rollen:
- Rolle „Besitzer des Abonnements“ oder
- Rolle [Mitwirkender](../../role-based-access-control/built-in-roles.md#sql-managed-instance-contributor) von Managed Instance oder
- Benutzerdefinierte Rolle mit der folgenden Berechtigung:
  - `Microsoft.Sql/managedInstances/databases/*`

## <a name="best-practices"></a>Bewährte Methoden

Die folgenden bewährten Methoden werden dringend empfohlen:
- Führen Sie den [Datenmigrations-Assistenten](/sql/dma/dma-overview) aus, um zu überprüfen, ob Ihre Datenbanken zur Migration zu SQL Managed Instance bereit sind. 
- Teilen Sie vollständige und differenzielle Sicherungen in mehrere Dateien auf, anstatt sie in einer einzelnen Datei zu belassen.
- Aktivieren Sie die Sicherungskomprimierung.
- Verwenden Sie Cloud Shell, um Skripts auszuführen, da es immer auf die neuesten veröffentlichten Cmdlets aktualisiert wird.
- Planen Sie den Abschluss der Migration innerhalb von 47 Stunden ab dem Start des LRS-Diensts. Dies ist eine Toleranzperiode, die nach dem Start von LRS vom System verwaltete Softwarepatches verhindert.

> [!IMPORTANT]
> - Die Datenbank, die mithilfe von LRS wiederhergestellt wird, kann erst verwendet werden, wenn der Migrationsprozess abgeschlossen wurde.
> - Der schreibgeschützte Zugriff auf Datenbanken während der Migration wird von LRS nicht unterstützt.
> - Nachdem die Migration abgeschlossen wurde, wird der Migrationsvorgang abgeschlossen, da LRS die Wiederaufnahme nicht unterstützt.

## <a name="steps-to-execute"></a>Auszuführende Schritte

### <a name="make-backups-on-the-sql-server"></a>Erstellen von Sicherungen in SQL Server

Sicherungen in SQL Server können mit einer der folgenden beiden Optionen erstellt werden:

- Sichern Sie die Daten auf dem lokalen Datenträger, und laden Sie die Dateien dann für den Fall, dass Ihre Umgebung die direkte Sicherung in Azure Blob Storage einschränkt, in Azure Blob Storage hoch.
- Sichern Sie die Daten mit der Option „TO URL“ in T-SQL direkt in Azure Blob Storage, falls Ihre Umgebung und Ihre Sicherheitsverfahren dies ermöglichen. 

Legen Sie Datenbanken fest, die Sie zum vollständigen Wiederherstellungsmodus migrieren möchten, um Protokollsicherungen zuzulassen.

```SQL
-- To permit log backups, before the full database backup, modify the database to use the full recovery
USE master
ALTER DATABASE SampleDB
SET RECOVERY FULL
GO
```

Um die vollständige, differenzielle und Protokollsicherung Ihrer Datenbank im lokalen Speicher manuell zu erstellen, verwenden Sie die folgenden T-SQL-Beispielskripts. Stellen Sie sicher, dass die Option CHECKSUM aktiviert ist, da dies für LRS obligatorisch ist.

```SQL
-- Example on how to make full database backup to the local disk
BACKUP DATABASE [SampleDB]
TO DISK='C:\BACKUP\SampleDB_full.bak'
WITH INIT, COMPRESSION, CHECKSUM
GO

-- Example on how to make differential database backup to the locak disk
BACKUP DATABASE [SampleDB]
TO DISK='C:\BACKUP\SampleDB_diff.bak'
WITH DIFFERENTIAL, COMPRESSION, CHECKSUM
GO

-- Example on how to make the transactional log backup to the local disk
BACKUP LOG [SampleDB]
TO DISK='C:\BACKUP\SampleDB_log.trn'
WITH COMPRESSION, CHECKSUM
GO
```

### <a name="create-azure-blob-storage"></a>Erstellen einer Azure Blob Storage-Instanz

Azure Blob Storage wird als zwischengeschalteter Speicher für Sicherungsdateien zwischen SQL Server und SQL Managed Instance verwendet. Führen Sie die folgenden Schritte aus, um ein neues Speicherkonto und einen Blobcontainer im Speicherkonto zu erstellen:

1. [Erstellen eines Speicherkontos](../../storage/common/storage-account-create.md?tabs=azure-portal)
2. [Erstellen eines Blobcontainers](../../storage/blobs/storage-quickstart-blobs-portal.md) im Speicherkonto

### <a name="copy-backups-from-sql-server-to-azure-blob-storage"></a>Kopieren von Sicherungen aus SQL Server in Azure Blob Storage

Einige der folgenden Ansätze können verwendet werden, um Sicherungen in den Blobspeicher hochzuladen, wenn Datenbanken mithilfe von LRS zu Managed Instance migriert werden:
- Verwenden von [Azcopy](/azure/storage/common/storage-use-azcopy-v10) oder des [Azure Storage-Explorers](https://azure.microsoft.com/features/storage-explorer), um Sicherungen in einen Blobcontainer hochzuladen.
- Verwenden des Storage-Explorers im Azure-Portal.

### <a name="make-backups-from-sql-server-directly-to-azure-blob-storage"></a>Erstellen von Sicherungen aus SQL Server direkt in Azure Blob Storage.

Für den Fall, dass Ihre Unternehmens- und Netzwerkrichtlinie dies zulässt, können Sie Sicherungen aus SQL Server alternativ direkt in Azure Blob Storage erstellen, indem Sie die native Option [BACKUP TO URL](/sql/relational-databases/backup-restore/sql-server-backup-to-url) von SQL Server verwenden. Wenn Sie diese Option nutzen können, ist das Erstellen von Sicherungen im lokalen Speicher und deren Hochladen in Azure Blob Storage nicht erforderlich.

Im ersten Schritt muss für diesen Vorgang ein SAS-Authentifizierungstoken für Azure Blob Storage generiert werden, und das Token muss in SQL Server importiert werden. Der zweite Schritt ist das Erstellen von Sicherungen mit der Option „TO URL“ in T-SQL. Stellen Sie sicher, dass alle Sicherungen mit aktivierter CHEKSUM-Option durchgeführt werden.

Beispielcode zum Erstellen von Sicherungen in Azure Blob Storage finden Sie unten. Dieses Beispiel enthält keine Anweisungen zum Importieren des SAS-Tokens. Ausführliche Anweisungen, z. B. zum Generieren und Importieren des SAS-Tokens in SQL Server, finden Sie im folgenden Tutorial: [Verwenden des Microsoft Azure BLOB-Speicherdiensts mit SQL Server 2016](/sql/relational-databases/tutorial-use-azure-blob-storage-service-with-sql-server-2016#1---create-stored-access-policy-and-shared-access-storage). 

```SQL
-- Example on how to make full database backup to URL
BACKUP DATABASE [SampleDB]
TO URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/SampleDB_full.bak'
WITH INIT, COMPRESSION, CHECKSUM
GO

-- Example on how to make differential database backup to URL
BACKUP DATABASE [SampleDB]
TO URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/SampleDB_diff.bak'  
WITH DIFFERENTIAL, COMPRESSION, CHECKSUM
GO

-- Example on how to make the transactional log backup to URL
BACKUP LOG [SampleDB]
TO URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/SampleDB_log.trn'  
WITH COMPRESSION, CHECKSUM
```

### <a name="generate-azure-blob-storage-sas-authentication-for-lrs"></a>Generieren der Azure Blob Storage-SAS-Authentifizierung für LRS

Azure Blob Storage wird als zwischengeschalteter Speicher für Sicherungsdateien zwischen SQL Server und SQL Managed Instance verwendet. Für die Verwendung durch den LRS-Dienst muss ein SAS-Authentifizierungstoken mit Berechtigungen nur zum Auflisten und Lesen generiert werden. Dadurch kann der LRS-Dienst auf Azure Blob Storage zugreifen und die Sicherungsdateien verwenden, um sie in SQL Managed Instance wiederherzustellen. Führen Sie diese Schritte aus, um die SAS-Authentifizierung für LRS zu generieren:

1. Zugreifen auf den Storage-Explorer über das Azure-Portal
2. Erweitern Sie Blobcontainer.
3. Klicken Sie mit der rechten Maustaste auf den Blobcontainer, und wählen Sie „Shared Access Signature abrufen“ aus. ![ LRS: SAS-Authentifizierungstoken generieren](./media/log-replay-service-migrate/lrs-sas-token-01.png)
4. Wählen Sie den Ablaufzeitraum für das Token aus. Stellen Sie sicher, dass das Token für die Dauer der Migration gültig ist.
5. Wählen Sie die Zeitzone für das Token aus – UTC oder Ortszeit.
    - Die Zeitzone des Tokens stimmt möglicherweise nicht mit der Ihrer SQL Managed Instance-Instanz überein. Stellen Sie sicher, dass das SAS-Token unter Berücksichtigung der Zeitzonen über die entsprechende Gültigkeit verfügt. Legen Sie die Zeitzone nach Möglichkeit auf einen früheren und späteren Zeitpunkt des geplanten Migrationsfensters fest.
6. Wählen Sie nur die Berechtigungen zum Lesen und Auflisten aus.
    - Es dürfen keine weiteren Berechtigungen ausgewählt werden, andernfalls kann LRS nicht gestartet werden. Diese Sicherheitsanforderung ist entwurfsbedingt.
7. Klicken Sie auf die Schaltfläche „Erstellen“. ![LRS: Generieren des SAS-Authentifizierungstokens](./media/log-replay-service-migrate/lrs-sas-token-02.png)

Die SAS-Authentifizierung wird mit der Gültigkeit generiert, die Sie zuvor angegeben haben. Sie benötigen die URI-Version des generierten Tokens, wie im folgenden Screenshot gezeigt.

![Beispiel für einen generierten SAS-Authentifizierungs-URI für LRS](./media/log-replay-service-migrate/lrs-generated-uri-token.png)

### <a name="copy-parameters-from-sas-token-generated"></a>Kopieren von Parametern aus dem generierten SAS-Token

Um das SAS-Token ordnungsgemäß zum Starten von LRS verwenden zu können, müssen wir seine Struktur verstehen. Der URI des generierten SAS-Tokens besteht aus zwei Teilen:
- StorageContainerUri und 
- StorageContainerSasToken, getrennt durch ein Fragezeichen (?), wie in der folgenden Abbildung dargestellt.

    ![Beispiel für einen generierten SAS-Authentifizierungs-URI für LRS](./media/log-replay-service-migrate/lrs-token-structure.png)

- Der erste Teil, der mit „https://“ beginnt und bis zum Fragezeichen (?) reicht, wird für den StorageContainerURI-Parameter verwendet, der als Eingabe an LRS übergeben wird. Dieser liefert LRS-Informationen zu dem Ordner, in dem Datenbanksicherungs-Dateien gespeichert werden.
- Der zweite, nach dem Fragezeichen (?) beginnende Teil, im Beispiel „sp=“ bis zum Ende der Zeichenfolge, ist der StorageContainerSasToken-Parameter. Dieser ist das eigentliche signierte Authentifizierungstoken, das für die angegebene Zeitspanne gültig ist. Dieser Teil muss nicht unbedingt mit „sp=“ beginnen, wie hier gezeigt, und die Groß-/Kleinschreibung ist möglicherweise anders.

Kopieren Sie die Parameter wie folgt:

1. Kopieren Sie den ersten Teil des Tokens beginnend mit „https://“ bis zum Fragezeichen (?), und verwenden Sie ihn als StorageContainerUri-Parameter in PowerShell oder in der CLI, um LRS zu starten, wie im folgenden Screenshot gezeigt.

    ![LRS: Kopieren des StorageContainerUri-Parameters](./media/log-replay-service-migrate/lrs-token-uri-copy-part-01.png)

2. Kopieren Sie den zweiten Teil des Tokens ab dem Fragezeichen (?) bis zum Ende der Zeichenfolge, und verwenden Sie ihn als StorageContainerSasToken-Parameter in PowerShell oder in der CLI, um LRS zu starten, wie im folgenden Screenshot gezeigt.

    ![LRS: Kopieren des StorageContainerSasToken-Parameters](./media/log-replay-service-migrate/lrs-token-uri-copy-part-02.png)

> [!IMPORTANT]
> - Nur die Berechtigungen zum Lesen und Auflisten sind für das SAS-Token für Azure Blob Storage zulässig. Wenn eine andere Berechtigung für das SAS-Authentifizierungstoken erteilt wird, tritt beim Starten des LRS-Diensts ein Fehler auf. Diese Sicherheitsanforderungen sind entwurfsbedingt.
> - Das Token muss über die entsprechende Gültigkeitsdauer verfügen. Stellen Sie sicher, dass die Zeitzonen zwischen dem Token und Managed Instance berücksichtigt werden.
> - Achten Sie darauf, den StorageContainerUri-Parameter für PowerShell oder die CLI aus dem URI des generierten Tokens zu kopieren, beginnend mit „https://“ bis zum Fragezeichen (?). Schließen Sie das Fragezeichen nicht ein.
> Achten Sie darauf, den Parameter StorageContainerSasToken für PowerShell oder die CLI aus dem URI des generierten Tokens zu kopieren, ab dem Fragezeichen (?) bis zum Ende der Zeichenfolge. Schließen Sie das Fragezeichen nicht ein.

### <a name="log-in-to-azure-and-select-subscription"></a>Anmelden bei Azure und Auswählen des Abonnements

Verwenden Sie das folgende PowerShell-Cmdlet, um sich bei Azure anzumelden:

```powershell
Login-AzAccount
```

Wählen Sie mit dem folgenden PowerShell-Cmdlet das entsprechende Abonnement aus, in dem sich Ihre SQL Managed Instance-Instanz befindet:

```powershell
Select-AzSubscription -SubscriptionId <subscription ID>
```

## <a name="start-the-migration"></a>Starten der Migration

Die Migration wird durch Aufrufen des LRS-Diensts gestartet. Der Dienst kann im AutoVervollständigen- oder kontinuierlichen Modus gestartet werden. Beim Starten im AutoVervollständigen-Modus wird die Migration automatisch abgeschlossen, wenn die zuletzt angegebene Sicherungsdatei wiederhergestellt wurde. Diese Option erfordert, dass mit dem Startbefehl der Dateiname der letzten Sicherungsdatei angegeben wird. Wenn LRS im kontinuierlichen Modus gestartet wird, werden alle neu hinzugefügten Sicherungsdateien fortlaufend wiederhergestellt, und die Migration wird nur bei manueller Übernahme abgeschlossen. 

### <a name="start-lrs-in-autocomplete-mode"></a>Starten von LRS im AutoVervollständigen-Modus

Verwenden Sie den folgenden PowerShell-Befehl oder CLI-Befehle, um den LRS-Dienst im AutoVervollständigen-Modus zu starten. Geben Sie den Namen der letzten Sicherungsdatei mit dem Parameter -LastBackupName an. Nach dem Wiederherstellen der letzten angegebenen Sicherungsdatei initiiert der Dienst automatisch eine Übernahme.

Starten von LRS im AutoVervollständigen-Modus – PowerShell-Beispiel:

```PowerShell
Start-AzSqlInstanceDatabaseLogReplay -ResourceGroupName "ResourceGroup01" `
    -InstanceName "ManagedInstance01" `
    -Name "ManagedDatabaseName" `
    -Collation "SQL_Latin1_General_CP1_CI_AS" `
    -StorageContainerUri "https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>" `
    -StorageContainerSasToken "sv=2019-02-02&ss=b&srt=sco&sp=rl&se=2023-12-02T00:09:14Z&st=2019-11-25T16:09:14Z&spr=https&sig=92kAe4QYmXaht%2Fgjocqwerqwer41s%3D" `
    -AutoCompleteRestore `
    -LastBackupName "last_backup.bak"
```

Starten von LRS im AutoVervollständigen-Modus – CLI-Beispiel:

```CLI
az sql midb log-replay start -g mygroup --mi myinstance -n mymanageddb -a --last-bn "backup.bak"
    --storage-uri "https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>"
    --storage-sas "sv=2019-02-02&ss=b&srt=sco&sp=rl&se=2023-12-02T00:09:14Z&st=2019-11-25T16:09:14Z&spr=https&sig=92kAe4QYmXaht%2Fgjocqwerqwer41s%3D"
```

### <a name="start-lrs-in-continuous-mode"></a>Starten von LRS im kontinuierlichen Modus

Starten von LRS im kontinuierlichen Modus – PowerShell-Beispiel:

```PowerShell
Start-AzSqlInstanceDatabaseLogReplay -ResourceGroupName "ResourceGroup01" `
    -InstanceName "ManagedInstance01" `
    -Name "ManagedDatabaseName" `
    -Collation "SQL_Latin1_General_CP1_CI_AS" -StorageContainerUri "https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>" `
    -StorageContainerSasToken "sv=2019-02-02&ss=b&srt=sco&sp=rl&se=2023-12-02T00:09:14Z&st=2019-11-25T16:09:14Z&spr=https&sig=92kAe4QYmXaht%2Fgjocqwerqwer41s%3D"
```

Starten von LRS im kontinuierlichen Modus – CLI-Beispiel:

```CLI
az sql midb log-replay start -g mygroup --mi myinstance -n mymanageddb
    --storage-uri "https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>"
    --storage-sas "sv=2019-02-02&ss=b&srt=sco&sp=rl&se=2023-12-02T00:09:14Z&st=2019-11-25T16:09:14Z&spr=https&sig=92kAe4QYmXaht%2Fgjocqwerqwer41s%3D"
```

### <a name="scripting-lrs-start-in-continuous-mode"></a>Skripterstellung für den LRS-Start im kontinuierlichen Modus

PowerShell- und CLI-Clients zum Starten von LRS im kontinuierlichen Modus sind synchron. Dies bedeutet, dass Clients warten, bis die API-Antwort einen Erfolg oder Fehler beim Starten des Auftrags gemeldet hat. Während dieses Wartens wird die Steuerung nicht an die Eingabeaufforderung zurückgegeben. Für den Fall, dass Sie ein Skript für die Migration erstellen und den LRS-Startbefehl benötigen, um die Steuerung sofort wieder zurückzugeben, um mit dem Rest des Skripts fortzufahren, können Sie PowerShell als Hintergrundauftrag mit dem Schalter -AsJob ausführen. Beispiel:

```PowerShell
$lrsjob = Start-AzSqlInstanceDatabaseLogReplay <required parameters> -AsJob
```

Wenn Sie einen Hintergrundauftrag starten, wird selbst dann sofort ein Auftragsobjekt zurückgegeben, wenn der Abschluss des Auftrags längere Zeit in Anspruch nimmt. Sie können ohne Unterbrechung in der Sitzung weiterarbeiten, während der Auftrag abgeschlossen wird. Ausführliche Informationen zum Ausführen von PowerShell als Hintergrundauftrag finden Sie in der PowerShell-Dokumentation zu [Start-Job](/powershell/module/microsoft.powershell.core/start-job#description).

Wenn Sie einen CLI-Befehl unter Linux als Hintergrundprozess starten, verwenden Sie das kaufmännische „und“-Zeichen (&) am Ende des LRS-Startbefehls.

```CLI
az sql midb log-replay start <required parameters> &
```

> [!IMPORTANT]
> Nachdem LRS gestartet wurde, werden alle vom System verwalteten Softwarepatches während der nächsten 47 Stunden angehalten. Nach Ablauf dieses Zeitfensters stoppt das nächste automatisierte Softwarepatch automatisch den laufenden LRS. In diesem Fall kann die Migration nicht fortgesetzt werden und muss neu gestartet werden. 

## <a name="monitor-the-migration-progress"></a>Überwachen des Migrationsfortschritts

Verwenden Sie den folgenden PowerShell-Befehl, um den Migrationsvorgang zu überwachen:

```PowerShell
Get-AzSqlInstanceDatabaseLogReplay -ResourceGroupName "ResourceGroup01" `
    -InstanceName "ManagedInstance01" `
    -Name "ManagedDatabaseName"
```

Verwenden Sie den folgenden CLI-Befehl, um den Migrationsvorgang zu überwachen:

```CLI
az sql midb log-replay show -g mygroup --mi myinstance -n mymanageddb
```

## <a name="stop-the-migration"></a>Stoppen der Migration

Wenn Sie die Migration stoppen müssen, verwenden Sie die folgenden Cmdlets. Durch das Stoppen der Migration wird die wiederherzustellende Daten in SQL Managed Instance gelöscht, sodass die Migration nicht fortgesetzt werden kann.

Um den Migrationsprozess zu stoppen\abzubrechen, verwenden Sie den folgenden PowerShell-Befehl:

```PowerShell
Stop-AzSqlInstanceDatabaseLogReplay -ResourceGroupName "ResourceGroup01" `
    -InstanceName "ManagedInstance01" `
    -Name "ManagedDatabaseName"
```

Um den Migrationsprozess zu stoppen\abzubrechen, verwenden Sie den folgenden CLI-Befehl:

```CLI
az sql midb log-replay stop -g mygroup --mi myinstance -n mymanageddb
```

## <a name="complete-the-migration-continuous-mode"></a>Abschließen der Migration (kontinuierlicher Modus)

Für den Fall, dass LRS im kontinuierlichen Modus gestartet wird, wird die Migration durch Initiieren der Übernahme abgeschlossen, sobald Sie sichergestellt haben, dass alle Sicherungen wiederhergestellt wurden. Nach dem Abschluss der Übernahme wird die Datenbank migriert und ist für den Lese- und Schreibzugriff bereit.

Verwenden Sie den folgenden PowerShell-Befehl, um den Migrationsprozess im kontinuierlichen LRS-Modus abzuschließen:

```PowerShell
Complete-AzSqlInstanceDatabaseLogReplay -ResourceGroupName "ResourceGroup01" `
-InstanceName "ManagedInstance01" `
-Name "ManagedDatabaseName" `
-LastBackupName "last_backup.bak"
```

Verwenden Sie den folgenden CLI-Befehl, um den Migrationsprozess im kontinuierlichen LRS-Modus abzuschließen:

```CLI
az sql midb log-replay complete -g mygroup --mi myinstance -n mymanageddb --last-backup-name "backup.bak"
```

## <a name="functional-limitations"></a>Funktionale Beschränkungen

Funktionale Einschränkungen des Protokollwiedergabediensts (LRS):
- Die wiederhergestellte Datenbank kann während des Migrationsvorgangs nicht für den schreibgeschützten Zugriff verwendet werden.
- Vom System verwaltete Softwarepatches werden ab dem LRS-Start 47 Stunden blockiert. Nach Ablauf dieses Zeitfensters stoppt das nächste Softwareupdate LRS. In diesem Fall muss LRS von Grund auf neu gestartet werden.
- Für LRS müssen Datenbanken auf dem SQL Server mit aktivierter CHECKSUM-Option gesichert werden.
- Das SAS-Token für die Verwendung durch LRS muss für den gesamten Azure Blob Storage-Container generiert werden und darf nur über Berechtigungen zum Lesen und Auflisten verfügen.
- Sicherungsdateien für verschiedene Datenbanken müssen in Azure Blob Storage in separaten Ordnern abgelegt werden.
- LRS muss separat für jede Datenbank gestartet werden, die auf separate Ordner mit Sicherungsdateien in Azure Blob Storage verweist.
- LRS kann bis zu 100 gleichzeitige Wiederherstellungsprozesse pro einzelner SQL Managed Instance-Instanz unterstützen.

## <a name="troubleshooting"></a>Problembehandlung

Nachdem Sie den LRS gestartet haben, verwenden Sie die Überwachungscmdlets (get-azsqlinstancedatabaselogreplay oder az_sql_midb_log_replay_show), um den Status des Vorgangs anzuzeigen. Wenn LRS nach einiger Zeit nicht startet und eine Fehlermeldung ausgegeben wird, überprüfen Sie, ob eines der häufigsten Probleme aufgetreten ist:
- Gibt es bereits eine Datenbank mit demselben Namen in SQL Managed Instance, die Sie von SQL Server migrieren möchten? Lösen Sie diesen Konflikt auf, indem Sie eine der Datenbanken umbenennen.
- Wurde die Datenbanksicherung in SQL Server mit der Option **CHECKSUM** vorgenommen?
- Gelten die Berechtigungen **Lesen** und **Auflisten** für das SAS-Token nur für den LRS-Dienst?
- Wurde das SAS-Token für LRS beginnend nach dem Fragezeichen „?“ mit Inhalten kopiert, die ähnlich wie „sv=2020-02-10...“ beginnen? 
- Stimmt die **Gültigkeitsdauer** des SAS-Tokens mit dem Zeitfenster für Starten und Abschließen der Migration überein? Möglicherweise fehlt die Übereinstimmung aufgrund unterschiedlicher **Zeitzonen** für SQL Managed Instance. Versuchen Sie, das SAS-Token erneut zu generieren, indem Sie das Gültigkeitsdauer-Zeitfenster des Tokens vor und nach dem aktuellen Datum erweitern.
- Sind Datenbankname, Ressourcengruppenname und Name der Managed Instance-Instanz richtig geschrieben?
- Wenn LRS im AutoVervollständigen-Modus gestartet wurde, wurde ein gültiger Dateiname für die letzte Sicherungsdatei angegeben?

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über das [Migrieren von SQL Server zu SQL Managed Instance](../migration-guides/managed-instance/sql-server-to-managed-instance-guide.md).
- Erfahren Sie mehr über [Unterschiede zwischen SQL Server und Azure SQL Managed Instance](transact-sql-tsql-differences-sql-server.md).
- Erfahren Sie mehr über [Bewährte Methoden für Kostenermittlung und Größenanpassung von zu Azure migrierten Workloads](/azure/cloud-adoption-framework/migrate/azure-best-practices/migrate-best-practices-costs).
