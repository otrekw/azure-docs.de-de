---
title: Erstellen einer Vorschlagsfunktion
titleSuffix: Azure Cognitive Search
description: Aktivieren Sie Eingabevorschläge für Abfrageaktionen in der kognitiven Azure-Suche, indem Sie Vorschlagsfunktionen erstellen und Anforderungen formulieren, die automatisch Vervollständigungen oder Vorschläge für Abfrageausdrücke abrufen.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 04/21/2020
ms.openlocfilehash: 7eb2988628d60fa72c7d83b81a58a1e0fae5de33
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2020
ms.locfileid: "81770100"
---
# <a name="create-a-suggester-to-enable-autocomplete-and-suggested-results-in-a-query"></a>Erstellen einer Vorschlagsfunktion zum Ermöglichen von AutoVervollständigen und vorgeschlagenen Ergebnissen in einer Abfrage

In Azure Cognitive Search wird die Suche während der Eingabe über ein Konstrukt mit einer **Vorschlagsfunktion** ermöglicht, das einem [Suchindex](search-what-is-an-index.md) hinzugefügt wird. Eine Vorschlagsfunktion unterstützt zwei Umgebungen: *AutoVervollständigen* zum Vervollständigen einer partiellen Eingabe zu einem Gesamtbegriff für eine Abfrage und *Vorschläge*, die zum Durchklicken zu einer bestimmten Übereinstimmung einladen. AutoVervollständigen führt zu einer Abfrage. Vorschläge führen zu einem übereinstimmenden Dokument.

Der folgende Screenshot aus dem Beispiel [Erstellen Ihrer ersten App in C#](tutorial-csharp-type-ahead-and-suggestions.md) veranschaulicht beide Varianten. AutoVervollständigen erwartet einen potenziellen Begriff und ergänzt etwa „Zw“ mit „illing“. Vorschläge sind Minisuchergebnisse, bei denen ein Feld wie „Hotelname“ für ein entsprechendes Hotelsuchdokument aus dem Index steht. Für Vorschläge können Sie alle Felder bereitstellen, die beschreibende Informationen enthalten.

![Visueller Vergleich von AutoVervollständigen und vorgeschlagenen Abfragen](./media/index-add-suggesters/hotel-app-suggestions-autocomplete.png "Visueller Vergleich von AutoVervollständigen und vorgeschlagenen Abfragen")

Sie können diese Features einzeln oder zusammen verwenden. Um dieses Verhalten in der kognitiven Azure-Suche zu implementieren, gibt es eine Index- und eine Abfragekomponente. 

+ Fügen Sie im Index eine Vorschlagsfunktion zu einem Index hinzu. Sie können das Portal, eine [REST-API](https://docs.microsoft.com/rest/api/searchservice/create-index) oder das [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.suggester?view=azure-dotnet) verwenden. Im weiteren Verlauf dieses Artikels geht es um die Erstellung einer Vorschlagsfunktion.

+ Rufen Sie in der Abfrageanforderung eine der [folgenden APIs](#how-to-use-a-suggester) auf.

Die Unterstützung der Suche während der Eingabe wird auf Feldebene für Zeichenfolgenfelder aktiviert. Sie können beide Verhaltensweisen für Eingabevorschläge in derselben Suchlösung implementieren, wenn Sie ein ähnliches Erlebnis wie im Screenshot wünschen. Beide Anforderungen zielen auf die *Dokumentensammlung* eines bestimmten Index ab, und die Antworten werden zurückgegeben, nachdem ein Benutzer eine Zeichenfolge aus mindestens 3 Zeichen eingegeben hat.

## <a name="what-is-a-suggester"></a>Was ist eine Vorschlagsfunktion?

Bei einer Vorschlagsfunktion handelt es sich um eine interne Datenstruktur, die das Verhalten einer Suche während der Eingabe unterstützt, indem Präfixe für die Übereinstimmungssuche bei partiellen Abfragen gespeichert werden. Ähnlich wie bei tokenisierten Begriffen werden Präfixe in invertierten Indizes gespeichert – eines für jedes Feld, das in der Feldsammlung der Vorschlagsfunktion angegeben ist.

## <a name="define-a-suggester"></a>Definieren einer Vorschlagsfunktion

Um eine Vorschlagsfunktion zu erstellen, fügen Sie sie zu einem [Indexschema](https://docs.microsoft.com/rest/api/searchservice/create-index) hinzu, und [legen Sie jede Eigenschaft fest](#property-reference). Der beste Zeitpunkt zum Erstellen einer Vorschlagsfunktion ist bei der Definition des Felds, für das sie verwendet wird.

+ Verwenden Sie nur Zeichenfolgenfelder.

+ Verwenden Sie für das Feld das standardmäßige Lucene-Standardanalysetool (`"analyzer": null`) oder ein [Sprachanalysetool](index-add-language-analyzers.md) (z. B. `"analyzer": "en.Microsoft"`).

### <a name="choose-fields"></a>Auswählen von Feldern

Auch wenn eine Vorschlagsfunktion über mehrere Eigenschaften verfügt, handelt es sich in erster Linie jedoch um eine Sammlung von Zeichenfolgenfeldern, für die Sie eine Umgebung für die Suche während der Eingabe aktivieren. Es gibt für jeden Index eine Vorschlagsfunktion. Daher muss die Liste der Vorschlagsfunktionen alle Felder enthalten, die Inhalte für Vorschläge und AutoVervollständigen beitragen.

AutoVervollständigen profitiert von einem größeren Pool von Feldern, aus denen Begriffe abgerufen werden können, da der zusätzliche Inhalt mehr Potenzial für die Vervollständigung von Begriffen bietet.

Andererseits führen Vorschläge zu besseren Ergebnissen, wenn die Feldauswahl selektiv ist. Denken Sie daran, dass es sich bei einem Vorschlag um einen Proxy für ein Suchdokument handelt. Daher benötigen Sie Felder, die am besten ein Einzelergebnis darstellen. Namen, Titel oder andere eindeutige Felder, die für Unterscheidung zwischen mehreren Übereinstimmungen sorgen, funktionieren am besten. Wenn Felder aus wiederkehrenden Werten bestehen, setzen sich die Vorschläge aus identischen Ergebnissen zusammen, und ein Benutzer weiß nicht, auf welches er klicken soll.

Um beide Anforderungen für die Suche während der Eingabe zu erfüllen, fügen Sie alle Felder hinzu, die Sie zum AutoVervollständigen benötigen, und steuern Sie dann mit **$Select**, **$top**, **$filter** und **searchFields** die Ergebnisse für Vorschläge.

### <a name="choose-analyzers"></a>Auswählen der Analysetools

Durch die Auswahl eines Analysetools legen Sie fest, wie Felder in Token umgewandelt und anschließend mit einem Präfix versehen werden. Beispielsweise führt die Verwendung eines Sprachanalysetools für eine Zeichenfolge mit Bindestrichen wie „App-basiert“ zu den Tokenkombinationen „App“, „basiert“, „App-basiert“. Wenn Sie das Standard-Lucene-Analysetool verwenden, ist die Zeichenfolge mit dem Bindestrich nicht enthalten. 

Berücksichtigen Sie bei der Auswertung von Analysetools auch die Verwendung der [API für die Textanalyse](https://docs.microsoft.com/rest/api/searchservice/test-analyzer), um zu erkennen, wie Begriffe tokenisiert und anschließend mit Präfixen versehen werden. Beim Erstellen eines Indexes können Sie verschiedene Analysetools für eine Zeichenfolge ausprobieren, um sich die ausgegebenen Token anzusehen.

Felder, die [benutzerdefinierte Analysetools](index-add-custom-analyzers.md) oder [vordefinierte Analysetools](index-add-custom-analyzers.md#predefined-analyzers-reference) verwenden (mit Ausnahme von Standard-Lucene) sind explizit nicht zulässig, um mangelhafte Ergebnisse zu vermeiden.

> [!NOTE]
> Wenn Sie Einschränkungen der Analysetools umgehen müssen, z. B. wenn Sie ein Schlüsselwort oder eine NGram-Analyse für bestimmte Abfrageszenarien benötigen, sollten Sie zwei separate Felder für denselben Inhalt verwenden. Eines der Felder kann so über Vorschlagsfunktionen verfügen, während das andere mit einer benutzerdefinierten Analysetoolkonfiguration eingerichtet wird.

### <a name="when-to-create-a-suggester"></a>Wann wird eine Vorschlagsfunktion erstellt?

Der beste Zeitpunkt zum Erstellen einer Vorschlagsfunktion ist während der Erstellung der eigentlichen Felddefinition.

Wenn Sie versuchen, eine Vorschlagsfunktion mithilfe bereits vorhandener Felder zu erstellen, wird dies von der API nicht zugelassen. Präfixe werden während der Indizierung erstellt, wenn partielle Begriffe in Kombinationen aus mehreren Zeichen zusammen mit ganzen Begriffen in Token umgewandelt werden. Wenn vorhandene Felder bereits tokenisiert sind, müssen Sie den Index neu erstellen, wenn Sie ihn zu einer Vorschlagsfunktion hinzufügen möchten. Weitere Informationen finden Sie unter [Neuerstellen eines Azure Cognitive Search-Indexes](search-howto-reindex.md).

## <a name="create-using-rest"></a>Erstellen mit der REST-API

Fügen Sie Vorschlagsfunktionen in der REST-API über [Index erstellen](https://docs.microsoft.com/rest/api/searchservice/create-index) oder [Index aktualisieren](https://docs.microsoft.com/rest/api/searchservice/update-index) hinzu. 

  ```json
  {
    "name": "hotels-sample-index",
    "fields": [
      . . .
          {
              "name": "HotelName",
              "type": "Edm.String",
              "facetable": false,
              "filterable": false,
              "key": false,
              "retrievable": true,
              "searchable": true,
              "sortable": false,
              "analyzer": "en.microsoft",
              "indexAnalyzer": null,
              "searchAnalyzer": null,
              "synonymMaps": [],
              "fields": []
          },
    ],
    "suggesters": [
      {
        "name": "sg",
        "searchMode": "analyzingInfixMatching",
        "sourceFields": ["HotelName"]
      }
    ],
    "scoringProfiles": [
      . . .
    ]
  }
  ```

## <a name="create-using-net"></a>Erstellen mit .NET

Definieren Sie in C# ein [Suggester-Objekt](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.suggester?view=azure-dotnet). `Suggesters` ist eine Sammlung, aber sie kann nur ein Element enthalten. 

```csharp
private static void CreateHotelsIndex(SearchServiceClient serviceClient)
{
    var definition = new Index()
    {
        Name = "hotels-sample-index",
        Fields = FieldBuilder.BuildForType<Hotel>(),
        Suggesters = new List<Suggester>() {new Suggester()
            {
                Name = "sg",
                SourceFields = new string[] { "HotelName", "Category" }
            }}
    };

    serviceClient.Indexes.Create(definition);

}
```

## <a name="property-reference"></a>Eigenschaftsverweis

|Eigenschaft      |BESCHREIBUNG      |
|--------------|-----------------|
|`name`        |Der Name der Vorschlagsfunktion.|
|`searchMode`  |Die Strategie, mit der nach möglichen Ausdrücken gesucht wird. Aktuell wird nur der Modus `analyzingInfixMatching` unterstützt, der derzeit am Anfang eines Begriffs Übereinstimmungen findet.|
|`sourceFields`|Eine Liste mit einem oder mehreren Feldern, die als Quelle für den Inhalt von Vorschlägen dienen. Felder müssen vom Typ `Edm.String` und `Collection(Edm.String)` sein. Wenn ein Analysetool für das Feld angegeben wird, muss es sich um ein benanntes Analysetool aus [dieser Liste](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.analyzername?view=azure-dotnet) (kein benutzerdefiniertes Analysetool) handeln.<p/> Folgende Vorgehensweise wird empfohlen: Geben Sie nur die Felder an, die sich für eine erwartete und angemessene Antwort eignen, sei es eine vollständige Zeichenfolge in einer Suchleiste oder eine Dropdownliste.<p/>Ein Hotelname ist ein guter Kandidat, weil er präzise ist. Ausführliche Felder wie Beschreibungen und Kommentare sind zu informationsreich. Sich wiederholende Felder wie Kategorien und Tags sind ebenso weniger effektiv. In den Beispielen schließen wir ohnehin „category“ ein, um zu zeigen, dass Sie mehrere Felder einbeziehen können. |

<a name="how-to-use-a-suggester"></a>

## <a name="use-a-suggester"></a>Verwenden einer Vorschlagsfunktion

Eine Vorschlagsfunktion wird in einer Abfrage verwendet. Nachdem Sie eine Vorschlagsfunktion erstellt haben, rufen Sie eine der folgenden APIs auf, um eine Umgebung für die Suche während der Eingabe bereitzustellen:

+ [Vorschläge-REST-API](https://docs.microsoft.com/rest/api/searchservice/suggestions) 
+ [AutoVervollständigen-REST-API](https://docs.microsoft.com/rest/api/searchservice/autocomplete) 
+ [SuggestWithHttpMessagesAsync-Methode](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations.suggestwithhttpmessagesasync?view=azure-dotnet)
+ [AutocompleteWithHttpMessagesAsync-Methode](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations.autocompletewithhttpmessagesasync?view=azure-dotnet&viewFallbackFrom=azure-dotnet)

In einer Suchanwendung sollte für Clientcode eine Bibliothek wie [jQuery UI Autocomplete](https://jqueryui.com/autocomplete/) verwendet werden, um die partielle Abfrage zu erfassen und die Übereinstimmung bereitzustellen. Weitere Informationen zu dieser Aufgabe finden Sie unter [Hinzufügen von AutoVervollständigen oder Vorschlägen zu Clientcode](search-autocomplete-tutorial.md).

Die Verwendung der API wird im folgenden Aufruf der AutoVervollständigen-REST-API veranschaulicht. In diesem Beispiel ergeben sich zwei wichtige Erkenntnisse. Erstens: Wie bei allen Abfragen wird der Vorgang für die Dokumentensammlung eines Indexes ausgeführt. Die Abfrage enthält einen **Such**parameter, der in diesem Fall die partielle Abfrage bereitstellt. Zweitens: Sie müssen der Anforderung den Namen der Vorschlagsfunktion (**suggesterName**) hinzufügen. Wenn eine Vorschlagsfunktion nicht im Index definiert ist, tritt bei einem Aufruf von „AutoVervollständigen“ oder „Vorschläge“ ein Fehler auf.

```http
POST /indexes/myxboxgames/docs/autocomplete?search&api-version=2019-05-06
{
  "search": "minecraf",
  "suggesterName": "sg"
}
```

## <a name="sample-code"></a>Beispielcode

+ Das Beispiel [Erstellen Ihrer ersten App in C# (Lektion 3 – Hinzufügen der Suche während der Eingabe)](tutorial-csharp-type-ahead-and-suggestions.md) veranschaulicht ein Vorschlagsfunktion-Konstrukt, vorgeschlagene Abfragen, AutoVervollständigen und Facettennavigation. Dieses Codebeispiel wird in einem Sandboxdienst für die kognitive Azure-Suche ausgeführt und verwendet einen vorinstallierten Hotelindex, sodass Sie zum Ausführen der Anwendung lediglich F5 drücken müssen. Es ist weder ein Abonnement noch eine Anmeldung erforderlich.

+ [DotNetHowToAutocomplete](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToAutocomplete) ist ein älteres Beispiel, das sowohl C#- als auch Java-Code enthält. Es veranschaulicht ebenfalls ein Vorschlagsfunktion-Konstrukt, vorgeschlagene Abfragen, AutoVervollständigen und Facettennavigation. In diesem Codebeispiel werden die Beispieldaten des gehosteten [NYCJobs](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs) verwendet. 

## <a name="next-steps"></a>Nächste Schritte

Wir empfehlen den folgenden Artikel, um mehr über das Formulieren von Anforderungen zu erfahren.

> [!div class="nextstepaction"]
> [Hinzufügen von AutoVervollständigen und Vorschlägen zu Clientcode](search-autocomplete-tutorial.md) 
