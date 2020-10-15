---
title: Kompatibilität von Azure Cosmos DB Gremlin mit TinkerPop-Features
description: Referenzdokumentation zu Kompatibilitätsproblemen der Graph-Engine
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: reference
ms.date: 09/10/2019
ms.author: sngun
ms.openlocfilehash: 7257246f618e3028534f3ebd60eaf6f94a3a4720
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87092506"
---
# <a name="azure-cosmos-db-gremlin-compatibility"></a>Kompatibilität von Azure Cosmos DB Gremlin
Die Azure Cosmos DB Graph-Engine hält sich eng an die [Apache TinkerPop](https://tinkerpop.apache.org/docs/current/reference/#graph-traversal-steps)-Spezifikation für die Durchlaufschritte, es gibt jedoch Azure Cosmos DB-spezifische Unterschiede bei der Implementierung. Eine Liste mit unterstützten Gremlin-Schritten finden Sie im Artikel [Unterstützung für Gremlin-Diagramme in Azure Cosmos DB](gremlin-support.md).

## <a name="behavior-differences"></a>Unterschiede im Verhalten

* Die Azure Cosmos DB Graph-Engine führt einen Durchlauf mit ***breitem*** Ansatz aus, während TinkerPop Gremlin einen „tiefen“ Ansatz verfolgt. Dieses Verhalten erzielt eine bessere Leistung in einem horizontal skalierbaren System wie Cosmos DB. 

## <a name="unsupported-features"></a>Nicht unterstützte Funktionen

* ***[Gremlin Bytecode](https://tinkerpop.apache.org/docs/current/tutorials/gremlin-language-variants/)*** ist eine von der Programmiersprache unabhängige Spezifikation für Diagrammdurchläufe. Cosmos DB Graph unterstützt dies noch nicht. Verwenden Sie `GremlinClient.SubmitAsync()`, und übergeben Sie den Durchlauf als eine Textzeichenfolge.

* ***`property(set, 'xyz', 1)`***: Das Festlegen der Kardinalität wird noch nicht unterstützt. Verwenden Sie stattdessen `property(list, 'xyz', 1)`. Weitere Informationen finden Sie unter [Vertex-Eigenschaften mit TinkerPop](http://tinkerpop.apache.org/docs/current/reference/#vertex-properties).

* Der ***Schritt `match()`*** ist derzeit nicht verfügbar. Dieser Schritt bietet deklarative Abfragefunktionen.

* ***Objekte als Eigenschaften*** werden für Vertices und Edges nicht unterstützt. Eigenschaften können nur primitive Typen oder Arrays sein.

* ***Sortieren nach Arrayeigenschaften*** `order().by(<array property>)` wird nicht unterstützt. Die Sortierung wird nur von primitiven Typen unterstützt.

* ***Nicht primitive JSON-Typen*** werden nicht unterstützt. Verwenden Sie die Typen `string`, `number` oder `true`/`false`. `null`-Werte werden nicht unterstützt. 

* Das ***GraphSONv3***-Serialisierungsmodul wird derzeit noch nicht unterstützt. Verwenden Sie das `GraphSONv2`-Serialisierungsmodul sowie Reader- und Writerklassen in der Verbindungskonfiguration. Das Format der von der Azure Cosmos DB Gremlin-API zurückgegebenen Ergebnisse ist nicht identisch mit dem GraphSON-Format. 

* **Lambdaausdrücke und -funktionen** werden derzeit nicht unterstützt. Dies umfasst die Funktionen `.map{<expression>}`, `.by{<expression>}` und `.filter{<expression>}`. Weitere Informationen und wie Sie sie mithilfe der Gremlin-Schritte neu schreiben können, finden Sie unter [A Note on Lambdas](http://tinkerpop.apache.org/docs/current/reference/#a-note-on-lambdas) (Hinweis zu Lambdas).

* ***Transaktionen*** werden aufgrund der verteilten Natur des Systems nicht unterstützt.  Konfigurieren Sie ein geeignetes Konsistenzmodell für das Gremlin-Konto, um „Ihre eigenen Schreibvorgänge zu lesen“, und verwenden Sie optimistische Nebenläufigkeit, um Schreibvorgänge aufzulösen, zu einen Konflikt verursachen.

## <a name="known-limitations"></a>Bekannte Einschränkungen

* **Indexverwendung für Gremlin-Abfragen mit Schritten vom Typ `.V()` während des Durchlaufs:** Aktuell wird nur beim ersten `.V()`-Aufruf eines Durchlaufs der Index genutzt, um alle angefügten Filter oder Prädikate aufzulösen. Bei späteren Aufrufen wird der Index nicht herangezogen, wodurch sich die Wartezeit und die Kosten der Abfrage erhöhen können.
    
    Bei Verwendung der Standardindizierung werden von einer typischen Gremlin-Leseabfrage, die mit dem Schritt `.V()` beginnt, Parameter wie `.has()` oder `.where()` in den angefügten Filterschritten verwendet, um die Kosten und die Leistung der Abfrage zu optimieren. Beispiel:

    ```java
    g.V().has('category', 'A')
    ```

    Enthält die Gremlin-Abfrage allerdings mehrere Schritte vom Typ `.V()`, ist die Auflösung der Daten für die Abfrage möglicherweise nicht optimal. Ein Beispiel hierfür wäre etwa die folgende Abfrage:

    ```java
    g.V().has('category', 'A').as('a').V().has('category', 'B').as('b').select('a', 'b')
    ```

    Diese Abfrage gibt auf der Grundlage der Eigenschaft `category` zwei Gruppen von Vertices zurück. In diesem Fall wird nur beim ersten Aufruf (`g.V().has('category', 'A')`) der Index genutzt, um die Vertices auf der Grundlage der Werte ihrer Eigenschaften aufzulösen.

    Zur Umgehung dieses Problems können bei dieser Abfrage untergeordnete Durchlaufschritte wie `.map()` und `union()` verwendet werden. Beispiel:

    ```java
    // Query workaround using .map()
    g.V().has('category', 'A').as('a').map(__.V().has('category', 'B')).as('b').select('a','b')

    // Query workaround using .union()
    g.V().has('category', 'A').fold().union(unfold(), __.V().has('category', 'B'))
    ```

    Die Leistung der Abfragen kann mithilfe des [Gremlin`executionProfile()`-Schritts ](graph-execution-profile.md) überprüft werden.

## <a name="next-steps"></a>Nächste Schritte
* Besuchen Sie die Seite [Cosmos DB User Voice](https://feedback.azure.com/forums/263030-azure-cosmos-db), um Feedback zu übermitteln und dem Team dabei zu helfen, sich auf die für Sie wichtigen Features zu konzentrieren.
