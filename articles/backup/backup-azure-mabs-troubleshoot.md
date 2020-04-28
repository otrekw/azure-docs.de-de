---
title: Behandeln von Problemen mit Azure Backup Server
description: Behandeln von Problemen bei der Installation, bei der Registrierung von Azure Backup Server und bei der Sicherung und Wiederherstellung von Anwendungsworkloads.
ms.reviewer: srinathv
ms.topic: troubleshooting
ms.date: 07/05/2019
ms.openlocfilehash: 7a1cac63ba6497b8580c83fe2b666b020701283a
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/21/2020
ms.locfileid: "81688051"
---
# <a name="troubleshoot-azure-backup-server"></a>Behandeln von Problemen mit Azure Backup Server

Verwenden Sie die Informationen in der folgenden Tabelle für die Problembehandlung von Fehlern, die bei der Verwendung von Azure Backup Server auftreten.

## <a name="basic-troubleshooting"></a>Grundlegendes zur Problembehandlung

Wir empfehlen, dass Sie die nachstehende Prüfung durchführen, bevor Sie mit der Problembehandlung von Microsoft Azure Backup Server (MABS) beginnen:

- [Sicherstellen, dass der Microsoft Azure Recovery Services-Agent (MARS) auf dem neuesten Stand ist](https://go.microsoft.com/fwlink/?linkid=229525&clcid=0x409)
- [Sicherstellen, dass zwischen dem MARS-Agent und Azure Netzwerkkonnektivität besteht](https://docs.microsoft.com/azure/backup/backup-azure-mars-troubleshoot#the-microsoft-azure-recovery-service-agent-was-unable-to-connect-to-microsoft-azure-backup)
- Vergewissern Sie sich, dass Microsoft Azure Recovery Services ausgeführt wird (auf der Dienstkonsole). Führen Sie bei Bedarf einen Neustart durch, und wiederholen Sie den Vorgang.
- [Sicherstellen, dass am Speicherort des Ablageordners 5 - 10% freier Volumespeicherplatz vorhanden ist](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq#whats-the-minimum-size-requirement-for-the-cache-folder)
- Wenn bei der Registrierung ein Fehler auftritt, stellen Sie sicher, dass der Server, auf dem Sie Azure Backup Server installieren möchten, nicht bereits für einen anderen Tresor registriert wurde.
- Wenn die Pushinstallation nicht erfolgreich ist, sollten Sie überprüfen, ob der DPM-Agent bereits vorhanden ist. Wenn ja, können Sie den Agent deinstallieren und dann versuchen, die Installation durchzuführen.
- [Stellen Sie sicher, dass kein anderer Prozess oder Antivirensoftware in Azure Backup eingreift](https://docs.microsoft.com/azure/backup/backup-azure-troubleshoot-slow-backup-performance-issue#cause-another-process-or-antivirus-software-interfering-with-azure-backup)<br>
- Stellen Sie sicher, dass der SQL Agent-Dienst ausgeführt und auf dem MABS-Server auf „Automatisch“ festgelegt ist.<br>

## <a name="invalid-vault-credentials-provided"></a>Es wurden ungültige Tresoranmeldeinformationen angegeben.

| Vorgang | Fehlerdetails | Problemumgehung |
| --- | --- | --- |
| Registrieren bei einem Tresor | Es wurden ungültige Tresoranmeldeinformationen angegeben. Die Datei ist beschädigt oder enthält nicht die aktuellen Anmeldeinformationen für den Wiederherstellungsdienst. | Empfohlene Maßnahme: <br> <ul><li> Laden Sie die neueste Datei mit Anmeldeinformationen aus dem Tresor herunter, und versuchen Sie es dann erneut. <br>(ODER)</li> <li> Falls sich das Problem mit der vorherigen Aktion nicht beheben lässt, laden Sie die Anmeldeinformationen in ein anderes lokales Verzeichnis herunter, oder erstellen Sie einen neuen Tresor. <br>(ODER)</li> <li> Aktualisieren Sie die Datums- und Uhrzeiteinstellungen wie in [diesem Artikel](https://docs.microsoft.com/azure/backup/backup-azure-mars-troubleshoot#invalid-vault-credentials-provided) beschrieben. <br>(ODER)</li> <li> Überprüfen Sie, ob sich unter „C:\Windows\Temp“ mehr als 65.000 Dateien befinden. Verschieben Sie veraltete Dateien an einen anderen Speicherort, oder löschen Sie die Elemente im Ordner „Temp“. <br>(ODER)</li> <li> Überprüfen Sie den Status der Zertifikate. <br> a. Öffnen Sie **Computerzertifikate verwalten** (in der Systemsteuerung). <br> b. Erweitern Sie den Knoten **Persönlich** und den untergeordneten Knoten **Zertifikate**.<br> c.  Entfernen Sie das Zertifikat **Windows Azure Tools**. <br> d. Wiederholen Sie die Registrierung im Azure Backup-Client. <br> (ODER) </li> <li> Überprüfen Sie, ob eine Gruppenrichtlinie eingerichtet wurde. </li></ul> |

## <a name="replica-is-inconsistent"></a>Replikat inkonsistent

| Vorgang | Fehlerdetails | Problemumgehung |
| --- | --- | --- |
| Backup | Replikat inkonsistent | Stellen Sie sicher, dass die Option für die automatische Konsistenzprüfung im Schutzgruppen-Assistenten aktiviert ist. Weitere Informationen zu den Ursachen von Replikatinkonsistenz und entsprechende Vorschläge finden Sie im Artikel [Replikat ist inkonsistent](https://docs.microsoft.com/previous-versions/system-center/data-protection-manager-2006/cc161593(v=technet.10)).<br> <ol><li> Stellen Sie bei einer Systemstatus- oder BMR-Sicherung sicher, dass die Windows Server-Sicherung auf dem geschützten Server installiert ist.</li><li> Überprüfen Sie den DPM-Speicherpool auf dem DPM-/Microsoft Azure Backup Server auf Speicherplatzprobleme, und weisen Sie ggf. Speicher zu.</li><li> Überprüfen Sie den Zustand des Volumeschattenkopie-Diensts auf dem geschützten Server. Wenn er deaktiviert ist, legen Sie ihn auf manuellen Start fest. Starten Sie den Dienst auf dem Server. Kehren Sie anschließend zur DPM-/Microsoft Azure Backup Server-Konsole zurück, und starten Sie die Synchronisierung mit dem Auftrag zur Konsistenzprüfung.</li></ol>|

## <a name="online-recovery-point-creation-failed"></a>Fehler bei der Erstellung eines Onlinewiederherstellungspunkts.

| Vorgang | Fehlerdetails | Problemumgehung |
| --- | --- | --- |
| Backup | Fehler bei der Erstellung eines Onlinewiederherstellungspunkts. | **Fehlermeldung**: Der Microsoft Azure Backup-Agent konnte keine Momentaufnahme des ausgewählten Volumes erstellen. <br> **Problemumgehung**: Erhöhen Sie den Speicherplatz im Replikat- und Wiederherstellungspunktvolume.<br> <br> **Fehlermeldung**: Die Verbindung des Windows Azure Backup-Agents mit dem OBEngine-Dienst konnte nicht hergestellt werden. <br> **Problemumgehung**: Überprüfen Sie, ob OBEngine in der Liste mit den ausgeführten Diensten auf dem Computer vorhanden ist. Falls der OBEngine-Dienst nicht ausgeführt wird, können Sie den Befehl „net start OBEngine“ verwenden, um ihn zu starten. <br> <br> **Fehlermeldung**: Die Verschlüsselungspassphrase für diesen Server ist nicht festgelegt. Konfigurieren Sie eine Verschlüsselungspassphrase. <br> **Problemumgehung**: Konfigurieren Sie eine Verschlüsselungspassphrase. Führen Sie die folgenden Schritte aus, wenn dabei ein Fehler auftritt: <br> <ol><li>Stellen Sie sicher, dass das Scratchverzeichnis vorhanden ist. Der in der Registrierung **HKEY_LOCAL_MACHINE\Software\Microsoft\Windows Azure Backup\Config** angegebene Speicherort namens **ScratchLocation** muss vorhanden sein.</li><li> Falls der Ablageordner vorhanden ist, führen Sie eine erneute Registrierung mit der alten Passphrase durch. *Speichern Sie konfigurierte Verschlüsselungspassphrasen immer an einem sicheren Ort.*</li><ol>|

## <a name="the-original-and-external-dpm-servers-must-be-registered-to-the-same-vault"></a>Die ursprünglichen und externen DPM-Server müssen im gleichen Tresor registriert werden.

| Vorgang | Fehlerdetails | Problemumgehung |
| --- | --- | --- |
| Restore | **Fehlercode**: CBPServerRegisteredVaultDontMatchWithCurrent/Vault Credentials Error: 100110 <br/> <br/>**Fehlermeldung**: Die ursprünglichen und externen DPM-Server müssen im gleichen Tresor registriert werden. | **Ursache:** Dieses Problem tritt beim Wiederherstellen von Dateien auf einem anderen Server als dem Originalserver auf, wenn die Option „Externe DPM-Wiederherstellung“ verwendet wird und Folgendes gilt: Der Server, der wiederhergestellt werden soll, und der Originalserver, von dem die Daten gesichert werden, sind nicht demselben Recovery Services-Tresor zugeordnet.<br/> <br/>**Problemumgehung**: Stellen Sie sicher, dass sowohl der Originalserver als auch der andere Server unter demselben Tresor registriert sind.|

## <a name="online-recovery-point-creation-jobs-for-vmware-vm-fail"></a>Fehler bei Aufträgen zum Erstellen von Onlinewiederherstellungspunkten für virtuellen VMware-Computer

| Vorgang | Fehlerdetails | Problemumgehung |
| --- | --- | --- |
| Backup | Fehler bei Aufträgen zum Erstellen von Onlinewiederherstellungspunkten für virtuellen VMware-Computer. DPM hat beim Abrufen von ChangeTracking-Informationen einen VMware-Fehler festgestellt. ErrorCode – FileFaultFault (ID 33621) |  <ol><li> Setzen Sie CTK für VMware für die betroffenen virtuellen Computer zurück.</li> <li>Überprüfen Sie, ob der unabhängige Datenträger für VMware ggf. nicht vorhanden ist.</li> <li>Beenden Sie den Schutz für die betroffenen virtuellen Computer, und schützen Sie sie dann erneut mit der Schaltfläche **Aktualisieren**. </li><li>Führen Sie einen CC-Vorgang für die betroffenen virtuellen Computer aus.</li></ol>|

## <a name="the-agent-operation-failed-because-of-a-communication-error-with-the-dpm-agent-coordinator-service-on-the-server"></a>Fehler beim Agent-Vorgang. Bei der Kommunikation mit dem DPM-Agent-Koordinator-Dienst auf dem Server ist ein Fehler aufgetreten.

| Vorgang | Fehlerdetails | Problemumgehung |
| --- | --- | --- |
| Übertragen von Agents auf geschützte Server mithilfe von Push | Fehler beim Agent-Vorgang. Bei der Kommunikation mit dem DPM-Agent-Koordinator-Dienst auf \<Servername> ist ein Fehler aufgetreten. | **Falls die im Produkt empfohlene Aktion nicht funktioniert, können Sie die folgenden Schritte ausführen**: <ul><li> Falls Sie einen Computer aus einer nicht vertrauenswürdigen Domäne anfügen, führen Sie [diese Schritte](https://docs.microsoft.com/system-center/dpm/back-up-machines-in-workgroups-and-untrusted-domains?view=sc-dpm-2019) aus. <br> (ODER) </li><li> Falls Sie einen Computer aus einer vertrauenswürdigen Domäne anfügen, verwenden Sie die Problembehandlungsschritte aus [diesem Blog](https://techcommunity.microsoft.com/t5/system-center-blog/data-protection-manager-agent-network-troubleshooting/ba-p/344726). <br>(ODER)</li><li> Deaktivieren Sie die Antivirensoftware als Problembehandlungsschritt. Wird das Problem dadurch behoben, ändern Sie die Antivireneinstellungen, wie in [diesem Artikel](https://docs.microsoft.com/system-center/dpm/run-antivirus-server?view=sc-dpm-2019) beschrieben.</li></ul> |

## <a name="setup-could-not-update-registry-metadata"></a>Setup konnte Registrierungsmetadaten nicht aktualisieren.

| Vorgang | Fehlerdetails | Problemumgehung |
|-----------|---------------|------------|
|Installation | Setup konnten Registrierungsmetadaten nicht aktualisieren. Dieser Updatefehler kann zu einer übermäßigen Speicherbelegung führen. Um dies zu vermeiden, aktualisieren Sie den Registrierungseintrag zum Trimmen von ReFS. | Passen Sie den Registrierungsschlüssel **SYSTEM\CurrentControlSet\Control\FileSystem\RefsEnableInlineTrim** an. Legen Sie den Wert „Dword“ auf 1 fest. |
|Installation | Setup konnten Registrierungsmetadaten nicht aktualisieren. Dieser Updatefehler kann zu einer übermäßigen Speicherbelegung führen. Um dies zu vermeiden, aktualisieren Sie den Registrierungseintrag „Volume SnapOptimization“. | Erstellen Sie den Registrierungsschlüssel **SOFTWARE\Microsoft Data Protection Manager\Configuration\VolSnapOptimization\WriteIds** mit einem leeren Zeichenfolgenwert. |

## <a name="registration-and-agent-related-issues"></a>Probleme mit der Registrierung oder dem Agent

| Vorgang | Fehlerdetails | Problemumgehung |
| --- | --- | --- |
| Übertragen von Agents auf geschützte Server mithilfe von Push | Die für den Server angegebenen Anmeldeinformationen sind ungültig. | **Sollte die im Produkt empfohlene Aktion nicht funktionieren, führen Sie die folgenden Schritte aus**: <br> Installieren Sie den Schutz-Agent manuell auf dem Produktionsserver, wie in [diesem Artikel](https://docs.microsoft.com/system-center/dpm/deploy-dpm-protection-agent?view=sc-dpm-2019) beschrieben.|
| Azure Backup-Agent konnte keine Verbindung mit dem Azure Backup-Dienst herstellen (ID 100050). | Der Azure Backup-Agent konnte keine Verbindung mit dem Azure Backup-Dienst herstellen. | **Sollte die im Produkt empfohlene Aktion nicht funktionieren, führen Sie die folgenden Schritte aus**: <br>1. Führen Sie den folgenden Befehl an einer Eingabeaufforderung mit erhöhten Rechten aus: **psexec -i -s "C:\Programme\Internet Explorer\iexplore.exe**. Das Internet Explorer-Fenster wird geöffnet. <br/> 2. Navigieren Sie zu **Extras** > **Internetoptionen** > **Verbindungen** > **LAN-Einstellungen**. <br/> 3. Ändern Sie die Einstellungen dahingehend, dass ein Proxyserver verwendet wird. Geben Sie dann die Proxyserverdetails an.<br/> 4. Wenn Ihr Computer über einen eingeschränkten Internetzugang verfügt, stellen Sie sicher, dass die Firewalleinstellungen auf dem Computer oder Proxy diese [URLs](install-mars-agent.md#verify-internet-access) und [IP-Adressen](install-mars-agent.md#verify-internet-access) zulassen.|
| Fehler bei der Installation des Azure Backup-Agents | Bei der Installation von Microsoft Azure Recovery Services ist ein Fehler aufgetreten. Für alle Änderungen, die von der Microsoft Azure Recovery Services-Installation am System vorgenommen wurden, wurde ein Rollback ausgeführt. (ID: 4024) | Installieren Sie den Azure-Agent manuell.

## <a name="configuring-protection-group"></a>Konfigurieren von Schutzgruppen

| Vorgang | Fehlerdetails | Problemumgehung |
| --- | --- | --- |
| Konfigurieren von Schutzgruppen | Die Anwendungskomponente konnte von DPM auf dem geschützten Computer (Name des geschützten Computers) nicht aufgelistet werden. | Wählen Sie auf dem Konfigurationsbildschirm für Schutzgruppen auf der entsprechenden Datenquellen-/Komponentenebene auf **Aktualisieren** aus. |
| Konfigurieren von Schutzgruppen | Der Schutz kann nicht konfiguriert werden. | Falls es sich bei dem geschützten Server um einen SQL-Server handelt, überprüfen Sie, ob das Systemkonto (NTAuthority\System) auf dem geschützten Computer über Berechtigungen der SysAdmin-Rolle verfügt, wie in [diesem Artikel](https://docs.microsoft.com/previous-versions/system-center/system-center-2012-R2/hh757977(v=sc.12)) beschrieben.
| Konfigurieren von Schutzgruppen | Im Speicherpool für diese Schutzgruppe ist nicht genügend freier Speicherplatz vorhanden. | Die dem Speicherpool hinzugefügten Datenträger [dürfen keine Partition enthalten](https://docs.microsoft.com/previous-versions/system-center/system-center-2012-R2/hh758075(v=sc.12)). Löschen Sie alle vorhandenen Volumes auf den Datenträgern. Fügen Sie sie dann dem Speicherpool hinzu.|
| Richtlinienänderung |Die Sicherungsrichtlinie konnte nicht geändert werden. Error: Beim aktuellen Vorgang ist aufgrund eines internen Dienstfehlers [0x29834] ein Fehler aufgetreten. Wiederholen Sie den Vorgang nach einiger Zeit. Wenn das Problem weiterhin besteht, wenden Sie sich an den Microsoft-Support. | **Ursache:**<br/>Für diesen Fehler kommen drei mögliche Ursachen in Betracht: Die Sicherheitseinstellungen sind aktiviert, Sie versuchen, die Beibehaltungsdauer unter die oben angegebenen Mindestwerte zu verkürzen, oder Sie verwenden eine nicht unterstützte Version. (Nicht unterstützte Versionen sind niedrigere Versionen als Microsoft Azure Backup Server, Version 2.0.9052, und Azure Backup Server Update 1.) <br/>**Empfohlene Maßnahme:**<br/> Um mit richtlinienbezogenen Updates fortzufahren, müssen Sie die Beibehaltungsdauer auf einen größeren Wert als die angegebene minimale Beibehaltungsdauer festlegen. (Die minimale Beibehaltungsdauer beträgt sieben Tage für die tägliche Beibehaltungsdauer, vier Wochen für die wöchentliche Beibehaltungsdauer, drei Wochen für die monatliche Beibehaltungsdauer und ein Jahr für die jährliche Beibehaltungsdauer.) <br><br>Optional besteht eine weitere bevorzugte Vorgehensweise darin, den Backup-Agent und Azure Backup Server zu aktualisieren, um alle Sicherheitsupdates zu nutzen. |

## <a name="backup"></a>Backup

| Vorgang | Fehlerdetails | Problemumgehung |
| --- | --- | --- |
| Backup | Unerwarteter Fehler beim Ausführen des Auftrags. Das Gerät ist nicht bereit. | **Sollte die im Produkt empfohlene Aktion nicht funktionieren, führen Sie die folgenden Schritte aus:** <br> <ul><li>Legen Sie den Schattenkopie-Speicherbereich für die Elemente in der Schutzgruppe auf „Unbegrenzt“ fest, und führen Sie dann die Konsistenzprüfung aus.<br></li> (ODER) <li>Löschen Sie die vorhandene Schutzgruppe, und erstellen Sie mehrere neue Gruppen. Jede neue Schutzgruppe sollte ein einzelnes Element enthalten.</li></ul> |
| Backup | Wenn nur der Systemstatus gesichert werden soll, überprüfen Sie, ob ausreichend freier Speicherplatz auf dem geschützten Computer vorhanden ist, um die Systemstatussicherung zu speichern. | <ol><li>Stellen Sie sicher, dass die Windows Server-Sicherung auf dem geschützten Computer installiert ist.</li><li>Stellen Sie sicher, dass genügend Speicherplatz auf dem geschützten Computer für den Systemstatus vorhanden ist. Der einfachste Weg, dies zu überprüfen, besteht darin, zum geschützten Computer zu navigieren, die Windows Server-Sicherung zu öffnen, die Auswahlmöglichkeiten zu durchlaufen und dann „BMR“ auszuwählen. Auf der Benutzeroberfläche wird dann der erforderliche Speicherplatz angezeigt. Öffnen Sie **WSB** > **Lokale Sicherung** > **Sicherungszeitplan** > **Sicherungskonfiguration auswählen** > **Vollständiger Server** (die Größe wird angezeigt). Verwenden Sie diese Größe zur Überprüfung.</li></ol>
| Backup | Sicherungsfehler bei BMR | Wenn die BMR sehr groß ist, verschieben Sie einige Anwendungsdateien auf das Betriebssystemlaufwerk, und versuchen Sie es erneut. |
| Backup | Die Option zum erneuten Schützen eines virtuellen VMware-Computers in einer neuen Microsoft Azure Backup Server-Instanz wird nicht als zum Hinzufügen verfügbar angezeigt. | VMware-Eigenschaften verweisen auf eine alte, außer Betrieb genommene Instanz von Microsoft Azure Backup Server. So lösen Sie das Problem:<br><ol><li>Navigieren Sie in VCenter (entspricht SC-VMM) zur Registerkarte **Zusammenfassung**, und klicken Sie dann auf **Benutzerdefinierte Attribute**.</li>  <li>Löschen Sie den alten Microsoft Azure Backup Server-Namen aus dem Wert **DPMServer**.</li>  <li>Navigieren Sie zurück zum neuen Microsoft Azure Backup Server, und ändern Sie die PG.  Nachdem Sie die Schaltfläche **Aktualisieren** ausgewählt haben, wird der virtuelle Computer mit einem Kontrollkästchen zum Hinzufügen von Schutz angezeigt.</li></ol> |
| Backup | Fehler beim Zugreifen auf Dateien/freigegebene Ordner | Versuchen Sie, die Einstellungen der Antivirensoftware zu ändern (wie in diesem Artikel [Ausführen von Antivirensoftware auf dem DPM-Server](https://docs.microsoft.com/system-center/dpm/run-antivirus-server?view=sc-dpm-2019) beschrieben).|

## <a name="change-passphrase"></a>Ändern der Passphrase

| Vorgang | Fehlerdetails | Problemumgehung |
| --- | --- | --- |
| Ändern der Passphrase |Die eingegebene Sicherheits-PIN ist nicht richtig. Geben Sie die richtige Sicherheits-PIN an, um diesen Vorgang abzuschließen. |**Ursache:**<br/> Dieser Fehler tritt auf, wenn Sie eine ungültige oder abgelaufene Sicherheits-PIN eingeben, während Sie einen wichtigen Vorgang (z.B. das Ändern einer Passphrase) ausführen. <br/>**Empfohlene Maßnahme:**<br/> Um den Vorgang abzuschließen, müssen Sie eine gültige Sicherheits-PIN eingeben. Um die PIN abzurufen, melden Sie sich am Azure-Portal an, und navigieren Sie dann zum Recovery Services-Tresor. Navigieren Sie dann zu **Einstellungen** > **Eigenschaften** > **Sicherheits-PIN generieren**. Verwenden Sie diese PIN, um die Passphrase zu ändern. |
| Ändern der Passphrase |Fehler bei dem Vorgang. ID: 120002 |**Ursache:**<br/>Dieser Fehler tritt auf, wenn Sicherheitseinstellungen aktiviert sind oder Sie versuchen, die Passphrase zu ändern, wenn Sie eine nicht unterstützte Version verwenden.<br/>**Empfohlene Maßnahme:**<br/> Um die Passphrase zu ändern, müssen Sie zunächst den Backup-Agent auf die minimale Version 2.0.9052 aktualisieren. Außerdem müssen Sie Azure Backup Server auf mindestens Update 1 aktualisieren und dann eine gültige Sicherheits-PIN eingeben. Um die PIN abzurufen, melden Sie sich am Azure-Portal an, und navigieren Sie dann zum Recovery Services-Tresor. Navigieren Sie dann zu **Einstellungen** > **Eigenschaften** > **Sicherheits-PIN generieren**. Verwenden Sie diese PIN, um die Passphrase zu ändern. |

## <a name="configure-email-notifications"></a>Konfigurieren von E-Mail-Benachrichtigungen

| Vorgang | Fehlerdetails | Problemumgehung |
| --- | --- | --- |
| Einrichten von E-Mail-Benachrichtigungen mit einem Office 365-Konto |Fehler-ID: 2013| **Ursache:**<br> Versuchte Verwendung des Office 365-Kontos <br>**Empfohlene Maßnahme:**<ol><li> Stellen Sie zuerst sicher, dass für Ihren DPM-Server in Exchange „Zulassen von anonymem Relay für einen Empfangsconnector“ eingerichtet ist. Weitere Informationen dazu, wie Sie diese Option konfigurieren, finden Sie unter [Zulassen von anonymem Relay für einen Empfangsconnector](https://docs.microsoft.com/exchange/mail-flow/connectors/allow-anonymous-relay?view=exchserver-2019).</li> <li> Falls Sie kein internes SMTP-Relay verwenden können und für die Einrichtung Ihren Office 365-Server verwenden müssen, können Sie IIS als Relay einrichten. Konfigurieren Sie den DPM-Server so, dass er [SMTP mithilfe von ISS an O365 weiterleitet](https://docs.microsoft.com/exchange/mail-flow/test-smtp-with-telnet?view=exchserver-2019).<br><br>  Achten Sie darauf, das Format „benutzer\@domäne.com“ und *nicht* „domäne\benutzer“ zu verwenden.<br><br><li>Weisen Sie DPM an, den lokalen Servernamen als SMTP-Server zu verwenden (Port 587). Verweisen Sie DPM dann auf die Benutzer-E-Mail, von der die E-Mail-Nachrichten stammen sollen.<li> Der Benutzername und das Kennwort auf der DPM-SMTP-Setupseite sollten sich auf ein Domänenkonto in der Domäne beziehen, in der sich DPM befindet. </li><br> Nehmen Sie beim Ändern der SMTP-Serveradresse Änderungen an den neuen Einstellungen vor, schließen Sie das Feld mit den Einstellungen, und öffnen Sie es dann erneut. So können Sie prüfen, ob der neue Wert vorhanden ist.  Wenn Sie die Werte nur ändern und testen, werden die neuen Einstellungen unter Umständen nicht immer wirksam. Diese Überprüfung ist daher die empfohlene bewährte Methode.<br><br>Sie können diese Einstellungen während des Prozesses jederzeit löschen, indem Sie die DPM-Konsole schließen und die folgenden Registrierungsschlüssel bearbeiten: **HKLM\SOFTWARE\Microsoft\Microsoft Data Protection Manager\Notification\ <br/> Delete SMTPPassword and SMTPUserName keys**. Sie können sie der Benutzeroberfläche wieder hinzufügen, wenn Sie sie neu starten.

## <a name="common-issues"></a>Häufige Probleme

In diesem Abschnitt werden häufige Fehler beschrieben, die bei der Verwendung von Azure Backup Server auftreten könnten.

### <a name="cbpsourcesnapshotfailedreplicamissingorinvalid"></a>CBPSourceSnapshotFailedReplicaMissingOrInvalid

Fehlermeldung | Empfohlene Maßnahme |
-- | --
Bei der Sicherung ist ein Fehler aufgetreten, weil das Replikat für die Datenträgersicherung entweder ungültig ist oder fehlt. | Führen Sie zum Beheben des Problems die folgenden Schritte aus, und wiederholen Sie den Vorgang: <br/> 1. Erstellen Sie einen Datenträger-Wiederherstellungpunkt.<br/> 2. Führen Sie eine Konsistenzprüfung der Datenquelle durch. <br/> 3. Beenden Sie den Schutz der Datenquelle, und konfigurieren Sie den Schutz für diese Datenquelle dann erneut.

### <a name="cbpsourcesnapshotfailedreplicametadatainvalid"></a>CBPSourceSnapshotFailedReplicaMetadataInvalid

Fehlermeldung | Empfohlene Maßnahme |
-- | --
Bei der Quellvolume-Momentaufnahme ist ein Fehler aufgetreten, weil die Metadaten auf dem Replikat ungültig sind. | Erstellen Sie einen Datenträger-Wiederherstellungspunkt dieser Datenquelle, und wiederholen Sie die Onlinesicherung.

### <a name="cbpsourcesnapshotfailedreplicainconsistent"></a>CBPSourceSnapshotFailedReplicaInconsistent

Fehlermeldung | Empfohlene Maßnahme |
-- | --
Bei der Quellvolume-Momentaufnahme ist aufgrund eines inkonsistenten Datenquellenreplikats ein Fehler aufgetreten. | Führen Sie eine Konsistenzprüfung der Datenquelle durch, und versuchen Sie es erneut.

### <a name="cbpsourcesnapshotfailedreplicacloningissue"></a>CBPSourceSnapshotFailedReplicaCloningIssue

Fehlermeldung | Empfohlene Maßnahme |
-- | --
Fehler bei der Sicherung, weil das Datenträgersicherungs-Replikat nicht geklont werden konnte.| Stellen Sie sicher, dass alle vorherigen Datenträgersicherungs-Replikatdateien (VHDX) nicht eingebunden sind und während Onlinesicherungen keine Datenträger-zu-Datenträger-Sicherung erfolgt.
