---
title: Einstellung der Unterstützung von IPConfig-Parametern für das Cmdlet „New-AzRecoveryServicesAsrVMNicConfig“ | Microsoft-Dokumentation
description: Details zur Einstellung der Unterstützung von IPConfig-Parametern des Cmdlets „New-AzRecoveryServicesAsrVMNicConfig“ und Informationen zur Verwendung des neuen Cmdlets „New-AzRecoveryServicesAsrVMNicIPConfig“
services: site-recovery
author: rishjai-msft
manager: gaggupta
ms.service: site-recovery
ms.topic: article
ms.date: 04/30/2021
ms.author: rishjai
ms.openlocfilehash: 569687a6efca34e631b096b448d988c18b21a6c9
ms.sourcegitcommit: f6b76df4c22f1c605682418f3f2385131512508d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2021
ms.locfileid: "108326913"
---
# <a name="deprecation-of-ip-config-parameters-for-the-cmdlet-new-azrecoveryservicesasrvmnicconfig"></a>Einstellung der Unterstützung von IPConfig-Parametern für das Cmdlet „New-AzRecoveryServicesAsrVMNicConfig“

In diesem Artikel werden die Einstellung, die entsprechenden Auswirkungen und die für die Kunden verfügbaren alternativen Optionen für das folgende Szenario beschrieben:

Konfigurieren der Konfigurationseinstellungen für die primäre IP-Adresse für das Failover oder Testfailover Dieses Cmdlet wirkt sich auf alle Kunden des Szenarios „Notfallwiederherstellung von Azure zu Azure“ aus, die das Cmdlet „New-AzRecoveryServicesAsrVMNicConfig“ verwenden.

> [!IMPORTANT]
> Den Kunden wird empfohlen, dass sie schnellstmöglich die erforderlichen Wartungsschritte ausführen, um jegliche Unterbrechung in ihrer Umgebung zu vermeiden. 

## <a name="what-changes-should-you-expect"></a>Welche Änderungen sind zu erwarten?

„New-AzRecoveryServicesAsrVMNicConfig“ nutzt die folgenden Parameter für die Konfiguration der IP-Konfigurationswerte für Failover/Testfailover:
- RecoveryVMSubnetName
- RecoveryNicStaticIPAddress
- RecoveryPublicIPAddressId
- RecoveryLBBackendAddressPoolId
- TfoVMSubnetName
- TfoNicStaticIPAddress
- TfoPublicIPAddressId
- TfoLBBackendAddressPoolId

Diese Parameter werden vom Cmdlet nicht mehr akzeptiert.

- Ab dem 4. Mai 2021 erhalten Sie Benachrichtigungen im Azure-Portal sowie E-Mails mit Informationen zur Einstellung der Unterstützung von IP-Konfigurationsparametern im Cmdlet „New-AzRecoveryServicesAsrVMNicConfig“.

- Ein vorhandenes Skript, das dieses Cmdlet verwendet, wird nicht mehr unterstützt.
 
## <a name="alternatives"></a>Alternativen 

Als Alternative wird das neue Cmdlet [New-AzRecoveryServicesAsrVMNicIPConfig](https://docs.microsoft.com/powershell/module/az.recoveryservices/new-azrecoveryservicesasrvmnicipconfig) zum Konfigurieren von IP-Konfigurationseinstellungen für Failover/Testfailover eingeführt. 


## <a name="remediation-steps"></a>Schritte zur Bereinigung

Es wird erwartet, dass Sie Ihre Skripts ändern und diese Parameter entfernen. Verwenden Sie stattdessen das neue Cmdlet **New-AzRecoveryServicesAsrVMNicIPConfig**, um ein IP-Konfigurationsobjekt zu erstellen. Nachfolgend wird dies veranschaulicht:

Ihre **vorhandenen Skripts** wurden wie folgt geschrieben:
```azurepowershell
# Fetching the Protected Item Object (for the Protected VM)
$protectedItemObject = Get-AsrReplicationProtectedItem -ProtectionContainer $primaryContainerObject | where { $_.FriendlyName -eq $VMName };$protectedItemObject

# ID of the NIC whose settings are to be updated.
$nicId = $protectedItemObject.NicDetailsList[0].NicId

$nic1 = New-AzRecoveryServicesAsrVMNicConfig -NicId $nicId -ReplicationProtectedItem $protectedItemObject -RecoveryVMNetworkId <networkArmId> -TfoVMNetworkId <networkArmId> -RecoveryVMSubnetName "default" -TfoVMSubnetName "default" -RecoveryNicStaticIPAddress "10.1.40.223" -TfoNicStaticIPAddress "10.33.0.223"

$nics = @($nic1)
Set-AzRecoveryServicesAsrReplicationProtectedItem -InputObject $protectedItemObject -ASRVMNicConfiguration $nics
```

Ändern Sie Ihre Skripts **wie folgt**:
```azurepowershell
# Fetching the Protected Item Object (for the Protected VM)
$protectedItemObject = Get-AsrReplicationProtectedItem -ProtectionContainer $primaryContainerObject | where { $_.FriendlyName -eq $VMName };$protectedItemObject

# Create the config object for Primary IP Config
$ipConfig = New-AzRecoveryServicesAsrVMNicIPConfig  -IpConfigName <ipConfigName> -RecoverySubnetName "default" -TfoSubnetName "default" -RecoveryStaticIPAddress "10.1.40.223" -TfoStaticIPAddress "10.33.0.223"

$ipConfigs = @($ipConfig)

# ID of the NIC whose settings are to be updated.
$nicId = $protectedItemObject.NicDetailsList[0].NicId

$nic1 = New-AzRecoveryServicesAsrVMNicConfig -NicId $nicId -ReplicationProtectedItem $protectedItemObject -RecoveryVMNetworkId <networkArmId> -TfoVMNetworkId <networkArmId> -IPConfig $ipConfigs

$nics = @($nic1)
Set-AzRecoveryServicesAsrReplicationProtectedItem -InputObject $protectedItemObject -ASRVMNicConfiguration $nics
```

## <a name="next-steps"></a>Nächste Schritte
Ändern Sie Ihre Skripts wie oben dargestellt. Falls Sie Fragen dazu haben, wenden Sie sich an den Microsoft-Support.