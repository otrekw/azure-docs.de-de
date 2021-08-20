---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 06/30/2021
ms.author: alkohli
ms.openlocfilehash: e0770eb27bfebc085d99af9fdc9749676b699b70
ms.sourcegitcommit: 98308c4b775a049a4a035ccf60c8b163f86f04ca
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/30/2021
ms.locfileid: "113106602"
---
Stellen Sie sicher, dass Sie mit den folgenden Schritten von Ihrem Client aus auf das Gerät zugreifen können.

Vergewissern Sie sich, dass Ihr Client eine Verbindung mit der lokalen Azure Resource Manager-Instanz herstellen kann. 

1. Rufen Sie zur Authentifizierung lokale Geräte-APIs auf:

    ### <a name="az"></a>[Az](#tab/az)

    ```powershell
    login-AzAccount -EnvironmentName <Environment Name> -TenantId c0257de7-538f-415c-993a-1b87a031879d  
    ```

    ### <a name="azurerm"></a>[AzureRM](#tab/azure-rm)

    ```powershell
    login-AzureRMAccount -EnvironmentName <Environment Name> -TenantId c0257de7-538f-415c-993a-1b87a031879d  
    ```

1. Geben Sie den Benutzernamen `EdgeArmUser` und das Kennwort für die Verbindung über Azure Resource Manager an. Sollten Sie Ihr Kennwort vergessen haben, [setzen Sie das Kennwort für Azure Resource Manager zurück](../articles/databox-online/azure-stack-edge-gpu-set-azure-resource-manager-password.md), und melden Sie sich mit diesem Kennwort an.