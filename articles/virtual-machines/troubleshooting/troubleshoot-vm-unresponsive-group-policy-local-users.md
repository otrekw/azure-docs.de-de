---
title: Virtueller Computer reagiert beim Anwenden der Gruppenrichtlinie für lokale Benutzer und Gruppen nicht
description: In diesem Artikel werden die Schritte zum Beheben von Problemen beschrieben, bei denen der Ladebildschirm beim Anwenden einer Richtlinie während des Starts einer Azure-VM (virtueller Computer) nicht mehr reagiert.
services: virtual-machines-windows
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: ff113268-f5bf-4e6a-986e-63b9b0ceff20
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.date: 04/02/2020
ms.author: v-mibufo
ms.openlocfilehash: 085880122e9a80e976cfe59686748b58aeba1922
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/02/2020
ms.locfileid: "80620810"
---
# <a name="virtual-machine-is-unresponsive-while-applying-group-policy-local-users--groups-policy"></a>Virtueller Computer reagiert beim Anwenden der Gruppenrichtlinie für lokale Benutzer und Gruppen nicht

In diesem Artikel werden die Schritte zum Beheben von Problemen beschrieben, bei denen der Ladebildschirm beim Anwenden einer Richtlinie während des Starts einer Azure-VM (virtueller Computer) nicht mehr reagiert.

## <a name="symptom"></a>Symptom

Wenn Sie die [Startdiagnose](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/boot-diagnostics) verwenden, um den Screenshot der VM anzuzeigen, sehen Sie die folgende Anzeige, die beim Laden nicht mehr reagiert: *Gruppenrichtlinie für lokale Benutzer und Gruppen wird angewendet*.

![Alternativer Text: Anzeige des Ladevorgangs zum Anwenden der Gruppenrichtlinie für lokale Benutzer und Gruppen (Windows Server 2012)](media/troubleshoot-vm-unresponsive-group-policy-local-users/1.png)

Windows Server 2012

![Alternativer Text: Anzeige des Ladevorgangs zum Anwenden der Gruppenrichtlinie für lokale Benutzer und Gruppen (Windows Server 2012 R2)](media/troubleshoot-vm-unresponsive-group-policy-local-users/2.png)

Windows Server 2012 R2

## <a name="cause"></a>Ursache

Die Symptome dieser Einfrierung werden durch einen Codefehler in der DLL (Dynamic Link Library) des Windows-Profildiensts (*profsvc.dll*) ausgelöst.

> [!NOTE]
> Diese Fehler betrifft nur Windows Server 2012 und Windows Server 2012 R2.

### <a name="the-policy-in-question"></a>Die betreffende Richtlinie

Die Anwendung der folgenden Richtlinie wird nicht beendet:

* *Computerkonfiguration\Richtlinien\Administrative Vorlagen\System/Benutzerprofile\Benutzerprofile, die älter als eine bestimmte Anzahl von Tagen sind, beim Systemneustart löschen*

Diese Richtlinie stürzt nur ab, wenn die folgenden sechs Bedingungen vorliegen:

* Die Richtlinie *Benutzerprofile, die älter als eine bestimmte Anzahl von Tagen sind, beim Systemneustart löschen* ist aktiviert.
* Sie verfügen über Profile, die die Altersanforderungen für die Bereinigung erfüllen.
* Sie verfügen über Komponenten, die für die Benachrichtigung zur Löschung von Profilen registriert wurden.
* Die Komponenten führen Aufrufe (direkt oder indirekt) aus, die Daten von SCM-Komponenten (Service Control Manager, Dienststeuerungs-Manager) von Windows abrufen, z. B. zum Starten, Beenden oder Abfragen von Informationen über einen Dienst.
* Sie verfügen über einen Dienst, der zum *automatischen* Starten konfiguriert ist.
* Dieser Dienst ist so festgelegt, dass er im Kontext eines Domänenkontos ausgeführt wird (im Gegensatz zur Verwendung eines integrierten Kontos, z. B. ein lokales System).

### <a name="the-code-defect"></a>Der Codefehler

Der Codefehler ist auf den Dienststeuerungs-Manager und die Profildienste zurückzuführen, die versuchen, sich gleichzeitig gegenseitig zu sperren. Sperren sollen verhindern, dass mehrere Dienste Änderungen an denselben Daten gleichzeitig vornehmen, was zu Beschädigungen führt. Normalerweise würden mehrere Sperranforderungen kein Problem verursachen. Da dies jedoch während des Starts geschieht, kann keiner der Dienste seine Prozesse abschließen, da sie aufeinander warten müssen.

### <a name="os-bug-5880648---service-control-manager-deadlocks-with-the-delete-user-profiles-on-restart-policy"></a>Betriebssystemfehler 5880648: Beim Dienststeuerungs-Manager tritt ein Deadlock bei der Richtlinie zum Löschen von Benutzerprofilen beim Neustart auf

Es gibt zwei Aktionen, die wie folgt überlappen:

* Aktion 1 ruft die Profilsperre ab, hat die SCM-Sperre jedoch noch nicht abgerufen.

  **AND**

* Aktion 2 hat die SCM-Sperre abgerufen, hat die Profilsperre jedoch noch nicht abgerufen.

Sobald der Deadlock auftritt, stürzt die Aktion beim Versuch ab, die zweite erforderliche Sperre abzurufen.

### <a name="action-1---old-profile-deletion-notification-has-profile-lock-needs-scm-lock"></a>Aktion 1: Benachrichtigung zur Löschung des alten Profils (besitzt **Profilsperre**, benötigt **SCM-Sperre**)

1. Zuerst ruft die Richtlinie, die zum Löschen alter Profile festgelegt wird, eine interne Profildienstsperre ab.

   * Diese Sperre dient dazu, die Interaktion von zwei Threads mit den Profilen zu verhindern, während der *Löschvorgang* im Gange ist.

2. Die Richtlinie ermittelt Profile, die alt genug sind, um gelöscht zu werden.
3. Eine Komponente, die für Benachrichtigungen zur Löschung von Profilen registriert wurde, versucht im Rahmen der Profillöschung, **einen Dienst zu starten**.
4. Bevor der Dienst gestartet wird, muss der Dienststeuerungs-Manager eine **interne SCM-Sperre** abrufen, die in den Threads von **Aktion 2** enthalten sind.

### <a name="action-2---profile-loadcreation-for-user-specific-data-has-scm-lock-needs-profile-lock"></a>Aktion 2: Lade-/Erstellvorgang des Profils für benutzerspezifische Daten (besitzt **SCM-Sperre**, benötigt **Profilsperre**)

1. Beim Start muss der SCM alle *automatisch startende* Dienste nach ihrer Gruppe sowie alle anderen Dienste sortieren, von denen sie abhängig sind.

2. **Der SCM ruft eine interne SCM-Sperre ab**, die zum Steuern des Zugriffs zum Starten, Beenden oder Konfigurieren von Diensten beim Sortieren der Dienste verwendet werden.

3. Sobald die Dienste sortiert wurden, durchläuft der SCM alle Dienste und startet sie.

4. Wenn der Dienst im Kontext eines Domänenkontos ausgeführt wird, muss ein Profil entweder für das Domänenkonto geladen oder erstellt werden, damit es benutzerspezifische Daten speichern kann.

5. Diese Anforderung wird an den **Profildienst** gesendet.

6. Der Profildienst benötigt Zugriff auf die **interne Sperre**, die in **Aktion 1** abgerufen wird.

## <a name="solution"></a>Lösung

### <a name="process-overview"></a>Prozessübersicht

1. Erstellen einer Reparatur-VM und Zugreifen darauf
2. Aktivieren Sie die serielle Konsole und die Speicherabbilderfassung.
3. Erstellen Sie die VM neu.
4. Erfassen der Speicherabbilddatei

   > [!NOTE]
   > Wenn dieser Startfehler auftritt, ist das Gastbetriebssystem nicht funktionsfähig. Sie müssen die Problembehandlung im Offlinemodus durchführen, um dieses Problem zu beheben.

### <a name="create-and-access-a-repair-vm"></a>Erstellen einer Reparatur-VM und Zugreifen darauf

1. Führen Sie die [Schritte 1-3 der VM-Reparaturbefehle](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands#repair-process-example) aus, um eine Reparatur-VM vorzubereiten.
2. Stellen Sie über eine Remotedesktopverbindung eine Verbindung mit der Reparatur-VM her.

### <a name="enable-serial-console-and-memory-dump-collection"></a>Aktivieren der seriellen Konsole und der Speicherabbilderfassung

Führen Sie das folgende Skript aus, um die Speicherabbilderfassung und die serielle Konsole zu aktivieren:

1. Öffnen Sie eine Eingabeaufforderungssitzung mit erhöhten Rechten („Als Administrator ausführen“).
2. Führen Sie die folgenden Befehle aus:

   * Aktivieren der seriellen Konsole:

     `bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /ems {<BOOT LOADER IDENTIFIER>} ON`

     `bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200`

3. Überprüfen Sie, ob der freie Speicherplatz auf dem Betriebssystemdatenträger der Größe des Arbeitsspeichers (RAM) auf der VM entspricht.

   * Wenn nicht genügend Speicherplatz auf dem Betriebssystemdatenträger vorhanden ist, sollten Sie den Speicherort für das Erstellen der Speicherabbilddatei ändern und auf einen an die VM angefügten Datenträger verweisen, der über genügend freien Speicherplatz verfügt. Ersetzen Sie `%SystemRoot%` in den folgenden Befehlen durch den Laufwerkbuchstaben (z. B. „F:“) des Datenträgers, um den Speicherort zu ändern.

#### <a name="suggested-configuration-to-enable-os-dump"></a>Empfohlene Konfiguration für das Aktivieren des Betriebssystemabbilds

**Laden des beschädigten Betriebssystemdatenträgers:**

`REG LOAD HKLM\BROKENSYSTEM <VOLUME LETTER OF BROKEN OS DISK>:\windows\system32\config\SYSTEM`

**Aktivieren für „ControlSet001“** :

`REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f`

`REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f`

`REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f`

**Aktivieren für „ControlSet002“** :

`REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f`

`REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f`

`REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f`

### <a name="rebuild-the-vm"></a>Neuerstellen der VM

Führen Sie [Schritt 5 der VM-Reparaturbefehle](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands#repair-process-example) aus, um die VM zu reassemblieren.

### <a name="collect-the-memory-dump-file"></a>Erfassen der Speicherabbilddatei

Sie müssen die erste Speicherabbilddatei für den Absturz erfassen und sich mit dieser an den Support wenden, um dieses Problem zu lösen. Gehen Sie wie folgt vor, um die Speicherabbilddatei zu erfassen:

#### <a name="attach-the-os-disk-to-a-new-repair-vm"></a>Anfügen des Betriebssystemdatenträgers an eine neue Reparatur-VM

1. Führen Sie die [Schritte 1-3 der VM-Reparaturbefehle](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands#repair-process-example) aus, um eine neue Reparatur-VM vorzubereiten.

2. Stellen Sie über eine Remotedesktopverbindung eine Verbindung mit der Reparatur-VM her.

#### <a name="locate-the-dump-file-and-submit-a-support-ticket"></a>Suchen nach der Speicherabbilddatei und Senden eines Supporttickets

1. Navigieren Sie auf der Reparatur-VM zum Windows-Ordner auf dem angefügten Betriebssystemdatenträger. Wenn der Laufwerkbuchstabe, der dem angefügten Betriebssystemdatenträger zugewiesen ist, „F“ lautet, müssen Sie zu „F:\Windows“ navigieren.

2. Suchen Sie nach der Datei „memory.dmp“, und [senden Sie dann ein Supportticket](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) mit der Speicherabbilddatei.

3. Wenn Sie die Datei „memory.dmp“ nicht finden, sollten Sie möglicherweise stattdessen [NMI-Aufrufe (Nicht maskierbarer Interrupt) in der seriellen Konsole](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/serial-console-windows#use-the-serial-console-for-nmi-calls) verwenden. Sie können den Leitfaden zum [Generieren eines Kernels oder eines vollständigen Absturzabbilds](https://docs.microsoft.com/windows/client-management/generate-kernel-or-complete-crash-dump) mithilfe von NMI-Aufrufen befolgen.
