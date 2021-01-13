---
title: Wartungssteuerung für Betriebssystemimageupgrades in Azure-VM-Skalierungsgruppen über PowerShell
description: Erfahren Sie, wie Sie steuern können, wann automatische Betriebssystemimageupgrades in Ihren Azure-VM-Skalierungsgruppen über Wartungssteuerung und PowerShell bereitgestellt (ausgerollt) werden.
author: ju-shim
ms.service: virtual-machine-scale-sets
ms.topic: how-to
ms.workload: infrastructure-services
ms.date: 09/11/2020
ms.author: jushiman
ms.openlocfilehash: d8acab17e9d8dfc078b46f6a279cc671a70b0a50
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2020
ms.locfileid: "91974837"
---
# <a name="preview-maintenance-control-for-os-image-upgrades-on-azure-virtual-machine-scale-sets-using-powershell"></a>Vorschau: Wartungssteuerung für Betriebssystemimageupgrades in Azure-VM-Skalierungsgruppen über PowerShell

Über die Wartungssteuerung können Sie entscheiden, wann automatische Gastbetriebssystemimageupgrades auf Ihre VM-Skalierungsgruppen angewendet werden sollen. In diesem Thema werden die Azure PowerShell-Optionen für die Wartungssteuerung behandelt. Weitere Informationen zur Verwendung der Wartungssteuerung finden Sie unter [Wartungssteuerung für Azure-VM-Skalierungsgruppen](virtual-machine-scale-sets-maintenance-control.md).

> [!IMPORTANT]
> Die Wartungssteuerung für Betriebssystemimageupgrades in Azure-VM-Skalierungsgruppen befindet sich derzeit in der öffentlichen Vorschau (Public Preview).
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).


## <a name="enable-the-powershell-module"></a>Aktivieren des PowerShell-Moduls

Stellen Sie sicher, dass `PowerShellGet` auf dem neuesten Stand ist.    

```azurepowershell-interactive  
Install-Module -Name PowerShellGet -Repository PSGallery -Force 
``` 

Installieren Sie das `Az.Maintenance`-PowerShell-Modul.     

```azurepowershell-interactive  
Install-Module -Name Az.Maintenance
``` 

Wenn Sie lokal installieren, müssen Sie die PowerShell-Eingabeaufforderung als Administrator öffnen.

Möglicherweise werden Sie auch aufgefordert zu bestätigen, dass Sie aus einem *nicht vertrauenswürdigen Repository* installieren möchten. Geben Sie `Y` ein, oder wählen Sie **Ja, alle** aus, um das Modul zu installieren.

## <a name="connect-to-an-azure-account"></a>Verbinden mit einem Azure-Konto

Stellen Sie über [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount) und [Set-AzAccount](/powershell/module/az.accounts/set-azcontext) eine Verbindung mit Ihrem gewünschten Azure-Konto her.

```azurepowershell-interactive
Connect-AzAccount
Set-AzContext 00a000aa-0a00-0a0a-00aa-a00a000aaa00

$RGName="myMaintenanceRG"
$MaintenanceConfig="myMaintenanceConfig"
$location="eastus2"
$vmss="myMaintenanceVMSS"
```

## <a name="create-a-maintenance-configuration"></a>Erstellen einer Wartungskonfiguration

Erstellen Sie eine Ressourcengruppe als Container für Ihre Konfiguration. In diesem Beispiel wird eine Ressourcengruppe mit dem Namen *myMaintenanceRG* in *eastus2* erstellt. Wenn Sie bereits eine Ressourcengruppe haben, die Sie verwenden möchten, können Sie diesen Abschnitt überspringen. Ersetzen Sie einfach den Ressourcengruppennamen in den restlichen Beispielen durch den von Ihnen verwendeten Namen.

```azurepowershell-interactive
New-AzResourceGroup `
   -Location $location `
   -Name $RGName
```

Verwenden Sie [New-AzMaintenanceConfiguration](/powershell/module/az.maintenance/new-azmaintenanceconfiguration), um eine Wartungskonfiguration zu erstellen. In diesem Beispiel wird eine Wartungskonfiguration mit dem Namen *myConfig* erstellt, die für das Betriebssystemimage gilt. 

```azurepowershell-interactive
$config = New-AzMaintenanceConfiguration `
   -ResourceGroup $RGName `
   -Name $MaintenanceConfig `
   -MaintenanceScope OSImage `
   -Location $location `
   -StartDateTime "2020-10-01 00:00" `
   -TimeZone "Pacific Standard Time" `
   -Duration "05:00" `
   -RecurEvery "Day"
```

> [!IMPORTANT]
> Die Dauer (**Duration**) der Wartung muss *5 Stunden* oder länger sein. Die Wartungswiederholung (**RecurEvery**) muss auf *Day* festgelegt werden.

Durch die Verwendung von `-MaintenanceScope OSImage` wird sichergestellt, dass die Wartungskonfiguration zum Steuern von Updates für das Gastbetriebssystem verwendet wird.

Wenn Sie versuchen, eine Konfiguration mit demselben Namen, aber an einem anderen Speicherort zu erstellen, erhalten Sie eine Fehlermeldung. Konfigurationsnamen müssen für Ihre Ressourcengruppe eindeutig sein.

Sie können verfügbare Wartungskonfigurationen abfragen, indem Sie [Get-AzMaintenanceConfiguration](/powershell/module/az.maintenance/get-azmaintenanceconfiguration) verwenden.

```azurepowershell-interactive
Get-AzMaintenanceConfiguration | Format-Table -Property Name,Id
```

## <a name="associate-your-virtual-machine-scale-set-to-the-maintenance-configuration"></a>Zuordnen Ihrer VM-Skalierungsgruppe zur Wartungskonfiguration

Eine VM-Skalierungsgruppe kann jeder Wartungskonfiguration zugeordnet werden, unabhängig von der Region und dem Abonnement der Wartungskonfiguration. Wenn Sie die Wartungskonfiguration abonnieren, werden neue Betriebssystemimageupdates für die Skalierungsgruppe automatisch für das nächste verfügbare Wartungsfenster geplant.

Verwenden Sie [New-AzConfigurationAssignment](/powershell/module/az.maintenance/new-azconfigurationassignment), um Ihre VM-Skalierungsgruppe zur Wartungskonfiguration zuzuordnen.

```azurepowershell-interactive
New-AzConfigurationAssignment `
   -ResourceGroupName $RGName `
   -Location $location `
   -ResourceName $vmss `
   -ResourceType VirtualMachineScaleSets `
   -ProviderName Microsoft.Compute `
   -ConfigurationAssignmentName $config.Name`
   -MaintenanceConfigurationId $config.Id
``` 

## <a name="enable-automatic-os-upgrade"></a>Aktivieren von automatischen Betriebssystemupgrades

Sie können automatische Betriebssystemupgrades für jede VM-Skalierungsgruppe aktivieren, für die die Wartungssteuerung verwendet werden soll. Weitere Informationen zum Aktivieren automatischer Betriebssystemupgrades für Ihre VM-Skalierungsgruppe finden Sie im Dokument [Automatische Betriebssystemimageupgrades mit Azure-VM-Skalierungsgruppen](../virtual-machine-scale-sets/virtual-machine-scale-sets-automatic-upgrade.md). 


## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über Wartung und Updates für virtuelle Computer, die in Azure ausgeführt werden.

> [!div class="nextstepaction"]
> [Wartung und Updates](maintenance-and-updates.md)