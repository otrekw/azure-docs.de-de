---
title: 'Behandeln von Startfehlern: Fehler beim Lesen des Datenträgers'
description: Dieser Artikel enthält die Schritte zum Beheben von Problemen, bei denen der Datenträger einer Azure-VM nicht gelesen werden kann.
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: 817c9c5c-6a81-4b42-a6ad-0a0a11858b84
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 06/01/2020
ms.author: v-miegge
ms.openlocfilehash: f59903ed111be1fe414f4b3ded250d754c91d323
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87069156"
---
# <a name="troubleshoot-boot-error---disk-read-error-occurred"></a>Behandeln von Startfehlern: Fehler beim Lesen des Datenträgers

Dieser Artikel enthält die Schritte zum Beheben von Problemen, bei denen der Datenträger eines virtuellen Azure-Computers (VM) nicht gelesen werden kann.

## <a name="symptoms"></a>Symptome

Wenn Sie die [Startdiagnose](./boot-diagnostics.md) verwenden, um den Screenshot des virtuellen Computers anzuzeigen, wird im Screenshot eine Eingabeaufforderung mit folgender Meldung angezeigt: „Fehler beim Lesen des Datenträgers. Drücken Sie STRG+ALT+ENTF für einen Neustart.“

   ![Fehlermeldung: Fehler beim Lesen des Datenträgers. Drücken Sie STRG+ALT+ENTF für einen Neustart.](./media/disk-read-error-occurred/1.png)

## <a name="cause"></a>Ursache

Diese Fehlermeldung gibt an, dass die Datenträgerstruktur beschädigt und nicht mehr lesbar ist. Wenn Sie einen virtuellen Computer der Generation 1 verwenden, ist es auch möglich, dass die Datenträgerpartition, die die Startkonfigurationsdaten enthält, nicht auf **Active** festgelegt ist.

## <a name="solution"></a>Lösung

### <a name="process-overview"></a>Übersicht über den Prozess

1. Erstellen Sie eine Reparatur-VM, und greifen Sie darauf zu.
1. Wählen Sie eine Lösung:
   - [Festlegen des Partitionsstatus auf „Active“](#set-partition-status-to-active)
   - [Korrigieren der Datenträgerpartition](#fix-the-disk-partition)
1. Aktivieren der seriellen Konsole und der Speicherabbilderfassung.
1. Erstellen Sie den virtuellen Computer neu.

> [!NOTE]
> Wenn dieser Startfehler auftritt, ist das Gastbetriebssystem nicht funktionsfähig. Sie müssen die Problembehandlung im Offlinemodus durchführen, um dieses Problem zu beheben.

### <a name="create-and-access-a-repair-vm"></a>Erstellen und Aufrufen einer Reparatur-VM

1. Führen Sie die Schritte 1-3 der [Reparaturbefehle für virtuelle Computer](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md) aus, um eine Reparatur-VM vorzubereiten.
1. Stellen Sie über eine Remotedesktopverbindung eine Verbindung mit der Reparatur-VM her.

### <a name="set-partition-status-to-active"></a>Festlegen des Partitionsstatus auf „Active“

Bei virtuellen Computern der Generation 1 sollten Sie zunächst überprüfen, ob die Betriebssystempartition, die den BCD-Speicher enthält, als **Active** gekennzeichnet ist. Wenn Sie über einen virtuellen Computer der Generation 2 verfügen, fahren Sie mit [Korrigieren der Datenträgerpartition](#fix-the-disk-partition) fort, da das Status-Flag in der späteren Generation nicht mehr verwendet wird.

1. Öffnen Sie eine Eingabeaufforderung mit erhöhten Rechten (cmd.exe).
1. Geben Sie **diskpart** ein, um das Tool **DiskPart** zu starten.
1. Geben Sie **list disk** ein, um die Datenträger im System aufzulisten und die angefügte virtuelle Festplatte (VHD) für das Betriebssystem zu ermitteln.
1. Geben Sie **sel disk <Nr.>** ein, nachdem die angefügte Betriebssystem-VHD ermittelt wurde, um den Datenträger auszuwählen. In der folgenden Abbildung finden Sie ein Beispiel, bei dem Disk 1 die angefügte Betriebssystem-VHD ist.

   ![Das DiskPart-Fenster mit der Ausgabe des Befehls **list disk**, wobei Disk 0 und Disk 1 in der Tabelle angezeigt werden. Im Fenster wird außerdem wird die Ausgabe des Befehls **sel disk 1** angezeigt, wobei Disk 1 der ausgewählte Datenträger ist.](./media/disk-read-error-occurred/2.png)

1. Geben Sie **list partition** ein, nachdem der Datenträger ausgewählt wurde, um die Partitionen des ausgewählten Datenträgers aufzulisten.
1. Geben Sie **sel partition <Nr.>** ein, nachdem die Startpartition ermittelt wurde, um die Partition auszuwählen. Die Startpartition ist oft ungefähr 350 MB groß.  Sehen Sie sich beispielsweise die folgende Abbildung an, in der Partition 1 die Startpartition ist.

   ![Das DiskPart-Fenster mit der Ausgabe des Befehls **list partition**, wobei Partition 1 und Partition 2 in der Tabelle angezeigt werden. Im Fenster wird außerdem wird die Ausgabe des Befehls **sel partition 1** angezeigt, wobei Partition 1 der ausgewählte Datenträger ist.](./media/disk-read-error-occurred/3.png)

1. Geben Sie **detail partition** ein, um den Status der Partition zu überprüfen. In den folgenden Screenshots finden Sie Beispiele für die Festlegung der Partition auf **Active: No** bzw. **Active: Yes**.

   **Active: Nein**

   ![Das DiskPart-Fenster mit der Ausgabe des Befehls **detail partition**, wobei Partition 1 festgelegt ist auf **Active: No**.](./media/disk-read-error-occurred/4.png)

   **Active: Ja**

   ![Das DiskPart-Fenster mit der Ausgabe des Befehls **detail partition**, wobei Partition 1 festgelegt ist auf **Active: Yes**.](./media/disk-read-error-occurred/5.png)

1. Wenn die Partition nicht auf **Active** festgelegt ist, geben Sie **active** ein, um das Active-Flag zu ändern.
1. Geben Sie **detail partition** ein, um zu überprüfen, ob die Statusänderung ordnungsgemäß abgeschlossen wurde, und vergewissern Sie sich, dass die Ausgabe einschließt: **Active: Yes**. 
1. Geben Sie **exit** ein, um das DISKPART-Tool zu schließen und die Konfigurationsänderungen zu speichern.

### <a name="fix-the-disk-partition"></a>Korrigieren der Datenträgerpartition

1. Öffnen Sie eine Eingabeaufforderung mit erhöhten Rechten (cmd.exe).
1. Verwenden Sie den folgenden Befehl, um **CHKDSK** für die Datenträger auszuführen und Fehler zu beheben:

   `chkdsk <DRIVE LETTER>: /f`

   Durch das Hinzufügen der Befehlsoption **/f** werden alle Fehler auf dem Datenträger behoben. Sie müssen dabei **< DRIVE LETTER >** durch den Buchstaben der angefügten Betriebssystem-VHD ersetzen.

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
