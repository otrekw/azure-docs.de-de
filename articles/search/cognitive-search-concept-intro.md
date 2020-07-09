---
title: Einführung in die KI-Anreicherung
titleSuffix: Azure Cognitive Search
description: Inhaltsextrahierung, Verarbeitung natürlicher Sprache (Natural Language Processing, NLP) und Bildverarbeitung werden zum Erstellen durchsuchbarer Inhalte in Azure Cognitive Search-Indizes mit sowohl vordefinierten kognitiven Fähigkeiten als auch benutzerdefinierten KI-Algorithmen verwendet.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 03/24/2020
ms.openlocfilehash: cdff42c6ff0cadb5ce4b3d7fc469d648349d1e88
ms.sourcegitcommit: 309cf6876d906425a0d6f72deceb9ecd231d387c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2020
ms.locfileid: "84265198"
---
# <a name="getting-started-with-ai-enrichment"></a>Erste Schritte mit der KI-Anreicherung

KI-Anreicherung ist eine Funktion der Azure Cognitive Search-Indizierung, die zum Extrahieren von Text aus Bildern, Blobs und anderen unstrukturierten Datenquellen verwendet wird. Durch Anreicherung und Extraktion können Ihre Inhalte in einem [Index](search-what-is-an-index.md) oder [Wissensspeicher](knowledge-store-concept-intro.md) einfacher durchsucht werden. Extrahierung und Anreicherung werden über *kognitive Fähigkeiten* implementiert, die an die Indizierungspipeline angefügt sind. In den Dienst integrierte kognitive Qualifikationen werden wie folgt kategorisiert: 

+ Zu den Skills in Bezug auf die **Verarbeitung natürlicher Sprache** gehören [Entitätserkennung](cognitive-search-skill-entity-recognition.md), [Sprachenerkennung](cognitive-search-skill-language-detection.md), [Schlüsselbegriffserkennung](cognitive-search-skill-keyphrases.md), Textbearbeitung, [Stimmungserkennung](cognitive-search-skill-sentiment.md) und [PII-Erkennung](cognitive-search-skill-pii-detection.md). Mit diesen Fähigkeiten wird unstrukturierter Text in Form von durchsuchbaren und filterbaren Feldern in einem Index zugeordnet.

+ Die **Bildverarbeitungsfähigkeiten** umfassen [Optical Character Recognition (OCR)](cognitive-search-skill-ocr.md) und die Identifizierung von [visuellen Features](cognitive-search-skill-image-analysis.md), z.B. Gesichtserkennung, Bildinterpretation, Bilderkennung (berühmte Personen und Wahrzeichen) oder Attribute wie Bildausrichtung. Diese Fähigkeiten erstellen Textdarstellungen von Bildinhalt, sodass er mit den Abfragefunktionen von Azure Cognitive Search durchsucht werden kann.

![Diagramm der KI-Anreicherungspipeline](./media/cognitive-search-intro/cogsearch-architecture.png "Übersicht über die KI-Anreicherungspipeline")

Die kognitiven Fähigkeiten in Azure Cognitive Search basieren auf vortrainierten Machine Learning-Modellen in Cognitive Services-APIs: [Maschinelles Sehen](https://docs.microsoft.com/azure/cognitive-services/computer-vision/) und [Textanalyse](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview). 

Die Verarbeitung von natürlicher Sprache und Bildern wird während der Phase der Datenerfassung angewendet, wobei die Ergebnisse zu einem Teil einer Dokumentkomposition in einem durchsuchbaren Index in Azure Cognitive Search werden. Daten stammen aus einem Azure-Dataset und werden dann über eine Indizierungspipeline übertragen, indem jeweils die erforderlichen [integrierten Fähigkeiten](cognitive-search-predefined-skills.md) verwendet werden. Die Architektur ist erweiterbar. Falls die integrierten Fähigkeiten also nicht ausreichen, können Sie [benutzerdefinierte Fähigkeiten](cognitive-search-create-custom-skill-example.md) erstellen und anfügen, um die benutzerdefinierte Verarbeitung zu integrieren. Beispiele hierfür sind ein benutzerdefiniertes Entitätsmodul oder ein benutzerdefinierter Dokumentenklassifizierer, das bzw. der auf eine bestimmte Domäne ausgerichtet ist, z.B. Finanzen, wissenschaftliche Veröffentlichungen oder Medizin.

## <a name="when-to-use-ai-enrichment"></a>Einsatzgebiete der KI-Anreicherung

Integrierte kognitive Qualifikationen können verwendet werden, wenn es sich bei Ihren Rohinhalten um unstrukturierten Text, um Bildinhalte oder um Inhalte handelt, für die Spracherkennung und -übersetzung benötigt werden. Die Anwendung von KI in Form der integrierten kognitiven Qualifikationen kann den Nutzen dieser Inhalte in Ihren Such- und Data Science-Apps erhöhen. 

Darüber hinaus können Sie auch eine benutzerdefinierte Qualifikation hinzufügen, wenn Sie über Open-Source-, Drittanbieter- oder Erstanbietercode verfügen, den Sie in die Pipeline integrieren möchten. Zu dieser Kategorie gehören Klassifizierungsmodelle, mit denen wichtige Merkmale verschiedener Dokumenttypen identifiziert werden. Es könnte aber jedes beliebige Paket verwendet werden, das den Nutzen Ihrer Inhalte erhöht.

### <a name="more-about-built-in-skills"></a>Weitere Informationen zu integrierten Qualifikationen

Ein [Skillset](cognitive-search-defining-skillset.md) mit integrierten Fähigkeiten eignet sich sehr gut für die folgenden Anwendungsszenarien:

+ Gescannte Dokumente (JPEG), die für die Volltextsuche verfügbar gemacht werden sollen. Sie können eine Fähigkeit zur optischen Zeichenerkennung (Optical Character Recognition, OCR) anfügen, um Text aus JPEG-Dateien zu identifizieren, zu extrahieren und zu erfassen.

+ PDF-Dateien mit Kombinationen aus Bild und Text. Texte in PDF-Dateien können während der Indizierung extrahiert werden, ohne dass die Schritte zur Anreicherung ausgeführt werden. Beim Hinzufügen von Bildverarbeitung und natürlicher Sprachverarbeitung erzielen Sie jedoch häufig ein besseres Ergebnis als bei einer Standardindizierung.

+ Mehrsprachiger Inhalt, für den Sie die Spracherkennung und möglicherweise die Textübersetzung anwenden möchten.

+ Unstrukturierte oder teilweise strukturierte Dokumente mit Inhalten, die eine inhärente Bedeutung oder einen Kontext haben, der im größeren Dokument ausgeblendet ist. 

  Insbesondere Blobs enthalten häufig einen großen Textteil in einem einzelnen „Feld“. Durch das Anfügen von Fähigkeiten zur Bildverarbeitung und natürlicher Sprachverarbeitung an einen Indexer können Sie neue Informationen erstellen, die in den Rohdaten noch vorhanden sind, aber sonst nicht als unterschiedliche Felder aufgeführt werden. Einige einsatzbereite, integrierte kognitive Fähigkeiten, die hilfreich sein können: Schlüsselbegriffserkennung, Stimmungsanalyse und Entitätserkennung (Personen, Organisationen und Standorte).

  Darüber hinaus können Sie mit integrierten Fähigkeiten Inhalte durch Textaufteilung, Textzusammenführung und Shape-Vorgänge neu strukturieren.

### <a name="more-about-custom-skills"></a>Weitere Informationen zu benutzerdefinierten Qualifikationen

Benutzerdefinierte Fähigkeiten können komplexere Szenarien unterstützen, z. B. das Erkennen von Formularen oder die benutzerdefinierte Entitätserkennung mithilfe eines Modells, das Sie bereitstellen und in der [benutzerdefinierten Skills-Webschnittstelle](cognitive-search-custom-skill-interface.md) umschließen. Beispiele für benutzerdefinierte Fähigkeiten sind die [Formularerkennung](/azure/cognitive-services/form-recognizer/overview), die Integration der [Bing-Entitätssuche-API](https://docs.microsoft.com/azure/search/cognitive-search-create-custom-skill-example) und [die Erkennung von benutzerdefinierten Entitäten](https://github.com/Microsoft/SkillsExtractorCognitiveSearch).


## <a name="steps-in-an-enrichment-pipeline"></a>Schritte in einer Anreicherungspipeline

Eine Anreicherungspipeline basiert auf [*Indexern*](search-indexer-overview.md). Indexer füllen einen Index auf der Grundlage von Feld-zu-Feld-Zuordnungen zwischen dem Index und Ihrer Datenquelle für Dokumententschlüsselung auf. Die jetzt an Indexer angefügten Fähigkeiten fangen Dokumente entsprechend dem/den von Ihnen definierten Skillset(s) ab und reichern sie an. Nach der Indizierung können Sie über Suchanforderungen über sämtliche [von Azure Cognitive Search unterstützte Abfragetypen](search-query-overview.md) auf Inhalte zugreifen.  Wenn Sie mit Indexern noch nicht vertraut sind, werden Ihnen in diesem Abschnitt die erforderlichen Schritte erläutert.

### <a name="step-1-connection-and-document-cracking-phase"></a>Schritt 1: Phase der Entschlüsselung von Verbindung und Dokument

Am Anfang der Pipeline befindet sich unstrukturierter Text oder Nicht-Text-Inhalt (z. B. Bilder, gescannte Dokumente oder JPEG-Dateien). Die Daten müssen in einem Azure-Datenspeicherdienst enthalten sein, auf den ein Indexer Zugriff hat. Indexer können Quelldokumente entschlüsseln, um Text aus Quelldaten zu extrahieren. Dokumententschlüsselung ist der Prozess des Extrahierens oder Erstellens von Textinhalt aus Nicht-Text-Quellen während der Indizierung.

![Phase der Dokumententschlüsselung](./media/cognitive-search-intro/document-cracking-phase-blowup.png "Dokumententschlüsselung")

 Zu den unterstützten Quellen zählen Azure Blob Storage, Azure Table Storage, Azure SQL-Datenbank und Azure Cosmos DB. Textbasierte Inhalte können aus den folgenden Dateitypen extrahiert werden: PDF-, Word-, PowerPoint- und CSV-Dateien. Eine vollständige Liste finden Sie unter [Unterstützte Formate](search-howto-indexing-azure-blob-storage.md#supported-document-formats). Die Indizierung braucht Zeit. Daher sollten Sie mit einem kleinen, repräsentativen Dataset beginnen, das Sie anschließend schrittweise aufbauen, während sich Ihre Lösung entwickelt.

### <a name="step-2-cognitive-skills-and-enrichment-phase"></a>Schritt 2: Phase der kognitiven Fähigkeiten und Anreicherung

Die Anreicherung erfolgt durch *kognitive Fähigkeiten*, die atomische Vorgänge durchführen. Sobald Sie eine PDF-Datei entschlüsselt haben, können Sie beispielsweise die Entitätserkennung, die Sprachenerkennung oder die Schlüsselbegriffserkennung anwenden, um neue Felder in Ihrem Index zu erstellen, die in der Quelle nativ nicht verfügbar sind. Die Auflistung der in Ihrer Pipeline verwendeten Fähigkeiten wird als *Fähigkeitengruppe* bezeichnet.  

![Phase der Anreicherung](./media/cognitive-search-intro/enrichment-phase-blowup.png "Phase der Anreicherung")

Ein Skillset basiert auf [integrierten kognitiven Fähigkeiten](cognitive-search-predefined-skills.md) oder [benutzerdefinierten Fähigkeiten](cognitive-search-create-custom-skill-example.md), die von Ihnen bereitgestellt und mit dem Skillset verbunden werden. Eine Fähigkeitengruppe kann minimal oder höchst komplex sein. Sie bestimmt nicht nur den Verarbeitungstyp, sondern auch die Reihenfolge der Vorgänge. Eine Fähigkeitengruppe bietet zusammen mit den als Bestandteil eines Indexers definierten Feldzuordnungen eine umfassende Beschreibung der Anreicherungspipeline. Weitere Informationen zum Zusammensetzen all dieser Teile finden Sie unter [Definieren einer Fähigkeitengruppe](cognitive-search-defining-skillset.md).

Die Pipeline generiert intern eine Sammlung angereicherter Dokumente. Sie können entscheiden, welche Teile der angereicherten Dokumente indizierbaren Feldern in Ihrem Suchindex zugeordnet werden sollen. Wenn Sie beispielsweise die Fähigkeiten „Schlüsselbegriffserkennung“ und „Entitätserkennung“ angewendet haben, würden diese neuen Felder ein Bestandteil des angereicherten Dokuments und können Feldern in Ihrem Index zugeordnet werden. Weitere Informationen zu Eingabe-/Ausgabeformationen finden Sie unter [Anmerkungen](cognitive-search-concept-annotations-syntax.md).

#### <a name="add-a-knowledgestore-element-to-save-enrichments"></a>Hinzufügen eines knowledgeStore-Elements zum Speichern von Anreicherungen

Die [REST-API-Version 2019-05-06-Preview des Search-Diensts](search-api-preview.md) erweitert Qualifikationsgruppen um eine `knowledgeStore`-Definition, die eine Azure Storage-Verbindung und Projektionen bereitstellt, die beschreiben, wie die Anreicherungen gespeichert werden. Dies geschieht zusätzlich zu Ihrem Index. In einer KI-Standardpipeline sind angereicherte Dokumente kurzlebig: Sie werden nur während der Indizierung verwendet und anschließend verworfen. Mit dem Wissensspeicher bleiben erweiterte Dokumente erhalten. Weitere Informationen finden Sie unter [Wissensspeicher (Vorschau)](knowledge-store-concept-intro.md).

### <a name="step-3-search-index-and-query-based-access"></a>Schritt 3: Suchindex und abfragebasierter Zugriff

Nach Abschluss der Verarbeitung verfügen Sie über einen Suchindex mit angereicherten Dokumenten, die mit Azure Cognitive Search im Volltext durchsucht werden können. Entwickler und Benutzer greifen über das [Abfragen des Indexes](search-query-overview.md) auf den von der Pipeline generierten angereicherten Inhalt zu. 

![Index mit Suchsymbol](./media/cognitive-search-intro/search-phase-blowup.png "Index mit Suchsymbol")

Der Index ist vergleichbar mit beliebigen anderen Indizes, die für Azure Cognitive Search erstellt werden können: Sie können ihn durch benutzerdefinierte Analysen ergänzen, Fuzzysuchabfragen aufrufen, gefilterte Suchvorgänge hinzufügen oder zur Umgestaltung der Suchergebnisse mit Bewertungsprofilen experimentieren.

Indizes werden über ein Indexschema generiert, das die Felder, Attribute und weitere Konstrukte definiert, die an einen bestimmten Index angefügt wurden, wie z.B. Bewertungsprofile und Synonymzuordnungen. Nachdem ein Index definiert und aufgefüllt wurde, können Sie die Indizierung inkrementell durchführen, um neue und aktualisierte Quelldokumente zu übernehmen. Für bestimmte Änderungen ist eine vollständige Neuerstellung erforderlich. Sie sollten ein kleines Dataset verwenden, bis der Schemaentwurf stabil ist. Weitere Informationen finden Sie unter [Neuerstellen eines Indexes](search-howto-reindex.md).

**Prüfliste: Typischer Workflow**

1. Fügen Sie Teilmengen Ihrer Azure-Quelldaten in eine repräsentative Stichprobe ein. Die Indizierung braucht Zeit. Daher sollten Sie mit einem kleinen, repräsentativen Dataset beginnen, das Sie anschließend schrittweise aufbauen, während sich Ihre Lösung entwickelt.

1. Erstellen Sie ein [Datenquellenobjekt](https://docs.microsoft.com/rest/api/searchservice/create-data-source) in Azure Cognitive Search, um eine Verbindungszeichenfolge für den Datenabruf bereitzustellen.

1. Erstellen Sie mithilfe von Anreicherungsschritten [Fähigkeitengruppen](https://docs.microsoft.com/rest/api/searchservice/create-skillset).

1. Definieren Sie das [Indexschema](https://docs.microsoft.com/rest/api/searchservice/create-index). Die *Fields*-Auflistung enthält Felder aus Quelldaten. Sie sollten zudem zusätzliche Felder durch Stubs ersetzen, um generierte Werte für Inhalte zu speichern, die während der Anreicherung erstellt werden.

1. Definieren Sie den [Indexer](https://docs.microsoft.com/rest/api/searchservice/create-indexer), der auf die Datenquelle, die Fähigkeitengruppen und den Index verweist.

1. Fügen Sie *outputFieldMappings* innerhalb des Indexers hinzu. In diesem Abschnitt wird die Ausgabe aus der Fähigkeitengruppe (in Schritt 3) den Eingabefeldern im Indexschema (in Schritt 4) zugeordnet.

1. Senden Sie die gerade erstellte Anforderung *Indexer erstellen* (eine POST-Anforderung mit einer Indexerdefinition im Anforderungstext), um den Indexer in Azure Cognitive Search darzustellen. Dieser Schritt befasst sich mit der Ausführung des Indexers durch Aufrufen der Pipeline.

1. Führen Sie zum Auswerten von Ergebnissen und zum Ändern von Code Abfragen aus, um Fähigkeitengruppen, Schemas oder die Indexerkonfiguration zu aktualisieren.

1. [Setzen Sie den Indexer zurück](search-howto-reindex.md), bevor Sie die Pipeline neu erstellen.

## <a name="next-steps"></a>Nächste Schritte

+ [Dokumentation zur KI-Anreicherung](cognitive-search-resources-documentation.md)
+ [Beispiel: Erstellen einer benutzerdefinierten Fähigkeit für die KI-Anreicherung (C#)](cognitive-search-create-custom-skill-example.md)
+ [Schnellstart: Testen der KI-Anreicherung in einer exemplarischen Vorgehensweise im Portal](cognitive-search-quickstart-blob.md)
+ [Tutorial: Weitere Informationen zu den APIs für die KI-Anreicherung](cognitive-search-tutorial-blob.md)
+ [Wissensspeicher (Vorschau)](knowledge-store-concept-intro.md)
+ [Erstellen von Wissensspeichern in REST](knowledge-store-create-rest.md)
+ [Tipps zur Problembehandlung](cognitive-search-concept-troubleshooting.md)
