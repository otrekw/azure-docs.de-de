---
title: Datentypen in Vorlagen
description: Beschreibt die Datentypen, die in Azure Resource Manager-Vorlagen verfügbar sind.
ms.topic: conceptual
ms.author: tomfitz
author: tfitzmac
ms.date: 03/04/2021
ms.openlocfilehash: 4d6c8306b3dbdfe895055dc008d81cc0d85d8d6c
ms.sourcegitcommit: 49b2069d9bcee4ee7dd77b9f1791588fe2a23937
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2021
ms.locfileid: "107538062"
---
# <a name="data-types-in-arm-templates"></a>Datentypen in ARM-Vorlagen

In diesem Artikel werden die Datentypen beschrieben, die in Azure Resource Manager-Vorlagen (ARM-Vorlagen) unterstützt werden. Es werden sowohl JSON- als auch Bicep-Datentypen behandelt.

## <a name="supported-types"></a>Unterstützte Typen

In einer ARM-Vorlage können Sie die folgenden Datentypen verwenden:

* array
* bool
* INT
* Objekt (object)
* secureObject – durch Modifizierer in Bicep angegeben
* secureString – durch Modifizierer in Bicep angegeben
* Zeichenfolge

## <a name="arrays"></a>Arrays

Arrays beginnen mit einer linken eckigen Klammer (`[`) und enden mit einer rechten eckigen Klammer (`]`).

In JSON kann ein Array in einer einzelnen Zeile oder in mehreren Zeilen deklariert werden. Die Elemente werden durch ein Komma getrennt.

In Bicep muss ein Array in mehreren Zeilen deklariert werden. Verwenden Sie keine Kommas zwischen Werten.

# <a name="json"></a>[JSON](#tab/json)

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
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
param exampleArray array = [
  1
  2
  3
]
```

---

Die Elemente eines Arrays können vom selben Typ oder verschiedenen Typs sein.

# <a name="json"></a>[JSON](#tab/json)

```json
"variables": {
  "mixedArray": [
    "[resourceGroup().name]",
    1,
    true,
    "example string"
  ]
}
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
var mixedArray = [
  resourceGroup().name
  1
  true
  'example string'
]
```

---

## <a name="booleans"></a>Boolesche Werte

Wenn Sie boolesche Werte angeben, verwenden Sie `true` oder `false`. Umschließen Sie den Wert nicht mit Anführungszeichen.

# <a name="json"></a>[JSON](#tab/json)

```json
"parameters": {
  "exampleBool": {
    "type": "bool",
    "defaultValue": true
  }
},
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
param exampleBool bool = true
```

---

## <a name="integers"></a>Ganze Zahlen

Wenn Sie ganzzahlige Werte angeben, verwenden Sie keine Anführungszeichen.

# <a name="json"></a>[JSON](#tab/json)

```json
"parameters": {
  "exampleInt": {
    "type": "int",
    "defaultValue": 1
  }
}
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
param exampleInt int = 1
```

---

Für Integer, die als Inlineparameter übergeben werden, ist der Wertebereich möglicherweise durch das SDK oder Befehlszeilentool, das Sie zur Bereitstellung verwenden, eingeschränkt. Wenn Sie beispielsweise PowerShell zum Bereitstellen einer Vorlage verwenden, können Integertypen im Bereich von -2147483648 bis 2147483647 liegen. Um diese Einschränkung zu vermeiden, geben Sie große Werte in einer [Parameterdatei](parameter-files.md) an. Ressourcentypen wenden ihre eigenen Grenzwerte für Integereigenschaften an.

## <a name="objects"></a>Objekte

Objekte beginnen mit einer linken geschweiften Klammer (`{`) und enden mit einer rechten geschweiften Klammer (`}`). Jede Eigenschaft in einem Objekt besteht aus einem Schlüssel und einem Wert. Der Schlüssel und der Wert werden durch einen Doppelpunkt (`:`) getrennt.

# <a name="json"></a>[JSON](#tab/json)

In JSON wird der Schlüssel in doppelte Anführungszeichen eingeschlossen. Die Spalten werden durch ein Komma getrennt.

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

# <a name="bicep"></a>[Bicep](#tab/bicep)

In Bicep ist der Schlüssel nicht in Anführungszeichen eingeschlossen. Verwenden Sie keine Kommas zwischen Eigenschaften.

```bicep
param exampleObject object = {
  name: 'test name'
  id: '123-abc'
  isCurrent: true
  tier: 1
}
```

Eigenschaftenaccessoren werden verwendet, um auf Eigenschaften eines Objekts zuzugreifen. Sie werden mit dem `.`-Operator erstellt. Beispiel:

```bicep
var x = {
  y: {
    z: 'Hello`
    a: true
  }
  q: 42
}
```

Bei der vorherigen Deklaration wird der Ausdruck x.y.z als Literal-Zeichenfolge „Hello“ ausgewertet. Auf ähnliche Weise wird der Ausdruck x.q als ganzzahliges Literal „42“ ausgewertet.

Eigenschaftsaccessoren können mit jedem Objekt verwendet werden. Dies schließt Parameter und Variablen von Objekttypen und Objektliteralen ein. Die Verwendung eines Eigenschaftsaccessors für einen Ausdruck des Nichtobjekttyps ist ein Fehler.

---

## <a name="strings"></a>Zeichenfolgen

In JSON werden Zeichenfolgen mit doppelten Anführungszeichen gekennzeichnet. In Bicep werden Zeichenfolgen mit einfachen Anführungszeichen gekennzeichnet.

# <a name="json"></a>[JSON](#tab/json)

```json
"parameters": {
  "exampleString": {
    "type": "string",
    "defaultValue": "test value"
  }
},
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
param exampleString string = 'test value'
```
---

## <a name="secure-strings-and-objects"></a>Sichere Zeichenfolgen und Objekte

Die sichere Zeichenfolge verwendet das gleiche Format wie die Zeichenfolge, und das sichere Objekt verwendet das gleiche Format wie das Objekt. Wenn Sie einen Parameter auf eine sichere Zeichenfolge oder ein sicheres Objekt festlegen, wird der Wert des-Parameters weder im Bereitstellungsverlauf gespeichert noch protokolliert. Wenn Sie diesen sicheren Wert jedoch auf eine Eigenschaft festlegen, die keinen sicheren Wert erwartet, wird der Wert nicht geschützt. Wenn Sie z. B. eine sichere Zeichenfolge auf ein Tag festlegen, wird dieser Wert als reiner Text gespeichert. Verwenden Sie sichere Zeichenfolgen für Kennwörter und Geheimnisse.

Mit Bicep fügen Sie den `@secure()`-Modifizierer einer Zeichenfolge oder einem Objekt hinzu.

Das folgende Beispiel zeigt zwei sichere Parameter:

# <a name="json"></a>[JSON](#tab/json)

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

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
@secure()
param password string

@secure()
param configValues object
```

---

## <a name="next-steps"></a>Nächste Schritte

Informationen zur Vorlagensyntax finden Sie unter [Verstehen der Struktur und Syntax von ARM-Vorlagen](template-syntax.md).
