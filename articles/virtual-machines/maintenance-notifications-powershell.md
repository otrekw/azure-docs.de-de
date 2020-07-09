---
title: Erhalten von Wartungsbenachrichtigungen für virtuelle Azure-Computer mithilfe von PowerShell
description: Zeigen Sie mithilfe von PowerShell Wartungsbenachrichtigungen für in Azure ausgeführte virtuelle Computer an, und starten Sie eine Self-Service-Wartung.
author: shants123
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 11/19/2019
ms.author: shants
ms.openlocfilehash: 79ef649502af8962253121ea18804966b1fa043e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84677932"
---
# <a name="handling-planned-maintenance-using-powershell"></a>Behandeln von geplanten Wartungen mithilfe von PowerShell

**Dieser Artikel gilt für virtuelle Computer, auf denen sowohl Linux als auch Windows ausgeführt wird.**

Sie können mithilfe von Azure PowerShell ermitteln, wann die [Wartung](maintenance-notifications.md) für VMs geplant ist. Informationen zur geplanten Wartung können mit dem Cmdlet [Get-AzVM](https://docs.microsoft.com/powershell/module/az.compute/get-azvm) unter Verwendung des Parameters `-status` ermittelt werden.
  
Wartungsinformationen werden nur zurückgegeben, wenn eine Wartung geplant ist. Ist keine Wartung für den virtuellen Computer geplant, gibt das Cmdlet keine Wartungsinformationen zurück. 


```powershell
Get-AzVM -ResourceGroupName myResourceGroup -Name myVM -Status
```

Unter „MaintenanceRedeployStatus“ werden folgende Eigenschaften zurückgegeben: 

| value | BESCHREIBUNG   |
|-------|---------------|
| IsCustomerInitiatedMaintenanceAllowed | Gibt an, ob Sie zum aktuellen Zeitpunkt die Wartung für den virtuellen Computer starten können. |
| PreMaintenanceWindowStartTime         | Der Anfang des Self-Service-Wartungszeitfensters, in dem Sie die Wartung für Ihren virtuellen Computer initiieren können. |
| PreMaintenanceWindowEndTime           | Das Ende des Self-Service-Wartungszeitfensters, in dem Sie die Wartung für Ihren virtuellen Computer initiieren können. |
| MaintenanceWindowStartTime            | Der Anfang der geplanten Wartung, zu dem Azure die Wartung für Ihren virtuellen Computer initiiert |
| MaintenanceWindowEndTime              | Das Ende des geplanten Wartungszeitfensters, in dem Azure die Wartung für Ihren virtuellen Computer initiiert |
| LastOperationResultCode               | Das Ergebnis des letzten Wartungsinitiierungsversuchs für den virtuellen Computer. |



Sie können auch den Wartungsstatus für alle virtuellen Computer in einer Ressourcengruppe abrufen. Verwenden Sie hierzu [Get-AzVM](https://docs.microsoft.com/powershell/module/az.compute/get-azvm) ohne Angabe eines virtuellen Computers.
 
```powershell
Get-AzVM -ResourceGroupName myResourceGroup -Status
```

Das folgende PowerShell-Beispiel gibt auf der Grundlage Ihrer Abonnement-ID eine Liste mit virtuellen Computern zurück, für die eine Wartung geplant ist.

```powershell

function MaintenanceIterator
{
    Select-AzSubscription -SubscriptionId $args[0]

    $rgList= Get-AzResourceGroup 

    for ($rgIdx=0; $rgIdx -lt $rgList.Length ; $rgIdx++)
    {
        $rg = $rgList[$rgIdx]        
    $vmList = Get-AzVM -ResourceGroupName $rg.ResourceGroupName 
        for ($vmIdx=0; $vmIdx -lt $vmList.Length ; $vmIdx++)
        {
            $vm = $vmList[$vmIdx]
            $vmDetails = Get-AzVM -ResourceGroupName $rg.ResourceGroupName -Name $vm.Name -Status
              if ($vmDetails.MaintenanceRedeployStatus )
            {
                Write-Output "VM: $($vmDetails.Name)  IsCustomerInitiatedMaintenanceAllowed: $($vmDetails.MaintenanceRedeployStatus.IsCustomerInitiatedMaintenanceAllowed) $($vmDetails.MaintenanceRedeployStatus.LastOperationMessage)"               
            }
          }
    }
}

```

### <a name="start-maintenance-on-your-vm-using-powershell"></a>Starten der Wartung für Ihren virtuellen Computer mithilfe von PowerShell

Der folgende Befehl enthält Informationen aus der Funktion des vorherigen Abschnitts und startet die Wartung für einen virtuellen Computer, sofern **IsCustomerInitiatedMaintenanceAllowed** auf „true“ festgelegt ist.

```powershell
Restart-AzVM -PerformMaintenance -name $vm.Name -ResourceGroupName $rg.ResourceGroupName 
```

## <a name="classic-deployments"></a>Klassische Bereitstellungen

[!INCLUDE [classic-vm-deprecation](../../includes/classic-vm-deprecation.md)]

Wenn Sie ältere virtuelle Computer besitzen, die mit dem klassischen Bereitstellungsmodell bereitgestellt wurden, können Sie mit PowerShell virtuelle Computer abfragen und die Wartung initiieren.

Geben Sie zum Abrufen des Wartungsstatus eines virtuellen Computers Folgendes ein:

```
Get-AzureVM -ServiceName <Service name> -Name <VM name>
```

Geben Sie zum Starten der Wartung auf dem klassischen virtuellen Computer Folgendes ein:

```
Restart-AzureVM -InitiateMaintenance -ServiceName <service name> -Name <VM name>
```

## <a name="next-steps"></a>Nächste Schritte

Sie können die geplante Wartung auch mithilfe der [Azure CLI](maintenance-notifications-cli.md) oder mit dem [Portal](maintenance-notifications-portal.md) vornehmen.
