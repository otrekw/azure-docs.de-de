---
title: Benutzerdefinierte Funktionen in Vorlagen
description: Beschreibt, wie benutzerdefinierte Funktionen in einer Azure Resource Manager-Vorlage definiert und verwendet werden.
ms.topic: conceptual
ms.date: 09/05/2019
ms.openlocfilehash: 94fdf80ffc309645a4bc10109a5e8dd4450731e2
ms.sourcegitcommit: 5cfe977783f02cd045023a1645ac42b8d82223bd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/17/2019
ms.locfileid: "74143709"
---
# <a name="user-defined-functions-in-azure-resource-manager-template"></a>Benutzerdefinierte Funktionen in einer Azure Resource Manager-Vorlage

In Ihrer Vorlage können Sie Ihre eigenen Funktionen erstellen. Diese Funktionen stehen dann zur Verwendung in der Vorlage zur Verfügung. Benutzerdefinierte Funktionen sind von den [standardmäßigen Vorlagenfunktionen](resource-group-template-functions.md) getrennt, die automatisch in der Vorlage verfügbar sind. Erstellen Sie eigene Funktionen, wenn Sie über komplizierte Ausdrücke verfügen, die in Ihrer Vorlage wiederholt verwendet werden.

Dieser Artikel beschreibt, wie benutzerdefinierte Funktionen in einer Azure Resource Manager-Vorlage hinzugefügt werden.

## <a name="define-the-function"></a>Definieren der Funktion

Für benutzerdefinierte Funktionen müssen Namespacewerte verwendet werden, um Namenskonflikte mit Vorlagenfunktionen zu vermeiden. Im folgenden Beispiel wird eine Funktion veranschaulicht, die einen Speicherkontonamen zurückgibt:

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

Das folgende Beispiel zeigt, wie Sie die Funktion aufrufen.

```json
"resources": [
  {
    "name": "[contoso.uniqueName(parameters('storageNamePrefix'))]",
    "type": "Microsoft.Storage/storageAccounts",
    "apiVersion": "2016-01-01",
    "sku": {
      "name": "Standard_LRS"
    },
    "kind": "Storage",
    "location": "South Central US",
    "tags": {},
    "properties": {}
  }
]
```

## <a name="limitations"></a>Einschränkungen

Beim Definieren einer benutzerdefinierten Funktion gelten einige Einschränkungen:

* Die Funktion kann nicht auf Variablen zugreifen.
* Die Funktion kann nur Parameter verwenden, die in der Funktion definiert sind. Bei Verwendung der [parameters](resource-group-template-functions-deployment.md#parameters)-Funktion innerhalb einer benutzerdefinierten Funktion sind Sie auf die Parameter für diese Funktion beschränkt.
* Die Funktion kann keine anderen benutzerdefinierten Funktionen aufrufen.
* Die Funktion kann nicht die [reference](resource-group-template-functions-resource.md#reference)-Funktion oder eine der [list](resource-group-template-functions-resource.md#list)-Funktionen verwenden.
* Für die Parameter der Funktion können keine Standardwerte verwendet werden.


## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu den verfügbaren Eigenschaften für benutzerdefinierte Funktionen finden Sie unter [Verstehen der Struktur und Syntax von Azure Resource Manager-Vorlagen](resource-group-authoring-templates.md).
* Unter [Vorlagenfunktionen in Azure Resource Manager](resource-group-template-functions.md) finden Sie eine Liste der verfügbaren Vorlagenfunktionen.
