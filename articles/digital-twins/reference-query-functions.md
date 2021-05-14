---
title: 'Referenz zur Azure Digital Twins-Abfragesprache: Funktionen'
titleSuffix: Azure Digital Twins
description: Referenzdokumentation für die Funktionen der Azure Digital Twins-Abfragesprache
author: baanders
ms.author: baanders
ms.date: 03/22/2021
ms.topic: article
ms.service: digital-twins
ms.openlocfilehash: 80b32907499d3c8588a7a565972f49923abbe5c7
ms.sourcegitcommit: fc9fd6e72297de6e87c9cf0d58edd632a8fb2552
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2021
ms.locfileid: "108296407"
---
# <a name="azure-digital-twins-query-language-reference-functions"></a>Referenz zur Azure Digital Twins-Abfragesprache: Funktionen

Dieses Dokument enthält Referenzinformationen zu **Funktionen** für die [Azure Digital Twins-Abfragesprache](concepts-query-language.md).

## <a name="endswith"></a>ENDSWITH

Eine Zeichenfolgenfunktion, mit der bestimmt wird, ob eine bestimmte Zeichenfolge mit einer bestimmten anderen Zeichenfolge endet. 

### <a name="syntax"></a>Syntax

:::code language="sql" source="~/digital-twins-docs-samples/queries/reference.sql" ID="EndsWithSyntax":::

### <a name="arguments"></a>Argumente

* `<string-to-check>`: Eine Zeichenfolge, deren Ende überprüft werden soll.
* `<ending-string>`: Eine Zeichenfolge, die die Endung darstellt, auf die geprüft werden soll.

### <a name="returns"></a>Rückgabe

Ein boolescher Wert, um anzugeben, ob der erste Zeichenfolgenausdruck mit dem zweiten beginnt.

### <a name="example"></a>Beispiel

Die folgende Abfrage gibt alle digitalen Zwillinge zurück, deren IDs auf `-small` enden.

:::code language="sql" source="~/digital-twins-docs-samples/queries/reference.sql" ID="EndsWithExample":::

## <a name="is_defined"></a>IS_DEFINED

Eine Typüberprüfungs- und Umwandlungsfunktion, um zu überprüfen, ob eine Eigenschaft definiert ist.

Dies wird nur unterstützt, wenn es sich bei dem Eigenschaftswert um einen primitiven Typ handelt. Primitive Typen umfassen Zeichenfolgen, boolesche Werte, numerische Werte und `null`. `DateTime`, Objekttypen und Arrays werden nicht unterstützt.

### <a name="syntax"></a>Syntax

:::code language="sql" source="~/digital-twins-docs-samples/queries/reference.sql" ID="IsDefinedSyntax":::

### <a name="arguments"></a>Argumente

`<property>`, eine Eigenschaft, um festzustellen, ob sie definiert ist. Die Eigenschaft muss einen primitiven Typ aufweisen.

### <a name="returns"></a>Gibt zurück

Ein boolescher Wert, um anzugeben, ob der Eigenschaft ein Wert zugewiesen wurde.

### <a name="example"></a>Beispiel

Die folgende Abfrage gibt alle digitalen Zwillinge zurück, die über eine definierte *Location*-Eigenschaft verfügen.

:::code language="sql" source="~/digital-twins-docs-samples/queries/reference.sql" ID="IsDefinedExample":::

## <a name="is_of_model"></a>IS_OF_MODEL

Eine Typüberprüfungs- und Umwandlungsfunktion, um zu bestimmen, ob ein Zwilling einen bestimmten Modelltyp aufweist. Schließt Modelle ein, die vom angegebenen Modell erben.

### <a name="syntax"></a>Syntax

:::code language="sql" source="~/digital-twins-docs-samples/queries/reference.sql" ID="IsOfModelSyntax":::

### <a name="arguments"></a>Argumente

Erforderlich:
* `<model-ID>`: Die Modell-ID, nach der gesucht werden soll.

Optional:
* `<twin-collection>`: Gibt eine zu durchsuchende Zwillingssammlung an, wenn mehrere Sammlungen vorhanden sind (wenn z. B. `JOIN` verwendet wird).
* `exact`: Erfordert eine genaue Übereinstimmung. Wenn dieser Parameter nicht festgelegt ist, enthält das Resultset Zwillinge mit Modellen, die vom angegebenen Modell erben.

### <a name="returns"></a>Rückgabe

Ein boolescher Wert, der angibt, ob der angegebene Zwilling dem angegebenen Modelltyp entspricht.

### <a name="example"></a>Beispiel

Die folgende Abfrage gibt Zwillinge aus der DT-Sammlung zurück, die genau vom Modelltyp `dtmi:example:room;1` sind.

:::code language="sql" source="~/digital-twins-docs-samples/queries/reference.sql" ID="IsOfModelExample":::

## <a name="is_bool"></a>IS_BOOL

Eine Typüberprüfungs- und Umwandlungsfunktion zum Bestimmen, ob ein Ausdruck über einen booleschen Wert verfügt.

Diese Funktion wird häufig mit anderen Prädikaten kombiniert, wenn das Programm, das die Abfrageergebnisse verarbeitet, einen booleschen Wert erfordert und Sie Fälle herausfiltern möchten, in denen die Eigenschaft kein boolescher Wert ist.

### <a name="syntax"></a>Syntax

:::code language="sql" source="~/digital-twins-docs-samples/queries/reference.sql" ID="IsBoolSyntax":::

### <a name="arguments"></a>Argumente

`<expression>`, ein Ausdruck, um zu überprüfen, ob es sich um einen booleschen Wert handelt.

### <a name="returns"></a>Rückgabe

Ein boolescher Wert, der angibt, ob der angegebene Ausdruck vom Typ „boolesch“ ist.

### <a name="example"></a>Beispiel

Die folgende Abfrage wählt die digitalen Zwillinge aus, die über eine boolesche `HasTemperature`-Eigenschaft verfügen.

:::code language="sql" source="~/digital-twins-docs-samples/queries/reference.sql" ID="IsBoolExample":::

Die folgende Abfrage baut auf dem Beispiel oben auf, um die digitalen Zwillinge auszuwählen, die über eine boolesche Eigenschaft verfügen, und der Wert dieser Eigenschaft `HasTemperature` ist nicht `false`.

:::code language="sql" source="~/digital-twins-docs-samples/queries/reference.sql" ID="IsBoolNotFalseExample":::

## <a name="is_number"></a>IS_NUMBER

Eine Typüberprüfungs- und Umwandlungsfunktion zum Bestimmen, ob ein Ausdruck über einen Zahlenwert verfügt.

Diese Funktion wird häufig mit anderen Prädikaten kombiniert, wenn das Programm, das die Abfrageergebnisse verarbeitet, einen Zahlenwert erfordert und Sie Fälle herausfiltern möchten, in denen die Eigenschaft keine Zahl ist.

### <a name="syntax"></a>Syntax

:::code language="sql" source="~/digital-twins-docs-samples/queries/reference.sql" ID="IsNumberSyntax":::

### <a name="arguments"></a>Argumente

`<expression>`, ein Ausdruck, um zu überprüfen, ob es sich um eine Zahl handelt.

### <a name="returns"></a>Rückgabe

Ein boolescher Wert, der angibt, ob der angegebene Ausdruck eine Zahl ist.

### <a name="example"></a>Beispiel

Die folgende Abfrage wählt die digitalen Zwillinge aus, die über eine numerische `Capacity`-Eigenschaft verfügen, deren Wert nicht gleich 0 ist.

:::code language="sql" source="~/digital-twins-docs-samples/queries/reference.sql" ID="IsNumberExample":::

## <a name="is_string"></a>IS_STRING

Eine Typüberprüfungs- und Umwandlungsfunktion zum Bestimmen, ob ein Ausdruck über einen Zeichenfolgenwert verfügt. 

Diese Funktion wird häufig mit anderen Prädikaten kombiniert, wenn das Programm, das die Abfrageergebnisse verarbeitet, einen Zeichenfolgenwert erfordert und Sie Fälle herausfiltern möchten, in denen die Eigenschaft keine Zeichenfolge ist.

### <a name="syntax"></a>Syntax

:::code language="sql" source="~/digital-twins-docs-samples/queries/reference.sql" ID="IsStringSyntax":::

### <a name="arguments"></a>Argumente

`<expression>`, ein Ausdruck, um zu überprüfen, ob es sich um eine Zeichenfolge handelt.

### <a name="returns"></a>Rückgabe

Ein boolescher Wert, der angibt, ob der angegebene Ausdruck eine Zeichenfolge ist.

### <a name="example"></a>Beispiel

Die folgende Abfrage wählt die digitalen Zwillinge aus, die über eine `Status`-Zeichenfolgeneigenschaft verfügen, deren Wert nicht gleich `Completed` ist.

:::code language="sql" source="~/digital-twins-docs-samples/queries/reference.sql" ID="IsStringExample":::

## <a name="is_null"></a>IS_NULL

Eine Typüberprüfungs- und Umwandlungsfunktion zum Bestimmen, ob der Wert eines Ausdrucks `null` ist.

### <a name="syntax"></a>Syntax

:::code language="sql" source="~/digital-twins-docs-samples/queries/reference.sql" ID="IsNullSyntax":::

### <a name="arguments"></a>Argumente

`<expression>`, ein Ausdruck, um zu überprüfen, ob der Wert NULL ist.

### <a name="returns"></a>Rückgabe

Ein boolescher Wert, der angibt, ob der angegebene Ausdruck `null` ist.

### <a name="example"></a>Beispiel

Die folgende Abfrage gibt Zwillinge zurück, die keinen NULL-Wert für Temperatur haben. Weitere Informationen zum in dieser Abfrage verwendeten `NOT`-Operator finden Sie unter [Azure Digital Twins – Referenz zur Abfragesprache: Operatoren](reference-query-operators.md#logical-operators).

:::code language="sql" source="~/digital-twins-docs-samples/queries/reference.sql" ID="IsNullExample":::

## <a name="is_primitive"></a>IS_PRIMITIVE

Eine Typüberprüfungs- und Umwandlungsfunktion, mit der bestimmt wird, ob der Wert eines Ausdrucks ein primitiver Typ ist (Zeichenfolge, boolescher Wert, numerischer Wert oder `null`).

Diese Funktion wird häufig mit anderen Prädikaten kombiniert, wenn das Programm, das die Abfrageergebnisse verarbeitet, einen Wert mit einem primitiven Typ erfordert und Sie Fälle herausfiltern möchten, in denen die Eigenschaft kein primitiver Typ ist.

### <a name="syntax"></a>Syntax

:::code language="sql" source="~/digital-twins-docs-samples/queries/reference.sql" ID="IsPrimitiveSyntax":::

### <a name="arguments"></a>Argumente

`<expression>`, ein Ausdruck, um zu überprüfen, ob es sich um einen primitiven Typ handelt.

### <a name="returns"></a>Rückgabe

Ein boolescher Wert, der anzeigt, ob der angegebene Ausdruck ein primitiver Typ ist (Zeichenfolge, boolescher Wert, numerischer Wert oder `null`).

### <a name="example"></a>Beispiel

Die folgende Abfrage gibt nur dann die `area`-Eigenschaft der Factory mit der ID „ABC“ zurück, wenn die `area`-Eigenschaft ein primitiver Typ ist. Weitere Informationen zum Projizieren bestimmter Spalten im Abfrageergebnis (wie in dieser Abfrage mit `area`) finden Sie unter [Azure Digital Twins – Abfragesprachenreferenz: SELECT-Klausel](reference-query-clause-select.md#select-columns-with-projections).

:::code language="sql" source="~/digital-twins-docs-samples/queries/reference.sql" ID="IsPrimitiveExample":::

## <a name="is_object"></a>IS_OBJECT

Eine Typüberprüfungs- und Umwandlungsfunktion zum Bestimmen, ob der Wert eines Ausdrucks ein JSON-Objekttyp ist.

Diese Funktion wird häufig mit anderen Prädikaten kombiniert, wenn das Programm, das die Abfrageergebnisse verarbeitet, ein JSON-Objekt erfordert und Sie Fälle herausfiltern möchten, in denen die Eigenschaft kein JSON-Objekt ist.

### <a name="syntax"></a>Syntax

:::code language="sql" source="~/digital-twins-docs-samples/queries/reference.sql" ID="IsObjectSyntax":::

### <a name="arguments"></a>Argumente

`<expression>`, ein Ausdruck, um zu überprüfen, ob es sich um einen Objekttyp handelt.

### <a name="returns"></a>Rückgabe

Ein boolescher Wert, der angibt, ob der angegebene Ausdruck ein JSON-Objekt ist.

### <a name="example"></a>Beispiel

Die folgende Abfrage wählt alle digitalen Zwillinge aus, bei denen dies ein Objekt namens `MapObject` ist und keine untergeordnete `TemperatureReading`-Eigenschaft vorhanden ist.

:::code language="sql" source="~/digital-twins-docs-samples/queries/reference.sql" ID="IsObjectExample":::

## <a name="startswith"></a>STARTSWITH

Eine Zeichenfolgenfunktion, mit der bestimmt wird, ob eine bestimmte Zeichenfolge mit einer bestimmten anderen Zeichenfolge beginnt. 

### <a name="syntax"></a>Syntax

:::code language="sql" source="~/digital-twins-docs-samples/queries/reference.sql" ID="StartsWithSyntax":::

### <a name="arguments"></a>Argumente

* `<string-to-check>`: Eine Zeichenfolge deren Anfang überprüft wird.
* `<beginning-string>`: Eine Zeichenfolge, die den Anfang darstellt, auf den geprüft werden soll.

### <a name="returns"></a>Rückgabe

Ein boolescher Wert, der angibt, ob der erste Zeichenfolgenausdruck mit dem zweiten beginnt.

### <a name="example"></a>Beispiel

Die folgende Abfrage gibt alle digitalen Zwillinge zurück, deren IDs mit `area1-` beginnen.

:::code language="sql" source="~/digital-twins-docs-samples/queries/reference.sql" ID="StartsWithExample":::
