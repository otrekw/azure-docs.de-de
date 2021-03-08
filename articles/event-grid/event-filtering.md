---
title: Ereignisfilter für Azure Event Grid
description: Hier erfahren Sie, wie Sie Ereignisse beim Erstellen eines Azure Event Grid-Abonnements filtern können.
ms.topic: conceptual
ms.date: 02/26/2021
ms.openlocfilehash: 7253c4a38660b0041f27918309efae21675fdc8f
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101721955"
---
# <a name="understand-event-filtering-for-event-grid-subscriptions"></a>Grundlegendes zur Ereignisfilterung für Event Grid-Abonnements

Dieser Artikel beschreibt die verschiedenen Möglichkeiten, um zu filtern, welche Ereignisse an den Endpunkt gesendet werden. Beim Erstellen eines Ereignisabonnements haben Sie drei Optionen zum Filtern:

* Ereignistypen
* Betreff beginnt oder endet mit
* Erweiterte Felder und Operatoren

## <a name="event-type-filtering"></a>Ereignistypfilterung

Standardmäßig werden alle [Ereignistypen](event-schema.md) für die Ereignisquelle an den Endpunkt gesendet. Sie können entscheiden, nur bestimmte Ereignistypen an Ihren Endpunkt zu senden. Beispielsweise können Sie sich bezüglich Aktualisierungen Ihrer Ressourcen benachrichtigen lassen, aber nicht zu anderen Vorgängen, wie z.B. das Löschen. Filtern Sie in diesem Fall nach dem Ereignistyp `Microsoft.Resources.ResourceWriteSuccess`. Geben Sie ein Array mit den Ereignistypen an oder geben Sie `All` an, um alle Ereignistypen für die Ereignisquelle zu erhalten.

Die JSON-Syntax für das Filtern nach Ereignistyp lautet:

```json
"filter": {
  "includedEventTypes": [
    "Microsoft.Resources.ResourceWriteFailure",
    "Microsoft.Resources.ResourceWriteSuccess"
  ]
}
```

## <a name="subject-filtering"></a>Betrefffilterung

Zum einfachen Filtern nach Betreff geben Sie einen Start- oder Endwert für den Betreff ein. Sie können beispielsweise angeben, dass der Betreff mit `.txt` endet, um nur Ereignisse im Zusammenhang mit dem Hochladen einer Textdatei in das Speicherkonto zu erhalten. Oder Sie können nach Betreffs filtern, die mit `/blobServices/default/containers/testcontainer` beginnen, um alle Ereignisse für diesen Container abzurufen, aber nicht für andere Container des Speicherkontos.

Erstellen Sie beim Veröffentlichen von Ereignissen für benutzerdefinierte Themen Betreffinformationen für Ihre Ereignisse, an denen Abonnenten einfach erkennen können, ob Interesse am Ereignis besteht. Abonnenten verwenden die Betreffeigenschaft zum Filtern und Weiterleiten von Ereignissen. Erwägen Sie, den Pfad zum Ereignisort hinzuzufügen, damit Abonnenten nach Segmenten dieses Pfads filtern können. Mit dem Pfad können Abonnenten Ereignisse speziell oder allgemein filtern. Wenn Sie einen Pfad mit drei Segmenten im Betreff angeben, z.B. `/A/B/C`, können Abonnenten nach dem ersten Segment `/A` filtern, um eine umfassendere Gruppe von Ereignissen angezeigt zu bekommen. Diese Abonnenten erhalten Ereignisse mit Betreffinformationen wie `/A/B/C` oder `/A/D/E`. Andere Abonnenten können nach `/A/B` filtern, um eine eingeschränktere Gruppe mit Ereignissen zu erhalten.

Die JSON-Syntax für das Filtern nach Betreff lautet:

```json
"filter": {
  "subjectBeginsWith": "/blobServices/default/containers/mycontainer/log",
  "subjectEndsWith": ".jpg"
}

```

## <a name="advanced-filtering"></a>Erweiterte Filterung

Um nach Werten in den Datenfeldern zu filtern und den Vergleichsoperator anzugeben, verwenden Sie die erweiterte Filteroption. Bei der erweiterten Filterung können Sie folgende Informationen angeben:

* Operatortyp – Typ des Vergleichs.
* Schlüssel – Feld in den Ereignisdaten, die Sie für die Filterung verwenden. Dabei kann es sich um eine Zahl, einen booleschen Wert, eine Zeichenfolge oder ein Array handeln.
* Werte – Werte, die mit dem Schlüssel verglichen werden sollen.

## <a name="key"></a>Key
Das Feld „key“ in den Ereignisdaten wird für die Filterung verwendet. Dabei kann es sich um eine Zahl, einen booleschen Wert, eine Zeichenfolge oder ein Array handeln. Verwenden Sie für Ereignisse im **Event Grid-Schema** die folgenden Werte für „key“: `ID`, `Topic`, `Subject`, `EventType`, `DataVersion` oder Ereignisdaten wie `data.key1`.

Verwenden Sie für Ereignisse im **CloudEvents-Schema** die folgenden Werte für „key“: `eventid`, `source`, `eventtype`, `eventtypeversion` oder Ereignisdaten wie `data.key1`.

Verwenden Sie für ein **benutzerdefiniertes Eingabeschema** die Ereignisdatenfelder wie `data.key1`.

Verwenden Sie den Punktoperator (`.`), um auf Felder im Datenabschnitt zuzugreifen. Verwenden Sie beispielsweise `data.sitename` oder `data.appEventTypeDetail.action`, um auf `sitename` oder `action` im folgenden Beispielereignis zuzugreifen.

```json
    "data": {
        "appEventTypeDetail": {
            "action": "Started"
        },
        "siteName": "<site-name>",
        "clientRequestId": "None",
        "correlationRequestId": "None",
        "requestId": "292f499d-04ee-4066-994d-c2df57b99198",
        "address": "None",
        "verb": "None"
    },
```


## <a name="values"></a>Werte
Zulässige Werte sind Zahlen, Zeichenfolgen, boolesche Werte oder Arrays.


## <a name="operators"></a>Operatoren

Die verfügbaren Operatoren für **Zahlen** sind:

## <a name="numberin"></a>NumberIn
Der NumberIn-Operator wird als „true“ ausgewertet, wenn der **key**-Wert einem der angegebenen **filter**-Werte entspricht. Im folgenden Beispiel wird überprüft, ob der Wert des Attributs `counter` im Abschnitt `data` 5 oder 1 ist. 

```json
"advancedFilters": [{
    "operatorType": "NumberIn",
    "key": "data.counter",
    "values": [
        5,
        1
    ]
}]
```


Wenn es sich bei „key“ um ein Array handelt, werden alle Werte im Array mit dem Array der filter-Werte verglichen. Hier sehen Sie Pseudocode mit den key-Werten `[v1, v2, v3]` und den filter-Werten `[a, b, c]`. Sämtliche Werte von „key“, deren Datentypen nicht mit den Datentypen von „filter“ übereinstimmen, werden ignoriert.

```
FOR_EACH filter IN (a, b, c)
    FOR_EACH key IN (v1, v2, v3)
        IF filter == key
            MATCH
```

## <a name="numbernotin"></a>NumberNotIn
Der NumberNotIn-Operator wird als „true“ ausgewertet, wenn der **key**-Wert **keinem** der angegebenen **filter**-Werte entspricht. Im folgenden Beispiel wird überprüft, ob der Wert des Attributs `counter` im Abschnitt `data` nicht 41 und nicht 0 ist. 

```json
"advancedFilters": [{
    "operatorType": "NumberNotIn",
    "key": "data.counter",
    "values": [
        41,
        0
    ]
}]
```

Wenn es sich bei „key“ um ein Array handelt, werden alle Werte im Array mit dem Array der filter-Werte verglichen. Hier sehen Sie Pseudocode mit den key-Werten `[v1, v2, v3]` und den filter-Werten `[a, b, c]`. Sämtliche Werte von „key“, deren Datentypen nicht mit den Datentypen von „filter“ übereinstimmen, werden ignoriert.

```
FOR_EACH filter IN (a, b, c)
    FOR_EACH key IN (v1, v2, v3)
        IF filter == key
            FAIL_MATCH
```

## <a name="numberlessthan"></a>NumberLessThan
Der NumberLessThan-Operator wird als „true“ ausgewertet, wenn der **key**-Wert **kleiner als** der angegebene **filter**-Wert ist. Im folgenden Beispiel wird überprüft, ob der Wert des Attributs `counter` im Abschnitt `data` kleiner als 100 ist. 

```json
"advancedFilters": [{
    "operatorType": "NumberLessThan",
    "key": "data.counter",
    "value": 100
}]
```

Wenn es sich bei „key“ um ein Array handelt, werden alle Werte im Array mit den filter-Werten verglichen. Hier sehen Sie den Pseudocode mit den key-Werten `[v1, v2, v3]`. Sämtliche Werte von „key“, deren Datentypen nicht mit den Datentypen von „filter“ übereinstimmen, werden ignoriert.

```
FOR_EACH key IN (v1, v2, v3)
    IF key < filter
        MATCH
```

## <a name="numbergreaterthan"></a>NumberGreaterThan
Der NumberGreaterThan-Operator wird als „true“ ausgewertet, wenn der **key**-Wert **größer als** der angegebene **filter**-Wert ist. Im folgenden Beispiel wird überprüft, ob der Wert des Attributs `counter` im Abschnitt `data` größer als 20 ist. 

```json
"advancedFilters": [{
    "operatorType": "NumberGreaterThan",
    "key": "data.counter",
    "value": 20
}]
```

Wenn es sich bei „key“ um ein Array handelt, werden alle Werte im Array mit den filter-Werten verglichen. Hier sehen Sie den Pseudocode mit den key-Werten `[v1, v2, v3]`. Sämtliche Werte von „key“, deren Datentypen nicht mit den Datentypen von „filter“ übereinstimmen, werden ignoriert.

```
FOR_EACH key IN (v1, v2, v3)
    IF key > filter
        MATCH
```

## <a name="numberlessthanorequals"></a>NumberLessThanOrEquals
Der NumberLessThanOrEquals-Operator wird als „true“ ausgewertet, wenn der **key**-Wert **kleiner oder gleich** dem angegebenen **filter**-Wert ist. Im folgenden Beispiel wird überprüft, ob der Wert des Attributs `counter` im Abschnitt `data` kleiner oder gleich 100 ist. 

```json
"advancedFilters": [{
    "operatorType": "NumberLessThanOrEquals",
    "key": "data.counter",
    "value": 100
}]
```

Wenn es sich bei „key“ um ein Array handelt, werden alle Werte im Array mit den filter-Werten verglichen. Hier sehen Sie den Pseudocode mit den key-Werten `[v1, v2, v3]`. Sämtliche Werte von „key“, deren Datentypen nicht mit den Datentypen von „filter“ übereinstimmen, werden ignoriert.

```
FOR_EACH key IN (v1, v2, v3)
    IF key <= filter
        MATCH
```

## <a name="numbergreaterthanorequals"></a>NumberGreaterThanOrEquals
Der NumberGreaterThanOrEquals-Operator wird als „true“ ausgewertet, wenn der **key**-Wert **größer oder gleich** dem angegebenen **filter**-Wert ist. Im folgenden Beispiel wird überprüft, ob der Wert des Attributs `counter` im Abschnitt `data` größer oder gleich 30 ist. 

```json
"advancedFilters": [{
    "operatorType": "NumberGreaterThanOrEquals",
    "key": "data.counter",
    "value": 30
}]
```

Wenn es sich bei „key“ um ein Array handelt, werden alle Werte im Array mit den filter-Werten verglichen. Hier sehen Sie den Pseudocode mit den key-Werten `[v1, v2, v3]`. Sämtliche Werte von „key“, deren Datentypen nicht mit den Datentypen von „filter“ übereinstimmen, werden ignoriert.

```
FOR_EACH key IN (v1, v2, v3)
    IF key >= filter
        MATCH
```

## <a name="numberinrange"></a>NumberInRange
Der NumberInRange-Operator wird als „true“ ausgewertet, wenn der **key**-Wert in einem der angegebenen **filter**-Bereiche liegt. Im folgenden Beispiel wird überprüft, ob der Wert des Attributs `key1` im Abschnitt `data` im Bereich von 3,14159 bis 999,95 oder von 3000 bis 4000 liegt. 

```json
{
    "operatorType": "NumberInRange",
    "key": "data.key1",
    "values": [[3.14159, 999.95], [3000, 4000]]
}
```

Die Eigenschaft `values` ist ein Array, das aus Bereichen besteht. Im vorherigen Beispiel handelt es sich um ein Array aus zwei Bereichen. Hier sehen Sie ein Beispiel für ein Array mit einem zu überprüfenden Bereich. 

**Array mit einem Bereich:** 
```json
{
    "operatorType": "NumberInRange",
    "key": "data.key1",
    "values": [[3000, 4000]]
}
```

Wenn es sich bei „key“ um ein Array handelt, werden alle Werte im Array mit dem Array der filter-Werte verglichen. Hier sehen Sie Pseudocode mit den key-Werten `[v1, v2, v3]` und filter-Werten, die ein Array aus Bereichen darstellen. In diesem Pseudo entsprechen `a` und `b` dem Mindest- bzw. Maximalwert jedes Bereichs im Array. Sämtliche Werte von „key“, deren Datentypen nicht mit den Datentypen von „filter“ übereinstimmen, werden ignoriert.

```
FOR_EACH (a,b) IN filter.Values
    FOR_EACH key IN (v1, v2, v3)
       IF key >= a AND key <= b
           MATCH
```


## <a name="numbernotinrange"></a>NumberNotInRange
Der NumberNotInRange-Operator wird als „true“ ausgewertet, wenn der **key**-Wert in **keinem** der angegebenen **filter**-Bereiche liegt. Im folgenden Beispiel wird überprüft, ob der Wert des Attributs `key1` im Abschnitt `data` im Bereich von 3,14159 bis 999,95 oder von 3000 bis 4000 liegt. Wenn das der Fall ist, gibt der Operator „false“ zurück. 

```json
{
    "operatorType": "NumberNotInRange",
    "key": "data.key1",
    "values": [[3.14159, 999.95], [3000, 4000]]
}
```
Die Eigenschaft `values` ist ein Array, das aus Bereichen besteht. Im vorherigen Beispiel handelt es sich um ein Array aus zwei Bereichen. Hier sehen Sie ein Beispiel für ein Array mit einem zu überprüfenden Bereich.

**Array mit einem Bereich:** 
```json
{
    "operatorType": "NumberNotInRange",
    "key": "data.key1",
    "values": [[3000, 4000]]
}
```

Wenn es sich bei „key“ um ein Array handelt, werden alle Werte im Array mit dem Array der filter-Werte verglichen. Hier sehen Sie Pseudocode mit den key-Werten `[v1, v2, v3]` und filter-Werten, die ein Array aus Bereichen darstellen. In diesem Pseudo entsprechen `a` und `b` dem Mindest- bzw. Maximalwert jedes Bereichs im Array. Sämtliche Werte von „key“, deren Datentypen nicht mit den Datentypen von „filter“ übereinstimmen, werden ignoriert.

```
FOR_EACH (a,b) IN filter.Values
    FOR_EACH key IN (v1, v2, v3)
        IF key >= a AND key <= b
            FAIL_MATCH
```


Der verfügbare Operator für **boolesche Werte** ist: 

## <a name="boolequals"></a>BoolEquals
Der BoolEquals-Operator wird als „true“ ausgewertet, wenn der **key**-Wert dem für **filter** angegebenen booleschen Wert entspricht. Im folgenden Beispiel wird überprüft, ob der Wert des Attributs `isEnabled` im Abschnitt `data` `true` ist. 

```json
"advancedFilters": [{
    "operatorType": "BoolEquals",
    "key": "data.isEnabled",
    "value": true
}]
```

Wenn es sich bei „key“ um ein Array handelt, werden alle Werte im Array mit den booleschen Werten für „filter“ verglichen. Hier sehen Sie den Pseudocode mit den key-Werten `[v1, v2, v3]`. Sämtliche Werte von „key“, deren Datentypen nicht mit den Datentypen von „filter“ übereinstimmen, werden ignoriert.

```
FOR_EACH key IN (v1, v2, v3)
    IF filter == key
        MATCH
```

Die verfügbaren Operatoren für **Zeichenfolgen** sind:

## <a name="stringcontains"></a>StringContains
Der **StringContains**-Operator wird als „true“ ausgewertet, wenn der **key**-Wert einen der angegebenen **filter**-Werte als Teilzeichenfolge **enthält**. Im folgenden Beispiel wird überprüft, ob der Wert des Attributs `key1` im Abschnitt `data` eine der angegebenen Teilzeichenfolgen (`microsoft` oder `azure`) enthält. `azure data factory` enthält beispielsweise `azure`. 

```json
"advancedFilters": [{
    "operatorType": "StringContains",
    "key": "data.key1",
    "values": [
        "microsoft", 
        "azure"
    ]
}]
```

Wenn es sich bei „key“ um ein Array handelt, werden alle Werte im Array mit dem Array der filter-Werte verglichen. Hier sehen Sie Pseudocode mit den key-Werten `[v1, v2, v3]` und den filter-Werten `[a,b,c]`. Sämtliche Werte von „key“, deren Datentypen nicht mit den Datentypen von „filter“ übereinstimmen, werden ignoriert.

```
FOR_EACH filter IN (a, b, c)
    FOR_EACH key IN (v1, v2, v3)
        IF key CONTAINS filter
            MATCH
```

## <a name="stringnotcontains"></a>StringNotContains
Der **StringNotContains**-Operator wird als „true“ ausgewertet, wenn der **key**-Wert **keinen** der angegebenen **filter**-Werte als Teilzeichenfolge enthält. Wenn der Schlüssel einen der angegebenen Werte als Teilzeichenfolge enthält, wird der Operator als „false“ ausgewertet. Im folgenden Beispiel gibt der Operator nur „true“ zurück, wenn der Wert des Attributs `key1` im Abschnitt `data` weder die Teilzeichenfolge `contoso` noch `fabrikam` enthält. 

```json
"advancedFilters": [{
    "operatorType": "StringNotContains",
    "key": "data.key1",
    "values": [
        "contoso", 
        "fabrikam"
    ]
}]
```

Wenn es sich bei „key“ um ein Array handelt, werden alle Werte im Array mit dem Array der filter-Werte verglichen. Hier sehen Sie Pseudocode mit den key-Werten `[v1, v2, v3]` und den filter-Werten `[a,b,c]`. Sämtliche Werte von „key“, deren Datentypen nicht mit den Datentypen von „filter“ übereinstimmen, werden ignoriert.

```
FOR_EACH filter IN (a, b, c)
    FOR_EACH key IN (v1, v2, v3)
        IF key CONTAINS filter
            FAIL_MATCH
```

## <a name="stringbeginswith"></a>StringBeginsWith
Der **StringBeginsWith**-Operator wird als „true“ ausgewertet, wenn der **key**-Wert mit einem der angegebenen **filter**-Werte **beginnt**. Im folgenden Beispiel wird überprüft, ob der Wert des Attributs `key1` im Abschnitt `data` mit `event` oder `grid` beginnt. `event hubs` beginnt beispielsweise mit `event`.  

```json
"advancedFilters": [{
    "operatorType": "StringBeginsWith",
    "key": "data.key1",
    "values": [
        "event", 
        "message"
    ]
}]
```

Wenn es sich bei „key“ um ein Array handelt, werden alle Werte im Array mit dem Array der filter-Werte verglichen. Hier sehen Sie Pseudocode mit den key-Werten `[v1, v2, v3]` und den filter-Werten `[a,b,c]`. Sämtliche Werte von „key“, deren Datentypen nicht mit den Datentypen von „filter“ übereinstimmen, werden ignoriert.

```
FOR_EACH filter IN (a, b, c)
    FOR_EACH key IN (v1, v2, v3)
        IF key BEGINS_WITH filter
            MATCH
```

## <a name="stringnotbeginswith"></a>StringNotBeginsWith
Der **StringNotBeginsWith**-Operator wird als „true“ ausgewertet, wenn der **key**-Wert **nicht** mit einem der angegebenen **filter**-Werte beginnt. Im folgenden Beispiel wird überprüft, ob der Wert des Attributs `key1` im Abschnitt `data` nicht mit `event` oder `message` beginnt.

```json
"advancedFilters": [{
    "operatorType": "StringNotBeginsWith",
    "key": "data.key1",
    "values": [
        "event", 
        "message"
    ]
}]
```

Wenn es sich bei „key“ um ein Array handelt, werden alle Werte im Array mit dem Array der filter-Werte verglichen. Hier sehen Sie Pseudocode mit den key-Werten `[v1, v2, v3]` und den filter-Werten `[a,b,c]`. Sämtliche Werte von „key“, deren Datentypen nicht mit den Datentypen von „filter“ übereinstimmen, werden ignoriert.

```
FOR_EACH filter IN (a, b, c)
    FOR_EACH key IN (v1, v2, v3)
        IF key BEGINS_WITH filter
            FAIL_MATCH
```

## <a name="stringendswith"></a>StringEndsWith
Der **StringEndsWith**-Operator wird als „true“ ausgewertet, wenn der **key**-Wert mit einem der angegebenen **filter**-Werte **endet**. Im folgenden Beispiel wird überprüft, ob der Wert des Attributs `key1` im Abschnitt `data` mit `jpg`, `jpeg` oder `png` endet. `eventgrid.png` endet beispielsweise mit `png`.


```json
"advancedFilters": [{
    "operatorType": "StringEndsWith",
    "key": "data.key1",
    "values": [
        "jpg", 
        "jpeg", 
        "png"
    ]
}]
```

Wenn es sich bei „key“ um ein Array handelt, werden alle Werte im Array mit dem Array der filter-Werte verglichen. Hier sehen Sie Pseudocode mit den key-Werten `[v1, v2, v3]` und den filter-Werten `[a,b,c]`. Sämtliche Werte von „key“, deren Datentypen nicht mit den Datentypen von „filter“ übereinstimmen, werden ignoriert.

```
FOR_EACH filter IN (a, b, c)
    FOR_EACH key IN (v1, v2, v3)
        IF key ENDS_WITH filter
            MATCH
```

## <a name="stringnotendswith"></a>StringNotEndsWith
Der **StringNotEndsWith**-Operator wird als „true“ ausgewertet, wenn der **key**-Wert **nicht** mit einem der angegebenen **filter**-Werte endet. Im folgenden Beispiel wird überprüft, ob der Wert des Attributs `key1` im Abschnitt `data` nicht mit `jpg`, `jpeg` oder `png` endet. 


```json
"advancedFilters": [{
    "operatorType": "StringNotEndsWith",
    "key": "data.key1",
    "values": [
        "jpg", 
        "jpeg", 
        "png"
    ]
}]
```

Wenn es sich bei „key“ um ein Array handelt, werden alle Werte im Array mit dem Array der filter-Werte verglichen. Hier sehen Sie Pseudocode mit den key-Werten `[v1, v2, v3]` und den filter-Werten `[a,b,c]`. Sämtliche Werte von „key“, deren Datentypen nicht mit den Datentypen von „filter“ übereinstimmen, werden ignoriert.

```
FOR_EACH filter IN (a, b, c)
    FOR_EACH key IN (v1, v2, v3)
        IF key ENDS_WITH filter
            FAIL_MATCH
```

## <a name="stringin"></a>StringIn
Der **StringIn**-Operator überprüft, ob der **key**-Wert **exakt** mit einem der angegebenen **filter**-Werte übereinstimmt. Im folgenden Beispiel wird überprüft, ob der Wert des Attributs `key1` im Abschnitt `data` genau `exact`, `string` oder `matches` ist. 

```json
"advancedFilters": [{
    "operatorType": "StringIn",
    "key": "data.key1",
    "values": [
        "contoso", 
        "fabrikam", 
        "factory"
    ]
}]
```

Wenn es sich bei „key“ um ein Array handelt, werden alle Werte im Array mit dem Array der filter-Werte verglichen. Hier sehen Sie Pseudocode mit den key-Werten `[v1, v2, v3]` und den filter-Werten `[a,b,c]`. Sämtliche Werte von „key“, deren Datentypen nicht mit den Datentypen von „filter“ übereinstimmen, werden ignoriert.

```
FOR_EACH filter IN (a, b, c)
    FOR_EACH key IN (v1, v2, v3)
        IF filter == key
            MATCH
```

## <a name="stringnotin"></a>StringNotIn
Der **StringNotIn**-Operator überprüft, ob der **key**-Wert mit **keinem** der angegebenen **filter**-Werte übereinstimmt. Im folgenden Beispiel wird überprüft, ob der Wert des Attributs `key1` im Abschnitt `data` nicht `aws` und nicht `bridge` ist. 

```json
"advancedFilters": [{
    "operatorType": "StringNotIn",
    "key": "data.key1",
    "values": [
        "aws", 
        "bridge"
    ]
}]
```

Wenn es sich bei „key“ um ein Array handelt, werden alle Werte im Array mit dem Array der filter-Werte verglichen. Hier sehen Sie Pseudocode mit den key-Werten `[v1, v2, v3]` und den filter-Werten `[a,b,c]`. Sämtliche Werte von „key“, deren Datentypen nicht mit den Datentypen von „filter“ übereinstimmen, werden ignoriert.

```
FOR_EACH filter IN (a, b, c)
    FOR_EACH key IN (v1, v2, v3)
        IF filter == key
            FAIL_MATCH
```


Bei sämtlichen Zeichenfolgenvergleichen wird die Groß-/Kleinschreibung nicht beachtet.

> [!NOTE]
> Wenn der JSON-Code für das Ereignis den erweiterten Filterschlüssel nicht enthält, wird „filter“ von den folgenden Operatoren als **nicht übereinstimmend** ausgewertet: NumberGreaterThan, NumberGreaterThanOrEquals, NumberLessThan, NumberLessThanOrEquals, NumberIn, BoolEquals, StringContains, StringNotContains, StringBeginsWith, StringNotBeginsWith, StringEndsWith, StringNotEndsWith und StringIn.
> 
>Der Filter wird für die folgenden Operatoren als **übereinstimmend** ausgewertet: NumberNotIn und StringNotIn.


## <a name="isnullorundefined"></a>IsNullOrUndefined
Der IsNullOrUndefined-Operator wird als „true“ ausgewertet, wenn der key-Wert NULL lautet oder undefiniert ist. 

```json
{
    "operatorType": "IsNullOrUndefined",
    "key": "data.key1"
}
```

Im folgenden Beispiel fehlt key1, deshalb wird der Operator als „true“ ausgewertet. 

```json
{ 
    "data": 
    { 
        "key2": 5 
    } 
}
```

Im folgenden Beispiel ist key1 auf NULL festgelegt, deshalb wird der Operator als „true“ ausgewertet.

```json
{
    "data": 
    { 
        "key1": null
    }
}
```

Wenn key1 in diesen Beispielen einen anderen Wert aufweisen würde, würde der Operator als „false“ ausgewertet werden. 

## <a name="isnotnull"></a>IsNotNull
Der IsNotNull-Operator wird als „true“ ausgewertet, wenn der key-Wert nicht NULL lautet oder nicht undefiniert ist. 

```json
{
    "operatorType": "IsNotNull",
    "key": "data.key1"
}
```

## <a name="or-and-and"></a>OR und AND
Wenn Sie einen einzelnen Filter mit mehreren Werten angeben, wird ein **OR**-Vorgang ausgeführt, daher muss das Schlüsselfeld einen der folgenden Werte aufweisen. Beispiel:

```json
"advancedFilters": [
    {
        "operatorType": "StringContains",
        "key": "Subject",
        "values": [
            "/providers/microsoft.devtestlab/",
            "/providers/Microsoft.Compute/virtualMachines/"
        ]
    }
]
```

Wenn Sie mehrere verschiedene Filter angeben, wird ein **AND**-Vorgang ausgeführt, sodass jede Filterbedingung erfüllt sein muss. Hier sehen Sie ein Beispiel: 

```json
"advancedFilters": [
    {
        "operatorType": "StringContains",
        "key": "Subject",
        "values": [
            "/providers/microsoft.devtestlab/"
        ]
    },
    {
        "operatorType": "StringContains",
        "key": "Subject",
        "values": [
            "/providers/Microsoft.Compute/virtualMachines/"
        ]
    }
]
```

## <a name="cloudevents"></a>CloudEvents 
Verwenden Sie für Ereignisse im **CloudEvents-Schema** die folgenden Werte für „key“: `eventid`, `source`, `eventtype`, `eventtypeversion` oder Ereignisdaten wie `data.key1`. 

Sie können auch die [Erweiterungskontextattribute in CloudEvents 1.0](https://github.com/cloudevents/spec/blob/v1.0.1/spec.md#extension-context-attributes) verwenden. Im folgenden Beispiel entsprechen `comexampleextension1` und `comexampleothervalue` den Erweiterungskontextattributen. 

```json
{
    "specversion" : "1.0",
    "type" : "com.example.someevent",
    "source" : "/mycontext",
    "id" : "C234-1234-1234",
    "time" : "2018-04-05T17:31:00Z",
    "subject": null,
    "comexampleextension1" : "value",
    "comexampleothervalue" : 5,
    "datacontenttype" : "application/json",
    "data" : {
        "appinfoA" : "abc",
        "appinfoB" : 123,
        "appinfoC" : true
    }
}
```

Hier sehen Sie ein Beispiel für die Verwendung eines Erweiterungskontextattributs in einem Filter.

```json
"advancedFilters": [{
    "operatorType": "StringBeginsWith",
    "key": "comexampleothervalue",
    "values": [
        "5", 
        "1"
    ]
}]
```


## <a name="limitations"></a>Einschränkungen

Für die erweiterte Filterung gelten folgende Einschränkungen:

* 5 erweiterte Filter und 25 Filterwerte für alle Filter pro Event Grid-Abonnement
* 512 Zeichen pro Zeichenfolgenwert
* Fünf Werte für **in**- und **not in**-Operatoren
* Schlüssel mit enthaltenem **`.` (Punktzeichen)** . Zum Beispiel: `http://schemas.microsoft.com/claims/authnclassreference` oder `john.doe@contoso.com`. Derzeit gibt es keine Unterstützung für Escapezeichen in Schlüsseln. 

Der gleiche Schlüssel kann in mehr als einem Filter verwendet werden.





## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Filtern von Ereignissen mit PowerShell und Azure CLI finden Sie unter [Filtern von Ereignissen für Event Grid](how-to-filter-events.md).
* Um sich schnell mit der Verwendung von Event Grid vertraut zu machen, lesen Sie [Erstellen und Weiterleiten benutzerdefinierter Ereignisse mit Azure Event Grid](custom-event-quickstart.md).
