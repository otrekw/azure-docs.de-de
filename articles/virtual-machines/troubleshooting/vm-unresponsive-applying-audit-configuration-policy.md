---
title: Der virtuelle Computer reagiert nicht, während die Richtlinie zur Überwachungsrichtlinienkonfiguration angewendet wird
description: In diesem Artikel sind die Schritte beschrieben, um Probleme zu behandeln, bei denen ein virtueller Computer nicht mehr reagiert, wenn die Richtlinie zur Überwachungsrichtlinienkonfiguration angewendet wird. Durch diesen Vorgang wird das Starten einer Azure-VM verhindert.
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: 3f6383b5-81fa-49ea-9434-2fe475e4cbef
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 08/24/2020
ms.author: v-miegge
ms.openlocfilehash: ff21975c34c28d7476635467e0c1abb8e6575e35
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2020
ms.locfileid: "91977951"
---
# <a name="virtual-machine-is-unresponsive-while-applying-audit-policy-configuration-policy"></a>Der virtuelle Computer reagiert nicht, während die Richtlinie zur Überwachungsrichtlinienkonfiguration angewendet wird

In diesem Artikel sind die Schritte beschrieben, um Probleme zu behandeln, bei denen ein virtueller Computer nicht mehr reagiert, wenn die Richtlinie zur Überwachungsrichtlinienkonfiguration angewendet wird. Durch diesen Vorgang wird das Starten einer Azure-VM verhindert.

## <a name="symptom"></a>Symptom

Wenn Sie die [Startdiagnose](./boot-diagnostics.md) zum Anzeigen des VM-Screenshots verwenden, sehen Sie, dass das Betriebssystem beim Start nicht mehr reagiert hat und die Meldung **Richtlinie zur Überwachungsrichtlinienkonfiguration wird angewendet** angezeigt wurde.

  ![Start des Betriebssystems mit folgender Meldung: „Richtlinie zur Überwachungsrichtlinienkonfiguration wird angewendet“](./media/vm-unresponsive-applying-audit-configuration-policy/1.png)

  ![Start des Betriebssystems unter Windows Server 2012 mit folgender Meldung: „Richtlinie zur Überwachungsrichtlinienkonfiguration wird angewendet“](./media/vm-unresponsive-applying-audit-configuration-policy/2.png)

## <a name="cause"></a>Ursache

Es gibt widersprüchliche Sperren, wenn die Richtlinie versucht, alte Benutzerprofile zu bereinigen.

> [!NOTE]
> Dies betrifft nur Windows Server 2012 und Windows Server 2012 R2.

Dies ist die problematische Richtlinie: *Computerkonfiguration\Richtlinien\Administrative Vorlagen\System/Benutzerprofile\Benutzerprofile, die älter als eine bestimmte Anzahl von Tagen sind, beim Systemneustart löschen.*

## <a name="solution"></a>Lösung

### <a name="process-overview"></a>Übersicht über den Prozess

1. Erstellen Sie eine Reparatur-VM, und greifen Sie darauf zu.
1. Deaktivieren der Richtlinie
1. Aktivieren Sie die serielle Konsole und die Speicherabbilderfassung.
1. Erstellen Sie den virtuellen Computer neu.
1. Erfassen Sie eine Speicherabbilddatei, und erstellen Sie ein Supportticket.

### <a name="create-and-access-a-repair-vm"></a>Erstellen und Aufrufen einer Reparatur-VM

1. Führen Sie die Schritte 1-3 der [Reparaturbefehle für virtuelle Computer](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md) aus, um eine Reparatur-VM vorzubereiten.
1. Stellen Sie über eine Remotedesktopverbindung eine Verbindung mit der Reparatur-VM her.

### <a name="disable-the-policy"></a>Deaktivieren der Richtlinie

1. Öffnen Sie auf der Reparatur-VM den **Registrierungs-Editor**.
1. Wählen Sie den Schlüssel **HKEY_LOCAL_MACHINE** und dann im Menü **Datei > Struktur laden** aus.

   ![Navigation im Registrierungs-Editor zum Laden einer Struktur.](./media/vm-unresponsive-applying-audit-configuration-policy/3.png)

   - Über „Struktur laden“ können Sie Registrierungsschlüssel aus einem Offlinesystem laden. In diesem Fall ist das System der mit der Reparatur-VM verknüpfte beschädigte Datenträger.
   - Systemweite Einstellungen werden in **HKEY_LOCAL_MACHINE** gespeichert und können als **HKLM** abgekürzt werden.

1. Öffnen Sie auf dem angefügten Datenträger die Datei `\windows\system32\config\SOFTWARE`.

   - Geben Sie **BROKENSOFTWARE** ein, wenn Sie zur Eingabe eines Namens aufgefordert werden.
   - Erweitern Sie **HKEY_LOCAL_MACHINE**, und suchen Sie nach dem hinzugefügten Schlüssel **BROKENSOFTWARE**, um zu überprüfen, ob **BROKENSOFTWARE** geladen wurde.

1. Navigieren Sie zu **BROKENSOFTWARE**, und überprüfen Sie, ob der Schlüssel **CleanupProfiles** in der geladenen Struktur vorhanden ist.

   - Wenn der Schlüssel vorhanden ist, ist die Richtlinie **CleanupProfiles** festgelegt. Der Wert stellt die in der Aufbewahrungsrichtlinie festgelegte Dauer in Tagen dar.
   - Wenn der Schlüssel nicht vorhanden ist, ist die Richtlinie **CleanupProfiles** nicht festgelegt. In diesem Fall sollten Sie direkt [ein Supportticket mit einer Speicherabbilddatei erstellen](#collect-the-memory-dump-file-and-submit-a-support-ticket).

1. Löschen Sie den Schlüssel **CleanupProfiles** mit diesem Befehl:

   `reg delete "HKLM\BROKENSOFTWARE\Policies\Microsoft\Windows\System" /v CleanupProfiles /f`

1. Entladen Sie die Struktur **BROKENSOFTWARE** mit diesem Befehl:

   `reg unload HKLM\BROKENSOFTWARE`

### <a name="enable-the-serial-console-and-memory-dump-collection"></a>Aktivieren der seriellen Konsole und der Speicherabbilderfassung

**Empfohlen:** Aktivieren Sie die serielle Konsole und die Speicherabbilderfassung, bevor Sie die VM neu erstellen, indem Sie das folgende Skript ausführen:

1. Öffnen Sie eine Eingabeaufforderungssitzung mit erhöhten Rechten als Administrator.
1. Listen Sie die BCD-Speicherdaten auf, und bestimmen Sie den Bezeichner des Startladeprogramms, den Sie im nächsten Schritt verwenden werden.

   1. Geben Sie für eine VM der Generation 1 den folgenden Befehl ein, und notieren Sie sich den angezeigten Bezeichner:

      `bcdedit /store <BOOT PARTITON>:\boot\bcd /enum`

      - Ersetzen Sie im Befehl `<BOOT PARTITON>` durch den Buchstaben der Partition auf dem angefügten Datenträger, der den Startordner enthält.

        ![Abbildung 4 zeigt die Ausgabeliste des BCD-Speichers in einer VM der Generation 1. Dabei ist die Bezeichnernummer unterhalb des Windows-Startladeprogramms aufgeführt.](./media/vm-unresponsive-applying-audit-configuration-policy/4.png)

   1. Geben Sie für eine VM der Generation 2 den folgenden Befehl ein, und notieren Sie sich den aufgelisteten Bezeichner:

      `bcdedit /store <LETTER OF THE EFI SYSTEM PARTITION>:EFI\Microsoft\boot\bcd /enum`

      - Ersetzen Sie im Befehl `<LETTER OF THE EFI SYSTEM PARTITION>` durch den Buchstaben der EFI-Systempartition.
      - Es kann hilfreich sein, die Datenträgerverwaltungskonsole zu starten, um die entsprechende als **EFI-Systempartition** bezeichnete Systempartition zu ermitteln.
      - Beim Bezeichner kann es sich um eine eindeutige GUID oder die Standardpartition **bootmgr** handeln.

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
   
### <a name="rebuild-the-virtual-machine"></a>Neuerstellung des virtuellen Computers

1. Verwenden Sie [Schritt 5 der VM-Reparaturbefehle](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md#repair-process-example), um die VM neu zu erstellen.

1. Testen Sie, ob Ihre VM normal gestartet wird, um herauszufinden, ob das Problem behoben ist.

   - Wenn das Problem nicht behoben ist, fahren Sie mit den Schritten zum [Erfassen einer Speicherabbilddatei und Erstellen eines Supporttickets](#collect-the-memory-dump-file-and-submit-a-support-ticket) fort.
   - Wenn das Problem behoben ist, sind keine weiteren Schritte erforderlich.

Wenn das Problem behoben ist, ist die Richtlinie nun lokal deaktiviert. Für eine dauerhafte Lösung darf die CleanupProfiles-Richtlinie nicht für VMs verwendet werden, da durch diesen Vorgang automatisch Benutzerprofile gelöscht werden. Verwenden Sie für Profilbereinigungen eine andere Methode (z. B. eine geplante Aufgabe oder ein Skript).

**Verwenden Sie nicht diese Richtlinie:** 
*Computer\Administrative Vorlagen\System\Benutzerprofile\Benutzerprofile, die älter als eine bestimmte Anzahl von Tagen sind, beim Systemneustart löschen.*

### <a name="the-issue-should-now-be-fixed"></a>Das Problem sollte nun behoben sein.

Testen Sie Ihre VM, um eine normale Funktionsweise sicherzustellen. Wenn weiterhin Probleme auftreten, erhalten Sie im nächsten Abschnitt weitere Unterstützung.

### <a name="collect-the-memory-dump-file-and-submit-a-support-ticket"></a>Erfassen einer Speicherabbilddatei und Erstellen eines Supporttickets

Um dieses Problem zu behandeln, müssen Sie zunächst die Speicherabbilddatei für den Absturz erstellen und an den Support senden. Gehen Sie wie folgt vor, um die Speicherabbilddatei zu erfassen:

#### <a name="attach-the-os-disk-to-a-new-repair-vm"></a>Anfügen des Betriebssystemdatenträgers an eine neue Reparatur-VM

1. Führen Sie die Schritte 1-3 der [Reparaturbefehle für virtuelle Computer](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md) aus, um eine neue Reparatur-VM vorzubereiten.
1. Stellen Sie über eine Remotedesktopverbindung eine Verbindung mit der Reparatur-VM her.

#### <a name="locate-the-dump-file-and-submit-a-support-ticket"></a>Suchen nach der Speicherabbilddatei und Senden eines Supporttickets

1. Navigieren Sie auf der Reparatur-VM zum Windows-Ordner auf dem angefügten Betriebssystemdatenträger. Wenn der dem angefügten Betriebssystemdatenträger zugewiesene Laufwerkbuchstabe *F* lautet, müssen Sie zu `F:\Windows` navigieren.
1. Suchen Sie nach der Datei `memory.dmp`, und [senden Sie dann ein Supportticket](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) mit der Speicherabbilddatei.
1. Wenn Sie die Datei `memory.dmp` nicht finden können, verwenden Sie stattdessen [NMI-Aufrufe (Nicht maskierbarer Interrupt) in der seriellen Konsole](./serial-console-windows.md#use-the-serial-console-for-nmi-calls). Befolgen Sie den Leitfaden zum [Generieren eines Absturzabbilds mithilfe von NMI-Aufrufen](/windows/client-management/generate-kernel-or-complete-crash-dump).