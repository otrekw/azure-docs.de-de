---
title: Problembehandlung beim Windows-Start-Manager-Fehler 0xC0000225 „Status not found“
description: Schritte zum Beheben von Problemen, bei denen der Fehlercode 0xc0000225 auf einer Azure-VM auftritt.
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: c17899a4-b270-4725-9530-0dcd829b178c
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 05/11/2020
ms.author: v-miegge
ms.openlocfilehash: 3677d67f55cfccdc80245b2ec870ffa76b0a1940
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87088664"
---
# <a name="troubleshoot-windows-boot-manager-error----0xc0000225-status-not-found"></a>Problembehandlung beim Windows-Start-Manager-Fehler 0xC0000225 „Status not found“
 
In diesem Artikel finden Sie Schritte zum Beheben von Problemen, bei denen Fehlercode 0xc0000225 auf einem virtuellen Azure-Computer auftritt. Dieser Fehler gibt an, dass der Status oder das Objekt nicht gefunden wurde.

## <a name="symptoms"></a>Symptome

Wenn Sie die [Startdiagnose](./boot-diagnostics.md) verwenden, um den Screenshot des virtuellen Computers anzuzeigen, wird im Screenshot der Windows-Startfehler mit dem Statuscode *0xc0000225* angezeigt.

Die diesem Fehlercode zugeordnete Datei informiert Sie über die erforderlichen Schritte zum Beheben dieses Problems. Suchen Sie den Text im Abschnitt **File:** (Datei:), um die entsprechende Vorgehensweise zu bestimmen.

### <a name="drivers-os-related-or-third-party"></a>Treiber, mit dem Betriebssystem oder einem Drittanbieter zusammenhängend

Wenn die Datei vorhanden ist, aber auf einen Treiber verweist (wie angezeigt) oder mit dem Betriebssystem oder einen Drittanbieter zusammenhängt, befolgen Sie die Schritte im Abschnitt [Reparieren der Systemdatei](#repair-the-system-file).
 
In der folgenden Abbildung gibt der Windows-Start-Manager an „Windows konnte nicht gestartet werden. Dies kann auf eine Hardware- oder Softwareänderung zurückzuführen sein“. Die Abbildung zeigt außerdem den Status als „0xc0000225“, die **Datei:** als `\windows\System32\drivers\atapi.sys` und **Info:** als „The operating system couldn’t be loaded because a critical system driver is missing or contains errors“ (Das Betriebssystem konnte nicht geladen werden, da ein kritischer Systemtreiber fehlt oder Fehler aufweist) an.

![Der Windows-Start-Manager gibt an ‚Windows konnte nicht gestartet werden. Dies kann auf eine Hardware- oder Softwareänderung zurückzuführen sein‘. Außerdem werden der Status als „0xc0000225“ und die Datei als „\windows\System32\drivers\atapi.sys“ angezeigt, und der Infoabschnitt besagt: „Das Betriebssystem konnte nicht geladen werden, da ein kritischer Systemtreiber fehlt oder Fehler aufweist“.](./media/troubleshoot-boot-error-status-not-found/1.png)

### <a name="no-file"></a>Keine Datei

Wenn der Statuscode angezeigt wird, dazu aber keine Datei angegeben wird, befolgen Sie die Schritte unter dem Abschnitt [Hinzufügen der OSDEVICE-Variable](#add-the-osdevice-variable).

In der folgenden Abbildung gibt der Windows-Start-Manager an „Windows konnte nicht gestartet werden. Dies kann auf eine Hardware- oder Softwareänderung zurückzuführen sein“. Die Abbildung zeigt den Status ebenfalls als „0xc0000225“ und die **Info:** als „Fehler bei Startauswahl. Zugriff auf ein erforderliches Gerät nicht möglich“ an.

![Der Windows-Start-Manager gibt an ‚Windows konnte nicht gestartet werden. Dies kann auf eine Hardware- oder Softwareänderung zurückzuführen sein‘. Außerdem wird der Status als „0xc0000225“ angezeigt und im Infoabschnitt angegeben: „Fehler bei der Startauswahl. Zugriff auf ein erforderliches Gerät nicht möglich“.](./media/troubleshoot-boot-error-status-not-found/2.png)

### <a name="registry-file"></a>Registrierungsdatei

Wenn auf eine der Registrierungsdateien verwiesen wird, etwa auf „\windows\system32\config\system“, führen Sie die Schritte im Abschnitt [Erstellen eines Supporttickets](#contact-support) aus.
 
In der folgenden Abbildung gibt der Windows-Start-Manager an „Windows konnte nicht gestartet werden. Dies kann auf eine Hardware- oder Softwareänderung zurückzuführen sein“. Die Abbildung zeigt darüber hinaus den Status als „0xc0000225“, die Datei als `\windows\System32\config\system` und **Info:** als „The operating system couldn’t be loaded because the system registry file is missing or contains errors“ (Fehler beim Laden des Betriebssystems, da die Systemregistrierungsdatei fehlt oder Fehler enthält) an.

![Der Windows-Start-Manager gibt an ‚Windows konnte nicht gestartet werden. Dies kann auf eine Hardware- oder Softwareänderung zurückzuführen sein‘. Darüber hinaus wird der Status als „0xc0000225“, die Datei als ‚\windows\System32\config\system‘ angezeigt und im Infoabschnitt angegeben: „The operating system couldn’t be loaded because the system registry file is missing or contains errors“ (Fehler beim Laden des Betriebssystems, da die Systemregistrierungsdatei fehlt oder Fehler enthält).](./media/troubleshoot-boot-error-status-not-found/3.png)

In der folgenden Abbildung besagt der Wiederherstellungsbildschirm „Your PC/Device needs to be repaired. The operating system couldn’t be loaded because the system registry file is missing or contains errors“ (Der PC/das Gerät muss repariert werden. Fehler beim Laden des Betriebssystems, da die Systemregistrierungsdatei fehlt oder Fehler enthält). Die Abbildung zeigt darüber hinaus den Fehlercode als „0xc0000225“ und die Datei als `\windows\System32\config\system` an.

![Der Wiederherstellungsbildschirm besagt ‚Your PC/Device needs to be repaired. The operating system couldn’t be loaded because the system registry file is missing or contains errors‘ (Der PC/das Gerät muss repariert werden. Fehler beim Laden des Betriebssystems, da die Systemregistrierungsdatei fehlt oder Fehler enthält). Die Abbildung zeigt darüber hinaus den Fehlercode als ‚0xc0000225‘ und die Datei als ‚\windows\System32\config\system‘ an.](./media/troubleshoot-boot-error-status-not-found/4.png)

## <a name="causes"></a>Ursachen

### <a name="missing-binary"></a>Fehlende Binärdatei

Möglicherweise kommt es unter Ihren Systemdateien **(.sys)** zu einer fehlenden oder beschädigten Binärdatei.

### <a name="bcd-corruption-or-improper-vhd-migration"></a>BCD-Beschädigung oder fehlerhafte VHD-Migration

In diesem Fall sind entweder die **Startkonfigurationsdaten (Boot Configuration Data, BCD)** beschädigt, oder die **virtuelle Festplatte (VHD)** wurde vom lokalen System migriert, dabei jedoch nicht ordnungsgemäß vorbereitet. Das hat zur Folge, dass die **OSDEVICE**-Variable fehlt und hinzugefügt werden muss.

### <a name="registry-hive-corruption"></a>Beschädigung der Registrierungsstruktur

Eine Beschädigung der Registrierungsstruktur kann folgende Ursachen haben:

- Die Struktur fällt aus
- Die Struktur wird eingebunden, ist aber leer
- Die Struktur wurde nicht ordnungsgemäß geschlossen
## <a name="solution"></a>Lösung

### <a name="process-overview"></a>Prozessübersicht

1. Erstellen Sie eine Reparatur-VM, und greifen Sie darauf zu.
1. Wählen Sie eine Lösung:
   - [Reparieren der Systemdatei](#repair-the-system-file)
   - [Hinzufügen der OSDevice-Variable](#add-the-osdevice-variable)
   - [Erstellen eines Supporttickets](#contact-support)
1. Aktivieren Sie die serielle Konsole und die Speicherabbilderfassung.
1. Erstellen Sie den virtuellen Computer neu.

### <a name="create-and-access-a-repair-vm"></a>Erstellen einer Reparatur-VM und Zugreifen darauf

1. Führen Sie die Schritte 1-3 der [Reparaturbefehle für virtuelle Computer](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md) aus, um eine Reparatur-VM vorzubereiten.
1. Stellen Sie über eine Remotedesktopverbindung eine Verbindung mit der Reparatur-VM her.

### <a name="select-a-solution"></a>Auswählen einer Lösung

1. Öffnen Sie eine Eingabeaufforderung mit erhöhten Rechten.
1. Führen Sie ausgehend vom zuvor identifizierten Symptom die Schritte der entsprechenden Lösung aus. Sie können die Schritte in den anderen Lösungen überspringen, da sie nicht auf Ihr Problem zutreffen:

- [Reparieren der Systemdatei](#repair-the-system-file)
- [Hinzufügen der OSDevice-Variable](#add-the-osdevice-variable)
- [Erstellen eines Supporttickets](#contact-support)

### <a name="repair-the-system-file"></a>Reparieren der Systemdatei

1. Navigieren Sie mithilfe der angefügten VHD-Datei zum Speicherort der Binärdatei, die auf dem Screenshot des virtuellen Computers (VM) angezeigt wird.
1. Klicken Sie mit der rechten Maustaste auf die Datei, wählen Sie **Eigenschaften** und dann die Registerkarte **Details** aus, um die Informationen zur Datei anzuzeigen.
   1. Notieren Sie die Version der Datei, wie in der Abbildung unten dargestellt:

      ![Eigenschaftenfenster für die Datei ‚cng.sys‘ mit hervorgehobener Dateiversion.](./media/troubleshoot-boot-error-status-not-found/5.png)

1. Benennen Sie die Datei in **< BINARY.SYS >.old** um, wobei Sie **< BINARY.SYS >** durch den Namen der Datei ersetzen.

   Für die Abbildung im Schritt oben würde die Datei **cng.sys** in **cng.sys.old** umbenannt.

   > [!NOTE]
   > Wenn Sie versuchen, die Datei umzubenennen und dabei die Meldung „Die Datei ist beschädigt und kann nicht gelesen werden“ ausgegeben wird, [bitten Sie den Support um Hilfe](https://azure.microsoft.com/support/create-ticket/), da diese Lösung nicht funktionieren wird.

1. Nachdem Sie jetzt die beschädigte Datei umbenannt haben, reparieren Sie die Datei, indem Sie sie aus ihrem internen Repository wiederherstellen.
   1. Starten Sie eine **CMD**-Sitzung.
   1. Navigieren Sie zu **\windows\winsxs**.

   1. Suchen Sie nach der Binärdatei, die sich am Anfang dieses Abschnitts befindet, und verwenden Sie dazu den folgenden Befehl:

      `dir <BINARY WITH ".SYS" EXTENSION>  /s`

      Mit diesem Befehl werden alle Versionen der Datei aufgelistet, die sich auf dem Computer befinden, und Sie erhalten den Pfadverlauf dieser Komponente.
      
      Beispielsweise müssten Sie **dir cng.sys** in **dir cng.sys /s** umbenennen.

   1. Wählen Sie die neueste Version der Datei auf der Liste (oder eine andere, die Sie bevorzugen) aus, und kopieren Sie die Datei in den Ordner **windows\system32**. Verwenden Sie dazu den vorherigen Pfad und folgenden Befehl:

      `copy <drive>:\Windows\WinSxS\<DIRECTORY WHERE FILE IS>\<BINARY WITH ".SYS" EXTENSION> <DRIVE>:\Windows\System32\Drivers\`

      > [!NOTE]
      > Wenn die letzte Binärdatei nicht funktioniert, versuchen Sie eine Version vor dieser oder eine beliebige Version, von der Sie wissen, dass es sich um eine stabile Datei handelt, beispielsweise eine Version vor einem Patch.

      Wenn beispielsweise die gesuchte Binärdatei **cmimcext.sys** und das fehlerhafte Laufwerk **F:** ist, und Sie gerade eine Suche nach der aktuellen Version durchgeführt haben, würden Sie das folgende Bild sehen, bei dem eine Abfrage von `dir cmim* /s` in einer Eingabeaufforderung die neueste Version der Datei „cmimcext.sys“ findet.

      ![Eine Abfrage von „dir cmim* /s“ in der Eingabeaufforderung zum Auffinden der neuesten Version der cmimcext.sys-Datei.](./media/troubleshoot-boot-error-status-not-found/6.png)

      In der Beispielabbildung oben wurde die Abfrage auf **C:** ausgeführt, während der Laufwerkbuchstabe der des fehlerhaften Laufwerks **F:** sein sollte, nämlich des Betriebssystem-Datenträgers, der als Datenträger an die Reparatur-VM angeschlossen ist.

      Der resultierende Befehl zum Kopieren der Datei wäre: `copy F:\Windows\WinSxS\amd64_xxxxxx\cmimcext.sys F:\Windows\System32\Drivers`.

Nachdem diese Aufgabe abgeschlossen ist, fahren Sie mit [Aktivieren der seriellen Konsole und der Speicherabbilderfassung](#enable-the-serial-console-and-memory-dump-collection) fort.

### <a name="add-the-osdevice-variable"></a>Hinzufügen der OSDEVICE-Variable

Sammeln Sie die Informationen zur aktuellen Starteinrichtung, und notieren Sie den Bezeichner der aktiven Partition. Sie benötigen diese Informationen später zum Hinzufügen der **OSDEVICE**-Variablen im Anschluss an die Anweisungen für die Generierung Ihrer VM.

Wenn diese Informationssammlung zu einem Fehler wegen einer fehlenden **\boot\bcd**-Datei führt, befolgen Sie stattdessen die Anweisungen unter [Reparieren der Systemdatei](#repair-the-system-file). 

1. Öffnen Sie für VMs der ersten Generation eine Eingabeaufforderung mit erhöhten Rechten als Administrator, und geben Sie den folgenden Befehl ein:

   `bcdedit /store <LETTER OF BOOT FOLDER>:\boot\bcd /enum`

   Diese Abbildung zeigt das Windows-Startladeprogramm in einer VM der ersten Generation mit hervorgehobenem Bezeichnerattribut. Das hervorgehobene Bezeichnerattribut stellt eine eindeutige alphanumerische Zeichenfolge dar.

   ![Windows-Startladeprogramm, dargestellt in einer VM der ersten Generation, mit hervorgehobenem Bezeichnerattribut. Das hervorgehobene Bezeichnerattribut stellt eine eindeutige alphanumerische Zeichenfolge dar.](./media/troubleshoot-boot-error-status-not-found/7.png)

   Notieren Sie den Bezeichner des Windows-Startladeprogramms, dessen Pfad **\windows\system32\winload.exe** ist.

1. Überprüfen Sie bei VMs der zweiten Generation sowohl, ob der Betriebssystem-Datenträger online ist als auch, ob seinen Partitionen Laufwerksbuchstaben zugewiesen wurden. Nachdem dies überprüft wurde, erfassen Sie die Informationen zur Starteinrichtung.
   1. Geben Sie in der **Windows-Suche** **Datenträgerverwaltung** ein, und öffnen Sie die Konsole der Datenträgerverwaltung. Verwenden Sie diese Konsole, um die Datenträgernummer des an Ihre Reparatur-VM angefügten Datenträgers und die EFI-Partition (Extensible Firmware Interface) zu bestimmen, die den BCD-Speicher enthält.

   In der folgenden Abbildung ist Datenträger 2 die Nummer des an Ihre Reparatur-VM angefügten Datenträgers. Die Abbildung zeigt außerdem die EFI-Systempartition auf Datenträger 2, die 100 MB groß ist und der kein Laufwerksbuchstabe zugeordnet ist.

   ![Datenträger 2 wird als Datenträgernummer des an die Reparatur-VM angefügten Datenträgers angezeigt. Außerdem wird die EFI-Systempartition auf Datenträger 2 angezeigt, die 100 MB groß ist und der kein Laufwerksbuchstabe zugeordnet ist.](./media/troubleshoot-boot-error-status-not-found/8.png)

   1. Öffnen Sie eine Eingabeaufforderung mit erhöhten Rechten als Administrator, und geben Sie die folgenden Befehle ein:
      1. Öffnen Sie das **DISKPART-TOOL** mit dem Befehl `diskpart`.
      1. Listen Sie alle Datenträger auf, und wählen Sie dann den angefügten Datenträger aus, der im vorherigen Schritt identifiziert wurde:
      
         ```
         list disk
         sel disk <DISK #>
         ```

         Die folgende Abbildung zeigt das Ergebnis der Auflistung und Auswahl eines Datenträgers. Datenträger 0 (127 GB / Online), Datenträger 1 (32 GB / Online) und Datenträger 2 (127 GB / Online) werden aufgelistet, Datenträger 2 wurde mithilfe des Befehls `sel disk 2` ausgewählt.

         ![Das Ergebnis der Auflistung und anschließenden Auswahl eines Datenträgers. Datenträger 0 (127 GB | Online), Datenträger 1 (32 GB | Online) und Datenträger 2 (127 GB | Online) werden aufgelistet, wobei Datenträger 2 ausgewählt ist.](./media/troubleshoot-boot-error-status-not-found/9.png)

      1. Listen Sie die Partitionen auf, und wählen Sie die im vorherigen Schritt bestimmte EFI-Systempartition aus:
      
         ```
         list partition
         sel partition <PARTITION #>
         ```

         Die folgende Abbildung zeigt das Ergebnis der Auflistung und Auswahl einer Partition. Partition 1 (Reserviert | 16 MB), Partition 2 (System | 100 MB) und Partition 3 (Primär | 126 GB) werden aufgelistet, Partition 2 wurde mithilfe des Befehls `sel part 2` ausgewählt.

         ![Das Ergebnis der Auflistung und anschließenden Auswahl einer Partition. Partition 1 (Reserviert | 16 MB), Partition 2 (System | 100 MB) und Partition 3 (Primär | 126 GB) werden aufgelistet, mit ausgewählter Partition 2.](./media/troubleshoot-boot-error-status-not-found/10.png)

      1. Weisen Sie der EFI-Partition mithilfe des Befehls `assign` einen Buchstaben zu.

         In der folgenden Abbildung sind der `assign`-Befehl und das neue Laufwerk **SYSTEM (F:)** beide im Datei-Explorer zu sehen.

         ![Der Assign-Befehl und das neue Laufwerk SYSTEM (F:) können beide im Datei-Explorer angezeigt werden.](./media/troubleshoot-boot-error-status-not-found/11.png)

      1. Listen Sie die BCD-Speicherdaten mit dem folgenden Befehl auf:
      
         `bcdedit /store <LETTER OF EFI SYSTEM PARTITION>:EFI\Microsoft\boot\bcd /enum`

         In der folgenden Abbildung wird das Windows-Startladeprogramm in einer VM der zweiten Generation mit hervorgehobenem Bezeichnerattribut dargestellt. Das hervorgehobene Bezeichnerattribut weist den Wert **{default}** (Standard) auf.

         ![Windows-Startladeprogramm, dargestellt in einer VM der zweiten Generation, mit hervorgehobenem Bezeichnerattribut. Das hervorgehobene Bezeichnerattribut zeigt den Standardwert als Wert an.](./media/troubleshoot-boot-error-status-not-found/12.png)

         Notieren Sie den Bezeichner des Windows-Startladeprogramms, dessen Pfad **\windows\system32\winload.efi** ist.

1. Beachten Sie, dass die OSDEVICE-Variable für die aktive Partition fehlt:

   ![Die Attribute des Windows-Start-Managers und des Windows-Startladeprogramms werden in der Eingabeaufforderung aufgelistet, mit fehlendem Attribut für das Betriebssystemgerät.](./media/troubleshoot-boot-error-status-not-found/13.png)
   
   Auf dieser Abbildung sind die Attribute des Windows-Start-Managers und des Windows-Startladepprogramms in der Eingabeaufforderung aufgelistet, aber das OSDEVICE-Attribut fehlt.

1. Fügen Sie die OSDEVICE-Variable auf der Grundlage der folgenden Informationen hinzu:

   Fügen Sie für Betriebssystem-Datenträger mit einer einzelnen Partition `BOOT` hinzu.

   > [!NOTE]
   > Der startfähige Ordner befindet sich auf der gleichen Partition wie der Windows-Ordner **\windows-Ordner**.
   > - Der startfähige Ordner für VMs der ersten Generation ist **(\boot\bcd-Ordner)** .
   > - Der startfähige Ordner für VMs der zweiten Generation ist **EFI\Microsoft\boot\bcd**.

   Geben Sie für VMs der ersten Generation den folgenden Befehl ein:

   `bcdedit /store <LETTER OF BOOT FOLDER>:\boot\bcd /set {<IDENTIFIER>} OSDEVICE BOOT`

   Geben Sie für VMs der zweiten Generation den folgenden Befehl ein:

   `bcdedit /store <LETTER OF EFI FOLDER>:EFI\Microsoft\boot\bcd /set {<IDENTIFIER>} OSDEVICE BOOT`

   Fügen Sie für Betriebssystem-Datenträger mit mehreren Partitionen `PARTITION=<LETTER OF WINDOWS FOLDER>:` hinzu.

   > [!NOTE]
   > Der startfähige Ordner befindet sich höchstwahrscheinlich auf einer anderen Partition als der Windows-Ordner **\windows-Ordner**.
   > - Der startfähige Ordner für VMs der ersten Generation ist **(\boot\bcd-Ordner)** .
   > - Der startfähige Ordner für VMs der zweiten Generation ist **EFI\Microsoft\boot\bcd**.

   Geben Sie für VMs der ersten Generation den folgenden Befehl ein:

   `bcdedit /store <LETTER OF BOOT FOLDER>:\boot\bcd /set {<IDENTIFIER>} OSDEVICE partition=<LETTER OF WINDOWS FOLDER>:`

   Geben Sie für VMs der zweiten Generation den folgenden Befehl ein:

   `bcdedit /store <LETTER OF EFI FOLDER>:EFI\Microsoft\boot\bcd /set {< IDENTIFIER>} OSDEVICE partition=<LETTER OF WINDOWS FOLDER>:`

Nachdem diese Aufgabe abgeschlossen ist, fahren Sie mit [Aktivieren der seriellen Konsole und der Speicherabbilderfassung](#enable-the-serial-console-and-memory-dump-collection) fort.

### <a name="contact-support"></a>Kontaktieren des Supports

Für den **Registrierungsdatei**-Fehler gibt es eine Lösung, Sie müssen aber [ein Supportticket erstellen](https://azure.microsoft.com/support/create-ticket/), um weitere Unterstützung zu erhalten.

### <a name="enable-the-serial-console-and-memory-dump-collection"></a>Aktivieren der seriellen Konsole und der Speicherabbilderfassung

**Empfohlen:** Aktivieren Sie die serielle Konsole und die Speicherabbilderfassung, bevor Sie die VM neu erstellen, indem Sie das folgende Skript ausführen:

1. Öffnen Sie eine Eingabeaufforderungssitzung mit erhöhten Rechten als Administrator.
1. Führen Sie die folgenden Befehle aus:

   **Aktivieren der seriellen Konsole**:
   
   ```
   bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /ems {<BOOT LOADER IDENTIFIER>} ON 
   bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200
   ```

1. Überprüfen Sie, ob der freie Speicherplatz auf dem Betriebssystemdatenträger größer als die Größe des Arbeitsspeichers (RAM) auf der VM ist.

   Wenn nicht genügend Speicherplatz auf dem Betriebssystemdatenträger vorhanden ist, ändern Sie den Speicherort für das Erstellen der Speicherabbilddatei, und verweisen Sie auf einen an die VM angefügten Datenträger, der über genügend freien Speicherplatz verfügt. Um den Speicherort zu ändern, ersetzen Sie in den folgenden Befehlen **%SystemRoot%** durch den Laufwerksbuchstaben des Datenträgers, beispielsweise **F:** .

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

Verwenden Sie [Schritt 5 der VM-Reparaturbefehle](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md#repair-process-example), um die VM neu zu erstellen.
