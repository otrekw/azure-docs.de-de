---
title: Benutzerdefinierte Funktionen in Vorlagen
description: Beschreibt, wie benutzerdefinierte Funktionen in einer Azure Resource Manager-Vorlage definiert und verwendet werden.
ms.topic: conceptual
ms.date: 09/05/2019
ms.openlocfilehash: 58b9ba7b162736329cf775e2be5a47bfcae0a4ca
ms.sourcegitcommit: 5bbe87cf121bf99184cc9840c7a07385f0d128ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2020
ms.locfileid: "76122473"
---
# <a name="user-defined-functions-in-azure-resource-manager-template"></a>Benutzerdefinierte Funktionen in einer Azure Resource Manager-Vorlage

In Ihrer Vorlage können Sie Ihre eigenen Funktionen erstellen. Diese Funktionen stehen dann zur Verwendung in der Vorlage zur Verfügung. Benutzerdefinierte Funktionen sind von den [standardmäßigen Vorlagenfunktionen](template-functions.md) getrennt, die automatisch in der Vorlage verfügbar sind. Erstellen Sie eigene Funktionen, wenn Sie über komplizierte Ausdrücke verfügen, die in Ihrer Vorlage wiederholt verwendet werden.

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
    "apiVersion": "2016-01-01",
    "type": "Microsoft.Storage/storageAccounts",
    "location": "South Central US",
    "tags": {},
    "sku": {
      "name": "Standard_LRS"
    },
    "kind": "Storage",
    "properties": {}
  }
]
```

## <a name="limitations"></a>Einschränkungen

Beim Definieren einer benutzerdefinierten Funktion gelten einige Einschränkungen:

* Die Funktion kann nicht auf Variablen zugreifen.
* Die Funktion kann nur Parameter verwenden, die in der Funktion definiert sind. Bei Verwendung der [parameters](template-functions-deployment.md#parameters)-Funktion innerhalb einer benutzerdefinierten Funktion sind Sie auf die Parameter für diese Funktion beschränkt.
* Die Funktion kann keine anderen benutzerdefinierten Funktionen aufrufen.
* Die Funktion kann nicht die [reference](template-functions-resource.md#reference)-Funktion oder eine der [list](template-functions-resource.md#list)-Funktionen verwenden.
* Für die Parameter der Funktion können keine Standardwerte verwendet werden.


## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu den verfügbaren Eigenschaften für benutzerdefinierte Funktionen finden Sie unter [Verstehen der Struktur und Syntax von Azure Resource Manager-Vorlagen](template-syntax.md).
* Unter [Vorlagenfunktionen in Azure Resource Manager](template-functions.md) finden Sie eine Liste der verfügbaren Vorlagenfunktionen.
