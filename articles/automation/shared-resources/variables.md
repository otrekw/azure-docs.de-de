---
title: Variable Objekte in Azure Automation
description: Variablenobjekte sind Werte, die allen Runbooks und DSC-Konfigurationen in Azure Automation zur Verfügung stehen.  Dieser Artikel stellt eine ausführliche Beschreibung von Variablen bereit und zeigt, wie diese in Textrunbooks und grafischen Runbooks eingesetzt werden.
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: mgoedtel
ms.author: magoedte
ms.date: 05/14/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: d4a4a92feb3e1b400c0f40076148f7898c4bdef1
ms.sourcegitcommit: 07d62796de0d1f9c0fa14bfcc425f852fdb08fb1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "80365835"
---
# <a name="variable-assets-in-azure-automation"></a>Variable Objekte in Azure Automation

Variablenobjekte sind Werte, die allen Runbooks und DSC-Konfigurationen in Ihrem Automation-Konto zur Verfügung stehen. Sie können sie über das Azure-Portal, mit PowerShell, in einem Runbook oder in einer DSC-Konfiguration verwalten.

Automatisierungsvariablen sind in folgenden Szenarien hilfreich:

- Gemeinsame Nutzung eines Werts durch mehrere Runbooks oder DSC-Konfigurationen

- Gemeinsame Nutzung eines Werts durch mehrere Aufträge desselben Runbooks oder derselben DSC-Konfiguration

- Verwalten eines von Runbooks oder DSC-Konfigurationen verwendeten Werts über das Portal oder über die PowerShell-Befehlszeile Ein Beispiel hierfür ist ein Satz von gemeinsamen Konfigurationselementen, z. B. eine bestimmte Liste von VM-Namen, eine bestimmte Ressourcengruppe, ein AD-Domänenname u. v. m.  

Azure Automation speichert Variablen persistent und macht sie auch dann verfügbar, wenn die Ausführung eines Runbooks oder einer DSC-Konfiguration zu einem Fehler führt. Durch dieses Verhalten kann ein Runbook oder eine DSC-Konfiguration einen Wert festlegen, der dann bei der nächsten Ausführung von einem anderen Runbook bzw. vom selben Runbook oder derselben DSC-Konfiguration genutzt werden kann.

Azure Automation speichert jede verschlüsselte Variable sicher. Beim Erstellen einer Variable können Sie die Verschlüsselung und den Speicher durch Azure Automation als sichere Ressource angeben. Andere sichere Ressourcen sind Anmeldeinformationen, Zertifikate und Verbindungen. Azure Automation verschlüsselt diese Ressourcen und speichert sie mithilfe eines eindeutigen Schlüssels, der für jedes Automation-Konto generiert wird. Der Schlüssel wird in einem systemseitig verwalteten Schlüsseltresor gespeichert. Vor dem Speichern einer sicheren Ressource lädt Azure Automation den Schlüssel aus dem Schlüsseltresor und verwendet ihn dann zum Verschlüsseln der Ressource. 

>[!NOTE]
>Dieser Artikel wurde aktualisiert und beinhaltet jetzt das neue Az-Modul von Azure PowerShell. Sie können das AzureRM-Modul weiterhin verwenden, das bis mindestens Dezember 2020 weiterhin Fehlerbehebungen erhält. Weitere Informationen zum neuen Az-Modul und zur Kompatibilität mit AzureRM finden Sie unter [Introducing the new Azure PowerShell Az module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0) (Einführung in das neue Az-Modul von Azure PowerShell). Installationsanweisungen für das Az-Modul auf Ihrem Hybrid Runbook Worker finden Sie unter [Installieren des Azure PowerShell-Moduls](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). In Ihrem Automation-Konto können Sie die Module mithilfe der Informationen unter [Aktualisieren von Azure PowerShell-Modulen in Azure Automation](../automation-update-azure-modules.md) auf die neueste Version aktualisieren.

## <a name="variable-types"></a>Variablentypen

Beim Erstellen einer Variable über das Azure-Portal müssen Sie einen Datentyp aus der Dropdownliste angeben, damit das entsprechende Steuerelement zum Eingeben des Variablenwerts im Portal angezeigt werden kann. Im Folgenden finden Sie die in Azure Automation verfügbaren Variablentypen:

* String
* Integer
* Datetime
* Boolean
* Null

Die Variable ist nicht auf den festgelegten Datentyp beschränkt. Sie müssen die Variable in Windows PowerShell festlegen, wenn Sie einen anderen Wertetyp angeben möchten. Wenn Sie `Not defined` angeben, wird der Wert der Variable auf NULL festgelegt, und Sie müssen den Wert mithilfe des Cmdlets [Set-AzAutomationVariable](https://docs.microsoft.com/powershell/module/az.automation/set-azautomationvariable?view=azps-3.5.0) oder der Aktivität `Set-AutomationVariable` festlegen.

Sie können den Wert für einen komplexen Variablentyp nicht im Azure-Portal erstellen oder ändern. Sie können jedoch einen Wert eines beliebigen Typs mithilfe von Windows PowerShell bereitstellen. Komplexe Typen werden als [PSCustomObject](/dotnet/api/system.management.automation.pscustomobject) abgerufen.

Sie können mehrere Werte in einer einzigen Variable speichern, indem Sie ein Array oder eine Hashtabelle erstellen und in der Variable speichern.

## <a name="powershell-cmdlets-that-create-and-manage-variable-assets"></a>PowerShell-Cmdlets zum Erstellen und Verwalten von Variablenressourcen

Die Cmdlets in der folgenden Tabelle werden für das Az-Modul zum Erstellen und Verwalten von Automation-Variablenressourcen mit Windows PowerShell verwendet. Sie gehören zum Funktionsumfang des [Az.Automation-Moduls](/powershell/azure/overview), das zur Verwendung in Automation-Runbooks und DSC-Konfigurationen verfügbar ist.

| Cmdlet | BESCHREIBUNG |
|:---|:---|
|[Get-AzAutomationVariable](https://docs.microsoft.com/powershell/module/az.automation/get-azautomationvariable?view=azps-3.5.0) | Ruft den Wert einer vorhandenen Variable ab. Sie können dieses Cmdlet nicht verwenden, um den Wert einer verschlüsselten Variable abzurufen. Dies ist nur mit der Aktivität `Get-AutomationVariable` in einem Runbook oder einer DSC-Konfiguration möglich. |
|[New-AzAutomationVariable](https://docs.microsoft.com/powershell/module/az.automation/new-azautomationvariable?view=azps-3.5.0) | Erstellt eine neue Variable und legt ihren Wert fest.|
|[Remove-AzAutomationVariable](https://docs.microsoft.com/powershell/module/az.automation/remove-azautomationvariable?view=azps-3.5.0)| Entfernt eine vorhandene Variable.|
|[Set-AzAutomationVariable](https://docs.microsoft.com/powershell/module/az.automation/set-azautomationvariable?view=azps-3.5.0)| Legt den Wert für eine vorhandene Variable fest. |

## <a name="activities-to-access-variables-in-runbooks-and-dsc-configurations"></a>Aktivitäten für den Zugriff auf Variablen in Runbooks und DSC-Konfigurationen

Die Aktivitäten in der folgenden Tabelle werden für den Zugriff auf Variablen in Runbooks und DSC-Konfigurationen verwendet. Die Cmdlets für diese Aktivitäten sind im globalen Modul `Orchestrator.AssetManagement.Cmdlets` enthalten.

| Aktivität | BESCHREIBUNG |
|:---|:---|
|`Get-AutomationVariable`|Ruft den Wert einer vorhandenen Variable ab.|
|`Set-AutomationVariable`|Legt den Wert für eine vorhandene Variable fest.|

> [!NOTE]
> Vermeiden Sie die Verwendung von Variablen im Parameter `Name` von `Get-AutomationVariable` in einem Runbook oder einer DSC-Konfiguration. Durch die Verwendung dieses Parameters kann die Ermittlung von Abhängigkeiten zwischen Runbooks oder DSC-Konfigurationen und Automation-Variablen zur Entwurfszeit erschwert werden.

Beachten Sie, dass `Get-AutomationVariable` in PowerShell nicht funktioniert, sondern nur in einem Runbook oder einer DSC-Konfiguration. Wenn Sie z. B. den Wert einer verschlüsselten Variable anzeigen möchten, können Sie ein Runbook erstellen, um die Variable abzurufen und dann in den Ausgabestream zu schreiben:
 
```powershell
$mytestencryptvar = Get-AutomationVariable -Name TestVariable
Write-output "The encrypted value of the variable is: $mytestencryptvar"
```

## <a name="functions-to-access-variables-in-python-2-runbooks"></a>Funktionen für den Zugriff auf Variablen in Python 2-Runbooks

Die Funktionen in der folgenden Tabelle werden zum Zugreifen auf Variablen in einem Python 2-Runbook verwendet.

|Python 2-Funktionen|BESCHREIBUNG|
|:---|:---|
|`automationassets.get_automation_variable`|Ruft den Wert einer vorhandenen Variable ab. |
|`automationassets.set_automation_variable`|Legt den Wert für eine vorhandene Variable fest. |

> [!NOTE]
> Sie müssen das Modul `automationassets` oben im Python-Runbook importieren, um auf die Ressourcenfunktionen zugreifen zu können.

## <a name="working-with-automation-variables"></a>Verwenden von Automation-Variablen

>[!NOTE]
>Wenn Sie die Verschlüsselung für eine Variable entfernen möchten, müssen Sie die Variable löschen und als unverschlüsselt neu erstellen.

### <a name="create-a-new-variable-using-the-azure-portal"></a>Erstellen einer neuen Variable über das Azure-Portal

1. Klicken Sie in Ihrem Automation-Konto auf die Kachel **Ressourcen** und dann auf das Blatt **Ressourcen**, und wählen Sie **Variablen** aus.
2. Klicken Sie auf dem Blatt **Variablen** auf **Variable hinzufügen**.
3. Füllen Sie die Optionen auf dem Blatt **Neue Variable** aus, und klicken Sie dann auf **Erstellen**, um die neue Variable zu speichern.

> [!NOTE]
> Nachdem Sie eine verschlüsselte Variable gespeichert haben, kann sie nicht mehr im Portal angezeigt werden. Sie kann lediglich aktualisiert werden.

### <a name="create-and-use-a-variable-in-windows-powershell"></a>Erstellen und Verwenden einer Variable mit Windows PowerShell

Ein PowerShell-Skript verwendet das Cmdlet `New-AzAutomationVariable` oder seine Entsprechung des AzureRM-Moduls, um eine neue Variable zu erstellen und ihren Anfangswert festzulegen. Wenn die Variable verschlüsselt ist, sollte beim Aufruf der Parameter `Encrypted` verwendet werden.

Das Skript ruft den Wert der Variable mithilfe von [Get-AzAutomationVariable](https://docs.microsoft.com/powershell/module/az.automation/get-azautomationvariable?view=azps-3.5.0) ab. Wenn es sich um einen einfachen Werttyp handelt, ruft das Cmdlet den gleichen Typ ab. Bei einem komplexen Typ wird der Typ `PSCustomObject` abgerufen.

>[!NOTE]
>Mit einem PowerShell-Skript kann kein verschlüsselter Wert abgerufen werden. Dies ist nur mit der Aktivität `Get-AutomationVariable` in einem Runbook oder einer DSC-Konfiguration möglich.

Das folgende Beispiel zeigt, wie eine Variable vom Typ „string“ erstellt und dann ihr Wert zurückgegeben wird.

```powershell
New-AzAutomationVariable -ResourceGroupName "ResourceGroup01" 
–AutomationAccountName "MyAutomationAccount" –Name 'MyStringVariable' `
–Encrypted $false –Value 'My String'
$string = (Get-AzAutomationVariable -ResourceGroupName "ResourceGroup01" `
–AutomationAccountName "MyAutomationAccount" –Name 'MyStringVariable').Value
```

Das folgende Beispiel zeigt, wie eine Variable mit einem komplexen Typ erstellt wird und dann ihre Eigenschaften abgerufen werden. In diesem Fall wird ein Objekt für einen virtuellen Computer von [Get-AzVM](https://docs.microsoft.com/powershell/module/Az.Compute/Get-AzVM?view=azps-3.5.0) verwendet.

```powershell
$vm = Get-AzVM -ResourceGroupName "ResourceGroup01" –Name "VM01"
New-AzAutomationVariable –AutomationAccountName "MyAutomationAccount" –Name "MyComplexVariable" –Encrypted $false –Value $vm

$vmValue = (Get-AzAutomationVariable -ResourceGroupName "ResourceGroup01" `
–AutomationAccountName "MyAutomationAccount" –Name "MyComplexVariable").Value
$vmName = $vmValue.Name
$vmIpAddress = $vmValue.IpAddress
```

### <a name="create-and-use-a-variable-in-a-runbook-or-dsc-configuration"></a>Erstellen und Verwenden einer Variable in einem Runbook oder einer DSC-Konfiguration

Die einzige Möglichkeit, eine neue Variable in einem Runbook oder einer DSC-Konfiguration zu erstellen, ist die Verwendung des Cmdlets `New-AzAutomationVariable` oder seiner Entsprechung im AzureRM-Modul. Das Skript verwendet dieses Cmdlet, um den Anfangswert der Variable festzulegen. Das Skript kann dann den Wert mithilfe von `Get-AzAutomationVariable` abrufen. Wenn es sich um einen einfachen Werttyp handelt, wird der gleiche Typ abgerufen. Bei einem komplexen Typ wird der Typ `PSCustomObject` abgerufen.

>[!NOTE]
>Ein verschlüsselter Wert kann ausschließlich über die Aktivität `Get-AutomationVariable` im Runbook oder in der DSC-Konfiguration abgerufen werden. 

### <a name="textual-runbook-samples"></a>Beispiele für Textrunbooks

#### <a name="set-and-retrieve-a-simple-value-from-a-variable"></a>Festlegen und Abrufen eines einfachen Werts aus einer Variable

Die folgenden Beispielbefehle zeigen, wie Sie eine Variable in einem Textrunbook festlegen und abrufen. In diesem Beispiel wird davon ausgegangen, dass die Integer-Variablen `NumberOfIterations` und `NumberOfRunnings` sowie eine String-Variable mit dem Namen `SampleMessage` erstellt werden.

```powershell
$NumberOfIterations = Get-AzAutomationVariable -ResourceGroupName "ResourceGroup01" –AutomationAccountName "MyAutomationAccount" -Name 'NumberOfIterations'
$NumberOfRunnings = Get-AzAutomationVariable -ResourceGroupName "ResourceGroup01" –AutomationAccountName "MyAutomationAccount" -Name 'NumberOfRunnings'
$SampleMessage = Get-AutomationVariable -Name 'SampleMessage'

Write-Output "Runbook has been run $NumberOfRunnings times."

for ($i = 1; $i -le $NumberOfIterations; $i++) {
    Write-Output "$i`: $SampleMessage"
}
Set-AzAutomationVariable -ResourceGroupName "ResourceGroup01" –AutomationAccountName "MyAutomationAccount" –Name NumberOfRunnings –Value ($NumberOfRunnings += 1)
```

#### <a name="set-and-retrieve-a-variable-in-a-python-2-runbook"></a>Festlegen und Abrufen einer Variable in einem Python 2-Runbook

Das folgende Beispiel veranschaulicht, wie Sie in einem Python 2-Runbook eine Variable verwenden, eine Variable festlegen und eine Ausnahme für eine nicht vorhandene Variable behandeln.

```python
import automationassets
from automationassets import AutomationAssetNotFound

# get a variable
value = automationassets.get_automation_variable("test-variable")
print value

# set a variable (value can be int/bool/string)
automationassets.set_automation_variable("test-variable", True)
automationassets.set_automation_variable("test-variable", 4)
automationassets.set_automation_variable("test-variable", "test-string")

# handle a non-existent variable exception
try:
    value = automationassets.get_automation_variable("nonexisting variable")
except AutomationAssetNotFound:
    print "variable not found"
```

### <a name="graphical-runbook-samples"></a>Beispiel für ein grafisches Runbook

In einem grafischen Runbook können Sie die Aktivitäten `Get-AutomationVariable` oder `Set-AutomationVariable` hinzufügen. Klicken Sie einfach mit der rechten Maustaste im Bibliotheksbereich des grafischen Editors auf die Variable, und wählen Sie die gewünschte Aktivität aus.

![Hinzufügen einer Variablen zum Zeichenbereich](../media/variables/runbook-variable-add-canvas.png)

#### <a name="set-values-in-a-variable"></a>Festlegen von Werten in einer Variable

Die folgende Abbildung zeigt Beispielaktivitäten zum Aktualisieren einer Variablen mit einem einfachen Wert in einem grafischen Runbook. In diesem Beispiel wird mit `Get-AzVM` ein einzelner virtueller Azure-Computer abgerufen, und der Name des Computers wird in einer vorhandenen Automation-Variable des Typs „string“ gespeichert. Es spielt keine Rolle, ob es sich bei der [Verknüpfung um eine Pipeline oder eine Sequenz handelt](../automation-graphical-authoring-intro.md#links-and-workflow), da der Code in der Ausgabe nur ein einziges Objekt erwartet.

![Festlegen einer einfachen Variablen](../media/variables/runbook-set-simple-variable.png)

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zur Verknüpfung von Aktivitäten bei der grafischen Inhaltserstellung finden Sie unter [Links bei der grafischen Erstellung](../automation-graphical-authoring-intro.md#links-and-workflow).
- Informationen zu den ersten Schritten mit grafischen Runbooks finden Sie unter [Mein erstes grafisches Runbook](../automation-first-runbook-graphical.md).
