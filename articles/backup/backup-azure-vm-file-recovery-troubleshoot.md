---
title: Problembehandlung bei der Azure VM-Dateiwiederherstellung
description: Erfahren Sie, wie Sie Probleme bei der Wiederherstellung von Dateien und Ordnern aus einer Azure-VM-Sicherung beheben.
ms.topic: troubleshooting
ms.date: 07/12/2020
ms.openlocfilehash: c4d0d233237cb477d72efea0b91d4e5288e2a302
ms.sourcegitcommit: 78ecfbc831405e8d0f932c9aafcdf59589f81978
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2021
ms.locfileid: "98735876"
---
# <a name="troubleshoot-issues-in-file-recovery-of-an-azure-vm-backup"></a>Behandlung von Problemen bei der Dateiwiederherstellung aus einer Azure VM-Sicherung

Dieser Artikel enthält Schritte zur Problembehandlung, die Ihnen helfen können, Probleme beim Wiederherstellen von Dateien und Ordnern aus einer Azure-VM-Sicherung (Virtual Machine, virtueller Computer) zu beheben.

## <a name="common-error-messages"></a>Häufige Fehlermeldungen

Dieser Abschnitt enthält Schritte zum Beheben von Problemen, wenn Fehlermeldungen auftreten.

### <a name="exception-caught-while-connecting-to-target"></a>„Ausnahme beim Herstellen einer Verbindung mit dem Ziel“

**Mögliche Ursache:** Das Skript kann nicht auf den Wiederherstellungspunkt zugreifen.

**Empfohlene Maßnahme:** Um dieses Problem zu beheben, führen Sie die Schritte unter [Das Skript wird ausgeführt, aber bei der Verbindung zum iSCSI-Ziel ist ein Fehler aufgetreten](#the-script-runs-but-the-connection-to-the-iscsi-target-failed) aus.

### <a name="the-target-has-already-been-logged-in-via-an-iscsi-session"></a>„Das Ziel wurde bereits über eine iSCSI-Sitzung angemeldet“

**Mögliche Ursache:** Das Skript wurde bereits auf demselben Computer ausgeführt, und die Laufwerke wurden angefügt.

**Empfohlene Maßnahme:** Die Volumes des Wiederherstellungspunkts wurden bereits angefügt. Sie können nicht mit denselben Laufwerkbuchstaben des ursprünglichen virtuellen Computers eingebunden werden. Durchsuchen Sie im Datei-Explorer die verfügbaren Volumes.

### <a name="this-script-is-invalid-because-the-disks-have-been-dismounted-via-portalexceeded-the-12-hr-limit-download-a-new-script-from-the-portal"></a>„Dieses Skript ist ungültig, da die Bereitstellung der Datenträger über das Portal aufgehoben bzw. das 12-Stunden-Limit überschritten wurde. Laden Sie ein neues Skript aus dem Portal herunter“

**Mögliche Ursache:** Die Bereitstellung der Datenträger über das Portal wurde aufgehoben, oder das 12-Stunden-Limit wurde überschritten.

**Empfohlene Maßnahme:** 12 Stunden nach dem Herunterladen wird das Skript ungültig und kann nicht ausgeführt werden. Laden Sie im Portal ein neues Skript herunter, um die Dateiwiederherstellung fortzusetzen.

### <a name="iscsi_tcp-module-cant-be-loaded-or-iscsi_tcp_module-not-found"></a>iscsi_tcp_module kann nicht geladen werden (oder) iscsi_tcp_module kann nicht gefunden werden

**Empfohlene Maßnahme:** Um dieses Problem zu beheben, führen Sie die Schritte in [Das Skript wird erfolgreich heruntergeladen, aber bei der Ausführung tritt ein Fehler auf](#the-script-downloads-successfully-but-fails-to-run) aus.

## <a name="common-problems"></a>Häufige Probleme

Dieser Abschnitt enthält Schritte zum Beheben häufig auftretender Probleme, die beim Herunterladen und Ausführen des Skripts für die Dateiwiederherstellung auftreten können.

### <a name="you-cant-download-the-script"></a>Sie können das Skript nicht herunterladen

1. Sorgen Sie dafür, dass Sie die [erforderlichen Berechtigungen zum Herunterladen des Skripts](./backup-azure-restore-files-from-vm.md#select-recovery-point-who-can-generate-script) haben.
1. Überprüfen Sie die Verbindung mit den Azure-Ziel-APIs. Führen Sie einen der folgenden Befehle in einer Eingabeaufforderung mit erhöhten Rechten aus:

   `nslookup download.microsoft.com`

    oder

    `ping download.microsoft.com`

### <a name="the-script-downloads-successfully-but-fails-to-run"></a>Das Skript wird erfolgreich heruntergeladen, aber bei der Ausführung tritt ein Fehler auf.

Während der Ausführung des Python-Skripts für die Wiederherstellung auf Elementebene (Item Level Recovery, ILR) unter SUSE Linux Enterprise Server 12 SP4 tritt ein Fehler „iscsi_tcp_module kann nicht geladen werden“ oder „iscsi_tcp_module kann nicht gefunden werden“ auf.

**Mögliche Ursache:** Das ILR-Modul verwendet **iscsi_tcp**, um eine TCP-Verbindung mit dem Backup-Dienst herzustellen. Als Teil von SLES 12 SP4 entfernte SUSE **iscsi_tcp** aus dem open-iscsi-Paket, sodass der ILR-Vorgang zu einem Fehler führt.

**Empfohlene Maßnahme:**  Die Ausführung des Dateiwiederherstellungsskripts wird auf VMs mit SUSE 12 SP4 nicht unterstützt. Wiederholen Sie den Wiederherstellungsvorgang unter einer älteren Version als SUSE 12 SP4.

### <a name="the-script-runs-but-the-connection-to-the-iscsi-target-failed"></a>Das Skript wird ausgeführt, aber beim Herstellen der Verbindung mit dem iSCSI-Ziel ist ein Fehler aufgetreten

Möglicherweise wird die Fehlermeldung „Ausnahme beim Herstellen einer Verbindung mit dem Ziel“ angezeigt.

1. Stellen Sie sicher, dass der Computer, auf dem das Skript ausgeführt wird, die [Zugriffsanforderungen](./backup-azure-restore-files-from-vm.md#step-4-access-requirements-to-successfully-run-the-script) erfüllt.
1. Überprüfen Sie die Verbindung mit den Azure-Ziel-APIs. Führen Sie einen der folgenden Befehle in einer Eingabeaufforderung mit erhöhten Rechten aus:

   `nslookup download.microsoft.com`

   oder

   `ping download.microsoft.com`
1. Stellen Sie sicher, dass auf den ausgehenden iSCSI-Port 3260 zugegriffen werden kann.
1. Überprüfen Sie, ob eine Firewall oder NSG den Datenverkehr zu Azure-Ziel-IP-Adressen oder Wiederherstellungsdienst-URLs blockiert.
1. Stellen Sie sicher, dass Ihre Antivirussoftware die Ausführung des Skripts nicht blockiert.

### <a name="youre-connected-to-the-recovery-point-but-the-disks-werent-attached"></a>Sie sind mit dem Wiederherstellungspunkt verbunden, aber die Datenträger wurden nicht angefügt

Beheben Sie dieses Problem mit den Schritten für Ihr Betriebssystem.

#### <a name="windows-file-recovery-fails-on-server-with-storage-pools"></a>Fehler bei der Windows-Dateiwiederherstellung auf einem Server mit Speicherpools

Während der erstmaligen Ausführung des Skripts unter Windows Server 2012 R2 und Windows Server 2016 (mit Speicherpools) wird der Speicherpool der VM möglicherweise im schreibgeschützten Zustand angefügt.

>[!Tip]
> Sorgen Sie dafür, dass Sie den [richtigen Computer zum Ausführen des Skripts](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm#step-2-ensure-the-machine-meets-the-requirements-before-executing-the-script) haben.

Zur Behebung dieses Problems nehmen Sie die Zuweisung des Lese-/Schreibzugriffs für den Speicherpool und das Anfügen der virtuellen Datenträger manuell vor:

1. Gehen Sie zu **Server-Manager** > **Datei- und Speicherdienste** > **Volumes** > **Speicherpools**.

   ![Screenshot der Optionen für Speicherpools.](./media/backup-azure-restore-files-from-vm/windows-storage-1.png)

1. Klicken Sie im Fenster **Speicherpool** mit der rechten Maustaste auf den verfügbaren Speicherpool, und wählen Sie **Lese-/Schreibzugriff festlegen** aus.

   ![Screenshot, der Kontextmenüoptionen für einen Speicherpool zeigt.](./media/backup-azure-restore-files-from-vm/windows-storage-read-write-2.png)

1. Nach dem Zuweisen des Lese-/Schreibzugriffs für den Speicherpool klicken Sie mit der rechten Maustaste auf den Abschnitt **Virtuelle Datenträger**, und wählen Sie dann **Virtuellen Datenträger anfügen** aus.

   ![Screenshot, der Kontextmenüoptionen für einen virtuellen Datenträger zeigt.](./media/backup-azure-restore-files-from-vm/server-manager-virtual-disk-3.png)

#### <a name="linux-file-recovery-fails-to-auto-mount-because-the-disk-doesnt-contain-volumes"></a>Bei der Linux-Dateiwiederherstellung kann keine automatische Einbindung erfolgen, da der Datenträger keine Volumes enthält

Während der Ausführung der Dateiwiederherstellung erkennt der Sicherungsdienst Volumes und automatische Einbindungen. Wenn die gesicherten Datenträger aber unformatierte Partitionen enthalten, werden diese Datenträger nicht automatisch eingebunden, und der Datenträger wird bei der Wiederherstellung nicht angezeigt.

Um dieses Problem zu lösen, gehen Sie zu [Wiederherstellen von Dateien aus einer Sicherung von virtuellen Azure-Computern](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm#lvmraid-arrays-for-linux-vms).

#### <a name="linux-file-recovery-fails-because-the-os-couldnt-identify-the-file-system"></a>Bei der Linux-Dateiwiederherstellung tritt ein Fehler auf, weil das Betriebssystem das Dateisystem nicht identifizieren konnte

Wenn Sie das Skript zur Dateiwiederherstellung ausführen, kann der Datenträger nicht angefügt werden. Sie sehen die Fehlermeldung: „Die folgenden Partitionen konnten nicht eingebunden werden, da das Betriebssystem das Dateisystem nicht identifizieren konnte.“

Überprüfen Sie zur Behebung dieses Problems, ob das Volume mit einer Drittanbieteranwendung verschlüsselt wurde. Wenn es verschlüsselt wurde, wird der Datenträger oder die VM im Portal nicht als verschlüsselt angezeigt.

1. Melden Sie sich auf der gesicherten VM an, und führen Sie den folgenden Befehl aus:

   `lsblk -f`

   ![Screenshot, der die Ergebnisse des Befehls zeigt, mit dem Geräte blockiert werden.](./media/backup-azure-restore-files-from-vm/disk-without-volume-5.png)

1. Überprüfen Sie das Dateisystem und die Verschlüsselung. Wenn das Volume verschlüsselt wurde, wird die Dateiwiederherstellung nicht unterstützt. Weitere Informationen finden Sie unter [Unterstützungsmatrix für die Sicherung virtueller Azure-Computer](https://docs.microsoft.com/azure/backup/backup-support-matrix-iaas#support-for-file-level-restore).

### <a name="disks-are-attached-but-the-volumes-arent-mounted"></a>Datenträger werden angefügt, aber die Volumes nicht eingebunden

Beheben Sie dieses Problem mit den Schritten für Ihr Betriebssystem.

#### <a name="windows"></a>Windows

Wenn Sie das Skript zur Dateiwiederherstellung für Windows ausführen, wird die Meldung „0 Wiederherstellungsvolumes angefügt“ angezeigt. Die Datenträger werden jedoch in der Datenträgerverwaltungskonsole erkannt.

**Mögliche Ursache:** Beim Anfügen von Volumes über iSCSI wechselten einige erkannte Volumes in den Offlinestatus. Wenn der iSCSI-Kanal zwischen der VM und dem Dienst kommuniziert, erkennt er diese Volumes und schaltet sie online, aber sie werden nicht eingebunden.

   ![Screenshot mit „0 Wiederherstellungsvolumes angefügt“.](./media/backup-azure-restore-files-from-vm/disk-not-attached-6.png)

Führen Sie zum Identifizieren und Beheben dieses Problems die folgenden Schritte aus:

>[!Tip]
>Sorgen Sie dafür, dass Sie den [richtigen Computer zum Ausführen des Skripts](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm#step-2-ensure-the-machine-meets-the-requirements-before-executing-the-script) haben.

1. Führen Sie im Fenster **cmd** den Befehl **diskmgmt** aus, um die **Datenträgerverwaltung** zu öffnen.
1. Suchen Sie nach zusätzlichen Datenträgern. Im folgenden Beispiel ist **Datenträger 2** ein zusätzlicher Datenträger.

   ![Screenshot des Fensters „Datenträgerverwaltung“ mit zusätzlichem Datenträger.](./media/backup-azure-restore-files-from-vm/disk-management-7.png)

1. Klicken Sie mit der rechten Maustaste auf **Neues Volume**, und wählen Sie **Laufwerkbuchstaben und -pfade ändern** aus.

   ![Screenshot mit den Kontextmenüoptionen auf dem zusätzlichen Datenträger.](./media/backup-azure-restore-files-from-vm/disk-management-8.png)

1. Wählen Sie im Fenster **Laufwerkbuchstabe oder -pfad ändern** die Option **Folgenden Laufwerkbuchstaben zuweisen** aus, weisen Sie ein verfügbares Laufwerk zu, und wählen Sie **OK** aus.

   ![Screenshot des Fensters „Laufwerkbuchstabe oder -pfad ändern“.](./media/backup-azure-restore-files-from-vm/disk-management-9.png)

1. Öffnen Sie den Datei-Explorer, um das ausgewählte Laufwerk anzuzeigen, und untersuchen Sie die Dateien.

#### <a name="linux"></a>Linux

>[!Tip]
>Sorgen Sie dafür, dass Sie den [richtigen Computer zum Ausführen des Skripts](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm#step-2-ensure-the-machine-meets-the-requirements-before-executing-the-script) haben.

Wenn der geschützte virtuelle Linux-Computer LVM- oder RAID-Arrays verwendet, befolgen Sie die Schritte unter [LVM/RAID-Arrays (für virtuelle Linux-Computer)](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm#lvmraid-arrays-for-linux-vms).

### <a name="you-cant-copy-the-files-from-mounted-volumes"></a>Sie können Dateien aus eingebundenen Volumes nicht kopieren

Der Kopiervorgang verursacht möglicherweise den Fehler „0x80070780: Das System kann auf die Datei nicht zugreifen.“ 

Überprüfen Sie, ob beim Quellserver die Datenträgerdeduplizierung aktiviert wurde. Falls ja, vergewissern Sie sich, dass beim Wiederherstellungsserver die Deduplizierung auf den Laufwerken ebenfalls aktiviert wurde. Sie können die Deduplizierung unkonfiguriert lassen, sodass Sie die Laufwerke auf dem Wiederherstellungsserver nicht deduplizieren.

## <a name="next-steps"></a>Nächste Schritte

- [Wiederherstellen von Dateien und Ordnern aus einer Sicherung von Azure-VMs](backup-azure-restore-files-from-vm.md)