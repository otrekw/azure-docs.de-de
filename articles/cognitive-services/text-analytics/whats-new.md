---
title: Neuigkeiten bei der Textanalyse-API
titleSuffix: Text Analytics - Azure Cognitive Services
description: Dieser Artikel bietet Informationen zu neuen Releases und Features für Azure Cognitive Services-Textanalysen.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 05/19/2020
ms.author: aahi
ms.openlocfilehash: 32dc7e86d5cd737533a4a6c8b3d9ce7d00795c65
ms.sourcegitcommit: f0b206a6c6d51af096a4dc6887553d3de908abf3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84140503"
---
# <a name="whats-new-in-the-text-analytics-api"></a>Neuigkeiten bei der Textanalyse-API

Die Textanalyse-API wird fortlaufend aktualisiert. Damit Sie bezüglich der aktuellen Entwicklungen auf dem neuesten Stand bleiben, bietet dieser Artikel Informationen zu neuen Releases und Funktionen.

## <a name="may-2020"></a>Mai 2020

### <a name="text-analytics-api-v3-general-availability"></a>Textanalyse-API v3: Allgemeine Verfügbarkeit

Die Textanalyse-API v3 ist jetzt mit den folgenden Updates allgemein verfügbar:

* Modellversion `2020-04-01`
* Neue [Datengrenzwerte](concepts/data-limits.md) für jedes Feature
* Aktualisierte [Sprachunterstützung](language-support.md) für [Standpunktanalyse v3](how-tos/text-analytics-how-to-sentiment-analysis.md)
* Separater Endpunkt für Entitätsverknüpfung 
* Neue Entitätskategorie „Address“ bei der [Erkennung benannter Entitäten (NER) v3](how-tos/text-analytics-how-to-entity-linking.md).
* Neue Unterkategorien in NER v3:
   * Standort: Geografisch
   * Standort: Strukturell
   * Organisation: Börse
   * Organisation: Medizin
   * Organisation: Sport
   * Veranstaltung: Kultur
   * Veranstaltung: Natur
   * Veranstaltung: Sport

Der JSON-Antwort wurden die folgenden Eigenschaften hinzugefügt:
   * `SentenceText` in Standpunktanalyse
   * `Warnings` für jedes Dokument 

Die Namen der folgenden Eigenschaften in der JSON-Antwort wurden geändert (soweit zutreffend):

* `score` wurde in `confidenceScore` umbenannt.
    * `confidenceScore` verfügt über eine Genauigkeit von zwei Dezimalstellen. 
* `type` wurde in `category` umbenannt.
* `subtype` wurde in `subcategory` umbenannt.

[!INCLUDE [v3 region availability](includes/v3-region-availability.md)]

> [!div class="nextstepaction"]
> [Weitere Informationen zur Textanalyse-API v3](https://westus2.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0/operations/Languages)

### <a name="text-analytics-api-v31-public-preview"></a>Textanalyse-API v3.1: Public Preview
   * Neues Feature für die Standpunktanalyse: [Opinion Mining](how-tos/text-analytics-how-to-sentiment-analysis.md#opinion-mining)
   * Neuer [Filter für den persönlichen Bereich (`PII`)](how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-versions-and-features) für geschützte Informationen zur Gesundheit (`PHI`).
   * Neue Kategorien für den persönlichen Bereich (`PII`):
      * Internationale Klassifikation der Krankheiten (ICD-9-CM)
      * Internationale Klassifikation der Krankheiten (ICD-10-CM)

> [!div class="nextstepaction"]
> [Weitere Informationen zur Textanalyse-API v3.1 (Vorschau)](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-1-preview-1/operations/Languages)

## <a name="february-2020"></a>Februar 2020

### <a name="sdk-support-for-text-analytics-api-v3-public-preview"></a>SDK-Unterstützung für die öffentliche Vorschauversion der Textanalyse-API v3

Als Teil der [vereinheitlichten Azure SDK-Veröffentlichung](https://techcommunity.microsoft.com/t5/azure-sdk/january-2020-unified-azure-sdk-release/ba-p/1097290) ist das Textanalyse-API v3 SDK jetzt als öffentliche Vorschauversion für die folgenden Programmiersprachen verfügbar:
   * [C#](https://docs.microsoft.com/azure/cognitive-services/text-analytics/quickstarts/text-analytics-sdk?tabs=version-3&pivots=programming-language-csharp)
   * [Python](https://docs.microsoft.com/azure/cognitive-services/text-analytics/quickstarts/text-analytics-sdk?tabs=version-3&pivots=programming-language-python)
   * [JavaScript (Node.js)](https://docs.microsoft.com/azure/cognitive-services/text-analytics/quickstarts/text-analytics-sdk?tabs=version-3&pivots=programming-language-javascript)
   * [Java](https://docs.microsoft.com/azure/cognitive-services/text-analytics/quickstarts/text-analytics-sdk?tabs=version-3&pivots=programming-language-java)
   
   > [!div class="nextstepaction"]
> [Weitere Informationen zum Textanalyse-API v3 SDK](https://docs.microsoft.com/azure/cognitive-services/text-analytics/quickstarts/text-analytics-sdk?tabs=version-3)

### <a name="named-entity-recognition-v3-public-preview"></a>Named Entity Recognition v3 – öffentliche Vorschau

Weitere Entitätstypen stehen jetzt in der öffentlichen Vorschauversion des Diensts zur Erkennung benannter Entitäten (NER) v3 zur Verfügung, da wir die Erkennung allgemeiner und persönlicher Informationsentitäten, die in Text erkannt werden, ausweiten. Dieses Update führt die [Modellversion](concepts/model-versioning.md) `2020-02-01` ein, die Folgendes beinhaltet:

* Erkennung der folgenden allgemeinen Entitätstypen (nur in englischer Sprache):
    * PersonType
    * Produkt
    * Ereignis
    * Geopolitische Entität (GPE) als Untertyp unter Standort
    * Skill

* Erkennung der folgenden persönlichen Entitätstypen (nur in englischer Sprache):
    * Person
    * Organization
    * Alter als Untertyp unter Menge
    * Datum als Untertyp unter DateTime
    * Email 
    * Telefonnummer (nur USA)
    * URL
    * IP-Adresse

> [!div class="nextstepaction"]
> [Weitere Informationen zu Named Entity Recognition v3](how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-versions-and-features)

### <a name="october-2019"></a>Oktober 2019

#### <a name="named-entity-recognition-ner"></a>Erkennung benannter Entitäten (NER)

* Einen [neuen Endpunkt](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0-Preview-1/operations/EntitiesRecognitionPii) für das Erkennen von Entitätstypen für personenbezogene Informationen (nur für die englische Sprache)

* Separate Endpunkte für die [Entitätserkennung](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0-Preview-1/operations/EntitiesRecognitionGeneral) und [Entitätsverknüpfung](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0-Preview-1/operations/EntitiesLinking).

* [Modellversion](concepts/model-versioning.md) `2019-10-01`, die Folgendes beinhaltet:
    * Erweiterte Erkennung und Kategorisierung von Entitäten, die im Text erkannt wurden. 
    * Erkennung der folgenden neuen Entitätstypen:
        * Telefonnummer
        * IP-Adresse

Die Entitätsverknüpfung unterstützt Englisch und Spanisch. Die NER-Sprachunterstützung ist je nach Entitätstyp unterschiedlich.

#### <a name="sentiment-analysis-v3-public-preview"></a>Öffentliche Vorschau der Standpunktanalyse v3

* Einen [neuen Endpunkt](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0-Preview-1/operations/Sentiment) für die Stimmungsanalyse.
* [Modellversion](concepts/model-versioning.md) `2019-10-01`, die Folgendes beinhaltet:

    * Deutliche Verbesserungen bei Genauigkeit und Details der Textkategorisierung und -bewertung der API.
    * Automatische Bezeichnung für verschiedene Stimmungen im Text
    * Standpunktanalyse und Ausgabe auf Dokument- und Satzebene 

Unterstützt derzeit Englisch (`en`), Japanisch (`ja`), vereinfachtes Chinesisch (`zh-Hans`), traditionelles Chinesisch (`zh-Hant`), Französisch (`fr`), Italienisch (`it`), Spanisch (`es`), Niederländisch (`nl`), Portugiesisch (`pt`) und Deutsch (`de`) und ist in den folgenden Regionen erhältlich: `Australia East`, `Central Canada`, `Central US`, `East Asia`, `East US`, `East US 2`, `North Europe`, `Southeast Asia`, `South Central US`, `UK South`, `West Europe` und `West US 2`. 

> [!div class="nextstepaction"]
> [Weitere Informationen zur Standpunktanalyse v3](how-tos/text-analytics-how-to-sentiment-analysis.md#sentiment-analysis-versions-and-features)

## <a name="next-steps"></a>Nächste Schritte

* [Worum handelt es sich bei der Textanalyse-API?](overview.md)  
* [Beispielbenutzerszenarien](text-analytics-user-scenarios.md)
* [Standpunktanalyse](how-tos/text-analytics-how-to-sentiment-analysis.md)
* [Sprachenerkennung](how-tos/text-analytics-how-to-language-detection.md)
* [Entitätserkennung](how-tos/text-analytics-how-to-entity-linking.md)
* [Schlüsselbegriffserkennung](how-tos/text-analytics-how-to-keyword-extraction.md)
