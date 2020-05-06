---
title: Übersicht über Ähnlichkeit und Bewertung
titleSuffix: Azure Cognitive Search
description: In diesem Artikel werden die Konzepte „Ähnlichkeit“ und „Bewertung“ erläutert sowie Arten, auf die Entwickler Bewertungsergebnisse anpassen können.
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 04/27/2020
ms.openlocfilehash: 9f9cc4c29b117c83595a36c4e28b1edb428c3cde
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82254063"
---
# <a name="similarity-and-scoring-in-azure-cognitive-search"></a>Ähnlichkeit und Bewertung in Azure Cognitive Search

Der Begriff „Bewertung“ bezieht sich auf die Berechnung einer Suchbewertung für jedes in Suchergebnissen für Volltext-Suchabfragen zurückgegebene Element. Die Bewertung ist ein Indikator für die Relevanz eines Elements im Kontext des aktuellen Suchvorgangs. Je höher die Bewertung, desto relevanter das Element. In den Suchergebnissen ist den Elementen eine absteigende Reihenfolge zugeordnet, die auf den für die einzelnen Elemente berechneten Suchbewertungen basiert. 

Standardmäßig werden in der Antwort die ersten 50 Elemente zurückgegeben, Sie können jedoch den **$top**-Parameter verwenden, um eine kleinere oder größere Anzahl von Elementen (bis zu 1000 in einer einzelnen Antwort) zurückzugeben, sowie den Parameter **$skip**, um zu den nächsten Ergebnissen zu gelangen.

Eine Suchbewertung wird auf Basis der statistischen Eigenschaften der Daten und der Abfrage berechnet. Azure Cognitive Search findet Dokumente, die Suchbegriffe enthalten (einige oder alle, in Abhängigkeit von [searchMode](https://docs.microsoft.com/rest/api/searchservice/search-documents#searchmodeany--all-optional)), wobei Dokumente bevorzugt werden, in denen der Suchbegriff häufig vorkommt. Die Suchbewertung fällt sogar noch höher aus, wenn der Begriff nur selten im Datenindex, jedoch innerhalb des Dokuments häufig vorkommt. Die Grundlage für diesen Ansatz zur Berechnung der Relevanz wird als *TF-IDF* (Term Frequency – Inverse Document Frequency, Vorkommenshäufigkeit – Inverse Dokumenthäufigkeit) bezeichnet.

Suchbewertungswerte können in einem Resultset wiederholt vorkommen. Wenn mehrere Treffer die gleiche Suchbewertung aufweisen, ist die Sortierung von Elementen mit gleicher Bewertung nicht definiert und somit auch nicht stabil. Wenn Sie die Abfrage noch mal ausführen, sehen Sie möglicherweise, dass sich die Position von Elementen ändert. Dies ist insbesondere dann der Fall, wenn Sie den kostenlosen Dienst oder einen abrechenbaren Dienst mit vielen Replikaten verwenden. Wenn zwei Elemente mit identischer Bewertung vorliegen, kann nicht garantiert werden, welches Element zuerst angezeigt wird.

Wenn Sie eine Reihenfolge für Elemente mit der gleichen Bewertung festlegen möchten, können Sie eine **$orderby**-Klausel hinzufügen, um erst nach Bewertung und dann nach einem anderen sortierbaren Feld zu sortieren (z. B. `$orderby=search.score() desc,Rating desc`). Weitere Informationen finden Sie unter [$orderby](https://docs.microsoft.com/azure/search/search-query-odata-orderby).

> [!NOTE]
> `@search.score = 1.00` bedeutet, dass es sich um ein nicht bewertetes oder unsortiertes Resultset handelt. Die Bewertung wird für alle Ergebnisse gleich durchgeführt. Nicht bewertete Ergebnisse treten auf, wenn das Abfrageformular eine Fuzzysuche, Platzhalterabfrage oder Abfrage mit regulären Ausdrücken ausführt oder es sich um einen **$filter**-Ausdruck handelt. 

## <a name="scoring-profiles"></a>Bewertungsprofile

Sie können die Sortierung der verschiedenen Felder anpassen, indem Sie ein benutzerdefiniertes *Bewertungsprofil* definieren. Bewertungsprofile bieten Ihnen mehr Kontrolle über die Rangfolge der Elemente in Suchergebnissen. Sie können beispielsweise Elemente auf Basis ihres Umsatzpotentials optimieren, neuere Elemente hochstufen oder auch Elemente fördern, die sich bereits zu lange im Lager befinden. 

Ein Bewertungsprofil ist Teil der Indexdefinition und besteht aus gewichteten Feldern, Funktionen und Parametern. Weitere Informationen zum Definieren von Bewertungsprofilen finden Sie unter [Hinzufügen von Bewertungsprofilen zu einem Index für Azure Cognitive Search](index-add-scoring-profiles.md).

## <a name="scoring-statistics"></a>Bewertungsstatistiken

Aus Gründen der Skalierbarkeit verteilt Azure Cognitive Search jeden Index horizontal mithilfe eines Shardingprozesses. Dies bedeutet, dass Teile eines Indexes physisch voneinander getrennt sind.

Standardmäßig wird die Bewertung eines Dokuments basierend auf statistischen Eigenschaften der Daten *innerhalb eines Shards* berechnet. Diese Vorgehensweise stellt in der Regel bei einem großen Datenkorpus kein Problem dar und bietet eine bessere Leistung als die Berechnung der Bewertung auf Grundlage von Informationen in allen Shards. Allerdings kann die Verwendung dieser Leistungsoptimierung dazu führen, dass zwei sehr ähnliche (oder sogar identische) Dokumente unterschiedliche Relevanzbewertungen erhalten, wenn sie sich in verschiedenen Shards befinden.

Wenn Sie die Bewertung lieber auf Grundlage der statistischen Eigenschaften in allen Shards berechnen möchten, können Sie dies tun, indem Sie *scoringStatistics=global* als [Abfrageparameter](https://docs.microsoft.com/rest/api/searchservice/search-documents) hinzufügen (oder *"scoringStatistics": "global"* als Textkörperparameter der [Abfrageanforderung](https://docs.microsoft.com/rest/api/searchservice/search-documents)).

```http
GET https://[service name].search.windows.net/indexes/[index name]/docs?scoringStatistics=global
  Content-Type: application/json
  api-key: [admin key]  
```

> [!NOTE]
> Für den Parameter `scoringStatistics` ist ein Administrator-API-Schlüssel erforderlich.

## <a name="similarity-ranking-algorithms"></a>Ähnlichkeitsalgorithmus für die Rangfolge

Azure Cognitive Search unterstützt zwei verschiedene Ähnlichkeitsalgorithmen für die Rangfolge: einen *klassischen Ähnlichkeitsalgorithmus* und die offizielle Implementierung des *Okapi BM25*-Algorithmus (derzeit in der Vorschauphase). Der klassische Ähnlichkeitsalgorithmus ist der Standardalgorithmus. Allerdings werden alle nach dem 15. Juli neu erstellten Dienste den neuen BM25-Algorithmus verwenden. Dieser wird als einziger Algorithmus für neue Dienste zur Verfügung stehen.

Aktuell können Sie angeben, welchen Ähnlichkeitsalgorithmus für die Rangfolge Sie verwenden möchten. Weitere Informationen finden Sie unter [Ähnlichkeitsalgorithmus für die Rangfolge in Azure Cognitive Search](index-ranking-similarity.md).

## <a name="watch-this-video"></a>Video ansehen

In diesem 16-minütigen Video erläutert Softwareentwickler Raouf Merouche den Prozess der Indizierung und Abfrage sowie das Erstellen von Bewertungsprofilen. Es vermittelt eine gute Vorstellung davon, was im Hintergrund passiert, wenn Ihre Dokumente indiziert und abgerufen werden.

>[!VIDEO https://channel9.msdn.com/Shows/AI-Show/Similarity-and-Scoring-in-Azure-Cognitive-Search/player]

+ In Minute 2 bis 3 werden folgende Aspekte der Indizierung behandelt: Textverarbeitung und lexikalische Analyse.
+ In Minute 3 bis 4 wird folgender Aspekt der Indizierung behandelt: invertierte Indizes.
+ In Minute 4 bis 6 werden folgende Aspekte der Abfrage behandelt: Abrufen und Rangzuweisung.
+ In Minute 7 bis 16 werden Bewertungsprofile behandelt.

## <a name="see-also"></a>Weitere Informationen

 [Hinzufügen von Bewertungsprofilen zu einem Index für Azure Cognitive Search](index-add-scoring-profiles.md) [Azure Cognitive Search-Dienst REST](https://docs.microsoft.com/rest/api/searchservice/)   
 [Dokumente durchsuchen (Azure Cognitive Search-REST-API)](https://docs.microsoft.com/rest/api/searchservice/search-documents)   
 [.NET SDK für die kognitive Azure-Suche](https://docs.microsoft.com/dotnet/api/overview/azure/search?view=azure-dotnet)  
