---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/09/2021
ms.author: alkohli
ms.openlocfilehash: 1298673c475a0308f1db27641aae93837e6b5df3
ms.sourcegitcommit: b4fbb7a6a0aa93656e8dd29979786069eca567dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2021
ms.locfileid: "107305554"
---
Stellen Sie sicher, dass Sie mit den folgenden Schritten von Ihrem Client aus auf das Gerät zugreifen können.

Vergewissern Sie sich, dass Ihr Client eine Verbindung mit der lokalen Azure Resource Manager-Instanz herstellen kann. 

1. Rufen Sie zur Authentifizierung lokale Geräte-APIs auf:

    ```powershell
    login-AzureRMAccount -EnvironmentName <Environment Name> -TenantId c0257de7-538f-415c-993a-1b87a031879d  
    ```

1. Geben Sie den Benutzernamen `EdgeArmUser` und das Kennwort für die Verbindung über Azure Resource Manager an. Sollten Sie Ihr Kennwort vergessen haben, [setzen Sie das Kennwort für Azure Resource Manager zurück](../articles/databox-online/azure-stack-edge-gpu-set-azure-resource-manager-password.md), und melden Sie sich mit diesem Kennwort an.