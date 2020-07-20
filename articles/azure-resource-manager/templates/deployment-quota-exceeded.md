---
title: Überschreitung des Bereitstellungskontingents
description: Beschreibt die Behebung des Fehlers, dass im Verlauf der Ressourcengruppe mehr als 800 Bereitstellungen vorkommen.
ms.topic: troubleshooting
ms.date: 06/25/2020
ms.openlocfilehash: 1b0c3de6007964b487a13e71cd43bd984cd970f1
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85391178"
---
# <a name="resolve-error-when-deployment-count-exceeds-800"></a>Beheben des Fehlers, dass die Anzahl der Bereitstellungen 800 überschreitet

Jede Ressourcengruppe ist in ihrem Bereitstellungsverlauf auf 800 Bereitstellungen beschränkt. Dieser Artikel beschreibt den Fehler, den Sie erhalten, wenn bei einer Bereitstellung ein Fehler auftritt, da dadurch die zulässigen 800 Bereitstellungen überschritten würden. Um diesen Fehler zu beheben, löschen Sie Bereitstellungen aus dem Verlauf der Ressourcengruppe. Das Löschen einer Bereitstellung aus dem Verlauf hat keinerlei Auswirkungen auf die bereitgestellten Ressourcen.

> [!NOTE]
> Azure Resource Manager beginnt bald mit dem automatischen Löschen von Bereitstellungen aus Ihrem Verlauf, wenn Sie sich dem Grenzwert nähern. Dieser Fehler wird möglicherweise weiterhin angezeigt, wenn Sie automatische Löschungen deaktiviert haben. Weitere Informationen finden Sie unter [Automatische Löschungen aus dem Bereitstellungsverlauf](deployment-history-deletions.md).

## <a name="symptom"></a>Symptom

Während einer Bereitstellung erhalten Sie einen Fehler, dass die aktuelle Bereitstellung das Kontingent von 800 Bereitstellungen überschreitet.

## <a name="solution"></a>Lösung

### <a name="azure-cli"></a>Azure CLI

Verwenden Sie den Befehl [az deployment group delete](/cli/azure/group/deployment), um Bereitstellungen aus dem Verlauf zu löschen.

```azurecli-interactive
az deployment group delete --resource-group exampleGroup --name deploymentName
```

Verwenden Sie Folgendes, um alle Bereitstellungen zu löschen, die älter als fünf Tage sind:

```azurecli-interactive
startdate=$(date +%F -d "-5days")
deployments=$(az deployment group list --resource-group exampleGroup --query "[?properties.timestamp<'$startdate'].name" --output tsv)

for deployment in $deployments
do
  az deployment group delete --resource-group exampleGroup --name $deployment
done
```

Sie können die aktuelle Anzahl im Bereitstellungsverlauf mit dem folgenden Befehl abrufen:

```azurecli-interactive
az deployment group list --resource-group exampleGroup --query "length(@)"
```

### <a name="azure-powershell"></a>Azure PowerShell

Verwenden Sie den Befehl [Remove-AzResourceGroupDeployment](/powershell/module/az.resources/remove-azresourcegroupdeployment), um Bereitstellungen aus dem Verlauf zu löschen.

```azurepowershell-interactive
Remove-AzResourceGroupDeployment -ResourceGroupName exampleGroup -Name deploymentName
```

Verwenden Sie Folgendes, um alle Bereitstellungen zu löschen, die älter als fünf Tage sind:

```azurepowershell-interactive
$deployments = Get-AzResourceGroupDeployment -ResourceGroupName exampleGroup | Where-Object Timestamp -lt ((Get-Date).AddDays(-5))

foreach ($deployment in $deployments) {
  Remove-AzResourceGroupDeployment -ResourceGroupName exampleGroup -Name $deployment.DeploymentName
}
```

Sie können die aktuelle Anzahl im Bereitstellungsverlauf mit dem folgenden Befehl abrufen:

```azurepowershell-interactive
(Get-AzResourceGroupDeployment -ResourceGroupName exampleGroup).Count
```

## <a name="third-party-solutions"></a>Lösungen von Drittanbietern

Die folgenden externen Lösungen berücksichtigen spezifische Szenarien:

* [Azure Logic-Apps und PowerShell-Lösungen](https://devkimchi.com/2018/05/30/managing-excessive-arm-deployment-histories-with-logic-apps/)
* [AzDevOps-Erweiterung](https://github.com/christianwaha/AzureDevOpsExtensionCleanRG)
