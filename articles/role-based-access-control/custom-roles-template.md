---
title: Erstellen einer benutzerdefinierten Azure-Rolle mithilfe einer Azure Resource Manager-Vorlage – Azure RBAC
description: Erfahren Sie, wie Sie mithilfe von Azure Resource Manager-Vorlagen und rollenbasierter Zugriffssteuerung (Azure RBAC) eine benutzerdefinierte Azure-Rolle erstellen können.
services: role-based-access-control,azure-resource-manager
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.topic: how-to
ms.custom: subject-armqs
ms.workload: identity
ms.date: 06/25/2020
ms.author: rolyon
ms.openlocfilehash: bcf1966ffc326291448cb611d99390fe0d652151
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "85397995"
---
# <a name="create-an-azure-custom-role-using-an-azure-resource-manager-template"></a>Erstellen einer benutzerdefinierten Azure-Rolle mithilfe einer Azure Resource Manager-Vorlage

Wenn die [integrierten Azure-Rollen](built-in-roles.md) die Anforderungen Ihrer Organisation nicht erfüllen, können Sie Ihre eigenen [benutzerdefinierten Rollen](custom-roles.md) erstellen. In diesem Artikel wird beschrieben, wie eine benutzerdefinierte Rolle mithilfe einer Azure Resource Manager-Vorlage erstellt wird.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

## <a name="prerequisites"></a>Voraussetzungen

Zum Erstellen einer benutzerdefinierten Rolle benötigen Sie Folgendes:

- Berechtigungen zum Erstellen von benutzerdefinierten Rollen, etwa [Besitzer](built-in-roles.md#owner) oder [Benutzerzugriffsadministrator](built-in-roles.md#user-access-administrator)

## <a name="create-a-custom-role"></a>Erstellen einer benutzerdefinierten Rolle

Zum Erstellen einer benutzerdefinierten Rolle geben Sie einen Rollennamen, Berechtigungen und den Ort an, an dem die Rolle verwendet werden kann. In diesem Artikel erstellen Sie eine Rolle namens „Benutzerdefinierte Rolle – RG Reader“ mit Ressourcenberechtigungen, die in einem Abonnementbereich oder einer niedrigeren Ebene zugewiesen werden können.

### <a name="review-the-template"></a>Überprüfen der Vorlage

Die in diesem Artikel verwendete Vorlage stammt aus den [Azure-Schnellstartvorlagen](https://github.com/Azure/azure-quickstart-templates/tree/master/subscription-deployments/create-role-def). Die Vorlage hat vier Parameter und einen Ressourcenabschnitt. Die vier Parameter sind:

- Array von Aktionen mit einem Standardwert von [„Microsoft.Resources/subscriptions/resourceGroups/read“]
- Array von Nicht-Aktionen mit einem leeren Standardwert
- Rollenname mit einem Standardwert von „Benutzerdefinierte Rolle – RG Reader“.
- Rollenbeschreibung mit dem Standardwert „Subscription Level Deployment of a Role Definition" (Bereitstellung einer Rollendefinition auf Abonnementebene).

In der Vorlage ist die folgende Ressource definiert:

- [Microsoft.Authorization/roleDefinitions](/azure/templates/Microsoft.Authorization/roleDefinitions)

Der Bereich, in dem diese benutzerdefinierte Rolle zugeordnet werden kann, wird auf das aktuelle Abonnement festgelegt.

:::code language="json" source="~/quickstart-templates/subscription-deployments/create-role-def/azuredeploy.json":::

### <a name="deploy-the-template"></a>Bereitstellen der Vorlage

Führen Sie diese Schritte aus, um die vorherige Vorlage bereitzustellen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Öffnen Sie Azure Cloud Shell für PowerShell.

1. Kopieren Sie das folgende Skript, und fügen Sie es in Cloud Shell ein.

    ```azurepowershell
    $location = Read-Host -Prompt "Enter a location (i.e. centralus)"
    [string[]]$actions = Read-Host -Prompt "Enter actions as a comma-separated list (i.e. action1,action2)"
    $actions = $actions.Split(',')

    $templateUri = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/subscription-deployments/create-role-def/azuredeploy.json"

    New-AzDeployment -Location $location -TemplateUri $templateUri -actions $actions
    ```

1. Geben Sie einen Standort für die Bereitstellung ein, z. B. *centralus*.

1. Geben Sie eine Liste von Aktionen für die benutzerdefinierte Rolle als durch Trennzeichen getrennte Liste ein, z. B. *Microsoft.Resources/resources/read,Microsoft.Resources/subscriptions/resourceGroups/read*.

1. Drücken Sie ggf. die EINGABETASTE, um den Befehl „New-AzDeployment“ auszuführen.

    Der Befehl [New-AzDeployment](/powershell/module/az.resources/new-azdeployment) stellt die Vorlage zum Erstellen der benutzerdefinierten Rolle bereit.

    Die Ausgabe sollte etwa folgendermaßen aussehen:

    ```azurepowershell
    PS> New-AzDeployment -Location $location -TemplateUri $templateUri -actions $actions
    
    Id                      : /subscriptions/{subscriptionId}/providers/Microsoft.Resources/deployments/azuredeploy
    DeploymentName          : azuredeploy
    Location                : centralus
    ProvisioningState       : Succeeded
    Timestamp               : 6/25/2020 8:08:32 PM
    Mode                    : Incremental
    TemplateLink            :
                              Uri            : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/subscription-deployments/create-role-def/azuredeploy.json
                              ContentVersion : 1.0.0.0
    
    Parameters              :
                              Name               Type                       Value
                              =================  =========================  ==========
                              actions            Array                      [
                                "Microsoft.Resources/resources/read",
                                "Microsoft.Resources/subscriptions/resourceGroups/read"
                              ]
                              notActions         Array                      []
                              roleName           String                     Custom Role - RG Reader
                              roleDescription    String                     Subscription Level Deployment of a Role Definition
    
    Outputs                 :
    DeploymentDebugLogLevel :
    ```

## <a name="review-deployed-resources"></a>Überprüfen der bereitgestellten Ressourcen

Führen Sie diese Schritte aus, um sicherzustellen, dass die benutzerdefinierte Rolle erstellt wurde.

1. Führen Sie den Befehl [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition) aus, um die benutzerdefinierte Rolle aufzulisten.

    ```azurepowershell
    Get-AzRoleDefinition "Custom Role - RG Reader" | ConvertTo-Json
    ```

    Die Ausgabe sollte etwa folgendermaßen aussehen:

    ```azurepowershell
    {
      "Name": "Custom Role - RG Reader",
      "Id": "11111111-1111-1111-1111-111111111111",
      "IsCustom": true,
      "Description": "Subscription Level Deployment of a Role Definition",
      "Actions": [
        "Microsoft.Resources/resources/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read"
      ],
      "NotActions": [],
      "DataActions": [],
      "NotDataActions": [],
      "AssignableScopes": [
        "/subscriptions/{subscriptionId}"
      ]
    }
    ```

1. Öffnen Sie im Azure-Portal Ihr Abonnement.

1. Klicken Sie im linken Menü auf **Zugriffssteuerung (IAM)** .

1. Klicken Sie auf die Registerkarte **Rollen**.

1. Legen Sie die Liste **Typ** auf **CustomRole** fest.

1. Stellen Sie sicher, dass die Rolle **Benutzerdefinierte Rolle – RG Reader** aufgeführt ist.

   ![Neue benutzerdefinierte Rolle im Azure-Portal](./media/custom-roles-template/custom-role-template-portal.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Um die benutzerdefinierte Rolle zu entfernen, führen Sie die folgenden Schritte aus.

1. Führen Sie den folgenden Befehl aus, um die benutzerdefinierte Rolle zu entfernen.

    ```azurepowershell
    Get-AzRoleDefinition -Name "Custom Role - RG Reader" | Remove-AzRoleDefinition
    ```

1. Geben Sie **J** ein, um zu bestätigen, dass Sie die benutzerdefinierte Rolle entfernen möchten.

## <a name="next-steps"></a>Nächste Schritte

- [Grundlegendes zu Azure-Rollendefinitionen](role-definitions.md)
- [Schnellstart: Hinzufügen einer Azure-Rollenzuweisung mithilfe einer Azure Resource Manager-Vorlage](quickstart-role-assignments-template.md)
- [Dokumentation zu ARM-Vorlagen](../azure-resource-manager/templates/index.yml)
