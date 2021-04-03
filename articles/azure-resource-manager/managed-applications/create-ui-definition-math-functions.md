---
title: Erstellen von mathematischen Benutzeroberflächendefinitions-Funktionen
description: Beschreibt die Funktionen, die beim Durchführen mathematischer Operationen verwendet werden können.
author: tfitzmac
ms.topic: conceptual
ms.date: 07/13/2020
ms.author: tomfitz
ms.openlocfilehash: 022e59d847a4d89b4243223637fc6fd1e73255a9
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "87094440"
---
# <a name="createuidefinition-math-functions"></a>Mathematische CreateUiDefinition-Funktionen

Die Funktionen ermöglichen es Ihnen, mathematische Operationen auszuführen.

## <a name="add"></a>add

Addiert zwei Zahlen und gibt das Ergebnis zurück.

Das folgende Beispiel gibt `3`zurück:

```json
"[add(1, 2)]"
```

## <a name="ceil"></a>ceil

Gibt den größten ganzzahligen Wert zurück, der im Vergleich zur angegebenen Zahl größer oder gleich groß ist.

Das folgende Beispiel gibt `4`zurück:

```json
"[ceil(3.14)]"
```

## <a name="div"></a>div

Dividiert die erste Zahl durch die zweite Zahl und gibt das Ergebnis zurück. Das Ergebnis ist immer eine ganze Zahl.

Das folgende Beispiel gibt `2`zurück:

```json
"[div(6, 3)]"
```

## <a name="floor"></a>floor

Gibt den größten ganzzahligen Wert zurück, der im Vergleich zur angegebenen Zahl kleiner oder gleich groß ist.

Das folgende Beispiel gibt `3`zurück:

```json
"[floor(3.14)]"
```

## <a name="max"></a>max

Gibt die größere der beiden Zahlen zurück.

Das folgende Beispiel gibt `2`zurück:

```json
"[max(1, 2)]"
```

## <a name="min"></a>Min

Gibt die kleinere der beiden Zahlen zurück.

Das folgende Beispiel gibt `1`zurück:

```json
"[min(1, 2)]"
```

## <a name="mod"></a>mod

Dividiert die erste Zahl durch die zweite Zahl und gibt den Restwert zurück.

Das folgende Beispiel gibt `0`zurück:

```json
"[mod(6, 3)]"
```

Das folgende Beispiel gibt `2`zurück:

```json
"[mod(6, 4)]"
```

## <a name="mul"></a>mul

Multipliziert zwei Zahlen und gibt das Ergebnis zurück.

Das folgende Beispiel gibt `6`zurück:

```json
"[mul(2, 3)]"
```

## <a name="rand"></a>rand

Gibt eine zufällige ganze Zahl innerhalb des angegebenen Bereichs zurück. Diese Funktion generiert keine kryptografisch sicheren Zufallszahlen.

Im folgenden Beispiel könnte `42` zurückgegeben werden:

```json
"[rand(-100, 100)]"
```

## <a name="range"></a>range

Generiert eine Sequenz von ganzen Zahlen innerhalb des angegebenen Bereichs.

Das folgende Beispiel gibt `[1,2,3]`zurück:

```json
"[range(1, 3)]"
```

## <a name="sub"></a>sub

Subtrahiert die zweite Zahl von der ersten Zahl und gibt das Ergebnis zurück.

Das folgende Beispiel gibt `1`zurück:

```json
"[sub(3, 2)]"
```

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung in Azure Resource Manager finden Sie unter [Übersicht über Azure Resource Manager](../management/overview.md).
