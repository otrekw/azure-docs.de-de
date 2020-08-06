---
title: Azure-VM reagiert nicht mehr bei Anwenden von Windows Update, und Fehler C01A001D wird ausgegeben
description: In diesem Artikel werden Schritte zur Behebung von Problemen erläutert, bei denen Windows Update einen Fehler generiert und in einer Azure-VM nicht mehr reagiert.
services: virtual-machines-windows
documentationcenter: ''
author: TobyTu
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: a97393c3-351d-4324-867d-9329e31b3528
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.date: 03/31/2020
ms.author: v-mibufo
ms.openlocfilehash: 76c3f729a8520c7bff7b49a1d2200d7950f8a9f4
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87074309"
---
# <a name="vm-is-unresponsive-with-c01a001d-error-when-applying-windows-update"></a>VM reagiert nicht mehr bei Anwenden von Windows Update, und Fehler C01A001D wird ausgegeben

In diesem Artikel werden Schritte zur Behebung von Problemen erläutert, bei denen Windows Update (KB) einen Fehler generiert und in einer Azure-VM nicht mehr reagiert.

## <a name="symptoms"></a>Symptome

Wenn Sie den Screenshot der VM mithilfe der [Startdiagnose](./boot-diagnostics.md) anzeigen, wird das laufende Windows Update (KB) angezeigt; dieses schlägt jedoch mit dem folgenden Fehlercode fehl: „C01A001D“.

![Nicht reagierendes Windows Update](./media/unresponsive-vm-apply-windows-update/unresponsive-windows-update.png)

## <a name="cause"></a>Ursache

Eine Kerndatei kann im Dateisystem nicht erstellt werden. Das Betriebssystem kann keine Dateien auf den Datenträger schreiben.

## <a name="resolution"></a>Lösung

### <a name="process-overview"></a>Übersicht über den Prozess

1. [Erstellen und Aufrufen einer Reparatur-VM](#create-and-access-a-repair-vm).
2. [Freigeben von Speicherplatz auf der Festplatte](#free-up-space-on-the-hard-disk).
3. [Empfehlung: Aktivieren Sie vor dem Neuerstellen der VM die serielle Konsole und die Speicherabbildsammlung](#recommended-before-rebuilding-the-vm-enable-serial-console-and-memory-dump-collection).
4. [Erstellen Sie den virtuellen Computer neu](#rebuild-the-vm).

> [!NOTE]
> Wenn dieser Fehler auftritt, ist das Gastbetriebssystem nicht funktionstüchtig. Sie müssen die Problembehandlung im Offlinemodus durchführen, um dieses Problem zu beheben.

### <a name="create-and-access-a-repair-vm"></a>Erstellen und Aufrufen einer Reparatur-VM

1. Führen Sie die [Schritte 1 bis 3 der VM-Reparaturbefehle](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md) aus, um eine Reparatur-VM vorzubereiten.
2. Stellen Sie über eine Remotedesktopverbindung eine Verbindung mit der Reparatur-VM her.

### <a name="free-up-space-on-the-hard-disk"></a>Freigeben von Speicherplatz auf der Festplatte

Wenn der Datenträger nicht bereits 1 TB groß ist, müssen Sie seine Größe ändern. Sobald die Größe des Datenträgers 1 TB beträgt, führen Sie eine Bereinigung und Defragmentierung des Datenträgers durch.

1. Überprüfen Sie, ob der Datenträger voll ist. Wenn die Größe des Datenträgers unter 1 TB liegt, [erweitern Sie ihn mit PowerShell auf eine maximale Größe von 1 TB](../windows/expand-os-disk.md?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Ftoc.json).
2. Sobald die Größe des Datenträgers 1 TB beträgt, führen Sie eine Datenträgerbereinigung durch.
    - [Trennen Sie den Datenträger von der defekten VM](../windows/detach-disk.md).
    - [Fügen Sie den Datenträger an eine funktionsfähige VM an](../windows/attach-disk-ps.md#attach-an-existing-data-disk-to-a-vm).
    - Geben Sie mit dem [Datenträgerbereinigungstool](https://support.microsoft.com/help/4026616/windows-10-disk-cleanup) Speicherplatz frei.
3. Nach dem Ändern der Größe und dem Bereinigen müssen Sie das Laufwerk defragmentieren:

    ```
    defrag <LETTER ASSIGN TO THE OS DISK>: /u /x /g
    ```
    Je nach Grad der Fragmentierung kann dies einige Stunden dauern.

### <a name="recommended-before-rebuilding-the-vm-enable-serial-console-and-memory-dump-collection"></a>Empfohlen: Aktivieren Sie vor dem Neuerstellen der VM die serielle Konsole und die Speicherabbildsammlung.

1. Öffnen Sie eine Eingabeaufforderungssitzung mit erhöhten Rechten („Als Administrator ausführen“).
2. Führen Sie die folgenden Befehle aus:

    Aktivieren der seriellen Konsole:

    ```
    bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /ems {<BOOT LOADER IDENTIFIER>} ON
    bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200
    ```
3. Stellen Sie sicher, dass der freie Speicherplatz auf dem Betriebssystemdatenträger mindestens der Größe des VM-Speichers (RAM) entspricht.

    Wenn kein ausreichender Speicherplatz auf dem Betriebssystemdatenträger vorhanden ist, ändern Sie den Speicherort für das Erstellen der Speicherabbilddatei, und verweisen Sie auf einen an die VM angefügten Datenträger, der über ausreichenden Speicherplatz verfügt. Um den Speicherort zu ändern, ersetzen Sie `%SystemRoot%` in den folgenden Befehlen durch den Laufwerkbuchstaben (z. B. „F:“) des Datenträgers:

    **Empfohlene Konfiguration für das Aktivieren des Betriebssystemabbilds:**

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

### <a name="rebuild-the-vm"></a>Neuerstellen der VM

Führen Sie [Schritt 5 der VM-Reparaturbefehle](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md#repair-process-example) aus, um die VM zu reassemblieren.
