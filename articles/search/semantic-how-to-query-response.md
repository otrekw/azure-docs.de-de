---
title: Strukturieren einer semantischen Antwort
titleSuffix: Azure Cognitive Search
description: Hier finden Sie Informationen zum semantischen Rangfolgealgorithmus in Cognitive Search sowie zur Strukturierung semantischer Antworten und Beschriftungen aus einem Resultset.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 03/02/2021
ms.custom: references_regions
ms.openlocfilehash: febbfd0f3def8e681107ef78d9478463b1c2a872
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101678759"
---
# <a name="semantic-ranking-and-responses-in-azure-cognitive-search"></a>Semantische Rangfolge und Antworten in Azure Cognitive Search

> [!IMPORTANT]
> Der semantische Rangfolgealgorithmus sowie semantische Antworten/Beschriftungen befinden sich in der Public Preview-Phase und sind nur über die Vorschau-REST-API verfügbar. Vorschaufeatures werden wie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) beschrieben im Ist-Zustand angeboten.

Die semantische Rangfolge verbessert die Genauigkeit von Suchergebnissen. Hierzu wird die Rangfolge der besten Treffer mithilfe eines semantischen Rangfolgemodells angepasst, das für Abfragen in natürlicher Sprache (im Gegensatz zu Schlüsselwörtern) trainiert ist.

Dieser Artikel enthält Informationen zum semantischen Rangfolgealgorithmus und zur Generierung einer semantischen Antwort. Eine Antwort enthält Beschriftungen, sowohl in Klartext als auch mit Hervorhebungen, sowie Antworten (optional).

+ Semantische Beschriftungen sind Textabschnitte, die für die aus den Suchergebnissen extrahierte Abfrage relevant sind. Sie können dabei helfen, ein Ergebnis zusammenzufassen, wenn einzelne Inhaltsfelder zu groß für die Inhaltsseite sind. Beschriftungen umfassen semantische Hervorhebungen, die es Benutzern ermöglichen, Abfrageergebnisse schnell zu überfliegen, um die relevantesten Dokumente zu finden, wodurch das gesamte Benutzererlebnis verbessert wird.

+ Bei semantischen Antworten werden Machine Learning-Modelle von Bing verwendet, um Antworten auf Abfragen zu formulieren, die wie Fragen aussehen. Die Antworten werden aus einer Liste von für die Abfrage relevantesten Passagen ausgewählt, so wie sie auch aus den obersten Dokumenten im Abfrageresultset extrahiert wurden. Antworten werden als unabhängiges Objekt der obersten Ebene in der Abfrageantwortnutzlast zurückgegeben. Sie können auswählen, ob dieses Objekt auf den Suchseiten zusammen mit den Suchergebnissen gerendert werden soll.

## <a name="prerequisites"></a>Voraussetzungen

+ Als Semantikabfragen formulierte Abfragen. Weitere Informationen finden Sie unter [Erstellen einer Semantikabfrage](semantic-how-to-query-request.md).

## <a name="understanding-a-semantic-response"></a>Grundlegendes zu semantischen Antworten

Eine semantische Antwort enthält neue Eigenschaften für Bewertungen, Beschriftungen und Antworten. Eine semantische Antwort wird auf der Grundlage der Standardantwort erstellt. Hierzu werden die 50 besten Ergebnisse, die von der [Engine für die Volltextsuche](search-lucene-query-architecture.md) zurückgegeben wurden, mithilfe des semantischen Bewerters neu sortiert. Wurden mehr als 50 angegeben, werden die zusätzlichen Ergebnisse zwar zurückgegeben, aber nicht semantisch neu sortiert.

Eine Antwort umfasst genau wie bei anderen Abfragen entweder alle als abrufbar markierten Felder oder nur Felder, die in der Auswahlanweisung aufgeführt sind. Darüber hinaus enthält sie auch ein Antwortfeld und Beschriftungen.

+ Für jedes semantische Ergebnis ist standardmäßig eine einzelne Antwort vorhanden. Diese Antwort wird als individuelles Feld zurückgegeben, das auf Wunsch auf einer Suchseite gerendert werden kann. Sie können bis zu fünf Antworten angeben. Die Antwort wird automatisch formuliert. Hierzu werden alle Dokumente in den ersten Ergebnissen gelesen und eine extrahierende Zusammenfassung sowie Machine Reading Comprehension durchgeführt, bevor letztendlich im Antwortfeld eine direkte Antwort auf die Frage des Benutzers bereitgestellt wird.

+ Bei einer Beschriftung handelt es sich um eine extraktionsbasierte Zusammenfassung von Dokumentinhalten, die als Nur-Text oder mit Hervorhebungen zurückgegeben wird. Beschriftungen werden automatisch eingeschlossen und können nicht unterdrückt werden. Für Hervorhebungen wird mithilfe von Machine Reading Comprehension ermittelt, welche Zeichenfolgen hervorgehoben werden sollen. Hervorhebungen lenken die Aufmerksamkeit auf die relevantesten Passagen, damit Sie schnell eine Seite mit Ergebnissen überfliegen können, um das richtige Dokument zu finden.

Bei einem semantisch neu bewerteten Resultset werden Ergebnisse nach dem Wert @search.rerankerScore sortiert. Wenn Sie eine OrderBy-Klausel hinzufügen, wird ein HTTP 400-Fehler zurückgegeben, da diese Klausel in einer Semantikabfrage nicht unterstützt wird.

## <a name="example"></a>Beispiel

@search.rerankerScore ist parallel zum Standardwert @search.score vorhanden und wird verwendet, um die Rangfolge der Ergebnisse anzupassen.

Sehen Sie sich beispielsweise die folgende Abfrage an:

```http
POST /indexes/hotels-sample-index/docs/search?api-version=2020-06-30-Preview
{
    "search": "newer hotel near the water with a great restaurant",
    "queryType": "semantic",
    "queryLanguage": "en-us",
    "answers": "none",
    "searchFields": "HotelName,Category,Description",
    "select": "HotelId,HotelName,Description,Category",
    "count": true
}
```

Eine semantische Antwort sieht in etwa wie im folgenden Ergebnis aus. In diesen Ergebnissen werden lediglich die ersten drei Antworten angezeigt (sortiert nach @search.rerankerScore). Wie Sie sehen, wird „Oceanside Resort“ jetzt an erster Stelle angezeigt. In der Standardrangfolge von @search.score hätte sich dieses Ergebnis an zweiter Stelle nach „Trails End“ befunden.

```http
{
    "@odata.count": 31,
    "@search.answers": [],
    "value": [
        {
            "@search.score": 1.8920634,
            "@search.rerankerScore": 1.1091284966096282,
            "@search.captions": [
                {
                    "text": "Oceanside Resort. Budget. New Luxury Hotel.  Be the first to stay. Bay views from every room, location near the piper, rooftop pool, waterfront dining & more.",
                    "highlights": "<em>Oceanside Resort.</em> Budget. New Luxury Hotel.  Be the first to stay.<em> Bay views</em> from every room, location near the piper, rooftop pool, waterfront dining & more."
                }
            ],
            "HotelId": "18",
            "HotelName": "Oceanside Resort",
            "Description": "New Luxury Hotel.  Be the first to stay. Bay views from every room, location near the piper, rooftop pool, waterfront dining & more.",
            "Category": "Budget"
        },
        {
            "@search.score": 2.5204072,
            "@search.rerankerScore": 1.0731962407007813,
            "@search.captions": [
                {
                    "text": "Trails End Motel. Luxury. Only 8 miles from Downtown.  On-site bar/restaurant, Free hot breakfast buffet, Free wireless internet, All non-smoking hotel. Only 15 miles from airport.",
                    "highlights": "<em>Trails End Motel.</em> Luxury. Only 8 miles from Downtown.  On-site bar/restaurant, Free hot breakfast buffet, Free wireless internet, All non-smoking hotel. Only 15 miles from airport."
                }
            ],
            "HotelId": "40",
            "HotelName": "Trails End Motel",
            "Description": "Only 8 miles from Downtown.  On-site bar/restaurant, Free hot breakfast buffet, Free wireless internet, All non-smoking hotel. Only 15 miles from airport.",
            "Category": "Luxury"
        },
        {
            "@search.score": 1.4104348,
            "@search.rerankerScore": 1.06992666143924,
            "@search.captions": [
                {
                    "text": "Winter Panorama Resort. Resort and Spa. Newly-renovated with large rooms, free 24-hr airport shuttle & a new restaurant. Rooms/suites offer mini-fridges & 49-inch HDTVs.",
                    "highlights": "<em>Winter Panorama Resort</em>. Resort and Spa. Newly-renovated with large rooms, free 24-hr airport shuttle & a new restaurant. Rooms/suites offer mini-fridges & 49-inch HDTVs."
                }
            ],
            "HotelId": "12",
            "HotelName": "Winter Panorama Resort",
            "Description": "Newly-renovated with large rooms, free 24-hr airport shuttle & a new restaurant. Rooms/suites offer mini-fridges & 49-inch HDTVs.",
            "Category": "Resort and Spa"
        }
```

## <a name="next-steps"></a>Nächste Schritte

+ [Übersicht über die semantische Suche](semantic-search-overview.md)
+ [Ähnlichkeitsalgorithmus](index-ranking-similarity.md)
+ [Erstellen einer Semantikabfrage](semantic-how-to-query-request.md)
+ [Erstellen einer einfachen Abfrage](search-query-create.md)