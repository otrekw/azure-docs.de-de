---
title: 'PowerShell-Skript: Suchen nach Tresor für Speicherkonto'
description: Hier erfahren Sie, wie Sie mithilfe eines Azure PowerShell-Skripts nach dem Recovery Services-Tresor suchen können, bei dem Ihr Speicherkonto registriert ist.
ms.topic: sample
ms.date: 1/28/2020
ms.openlocfilehash: b343b2f93ed439188c5c0238bf108064f6e132c1
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86513250"
---
# <a name="powershell-script-to-find-the-recovery-services-vault-where-a-storage-account-is-registered"></a>PowerShell-Skript für die Suche nach dem Recovery Services-Tresor, bei dem ein Speicherkonto registriert ist

Mithilfe dieses Skripts können Sie nach dem Recovery Services-Tresor suchen, bei dem Ihr Speicherkonto registriert ist.

## <a name="sample-script"></a>Beispielskript

```powershell
Param(
        [Parameter(Mandatory=$True)][System.String] $ResourceGroupName,
        [Parameter(Mandatory=$True)][System.String] $StorageAccountName,
        [Parameter(Mandatory=$True)][System.String] $SubscriptionId
    )

Connect-AzAccount
Select-AzSubscription -Subscription $SubscriptionId
$vaults = Get-AzRecoveryServicesVault
$found = $false
foreach($vault in $vaults)
{
  Write-Verbose "Checking vault: $($vault.Id)" -Verbose
  
  $containers = Get-AzRecoveryServicesBackupContainer -ContainerType AzureStorage -FriendlyName $StorageAccountName -ResourceGroupName $ResourceGroupName -VaultId $vault.Id -Status Registered
  
  if($containers -ne $null)
  {
    $found = $True
    Write-Information "Found Storage account $StorageAccountName registered in vault: $($vault.Id)" -InformationAction Continue
    break;
  }
}

if(!$found)
{
     Write-Information "Storage account: $StorageAccountName is not registered in any vault of this subscription" -InformationAction Continue
}
```

## <a name="how-to-execute-the-script"></a>Ausführen des Skripts

1. Speichern Sie das obige Skript auf Ihrem Computer mit einem Namen Ihrer Wahl. In diesem Beispiel wurde es als *FindRegisteredStorageAccount.ps1* gespeichert.
2. Führen Sie das Skript unter Angabe folgender Parameter aus:

    * **-ResourceGroupName**: Die Ressourcengruppe des Speicherkontos.
    * **-StorageAccountName**: Der Name des Speicherkontos.
    * **-SubscriptionID**: Die ID des Abonnements, in dem sich das Speicherkonto befindet.

Im folgenden Beispiel wird versucht, den Recovery Services-Tresor zu finden, bei dem das Speicherkonto *afsaccount* registriert ist:

```powershell
.\FindRegisteredStorageAccount.ps1 -ResourceGroupName AzureFiles -StorageAccountName afsaccount -SubscriptionId ef4ad5a7-c2c0-4304-af80-af49f49af3d1
```

## <a name="output"></a>Output

Die Ausgabe enthält den vollständigen Pfad des Recovery Services-Tresors, bei dem das Speicherkonto registriert ist. Hier ist eine Beispielausgabe:

```output
Found Storage account afsaccount registered in vault: /subscriptions/ ef4ad5a7-c2c0-4304-af80-af49f49af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault123
```

## <a name="next-steps"></a>Nächste Schritte

Machen Sie sich mit dem [Sichern von Azure-Dateifreigaben in einem Recovery Services-Tresor](../backup-afs.md) vertraut.
