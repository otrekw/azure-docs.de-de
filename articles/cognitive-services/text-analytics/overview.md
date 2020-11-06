---
title: 'Azure Cognitive Services: Textmining und -analyse mit der Textanalyse-API'
titleSuffix: Azure Cognitive Services
description: Hier erfahren Sie mehr über Textmining mit der Textanalyse-API. Verwenden Sie es für Stimmungsanalysen, Spracherkennung und andere Arten der Verarbeitung natürlicher Sprache.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: overview
ms.date: 11/02/2020
ms.author: aahi
keywords: Textmining, Stimmungsanalyse, Textanalyse
ms.custom: cog-serv-seo-aug-2020
ms.openlocfilehash: d58c501af3d90fec1eea43d13fa2383c8e847f18
ms.sourcegitcommit: 7863fcea618b0342b7c91ae345aa099114205b03
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/03/2020
ms.locfileid: "93289695"
---
# <a name="what-is-the-text-analytics-api"></a>Worum handelt es sich bei der Textanalyse-API?

Die Textanalyse-API ist ein cloudbasierter Dienst für Features zur Verarbeitung natürlicher Sprache, z. B. Textmining und -analyse, einschließlich Stimmungsanalysen, Opinion Mining, Schlüsselbegriffserkennung, Spracherkennung und Erkennung benannter Entitäten.

Die API ist ein Teil von [Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/), einer Sammlung von Algorithmen für maschinelles Lernen und künstliche Intelligenz (KI) in der Cloud, die Sie für Ihre Entwicklungsprojekte verwenden können. Sie können diese Features mit der [REST-API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1/) oder der [Clientbibliothek ](quickstarts/text-analytics-sdk.md) verwenden.

> [!VIDEO https://channel9.msdn.com/Shows/AI-Show/Understanding-Text-using-Cognitive-Services/player]

## <a name="sentiment-analysis"></a>Stimmungsanalyse

Ermitteln Sie anhand der [Stimmungsanalyse](how-tos/text-analytics-how-to-sentiment-analysis.md), was Menschen von Ihrer Marke oder Ihrem Thema halten, indem Sie unformatierten Text auf Hinweise einer positiven oder negativen Stimmung analysieren. 

Bei diesem Feature werden Stimmungsbezeichnungen (z. B. „negativ“, „neutral“ und „positiv“) basierend auf der höchsten Zuverlässigkeitsbewertung angegeben, die vom Dienst auf Satz- und Dokumentebene gefunden wird. Darüber hinaus werden bei diesem Feature Zuverlässigkeitsbewertungen zwischen 0 und 1 für jedes Dokument und jeden darin enthaltenen Satz zurückgegeben (positive, neutrale und negative Stimmung). Sie können den Dienst auch lokal ausführen, indem Sie [einen Container verwenden](how-tos/text-analytics-how-to-install-containers.md).

Opinion Mining ist ab Version 3.1 Preview ein Feature der Stimmungsanalyse. Dieses Feature wird in der Verarbeitung natürlicher Sprache (Natural Language Processing, NLP) auch als aspektbasierte Standpunktanalyse bezeichnet und bietet feiner abgestufte Informationen zu den Meinungen in Bezug auf Aspekte (z. B. Attribute von Produkten oder Dienstleistungen) in Texten.

## <a name="key-phrase-extraction"></a>Schlüsselwortextraktion

Verwenden Sie die [Schlüsselbegriffserkennung](how-tos/text-analytics-how-to-keyword-extraction.md), um schnell die Hauptkonzepte eines Texts zu identifizieren. Im Text „Das Essen war köstlich, und es gab hervorragendes Personal“ gibt die Schlüsselbegriffserkennung beispielsweise die Kernpunkte „Essen“ und „hervorragendes Personal“ zurück.

## <a name="language-detection"></a>Spracherkennung

Die Spracherkennung kann eine Vielzahl von Sprachen, Varianten und Dialekten sowie einige Regional- und Kultursprachen [die Sprache von Eingabetexten erkennen](how-tos/text-analytics-how-to-language-detection.md) und einen einzigen Sprachcode für jedes Dokument melden, das auf Anforderung gesendet wird. Der Sprachcode wird mit einer Zuverlässigkeitsbewertung bereitgestellt.

## <a name="named-entity-recognition"></a>Erkennung benannter Entitäten

Die Erkennung benannter Entitäten (NER) kann in Ihrem Text [Entitäten identifizieren und kategorisieren](how-tos/text-analytics-how-to-entity-linking.md), die als Personen, Orte, Organisationen und Mengen erkannt werden. Bekannte Entitäten werden ebenfalls erkannt, und es werden Links zu weiteren Informationen im Internet bereitgestellt.

## <a name="use-containers"></a>Verwenden von Containern

[Verwenden Sie die Textanalysecontainer](how-tos/text-analytics-how-to-install-containers.md) als lokale Lösung für das Textmining und die Verwendung der API. Mithilfe dieser Docker-Container können Sie in Ihren Daten Schlüsselbegriffe extrahieren, Sprachen erkennen und die Stimmung erkennen.

## <a name="typical-workflow"></a>Typischer Workflow

Der Workflow ist einfach: Sie übermitteln Daten für die Analyse und verarbeiten Ausgaben in Ihrem Code. Analysetool werden unverändert (d.h. ohne Konfigurationen oder Anpassungen) verwendet.

1. [Erstellen Sie eine Azure-Ressource](../cognitive-services-apis-create-account.md) für die Textanalyse. [Rufen Sie anschließend den Schlüssel ab](../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource), der für Sie zum Authentifizieren Ihrer Anforderungen generiert wurde.

2. [Formulieren Sie eine Anforderung](how-tos/text-analytics-how-to-call-api.md#json-schema), die Ihre Daten als unformatierten, unstrukturierten Text im JSON-Format enthält.

3. Übermitteln Sie die Anforderung an den Endpunkt, der im Rahmen der Registrierung eingerichtet wurde, und fügen Sie die gewünschte Ressource an: Standpunktanalyse, Schlüsselbegriffserkennung, Sprachenerkennung oder Erkennung benannter Entitäten.

4. Streamen oder speichern Sie die Antwort lokal. Je nach Anforderung ist das Ergebnis entweder eine Standpunktbewertung, eine Sammlung von extrahierten Schlüsselbegriffen oder ein Sprachcode.

Die Ausgabe wird als einzelnes JSON-Dokument mit Ergebnissen für jedes bereitgestellte Textdokument zurückgegeben und basiert auf der ID. Sie können die Ergebnisse anschließend analysieren, visualisieren oder in verwertbare Erkenntnisse kategorisieren.

Daten werden nicht in Ihrem Konto gespeichert. Von der Textanalyse-API durchgeführte Vorgänge sind zustandslos. Dies bedeutet, dass der bereitgestellte Text verarbeitet wird und die Ergebnisse sofort zurückgegeben werden.

## <a name="text-analytics-for-multiple-programming-experience-levels"></a>Textanalyse für mehrere Stufen der Programmierungserfahrung

Sie können mit der Verwendung der Textanalyse-API in Ihren Prozessen beginnen, auch wenn Sie über wenig Erfahrung bei der Programmierung verfügen. Anhand dieser Tutorials erfahren Sie, wie Sie die API verwenden können, um Text gemäß Ihrem Erfahrungsstand auf verschiedene Weise zu analysieren. 

* Minimaler Programmieraufwand:
    * [Extrahieren von Informationen aus Excel mithilfe von Textanalyse und Power Automate](tutorials/extract-excel-information.md)
    * [Verwenden der Textanalyse-API und von MS Flow zum Ermitteln der Stimmung in Kommentaren in einer Yammer-Gruppe](https://docs.microsoft.com/Yammer/integrate-yammer-with-other-apps/sentiment-analysis-flow-azure?toc=%2F%2Fazure%2Fcognitive-services%2Ftext-analytics%2Ftoc.json&bc=%2F%2Fazure%2Fbread%2Ftoc.json)
    * [Integrieren von Power BI mit der Textanalyse-API zur Analyse von Kundenfeedback](tutorials/tutorial-power-bi-key-phrases.md)
* Programmiererfahrung empfohlen:
    * [Standpunktanalyse für Streamingdaten mit Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/databricks-sentiment-analysis-cognitive-services?toc=%2F%2Fazure%2Fcognitive-services%2Ftext-analytics%2Ftoc.json&bc=%2F%2Fazure%2Fbread%2Ftoc.json)
    * [Erstellen einer Flask-App zum Übersetzen von Text, Analysieren der Stimmung und Synthetisieren von Sprache](https://docs.microsoft.com/azure/cognitive-services/translator/tutorial-build-flask-app-translation-synthesis?toc=%2F%2Fazure%2Fcognitive-services%2Ftext-analytics%2Ftoc.json&bc=%2F%2Fazure%2Fbread%2Ftoc.json)


<a name="supported-languages"></a>

## <a name="supported-languages"></a>Unterstützte Sprachen

Für eine bessere Auffindbarkeit wurde dieser Abschnitt in einen separaten Artikel verschoben. Diesen Inhalt finden Sie unter [Supported languages in the Text Analytics API](text-analytics-supported-languages.md) (Unterstützte Sprachen in der Textanalyse-API).

<a name="data-limits"></a>

## <a name="data-limits"></a>Datengrenzwerte

Alle Endpunkte der Textanalyse-API akzeptieren unformatierte Textdaten. Weitere Informationen finden Sie im Artikel zu den [Datengrenzwerten](concepts/data-limits.md).

## <a name="unicode-encoding"></a>Unicode-Codierung

Die Textanalyse-API verwendet Unicode-Codierung für die Textdarstellung und die Berechnung der Zeichenanzahl. Anforderungen können in UTF-8 und UTF-16 übermittelt werden, dabei gibt es keine messbaren Unterschiede bei der Zeichenanzahl. Unicode-Codepunkte werden als Heuristik für die Zeichenlänge verwendet und werden für die Grenzwerte der Textanalysedaten als äquivalent betrachtet. Wenn Sie [`StringInfo.LengthInTextElements`](https://docs.microsoft.com/dotnet/api/system.globalization.stringinfo.lengthintextelements) verwenden, um die Zeichenanzahl abzurufen, verwenden Sie die gleiche Methode wie wir, um die Datengröße zu ermitteln.

## <a name="next-steps"></a>Nächste Schritte

+ [Erstellen Sie eine Azure-Ressource](../cognitive-services-apis-create-account.md) für die Textanalyse, um einen Schlüssel und einen Endpunkt für Ihre Anwendungen zu erhalten.

+ Verwenden Sie die [Schnellstartanleitung](quickstarts/text-analytics-sdk.md), um mit dem Senden von API-Aufrufen zu beginnen. Erfahren Sie, wie Sie mit minimalem Code Text übermitteln, eine Analyse auswählen und Ergebnisse anzeigen.

+ Informationen zu neuen Releases und Features finden Sie unter [What's new in the Text Analytics API?](whats-new.md) (Neuigkeiten bei der Textanalyse-API).

+ In [diesem Tutorial zur Standpunktanalyse](https://docs.microsoft.com/azure/azure-databricks/databricks-sentiment-analysis-cognitive-services) mit Azure Databricks erhalten Sie ausführlichere Informationen.

+ Eine Liste von Blogbeiträgen und Videos, in denen die Verwendung der Textanalyse-API mit anderen Tools und Technologien veranschaulicht wird, finden Sie auf der Seite [Externe Inhalte und Community-Inhalte für die Textanalyse in Cognitive Service](text-analytics-resource-external-community.md).
