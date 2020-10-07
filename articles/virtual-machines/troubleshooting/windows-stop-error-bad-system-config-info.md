---
title: 'Windows-kritischer Fehler: 0x00000074 Fehlerhafte Systemkonfigurationsinformationen'
description: Dieser Artikel enthält Schritte zum Beheben von Problemen, bei denen Windows nicht gestartet werden kann und aufgrund fehlerhafter Informationen zur Betriebssystemkonfiguration auf einem virtuellen Azure-Computer (VM) neu gestartet werden muss.
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: a1e47f6a-c7d5-4327-a7b0-ad48ed543641
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 08/24/2020
ms.author: v-miegge
ms.openlocfilehash: 4f2b338b8629209363acb7bbe0533831a089fe6f
ms.sourcegitcommit: 3792cf7efc12e357f0e3b65638ea7673651db6e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/29/2020
ms.locfileid: "91447317"
---
# <a name="windows-stop-error---0x00000074-bad-system-config-info"></a>Windows-kritischer Fehler: 0x00000074 Fehlerhafte Systemkonfigurationsinformationen

Dieser Artikel enthält Schritte zum Beheben von Problemen, bei denen Windows nicht gestartet werden kann und aufgrund fehlerhafter Informationen zur Betriebssystemkonfiguration auf einem virtuellen Azure-Computer (VM) neu gestartet werden muss.

## <a name="symptom"></a>Symptom

Wenn Sie die [Startdiagnose](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/boot-diagnostics) verwenden, um den Screenshot des virtuellen Computers anzuzeigen, sehen Sie, dass im Screenshot der Windows-Stillstandcode **#0x00000074** oder **BAD_SYSTEM_CONFIG_INFO** angezeigt wird.

*Auf dem PC ist ein Problem aufgetreten. Er muss neu gestartet werden. Sie können einen Neustart ausführen.* 
*Weitere Informationen zu diesem Problem und mögliche Lösungen finden Sie unter http://windows.com/stopcode* 
*Halten Sie folgende Infos bereit, wenn Sie den Support anrufen:* 
*Stillstandcode: BAD_SYSTEM_CONFIG_INFO*

  ![Der Windows-Stillstandcode 0x00000074, der auch als „BAD_SYSTEM_CONFIG_INFO“ angezeigt wird. Windows informiert den Benutzer, dass auf dem PC ein Problem aufgetreten ist und er neu gestartet werden muss.](./media/windows-stop-error-bad-system-config-info/1.png)

## <a name="cause"></a>Ursache

Der Stillstandcode **BAD_SYSTEM_CONFIG_INFO** tritt auf, wenn die Registrierungsstruktur **SYSTEM** beschädigt zu sein scheint. Dieser Fehler kann die folgenden Gründe haben:

- Die Registrierungsstruktur wurde nicht ordnungsgemäß geschlossen.
- Die Registrierungsstruktur ist beschädigt.
- Es fehlen Registrierungsschlüssel oder -werte.

## <a name="solution"></a>Lösung

### <a name="process-overview"></a>Prozessübersicht:

1. Erstellen Sie eine Reparatur-VM, und greifen Sie darauf zu.
1. Prüfen Sie auf eine Beschädigung der Struktur.
1. Aktivieren Sie die serielle Konsole und die Speicherabbilderfassung.
1. Erstellen Sie den virtuellen Computer neu.

> [!NOTE]
> Wenn dieser Fehler auftritt, ist das Gastbetriebssystem nicht funktionsfähig. Sie führen die Problembehandlung im Offlinemodus durch, um dieses Problem zu beheben.

### <a name="create-and-access-a-repair-vm"></a>Erstellen und Aufrufen einer Reparatur-VM

1. Führen Sie die Schritte 1-3 der [Reparaturbefehle für virtuelle Computer](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands) aus, um eine Reparatur-VM vorzubereiten.
1. Prüfen Sie auf eine Beschädigung der Struktur.
1. Stellen Sie über die Remotedesktopverbindung eine Verbindung mit der Reparatur-VM her.
1. Kopieren Sie den Ordner `<VOLUME LETTER OF BROKEN OS DISK>:\windows\system32\config`, und speichern Sie ihn entweder in der fehlerfreien Datenträgerpartition oder an einem anderen sicheren Speicherort. Sichern Sie diesen Ordner vorsichtshalber, da Sie kritische Registrierungsdateien bearbeiten werden. 

> [!NOTE]
> Erstellen Sie eine Kopie des Ordners `<VOLUME LETTER OF BROKEN OS DISK>:\windows\system32\config` als Sicherung für den Fall, dass Sie an der Registrierung vorgenommene Änderungen rückgängig machen müssen.

### <a name="check-for-hive-corruption"></a>Prüfen auf Beschädigung der Struktur

Mithilfe der folgenden Anweisungen können Sie feststellen, ob die Ursache eine Beschädigung der Struktur ist oder die Struktur nicht ordnungsgemäß geschlossen wurde. Wenn die Struktur nicht ordnungsgemäß geschlossen wurde, können Sie die Datei entsperren und Ihre VM reparieren.

1. Öffnen Sie auf der Reparatur-VM die Anwendung **Registrierungs-Editor**. Geben Sie in der Windows-Suchleiste „REGEDIT“ ein, um danach zu suchen.
1. Wählen Sie im Registrierungs-Editor den Eintrag **HKEY_LOCAL_MACHINE** aus, um ihn hervorzuheben, und wählen Sie dann **Datei > Struktur laden...** im Menü aus.
1. Navigieren Sie zu `<VOLUME LETTER OF BROKEN OS DISK>:\windows\system32\config\SYSTEM`, und wählen Sie **Öffnen** aus.
1. Wenn Sie zur Eingabe eines Namens aufgefordert werden, geben Sie **BROKENSYSTEM** ein.

   1. Wenn die Struktur nicht geöffnet werden kann oder leer ist, dann ist sie beschädigt. Falls die Struktur beschädigt ist, [öffnen Sie ein Supportticket](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

     ![Es wird ein Fehler angezeigt, der besagt, dass der Registrierungs-Editor die Struktur nicht laden kann.](./media/windows-stop-error-bad-system-config-info/2.png)

   1. Wenn die Struktur normal geöffnet wird, wurde sie nicht ordnungsgemäß geschlossen. Fahren Sie mit Schritt 5 fort.

1. Zum Korrigieren einer Struktur, die nicht ordnungsgemäß geschlossen wurde, markieren Sie **BROKENSYSTEM**, und wählen Sie dann **Datei > Struktur entfernen...** aus, um die Dateisperre aufzuheben.

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

Verwenden Sie [Schritt 5 der VM-Reparaturbefehle](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands#repair-process-example), um die VM neu zu erstellen.
