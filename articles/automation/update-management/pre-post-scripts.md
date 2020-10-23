---
title: Verwalten von Pre- und Post-Skripts in der Bereitstellung der Updateverwaltung in Azure
description: In diesem Artikel erfahren Sie, wie Sie Pre- und Post-Skripts für Updatebereitstellungen konfigurieren und verwalten.
services: automation
ms.subservice: update-management
ms.date: 05/17/2019
ms.topic: conceptual
ms.openlocfilehash: bb2a272829374cfeba5c334ff87268c4928885f5
ms.sourcegitcommit: 8d8deb9a406165de5050522681b782fb2917762d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/20/2020
ms.locfileid: "92221614"
---
# <a name="manage-pre-scripts-and-post-scripts"></a>Verwalten von Pre- und Post-Skripts

Pre- und Post-Skripts sind Runbooks, die vor (Pre-Aufgaben) und nach (Post-Aufgaben) einer Updatebereitstellung in Ihrem Azure Automation-Konto ausgeführt werden. Pre- und Post-Skripts werden nicht lokal, sondern im Azure-Kontext ausgeführt. Pre-Skripts werden zu Beginn der Updatebereitstellung ausgeführt. Post-Skripts werden am Ende der Bereitstellung sowie nach allen konfigurierten Neustarts ausgeführt.

## <a name="pre-script-and-post-script-requirements"></a>Anforderungen an Pre- und Post-Skripts

Damit ein Runbook als Pre- oder Post-Skript verwendet werden kann, müssen Sie es in Ihr Automation-Konto importieren und [veröffentlichen](../manage-runbooks.md#publish-a-runbook).

## <a name="pre-script-and-post-script-parameters"></a>Parameter für Pre- und Post-Skripts

Beim Konfigurieren von Pre- und Post-Skripts können Sie genau wie beim Planen eines Runbooks Parameter übergeben. Parameter werden beim Erstellen der Updatebereitstellung definiert. Pre- und Post-Skripts unterstützen folgende Typen:

* [char]
* [byte]
* [int]
* [long]
* [decimal]
* [single]
* [double]
* [DateTime]
* [string]

Runbookparameter für Pre- und Post-Skripts unterstützen keine Boole-, Objekt- oder Arraytypen. Bei solchen Werten treten in Runbooks Fehler auf. 

Wenn Sie einen anderen Objekttyp benötigen, können Sie eine Typumwandlung in einen anderen Typ mit Ihrer eigenen Logik im Runbook durchführen.

Neben den Standardrunbookparametern steht auch der `SoftwareUpdateConfigurationRunContext`-Parameter (JSON-Zeichenfolgentyp) zur Verfügung. Wenn Sie diesen Parameter in Ihrem Pre- oder Post-Skript-Runbook definieren, wird er automatisch von der Updatebereitstellung übergeben. Der Parameter enthält Informationen zur Updatebereitstellung, bei denen es sich um eine Teilmenge der von der [SoftwareUpdateconfigurations-API](/rest/api/automation/softwareupdateconfigurations/getbyname#updateconfiguration) zurückgegebenen Informationen handelt. Die folgenden Abschnitte definieren die zugeordneten Eigenschaften.

### <a name="softwareupdateconfigurationruncontext-properties"></a>SoftwareUpdateConfigurationRunContext-Eigenschaften

|Eigenschaft  |BESCHREIBUNG  |
|---------|---------|
|SoftwareUpdateConfigurationName     | Der Name der Softwareupdatekonfiguration.        |
|SoftwareUpdateConfigurationRunId     | Die eindeutige ID für die Ausführung        |
|SoftwareUpdateConfigurationSettings     | Eine Sammlung von Eigenschaften im Zusammenhang mit der Softwareupdatekonfiguration.         |
|SoftwareUpdateConfigurationSettings.operatingSystem     | Die Zielbetriebssysteme für die Updatebereitstellung.         |
|SoftwareUpdateConfigurationSettings.duration     | Die maximale Dauer der Ausführung der Updatebereitstellung im Format `PT[n]H[n]M[n]S` gemäß ISO 8601 (das so genannte „Wartungsfenster“).          |
|SoftwareUpdateConfigurationSettings.Windows     | Eine Sammlung von Eigenschaften im Zusammenhang mit Windows-Computern.         |
|SoftwareUpdateConfigurationSettings.Windows.excludedKbNumbers     | Eine Liste mit KBs, die von der Bereitstellung ausgeschlossen werden.        |
|SoftwareUpdateConfigurationSettings.Windows.includedUpdateClassifications     | Für die Updatebereitstellung ausgewählte Updateklassifizierungen.        |
|SoftwareUpdateConfigurationSettings.Windows.rebootSetting     | Neustarteinstellungen für die Updatebereitstellung.        |
|azureVirtualMachines     | Eine Liste mit Ressourcen-IDs für die virtuellen Azure-Computer in der Updatebereitstellung.        |
|nonAzureComputerNames|Eine Liste mit den FQDNs der Azure-fremden Computer in der Updatebereitstellung.|

Das folgende Beispiel ist eine JSON-Zeichenfolge, die an den **SoftwareUpdateConfigurationRunContext**-Parameter übergeben wird:

```json
"SoftwareUpdateConfigurationRunContext":{
      "SoftwareUpdateConfigurationName":"sampleConfiguration",
      "SoftwareUpdateConfigurationRunId":"00000000-0000-0000-0000-000000000000",
      "SoftwareUpdateConfigurationSettings":{
         "operatingSystem":"Windows",
         "duration":"PT2H0M",
         "windows":{
            "excludedKbNumbers":[
               "168934",
               "168973"
            ],
            "includedUpdateClassifications":"Critical",
            "rebootSetting":"IfRequired"
         },
         "azureVirtualMachines":[
            "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresources/providers/Microsoft.Compute/virtualMachines/vm-01",
            "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresources/providers/Microsoft.Compute/virtualMachines/vm-02",
            "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresources/providers/Microsoft.Compute/virtualMachines/vm-03"
         ],
         "nonAzureComputerNames":[
            "box1.contoso.com",
            "box2.contoso.com"
         ]
      }
   }
```

Ein vollständiges Beispiel mit allen Eigenschaften finden Sie unter: [Abrufen der Softwareupdatekonfiguration anhand des Namens](/rest/api/automation/softwareupdateconfigurations/getbyname#examples).

> [!NOTE]
> Das `SoftwareUpdateConfigurationRunContext`-Objekt kann doppelte Einträge für Computer enthalten. Dies kann dazu führen, dass Pre- und Post-Skripts mehrmals auf dem gleichen Computer ausgeführt werden. Verwenden Sie zur Umgehung dieses Verhaltens `Sort-Object -Unique`, um nur eindeutige VM-Namen auszuwählen.

> [!NOTE]
> Zurzeit werden als Skripts vor oder nach der Bereitstellung nur PowerShell-Runbooks unterstützt. Andere Runbooktypen wie Python, grafisch, PowerShell-Workflow und grafischer PowerShell-Workflow werden derzeit nicht als Skripts vor oder nach der Bereitstellung unterstützt.

## <a name="use-a-pre-script-or-post-script-in-a-deployment"></a>Verwenden eines Pre- oder Post-Skripts in einer Bereitstellung

Wenn Sie ein Pre- oder Post-Skript in einer Updatebereitstellung verwenden möchten, müssen Sie zunächst eine Updatebereitstellung erstellen. Wählen Sie **Vor und nach dem Vorgang auszuführende Skripts** aus. Daraufhin wird die Seite **Vor und nach dem Vorgang auszuführende Skripts auswählen** angezeigt.

![Auswählen der Skripts](./media/pre-post-scripts/select-scripts.png)

Wählen Sie das Skript aus, das Sie verwenden möchten. In diesem Beispiel verwenden wir das Runbook **UpdateManagement-TurnOnVms**. Wenn Sie das Runbook auswählen, wird die Seite **Skript konfigurieren** geöffnet. Wählen Sie **Skript vor Vorgang** und anschließend **OK** aus.

Wiederholen Sie diese Schritte für das Skript **UpdateManagement-TurnOffVms**. Wählen Sie diesmal für **Skripttyp** jedoch **Skript nach Vorgang** aus.

Im Abschnitt **Ausgewählte Elemente** werden nun die beiden ausgewählten Skripts angezeigt. Eines ist ein Pre-Skript, das andere ein Post-Skript:

![Ausgewählte Elemente](./media/pre-post-scripts/selected-items.png)

Schließen Sie die Konfiguration der Updatebereitstellung ab.

Nach Abschluss der Updatebereitstellung können Sie unter **Updatebereitstellungen** die Ergebnisse anzeigen. Wie Sie sehen, wird der Status des Pre- und Post-Skripts angezeigt:

![Updateergebnisse](./media/pre-post-scripts/update-results.png)

Wenn Sie die Ausführung der Updatebereitstellung auswählen, werden weitere Details der Pre- und Post-Skripts angezeigt. Dort finden Sie auch einen Link zur Skriptquelle zum Zeitpunkt der Ausführung.

![Ergebnisse der Bereitstellungsausführung](./media/pre-post-scripts/deployment-run.png)

## <a name="stop-a-deployment"></a>Beenden einer Bereitstellung

Wenn Sie eine Bereitstellung basierend auf einem vorbereitenden Skript beenden möchten, müssen Sie eine Ausnahme [auslösen](../automation-runbook-execution.md#throw). Andernfalls werden die Bereitstellung und das Post-Skript trotzdem ausgeführt. Der folgende Codeausschnitt veranschaulicht das Auslösen einer Ausnahme:

```powershell
#In this case, we want to terminate the patch job if any run fails.
#This logic might not hold for all cases - you might want to allow success as long as at least 1 run succeeds
foreach($summary in $finalStatus)
{
    if ($summary.Type -eq "Error")
    {
        #We must throw in order to fail the patch deployment.
        throw $summary.Summary
    }
}
```

## <a name="interact-with-machines"></a>Interagieren mit Computern

Pre- und Post-Skripts werden nicht direkt auf den Computern in Ihrer Bereitstellung, sondern als Runbooks in Ihrem Automation-Konto ausgeführt. Pre- und Post-Aufgaben werden auch im Azure-Kontext ausgeführt und haben keinen Zugriff auf Azure-fremde Computer. In den folgenden Abschnitten erfahren Sie, wie Sie direkt mit den Computern interagieren. Dabei spielt es keine Rolle, ob es sich um virtuelle Azure-Computer oder um Azure-fremde Computer handelt.

### <a name="interact-with-azure-machines"></a>Interaktion mit Azure-Computern

Pre- und Post-Aufgaben werden nicht nativ auf den virtuellen Azure-Computern in Ihrer Bereitstellung, sondern als Runbooks ausgeführt. Für die Interaktion mit den Azure-VMs benötigen Sie die folgenden Elemente:

* Ein ausführendes Konto
* Ein Runbook, das Sie ausführen möchten

Verwenden Sie das Cmdlet [Invoke-AzVMRunCommand](/powershell/module/az.compute/invoke-azvmruncommand?view=azps-3.7.0) für die Interaktion mit Ihren Azure-VMs. Ein Beispiel dafür finden Sie im Runbookbeispiel [Updateverwaltung: Ausführen eines Skripts mit dem Befehl „run“](https://gallery.technet.microsoft.com/Update-Management-Run-40f470dc).

### <a name="interact-with-non-azure-machines"></a>Interagieren mit Azure-fremden Computern

Pre- und Post-Aufgaben werden im Azure-Kontext ausgeführt und haben keinen Zugriff auf Azure-fremde Computer. Für die Interaktion mit den Azure-fremden Computern benötigen Sie Folgendes:

* Ein ausführendes Konto
* Einen auf dem Computer installierten Hybrid Runbook Worker
* Ein Runbook, das Sie lokal ausführen möchten
* Ein übergeordnetes Runbook

Für die Interaktion mit Azure-fremden Computern wird ein übergeordnetes Runbook im Azure-Kontext ausgeführt. Dieses Runbook ruft mit dem Cmdlet [Start-AzAutomationRunbook](/powershell/module/Az.Automation/Start-AzAutomationRunbook?view=azps-3.7.0) ein untergeordnetes Runbook auf. Sie müssen den `RunOn`-Parameter und den Namen des Hybrid Runbook Workers, auf dem das Skript ausgeführt werden soll, angeben. Weitere Informationen finden Sie im Runbookbeispiel [Update Management – run script locally](https://gallery.technet.microsoft.com/Update-Management-Run-6949cc44) (Updateverwaltung – Skript lokal ausführen).

## <a name="abort-patch-deployment"></a>Abbrechen der Bereitstellung von Patches

Falls Ihr Pre-Skript einen Fehler zurückgibt, empfiehlt es sich ggf., die Bereitstellung abzubrechen. Dazu müssen Sie in Ihrem Skript für jegliche Logik, die einen Fehler zur Folge hätte, einen Fehler [auslösen](/powershell/module/microsoft.powershell.core/about/about_throw).

```powershell
if (<My custom error logic>)
{
    #Throw an error to fail the patch deployment.
    throw "There was an error, abort deployment"
}
```

## <a name="samples"></a>Beispiele

Beispiele für Pre- und Post-Skripts können aus dem [Script Center-Katalog](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&f%5B0%5D.Value=WindowsAzure&f%5B0%5D.Text=Windows%20Azure&f%5B1%5D.Type=SubCategory&f%5B1%5D.Value=WindowsAzure_automation&f%5B1%5D.Text=Automation&f%5B2%5D.Type=SearchText&f%5B2%5D.Value=update%20management&f%5B3%5D.Type=Tag&f%5B3%5D.Value=Patching&f%5B3%5D.Text=Patching&f%5B4%5D.Type=ProgrammingLanguage&f%5B4%5D.Value=PowerShell&f%5B4%5D.Text=PowerShell) und dem [PowerShell-Katalog](https://www.powershellgallery.com/packages?q=Tags%3A%22UpdateManagement%22+Tags%3A%22Automation%22) heruntergeladen oder über das Azure-Portal importiert werden. Wählen Sie hierzu in Ihrem Automation-Konto unter **Prozessautomatisierung** die Option **Runbookkatalog** aus. Wählen Sie **Updateverwaltung** als Filter aus.

![Katalogliste](./media/pre-post-scripts/runbook-gallery.png)

Alternativ können Sie anhand der Namen in der folgenden Liste nach den Skripts suchen:

* Update Management – Turn On VMs (Updateverwaltung – VMs aktivieren)
* Update Management – Turn Off VMs (Updateverwaltung – VMs deaktivieren)
* Update Management – Run Script Locally (Updateverwaltung – Skript lokal ausführen)
* Update Management – Template for Pre/Post Scripts (Updateverwaltung – Vorlage für Pre-/Post-Skripts)
* Update Management – Run Script with Run Command (Updateverwaltung – Skript mit Ausführungsbefehl ausführen)

> [!IMPORTANT]
> Nachdem Sie die Runbooks importiert haben, müssen Sie sie veröffentlichen, um sie verwenden zu können. Suchen Sie hierzu in Ihrem Automation-Konto nach dem gewünschten Runbook, und wählen Sie **Bearbeiten** > **Veröffentlichen** aus.

Die Beispiele basieren alle auf der einfachen Vorlage, die im folgenden Beispiel definiert ist. Mit dieser Vorlage können Sie Ihr eigenes Runbook für die Verwendung mit Pre- und Post-Skripts erstellen. Die erforderliche Logik für die Authentifizierung bei Azure und die Behandlung des Parameters `SoftwareUpdateConfigurationRunContext` ist in der Vorlage enthalten.

```powershell
<#
.SYNOPSIS
 Barebones script for Update Management Pre/Post

.DESCRIPTION
  This script is intended to be run as a part of Update Management pre/post-scripts.
  It requires a RunAs account.

.PARAMETER SoftwareUpdateConfigurationRunContext
  This is a system variable which is automatically passed in by Update Management during a deployment.
#>

param(
    [string]$SoftwareUpdateConfigurationRunContext
)
#region BoilerplateAuthentication
#This requires a RunAs account
$ServicePrincipalConnection = Get-AutomationConnection -Name 'AzureRunAsConnection'

Add-AzAccount `
    -ServicePrincipal `
    -TenantId $ServicePrincipalConnection.TenantId `
    -ApplicationId $ServicePrincipalConnection.ApplicationId `
    -CertificateThumbprint $ServicePrincipalConnection.CertificateThumbprint

$AzureContext = Select-AzSubscription -SubscriptionId $ServicePrincipalConnection.SubscriptionID
#endregion BoilerplateAuthentication

#If you wish to use the run context, it must be converted from JSON
$context = ConvertFrom-Json  $SoftwareUpdateConfigurationRunContext
#Access the properties of the SoftwareUpdateConfigurationRunContext
$vmIds = $context.SoftwareUpdateConfigurationSettings.AzureVirtualMachines | Sort-Object -Unique
$runId = $context.SoftwareUpdateConfigurationRunId

Write-Output $context

#Example: How to create and write to a variable using the pre-script:
<#
#Create variable named after this run so it can be retrieved
New-AzAutomationVariable -ResourceGroupName $ResourceGroup –AutomationAccountName $AutomationAccount –Name $runId -Value "" –Encrypted $false
#Set value of variable
Set-AutomationVariable –Name $runId -Value $vmIds
#>

#Example: How to retrieve information from a variable set during the pre-script
<#
$variable = Get-AutomationVariable -Name $runId
#>
```

> [!NOTE]
> Für nicht grafische PowerShell-Runbooks sind `Add-AzAccount` und `Add-AzureRMAccount` Aliase für [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount?view=azps-3.5.0). Sie können diese Cmdlets verwenden, oder Sie können Ihre Module in Ihrem Automation-Konto auf die aktuellen Versionen [aktualisieren](../automation-update-azure-modules.md). Möglicherweise müssen Sie Ihre Module auch dann aktualisieren, wenn Sie gerade ein neues Automation-Konto erstellt haben.

## <a name="next-steps"></a>Nächste Schritte

* Einzelheiten zur Updateverwaltung finden Sie unter [Verwalten von Updates und Patches für Ihre VMs](manage-updates-for-vm.md).
