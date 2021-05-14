---
title: Rangtransformation in einem Zuordnungsdatenfluss
description: Hier erfahren Sie, wie Sie die Rangtransformation im Zuordnungsdatenfluss von Azure Data Factory zum Generieren einer Rangspalte verwenden.
author: kromerm
ms.author: makromer
ms.reviewer: makromer
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 10/05/2020
ms.openlocfilehash: bc0a4d8701f8d53f0acff362713054269378a88a
ms.sourcegitcommit: b4032c9266effb0bf7eb87379f011c36d7340c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2021
ms.locfileid: "107903738"
---
# <a name="rank-transformation-in-mapping-data-flow"></a>Rangtransformation in einem Zuordnungsdatenfluss 

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Verwenden Sie die Rangtransformation, um eine geordnete Rangfolge basierend auf den vom Benutzer angegebenen Sortierbedingungen zu generieren. 

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4GGJo]

## <a name="configuration"></a>Konfiguration

![Rangeinstellungen](media/data-flow/rank-configuration.png "Rangeinstellungen")

**Keine Beachtung von Groß-/Kleinschreibung**: Wenn eine Sortierspalte vom Typ „String“ ist, wird die Groß-/Kleinschreibung bei der Rangfolge berücksichtigt. 

**Dicht:** Wenn diese Option aktiviert ist, wird die Rangspalte in dichter Rangfolge ausgeführt. Bei jeder Rangzahl handelt es sich um eine fortlaufende Nummer, und Rangwerte werden nach einem gleichwertigen Objekt nicht übersprungen.

**Rangspalte:** Der Name der generierten Rangspalte. Diese Spalte ist vom Typ „Long“.

**Sortierbedingungen**: Wählen Sie aus, nach welchen Spalten sortiert werden soll und in welcher Reihenfolge die Sortierung erfolgt. Die Reihenfolge bestimmt die Sortierpriorität.

Bei der oben gezeigten Konfiguration werden eingehende Basketballdaten übernommen und eine Rangspalte mit dem Namen „pointsRanking“ erstellt. Die Zeile mit dem höchsten Wert der Spalte *PTS* weist einen *pointsRanking*-Wert von 1 auf.

## <a name="data-flow-script"></a>Datenflussskript

### <a name="syntax"></a>Syntax

```
<incomingStream>
    rank(
        desc(<sortColumn1>),
        asc(<sortColumn2>),
        ...,
        caseInsensitive: { true | false }
        dense: { true | false }
        output(<rankColumn> as long)
    ) ~> <sortTransformationName<>
```

### <a name="example"></a>Beispiel

![Rangeinstellungen](media/data-flow/rank-configuration.png "Rangeinstellungen")

Der folgende Codeausschnitt zeigt das Datenflussskript für die obige Rangkonfiguration.

```
PruneColumns
    rank(
        desc(PTS, true),
        caseInsensitive: false,
        output(pointsRanking as long),
        dense: false
    ) ~> RankByPoints
```

## <a name="next-steps"></a>Nächste Schritte

Filtern Sie Zeilen basierend auf den Rangwerten mithilfe der [Filtertransformation](data-flow-filter.md).
