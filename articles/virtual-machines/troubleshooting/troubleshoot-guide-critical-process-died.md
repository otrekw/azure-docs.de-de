---
title: Windows-kritischer Fehler –
description: ''
services: virtual-machines-windows
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: 5b3ed56a-5a11-4ff9-9ee8-76aea4a5689b
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.date: 03/26/2020
ms.author: v-mibufo
ms.openlocfilehash: 9e4c4b9c809a626c71b4a7e9235d917b442be160
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "80373289"
---
# <a name="windows-stop-error---0x000000ef-critical-process-died"></a>Windows-kritischer Fehler – #0x000000EF „Critical Process Died“

In diesem Artikel finden Sie Schritte zum Beheben von Problemen, bei denen ein kritischer Prozess während des Starts auf einem virtuellen Azure-Computer abstürzt.

## <a name="symptom"></a>Symptom

Wenn Sie die [Startdiagnose](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/boot-diagnostics) verwenden, um den Screenshot des virtuellen Computers anzuzeigen, wird im Screenshot der Fehler *#0x000000EF* mit der Meldung *Critical Process Died* angezeigt.

![„Your PC ran into a problem and needs to restart. Es werden einige Fehlerinformationen gesammelt, und dann können Sie einen Neustart ausführen. (##% abgeschlossen) Wenn Sie weitere Informationen benötigen, können Sie später online nach dem folgenden Fehler suchen: 0x000000EF"](media/troubleshoot-guide-critical-process-died/1.jpg)

## <a name="cause"></a>Ursache

Dies ist in der Regel darauf zurückzuführen, dass während des Starts ein kritischer Systemprozess fehlschlägt. Weitere Informationen zu Problemen in Bezug auf kritische Prozesse finden Sie unter „[Fehlerprüfung 0xEF: CRITICAL_PROCESS_DIED](https://docs.microsoft.com/windows-hardware/drivers/debugger/bug-check-0xef--critical-process-died)“.

## <a name="solution"></a>Lösung

### <a name="process-overview"></a>Übersicht über den Prozess:

1. Erstellen Sie eine Reparatur-VM (Repair-VM), und greifen Sie darauf zu.
2. Beheben Sie Beschädigungen des Betriebssystems.
3. **Empfohlen:** Aktivieren Sie vor der Neuerstellung des virtuellen Computers die serielle Konsole und die Speicherabbildsammlung.
4. Erstellen Sie den virtuellen Computer neu.

> [!NOTE]
> Wenn dieser Startfehler auftritt, ist das Gastbetriebssystem nicht funktionsfähig. Sie müssen die Problembehandlung im Offlinemodus durchführen, um dieses Problem zu beheben.

### <a name="create-and-access-a-repair-vm"></a>Erstellen einer Reparatur-VM und Zugreifen darauf

1. Führen Sie die [Schritte 1-3 der VM-Reparaturbefehle](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands) aus, um eine Reparatur-VM vorzubereiten.
2. Stellen Sie über eine Remotedesktopverbindung eine Verbindung mit der Reparatur-VM her.

### <a name="fix-any-os-corruption"></a>Beheben aller Beschädigungen des Betriebssystems

1. Öffnen Sie eine Eingabeaufforderung mit erhöhten Rechten.
2. Führen Sie den folgenden SFC-Befehl (System File Checker) aus:

   `sfc /scannow /offbootdir=<BOOT DISK DRIVE>:\ /offwindir=<BROKEN DISK DRIVE>:\windows`

   * Dabei ist <BOOT DISK DRIVE> das Startvolume der Reparatur-VM (in der Regel „C:“), und <BROKEN DISK DRIVE> ist der Laufwerkbuchstabe für den angefügten Datenträger der fehlerhaften VM. Ersetzen Sie die Symbole „Größer als“/„Kleiner als“ sowie den darin enthaltenen Text (z. B. „<Text hier>“) durch den entsprechenden Buchstaben.

3. Führen Sie als Nächstes [Schritt 5 der VM-Reparaturbefehle](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands#repair-process-example) aus, um die VM zu reassemblieren und zu überprüfen, ob sie startet.
4. Wenn die VM dennoch nicht startet, erfassen Sie die Speicherabbilddatei.

### <a name="collect-the-memory-dump-file"></a>Erfassen der Speicherabbilddatei

Wenn das Problem nach dem Ausführen von SFC weiterhin besteht, muss die Speicherabbilddatei analysiert werden, um die Ursache des Problems zu ermitteln. Führen Sie die folgenden Schritte aus, um die Speicherabbilddatei zu erfassen:

### <a name="attach-the-os-disk-to-a-new-repair-vm"></a>Anfügen des Betriebssystemdatenträgers an eine neue Reparatur-VM

1. Führen Sie die [Schritte 1-3 der VM-Reparaturbefehle](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands) aus, um eine neue Reparatur-VM vorzubereiten.
2. Stellen Sie über eine Remotedesktopverbindung eine Verbindung mit der Reparatur-VM her.

### <a name="locate-the-dump-file-and-submit-a-support-ticket"></a>Suchen nach der Speicherabbilddatei und Senden eines Supporttickets

3. Navigieren Sie auf der Reparatur-VM zum Windows-Ordner auf dem angefügten Betriebssystemdatenträger. Wenn der dem angefügten Betriebssystemdatenträger zugewiesene Laufwerkbuchstabe *F* lautet, müssen Sie zu *F:\Windows* navigieren.
4. Suchen Sie nach der Datei *memory.dmp*, und [senden Sie dann ein Supportticket](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) mit der Speicherabbilddatei.

   > [!NOTE]
   > Wenn Sie die Speicherabbilddatei nicht finden können, führen Sie die nachstehenden Schritte aus, um die Speicherabbildsammlung und die serielle Konsole zu aktivieren. Kehren Sie dann zu diesem Abschnitt zurück, und wiederholen Sie die Schritte der obigen Aufgabe zum Erfassen der Speicherabbilddatei.

### <a name="recommended-before-you-rebuild-the-vm-enable-serial-console-and-memory-dump-collection"></a>Empfohlen: Aktivieren der seriellen Konsole und der Speicherabbildsammlung vor der Neuerstellung der VM

Führen Sie das folgende Skript aus, um die Speicherabbildsammlung und die serielle Konsole zu aktivieren:

1. Öffnen Sie eine Eingabeaufforderungssitzung mit erhöhten Rechten („Als Administrator ausführen“).
2. Führen Sie die folgenden Befehle aus:

   Aktivieren der seriellen Konsole

   `bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /ems {<BOOT LOADER IDENTIFIER>} ON`

   `bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200`

   Ersetzen Sie die Symbole „Größer als“ oder „Kleiner als“ und den darin enthaltenen Text (z. B. „<Text hier>“) durch die entsprechenden Angaben.

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

### <a name="rebuild-the-original-vm"></a>Neuerstellung der ursprünglichen VM

Führen Sie [Schritt 5 der VM-Reparaturbefehle](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands#repair-process-example) aus, um die VM zu reassemblieren.
