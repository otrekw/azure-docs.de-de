---
title: Behandeln von Problemen bei Verwendung von Azure Cosmos DB
description: Erfahren Sie, wie Sie Probleme mit SQL-Abfragen in Azure Cosmos DB identifizieren, diagnostizieren und beheben.
author: timsander1
ms.service: cosmos-db
ms.topic: troubleshooting
ms.date: 02/10/2020
ms.author: tisande
ms.subservice: cosmosdb-sql
ms.reviewer: sngun
ms.openlocfilehash: 852ed8c49eda7f13542eb0bad63d84e1cf770e92
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80131376"
---
# <a name="troubleshoot-query-issues-when-using-azure-cosmos-db"></a>Behandeln von Problemen bei Verwendung von Azure Cosmos DB

In diesem Artikel werden Sie durch einen allgemeinen empfohlenen Ansatz für die Problembehandlung von Abfragen in Azure Cosmos DB geführt. Sie sollten die in diesem Artikel beschriebenen Schritte zwar nicht als umfassende Verteidigung gegen mögliche Abfrageprobleme ansehen, hier sind jedoch die gängigsten Leistungstipps zusammengefasst. Sie können diesen Artikel als Ausgangspunkt für das Troubleshooting von langsamen oder aufwendigen Abfragen in der Core (SQL)-API von Azure Cosmos DB verwenden. Sie können auch [Diagnoseprotokolle](cosmosdb-monitor-resource-logs.md) verwenden, um Abfragen zu identifizieren, die langsam sind oder beträchtliche Durchsatzmengen verbrauchen.

Sie können Abfrageoptimierungen in Azure Cosmos DB umfassend kategorisieren: 

- Optimierungen, die die Anzahl der verbrauchten Anforderungseinheiten (Request Units, RUs) der Abfrage verringern
- Optimierungen ausschließlich zum Verringern der Latenz

Durch Reduzieren der Anzahl der verbrauchten RUs einer Abfrage können Sie mit ziemlicher Sicherheit auch die Latenz verringern.

Dieser Artikel enthält Beispiele, die Sie mit dem Dataset [Nutrition](https://github.com/CosmosDB/labs/blob/master/dotnet/setup/NutritionData.json) neu erstellen können.

## <a name="important"></a>Wichtig

- Befolgen Sie unsere [Tipps zur Leistungssteigerung](performance-tips.md), um die bestmögliche Leistung zu erzielen.
    > [!NOTE]
    > Für eine bessere Leistung wird die Windows-64-Bit-Hostverarbeitung empfohlen. Das SQL SDK enthält die native Datei „ServiceInterop.dll“, um Abfragen lokal zu analysieren und zu optimieren. „ServiceInterop.dll“ wird nur auf der Windows x64-Plattform unterstützt. Bei Linux und anderen nicht unterstützten Plattformen, bei denen die Datei „ServiceInterop.dll“ nicht verfügbar ist, erfolgt ein zusätzlicher Netzwerkaufruf an das Gateway, um die optimierte Abfrage zu erhalten.
- Bei der Azure Cosmos DB-Abfrage wird keine Mindestelementanzahl unterstützt.
    - Der Code sollte jede Seitengröße von 0 bis zur maximalen Anzahl von Elementen verarbeiten können.
    - Die Anzahl der Elemente auf einer Seite kann und wird ohne vorherige Ankündigung geändert.
- Leere Seiten sind bei Abfragen zu erwarten und können jederzeit auftreten.
    - Leere Seiten werden in SDKs verfügbar gemacht, da sie so mehr Gelegenheiten zum Abbrechen einer Abfrage bieten. Außerdem wird so deutlich, dass das SDK mehrere Netzwerkaufrufe tätigt.
    - Leere Seiten können in vorhandenen Workloads auftreten, wenn eine physische Partition in Azure Cosmos DB aufgeteilt wird. Die erste Partition enthält 0 Ergebnisse und verursacht die leere Seite.
    - Leere Seiten werden dadurch verursacht, dass das Back-End eine Abfrage vorzeitig entfernt, da diese mehr als eine bestimmte Zeitspanne auf dem Back-End zum Abrufen der Dokumente benötigt. Wenn Azure Cosmos DB eine Abfrage vorzeitig entfernt, wird ein Fortsetzungstoken zurückgegeben, mit dem die Abfrage fortgesetzt werden kann.
- Stellen Sie sicher, die Abfrage vollständig ablaufen zu lassen. Sehen Sie sich die SDK-Beispiele an, und verwenden Sie eine `while`-Schleife für `FeedIterator.HasMoreResults`, um die gesamte Abfrage ablaufen zu lassen.

## <a name="get-query-metrics"></a>Abrufen von Abfragemetriken

Beim Optimieren einer Abfrage in Azure Cosmos DB ist der erste Schritt immer das [Abrufen der Abfragemetriken](profile-sql-api-query.md) für Ihre Abfrage. Diese Metriken sind auch über das Azure-Portal verfügbar:

[ ![Abrufen von Abfragemetriken](./media/troubleshoot-query-performance/obtain-query-metrics.png) ](./media/troubleshoot-query-performance/obtain-query-metrics.png#lightbox)

Vergleichen Sie nach dem Abrufen der Abfragemetriken die Anzahl der abgerufenen Dokumente mit der Anzahl der Ausgabedokumente für Ihre Abfrage. Verwenden Sie diesen Vergleich zum Identifizieren der relevanten Abschnitte, auf die in diesem Artikel verwiesen werden soll.

Bei der Anzahl der abgerufenen Dokumente handelt es sich um die Anzahl von Dokumenten, die für die Abfrage geladen werden mussten. Bei der Anzahl der Ausgabedokumente handelt es sich um die Anzahl von Dokumenten, die für die Ergebnisse der Abfrage benötigt wurden. Wenn die Anzahl der abgerufenen Dokumente deutlich höher ist als die Anzahl der Ausgabedokumente, konnte mindestens ein Teil der Abfrage den Index nicht nutzen, und es musste eine Überprüfung durchgeführt werden.

Die folgenden Abschnitte sollen Ihnen helfen, die relevanten Abfrageoptimierungen für Ihr Szenario besser zu verstehen.

### <a name="querys-ru-charge-is-too-high"></a>Die Anzahl der verbrauchten RUs der Abfrage ist zu hoch

#### <a name="retrieved-document-count-is-significantly-higher-than-output-document-count"></a>Die Anzahl der abgerufenen Dokumente ist deutlich höher als die Anzahl der Ausgabedokumente

- [Schließen Sie die erforderlichen Pfade in die Indizierungsrichtlinie ein.](#include-necessary-paths-in-the-indexing-policy)

- [Informieren Sie sich, welche Systemfunktionen den Index verwenden.](#understand-which-system-functions-use-the-index)

- [Ändern Sie Abfragen mit einem Filter und einer ORDER BY-Klausel.](#modify-queries-that-have-both-a-filter-and-an-order-by-clause)

- [Optimieren Sie JOIN-Ausdrücken mithilfe einer Unterabfrage.](#optimize-join-expressions-by-using-a-subquery)

<br>

#### <a name="retrieved-document-count-is-approximately-equal-to-output-document-count"></a>Die Anzahl der abgerufenen Dokumente entspricht ungefähr der Anzahl der Ausgabedokumente

- [Vermeiden Sie partitionsübergreifende Abfragen.](#avoid-cross-partition-queries)

- [Optimieren Sie Abfragen mit Filtern für mehrere Eigenschaften.](#optimize-queries-that-have-filters-on-multiple-properties)

- [Ändern Sie Abfragen mit einem Filter und einer ORDER BY-Klausel.](#modify-queries-that-have-both-a-filter-and-an-order-by-clause)

<br>

### <a name="querys-ru-charge-is-acceptable-but-latency-is-still-too-high"></a>Die Anzahl der verbrauchten RUs der Abfrage ist akzeptabel, aber die Latenz ist immer noch zu hoch

- [Erhöhen Sie die Nähe.](#improve-proximity)

- [Erhöhen Sie den bereitgestellten Durchsatz.](#increase-provisioned-throughput)

- [Erhöhen Sie den Wert für MaxConcurrency.](#increase-maxconcurrency)

- [Erhöhen Sie den Wert für MaxBufferedItemCount.](#increase-maxbuffereditemcount)

## <a name="queries-where-retrieved-document-count-exceeds-output-document-count"></a>Abfragen, bei denen die Anzahl der abgerufenen Dokumente die Anzahl der Ausgabedokumente überschreitet

 Bei der Anzahl der abgerufenen Dokumente handelt es sich um die Anzahl von Dokumenten, die für die Abfrage geladen werden mussten. Bei der Anzahl der Ausgabedokumente handelt es sich um die Anzahl von Dokumenten, die für die Ergebnisse der Abfrage benötigt wurden. Wenn die Anzahl der abgerufenen Dokumente deutlich höher ist als die Anzahl der Ausgabedokumente, konnte mindestens ein Teil der Abfrage den Index nicht nutzen, und es musste eine Überprüfung durchgeführt werden.

Nachstehend finden Sie ein Beispiel für eine Überprüfungsabfrage, die nicht vollständig vom Index bedient wurde:

Abfrage:

 ```sql
SELECT VALUE c.description
FROM c
WHERE UPPER(c.description) = "BABYFOOD, DESSERT, FRUIT DESSERT, WITHOUT ASCORBIC ACID, JUNIOR"
 ```

Abfragemetriken:

```
Retrieved Document Count                 :          60,951
Retrieved Document Size                  :     399,998,938 bytes
Output Document Count                    :               7
Output Document Size                     :             510 bytes
Index Utilization                        :            0.00 %
Total Query Execution Time               :        4,500.34 milliseconds
  Query Preparation Times
    Query Compilation Time               :            0.09 milliseconds
    Logical Plan Build Time              :            0.05 milliseconds
    Physical Plan Build Time             :            0.04 milliseconds
    Query Optimization Time              :            0.01 milliseconds
  Index Lookup Time                      :            0.01 milliseconds
  Document Load Time                     :        4,177.66 milliseconds
  Runtime Execution Times
    Query Engine Times                   :          322.16 milliseconds
    System Function Execution Time       :           85.74 milliseconds
    User-defined Function Execution Time :            0.00 milliseconds
  Document Write Time                    :            0.01 milliseconds
Client Side Metrics
  Retry Count                            :               0
  Request Charge                         :        4,059.95 RUs
```

Die Anzahl abgerufener Dokumente (60.951) ist deutlich höher als die Anzahl der Ausgabedokumente (7). Daher musste diese Abfrage eine Überprüfung durchführen. In diesem Fall wird der Index von der Systemfunktion [UPPER()](sql-query-upper.md) nicht verwendet.

### <a name="include-necessary-paths-in-the-indexing-policy"></a>Einschließen der erforderlichen Pfade in die Indizierungsrichtlinie

Ihre Indizierungsrichtlinie sollte alle Eigenschaften abdecken, die in `WHERE`-Klauseln, `ORDER BY`-Klauseln, `JOIN` und den meisten Systemfunktionen enthalten sind. Der in der Indizierungsrichtlinie angegebene Pfad sollte der Eigenschaft in den JSON-Dokumenten entsprechen (Groß-/Kleinschreibung beachten).

Wenn Sie eine einfache Abfrage für das [Nutrition](https://github.com/CosmosDB/labs/blob/master/dotnet/setup/NutritionData.json)-Dataset ausführen, können Sie eine deutlich niedrigere Anzahl der verbrauchten RUs beobachten, wenn die Eigenschaft in der `WHERE`-Klausel indiziert wird:

#### <a name="original"></a>Original

Abfrage:

```sql
SELECT * FROM c WHERE c.description = "Malabar spinach, cooked"
```

Indizierungsrichtlinie:

```json
{
    "indexingMode": "consistent",
    "automatic": true,
    "includedPaths": [
        {
            "path": "/*"
        }
    ],
    "excludedPaths": [
        {
            "path": "/description/*"
        }
    ]
}
```

**Verbrauchte RUs:** 409,51 RUs

#### <a name="optimized"></a>Optimiert

Aktualisierte Indizierungsrichtlinie:

```json
{
    "indexingMode": "consistent",
    "automatic": true,
    "includedPaths": [
        {
            "path": "/*"
        }
    ],
    "excludedPaths": []
}
```

**Verbrauchte RUs:** 2,98 RUs

Sie können der Indizierungsrichtlinie jederzeit Eigenschaften hinzufügen, ohne dass sich dies auf die Schreibverfügbarkeit oder die Leistung auswirkt. Wenn Sie dem Index eine neue Eigenschaft hinzufügen, nutzen Abfragen, die diese Eigenschaft verwenden, den neuen verfügbaren Index sofort. Die Abfrage verwendet den neuen Index, während er erstellt wird. Daher können die Abfrageergebnisse während der Indexneuerstellung inkonsistent sein. Wenn eine neue Eigenschaft indiziert wird, sind Abfragen, die nur vorhandene Indizes verwenden, während der Indexneuerstellung nicht betroffen. Sie können den [Fortschritt der Indextransformation nachverfolgen](https://docs.microsoft.com/azure/cosmos-db/how-to-manage-indexing-policy#use-the-net-sdk-v3).

### <a name="understand-which-system-functions-use-the-index"></a>Ermitteln der Systemfunktionen, die den Index verwenden

Wenn der Ausdruck in einen Bereich von Zeichenfolgenwerten übersetzt werden kann, kann der Index genutzt werden. Andernfalls ist dies nicht möglich.

Die Zeichenfolgenfunktionen in der folgenden Liste können den Index verwenden:

- STARTSWITH(str_expr, str_expr)
- LEFT(str_expr, num_expr) = str_expr
- SUBSTRING(str_expr, num_expr, num_expr) = str_expr, aber nur, wenn der erste num_expr-Wert „0“ ist

Nachstehend sind einige allgemeine Systemfunktionen aufgeführt, die den Index nicht verwenden und jedes Dokument laden müssen:

| **Systemfunktion**                     | **Vorschläge für Optimierungen**             |
| --------------------------------------- |------------------------------------------------------------ |
| CONTAINS                                | Verwenden Sie Azure Search für die Volltextsuche.                        |
| UPPER/LOWER                             | Normalisieren Sie die Groß-/Kleinschreibung beim Einfügen, statt die Systemfunktion zum Normalisieren der Daten für Vergleiche zu verwenden. Eine Abfrage wie ```SELECT * FROM c WHERE UPPER(c.name) = 'BOB'``` wird zu ```SELECT * FROM c WHERE c.name = 'BOB'```. |
| Mathematische Funktionen (Nicht-Aggregate) | Wenn Sie in der Abfrage einen Wert häufig berechnen müssen, sollten Sie diesen Wert als Eigenschaft in Ihrem JSON-Dokument speichern. |

------

Andere Teile der Abfrage verwenden möglicherweise weiterhin den Index, auch wenn die für die Systemfunktionen nicht gilt.

### <a name="modify-queries-that-have-both-a-filter-and-an-order-by-clause"></a>Ändern von Abfragen mit einem Filter und einer ORDER BY-Klausel

Obwohl Abfragen mit einem Filter und einer `ORDER BY`-Klausel normalerweise einen Bereichsindex verwenden, sind sie effizienter, wenn sie von einem zusammengesetzten Index bedient werden können. Zusätzlich zum Ändern der Indizierungsrichtlinie sollten Sie der `ORDER BY`-Klausel alle Eigenschaften im zusammengesetzten Index hinzufügen. Durch diese Änderung der Abfrage wird sichergestellt, dass der zusammengesetzte Index verwendet wird.  Sie können die Auswirkung beobachten, indem Sie eine Abfrage für das [Nutrition](https://github.com/CosmosDB/labs/blob/master/dotnet/setup/NutritionData.json)-Dataset ausführen:

#### <a name="original"></a>Original

Abfrage:

```sql
SELECT * FROM c WHERE c.foodGroup = "Soups, Sauces, and Gravies" ORDER BY c._ts ASC
```

Indizierungsrichtlinie:

```json
{

        "automatic":true,
        "indexingMode":"Consistent",
        "includedPaths":[  
            {  
                "path":"/*"
            }
        ],
        "excludedPaths":[]
}
```

**Verbrauchte RUs:** 44,28 RUs

#### <a name="optimized"></a>Optimiert

Aktualisierte Abfrage (enthält beide Eigenschaften in der `ORDER BY`-Klausel):

```sql
SELECT * FROM c
WHERE c.foodGroup = "Soups, Sauces, and Gravies"
ORDER BY c.foodGroup, c._ts ASC
```

Aktualisierte Indizierungsrichtlinie:

```json
{  
        "automatic":true,
        "indexingMode":"Consistent",
        "includedPaths":[  
            {  
                "path":"/*"
            }
        ],
        "excludedPaths":[],
        "compositeIndexes":[  
            [  
                {  
                    "path":"/foodGroup",
                    "order":"ascending"
        },
                {  
                    "path":"/_ts",
                    "order":"ascending"
                }
            ]
        ]
    }

```

**Verbrauchte RUs:** 8,86 RUs

### <a name="optimize-join-expressions-by-using-a-subquery"></a>Optimieren von JOIN-Ausdrücken mithilfe einer Unterabfrage
Mehrwertige Unterabfragen können `JOIN`-Ausdrücke optimieren, indem Prädikate nach jedem select-many-Ausdruck statt nach allen Kreuzprodukten in der `WHERE`-Klausel gepusht werden.

Angenommen, die folgende Abfrage wird ausgeführt:

```sql
SELECT Count(1) AS Count
FROM c
JOIN t IN c.tags
JOIN n IN c.nutrients
JOIN s IN c.servings
WHERE t.name = 'infant formula' AND (n.nutritionValue > 0
AND n.nutritionValue < 10) AND s.amount > 1
```

**Verbrauchte RUs:** 167,62 RUs

Für diese Abfrage vergleicht der Index jedes Dokument, das ein Tag mit dem Namen „infant formula“, einen „nutritionValue“ größer als 0 und eine Portionsmenge größer als 1 enthält. Der `JOIN`-Ausdruck ermittelt hier das Kreuzprodukt aller Elemente der tags-, nutrients- und servings-Arrays für jedes übereinstimmende Dokument, bevor ein Filter angewendet wird. Die `WHERE`-Klausel wendet dann das Filterprädikat auf jedes `<c, t, n, s>`-Tupel an.

Wenn z. B. ein übereinstimmendes Dokument 10 Elemente in jedem der drei Arrays hat, wird auf 1 × 10 × 10 × 10 (d. h. 1.000) Tupel erweitert. Die Verwendung von Unterabfragen kann hier helfen, indem verknüpfte Arrayelemente herausgefiltert werden, bevor der Joinvorgang mit dem nächsten Ausdruck ausgeführt wird.

Diese Abfrage entspricht der vorherigen, verwendet aber Unterabfragen:

```sql
SELECT Count(1) AS Count
FROM c
JOIN (SELECT VALUE t FROM t IN c.tags WHERE t.name = 'infant formula')
JOIN (SELECT VALUE n FROM n IN c.nutrients WHERE n.nutritionValue > 0 AND n.nutritionValue < 10)
JOIN (SELECT VALUE s FROM s IN c.servings WHERE s.amount > 1)
```

**Verbrauchte RUs:** 22,17 RUs

Es wird davon ausgegangen, dass nur ein Element im tags-Array dem Filter entspricht, und es gibt fünf Elemente sowohl für das nutrients- als auch das servings-Array. Die `JOIN`-Ausdrücke werden im Gegensatz zu 1.000 Elementen in der ersten Abfrage auf 1 × 1 × 5 × 5 = 25 Elemente erweitert.

## <a name="queries-where-retrieved-document-count-is-equal-to-output-document-count"></a>Abfragen, bei denen die Anzahl der abgerufenen Dokumente der Anzahl der Ausgabedokumente entspricht

Wenn die Anzahl der abgerufenen Dokumente ungefähr der Anzahl der Ausgabedokumente entspricht, musste die Abfrage nicht viele unnötige Dokumente überprüfen. Bei vielen Abfragen, z. B. bei Abfragen, die das TOP-Schlüsselwort verwenden, kann die Anzahl der abgerufenen Dokumente die Anzahl der Ausgabedokumente um 1 überschreiten. Sie müssen sich darum nicht kümmern.

### <a name="avoid-cross-partition-queries"></a>Vermeiden partitionsübergreifender Abfragen

Azure Cosmos DB verwendet die [Partitionierung](partitioning-overview.md), um einzelne Container zu skalieren, wenn die Anzahl von Anforderungseinheiten und die Datenspeicheranforderungen steigen. Jede physische Partition verfügt über einen separaten und unabhängigen Index. Wenn Ihre Abfrage einen Gleichheitsfilter enthält, der mit dem Partitionsschlüssel Ihres Containers übereinstimmt, müssen Sie nur den Index der relevanten Partition überprüfen. Durch diese Optimierung wird die Gesamtanzahl der für die Abfrage erforderlichen RUs reduziert.

Im Fall einer großen Anzahl von bereitgestellten Anforderungseinheiten (über 30.000) oder einer großen Menge an gespeicherten Daten (über ca. 100 GB) verfügen Sie wahrscheinlich über einen ausreichend großen Container, um eine deutliche Reduzierung der Anzahl der verbrauchten RUs für die Abfrage zu erreichen.

Wenn Sie beispielsweise einen Container mit dem Partitionsschlüssel „foodGroup“ erstellen, müssen die folgenden Abfragen nur eine einzige physische Partition überprüfen:

```sql
SELECT * FROM c
WHERE c.foodGroup = "Soups, Sauces, and Gravies" and c.description = "Mushroom, oyster, raw"
```

Diese Abfragen könnten auch durch Hinzufügen des Partitionsschlüssels zur Abfrage optimiert werden:

```sql
SELECT * FROM c
WHERE c.foodGroup IN("Soups, Sauces, and Gravies", "Vegetables and Vegetable Products") and c.description = "Mushroom, oyster, raw"
```

Abfragen, die Bereichsfilter für den Partitionsschlüssel oder keine Filter für den Partitionsschlüssel enthalten, müssen den Index jeder physischen Partition auf Ergebnisse überprüfen:

```sql
SELECT * FROM c
WHERE c.description = "Mushroom, oyster, raw"
```

```sql
SELECT * FROM c
WHERE c.foodGroup > "Soups, Sauces, and Gravies" and c.description = "Mushroom, oyster, raw"
```

### <a name="optimize-queries-that-have-filters-on-multiple-properties"></a>Optimieren von Abfragen mit Filtern für mehrere Eigenschaften

Obwohl Abfragen mit Filtern für mehrere Eigenschaften normalerweise einen Bereichsindex verwenden, sind sie effizienter, wenn sie von einem zusammengesetzten Index bedient werden können. Bei kleinen Datenmengen hat diese Optimierung keine spürbaren Auswirkungen. Bei großen Datenmengen kann sie sich jedoch als hilfreich erweisen. Sie können höchstens einen Ungleichheitsfilter pro zusammengesetztem Index optimieren. Wenn Ihre Abfrage mehrere Ungleichheitsfilter enthält, wählen Sie einen davon aus, der den zusammengesetzten Index verwendet. Der Rest verwendet weiterhin Bereichsindizes. Der Ungleichheitsfilter muss im zusammengesetzten Index zuletzt definiert werden. Erfahren Sie mehr über [zusammengesetzte Indizes](index-policy.md#composite-indexes).

Nachstehend finden Sie einige Beispiele für Abfragen, die mit einem zusammengesetzten Index optimiert werden könnten:

```sql
SELECT * FROM c
WHERE c.foodGroup = "Vegetables and Vegetable Products" AND c._ts = 1575503264
```

```sql
SELECT * FROM c
WHERE c.foodGroup = "Vegetables and Vegetable Products" AND c._ts > 1575503264
```

Hier ist der entsprechende zusammengesetzte Index:

```json
{  
        "automatic":true,
        "indexingMode":"Consistent",
        "includedPaths":[  
            {  
                "path":"/*"
            }
        ],
        "excludedPaths":[],
        "compositeIndexes":[  
            [  
                {  
                    "path":"/foodGroup",
                    "order":"ascending"
                },
                {  
                    "path":"/_ts",
                    "order":"ascending"
                }
            ]
        ]
}
```

## <a name="optimizations-that-reduce-query-latency"></a>Optimierungen zum Verringern der Abfragelatenz

In vielen Fällen ist die Anzahl der verbrauchten RUs akzeptabel, aber die Abfragelatenz immer noch zu hoch. Die folgenden Abschnitte bieten eine Übersicht über Tipps zum Verringern der Abfragelatenz. Wenn Sie dieselbe Abfrage mehrmals für dasselbe Dataset ausführen, wird jedes Mal die gleiche Anzahl von RUs verbraucht. Die Abfragelatenz kann zwischen den einzelnen Abfrageausführungen variieren.

### <a name="improve-proximity"></a>Verbessern der Nähe

Abfragen, die in einer anderen Region als das Azure Cosmos DB-Konto ausgeführt werden, haben eine höhere Latenz als in derselben Region ausgeführte Abfragen. Wenn Sie beispielsweise Code auf Ihrem Desktopcomputer ausgeführt haben, sollten Sie eine um zehn oder Hunderte (oder mehr) Millisekunden höhere Latenz erwarten, als wenn die Abfrage auf einem virtuellen Computer in derselben Azure-Region wie Azure Cosmos DB ausgeführt wurde. Es ist einfach, [Daten in Azure Cosmos DB global zu verteilen](distribute-data-globally.md), um sicherzustellen, dass sich Ihre Daten näher an Ihrer App befinden.

### <a name="increase-provisioned-throughput"></a>Erhöhen des bereitgestellten Durchsatzes

In Azure Cosmos DB wird der bereitgestellte Durchsatz in Anforderungseinheiten (Request Units, RUs) gemessen. Angenommen, Sie verfügen über eine Abfrage, die 5 RUs an Durchsatz verbraucht. Wenn Sie z. B. 1.000 RUs bereitstellen, könnten Sie diese Abfrage 200-mal pro Sekunde ausführen. Wenn Sie versuchen würden, die Abfrage auszuführen, wenn nicht genug Durchsatz verfügbar ist, würde Azure Cosmos DB einen HTTP-Fehler mit dem Statuscode 429 zurückgeben. Alle aktuellen Core (SQL)-API SDKs versuchen, diese Abfrage nach einer kurzen Wartezeit automatisch erneut auszuführen. Gedrosselte Anforderungen nehmen mehr Zeit in Anspruch, sodass eine Erhöhung des bereitgestellten Durchsatzes die Abfragelatenz verbessern kann. Im Azure-Portal können Sie auf dem Blatt **Metriken** die [Gesamtanzahl der (gedrosselten) Anforderungen](use-metrics.md#understand-how-many-requests-are-succeeding-or-causing-errors) anzeigen.

### <a name="increase-maxconcurrency"></a>Erhöhen des Werts für MaxConcurrency

Bei parallelen Abfragen werden mehrere Partitionen parallel abgefragt. Die Daten einer individuell partitionierten Sammlung werden in Bezug auf die Abfrage aber seriell abgerufen. Wenn Sie MaxConcurrency also auf die Anzahl von Partitionen festlegen, ist die Wahrscheinlichkeit am höchsten, dass die bestmögliche Leistung für die Abfrage erzielt wird, sofern alle anderen Systembedingungen unverändert bleiben. Wenn Sie die Anzahl der Partitionen nicht kennen, können Sie MaxConcurrency (oder bei älteren SDK-Versionen MaxDegreesOfParallelism) auf eine hohe Zahl festlegen. Das System wählt den Mindestwert (Anzahl der Partitionen, vom Benutzer bereitgestellte Eingabe) als maximalen Grad an Parallelität aus.

### <a name="increase-maxbuffereditemcount"></a>Erhöhen des Werts für MaxBufferedItemCount

Abfragen sind so konzipiert, dass Ergebnisse vorab abgerufen werden, während der Client den aktuellen Batch der Ergebnisse verarbeitet. Ein Vorababruf führt zu einer Verbesserung der allgemeinen Latenz einer Abfrage. Durch Festlegen von MaxBufferedItemCount wird die Anzahl von vorab abgerufenen Ergebnissen begrenzt. Wenn Sie diesen Wert auf die erwartete Anzahl von zurückgegebenen Ergebnissen (oder eine höhere Anzahl) festlegen, ist der Vorteil durch das vorherige Abrufen für die Abfrage am größten. Wenn Sie diesen Wert auf „-1“ festlegen, kann das System die Anzahl der zu puffernden Elemente automatisch festlegen.

## <a name="next-steps"></a>Nächste Schritte
Die folgenden Artikel enthalten u. a. Informationen dazu, wie Sie RUs pro Abfrage messen und Ausführungsstatistiken zum Optimieren Ihrer Abfragen erhalten:

* [Abrufen von SQL-Abfrageausführungsmetriken und Analysieren der Abfrageleistung mit dem .NET SDK](profile-sql-api-query.md)
* [Optimieren der Abfrageleistung mit Azure Cosmos DB](sql-api-sql-query-metrics.md)
* [Tipps zur Leistungssteigerung für das .NET SDK](performance-tips.md)
