---
title: Abfragen des Zwillingsgraphen
titleSuffix: Azure Digital Twins
description: Erfahren Sie, wie Sie Informationen im Zwillingsdiagramm von Azure Digital Twins abfragen.
author: baanders
ms.author: baanders
ms.date: 3/26/2020
ms.topic: conceptual
ms.service: digital-twins
ms.openlocfilehash: e6236d9ed5ed75b6b5e10914e668de545c48fc2c
ms.sourcegitcommit: 420c30c760caf5742ba2e71f18cfd7649d1ead8a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2020
ms.locfileid: "89055633"
---
# <a name="query-the-azure-digital-twins-twin-graph"></a>Abfragen des Zwillingsdiagramms von Azure Digital Twins

Dieser Artikel bietet Beispiele und ausführliche Informationen dazu, wie Sie die [Azure Digital Twins-Abfragesprache](concepts-query-language.md) verwenden, um Informationen im [Zwillingsgraphen](concepts-twins-graph.md) abzufragen. Sie führen Abfragen im Diagramm mithilfe der [**Abfrage-APIs**](how-to-use-apis-sdks.md) von Azure Digital Twins aus.

[!INCLUDE [digital-twins-query-operations.md](../../includes/digital-twins-query-operations.md)]

Im weiteren Verlauf dieses Artikels finden Sie Beispiele für die Verwendung dieser Vorgänge.

## <a name="query-syntax"></a>Abfragesyntax

Dieser Abschnitt enthält Beispiele für Abfragen, die die Struktur der Abfragesprache veranschaulichen und mögliche Abfragevorgänge mit [digitalen Zwillingen](concepts-twins-graph.md) durchführen.

### <a name="select-top-items"></a>Auswählen der obersten Elemente

Mithilfe der `Select TOP`-Klausel können Sie die verschiedenen obersten Elemente in einer Abfrage auswählen.

```sql
SELECT TOP (5)
FROM DIGITALTWINS
WHERE ...
```

### <a name="query-by-property"></a>Abfrage nach Eigenschaft

Abrufen von digitalen Zwillingen nach **Eigenschaften** (einschließlich ID und Metadaten):
```sql
SELECT  * 
FROM DigitalTwins T  
WHERE T.firmwareVersion = '1.1'
AND T.$dtId in ['123', '456']
AND T.Temperature = 70
```

> [!TIP]
> Die ID eines digitalen Zwillings wird mithilfe des Metadatenfelds `$dtId` abgefragt.

Sie können Zwillinge auch auf der Grundlage abrufen, **ob eine bestimmte Eigenschaft definiert ist**. Hier folgt eine Abfrage, die Zwillinge abruft, die eine definierte *Location*-Eigenschaft aufweisen:

```sql
SELECT *
FROM DIGITALTWINS WHERE IS_DEFINED(Location)
```

Dies kann Ihnen helfen, Zwillinge anhand ihrer *Tageigenschaften* abzurufen, wie unter [Hinzufügen von Tags zu digitalen Zwillingen](how-to-use-tags.md) beschrieben. Hier folgt eine Abfrage, bei der alle Zwillinge *rot* markiert werden:

```sql
select * from digitaltwins where is_defined(tags.red) 
```

Sie können Zwillinge auch auf der Grundlage des **Typs einer Eigenschaft** abrufen. Hier folgt eine Abfrage, die Zwillinge abruft, deren *Temperature*-Eigenschaft eine Zahl ist:

```sql
SELECT * FROM DIGITALTWINS T
WHERE IS_NUMBER(T.Temperature)
```

### <a name="query-by-model"></a>Abfrage nach Modell

Der `IS_OF_MODEL`-Operator kann verwendet werden, um anhand des [**Modells**](concepts-models.md) des Zwillings zu filtern. Er unterstützt Vererbung und weist mehrere Überladungsoptionen auf.

Die einfachste Verwendung von `IS_OF_MODEL` nimmt nur einen `twinTypeName`-Parameter entgegen: `IS_OF_MODEL(twinTypeName)`.
Hier ist eine Beispielabfrage, die in diesem Parameter einen Wert übergibt:

```sql
SELECT * FROM DIGITALTWINS WHERE IS_OF_MODEL('dtmi:sample:thing;1')
```

Um eine zu durchsuchende Zwillingssammlung anzugeben, wenn mehrere vorhanden sind (z. B. wenn `JOIN` verwendet wird), fügen Sie den Parameter `twinCollection` hinzu: `IS_OF_MODEL(twinCollection, twinTypeName)`.
Hier ist eine Beispielabfrage, die einen Wert für diesen Parameter hinzufügt:

```sql
SELECT * FROM DIGITALTWINS DT WHERE IS_OF_MODEL(DT, 'dtmi:sample:thing;1')
```

Um eine genaue Übereinstimmung zu überprüfen, fügen Sie den Parameter `exact` hinzu: `IS_OF_MODEL(twinTypeName, exact)`.
Hier ist eine Beispielabfrage, die einen Wert für diesen Parameter hinzufügt:

```sql
SELECT * FROM DIGITALTWINS WHERE IS_OF_MODEL('dtmi:sample:thing;1', exact)
```

Sie können auch alle drei Argumente gemeinsam übergeben: `IS_OF_MODEL(twinCollection, twinTypeName, exact)`.
Hier ist eine Beispielabfrage, in der für alle drei Parameter ein Wert angegeben ist:

```sql
SELECT ROOM FROM DIGITALTWINS DT WHERE IS_OF_MODEL(DT, 'dtmi:sample:thing;1', exact)
```

### <a name="query-based-on-relationships"></a>Abfrage basierend auf Beziehungen

Für Abfragen anhand der **Beziehungen** von digitalen Zwillingen weist die Azure Digital Twins-Abfragesprache eine besondere Syntax auf.

Beziehungen werden in der `FROM`-Klausel in den Abfragebereich übernommen. Ein wichtiger Unterschied zu „klassischen“ SQL-artigen Sprachen besteht darin, dass die Ausdrücke in dieser `FROM`-Klausel keine Tabellen sind. Stattdessen drückt die `FROM`-Klausel einen entitätsübergreifenden Beziehungsdurchlauf aus, der mit einer Azure Digital Twins-Version von `JOIN` geschrieben wird. 

Denken Sie daran, dass in den Funktionen des Azure Digital Twins-[Modells](concepts-models.md) Beziehungen nur in Abhängigkeit von Zwillingen vorhanden sind. Dies bedeutet, dass sich `JOIN` in der Azure Digital Twins-Abfragesprache leicht von `JOIN` in allgemeiner SQL-Form unterscheidet, da Beziehungen hier nicht unabhängig abgefragt werden können und an einen Zwilling gebunden sein müssen.
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

Ähnlich wie die Eigenschaften digitaler Zwillinge über DTDL beschrieben werden, können Beziehungen ebenfalls über Eigenschaften verfügen. Sie können Zwillinge **auf der Grundlage der Eigenschaften ihrer Beziehungen** abfragen.
Die Azure Digital Twins-Abfragesprache ermöglicht das Filtern und Projizieren von Beziehungen, indem der Beziehung innerhalb der `JOIN`-Klausel ein Alias zugewiesen wird. 

Als Beispiel sei eine *servicedBy*-Beziehung angenommen, die über die *reportedCondition*-Eigenschaft verfügt. In der nachstehenden Abfrage erhält diese Beziehung den Alias „R“, um auf die zugehörige-Eigenschaft zu verweisen.

```sql
SELECT T, SBT, R
FROM DIGITALTWINS T
JOIN SBT RELATED T.servicedBy R
WHERE T.$dtId = 'ABC' 
AND R.reportedCondition = 'clean'
```

Beachten Sie im obigen Beispiel, dass *reportedCondition* eine Eigenschaft der *servicedBy*-Beziehung selbst ist, NICHT eines digitalen Zwillings, der über eine *servicedBy*-Beziehung verfügt.

### <a name="query-with-multiple-joins"></a>Abfragen mit mehreren JOINs

Zurzeit werden in der Vorschauphase bis zu fünf `JOIN`s in einer einzelnen Abfrage unterstützt. Dies ermöglicht es Ihnen, mehrere Beziehungsebenen gleichzeitig zu durchlaufen.

Hier ist ein Beispiel für eine Multijoin-Abfrage, die alle Glühbirnen in den hellen Bereichen in den Räumen 1 und 2 abruft.

```sql
SELECT LightBulb 
FROM DIGITALTWINS Room 
JOIN LightPanel RELATED Room.contains 
JOIN LightBulb RELATED LightPanel.contains 
WHERE IS_OF_MODEL(LightPanel, 'dtmi:contoso:com:lightpanel;1') 
AND IS_OF_MODEL(LightBulb, 'dtmi:contoso:com:lightbulb ;1') 
AND Room.$dtId IN ['room1', 'room2'] 
```

### <a name="other-compound-query-examples"></a>Weitere zusammengesetzte Abfragebeispiele

Mithilfe von Kombinationsoperatoren können Sie jeden der oben genannten Abfragetypen **kombinieren**, um mehr Details in eine einzelne Abfrage einzubeziehen. Hier folgen einige zusätzliche Beispiele für zusammengesetzte Abfragen, die mehr als einen Typ von Zwillingsdeskriptor gleichzeitig abfragen.

| BESCHREIBUNG | Abfrage |
| --- | --- |
| Von den Geräten, über die *Room 123* verfügt, geben Sie die MxChip-Geräte zurück, die die Rolle des Operators übernehmen | `SELECT device`<br>`FROM DigitalTwins space`<br>`JOIN device RELATED space.has`<br>`WHERE space.$dtid = 'Room 123'`<br>`AND device.$metadata.model = 'dtmi:contosocom:DigitalTwins:MxChip:3'`<br>`AND has.role = 'Operator'` |
| Rufen Sie Zwillinge ab, die eine Beziehung namens *Enthält* mit einem anderen Zwilling aufweisen, der die ID *id1* besitzt | `SELECT Room`<br>`FROM DIGITIALTWINS Room`<br>`JOIN Thermostat ON Room.Contains`<br>`WHERE Thermostat.$dtId = 'id1'` |
| Rufen Sie alle Räume dieses Raummodells ab, die in *floor11* enthalten sind | `SELECT Room`<br>`FROM DIGITALTWINS Floor`<br>`JOIN Room RELATED Floor.Contains`<br>`WHERE Floor.$dtId = 'floor11'`<br>`AND IS_OF_MODEL(Room, 'dtmi:contosocom:DigitalTwins:Room;1')` |

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
* Während der Vorschauphase ist die Durchlauftiefe für den Graphen auf fünf `JOIN`-Ebenen pro Abfrage beschränkt.
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
