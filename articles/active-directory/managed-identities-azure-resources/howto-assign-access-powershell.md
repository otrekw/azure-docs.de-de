---
title: Zuweisen des Zugriffs für eine verwaltete Identität auf eine Ressource mithilfe von PowerShell – Azure AD
description: Hier finden Sie schrittweise Anleitungen für das Zuweisen einer verwalteten Identität für eine Ressource und für das Zuweisen des Zugriffs auf eine andere Ressource mithilfe von PowerShell.
services: active-directory
documentationcenter: ''
author: barclayn
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/06/2018
ms.author: barclayn
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8ae2da130f61d31db4904ed2dd5ac18444929950
ms.sourcegitcommit: 3fb5e772f8f4068cc6d91d9cde253065a7f265d6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89177498"
---
# <a name="assign-a-managed-identity-access-to-a-resource-using-powershell"></a>Zuweisen des Zugriffs einer verwalteten Identität auf eine Ressource mithilfe von PowerShell

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Nachdem Sie eine Azure-Ressource mit einer verwalteten Identität konfiguriert haben, können Sie der verwalteten Identität genau wie bei allen anderen Sicherheitsprinzipalen Zugriff auf eine andere Ressource erteilen. In diesem Beispiel wird gezeigt, wie Sie der verwalteten Identität eines virtuellen Azure-Computers mithilfe von PowerShell Zugriff auf ein Azure-Speicherkonto erteilen.

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie nicht mit verwalteten Identitäten für Azure-Ressourcen vertraut sind, helfen Ihnen die Informationen in der [Übersicht](overview.md) weiter. **Machen Sie sich den [Unterschied zwischen einer vom System und einer vom Benutzer zugewiesenen verwalteten Identität](overview.md#managed-identity-types)** bewusst.
- Wenn Sie noch kein Azure-Konto haben, sollten Sie sich [für ein kostenloses Konto registrieren](https://azure.microsoft.com/free/), bevor Sie fortfahren.
- Installieren Sie [die aktuelle Version von Azure PowerShell](/powershell/azure/install-az-ps), sofern noch nicht geschehen.

## <a name="use-azure-rbac-to-assign-a-managed-identity-access-to-another-resource"></a>Verwenden von Azure RBAC zum Zuweisen des Zugriffs einer verwalteten Identität auf eine andere Ressource

Nachdem Sie die verwaltete Identität für eine Azure-Ressource aktiviert haben, [zum Beispiel für einen virtuellen Azure-Computer](qs-configure-powershell-windows-vm.md), gehen Sie wie folgt vor:

1. Melden Sie sich bei Azure mithilfe des Cmdlets `Connect-AzAccount` an. Verwenden Sie ein Konto, das dem Azure-Abonnement zugeordnet ist, unter dem Sie die verwaltete Identität konfiguriert haben:

   ```powershell
   Connect-AzAccount
   ```
2. In diesem Beispiel wird einem virtuellen Azure-Computer der Zugriff auf ein Speicherkonto gewährt. Zuerst wird [Get-AzRMVM](/powershell/module/az.compute/get-azvm) verwendet, um den Dienstprinzipal für den virtuellen Computer mit dem Namen `myVM` abzurufen, der beim Aktivieren der verwalteten Identität erstellt wurde. Verwenden Sie dann [New-AzRmRoleAssignment](/powershell/module/Az.Resources/New-AzRoleAssignment), um dem virtuellen Computer **Reader** Zugriff auf ein Speicherkonto namens `myStorageAcct` zu erteilen:

    ```powershell
    $spID = (Get-AzVM -ResourceGroupName myRG -Name myVM).identity.principalid
    New-AzRoleAssignment -ObjectId $spID -RoleDefinitionName "Reader" -Scope "/subscriptions/<mySubscriptionID>/resourceGroups/<myResourceGroup>/providers/Microsoft.Storage/storageAccounts/<myStorageAcct>"
    ```

## <a name="next-steps"></a>Nächste Schritte

- [Verwaltete Identitäten für Azure-Ressourcen – Übersicht](overview.md)
- Informationen zum Aktivieren einer verwalteten Identität auf einem virtuellen Azure-Computer finden Sie unter [Konfigurieren von verwalteten Identitäten für Azure-Ressourcen auf einem virtuellen Azure-Computer mithilfe von PowerShell](qs-configure-powershell-windows-vm.md).
