---
title: Bereitstellen von Ressourcen über die Abonnement- und Ressourcengruppe
description: Hier wird gezeigt, wie während der Bereitstellung mehrere Azure-Abonnements und -Ressourcengruppen als Ziel festgelegt werden.
ms.topic: conceptual
ms.date: 05/18/2020
ms.openlocfilehash: 34de1d9df53d61d849ffbb81a57b468020bc3b65
ms.sourcegitcommit: bcb962e74ee5302d0b9242b1ee006f769a94cfb8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/07/2020
ms.locfileid: "86057380"
---
# <a name="deploy-azure-resources-across-subscriptions-or-resource-groups"></a>Bereitstellen von Azure-Ressourcen in Abonnements oder Ressourcengruppen

Resource Manager ermöglicht es Ihnen, in einer einzelnen Bereitstellung in mehr als einer Ressourcengruppe bereitzustellen. Mithilfe geschachtelter Vorlagen können Sie Ressourcengruppen angeben, die sich von der Ressourcengruppe im Bereitstellungsvorgang unterscheiden. Die Ressourcengruppen können sich in unterschiedlichen Abonnements befinden.

> [!NOTE]
> Sie können mit einer einzigen Bereitstellung in **800 Ressourcengruppen** bereitstellen. Normalerweise bedeutet diese Einschränkung, dass Sie die Bereitstellung für eine Ressourcengruppe durchführen können, die für die übergeordnete Vorlage angegeben ist, sowie für bis zu 799 Ressourcengruppen in geschachtelten oder verknüpften Bereitstellungen. Wenn Ihre übergeordnete Vorlage aber nur geschachtelte oder verknüpfte Vorlagen enthält und selbst keine Ressourcen bereitstellt, können Sie bis zu 800 Ressourcengruppen in geschachtelte oder verknüpfte Bereitstellungen einbinden.

## <a name="specify-subscription-and-resource-group"></a>Angeben von Abonnement und Ressourcengruppe

Zum Festlegen einer Ressourcengruppe, die nicht die der übergeordneten Vorlage ist, verwenden Sie eine [geschachtelte oder verknüpfte Vorlage](linked-templates.md). Geben Sie innerhalb des Bereitstellungsressourcentyps Werte für die Abonnement-ID und die Ressourcengruppe an, für die Sie die geschachtelte Vorlage bereitstellen möchten.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/crosssubscription.json" range="38-43" highlight="5-6":::

Ohne Angabe von Abonnement-ID oder Ressourcengruppe werden das Abonnement und die Ressourcengruppe aus der übergeordneten Vorlage verwendet. Alle Ressourcengruppen müssen vorhanden sein, bevor Sie die Bereitstellung ausführen.

Das zum Bereitstellen der Vorlage verwendete Konto muss über Berechtigungen für die Bereitstellung unter der angegebenen Abonnement-ID verfügen. Wenn das angegebene Abonnement in einem anderen Azure Active Directory-Mandanten vorhanden ist, müssen Sie [Gastbenutzer aus einem anderen Verzeichnis hinzufügen](../../active-directory/b2b/what-is-b2b.md).

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

## <a name="use-functions"></a>Verwenden von Funktionen

Die Funktionen [resouceGroup()](template-functions-resource.md#resourcegroup) und [subscription()](template-functions-resource.md#subscription) werden unterschiedlich aufgelöst, je nachdem, wie Sie die Vorlage festlegen. Wenn Sie auf eine externe Vorlage verweisen, werden die Funktionen immer im Bereich dieser Vorlage aufgelöst. Wenn Sie eine Vorlage in einer übergeordneten Vorlage schachteln, geben Sie mit der Eigenschaft `expressionEvaluationOptions` an, ob die Funktionen in die Ressourcengruppe und das Abonnement für die übergeordnete Vorlage oder die geschachtelte Vorlage aufgelöst werden. Legen Sie die Eigenschaft auf `inner` fest, um in den Bereich für die geschachtelte Vorlage aufzulösen. Legen Sie die Eigenschaft auf `outer` fest, um in den Bereich für die übergeordnete Vorlage aufzulösen.

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
