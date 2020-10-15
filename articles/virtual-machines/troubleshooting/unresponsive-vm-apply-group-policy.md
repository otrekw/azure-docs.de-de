---
title: Azure-VM reagiert nicht beim Anwenden einer Richtlinie
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
ms.openlocfilehash: cbf2fe491e1fe0b553eab04ca7190da0413a3ba6
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "86526009"
---
# <a name="vm-is-unresponsive-when-applying-group-policy-local-users-and-groups-policy"></a>VM reagiert nicht beim Anwenden der Richtlinie „Gruppenrichtlinie für lokale Benutzer und Gruppen“

In diesem Artikel werden die Schritte zum Beheben von Problemen beschrieben, bei denen der Ladebildschirm beim Anwenden einer Richtlinie während des Starts eines virtuellen Azure-Computers (VM) nicht mehr reagiert.

## <a name="symptoms"></a>Symptome

Wenn Sie die [Startdiagnose](./boot-diagnostics.md) verwenden, um einen Screenshot der VM anzuzeigen, reagiert der Bildschirm beim Laden nicht mehr und zeigt die folgende Meldung an: „Applying Group Policy Local Users and Groups policy“ (Gruppenrichtlinie für lokale Benutzer und Gruppen wird angewendet).

:::image type="content" source="media//unresponsive-vm-apply-group-policy/applying-group-policy-1.png" alt-text="Screenshot des Ladevorgangs zum Anwenden der Gruppenrichtlinie für lokale Benutzer und Gruppen (Windows Server 2012 R2)":::

:::image type="content" source="media/unresponsive-vm-apply-group-policy/applying-group-policy-2.png" alt-text="Screenshot des Ladevorgangs zum Anwenden der Gruppenrichtlinie für lokale Benutzer und Gruppen (Windows Server 2012 R2)":::

## <a name="cause"></a>Ursache

Es gibt widersprüchliche Sperren, wenn die Richtlinie versucht, alte Benutzerprofile zu bereinigen.

> [!NOTE]
> Dies betrifft nur Windows Server 2012 und Windows Server 2012 R2.

Dies ist die problematische Richtlinie:

`Computer Configuration\Policies\Administrative Templates\System/User Profiles\Delete user profiles older than a specified number of days on system restart`

## <a name="resolution"></a>Lösung

### <a name="process-overview"></a>Übersicht über den Prozess

1. [Erstellen und Aufrufen einer Reparatur-VM](#step-1-create-and-access-a-repair-vm)
1. [Deaktivieren der Richtlinie](#step-2-disable-the-policy)
1. [Aktivieren der seriellen Konsole und der Speicherabbilderfassung](#step-3-enable-serial-console-and-memory-dump-collection)
1. [Neuerstellen der VM](#step-4-rebuild-the-vm)

> [!NOTE]
> Wenn dieser Startfehler auftritt, ist das Gastbetriebssystem nicht funktionsfähig. Sie müssen die Problembehandlung im Offlinemodus durchführen.

### <a name="step-1-create-and-access-a-repair-vm"></a>Schritt 1: Erstellen und Aufrufen einer Reparatur-VM

1. Führen Sie die [Schritte 1 bis 3 der Reparaturbefehle für virtuelle Computer](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md#repair-process-example) aus, um eine Reparatur-VM vorzubereiten.
2. Stellen Sie über die Remotedesktopverbindung eine Verbindung mit der Reparatur-VM her.

### <a name="step-2-disable-the-policy"></a>Schritt 2: Deaktivieren der Richtlinie

1. Öffnen Sie auf der Reparatur-VM den Registrierungs-Editor.
1. Suchen Sie den Schlüssel **HKEY_LOCAL_MACHINE**, und klicken Sie dann im Menü **Datei** auf **Struktur laden**.

    :::image type="content" source="media/unresponsive-vm-apply-group-policy/registry.png" alt-text="Screenshot des Ladevorgangs zum Anwenden der Gruppenrichtlinie für lokale Benutzer und Gruppen (Windows Server 2012 R2)" /v CleanupProfiles /f
    ```
1.  Entladen Sie die BROKENSOFTWARE-Struktur mit diesem Befehl:

    ```
    reg unload HKLM\BROKENSOFTWARE
    ```

### <a name="step-3-enable-serial-console-and-memory-dump-collection"></a>Schritt 3: Aktivieren der seriellen Konsole und der Speicherabbilderfassung

Führen Sie dieses Skript aus, um die Speicherabbilderfassung und die serielle Konsole zu aktivieren:

1. Öffnen Sie eine Eingabeaufforderungssitzung mit erhöhten Rechten. (Als Administrator ausführen.)
1. Führen Sie diese Befehle aus, um die serielle Konsole zu aktivieren:
    
    ```
    bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /ems {<BOOT LOADER IDENTIFIER>} ON
    ```

    ```
    bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200
    ```
1. Überprüfen Sie, ob der freie Speicherplatz auf dem Betriebssystem-Datenträger mindestens der Größe des Arbeitsspeichers (RAM) der VM entspricht.

    Wenn auf dem Betriebssystem-Datenträger nicht genügend Speicherplatz vorhanden ist, ändern Sie den Speicherort des Speicherabbilds in einen Ort auf einen angefügten Datenträger mit genügend freiem Speicherplatz. Ersetzen Sie „%SystemRoot%“ in den folgenden Befehlen durch den Laufwerkbuchstaben (z. B. „F:“) des Datenträgers, um den Speicherort zu ändern.

    **Empfohlene Konfiguration für das Aktivieren des Betriebssystemabbilds**

    Laden Sie den beschädigten Betriebssystem-Datenträger:

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

Führen Sie [Schritt 5 der VM-Reparaturbefehle](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md#repair-process-example) aus, um die VM zu reassemblieren.

Wenn das Problem behoben wurde, ist die Richtlinie nun lokal deaktiviert. Eine dauerhafte Lösung ist das Nichtverwenden der CleanupProfiles-Richtlinie auf VMs. Verwenden Sie eine andere Methode, um Profilbereinigungen durchzuführen.

Verwenden Sie diese Richtlinie nicht:

`Machine\Admin Templates\System\User Profiles\Delete user profiles older than a specified number of days on system restart`

## <a name="next-steps"></a>Nächste Schritte

Wenn beim Anwenden eines Windows Updates Probleme auftreten, lesen Sie [VM reagiert nicht mehr bei Anwenden von Windows Update, und Fehler C01A001D wird ausgegeben](./unresponsive-vm-apply-windows-update.md).
