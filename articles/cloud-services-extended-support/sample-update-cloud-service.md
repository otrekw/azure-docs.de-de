---
title: 'Azure PowerShell-Beispiele: Aktualisieren einer Instanz von Azure Cloud Services (erweiterter Support)'
description: Beispielskripts zum Aktualisieren von Azure Cloud Services-Bereitstellungen (erweiterter Support)
ms.topic: sample
ms.service: cloud-services-extended-support
author: gachandw
ms.author: gachandw
ms.reviewer: mimckitt
ms.date: 10/13/2020
ms.custom: ''
ms.openlocfilehash: fcb92ec3aa8f360107f0e7220415c57344c6d83a
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "98881468"
---
# <a name="update-an-azure-cloud-service-extended-support"></a>Aktualisieren eines Azure-Clouddiensts (erweiterter Support)

In diesen Beispielen werden verschiedene Möglichkeiten zum Aktualisieren einer vorhandenen Azure Cloud Services-Bereitstellung (erweiterter Support) behandelt.

## <a name="add-an-extension-to-existing-cloud-service"></a>Hinzufügen einer Erweiterung zu einer vorhandenen Cloud Services-Instanz
```powershell
# Create RDP extension object
$rdpExtension = New-AzCloudServiceRemoteDesktopExtensionObject -Name "RDPExtension" -Credential $credential -Expiration $expiration -TypeHandlerVersion "1.2.1"
# Get existing Cloud Service
$cloudService = Get-AzCloudService -ResourceGroup "ContosOrg" -CloudServiceName "ContosoCS"
# Add RDP extension to existing Cloud Service extension object
$cloudService.ExtensionProfileExtension = $cloudService.ExtensionProfileExtension + $rdpExtension
# Update Cloud Service
$cloudService | Update-AzCloudService
```

## <a name="remove-all-extensions-from-a-cloud-service"></a>Entfernen aller Erweiterungen aus einer Cloud Services-Instanz
```powershell
# Get existing Cloud Service
$cloudService = Get-AzCloudService -ResourceGroup "ContosOrg" -CloudServiceName "ContosoCS"
# Set extension to empty list
$cloudService.ExtensionProfileExtension = @()
# Update Cloud Service
$cloudService | Update-AzCloudService
```

## <a name="remove-the-remote-desktop-extension-from-cloud-service"></a>Entfernen der Remotedesktoperweiterung aus Cloud Services
```powershell
# Get existing Cloud Service
$cloudService = Get-AzCloudService -ResourceGroup "ContosOrg" -CloudServiceName "ContosoCS"
# Remove extension by name RDPExtension
$cloudService.ExtensionProfileExtension = $cloudService.ExtensionProfileExtension | Where-Object { $_.Name -ne "RDPExtension" }
# Update Cloud Service
$cloudService | Update-AzCloudService
```

## <a name="scale-out--scale-in-role-instances"></a>Horizontales Skalieren/Abskalieren von Rolleninstanzen
```powershell
# Get existing Cloud Service
$cloudService = Get-AzCloudService -ResourceGroup "ContosOrg" -CloudServiceName "ContosoCS"

# Scale-out all role instance count by 1
$cloudService.RoleProfileRole | ForEach-Object {$_.SkuCapacity += 1}

# Scale-in ContosoFrontend role instance count by 1
$role = $cloudService.RoleProfileRole | Where-Object {$_.Name -eq "ContosoFrontend"}
 $role.SkuCapacity -= 1

# Update Cloud Service configuration as per the new role instance count
$cloudService.Configuration = $configuration

# Update Cloud Service
$cloudService | Update-AzCloudService
```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Azure Cloud Services (erweiterter Support) finden Sie unter [Informationen zu Azure Cloud Services (erweiterter Support)](overview.md).
- Besuchen Sie das [Beispielrepository zu Cloud Services (erweiterter Support)](https://github.com/Azure-Samples/cloud-services-extended-support).