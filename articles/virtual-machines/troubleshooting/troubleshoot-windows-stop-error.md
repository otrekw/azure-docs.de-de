---
title: 'Windows-kritischer Fehler: Status „Nicht genügend Arbeitsspeicher“'
description: In diesem Artikel wird beschrieben, wie Probleme behoben werden, bei denen Windows nicht gestartet werden kann und der Status „Nicht genügend Arbeitsspeicher“ angezeigt wird.
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: d29c7e49-4578-43c8-b829-831da4e48932
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 06/26/2020
ms.author: v-mibufo
ms.openlocfilehash: 67064cf694445acf8472b958660133c2f2d31db9
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85656784"
---
# <a name="windows-stop-error---status-no-memory"></a>Windows-kritischer Fehler: Status „Nicht genügend Arbeitsspeicher“

In diesem Artikel wird beschrieben, wie Probleme behoben werden, bei denen Windows nicht gestartet werden kann und Fehlercode „#0xC0000017“, auch als Status „Nicht genügend Arbeitsspeicher“ bekannt, angezeigt wird.

## <a name="symptom"></a>Symptom

Wenn Sie die [Startdiagnose](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/boot-diagnostics) verwenden, um den Screenshot der VM anzuzeigen, enthält der Screenshot den folgenden Fehlercode: `0xC0000017`. Dieser Fehler wird abhängig von der verwendeten Windows-Version entweder im **Windows-Start-Manager** oder im **Wiederherstellungsbildschirm** angezeigt.

   **Windows-Start-Manager**

   ![Der Windows-Start-Manager gibt Folgendes an: „Windows konnte nicht gestartet werden. Dies kann auf eine Hardware- oder Softwareänderung zurückzuführen sein“. Wenn Sie nach unten scrollen, sehen Sie den Statuscode „0xC0000017“ und folgende Meldung: „Die Anwendung oder das Betriebssystem konnte nicht geladen werden, da eine erforderliche Datei fehlt oder Fehler enthält.“](./media/troubleshoot-windows-stop-error/1.png)

   **Wiederherstellungsbildschirm**
 
   ![Auf dem Wiederherstellungsbildschirm wird folgende Meldung angezeigt: „Der PC/das Gerät muss repariert werden. Für die Erstellung eines RAMDISK-Geräts ist nicht genügend Speicherplatz verfügbar.“ Zudem wird der Fehlercode „0xC0000017“ angezeigt.](./media/troubleshoot-windows-stop-error/2.png)

## <a name="cause"></a>Ursache

Der Datenträger mit dem Betriebssystem ist entweder zu voll oder zu sehr fragmentiert oder das Betriebssystem (BS) hat keinen Zugriff auf den Arbeitsspeicher, auf die Auslagerungsdatei oder auf beides.

## <a name="solution"></a>Lösung

### <a name="process-overview"></a>Prozessübersicht:

1. Erstellen einer Reparatur-VM und Zugreifen darauf
1. Freigeben von Speicherplatz auf der Festplatte
1. Löschen von ungültigem Speicher im BCD-Speicher
1. Wiederherstellen der Auslagerungsdatei am Standardspeicherort
1. Aktivieren der seriellen Konsole und der Speicherabbilderfassung
1. Neuerstellen der VM

> [!NOTE]
> Wenn dieser Fehler auftritt, ist das Gastbetriebssystem nicht funktionsfähig. Sie müssen die Problembehandlung im Offlinemodus durchführen, um dieses Problem zu beheben.

### <a name="create-and-access-a-repair-vm"></a>Erstellen und Aufrufen einer Reparatur-VM

1. Führen Sie die [Schritte 1-3 der VM-Reparaturbefehle](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands) aus, um eine Reparatur-VM vorzubereiten.
1. Stellen Sie über eine Remotedesktopverbindung eine Verbindung mit der Reparatur-VM her.

### <a name="for-generation-2-vms-assign-a-letter-to-the-extensible-firmware-interface-efi-partition"></a>Weisen Sie bei VMs der 2. Generation der EFI-Partition (Extensible Firmware Interface) einen Buchstaben zu:

Wenn Sie eine VM der 2. Generation verwenden, wurde der EFI-Partition des angefügten Datenträgers möglicherweise noch kein Buchstabe zugewiesen. Sie müssen die folgenden Schritte ausführen, um der Partition einen Buchstaben zuzuweisen, bevor Sie mit diesem Leitfaden zur Problembehandlung fortfahren.

1. Geben Sie in der Windows-Suche `diskmgmt` ein, und öffnen Sie die **Datenträgerverwaltungskonsole**.
1. Ermitteln Sie den defekten an die Reparatur-VM angefügten Datenträger. Dieser Datenträger wird in der Regel als letzter in der Konsole angezeigt und weist den höchsten numerischen Wert auf.
1. Prüfen Sie, ob sich auf diesem Datenträger eine Partition mit der **EFI-Systempartition** befindet, dem ebenfalls kein Buchstabenwert zugewiesen ist (z. B. Laufwerk *F:* ). Wenn alle Partitionen zugewiesen sind, können Sie auf dem Datenträger Speicherplatz freigeben. Andernfalls weisen Sie diesem Datenträger einen Buchstaben zu.

   ![Die Datenträgerverwaltungskonsole mit dem angefügten Datenträger „Disk 2“ und der nicht zugewiesenen Partition mit 100 MB, bei der es sich um die „EFI-Systempartition“ handelt](./media/troubleshoot-windows-stop-error/3.png)

1. Öffnen Sie eine Eingabeaufforderung mit erhöhten Rechten als Administrator, und geben Sie `diskpart` ein, um das Tool **DISKPART** zu starten.
1. Geben Sie die folgenden Befehle ein:

   ```
   list disk
   sel disk <NUMBER OF THE ATTACHED DISK>
   list partition
   sel partition <NUMBER OF THE SYSTEM (EFI) PARTION>
   assign
   ```
   
   - Ersetzen Sie im Befehl `<NUMBER OF THE ATTACHED DISK>` durch die Datenträgernummer, die Sie in Schritt 2 ermittelt haben.
   - Ersetzen Sie im Befehl `<NUMBER OF THE SYSTEM PARTION>` durch die Partitionsnummer der EFI-Systempartition. Dieser Partition wurde noch kein Buchstabe zugewiesen. Sie sollte jedoch vom Typ **System** und etwa 100 MB groß sein.

   > [!NOTE]
   > Die Partitionen in der Datenträgerverwaltungskonsole mit den im DISKPART-Tool aufgeführten Partitionen zu vergleichen, kann hilfreich sein, um zu bestimmen, welche Partitionsnummer der EFI-Systempartition auf dem angefügten Datenträger entspricht.

1. Schließen Sie das Eingabeaufforderungsfenster.

### <a name="free-up-space-on-the-disk"></a>Freigeben von Speicherplatz auf der Festplatte

Nachdem der defekte Datenträger an die Reparatur-VM angefügt wurde, sollten Sie überprüfen, ob das Betriebssystem auf diesem Datenträger über genügend Speicherplatz für die ordnungsgemäße Funktion verfügt. 

1. Überprüfen Sie, ob der Datenträger voll ist, indem Sie mit der rechten Maustaste auf das Laufwerk des angefügten Datenträgers klicken und **Eigenschaften** auswählen.
1. Wenn der Datenträger **weniger als 300 MB freien Speicherplatz** aufweist, [erweitern Sie ihn mithilfe von PowerShell auf den Maximalwert von 1 TB](https://docs.microsoft.com/azure/virtual-machines/windows/expand-os-disk).
1. Sobald der Datenträger eine Größe von **1 TB** aufweist, müssen Sie eine Datenträgerbereinigung durchführen. Sie können das [Datenträgerbereinigungstool](https://support.microsoft.com/help/4026616/windows-10-disk-cleanup) verwenden, um Speicherplatz freizugeben.
1. Öffnen Sie eine Instanz der Eingabeaufforderung mit erhöhten Rechten (als Administrator ausführen), und führen Sie eine Defragmentierung des Laufwerks durch:

   ``
   defrag <LETTER ASSIGNED TO THE OS DISK>: /u /x /g
   ``
   
   - Je nach dem Grad der Fragmentierung kann die Defragmentierung mehrere Stunden dauern.
   - Ersetzen Sie im Befehl `<LETTER ASSIGNED TO THE OS DISK>` durch den Buchstaben des Datenträgers mit dem Betriebssystem (z. B. Laufwerk *F:* ).

### <a name="clean-out-bad-memory-from-the-boot-configuration-data-bcd-store"></a>Löschen von ungültigem Speicher im BCD-Speicher (Boot Configuration Data, Startkonfigurationsdaten)

1. Öffnen Sie eine Eingabeaufforderung mit erhöhten Rechten („Als Administrator ausführen“).
1. Fragen Sie die Startkonfigurationsdatei mit dem folgenden Befehl nach ungültigen Speicherflags ab:

   ``
   bcdedit /store <LETTER ASSIGNED TO THE OS DISK>:\boot\bcd /enum {badmemory}
   ``
   
   - Ersetzen Sie im Befehl `<LETTER ASSIGNED TO THE OS DISK>` durch den Buchstaben des Datenträgers mit dem Betriebssystem (z. B. Laufwerk *F:* ).

1. Wenn die Abfrage keine ungültigen Speicherblöcke enthält, fahren Sie mit der nächsten Aufgabe fort. Andernfalls fahren Sie mit Schritt 4 fort.
1. Wenn ungültige Speicherblöcke vorhanden sind, verhindern diese Blöcke die Erstellung eines RAMDISK-Geräts und müssen mit dem folgenden Befehl gelöscht werden:

   ``
   bcdedit /store <LETTER ASSIGNED TO THE OS DISK>:\boot\bcd /deletevalue {badmemory} badmemorylist
   ``
   
   - Ersetzen Sie im Befehl `<LETTER ASSIGNED TO THE OS DISK>` durch den Buchstaben des Datenträgers mit dem Betriebssystem (z. B. Laufwerk *F:* ).

### <a name="restore-the-page-file-to-its-default-location"></a>Wiederherstellen der Auslagerungsdatei am Standardspeicherort

In der Auslagerungsdatei werden Daten in Form eines Überlaufs oder einer Sicherung gespeichert, die nicht im Arbeitsspeicher (RAM) des Computers gespeichert werden können. Die Datei kann statt im temporären Laufwerk auf einer virtuellen Festplatte (dem standardmäßigen Azure-Standort) gehostet werden. Wenn der Wert TRUE lautet, kann auf die Datei möglicherweise nicht zugegriffen werden. Dann muss sie am Standardspeicherort wiederhergestellt werden.

Bevor Sie irgendwelche Schritte ausführen, sollten Sie eine Kopie des Ordners **\windows\system32\config** auf einem fehlerfreien Datenträger erstellen. Mit diesem Schritt wird sichergestellt, dass unerwünschte Änderungen rückgängig gemacht werden können. Sie arbeiten an wichtigen Systemdateien. Daher wird diese Vorsichtsmaßnahme dringend empfohlen.

1. Geben Sie in der Windows-Suche **regedit** ein, und öffnen Sie die Anwendung Registrierungs-Editor.
1. Markieren Sie im Registrierungs-Editor den Schlüssel **HKEY_LOCAL_MACHINE**, und wählen Sie im Menü **Datei > Struktur laden...** aus.

   ![Das Menü „Struktur laden“ im Registrierungs-Editor](./media/troubleshoot-windows-stop-error/4.png)

1. Wählen Sie im Dialogfeld „Struktur laden“ **\windows\system32\config\SYSTEM** aus, und klicken Sie auf „Öffnen“.
   1. Wenn Sie aufgefordert werden, einen Namen einzugeben, geben Sie **BROKENSYSTEM** ein. Dieser Name hilft Ihnen bei der Problembehandlung die betroffenen Strukturen abzugrenzen.
   1. Erweitern Sie den Schlüssel **HKEY_LOCAL_MACHINE**, um den neuen BROKENSYSTEM-Schlüssel anzuzeigen, den Sie hinzugefügt haben.
1. Legen Sie über den Registrierungs-Editor fest, über welchen ControlSet-Schlüssel der Computer gestartet wird.
   1. Navigieren Sie zu **HKEY_LOCAL_MACHINE >> BROKENSYSTEM >> Auswählen**.
   1. Notieren Sie sich in den aufgelisteten Schlüsseln den Datenwert von „Current“. Wenn dieser Wert beispielsweise **1** oder **0x00000001 (1)** lautet, wird der ControlSet-Schlüssel „ControlSet001“ verwendet.
1. Überprüfen Sie den Speicherort, der für die Erstellung der Auslagerungsdatei konfiguriert wurde.
   1. Erweitern Sie im Schlüssel „HKEY_LOCAL_MACHINE\BROKENSYSTEM“ das Verzeichnis, das der ControlSet-Nummer entspricht, die Sie in Schritt 4 ermittelt haben, z. B. **ControlSet001**.
   1. Navigieren Sie zu **Control (Steuerung) >> Sitzungs-Manager >> Speicherverwaltung**, und notieren Sie sich den Speicherort des Schlüssels **ExistingPageFiles**.
   1. Dieser Schlüssel sollte sich am standardmäßigen Azure-Speicherort des temporären Laufwerks befinden. Wenn er dort nicht vorhanden ist und sich stattdessen auf einer virtuellen Festplatte an einem anderen Speicherort wie etwa auf dem Datenträgerlaufwerk oder auf dem Betriebssystemlaufwerk befindet, muss er gelöscht werden.
   1. Navigieren Sie zu diesem Speicherort im Datei-Explorer, und löschen Sie die Datei **pagefile.sys**.

### <a name="enable-the-serial-console-and-memory-dump-collection"></a>Aktivieren der seriellen Konsole und der Speicherabbilderfassung

**Empfohlen:** Aktivieren Sie die serielle Konsole und die Speicherabbilderfassung, bevor Sie die VM neu erstellen, indem Sie das folgende Skript ausführen:

Führen Sie das folgende Skript aus, um die Speicherabbildsammlung und die serielle Konsole zu aktivieren:

1. Öffnen Sie eine Eingabeaufforderungssitzung mit erhöhten Rechten als Administrator.
1. Listen Sie die Daten im BCD-Speicher auf, und bestimmen Sie den Bezeichner des Startladeprogramms, das Sie im nächsten Schritt verwenden.

   1. Geben Sie für eine VM der Generation 1 den folgenden Befehl ein, und notieren Sie sich den angezeigten Bezeichner:
   
      ``
      bcdedit /store <BOOT PARTITON>:\boot\bcd /enum
      ``
   
   - Ersetzen Sie im Befehl `<BOOT PARTITON>` durch den Buchstaben der Partition auf dem angefügten Datenträger, der den Startordner enthält.

      ![Die Ausgabe mit dem BCD-Speicher in einer VM der 1. Generation, in der unter dem Windows-Startladeprogramm die Bezeichnernummer angezeigt wird](./media/troubleshoot-windows-stop-error/5.png)

   1. Geben Sie für eine VM der Generation 2 den folgenden Befehl ein, und notieren Sie sich den aufgelisteten Bezeichner:
   
      ``
      bcdedit /store <LETTER OF THE EFI SYSTEM PARTITION>:EFI\Microsoft\boot\bcd /enum
      ``
   
   - Ersetzen Sie im Befehl `<LETTER OF THE EFI SYSTEM PARTITION>` durch den Buchstaben der EFI-Systempartition.
   - Es kann hilfreich sein, die Datenträgerverwaltungskonsole zu starten, um die entsprechende als **EFI-Systempartition** bezeichnete Systempartition zu ermitteln.
   - Beim Bezeichner kann es sich um eine eindeutige GUID oder die Standardpartition **bootmgr** handeln.

1. Führen Sie die folgenden Befehle aus, um die serielle Konsole zu aktivieren:

   ```
   bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /ems {<BOOT LOADER IDENTIFIER>} ON 
   bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200
   ```
   
   - Ersetzen Sie im Befehl `<VOLUME LETTER WHERE THE BCD FOLDER IS>` durch den Buchstaben des BCD-Ordners.
   - Ersetzen Sie im Befehl `<BOOT LOADER IDENTIFIER>` durch den Bezeichner, den Sie im vorherigen Schritt ermittelt haben.

1. Überprüfen Sie, ob der freie Speicherplatz auf dem Betriebssystemdatenträger größer als die Größe des Arbeitsspeichers (RAM) auf der VM ist.

   Wenn nicht genügend Speicherplatz auf dem Betriebssystemdatenträger vorhanden ist, ändern Sie den Speicherort für das Erstellen der Speicherabbilddatei, und verweisen Sie auf einen an die VM angefügten Datenträger, der über genügend freien Speicherplatz verfügt. Um den Speicherort zu ändern, ersetzen Sie in den folgenden Befehlen **%SystemRoot%** durch den Laufwerksbuchstaben des Datenträgers, beispielsweise Laufwerk **F:** .

   Empfohlene Konfiguration für das Aktivieren des Betriebssystemabbilds:

   **Laden der Registrierungsstruktur vom beschädigten Betriebssystem-Datenträger:**

   ```
   REG LOAD HKLM\BROKENSYSTEM <VOLUME LETTER OF BROKEN OS DISK>:\windows\system32\config\SYSTEM
   ```

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

   ```
   REG UNLOAD HKLM\BROKENSYSTEM
   ```
   
### <a name="rebuild-the-vm"></a>Neuerstellen der VM

Verwenden Sie [Schritt 5 der VM-Reparaturbefehle](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands#repair-process-example), um die VM neu zu erstellen.
