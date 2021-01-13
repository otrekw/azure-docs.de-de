---
title: Abfragen des Zwillingsgraphen
titleSuffix: Azure Digital Twins
description: Erfahren Sie, wie Sie Informationen im Zwillingsdiagramm von Azure Digital Twins abfragen.
author: baanders
ms.author: baanders
ms.date: 11/19/2020
ms.topic: how-to
ms.service: digital-twins
ms.custom: contperf-fy21q2
ms.openlocfilehash: 47883c742d77a88adb662e8dded0723f0e105385
ms.sourcegitcommit: 8dd8d2caeb38236f79fe5bfc6909cb1a8b609f4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2021
ms.locfileid: "98044185"
---
# <a name="query-the-azure-digital-twins-twin-graph"></a>Abfragen des Zwillingsdiagramms von Azure Digital Twins

Dieser Artikel bietet Abfragebeispiele und ausführlichere Anleitungen, wie Sie die **Azure Digital Twins-Abfragesprache** verwenden, um Informationen von Ihrem [Zwillingsgraphen](concepts-twins-graph.md) abzufragen. (Eine Einführung in die Abfragesprache und eine vollständige Liste der Funktionen finden Sie unter [*Konzepte: Abfragesprache*](concepts-query-language.md).)

Dieser Artikel beginnt mit Beispielabfragen, die die Struktur der Abfragesprache sowie gängige Abfragevorgänge für digitale Zwillinge veranschaulichen. Anschließend wird beschrieben, wie Sie Ihre Abfragen, nachdem Sie sie geschrieben haben. mithilfe der [Abfrage-API](/rest/api/digital-twins/dataplane/query) von Azure Digital Twins oder mit einem [SDK](how-to-use-apis-sdks.md#overview-data-plane-apis) ausführen.

> [!TIP]
> Wenn Sie die Beispielabfragen unten mit einem API- oder SDK-Aufruf ausführen, müssen Sie den Abfragetext in eine einzelne Zeile zusammenfassen.

## <a name="show-all-digital-twins"></a>Anzeigen aller digitalen Zwillinge

Im Folgenden finden Sie die grundlegende Abfrage, die eine Liste aller digitalen Zwillinge in der Instanz zurückgibt:

:::code language="sql" source="~/digital-twins-docs-samples/queries/queries.sql" id="GetAllTwins":::

## <a name="query-by-property"></a>Abfrage nach Eigenschaft

Abrufen von digitalen Zwillingen nach **Eigenschaften** (einschließlich ID und Metadaten):

:::code language="sql" source="~/digital-twins-docs-samples/queries/queries.sql" id="QueryByProperty1":::

> [!TIP]
> Die ID eines digitalen Zwillings wird mithilfe des Metadatenfelds `$dtId` abgefragt.

Sie können Zwillinge auch auf der Grundlage abrufen, **ob eine bestimmte Eigenschaft definiert ist**. Hier folgt eine Abfrage, die Zwillinge abruft, die eine definierte *Location*-Eigenschaft aufweisen:

:::code language="sql" source="~/digital-twins-docs-samples/queries/queries.sql" id="QueryByProperty2":::

Dies kann Ihnen helfen, Zwillinge anhand ihrer *Tageigenschaften* abzurufen, wie unter [Hinzufügen von Tags zu digitalen Zwillingen](how-to-use-tags.md) beschrieben. Hier folgt eine Abfrage, bei der alle Zwillinge *rot* markiert werden:

:::code language="sql" source="~/digital-twins-docs-samples/queries/queries.sql" id="QueryMarkerTags1":::

Sie können Zwillinge auch auf der Grundlage des **Typs einer Eigenschaft** abrufen. Hier folgt eine Abfrage, die Zwillinge abruft, deren *Temperature*-Eigenschaft eine Zahl ist:

:::code language="sql" source="~/digital-twins-docs-samples/queries/queries.sql" id="QueryByProperty3":::

## <a name="query-by-model"></a>Abfrage nach Modell

Der `IS_OF_MODEL`-Operator kann verwendet werden, um anhand des [**Modells**](concepts-models.md) des Zwillings zu filtern.

Er berücksichtigt [Vererbung](concepts-models.md#model-inheritance) und [Versionsverwaltung](how-to-manage-model.md#update-models) des Modells und wird in **TRUE** für einen bestimmten Zwilling ausgewertet, wenn der Zwilling eine der folgenden Bedingungen erfüllt:

* Der Zwilling implementiert das Modell direkt, das für `IS_OF_MODEL()` bereitgestellt wird, und die Versionsnummer des Modells auf dem Zwilling ist *größer oder gleich* der Versionsnummer des bereitgestellten Modells.
* Der Zwilling implementiert ein Modell, das das für `IS_OF_MODEL()` bereitgestellte Modell *erweitert*, und die Versionsnummer des erweiterten Modells des Zwillings ist *größer oder gleich* der Versionsnummer des bereitgestellten Modells.

Wenn Sie z. B. nach Zwillingen des Modells `dtmi:example:widget;4` abfragen, gibt die Abfrage alle Zwillinge zurück, die auf **Version 4 oder höher** des **Widgets**-Modells basieren, sowie auch Zwillinge, die auf Version **4 oder höher** von **Modellen basieren, die vom Widget erben**.

`IS_OF_MODEL` kann verschiedene Parameter annehmen, und der Rest dieses Abschnitts beschäftigt sich mit den verschiedenen Überladungsoptionen.

Die einfachste Verwendung von `IS_OF_MODEL` nimmt nur einen `twinTypeName`-Parameter entgegen: `IS_OF_MODEL(twinTypeName)`.
Hier ist eine Beispielabfrage, die in diesem Parameter einen Wert übergibt:

:::code language="sql" source="~/digital-twins-docs-samples/queries/queries.sql" id="QueryByModel1":::

Um eine zu durchsuchende Zwillingssammlung anzugeben, wenn mehrere vorhanden sind (z. B. wenn `JOIN` verwendet wird), fügen Sie den Parameter `twinCollection` hinzu: `IS_OF_MODEL(twinCollection, twinTypeName)`.
Hier ist eine Beispielabfrage, die einen Wert für diesen Parameter hinzufügt:

:::code language="sql" source="~/digital-twins-docs-samples/queries/queries.sql" id="QueryByModel2":::

Um eine genaue Übereinstimmung zu überprüfen, fügen Sie den Parameter `exact` hinzu: `IS_OF_MODEL(twinTypeName, exact)`.
Hier ist eine Beispielabfrage, die einen Wert für diesen Parameter hinzufügt:

:::code language="sql" source="~/digital-twins-docs-samples/queries/queries.sql" id="QueryByModel3":::

Sie können auch alle drei Argumente gemeinsam übergeben: `IS_OF_MODEL(twinCollection, twinTypeName, exact)`.
Hier ist eine Beispielabfrage, in der für alle drei Parameter ein Wert angegeben ist:

:::code language="sql" source="~/digital-twins-docs-samples/queries/queries.sql" id="QueryByModel4":::

## <a name="query-by-relationship"></a>Abfragen nach Beziehung

Für Abfragen anhand der **Beziehungen** von digitalen Zwillingen weist die Azure Digital Twins-Abfragesprache eine besondere Syntax auf.

Beziehungen werden in der `FROM`-Klausel in den Abfragebereich übernommen. Ein wichtiger Unterschied zu „klassischen“ SQL-artigen Sprachen besteht darin, dass die Ausdrücke in dieser `FROM`-Klausel keine Tabellen sind. Stattdessen drückt die `FROM`-Klausel einen entitätsübergreifenden Beziehungsdurchlauf aus, der mit einer Azure Digital Twins-Version von `JOIN` geschrieben wird.

Denken Sie daran, dass in den Funktionen des Azure Digital Twins-[Modells](concepts-models.md) Beziehungen nur in Abhängigkeit von Zwillingen vorhanden sind. Dies bedeutet, dass sich `JOIN` in der Azure Digital Twins-Abfragesprache leicht von `JOIN` in allgemeiner SQL-Form unterscheidet, da Beziehungen hier nicht unabhängig abgefragt werden können und an einen Zwilling gebunden sein müssen.
Um diesen Unterschied zu berücksichtigen, wird das Schlüsselwort `RELATED` in der `JOIN`-Klausel verwendet, um auf den Beziehungssatz eines Zwillings zu verweisen.

Der folgende Abschnitt enthält einige Beispiele dafür.

> [!TIP]
> Konzeptionell imitiert dieses Feature die dokumentzentrische Funktionalität von Cosmos DB, bei der `JOIN` für untergeordnete Objekte innerhalb eines Dokuments ausgeführt werden kann. Cosmos DB gibt über das Schlüsselwort `IN` an, dass `JOIN` Arrayelemente im aktuellen Kontextdokument durchlaufen soll.

### <a name="relationship-based-query-examples"></a>Beispiele für beziehungsbasierte Abfragen

Um ein Dataset mit Beziehungen zu erhalten, verwenden Sie eine einzelne `FROM`-Anweisung gefolgt von N `JOIN`-Anweisungen, wobei die `JOIN`-Anweisungen Beziehungen zum Ergebnis einer vorherigen `FROM`- oder `JOIN`-Anweisung ausdrücken.

Hier ist ein Beispiel für eine beziehungsbasierte Abfrage. Mit diesem Codeausschnitt werden alle digitalen Zwillinge mit der *ID*-Eigenschaft „ABC“ und alle digitalen Zwillinge, die zu diesen digitalen Zwillingen eine *contains*-Beziehung haben, ausgewählt.

:::code language="sql" source="~/digital-twins-docs-samples/queries/queries.sql" id="QueryByRelationship1":::

> [!NOTE]
> Der Entwickler muss dieses `JOIN` nicht mit einem Schlüsselwert in der `WHERE`-Klausel korrelieren (oder in der `JOIN`-Definition einen Schlüsselwert angeben). Diese Korrelation wird automatisch vom System berechnet, da die Beziehungseigenschaften die Zielentität direkt identifizieren.

### <a name="query-the-properties-of-a-relationship"></a>Abfragen der Eigenschaften einer Beziehung

Ähnlich wie die Eigenschaften digitaler Zwillinge über DTDL beschrieben werden, können Beziehungen ebenfalls über Eigenschaften verfügen. Sie können Zwillinge **auf der Grundlage der Eigenschaften ihrer Beziehungen** abfragen.
Die Azure Digital Twins-Abfragesprache ermöglicht das Filtern und Projizieren von Beziehungen, indem der Beziehung innerhalb der `JOIN`-Klausel ein Alias zugewiesen wird.

Als Beispiel sei eine *servicedBy*-Beziehung angenommen, die über die *reportedCondition*-Eigenschaft verfügt. In der nachstehenden Abfrage erhält diese Beziehung den Alias „R“, um auf die zugehörige-Eigenschaft zu verweisen.

:::code language="sql" source="~/digital-twins-docs-samples/queries/queries.sql" id="QueryByRelationship2":::

Beachten Sie im obigen Beispiel, dass *reportedCondition* eine Eigenschaft der *servicedBy*-Beziehung selbst ist, NICHT eines digitalen Zwillings, der über eine *servicedBy*-Beziehung verfügt.

### <a name="query-with-multiple-joins"></a>Abfragen mit mehreren JOINs

Bis zu fünf `JOIN`s werden in einer einzelnen Abfrage unterstützt. Dies ermöglicht es Ihnen, mehrere Beziehungsebenen gleichzeitig zu durchlaufen.

Hier ist ein Beispiel für eine Multijoin-Abfrage, die alle Glühbirnen in den hellen Bereichen in den Räumen 1 und 2 abruft.

:::code language="sql" source="~/digital-twins-docs-samples/queries/queries.sql" id="QueryByRelationship3":::

## <a name="count-items"></a>Zählen von Elementen

Sie können die Anzahl von Elementen in einem Resultset mit der `Select COUNT`-Klausel zählen:

:::code language="sql" source="~/digital-twins-docs-samples/queries/queries.sql" id="SelectCount1":::

Fügen Sie eine `WHERE`-Klausel hinzu, um die Anzahl von Elementen zu zählen, die ein bestimmtes Kriterium erfüllen. Im Folgenden finden Sie einige Beispiele für das Zählen mit einem angewendeten Filter basierend auf dem Typ des Zwillingsmodells (weitere Informationen zu dieser Syntax finden Sie unter [*Abfragen nach Modell*](#query-by-model) weiter unten):

:::code language="sql" source="~/digital-twins-docs-samples/queries/queries.sql" id="SelectCount2":::

Sie können auch `COUNT` zusammen mit der `JOIN`-Klausel verwenden. Im Folgenden finden Sie eine Abfrage, mit der alle Glühbirnen gezählt werden, die in der Beleuchtung der Räume 1 und 2 enthalten sind:

:::code language="sql" source="~/digital-twins-docs-samples/queries/queries.sql" id="SelectCount3":::

## <a name="filter-results-select-top-items"></a>Filterergebnisse: Auswählen der obersten Elemente

Mithilfe der `Select TOP`-Klausel können Sie die verschiedenen obersten Elemente in einer Abfrage auswählen.

:::code language="sql" source="~/digital-twins-docs-samples/queries/queries.sql" id="SelectTop":::

## <a name="filter-results-specify-return-set-with-projections"></a>Filterergebnisse: Angeben eines Rückgabesatzes mit Projektionen

Wenn Sie Projektionen in der `SELECT`-Anweisung verwenden, können Sie auswählen, welche Spalten von einer Abfrage zurückgegeben werden.

>[!NOTE]
>Zurzeit werden komplexe Eigenschaften nicht unterstützt. Um sicherzustellen, dass die Projektionseigenschaften gültig sind, kombinieren Sie die Projektionen mit einer `IS_PRIMITIVE`-Überprüfung.

Im Folgenden finden Sie ein Beispiel für eine Abfrage, die eine Projektion verwendet, um Zwillinge und Beziehungen zurückzugeben. Die folgende Abfrage projiziert die Angaben *Consumer*, *Factory* und *Edge* aus einem Szenario, in dem eine *Factory* mit einer ID von *ABC* mit dem *Consumer* durch eine Beziehung von *Factory.customer* verbunden ist und diese Beziehung als *Edge* dargestellt wird.

:::code language="sql" source="~/digital-twins-docs-samples/queries/queries.sql" id="Projections1":::

Sie können Projektion auch verwenden, um eine Eigenschaft eines Zwillings zurückzugeben. Die folgende Abfrage projiziert die Eigenschaft *Name* der *Consumers*, die mit der *Factory* mit einer ID von *ABC* durch eine Beziehung von *Factory.customer* verbunden sind.

:::code language="sql" source="~/digital-twins-docs-samples/queries/queries.sql" id="Projections2":::

Sie können Projektion auch verwenden, um eine Eigenschaft einer Beziehung zurückzugeben. Wie im vorhergehenden Beispiel projiziert die folgende Abfrage die Eigenschaft *Name* der *Consumers*, die mit der *Factory* mit einer ID von *ABC* über eine Beziehung von *Factory.customer* verbunden sind. Jetzt gibt sie aber auch zwei Eigenschaften dieser Beziehung zurück: *prop1* und *prop2*. Dies geschieht durch Benennen der Beziehung *Edge* und Erfassen der zugehörigen Eigenschaften.  

:::code language="sql" source="~/digital-twins-docs-samples/queries/queries.sql" id="Projections3":::

Sie können auch Aliase verwenden, um Abfragen mit Projektion zu vereinfachen.

Mit der folgenden Abfrage werden dieselben Vorgänge wie im vorherigen Beispiel durchgeführt, aber für die Eigenschaftsnamen werden die Aliase `consumerName`, `first`, `second` und `factoryArea` verwendet.

:::code language="sql" source="~/digital-twins-docs-samples/queries/queries.sql" id="Projections4":::

Dies ist eine ähnliche Abfrage, die die gleiche Menge wie oben festgelegt abfragt, aber nur die Eigenschaft *Consumer.name* als `consumerName` und die vollständige *Factory* als Zwilling projiziert.

:::code language="sql" source="~/digital-twins-docs-samples/queries/queries.sql" id="Projections5":::

## <a name="build-efficient-queries-with-the-in-operator"></a>Erstellen effizienter Abfragen mit dem IN-Operator

Sie können die Anzahl der erforderlichen Abfragen deutlich verringern, indem Sie ein Array aus Zwillingen erstellen und die Abfrage mit dem `IN`-Operator durchführen. 

Stellen Sie sich beispielsweise ein Szenario vor, in dem *Gebäude* aus *Etagen* und *Etagen* aus *Räumen* bestehen. Um in einem Gebäude nach heißen Räumen zu suchen, besteht eine Möglichkeit darin, diese Schritte durchzuführen.

1. Etagen im Gebäude basierend auf der `contains`-Beziehung suchen.

    :::code language="sql" source="~/digital-twins-docs-samples/queries/queries.sql" id="INOperatorWithout":::

2. Wenn Sie Räume suchen möchten, können Sie eine Abfrage mit einer Sammlung der Etagen im Gebäude (in der nachstehenden Abfrage als *Floor* bezeichnet) ausführen, anstatt die Etagen einzeln zu überprüfen und jeweils eine `JOIN`-Abfrage zum Auffinden der Räume auszuführen.

    In der Client-App:
    
    ```csharp
    var floors = "['floor1','floor2', ..'floorn']"; 
    ```
    
    In der Abfrage:
    
    :::code language="sql" source="~/digital-twins-docs-samples/queries/queries.sql" id="INOperatorWith":::

## <a name="other-compound-query-examples"></a>Weitere zusammengesetzte Abfragebeispiele

Mithilfe von Kombinationsoperatoren können Sie jeden der oben genannten Abfragetypen **kombinieren**, um mehr Details in eine einzelne Abfrage einzubeziehen. Hier folgen einige zusätzliche Beispiele für zusammengesetzte Abfragen, die mehr als einen Typ von Zwillingsdeskriptor gleichzeitig abfragen.

* Von den Geräten, über die *Room 123* verfügt, geben Sie die MxChip-Geräte zurück, die die Rolle des Operators übernehmen
    :::code language="sql" source="~/digital-twins-docs-samples/queries/queries.sql" id="OtherExamples1":::
* Rufen Sie Zwillinge ab, die eine Beziehung namens *Enthält* mit einem anderen Zwilling aufweisen, der die ID *id1* besitzt
    :::code language="sql" source="~/digital-twins-docs-samples/queries/queries.sql" id="OtherExamples2":::
* Rufen Sie alle Räume dieses Raummodells ab, die in *floor11* enthalten sind
    :::code language="sql" source="~/digital-twins-docs-samples/queries/queries.sql" id="OtherExamples3":::

## <a name="run-queries-with-the-api"></a>Ausführen von Abfragen mit der API

Nachdem Sie sich für eine Abfragezeichenfolge entschieden haben, führen Sie sie durch Aufrufen der [**Abfrage-API**](/rest/api/digital-twins/dataplane/query) aus.

Sie können die API direkt aufrufen oder eins der [SDKs](how-to-use-apis-sdks.md#overview-data-plane-apis) verwenden, die für Azure Digital Twins verfügbar sind.

Der folgende Codeausschnitt veranschaulicht den [.NET (C#) SDK](/dotnet/api/overview/azure/digitaltwins/client?view=azure-dotnet&preserve-view=true)-Aufruf aus einer Client-App:

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/queries.cs" id="RunQuery":::

Dieser Aufruf gibt Abfrageergebnisse in Form eines [BasicDigitalTwin](/dotnet/api/azure.digitaltwins.core.basicdigitaltwin?view=azure-dotnet&preserve-view=true)-Objekts zurück.

Abfrageaufrufe unterstützen Paging. Im Folgenden finden Sie ein vollständiges Beispiel mit `BasicDigitalTwin` als Abfrageergebnistyp mit Fehlerbehandlung und Paging:

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/queries.cs" id="FullQuerySample":::

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die [Azure Digital Twins-APIs und SDKs](how-to-use-apis-sdks.md), einschließlich der Abfrage-API, die zum Ausführen der Abfragen in diesem Artikel verwendet wird.
