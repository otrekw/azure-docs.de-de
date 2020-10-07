---
title: Graph-Suchabfragesyntax
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie die Suchabfragesyntax verwenden, um nach Knoten im Pipelinediagramm zu suchen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 8/24/2020
ms.openlocfilehash: 166b87159b5ac2434c3ce7a7c66529a4b61ee565
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91290499"
---
# <a name="graph-search-query-syntax"></a>Graph-Suchabfragesyntax

In diesem Artikel erfahren Sie mehr über die Graph-Suchabfragesyntax in Azure Machine Learning. Mit der Graph-Suchfunktion können Sie anhand des Namens und seinen Eigenschaften nach einem Knoten suchen. 

 ![Animierter Screenshot mit einem Beispiel für die Graph-Suche](media/search/graph-search.gif)

Die Graph-Suche unterstützt Volltext-Schlüsselwortsuche nach Knotennamen und Kommentaren. Sie können auch nach Knoteneigenschaften filtern, z. B. nach runStatus, duration, computeTarget. Die Schlüsselwortsuche basiert auf Lucene-Abfragen. Eine vollständige Suchabfrage sieht wie folgt aus:  

**[Lucene-Abfrage | [Filterabfrage]** 

Sie können die Lucene-Abfrage oder die Filterabfrage verwenden. Um beide Abfragen zu verwenden, verwenden Sie das Trennzeichen **|** . Die Syntax der Filterabfrage ist strenger als für Lucene-Abfragen. Wenn die Kundeneingabe mit beiden Abfragen analysiert werden kann, wird daher die Filterabfrage angewendet.

 

## <a name="lucene-query"></a>Lucene-Abfrage

Die Graph-Suche verwendet einfache Lucene-Abfragen als Volltextsuchsyntax für den Knoten „name“ und „comment“. Folgende Lucene-Operatoren werden unterstützt:

 
- UND/ODER
- Platzhalterübereinstimmung mit **?** und **\*** -Operatoren.

### <a name="examples"></a>Beispiele

- Einfache Suche: `JSON Data`

- AND/OR: `JSON AND Validation`

- Mehrere AND-/OR-Operatoren: `(JSON AND Validation) OR (TSV AND Training)`

 
- Platzhalterübereinstimmung: 
    - `machi?e learning`
    - `mach*ing`
 
>[!NOTE]
> Eine Lucene-Abfrage darf nicht mit dem Zeichen „*“ beginnen.

##  <a name="filter-query"></a>Filterabfrage

 
Filterabfragen verwenden das folgende Muster:
 
`**[key1] [operator1] [value1]; [key2] [operator1] [value2];**`

 
Die folgenden Knoteneigenschaften können als Schlüssel verwendet werden:

- runStatus
- compute
- duration
- reuse

Folgende Operatoren können verwendet werden:

- Größer als oder gleich: `>=`
- Kleiner als oder gleich: `<=`
- Größer als: `>`
- Kleiner als: `<`
- Gleich: `==`
- Enthält: `=`
- NotEqual: `!=`
- In: `in`

 
 

### <a name="example"></a>Beispiel

- duration > 100;
- status in {Failed,NotStarted}
- compute in {gpu-cluster}; runStatus in {Completed}

## <a name="technical-notes"></a>Technische Hinweise

- Die Beziehung zwischen mehreren Filtern ist „AND“.
- Wenn `>=,  >,  <, or  <=` ausgewählt wird, werden Werte automatisch in den Typ „number“ konvertiert. Andernfalls werden Zeichenfolgentypen für Vergleiche verwendet.
- Bei allen Werten vom Zeichenfolgentyp wird Groß-/Kleinschreibung in Vergleichen nicht berücksichtigt.
- Der Operator „In“ erwartet eine Sammlung als Wert, die Sammlungssyntax ist `{name1, name2, name3}`.
- Leerzeichen zwischen Schlüsselwörtern werden ignoriert.