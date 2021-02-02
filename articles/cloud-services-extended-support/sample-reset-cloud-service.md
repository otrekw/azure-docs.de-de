---
title: 'Azure PowerShell-Beispiele: Zurücksetzen von Azure Cloud Services (erweiterter Support)'
description: Beispielskripts zum Zurücksetzen einer Azure-Clouddienstbereitstellung mit erweitertem Support
ms.topic: sample
ms.service: cloud-services-extended-support
author: gachandw
ms.author: gachandw
ms.reviewer: mimckitt
ms.date: 10/13/2020
ms.custom: ''
ms.openlocfilehash: 40b44fd277eac14a5bf2c15f58fccfd9d5b156c4
ms.sourcegitcommit: aaa65bd769eb2e234e42cfb07d7d459a2cc273ab
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/27/2021
ms.locfileid: "98881484"
---
# <a name="reset-an-azure-cloud-service-extended-support"></a>Zurücksetzen eines Azure-Clouddiensts (erweiterter Support) 
In diesen Beispielen werden verschiedene Möglichkeiten zum Zurücksetzen einer vorhandenen Azure-Clouddienstbereitstellung mit erweitertem Support behandelt.

## <a name="reimage-role-instances-of-cloud-service"></a>Durchführen eines Reimagings für Rolleninstanzen des Clouddiensts
```powershell
$roleInstances = @("ContosoFrontEnd_IN_0", "ContosoBackEnd_IN_1")
Reset-AzCloudService -ResourceGroupName "ContosOrg" -CloudServiceName "ContosoCS" -RoleInstance $roleInstances -Reimage
```
Mit diesem Befehl wird ein Reimaging für zwei Rolleninstanzen (**ContosoFrontEnd\_IN\_0** und **ContosoBackEnd\_IN\_1**) des Clouddiensts „ContosoCS“ durchgeführt, der zur Ressourcengruppe „ContosOrg“ gehört.

## <a name="reimage-all-roles-of-cloud-service"></a>Durchführen eines Reimagings für alle Rollen des Clouddiensts
```powershell
Reset-AzCloudService -ResourceGroupName "ContosOrg" -CloudServiceName "ContosoCS" -RoleInstance "*" -Reimage
```

## <a name="reimage-a-single-role-instance-of-a-cloud-service"></a>Durchführen eines Reimagings für eine einzelne Rolleninstanz des Clouddiensts
```powershell
Reset-AzCloudServiceRoleInstance -ResourceGroupName "ContosOrg" -CloudServiceName "ContosoCS" -RoleInstanceName "ContosoFrontEnd_IN_0" -Reimage
```

## <a name="restart-a-single-role-instance-of-a-cloud-service"></a>Neustarten einer einzelnen Rolleninstanz des Clouddiensts
```powershell
Reset-AzCloudService -ResourceGroupName "ContosOrg" -CloudServiceName "ContosoCS" -RoleInstance "*" -Restart
```

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu Azure Cloud Services (erweiterter Support) finden Sie unter [Informationen zu Azure Cloud Services (erweiterter Support)](overview.md).
- Besuchen Sie das [Beispielrepository zu Cloud Services (erweiterter Support)](https://github.com/Azure-Samples/cloud-services-extended-support).