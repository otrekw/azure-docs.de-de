---
title: Einführung in Azure Cognitive Search
titleSuffix: Azure Cognitive Search
description: Azure Cognitive Search ist ein vollständig verwalteter Cloudsuchdienst von Microsoft. Hier erfahren Sie mehr über Anwendungsfälle, den Entwicklungsworkflow und Vergleiche mit anderen Microsoft-Suchprodukten. Außerdem erhalten Sie Informationen zu den ersten Schritten.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: overview
ms.date: 10/22/2020
ms.custom: contperfq1
ms.openlocfilehash: f9a5197b982958fe0a0ff21c4b442142beb38882
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2020
ms.locfileid: "93422466"
---
# <a name="what-is-azure-cognitive-search"></a>Was ist Azure Cognitive Search?

Azure Cognitive Search ([früher „Azure Search“](whats-new.md#new-service-name)) ist ein Cloudsuchdienst, der Entwicklern APIs und Tools zum Erstellen umfangreicher Suchfunktionen für private, heterogene Inhalte in Web- und Unternehmensanwendungen sowie in mobilen Anwendungen bietet.

Wenn Sie einen Cognitive Search-Dienst erstellen, erhalten Sie eine Suchmaschine, die die Indizierung und Abfrageausführung übernimmt, eine beständige Speicherung der von Ihnen erstellten und verwalteten Indizes sowie eine Abfragesprache zum Verfassen einfacher und komplexer Abfragen. Ein Suchdienst kann optional in andere Azure-Dienste integriert werden. Dies geschieht in Form von *Indexern* , die das Erfassen/Abrufen von Daten aus Azure-Datenquellen automatisieren, und *Qualifikationsgruppen* , mit denen nutzbare KI aus Cognitive Services (z. B. die Bild- und Textanalyse) oder benutzerdefinierte KI, die Sie in Azure Machine Learning erstellen oder in Azure Functions einbinden, einbezogen wird.

![Architektur von Azure Cognitive Search](media/search-what-is-azure-search/azure-search-diagram.svg "Architektur von Azure Cognitive Search")

Aus architektonischer Sicht befindet sich ein Suchdienst zwischen den externen Datenspeichern, die Ihre nicht indizierten Daten enthalten, und einer Client-App, die Abfrageanforderungen an einen Suchindex sendet und die Antwort verarbeitet.  Ein Indexschema bestimmt die Struktur von durchsuchbarem Inhalt. 

Die beiden primären Workloads eines Suchdiensts sind *Indizierung* und *Abfrage*.

+ Durch Indizierung wird Text in Ihren Suchdienst integriert und durchsuchbar. Intern wird eingehender Text in Token verarbeitet und für schnelle Scans in invertierten Indizes gespeichert. Während der Indizierung haben Sie die Möglichkeit, *kognitive Skills* hinzuzufügen. Dabei kann es sich entweder um von Microsoft vordefinierte oder von Ihnen erstellte benutzerdefinierte Skills handeln. Die anschließenden Analysen und Transformationen können zu neuen Informationen und Strukturen führen, die zuvor nicht vorhanden waren und für viele Such- und Knowledge Mining-Szenarien von großem Nutzen sind.

+ Sobald ein Index mit durchsuchbaren Daten aufgefüllt wurde, sendet Ihre Client-App Abfrageanforderungen an einen Suchdienst und verarbeitet die Antworten. Die gesamte Abfrageausführung erfolgt über einem Suchindex, den Sie erstellen, besitzen und in Ihrem Dienst speichern. In Ihrer Client-App wird die Suchfunktion mithilfe von APIs aus Azure Cognitive Search definiert und kann Relevanzoptimierung, automatische Vervollständigung, Synonymabgleich, Fuzzyübereinstimmung, Musterabgleich, Filter und Sortierung umfassen.

Die Funktionalität wird über eine einfache [REST-API](/rest/api/searchservice/) oder ein [.NET SDK](search-howto-dotnet-sdk.md) bereitgestellt, sodass die inhärente Komplexität des Informationsabrufs verborgen bleibt. Sie können auch das Azure-Portal für die Dienst- und Inhaltsverwaltung mit Tools für die Prototyperstellung und Abfrage Ihrer Indizes und Qualifikationsgruppen verwenden. Da der Dienst in der Cloud ausgeführt wird, werden die Infrastruktur und Verfügbarkeit von Microsoft verwaltet.

## <a name="when-to-use-cognitive-search"></a>Einsatzgebiete von Cognitive Search

Azure Cognitive Search eignet sich sehr gut für die folgenden Anwendungsszenarien:

+ Konsolidierung von heterogenen Inhaltstypen in einem privaten, benutzerdefinierten Suchindex. Sie können einen Suchindex mit Streams aus JSON-Dokumenten aus jeder beliebigen Quelle auffüllen. Verwenden Sie für unterstützte Quellen in Azure einen *Indexer* , um die Indizierung zu automatisieren. Die Steuerung des Indexschemas und des Aktualisierungszeitplans ist ein Hauptgrund für die Verwendung von Cognitive Search.

+ Einfache Implementierung von Features in Zusammenhang mit der Suche. Such-APIs vereinfachen Abfrageerstellung, Facettennavigation, Filter (einschließlich geografisch-räumlicher Suche), Synonymzuordnung, automatische Vervollständigung und Relevanzoptimierung. Mithilfe von integrierten Features können Sie die Erwartungen von Endbenutzern an einen Suchdienst erfüllen, der der Funktionalität von kommerziellen Suchmaschinen in nichts nachsteht.

+ Bei unformatiertem Inhalt handelt es sich um große undifferenzierte Text- oder Bilddateien oder um Anwendungsdateien, die in Azure Blob Storage oder Cosmos DB gespeichert sind. Sie können [kognitive Qualifikationen](cognitive-search-concept-intro.md) während der Indizierung anwenden, um Text zu identifizieren und extrahieren, eine Struktur zu erstellen oder neue Informationen wie übersetzten Text oder Entitäten zu erstellen.

+ Inhalte erfordern eine linguistische oder benutzerdefinierte Textanalyse. Bei nicht englischsprachigen Inhalten unterstützt Azure Cognitive Search sowohl Lucene-Analysetools als auch die Microsoft-Prozessoren für die Verarbeitung natürlicher Sprache. Sie können Analysetools auch so konfigurieren, dass Rohdaten einer speziellen Verarbeitung unterzogen werden, um z. B. diakritische Zeichen herauszufiltern oder Muster in Zeichenfolgen zu erkennen und beizubehalten.

Weitere Informationen zu bestimmten Funktionen finden Sie unter [Azure Cognitive Search-Features](search-features-list.md).

## <a name="how-to-use-cognitive-search"></a>Verwendung von Cognitive Search

### <a name="step-1-provision-service"></a>Schritt 1: Bereitstellen des Diensts

Sie können einen [kostenlosen Dienst erstellen](search-create-service-portal.md), der gemeinsam mit anderen Abonnenten genutzt wird, oder einen [kostenpflichtigen Tarif](https://azure.microsoft.com/pricing/details/search/) auswählen, bei dem Ressourcen zur ausschließlichen Nutzung für Ihren Dienst reserviert werden. Alle Schnellstartanleitungen und Tutorials können mit dem kostenlosen Dienst abgeschlossen werden.

Bei kostenpflichtigen Tarifen können Sie einen Dienst in zwei Dimensionen skalieren und so die Ressourcen entsprechend den Produktionsanforderungen abstimmen:

+ Hinzufügen von Replikaten, um die Kapazität für ein hohes Abfrageaufkommen zu erhöhen
+ Hinzufügen von Partitionen, um den Speicher für weitere Dokumente zu vergrößern

### <a name="step-2-create-an-index"></a>Schritt 2: Erstellen eines Index

Definieren Sie ein zuzuordnendes Indexschema, um die Struktur der Dokumente widerzuspiegeln, die Sie durchsuchen möchten (ähnlich wie bei Feldern in einer Datenbank). Ein Suchindex ist eine spezialisierte Datenstruktur, die für die schnelle Abfrageausführung optimiert ist.

Häufig wird das [Indexschema im Azure-Portal erstellt](search-what-is-an-index.md) oder programmgesteuert mithilfe des [.NET SDK](search-howto-dotnet-sdk.md) oder der [REST-API](/rest/api/searchservice/).

> [!TIP]
> Beginnen Sie mit [Schnellstart: Datenimport-Assistent](search-get-started-portal.md), um einen Index innerhalb weniger Minuten zu erstellen, zu laden und abzufragen.

### <a name="step-3-load-data"></a>Schritt 3: Laden der Daten

Nachdem Sie einen Index definiert haben, können Sie Inhalte hochladen. Sie können entweder ein Push- oder Pullmodell verwenden.

Das Pushmodell überträgt JSON-Dokumente mithilfe von APIs aus einem [SDK](search-howto-dotnet-sdk.md) oder [REST](/rest/api/searchservice/addupdate-or-delete-documents) in einen Index. Beim externen Dataset kann es sich praktisch um jede Datenquelle handeln, solange es JSON-Dokumente sind.

Das Pullmodell ruft Daten aus Quellen in Azure ab und sendet sie an einen Suchindex. Das Pullmodell wird über [*Indexer*](/rest/api/searchservice/Indexer-operations) implementiert, die Aspekte der Datenerfassung optimieren und automatisieren, z. B. das Herstellen einer Verbindung mit Daten, Lesen und Serialisieren von Daten. Zu den unterstützten Datenquellen gehören Azure Cosmos DB, Azure SQL und Azure Storage.

### <a name="step-4-send-queries-and-handle-responses"></a>Schritt 4: Senden von Abfragen und Verarbeiten von Antworten

Nach dem Füllen eines Indexes können Sie über einfache HTTP-Anforderungen mit der [REST-API](/rest/api/searchservice/Search-Documents) oder dem [.NET-SDK](/dotnet/api/azure.search.documents.searchclient.search)[Suchabfragen](search-query-overview.md) an Ihren Dienstendpunkt richten.

Führen Sie die Schritte zum [Erstellen Ihrer ersten Such-App](tutorial-csharp-create-first-app.md) aus, um eine Webseite zu erstellen und anschließend zu erweitern, die Benutzereingaben sammelt und Ergebnisse verarbeitet. Sie können auch [Postman für interaktive REST-Aufrufe](search-get-started-postman.md) oder den integrierten [Suchexplorer](search-explorer.md) im Azure-Portal verwenden, um einen vorhandenen Index abzufragen.

## <a name="how-it-compares"></a>Im Vergleich

Kunden interessieren sich häufig für einen Vergleich von Azure Cognitive Search mit anderen Suchlösungen. Die wesentlichen Unterschiede sind in der folgenden Tabelle zusammengefasst.

| Im Vergleich zu | Wesentliche Unterschiede |
|-------------|-----------------|
| Microsoft Search | [Microsoft Search](https://docs.microsoft.com/microsoftsearch/overview-microsoft-search) ist für authentifizierte Microsoft 365-Benutzer bestimmt, die Abfragen für den Inhalt in SharePoint ausführen müssen. Sie wird als sofort einsatzbereite Suchoberfläche angeboten, die von Administratoren aktiviert und konfiguriert wird. Dabei besteht die Möglichkeit, über Connectors externe Inhalte von Microsoft und anderen Quellen zu akzeptieren. Wenn diese Beschreibung auf Ihr Szenario zutrifft, ist Microsoft Search mit Microsoft 365 eine attraktive Option.<br/><br/>Azure Cognitive Search hingegen führt Abfragen für einen von Ihnen definierten Index aus, der mit Ihren Daten und Dokumenten (häufig aus verschiedenen Quellen) gefüllt ist. Azure Cognitive Search verfügt über Crawlerfunktionen für einige Azure-Datenquellen über [Indexer](search-indexer-overview.md), aber Sie können jedes JSON-Dokument, das Ihrem Indexschema entspricht, mittels Pushübertragung in eine einzelne, konsolidierte durchsuchbare Ressource übertragen. Sie können die Indizierungspipeline auch so anpassen, dass sie Analysen für maschinelles Lernen und lexikalische Analysen umfasst. Da Cognitive Search als Plug-In-Komponente für umfangreichere Lösungen entwickelt wurde, können Sie die Suche in nahezu jede App auf jeder beliebigen Plattform integrieren.|
|Bing | Die [Bing-Websuche-API](../cognitive-services/bing-web-search/index.yml) durchsucht die Indizes auf „Bing.com“ nach Übereinstimmungen mit Begriffen, die Sie übermitteln. Die Indizes werden auf der Basis von HTML- und XML-Seiten sowie anderer Webinhalte aus öffentlich zugänglichen Websites erstellt. Die [benutzerdefinierte Bing-Suche](/azure/cognitive-services/bing-custom-search/) basiert auf der gleichen Grundlage und bietet die gleiche Crawlertechnologie für Webinhaltstypen beschränkt auf einzelne Websites.<br/><br/>In Cognitive Search können Sie den Index definieren und auffüllen. Sie können [Indexer](search-indexer-overview.md) verwenden, um Daten in Azure-Datenquellen aufzufüllen oder beliebige indexkonforme JSON-Dokumente per Push an Ihren Suchdienst zu übertragen. |
|Datenbanksuche | Zahlreiche Systemplattformen umfassen eine integrierte Suchfunktion. SQL Server verfügt über eine [Volltextsuche](/sql/relational-databases/search/full-text-search). Cosmos DB und ähnliche Technologien verfügen über abfragbare Indizes. Bei der Beurteilung von Produkten, die Suche und Speicher kombinieren, kann die Entscheidung eine Herausforderung darstellen. Viele Lösungen verwenden beides: DBMS als Speicher und Azure Cognitive Search für spezialisierte Suchfunktionen.<br/><br/>Im Vergleich zur DBMS-Suche speichert Azure Cognitive Search Inhalt aus heterogenen Quellen und bietet spezialisierte Textverarbeitungsfunktionen wie z. B. linguistikbasierte Textverarbeitung (Wortstammerkennung, Lemmatisierung, Wortformen) in [56 Sprachen](/rest/api/searchservice/language-support). Der Dienst unterstützt auch die Autokorrektur falsch geschriebener Wörter, [Synonyme](/rest/api/searchservice/synonym-map-operations), [Vorschläge](/rest/api/searchservice/suggestions), [Bewertungssteuerelemente](/rest/api/searchservice/add-scoring-profiles-to-a-search-index), [Facets](./search-filters-facets.md) und [benutzerdefinierte Tokenisierung](/rest/api/searchservice/custom-analyzers-in-azure-search). Das [Volltext-Suchmodul](search-lucene-query-architecture.md) in Azure Cognitive Search basiert auf Apache Lucene, einem Branchenstandard für den Informationsabruf. Azure Cognitive Search speichert Daten zwar in Form eines invertierten Index, doch ist dies kein Ersatz für eine echte Datenspeicherung und von der Verwendung für diesen Zweck wird abgeraten. Weitere Informationen finden Sie in diesem [Forumsbeitrag](https://stackoverflow.com/questions/40101159/can-azure-search-be-used-as-a-primary-database-for-some-data). <br/><br/>Die Ressourcenverwendung ist ein weiterer Wendepunkt in dieser Kategorie. Die Indizierung und einige Abfragevorgänge sind häufig rechenintensiv. Die Abladung der Suche vom DBMS auf eine dedizierte Lösung in der Cloud hält Systemressourcen für die Verarbeitung von Transaktionen frei. Wenn Sie die Suche nach außen verlagern, können Sie außerdem problemlos Ihrem Abfragevolumen gemäß skalieren.|
|Dedizierte Suchlösung | Wenn Sie sich für eine dedizierte Suche mit voller Funktionalität entschieden haben, erfolgt der endgültige Vergleich letztendlich zwischen einer lokalen Lösung und einem Cloud-Dienst. Viele Suchtechnologien bieten Kontrolle über Indizierungs- und Abfragepipelines, Zugriff auf umfangreichere Abfrage- und Filtersyntax, Kontrolle über Rangfolge und Relevanz sowie Funktionen für selbstgesteuerte und intelligente Suche. <br/><br/>Ein Clouddienst ist die richtige Wahl, wenn Sie eine sofort einsetzbare Lösung mit minimalem Aufwand, minimaler Wartung und anpassbarer Skalierung wünschen. <br/><br/>Innerhalb des Cloudparadigmas bieten einige Anbieter vergleichbare Basisfunktionen mit Volltextsuche, geografischer Suche und der Möglichkeit zur Verarbeitung einer gewissen Ungenauigkeit bei der Sucheingabe. Das passende Suchmodul ergibt sich in der Regel durch ein [spezialisiertes Feature](search-features-list.md) oder durch die Benutzerfreundlichkeit und Einfachheit von APIs, Tools und Verwaltungsfunktionen. |

Unter den Cloudanbietern eignet Azure Cognitive Search sich am besten für die Volltextsuche in Inhaltsspeichern und Datenbanken in Azure, für Apps, bei denen Informationsabruf und Inhaltsnavigation primär auf der Suche basieren. 

Wichtige Vorteile umfassen:

+ Azure-Datenintegration (Crawler) auf Indexebene
+ Azure-Portal für eine zentrale Verwaltung
+ Skalierung, Zuverlässigkeit und erstklassige Verfügbarkeit von Azure
+ Dank der Verarbeitung von Rohdaten durch KI-Funktionen lassen sich die Daten besser durchsuchen – es kann beispielsweise nach Text in Bildern oder nach Mustern in unstrukturierten Inhalten gesucht werden.
+ Linguistische und benutzerdefinierte Analyse, mit Analysemodulen für eine robuste Volltextsuche in 56 Sprachen
+ [Kernfeatures für auf die Suche ausgerichtete Apps](search-features-list.md): Bewertung, Facettennavigation, Vorschläge, Synonyme, geografische Suche und mehr.

Unter unseren Kunden zählen diejenigen mit Onlinekatalogen, Branchenprogrammen und Anwendungen zur Dokumentermittlung zu denen, die am meisten von den umfassenden Features in Azure Cognitive Search profitieren.

## <a name="watch-this-video"></a>Video ansehen

In diesem 15-minütigen Video stellt Programm-Manager Luis Cabrera Azure Cognitive Search vor.

>[!VIDEO https://www.youtube.com/embed/kOJU0YZodVk?version=3]