---
title: Abfragen des Zwillingsgraphen
titleSuffix: Azure Digital Twins
description: Erfahren Sie, wie Sie Informationen im Zwillingsdiagramm von Azure Digital Twins abfragen.
author: baanders
ms.author: baanders
ms.date: 3/26/2020
ms.topic: conceptual
ms.service: digital-twins
ms.openlocfilehash: 05bcbf8df695ba308a6eaff5e7401f0a6d638747
ms.sourcegitcommit: 46f8457ccb224eb000799ec81ed5b3ea93a6f06f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87337601"
---
# <a name="query-the-azure-digital-twins-twin-graph"></a>Abfragen des Zwillingsdiagramms von Azure Digital Twins

Dieser Artikel bietet Beispiele und ausführliche Informationen dazu, wie Sie die [Abfragedatenspeicher-Sprache von Azure Digital Twins](concepts-query-language.md) verwenden, um Informationen im [Zwillingsdiagramm](concepts-twins-graph.md) abzufragen. Sie führen Abfragen im Diagramm mithilfe der [**Abfrage-APIs**](how-to-use-apis-sdks.md) von Azure Digital Twins aus.

## <a name="query-syntax"></a>Abfragesyntax

Im Folgenden finden Sie einige Beispielabfragen, die die Struktur der Abfragesprache veranschaulichen und mögliche Abfragevorgänge durchführen.

Abrufen von [digitalen Zwillingen](concepts-twins-graph.md) nach Eigenschaften (einschließlich ID und Metadaten):
```sql
SELECT  * 
FROM DigitalTwins T  
WHERE T.firmwareVersion = '1.1'
AND T.$dtId in ['123', '456']
AND T.Temperature = 70
```

Abrufen von digitalen Zwillingen nach [Modell](concepts-models.md)
```sql
SELECT  * 
FROM DigitalTwins T  
WHERE IS_OF_MODEL(T , 'dtmi:com:contoso:Space;3')
AND T.roomSize > 50
```

> [!TIP]
> Die ID eines digitalen Zwillings wird mithilfe des Metadatenfelds `$dtId` abgefragt.

### <a name="query-based-on-relationships"></a>Abfrage basierend auf Beziehungen

Für Abfragen anhand der Beziehungen von digitalen Zwillingen weist die Abfragespeichersprache von Azure Digital Twins eine besondere Syntax auf.

Beziehungen werden in der `FROM`-Klausel in den Abfragebereich übernommen. Ein wichtiger Unterschied zu „klassischen“ SQL-artigen Sprachen besteht darin, dass die Ausdrücke in dieser `FROM`-Klausel keine Tabellen sind. Stattdessen drückt die `FROM`-Klausel einen entitätsübergreifenden Beziehungsdurchlauf aus, der mit einer Azure Digital Twins-Version von `JOIN` geschrieben wird. 

Denken Sie daran, dass in den Funktionen des Azure Digital Twins-[Modells](concepts-models.md) Beziehungen nur in Abhängigkeit von Zwillingen vorhanden sind. Dies bedeutet, dass sich `JOIN` in der Abfragespeichersprache von Azure Digital Twins leicht von `JOIN` in allgemeiner SQL-Form unterscheidet, da Beziehungen hier nicht unabhängig abgefragt werden können und an einen Zwilling gebunden sein müssen.
Um diesen Unterschied zu berücksichtigen, wird das Schlüsselwort `RELATED` in der `JOIN`-Klausel verwendet, um auf den Beziehungssatz eines Zwillings zu verweisen. 

Der folgende Abschnitt enthält einige Beispiele dafür.

> [!TIP]
> Konzeptionell imitiert dieses Feature die dokumentzentrische Funktionalität von Cosmos DB, bei der `JOIN` für untergeordnete Objekte innerhalb eines Dokuments ausgeführt werden kann. Cosmos DB gibt über das Schlüsselwort `IN` an, dass `JOIN` Arrayelemente im aktuellen Kontextdokument durchlaufen soll.

#### <a name="relationship-based-query-examples"></a>Beispiele für beziehungsbasierte Abfragen

Um ein Dataset mit Beziehungen zu erhalten, verwenden Sie eine einzelne `FROM`-Anweisung gefolgt von N `JOIN`-Anweisungen, wobei die `JOIN`-Anweisungen Beziehungen zum Ergebnis einer vorherigen `FROM`- oder `JOIN`-Anweisung ausdrücken.

Hier ist ein Beispiel für eine beziehungsbasierte Abfrage. Mit diesem Codeausschnitt werden alle digitalen Zwillinge mit der *ID*-Eigenschaft „ABC“ und alle digitalen Zwillinge, die zu diesen digitalen Zwillingen eine *contains*-Beziehung haben, ausgewählt. 

```sql
SELECT T, CT
FROM DIGITALTWINS T
JOIN CT RELATED T.contains
WHERE T.$dtId = 'ABC' 
```

>[!NOTE] 
> Der Entwickler muss dieses `JOIN` nicht mit einem Schlüsselwert in der `WHERE`-Klausel korrelieren (oder in der `JOIN`-Definition einen Schlüsselwert angeben). Diese Korrelation wird automatisch vom System berechnet, da die Beziehungseigenschaften die Zielentität direkt identifizieren.

#### <a name="query-the-properties-of-a-relationship"></a>Abfragen der Eigenschaften einer Beziehung

Ähnlich wie die Eigenschaften digitaler Zwillinge über DTDL beschrieben werden, können Beziehungen ebenfalls über Eigenschaften verfügen. Die Abfragespeichersprache von Azure Digital Twins ermöglicht das Filtern und Projizieren von Beziehungen, indem der Beziehung innerhalb der `JOIN`-Klausel ein Alias zugewiesen wird. 

Als Beispiel sei eine *servicedBy*-Beziehung angenommen, die über die *reportedCondition*-Eigenschaft verfügt. In der nachstehenden Abfrage erhält diese Beziehung den Alias „R“, um auf die zugehörige-Eigenschaft zu verweisen.

```sql
SELECT T, SBT, R
FROM DIGITALTWINS T
JOIN SBT RELATED T.servicedBy R
WHERE T.$dtId = 'ABC' 
AND R.reportedCondition = 'clean'
```

Beachten Sie im obigen Beispiel, dass *reportedCondition* eine Eigenschaft der *servicedBy*-Beziehung selbst ist, NICHT eines digitalen Zwillings, der über eine *servicedBy*-Beziehung verfügt.

## <a name="run-queries-with-an-api-call"></a>Ausführen von Abfragen mit einem API-Aufruf

Nachdem Sie sich für eine Abfragezeichenfolge entschieden haben, führen Sie sie durch Aufrufen der **Abfrage-API** aus.
Der folgende Codeausschnitt veranschaulicht diesen Aufruf aus der Client-App:

```csharp
var client = new AzureDigitalTwinsAPIClient(<your-credentials>);
client.BaseUri = new Uri(<your-Azure-Digital-Twins-instance-URL>);

QuerySpecification spec = new QuerySpecification("SELECT * FROM digitaltwins");
QueryResult result = await client.Query.QueryTwinsAsync(spec);
```

Dieser Aufruf gibt Abfrageergebnisse in Form eines QueryResult-Objekts zurück. 

Abfrageaufrufe unterstützen Paging. Im Folgenden finden Sie ein vollständiges Beispiel mit Fehlerbehandlung und Paging:

```csharp
string query = "SELECT * FROM digitaltwins";
try
{
    AsyncPageable<string> qresult = client.QueryAsync(query);
    await foreach (string item in qresult) 
    {
        // Do something with each result
    }
}
catch (RequestFailedException e)
{
    Log.Error($"Error {e.Status}: {e.Message}");
    return null;
}
```

## <a name="query-limitations"></a>Abfrageeinschränkungen

Es kann bis zu 10 Sekunden dauern, bis Änderungen in Ihrer Instanz in Abfragen berücksichtigt werden. Wenn Sie beispielsweise einen Vorgang wie das Erstellen oder Löschen von Zwillingen mit der Digital Twins-API durchführen, wird das Ergebnis möglicherweise in Anforderungen an die Abfrage-API nicht sofort reflektiert. Zum Beheben dieser Einschränkung ist es normalerweise ausreichend, eine kurze Zeit zu warten.

Für die Verwendung von `JOIN` während der Vorschauphase gelten weitere Einschränkungen.
* In der `FROM`-Anweisung werden keine Unterabfragen unterstützt.
* `OUTER JOIN`-Semantik wird nicht unterstützt. Das bedeutet, dass die gesamte „Zeile“ aus dem Ausgaberesultset entfernt wird, wenn die Beziehung den Rang null aufweist.
* Während der Public Preview ist die Durchlauftiefe für den Graphen eingeschränkt: Pro Abfrage ist nur ein `JOIN` zulässig.
* Die Quelle für `JOIN`-Vorgänge ist eingeschränkt: Die Abfrage muss die Zwillinge deklarieren, in denen die Abfrage beginnt.

## <a name="query-best-practices"></a>Best Practices für Abfragen

Im Folgenden finden Sie einige Tipps für Abfragen mit Azure Digital Twins.

* Beachten Sie das Abfragemuster während der Modellentwurfsphase. Versuchen Sie nach Möglichkeit, Beziehungen, die in einer einzelnen Abfrage beantwortet werden müssen, als einstufige Beziehung zu modellieren.
* Entwerfen Sie Eigenschaften so, dass große Resultsets aus Diagrammdurchläufen vermieden werden.
* Sie können die Anzahl der erforderlichen Abfragen deutlich verringern, indem Sie ein Array aus Zwillingen erstellen und die Abfrage mit dem `IN`-Operator durchführen. Stellen Sie sich beispielsweise ein Szenario vor, in dem *Gebäude* aus *Etagen* und *Etagen* aus *Räumen* bestehen. Wenn Sie in einem Gebäude nach heißen Räumen suchen möchten, können Sie

    1. Etagen im Gebäude basierend auf `contains`-Beziehungen suchen.
        ```sql
        SELECT Floor
        FROM DIGITALTWINS Building
        JOIN Floor RELATED Building.contains
        WHERE Building.$dtId = @buildingId
        ``` 
    2. Wenn Sie Räume suchen möchten, können Sie eine Abfrage mit einer Sammlung der Etagen im Gebäude (in der nachstehenden Abfrage als *Floor* bezeichnet) ausführen, anstatt die Etagen einzeln zu überprüfen und jeweils eine `JOIN`-Abfrage zum Auffinden der Räume auszuführen.

        In der Client-App:
        ```csharp
        var floors = "['floor1','floor2', ..'floorn']"; 
        ```
        In der Abfrage:
        ```sql
        SELECT Room
        FROM DIGITALTWINS Floor
        JOIN Room RELATED Floor.contains
        WHERE Floor.$dtId IN ['floor1','floor2', ..'floorn']
        AND Room. Temperature > 72
        AND IS_OF_MODEL(Room, 'dtmi:com:contoso:Room;1')
        ```
* Bei Eigenschaftennamen und -werten wird die Groß-/Kleinschreibung beachtet. Verwenden Sie daher die genauen in den Modellen definierten Namen. Wenn Eigenschaftennamen falsch oder in falscher Groß-/Kleinschreibung geschrieben werden, ist das Resultset leer, und es werden keine Fehler zurückgegeben.


## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die [Azure Digital Twins-APIs und SDKs](how-to-use-apis-sdks.md), einschließlich der Abfrage-API, die zum Ausführen der Abfragen in diesem Artikel verwendet wird.
