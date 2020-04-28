---
title: Beheben von Problemen mit dem Windows Update-Agent in der Azure Automation-Updateverwaltung
description: Erfahren Sie, wie Sie Fehler und Probleme mit dem Windows Update-Agent mithilfe der Lösung für die Updateverwaltung beheben können.
services: automation
author: mgoedtel
ms.author: magoedte
ms.date: 01/16/2020
ms.topic: conceptual
ms.service: automation
ms.subservice: update-management
manager: carmonm
ms.openlocfilehash: 6983a2ac7ab5fafcb00aee0b72221a8540ea1668
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/21/2020
ms.locfileid: "81678972"
---
# <a name="troubleshoot-windows-update-agent-issues"></a>Beheben von Problemen mit dem Windows Update-Agent

Es gibt viele mögliche Gründe dafür, warum Ihr Computer in der Updateverwaltung nicht als „bereit“ (fehlerfrei) angezeigt wird. In der Updateverwaltung können Sie die Integrität eines Hybrid Runbook Worker-Agents überprüfen, um das zugrunde liegende Problem zu ermitteln. In diesem Artikel wird erläutert, wie Sie die Problembehandlung für Azure-Computer im Azure-Portal und für Nicht-Azure-Computer im [Offlineszenario](#troubleshoot-offline) ausführen.

Im Folgenden finden Sie die drei Bereitschaftszustände eines Computers:

* Bereit: Der Hybrid Runbook Worker ist bereitgestellt und wurde vor weniger als 1 Stunde zuletzt gesehen.
* Verbindung getrennt: Der Hybrid Runbook Worker ist bereitgestellt und wurde vor über 1 Stunde zuletzt gesehen.
* Nicht konfiguriert: Der Hybrid Runbook Worker wurde nicht gefunden oder hat das Onboarding noch nicht abgeschlossen.

> [!NOTE]
> Möglicherweise gibt es eine kleine Verzögerung zwischen der Anzeige im Azure-Portal und dem aktuellen Zustand eines Computers.

## <a name="start-the-troubleshooter"></a>Starten der Problembehandlung

Bei Azure-Computern klicken Sie im Portal unter der Spalte **Update-Agent-Bereitschaft** auf den Link **Problembehandlung**, um die Seite „Problembehandlung von Update-Agent“ zu öffnen. Bei Azure-fremden Computern gelangen Sie über den Link zu diesem Artikel. In der [Offlineanleitung](#troubleshoot-offline) erfahren Sie, wie Sie Probleme mit einem Nicht-Azure-Computer behandeln.

![Updateverwaltung: Liste von virtuellen Computern](../media/update-agent-issues/vm-list.png)

> [!NOTE]
> Zum Überprüfen der Integrität des Hybrid Runbook Workers muss die VM ausgeführt werden. Wenn der virtuelle Computer nicht ausgeführt wird, wird die Schaltfläche **Start the VM** (Virtuellen Computer starten) angezeigt.

Wählen Sie auf der Seite **Problembehandlung von Update-Agent** die Option „Überprüfungen ausführen“ aus, um mit der Problembehandlung zu beginnen. Die Problembehandlung verwendet die [Skriptausführung](../../virtual-machines/windows/run-command.md), um ein Skript auf dem Computer auszuführen, mit dem Abhängigkeiten überprüft werden. Wenn die Problembehandlung abgeschlossen ist, werden die Ergebnisse der Überprüfungen zurückgegeben.

![Seite „Problembehandlung von Update-Agent“](../media/update-agent-issues/troubleshoot-page.png)

Ergebnisse werden bei Verfügbarkeit auf der Seite angezeigt. In den Überprüfungsabschnitten wird aufgeführt, was in den einzelnen Überprüfungen enthalten ist.

![Überprüfungen bei „Problembehandlung von Update-Agent“](../media/update-agent-issues/update-agent-checks.png)

## <a name="prerequisite-checks"></a>Voraussetzungsprüfungen

### <a name="operating-system"></a>Betriebssystem

Die Betriebssystemprüfung untersucht, ob der Hybrid Runbook Worker unter einem der folgenden Betriebssysteme ausgeführt wird:

|Betriebssystem  |Notizen  |
|---------|---------|
|Windows Server 2012 und höher |.NET Framework 4.6 oder höher ist erforderlich. ([.NET Framework herunterladen](/dotnet/framework/install/guide-for-developers))<br/> Windows PowerShell 5.1 ist erforderlich.  ([Windows Management Framework 5.1 herunterladen](https://www.microsoft.com/download/details.aspx?id=54616))        |

### <a name="net-462"></a>.NET 4.6.2

Bei der .NET Framework-Überprüfung wird überprüft, ob auf dem System mindestens [.NET Framework 4.6.2](https://www.microsoft.com/en-us/download/details.aspx?id=53345) installiert ist.

### <a name="wmf-51"></a>WMF 5.1

Bei der WMF-Überprüfung wird überprüft, ob auf dem System die erforderliche WMF-Version (Windows Management Framework) installiert ist: [Windows Management Framework 5.1](https://www.microsoft.com/download/details.aspx?id=54616).

### <a name="tls-12"></a>TLS 1.2

Bei dieser Überprüfung wird untersucht, ob Sie zum Verschlüsseln der Kommunikation TLS 1.2 verwenden. TLS 1.0 wird von der Plattform nicht mehr unterstützt. Es wird empfohlen, dass Clients für die Kommunikation mit der Updateverwaltung TLS 1.2 verwenden.

## <a name="connectivity-checks"></a>Konnektivitätsprüfungen

### <a name="registration-endpoint"></a>Registrierungsendpunkt

Bei dieser Überprüfung wird untersucht, ob der Agent ordnungsgemäß mit dem Agent-Dienst kommunizieren kann.

Die Proxy- und Firewallkonfigurationen müssen die Kommunikation des Hybrid Runbook Worker-Agents mit dem Registrierungsendpunkt zulassen. Eine Liste der Adressen und zu öffnenden Ports finden Sie unter [Netzwerkplanung für Hybrid Worker](../automation-hybrid-runbook-worker.md#network-planning).

### <a name="operations-endpoint"></a>Endpunkt für Vorgänge

Bei dieser Überprüfung wird untersucht, ob der Agent ordnungsgemäß mit dem Auftragsruntime-Datendienst kommunizieren kann.

Die Proxy- und Firewallkonfigurationen müssen die Kommunikation des Hybrid Runbook Worker-Agents mit dem Auftragsruntime-Datendienst zulassen. Eine Liste der Adressen und zu öffnenden Ports finden Sie unter [Netzwerkplanung für Hybrid Worker](../automation-hybrid-runbook-worker.md#network-planning).

## <a name="vm-service-health-checks"></a>Integritätsprüfungen für den VM-Dienst

### <a name="monitoring-agent-service-status"></a>Status des Monitoring Agent-Diensts

Dieser Test überprüft, ob der Log Analytics-Agent für Windows (`healthservice`) auf dem Computer ausgeführt wird. Weitere Informationen zur Behandlung von Problemen mit dem Dienst finden Sie unter [Der Log Analytics-Agent für Windows wird nicht ausgeführt](hybrid-runbook-worker.md#mma-not-running).

Informationen zum erneuten Installieren des Log Analytics-Agents für Windows finden Sie unter [Installieren und Konfigurieren des Log Analytics-Agents für Windows](../../azure-monitor/learn/quick-collect-windows-computer.md#install-the-agent-for-windows).

### <a name="monitoring-agent-service-events"></a>Ereignisse des Monitoring Agent-Diensts

Dieser Test überprüft, ob in den letzten 24 Stunden 4502-Ereignisse im Azure Operations Manager-Protokoll auf dem Computer zu finden sind.

Weitere Informationen zu diesem Ereignis finden Sie im [Leitfaden zur Problembehandlung](hybrid-runbook-worker.md#event-4502) für dieses Ereignis.

## <a name="access-permissions-checks"></a>Überprüfung von Zugriffsberechtigungen

### <a name="machinekeys-folder-access"></a>Zugriff auf MachineKeys-Ordner

Bei der Überprüfung des Zugriffs auf den Crypto-Ordner wird untersucht, ob das lokale Systemkonto auf „C:\ProgramData\Microsoft\Crypto\RSA“ zugreifen kann.

## <a name="troubleshoot-offline"></a><a name="troubleshoot-offline"></a>Offlineproblembehandlung

Sie können die Problembehandlung offline auf einem Hybrid Runbook Worker ausführen, indem Sie das Skript lokal ausführen. Sie finden das Skript [Troubleshoot-WindowsUpdateAgentRegistration](https://www.powershellgallery.com/packages/Troubleshoot-WindowsUpdateAgentRegistration) im PowerShell-Katalog. Zum Ausführen des Skripts muss WMF 4.0 oder höher installiert sein. Informationen zum Herunterladen der neueste Version von PowerShell finden Sie unter [Installieren verschiedener Versionen von PowerShell](https://docs.microsoft.com/powershell/scripting/install/installing-powershell).

Die Ausgabe dieses Skripts ähnelt dem folgenden Beispiel:

```output
RuleId                      : OperatingSystemCheck
RuleGroupId                 : prerequisites
RuleName                    : Operating System
RuleGroupName               : Prerequisite Checks
RuleDescription             : The Windows Operating system must be version 6.2.9200 (Windows Server 2012) or higher
CheckResult                 : Passed
CheckResultMessage          : Operating System version is supported
CheckResultMessageId        : OperatingSystemCheck.Passed
CheckResultMessageArguments : {}

RuleId                      : DotNetFrameworkInstalledCheck
RuleGroupId                 : prerequisites
RuleName                    : .NET Framework 4.5+
RuleGroupName               : Prerequisite Checks
RuleDescription             : .NET Framework version 4.5 or higher is required
CheckResult                 : Passed
CheckResultMessage          : .NET Framework version 4.5+ is found
CheckResultMessageId        : DotNetFrameworkInstalledCheck.Passed
CheckResultMessageArguments : {}

RuleId                      : WindowsManagementFrameworkInstalledCheck
RuleGroupId                 : prerequisites
RuleName                    : WMF 5.1
RuleGroupName               : Prerequisite Checks
RuleDescription             : Windows Management Framework version 4.0 or higher is required (version 5.1 or higher is preferable)
CheckResult                 : Passed
CheckResultMessage          : Detected Windows Management Framework version: 5.1.17763.1
CheckResultMessageId        : WindowsManagementFrameworkInstalledCheck.Passed
CheckResultMessageArguments : {5.1.17763.1}

RuleId                      : AutomationAgentServiceConnectivityCheck1
RuleGroupId                 : connectivity
RuleName                    : Registration endpoint
RuleGroupName               : connectivity
RuleDescription             :
CheckResult                 : Failed
CheckResultMessage          : Unable to find Workspace registration information in registry
CheckResultMessageId        : AutomationAgentServiceConnectivityCheck1.Failed.NoRegistrationFound
CheckResultMessageArguments : {}

RuleId                      : AutomationJobRuntimeDataServiceConnectivityCheck
RuleGroupId                 : connectivity
RuleName                    : Operations endpoint
RuleGroupName               : connectivity
RuleDescription             : Proxy and firewall configuration must allow Automation Hybrid Worker agent to communicate with eus2-jobruntimedata-prod-su1.azure-automation.net
CheckResult                 : Passed
CheckResultMessage          : TCP Test for eus2-jobruntimedata-prod-su1.azure-automation.net (port 443) succeeded
CheckResultMessageId        : AutomationJobRuntimeDataServiceConnectivityCheck.Passed
CheckResultMessageArguments : {eus2-jobruntimedata-prod-su1.azure-automation.net}

RuleId                      : MonitoringAgentServiceRunningCheck
RuleGroupId                 : servicehealth
RuleName                    : Monitoring Agent service status
RuleGroupName               : VM Service Health Checks
RuleDescription             : HealthService must be running on the machine
CheckResult                 : Failed
CheckResultMessage          : Log Analytics for Windows service (HealthService) is not running
CheckResultMessageId        : MonitoringAgentServiceRunningCheck.Failed
CheckResultMessageArguments : {Log Analytics agent for Windows, HealthService}

RuleId                      : MonitoringAgentServiceEventsCheck
RuleGroupId                 : servicehealth
RuleName                    : Monitoring Agent service events
RuleGroupName               : VM Service Health Checks
RuleDescription             : Event Log must not have event 4502 logged in the past 24 hours
CheckResult                 : Failed
CheckResultMessage          : Log Analytics agent for Windows service Event Log (Operations Manager) does not exist on the machine
CheckResultMessageId        : MonitoringAgentServiceEventsCheck.Failed.NoLog
CheckResultMessageArguments : {Log Analytics agent for Windows, Operations Manager, 4502}

RuleId                      : CryptoRsaMachineKeysFolderAccessCheck
RuleGroupId                 : permissions
RuleName                    : Crypto RSA MachineKeys Folder Access
RuleGroupName               : Access Permission Checks
RuleDescription             : SYSTEM account must have WRITE and MODIFY access to 'C:\\ProgramData\\Microsoft\\Crypto\\RSA\\MachineKeys'
CheckResult                 : Passed
CheckResultMessage          : Have permissions to access C:\\ProgramData\\Microsoft\\Crypto\\RSA\\MachineKeys
CheckResultMessageId        : CryptoRsaMachineKeysFolderAccessCheck.Passed
CheckResultMessageArguments : {C:\\ProgramData\\Microsoft\\Crypto\\RSA\\MachineKeys}

RuleId                      : TlsVersionCheck
RuleGroupId                 : prerequisites
RuleName                    : TLS 1.2
RuleGroupName               : Prerequisite Checks
RuleDescription             : Client and Server connections must support TLS 1.2
CheckResult                 : Passed
CheckResultMessage          : TLS 1.2 is enabled by default on the Operating System.
CheckResultMessageId        : TlsVersionCheck.Passed.EnabledByDefault
CheckResultMessageArguments : {}
```

## <a name="next-steps"></a>Nächste Schritte

Informationen zur Behandlung weiterer Probleme mit Ihren Hybrid Runbook Workern finden Sie unter [Problembehandlung für Hybrid Runbook Worker](hybrid-runbook-worker.md).
