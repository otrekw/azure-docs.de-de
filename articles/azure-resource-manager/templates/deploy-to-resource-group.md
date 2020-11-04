---
title: Bereitstellen von Ressourcen in Ressourcengruppen
description: In diesem Artikel wird beschrieben, wie Sie Ressourcen in einer Azure Resource Manager-Vorlage bereitstellen. Es wird gezeigt, wie Sie mehrere Ressourcengruppen als Ziel festlegen.
ms.topic: conceptual
ms.date: 10/26/2020
ms.openlocfilehash: fd211641d7fcc02a1db154053597497583b21ae5
ms.sourcegitcommit: 4cb89d880be26a2a4531fedcc59317471fe729cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2020
ms.locfileid: "92681302"
---
# <a name="resource-group-deployments-with-arm-templates"></a>Bereitstellung von Ressourcengruppen mit ARM-Vorlagen

In diesem Artikel wird beschrieben, wie Sie eine Bereitstellung auf eine Ressourcengruppe beschränken. Dabei verwenden Sie eine Azure Resource Manager-Vorlage (ARM-Vorlage) für die Bereitstellung. Außerdem wird erläutert, wie Sie den Bereich während des Bereitstellungsvorgangs über die Ressourcengruppe hinaus erweitern.

## <a name="supported-resources"></a>Unterstützte Ressourcen

Die meisten Ressourcen können in einer Ressourcengruppe bereitgestellt werden. Eine Liste der verfügbaren Ressourcen finden Sie unter [Definieren von Ressourcen in Azure Resource Manager-Vorlagen](/azure/templates).

## <a name="schema"></a>Schema

Verwenden Sie für Vorlagen das folgende Schema:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    ...
}
```

Verwenden Sie für Parameterdateien Folgendes:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
    ...
}
```

## <a name="deployment-commands"></a>Bereitstellungsbefehle

Verwenden Sie für die Bereitstellung in einer Ressourcengruppe die Ressourcengruppen-Bereitstellungsbefehle.

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Verwenden Sie für die Azure CLI [az deployment group create](/cli/azure/deployment/group#az_deployment_group_create). Das folgende Beispiel stellt eine Vorlage zum Erstellen einer Ressourcengruppe bereit:

```azurecli-interactive
az deployment group create \
  --name demoRGDeployment \
  --resource-group ExampleGroup \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json" \
  --parameters storageAccountType=Standard_GRS
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Verwenden Sie als Bereitstellungsbefehl in PowerShell [New-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment). Das folgende Beispiel stellt eine Vorlage zum Erstellen einer Ressourcengruppe bereit:

```azurepowershell-interactive
New-AzResourceGroupDeployment `
  -Name demoRGDeployment `
  -ResourceGroupName ExampleGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json `
  -storageAccountType Standard_GRS `
```

---

Ausführlichere Informationen über Bereitstellungsbefehle und -optionen für die Bereitstellung von ARM-Vorlagen finden Sie in den folgenden Artikeln:

* [Bereitstellen von Ressourcen mit ARM-Vorlagen und dem Azure-Portal](deploy-portal.md)
* [Bereitstellen von Ressourcen mit ARM-Vorlagen und der Azure CLI](deploy-cli.md)
* [Bereitstellen von Ressourcen mit ARM-Vorlagen und Azure PowerShell](deploy-powershell.md)
* [Bereitstellen von Ressourcen mit ARM-Vorlagen und der Resource Manager-REST-API](deploy-rest.md)
* [Verwenden einer Bereitstellungsschaltfläche zum Bereitstellen von Vorlagen aus einem GitHub-Repository](deploy-to-azure-button.md)
* [Bereitstellen von ARM-Vorlagen über Cloud Shell](deploy-cloud-shell.md)

## <a name="deployment-scopes"></a>Bereitstellungsbereiche

Bei der Bereitstellung in einer Ressourcengruppe können Sie Ressourcen an folgenden Orten bereitstellen:

* In der Zielressourcengruppe des Vorgangs
* In anderen Ressourcengruppen im selben Abonnement oder in anderen Abonnements
* [Erweiterungsressourcen](scope-extension-resources.md) können auf Ressourcen angewendet werden.

Der Benutzer, der die Vorlage bereitstellt, muss Zugriff auf den angegebenen Bereich besitzen.

In diesem Abschnitt wird das Festlegen verschiedener Bereiche veranschaulicht. Sie können diese verschiedenen Bereiche in einer Vorlage kombinieren.

### <a name="scope-to-target-resource-group"></a>Bereich: Zielressourcengruppe

Zum Bereitstellen von Ressourcen in der Zielressourcengruppe fügen Sie die Ressourcen zum Ressourcenabschnitt der Vorlage hinzu.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/scope/default-rg.json" highlight="5":::

### <a name="scope-to-resource-group-in-same-subscription"></a>Bereich: Ressourcengruppe im selben Abonnement

Zum Bereitstellen von Ressourcen in einer anderen Ressourcengruppe im selben Abonnement fügen Sie eine geschachtelte Bereitstellung hinzu und verwenden dabei die Eigenschaft `resourceGroup`. Ohne Angabe von Abonnement-ID oder Ressourcengruppe werden das Abonnement und die Ressourcengruppe aus der übergeordneten Vorlage verwendet. Alle Ressourcengruppen müssen vorhanden sein, bevor Sie die Bereitstellung ausführen.

In dem folgenden Beispiel verwendet die geschachtelte Bereitstellung eine Ressourcengruppe namens `demoResourceGroup` als Ziel.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/scope/same-sub-to-resource-group.json" highlight="9,13":::

### <a name="scope-to-resource-group-in-different-subscription"></a>Bereich: Ressourcengruppe in einem anderen Abonnement

Zum Bereitstellen von Ressourcen in einer Ressourcengruppe in einem anderen Abonnement fügen Sie eine geschachtelte Bereitstellung hinzu und verwenden dabei die Eigenschaften `subscriptionId` und `resourceGroup`. In dem folgenden Beispiel verwendet die geschachtelte Bereitstellung eine Ressourcengruppe namens `demoResourceGroup` als Ziel.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/scope/different-sub-to-resource-group.json" highlight="9,10,14":::

## <a name="cross-resource-groups"></a>Ressourcengruppenübergreifend

Sie können in einer einzelnen ARM-Vorlage Ressourcen in mehr als einer Ressourcengruppe bereitstellen. Zum Festlegen einer Ressourcengruppe, die nicht die der übergeordneten Vorlage ist, verwenden Sie eine [geschachtelte oder verknüpfte Vorlage](linked-templates.md). Geben Sie innerhalb des Bereitstellungsressourcentyps Werte für die Abonnement-ID und die Ressourcengruppe an, für die Sie die geschachtelte Vorlage bereitstellen möchten. Die Ressourcengruppen können sich in unterschiedlichen Abonnements befinden.

> [!NOTE]
> Sie können mit einer einzigen Bereitstellung in **800 Ressourcengruppen** bereitstellen. Normalerweise bedeutet diese Einschränkung, dass Sie die Bereitstellung für eine Ressourcengruppe durchführen können, die für die übergeordnete Vorlage angegeben ist, sowie für bis zu 799 Ressourcengruppen in geschachtelten oder verknüpften Bereitstellungen. Wenn Ihre übergeordnete Vorlage aber nur geschachtelte oder verknüpfte Vorlagen enthält und selbst keine Ressourcen bereitstellt, können Sie bis zu 800 Ressourcengruppen in geschachtelte oder verknüpfte Bereitstellungen einbinden.

Im folgenden Beispiel werden zwei Speicherkonten bereitgestellt. Das erste Speicherkonto wird in der Ressourcengruppe bereitgestellt, die Sie im Bereitstellungsvorgang angegeben haben. Das zweite Speicherkonto wird in der Ressourcengruppe bereitgestellt, die in den Parametern `secondResourceGroup` und `secondSubscriptionID` angegeben wurde:

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/crosssubscription.json":::

Wenn Sie für `resourceGroup` den Namen einer nicht vorhandenen Ressourcengruppe angeben, tritt bei der Bereitstellung ein Fehler auf.

Verwenden Sie PowerShell oder die Azure CLI, um die vorherige Vorlage zu testen und die Ergebnisse anzuzeigen.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Verwenden Sie zum Bereitstellen von zwei Speicherkonten in zwei Ressourcengruppen im **gleichen Abonnement** Folgendes:

```azurepowershell-interactive
$firstRG = "primarygroup"
$secondRG = "secondarygroup"

New-AzResourceGroup -Name $firstRG -Location southcentralus
New-AzResourceGroup -Name $secondRG -Location eastus

New-AzResourceGroupDeployment `
  -ResourceGroupName $firstRG `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crosssubscription.json `
  -storagePrefix storage `
  -secondResourceGroup $secondRG `
  -secondStorageLocation eastus
```

Verwenden Sie zum Bereitstellen von zwei Speicherkonten in **zwei Abonnements** Folgendes:

```azurepowershell-interactive
$firstRG = "primarygroup"
$secondRG = "secondarygroup"

$firstSub = "<first-subscription-id>"
$secondSub = "<second-subscription-id>"

Select-AzSubscription -Subscription $secondSub
New-AzResourceGroup -Name $secondRG -Location eastus

Select-AzSubscription -Subscription $firstSub
New-AzResourceGroup -Name $firstRG -Location southcentralus

New-AzResourceGroupDeployment `
  -ResourceGroupName $firstRG `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crosssubscription.json `
  -storagePrefix storage `
  -secondResourceGroup $secondRG `
  -secondStorageLocation eastus `
  -secondSubscriptionID $secondSub
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Verwenden Sie zum Bereitstellen von zwei Speicherkonten in zwei Ressourcengruppen im **gleichen Abonnement** Folgendes:

```azurecli-interactive
firstRG="primarygroup"
secondRG="secondarygroup"

az group create --name $firstRG --location southcentralus
az group create --name $secondRG --location eastus
az deployment group create \
  --name ExampleDeployment \
  --resource-group $firstRG \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crosssubscription.json \
  --parameters storagePrefix=tfstorage secondResourceGroup=$secondRG secondStorageLocation=eastus
```

Verwenden Sie zum Bereitstellen von zwei Speicherkonten in **zwei Abonnements** Folgendes:

```azurecli-interactive
firstRG="primarygroup"
secondRG="secondarygroup"

firstSub="<first-subscription-id>"
secondSub="<second-subscription-id>"

az account set --subscription $secondSub
az group create --name $secondRG --location eastus

az account set --subscription $firstSub
az group create --name $firstRG --location southcentralus

az deployment group create \
  --name ExampleDeployment \
  --resource-group $firstRG \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crosssubscription.json \
  --parameters storagePrefix=storage secondResourceGroup=$secondRG secondStorageLocation=eastus secondSubscriptionID=$secondSub
```

---

## <a name="next-steps"></a>Nächste Schritte

* Unter [deployASCwithWorkspaceSettings.json](https://github.com/krnese/AzureDeploy/blob/master/ARM/deployments/deployASCwithWorkspaceSettings.json) finden Sie ein Beispiel für die Bereitstellung von Arbeitsbereichseinstellungen für Azure Security Center.
