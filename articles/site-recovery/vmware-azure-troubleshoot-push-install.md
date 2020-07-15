---
title: Beheben von Problemen bei Pushinstallationen von Mobility Service mit Azure Site Recovery
description: Beheben von Fehlern bei der Mobility Service-Installation während der Aktivierung der Replikation für die Notfallwiederherstellung mit Azure Site Recovery.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.author: ramamill
ms.date: 04/03/2020
ms.openlocfilehash: 1afd931249d4dbeda2b4b25f822837e2a564f959
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "80656325"
---
# <a name="troubleshoot-mobility-service-push-installation"></a>Beheben von Problemen bei Pushinstallationen von Mobility Service

Die Installation von Mobility Service ist ein wichtiger Schritt bei der Aktivierung von Replikation. Der Erfolg dieses Schritts hängt davon ab, ob Sie die Voraussetzungen erfüllen und mit unterstützten Konfigurationen arbeiten. Im Folgenden werden die Gründe für die häufigsten Fehler aufgeführt, die bei der Installation von Mobility Service auftreten können:

* [Fehler bei Anmeldeinformationen/Berechtigungen](#credentials-check-errorid-95107--95108)
* [Anmeldefehler](#login-failures-errorid-95519-95520-95521-95522)
* [Verbindungsfehler](#connectivity-failure-errorid-95117--97118)
* [Fehler bei Datei- und Druckerfreigabe](#file-and-printer-sharing-services-check-errorid-95105--95106)
* [WMI-Fehler (Windows Management Instrumentation, Windows-Verwaltungsinstrumentation)](#windows-management-instrumentation-wmi-configuration-check-error-code-95103)
* [Nicht unterstützte Betriebssysteme](#unsupported-operating-systems)
* [Nicht unterstützte Startkonfigurationen](#unsupported-boot-disk-configurations-errorid-95309-95310-95311)
* [Fehler bei der Installation des Volumeschattenkopie-Diensts (Volume Shadow Copy Service, VSS)](#vss-installation-failures)
* [Gerätename in GRUB-Konfiguration anstelle von Geräte-UUID](#enable-protection-failed-as-device-name-mentioned-in-the-grub-configuration-instead-of-uuid-errorid-95320)
* [Logische Volumeverwaltung (Logical Volume Management, LVM)](#lvm-support-from-920-version)
* [Neustartwarnungen](#install-mobility-service-completed-with-warning-to-reboot-errorid-95265--95266)

Wenn Sie die Replikation aktivieren, versucht Azure Site Recovery, den Mobility Service-Agent auf Ihrem virtuellen Computer (VM) zu installieren. Im Rahmen dieses Vorgangs versucht der Konfigurationsserver, eine Verbindung mit dem virtuellen Computer herzustellen und den Agent zu kopieren. Folgen Sie schrittweisen Anleitung zur Problembehandlung, um eine erfolgreiche Installation zu ermöglichen.

## <a name="credentials-check-errorid-95107--95108"></a>Überprüfung der Anmeldeinformationen (Fehler-ID: 95107 & 95108)

Überprüfen Sie, ob das beim Aktivieren der Replikation ausgewählte Benutzerkonto gültig und richtig ist. Azure Site Recovery benötigt ein **Stammkonto** oder ein Benutzerkonto mit **Administratorberechtigungen**, um die Pushinstallation ausführen zu können. Andernfalls wird die Pushinstallation auf dem Quellcomputer blockiert.

Vergewissern Sie sich unter Windows (**Fehler 95107**), dass das Benutzerkonto über Administratorzugriff auf dem Quellcomputer mit einem lokalen Konto oder einem Domänenkonto verfügt. Wenn Sie kein Domänenkonto verwenden, müssen Sie die Remote-Benutzerzugriffssteuerung auf dem lokalen Computer deaktivieren.

* So fügen Sie manuell einen Registrierungsschlüssel hinzu, der die Remote-Benutzerzugriffssteuerung deaktiviert:

  * `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System`
  * Fügen Sie einen neuen `DWORD`-Wert hinzu: `LocalAccountTokenFilterPolicy`
  * Legen Sie den Wert auf `1` fest.

* Geben Sie in der Eingabeaufforderung den folgenden Befehl ein, um den Registrierungsschlüssel hinzuzufügen:

  `REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`

Unter Linux (**Fehler 95108**) müssen Sie das **root**-Konto auswählen, um die Installation des Mobility Service-Agents erfolgreich ausführen zu können. Darüber hinaus sollten die SSH-Dateiübertragungsprotokoll-Dienste (SFTP) ausgeführt werden. Gehen Sie wie folgt vor, um das SFTP-Subsystem und Kennwortauthentifizierung in der Datei _sshd_config_ zu aktivieren:

1. Melden Sie sich als **Root**-Benutzer an.
1. Wechseln Sie zur Datei _/etc/ssh/sshd_config_, und suchen Sie die Zeile, die mit `PasswordAuthentication` beginnt.
1. Heben Sie die Auskommentierung der Zeile auf, und ändern Sie den Wert in `yes`.
1. Suchen Sie die Zeile, die mit `Subsystem` beginnt, und heben Sie die Auskommentierung der Zeile auf.
1. Starten Sie den `sshd` -Dienst neu.

Wenn Sie die Anmeldeinformationen des ausgewählten Benutzerkontos ändern möchten, befolgen Sie [diese Anweisungen](vmware-azure-manage-configuration-server.md#modify-credentials-for-mobility-service-installation).

## <a name="insufficient-privileges-failure-errorid-95517"></a>Fehler durch unzureichende Berechtigungen (Fehler-ID: 95517)

Wenn der zum Installieren des Mobility-Agents ausgewählte Benutzer nicht über Administratorberechtigungen verfügt, wird dem Konfigurationsserver/Prozessserver für horizontales Hochskalieren das Kopieren der Mobility-Agent-Software auf den Quellcomputer untersagt. Dieser Fehler ist das Ergebnis eines Fehlers vom Typ „Zugriff verweigert“. Stellen Sie sicher, dass das Benutzerkonto über Administratorberechtigungen verfügt.

Wenn Sie die Anmeldeinformationen des ausgewählten Benutzerkontos ändern möchten, befolgen Sie [diese Anweisungen](vmware-azure-manage-configuration-server.md#modify-credentials-for-mobility-service-installation).

## <a name="insufficient-privileges-failure-errorid-95518"></a>Fehler durch unzureichende Berechtigungen (Fehler-ID: 95518)

Wenn beim Versuch, sich beim Quellcomputer anzumelden, ein Fehler bei der Einrichtung der Vertrauensstellung zwischen der primären Domäne und der Arbeitsstation auftritt, tritt bei der Installation des Mobility-Agents ein Fehler mit der ID 95518 auf. Stellen Sie sicher, dass das Benutzerkonto zum Installieren des Mobility-Agents über Administratorberechtigungen für die Anmeldung über die primäre Domäne des Quellcomputers verfügt.

Wenn Sie die Anmeldeinformationen des ausgewählten Benutzerkontos ändern möchten, befolgen Sie [diese Anweisungen](vmware-azure-manage-configuration-server.md#modify-credentials-for-mobility-service-installation).

## <a name="login-failures-errorid-95519-95520-95521-95522"></a>Anmeldefehler (ErrorID: 95519, 95520, 95521, 95522)

In diesem Abschnitt werden Anmeldeinformationen- und Anmeldefehlermeldungen beschrieben.

### <a name="credentials-of-the-user-account-have-been-disabled-errorid-95519"></a>Die Anmeldeinformationen des Benutzerkontos wurden deaktiviert (Fehler-ID: 95519)

Das beim Aktivieren der Replikation ausgewählte Benutzerkonto wurde deaktiviert. Um das Benutzerkonto zu aktivieren, lesen Sie [diesen Artikel](/powershell/module/Microsoft.PowerShell.LocalAccounts/Enable-LocalUser), oder führen Sie den folgenden Befehl aus, wobei Sie _username_ durch den tatsächlichen Benutzernamen ersetzen.

`net user 'username' /active:yes`

### <a name="credentials-locked-out-because-of-multiple-failed-login-attempts-errorid-95520"></a>Die Anmeldeinformationen wurden aufgrund mehrerer fehlgeschlagener Anmeldeversuche gesperrt (ErrorID: 95520)

Durch mehrfache fehlerhafte Versuche des Zugriffs auf einen Computer wird das Benutzerkonto gesperrt. Der Fehler kann folgende Ursachen haben:

* Die bei der Konfiguration angegebene Anmeldeinformationen sind falsch.
* Das beim Aktivieren der Replikation ausgewählte Benutzerkonto ist falsch.

Ändern Sie die ausgewählten Anmeldeinformationen, indem Sie [diese Anweisungen](vmware-azure-manage-configuration-server.md#modify-credentials-for-mobility-service-installation) befolgen, und wiederholen Sie den Vorgang.

### <a name="logon-servers-arent-available-on-the-source-machine-errorid-95521"></a>Die Anmeldeserver sind auf dem Quellcomputer nicht verfügbar (ErrorID: 95521)

Dieser Fehler tritt auf, wenn die Anmeldeserver auf dem Quellcomputer nicht verfügbar sind. Wenn Anmeldeserver nicht verfügbar sind, treten bei Anmeldeanforderungen Fehler auf, und der Mobility-Agent kann nicht installiert werden. Um sich erfolgreich anzumelden, stellen Sie sicher, dass die Anmeldeserver auf dem Quellcomputer verfügbar sind, und starten Sie den Anmeldedienst. Weitere Informationen finden Sie unter [Windows- Anmeldeszenarien](/windows-server/security/windows-authentication/windows-logon-scenarios).

### <a name="logon-service-isnt-running-on-the-source-machine-errorid-95522"></a>Der Anmeldedienst wird auf dem Quellcomputer nicht ausgeführt (Fehler-ID: 95522)

Der Anmeldedienst wird auf dem Quellcomputer nicht ausgeführt. Dies führte zu einem Fehler bei der Anmeldeanforderung. Der Mobility-Agent kann nicht installiert werden. Um den Fehler zu beheben, verwenden Sie eine der folgenden Methoden, um den `Netlogon`-Dienst auf dem Quellcomputer zu starten:

* Führen Sie den Befehl `net start Netlogon` aus, um den `Netlogon`-Dienst über eine Eingabeaufforderung zu starten.
* Starten Sie im Task-Manager den `Netlogon`-Dienst.

## <a name="connectivity-failure-errorid-95117--97118"></a>Konnektivitätsfehler (Fehler-ID: 95117 und 97118)

Der Konfigurationsserver/Prozessserver für horizontales Hochskalieren versucht, eine Verbindung mit dem virtuellen Quellcomputer herzustellen, um den Mobility-Agent zu installieren. Dieser Fehler tritt auf, wenn der Quellcomputer aufgrund von Netzwerkverbindungsproblemen nicht erreichbar ist.

So beheben Sie den Fehler

* Stellen Sie sicher, dass Sie Ihren Quellcomputer über den Konfigurationsserver pingen können. Wenn Sie beim Aktivieren der Replikation einen Prozessserver für horizontales Skalieren ausgewählt haben, stellen Sie sicher, dass Sie Ihren Quellcomputer über den Prozessserver pingen können.

* Verwenden Sie in der Befehlszeile des Quellservercomputers `Telnet`, um den Konfigurationsserver oder den Prozessserver für horizontales Hochskalieren an HTTPS-Port 135 zu pingen, wie im folgenden Befehl gezeigt. Mit diesem Befehl wird überprüft, ob Probleme mit der Netzwerkkonnektivität oder Blockierungsproblem mit dem Firewallport auftreten.

  `telnet <CS/ scale-out PS IP address> <135>`

* Darüber hinaus gilt für Linux-VMs Folgendes:
  * Überprüfen Sie, ob die aktuellen Versionen der OpenSSH-, OpenSSH Server- und OpenSSL-Pakete installiert sind.
  * Stellen Sie unbedingt sicher, dass Secure Shell (SSH) auf Port 22 aktiviert ist und ausgeführt wird.
  * Die SFTP-Dienste müssen ausgeführt werden. Gehen Sie wie folgt vor, um das SFTP-Subsystem und die Kennwortauthentifizierung in der Datei _sshd_config_ zu aktivieren:

    1. Melden Sie sich als **Root**-Benutzer an.
    1. Navigieren Sie zur Datei _/etc/ssh/sshd_config_, und suchen Sie die Zeile, die mit `PasswordAuthentication` beginnt.
    1. Heben Sie die Auskommentierung der Zeile auf, und ändern Sie den Wert in `yes`.
    1. Suchen Sie die Zeile, die mit `Subsystem` beginnt, und heben Sie die Auskommentierung der Zeile auf.
    1. Starten Sie den `sshd` -Dienst neu.

* Ein Verbindungsversuch könnte fehlschlagen, wenn nach einer bestimmten Zeit keine ordnungsgemäße Antwort erfolgt ist, oder es ist ein Fehler bei einer hergestellten Verbindung aufgetreten, weil der verbundene Host nicht reagiert hat.
* Es kann sich um ein Verbindungs-, Netzwerk- oder Domänenproblem handeln. Es könnte sich auch um ein Problem beim Auflösen von DNS-Namen oder um ein Auslastungsproblem des TCP-Ports handeln. Überprüfen Sie, ob in Ihrer Domäne derartige bekannte Probleme vorkommen.

## <a name="connectivity-failure-errorid-95523"></a>Konnektivitätsfehler (Fehler-ID: 95523)

Dieser Fehler tritt auf, wenn das Netzwerk, in dem sich der Quellcomputer befindet, nicht gefunden wurde. Möglicherweise wurde er gelöscht oder ist nicht mehr verfügbar. Dieser Fehler kann nur behoben werden, indem Sie sicherstellen, dass das Netzwerk vorhanden ist.

## <a name="file-and-printer-sharing-services-check-errorid-95105--95106"></a>Überprüfung der Datei- und Druckerfreigabedienste (Fehler-ID: 95105 & 95106)

Überprüfen Sie nach der Konnektivitätsprüfung, ob der Datei- und Druckerfreigabedienst auf Ihrem virtuellen Computer aktiviert ist. Diese Einstellungen sind erforderlich, um den Mobility-Agent auf den Quellcomputer zu kopieren.

**Windows 2008 R2 und frühere Versionen**:

* Gehen Sie wie folgt vor, um die Datei- und Druckfreigabe über die Windows-Firewall zu aktivieren:
  1. Öffnen Sie **Systemsteuerung** > **System und Sicherheit** > **Windows-Firewall**. Wählen Sie im linken Bereich **Erweiterte Einstellungen** > **Eingehende Regeln** in der Konsolenstruktur aus.
  1. Suchen Sie nach den Regeln „Datei- und Druckerfreigabe (NB-Sitzung eingehend)“ und „Datei- und Druckerfreigabe (SMB eingehend)“.
  1. Klicken Sie mit der rechten Maustaste auf jede Regel, und klicken Sie dann auf **Regel aktivieren**.

* Gehen Sie wie folgt vor, um die Dateifreigabe mit der Gruppenrichtlinie zu aktivieren:
  1. Wechseln Sie zu **Start**, geben Sie `gpmc.msc` ein, und starten Sie die Suche.
  1. Öffnen Sie im Navigationsbereich die folgenden Ordner: **Richtlinie für lokaler Computer** > **Benutzerkonfiguration** > **Administrative Vorlagen** > **Windows-Komponenten** > **Netzwerkfreigabe**.
  1. Doppelklicken Sie im Detailbereich auf **Verhindern, dass Benutzer Dateien in ihrem Profil freigeben**.

     Wenn Sie die Gruppenrichtlinieneinstellung deaktivieren und die Option zum Freigeben von Dateien für den Benutzer aktivieren möchten, klicken Sie auf **Deaktiviert**.

  1. Klicken Sie zum Speichern der Änderungen auf **OK** .

  Weitere Informationen finden Sie unter [Enable or disable File Sharing with Group Policy (Aktivieren oder Deaktivieren der Dateifreigabe mit der Gruppenrichtlinie)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754359(v=ws.10)).

Um die Datei- und Druckerfreigabe für spätere Versionen von Windows oder Linux zu aktivieren, befolgen Sie die Anweisungen unter [Installieren von Mobility Service für die Notfallwiederherstellung von VMware-VMs und physischen Servern](vmware-azure-install-mobility-service.md).

## <a name="windows-management-instrumentation-wmi-configuration-check-error-code-95103"></a>Überprüfung der Konfiguration der Windows-Verwaltungsinstrumentation (Windows Management Instrumentation, WMI) (Fehlercode: 95103)

Aktivieren Sie nach der Überprüfung der Datei- und Druckerfreigabedienste den WMI-Dienst für private, öffentliche und Domänenprofile über die Firewall. Diese Einstellungen sind erforderlich, um die Remoteausführung auf dem Quellcomputer abzuschließen.

So aktivieren Sie WMI:

1. Öffnen Sie **Systemsteuerung** > **Sicherheit**, und klicken Sie auf **Windows Firewall**.
1. Klicken Sie auf **Einstellungen ändern**, und klicken Sie dann auf die Registerkarte **Ausnahmen**.
1. Aktivieren Sie im Fenster **Ausnahmen** das Kontrollkästchen für die Windows-Verwaltungsinstrumentation (WMI), um den WMI-Datenverkehr durch die Firewall zu ermöglichen.

Sie können den WMI-Datenverkehr durch die Firewall auch über eine Eingabeaufforderung mit dem folgenden Befehl aktivieren:

`netsh advfirewall firewall set rule group="windows management instrumentation (wmi)" new enable=yes`

Weitere Artikel zur WMI-Problembehandlung finden Sie unter den folgenden Links.

* [Grundlegende WMI-Tests](https://blogs.technet.microsoft.com/askperf/2007/06/22/basic-wmi-testing/)
* [WMI-Problembehandlung](/windows/win32/wmisdk/wmi-troubleshooting)
* [Problembehandlung bei WMI-Skripts und WMI-Diensten](/previous-versions/tn-archive/ff406382(v=msdn.10))

## <a name="unsupported-operating-systems"></a>Nicht unterstützte Betriebssysteme

Eine andere häufige Ursache für Fehler ist ein nicht unterstütztes Betriebssystem. Verwenden Sie ein unterstütztes Betriebssystem/eine unterstützte Kernel-Version, um eine erfolgreiche Installation von Mobility Service zu ermöglichen. Verwenden Sie keine privaten Patches.

In der [Dokumentation zur Unterstützungsmatrix](vmware-physical-azure-support-matrix.md#replicated-machines) finden Sie eine Liste aller von Azure Site Recovery unterstützen Betriebssysteme und Kernel-Versionen.

## <a name="unsupported-boot-disk-configurations-errorid-95309-95310-95311"></a>Nicht unterstützte Startdatenträgerkonfigurationen (Fehler-ID: 95309, 95310, 95311)

### <a name="boot-and-system-partitions--volumes-arent-the-same-disk-errorid-95309"></a>Start- und Systempartitionen/Volumes sind nicht derselbe Datenträger (ErrorID: 95309)

Vor Version 9.20 waren Start- und Systempartitionen/Volumes auf verschiedenen Datenträgern eine nicht unterstützte Konfiguration. Ab [Version 9.20](https://support.microsoft.com/help/4478871/update-rollup-31-for-azure-site-recovery) wird diese Konfiguration unterstützt.

### <a name="the-boot-disk-isnt-available-errorid-95310"></a>Der Startdatenträger ist nicht verfügbar (ErrorID: 95310)

Eine VM ohne Startdatenträger kann nicht geschützt werden. Ein Startdatenträger stellt die reibungslose Wiederherstellung der VM während eines Failovervorgangs sicher. Das Fehlen eines Startdatenträgers führt zu einem Fehler beim Starten des Computers nach dem Failover. Stellen Sie sicher, dass die VM einen Startdatenträger enthält, und wiederholen Sie den Vorgang. Außerdem werden auch mehrere Startdatenträger auf dem gleichen Computer nicht unterstützt.

### <a name="multiple-boot-disks-present-on-the-source-machine-errorid-95311"></a>Mehrere Startdatenträger auf dem Quellcomputer vorhanden (Fehler-ID: 95311)

Eine VM mit mehreren Startdatenträgern ist keine [unterstützte Konfiguration](vmware-physical-azure-support-matrix.md#linux-file-systemsguest-storage).

## <a name="system-partition-on-multiple-disks-errorid-95313"></a>Systempartition auf mehreren Datenträgern (Fehler-ID: 95313)

Vor Version 9.20 wurden Stammpartitionen oder Volumes auf mehreren Datenträgern als Konfiguration nicht unterstützt. Ab [Version 9.20](https://support.microsoft.com/help/4478871/update-rollup-31-for-azure-site-recovery) wird diese Konfiguration unterstützt.

## <a name="enable-protection-failed-as-device-name-mentioned-in-the-grub-configuration-instead-of-uuid-errorid-95320"></a>Fehler bei Schutz aktiviert, da in der GRUB-Konfiguration erwähnter Gerätename statt UUID vorliegt (Fehler-ID: 95320)

### <a name="possible-cause"></a>Mögliche Ursache

Die GRUB-Konfigurationsdateien (Grand Unified Bootloader, _/boot/grub/menu.lst_, _/boot/grub/grub.cfg_, _/boot/grub2/grub.cfg_ oder _/etc/default/grub_) enthalten anstelle der UUID (universell eindeutiger Bezeichner) als tatsächliche Gerätenamen ggf. den Wert für die Parameter **root** und **resume**. Site Recovery erfordert den UUID-Ansatz, da sich die Gerätenamen bei einem Neustart der VM ändern können. Beispielsweise geht die VM bei einem Failover möglicherweise nicht mit demselben Namen online, was zu Problemen führt.

Beispiel:

- Die folgende Zeile stammt aus der GRUB-Datei _/boot/grub2/grub.cfg_:

  `linux /boot/vmlinuz-3.12.49-11-default root=/dev/sda2  ${extra_cmdline} resume=/dev/sda1 splash=silent quiet showopts`

- Die folgende Zeile stammt aus der GRUB-Datei _„/boot/grub/menu.lst“_ :

  `kernel /boot/vmlinuz-3.0.101-63-default root=/dev/sda2 resume=/dev/sda1 splash=silent crashkernel=256M-:128M showopts vga=0x314`

> [!NOTE]
> Die GRUB-Zeilen enthalten tatsächliche Gerätenamen für die Parameter **root** und **resume** und keine UUID.

### <a name="how-to-fix"></a>Problembehandlung

Die Gerätenamen sollten mit dem entsprechenden UUID ersetzt werden.

1. Ermitteln Sie die UUID des Geräts durch Ausführen des Befehls `blkid \<device name>`.

   Beispiel:

   ```shell
   blkid /dev/sda1
   /dev/sda1: UUID="6f614b44-433b-431b-9ca1-4dd2f6f74f6b" TYPE="swap"
   blkid /dev/sda2
   /dev/sda2: UUID="62927e85-f7ba-40bc-9993-cc1feeb191e4" TYPE="ext3"
   ```

1. Ersetzen Sie den Gerätenamen nun durch die UUID im Format `root=UUID=\<UUID>`. Wenn Sie die Gerätenamen durch die UUID für die Parameter „root“ und „resume“ ersetzen, die in den Dateien _/boot/grub2/grub.cfg_, _/boot/grub2/grub.cfg_ oder _/etc/default/grub_ erwähnt werden, sehen die Zeilen in den Dateien wie die folgende Zeile aus:

   `kernel /boot/vmlinuz-3.0.101-63-default root=UUID=62927e85-f7ba-40bc-9993-cc1feeb191e4 resume=UUID=6f614b44-433b-431b-9ca1-4dd2f6f74f6b splash=silent crashkernel=256M-:128M showopts vga=0x314`

1. Starten Sie den Schutz neu.

## <a name="install-mobility-service-completed-with-warning-to-reboot-errorid-95265--95266"></a>Installation von Mobility Service mit Neustartwarnung abgeschlossen (ErrorID: 95265, 95266)

Site Recovery Mobility Service verfügt über zahlreiche Komponenten, z. B. über den Filtertreiber. Der Filtertreiber wird nur bei einem Systemneustart in den Arbeitsspeicher des Systems geladen. Korrekturen des Filtertreibers können nur dann umgesetzt werden, wenn ein neuer Filtertreiber zum Zeitpunkt des Systemneustarts geladen wird.

> [!IMPORTANT]
> Hierbei handelt es sich um eine Warnung, und die vorhandene Replikation funktioniert auch nach dem neuen Agent-Update. Sie können jederzeit einen Neustart durchführen, um die Vorteile des neuen Filtertreibers zu nutzen. Wenn Sie jedoch keinen Neustart durchführen, funktioniert der alte Filtertreiber weiterhin. So werden nach einem Update ohne Neustart, abgesehen vom Filtertreiber, **die Vorteile anderer Verbesserungen und Korrekturen in Mobility Service übernommen**. Es wird zwar empfohlen, nach jedem Upgrade einen Neustart durchzuführen, dies ist aber nicht zwingend erforderlich. Weitere Informationen dazu, wann ein Neustart zwingend erforderlich ist, finden Sie im Abschnitt [Neustart nach dem Upgrade des Mobility Service-Agents](service-updates-how-to.md#reboot-after-mobility-service-upgrade) in den Dienstupdates von Azure Site Recovery.

> [!TIP]
>Weitere Informationen zu bewährten Methoden für die Planung von Upgrades während des Wartungsfensters finden Sie unter [Unterstützung für die neuesten Betriebssystem-/Kernel-Versionen](service-updates-how-to.md#support-for-latest-operating-systemskernels) in den Dienstupdates von Azure Site Recovery.

## <a name="lvm-support-from-920-version"></a>LVM-Unterstützung ab Version 9.20

Vor Version 9.20 wurde LVM (Logical Volume Manager) nur für Datenträger unterstützt. Die `/boot`-Partition muss sich auf einer Datenträgerpartition und nicht auf einem LVM-Volume befinden.

Ab [Version 9.20](https://support.microsoft.com/help/4478871/update-rollup-31-for-azure-site-recovery) wird der [Betriebssystemdatenträger unter LVM](vmware-physical-azure-support-matrix.md#linux-file-systemsguest-storage) unterstützt.

## <a name="insufficient-space-errorid-95524"></a>Nicht genügend Speicherplatz (Fehler-ID: 95524)

Zum Kopieren des Mobility-Agents auf den Quellcomputer ist freier Speicherplatz von mindestens 100 MB erforderlich. Stellen Sie daher sicher, dass der Quellcomputer über den erforderlichen freien Speicherplatz verfügt, und wiederholen Sie den Vorgang.

## <a name="vss-installation-failures"></a>Fehler bei der VSS-Installation

Die VSS-Installation (Volumeschattenkopie-Dienst) ist Teil der Installation des Mobility Service-Agents. Dieser Dienst wird beim Generieren von anwendungskonsistenten Wiederherstellungspunkten verwendet. Fehler bei der VSS-Installation können aus verschiedenen Gründen auftreten. Die genauen Fehler können Sie unter _C:\ProgramData\ASRSetupLogs\ASRUnifiedAgentInstaller.log_ ermitteln. Einige häufige Fehler und die entsprechenden Lösungsschritte werden im folgenden Abschnitt beschrieben.

### <a name="vss-error--2147023170-0x800706be---exit-code-511"></a>VSS-Fehler -2147023170 [0x800706BE] – Exitcode 511

Dieses Problem tritt hauptsächlich auf, wenn eine Virenschutzsoftware die Vorgänge von Azure Site Recovery-Diensten blockiert.

So lösen Sie das Problem:

1. Überprüfen Sie die Liste der [vom Antivirenprogramm ausgeschlossene Ordner](vmware-azure-set-up-source.md#azure-site-recovery-folder-exclusions-from-antivirus-program).
1. Befolgen Sie die vom Anbieter der Antivirensoftware veröffentlichten Richtlinien, um die Blockierung der Registrierung der DLL in Windows aufzuheben.

### <a name="vss-error-7-0x7---exit-code-511"></a>VSS-Fehler 7 [0x7] – Exitcode 511

Dieser Fehler ist ein Laufzeitfehler, der verursacht wird, weil nicht genügend Arbeitsspeicher zum Installieren von VSS vorhanden ist. Erhöhen Sie den Speicherplatz, um den Installationsvorgang erfolgreich abzuschließen.

### <a name="vss-error--2147023824-0x80070430---exit-code-517"></a>VSS-Fehler -2147023824 [0x80070430] – Exitcode 517

Dieser Fehler tritt auf, wenn der Azure Site Recovery-VSS-Anbieter-Dienst [zum Löschen markiert](/previous-versions/ms838153(v=msdn.10)) ist. Versuchen Sie, VSS manuell auf dem Quellcomputer zu installieren, indem Sie den folgenden Befehl ausführen:

`"C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\InMageVSSProvider_Install.cmd"`

### <a name="vss-error--2147023841-0x8007041f---exit-code-512"></a>VSS-Fehler -2147023841 [0x8007041F] – Exitcode 512

Dieser Fehler tritt auf, wenn die Datenbank des VSS-Anbieterdiensts von Azure Site Recovery [gesperrt](/previous-versions/ms833798(v=msdn.10)) ist. Versuchen Sie, VSS manuell auf dem Quellcomputer zu installieren, indem Sie den folgenden Befehl über die Befehlszeile ausführen:

`"C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\InMageVSSProvider_Install.cmd"`

Überprüfen Sie im Falle eines Fehlers, ob ein Antivirenprogramm oder andere Dienste den Status **Wird gestartet** aufweisen. Ein Prozess mit dem Status **Wird gestartet** kann die Sperre für Datenbankdienste aufrecht erhalten. Dies führt zu Fehlern bei der Installation des VSS-Anbieters. Stellen Sie sicher, dass sich kein Dienst im Status **Wird gestartet** befindet, und wiederholen Sie dann den oben genannten Vorgang.

### <a name="vss-exit-code-806"></a>VSS Exitcode 806

Dieser Fehler tritt auf, wenn das für die Installation verwendete Benutzerkonto nicht über Berechtigungen zum Ausführen des `CSScript`-Befehls verfügt. Geben Sie die erforderlichen Berechtigungen für das Benutzerkonto an, um das Skript auszuführen und den Vorgang zu wiederholen.

### <a name="other-vss-errors"></a>Andere VSS-Fehler

Versuchen Sie, den VSS-Anbieterdienst manuell auf dem Quellcomputer zu installieren, indem Sie den folgenden Befehl in einer Befehlszeile ausführen:

`"C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\InMageVSSProvider_Install.cmd"`

## <a name="vss-error---0x8004e00f"></a>VSS-Fehler: 0x8004E00F

Dieser Fehler tritt üblicherweise bei der Installation des Mobility-Agents auf, wenn es zu Fehlern in `DCOM` kommt und sich `DCOM` in einem kritischen Zustand befindet.

Mithilfe des folgenden Verfahrens können Sie die Ursache des Fehlers ermitteln.

### <a name="examine-the-installation-logs"></a>Untersuchen der Installationsprotokolle

1. Öffnen Sie das Installationsprotokoll unter _C:\ProgramData\ASRSetupLogs\ASRUnifiedAgentInstaller.log_.
2. Folgende Fehlermeldung weist auf dieses Problem hin:

    ```Output
    Unregistering the existing application...
    Create the catalogue object
    Get the collection of Applications

    ERROR:

    - Error code: -2147164145 [0x8004E00F]
    - Exit code: 802
    ```

So lösen Sie das Problem:

Kontaktieren Sie das [Team für Microsoft Windows](https://aka.ms/Windows_Support), wenn Sie Hilfe beim Beheben des DCOM-Fehlers benötigen.

Wenn der DCOM-Fehler behoben ist, sollten Sie den VSS-Anbieter von Azure Site Recovery manuell mit dem folgenden Befehl in einer Eingabeaufforderung installieren:

`"C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\InMageVSSProvider_Install.cmd"`

Wenn die Anwendungskonsistenz für Ihre Anforderungen für die Notfallwiederherstellung nicht entscheidend ist, können Sie die Installation des VSS-Anbieters umgehen.

Gehen Sie folgendermaßen vor, um die Installation des VSS-Anbieters von Azure Site Recovery zu umgehen und diesen nach der Installation manuell zu installieren:

1. Installieren Sie den Mobilitätsdienst. Im folgenden Schritt tritt ein Installationsfehler auf: **Konfigurationen nach der Installation**.
1. So können Sie die Installation des VSS-Anbieters umgehen:
   1. Öffnen Sie das Installationsverzeichnis für Azure Site Recovery Mobility Service:

      _C:\Programme (x86)\Microsoft Azure Site Recovery\agent_

   1. Bearbeiten Sie die Installationsskripts _InMageVSSProvider_Install_ und _InMageVSSProvider_Uninstall.cmd_ für den VSS-Anbieter von Azure Site Recovery so, dass die Ausführung immer erfolgreich ist, indem Sie folgende Zeilen hinzufügen:

      ```plaintext
      rem @echo off
      setlocal
      exit /B 0
      ```

1. Führen Sie die Installation des Mobility-Agents manuell aus.
1. Wenn die Installation erfolgreich ist und mit dem nächsten Schritt (**Konfiguration**) fortgesetzt wird, sollten Sie die Zeilen wieder entfernen, die Sie hinzugefügt haben.
1. Öffnen Sie eine Eingabeaufforderung als Administrator, und führen Sie den folgenden Befehl aus, um den VSS-Anbieter zu installieren:

   `"C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\InMageVSSProvider_Install.cmd"`

1. Vergewissern Sie sich, dass der VSS-Anbieter von Azure Site Recovery als Dienst in den Windows-Diensten installiert ist. Öffnen Sie die MMC des Komponentendiensts, um zu bestätigen, dass der VSS-Anbieter aufgelistet wird.
1. Wenn die Installation des VSS-Anbieters weiterhin fehlschlägt, wenden Sie sich an den technischen Support, um die Berechtigungsfehler in der kryptografischen Anwendungsprogrammierschnittstelle (CAPI2) zu beheben.

## <a name="vss-provider-installation-fails-because-the-cluster-service-being-enabled-on-non-cluster-machine"></a>Installation des VSS-Anbieters schlägt aufgrund des aktivierten Clusterdiensts auf einem Nicht-Clustercomputer fehl

Dieses Problem bewirkt, dass die Installation des Mobility-Agents von Azure Site Recovery während der Installation des VSS-Anbieters von Azure Site Recovery fehlschlägt. Der Fehler tritt auf, weil ein Problem mit `COM+` vorliegt, das die Installation des VSS-Anbieters verhindert.

### <a name="to-identify-the-issue"></a>Identifizieren des Problems

In dem Protokoll, das sich auf dem Konfigurationsserver unter _C:\ProgramData\ASRSetupLogs\UploadedLogs\<date-time>UA_InstallLogFile.log_ befindet, ist die folgende Ausnahme enthalten:

```plaintext
COM+ was unable to talk to the Microsoft Distributed Transaction Coordinator (Exception from HRESULT: 0x8004E00F)
```

So lösen Sie das Problem:

1. Wenn es sich bei diesem Computer um einen Nicht-Clustercomputer handelt, sollten Sie sicherstellen, dass keine Clusterkomponenten verwendet werden.
1. Wenn die Komponenten nicht verwendet werden, entfernen Sie die Clusterkomponenten vom Computer.

## <a name="drivers-are-missing-on-the-source-server"></a>Fehlende Treiber auf dem Quellserver

Wenn die Installation des Mobility-Agents fehlschlägt, sollten Sie die Protokolle unter _C:\ProgramData\ASRSetupLogs_ darauf überprüfen, ob erforderliche Treiber in den Konfigurationen fehlen.

So lösen Sie das Problem:

1. Öffnen Sie die Registrierung mit einem Registrierungs-Editor wie `regedit.msc`.
1. Öffnen Sie den Knoten `HKEY_LOCAL_MACHINE\SYSTEM`.
1. Suchen Sie im Knoten `SYSTEM` nach den Konfigurationen.
1. Öffnen Sie jede Konfiguration, und überprüfen Sie, ob folgende Windows-Treiber vorhanden sind:

   * Atapi
   * Vmbus
   * Storflt
   * Storvsc
   * Intelide

1. Installieren Sie fehlende Treiber neu.

## <a name="next-steps"></a>Nächste Schritte

[Erfahren Sie mehr](vmware-azure-tutorial.md) zum Einrichten der Notfallwiederherstellung für VMware-VMs.
