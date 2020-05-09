---
title: Beheben von Problemen mit freigegebenen Ressourcen in Azure Automation
description: Hier erfahren Sie, wie Sie Probleme mit freigegebenen Azure Automation-Ressourcen beheben.
services: automation
author: mgoedtel
ms.author: magoedte
ms.date: 03/12/2019
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: e83c7074d252083329537e205666374705a31873
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "81733571"
---
# <a name="troubleshoot-shared-resources-in-azure-automation"></a>Beheben von Problemen mit freigegebenen Ressourcen in Azure Automation

In diesem Artikel werden Lösungen zum Beheben von Problemen beschrieben, die bei Verwendung von [freigegebenen Ressourcen](../automation-intro.md#shared-resources) in Azure Automation auftreten können.

>[!NOTE]
>Dieser Artikel wurde aktualisiert und beinhaltet jetzt das neue Az-Modul von Azure PowerShell. Sie können das AzureRM-Modul weiterhin verwenden, das bis mindestens Dezember 2020 weiterhin Fehlerbehebungen erhält. Weitere Informationen zum neuen Az-Modul und zur Kompatibilität mit AzureRM finden Sie unter [Introducing the new Azure PowerShell Az module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0) (Einführung in das neue Az-Modul von Azure PowerShell). Installationsanweisungen für das Az-Modul auf Ihrem Hybrid Runbook Worker finden Sie unter [Installieren des Azure PowerShell-Moduls](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). In Ihrem Automation-Konto können Sie die Module mithilfe der Informationen unter [Aktualisieren von Azure PowerShell-Modulen in Azure Automation](../automation-update-azure-modules.md) auf die neueste Version aktualisieren.

## <a name="modules"></a>Module

### <a name="scenario-a-module-is-stuck-during-import"></a><a name="module-stuck-importing"></a>Szenario: Ein Modul bleibt beim Import hängen

#### <a name="issue"></a>Problem

Ein Modul bleibt beim Importieren oder Aktualisieren von Azure Automation-Modulen im Zustand „Wird importiert“ hängen.

#### <a name="cause"></a>Ursache

Da das Importieren von PowerShell-Modulen ein komplexer Prozess mit vielen Schritten ist, wird ein Modul möglicherweise nicht ordnungsgemäß importiert, und es bleibt in einem vorübergehenden Zustand hängen. Weitere Informationen zum Importvorgang finden Sie unter [Importieren eines PowerShell-Moduls](/powershell/scripting/developer/module/importing-a-powershell-module#the-importing-process).

#### <a name="resolution"></a>Lösung

Um dieses Problem zu beheben, müssen Sie das Modul, das im Zustand „Wird importiert“ hängen geblieben ist, mithilfe des Cmdlets [Remove-AzAutomationModule](https://docs.microsoft.com/powershell/module/Az.Automation/Remove-AzAutomationModule?view=azps-3.7.0) entfernen. Sie können dann das Importieren des Moduls erneut versuchen.

```azurepowershell-interactive
Remove-AzAutomationModule -Name ModuleName -ResourceGroupName ExampleResourceGroup -AutomationAccountName ExampleAutomationAccount -Force
```

### <a name="scenario-azurerm-modules-are-stuck-during-import-after-an-update-attempt"></a><a name="update-azure-modules-importing"></a>Szenario: AzureRM-Module bleiben nach einem Aktualisierungsversuch während des Imports hängen

#### <a name="issue"></a>Problem

Ein Banner mit der folgenden Meldung wird weiterhin in Ihrem Konto angezeigt, nachdem Sie versucht haben, Ihre AzureRM-Module zu aktualisieren:

```error
Azure modules are being updated
```

#### <a name="cause"></a>Ursache

Beim Aktualisieren der AzureRM-Module in einem Automation-Konto tritt in Ressourcengruppen mit einem mit 0 beginnenden numerischen Namen ein bekanntes Problem auf.

#### <a name="resolution"></a>Lösung

Um die AzureRM-Module in Ihrem Automation-Konto zu aktualisieren, muss sich das Konto in einer Ressourcengruppe mit einem alphanumerischen Namen befinden. Ressourcengruppen, deren numerischer Name mit 0 beginnt, können zurzeit keine AzureRM-Module aktualisieren.

### <a name="scenario-module-fails-to-import-or-cmdlets-cant-be-executed-after-importing"></a><a name="module-fails-to-import"></a>Szenario: Fehler beim Modulimport oder Ausführung von Cmdlets nach Import nicht möglich

#### <a name="issue"></a>Problem

Ein Modul kann nicht importiert werden, oder sein Import ist erfolgreich, aber es werden keine Cmdlets extrahiert.

#### <a name="cause"></a>Ursache

Es folgen häufige Ursachen, warum ein Modul nicht erfolgreich in Azure Automation importiert wird:

* Die Struktur stimmt nicht mit der Struktur überein, die für Automation erforderlich ist.
* Das Modul hängt von einem anderen Modul ab, das für Ihr Automation-Konto nicht bereitgestellt wurde.
* Für das Modul fehlen die Abhängigkeiten im Ordner.
* Das Cmdlet [New-AzAutomationModule](https://docs.microsoft.com/powershell/module/Az.Automation/New-AzAutomationModule?view=azps-3.7.0) wird zum Hochladen des Moduls verwendet, und Sie haben nicht den vollständigen Speicherpfad angegeben oder das Modul nicht mit einer öffentlich zugänglichen URL geladen.

#### <a name="resolution"></a>Lösung

Verwenden Sie eine der folgenden Lösungen, um das Problem zu beheben.

* Stellen Sie sicher, dass das Modul das folgende Format aufweist: Modulname.zip -> Modulname oder Versionsnummer -> (Modulname.psm1, Modulname.psd1).
* Öffnen Sie die **PSD1**-Datei, und prüfen Sie, ob für das Modul Abhängigkeiten bestehen. Wenn ja, laden Sie diese Module in das Automation-Konto hoch.
* Stellen Sie sicher, dass alle referenzierten **DLL**-Dateien im Modulordner vorhanden sind.

### <a name="scenario-update-azuremoduleps1-suspends-when-updating-modules"></a><a name="all-modules-suspended"></a>Szenario: Update-AzureModule.ps1 wird beim Aktualisieren der Module angehalten.

#### <a name="issue"></a>Problem

Bei Verwendung des [Update-AzureModule.ps1](https://github.com/azureautomation/runbooks/blob/master/Utility/ARM/Update-AzureModule.ps1)-Runbooks zum Aktualisieren Ihrer Azure-Module wird der Updateprozess angehalten.

#### <a name="cause"></a>Ursache

Die Standardeinstellung zur Bestimmung, wie viele Module gleichzeitig aktualisiert werden, liegt bei Verwendung von **Update-AzureModule.ps1** bei 10. Der Updateprozess ist fehleranfällig, wenn zu viele Module gleichzeitig aktualisiert werden.

#### <a name="resolution"></a>Lösung

Es ist keine allgemeine Anforderung, dass sich alle AzureRM- oder Az-Module im selben Automation-Konto befinden müssen. Es wird empfohlen, nur die spezifischen Module zu importieren, die Sie benötigen.

> [!NOTE]
> Vermeiden Sie den Import des gesamten `Az.Automation`- oder `AzureRM.Automation`-Moduls – dadurch werden alle enthaltenen Module importiert.

Wenn der Updateprozess angehalten wird, fügen Sie den `SimultaneousModuleImportJobCount`-Parameter zum **Update-AzureModules.ps1**-Skript hinzu und geben einen niedrigeren Wert als den Standardwert 10 an. Wenn Sie diese Logik implementieren, empfiehlt es sich, mit dem Wert 3 oder 5 zu beginnen. `SimultaneousModuleImportJobCount` ist ein Parameter des **Update-AutomationAzureModulesForAccount**-Systemrunbooks, das zum Aktualisieren der Azure-Module verwendet wird. Wenn Sie diese Anpassung vornehmen, dauert der Updateprozess länger, aber die Wahrscheinlichkeit, dass er vollständig abgeschlossen wird, ist höher. Das folgende Beispiel zeigt den Parameter und seine Position im Runbook:

 ```powershell
         $Body = @"
            {
               "properties":{
               "runbook":{
                   "name":"Update-AutomationAzureModulesForAccount"
               },
               "parameters":{
                    ...
                    "SimultaneousModuleImportJobCount":"3",
                    ... 
               }
              }
           }
"@
```

## <a name="run-as-accounts"></a>Ausführende Konten

### <a name="scenario-youre-unable-to-create-or-update-a-run-as-account"></a><a name="unable-create-update"></a>Szenario: Ausführendes Konto kann nicht erstellt oder aktualisiert werden

#### <a name="issue"></a>Problem

Beim Versuch, ein ausführendes Konto zu erstellen oder zu aktualisieren, erhalten Sie eine ähnliche Fehlermeldung wie die folgende:

```error
You do not have permissions to create…
```

#### <a name="cause"></a>Ursache

Sie haben keine Berechtigungen zum Erstellen oder Aktualisieren des ausführenden Kontos, oder die Ressource ist auf Ressourcengruppenebene gesperrt.

#### <a name="resolution"></a>Lösung

Um ein ausführendes Konto zu erstellen oder zu aktualisieren, müssen Sie über geeignete [Berechtigungen](../manage-runas-account.md#permissions) für die verschiedenen Ressourcen verfügen, die vom ausführenden Konto verwendet werden. 

Wenn das Problem durch eine Sperre verursacht wird, überprüfen Sie, ob die Sperre entfernt werden kann. Navigieren Sie im Azure-Portal zu der gesperrten Ressource, klicken Sie mit der rechten Maustaste auf das Schlosssymbol, und klicken Sie auf **Löschen**.

### <a name="scenario-you-receive-the-error-unable-to-find-an-entry-point-named-getperadapterinfo-in-dll-iplpapidll-when-executing-a-runbook"></a><a name="iphelper"></a>Szenario: Sie erhalten beim Ausführen eines Runbooks folgenden Fehler: Der Einstiegspunkt „GetPerAdapterInfo“ wurde nicht in der DLL „iplpapi.dll“ gefunden

#### <a name="issue"></a>Problem

Sie erhalten beim Ausführen eines Runbooks die folgende Ausnahme:

```error
Unable to find an entry point named 'GetPerAdapterInfo' in DLL 'iplpapi.dll'
```

#### <a name="cause"></a>Ursache

Dieser Fehler wird wahrscheinlich durch ein falsch konfiguriertes [ausführendes Konto](../manage-runas-account.md) verursacht.

#### <a name="resolution"></a>Lösung

Stellen Sie sicher, dass Ihr ausführendes Konto ordnungsgemäß konfiguriert ist. Vergewissern Sie sich dann, dass der Code in Ihrem Runbook für die Authentifizierung bei Azure korrekt ist. Das folgende Beispiel zeigt einen Codeausschnitt in einem Runbook zum Authentifizieren bei Azure mithilfe eines ausführenden Kontos.

```powershell
$connection = Get-AutomationConnection -Name AzureRunAsConnection
Connect-AzAccount -ServicePrincipal -Tenant $connection.TenantID `
-ApplicationID $connection.ApplicationID -CertificateThumbprint $connection.CertificateThumbprint
```

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt wird oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Erhalten Sie Antworten von Azure-Experten über [Azure-Foren](https://azure.microsoft.com/support/forums/).
* Nutzen Sie [@AzureSupport](https://twitter.com/azuresupport) – das offizielle Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit. Hierüber hat die Azure-Community Zugriff auf die richtigen Ressourcen: Antworten, Support und Experten.
* Erstellen Sie einen Azure-Supportfall. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und wählen Sie **Support erhalten**aus.
