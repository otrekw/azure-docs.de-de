---
title: Benutzerdefinierte Funktionen in Vorlagen
description: Beschreibt, wie benutzerdefinierte Funktionen in einer Azure Resource Manager-Vorlage (ARM-Vorlage) definiert und verwendet werden.
ms.topic: conceptual
ms.date: 04/12/2021
ms.openlocfilehash: 802ac6e1c70db8caa58aecde3efe814430c79682
ms.sourcegitcommit: b4fbb7a6a0aa93656e8dd29979786069eca567dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2021
ms.locfileid: "107307774"
---
# <a name="user-defined-functions-in-arm-template"></a>Benutzerdefinierte Funktionen in ARM-Vorlagen

In Ihrer Vorlage können Sie Ihre eigenen Funktionen erstellen. Diese Funktionen stehen dann zur Verwendung in der Vorlage zur Verfügung. Benutzerdefinierte Funktionen sind von den [standardmäßigen Vorlagenfunktionen](template-functions.md) getrennt, die automatisch in der Vorlage verfügbar sind. Erstellen Sie eigene Funktionen, wenn Sie über komplizierte Ausdrücke verfügen, die in Ihrer Vorlage wiederholt verwendet werden.

Dieser Artikel beschreibt, wie benutzerdefinierte Funktionen in einer Azure Resource Manager-Vorlage (ARM-Vorlage) hinzugefügt werden.

## <a name="define-the-function"></a>Definieren der Funktion

Für benutzerdefinierte Funktionen müssen Namespacewerte verwendet werden, um Namenskonflikte mit Vorlagenfunktionen zu vermeiden. Im folgenden Beispiel wird eine Funktion veranschaulicht, die einen eindeutigen Namen zurückgibt:

```json
"functions": [
  {
    "namespace": "contoso",
    "members": {
      "uniqueName": {
        "parameters": [
          {
            "name": "namePrefix",
            "type": "string"
          }
        ],
        "output": {
          "type": "string",
          "value": "[concat(toLower(parameters('namePrefix')), uniqueString(resourceGroup().id))]"
        }
      }
    }
  }
],
```

## <a name="use-the-function"></a>Verwenden der Funktion

Das folgende Beispiel zeigt eine Vorlage, die eine benutzerdefinierte Funktion enthält, um einen eindeutigen Namen für ein Speicherkonto abzurufen. Die Vorlage weist einen Parameter namens `storageNamePrefix` auf, der als Parameter an die Funktion übergeben wird.

```json
{
 "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
 "contentVersion": "1.0.0.0",
 "parameters": {
   "storageNamePrefix": {
     "type": "string",
     "maxLength": 11
   }
 },
 "functions": [
  {
    "namespace": "contoso",
    "members": {
      "uniqueName": {
        "parameters": [
          {
            "name": "namePrefix",
            "type": "string"
          }
        ],
        "output": {
          "type": "string",
          "value": "[concat(toLower(parameters('namePrefix')), uniqueString(resourceGroup().id))]"
        }
      }
    }
  }
],
 "resources": [
   {
     "type": "Microsoft.Storage/storageAccounts",
     "apiVersion": "2019-04-01",
     "name": "[contoso.uniqueName(parameters('storageNamePrefix'))]",
     "location": "South Central US",
     "sku": {
       "name": "Standard_LRS"
     },
     "kind": "StorageV2",
     "properties": {
       "supportsHttpsTrafficOnly": true
     }
   }
 ]
}
```

Während der Bereitstellung wird der Parameter `storageNamePrefix` an die-Funktion übergeben:

* Die Vorlage definiert einen Parameter namens `storageNamePrefix`.
* Die Funktion verwendet `namePrefix`, weil Sie nur Parameter verwenden können, die in der Funktion definiert sind. Weitere Informationen finden Sie unter [Einschränkungen](#limitations).
* Im Abschnitt `resources` der Vorlage verwendet das- `name`-Element die Funktion und übergibt den `storageNamePrefix`-Wert an das `namePrefix` der Funktion.

## <a name="limitations"></a>Einschränkungen

Beim Definieren einer benutzerdefinierten Funktion gelten einige Einschränkungen:

* Die Funktion kann nicht auf Variablen zugreifen.
* Die Funktion kann nur Parameter verwenden, die in der Funktion definiert sind. Bei Verwendung der [parameters](template-functions-deployment.md#parameters)-Funktion innerhalb einer benutzerdefinierten Funktion sind Sie auf die Parameter für diese Funktion beschränkt.
* Die Funktion kann keine anderen benutzerdefinierten Funktionen aufrufen.
* Die Funktion kann nicht die [reference](template-functions-resource.md#reference)-Funktion oder eine der [list](template-functions-resource.md#list)-Funktionen verwenden.
* Die Funktion kann nicht die [dateTimeAdd](template-functions-date.md#datetimeadd)-Funktion verwenden.
* Für die Parameter der Funktion können keine Standardwerte verwendet werden.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu den verfügbaren Eigenschaften für benutzerdefinierte Funktionen finden Sie unter [Verstehen der Struktur und Syntax von ARM-Vorlagen](template-syntax.md).
* Eine Liste der verfügbaren Funktionen in einer Vorlage finden Sie unter [Funktionen von ARM-Vorlagen](template-functions.md).
