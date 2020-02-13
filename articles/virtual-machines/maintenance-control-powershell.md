---
title: Steuerung der Wartung für virtuelle Azure-Computer mithilfe von PowerShell
description: Erfahren Sie, wie Sie die Wartung steuern, wenn diese mithilfe der Wartungssteuerung und PowerShell auf Ihre virtuellen Azure-Computer angewendet wird.
author: cynthn
ms.service: virtual-machines
ms.topic: article
ms.workload: infrastructure-services
ms.date: 01/31/2020
ms.author: cynthn
ms.openlocfilehash: 58bdbdfcb82806dfa0b2779967bddf1242a8b702
ms.sourcegitcommit: 7c18afdaf67442eeb537ae3574670541e471463d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/11/2020
ms.locfileid: "77115701"
---
# <a name="preview-control-updates-with-maintenance-control-and-azure-powershell"></a>Vorschau: Steuern von Updates mit der Wartungssteuerung und Azure PowerShell

Verwalten Sie mit der Wartungssteuerung Plattformupdates, die keinen Neustart erfordern. Azure aktualisiert die eigene Infrastruktur häufig, um die Zuverlässigkeit, Leistung und Sicherheit zu verbessern oder neue Features zu integrieren. Die meisten Updates sind für Benutzer transparent. Einige empfindlichen Workloads wie Gaming, Medienstreaming und Finanzbuchungen können nicht einmal wenige Sekunden tolerieren, in denen eine VM zur Wartung eingefroren oder getrennt ist. Die Wartungssteuerung bietet Ihnen die Möglichkeit, auf Plattformupdates zu warten und diese innerhalb eines 35-tägigen rollierenden Zeitfensters anzuwenden. 

Mit der Wartungssteuerung können Sie entscheiden, wann Sie Updates auf Ihre isolierten VMs anwenden.

Mit der Wartungssteuerung können Sie folgende Aktionen ausführen:
- Batchupdates in ein Updatepaket
- Warten von bis zu 35 Tagen bis zur Anwendung der Updates 
- Automatisieren von Plattformupdates für Ihr Wartungsfenster mit Azure Functions
- Wartungskonfigurationen funktionieren abonnement- und ressourcengruppenübergreifend. 

> [!IMPORTANT]
> Die Wartungssteuerung ist derzeit als öffentliche Vorschauversion verfügbar.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
> 

## <a name="limitations"></a>Einschränkungen

- Virtuelle Computer müssen sich auf einem [dedizierten Host](./linux/dedicated-hosts.md) befinden oder mithilfe einer [isolierten VM-Größe](./linux/isolation.md) erstellt werden.
- Nach 35 Tagen wird automatisch ein Update angewendet.
- Der Benutzer muss über einen **Ressourcenmitwirkender**-Zugriff verfügen.


## <a name="enable-the-powershell-module"></a>Aktivieren des PowerShell-Moduls

Stellen Sie sicher, dass `PowerShellGet` auf dem neuesten Stand ist.

```azurepowershell-interactive
Install-Module -Name PowerShellGet -Repository PSGallery -Force
```

Die Az.Maintenance-Cmdlets der PowerShell befinden sich in der Vorschauphase, weshalb Sie das Modul mit dem Parameter `AllowPrerelease` in Cloud Shell oder Ihrer lokalen PowerShell-Installation installieren müssen.   

```azurepowershell-interactive
Install-Module -Name Az.Maintenance -AllowPrerelease
```

Wenn Sie lokal installieren, stellen Sie sicher, dass Sie die PowerShell-Eingabeaufforderung als Administrator öffnen.

Möglicherweise werden Sie auch aufgefordert zu bestätigen, dass Sie aus einem *nicht vertrauenswürdigen Repository* installieren möchten. Geben Sie `Y` ein, oder wählen Sie **Ja, alle** aus, um das Modul zu installieren.



## <a name="create-a-maintenance-configuration"></a>Erstellen einer Wartungskonfiguration

Erstellen Sie eine Ressourcengruppe als Container für Ihre Konfiguration. In diesem Beispiel wird eine Ressourcengruppe mit dem Namen *myMaintenanceRG* in *eastus* erstellt. Wenn Sie bereits über eine Ressourcengruppe verfügen, die Sie verwenden möchten, können Sie diesen Teil überspringen und den Ressourcengruppennamen in den restlichen Beispielen durch Ihren eigenen ersetzen.

```azurepowershell-interactive
New-AzResourceGroup `
   -Location eastus `
   -Name myMaintenanceRG
```

Verwenden Sie [New-AzMaintenanceConfiguration](https://docs.microsoft.com/powershell/module/az.maintenance/new-azmaintenanceconfiguration), um eine Wartungskonfiguration zu erstellen. In diesem Beispiel wird eine Wartungskonfiguration mit dem Namen *myConfig* erstellt, die für den Host gilt. 

```azurepowershell-interactive
$config = New-AzMaintenanceConfiguration `
   -ResourceGroup myMaintenanceRG `
   -Name myConfig `
   -MaintenanceScope host `
   -Location  eastus
```

Durch die Verwendung von `-MaintenanceScope host` wird sichergestellt, dass die Wartungskonfiguration zum Steuern von Updates für den Host verwendet wird.

Wenn Sie versuchen, eine Konfiguration mit dem gleichen Namen, aber an einem anderen Speicherort zu erstellen, erhalten Sie eine Fehlermeldung. Konfigurationsnamen müssen für Ihr Abonnement eindeutig sein.

Sie können verfügbare Wartungskonfigurationen abfragen, indem Sie [Get-AzMaintenanceConfiguration](https://docs.microsoft.com/powershell/module/az.maintenance/get-azmaintenanceconfiguration) verwenden.

```azurepowershell-interactive
Get-AzMaintenanceConfiguration | Format-Table -Property Name,Id
```

## <a name="assign-the-configuration"></a>Zuweisen der Konfiguration

Verwenden Sie [New-AzConfigurationAssignment](https://docs.microsoft.com/powershell/module/az.maintenance/new-azconfigurationassignment), um die Konfiguration Ihrer isolierten VM oder dem Azure Dedicated Host zuzuweisen.

### <a name="isolated-vm"></a>Isolierte VM

Wenden Sie die Konfiguration auf einen virtuellen Computer an, indem Sie die ID der Konfiguration verwenden. Geben Sie `-ResourceType VirtualMachines` an, und geben Sie den Namen der VM für `-ResourceName` sowie die Ressourcengruppe der VM für `-ResourceGroupName` an. 

```azurepowershell-interactive
New-AzConfigurationAssignment `
   -ResourceGroupName myResourceGroup `
   -Location eastus `
   -ResourceName myVM `
   -ResourceType VirtualMachines `
   -ProviderName Microsoft.Compute `
   -ConfigurationAssignmentName $config.Name `
   -MaintenanceConfigurationId $config.Id
```

### <a name="dedicated-host"></a>Dedicated Host

Wenn Sie eine Konfiguration auf einen dedizierten Host anwenden möchten, müssen Sie auch `-ResourceType hosts`, `-ResourceParentName` mit dem Namen der Hostgruppe und `-ResourceParentType hostGroups` einschließen. 


```azurepowershell-interactive
New-AzConfigurationAssignment `
   -ResourceGroupName myResourceGroup `
   -Location eastus `
   -ResourceName myHost `
   -ResourceType hosts `
   -ResourceParentName myHostGroup `
   -ResourceParentType hostGroups `
   -ProviderName Microsoft.Compute `
   -ConfigurationAssignmentName $config.Name `
   -MaintenanceConfigurationId $config.Id
```

## <a name="check-for-pending-updates"></a>Prüfen auf ausstehende Updates

Verwenden Sie [Get-AzMaintenanceUpdate](https://docs.microsoft.com/powershell/module/az.maintenance/get-azmaintenanceupdate), um zu überprüfen, ob noch Updates ausstehen. Verwenden Sie `-subscription`, um das Azure-Abonnement der VM anzugeben, wenn diese nicht der Computer ist, bei dem Sie sich angemeldet haben.

Wenn keine Updates zum Anzeigen vorhanden sind, gibt der Befehl keinen Wert zurück. Andernfalls wird ein PSApplyUpdate-Objekt zurückgegeben:

```json
{
   "maintenanceScope": "Host",
   "impactType": "Freeze",
   "status": "Pending",
   "impactDurationInSec": 9,
   "notBefore": "2020-02-21T16:47:44.8728029Z",
   "properties": {
      "resourceId": "/subscriptions/39c6cced-4d6c-4dd5-af86-57499cd3f846/resourcegroups/Ignite2019/providers/Microsoft.Compute/virtualMachines/MCDemo3"
} 
```

### <a name="isolated-vm"></a>Isolierte VM

Überprüfen Sie, ob für eine isolierte VM ausstehende Updates angezeigt werden. In diesem Beispiel ist die Ausgabe zur besseren Lesbarkeit als Tabelle formatiert.

```azurepowershell-interactive
Get-AzMaintenanceUpdate `
  -ResourceGroupName myResourceGroup `
  -ResourceName myVM `
  -ResourceType VirtualMachines `
  -ProviderName Microsoft.Compute | Format-Table
```


### <a name="dedicated-host"></a>Dedicated Host

Überprüfen Sie, ob für einen dedizierten Host ausstehende Updates angezeigt werden. In diesem Beispiel ist die Ausgabe zur besseren Lesbarkeit als Tabelle formatiert. Ersetzen Sie die Werte für die Ressourcen durch Ihre eigenen.

```azurepowershell-interactive
Get-AzMaintenanceUpdate `
   -ResourceGroupName myResourceGroup `
   -ResourceName myHost `
   -ResourceType hosts `
   -ResourceParentName myHostGroup `
   -ResourceParentType hostGroups `
   -ProviderName Microsoft.Compute | Format-Table
```


## <a name="apply-updates"></a>Anwenden von Updates

Verwenden Sie [New-AzApplyUpdate](https://docs.microsoft.com/powershell/module/az.maintenance/new-azapplyupdate), um ausstehende Updates anzuwenden.

### <a name="isolated-vm"></a>Isolierte VM

Erstellen Sie eine Anforderung, um Updates auf eine isolierte VM anzuwenden.

```azurepowershell-interactive
New-AzApplyUpdate `
   -ResourceGroupName myResourceGroup `
   -ResourceName myVM `
   -ResourceType VirtualMachines `
   -ProviderName Microsoft.Compute
```

Bei einer erfolgreichen Ausführung gibt dieser Befehl ein `PSApplyUpdate`-Objekt zurück. Sie können das Name-Attribut im Befehl `Get-AzApplyUpdate` verwenden, um den Aktualisierungsstatus zu überprüfen. Weitere Informationen finden Sie unter [Überprüfen des Updatestatus](#check-update-status).

### <a name="dedicated-host"></a>Dedicated Host

Wenden Sie Updates auf einen dedizierten Host an.

```azurepowershell-interactive
New-AzApplyUpdate `
   -ResourceGroupName myResourceGroup `
   -ResourceName myHost `
   -ResourceType hosts `
   -ResourceParentName myHostGroup `
   -ResourceParentType hostGroups `
   -ProviderName Microsoft.Compute
```

## <a name="check-update-status"></a>Überprüfen des Updatestatus
Verwenden Sie [Get-AzApplyUpdate](https://docs.microsoft.com/powershell/module/az.maintenance/get-azapplyupdate), um den Status eines Updates zu überprüfen. Die unten gezeigten Befehle zeigen den Status des neuesten Updates mithilfe von `default` für den Parameter `-ApplyUpdateName` an. Sie können den Namen des Updates ersetzen (vom Befehl [New-AzApplyUpdate](https://docs.microsoft.com/powershell/module/az.maintenance/new-azapplyupdate) zurückgegeben), um den Status eines bestimmten Updates zu erhalten.

```text
Status         : Completed
ResourceId     : /subscriptions/12ae7457-4a34-465c-94c1-17c058c2bd25/resourcegroups/TestShantS/providers/Microsoft.Comp
ute/virtualMachines/DXT-test-04-iso
LastUpdateTime : 1/1/2020 12:00:00 AM
Id             : /subscriptions/12ae7457-4a34-465c-94c1-17c058c2bd25/resourcegroups/TestShantS/providers/Microsoft.Comp
ute/virtualMachines/DXT-test-04-iso/providers/Microsoft.Maintenance/applyUpdates/default
Name           : default
Type           : Microsoft.Maintenance/applyUpdates
```

### <a name="isolated-vm"></a>Isolierte VM

Suchen Sie nach Updates für einen bestimmten virtuellen Computer.

```azurepowershell-interactive
Get-AzApplyUpdate `
   -ResourceGroupName myResourceGroup `
   -ResourceName myVM `
   -ResourceType VirtualMachines `
   -ProviderName Microsoft.Compute `
   -ApplyUpdateName default
```

### <a name="dedicated-host"></a>Dedicated Host

Suchen Sie nach Updates für einen dedizierten Host.

```azurepowershell-interactive
Get-AzApplyUpdate `
   -ResourceGroupName myResourceGroup `
   -ResourceName myHost `
   -ResourceType hosts `
   -ResourceParentName myHostGroup `
   -ResourceParentType hostGroups `
   -ProviderName Microsoft.Compute `
   -ApplyUpdateName myUpdateName
```

## <a name="remove-a-maintenance-configuration"></a>Entfernen einer Wartungskonfiguration

Verwenden Sie [Remove-AzMaintenanceConfiguration](https://docs.microsoft.com/powershell/module/az.maintenance/remove-azmaintenanceconfiguration), um eine Wartungskonfiguration zu löschen.

```azurecli-interactive
Remove-AzMaintenanceConfiguration `
   -ResourceGroupName myResourceGroup `
   -Name $config.Name
```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie unter [Wartung und Updates](maintenance-and-updates.md).
