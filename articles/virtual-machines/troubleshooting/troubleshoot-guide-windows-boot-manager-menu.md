---
title: Der virtuelle Windows-Computer kann aufgrund des Windows-Start-Managers nicht gestartet werden.
description: Dieser Artikel enthält Schritte zum Beheben von Problemen, bei denen der Windows-Start-Manager das Starten eines virtuellen Azure-Computers verhindert.
services: virtual-machines-windows
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: a97393c3-351d-4324-867d-9329e31b3598
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.date: 03/26/2020
ms.author: v-mibufo
ms.openlocfilehash: 5d2fb62870e2c41af635627f5d692f08c67f8394
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "80373285"
---
# <a name="windows-vm-cannot-boot-due-to-windows-boot-manager"></a>Der virtuelle Windows-Computer kann aufgrund des Windows-Start-Managers nicht gestartet werden.

Dieser Artikel enthält Schritte zum Beheben von Problemen, bei denen der Windows-Start-Manager das Starten eines virtuellen Azure-Computers (VM) verhindert.

## <a name="symptom"></a>Symptom

Der virtuelle Computer wartet auf eine Benutzereingabeaufforderung und wird erst gestartet, wenn er manuell dazu angewiesen wird.

Wenn Sie [Startdiagnose](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/boot-diagnostics) verwenden, um den Screenshot der VM anzuzeigen, sehen Sie, dass im Screenshot der Windows-Start-Manager mit der Meldung *Wählen Sie ein Betriebssystem aus, um zu starten, oder drücken Sie die TAB-TASTE, um ein Tool auszuwählen* angezeigt wird.

Abbildung 1
 
![Windows-Start-Manager mit der Meldung „Wählen Sie ein Betriebssystem aus, um zu starten, oder drücken Sie die TAB-TASTE, um ein Tool auszuwählen“.](media/troubleshoot-guide-windows-boot-manager-menu/1.jpg)

## <a name="cause"></a>Ursache

Der Fehler ist auf das BCD-Flag *displaybootmenu* im Windows-Start-Manager zurückzuführen. Wenn das Flag aktiviert ist, wird der Benutzer während des Startvorgangs vom Windows-Start-Manager aufgefordert, das auszuführende Ladeprogramm auszuwählen, was zu einer Startverzögerung führt. In Azure kann dieses Feature die Startzeit eines virtuellen Computers verlängern.

## <a name="solution"></a>Lösung

Prozessübersicht:

1. Konfigurieren Sie mithilfe der seriellen Konsole eine kürzere Startzeit.
2. Erstellen Sie eine Reparatur-VM, und greifen Sie darauf zu.
3. Konfigurieren Sie eine kürzere Startzeit auf einer Reparatur-VM.
4. **Empfohlen:** Aktivieren Sie vor der Neuerstellung des virtuellen Computers die serielle Konsole und die Speicherabbildsammlung.
5. Erstellen Sie den virtuellen Computer neu.

### <a name="configure-for-faster-boot-time-using-serial-console"></a>Konfigurieren Sie mithilfe der seriellen Konsole eine kürzere Startzeit.

Wenn Sie Zugriff auf die serielle Konsole haben, gibt es zwei Möglichkeiten, kürzere Startzeiten zu erzielen. Verringern Sie entweder die *displaybootmenu*-Wartezeit, oder entfernen Sie das Flag vollständig.

1. Befolgen Sie die Anweisungen für den Zugriff auf die [serielle Azure-Konsole für Windows](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/serial-console-windows), um Zugriff auf die textbasierte Konsole zu erhalten.

   > [!NOTE]
   > Wenn Sie nicht auf die serielle Konsole zugreifen können, fahren Sie mit [Erstellen einer Reparatur-VM und Zugriff darauf](#create-and-access-a-repair-vm) fort.

2. **Option A**: Verkürzen der Wartezeit

   a. Die Wartezeit ist standardmäßig auf 30 Sekunden festgelegt, kann jedoch verkürzt werden (z. B. auf 5 Sekunden).

   b. Verwenden Sie den folgenden Befehl in der seriellen Konsole, um den Timeoutwert anzupassen:

      `bcdedit /set {bootmgr} timeout 5`

3. **Option B**: Entfernen des BCD-Flags

   a. Geben Sie den folgenden Befehl ein, um zu verhindern, dass die Aufforderung „Startmenü anzeigen“ angezeigt wird:

      `bcdedit /deletevalue {bootmgr} displaybootmenu`

      > [!NOTE]
      > Wenn Sie die serielle Konsole nicht verwenden konnten, um in den obigen Schritten eine kürzere Startzeit zu konfigurieren, können Sie stattdessen mit den folgenden Schritten fortfahren. Sie führen die Problembehandlung nun im Offlinemodus durch, um dieses Problem zu beheben.

### <a name="create-and-access-a-repair-vm"></a>Erstellen einer Reparatur-VM und Zugriff darauf

1. Führen Sie die [Schritte 1-3 der VM-Reparaturbefehle](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands) aus, um eine Reparatur-VM vorzubereiten.
2. Stellen Sie über eine Remotedesktopverbindung eine Verbindung mit der Reparatur-VM her.

### <a name="configure-for-faster-boot-time-on-a-repair-vm"></a>Konfigurieren einer kürzeren Startzeit auf einer Reparatur-VM

1. Öffnen Sie eine Eingabeaufforderung mit erhöhten Rechten.
2. Geben Sie Folgendes ein, um DisplayBootMenu zu aktivieren:

   Verwenden Sie diesen Befehl für **VMs der Generation 1**:

   `bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /set {bootmgr} displaybootmenu yes`

   Verwenden Sie diesen Befehl für **VMs der Generation 2**:

   `bcdedit /store <VOLUME LETTER OF EFI SYSTEM PARTITION>:EFI\Microsoft\boot\bcd /set {bootmgr} displaybootmenu yes`

   Ersetzen Sie alle „größer als“- oder „kleiner als“-Symbole und den darin enthaltenen Text, z. B. „<Text hier>“.

3. Legen Sie den Timeoutwert auf 5 Sekunden fest:

   Verwenden Sie diesen Befehl für **VMs der Generation 1**:

   `bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /set {bootmgr} timeout 5`

   Verwenden Sie diesen Befehl für **VMs der Generation 2**:

   `bcdedit /store <VOLUME LETTER OF EFI SYSTEM PARTITION>:EFI\Microsoft\boot\bcd /set {bootmgr} timeout 5`

   Ersetzen Sie alle „größer als“- oder „kleiner als“-Symbole und den darin enthaltenen Text, z. B. „<Text hier>“.

### <a name="recommended-before-you-rebuild-the-vm-enable-serial-console-and-memory-dump-collection"></a>Empfohlen: Aktivieren Sie vor der Neuerstellung des virtuellen Computers die serielle Konsole und die Speicherabbildsammlung.

Um die Speicherabbildsammlung und die serielle Konsole zu aktivieren, führen Sie das folgende Skript aus:

1. Öffnen Sie eine Eingabeaufforderungssitzung mit erhöhten Rechten (Als Administrator ausführen).
2. Führen Sie die folgenden Befehle aus:

   Aktivieren der seriellen Konsole

   `bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /ems {<BOOT LOADER IDENTIFIER>} ON`

   `bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200`

   Ersetzen Sie alle „größer als“- oder „kleiner als“-Symbole und den darin enthaltenen Text, z. B. „<Text hier>“.

3. Vergewissern Sie sich, dass der freie Speicherplatz auf dem Betriebssystemdatenträger der Größe des Arbeitsspeichers (RAM) auf der VM entspricht.

   Wenn nicht genügend Speicherplatz auf dem Betriebssystemdatenträger vorhanden ist, sollten Sie den Speicherort, wo die Speicherabbilddatei erstellt wird, ändern und sie auf einem der VM angefügten Datenträger ablegen, der genügend freien Speicherplatz bietet. Um den Speicherort zu ändern, ersetzen Sie „%SystemRoot%“ in den folgenden Befehlen durch den Laufwerksbuchstaben (z. B. „F:“) des Datenträgers.

#### <a name="suggested-configuration-to-enable-os-dump"></a>Empfohlene Konfiguration zum Aktivieren des Betriebssystemabbilds

**Beschädigten Betriebssystemdatenträger laden**:

`REG LOAD HKLM\BROKENSYSTEM <VOLUME LETTER OF BROKEN OS DISK>:\windows\system32\config\SYSTEM`

**Aktivieren für ControlSet001**:

`REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f`

`REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f`

`REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f`

**Aktivieren für ControlSet002**:

`REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f`

`REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f`

`REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f`

**Beschädigten Betriebssystemdatenträger entladen**:

`REG UNLOAD HKLM\BROKENSYSTEM`

### <a name="rebuild-the-original-vm"></a>Neuerstellung der ursprünglichen VM

Führen Sie [Schritt 5 der VM-Reparaturbefehle](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands#repair-process-example) aus, um die VM zu reassemblieren.