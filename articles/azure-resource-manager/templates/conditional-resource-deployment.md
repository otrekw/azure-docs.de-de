---
title: Bedingte Bereitstellung mit Vorlagen
description: Beschreibt, wie eine Ressource in einer Azure Resource Manager-Vorlage (ARM-Vorlage) bedingt bereitgestellt werden kann.
ms.topic: conceptual
ms.date: 03/02/2021
ms.openlocfilehash: 409d258d7dfe3ed186e5cf97cc0dbe6dc149b849
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101741173"
---
# <a name="conditional-deployment-in-arm-templates"></a>Bedingte Bereitstellung in ARM-Vorlagen

Manchmal müssen Sie eine Ressource optional in einer Azure Resource Manager-Vorlage (ARM-Vorlage) oder Bicep-Datei bereitstellen. Verwenden Sie für JSON-Vorlagen das `condition`-Element, um anzugeben, ob die Ressource bereitgestellt wird. Verwenden Sie für Bicep das `if`-Schlüsselwort, um anzugeben, ob die Ressource bereitgestellt wird. Der Wert für die Bedingung wird in „true“ oder „false“ aufgelöst. Wenn der Wert TRUE ist, wird die Ressource erstellt. Wenn der Wert FALSE ist, wird die Ressource nicht erstellt. Der Wert kann nur auf die gesamte Ressource angewandt werden.

> [!NOTE]
> Die bedingte Bereitstellung wird nicht an [untergeordnete Ressourcen](child-resource-name-type.md) weitergegeben. Wenn Sie eine Ressource und ihre untergeordneten Ressourcen bedingt bereitstellen möchten, müssen Sie dieselbe Bedingung auf jeden Ressourcentyp anwenden.

## <a name="deploy-condition"></a>Bereitstellungsbedingung

Sie können einen Parameterwert übergeben, der angibt, ob eine Ressource bereitgestellt wird. Das folgende Beispiel stellt bedingt eine DNS-Zone bereit:

# <a name="json"></a>[JSON](#tab/json)

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "deployZone": {
      "type": "bool"
    }
  },
  "functions": [],
  "resources": [
    {
      "condition": "[parameters('deployZone')]",
      "type": "Microsoft.Network/dnsZones",
      "apiVersion": "2018-05-01",
      "name": "myZone",
      "location": "global"
    }
  ]
}
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
param deployZone bool

resource dnsZone 'Microsoft.Network/dnszones@2018-05-01' = if (deployZone) {
  name: 'myZone'
  location: 'global'
}
```

---

Ein komplexeres Beispiel finden Sie unter [Logischer Azure SQL-Server](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-logical-server).

## <a name="new-or-existing-resource"></a>Neue oder vorhandene Ressource

Sie können bedingte Bereitstellung verwenden, um eine neue Ressource zu erstellen oder eine vorhandene zu verwenden. Im folgenden Beispiel wird gezeigt, wie ein neues Speicherkonto bereitgestellt oder ein vorhandenes Speicherkonto verwendet wird.

# <a name="json"></a>[JSON](#tab/json)

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccountName": {
      "type": "string"
    },
    "location": {
      "type": "string",
      "defaultValue": "[resourceGroup().location]"
    },
    "newOrExisting": {
      "type": "string",
      "defaultValue": "new",
      "allowedValues": [
        "new",
        "existing"
      ]
    }
  },
  "functions": [],
  "resources": [
    {
      "condition": "[equals(parameters('newOrExisting'), 'new')]",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2019-06-01",
      "name": "[parameters('storageAccountName')]",
      "location": "[parameters('location')]",
      "sku": {
        "name": "Standard_LRS",
        "tier": "Standard"
      },
      "kind": "StorageV2",
      "properties": {
        "accessTier": "Hot"
      }
    }
  ]
}
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
param storageAccountName string
param location string = resourceGroup().location

@allowed([
  'new'
  'existing'
])
param newOrExisting string = 'new'

resource sa 'Microsoft.Storage/storageAccounts@2019-06-01' = if (newOrExisting == 'new') {
  name: storageAccountName
  location: location
  sku: {
    name: 'Standard_LRS'
    tier: 'Standard'
  }
  kind: 'StorageV2'
  properties: {
    accessTier: 'Hot'
  }
}
```

---

Wenn der Parameter `newOrExisting` auf **new** festgelegt ist, wird die Bedingung zu „true“ ausgewertet. Das Speicherkonto wird bereitgestellt. Ist `newOrExisting` dagegen auf **existing** festgelegt, wird die Bedingung zu „false“ ausgewertet, und das Speicherkonto wird nicht bereitgestellt.

Eine vollständige Beispielvorlage, die das `condition`-Element verwendet, finden Sie unter [VM mit einem neuen oder vorhandenen virtuellen Netzwerk, Speicher und einer neuen oder vorhandenen öffentlichen IP-Adresse](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-new-or-existing-conditions).

## <a name="runtime-functions"></a>Laufzeitfunktionen

Bei Verwendung einer Funktion vom Typ [reference](template-functions-resource.md#reference) oder [list](template-functions-resource.md#list) mit einer Ressource, die bedingt bereitgestellt wird, wird die Funktion auch dann ausgewertet, wenn die Ressource nicht bereitgestellt wird. Es wird eine Fehlermeldung angezeigt, wenn die Funktion auf eine nicht vorhandene Ressource verweist.

Verwenden Sie die [if](template-functions-logical.md#if)-Funktion, um sicherzustellen, dass die Funktion nur für Bedingungen ausgewertet wird, wenn die Ressource bereitgestellt wird. Eine Beispielvorlage, die `if` und `reference` mit einer bedingt bereitgestellten Ressource verwendet, finden Sie unter der [if](template-functions-logical.md#if)-Funktion.

Sie legen eine [Ressource als abhängig](define-resource-dependency.md) von einer bedingten Ressource fest, wie Sie auch jede andere Ressource festlegen. Wenn eine bedingte Ressource nicht bereitgestellt wurde, entfernt Azure Resource Manager sie automatisch aus den erforderlichen Abhängigkeiten.

## <a name="complete-mode"></a>Vollständiger Modus

Wenn Sie eine Vorlage mit [vollständigem Modus](deployment-modes.md) bereitstellen und eine Ressource nicht bereitgestellt wird, weil `condition` in FALSE ausgewertet wird, hängt das Ergebnis davon ab, welche REST-API-Version Sie zum Bereitstellen der Vorlage verwenden. Wenn Sie eine frühere Version als 2019-05-10 verwenden, wird die Ressource **nicht gelöscht**. Bei Version 2019-05-10 oder höher wird die Ressource **gelöscht**. Die neuesten Versionen von Azure PowerShell und Azure CLI löschen die Ressource, wenn die Bedingung nicht zutrifft.

## <a name="next-steps"></a>Nächste Schritte

* Ein Microsoft Learn-Modul, das die bedingte Bereitstellung behandelt, finden Sie unter [Verwalten komplexer Cloudbereitstellungen mithilfe erweiterter ARM-Vorlagenfunktionen](/learn/modules/manage-deployments-advanced-arm-template-features/).
* Empfehlungen zum Erstellen von Vorlagen finden Sie unter [Bewährte Methoden für ARM-Vorlagen](template-best-practices.md).
* Informationen, wie mehrere Instanzen einer Ressource erstellt werden, finden Sie unter [Ressourceniteration in ARM-Vorlagen](copy-resources.md).
