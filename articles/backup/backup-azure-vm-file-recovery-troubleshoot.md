---
title: Problembehandlung bei der Azure VM-Dateiwiederherstellung
description: Hier erhalten Sie Informationen über das Behandeln von Problemen bei der Wiederherstellung von Dateien und Ordnern aus einer Azure VM-Sicherung.
ms.reviewer: srinathv
ms.topic: troubleshooting
ms.date: 07/12/2020
ms.openlocfilehash: 96876415405cc5099b4d9ca209c36086b793d6e0
ms.sourcegitcommit: 63d0621404375d4ac64055f1df4177dfad3d6de6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/15/2020
ms.locfileid: "97512069"
---
# <a name="troubleshooting-issues-in-file-recovery-of-azure-vm-backup"></a>Behandlung von Problemen bei der Dateiwiederherstellung aus einer Azure VM-Sicherung

Dieser Artikel enthält Schritte zur Problembehandlung, die Ihnen helfen können, Azure Backup-Fehler im Zusammenhang mit Problemen beim Wiederherstellen von Dateien und Ordnern aus einer Azure VM-Sicherung zu beheben. 

## <a name="exception-caught-while-connecting-to-target"></a>Ausnahme beim Herstellen einer Verbindung mit dem Ziel

Mögliche Ursache: Das Skript kann auf die empfohlene Aktion für den Wiederherstellungspunkt nicht zugreifen: Überprüfen Sie, ob der Computer alle [Zugriffsanforderungen](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm#step-4-access-requirements-to-successfully-run-the-script) erfüllt.

## <a name="the-target-has-already-been-logged-in-via-an-iscsi-session"></a>Das Ziel wurde bereits über eine iSCSI-Sitzung angemeldet.

Mögliche Ursache: Das Skript wurde bereits auf demselben Computer ausgeführt, und die Laufwerke wurden angefügt.
Empfohlene Maßnahme: Die Volumes des Wiederherstellungspunkts wurden bereits angefügt. Sie wurden ggf. nicht mit denselben Laufwerkbuchstaben des ursprünglichen virtuellen Computers eingebunden. Durchsuchen Sie im Datei-Explorer alle verfügbaren Volumes.

## <a name="this-script-is-invalid-because-the-disks-have-been-dismounted-via-portalexceeded-the-12-hr-limit-download-a-new-script-from-the-portal"></a>Dieses Skript ist ungültig, da die Bereitstellung der Datenträger über das Portal aufgehoben bzw. das 12-Stunden-Limit überschritten wurde. Herunterladen eines neuen Skripts aus dem Portal

Mögliche Ursache: Die Bereitstellung der Datenträger über das Portal wurde aufgehoben, oder das 12-Stunden-Limit wurde überschritten.
Empfohlene Maßnahme: Das Skript wird 12 Stunden nach dem Zeitpunkt des Downloads ungültig und kann nicht mehr ausgeführt werden. Besuchen Sie das Portal, und laden Sie ein neues Skript herunter, um die Dateiwiederherstellung fortzusetzen.

## <a name="troubleshooting-common-issues"></a>Behandlung häufig auftretender Probleme

Dieser Abschnitt enthält Schritte zum Beheben von Problemen, die beim Herunterladen und Ausführen des Skripts für Dateiwiederherstellung aufgetreten sind.

## <a name="cannot-download-the-script"></a>Das Skript kann nicht heruntergeladen werden.

Empfohlene Maßnahme:

1. Sorgen Sie dafür, dass Sie alle [erforderlichen Berechtigungen zum Herunterladen des Skripts](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm#select-recovery-point-who-can-generate-script) haben.
2. Vergewissern Sie sich, dass eine Verbindung mit Azure-Ziel-IP(s) besteht.
Führen Sie zum Überprüfen der Verbindung den folgenden Befehl in einer Eingabeaufforderung mit erhöhten Rechten aus. 
    - *nslookup download.microsoft.com* oder
    - *ping download.microsoft.com*

## <a name="the-script-downloads-successfully-but-fails-to-run"></a>Das Skript wird erfolgreich heruntergeladen, aber bei der Ausführung tritt ein Fehler auf.

### <a name="for-sles-12-sp4-os-linux"></a>Bei SLES 12 SP4 OS (Linux)

Fehler: „iscsi_tcp_module“ nicht gefunden

Mögliche Ursache: Während der Ausführung des Python-Skripts für die Wiederherstellung auf Elementebene unter der SUSE Linux-Betriebssystemversion 12sp4 tritt ein Fehler des Typs ***iscsi_tcp-Modul kann nicht geladen werden** _ auf. Das ILR-Modul verwendet „iscsi_tcp“ zum Herstellen einer TCP-Verbindung mit dem Sicherungsdienst, und SUSE als Teil der Version 12SP4 hat „iscsi_tcp“ aus dem „Open-iSCSI“-Paket entfernt. Deshalb schlägt der ILR-Vorgang fehl.

Empfohlene Maßnahme:  Weil die Ausführung des Skripts zur Dateiwiederherstellung auf virtuellen Computern unter SUSE 12sp4 nicht unterstützt wird, probieren Sie den Wiederherstellungsvorgang auf einer älteren Version von SUSE 12sp4 aus.

## <a name="the-script-runs-but-connection-to-iscsi-target-failed"></a>Das Skript wird ausgeführt, aber bei der Verbindung zum iSCSI-Ziel ist ein Fehler aufgetreten.

Error: Ausnahme beim Herstellen einer Verbindung mit dem Ziel

1. Stellen Sie sicher, dass der Computer, auf dem das Skript ausgeführt wird, alle [Zugriffsanforderungen](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm#step-4-access-requirements-to-successfully-run-the-script) erfüllt.
2. Überprüfen Sie die Konnektivität mit Azure-Ziel-IP(s).
Führen Sie zum Überprüfen der Verbindung den folgenden Befehl in einer Eingabeaufforderung mit erhöhten Rechten aus. 
    - _„nslookup download.microsoft.com“* oder<br>
    - *ping download.microsoft.com*
3. Stellen Sie sicher, dass auf den ausgehenden iSCSI-Port 3260 zugegriffen werden kann.
4. Stellen Sie sicher, dass es keine Firewall/NSG gibt, die den Datenverkehr an Azure-Ziel-IP(s) oder Wiederherstellungsdienst-URLs blockiert.
5. Überprüfen Sie, ob Antivirus die Ausführung des Skripts blockiert.

## <a name="connected-to-recovery-point-but-disks-did-not-get-attached"></a>Mit dem Wiederherstellungspunkt verbunden, aber Datenträger wurden nicht angefügt.

Sorgen Sie dafür, dass Sie den [richtigen Computer zum Ausführen des Skripts](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm#step-2-ensure-the-machine-meets-the-requirements-before-executing-the-script) haben.

### <a name="on-windows-vm"></a>Auf Windows-VM

**Der Speicherpool auf der VM wird im schreibgeschützten Modus angefügt**. Unter Windows 2012 R2 und Windows 2016 (mit dem Speicherpool) wechselt der an die VM angefügte Speicherpool während der erstmaligen Ausführung des Skripts möglicherweise in den schreibgeschützten Status.

Zur Behebung dieses Problems müssen Sie den Lese-/Schreibzugriff auf den Speicherpool zum ersten Mal manuell festlegen und die virtuellen Datenträger anfügen. Führen Sie dazu die folgenden Schritte aus:

1. Legen Sie „Lese-/Schreibzugriff“ fest.
Navigieren Sie zu „Server-Manager“ > „Datei- und Speicherdienste“ > „Volumes“ > „Speicherpools“.

   ![Windows Storage](./media/backup-azure-restore-files-from-vm/windows-storage-1.png)

2. Klicken Sie im Fenster **Speicherpool** mit der rechten Maustaste auf den verfügbaren Speicherpool, und wählen Sie **Lese-/Schreibzugriff festlegen** aus.

   ![Lese-/Schreibzugriff unter Windows Storage](./media/backup-azure-restore-files-from-vm/windows-storage-read-write-2.png)

3. Nachdem der Speicherpool mit Lese-/Schreibzugriff zugewiesen wurde, müssen Sie den virtuellen Datenträger anfügen.
Navigieren Sie zur **Server-Manager-Benutzeroberfläche**, und klicken Sie im Abschnitt „Virtuelle Datenträger“ > mit der rechten Maustaste, um die Option **Virtuellen Datenträger anfügen** auszuwählen.

   ![Virtueller Datenträger des Server-Managers](./media/backup-azure-restore-files-from-vm/server-manager-virtual-disk-3.png)

### <a name="on-linux-vm"></a>Auf einer Linux-VM

#### <a name="file-recovery-fails-to-auto-mount-because-disk-does-not-contain-volumes"></a>Die Dateiwiederherstellung kann nicht automatisch eingebunden werden, weil der Datenträger keine Volumes enthält.

Während der Ausführung der Dateiwiederherstellung erkennt der Sicherungsdienst Volumes und automatische Einbindungen. Wenn die gesicherten Datenträger aber unformatierte Partitionen enthalten, werden diese Datenträger nicht automatisch eingebunden, und der Datenträger für die Wiederherstellung wird nicht angezeigt.

Führen Sie zur Behebung dieses Problems die Schritte in [diesem Artikel](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm#lvmraid-arrays-for-linux-vms) aus.
 
#### <a name="os-could-not-identify-the-filesystem-causing-linux-file-recovery-to-fail-while-mountings-disks"></a>Das Betriebssystem konnte das Betriebssystem nicht identifizieren, wodurch die Linux-Dateiwiederherstellung während des Einbindens von Datenträgern fehlschlug.

Während der Ausführung des Skripts zur Dateiwiederherstellung trat beim Anfügen des Datenträgers dieser Fehler auf: *Die folgenden Partitionen konnten nicht eingebunden werden, weil das Betriebssystem das Dateisystem nicht identifizieren konnte.*

- Überprüfen Sie zur Behebung dieses Problems, ob das Volume mit einer Drittanbieteranwendung verschlüsselt wurde. Wenn es verschlüsselt wurde, wird der Datenträger oder die VM im Portal nicht als verschlüsselt angezeigt.
1. Melden Sie sich bei der gesicherten VM an, und führen Sie den folgenden Befehl aus: *lsblk -f*<br>

   ![Datenträger ohne Volume](./media/backup-azure-restore-files-from-vm/disk-without-volume-5.png)

2. Überprüfen Sie das Dateisystem und die Verschlüsselung.
3. Wenn das Volume verschlüsselt wurde, wird die Dateiwiederherstellung nicht unterstützt. [Weitere Informationen](https://docs.microsoft.com/azure/backup/backup-support-matrix-iaas#support-for-file-level-restore)

## <a name="disks-are-attached-but-unable-to-mount-volumes"></a>Datenträger werden angefügt, können aber keine Volumes einbinden

- Sorgen Sie dafür, dass Sie den [richtigen Computer zum Ausführen des Skripts](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm#step-2-ensure-the-machine-meets-the-requirements-before-executing-the-script) haben.

### <a name="on-windows-vm"></a>Auf Windows-VM

Während der Ausführung des Dateiwiederherstellungsskripts für Windows wird eine Meldung angezeigt, dass ***0 Volumes angefügt werden** _, aber die Datenträger werden in der Datenträgerverwaltungskonsole erkannt. Beim Anfügen von Volumes über iSCSI wechseln einige erkannte Volumes in den Offlinestatus. Wenn der iSCSI-Kanal zwischen dem virtuellen Computer und dem Dienst kommuniziert, erkennt er diese Volumes und schaltet sie online, aber sie werden nicht eingebunden.

   ![Datenträger nicht angefügt](./media/backup-azure-restore-files-from-vm/disk-not-attached-6.png)

Führen Sie zum Identifizieren und Beheben dieses Problems die folgenden Schritte aus:

1. Wechseln Sie zu _ *Datenträgerverwaltung**, indem Sie im Befehlsfenster den Befehl **diskmgmt** ausführen.
2. Überprüfen Sie, ob zusätzliche Datenträger angezeigt werden. Im folgenden Beispiel ist „Datenträger 2“ ein zusätzlicher Datenträger.

   ![Datenträgerverwaltung0](./media/backup-azure-restore-files-from-vm/disk-management-7.png)

3. Klicken Sie mit der rechten Maustaste auf den **Datenträger**, und wählen Sie **Laufwerkbuchstaben und -pfade ändern** aus.

   ![Datenträgerverwaltung1](./media/backup-azure-restore-files-from-vm/disk-management-8.png)

4. Wählen Sie im Fenster **Laufwerkbuchstabe oder -pfad hinzufügen** die Option **Folgenden Laufwerkbuchstaben zuweisen** aus, weisen Sie ein verfügbares Laufwerk zu, und klicken Sie auf **OK**. 

   ![Datenträgerverwaltung2](./media/backup-azure-restore-files-from-vm/disk-management-9.png)

5. Sie können das Laufwerk im Datei-Explorer anzeigen.

   ![Datenträgerverwaltung3](./media/backup-azure-restore-files-from-vm/disk-management-10.png)

6. Die neuen Volumes sollten angefügt werden.  

   ![Datenträger wird nicht eingebunden](./media/backup-azure-restore-files-from-vm/disk-not-mounting-11.png)

7. Nach dem erneuten Ausführen des Skripts werden die neuen Volumes im Datei-Explorer angezeigt.

   ![Datenträger wird nicht eingebunden1](./media/backup-azure-restore-files-from-vm/disk-not-mounting-12.png)

   ![Datenträger wird nicht eingebunden2](./media/backup-azure-restore-files-from-vm/disk-not-mounting-13.png)

#### <a name="on-linux-vms"></a>Auf Linux-VMs 

- Sorgen Sie dafür, dass Sie den [richtigen Computer zum Ausführen des Skripts](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm#step-2-ensure-the-machine-meets-the-requirements-before-executing-the-script) haben.
- Wenn der geschützte virtuelle Linux-Computer LVM- und/oder RAID-Arrays verwendet, führen Sie die in diesem [Artikel](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm#lvmraid-arrays-for-linux-vms) aufgeführten Schritte aus.

## <a name="cannot-copy-the-files-from-mounted-volumes"></a>Die Dateien aus eingebundenen Volumes können nicht kopiert werden.

Die Kopie schlägt möglicherweise mit dem Fehler „0x80070780“ fehl: Das System kann auf die Datei nicht zugreifen. Überprüfen Sie in diesem Fall, ob beim Quellserver die Datenträgerdeduplizierung aktiviert wurde. Vergewissern Sie sich anschließend, dass beim Wiederherstellungsserver die Deduplizierung auf den Laufwerken ebenfalls aktiviert wurde. Sie können die Deduplizierungsrolle unkonfiguriert beibehalten, sodass Sie die Laufwerke auf dem Wiederherstellungsserver nicht deduplizieren.
