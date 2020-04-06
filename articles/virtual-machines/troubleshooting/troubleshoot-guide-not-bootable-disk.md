---
title: Startfehler – „This is not a Bootable Disk“
description: Dieser Artikel enthält die Schritte zum Beheben von Problemen, bei denen der Datenträger eines virtuellen Azure-Computers nicht gestartet werden kann.
services: virtual-machines-windows
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: 5d6db4e3-c2f5-40c7-afea-54edf745f5eb
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.date: 03/25/2020
ms.author: v-mibufo
ms.openlocfilehash: 9f0c6350b89dcfecefcadcc166f7af35abc4b128
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80299190"
---
# <a name="boot-error--this-is-not-a-bootable-disk"></a>Startfehler – This is not a Bootable Disk

Dieser Artikel enthält die Schritte zum Beheben von Problemen, bei denen der Datenträger eines virtuellen Azure-Computers nicht gestartet werden kann.

## <a name="symptoms"></a>Symptome

Wenn Sie die [Startdiagnose](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/boot-diagnostics) verwenden, um den Screenshot des virtuellen Computers anzuzeigen, wird im Screenshot eine Eingabeaufforderung mit der Meldung „This is not a bootable disk. Please insert a bootable floppy and press any key to try again...“ angezeigt.

   Abbildung 1

   ![Abbildung 1 zeigt die Meldung *„This is not a bootable disk. Please insert a bootable floppy and press any key to try again...“*.](media/troubleshoot-guide-not-bootable-disk/1.jpg)

## <a name="cause"></a>Ursache

Diese Fehlermeldung bedeutet, dass der Betriebssystemstartprozess keine aktive Systempartition finden konnte. Dieser Fehler kann auch darauf hinweisen, dass es einen fehlenden Verweis im Startkonfigurationsdatenspeicher (BCD) gibt, der das Ermitteln der Windows-Partition verhindert.

## <a name="solution"></a>Lösung

### <a name="process-overview"></a>Prozessübersicht

1. Erstellen Sie eine Reparatur-VM, und greifen Sie darauf zu.
2. Legen Sie Partitionsstatus auf „Active“ fest.
3. Korrigieren Sie die Datenträgerpartition.
4. **Empfohlen:** Aktivieren Sie vor der Neuerstellung des virtuellen Computers die serielle Konsole und die Speicherabbildsammlung.
5. Erstellen Sie die ursprüngliche VM neu.

   > [!NOTE]
   > Wenn dieser Startfehler auftritt, ist das Gastbetriebssystem nicht funktionsfähig. Sie müssen die Problembehandlung im Offlinemodus durchführen, um dieses Problem zu beheben.

### <a name="create-and-access-a-repair-vm"></a>Erstellen einer Reparatur-VM und Zugreifen darauf

1. Führen Sie die Schritte 1-3 der [Reparaturbefehle für virtuelle Computer](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands) aus, um eine Reparatur-VM vorzubereiten.
2. Stellen Sie über eine Remotedesktopverbindung eine Verbindung mit der Reparatur-VM her.

### <a name="set-partition-status-to-active"></a>Festlegen des Partitionsstatus auf „Active“

Virtuelle Computer der Generation 1 sollten zunächst überprüfen, ob die Betriebssystempartition, die den BCD-Speicher enthält, als *Active* gekennzeichnet ist. Wenn Sie über einen virtuellen Computer der Generation 2 verfügen, fahren Sie mit [Korrigieren der Datenträgerpartition](#fix-the-disk-partition) fort, da das *Status*-Flag in der späteren Generation nicht mehr verwendet wird.

1. Öffnen Sie eine Eingabeaufforderung mit erhöhten Rechten *(cmd.exe)* .
2. Geben Sie *diskpart* ein, um das DISKPART-Tool zu starten.
3. Geben Sie *list disk* ein, um die Datenträger im System aufzulisten und die angefügte Betriebssystem-VHD zu ermitteln.
4. Geben Sie *sel disk <Nr.>* ein, nachdem die angefügte Betriebssystem-VHD ermittelt wurde, um den Datenträger auszuwählen.  Siehe Abbildung 2, wobei Disk 1 die angefügte Betriebssystem-VHD ist.

   Abbildung 2

   ![Abbildung 2 zeigt das *DISKPART*-Fenster mit der Ausgabe des Befehls „list disk“, wobei Disk 0 und Disk 1 in der Tabelle angezeigt werden.  Außerdem wird die Ausgabe des Befehls „sel disk 1“ angezeigt, wobei Disk 1 der ausgewählte Datenträger ist.](media/troubleshoot-guide-not-bootable-disk/2.jpg)

5. Geben Sie *list partition* ein, nachdem der Datenträger ausgewählt wurde, um die Partitionen des ausgewählten Datenträgers aufzulisten.
6. Geben Sie *sel partition <Nr.>* ein, nachdem die Startpartition ermittelt wurde, um die Partition auszuwählen.  Die Startpartition ist normalerweise ca. 350 MB groß.  Siehe Abbildung 3, in der Partition 1 die Startpartition ist.

   Abbildung 3

   ![Abbildung 3 zeigt das *DISKPART*-Fenster mit der Ausgabe des Befehls *list partition*. In der Tabelle werden Partition 1 und Partition 2 angezeigt. Außerdem wird die Ausgabe des Befehls *sel partition 1* angezeigt, wenn Partition 1 der ausgewählte Datenträger ist.](media/troubleshoot-guide-not-bootable-disk/3.jpg)

7. Geben Sie „detail partition“ ein, um den Status der Partition zu überprüfen. Siehe Abbildung 4, in der für die Partition *Active: No* angezeigt wird, oder Abbildung 5, in der für die Partition „Active: Yes“ angezeigt wird.

   Abbildung 4

   ![Abbildung 4 zeigt das *DISKPART*-Fenster mit der Ausgabe des Befehls *detail partition*, wenn Partition 1 festgelegt ist auf *Active: Nein*](media/troubleshoot-guide-not-bootable-disk/4.jpg)

   Abbildung 5

   ![Abbildung 5 zeigt das *DISKPART*-Fenster mit der Ausgabe des Befehls *detail partition*, wenn Partition 1 festgelegt ist auf *Active:  Yes*.](media/troubleshoot-guide-not-bootable-disk/5.jpg)

8. Geben Sie *active* ein, wenn für die Partition **Active: No** angezeigt wird, um das *Active*-Flag zu ändern.
9. Überprüfen Sie, ob der Status ordnungsgemäß geändert wurde, indem Sie *detail partition* eingeben.

   Abbildung 6

   ![Abbildung 6 zeigt das DISKPART-Fenster mit der Ausgabe des Befehls *detail partition*, wenn Partition 1 festgelegt ist auf *Active: Ja*](media/troubleshoot-guide-not-bootable-disk/6.jpg)

10. Geben Sie *exit* ein, um das DISKPART-Tool zu schließen und die Konfigurationsänderungen zu speichern.

### <a name="fix-the-disk-partition"></a>Korrigieren der Datenträgerpartition

1. Öffnen Sie eine Eingabeaufforderung mit erhöhten Rechten (cmd.exe).
2. Verwenden Sie den folgenden Befehl, um *CHKDSK* für die Datenträger auszuführen und Fehler zu beheben:

   `chkdsk <DRIVE LETTER>: /f`

   Durch das Hinzufügen der Befehlsoption „/f“ werden alle Fehler auf dem Datenträger behoben. Sie müssen dabei <DRIVE LETTER> durch den Buchstaben der angefügten Betriebssystem-VHD ersetzen.

### <a name="recommended-before-you-rebuild-the-vm-enable-serial-console-and-memory-dump-collection"></a>Empfohlen: Aktivieren der seriellen Konsole und der Speicherabbildsammlung vor der Neuerstellung der VM

Führen Sie das folgende Skript aus, um die Speicherabbildsammlung und die serielle Konsole zu aktivieren:

1. Öffnen Sie eine Eingabeaufforderungssitzung mit erhöhten Rechten („Als Administrator ausführen“).
2. Führen Sie die folgenden Befehle aus:

   Aktivieren der seriellen Konsole

   `bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /ems {<BOOT LOADER IDENTIFIER>} ON`

   `bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200`

3. Überprüfen Sie, ob der freie Speicherplatz auf dem Betriebssystemdatenträger der Größe des Arbeitsspeichers (RAM) auf der VM entspricht.

   Wenn nicht genügend Speicherplatz auf dem Betriebssystemdatenträger vorhanden ist, sollten Sie den Speicherort für das Erstellen der Speicherabbilddatei ändern und auf einen an die VM angefügten Datenträger verweisen, der über genügend freien Speicherplatz verfügt. Um den Speicherort zu ändern, ersetzen Sie „%SystemRoot%“ in den folgenden Befehlen durch den Laufwerkbuchstaben (z. B. „F:“) des Datenträgers.

#### <a name="suggested-configuration-to-enable-os-dump"></a>Empfohlene Konfiguration für das Aktivieren des Betriebssystemabbilds

**Laden des beschädigten Betriebssystemdatenträgers**:

`REG LOAD HKLM\BROKENSYSTEM <VOLUME LETTER OF BROKEN OS DISK>:\windows\system32\config\SYSTEM`

**Aktivieren für „ControlSet001“** :

`REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f`

`REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f`

`REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f`

**Aktivieren für „ControlSet002“** :

`REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f`

`REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f`

`REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f`

**Entladen des beschädigten Betriebssystemdatenträgers**:

`REG UNLOAD HKLM\BROKENSYSTEM`

### <a name="rebuild-the-original-vm"></a>Neuerstellen der ursprünglichen VM

Führen Sie [Schritt 5 der VM-Reparaturbefehle](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands#repair-process-example) aus, um die VM zu reassemblieren.
