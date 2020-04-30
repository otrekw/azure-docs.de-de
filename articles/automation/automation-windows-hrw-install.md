---
title: Azure Automation Hybrid Runbook Worker (Windows)
description: Dieser Artikel enthält Informationen zum Installieren eines Azure Automation-Hybrid Runbook Workers, mit dem Sie Runbooks auf Windows-basierten Computern in Ihrem lokalen Datencenter oder einer Cloudumgebung ausführen können.
services: automation
ms.subservice: process-automation
ms.date: 12/10/2019
ms.topic: conceptual
ms.openlocfilehash: 53dfe07ebd4925c96290db140b6e613c38eef564
ms.sourcegitcommit: eefb0f30426a138366a9d405dacdb61330df65e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2020
ms.locfileid: "81617336"
---
# <a name="deploy-a-windows-hybrid-runbook-worker"></a>Bereitstellen eines Windows Hybrid Runbook Workers

Sie können mit dem Azure Automation-Feature „Hybrid Runbook Worker“ Runbooks direkt auf dem Computer ausführen, der die Rolle hostet, und außerhalb für Ressourcen in der Umgebung zur Verwaltung dieser lokalen Ressourcen. Azure Automation speichert und verwaltet Runbooks und übermittelt sie dann an dafür festgelegte Computer. Dieser Artikel beschreibt, wie Sie einen Hybrid Runbook Worker auf einem Windows-Computer bereitstellen.

Lesen Sie nach dem erfolgreichen Bereitstellen eines Runbook Workers [Running runbooks on a Hybrid Runbook Worker (Ausführen von Runbooks auf einem Hybrid Runbook Worker)](automation-hrw-run-runbooks.md), um zu erfahren, wie Sie Ihre Runbooks für die Automatisierung von Prozessen in Ihrem lokalen Datencenter oder in einer anderen Cloudumgebung konfigurieren.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

>[!NOTE]
>Dieser Artikel wurde aktualisiert und beinhaltet jetzt das neue Az-Modul von Azure PowerShell. Sie können das AzureRM-Modul weiterhin verwenden, das bis mindestens Dezember 2020 weiterhin Fehlerbehebungen erhält. Weitere Informationen zum neuen Az-Modul und zur Kompatibilität mit AzureRM finden Sie unter [Introducing the new Azure PowerShell Az module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0) (Einführung in das neue Az-Modul von Azure PowerShell). Installationsanweisungen für das Az-Modul auf Ihrem Hybrid Runbook Worker finden Sie unter [Installieren des Azure PowerShell-Moduls](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). In Ihrem Automation-Konto können Sie die Module mithilfe der Informationen unter [Aktualisieren von Azure PowerShell-Modulen in Azure Automation](automation-update-azure-modules.md) auf die neueste Version aktualisieren.

## <a name="windows-hybrid-runbook-worker-installation-and-configuration"></a>Installation und Konfiguration des Windows Hybrid Runbook Workers

Zum Installieren und Konfigurieren eines Windows Hybrid Runbook Workers können Sie eine der folgenden Methoden verwenden.

* Für Azure-VMs installieren Sie den Log Analytics-Agent für Windows mithilfe der [VM-Erweiterung für Windows](../virtual-machines/extensions/oms-windows.md). Die Erweiterung installiert den Log Analytics-Agent auf virtuellen Azure-Computern und registriert virtuelle Computer mithilfe einer Azure Resource Manager-Vorlage oder von PowerShell in einem vorhandenen Log Analytics-Arbeitsbereich. Nachdem der Agent installiert wurde, können Sie die VM einer Hybrid Runbook Worker-Gruppe in Ihrem Automation-Konto hinzufügen. Weitere Informationen finden Sie in den Schritten 3 und 4 im Abschnitt [Manuelle Bereitstellung](#manual-deployment).

* Verwenden Sie ein Automation-Runbook, um den Prozess zum Konfigurieren eines Windows-Computers vollständig zu automatisieren. Dies ist die empfohlene Methode für Computer in Ihrem Rechenzentrum oder einer anderen Cloudumgebung.

* Befolgen Sie ein schrittweises Verfahren, um die Hybrid Runbook Worker-Rolle auf Ihrer Nicht-Azure-VM zu installieren und zu konfigurieren.

> [!NOTE]
> Um die Konfiguration der Server zu verwalten, die die Hybrid Runbook Worker-Rolle mit DSC (Desired State Configuration) unterstützen, müssen Sie die Server als DSC-Knoten hinzufügen.

### <a name="minimum-requirements-for-windows-hybrid-runbook-worker"></a>Mindestanforderungen für Windows Hybrid Runbook Worker

Die folgenden Mindestanforderungen gelten für einen Windows Hybrid Runbook Worker:

* Windows Server 2012 oder höher
* Windows PowerShell 5.1 oder höher ([WMF 5.1 herunterladen](https://www.microsoft.com/download/details.aspx?id=54616))
* .NET Framework 4.6.2 oder höher
* Zwei Kerne
* 4 GB RAM
* Port 443 (ausgehend)

### <a name="network-configuration"></a>Netzwerkkonfiguration

Weitere Netzwerkanforderungen für den Hybrid Runbook Worker finden Sie unter [Konfigurieren des Netzwerks](automation-hybrid-runbook-worker.md#network-planning).

### <a name="server-onboarding-for-management-with-automation-dsc"></a>Serveronboarding für die Verwaltung mit Azure Automation DSC

Informationen zum Onboarding von Servern für die Verwaltung mit DSC finden Sie unter [Integrieren von Computern für die Verwaltung durch Azure Automation DSC](automation-dsc-onboarding.md).

Beim Aktivieren der [Lösung für die Updateverwaltung](../operations-management-suite/oms-solution-update-management.md) werden alle mit dem Log Analytics-Arbeitsbereich verbundenen Windows-Computer automatisch als Hybrid Runbook Worker konfiguriert, um in dieser Lösung enthaltene Runbooks zu unterstützen. Es erfolgt jedoch keine Registrierung des Workers für Hybrid Runbook Worker-Gruppen, die in Ihrem Automation-Konto bereits definiert wurden.

### <a name="addition-of-the-computer-to-a-hybrid-runbook-worker-group"></a>Hinzufügen des Computers zu einer Hybrid Runbook Worker-Gruppe

Sie können den Workercomputer einer Hybrid Runbook Worker-Gruppe in Ihrem Automation-Konto hinzufügen. Beachten Sie, dass Sie Automation-Runbooks unterstützen müssen, sofern Sie sowohl für die Lösung als auch die Mitgliedschaft in der Hybrid Runbook Worker-Gruppe dasselbe Konto verwenden. Diese Funktionalität wurde Version 7.2.12024.0 des Hybrid Runbook Worker hinzugefügt.

## <a name="automated-deployment"></a>Automatisierte Bereitstellung

Führen Sie auf dem Zielcomputer die folgenden Schritte aus, um die Installation und Konfiguration der Windows Hybrid Worker-Rolle zu automatisieren.

### <a name="step-1---download-the-powershell-script"></a>Schritt 1: Herunterladen des PowerShell-Skripts

Laden Sie das Skript **New-OnPremiseHybridWorker.ps1** aus dem [PowerShell-Katalog](https://www.powershellgallery.com/packages/New-OnPremiseHybridWorker) herunter. Der Download sollte direkt über den Computer stattfinden, auf dem die Hybrid Runbook Worker-Rolle ausgeführt wird, oder über einen anderen Computer in Ihrer Umgebung. Wenn Sie das Skript heruntergeladen haben, kopieren Sie es auf Ihren Worker. Das Skript **New-OnPremiseHybridWorker.ps1** verwendet bei der Ausführung die nachstehend beschriebenen Parameter.

| Parameter | Status | BESCHREIBUNG |
| --------- | ------ | ----------- |
| `AAResourceGroupName` | Obligatorisch. | Der Name der Ressourcengruppe, die Ihrem Automation-Konto zugeordnet ist. |
| `AutomationAccountName` | Obligatorisch. | Der Name Ihres Automation-Kontos.
| `Credential` | Optional | Die Anmeldeinformationen, die beim Anmelden bei der Azure-Umgebung verwendet werden sollen. |
| `HybridGroupName` | Obligatorisch. | Der Name einer Hybrid Runbook Worker-Gruppe, die Sie als Ziel für die Runbooks angeben, die dieses Szenario unterstützen. |
| `OMSResourceGroupName` | Optional | Der Name der Ressourcengruppe für den Log Analytics-Arbeitsbereich. Wenn diese Ressourcengruppe nicht angegeben ist, wird der Wert von `AAResourceGroupName` verwendet. |
| `SubscriptionID` | Obligatorisch. | Der Bezeichner des Azure-Abonnements, das Ihrem Automation-Konto zugeordnet ist. |
| `TenantID` | Optional | Der Bezeichner der Mandantenorganisation, die Ihrem Automation-Konto zugeordnet ist. |
| `WorkspaceName` | Optional | Name des Log Analytics-Arbeitsbereichs. Falls kein Log Analytics-Arbeitsbereich vorhanden ist, wird dieser durch das Skript erstellt und konfiguriert. |

> [!NOTE]
> Wenn Sie Lösungen aktivieren, unterstützt Azure Automation nur bestimmte Regionen für das Verknüpfen mit einem Log Analytics-Arbeitsbereich und einem Automation-Konto. Eine Liste der unterstützten Zuordnungspaare finden Sie unter [Regionszuordnung für Automation-Konto und Log Analytics-Arbeitsbereich](how-to/region-mappings.md).

### <a name="step-2---open-windows-powershell-command-line-shell"></a>Schritt 2: Öffnen einer Windows PowerShell-Befehlszeilenshell

Öffnen Sie **Windows PowerShell** über den **Start**bildschirm im Administratormodus.

### <a name="step-3---run-the-powershell-script"></a>Schritt 3: Ausführen des PowerShell-Skripts

Navigieren Sie in der PowerShell-Befehlszeilenshell zu dem Ordner, der das heruntergeladene Skript enthält. Ändern Sie die Werte der Parameter `AutomationAccountName`, `AAResourceGroupName`, `OMSResourceGroupName`, `HybridGroupName`, `SubscriptionID` und `WorkspaceName`. Führen Sie das Skript dann aus.

Nach dem Ausführen des Skripts werden Sie aufgefordert, sich bei Azure zu authentifizieren. Sie müssen sich mit einem Konto anmelden, das Mitglied der Rolle „Abonnement-Administratoren“ sowie Co-Administrator des Abonnements ist.

```powershell-interactive
.\New-OnPremiseHybridWorker.ps1 -AutomationAccountName <NameofAutomationAccount> -AAResourceGroupName <NameofResourceGroup>`
-OMSResourceGroupName <NameofOResourceGroup> -HybridGroupName <NameofHRWGroup> `
-SubscriptionID <AzureSubscriptionId> -WorkspaceName <NameOfLogAnalyticsWorkspace>
```

### <a name="step-4---install-nuget"></a>Schritt 4: Installieren von NuGet

Sie werden aufgefordert, der Installation von NuGet zuzustimmen und sich mit Ihren Azure-Anmeldeinformationen zu authentifizieren. Wenn Sie nicht über die neueste Version von NuGet verfügen, können Sie sie über [Verfügbare NuGet-Distributionsversionen](https://www.nuget.org/downloads) beziehen.

### <a name="step-5---verify-the-deployment"></a>Schritt 5: Überprüfen der Bereitstellung

Nach Abschluss des Skripts werden auf der Seite „Hybrid Worker-Gruppen“ die neue Gruppe und die Anzahl der Mitglieder angezeigt. Wenn die Gruppe bereits vorhanden ist, wird die Anzahl der Mitglieder erhöht. Sie können die Gruppe in der Liste auf der Seite „Hybrid Worker-Gruppen“ und dann die Kachel **Hybrid Worker** auswählen. Auf der Seite „Hybrid Worker“ werden die einzelnen Mitglieder der Gruppe aufgelistet.

## <a name="manual-deployment"></a>Manuelle Bereitstellung

Führen Sie auf dem Zielcomputer die ersten beiden Schritte einmalig für Ihre Automation-Umgebung aus. Führen Sie dann die verbleibenden Schritte für jeden Workercomputer aus.

### <a name="step-1---create-a-log-analytics-workspace"></a>Schritt 1: Erstellen eines Log Analytics-Arbeitsbereichs

Wenn Sie noch nicht über einen Log Analytics-Arbeitsbereich verfügen, lesen Sie zuerst die [Entwurfsanleitung für Azure Monitor-Protokolle](../azure-monitor/platform/design-logs-deployment.md), bevor Sie einen Arbeitsbereich erstellen.

### <a name="step-2---add-the-automation-solution-to-the-log-analytics-workspace"></a>Schritt 2: Hinzufügen der Automation-Lösung zum Log Analytics-Arbeitsbereich

Die Automation-Lösung fügt Azure Automation Funktionen hinzu, einschließlich Unterstützung für den Hybrid Runbook Worker. Wenn Sie die Lösung Ihrem Log Analytics-Arbeitsbereich hinzufügen, werden die Workerkomponenten, die Sie gemäß der Beschreibung im nächsten Schritt installieren, automatisch auf den Agent-Computer gepusht.

Führen Sie das folgende PowerShell-Cmdlet aus, um Ihrem Arbeitsbereich die Automation-Lösung hinzuzufügen.

```powershell-interactive
Set-AzOperationalInsightsIntelligencePack -ResourceGroupName <logAnalyticsResourceGroup> -WorkspaceName <LogAnalyticsWorkspaceName> -IntelligencePackName "AzureAutomation" -Enabled $true -DefaultProfile <IAzureContextContainer>
```

### <a name="step-3---install-the-log-analytics-agent-for-windows"></a>Schritt 3: Installieren des Log Analytics-Agents für Windows

Der Log Analytics-Agent für Windows verbindet Computer mit einem Azure Monitor Log Analytics-Arbeitsbereich. Wenn Sie den Agent auf Ihrem Computer installieren und mit Ihrem Arbeitsbereich verbinden, werden automatisch die erforderlichen Komponenten für Hybrid Runbook Worker heruntergeladen.

Befolgen Sie die Anweisungen unter [Verbinden von Windows-Computern mit Azure Monitor-Protokolle](../log-analytics/log-analytics-windows-agent.md), um den Agent auf Ihrem Computer zu installieren. Sie können diesen Vorgang für mehrere Computer wiederholen, um Ihrer Umgebung mehrere Worker hinzuzufügen.

Nachdem der Agent nach einigen Minuten eine Verbindung mit dem Log Analytics-Arbeitsbereich hergestellt hat, können Sie über die folgende Abfrage überprüfen, ob er Heartbeatdaten an den Arbeitsbereich sendet.

```kusto
Heartbeat 
| where Category == "Direct Agent" 
| where TimeGenerated > ago(30m)
```

In den Suchergebnissen sollten Heartbeat-Datensätze für den Computer angezeigt werden, die angeben, dass dieser verbunden ist und Berichte an den Dienst übermittelt. Standardmäßig leitet jeder Agent einen Heartbeat-Datensatz an seinen zugewiesenen Arbeitsbereich weiter. 

Führen Sie die folgenden Schritte aus, um Installation und Einrichtung des Agents abzuschließen.

1. Aktivieren Sie die Lösung, um den Agent-Computer zu integrieren. Weitere Informationen finden Sie unter [Integrieren von Computern in den Arbeitsbereich](https://docs.microsoft.com/azure/automation/automation-onboard-solutions-from-automation-account#onboard-machines-in-the-workspace).
2. Vergewissern Sie sich, dass der Agent die Azure Automation-Lösung ordnungsgemäß heruntergeladen hat. 
3. Navigieren Sie zum Überprüfen der Hybrid Runbook Worker-Version zu **C:\Programme\Microsoft Monitoring Agent\Agent\AzureAutomation**, und sehen Sie im Unterordner **version** nach.

### <a name="step-4---install-the-runbook-environment-and-connect-to-azure-automation"></a>Schritt 4: Installieren der Runbookumgebung und Herstellen einer Verbindung mit Azure Automation

Wenn Sie einen Agent für die Berichterstattung an einen Log Analytics-Arbeitsbereich konfigurieren, lädt die Automation-Lösung per Push das PowerShell-Modul `HybridRegistration` herunter, in dem das Cmdlet `Add-HybridRunbookWorker` enthalten ist. Verwenden Sie dieses Cmdlet zum Installieren der Runbookumgebung auf dem Computer und zu deren Registrierung bei Azure Automation.

Öffnen Sie eine PowerShell-Sitzung im Administratormodus, und führen Sie die folgenden Befehle zum Importieren des Moduls aus.

```powershell-interactive
cd "C:\Program Files\Microsoft Monitoring Agent\Agent\AzureAutomation\<version>\HybridRegistration"
Import-Module .\HybridRegistration.psd1
```

Führen Sie nun das Cmdlet `Add-HybridRunbookWorker` mit der folgenden Syntax aus.

```powershell-interactive
Add-HybridRunbookWorker –GroupName <String> -EndPoint <Url> -Token <String>
```

Sie finden die für dieses Cmdlet erforderlichen Informationen im Azure-Portal auf der Seite „Schlüssel verwalten“. Öffnen Sie diese Seite, indem Sie in Ihrem Automation-Konto auf der Seite „Einstellungen“ die Option **Schlüssel** auswählen.

![Seite „Schlüssel verwalten“](media/automation-hybrid-runbook-worker/elements-panel-keys.png)

* Verwenden Sie für den Parameter `GroupName` den Namen der Hybrid Runbook Worker-Gruppe. Wenn die Gruppe bereits im Automation-Konto vorhanden ist, wird ihr der aktuelle Computer hinzugefügt. Wenn diese Gruppe nicht vorhanden ist, wird sie hinzugefügt.
* Verwenden Sie für den Parameter `EndPoint` den Eintrag **URL** auf der Seite „Schlüssel verwalten“.
* Verwenden Sie für den Parameter `Token` den Eintrag **PRIMÄRER ZUGRIFFSSCHLÜSSEL** auf der Seite „Schlüssel verwalten“.
* Legen Sie bei Bedarf den Parameter `Verbose`fest, um Details zur Installation zu erhalten.

### <a name="step-5----install-powershell-modules"></a>Schritt 5: Installieren von PowerShell-Modulen

Runbooks können beliebige Aktivitäten und Cmdlets der Module verwenden, die Sie in Ihrer Azure Automation-Umgebung installiert haben. Da diese Module nicht automatisch auf den lokalen Computern bereitgestellt werden, müssen Sie sie manuell installieren. Die Ausnahme ist das Azure-Modul. Dieses Modul wird standardmäßig installiert und bietet Zugriff auf Cmdlets für alle Azure-Dienste und -Aktivitäten für Azure Automation.

Da der primäre Zweck des Features Hybrid Runbook Worker in der Verwaltung lokaler Ressourcen besteht, müssen Sie sehr wahrscheinlich die Module zur Unterstützung dieser Ressourcen installieren, insbesondere das Modul `PowerShellGet`. Informationen zum Installieren von Windows PowerShell-Modulen finden Sie unter [Windows PowerShell](https://docs.microsoft.com/powershell/scripting/developer/windows-powershell).

Installierte Module müssen sich an einem Speicherort befinden, auf den von der Umgebungsvariablen `PSModulePath` verwiesen wird, damit sie vom Hybrid Worker automatisch importiert werden. Weitere Informationen finden Sie unter [Installieren von Modulen in PSModulePath](https://docs.microsoft.com/powershell/scripting/developer/module/installing-a-powershell-module?view=powershell-7).

## <a name="next-steps"></a>Nächste Schritte

* Um zu erfahren, wie Sie Ihre Runbooks für die Automatisierung von Prozessen in Ihrem lokalen Rechenzentrum oder in einer anderen Cloudumgebung konfigurieren, lesen Sie sich [Ausführen von Runbooks auf einem Hybrid Runbook Worker](automation-hrw-run-runbooks.md) durch.
* Anweisungen zum Entfernen von Hybrid Runbook Workern finden Sie unter [Entfernen von Azure Automation-Hybrid Runbook Workern](automation-hybrid-runbook-worker.md#remove-a-hybrid-runbook-worker).
* Informationen zur Problembehandlung Ihrer Hybrid Runbook Worker finden Sie unter [Problembehandlung von Windows Hybrid Runbook Workern](troubleshoot/hybrid-runbook-worker.md#windows).
* Zusätzliche Schritte zum Beheben von Problemen mit der Updateverwaltung finden Sie unter [Updateverwaltung: Problembehandlung](troubleshoot/update-management.md).
