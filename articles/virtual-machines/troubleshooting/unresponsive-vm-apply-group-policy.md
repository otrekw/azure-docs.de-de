---
title: Eine Azure-VM reagiert nicht beim Anwenden einer Richtlinie
description: In diesem Artikel werden die Schritte zum Beheben von Problemen beschrieben, bei denen der Ladebildschirm beim Anwenden einer Richtlinie während des Starts einer Azure-VM nicht mehr reagiert.
services: virtual-machines-windows
documentationcenter: ''
author: TobyTu
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: a97393c3-351d-4324-867d-9329e31b5628
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.date: 05/07/2020
ms.author: v-mibufo
ms.openlocfilehash: 30f833bc49f92dcabfc75f0a1507c6f540bdea24
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2020
ms.locfileid: "83748729"
---
# <a name="vm-becomes-unresponsive-while-applying-group-policy-local-users--groups-policy"></a>VM reagiert beim Anwenden der Gruppenrichtlinie für lokale Benutzer und Gruppen nicht

In diesem Artikel werden die Schritte zum Beheben von Problemen beschrieben, bei denen der Ladebildschirm beim Anwenden einer Richtlinie während des Starts einer Azure-VM nicht mehr reagiert.

## <a name="symptoms"></a>Symptome

Wenn Sie die [Startdiagnose](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/boot-diagnostics) verwenden, um einen Screenshot der VM anzuzeigen, reagiert der Bildschirm beim Laden nicht mehr und zeigt die folgende Nachricht an: ‚*Gruppenrichtlinie für lokale Benutzer und Gruppen wird angewendet*‘.

:::image type="content" source="media//unresponsive-vm-apply-group-policy/applying-group-policy-1.png" alt-text="Screenshot des Ladevorgangs zum Anwenden der Gruppenrichtlinie für lokale Benutzer und Gruppen (Windows Server 2012 R2)":::

:::image type="content" source="media/unresponsive-vm-apply-group-policy/applying-group-policy-2.png" alt-text="Screenshot des Ladevorgangs zum Anwenden der Gruppenrichtlinie für lokale Benutzer und Gruppen (Windows Server 2012)":::

## <a name="cause"></a>Ursache

Es gibt widersprüchliche Sperren, wenn die Richtlinie versucht, alte Benutzerprofile zu bereinigen.

> [!NOTE]
> Dies betrifft nur Windows Server 2012 und Windows Server 2012 R2.

Dies ist die problematische Richtlinie:

`Computer Configuration\Policies\Administrative Templates\System/User Profiles\Delete user profiles older than a specified number of days on system restart`

## <a name="resolution"></a>Lösung

### <a name="process-overview"></a>Übersicht über den Prozess

1. [Erstellen und Aufrufen einer Reparatur-VM](#step-1-create-and-access-a-repair-vm)
2. [Deaktivieren der Richtlinie](#step-2-disable-the-policy)
3. [Aktivieren der seriellen Konsole und der Speicherabbilderfassung](#step-3-enable-serial-console-and-memory-dump-collection)
4. [Neuerstellen der VM](#step-4-rebuild-the-vm)

> [!NOTE]
> Wenn dieser Startfehler auftritt, ist das Gastbetriebssystem nicht funktionsfähig. Sie müssen die Problembehandlung im Offlinemodus durchführen.

### <a name="step-1-create-and-access-a-repair-vm"></a>Schritt 1: Erstellen und Aufrufen einer Reparatur-VM

1. Führen Sie die [Schritte 1 bis 3 der Reparaturbefehle für virtuelle Computer](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands#repair-process-example) aus, um eine Reparatur-VM vorzubereiten.
2. Stellen Sie über eine Remotedesktopverbindung eine Verbindung mit der Reparatur-VM her.

### <a name="step-2-disable-the-policy"></a>Schritt 2: Deaktivieren der Richtlinie

1. Öffnen Sie auf der Reparatur-VM den Registrierungs-Editor.
2. Suchen Sie den Schlüssel **HKEY_LOCAL_MACHINE**, und wählen Sie dann im Menü **Datei** > **Struktur laden...** aus.

    :::image type="content" source="media/unresponsive-vm-apply-group-policy/registry.png" alt-text="Screenshot mit hervorgehobenem Schlüssel HKEY_LOCAL_MACHINE und dem Menü, das „Struktur laden“ enthält.":::

    - Mithilfe von „Struktur laden“ können Sie die Registrierungsschlüssel von einem Offlinesystem laden, in diesem Fall vom beschädigten Datenträger, der an die Reparatur-VM angefügt ist.
    - Systemweite Einstellungen sind unter `HKEY_LOCAL_MACHINE` gespeichert und können als „HKLM“ abgekürzt werden.
3. Navigieren Sie auf dem angefügten Datenträger zur Datei `\windows\system32\config\SOFTWARE`, und öffnen Sie sie.

    1. Sie werden zur Eingabe eines Namens aufgefordert. Geben Sie BROKENSOFTWARE ein.<br/>
    2. Um zu überprüfen, ob BROKENSOFTWARE geladen wurde, erweitern Sie **HKEY_LOCAL_MACHINE**, und suchen Sie nach dem hinzugefügten Schlüssel BROKENSOFTWARE.
4. Navigieren Sie zu BROKENSOFTWARE, und überprüfen Sie, ob der Schlüssel CleanupProfile in der geladenen Struktur vorhanden ist.

    1. Wenn der Schlüssel vorhanden ist, ist die CleanupProfile-Richtlinie festgelegt, ihr Wert stellt die Aufbewahrungsrichtlinie in Tagen dar. Fahren Sie mit dem Löschen des Schlüssels fort.<br/>
    2. Wenn der Schlüssel nicht vorhanden ist, ist die CleanupProfile-Richtlinie nicht festgelegt. [Senden Sie ein Supportticket](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), und schließen Sie die memory.dmp-Datei ein, die sich im Windows-Verzeichnis des angefügten Betriebssystem-Datenträgers befindet.

5. Löschen Sie den CleanupProfiles-Schlüssel mit diesem Befehl:

    ```
    reg delete "HKLM\BROKENSOFTWARE\Policies\Microsoft\Windows\System" /v CleanupProfiles /f
    ```
6.  Entladen Sie die BROKENSOFTWARE-Struktur mit diesem Befehl:

    ```
    reg unload HKLM\BROKENSOFTWARE
    ```

### <a name="step-3-enable-serial-console-and-memory-dump-collection"></a>Schritt 3: Aktivieren der seriellen Konsole und der Speicherabbilderfassung

Führen Sie dieses Skript aus, um die Speicherabbilderfassung und die serielle Konsole zu aktivieren:

1. Öffnen Sie eine Eingabeaufforderungssitzung mit erhöhten Rechten („Als Administrator ausführen“).
2. Führen Sie diese Befehle aus.

    **Aktivieren der seriellen Konsole**: 
    
    ```
    bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /ems {<BOOT LOADER IDENTIFIER>} ON
    ```

    ```
    bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200 
    ```
3. Überprüfen Sie, ob der freie Speicherplatz auf dem Betriebssystemdatenträger mindestens der Größe des Arbeitsspeichers (RAM) auf der VM entspricht.

    Wenn auf dem Betriebssystem-Datenträger nicht genügend Speicherplatz vorhanden ist, ändern Sie den Speicherort des Speicherabbilds, und verweisen Sie es auf einen angefügten Datenträger mit genügend freiem Speicherplatz. Um den Speicherort zu ändern, ersetzen Sie „%SystemRoot%“ in den folgenden Befehlen durch den Laufwerkbuchstaben (z. B. „F:“) des Datenträgers.

    **Empfohlene Konfiguration für das Aktivieren des Betriebssystemabbilds**:

    Laden des beschädigten Betriebssystemdatenträgers:

    ```
    REG LOAD HKLM\BROKENSYSTEM <VOLUME LETTER OF BROKEN OS DISK>:\windows\system32\config\SYSTEM
    ```

    Aktivieren für „ControlSet001“:
    
    ```
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f 
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f 
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f 
    ```

    Aktivieren für „ControlSet002“:
    
    ```
    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f 
    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f 
    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f 
    ```
    
    Entladen des beschädigten Betriebssystemdatenträgers:
    
    ```
    REG UNLOAD HKLM\BROKENSYSTEM
    ```

### <a name="step-4-rebuild-the-vm"></a>Schritt 4: Neuerstellen der VM

Führen Sie [Schritt 5 der VM-Reparaturbefehle](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands#repair-process-example) aus, um die VM zu reassemblieren.

Wenn das Problem behoben ist, wurde die Richtlinie lokal deaktiviert. Für eine permanente Lösung sollten Sie keine CleanupProfiles-Richtlinie auf VMs verwenden. Verwenden Sie eine andere Methode, um Profilbereinigungen durchzuführen.

Verwenden Sie diese Richtlinie nicht:

`Machine\Admin Templates\System\User Profiles\Delete user profiles older than a specified number of days on system restart`

## <a name="next-steps"></a>Nächste Schritte

Wenn beim Anwenden von Windows Update Probleme auftreten, lesen Sie [VM reagiert nicht mehr bei Anwenden von Windows Update, und Fehler C01A001D wird ausgegeben](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/unresponsive-vm-apply-windows-update).