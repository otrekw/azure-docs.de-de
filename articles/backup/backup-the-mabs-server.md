---
title: Sichern des MABS-Servers
description: Erfahren Sie, wie Sie Microsoft Azure Backup Server (MABS) sichern.
ms.topic: conceptual
ms.date: 09/24/2020
ms.openlocfilehash: fbd3d1f2d7cb24c21962dbe5c88acebf73652a04
ms.sourcegitcommit: db925ea0af071d2c81b7f0ae89464214f8167505
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/15/2021
ms.locfileid: "107519118"
---
# <a name="back-up-the-mabs-server"></a>Sichern des MABS-Servers

Um sicherzustellen, dass die Daten wiederhergestellt werden können, wenn Microsoft Azure Backup Server (MABS) ausfällt, benötigen Sie eine Strategie zum Sichern des MABS-Servers. Wenn er nicht gesichert wurde, müssen Sie ihn nach einem Fehler manuell neu erstellen, und datenträgerbasierte Wiederherstellungspunkte können nicht wiederhergestellt werden. Sie können MABS-Server sichern, indem Sie die MABS-Datenbank sichern.

## <a name="back-up-the-mabs-database"></a>Sichern der MABS-Datenbank

Als Teil Ihrer MABS-Sicherungsstrategie müssen Sie die MABS-Datenbank sichern. Die MABS-Datenbank trägt den Namen DPMDB. Diese Datenbank enthält die MABS-Konfiguration sowie Daten zu Sicherungen von MABS. Bei einem Notfall können Sie die meisten Funktionen eines MABS-Servers mithilfe einer aktuellen Sicherung der Datenbank erneut erstellen. Unter der Annahme, dass Sie die Datenbank wiederherstellen können, kann auf bandbasierte Sicherungen zugegriffen werden, die alle Schutzgruppeneinstellungen und Sicherungszeitpläne beibehalten. Wenn die Datenträger des MABS-Speicherpools nicht vom Ausfall betroffen sind, können auch die datenträgerbasierten Sicherungen nach der Neuerstellung weiterhin verwendet werden. Sie können die Datenbank mithilfe mehrerer unterschiedlicher Methoden sichern.

|Datenbanksicherungsmethode|Vorteile|Nachteile|
|--------------------------|--------------|-----------------|
|[Sichern in Azure](#back-up-to-azure)|<li>Einfache Konfiguration und Überwachung in MABS.<li>Mehrere Speicherorte der gesicherten Datenbankdateien.<li>Cloud-Speicher bietet eine zuverlässige Lösung für die Notfallwiederherstellung.<li>Sehr sicherer Speicher für die Datenbank.<li>Unterstützung für 120 Online-Wiederherstellungspunkte.|<li>Erfordert ein Azure-Konto und zusätzliche MABS-Konfiguration. Verursacht Kosten für Azure-Speicher.<li> Erfordert eine unterstützte Version eines Windows Server-basierten Systems mit dem Azure-Agent für Zugriff auf MABS-Sicherungen, die im Azure-Sicherungstresor gespeichert sind. Dies darf kein anderer MABS-Server sein.<li>Keine Option, wenn die Datenbank lokal gehostet wird und Sie sekundären Schutz aktivieren möchten. <li>Erfordert zusätzliche Zeit für die Vorbereitung und Wiederherstellung.|
|[Sichern der Datenbank durch Sichern des MABS-Speicherpools](#back-up-the-database-by-backing-up-the-mabs-storage-pool)|<li>Einfache Konfiguration und Überwachung.<li>Die Sicherung wird auf den Datenträgern des MABS-Speicherpools gespeichert, und der lokale Zugriff ist einfach.<li>Geplante MABS-Sicherungen unterstützen 512 schnelle vollständige Sicherungen. Wenn Sie sich stündlich sichern, verfügen Sie über 21 Tage vollständigen Schutz.|<li>Keine gute Option für die Notfallwiederherstellung. Diese geschieht online, und die Wiederherstellung funktioniert möglicherweise nicht wie erwartet, wenn der MABS-Server oder der Speicherpooldatenträger ausfällt.<li>Keine Option, wenn die Datenbank lokal gehostet wird und Sie sekundären Schutz aktivieren möchten. <li>Einige Vorbereitung und spezielle Schritte für den Zugriff auf Wiederherstellungspunkte sind erforderlich, wenn der MABS-Dienst oder die Konsole nicht ausgeführt wird bzw. nicht funktioniert.|
|[Sichern mit der systemeigenen SQL Server-Sicherung auf einem lokalen Datenträger](#back-up-with-native-sql-server-backup-to-a-local-disk)|<li>In SQL Server integriert.<li>Die Sicherung wird auf einem lokalen Datenträger gespeichert, auf den leicht zugegriffen werden kann.<li>Sie können die Sicherung so häufig wie gewünscht planen.<li>Vollständig unabhängig von MABS.<li>Sie können eine Bereinigung der Sicherungsdatei planen.|<li>Keine gute Option für die Notfallwiederherstellung, es sei denn, die Sicherungen werden an einen Remotespeicherort kopiert.<li>Erfordert lokalen Speicher für Sicherungen, wodurch die Aufbewahrungsdauer und die Häufigkeit eingeschränkt werden können.|
|[Sichern mit der nativen SQL-Sicherung und dem MABS-Schutz in einer durch MABS geschützten Freigabe](#back-up-to-a-share-protected-by-mabs)|<li>Einfache Überwachung in MABS.<li>Mehrere Speicherorte der gesicherten Datenbankdateien.<li>Einfacher Zugriff über beliebigen Windows-Computer im Netzwerk.<li>Potenziell die schnellste Wiederherstellungsmethode.|<li>Unterstützt nur 64 Wiederherstellungspunkte.<li>Keine gute Option für die Wiederherstellung eines Standorts. Ein Ausfall des MABS-Servers oder des MABS-Speicherpools kann die Wiederherstellungsbemühungen behindern.<li>Keine Option, wenn die MABS-Datenbank lokal gehostet wird und Sie sekundären Schutz aktivieren möchten. <li>Einige zusätzliche Vorbereitung ist erforderlich, um diesen zu konfigurieren und zu testen.<li>Sollte der MABS-Server selbst ausfallen, aber die Datenträger des MABS-Speicherpools sind in Ordnung, ist etwas zusätzliche Vorbereitungs- und Wiederherstellungszeit erforderlich.|

- Wenn Sie Sicherung mithilfe einer MABS-Schutzgruppe verwenden, wird empfohlen, eine spezielle Schutzgruppe für die Datenbank zu verwenden.

    > [!NOTE]
    > Zu Wiederherstellungszwecken muss die MABS-Installation, die Sie mit der MABS-Datenbank wiederherstellen möchten, der Version der MABS-Datenbank selbst entsprechen.  Wenn beispielsweise die wiederherzustellende Datenbank die Version MABS V3 Updaterollup 1 hat, muss der MABS-Server dieselbe Version mit Updaterollup 1 ausführen. Dies bedeutet, dass Sie ggf. MABS deinstallieren und mit einer kompatiblen Version neu installieren müssen, bevor Sie die Datenbank wiederherstellen.  Zum Prüfen der Datenbankversion müssen Sie ggf. eine manuelle Bereitstellung in einer temporären Datenbank ausführen und dann eine SQL-Abfrage auf die Datenbank anwenden, um basierend auf den Haupt- und Nebenversionen das letzte installierte Rollup zu ermitteln.

- Gehen Sie folgendermaßen vor, um die Version der MABS-Datenbank zu überprüfen:

    1. Zum Ausführen der Abfrage öffnen Sie SQL Management Studio und stellen eine Verbindung mit der SQL-Instanz her, in der die MABS-Datenbank ausgeführt wird.

    2. Wählen Sie die MABS-Datenbank aus, und starten Sie dann eine neue Abfrage.

    3. Fügen Sie die folgende SQL-Abfrage in den Abfragebereich ein, und führen Sie sie aus:

        `Select distinct MajorVersionNumber,MinorVersionNumber ,BuildNumber, FileName FROM dbo.tbl\_AM\_AgentPatch order byMajorVersionNumber,MinorVersionNumber,BuildNumber`

    Wenn in den Abfrageergebnissen nichts zurückgegeben wird oder der MABS-Server aus früheren Versionen aktualisiert wurde, aber seitdem kein neues Updaterollup installiert wurde, gibt es keinen Eintrag für die Haupt- und Nebenversion für eine Basisinstallation von MABS. Informationen zu den MABS-Versionen von Updaterollups finden Sie in der [Liste der Buildnummern für MABS](https://social.technet.microsoft.com/wiki/contents/articles/36381.microsoft-azure-backup-server-list-of-build-numbers.aspx).

### <a name="back-up-to-azure"></a>Sicherung in Azure

1. Bevor Sie beginnen, müssen Sie ein Skript ausführen, um den Pfad des MABS-Replikat-Volumebereitstellungspunkts abzurufen, damit Sie wissen, welcher Wiederherstellungspunkt die MABS-Sicherung enthält. Führen Sie diesen Schritt nach der ersten Replikation mit Azure Backup aus. Ersetzen Sie im Skript `dplsqlservername%` durch den Namen der SQL Server-Instanz, die die MABS-Datenbank hostet.

    ```SQL
    Select ag.NetbiosName as ServerName,ds.DataSourceName,vol.MountPointPath
    from tbl_IM_DataSource as ds
    join tbl_PRM_LogicalReplica as lr on ds.DataSourceId=lr.DataSourceId
    join tbl_AM_Server as ag on ds.ServerId=ag.ServerId
    join tbl_SPM_Volume as vol on lr.PhysicalReplicaId=vol.VolumeSetID
    and vol.Usage =1
    and lr.Validity in (1,2)
    where ds.datasourcename like '%dpmdb%'
    and servername like '%dpmsqlservername%' --netbios name of server hosting DPMDB
    ```

    Stellen Sie sicher, dass Sie über den Passcode verfügen, der bei der Installation des Azure Recovery Services-Agents und der Registrierung des MABS-Servers im Azure Backup-Tresor angegeben wurde. Sie benötigen diese Kennung, um die Sicherung wiederherzustellen.

2. Erstellen Sie einen Azure Backup-Tresor, laden Sie Installationsdatei des Azure Backup-Agents sowie die Anmeldeinformationen für den Tresor herunter. Führen Sie die Installationsdatei aus, um den Agent auf dem MABS-Server zu installieren, und verwenden Sie die Tresoranmeldeinformationen, um den MABS-Server im Tresor zu registrieren. [Weitere Informationen](backup-azure-microsoft-azure-backup.md).

3. Richten Sie nach der Konfiguration des Tresors eine MABS-Schutzgruppe ein, die die MABS-Datenbank enthält. Wählen Sie diese aus, um sie auf einem Datenträger und in Azure zu sichern.

#### <a name="recover-the-mabs-database-from-azure"></a>Wiederherstellen der Datenbank aus Azure

Sie können die Datenbank mit einem beliebigen MABS-Server, der im Azure Backup-Tresor registriert ist, wie folgt wiederherstellen:

1. Wählen Sie in der MABS-Konsole **Wiederherstellung** > **Externen MABS hinzufügen** aus.

2. Geben Sie die Anmeldeinformationen für den Tresor an (aus dem Azure Backup-Tresor herunterladen). Beachten Sie, dass die Anmeldeinformationen nur zwei Tage lang gültig sind.

3. Wählen Sie unter **Externen MABS für Wiederherstellung auswählen** den MABS-Server aus, für den Sie die Datenbank wiederherstellen möchten, geben Sie die Verschlüsselungspassphrase ein, und wählen Sie **OK** aus.

4. Wählen Sie den zu verwendenden Wiederherstellungspunkt aus der Liste der verfügbaren Punkte aus. Wählen Sie **Externen MABS löschen** aus, um zur lokalen MABS-Ansicht zurückzukehren.

## <a name="back-up-the-mabs-database-to-mabs-storage-pool"></a>Sichern der MABS-Datenbank im MABS-Speicherpool

> [!NOTE]  
> Diese Option gilt für MABS mit Modern Backup Storage.

1. Wählen Sie in der MABS-Konsole **Schutz** > **Schutzgruppe erstellen** aus.
2. Wählen Sie auf der Seite **Schutzgruppentyp auswählen** die Option **Server** aus.
3. Wählen Sie auf der Seite **Gruppenmitglieder auswählen** die Option **DPM-Datenbank** aus. Erweitern Sie den MABS-Server, und wählen Sie DPMDB aus.
4. Wählen Sie auf der Seite **Datenschutzmethode auswählen** die Option **Ich möchte kurzfristigen Schutz per: Datenträger**. Legen Sie die Richtlinienoptionen für den kurzfristigen Schutz fest.
5. Führen Sie nach der ersten Replikation der MABS-Datenbank das folgende SQL-Skript aus:

```SQL
select AG.NetbiosName, DS.DatasourceName, V.AccessPath, LR.PhysicalReplicaId from tbl_IM_DataSource DS
join tbl_PRM_LogicalReplica as LR
on DS.DataSourceId = LR.DataSourceId
join tbl_AM_Server as AG
on DS.ServerId=AG.ServerId
join tbl_PRM_ReplicaVolume RV
on RV.ReplicaId = LR.PhysicalReplicaId
join tbl_STM_Volume V
on RV.StorageId = V.StorageId
where datasourcename like N'%dpmdb%' and ds.ProtectedGroupId is not null
and LR.Validity in (1,2)
and AG.ServerName like N'%<dpmsqlservername>%' -- <dpmsqlservername> is a placeholder, put netbios name of server hosting DPMDB
```

### <a name="recover-mabs-database"></a>Wiederherstellen der MABS-Datenbank

Zum Wiederherstellen des MABS-Servers mit derselben Datenbank müssen Sie zuerst die MABS-Datenbank wiederherstellen und sie mit dem neu installierten MABS-Server synchronisieren.

#### <a name="use-the-following-steps"></a>Führen Sie die folgenden Schritte aus:

1. Öffnen Sie eine Administrator-Eingabeaufforderung, und führen Sie `psexec.exe -s powershell.exe` aus, um ein PowerShell-Fenster im Systemkontext zu starten.
2. Geben Sie den Ort aus, von dem aus Sie die Datenbank wiederherstellen möchten:

#### <a name="to-copy-the-database-from-the-last-backup"></a>Kopieren der Datenbank aus der letzten Sicherung

1. Navigieren Sie zum Pfad der Replikat-VHD: `\<MABSServer FQDN\>\<PhysicalReplicaId\>\<PhysicalReplicaId\>` .
2. Binden Sie die darin enthaltene VHD **disk0.vhdx** mit dem Befehl `mount-vhd disk0.vhdx` ein.
3. Nachdem die Replikat-VHD eingebunden wurde, verwenden Sie `mountvol.exe`, um dem Replikatvolume mithilfe der physischen Replikat-ID aus der Ausgabe des SQL-Skripts einen Laufwerkbuchstaben zuzuweisen. Beispiel: `mountvol X: \?\Volume{}\`

#### <a name="to-copy-the-database-from-a-previous-recovery-point"></a>Kopieren der Datenbank von einem vorherigen Wiederherstellungspunkt

1. Navigieren Sie zum DPMDB-Containerverzeichnis `\<MABSServer FQDN\>\<PhysicalReplicaId\>`. Es werden mehrere Verzeichnisse mit einigen eindeutigen GUID-Bezeichnern darunter angezeigt, die den für die MABS-Datenbank erfassten Wiederherstellungspunkten entsprechen. Andere Verzeichnisse stellen einen PIT/Wiederherstellungspunkt dar.
2. Navigieren Sie zu einem beliebigen PIT-VHD-Pfad (z. B. `\<MABSServer FQDN\>\<PhysicalReplicaId\>\<PITId\>`), und binden Sie die darin enthaltene VHD **disk0.vhdx** mit dem Befehl `mount-vhd disk0.vhdx` ein.
3. Nachdem die Replikat-VHD eingebunden wurde, verwenden Sie `mountvol.exe`, um dem Replikatvolume mithilfe der physischen Replikat-ID aus der Ausgabe des SQL-Skripts einen Laufwerkbuchstaben zuzuweisen. Beispiel: `mountvol X: \?\Volume{}\`

   Alle Begriffe, die in den oben genannten Schritten in eckigen Klammern angegeben werden, sind Platzhalter. Ersetzen Sie diese wie folgt durch die entsprechenden Werte:
   - **ReFSVolume**: Zugriffspfad aus der Ausgabe des SQL-Skripts
   - **MABSServer FQDN**: Vollqualifizierter Name des MABS-Servers
   - **PhysicalReplicaId**: Physische Replikat-ID aus der Ausgabe des SQL-Scripts
   - **PITId**: Anderer GUID-Bezeichner als die physische Replikat-ID im Containerverzeichnis.
4. Öffnen Sie eine weitere Administratoreingabeaufforderung, und führen Sie `psexec.exe -s cmd.exe` aus, um eine Eingabeaufforderung im Systemkontext zu starten.
5. Ändern Sie das Verzeichnis in das Laufwerk „X:“, und navigieren Sie zum Speicherort der MABS-Datenbankdateien.
6. Kopieren Sie sie an einen Speicherort, der sich für die Wiederherstellung eignet. Beenden Sie das psexec-Befehlsfenster, nachdem der Kopiervorgang abgeschlossen ist.
7. Wechseln Sie zum psexec-PowerShell-Fenster, das Sie in Schritt 1 geöffnet haben, navigieren Sie zum VHDX-Pfad, und heben Sie die Einbindung der VHDX-Datei mithilfe des Befehls `dismount-vhd disk0.vhdx` auf.
8. Nachdem Sie den MABS-Server neu installiert haben, können Sie die wiederhergestellte DPMDB zum Anfügen an den MABS-Server verwenden, indem Sie den Befehl `DPMSYNC-RESTOREDB` ausführen.
9. Führen Sie `DPMSYNC-SYNC` aus, nachdem `DPMSYNC-RESTOREDB` abgeschlossen ist.

### <a name="back-up-the-database-by-backing-up-the-mabs-storage-pool"></a>Sichern der Datenbank durch Sichern des MABS-Speicherpools

> [!NOTE]
> Diese Option gilt für MABS mit Legacyspeicher.

Bevor Sie beginnen, müssen Sie ein Skript ausführen, um den Pfad des MABS-Replikat-Volumebereitstellungspunkts abzurufen, damit Sie wissen, welcher Wiederherstellungspunkt die MABS-Sicherung enthält. Führen Sie diesen Schritt nach der ersten Replikation mit Azure Backup aus. Ersetzen Sie im Skript `dplsqlservername%` durch den Namen der SQL Server-Instanz, die die MABS-Datenbank hostet.

```SQL
Select ag.NetbiosName as ServerName,ds.DataSourceName,vol.MountPointPath
from tbl_IM_DataSource as ds
join tbl_PRM_LogicalReplica as lr on ds.DataSourceId=lr.DataSourceId
join tbl_AM_Server as ag on ds.ServerId=ag.ServerId
join tbl_SPM_Volume as vol on lr.PhysicalReplicaId=vol.VolumeSetID
and vol.Usage =1
and lr.Validity in (1,2)
where ds.datasourcename like '%dpmdb%'
and servername like '%dpmsqlservername%' --netbios name of server hosting DPMDB
```

1. Wählen Sie in der MABS-Konsole **Schutz** > **Schutzgruppe erstellen** aus.

2. Wählen Sie auf der Seite **Schutzgruppentyp auswählen** die Option **Server** aus.

3. Wählen Sie auf der Seite **Gruppenmitglieder auswählen** die MABS-Datenbank aus. Erweitern Sie das MABS-Serverelement, und wählen Sie **DPMDB** aus.

4. Auf der Seite **Datenschutzmethode auswählen** wählen Sie **Ich möchte kurzfristigen Schutz per: Datenträger** aus. Legen Sie die Richtlinienoptionen für den kurzfristigen Schutz fest. Für MABS-Datenbanken empfiehlt sich eine Beibehaltungsdauer von zwei Wochen.

#### <a name="recover-the-database"></a>Wiederherstellen der Datenbank

Wenn der MABS-Server noch funktionstüchtig und der Speicherpool intakt ist (z. B. bei Problemen mit dem MABS-Dienst oder der Konsole), kopieren Sie die Datenbank wie folgt aus dem Replikatvolume oder einer Schattenkopie:

1. Wählen Sie den Zeitpunkt, an dem die Datenbank wiederhergestellt werden soll.

    - Wenn Sie die Datenbank aus der letzten Sicherung kopieren möchten, die direkt aus dem MABS-Replikatvolume erstellt wurde, verwenden Sie **mountvol.exe**, um dem Replikatvolume unter Verwendung der GUID aus der Ausgabe des SQL-Skripts einen Laufwerkbuchstaben zuzuweisen. Beispiel: `C:\Mountvol X: \\?\Volume{d7a4fd76\-a0a8\-11e2\-8fd3\-001c23cb7375}\`

    - Wenn Sie die Datenbank aus einem vorherigen Wiederherstellungspunkt (Schattenkopie) kopieren möchten, müssen Sie alle Schattenkopien für das Replikat mit der Volume-GUID aus der Ausgabe des SQL-Skripts auflisten. Dieser Befehl listet Schattenkopien für dieses Volume auf: `C:\>Vssadmin list shadows /for\=\\?\Volume{d7a4fd76-a0a8-11e2-8fd3-001c23cb7375}\`. Beachten Sie die Erstellungszeit und Schattenkopien-ID, die die Grundlage der Wiederherstellung bilden.

2. Verwenden Sie anschließend **diskshadow.exe** zum Bereitstellen der Schattenkopie auf dem nicht verwendeten Laufwerkbuchstaben „X:“ unter Verwendung der Schattenkopie-ID, damit Sie die Datenbankdateien kopieren können.

3. Öffnen Sie eine Administratoreingabeaufforderung, und führen Sie `psexec.exe -s cmd.exe` aus, um eine Eingabeaufforderung im Systemkontext zu starten, damit Sie über die Berechtigung zum Navigieren durch das Replikatvolume („X:“) und zum Kopieren der Dateien verfügen.

4. Wechseln Sie mit „cd“ zum Laufwerk „X:“, und navigieren Sie zum Speicherort der MABS-Datenbankdateien. Kopieren Sie sie an einen Speicherort, der sich für die Wiederherstellung eignet. Nachdem der Kopiervorgang abgeschlossen ist, schließen Sie das Fenster von „psexec cmd“. Führen Sie **diskshadow.exe** aus, und heben Sie die Verfügbarkeit des Volumes „X:“ auf.

5. Sie können nun die Datenbankdateien mit SQL Management Studio oder durch Ausführen von **DPMSYNC\-RESTOREDB** wiederherstellen.

## <a name="back-up-with-native-sql-server-backup-to-a-local-disk"></a>Sichern mit der systemeigenen SQL Server-Sicherung auf einem lokalen Datenträger

Sie können die MABS-Datenbank auf einem lokalen Datenträger mit nativer SQL Server-Sicherung unabhängig von MABS sichern.

- Verschaffen Sie sich einen [Überblick](/sql/relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases) über die SQL Server-Sicherung.

- [Erfahren Sie mehr](/sql/relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service) zum Sichern von SQL Server in die Cloud.

## <a name="back-up-to-a-share-protected-by-mabs"></a>Sichern in einer durch MABS geschützte Freigabe

Diese Sicherungsoption verwendet natives SQL zum Sichern der MABS-Datenbank in einer Freigabe, schützt die Freigabe mit MABS und verwendet die früheren Versionen von Windows VSS, um die Wiederherstellung zu ermöglichen.

### <a name="before-you-start"></a>Vor der Installation

1. Erstellen Sie auf dem SQL Server-Computer einen Ordner in einem Laufwerk mit ausreichend freiem Speicherplatz für eine einzelne Kopie einer Sicherung. Beispiel: `C:\MABSBACKUP`.

1. Geben Sie den Ordner frei. Geben Sie z. B. den Ordner `C:\MABSBACKUP` als *DPMBACKUP* frei.

1. Kopieren Sie den nachstehenden OSQL Befehl, und fügen Sie ihn in Editor ein. Speichern Sie die Datei als `C:\MABSACKUP\bkupdb.cmd`. Stellen Sie sicher, dass die Erweiterung TXT nicht vorhanden ist. Ändern Sie „SQL_Instance_name“ und „DPMDB_NAME“ entsprechend den vom MABS-Server verwendeten Instanz- und DPMDB-Namen.

    ```SQL
    OSQL -E -S localhost\SQL_INSTANCE_NAME -Q "BACKUP DATABASE DPMDB_NAME TO DISK='C:\DPMBACKUP\dpmdb.bak' WITH FORMAT"
    ```

1. Öffnen Sie mit Editor die Datei **ScriptingConfig.xml**, die sich unter dem Ordner `C:\Program Files\Microsoft System Center\DPM\DPM\Scripting` auf dem MABS-Server befindet.

1. Ändern Sie **Scriptingconfig.xml**, und ändern Sie **DataSourceName =** in das Laufwerk, das den Ordner bzw. die Freigabe DPMDBBACKUP enthält. Ändern Sie den Eintrag PreBackupScript in den vollständigen Pfad und Namen der Datei **bkupdb.cmd**, die Sie in Schritt 3 gespeichert haben.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <ScriptConfiguration xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:xsd="http://www.w3.org/2001/XMLSchema"
    xmlns="https://schemas.microsoft.com/2003/dls/ScriptingConfig.xsd">
    <DatasourceScriptConfig DataSourceName="C:">
    <PreBackupScript>C:\MABSDBBACKUP\bkupdb.cmd</PreBackupScript>
    <TimeOut>120</TimeOut>
    </DatasourceScriptConfig>
    </ScriptConfiguration>
    ```

1. Speichern Sie die Änderungen an der Datei **ScriptingConfig.xml**.

1. Schützen Sie den Ordner „C:\MABSBACKUP“ oder die Freigabe `\sqlservername\MABSBACKUP` mithilfe von MABS, und warten Sie, bis das erste Replikat erstellt wurde. Im Ordner „C:\MABSBACKUP“ sollte als Ergebnis der Ausführung des Skripts vor der Sicherung die Datei **dpmdb.bak** enthalten sein, die wiederum in das MABS-Replikat kopiert wurde.

1. Wenn Sie die Self-Service-Wiederherstellung nicht aktivieren, müssen einige zusätzliche Schritte zum Freigeben des Ordners „MABSBACKUP“ für das Replikat erfolgen:

    1. Bestimmen Sie in der MABS-Verwaltungskonsole unter **Schutz** die MABSBACKUP-Datenquelle, und wählen Sie sie aus. Wählen Sie im Detailbereich **Klicken Sie hier für Details** und dann den Link zum Replikatpfad aus, und kopieren Sie den Pfad in Editor. Entfernen Sie den Quellpfad, und behalten Sie den Zielpfad bei. Der Pfad sollte etwa wie folgt aussehen: `C:\Program Files\Microsoft System Center\DPM\DPM\Volumes\Replica\File System\vol_c9aea05f-31e6-45e5-880c-92ce5fba0a58\454d81a0-0d9d-4e07-9617-d49e3f2aa5de\Full\DPMBACKUP`.

    2. Stellen Sie eine Freigabe in diesem Pfad mithilfe des Freigabenamens **MABSSERVERNAME-DPMDB** her. Sie können an einer administrativen Eingabeaufforderung den folgenden Net Share-Befehl aufrufen.

        ```cmd
        Net Share MABSSERVERNAME-dpmdb="C:\Program Files\Microsoft System Center\DPM\DPM\Volumes\Replica\File System\vol_c9aea05f-31e6-45e5-880c-92ce5fba0a58\454d81a0-0d9d-4e07-9617-d49e3f2aa5de\Full\DPMBACKUP"
        ```

### <a name="configure-the-backup"></a>Konfigurieren der Sicherung

Sie können die MABS-Datenbank wie jede andere SQL Server-Datenbank sichern, indem Sie die native SQL Server-Sicherung verwenden.

- Verschaffen Sie sich einen [Überblick](/sql/relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases) über die SQL Server-Sicherung.

- [Erfahren Sie mehr](/sql/relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service) zum Sichern von SQL Server in die Cloud.

### <a name="recover-the-mabs-database"></a>Wiederherstellen der MABS-Datenbank

1. Stellen Sie auf einem Windows-Computer in Explorer eine Verbindung mit der Freigabe `\\MABSServer\MABSSERVERNAME-dpmdb` her.

2. Klicken Sie mit der rechten Maustaste auf die Datei **dpmdb.bak**, um Eigenschaften anzuzeigen. Auf der Registerkarte **Vorgängerversionen** werden alle Sicherungen gezeigt, die Sie auswählen und kopieren können. Der Ordner „C:\MABSBACKUP“ enthält zudem noch die allerletzte Sicherung, auf die ebenfalls einfach zugegriffen werden kann.

3. Wenn Sie einen an ein SAN angeschlossenen MABS-Speicherpooldatenträger auf einen anderen Server verschieben müssen, um Daten vom Replikatvolume lesen zu können, oder Windows neu installieren müssen, um Daten von lokal angeschlossenen Datenträgern zu lesen, müssen Sie vorher den Pfad des MABS-Replikat-Volumebereitstellungspunkts oder die Volume-GUID kennen, damit Sie wissen, welches Volume die Datenbanksicherung enthält. Mithilfe des nachstehenden SQL-Skripts können Sie diese Informationen jederzeit extrahieren, nachdem der anfänglichen Schutz aktiviert wurde, doch bevor die Wiederherstellung erfolgen soll. Ersetzen Sie `%dpmsqlservername%` durch den Namen des SQL Server-Computers, der die Datenbank hostet.

    ```sql
    Select ag.NetbiosName as
    ServerName,ds.DataSourceName,vol.MountPointPath,vol.GuidName
    from tbl_IM_DataSource as ds
    join tbl_PRM_LogicalReplica as lr on ds.DataSourceId=lr.DataSourceId
    join tbl_AM_Server as ag on ds.ServerId=ag.ServerId
    join tbl_SPM_Volume as vol on lr.PhysicalReplicaId=vol.VolumeSetID
    and vol.Usage =1
    and lr.Validity in (1,2)
    where ds.datasourcename like '%C:\%' -- volume drive letter for DPMBACKUP
    and servername like '%dpmsqlservername%' --netbios name of server hosting DPMDB

    ```

4. Wenn Sie nach dem Verschieben der MABS-Speicherpooldatenträger oder einer MABS-Serverneuerstellung eine Wiederherstellung ausführen müssen:

    1. Sie kennen die Volume-GUID. Wenn also dieses Volume auf einem anderen Windows-Server oder nach einer Neuerstellung eines MABS-Servers bereitgestellt werden muss, verwenden Sie **mountvol.exe**, um ihm mittels der Volume-GUID aus der SQL-Skriptausgabe einen Laufwerkbuchstabe zuzuweisen: `C:\Mountvol X: \\?\Volume{d7a4fd76-a0a8-11e2-8fd3-001c23cb7375}\`.

    2. Geben Sie den Ordner „MABSBACKUP“ auf dem Replikatvolume mit dem Laufwerkbuchstaben und dem Teil des Replikatpfads erneut frei, der die Ordnerstruktur darstellt.

        ```cmd
        net share SERVERNAME-DPMDB="X:\454d81a0-0d9d-4e07-9617-d49e3f2aa5de\Full\DPMBACKUP"
        ```

    3. Stellen Sie auf einem Windows-Computer in Explorer eine Verbindung mit der Freigabe `\\SERVERNAME\MABSSERVERNAME-dpmdb` her.

    4. Klicken Sie mit der rechten Maustaste auf die Datei **dpmdb.bak**, um die Eigenschaften anzuzeigen. Auf der Registerkarte **Vorgängerversionen** werden alle Sicherungen gezeigt, die Sie auswählen und kopieren können.

## <a name="using-dpmsync"></a>Verwenden von DPMSync

**DpmSync** ist ein Befehlszeilentool, mit dem Sie die MABS-Datenbank mit dem Zustand der Datenträger im Speicherpool und mit den installierten Schutz-Agents synchronisieren können. DpmSync stellt die MABS-Datenbank wieder her, synchronisiert die MABS-Datenbank mit den Replikaten im Speicherpool, stellt die Berichtsdatenbank wieder her und ordnet fehlende Replikate neu zu.

### <a name="parameters"></a>Parameter

| Parameter      | Beschreibung    |
|----------------|-----------------------------|
| **-RestoreDb**                       | Stellt eine MABS-Datenbank aus einem angegebenen Speicherort wieder her.|
| **-Sync**                            | Synchronisiert wiederhergestellte Datenbanken. Nachdem Sie die Datenbanken wiederhergestellt haben, müssen Sie **DpmSync -Sync** ausführen. Nachdem Sie **DpmSync -Sync** ausgeführt haben, können einige Replikate weiterhin als fehlend markiert sein. |
| **-DbLoc** *Speicherort*                | Gibt den Speicherort der Sicherung der MABS-Datenbank an.|
| **-InstanceName** <br/>*server\instance*     | Instanz, in der DPMDB wiederhergestellt werden muss.|
| **-ReallocateReplica**         | Ordnet alle fehlenden Replikatvolumes ohne Synchronisierung neu zu. |
| **-DataCopied**                      | Zeigt an, dass das Laden der Daten in die neu zugeordneten Replikatvolumes abgeschlossen ist. Dies gilt nur für Clientcomputer. |

**Beispiel 1:** Führen Sie den folgenden Befehl aus, um die MABS-Datenbank von einem lokalen Sicherungsmedium auf dem MABS-Server wiederherzustellen:

```cmd
DpmSync –RestoreDb -DbLoc G:\DPM\Backups\2005\November\DPMDB.bak
```

Nachdem Sie die MABS-Datenbank wiederhergestellt haben, führen Sie den folgenden Befehl aus, um die Datenbanken zu synchronisieren:

```cmd
DpmSync -Sync
```

Nachdem Sie die MABS-Datenbank wiederhergestellt und synchronisiert und bevor Sie die Replikate wiederhergestellt haben, führen Sie den folgenden Befehl aus, um Speicherplatz auf dem Datenträger für das Replikat neu zuzuweisen:

```cmd
DpmSync -ReallocateReplica
```

**Beispiel 2:** Um die MABS-Datenbank aus einer Remotedatenbank wiederherzustellen, führen Sie den folgenden Befehl auf dem Remotecomputer aus:

```cmd
DpmSync –RestoreDb -DbLoc G:\DPM\Backups\2005\November\DPMDB.bak –InstanceName contoso\ms$dpm
```

Nachdem Sie die MABS-Datenbank wiederhergestellt haben, führen Sie den folgenden Befehl auf dem MABS-Server aus, um die Datenbanken zu synchronisieren:

```cmd
DpmSync -Sync
```

Nachdem Sie die MABS-Datenbank wiederhergestellt und synchronisiert und bevor Sie die Replikate wiederhergestellt haben, führen Sie den folgenden Befehl auf dem MABS-Server aus, um Speicherplatz auf dem Datenträger für das Replikat neu zuzuweisen:

```cmd
DpmSync -ReallocateReplica
```

## <a name="next-steps"></a>Nächste Schritte

- [MABS-Unterstützungsmatrix](backup-support-matrix-mabs-dpm.md)
- [Häufig gestellte Fragen (FAQ) zu MABS](backup-azure-dpm-azure-server-faq.yml)