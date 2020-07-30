---
title: 'Problembehandlung beim Windows-Stillstandfehler: Fehler bei der Verzeichnisdienstinitialisierung'
description: Beheben von Problemen bei denen ein virtueller Computer (VM) mit einem Active Directory-Domänencontroller in Azure in einer Schleife festhängt und angibt, dass ein Neustart erforderlich ist.
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: 3396f8fe-7573-4a15-a95d-a1e104c6b76d
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 05/05/2020
ms.author: v-miegge
ms.openlocfilehash: 909481964f8aa3272715e235fa011562225a9422
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87028361"
---
# <a name="troubleshoot-windows-stop-error--directory-service-initialization-failure"></a>Problembehandlung beim Windows-Stillstandfehler: Fehler bei der Verzeichnisdienstinitialisierung

Dieser Artikel beschreibt Schritte zum Beheben von Problemen bei denen ein virtueller Computer (VM) mit einem Active Directory-Domänencontroller in Azure in einer Schleife festhängt und angibt, dass ein Neustart erforderlich ist.

## <a name="symptom"></a>Symptom

Wenn Sie die [Startdiagnose](./boot-diagnostics.md) verwenden, um den Screenshot der VM anzuzeigen, zeigt der Screenshot, dass die VM aufgrund eines Fehlers neu gestartet werden muss, wobei in Windows Server 2008 R2 der Stillstandcode **0xC00002E1** und in Windows Server 2012 oder höher **0xC00002E2** angezeigt wird.

![Der Startbildschirm von Windows Server 2012 besagt „Your PC ran into a problem and needs to restart. We're just collecting some error info, and then we'll restart for you“. (Auf dem PC ist ein Problem aufgetreten. Er muss neu gestartet werden. Wir sammeln lediglich einige Fehlerinformationen, dann erfolgt der Neustart).](./media/troubleshoot-directory-service-initialization-failure/1.png)

## <a name="cause"></a>Ursache

Fehlercode **0xC00002E2** stellt **STATUS_DS_INIT_FAILURE**, Fehlercode **0xC00002E1** **STATUS_DS_CANT_START** dar. Beide Fehler treten auf, wenn ein Problem beim Verzeichnisdienst vorliegt.

Beim Starten des Betriebssystems wird dann ein automatischer Neustart durch den Local Security Authentication Server (**LSASS.exe**) erzwungen, der Benutzeranmeldungen authentifiziert. Die Authentifizierung kann nicht erfolgen, wenn es sich bei dem Betriebssystem auf der VM um einen Domänencontroller handelt, der nicht über Lese-/Schreibzugriff auf seine lokale Active Directory-Datenbank verfügt. Aufgrund fehlender Zugriffsrechte auf **Active Directory (AD)** kann „LSASS.exe“ die Authentifizierung nicht durchführen und ist gezwungen, das Betriebssystem neu zu starten.

Dieser Fehler kann durch jede der folgenden Bedingungen verursacht werden:

- Es besteht kein Zugriff auf den Datenträger, der die lokale AD-Datenbank (**NTDS.DIT**) enthält.
- Auf dem Datenträger, der die lokale AD-Datenbank (NTDS.DIT) enthält, ist kein Speicherplatz mehr verfügbar.
- Die Datei der lokalen AD-Datenbank (NTDS.DIT) fehlt.
- Die VM weist mehrere Datenträger auf, und die SAN-Richtlinie (Storage Area Network) ist nicht ordnungsgemäß konfiguriert. Die SAN-Richtlinie ist nicht auf **ONLINEALL** festgelegt, und die nicht vom Betriebssystem belegten Datenträger sind in der Datenträgerverwaltung im Offlinemodus angefügt.
- Die Datei der lokalen AD-Datenbank (NTDS.DIT) ist beschädigt.

## <a name="solution"></a>Lösung

### <a name="process-overview"></a>Prozessübersicht:

1. Erstellen Sie eine Reparatur-VM, und greifen Sie darauf zu.
1. Geben Sie Speicherplatz auf dem Datenträger frei.
1. Überprüfen Sie, ob das Laufwerk, das die AD-Datenbank enthält, angefügt ist.
1. Aktivieren Sie den Wiederherstellungsmodus für Verzeichnisdienste.
1. **Empfohlen:** Aktivieren Sie vor der Neuerstellung des virtuellen Computers die serielle Konsole und die Speicherabbildsammlung.
1. Erstellen Sie den virtuellen Computer neu.
1. Konfigurieren Sie die SAN-Richtlinie neu.

> [!NOTE]
> Wenn dieser Fehler auftritt, ist das Gastbetriebssystem nicht funktionsfähig. Sie müssen die Problembehandlung im Offlinemodus durchführen, um das Problem zu beheben.

### <a name="create-and-access-a-repair-vm"></a>Erstellen und Aufrufen einer Reparatur-VM

1. Führen Sie die [Schritte 1-3 der VM-Reparaturbefehle](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md#repair-process-example) aus, um eine Reparatur-VM vorzubereiten.
1. Stellen Sie über eine Remotedesktopverbindung eine Verbindung mit der Reparatur-VM her.

### <a name="free-up-space-on-disk"></a>Freigeben von Speicherplatz auf dem Datenträger

Da der Datenträger jetzt an eine Reparatur-VM angefügt ist, überprüfen Sie, ob der Datenträger, der die interne Active Directory-Datenbank enthält, über ausreichend Speicherplatz für die ordnungsgemäße Funktion verfügt.

1. Überprüfen Sie, ob der Datenträger voll ist, indem Sie mit der rechten Maustaste auf das Laufwerk klicken und **Eigenschaften** auswählen.
1. Wenn der Datenträger weniger als 300 MB freien Speicherplatz aufweist, [erweitern Sie ihn mithilfe von PowerShell auf den Maximalwert von 1 TB](../windows/expand-os-disk.md).
1. Wenn der Datenträger 1 TB Speicherplatz erreicht hat, führen Sie eine Datenträgerbereinigung durch.

   1. Verwenden Sie PowerShell, um [den Datenträger von der defekten VM zu trennen](../windows/detach-disk.md#detach-a-data-disk-using-powershell).
   1. [Fügen Sie den Datenträger nach der Trennung](../windows/attach-disk-ps.md#attach-an-existing-data-disk-to-a-vm) von der defekten VM an eine funktionierende VM an.
   1. Verwenden Sie das [Datenträgerbereinigungstool](https://support.microsoft.com/help/4026616/windows-10-disk-cleanup), um weiteren Speicherplatz freizugeben.

1. **Optional**: Wenn mehr Speicherplatz erforderlich ist, öffnen Sie eine CMD-Instanz, und geben Sie den `defrag <LETTER ASSIGNED TO THE OS DISK>: /u /x /g`-Befehl ein, um eine Defragmentierung des Laufwerks auszuführen:

  * Ersetzen Sie in dem Befehl `<LETTER ASSIGNED TO THE OS DISK>` durch den Buchstaben des Betriebssystem-Datenträgers. Wenn der Buchstabe des Datenträgers beispielsweise `F:` ist, lautet der Befehl `defrag F: /u /x /g`.

  * Je nach dem Grad der Fragmentierung kann die Defragmentierung mehrere Stunden dauern.

Wenn auf dem Datenträger genügend Speicherplatz vorhanden ist, fahren Sie mit der nächsten Aufgabe fort.

### <a name="check-that-the-drive-containing-the-active-directory-database-is-attached"></a>Überprüfung, ob das Laufwerk mit der Active Directory-Datenbank angefügt ist

1. Öffnen Sie eine Instanz einer Eingabeaufforderung mit erhöhten Rechten, und führen Sie die folgenden Befehle aus:

   1. Laden der Registrierungsdatei:

      `REG LOAD HKLM\BROKENSYSTEM f:\windows\system32\config\SYSTEM`

      Bei der Bezeichnung `f:` wird davon ausgegangen, dass der Datenträger Laufwerk `F:`ist. Verwenden Sie den Laufwerkbuchstaben des Laufwerks, das den Betriebssystem-Datenträger enthält.

   1. Bestimmen von Laufwerkbuchstabe und Ordner von **NTDS.DIT**:

      ```
      REG QUERY "HKLM\BROKENSYSTEM\ControlSet001\Services\NTDS\parameters" /v "DSA Working Directory"
      REG QUERY "HKLM\BROKENSYSTEM\ControlSet001\Services\NTDS\parameters" /v "DSA Database file"
      REG QUERY "HKLM\BROKENSYSTEM\ControlSet001\Services\NTDS\parameters" /v "Database backup path"
      REG QUERY "HKLM\BROKENSYSTEM\ControlSet001\Services\NTDS\parameters" /v "Database log files path"
      ```

   1. Entladen der Registrierungsdatei:

      `REG UNLOAD HKLM\BROKENSYSTEM`

1. Überprüfen Sie mithilfe des Azure-Portals, ob das Laufwerk, auf dem NTDS.DIT eingerichtet ist, an die VM angefügt wurde.
1. Überprüfen Sie mithilfe der Konsole der Datenträgerverwaltung des Gastbetriebssystems, ob der Datenträger, der NTDS.DIT enthält, online ist.
   1. Das Datenträgerverwaltungs-Tool finden Sie in **Verwaltung > Computerverwaltung > Speicher**, alternativ ist der Zugriff über den Befehl `diskmgmt.msc` in einer CMD-Instanz möglich.
1. Wenn der Datenträger nicht an die VM angefügt ist, fügen Sie den Datenträger wieder an, um das Problem zu beheben.

   Wenn der Datenträger normal angefügt ist, fahren Sie mit der nächsten Aufgabe fort.

### <a name="enable-directory-services-restore-mode"></a>Aktivieren des Wiederherstellungsmodus für Verzeichnisdienste

Richten Sie die VM für das Starten im **Wiederherstellungsmodus für Verzeichnisdienste (Directory Services Restore Mode, DSRM)** ein, um während des Startvorgangs die Prüfung auf die Gegenwart der NTDS.DIT-Datei zu umgehen.

1. Bevor Sie fortfahren, vergewissern Sie sich, dass Sie die vorherigen Aufgaben abgeschlossen haben, um den Datenträger an eine Reparatur-VM anzufügen, und bestimmt haben, auf welchem Datenträger sich die NTDS.DIT-Datei befindet.
1. Listen Sie in einer CMD-Instanz mit erhöhten Rechten die Startpartitions-Informationen des betreffenden Speichers auf, um den Bezeichner der aktiven Partition zu finden:

   `bcdedit /store <Drive Letter>:\boot\bcd /enum`

   Ersetzen Sie `< Drive Letter >` durch den Laufwerkbuchstaben, der im vorherigen Schritt bestimmt wurde.

   ![Der Screenshot zeigt eine CMD-Instanz mit erhöhten Rechten nach der Eingabe des Befehls ‚bcdedit /store <Laufwerkbuchstabe>:\boot\bcd /enum‘, wodurch der Windows-Start-Manager mit dem Bezeichner angezeigt wird.](./media/troubleshoot-directory-service-initialization-failure/2.png)

1. Aktivieren Sie das Flag `safeboot DsRepair` für die Startpartition:

   `bcdedit /store <Drive Letter>:\boot\bcd /set {<Identifier>} safeboot dsrepair`

   Ersetzen Sie `< Drive Letter >` und `< Identifier >` durch die in den vorherigen Schritten bestimmten Werte.

1. Fragen Sie die Startoptionen erneut ab, um sicherzustellen, dass Ihre Änderung ordnungsgemäß festgelegt wurde.

   ![Der Screenshot zeigt eine CMD-Instanz mit erhöhten Rechten nach dem Aktivieren des DsRepair-Flags für den sicheren Systemstart.](./media/troubleshoot-directory-service-initialization-failure/3.png)

### <a name="recommended-before-you-rebuild-the-vm-enable-serial-console-and-memory-dump-collection"></a>Empfohlen: Aktivieren der seriellen Konsole und der Speicherabbildsammlung vor der Neuerstellung der VM

Um die Speicherabbildsammlung und die serielle Konsole zu aktivieren, führen Sie das folgende Skript aus, indem Sie eine Eingabeaufforderung mit erhöhten Rechten (Als Administrator ausführen) öffnen und die folgenden Befehle ausführen.

1. Aktivieren der seriellen Konsole:

  ```
  bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /ems {<BOOT LOADER IDENTIFIER>} ON
  bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200
  ```

1. Überprüfen Sie, ob der freie Speicherplatz auf dem Betriebssystemdatenträger mindestens der Größe des Arbeitsspeichers (RAM) auf der VM entspricht.

  1. Wenn nicht genügend Speicherplatz auf dem Betriebssystemdatenträger vorhanden ist, ändern Sie den Speicherort für das Erstellen der Speicherabbilddatei, und verweisen Sie auf einen an die VM angefügten Datenträger, der über genügend freien Speicherplatz verfügt.

     Ersetzen Sie `%SystemRoot%` in den folgenden Befehlen durch den Laufwerkbuchstaben (z. B. `F:`) des Datenträgers, um den Speicherort zu ändern.

  #### <a name="the-following-configuration-is-suggested-to-enable-os-dump"></a>Die folgende Konfiguration wird empfohlen, um Speicherabbilder vom Betriebssystem zu aktivieren:

  **Laden des beschädigten Betriebssystemdatenträgers**:

  `REG LOAD HKLM\BROKENSYSTEM <VOLUME LETTER OF BROKEN OS DISK>:\windows\system32\config\SYSTEM`

  **Aktivieren für „ControlSet001“** :

  ```
  REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f
  REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f
  REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f
  ```

  **Aktivieren für „ControlSet002“** :

  ```
  REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f
  REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f
  REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f
  ```

  **Entladen des beschädigten Betriebssystemdatenträgers**:

  `REG UNLOAD HKLM\BROKENSYSTEM`

### <a name="rebuild-the-vm"></a>Neuerstellen der VM

1. Führen Sie [Schritt 5 der VM-Reparaturbefehle](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md#repair-process-example) aus, um die VM zu reassemblieren.

### <a name="reconfigure-the-storage-area-network-policy"></a>Umkonfigurieren der Speicherbereichs-Netzwerkrichtlinie

1. Beim Starten im DSRM-Modus ist der einzige Benutzer, der für die Anmeldung zur Verfügung steht, der Wiederherstellungsadministrator, der verwendet wurde, als der virtuelle Computer zu einem Domänencontroller heraufgestuft wurde. Für alle anderen Benutzer wird ein Authentifizierungsfehler angezeigt.

   1. Wenn kein anderer Domänencontroller verfügbar ist, müssen Sie sich lokal mit `.\administrator` oder `machinename\administrator` und dem DSRM-Kennwort anmelden.

1. Richten Sie die SAN-Richtlinie so ein, dass alle Datenträger online sind.

   1. Öffnen Sie eine CMD-Instanz mit erhöhten Rechten, und geben Sie `DISKPART` ein.
   1. Fragen Sie die Liste der Datenträger ab.

      `DISKPART> list disk`

   1. Geben Sie die folgenden Befehle ein, um den Datenträger auszuwählen, der online geschaltet werden muss, und ändern Sie die SAN-Richtlinie:

      ```
      DISKPART> select disk 1
      Disk 1 is now the selected disk.

      DISKPART> attributes disk clear readonly
      Disk attributes cleared successfully.

      DISKPART> attributes disk
      Current Read-only State : No
      Read-only : No
      Boot Disk : No
      Pagefile Disk : No
      Hibernation File Disk : No
      Crashdump Disk : No
      Clustered Disk : No

      DISKPART> online disk
      DiskPart successfully onlined the selected disk.

      DISKPART> san
      SAN Policy : Online All
      ```

1. Nachdem das Problem behoben wurde, stellen Sie sicher, dass das Flag `DsRepair safeboot` entfernt wird:

   `bcdedit /deletevalue {default} safeboot dsrepair`

1. Starten Sie den virtuellen Computer neu.

   > [!NOTE]
   > Wenn Ihr virtueller Computer soeben von einem lokalen Standort aus migriert wurde und Sie weitere Domänencontroller aus der lokalen Umgebung zu Azure migrieren möchten, sollten Sie die Schritte im folgenden Artikel in Erwägung ziehen, um zu verhindern, dass dieses Problem in zukünftigen Migrationen auftritt:
   >
   > [Hochladen vorhandener lokaler Hyper-V-Domänencontroller in Azure mithilfe von Azure PowerShell](https://support.microsoft.com/help/2904015)
