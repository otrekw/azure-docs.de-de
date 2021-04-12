---
title: Konfigurieren des Ähnlichkeitsalgorithmus
titleSuffix: Azure Cognitive Search
description: Erfahren Sie, wie Sie BM25 für ältere Suchdienste aktivieren und wie BM25-Parameter geändert werden können, um den Inhalt ihrer Indizes besser zu unterstützen.
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 03/12/2021
ms.openlocfilehash: 52b3523d3c092f1b9375f53038cc3b20d0ddedcc
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "103232833"
---
# <a name="configure-the-similarity-ranking-algorithm-in-azure-cognitive-search"></a>Konfigurieren des Ähnlichkeitsrangfolgenalgorithmus in Azure Cognitive Suche

Azure Cognitive Search unterstützt zwei Ähnlichkeitsalgorithmen für die Rangfolge:

+ Einen *klassischen* Ähnlichkeitsalgorithmus, der von allen Suchdiensten bis zum 15. Juli 2020 verwendet wurde.
+ Eine Implementierung des *Okapi BM25*-Algorithmus, der in allen Suchdiensten verwendet wird, die nach dem 15. Juli erstellt wurden.

Die BM25-Rangfolge ist die neue Standardeinstellung, da sie tendenziell Suchrangfolgen erzeugt, die den Erwartungen von Benutzern besser entsprechen. Sie verfügt über [Parameter](#set-bm25-parameters), mit denen Ergebnisse auf der Basis von Faktoren wie der Dokumentgröße optimiert werden können. 

Für neue Dienste, die nach dem 15. Juli 2020 erstellt werden, wird BM25 automatisch verwendet und ist der einzige Ähnlichkeitsalgorithmus. Wenn Sie versuchen, Ähnlichkeit für einen neuen Dienst auf „ClassicSimilarity“ festzulegen, wird ein HTTP 400-Fehler zurückgegeben, da dieser Algorithmus durch den Dienst nicht unterstützt wird.

Für ältere Dienste, die vor dem 15. Juli 2020 erstellt wurden, bleibt die klassische Ähnlichkeit der Standardalgorithmus. Ältere Dienste können pro Index auf BM25 aktualisiert werden, wie unten erläutert. Wenn Sie von „klassisch“ zu „BM25“ wechseln, sind einige Unterschiede in der Anordnung der Suchergebnisse zu erwarten.

> [!NOTE]
> Die semantische Rangfolge, die sich derzeit in ausgewählten Regionen in der Vorschau für Standarddienste befindet, ist ein zusätzlicher Schritt, relevantere Ergebnisse zu erzielen. Im Gegensatz zu den anderen Algorithmen handelt es sich um ein zusätzliches Feature, das eine bestehende Ergebnismenge durchläuft. Weitere Informationen finden Sie unter [Übersicht über die semantische Suche](semantic-search-overview.md) und [Semantische Rangfolge in Azure Cognitive Search](semantic-ranking.md).

## <a name="enable-bm25-scoring-on-older-services"></a>Aktivieren der BM25-Bewertung für ältere Dienste

Wenn Sie einen Suchdienst ausführen, der vor dem 15. Juli 2020 erstellt wurde, können Sie BM25 aktivieren, indem Sie für neue Indizes eine Ähnlichkeitseigenschaft festlegen. Die Eigenschaft wird nur für neue Indizes verfügbar gemacht. Wenn Sie also BM25 für einen vorhandenen Index benötigen, müssen Sie den Index löschen und [neu erstellen](search-howto-reindex.md), wobei eine neue Ähnlichkeitseigenschaft auf „Microsoft.Azure.Search.BM25Similarity“ festgelegt wird.

Sobald ein Index mit einer Ähnlichkeitseigenschaft vorhanden ist, können Sie zwischen BM25Similarity und ClassicSimilarity wechseln. 

Unter den folgenden Links wird die Ähnlichkeitseigenschaft in den Azure SDKs beschrieben. 

| Clientbibliothek | Ähnlichkeitseigenschaft |
|----------------|---------------------|
| .NET  | [SearchIndex.Similarity](/dotnet/api/azure.search.documents.indexes.models.searchindex.similarity) |
| Java | [SearchIndex.setSimilarity](/java/api/com.azure.search.documents.indexes.models.searchindex.setsimilarity) |
| JavaScript | [SearchIndex.Similarity](/javascript/api/@azure/search-documents/searchindex#similarity) |
| Python | [Ähnlichkeitseigenschaft für SearchIndex](/python/api/azure-search-documents/azure.search.documents.indexes.models.searchindex) |

### <a name="rest-example"></a>Beispiel für REST

Sie können auch die [Rest-API](/rest/api/searchservice/create-index)verwenden, wie im folgenden Beispiel veranschaulicht:

```http
PUT https://[search service name].search.windows.net/indexes/[index name]?api-version=2020-06-30
{
    "name": "indexName",
    "fields": [
        {
            "name": "id",
            "type": "Edm.String",
            "key": true
        },
        {
            "name": "name",
            "type": "Edm.String",
            "searchable": true,
            "analyzer": "en.lucene"
        },
        ...
    ],
    "similarity": {
        "@odata.type": "#Microsoft.Azure.Search.BM25Similarity"
    }
}
```

## <a name="set-bm25-parameters"></a>Festlegen von BM25-Parametern

BM25-Ähnlichkeit fügt zwei durch Benutzer anpassbare Parameter hinzu, um die berechnete Relevanzbewertung zu steuern. Sie können BM25-Parameter während der Indexerstellung oder als Indexupdate festlegen, wenn der BM25-Algorithmus während der Indexerstellung angegeben wurde.

| Eigenschaft | type | BESCHREIBUNG |
|----------|------|-------------|
| k1 | number | Steuert die Skalierungsfunktion, die bestimmt, wie weit die endgültige Relevanzbewertung eines Dokument/Abfrage-Paars von der Häufigkeit der einzelnen übereinstimmenden Begriffe abhängt. Die Werte liegen in der Regel zwischen 0,0 und 3,0. Der Standardwert ist 1,2. </br></br>Der Wert 0,0 stellt ein „binäres Modell“ dar, bei dem der Anteil eines einzelnen übereinstimmenden Begriffs für alle übereinstimmenden Dokumente identisch ist, unabhängig davon, wie oft dieser Begriff im Text vorkommt, während ein größerer k1-Wert bestimmt, dass die Bewertung höher ausfällt, je mehr Instanzen desselben Begriffs im Dokument gefunden werden. </br></br>Die Verwendung eines höheren k1-Werts kann wichtig sein, wenn wir erwarten, dass mehrere Begriffe Teil einer Suchabfrage sind. In diesen Fällen bevorzugen wir möglicherweise Dokumente, in denen viele der unterschiedlichen gesuchten Abfragebegriffe vorkommen, gegenüber Dokumenten, in denen nur ein einzelner Begriff häufig vorkommt. Wenn Sie z. B. den Index nach Dokumenten abfragen, die die Begriffe „Apollo Spaceflight“ enthalten, möchten wir die Bewertung eines Artikels über griechische Mythologie, der den Begriff „Apollo“ ein paar Dutzend Mal enthält, ohne dass „Spaceflight“ vorkommt, gegenüber einem Artikel reduzieren, in dem „Apollo“ als auch „Spaceflight“ nur einige wenige Male explizit erwähnt werden. |
| b | number | Steuert, wie sich die Länge eines Dokuments auf die Relevanzbewertung auswirkt. Die Werte liegen zwischen 0 und 1. Der Standardwert ist 0,75. </br></br>Der Wert 0,0 bedeutet, dass die Länge des Dokuments das Ergebnis nicht beeinflusst, während der Wert 1,0 bedeutet, dass die Auswirkung der Begriffshäufigkeit auf die Relevanzbewertung durch die Länge des Dokuments normalisiert wird. </br></br>Das Normalisieren der Begriffshäufigkeit durch die Länge des Dokuments ist nützlich, wenn Sie längere Dokumente zurückstellen möchten. In einigen Fällen enthalten längere Dokumente (z. B. komplette Roman) im Vergleich zu deutlich kürzeren Dokumenten eher viele irrelevante Begriffe. |

### <a name="setting-k1-and-b-parameters"></a>Festlegen des „k1“- und „b“-Parameters

Um b- oder k1-Werte festzulegen oder zu ändern, fügen Sie sie dem BM25-Ähnlichkeitsobjekt hinzu. Das Festlegen oder Ändern dieser Werte bei einem vorhandenen Index schaltet den Index für mindestens ein paar Sekunden offline, sodass aktive Indizierungs- und Abfrageanforderungen fehlschlagen. Folglich sollten Sie den Parameter „allowIndexDowntime=true“ in der Aktualisierungsanforderung festlegen:

```http
PUT https://[search service name].search.windows.net/indexes/[index name]?api-version=2020-06-30&allowIndexDowntime=true
{
    "similarity": {
        "@odata.type": "#Microsoft.Azure.Search.BM25Similarity",
        "b" : 0.5,
        "k1" : 1.3
    }
}
```

## <a name="see-also"></a>Weitere Informationen  

+ [REST-API-Referenz](/rest/api/searchservice/)
+ [Hinzufügen von Bewertungsprofilen zum Index](index-add-scoring-profiles.md)
+ [Indexerstellungs-API](/rest/api/searchservice/create-index)
+ [.NET SDK für die kognitive Azure-Suche](/dotnet/api/overview/azure/search)