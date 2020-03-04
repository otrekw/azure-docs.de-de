---
title: Indizieren von räumlichen Daten mit Azure Cosmos DB
description: Indizieren räumlicher Daten mit Azure Cosmos DB
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 02/20/2020
ms.author: tisande
ms.openlocfilehash: 2cf682a404154b9c1bb94680b3adb673892c1c72
ms.sourcegitcommit: f27b045f7425d1d639cf0ff4bcf4752bf4d962d2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/23/2020
ms.locfileid: "77566475"
---
# <a name="index-geospatial-data-with-azure-cosmos-db"></a>Indizieren von räumlichen Daten mit Azure Cosmos DB

Wir haben die Datenbank-Engine von Azure Cosmos DB so entworfen, dass sie vollständig vom Schema unabhängig ist und erstklassige Unterstützung für JSON bietet. Die für Schreibvorgänge optimierte Datenbank-Engine von Azure Cosmos DB bietet jetzt auch native Unterstützung für räumliche Daten, die gemäß GeoJSON-Standard dargestellt sind.

Kurz gesagt, die Geometrie wird von geodätischen Koordinaten auf eine 2D-Ebene projiziert und dann schrittweise mithilfe eines **Quadtrees**in Zellen unterteilt. Diese Zellen werden zu 1D basierend auf der Position der Zelle auf einer **raumfüllenden Hilbert-Kurve**zugeordnet, die die Lage von Punkten beibehält. Wenn Positionsdaten darüber hinaus indiziert werden, durchlaufen sie einen als **Mosaikarbeit** bezeichneten Prozess, bei dem alle Zellen, die eine Position schneiden, im Azure Cosmos DB-Index als Schlüssel indiziert und gespeichert werden. Zur Abfragezeit werden Argumente wie Punkte und Polygone auch in den Mosaikprozess einbezogen, um die entsprechenden Zellen-ID-Bereiche zu extrahieren, und dann zum Abrufen von Daten aus dem Index verwendet.

Wenn Sie eine Indizierungsrichtlinie angeben, die einen räumlichen Index für „/*“ (alle Pfade) enthält, werden alle im Container gefundenen Daten für effiziente räumliche Abfragen indiziert.

> [!NOTE]
> Azure Cosmos DB unterstützt die Indizierung von Punkten, Linienfolgen (LineStrings), Polygonen und Multipolygonen.
>
>

## <a name="geography-data-indexing-examples"></a>Beispiele für die Indizierung räumlicher Daten

Der folgende JSON-Codeausschnitt zeigt eine Indizierungsrichtlinie mit aktivierter räumlicher Indizierung für den Datentyp **geography**. Er ist für räumliche Daten mit dem Datentyp „geography“ vorgesehen und indiziert alle GeoJSON-Punkte, -Polygone, -Multipolygone oder -LineStrings, die in Dokumenten für räumliche Abfragen gefunden werden. Wenn Sie die Indizierungsrichtlinie im Azure-Portal ändern, können Sie den folgenden JSON-Code für die Indizierungsrichtlinie angeben, um die räumliche Indizierung für Ihren Container zu aktivieren:

**JSON-Richtlinie für die Containerindizierung mit räumlicher Indizierung mit dem Datentyp „geography“**

```json
    {
       "automatic":true,
       "indexingMode":"Consistent",
        "includedPaths": [
        {
            "path": "/*"
        }
        ],
        "spatialIndexes": [
        {
            "path": "/*",
            "types": [
                "Point",
                "Polygon",
                "MultiPolygon",
                "LineString"
            ]
        }
    ],
       "excludedPaths":[]
    }
```

> [!NOTE]
> Wenn der GeoJSON-Wert "location" innerhalb des Dokuments fehlerhaft oder ungültig ist, wird er nicht für räumliche Abfragen indiziert. Sie können Werte von "location" mit ST_ISVALID und ST_ISVALIDDETAILED überprüfen.
>
>
>

Sie können die Indizierungsrichtlinie mit der Azure-Befehlszeilenschnittstelle, PowerShell oder einem beliebigen SDK [ändern](how-to-manage-indexing-policy.md).

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie die ersten Schritte mit räumlichen Daten in Azure Cosmos DB erlernt haben, haben Sie folgende Möglichkeiten:

* Weitere Informationen zur [Azure Cosmos DB-Abfrage](sql-query-getting-started.md)
* Weitere Informationen zum [Abfragen räumlicher Daten mit Azure Cosmos DB](sql-query-geospatial-query.md)
* Weitere Informationen zu [Räumlichen Daten und GeoJSON-Standortdaten in Azure Cosmos DB](sql-query-geospatial-intro.md)