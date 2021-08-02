---
title: Ausgaben in Vorlagen
description: Beschreibt, wie Ausgabewerte in einer Azure Resource Manager-Vorlage (ARM-Vorlage) definiert werden.
ms.topic: conceptual
ms.date: 05/18/2021
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: d81499ca7a85f8652ee353cde2e8e1cee48d4bf4
ms.sourcegitcommit: c072eefdba1fc1f582005cdd549218863d1e149e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111960340"
---
# <a name="outputs-in-arm-templates"></a>Ausgaben in ARM-Vorlagen

Dieser Artikel beschreibt, wie Sie Ausgabewerte in Ihrer Azure Resource Manager-Vorlage (ARM-Vorlage) definieren. Sie verwenden Ausgaben, wenn Werte von den bereitgestellten Ressourcen zurückgegeben werden müssen.

Das Format der einzelnen Ausgabewerte muss sich zu einem der [Datentypen](data-types.md) auflösen lassen.

## <a name="define-output-values"></a>Definieren von Ausgabewerten

Im folgenden Beispiel wird gezeigt, wie Sie eine Eigenschaft von einer bereitgestellten Ressource zurückgeben lassen.

Fügen Sie den Abschnitt `outputs` zur Vorlage hinzu. Der Ausgabewert erhält den vollqualifizierten Domänennamen für eine öffentliche IP-Adresse.

```json
"outputs": {
  "hostname": {
    "type": "string",
    "value": "[reference(resourceId('Microsoft.Network/publicIPAddresses', variables('publicIPAddressName'))).dnsSettings.fqdn]"
  },
}
```

Wenn Sie eine Eigenschaft ausgeben müssen, die einen Bindestrich im Namen aufweist, setzen Sie den Namen in eckige Klammern, anstatt die Punktnotation zu verwenden. Verwenden Sie beispielsweise `['property-name']` anstelle von `.property-name`.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "variables": {
    "user": {
      "user-name": "Test Person"
    }
  },
  "resources": [
  ],
  "outputs": {
    "nameResult": {
      "type": "string",
      "value": "[variables('user')['user-name']]"
    }
  }
}
```

## <a name="conditional-output"></a>Bedingte Ausgabe

Sie können das `condition`-Element verwenden, um einen Wert bedingt zurückzugeben. In der Regel verwenden Sie eine bedingte Ausgabe, wenn Sie eine Ressource [bedingt bereitgestellt](conditional-resource-deployment.md) haben. Das folgende Beispiel zeigt, wie die Ressourcen-ID für eine öffentliche IP-Adresse abhängig davon zurückgegeben wird, ob eine neue Ressourcen-ID bereitgestellt wurde:

```json
"outputs": {
  "resourceID": {
    "condition": "[equals(parameters('publicIpNewOrExisting'), 'new')]",
    "type": "string",
    "value": "[resourceId('Microsoft.Network/publicIPAddresses', parameters('publicIPAddresses_name'))]"
  }
}
```

Ein einfaches Beispiel der bedingten Ausgabe finden Sie unter der [Vorlage für die bedingte Ausgabe](https://github.com/bmoore-msft/AzureRM-Samples/blob/master/conditional-output/azuredeploy.json).

## <a name="dynamic-number-of-outputs"></a>Dynamische Anzahl von Ausgaben

In einigen Szenarien wissen Sie nicht, wie viele Instanzen eines Werts beim Erstellen der Vorlage zurückgegeben werden müssen. Sie können eine variable Anzahl von Werten zurückgeben, indem Sie eine iterative Ausgabe verwenden. Fügen Sie das `copy`-Element hinzu, um eine Ausgabe zu iterieren.

```json
"outputs": {
  "storageEndpoints": {
    "type": "array",
    "copy": {
      "count": "[parameters('storageCount')]",
      "input": "[reference(concat(copyIndex(), variables('baseName'))).primaryEndpoints.blob]"
    }
  }
}
```

Weitere Informationen finden Sie unter [Ausgabeiteration in ARM-Vorlagen](copy-outputs.md).

## <a name="linked-templates"></a>Verknüpfte Vorlagen

Sie können verwandte Vorlagen mithilfe [verknüpfter Vorlagen](linked-templates.md) bereitstellen. Mit der Funktion [reference](template-functions-resource.md#reference) in der übergeordneten Vorlage können Sie den Ausgabewert aus einer verknüpften Vorlage abrufen. Die Syntax in der übergeordneten Vorlage lautet wie folgt:

```json
"[reference('<deploymentName>').outputs.<propertyName>.value]"
```

Das folgende Beispiel zeigt, wie Sie die IP-Adresse auf einen Lastenausgleich festlegen, indem Sie einen Wert aus einer verknüpften Vorlage abrufen.

```json
"publicIPAddress": {
  "id": "[reference('linkedTemplate').outputs.resourceID.value]"
}
```

Wenn der Eigenschaftsname einen Bindestrich aufweist, setzen Sie den Namen in eckige Klammern, anstatt die Punktnotation zu verwenden.

```json
"publicIPAddress": {
  "id": "[reference('linkedTemplate').outputs['resource-ID'].value]"
}
```

Sie können die Funktion `reference` nicht im Ausgabeabschnitt einer [geschachtelten Vorlage](linked-templates.md#nested-template) verwenden. Um die Werte für eine bereitgestellte Ressource in einer geschachtelten Vorlage zurückzugeben, konvertieren Sie Ihre geschachtelte Vorlage in eine verknüpfte Vorlage.

Die [Öffentliche IP-Adressenvorlage](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/public-ip.json) erstellt eine öffentliche IP-Adresse und gibt die Ressourcen-ID aus. Die [Lastenausgleichsvorlage](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/public-ip-parentloadbalancer.json) verknüpft mit der vorherigen Vorlage. Sie verwendet beim Erstellen des Lastenausgleichs die Ressourcen-ID in der Ausgabe.

## <a name="example-template"></a>Beispielvorlage

Von der folgenden Vorlage werden keine Ressourcen bereitgestellt. Sie zeigt einige Möglichkeiten für die Rückgabe von Ausgaben unterschiedlicher Typen.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/outputs.json":::

## <a name="get-output-values"></a>Abrufen von Ausgabewerten

Bei erfolgreicher Bereitstellung werden die Ausgabewerte automatisch in den Ergebnissen der Bereitstellung zurückgegeben.

Sie können ein Skript verwenden, um Ausgabewerte aus dem Bereitstellungsverlauf abzurufen.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
(Get-AzResourceGroupDeployment `
  -ResourceGroupName <resource-group-name> `
  -Name <deployment-name>).Outputs.resourceID.value
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
az deployment group show \
  -g <resource-group-name> \
  -n <deployment-name> \
  --query properties.outputs.resourceID.value
```

---

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu den verfügbaren Eigenschaften für Ausgaben finden Sie unter [Verstehen der Struktur und Syntax von ARM-Vorlagen](./syntax.md).