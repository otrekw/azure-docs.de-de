---
title: Transformation zum Sortieren in einem Zuordnungsdatenfluss
description: 'Azure Data Factory Mapping-Daten: Transformation zum Sortieren'
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 04/14/2020
ms.openlocfilehash: 26852ec77194714c8236856b7cb496170bf0d777
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "81606335"
---
# <a name="sort-transformation-in-mapping-data-flow"></a>Transformation zum Sortieren in einem Zuordnungsdatenfluss

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Mit der Transformation zum Sortieren können Sie die eingehenden Zeilen im aktuellen Datenstrom sortieren. Sie können einzelne Spalten auswählen und in aufsteigender oder absteigender Reihenfolge sortieren.

> [!NOTE]
> Zuordnungsdatenflüsse werden auf Spark-Clustern ausgeführt, die Daten auf mehrere Knoten und Partitionen verteilen. Wenn Sie Ihre Daten in einer nachfolgenden Transformation neu partitionieren möchten, geht Ihre Sortierung möglicherweise aufgrund dieser Umverteilung von Daten verloren.

## <a name="configuration"></a>Konfiguration

![Sortiereinstellungen](media/data-flow/sort.png "Sortieren")

**Keine Beachtung von Groß-/Kleinschreibung**: Legen Sie fest, ob die Groß-/Kleinschreibung beim Sortieren von Zeichenfolgen- oder Textfeldern ignoriert werden soll.

**Nur innerhalb von Partitionen sortieren**: Wenn Datenflüsse auf Spark ausgeführt werden, wird jeder Datenfluss in Partitionen unterteilt. Mit dieser Einstellung werden Daten nur innerhalb der eingehenden Partitionen und nicht im gesamten Datenstrom sortiert. 

**Sortierbedingungen**: Wählen Sie aus, welche Spalten sortiert werden sollen und in welcher Reihenfolge die Sortierung erfolgt. Die Reihenfolge bestimmt die Sortierpriorität. Legen Sie fest, ob Nullen am Anfang und Ende des Datenstroms gezeigt werden.

### <a name="computed-columns"></a>Berechnete Spalten

Um einen Spaltenwert vor der Sortierung zu ändern oder zu extrahieren, zeigen Sie auf die Spalte und wählen „Berechnete Spalte“ aus. Dadurch wird für die Sortierung kein Spaltenwert verwendet, sondern der Ausdrucks-Generator geöffnet, um einen Ausdruck für den Sortiervorgang zu erstellen.

## <a name="data-flow-script"></a>Datenflussskript

### <a name="syntax"></a>Syntax

```
<incomingStream>
    sort(
        desc(<sortColumn1>, { true | false }),
        asc(<sortColumn2>, { true | false }),
        ...
    ) ~> <sortTransformationName<>
```

### <a name="example"></a>Beispiel

![Sortiereinstellungen](media/data-flow/sort.png "Sortieren")

Der nachfolgende Codeausschnitt zeigt das Datenflussskript für die obige Konfiguration der Sortierung.

```
BasketballStats sort(desc(PTS, true),
    asc(Age, true)) ~> Sort1
```

## <a name="next-steps"></a>Nächste Schritte

Nach dem Sortieren können Sie die [Transformation für das Aggregieren](data-flow-aggregate.md) verwenden.
