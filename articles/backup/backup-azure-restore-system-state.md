---
title: Wiederherstellen des Systemstatus auf einem Windows Server-Computer
description: Ausführliche Anleitung zum Wiederherstellen des Windows Server-Systemstatus aus einer Sicherung in Azure.
ms.topic: conceptual
ms.date: 06/30/2020
ms.openlocfilehash: 727268b2dc9473a6bda79f023e8a75202d43afe4
ms.sourcegitcommit: 64ad2c8effa70506591b88abaa8836d64621e166
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88263434"
---
# <a name="restore-system-state-to-windows-server"></a>Wiederherstellen des Systemstatus von Windows Server

In diesem Artikel wird erläutert, wie Sicherungen des Windows Server-Systemstatus aus einem Azure Recovery Services-Tresor wiederhergestellt werden. Um den Systemstatus wiederherstellen zu können, benötigen Sie eine Sicherung des Systemstatus (erstellt mit der Anleitung unter [Sichern des Systemstatus](backup-azure-system-state.md#back-up-windows-server-system-state)). Stellen Sie außerdem sicher, dass die [neueste Version des Microsoft Azure Recovery Services-Agents (MARS)](https://aka.ms/azurebackup_agent) installiert ist. Das Wiederherstellen von Daten des Windows Server-Systemstatus aus einem Azure Recovery Services-Tresor ist ein zweistufiger Prozess:

1. Stellen Sie den Systemstatus als Dateien aus Azure Backup wieder her. Beim Wiederherstellen des Systemstatus als Dateien aus Azure Backup haben Sie folgende Möglichkeiten:
   * Wiederherstellen des Systemstatus auf dem gleichen Server, auf dem die Sicherungen erstellt wurden, oder
   * Stellen Sie die Systemstatusdatei auf einem anderen Server wieder her.

2. Wenden Sie die wiederhergestellten Systemstatusdateien auf eine Windows Server-Instanz an, indem Sie das Hilfsprogramm für die Windows Server-Sicherung verwenden.

## <a name="recover-system-state-files-to-the-same-server"></a>Wiederherstellen der Systemstatusdateien auf dem gleichen Server

Die folgenden Schritte erläutern das Ausführen eines Rollbacks Ihrer Windows Server-Konfiguration zu einem früheren Zustand. Ein Rollback Ihrer Serverkonfiguration zu einem bekannten, stabilen Zustand kann sehr hilfreich sein. Mit den folgenden Schritten stellen Sie den Systemstatus des Servers aus einem Recovery Services-Tresor wieder her.

1. Öffnen Sie das Snap-In **Microsoft Azure Backup**. Falls Sie nicht wissen, wo das Snap-In installiert wurde, können Sie den für **Microsoft Azure Backup** verwendeten Computer oder Server durchsuchen.

    Die Desktop-App sollten in den Suchergebnissen angezeigt werden.

2. Wählen Sie **Daten wiederherstellen** aus, um den Assistenten zu starten.

    ![Daten wiederherstellen](./media/backup-azure-restore-windows-server/recover.png)

3. Wählen Sie im Bereich **Erste Schritte** die Option **Dieser Server (`<server name>`)** , um die Daten auf demselben Server oder Computer wiederherzustellen, und wählen Sie **Weiter** aus.

    ![Auswählen der Option „Dieser Server“ zum Wiederherstellen der Daten auf dem gleichen Computer](./media/backup-azure-restore-system-state/samemachine.png)

4. Wählen Sie im Bereich **Wiederherstellungsmodus auswählen** die Option **Systemstatus** aus, und wählen Sie dann **Weiter** aus.

    ![Dateien durchsuchen](./media/backup-azure-restore-system-state/recover-type-selection.png)

5. Wählen Sie im Kalender im Bereich **Volume und Datum auswählen** einen Wiederherstellungspunkt aus.

    Sie können den Zustand eines beliebigen Wiederherstellungszeitpunkts wiederherstellen. Wenn Datumsangaben in **Fettdruck** angezeigt werden, ist dies ein Hinweis darauf, dass mindestens ein Wiederherstellungszeitpunkt verfügbar ist. Falls mehrere Wiederherstellungspunkte verfügbar sind, müssen Sie nach dem Auswählen eines Datums im Dropdownmenü **Uhrzeit** den jeweiligen Wiederherstellungspunkt auswählen.

    ![Volume und Datum](./media/backup-azure-restore-system-state/select-date.png)

6. Wählen Sie **Weiter** aus, nachdem Sie den Wiederherstellungspunkt ausgewählt haben, der wiederhergestellt werden soll.

    Azure Backup stellt den lokalen Wiederherstellungspunkt bereit und verwendet ihn als Wiederherstellungsvolume.

7. Geben Sie im nächsten Bereich das Ziel für die wiederhergestellten Systemstatusdateien an. Wählen Sie anschließend **Durchsuchen** aus, um den Windows-Explorer zu öffnen und nach den gewünschten Dateien und Ordnern zu suchen. Die Option **Kopien erstellen, sodass Sie über beide Versionen verfüge** erstellt Kopien der einzelnen Dateien in einem vorhandenen Systemstatus-Dateiarchiv, anstatt eine Kopie des gesamten Systemstatusarchivs zu erstellen.

    ![Wiederherstellungsoptionen](./media/backup-azure-restore-system-state/recover-as-files.png)

8. Überprüfen Sie die Details der Wiederherstellung im Bereich **Bestätigung**, und wählen Sie dann **Wiederherstellen** aus.

   ![Auf „Wiederherstellen“ klicken, um die Wiederherstellungsaktion zu bestätigen](./media/backup-azure-restore-system-state/confirm-recovery.png)

9. Kopieren Sie das Verzeichnis *WindowsImageBackup* am Wiederherstellungsziel auf ein weniger wichtiges Volume des Servers. Normalerweise ist das Windows-Betriebssystemvolume das wichtigste Volume.

10. Nachdem die Wiederherstellung erfolgreich abgeschlossen wurde, führen Sie die Schritte im Abschnitt [Anwenden der wiederhergestellten Systemstatusdateien auf Windows Server](#apply-restored-system-state-on-a-windows-server) aus, um den Wiederherstellungsprozess für den Systemstatus abzuschließen.

## <a name="recover-system-state-files-to-an-alternate-server"></a>Wiederherstellen von Systemstatusdateien auf einem anderen Server

Wenn Windows Server beschädigt ist oder darauf nicht zugegriffen werden kann und Sie das System durch das Wiederherstellen des Systemstatus von Windows Server wieder in einen stabilen Zustand bringen möchten, können Sie auf einem beschädigten System den Systemstatus von einem anderen Server wiederherstellen. Gehen Sie anhand der folgenden Schritte vor, um den Systemstatus auf einem anderen Server wiederherzustellen.  

In diesen Schritten wird folgende Terminologie verwendet:

* *Quellcomputer* – Der ursprüngliche Computer, von dem die Sicherung erstellt wurde und der zurzeit nicht verfügbar ist.
* *Zielcomputer* – Der Computer, auf dem die Daten wiederhergestellt werden.
* *Beispieltresor* – Der Recovery Services-Tresor, bei dem der *Quellcomputer* und der *Zielcomputer* registriert sind.

> [!NOTE]
> Sicherungen von einem Computer können nicht auf einem Computer wiederhergestellt werden, auf dem eine frühere Version des Betriebssystems ausgeführt wird. Beispielsweise können Sicherungen von einem Windows Server 2016-Computer nicht unter Windows Server 2012 R2 wiederhergestellt werden. In umgekehrter Richtung ist dies jedoch möglich. Sie können Sicherungen von Windows Server 2012 R2 zum Wiederherstellen von Windows Server 2016 verwenden.
>

1. Öffnen Sie das Snap-In **Microsoft Azure Backup** auf dem *Zielcomputer*.
2. Stellen Sie sicher, dass der *Zielcomputer* und der *Quellcomputer* bei dem gleichen Recovery Services-Tresor registriert sind.
3. Wählen Sie **Daten wiederherstellen** aus, um den Workflow zu initiieren.
4. Wählen Sie **Anderer Server**

    ![Anderer Server](./media/backup-azure-restore-system-state/anotherserver.png)

5. Geben Sie die Datei mit den Tresoranmeldeinformationen an, die dem *Beispieltresor*entspricht. Wenn die Datei mit den Tresoranmeldeinformationen ungültig (oder abgelaufen) ist, laden Sie eine neue Anmeldeinformationsdatei für den Tresor aus dem *Beispieltresor* im Azure-Portal herunter. Sobald die Anmeldeinformationsdatei für den Tresor bereitgestellt wurde, wird der Recovery Services-Tresor angezeigt, der der Anmeldeinformationsdatei für den Tresor zugeordnet ist.

6. Wählen Sie im Bereich „Sicherungsserver auswählen“ in der Liste mit den angezeigten Computern den *Quellcomputer* aus.
7. Wählen Sie im Bereich „Wiederherstellungsmodus auswählen“ die Option **Systemstatus** aus, und wählen Sie dann **Weiter** aus.

    ![Suchen,](./media/backup-azure-restore-system-state/recover-type-selection.png)

8. Wählen Sie im Kalender im Bereich **Volume und Datum auswählen** einen Wiederherstellungspunkt aus. Sie können den Zustand eines beliebigen Wiederherstellungszeitpunkts wiederherstellen. Wenn Datumsangaben in **Fettdruck** angezeigt werden, ist dies ein Hinweis darauf, dass mindestens ein Wiederherstellungszeitpunkt verfügbar ist. Falls mehrere Wiederherstellungspunkte verfügbar sind, müssen Sie nach dem Auswählen eines Datums im Dropdownmenü **Uhrzeit** den jeweiligen Wiederherstellungspunkt auswählen.

    ![Elemente suchen](./media/backup-azure-restore-system-state/select-date.png)

9. Wählen Sie **Weiter** aus, nachdem Sie den Wiederherstellungspunkt ausgewählt haben, der wiederhergestellt werden soll.

10. Geben Sie im Bereich **Modus für die Systemstatuswiederherstellung auswählen** das Ziel für die Wiederherstellung der Systemstatusdateien an, und wählen Sie dann **Weiter** aus.

    ![Verschlüsselung](./media/backup-azure-restore-system-state/recover-as-files.png)

    Die Option **Kopien erstellen, sodass Sie über beide Versionen verfüge** erstellt Kopien der einzelnen Dateien in einem vorhandenen Systemstatus-Dateiarchiv, anstatt eine Kopie des gesamten Systemstatusarchivs zu erstellen.

11. Überprüfen Sie die Details der Wiederherstellung im Bereich „Bestätigung“, und wählen Sie dann **Wiederherstellen** aus.

    ![Auf die Schaltfläche „Wiederherstellen“ klicken, um den Wiederherstellungsprozess zu bestätigen](./media/backup-azure-restore-system-state/confirm-recovery.png)

12. Kopieren Sie das Verzeichnis *WindowsImageBackup* auf ein weniger wichtiges Volume des Servers (z.B. „D:\)“. Normalerweise ist das Windows-Betriebssystemvolume das wichtigste Volume.

13. Um den Wiederherstellungsprozess abzuschließen, befolgen Sie die Schritte im folgenden Abschnitt zum [Anwenden der wiederhergestellten Systemstatusdateien auf Windows Server](#apply-restored-system-state-on-a-windows-server).

## <a name="apply-restored-system-state-on-a-windows-server"></a>Anwenden der wiederhergestellten Systemstatusdateien auf Windows Server

Nachdem Sie mit dem Azure Recovery Services-Agent den Systemstatus als Dateien wiederhergestellt haben, wenden Sie den wiederhergestellten Systemstatus mithilfe des Hilfsprogramms Windows Server-Sicherung auf Windows Server an. Das Hilfsprogramm Windows Server-Sicherung ist bereits auf dem Server verfügbar. Die folgenden Schritte erläutern die Anwendung des wiederhergestellten Systemstatus.

1. Öffnen Sie das Snap-In „Windows Server-Sicherung“. Falls Sie nicht wissen, wo das Snap-In installiert wurde, können Sie den für **Windows Server-Sicherung** verwendeten Computer oder Server durchsuchen.

    Die Desktop-App wird in den Suchergebnissen angezeigt. Wenn sie nicht angezeigt wird oder beim Öffnen der Anwendung Fehler auftreten, müssen Sie die **Windows Server-Sicherungsfeatures** und davon abhängige Komponenten installieren, die in **Server-Manager** im **Assistenten zum Hinzufügen von Features** verfügbar sind.

1. Wählen Sie im Snap-In **Lokale Sicherung** aus.

    ![„Lokale Sicherung“ für die Wiederherstellung auswählen](./media/backup-azure-restore-system-state/win-server-backup-local-backup.png)

1. Wählen Sie in der Konsole „Lokale Sicherung“ im **Aktionsbereich** die Option **Wiederherstellen** aus, um den Wiederherstellungs-Assistenten zu öffnen.

1. Wählen Sie die Option **Eine an einem anderen Speicherort gespeicherte Sicherung** aus, und wählen Sie **Weiter**.

   ![Wiederherstellung auf einem anderen Server auswählen](./media/backup-azure-restore-system-state/backup-stored-in-diff-location.png)

1. Wenn Sie den Speicherort angeben, wählen Sie **Freigegebener Remoteordner** aus, wenn die Sicherung des Systemstatus auf einem anderen Server wiederhergestellt wurde. Wenn der Systemstatus lokal wiederhergestellt wurde, wählen Sie **Lokale Datenträger** aus.

    ![Wiederherstellung vom lokalen oder einem anderen Server auswählen](./media/backup-azure-restore-system-state/ss-recovery-remote-shared-folder.png)

1. Geben Sie den Pfad zum Verzeichnis *WindowsImageBackup* ein, oder wählen Sie das lokale Laufwerk mit diesem Verzeichnis aus (z. B. „D:\WindowsImageBackup“), das bei der Wiederherstellung der Systemstatusdateien mithilfe des Azure Recovery Services-Agents wiederhergestellt wurde, und wählen Sie **Weiter**.

    ![Pfad zur freigegebenen Datei](./media/backup-azure-restore-system-state/ss-recovery-remote-folder.png)

1. Wählen Sie die Systemstatusversion aus, die Sie wiederherstellen möchten, und wählen Sie **Weiter**.

1. Wählen Sie im Bereich „Wiederherstellungsmodus auswählen“ die Option **Systemstatus** aus, und wählen Sie **Weiter**.

1. Wählen Sie als Speicherort für die Systemstatuswiederherstellung **Ursprünglicher Speicherort** aus, und wählen Sie **Weiter**.

    Wenn Sie einen Domänencontroller wiederherstellen, wird die folgende zusätzliche Option angezeigt:

    ![Speicherort für die Systemstatuswiederherstellung](./media/backup-azure-restore-system-state/location-for-system-state-recovery.png)

    >[!NOTE]
    >Wählen Sie die Option „Perform an authoritative restore of Active Directory files“ (Autoritative Wiederherstellung von Active Directory-Dateien durchführen) nur aus, wenn Sie explizit eine autoritative Wiederherstellung aller Active Directory-Daten durchführen möchten.

1. Überprüfen Sie die Bestätigungsdetails und die Neustarteinstellungen, und wählen Sie **Wiederherstellen** aus, um die wiederhergestellten Systemstatusdateien anzuwenden.

    ![Wiederherstellung der Systemstatusdateien starten](./media/backup-azure-restore-system-state/launch-ss-recovery.png)

    >[!NOTE]
    >Aktivieren Sie die Option **Automatically reboot the server** (Server automatisch neu starten) nicht, wenn Sie die Wiederherstellung im DSRM-Modus durchführen.

1. Nachdem Sie eine Wiederherstellung erfolgreich abgeschlossen haben, müssen Sie den Server im normalen Modus neu starten. Öffnen Sie eine Eingabeaufforderung, und geben Sie Folgendes ein: `bcdedit /deletevalue safeboot`
1. Starten Sie den Server neu.

## <a name="special-considerations-for-system-state-recovery-on-a-domain-controller"></a>Besondere Überlegungen bei der Wiederherstellung des Systemstatus auf einem Domänencontroller

Sicherungen des Systemstatus umfassen auch Active Directory-Daten. Anhand der folgenden Schritte können Sie Active Directory Domain Services (AD DS) vom aktuellen Zustand in einen früheren Zustand wiederherstellen. Für diese Art der Wiederherstellung gibt es zwei Vorgehensweisen:

* Wiederherstellen aller Active Directory-Daten, wenn in der Gesamtstruktur keine funktionierenden Domänencontroller vorhanden sind
* Wiederherstellen eines Teils der Active Directory-Daten, wenn diese Objekte gelöscht oder beschädigt wurden

In diesem Artikel wird nur das erste Szenario beschrieben, bei dem eine nicht autoritative Wiederherstellung von AD DS und eine autoritative Wiederherstellung des Ordners „sysvol“ durchgeführt wird.  Falls Sie das zweite Szenario durchführen möchten (bei dem die Domänencontroller noch funktionieren, aber bestimmte AD-Objekte wiederhergestellt werden müssen), hilft Ihnen [diese Anleitung](https://support.microsoft.com/help/840001/how-to-restore-deleted-user-accounts-and-their-group-memberships-in-ac) weiter.

1. Führen Sie die weiter unten beschriebenen Schritte zum [Wiederherstellen von Systemstatusdateien auf einem anderen Server](#recover-system-state-files-to-an-alternate-server) aus.
1. Verwenden Sie die folgenden Befehle, um den Server im *Reparaturmodus für Verzeichnisdienste* neu zu starten. Führen Sie an einer Eingabeaufforderung mit erhöhten Rechten Folgendes durch:

    ```cmd
    Bcdedit /set safeboot dsrepair
    Shutdown /r /t 0
    ```

1. Sie können eine der beiden obigen Methoden auswählen, um Active Directory im Rahmen einer Systemstatuswiederherstellung wiederherzustellen:

    * Befolgen Sie die obige Anleitung zum [Anwenden der wiederhergestellten Systemstatusdateien auf Windows Server](#apply-restored-system-state-on-a-windows-server) mit dem Hilfsprogramm „Windows Server-Sicherung“.

        >[!NOTE]
        >Wenn Sie alle Active Directory-Daten wiederherstellen (und in der Gesamtstruktur keine funktionierenden Domänencontroller mehr vorhanden sind), sollten Sie in Schritt 9 oben darauf achten, dass Sie die Option **Perform an authoritative restore of Active Directory files** (Autoritative Wiederherstellung von Active Directory-Dateien durchführen) aktivieren.

    * Verwenden Sie das Hilfsprogramm [wbadmin](/windows-server/administration/windows-commands/wbadmin-start-systemstaterecovery), um die Wiederherstellung über die Befehlszeile durchzuführen.

        Sie benötigen den Versionsbezeichner der Sicherung, die Sie verwenden möchten. Sie können eine Liste mit den Versionsbezeichnern abrufen, indem Sie diesen Befehl ausführen:

        ```cmd
        wbadmin get versions -backuptarget <servername\sharename>
        ```

        Anschließend verwenden Sie diesen Versionsbezeichner, um die Wiederherstellung durchzuführen.

        Geben Sie beispielsweise Folgendes ein, um eine [nicht autoritative Wiederherstellung von AD DS und eine autoritative Wiederherstellung des Ordners „sysvol“](/windows-server/identity/ad-ds/manage/ad-forest-recovery-nonauthoritative-restore) durchzuführen, indem Sie die Sicherung vom 30.04.2020 um 9:00 Uhr verwenden, die auf der freigegebenen Ressource `\\servername\share` für `server01` gespeichert ist:

        ```cmd
        wbadmin start systemstaterecovery -version:04/30/2020-09:00 -backupTarget:\\servername\share -machine:server01 -authsysvol
        ```

1. Nachdem Sie eine Wiederherstellung erfolgreich abgeschlossen haben, sollten Sie den Server im normalen Modus neu starten. Öffnen Sie eine Eingabeaufforderung, und geben Sie Folgendes ein: `bcdedit /deletevalue safeboot`
1. Starten Sie den Server neu.

## <a name="troubleshoot-failed-system-state-restore"></a>Wiederherstellen nach einer nicht erfolgreichen Problembehandlung des Systemstatus

Wenn der vorherige Vorgang zum Anwenden des Systemstatus nicht erfolgreich abgeschlossen wurde, verwenden Sie die Windows-Wiederherstellungsumgebung (Windows Recovery Environment, Windows RE), um Windows Server wiederherzustellen. In den folgenden Schritten wird die Wiederherstellung mit Win RE erläutert. Verwenden Sie diese Option nur, wenn Windows Server nach einer Wiederherstellung des Systemstatus nicht normal gestartet wird. Beim folgenden Prozess werden nicht zum System gehörende Daten gelöscht. Seien Sie deshalb äußerst vorsichtig.

1. Starten Sie Windows Server mit der Windows-Wiederherstellungsumgebung (Windows RE).

2. Wählen Sie aus den drei verfügbaren Optionen die Problembehandlung aus.

    ![Auswählen der Problembehandlung](./media/backup-azure-restore-system-state/winre-1.png)

3. Wählen Sie auf dem Bildschirm **Erweiterte Optionen** die Option **Eingabeaufforderung** aus, und geben Sie den Benutzernamen und das Kennwort des Serveradministrators an.

   ![Auswählen der Eingabeaufforderung](./media/backup-azure-restore-system-state/winre-2.png)

4. Geben Sie den Benutzernamen und das Kennwort des Serveradministrators an.

    ![Kennwort eingeben](./media/backup-azure-restore-system-state/winre-3.png)

5. Wenn Sie die Eingabeaufforderung im Administratormodus öffnen, führen Sie folgenden Befehl aus, um die Versionen der Systemstatussicherung abzurufen.

    ```cmd
    Wbadmin get versions -backuptarget:<Volume where WindowsImageBackup folder is copied>:
    ```

    ![Abrufen von Versionen der Systemstatussicherung](./media/backup-azure-restore-system-state/winre-4.png)

6. Führen Sie den folgenden Befehl aus, um alle in der Sicherung verfügbaren Volumes abzurufen.

    ```cmd
    Wbadmin get items -version:<copy version from above step> -backuptarget:<Backup volume>
    ```

    ![Abrufen aller verfügbaren Volumes](./media/backup-azure-restore-system-state/winre-5.png)

7. Der folgende Befehl stellt alle Volumes, die Teil der Systemstatussicherung sind, wieder her. Beachten Sie, dass bei diesem Schritt nur die wichtigen Volumes wiederhergestellt werden, die Teil des Systemstatus sind. Alle nicht zum System gehörenden Daten werden gelöscht.

    ```cmd
    Wbadmin start recovery -items:C: -itemtype:Volume -version:<Backupversion> -backuptarget:<backup target volume>
    ```

     ![Wiederherstellen aller Volumes](./media/backup-azure-restore-system-state/winre-6.png)

## <a name="next-steps"></a>Nächste Schritte

* Nachdem Sie nun Ihre Dateien und Ordner wiederhergestellt haben, können Sie [Ihre Sicherungen verwalten](backup-azure-manage-windows-server.md).
