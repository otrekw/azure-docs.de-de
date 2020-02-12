---
title: Problembehandlung für Hybrid Runbook Worker von Azure Automation
description: Dieser Artikel bietet Informationen zur Problembehandlung für Hybrid Runbook Worker von Azure Automation.
services: automation
ms.service: automation
ms.subservice: ''
author: mgoedtel
ms.author: magoedte
ms.date: 11/25/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 4d804499116631be6f922f67f8b8f6c7063a6d5c
ms.sourcegitcommit: f0f73c51441aeb04a5c21a6e3205b7f520f8b0e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77030726"
---
# <a name="troubleshoot-hybrid-runbook-workers"></a>Problembehandlung für Hybrid Runbook Worker

Dieser Artikel bietet Informationen zur Problembehandlung für Hybrid Runbook Worker.

## <a name="general"></a>Allgemein

Der Hybrid Runbook Worker ist von einem Agent abhängig, um mit Ihrem Automation-Konto für die Registrierung des Workers zu kommunizieren, Runbookaufträge zu erhalten und den Status zu melden. Für Windows ist dies der Log Analytics-Agent für Windows (früher Microsoft Monitoring Agent (MMA) genannt). Für Linux handelt es sich um den Log Analytics-Agent für Linux.

### <a name="runbook-execution-fails"></a>Szenario: Fehler bei der Runbookausführung

#### <a name="issue"></a>Problem

Die Ausführung eines Runbooks wird mit der folgenden Fehlermeldung abgebrochen.

```error
"The job action 'Activate' cannot be run, because the process stopped unexpectedly. The job action was attempted three times."
```

Kurze Zeit nach drei Ausführungsversuchen wird Ihr Runbook angehalten. Es gibt Bedingungen, die den Abschluss des Runbooks unterbrechen können. Die dafür angezeigte Fehlermeldung enthält möglicherweise keine zusätzlichen Informationen.

#### <a name="cause"></a>Ursache

Folgende Ursachen kommen in Betracht:

* Die Runbooks können nicht bei lokalen Ressourcen authentifiziert werden.

* Der Hybrid Worker befindet sich hinter einem Proxy oder einer Firewall.

* Der für die Ausführung des Hybrid Runbook Worker-Features konfigurierte Computer erfüllt die Hardwaremindestanforderungen nicht.

#### <a name="resolution"></a>Lösung

Stellen Sie sicher, dass der Computer über einen ausgehenden Zugriff auf „*.azure-automation.net“ über Port 443 verfügt.

Computer, auf denen der Hybrid Runbook Worker ausgeführt werden soll, müssen die Mindestanforderungen an die Hardware erfüllen, damit sie als Host für dieses Feature konfiguriert werden können. Runbooks und die Hintergrundprozesse, die sie verwenden, führen möglicherweise zu einer starken Auslastung des Systems sowie zu Verzögerungen und Timeouts des Runbookauftrags.

Vergewissern Sie sich, dass der für die Ausführung des Hybrid Runbook Worker-Features vorgesehene Computer die Hardwaremindestanforderungen erfüllt. Wenn dies der Fall ist, überwachen Sie die CPU- und Arbeitsspeicherauslastung, um Zusammenhänge zwischen der Leistung der Hybrid Runbook Worker-Prozesse und Windows zu erkennen. Wenn der Arbeitsspeicher oder die CPU stark ausgelastet sind, kann dies auf die Notwendigkeit eines Upgrades von Ressourcen hindeuten. Sie können auch eine andere Computeressource auswählen, die die Mindestanforderungen erfüllt. Bei entsprechend großer Workload skalieren Sie sie nach Bedarf.

Prüfen Sie, ob im Ereignisprotokoll **Microsoft-SMA** ein entsprechendes Ereignis mit der Beschreibung *Win32-Prozess wurde mit dem Code [4294967295] beendet*vorhanden ist. Die Ursache dieses Fehlers liegt darin, dass Sie in Ihren Runbooks die Authentifizierung nicht konfiguriert haben oder dass Sie die „Ausführen als“-Anmeldeinformationen für die Hybrid Worker-Gruppe nicht angegeben haben. Überprüfen Sie die [Runbookberechtigungen](../automation-hrw-run-runbooks.md#runbook-permissions), und vergewissern Sie sich, dass Sie die Authentifizierung für Ihre Runbooks ordnungsgemäß konfiguriert haben.

### <a name="no-cert-found"></a>Szenario: Es wurde kein Zertifikat im Zertifikatspeicher auf dem Hybrid Runbook Worker gefunden.

#### <a name="issue"></a>Problem

Ein Runbook, das auf einem Hybrid Runbook Worker ausgeführt wird, schlägt mit der folgenden Fehlermeldung fehl.

```error
Connect-AzureRmAccount : No certificate was found in the certificate store with thumbprint 0000000000000000000000000000000000000000
At line:3 char:1
+ Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID -Appl ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : CloseError: (:) [Connect-AzureRmAccount], ArgumentException
    + FullyQualifiedErrorId : Microsoft.Azure.Commands.Profile.ConnectAzureRmAccountCommand
```
#### <a name="cause"></a>Ursache

Dieser Fehler tritt auf, wenn Sie versuchen, ein [„Ausführen als“-Konto](../manage-runas-account.md) in einem Runbook zu verwenden, das auf einem Hybrid Runbook Worker ausgeführt wird, auf dem das Zertifikat des „Ausführen als“-Kontos nicht vorhanden ist. Der Hybrid Runbook Worker enthält das Zertifikatobjekt, das für die ordnungsgemäße Funktion des „Ausführen als“-Kontos erforderlich ist. nicht standardmäßig lokal.

#### <a name="resolution"></a>Lösung

Wenn es sich bei Ihrem Hybrid Runbook Worker um eine Azure-VM handelt, können Sie stattdessen [verwaltete Identitäten für Azure-Ressourcen](../automation-hrw-run-runbooks.md#managed-identities-for-azure-resources) verwenden. Dieses Szenario gestattet Ihnen die Authentifizierung bei Azure-Ressourcen mithilfe der verwalteten Identität der Azure-VM anstelle des „Ausführen als“-Kontos, was die Authentifizierung vereinfacht. Wenn der Hybrid Runbook Worker ein lokaler Computer ist, müssen Sie das Zertifikat des „Ausführen als“-Kontos auf dem Computer installieren. Sehen Sie sich die Schritte an, die für die Ausführung des PowerShell-Runbooks „Export-RunAsCertificateToHybridWorker“ erforderlich sind und in [Ausführen von Runbooks auf einem Hybrid Runbook Worker](../automation-hrw-run-runbooks.md) erläutert werden, wenn Sie Informationen zum Installieren des Zertifikats erhalten möchten.

### <a name="error-403-on-registration"></a>Szenario: Fehler 403 während der Registrierung eines Hybrid Runbook Workers

#### <a name="issue"></a>Problem

Die Registrierungsphase des Workers schlägt fehlt, und der Fehler 403 wird zurückgegeben.

```error
"Forbidden: You don't have permission to access / on this server."
```

#### <a name="cause"></a>Ursache

Folgende Ursachen kommen in Betracht:
* In den Einstellungen des Agents wurde eine Arbeitsbereich-ID oder ein primärer Arbeitsbereichsschlüssel fehlerhaft eingegeben. 
* Der Hybrid Runbook Worker kann die Konfiguration nicht herunterladen, was zu einem Fehler bei der Kontoverknüpfung führt. Wenn Lösungen in Azure aktiviert werden, werden nur bestimmte Regionen zum Verknüpfen eines Log Analytics-Arbeitsbereichs und eines Automation-Kontos unterstützt. Möglicherweise wurde auch ein falsches Datum und/oder eine falsche Zeit auf dem Computer festgelegt. Wenn die Zeit ca. die 15 Minuten von der aktuellen Uhrzeit abweicht, tritt beim Onboarding ein Fehler auf.

#### <a name="resolution"></a>Lösung

##### <a name="mistyped-workspace-idkey"></a>Fehlerhaft eingegebene Arbeitsbereich-ID/fehlerhaft eingegebener Arbeitsbereichsschlüssel
Sehen Sie sich im Artikel „Verwalten und Warten des Log Analytics-Agents für Windows und Linux“ die Informationen im Abschnitt zu [Windows-Agent](../../azure-monitor/platform/agent-manage.md#windows-agent) bzw. [Linux-Agent](../../azure-monitor/platform/agent-manage.md#linux-agent) an, um zu überprüfen, ob die Arbeitsbereich-ID oder der Arbeitsbereichsschlüssel des Agents fehlerhaft eingegeben wurden.  Achten Sie darauf, im Azure-Portal die vollständige Zeichenfolge auszuwählen und sie korrekt zu kopieren und einzufügen.

##### <a name="configuration-not-downloaded"></a>Konfiguration nicht heruntergeladen

Ihr Log Analytics-Arbeitsbereich und Ihr Automation-Konto müssen sich in einer verknüpften Region befinden. Eine Liste unterstützter Regionen finden Sie unter [Arbeitsbereichzuordnungen](../how-to/region-mappings.md).

Möglicherweise müssen Sie auch das Datum und die Zeitzone auf Ihrem Computer aktualisieren. Wenn Sie eine benutzerdefinierte Zeitzone auswählen, sollten Sie sich vergewissern, dass die Zone der koordinierten Weltzeit entspricht, die sich von Ihrer lokalen Zeitzone unterscheiden kann.

## <a name="linux"></a>Linux

Der Linux Hybrid Runbook Worker ist abhängig vom [Log Analytics-Agent für Linux](../../azure-monitor/platform/log-analytics-agent.md), um mit Ihrem Automation-Konto für die Registrierung des Workers zu kommunizieren, Runbook-Aufträge zu erhalten und den Status zu melden. Wenn bei der Registrierung des Workers ein Fehler auftritt, gibt es mehrere mögliche Gründe dafür:

### <a name="oms-agent-not-running"></a>Szenario: Der Log Analytics-Agent für Linux wird nicht ausgeführt

#### <a name="issue"></a>Problem

Der Log Analytics-Agent für Linux wird nicht ausgeführt

#### <a name="cause"></a>Ursache

Wenn der Agent nicht ausgeführt wird, verhindert er die Kommunikation zwischen dem Linux Hybrid Runbook Worker und Azure Automation. Es kann verschiedene Gründe geben, warum der Agent nicht ausgeführt werden kann.

#### <a name="resolution"></a>Lösung

 Überprüfen Sie, dass der Agent ausgeführt wird, indem Sie den folgenden Befehl eingeben: `ps -ef | grep python`. Die Ausgabe sollte in etwa folgendermaßen aussehen, die Python-Prozesse mit dem **nxautomation**-Benutzerkonto. Wenn die Updateverwaltung oder Azure Automation nicht aktiviert ist, wird keiner der folgenden Prozesse ausgeführt.

```bash
nxautom+   8567      1  0 14:45 ?        00:00:00 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/main.py /var/opt/microsoft/omsagent/state/automationworker/oms.conf rworkspace:<workspaceId> <Linux hybrid worker version>
nxautom+   8593      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/state/automationworker/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
nxautom+   8595      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/<workspaceId>/state/automationworker/diy/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
```

Die folgende Liste enthält die Prozesse, die für einen Linux Hybrid Runbook Worker gestartet werden. Sie befinden sich alle im Verzeichnis `/var/opt/microsoft/omsagent/state/automationworker/`.

* **oms.conf**: Dies ist der Worker-Manager-Prozess. Er wird direkt vom DSC gestartet.

* **worker.conf**: Dies ist der automatisch registrierte Hybrid Worker-Prozess. Er wird vom Worker-Manager gestartet. Dieser Prozess dient der Updateverwaltung und ist für den Benutzer transparent. Dieser Prozess ist nicht vorhanden, wenn die Updateverwaltung auf dem Computer nicht aktiviert ist.

* **diy/worker.conf**: Dies ist der eigenständige Hybrid Worker-Prozess. Der eigenständige Hybrid Worker-Prozess wird verwendet, um Benutzerrunbooks auf dem Hybrid Runbook Worker auszuführen. Er unterscheidet sich vom automatisch registrierten Hybrid Worker-Prozess nur darin, dass eine andere Konfiguration verwendet wird. Dieser Prozess ist nicht vorhanden, wenn Azure Automation deaktiviert und der eigenständige Linux Hybrid Worker nicht registriert ist.

Wenn der Agent nicht ausgeführt wird, führen Sie zum Starten des Diensts den folgenden Befehl aus: `sudo /opt/microsoft/omsagent/bin/service_control restart`.

### <a name="class-does-not-exist"></a>Szenario: Die angegebene Klasse ist nicht vorhanden.

Wenn die Fehlermeldung **Die angegebene Klasse ist nicht vorhanden…** in `/var/opt/microsoft/omsconfig/omsconfig.log` angezeigt wird, muss der Log Analytics-Agent für Linux aktualisiert werden. Führen Sie den folgenden Befehl aus, um den Agent neu zu installieren:

```bash
wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <WorkspaceID> -s <WorkspaceKey>
```

## <a name="windows"></a>Windows

Der Windows Hybrid Runbook Worker ist abhängig vom [Log Analytics-Agent für Windows](../../azure-monitor/platform/log-analytics-agent.md), um mit Ihrem Automation-Konto für die Registrierung des Workers zu kommunizieren, Runbook-Aufträge zu erhalten und den Status zu melden. Wenn bei der Registrierung des Workers ein Fehler auftritt, gibt es mehrere mögliche Gründe dafür:

### <a name="mma-not-running"></a>Szenario: Microsoft Monitoring Agent wird nicht ausgeführt.

#### <a name="issue"></a>Problem

Auf dem Computer mit dem Hybrid Runbook Worker wird der Dienst `healthservice` nicht ausgeführt.

#### <a name="cause"></a>Ursache

Wenn der Microsoft Monitoring Agent-Dienst von Windows nicht ausgeführt wird, verhindert dieser Zustand die Kommunikation zwischen dem Hybrid Runbook Worker und Azure Automation.

#### <a name="resolution"></a>Lösung

Überprüfen Sie, dass der Agent ausgeführt wird, indem Sie den folgenden Befehl in PowerShell eingeben: `Get-Service healthservice`. Wenn der Dienst beendet wurde, geben Sie den folgenden Befehl in PowerShell ein, um ihn wieder zu starten: `Start-Service healthservice`.

### <a name="event-4502"></a>Szenario: Ereignis 4502 im Operations Manager-Protokoll

#### <a name="issue"></a>Problem

Im Ereignisprotokoll **Anwendungs- und Dienstprotokolle\Operations Manager** werden das Ereignis 4502 und eine Ereignisnachricht mit **Microsoft.EnterpriseManagement.HealthService.AzureAutomation.HybridAgent** und folgender Beschreibung angezeigt: *Das durch den Dienst „\<wsid\>.oms.opinsights.azure.com“ vorgelegte Zertifikat wurde nicht durch eine für Microsoft-Dienste verwendete Zertifizierungsstelle ausgestellt. Wenden Sie sich bitte an Ihren Netzwerkadministrator, um zu erfahren, ob ein Proxy ausgeführt wird, das die TLS/SSL-Kommunikation abfängt.*

#### <a name="cause"></a>Ursache

Dieses Problem kann auftreten, wenn Ihr Proxy oder Ihre Netzwerkfirewall die Kommunikation mit Microsoft Azure blockiert. Stellen Sie sicher, dass der Computer über einen ausgehenden Zugriff auf *.azure-automation.net über Port 443 verfügt. 

#### <a name="resolution"></a>Lösung

Protokolle werden lokal auf jedem Hybridworker unter "C:\ProgramData\Microsoft\System Center\Orchestrator\7.2\SMA\Sandboxes" gespeichert. Sie können überprüfen, ob die Ereignisprotokolle **Anwendungs- und Dienstprotokolle\Microsoft-SMA\Operations** und **Anwendungs- und Dienstprotokolle\Operations Manager** Warnungen oder Fehlerereignisse enthalten, die darauf hindeuten, dass ein Verbindungsproblem oder ein anderes Problem das Onboarding der Rolle in Azure Automation beeinträchtigt oder ein Problem bei der Ausführung regulärer Vorgänge vorliegt. Weitere Hilfe bei der Behandlung von Problemen mit dem Log Analytics-Agent finden Sie unter [Behandeln von Problemen mit dem Log Analytics Windows-Agent](../../azure-monitor/platform/agent-windows-troubleshoot.md).

[Runbookausgabe und -meldungen](../automation-runbook-output-and-messages.md) werden von Hybridworkern an Azure Automation gesendet (genau wie Runbookaufträge, die in der Cloud ausgeführt werden). Sie können auch die Verbose- und Progress-Streams auf dieselbe Weise aktivieren wie für andere Runbooks.

### <a name="corrupt-cache"></a>Szenario: Keine Meldungen vom Hybrid Runbook Worker

#### <a name="issue"></a>Problem

Der Computer mit dem Hybrid Runbook Worker wird zwar ausgeführt, im Arbeitsbereich werden jedoch keine Heartbeatdaten für den Computer angezeigt.

Die folgende Beispielabfrage zeigt die Computer in einem Arbeitsbereich und ihren jeweils letzten Heartbeat an:

```loganalytics
// Last heartbeat of each computer
Heartbeat
| summarize arg_max(TimeGenerated, *) by Computer
```

#### <a name="cause"></a>Ursache

Dieses Problem kann durch einen beschädigten Cache für den Hybrid Runbook Worker verursacht werden.

#### <a name="resolution"></a>Lösung

Melden Sie sich zur Behebung dieses Problems beim Hybrid Runbook Worker an, und führen Sie das folgende Skript aus. Dieses Skript beendet den Microsoft Monitoring Agent, entfernt den dazugehörigen Cache und startet den Dienst neu. Dadurch muss der Hybrid Runbook Worker seine Konfiguration noch mal aus Azure Automation herunterladen.

```powershell
Stop-Service -Name HealthService

Remove-Item -Path 'C:\Program Files\Microsoft Monitoring Agent\Agent\Health Service State' -Recurse

Start-Service -Name HealthService
```

### <a name="already-registered"></a>Szenario: Sie können keinen Hybrid Runbook Worker hinzufügen

#### <a name="issue"></a>Problem

Sie empfangen die folgende Nachricht bei dem Versuch, einen Hybrid Runbook Worker mithilfe des `Add-HybridRunbookWorker`-Cmdlets hinzuzufügen.

```error
Machine is already registered
```

#### <a name="cause"></a>Ursache

Dies kann dadurch verursacht werden, dass der Computer bereits bei einem anderen Automation-Konto registriert ist oder Sie versuchen, den Hybrid Runbook Worker nach dem Entfernen vom Computer wieder hinzuzufügen.

#### <a name="resolution"></a>Lösung

Um dieses Problem zu lösen, entfernen Sie den folgenden Registrierungsschlüssel und starten den `HealthService` erneut, und versuchen Sie erneut, das `Add-HybridRunbookWorker`-Cmdlet auszuführen:

`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\HybridRunbookWorker`

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Erhalten Sie Antworten von Azure-Experten über [Azure-Foren](https://azure.microsoft.com/support/forums/).
* Mit [@AzureSupport](https://twitter.com/azuresupport) verbinden – das offizielle Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit durch Verbinden der Azure-Community mit den richtigen Ressourcen: Antworten, Support und Experten.
* Wenn Sie weitere Hilfe benötigen, können Sie einen Azure-Supportvorgang anlegen. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und wählen Sie **Support erhalten**aus.