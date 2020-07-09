---
title: Was ist die Formularerkennung?
titleSuffix: Azure Cognitive Services
description: Die Formularerkennung von Azure Cognitive Services ermöglicht Ihnen das Identifizieren und Extrahieren von Schlüssel/Wert-Paaren und Tabellendaten aus Formulardokumenten.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: overview
ms.date: 04/14/2020
ms.author: pafarley
ms.openlocfilehash: 8885f66b3501217ead302486d2e5ef7de63f5f47
ms.sourcegitcommit: 73ac360f37053a3321e8be23236b32d4f8fb30cf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/30/2020
ms.locfileid: "85563244"
---
# <a name="what-is-form-recognizer"></a>Was ist die Formularerkennung?

[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

Die Azure-Formularerkennung ist ein kognitiver Dienst, der mithilfe von Machine Learning-Technologie Text, Schlüssel-Wert-Paare und Tabellendaten aus Formulardokumenten identifiziert und extrahiert. Texte aus Formularen werden erfasst und strukturierte Daten ausgegeben, die die Beziehungen in der Originaldatei enthalten. Sie können schnell präzise und auf Ihre spezifischen Inhalte zugeschnittene Ergebnisse erzielen, ohne dass komplizierte manuelle Eingriffe oder umfangreiche Data Science-Kenntnisse erforderlich sind. Die Formularerkennung besteht aus benutzerdefinierten Modellen, dem vordefinierten Belegmodell und der Layout-API. Sie können Formularerkennungsmodelle mithilfe einer REST-API aufrufen, um die Komplexität zu reduzieren und sie in Ihren Workflow oder Ihre Anwendung zu integrieren.

Die Formularerkennung besteht aus den folgenden Diensten:
* **Benutzerdefinierte Modelle**: Extrahieren von Schlüssel-Wert-Paaren und Tabellendaten aus Formularen. Diese Modelle werden mit Ihren eigenen Daten trainiert, sodass Sie auf Ihre Formulare zugeschnitten sind.
* **Vordefiniertes Belegmodell**: Extrahieren von Daten aus USA-Verkaufsbelegen mithilfe eines vordefinierten Modells.
* **Layout-API**: Extrahieren von Text- und Tabellenstrukturen zusammen mit ihren Begrenzungsrahmenkoordinaten aus Dokumenten.

<!-- add diagram -->

## <a name="custom-models"></a>Benutzerdefinierte Modelle

Benutzerdefinierte Formularerkennungsmodelle werden mit Ihren eigenen Daten trainiert, und Sie benötigen zu Beginn nur fünf Beispieleingabeformulare. Ein trainiertes Modell kann strukturierte Daten ausgeben, die die Beziehungen in der Originaldatei enthalten. Nachdem das Modell trainiert wurde, können Sie es testen, neu trainieren und schließlich verwenden, um Daten aus weiteren Formularen zuverlässig nach Ihren Bedürfnissen zu extrahieren.

Beim Trainieren von benutzerdefinierten Modellen haben Sie die folgenden Optionen: Training mit beschrifteten Daten und ohne beschriftete Daten.

### <a name="train-without-labels"></a>Trainieren ohne Beschriftungen

Standardmäßig verwendet die Formularerkennung nicht überwachtes Lernen, um das Layout und die Beziehungen zwischen Feldern und Einträgen in Ihren Formularen nachzuvollziehen. Anhand der übermittelten Eingabeformulare gruppiert der Algorithmus die Formulare nach Typ, erkennt, welche Schlüssel und Tabellen vorhanden sind, und ordnet Schlüsseln Werte und Tabellen Einträge zu. Hierfür ist keine manuelle Datenbeschriftung oder intensive Codierung und Wartung erforderlich, und Sie sollten diese Methode zuerst testen.

### <a name="train-with-labels"></a>Trainieren mit Beschriftungen

Wenn Sie mit beschrifteten Daten trainieren, extrahiert das Modell relevante Werte mit überwachtem Lernen, wobei die von Ihnen bereitgestellten beschrifteten Formulare verwendet werden. Dies führt zu Modellen mit besserer Leistung und kann Modelle hervorbringen, die mit komplexen Formularen oder Formularen arbeiten, die Werte ohne Schlüssel enthalten.

Die Formularerkennung verwendet die [Layout-API](#layout-api), um die erwarteten Größen und Positionen von gedruckten und handschriftlichen Textelementen zu erlernen. Anschließend werden benutzerdefinierte Beschriftungen verwendet, um die Schlüssel-Wert-Zuordnungen in den Dokumenten zu erlernen. Sie sollten fünf manuell beschriftete Formulare desselben Typs verwenden, um mit dem Trainieren eines neuen Modells zu beginnen, und weitere beschriftete Daten nach Bedarf hinzufügen, um die Modellgenauigkeit zu verbessern.

## <a name="prebuilt-receipt-model"></a>Vordefiniertes Belegmodell

Die Formularerkennung umfasst auch ein Modell zum Lesen englischsprachiger Verkaufsbelege aus den USA &mdash; des Typs, der von Restaurants, Tankstellen, Einzelhandel usw. verwendet wird ([Beispielbeleg](./media/contoso-receipt-small.png)). Dieses Modell extrahiert wichtige Informationen wie Zeitpunkt und Datum der Transaktion, Händlerinformationen, Steuer- und Summenbeträge und mehr. Darüber hinaus wird das vorgefertigte Belegmodell dazu trainiert, den gesamten Text eines Belegs zu erkennen und zurückzugeben.

## <a name="layout-api"></a>Layout-API

Die Formularerkennung kann auch Text- und Tabellenstruktur (die Zeilen- und Spaltennummern, die dem Text zugeordnet sind) mithilfe hochauflösender optischer Zeichenerkennung (Optical Character Recognition, OCR) extrahieren.

## <a name="get-started"></a>Erste Schritte

Befolgen Sie einen Schnellstart zum Extrahieren von Daten aus Ihren Formularen. Sie sollten den kostenlosen Dienst nutzen, wenn Sie die Technologie erlernen. Bedenken Sie, dass die Anzahl der kostenlosen Seiten auf 500 pro Monat beschränkt ist.

* Benutzerdefiniert – Trainieren eines Modells für Ihre Formulare
  * Trainieren ohne Beschriftungen
    * [Schnellstart: Trainieren eines Modells zur Formularerkennung und Extrahieren von Formulardaten unter Verwendung der REST-API mit cURL](quickstarts/curl-train-extract.md)
    * [Schnellstart: Trainieren eines Modells zur Formularerkennung und Extrahieren von Formulardaten unter Verwendung der REST-API mit Python](quickstarts/python-train-extract.md)
  * Trainieren mit Beschriftungen
    * [Trainieren eines Formularerkennungsmodells mit Beschriftungen mithilfe des Tools für die Beschriftung von Beispielen](quickstarts/label-tool.md)
    * [Trainieren eines Formularerkennungsmodells mit Beschriftungen mit der REST-API und Python](quickstarts/python-labeled-data.md)
* Vordefinierte Belege: Extrahieren von Daten aus USA-Verkaufsbelegen
  * [Schnellstart: Extrahieren von Verkaufsbelegdaten mithilfe von cURL](quickstarts/curl-receipts.md)
  * [Schnellstart: Extrahieren von Verkaufsbelegdaten mithilfe von Python](quickstarts/python-receipts.md)
* Layout: Extrahieren von Text- und Tabellenstruktur aus Formularen
  * [Schnellstart: Extrahieren von Layoutdaten mithilfe von Python](quickstarts/python-layout.md)

### <a name="review-the-rest-apis"></a>Überprüfen der REST-APIs

Verwenden Sie die folgenden APIs zum Trainieren von Modellen und Extrahieren strukturierter Daten aus Formularen.

|Name |BESCHREIBUNG |
|---|---|
| **Trainieren eines benutzerdefinierten Modells**| Trainieren eines neuen Modells zur Analyse Ihrer Formulare mit fünf Formularen gleichen Typs. Legen Sie den Parameter _useLabelFile_ auf `true` fest, um mit manuell beschrifteten Daten zu trainieren. |
| **Analysieren des Formulars** |Analysieren eines einzelnen Dokuments, das als Stream übergeben wird, um Schlüssel-Wert-Paare und Tabellen aus dem Formular mit Ihrem benutzerdefinierten Modell zu extrahieren.  |
| **Analysieren des Belegs** |Analysieren eines einzelnen Belegdokuments, um wichtige Informationen und anderen Belegtext zu extrahieren.|
| **Analysieren des Layouts** |Analysieren des Layouts eines Formulars, um Text- und Tabellenstruktur zu extrahieren.|

Sehen Sie sich die [Referenzdokumentation zur REST-API](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-preview/operations/AnalyzeWithCustomForm) an, um mehr zu erfahren. Wenn Sie mit einer früheren Version der API vertraut sind, finden Sie im Artikel [Neuerungen](./whats-new.md) weitere Informationen zu den aktuellen Änderungen.

## <a name="input-requirements"></a>Eingabeanforderungen
### <a name="custom-model"></a>Benutzerdefiniertes Modell

[!INCLUDE [input requirements](./includes/input-requirements.md)]

### <a name="prebuilt-receipt-model"></a>Vordefiniertes Belegmodell

Die Eingabeanforderungen für das Belegmodell unterscheiden sich geringfügig.

* Das Format muss JPEG, PNG, PDF (Text oder Scan) oder TIFF sein.
* Die Dateigröße muss weniger als 20 MB betragen.
* Bei Bildern müssen die Abmessungen zwischen 50 × 50 Pixel und 10.000 × 10.000 Pixel liegen.
* Die Abmessungen bei PDFs dürfen maximal 17 x 17 Zoll betragen. Dies entspricht den Papiergrößen Legal oder A3 und kleineren Formaten.
* Bei PDF- und TIFF-Dateien werden nur die ersten 200 Seiten verarbeitet. (Bei einem Abonnement im Free-Tarif werden nur die ersten beiden Seiten verarbeitet.)

## <a name="data-privacy-and-security"></a>Datenschutz und Sicherheit

Wie bei allen Cognitive Services-Diensten müssen Entwickler, die den Formularerkennungsdienst nutzen, die Microsoft-Richtlinien zu Kundendaten beachten. Weitere Informationen finden Sie im Microsoft Trust Center auf der [Seite zu Cognitive Services](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices).

## <a name="next-steps"></a>Nächste Schritte

Absolvieren Sie einen [Schnellstart](quickstarts/curl-train-extract.md) zu den ersten Schritten mit den [Formularerkennungs-APIs](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-preview/operations/AnalyzeWithCustomForm).
