---
title: Konfigurieren von Servern auf einen gewünschten Status und Verwalten der Abweichung mit Azure Automation
description: 'Tutorial: Verwalten von Serverkonfigurationen mit der Azure Automation-Zustandskonfiguration'
services: automation
ms.subservice: dsc
ms.topic: conceptual
ms.date: 08/08/2018
ms.openlocfilehash: a02c664ddf0802ad5ac306f98de14b7c0d5d7271
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/21/2020
ms.locfileid: "81678711"
---
# <a name="configure-servers-to-a-desired-state-and-manage-drift"></a>Konfigurieren von Servern mit einem gewünschten Status und Verwalten der Abweichung mit Azure Automation

Mit der Azure Automation-Zustandskonfiguration können Sie Konfigurationen für Ihre Server festlegen und sicherstellen, dass sich diese Server im Verlauf der Zeit im angegebenen Zustand befinden.

> [!div class="checklist"]
> - Integrieren eines virtuellen Computers in die Azure Automation DSC-Verwaltung
> - Hochladen einer Konfiguration in Azure Automation
> - Kompilieren einer Konfiguration in eine Knotenkonfiguration
> - Zuweisen einer Knotenkonfiguration zu einem verwalteten Knoten
> - Überprüfen des Konformitätsstatus eines verwalteten Knotens

Für dieses Tutorial verwenden wir eine einfache [DSC-Konfiguration](/powershell/scripting/dsc/configurations/configurations), die sicherstellt, dass IIS auf der VM installiert ist.

>[!NOTE]
>Dieser Artikel wurde aktualisiert und beinhaltet jetzt das neue Az-Modul von Azure PowerShell. Sie können das AzureRM-Modul weiterhin verwenden, das bis mindestens Dezember 2020 weiterhin Fehlerbehebungen erhält. Weitere Informationen zum neuen Az-Modul und zur Kompatibilität mit AzureRM finden Sie unter [Introducing the new Azure PowerShell Az module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0) (Einführung in das neue Az-Modul von Azure PowerShell). Installationsanweisungen für das Az-Modul auf Ihrem Hybrid Runbook Worker finden Sie unter [Installieren des Azure PowerShell-Moduls](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). In Ihrem Automation-Konto können Sie die Module mithilfe der Informationen unter [Aktualisieren von Azure PowerShell-Modulen in Azure Automation](automation-update-azure-modules.md) auf die neueste Version aktualisieren.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

- Ein Azure Automation-Konto. Informationen zum Erstellen eines ausführenden Azure Automation-Kontos finden Sie unter [Azure Run As Account](automation-sec-configure-azure-runas-account.md)(Ausführendes Azure-Konto).
- Eine Azure Resource Manager-VM (nicht klassisch) unter Windows Server 2008 R2 oder höher. Eine Anleitung zum Erstellen einer VM finden Sie unter [Erstellen Ihres ersten virtuellen Windows-Computers im Azure-Portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md).
- Azure PowerShell-Modul, Version 3.6 oder höher. Führen Sie `Get-Module -ListAvailable Az` aus, um die Version zu ermitteln. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/azurerm/install-azurerm-ps) Informationen dazu.
- Erfahrung mit DSC (Desired State Configuration, Konfiguration des gewünschten Zustands). Weitere Informationen zu DSC finden Sie unter [Windows PowerShell DSC – Übersicht](/powershell/scripting/dsc/overview/overview).

## <a name="support-for-partial-configurations"></a>Unterstützung für Teilkonfigurationen

Azure Automation State Configuration unterstützt die Verwendung von [Teilkonfigurationen](/powershell/scripting/dsc/pull-server/partialconfigs). In diesem Szenario ist DSC so konfiguriert, dass mehrere Konfigurationen unabhängig voneinander verwaltet werden können, wobei jede Konfiguration von Azure Automation abgerufen wird. Einem Knoten kann aber nur eine Konfiguration mittels Automation-Konto zugewiesen werden. Dies bedeutet, dass Sie bei Verwendung von zwei Konfigurationen für einen Knoten zwei Automation-Kontos benötigen.

Ausführliche Informationen zum Registrieren einer Teilkonfiguration aus einem Pulldienst finden Sie in der Dokumentation zu [Teilkonfigurationen](https://docs.microsoft.com/powershell/scripting/dsc/pull-server/partialconfigs#partial-configurations-in-pull-mode).

Weitere Informationen dazu, wie Teams zusammenarbeiten können, um Server gemeinsam mithilfe von Konfiguration als Code zu verwalten, finden Sie unter [Grundlegendes zu DSCs Rolle in einer CI/CD-Pipeline](/powershell/scripting/dsc/overview/authoringadvanced).

## <a name="log-in-to-azure"></a>Anmelden an Azure

Melden Sie sich mit dem Cmdlet [Connect-AzAccount](https://docs.microsoft.com/powershell/module/Az.Accounts/Connect-AzAccount?view=azps-3.7.0) bei Ihrem Azure-Abonnement an, und befolgen Sie die Anweisungen auf dem Bildschirm.

```powershell
Connect-AzAccount
```

## <a name="create-and-upload-a-configuration-to-azure-automation"></a>Erstellen und Hochladen einer Konfiguration in Azure Automation


Geben Sie in einem Texteditor Folgendes ein, und speichern Sie die Datei lokal als **TestConfig.ps1**.

```powershell
configuration TestConfig {
   Node WebServer {
      WindowsFeature IIS {
         Ensure               = 'Present'
         Name                 = 'Web-Server'
         IncludeAllSubFeature = $true
      }
   }
}
```

> [!NOTE]
> In komplexeren Szenarien, in denen mehrere Module importiert werden müssen, die DSC-Ressourcen bereitstellen, müssen Sie sicherstellen, dass jedes Modul über eine eindeutige Zeile vom Typ `Import-DscResource` in Ihrer Konfiguration verfügt.

Rufen Sie das Cmdlet [Import-AzAutomationDscConfiguration](https://docs.microsoft.com/powershell/module/Az.Automation/Import-AzAutomationDscConfiguration?view=azps-3.7.0) auf, um die Konfiguration in Ihr Automation-Konto hochzuladen.

```powershell
 Import-AzAutomationDscConfiguration -SourcePath 'C:\DscConfigs\TestConfig.ps1' -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -Published
```

## <a name="compile-a-configuration-into-a-node-configuration"></a>Kompilieren einer Konfiguration in eine Knotenkonfiguration

Eine DSC-Konfiguration muss in eine Knotenkonfiguration kompiliert werden, bevor sie einem Knoten zugewiesen werden kann. Weitere Informationen finden Sie unter [DSC-Konfigurationen](/powershell/scripting/dsc/configurations/configurations).

Rufen Sie das Cmdlet [Start-AzAutomationDscCompilationJob](https://docs.microsoft.com/powershell/module/Az.Automation/Start-AzAutomationDscCompilationJob?view=azps-3.7.0) auf, um die `TestConfig`-Konfiguration in eine Knotenkonfiguration namens `TestConfig.WebServer` in Ihrem Automation-Konto zu kompilieren.

```powershell
Start-AzAutomationDscCompilationJob -ConfigurationName 'TestConfig' -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount'
```

## <a name="register-a-vm-to-be-managed-by-state-configuration"></a>Registrieren eines virtuellen Computers für die Verwaltung durch die Zustandskonfiguration

Mit Azure Automation State Configuration können Sie virtuelle Azure-Computer (mit dem klassischen Modell oder dem Resource Manager-Modell), lokale virtuelle Computer, Linux-Computer, virtuelle AWS-Computer und lokale physische Computer verwalten. In diesem Thema wird das Registrieren von ausschließlich Azure Resource Manager-VMs behandelt. Informationen zum Registrieren anderer Computertypen finden Sie unter [Integrieren von Computern für die Verwaltung durch die Azure Automation-Zustandskonfiguration](automation-dsc-onboarding.md).

Rufen Sie das Cmdlet [Register-AzAutomationDscNode](https://docs.microsoft.com/powershell/module/Az.Automation/Register-AzAutomationDscNode?view=azps-3.7.0) auf, um Ihren virtuellen Computer bei der Azure Automation State Configuration als verwalteten Knoten zu registrieren. 

```powershell
Register-AzAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -AzureVMName 'DscVm'
```

### <a name="specify-configuration-mode-settings"></a>Angeben der Konfigurationsmoduseinstellungen

Verwenden Sie das Cmdlet [Register-AzAutomationDscNode](/powershell/module/azurerm.automation/register-azurermautomationdscnode), um einen virtuellen Computer als verwalteten Knoten zu registrieren und Konfigurationseigenschaften anzugeben. Sie können z. B. festlegen, dass der Zustand des Computers nur einmalig angewendet werden soll, indem Sie `ApplyOnly` als Wert der `ConfigurationMode`-Eigenschaft angeben. State Configuration versucht nach der anfänglichen Überprüfung nicht, die Konfiguration anzuwenden.

```powershell
Register-AzAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -AzureVMName 'DscVm' -ConfigurationMode 'ApplyOnly'
```

Sie können auch angeben, wie oft DSC den Konfigurationszustand überprüft, indem Sie die `ConfigurationModeFrequencyMins`-Eigenschaft verwenden. Weitere Informationen zu DSC-Konfigurationseinstellungen finden Sie unter [Konfigurieren des lokalen Konfigurations-Managers](/powershell/scripting/dsc/managing-nodes/metaConfig).

```powershell
# Run a DSC check every 60 minutes
Register-AzAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -AzureVMName 'DscVm' -ConfigurationModeFrequencyMins 60
```

## <a name="assign-a-node-configuration-to-a-managed-node"></a>Zuweisen einer Knotenkonfiguration zu einem verwalteten Knoten

Nun können wir die kompilierte Knotenkonfiguration der zu konfigurierenden VM zuordnen.

```powershell
# Get the ID of the DSC node
$node = Get-AzAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -Name 'DscVm'

# Assign the node configuration to the DSC node
Set-AzAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -NodeConfigurationName 'TestConfig.WebServer' -NodeId $node.Id
```

Damit wird dem registrierten DSC-Knoten `DscVm` die Knotenkonfiguration namens `TestConfig.WebServer` zugeordnet. Standardmäßig wird der DSC-Knoten alle 30 Minuten auf Konformität mit der Knotenkonfiguration geprüft. Informationen zum Ändern des Intervalls für die Konformitätsprüfung finden Sie unter [Konfigurieren des lokalen Konfigurations-Managers](/powershell/scripting/dsc/managing-nodes/metaConfig).

## <a name="check-the-compliance-status-of-a-managed-node"></a>Überprüfen des Konformitätsstatus eines verwalteten Knotens

Sie können Berichte zum Compliancestatus eines verwalteten Knotens abrufen, indem Sie das Cmdlet [Get-AzAutomationDscNodeReport](https://docs.microsoft.com/powershell/module/Az.Automation/Get-AzAutomationDscNodeReport?view=azps-3.7.0) verwenden.

```powershell
# Get the ID of the DSC node
$node = Get-AzAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -Name 'DscVm'

# Get an array of status reports for the DSC node
$reports = Get-AzAutomationDscNodeReport -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -NodeId $node.Id

# Display the most recent report
$reports[0]
```

## <a name="remove-nodes-from-service"></a>Entfernen von Knoten aus dem Dienst

Wenn Sie Azure Automation State Configuration einen Knoten hinzufügen, werden die Einstellungen im lokalen Konfigurations-Manager so festgelegt, dass eine Registrierung beim Dienst erfolgt und Konfigurationen sowie erforderliche Module zum Konfigurieren des Computers abgerufen werden.
Wenn Sie den Knoten aus dem Dienst entfernen möchten, können Sie dazu entweder das Azure-Portal oder die Az-Cmdlets verwenden.

> [!NOTE]
> Durch das Aufheben der Registrierung eines Knotens beim Dienst werden die Einstellungen des lokalen Konfigurations-Managers lediglich so festgelegt, dass der Knoten keine Verbindung mehr mit dem Dienst herstellt.
> Dies hat keine Auswirkung auf die Konfiguration, die derzeit auf den Knoten angewendet wird.
> Zum Entfernen der aktuellen Konfiguration verwenden Sie [PowerShell](https://docs.microsoft.com/powershell/module/psdesiredstateconfiguration/remove-dscconfigurationdocument?view=powershell-5.1), oder löschen Sie die lokale Konfigurationsdatei (dies ist für Linux-Knoten die einzige Option).

### <a name="azure-portal"></a>Azure-Portal

Klicken Sie in Azure Automation im Inhaltsverzeichnis auf **Zustandskonfiguration (DSC)** .
Klicken Sie dann auf **Knoten**, um die Liste der Knoten anzuzeigen, die beim Dienst registriert sind.
Klicken Sie auf den Namen des Knotens, den Sie entfernen möchten.
Klicken Sie in der daraufhin geöffneten Knotenansicht auf **Registrierung aufheben**.

### <a name="powershell"></a>PowerShell

Zum Aufheben der Registrierung eines Knotens beim Azure Automation State Configuration-Dienst mithilfe von PowerShell befolgen Sie die Anweisungen in der Dokumentation für das Cmdlet [Unregister-AzAutomationDscNode](https://docs.microsoft.com/powershell/module/az.automation/unregister-azautomationdscnode?view=azps-2.0.0).

## <a name="next-steps"></a>Nächste Schritte

- Eine Einführung finden Sie unter [Erste Schritte mit Azure Automation State Configuration](automation-dsc-getting-started.md).
- Weitere Informationen zum Onboarding von Knoten finden Sie unter [Onboarding von Computern zur Verwaltung durch Azure Automation DSC](automation-dsc-onboarding.md).
- Wie Sie DSC-Konfigurationen kompilieren und anschließend Zielknoten zuweisen, erfahren Sie unter [Kompilieren von DSC-Konfigurationen in Azure Automation DSC](automation-dsc-compile.md).
- Eine PowerShell-Cmdlet-Referenz ist unter [Azure Automation State Configuration-Cmdlets](/powershell/module/azurerm.automation/#automation) verfügbar.
- Eine Preisübersicht finden Sie unter [Automation – Preise](https://azure.microsoft.com/pricing/details/automation/).
- Ein Verwendungsbeispiel für Azure Automation State Configuration in einer Continuous Deployment-Pipeline finden Sie unter [Continuous Deployment mit Azure Automation State Configuration und Chocolatey](automation-dsc-cd-chocolatey.md).
