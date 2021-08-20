---
title: Datentypen in Vorlagen
description: Beschreibt die Datentypen, die in Azure Resource Manager-Vorlagen verfügbar sind.
ms.topic: conceptual
ms.author: tomfitz
author: tfitzmac
ms.date: 06/24/2021
ms.openlocfilehash: 4fc69126ee1f555e71e152a7c0369e4b81b8bf6a
ms.sourcegitcommit: 5be51a11c63f21e8d9a4d70663303104253ef19a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/25/2021
ms.locfileid: "112894228"
---
# <a name="data-types-in-arm-templates"></a>Datentypen in ARM-Vorlagen

In diesem Artikel werden die Datentypen beschrieben, die in Azure Resource Manager-Vorlagen (ARM-Vorlagen) unterstützt werden.

## <a name="supported-types"></a>Unterstützte Typen

In einer ARM-Vorlage können Sie die folgenden Datentypen verwenden:

* array
* bool
* INT
* object
* secureObject
* secureString
* Zeichenfolge

## <a name="arrays"></a>Arrays

Arrays beginnen mit einer linken eckigen Klammer (`[`) und enden mit einer rechten eckigen Klammer (`]`). Ein Array kann in einer einzelnen Zeile oder in mehreren Zeilen deklariert werden. Die Elemente werden durch ein Komma getrennt.

```json
"parameters": {
  "exampleArray": {
    "type": "array",
    "defaultValue": [
      1,
      2,
      3
    ]
  }
},

"outputs": {
  "arrayOutput": {
    "type": "array",
    "value": "[variables('exampleArray')]"
  },
  "firstExampleArrayElement": {
    "type": "int",
    "value": "[parameters('exampleArray')[0]]"
  }
}
```

Die Elemente eines Arrays können vom selben Typ oder verschiedenen Typs sein.

```json
"variables": {
  "mixedArray": [
    "[resourceGroup().name]",
    1,
    true,
    "example string"
  ]
}

"outputs": {
  "arrayOutput": {
    "type": "array",
    "value": "[variables('mixedArray')]"
  },
  "firstMixedArrayElement": {
    "type": "string",
    "value": "[variables('mixedArray')[0]]"
  }
}
```

## <a name="booleans"></a>Boolesche Werte

Wenn Sie boolesche Werte angeben, verwenden Sie `true` oder `false`. Umschließen Sie den Wert nicht mit Anführungszeichen.

```json
"parameters": {
  "exampleBool": {
    "type": "bool",
    "defaultValue": true
  }
},
```

## <a name="integers"></a>Ganze Zahlen

Wenn Sie ganzzahlige Werte angeben, verwenden Sie keine Anführungszeichen.

```json
"parameters": {
  "exampleInt": {
    "type": "int",
    "defaultValue": 1
  }
}
```

Für Integer, die als Inlineparameter übergeben werden, ist der Wertebereich möglicherweise durch das SDK oder Befehlszeilentool, das Sie zur Bereitstellung verwenden, eingeschränkt. Wenn Sie beispielsweise PowerShell zum Bereitstellen einer Vorlage verwenden, können Integertypen im Bereich von -2147483648 bis 2147483647 liegen. Um diese Einschränkung zu vermeiden, geben Sie große Werte in einer [Parameterdatei](parameter-files.md) an. Ressourcentypen wenden ihre eigenen Grenzwerte für Integereigenschaften an.

## <a name="objects"></a>Objekte

Objekte beginnen mit einer linken geschweiften Klammer (`{`) und enden mit einer rechten geschweiften Klammer (`}`). Jede Eigenschaft in einem Objekt besteht aus `key` und `value`. `key` und `value` sind in doppelte Anführungszeichen eingeschlossen und durch einen Doppelpunkt (`:`) getrennt. Die Spalten werden durch ein Komma getrennt.

```json
"parameters": {
  "exampleObject": {
    "type": "object",
    "defaultValue": {
      "name": "test name",
      "id": "123-abc",
      "isCurrent": true,
      "tier": 1
    }
  }
}
```

## <a name="strings"></a>Zeichenfolgen

Zeichenfolgen werden mit doppelten Anführungszeichen gekennzeichnet.

```json
"parameters": {
  "exampleString": {
    "type": "string",
    "defaultValue": "test value"
  }
},
```

## <a name="secure-strings-and-objects"></a>Sichere Zeichenfolgen und Objekte

Die sichere Zeichenfolge verwendet das gleiche Format wie die Zeichenfolge, und das sichere Objekt verwendet das gleiche Format wie das Objekt. Wenn Sie einen Parameter auf eine sichere Zeichenfolge oder ein sicheres Objekt festlegen, wird der Wert des-Parameters weder im Bereitstellungsverlauf gespeichert noch protokolliert. Wenn Sie diesen sicheren Wert jedoch auf eine Eigenschaft festlegen, die keinen sicheren Wert erwartet, wird der Wert nicht geschützt. Wenn Sie z. B. eine sichere Zeichenfolge auf ein Tag festlegen, wird dieser Wert als reiner Text gespeichert. Verwenden Sie sichere Zeichenfolgen für Kennwörter und Geheimnisse.

Das folgende Beispiel zeigt zwei sichere Parameter:

```json
"parameters": {
  "password": {
    "type": "secureString"
  },
  "configValues": {
    "type": "secureObject"
  }
}
```

## <a name="next-steps"></a>Nächste Schritte

Informationen zur Vorlagensyntax finden Sie unter [Verstehen der Struktur und Syntax von ARM-Vorlagen](./syntax.md).