---
title: Was ist die Formularerkennung?
titleSuffix: Azure Cognitive Services
description: Der Azure-Formularerkennungsdienst ermöglicht das Erkennen und Extrahieren von Schlüssel-Wert-Paaren und Tabellendaten aus Ihren Formulardokumenten sowie das Extrahieren wichtiger Informationen aus Verkaufsbelegen und Visitenkarten.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: overview
ms.date: 09/21/2020
ms.author: pafarley
ms.custom: cog-serv-seo-aug-2020
keywords: automatisierte Datenverarbeitung, Dokumentverarbeitung, automatisierte Dateneingabe, Formularverarbeitung
ms.openlocfilehash: 5243c170e1f6b5f647057b8cfafbcac9b2fb4db3
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91318958"
---
# <a name="what-is-form-recognizer"></a>Was ist die Formularerkennung?

[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

Die Azure-Formularerkennung ist ein kognitiver Dienst, mit dem Sie unter Verwendung von Technologie für maschinelles Lernen Software für die automatisierte Datenverarbeitung entwickeln können. Identifizieren und extrahieren Sie Schlüssel-Wert-Paare und Tabellendaten aus Formulardokumenten: Der Dienst gibt strukturierte Daten aus, die auch die Beziehungen in der ursprünglichen Datei umfassen. Sie können schnell präzise und auf Ihre spezifischen Inhalte zugeschnittene Ergebnisse erzielen, ohne dass komplizierte manuelle Eingriffe oder umfangreiche Data Science-Kenntnisse erforderlich sind. Verwenden Sie die Formularerkennung zur Automatisierung von Dateneingaben in Ihren Anwendungen.

Die Formularerkennung besteht aus benutzerdefinierten Dokumentverarbeitungsmodellen, den vordefinierten Beleg- und Visitenkartenmodellen und der Layout-API. Sie können Formularerkennungsmodelle mithilfe einer REST-API oder über Clientbibliothek-SDKs aufrufen, um die Komplexität zu reduzieren und sie in Ihren Workflow oder Ihre Anwendung zu integrieren.

Die Formularerkennung besteht aus den folgenden Diensten:
* **[Benutzerdefinierte Modelle:](#custom-models)** Dienen zum Extrahieren von Schlüssel-Wert-Paaren und Tabellendaten aus Formularen. Diese Modelle werden mit Ihren eigenen Daten trainiert, sodass Sie auf Ihre Formulare zugeschnitten sind.
* **[Vordefinierte Modelle:](#prebuilt-models)** Dienen zum Extrahieren eindeutiger Formulartypen mithilfe vordefinierter Modelle. Derzeit sind vordefinierte Modelle für Verkaufsbelege und Visitenkarten in englischer Sprache verfügbar.
* **[Layout-API:](#layout-api)** Dient zum Extrahieren von Text- und Tabellenstrukturen sowie der zugehörigen Begrenzungsrahmenkoordinaten aus Dokumenten.

## <a name="custom-models"></a>Benutzerdefinierte Modelle

Benutzerdefinierte Formularerkennungsmodelle werden mit Ihren eigenen Daten trainiert, und Sie benötigen zu Beginn nur fünf Beispieleingabeformulare. Ein trainiertes Dokumentverarbeitungsmodell kann strukturierte Daten ausgeben, die die Beziehungen in der Originaldatei enthalten. Nachdem das Modell trainiert wurde, können Sie es testen, neu trainieren und schließlich verwenden, um Daten aus weiteren Formularen zuverlässig nach Ihren Bedürfnissen zu extrahieren.

Beim Trainieren von benutzerdefinierten Modellen haben Sie die folgenden Optionen: Training mit beschrifteten Daten und ohne beschriftete Daten.

### <a name="train-without-labels"></a>Trainieren ohne Beschriftungen

Standardmäßig verwendet die Formularerkennung nicht überwachtes Lernen, um das Layout und die Beziehungen zwischen Feldern und Einträgen in Ihren Formularen nachzuvollziehen. Anhand der übermittelten Eingabeformulare gruppiert der Algorithmus die Formulare nach Typ, erkennt, welche Schlüssel und Tabellen vorhanden sind, und ordnet Schlüsseln Werte und Tabellen Einträge zu. Hierfür ist keine manuelle Datenbeschriftung oder intensive Codierung und Wartung erforderlich, und Sie sollten diese Methode zuerst testen.

Tipps zum Sammeln von Trainingsdokumenten finden Sie unter [Erstellen eines Trainingsdatasets für ein benutzerdefiniertes Modell](./build-training-data-set.md).

### <a name="train-with-labels"></a>Trainieren mit Beschriftungen

Wenn Sie mit beschrifteten Daten trainieren, extrahiert das Modell relevante Werte mit überwachtem Lernen, wobei die von Ihnen bereitgestellten beschrifteten Formulare verwendet werden. Dies führt zu Modellen mit besserer Leistung und kann Modelle hervorbringen, die mit komplexen Formularen oder Formularen arbeiten, die Werte ohne Schlüssel enthalten.

Die Formularerkennung verwendet die [Layout-API](#layout-api), um die erwarteten Größen und Positionen von gedruckten und handschriftlichen Textelementen zu erlernen. Anschließend werden benutzerdefinierte Beschriftungen verwendet, um die Schlüssel-Wert-Zuordnungen in den Dokumenten zu erlernen. Sie sollten fünf manuell beschriftete Formulare desselben Typs verwenden, um mit dem Trainieren eines neuen Modells zu beginnen, und weitere beschriftete Daten nach Bedarf hinzufügen, um die Modellgenauigkeit zu verbessern.

## <a name="prebuilt-models"></a>Vordefinierte Modelle

Die Formularerkennung enthält außerdem vordefinierte Modelle zur automatisierten Datenverarbeitung eindeutiger Formulartypen.

### <a name="prebuilt-receipt-model"></a>Vordefiniertes Belegmodell

Das vordefinierte Belegmodell wird zum Lesen englischsprachiger Verkaufsbelege aus Australien, Kanada, Großbritannien, Indien und den USA verwendet – des Typs, der von Restaurants, Tankstellen, Einzelhandel usw. verwendet wird. Dieses Modell extrahiert wichtige Informationen wie Zeitpunkt und Datum der Transaktion, Händlerinformationen, Steuer- und Summenbeträge, Einzelposten und mehr. Darüber hinaus wird das vorgefertigte Belegmodell dazu trainiert, den gesamten Text eines Belegs zu erkennen und zurückzugeben. Weitere Informationen finden Sie im Leitfaden [Konzepte zu Belegen](./concept-receipts.md).

![Beispielbeleg](./media/contoso-receipt-small.png)

### <a name="prebuilt-business-cards-model"></a>Vordefiniertes Visitenkartenmodell

Mit dem Visitenkartenmodell können Sie Informationen wie Name, Position, Adresse, E-Mail-Adresse, Unternehmen und Telefonnummern einer Person aus englischsprachigen Visitenkarten extrahieren. Weitere Informationen finden Sie im Leitfaden [Konzepte zu Visitenkarten](./concept-business-cards.md).

![Beispielvisitenkarte](./media/business-card-english.jpg)

## <a name="layout-api"></a>Layout-API

Die Formularerkennung kann auch Text- und Tabellenstruktur (die Zeilen- und Spaltennummern, die dem Text zugeordnet sind) mithilfe hochauflösender optischer Zeichenerkennung (Optical Character Recognition, OCR) extrahieren.

## <a name="get-started"></a>Erste Schritte

Befolgen Sie einen Schnellstart zum Extrahieren von Daten aus Ihren Formularen. Sie sollten den kostenlosen Dienst nutzen, wenn Sie die Technologie erlernen. Bedenken Sie, dass die Anzahl der kostenlosen Seiten auf 500 pro Monat beschränkt ist.

* [Schnellstartanleitungen für die Clientbibliothek](./quickstarts/client-library.md) (alle Sprachen, mehrere Szenarien)
* Schnellstartanleitungen für Webbenutzeroberfläche
  * [Trainieren mit Beschriftungen: Tool für die Beschriftung von Beispielen](quickstarts/label-tool.md)
* REST-Schnellstarts
  * Trainieren von benutzerdefinierten Modellen und Extrahieren von Formulardaten
    * [Trainieren ohne Beschriftungen: cURL](quickstarts/curl-train-extract.md)
    * [Trainieren ohne Beschriftungen: Python](quickstarts/python-train-extract.md)
    * [Trainieren mit Beschriftungen: Python](quickstarts/python-labeled-data.md)
  * Extrahieren von Daten aus Verkaufsbelegen
    * [Extrahieren von Verkaufsbelegdaten: cURL](quickstarts/curl-receipts.md)
    * [Extrahieren von Verkaufsbelegdaten: Python](quickstarts/python-receipts.md)
  * Extrahieren von Daten aus Visitenkarten
    * [Extrahieren von Visitenkartendaten – Python](quickstarts/python-business-cards.md)
  * Extrahieren von Text- und Tabellenstruktur aus Formularen
    * [Extrahieren von Layoutdaten: Python](quickstarts/python-layout.md)


### <a name="review-the-rest-apis"></a>Überprüfen der REST-APIs

Verwenden Sie die folgenden APIs zum Trainieren von Modellen und Extrahieren strukturierter Daten aus Formularen.

|Name |BESCHREIBUNG |
|---|---|
| **Trainieren eines benutzerdefinierten Modells**| Trainieren eines neuen Modells zur Analyse Ihrer Formulare mit fünf Formularen gleichen Typs. Legen Sie den Parameter _useLabelFile_ auf `true` fest, um mit manuell beschrifteten Daten zu trainieren. |
| **Analysieren des Formulars** |Analysieren eines einzelnen Dokuments, das als Stream übergeben wird, um Schlüssel-Wert-Paare und Tabellen aus dem Formular mit Ihrem benutzerdefinierten Modell zu extrahieren.  |
| **Analysieren des Belegs** |Analysieren eines einzelnen Belegdokuments, um wichtige Informationen und anderen Belegtext zu extrahieren.|
| **Analysieren von Visitenkarten** |Analysieren Sie eine Visitenkarte, um wichtige Informationen und Text zu extrahieren.|
| **Analysieren des Layouts** |Analysieren des Layouts eines Formulars, um Text- und Tabellenstruktur zu extrahieren.|

Sehen Sie sich die [Referenzdokumentation zur REST-API](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/AnalyzeWithCustomForm) an, um mehr zu erfahren. Wenn Sie mit einer früheren Version der API vertraut sind, finden Sie im Artikel [Neuerungen](./whats-new.md) weitere Informationen zu den aktuellen Änderungen.

## <a name="input-requirements"></a>Eingabeanforderungen

[!INCLUDE [input requirements](./includes/input-requirements.md)]

## <a name="data-privacy-and-security"></a>Datenschutz und Sicherheit

Wie bei allen Cognitive Services-Diensten müssen Entwickler, die den Formularerkennungsdienst nutzen, die Microsoft-Richtlinien zu Kundendaten beachten. Weitere Informationen finden Sie im Microsoft Trust Center auf der [Seite zu Cognitive Services](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices).

## <a name="next-steps"></a>Nächste Schritte

Durchlaufen Sie eine [Schnellstartanleitung für die Clientbibliothek](quickstarts/client-library.md), um damit zu beginnen, eine Formularverarbeitungs-App mit Formularerkennung in der Sprache Ihrer Wahl zu schreiben.