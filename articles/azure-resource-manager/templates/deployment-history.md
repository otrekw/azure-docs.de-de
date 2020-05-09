---
title: Bereitstellungsverlauf
description: Erfahren Sie, wie Sie Azure Resource Manager-Bereitstellungsvorgänge mit dem Portal, mit PowerShell, mit der Azure-Befehlszeilenschnittstelle (CLI) und der REST-API anzeigen.
tags: top-support-issue
ms.topic: conceptual
ms.date: 11/26/2019
ms.openlocfilehash: b0f196f86bed05094b04bfc20c7cef2248a91c65
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "79460295"
---
# <a name="view-deployment-history-with-azure-resource-manager"></a>Anzeigen des Bereitstellungsverlaufs mit Azure Resource Manager

Mit Azure Resource Manager können Sie Ihren Bereitstellungsverlauf anzeigen und bestimmte Vorgänge in früheren Bereitstellungen untersuchen. Ihnen werden die bereitgestellten Ressourcen angezeigt, und Sie erhalten Informationen zu etwaigen Fehlern.

Unterstützung beim Beheben bestimmter Bereitstellungsfehler finden Sie unter [Beheben von häufigen Fehlern beim Bereitstellen von Ressourcen in Azure mit Azure Resource Manager](common-deployment-errors.md).

## <a name="get-deployments-and-correlation-id"></a>Abrufen von Bereitstellungen und der Korrelations-ID

Sie können Details zu einer Bereitstellung über das Azure-Portal, die PowerShell, die Azure CLI oder die REST-API anzeigen. Jede Bereitstellung verfügt über eine Korrelations-ID, die zum Nachverfolgen verwandter Ereignisse verwendet wird. Sie kann hilfreich sein, wenn Sie bei der Behandlung von Problemen mit einer Bereitstellung mit dem technischen Support zusammenarbeiten.

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. Wählen Sie die zu untersuchende Ressourcengruppe aus.

1. Wählen Sie den Link unter **Bereitstellungen** aus.

   ![Auswählen des Bereitstellungsverlaufs](./media/deployment-history/select-deployment-history.png)

1. Wählen Sie eine der Bereitstellungen aus dem Bereitstellungsverlauf aus.

   ![Auswählen der Bereitstellung](./media/deployment-history/select-details.png)

1. Es wird eine Zusammenfassung der Bereitstellung angezeigt, einschließlich der Korrelations-ID.

    ![Zusammenfassung der Bereitstellungen](./media/deployment-history/show-correlation-id.png)

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Um alle Bereitstellungen für eine Ressourcengruppe aufzulisten, verwenden Sie den Befehl [Get-AzResourceGroupDeployment](/powershell/module/az.resources/Get-AzResourceGroupDeployment).

```azurepowershell-interactive
Get-AzResourceGroupDeployment -ResourceGroupName ExampleGroup
```

Um eine bestimmte Bereitstellung aus einer Ressourcengruppe abzurufen, fügen Sie den Parameter **DeploymentName** hinzu.

```azurepowershell-interactive
Get-AzResourceGroupDeployment -ResourceGroupName ExampleGroup -DeploymentName ExampleDeployment
```

Gehen Sie folgendermaßen vor, um die Korrelations-ID abzurufen:

```azurepowershell-interactive
(Get-AzResourceGroupDeployment -ResourceGroupName ExampleGroup -DeploymentName ExampleDeployment).CorrelationId
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Um die Bereitstellung für eine Ressourcengruppe aufzulisten, verwenden Sie [az deployment group list](/cli/azure/group/deployment?view=azure-cli-latest#az-deployment-group-list).

```azurecli-interactive
az deployment group list --resource-group ExampleGroup
```

Um eine bestimmte Bereitstellung abzurufen, verwenden Sie [az deployment group show](/cli/azure/group/deployment?view=azure-cli-latest#az-deployment-group-show).

```azurecli-interactive
az deployment group show --resource-group ExampleGroup --name ExampleDeployment
```

Gehen Sie folgendermaßen vor, um die Korrelations-ID abzurufen:

```azurecli-interactive
az deployment group show --resource-group ExampleGroup --name ExampleDeployment --query properties.correlationId
```

# <a name="http"></a>[HTTP](#tab/http)

Um die Bereitstellung für eine Ressourcengruppe aufzulisten, verwenden Sie den folgenden Vorgang. Informationen zur neuesten API-Versionsnummer, die in der Anforderung verwendet werden soll, finden Sie unter [Bereitstellungen – Liste nach Ressourcengruppe](/rest/api/resources/deployments/listbyresourcegroup).

```
GET https://management.azure.com/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.Resources/deployments/?api-version={api-version}
```

Um eine bestimmte Bereitstellung abzurufen, verwenden Sie den folgenden Vorgang. Informationen zur neuesten API-Versionsnummer, die in der Anforderung verwendet werden soll, finden Sie unter [Bereitstellungen – Abrufen](/rest/api/resources/deployments/get).

```
GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}?api-version={api-version}
```

Die Antwort enthält die Korrelations-ID.

```json
{
 ...
 "properties": {
   "mode": "Incremental",
   "provisioningState": "Failed",
   "timestamp": "2019-11-26T14:18:36.4518358Z",
   "duration": "PT26.2091817S",
   "correlationId": "47ff4228-bf2e-4ee5-a008-0b07da681230",
   ...
 }
}
```

---

## <a name="get-deployment-operations-and-error-message"></a>Vorgänge zum Abrufen von Bereitstellungen und Fehlermeldungen

Jede Bereitstellung kann mehrere Vorgänge umfassen. Um weiter Informationen zu einer Bereitstellung anzuzeigen, zeigen Sie die Bereitstellungsvorgänge an. Beim Fehlschlagen einer Bereitstellung enthalten die Bereitstellungsvorgänge eine Fehlermeldung.

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. Wählen Sie in der Zusammenfassung für eine Bereitstellung **Vorgangsdetails** aus.

    ![Auswählen von Bereitstellungsvorgängen](./media/deployment-history/get-operation-details.png)

1. Es werden die ausführlichen Informationen zu dem Bereitstellungsschritt angezeigt. Wenn ein Fehler auftritt, enthalten die Details die Fehlermeldung.

    ![Anzeigen von Vorgangsdetails](./media/deployment-history/see-operation-details.png)

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Verwenden Sie zum Anzeigen der Bereitstellungsvorgänge für die Bereitstellung in einer Ressourcengruppe den Befehl [Get-AzResourceGroupDeploymentOperation](/powershell/module/az.resources/get-azdeploymentoperation).

```azurepowershell-interactive
Get-AzResourceGroupDeploymentOperation -ResourceGroupName ExampleGroup -DeploymentName ExampleDeploy
```

Zum Anzeigen fehlerhafter Vorgänge filtern Sie Vorgänge mit dem Zustand **Fehler**.

```azurepowershell-interactive
(Get-AzResourceGroupDeploymentOperation -ResourceGroupName ExampleGroup -DeploymentName ExampleDeploy).Properties | Where-Object ProvisioningState -eq Failed
```

Verwenden Sie den folgenden Befehl, um die Statusmeldung für einen fehlerhaften Vorgang abzurufen:

```azurepowershell-interactive
((Get-AzResourceGroupDeploymentOperation -ResourceGroupName ExampleGroup -DeploymentName ExampleDeploy ).Properties | Where-Object ProvisioningState -eq Failed).StatusMessage.error
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Verwenden Sie zum Anzeigen der Bereitstellungsvorgänge für die Bereitstellung in einer Ressourcengruppe den Befehl [az deployment group operation list](/cli/azure/group/deployment/operation?view=azure-cli-latest#az-deployment-group-operation-list).

```azurecli-interactive
az deployment group operation list --resource-group ExampleGroup --name ExampleDeployment
```

Zum Anzeigen fehlerhafter Vorgänge filtern Sie Vorgänge mit dem Zustand **Fehler**.

```azurecli-interactive
az deployment group operation list --resource-group ExampleGroup --name ExampleDeploy --query "[?properties.provisioningState=='Failed']"
```

Verwenden Sie den folgenden Befehl, um die Statusmeldung für einen fehlerhaften Vorgang abzurufen:

```azurecli-interactive
az deployment group operation list --resource-group ExampleGroup --name ExampleDeploy --query "[?properties.provisioningState=='Failed'].properties.statusMessage.error"
```

# <a name="http"></a>[HTTP](#tab/http)

Um Bereitstellungsvorgänge abzurufen, verwenden Sie den folgenden Vorgang. Informationen zur neuesten API-Versionsnummer, die in der Anforderung verwendet werden soll, finden Sie unter [Bereitstellungsvorgänge – Auflisten](/rest/api/resources/deploymentoperations/list).

```
GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}/operations?$skiptoken={skiptoken}&api-version={api-version}
```

Die Antwort enthält eine Fehlermeldung.

```json
{
  "value": [
    {
      "id": "/subscriptions/xxxx/resourceGroups/examplegroup/providers/Microsoft.Resources/deployments/exampledeploy/operations/13EFD9907103D640",
      "operationId": "13EFD9907103D640",
      "properties": {
        "provisioningOperation": "Create",
        "provisioningState": "Failed",
        "timestamp": "2019-11-26T14:18:36.3177613Z",
        "duration": "PT21.0580179S",
        "trackingId": "9d3cdac4-54f8-486c-94bd-10c20867b8bc",
        "serviceRequestId": "01a9d0fe-896b-4c94-a30f-60b70a8f1ad9",
        "statusCode": "BadRequest",
        "statusMessage": {
          "error": {
            "code": "InvalidAccountType",
            "message": "The AccountType Standard_LRS1 is invalid. For more information, see - https://aka.ms/storageaccountskus"
          }
        },
        "targetResource": {
          "id": "/subscriptions/xxxx/resourceGroups/examplegroup/providers/Microsoft.Storage/storageAccounts/storageq2czadzfgizc2",
          "resourceType": "Microsoft.Storage/storageAccounts",
          "resourceName": "storageq2czadzfgizc2"
        }
      }
    },
    ...
  ]
}
```

---

## <a name="next-steps"></a>Nächste Schritte

* Unterstützung beim Beheben bestimmter Bereitstellungsfehler finden Sie unter [Beheben von häufigen Fehlern beim Bereitstellen von Ressourcen in Azure mit Azure Resource Manager](common-deployment-errors.md).
* Weitere Informationen zur Verwendung der Aktivitätsprotokolle für die Überwachung anderer Arten von Aktionen finden Sie unter [Anzeigen von Aktivitätsprotokolle zum Verwalten von Azure-Ressourcen](../management/view-activity-logs.md).
* Informationen zum Überprüfen der Bereitstellung vor der Ausführung finden Sie unter [Bereitstellen einer Ressourcengruppe mit Azure Resource Manager-Vorlagen](deploy-powershell.md).

