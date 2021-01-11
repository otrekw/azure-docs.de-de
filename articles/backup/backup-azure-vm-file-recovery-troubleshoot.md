---
title: Problembehandlung bei der Azure VM-Dateiwiederherstellung
description: Erfahren Sie, wie Sie Probleme bei der Wiederherstellung von Dateien und Ordnern aus einer Azure-VM-Sicherung beheben.
ms.topic: troubleshooting
ms.date: 07/12/2020
ms.openlocfilehash: bd369577e320cf6dca510183948f41e6cf91779b
ms.sourcegitcommit: e15c0bc8c63ab3b696e9e32999ef0abc694c7c41
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/16/2020
ms.locfileid: "97605291"
---
# <a name="troubleshooting-issues-in-file-recovery-of-azure-vm-backup"></a>Behandlung von Problemen bei der Dateiwiederherstellung aus einer Azure VM-Sicherung

Dieser Artikel enthält Schritte zur Problembehandlung, die Ihnen helfen können, Azure Backup-Fehler im Zusammenhang mit Problemen beim Wiederherstellen von Dateien und Ordnern aus einer Azure-VM-Sicherung zu beheben.

## <a name="common-error-messages"></a>Häufige Fehlermeldungen

### <a name="exception-caught-while-connecting-to-target"></a>Ausnahme beim Herstellen einer Verbindung mit dem Ziel

**Mögliche Ursache:** Das Skript kann nicht auf den Wiederherstellungspunkt zugreifen.

**Empfohlene Maßnahme:** Überprüfen Sie, ob der Computer alle [Zugriffsanforderungen](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm#step-4-access-requirements-to-successfully-run-the-script) erfüllt.

### <a name="the-target-has-already-been-logged-in-via-an-iscsi-session"></a>Das Ziel wurde bereits über eine iSCSI-Sitzung angemeldet.

**Mögliche Ursache:** Das Skript wurde bereits auf demselben Computer ausgeführt, und die Laufwerke wurden angefügt.

**Empfohlene Maßnahme:** Die Volumes des Wiederherstellungspunkts wurden bereits angefügt. Sie wurden ggf. nicht mit denselben Laufwerkbuchstaben des ursprünglichen virtuellen Computers eingebunden. Durchsuchen Sie im Datei-Explorer alle verfügbaren Volumes.

### <a name="this-script-is-invalid-because-the-disks-have-been-dismounted-via-portalexceeded-the-12-hr-limit-download-a-new-script-from-the-portal"></a>Dieses Skript ist ungültig, da die Bereitstellung der Datenträger über das Portal aufgehoben bzw. das 12-Stunden-Limit überschritten wurde. Herunterladen eines neuen Skripts aus dem Portal

**Mögliche Ursache:** Die Bereitstellung der Datenträger über das Portal wurde aufgehoben, oder das 12-Stunden-Limit wurde überschritten.

**Empfohlene Maßnahme:** Das Skript wird 12 Stunden nach dem Zeitpunkt des Downloads ungültig und kann nicht mehr ausgeführt werden. Besuchen Sie das Portal, und laden Sie ein neues Skript herunter, um die Dateiwiederherstellung fortzusetzen.

## <a name="troubleshooting-common-scenarios"></a>AKS-Problembehandlung

Dieser Abschnitt enthält Schritte zum Beheben von Problemen, die beim Herunterladen und Ausführen des Skripts für Dateiwiederherstellung auftreten können.

### <a name="cant-download-the-script"></a>Skript kann nicht heruntergeladen werden

Empfohlene Maßnahme:

1. Sorgen Sie dafür, dass Sie alle [erforderlichen Berechtigungen zum Herunterladen des Skripts](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm#select-recovery-point-who-can-generate-script) haben.
1. Vergewissern Sie sich, dass eine Verbindung mit den Ziel-IP-Adressen in Azure besteht.

Führen Sie zum Überprüfen der Verbindung einen der folgenden Befehl an einer Eingabeaufforderung mit erhöhten Rechten aus.

`nslookup download.microsoft.com`

oder

`ping download.microsoft.com`

### <a name="the-script-downloads-successfully-but-fails-to-run"></a>Das Skript wird erfolgreich heruntergeladen, aber bei der Ausführung tritt ein Fehler auf.

#### <a name="for-sles-12-sp4-os-linux"></a>Bei SLES 12 SP4 OS (Linux)

**Fehler:** „iscsi_tcp_module“ nicht gefunden

**Mögliche Ursache:** Während der Ausführung des Python-Skripts für die Wiederherstellung auf Elementebene (Item Level Recovery, ILR) unter der SUSE Linux-Betriebssystemversion 12sp4 tritt ein Fehler des Typs **_iscsi_tcp-Modul kann nicht geladen werden_* _ auf.

Das ILR-Modul verwendet _ *iscsi_tcp**, um eine TCP-Verbindung mit dem Backup-Dienst herzustellen. Als Teil von Release 12SP4 entfernte SUSE **iscsi_tcp** aus dem open-iscsi-Paket, sodass der ILR-Vorgang zu einem Fehler führt.

**Empfohlene Maßnahme:**  Die Ausführung des Dateiwiederherstellungsskripts wird auf VMs mit SUSE 12SP4 nicht unterstützt. Wiederholen Sie den Wiederherstellungsvorgang unter einer älteren Version als SUSE 12SP4.

### <a name="the-script-runs-but-connection-to-iscsi-target-failed"></a>Das Skript wird ausgeführt, aber bei der Verbindung zum iSCSI-Ziel ist ein Fehler aufgetreten.

**Fehler:** Ausnahme beim Herstellen einer Verbindung mit dem Ziel

1. Stellen Sie sicher, dass der Computer, auf dem das Skript ausgeführt wird, alle [Zugriffsanforderungen](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm#step-4-access-requirements-to-successfully-run-the-script) erfüllt.
1. Überprüfen Sie die Konnektivität mit den Ziel-IP-Adressen in Azure.
Führen Sie zum Überprüfen der Verbindung einen der folgenden Befehl an einer Eingabeaufforderung mit erhöhten Rechten aus.

   `nslookup download.microsoft.com`

   oder

   `ping download.microsoft.com`
1. Stellen Sie sicher, dass auf den ausgehenden iSCSI-Port 3260 zugegriffen werden kann.
1. Stellen Sie sicher, dass es keine Firewall oder NSG gibt, die den Datenverkehr an Azure-Ziel-IP-Adressen oder Wiederherstellungsdienst-URLs blockiert.
1. Überprüfen Sie, ob Antivirensoftware die Ausführung des Skripts blockiert.

### <a name="connected-to-recovery-point-but-disks-didnt-get-attached"></a>Mit dem Wiederherstellungspunkt verbunden, aber Datenträger wurden nicht angefügt

Sorgen Sie dafür, dass Sie den [richtigen Computer zum Ausführen des Skripts](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm#step-2-ensure-the-machine-meets-the-requirements-before-executing-the-script) haben.

#### <a name="on-a-windows-vm"></a>Auf einer Windows-VM

**Der Speicherpool auf der VM wird im schreibgeschützten Modus angefügt:**  Unter Windows 2012 R2 und Windows 2016 (mit den Speicherpools) wechselt der an die VM angefügte Speicherpool während der erstmaligen Ausführung des Skripts möglicherweise in den schreibgeschützten Zustand.

Zur Behebung dieses Problems müssen Sie den Lese-/Schreibzugriff auf den Speicherpool zum ersten Mal manuell festlegen und die virtuellen Datenträger anfügen. Führen Sie diese Schritte aus:

1. Legen Sie „Lese-/Schreibzugriff“ fest.

   Navigieren Sie nacheinander zu **Server-Manager** > **Datei- und Speicherdienste** > **Volumes** > **Speicherpools**.

   ![Windows Storage](./media/backup-azure-restore-files-from-vm/windows-storage-1.png)

1. Klicken Sie im Fenster **Speicherpool** mit der rechten Maustaste auf den verfügbaren Speicherpool, und wählen Sie die Option **Lese-/Schreibzugriff festlegen** aus.

   ![Lese-/Schreibzugriff unter Windows Storage](./media/backup-azure-restore-files-from-vm/windows-storage-read-write-2.png)

1. Fügen Sie, nachdem für den Speicherpool Lese-/Schreibzugriff zugewiesen wurde, den virtuellen Datenträger an.

   Navigieren Sie zur **Server-Manager-Benutzeroberfläche**. Klicken Sie im Abschnitt **Virtuelle Datenträger** mit der rechten Maustaste, und wählen Sie die Option **Virtuellen Datenträger anfügen** aus.

   ![Virtueller Datenträger des Server-Managers](./media/backup-azure-restore-files-from-vm/server-manager-virtual-disk-3.png)

#### <a name="on-a-linux-vm"></a>Auf einer Linux-VM

##### <a name="file-recovery-fails-to-auto-mount-because-disk-doesnt-contain-volumes"></a>Bei der Dateiwiederherstellung kann keine automatische Einbindung erfolgen, da der Datenträger keine Volumes enthält.

Während der Ausführung der Dateiwiederherstellung erkennt der Backup-Dienst Volumes und automatische Einbindungen. Wenn die gesicherten Datenträger aber unformatierte Partitionen enthalten, werden diese Datenträger nicht automatisch eingebunden, und der Datenträger wird bei der Wiederherstellung nicht angezeigt.

Führen Sie zur Behebung dieses Problems die Schritte in [diesem Artikel](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm#lvmraid-arrays-for-linux-vms) aus.

##### <a name="the-os-couldnt-identify-the-filesystem-causing-linux-file-recovery-to-fail-while-mountings-disks"></a>Das Betriebssystem konnte das Dateisystem nicht identifizieren, wodurch die Linux-Dateiwiederherstellung beim Einbinden von Datenträgern einen Fehler verursacht hat.

Während der Ausführung des Skripts zur Dateiwiederherstellung trat beim Anfügen des Datenträgers dieser Fehler auf:

„Die folgenden Partitionen konnten nicht eingebunden werden, da das Betriebssystem das Dateisystem nicht identifizieren konnte.“

Überprüfen Sie zur Behebung dieses Problems, ob das Volume mit einer Drittanbieteranwendung verschlüsselt wurde. Wenn es verschlüsselt wurde, wird der Datenträger oder die VM im Portal nicht als verschlüsselt angezeigt.

1. Melden Sie sich auf der gesicherten VM an, und führen Sie den folgenden Befehl aus:

   `*lsblk -f*`

   ![Datenträger ohne Volume](./media/backup-azure-restore-files-from-vm/disk-without-volume-5.png)

2. Überprüfen Sie das Dateisystem und die Verschlüsselung.
3. Wenn das Volume verschlüsselt wurde, wird die Dateiwiederherstellung nicht unterstützt. [Weitere Informationen](https://docs.microsoft.com/azure/backup/backup-support-matrix-iaas#support-for-file-level-restore)

### <a name="disks-are-attached-but-unable-to-mount-volumes"></a>Datenträger werden angefügt, können aber keine Volumes einbinden

- Sorgen Sie dafür, dass Sie den [richtigen Computer zum Ausführen des Skripts](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm#step-2-ensure-the-machine-meets-the-requirements-before-executing-the-script) haben.

#### <a name="on-windows-vms"></a>Auf Windows-VMs

Beim Ausführen des Dateiwiederherstellungsskripts unter Windows wird eine Meldung angezeigt, dass ***0 Volumes angefügt sind** _. Die Datenträger werden jedoch in der Datenträgerverwaltungskonsole erkannt. Beim Anfügen von Volumes über iSCSI wechseln einige erkannte Volumes in den Offlinestatus. Wenn der iSCSI-Kanal zwischen der VM und dem Dienst kommuniziert, erkennt er diese Volumes und schaltet sie online, aber sie werden nicht eingebunden.

   ![Datenträger nicht angefügt](./media/backup-azure-restore-files-from-vm/disk-not-attached-6.png)

Führen Sie zum Identifizieren und Beheben dieses Problems die folgenden Schritte aus:

1. Wechseln Sie zu _ *Datenträgerverwaltung**, indem Sie im Befehlsfenster den Befehl **diskmgmt** ausführen.
1. Überprüfen Sie, ob zusätzliche Datenträger angezeigt werden. Im folgenden Beispiel ist „Datenträger 2“ ein zusätzlicher Datenträger.

   ![Datenträgerverwaltung0](./media/backup-azure-restore-files-from-vm/disk-management-7.png)

1. Klicken Sie mit der rechten Maustaste auf **Neues Volume**, und wählen Sie **Laufwerkbuchstaben und -pfade ändern** aus.

   ![Datenträgerverwaltung1](./media/backup-azure-restore-files-from-vm/disk-management-8.png)

1. Wählen Sie im Fenster **Laufwerkbuchstabe oder -pfad hinzufügen** die Option **Folgenden Laufwerkbuchstaben zuweisen** aus, weisen Sie ein verfügbares Laufwerk zu, und wählen Sie **OK** aus.

   ![Datenträgerverwaltung2](./media/backup-azure-restore-files-from-vm/disk-management-9.png)

1. Zeigen Sie im Datei-Explorer den ausgewählten Laufwerkbuchstaben an, und untersuchen Sie die Dateien.

#### <a name="on-linux-vms"></a>Auf Linux-VMs

- Sorgen Sie dafür, dass Sie den [richtigen Computer zum Ausführen des Skripts](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm#step-2-ensure-the-machine-meets-the-requirements-before-executing-the-script) haben.
- Wenn die geschützte Linux-VM LVM- oder RAID-Arrays verwendet, führen Sie die in [diesem Artikel](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm#lvmraid-arrays-for-linux-vms) aufgeführten Schritte aus.

### <a name="cant-copy-the-files-from-mounted-volumes"></a>Dateien aus eingebundenen Volumes können nicht kopiert werden

Der Kopiervorgang verursacht möglicherweise den Fehler **0x80070780: Das System kann auf die Datei nicht zugreifen**. Überprüfen Sie in diesem Fall, ob beim Quellserver die Datenträgerdeduplizierung aktiviert wurde. Vergewissern Sie sich anschließend, dass beim Wiederherstellungsserver die Deduplizierung auf den Laufwerken ebenfalls aktiviert wurde. Sie können die Deduplizierungsrolle unkonfiguriert beibehalten und damit die Laufwerke auf dem Wiederherstellungsserver nicht deduplizieren.

## <a name="next-steps"></a>Nächste Schritte

- [Wiederherstellen von Dateien und Ordnern aus einer Sicherung von Azure-VMs](backup-azure-restore-files-from-vm.md)
