---
title: Bereichsübergreifende Bereitstellung von Ressourcen
description: Zeigt, wie Sie während einer Bereitstellung mehrere Bereiche als Ziel festlegen können. Bei dem Bereich kann es sich um einen Mandanten, Verwaltungsgruppen, Abonnements und Ressourcengruppen handeln.
ms.topic: conceptual
ms.date: 07/28/2020
ms.openlocfilehash: 6161401ac039551a814b595715f56df1ac62dd6c
ms.sourcegitcommit: d3c3f2ded72bfcf2f552e635dc4eb4010491eb75
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2020
ms.locfileid: "92558806"
---
# <a name="deploy-azure-resources-across-scopes"></a>Bereichsübergreifende Bereitstellung von Azure-Ressourcen

Mit Azure Resource Manager-Vorlagen (ARM-Vorlagen) können Sie mit einem einzigen Bereitstellungsvorgang eine Bereitstellung in mehreren Bereichen durchführen. Verfügbare Bereiche sind Mandanten, Verwaltungsgruppen, Abonnements und Ressourcengruppen. Sie können beispielsweise Ressourcen in einer Ressourcengruppe bereitstellen und dabei in derselben Vorlage Ressourcen in einer anderen Ressourcengruppe bereitstellen. Sie können auch Ressourcen in einer Verwaltungsgruppe bereitstellen und dabei gleichzeitig Ressourcen in einer Ressourcengruppe innerhalb dieser Verwaltungsgruppe bereitstellen.

Sie verwenden [verschachtelte oder verknüpfte Vorlagen](linked-templates.md), um Bereiche anzugeben, die sich von dem primären Bereich für den Bereitstellungsvorgang unterscheiden.

## <a name="available-scopes"></a>Verfügbare Bereiche

Der Bereich, den Sie für den Bereitstellungsvorgang verwenden, bestimmt, welche weiteren Bereiche verfügbar sind. Sie können eine Bereitstellung für [Mandanten](deploy-to-tenant.md), [Verwaltungsgruppen](deploy-to-management-group.md), [Abonnements](deploy-to-subscription.md) oder [Ressourcengruppen](deploy-powershell.md) vornehmen. Auf der primären Bereitstellungsebene können Sie keine höhere Hierarchieebene verwenden. Wenn Sie z. B. eine Bereitstellung für ein Abonnement vornehmen, können Sie keine höhere Ebene verwenden und Ressourcen für eine Verwaltungsgruppe bereitstellen. Sie können jedoch die Bereitstellung in der Verwaltungsgruppe ausführen und dann darunterliegende Ebenen für die Bereitstellung in einem Abonnement oder einer Ressourcengruppe verwenden.

Der Benutzer, der die Vorlage bereitstellt, muss für jeden Bereich über die erforderlichen Berechtigungen zum Erstellen von Ressourcen verfügen.

## <a name="cross-resource-groups"></a>Ressourcengruppenübergreifend

Zum Festlegen einer Ressourcengruppe, die nicht die der übergeordneten Vorlage ist, verwenden Sie eine [geschachtelte oder verknüpfte Vorlage](linked-templates.md). Geben Sie innerhalb des Bereitstellungsressourcentyps Werte für die Abonnement-ID und die Ressourcengruppe an, für die Sie die geschachtelte Vorlage bereitstellen möchten. Die Ressourcengruppen können sich in unterschiedlichen Abonnements befinden.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/crosssubscription.json" range="38-43" highlight="5-6":::

Ohne Angabe von Abonnement-ID oder Ressourcengruppe werden das Abonnement und die Ressourcengruppe aus der übergeordneten Vorlage verwendet. Alle Ressourcengruppen müssen vorhanden sein, bevor Sie die Bereitstellung ausführen.

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

## <a name="cross-subscription-management-group-and-tenant"></a>Abonnement-, verwaltungsgruppen- und mandantenübergreifend

Wenn Sie verschiedene Bereiche für die Bereitstellung auf der Ebene von Abonnements, Verwaltungsgruppen und Mandanten angeben, verwenden Sie – wie das Beispiel für Ressourcengruppen zeigt – verschachtelte Bereitstellungen. Die Eigenschaften, die Sie zum Angeben des Bereichs verwenden, können unterschiedlich sein. Diese Szenarios werden in den Artikeln zu den Bereitstellungsebenen behandelt. Weitere Informationen finden Sie unter

* [Erstellen von Ressourcengruppen und Ressourcen auf Abonnementebene](deploy-to-subscription.md)
* [Erstellen von Ressourcen auf Verwaltungsgruppenebene](deploy-to-management-group.md)
* [Erstellen von Ressourcen auf Mandantenebene](deploy-to-tenant.md)

## <a name="how-functions-resolve-in-scopes"></a>Auflösen von Funktionen in Bereichen

Bei der Bereitstellung in mehreren Bereichen werden die Funktionen [resouceGroup()](template-functions-resource.md#resourcegroup) und [subscription()](template-functions-resource.md#subscription) unterschiedlich aufgelöst, je nachdem, wie Sie die Vorlage festlegen. Wenn Sie auf eine externe Vorlage verweisen, werden die Funktionen immer im Bereich dieser Vorlage aufgelöst. Wenn Sie eine Vorlage in einer übergeordneten Vorlage schachteln, geben Sie mit der Eigenschaft `expressionEvaluationOptions` an, ob die Funktionen in die Ressourcengruppe und das Abonnement für die übergeordnete Vorlage oder die geschachtelte Vorlage aufgelöst werden. Legen Sie die Eigenschaft auf `inner` fest, um in den Bereich für die geschachtelte Vorlage aufzulösen. Legen Sie die Eigenschaft auf `outer` fest, um in den Bereich für die übergeordnete Vorlage aufzulösen.

Die folgende Tabelle zeigt, ob die Funktionen in die übergeordnete oder eingebettete Ressourcengruppe und das zugehörige Abonnement aufgelöst werden.

| Vorlagentyp | `Scope` | Lösung |
| ------------- | ----- | ---------- |
| Geschachtelt        | Außen (Standard) | Übergeordnete Ressourcengruppe |
| Geschachtelt        | Innerer Join | Unterressourcengruppe |
| Verknüpft        | –   | Unterressourcengruppe |

Die folgende [Beispielvorlage](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/crossresourcegroupproperties.json) zeigt Folgendes:

* eine geschachtelte Vorlage mit Standardbereich (außen)
* geschachtelte Vorlage mit innerem Bereich
* verknüpfte Vorlage

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/crossresourcegroupproperties.json":::

Verwenden Sie PowerShell oder die Azure CLI, um die vorherige Vorlage zu testen und die Ergebnisse anzuzeigen.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
New-AzResourceGroup -Name parentGroup -Location southcentralus
New-AzResourceGroup -Name inlineGroup -Location southcentralus
New-AzResourceGroup -Name linkedGroup -Location southcentralus

New-AzResourceGroupDeployment `
  -ResourceGroupName parentGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crossresourcegroupproperties.json
```

Die Ausgabe aus dem vorherigen Beispiel lautet wie folgt:

```output
 Name             Type                       Value
 ===============  =========================  ==========
 parentRG         String                     Parent resource group is parentGroup
 defaultScopeRG   String                     Default scope resource group is parentGroup
 innerScopeRG     String                     Inner scope resource group is inlineGroup
 linkedRG         String                     Linked resource group is linkedgroup
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
az group create --name parentGroup --location southcentralus
az group create --name inlineGroup --location southcentralus
az group create --name linkedGroup --location southcentralus

az deployment group create \
  --name ExampleDeployment \
  --resource-group parentGroup \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crossresourcegroupproperties.json
```

Die Ausgabe aus dem vorherigen Beispiel lautet wie folgt:

```output
"outputs": {
  "defaultScopeRG": {
    "type": "String",
    "value": "Default scope resource group is parentGroup"
  },
  "innerScopeRG": {
    "type": "String",
    "value": "Inner scope resource group is inlineGroup"
  },
  "linkedRG": {
    "type": "String",
    "value": "Linked resource group is linkedGroup"
  },
  "parentRG": {
    "type": "String",
    "value": "Parent resource group is parentGroup"
  }
},
```

---



## <a name="next-steps"></a>Nächste Schritte

* Um zu verstehen, wie Parameter in der Vorlage definiert werden, lesen Sie [Verstehen der Struktur und Syntax von Azure Resource Manager-Vorlagen](template-syntax.md).
* Tipps zum Beheben gängiger Azure-Bereitstellungsfehler finden Sie unter [Beheben gängiger Azure-Bereitstellungsfehler mit Azure Resource Manager](common-deployment-errors.md).
* Informationen zum Bereitstellen einer Vorlage, die ein SAS-Token erfordert, finden Sie unter [Bereitstellen einer privaten Vorlage mit SAS-Token](secure-template-with-sas-token.md).
