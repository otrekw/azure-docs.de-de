---
title: Behandeln von Problemen bei freigegebenen Azure Automation-Ressourcen
description: In diesem Artikel erfahren Sie, wie Sie Probleme mit freigegebenen Azure Automation-Ressourcen beheben.
services: automation
author: mgoedtel
ms.author: magoedte
ms.date: 03/12/2019
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 5b87a98ed38e3af315789adffc11824f2522b802
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2020
ms.locfileid: "83680883"
---
# <a name="troubleshoot-shared-resource-issues"></a>Behandeln von Problemen bei freigegebenen Ressourcen

In diesem Artikel werden Probleme beschrieben, die bei Verwendung von [freigegebenen Ressourcen](../automation-intro.md#shared-resources) in Azure Automation auftreten können.

## <a name="modules"></a>Module

### <a name="scenario-a-module-is-stuck-during-import"></a><a name="module-stuck-importing"></a>Szenario: Ein Modul bleibt beim Import hängen

#### <a name="issue"></a>Problem

Ein Modul bleibt beim Importieren oder Aktualisieren von Azure Automation-Modulen im Zustand *Wird importiert* hängen.

#### <a name="cause"></a>Ursache

Da das Importieren von PowerShell-Modulen ein komplexer Prozess mit vielen Schritten ist, wird ein Modul möglicherweise nicht ordnungsgemäß importiert, und es bleibt in einem vorübergehenden Zustand hängen. Weitere Informationen zum Importvorgang finden Sie unter [Importieren eines PowerShell-Moduls](/powershell/scripting/developer/module/importing-a-powershell-module#the-importing-process).

#### <a name="resolution"></a>Lösung

Um dieses Problem zu beheben, müssen Sie das Modul, das hängen geblieben ist, mithilfe des Cmdlets [Remove-AzAutomationModule](https://docs.microsoft.com/powershell/module/Az.Automation/Remove-AzAutomationModule?view=azps-3.7.0) entfernen. Sie können dann das Importieren des Moduls erneut versuchen.

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

Es liegt ein bekanntes Problem mit dem Update der AzureRM-Module in einem Automation-Konto vor. Dieses Problem tritt genau gesagt dann auf, wenn sich die Module in einer Ressourcengruppe mit einem numerischen Namen befinden, der mit 0 (null) beginnt.

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

Verwenden Sie eine der folgenden Lösungen, um das Problem zu beheben:

* Stellen Sie sicher, dass das Modul das folgende Format aufweist: Modulname.zip -> Modulname oder Versionsnummer -> (Modulname.psm1, Modulname.psd1).
* Öffnen Sie die **PSD1**-Datei, und prüfen Sie, ob für das Modul Abhängigkeiten bestehen. Wenn ja, laden Sie diese Module in das Automation-Konto hoch.
* Stellen Sie sicher, dass alle referenzierten **DLL**-Dateien im Modulordner vorhanden sind.

### <a name="scenario-update-azuremoduleps1-suspends-when-updating-modules"></a><a name="all-modules-suspended"></a>Szenario: Update-AzureModule.ps1 wird beim Aktualisieren der Module angehalten.

#### <a name="issue"></a>Problem

Bei Verwendung des Runbooks [Update-AzureModule.ps1](https://github.com/azureautomation/runbooks/blob/master/Utility/ARM/Update-AzureModule.ps1) zum Aktualisieren Ihrer Azure-Module wird der Updateprozess angehalten.

#### <a name="cause"></a>Ursache

Bei diesem Runbook lautet die Standardeinstellung „10“. Anhand dieses Werts wird festgelegt, wie viele Module gleichzeitig aktualisiert werden. Der Updateprozess ist fehleranfällig, wenn zu viele Module gleichzeitig aktualisiert werden.

#### <a name="resolution"></a>Lösung

Es ist keine allgemeine Anforderung, dass sich alle AzureRM- oder Az-Module im selben Automation-Konto befinden müssen. Sie sollten nur die spezifischen Module importieren, die Sie benötigen.

> [!NOTE]
> Vermeiden Sie den Import des gesamten `Az.Automation`- oder `AzureRM.Automation`-Moduls – dadurch werden alle enthaltenen Module importiert.

Wenn der Updateprozess angehalten wird, fügen Sie den dem Skript **Update-AzureModules.ps1** den Parameter `SimultaneousModuleImportJobCount` hinzu und geben einen niedrigeren Wert als den Standardwert 10 an. Beginnen Sie beim Implementieren dieser Logik mit einem Wert von 3 oder 5. `SimultaneousModuleImportJobCount` ist ein Parameter des **Update-AutomationAzureModulesForAccount**-Systemrunbooks, das zum Aktualisieren der Azure-Module verwendet wird. Wenn Sie diese Anpassung vornehmen, dauert der Updateprozess länger, aber die Wahrscheinlichkeit, dass er vollständig abgeschlossen wird, ist höher. Das folgende Beispiel zeigt den Parameter und seine Position im Runbook:

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

Wenn das Problem durch eine Sperre verursacht wird, überprüfen Sie, ob die Sperre entfernt werden kann. Navigieren Sie im Azure-Portal zu der gesperrten Ressource, klicken Sie mit der rechten Maustaste auf das Schlosssymbol, und wählen Sie **Löschen** aus.

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

Wenn Sie Ihr Problem nicht mit diesem Artikel lösen konnten, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Erhalten Sie Antworten von Azure-Experten über [Azure-Foren](https://azure.microsoft.com/support/forums/).
* Wenden Sie sich an [@AzureSupport](https://twitter.com/azuresupport) – Dies ist das offizielle Microsoft Azure-Konto, über das die Azure-Community Zugriff auf die richtigen Ressourcen erhält: Antworten, Support und Experten.
* Erstellen Sie einen Azure-Supportfall. Wechseln Sie zur [Azure-Supportwebsite](https://azure.microsoft.com/support/options/), und wählen Sie **Support erhalten** aus.

