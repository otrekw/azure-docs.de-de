---
title: Das Herunterfahren von virtuellen Azure-Computern bleibt bei „Wird neu gestartet“, „Wird herunterfahren“ oder „Dienste werden beendet“ hängen | Microsoft-Dokumentation
description: Dieser Artikel hilft Ihnen beim Beheben von Dienstfehlern in virtuellen Azure Windows-Computern.
services: virtual-machines-windows
documentationCenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 12/19/2019
ms.author: tibasham
ms.openlocfilehash: 5d6396efc9ab25baa0d32e7c33c7715863516249
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "77371353"
---
# <a name="azure-windows-vm-shutdown-is-stuck-on-restarting-shutting-down-or-stopping-services"></a>Das Herunterfahren einer Azure-Windows-VM bleibt bei „Wird neu gestartet“, „Wird herunterfahren“ oder „Dienste werden beendet“ hängen.

In diesem Artikel werden die Schritte zum Beheben von Problemen bei den Meldungen „Wird neu gestartet“, „Wird herunterfahren“ oder „Dienste werden beendet“, beschrieben die beim Neustarten eines virtuellen Windows-Computers in Microsoft Azure auftreten können.

## <a name="symptoms"></a>Symptome

Wenn Sie die [Startdiagnose](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/boot-diagnostics) verwenden, um den Screenshot der VM anzuzeigen, wird auf dem Screenshot möglicherweise die Meldung „Wird neu gestartet“, „Wird herunterfahren“ oder „Dienste werden beendet“ anzeigt.

![Bildschirme „Wird neu gestartet“, „Wird herunterfahren“ oder „Dienste werden beendet“](./media/boot-error-troubleshooting-windows/restart-shut-down-stop-service.png)
 
## <a name="cause"></a>Ursache

Windows verwendet den Herunterfahren-Prozess, um Systemwartungsvorgänge auszuführen und Änderungen wie Updates, Rollen und Features zu verarbeiten. Es wird nicht empfohlen, diesen kritischen Prozess zu unterbrechen, bevor er abgeschlossen ist. Abhängig von der Anzahl der Updates/Änderungen und der VM-Größe kann der Prozess sehr lange dauern. Wenn der Prozess beendet wird, kann das Betriebssystem beschädigt werden. Unterbrechen Sie den Prozess nur, wenn er übermäßig lange dauert.

## <a name="solution"></a>Lösung

### <a name="collect-a-process-memory-dump"></a>Erfassen eines Prozessspeicherabbilds

1. Laden Sie das Tool [Procdump](http://download.sysinternals.com/files/Procdump.zip) auf einen neuen oder vorhandenen Datenträger herunter, der an eine funktionierenden virtuellen Computer in der gleichen Region angefügt ist.

2. Trennen Sie den Datenträger, der die Dateien enthält, die vom funktionierenden virtuellen Computer benötigt werden, und fügen Sie den Datenträger an die getrennte VM an. Dieser Datenträger wird **Hilfsprogrammdatenträger**  genannt.

Verwenden Sie die [serielle Konsole](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/serial-console-windows), um die folgenden Schritte auszuführen:

1. Öffnen Sie eine administrative PowerShell-Eingabeaufforderung und ermitteln Sie den Dienst, der beim Beenden hängengeblieben ist.

   ``
   Get-Service | Where-Object {$_.Status -eq "STOP_PENDING"}
   ``

2. Ermitteln Sie an einer administrativen Eingabeaufforderung die PID des hängengebliebenen Diensts.

   ``
   tasklist /svc | findstr /i <STOPING SERVICE>
   ``

3. Fordern Sie ein Speicherabbild des hängengebliebenen Prozesses an<STOPPING SERVICE>.

   ``
   procdump.exe -s 5 -n 3 -ma <PID>
   ``

4. Beenden Sie nun den hängengebliebenen Prozess, um den Herunterfahren-Prozess zu entsperren.

   ``
   taskkill /PID <PID> /t /f
   ``

Vergewissern Sie sich beim Start des Betriebssystems, dass die Betriebssystemkonsistenz in Ordnung ist, wenn das Betriebssystem normal gestartet wird. Wenn eine Beschädigung gemeldet wird, führen Sie den folgenden Befehl aus, bis der Datenträger fehlerfrei ist:

``
dism /online /cleanup-image /restorehealth
``

Wenn Sie kein Prozessspeicherabbild erfassen können oder wenn es sich um ein rekursives Problem handelt und Sie eine Fehlerursachenanalyse durchführen müssen, erfassen Sie als nächstes ein Betriebssystemspeicherabbild und öffnen Sie anschließend eine Supportanfrage.

### <a name="collect-an-os-memory-dump"></a>Erfassen eines Betriebssystemspeicherabbilds

Wenn Sie auf die Verarbeitung der Änderungen gewartet haben und das Problem trotzdem bestehen bleibt, müssen Sie sich mit einer Speicherabbilddatei an den Support wenden. Um die Sicherungsdatei zu sammeln, gehen Sie folgendermaßen vor:

**Anfügen des Betriebssystemdatenträgers an eine VM für die Wiederherstellung**

1. Erstellen Sie eine Momentaufnahme des Betriebssystemdatenträgers des betroffenen virtuellen Computers als Sicherung. Weitere Informationen finden Sie unter [Erstellen einer Momentaufnahme eines Datenträgers](https://docs.microsoft.com/azure/virtual-machines/windows/snapshot-copy-managed-disk).

2. [Fügen Sie den Betriebssystemdatenträger an einen virtuellen Computer für die Wiederherstellung an](https://docs.microsoft.com/azure/virtual-machines/windows/troubleshoot-recovery-disks-portal).

3. Remotedesktopverbindung mit der Wiederherstellungs-VM.

4. Ist der Betriebssystemdatenträger verschlüsselt, müssen Sie die Verschlüsselung deaktivieren, bevor Sie mit dem nächsten Schritt fortfahren. Weitere Informationen zum Entschlüsseln des verschlüsselten Betriebssystemdatenträgers des virtuellen Computers, der nicht gestartet werden kann, finden Sie [hier](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/troubleshoot-bitlocker-boot-error#solution).

**Suche nach der Sicherungsdatei und Senden eines Supporttickets**

1. Navigieren Sie auf der Wiederherstellungs-VM zum Windows-Ordner im angefügten Betriebssystemdatenträger. Wenn der Laufwerkbuchstabe, der dem angefügten Betriebssystemdatenträger zugewiesen ist, „F“ lautet, müssen Sie zu „F:\Windows“ navigieren.

2. Suchen Sie nach der Datei „memory.dmp“, und [senden Sie dann ein Supportticket](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) mit der Sicherungsdatei.

Wenn Sie die Sicherungsdatei nicht finden können, fahren Sie mit dem nächsten Schritt zum Aktivieren des Sicherungsprotokolls und der seriellen Konsole fort.

**Aktivieren des Sicherungsprotokolls und der seriellen Konsole**

Um das Sicherungsprotokoll und die serielle Konsole zu aktivieren, führen Sie das folgende Skript aus.

1. Öffnen Sie die Eingabeaufforderungssitzung mit erhöhten Rechten (Als Administrator ausführen).

2. Führen Sie folgendes Skript aus:

   In diesem Skript wird davon ausgegangen, dass der dem angefügten Betriebssystemdatenträger zugewiesene Laufwerkbuchstabe „F“ ist. Ersetzen Sie ihn durch den entsprechenden Wert Ihrer VM.

   ```
   reg load HKLM\BROKENSYSTEM F:\windows\system32\config\SYSTEM.hiv
   
   REM Enable Serial Console
   bcdedit /store F:\boot\bcd /set {bootmgr} displaybootmenu yes
   bcdedit /store F:\boot\bcd /set {bootmgr} timeout 5
   bcdedit /store F:\boot\bcd /set {bootmgr} bootems yes
   bcdedit /store F:\boot\bcd /ems {<BOOT LOADER IDENTIFIER>} ON
   bcdedit /store F:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200
   
   REM Suggested configuration to enable OS Dump
   REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f
   REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f
   REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f

   REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f
   REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f
   REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v NMICrashDump / t REG_DWORD /d 1 /f
   
   reg unload HKLM\BROKENSYSTEM
   ```

3. Vergewissern Sie sich, dass genügend Speicherplatz auf dem Datenträger vorhanden ist, um genauso viel Arbeitsspeicher wie der RAM zuzuweisen. Dieser hängt von der Größe ab, die Sie für diese VM auswählen.

4. Wenn nicht genügend Speicherplatz vorhanden oder die VM sehr groß ist (Serie G, GS oder E), können Sie den Speicherort, in dem diese Datei erstellt wird, ändern und auf einen anderen Datenträger verweisen, der der VM angefügt ist. Um den Speicherort zu ändern, müssen Sie den folgenden Schlüssel ändern:

   ```
   reg load HKLM\BROKENSYSTEM F:\windows\system32\config\SYSTEM.hiv

   REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "<DRIVE LETTER OF YOUR DATA DISK>:\MEMORY.DMP" /f
   REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "<DRIVE LETTER OF YOUR DATA DISK>:\MEMORY.DMP" /f
   
   reg unload HKLM\BROKENSYSTEM
   ```

5. [Trennen Sie den Betriebssystemdatenträger, und fügen Sie ihn wieder an die betroffene VM an](https://docs.microsoft.com/azure/virtual-machines/windows/troubleshoot-recovery-disks-portal).

6. Starten Sie den virtuellen Computer, und greifen Sie auf die serielle Konsole zu.

7. Wählen Sie „Nicht maskierbaren Interrupt (NMI) senden“ aus, um die Generierung des Speicherabbilds auszulösen.

   ![Nicht maskierbaren Interrupt senden](./media/boot-error-troubleshooting-windows/send-nonmaskable-interrupt.png)

8. Fügen Sie den Betriebssystemdatenträger erneut an einen virtuellen Wiederherstellungscomputer an, und rufen Sie die Abbilddatei ab.

## <a name="contact-microsoft-support"></a>Microsoft-Support kontaktieren

Nachdem Sie die Abbilddatei erfasst haben, können Sie sich an den Microsoft-Support wenden, um die Hauptursache zu bestimmen.
