---
title: 'IDs: Formularerkennung'
titleSuffix: Azure Cognitive Services
description: Konzepte der Datenextraktion aus Ausweisdokumenten mit der vordefinierten ID-API der Formularerkennung.
services: cognitive-services
author: laujan
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: conceptual
ms.date: 03/15/2021
ms.author: lajanuar
ms.openlocfilehash: ed8516f9a898131338fb5b4d75e25cd774c5ab43
ms.sourcegitcommit: b8995b7dafe6ee4b8c3c2b0c759b874dff74d96f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2021
ms.locfileid: "106285355"
---
# <a name="form-recognizer-prebuilt-identification-card-id-model"></a>Vordefiniertes ID-Modell der Formularerkennung für Ausweise

Mit der Azure-Formularerkennung können Informationen aus amtlichen Ausweisen unter Verwendung des vordefinierten ID-Modells analysiert und extrahiert werden. Dabei werden unsere leistungsstarken Funktionen zur [optischen Zeichenerkennung (Optical Character Recognition, OCR)](../computer-vision/overview-ocr.md) mit ID-Erkennungsfunktionen kombiniert, um wesentliche Informationen aus internationalen Reisepässen und US-amerikanischen Führerscheinen (alle 50 Bundesstaaten). Mit der ID-API werden wesentliche Informationen aus diesen Ausweisdokumenten extrahiert, z. B. Vorname, Nachname, Geburtsdatum und Dokumentnummer. Diese API ist in der Vorschauversion 2.1 der Formularerkennung als Clouddienst und als lokaler Container verfügbar.

## <a name="what-does-the-id-service-do"></a>Funktionsweise des ID-Diensts 

Der vorgefertigte ID-Dienst extrahiert die Schlüsselwerte aus den internationalen Reisepässen und den US-Führerscheinen und gibt Sie in einer organisierten strukturierten JSON-Antwort zurück. 

![Beispiel eines Führerscheins](./media/id-example-drivers-license.JPG)

![Beispiel eines Reisepasses](./media/id-example-passport-result.JPG)

### <a name="fields-extracted"></a>Extrahierte Felder

|Name| Typ | BESCHREIBUNG | Wert | 
|:-----|:----|:----|:----|
|  Land | country | Ländercode, konform zu ISO 3166-Standard | „USA“ | 
|  DateOfBirth | date | Geburtsdatum im Format JJJJ-MM-TT | „1980-01-01“ | 
|  DateOfExpiration | date | Ablaufdatum im Format JJJJ-MM-TT | „2019-05-05“ |  
|  DocumentNumber | Zeichenfolge | Relevante Passnummer, Führerscheinnummer usw. | „340020013“ |  
|  FirstName | Zeichenfolge | Extrahierter Vorname und ggf. Mittelinitial | „JENNIFER“ | 
|  LastName | Zeichenfolge | Extrahierter Nachname | „BROOKS“ |   
|  Nationality | country | Ländercode, konform zu ISO 3166-Standard | „USA“ |
|  Geschlecht | gender | Mögliche extrahierte Werte: „M“, „F“ und „X“ | "F" | 
|  MachineReadableZone | Objekt | Maschinenlesbarer zweizeiliger Bereich (Machine Readable Zone, MRZ) mit jeweils 44 Zeichen | „P<USABROOKS<<JENNIFER<<<<<<<<<<<<<<<<<<<<<<< 3400200135USA8001014F1905054710000307<715816“ |
|  DocumentType | Zeichenfolge | Dokumenttyp, z. B. Pass oder Führerschein | „passport“ |  
|  Adresse | Zeichenfolge | Extrahierte Adresse (nur beim Führerschein) | „123 STREET ADDRESS YOUR CITY WA 99999-1234“|
|  Region | Zeichenfolge | Extrahierte Region, Bundesstaat, Provinz usw. (nur Führerschein) | „Washington“ | 

### <a name="additional-features"></a>Zusätzliche Features

Mit der ID-API werden außerdem die folgenden Informationen zurückgegeben:

* Feldvertrauensgrad (jedes Feld gibt einen zugehörigen Vertrauenswert zurück)
* OCR-Rohtext (OCR-extrahierte Textausgabe für den gesamten Beleg)
* Begrenzungsrahmen für jedes extrahierte Feld bei US-Führerscheinen
* Begrenzungsrahmen für jeden maschinenlesbaren Bereich in Reisepässen

  > [!NOTE]
  > Mit der vordefinierten ID-API wird die ID-Echtheit nicht erkannt.
  >
  > Mit der vordefinierten ID-API der Formularerkennung werden wesentliche Daten aus ID-Daten extrahiert. Die Gültigkeit oder Echtheit des Originalausweisdokuments kann jedoch nicht erkannt werden. 

## <a name="try-it-out"></a>Ausprobieren

Zum Testen des ID-Diensts der Formularerkennung wechseln Sie zum Onlinetool für die Beispielbenutzeroberfläche:

> [!div class="nextstepaction"]
> [Ausprobieren von vordefinierten Modellen](https://fott-preview.azurewebsites.net/)

## <a name="input-requirements"></a>Eingabeanforderungen

[!INCLUDE [input requirements](./includes/input-requirements-receipts.md)]

## <a name="supported-id-types"></a>Unterstützte ID-Typen  

* **Pre-built IDs v2.1-preview.3:** Extrahiert die Schlüsselwerte aus internationalen Reisepässen und US-Führerscheinen. 

  > [!NOTE]
  > Unterstützung von ID-Typen 
  >
  > Derzeit werden als ID-Typen internationale Reisepässe und US-Führerscheine unterstützt. Wir arbeiten aktiv daran, die Unterstützung auf andere Ausweisdokumente auf der ganzen Welt auszuweiten.

## <a name="post-analyze-id-document"></a>POST Analyze Id Document

Beim Vorgang [Analyze ID](https://westus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1-preview-3/operations/5f74a7daad1f2612c46f5822) wird ein Bild oder eine PDF-Datei eines Ausweisdokuments als Eingabe verwendet und die relevanten Werte extrahiert. Bei diesem Aufruf wird ein Antwortheaderfeld namens `Operation-Location` zurückgegeben. Der `Operation-Location`-Wert ist eine URL, die die Ergebnis-ID enthält, die im nächsten Schritt verwendet werden soll.

|Antwortheader| Ergebnis-URL |
|:-----|:----|
|Operation-Location | `https://cognitiveservice/formrecognizer/v2.1-preview.3/prebuilt/idDocument/analyzeResults/49a36324-fc4b-4387-aa06-090cfbf0064f` |

## <a name="get-analyze-id-document-result"></a>GET Analyze Id Document Result

<!---
Need to update this with updated APIM links when available
-->

Im zweiten Schritt wird der Vorgang [**Get Analyze idDocument Result**](https://westus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1-preview-3/operations/GetAnalyzeFormResult) aufgerufen. Bei diesem Vorgang wird die Ergebnis-ID, die durch den Vorgang „Analyze ID“ erstellt wurde, als Eingabe verwendet. Er gibt eine JSON-Antwort zurück, die ein **Status**-Feld mit den folgenden möglichen Werten enthält. Sie rufen diesen Vorgang iterativ auf, bis er mit dem Wert **succeeded** (erfolgreich) zurückgegeben wird. Verwenden Sie ein Intervall von 3 bis 5 Sekunden, um zu vermeiden, dass die Rate der Anforderungen pro Sekunde (RPS) überschritten wird.

|Feld| type | Mögliche Werte |
|:-----|:----:|:----|
|status | Zeichenfolge | notStarted: Der Analysevorgang wurde noch nicht gestartet. |
| |  | running: Der Analysevorgang wird ausgeführt. |
| |  | failed: Beim Analysevorgang ist ein Fehler aufgetreten. |
| |  | succeeded: Der Analysevorgang war erfolgreich. |

Wenn im Feld **status** der Wert **succeeded** angezeigt wird, enthält die JSON-Antwort die Ergebnisse der Belegerfassung und Texterkennung. Das Ergebnis des ID-Diensts ist als Wörterbuch benannter Feldwerte organisiert, wobei jeder Wert den extrahierten Text, den normalisierten Wert, den Begrenzungsrahmen, die Konfidenz und die entsprechenden Wortelemente enthält. Das Ergebnis der Texterkennung ist als eine Hierarchie von Zeilen und Wörtern mit Text, Begrenzungsrahmen und Informationen zum Vertrauensgrad organisiert.

![Ergebnisse des Beispielbelegs](./media/id-example-passport-result.JPG)

### <a name="sample-json-output"></a>JSON-Beispielausgabe

Eine erfolgreiche JSON-Antwort sieht in etwa wie folgendes Beispiel aus: Der `readResults`-Knoten enthält den gesamten erkannten Text. Der Text ist nach Seite, dann nach Zeile und dann nach einzelnen Wörtern sortiert. Der `documentResults`-Knoten enthält die vom Modell erkannten ID-Werte. Dieser Knoten enthält außerdem nützliche Schlüssel-Wert-Paare wie Vorname, Nachname, Dokumentnummer usw.

```json
{ 
   "status": "succeeded",
  "createdDateTime": "2021-03-04T22:29:33Z",
  "lastUpdatedDateTime": "2021-03-04T22:29:36Z",
  "analyzeResult": {
    "version": "2.1.0",
    "readResults": [
     {
        "page": 1,
        "angle": 0.3183,
        "width": 549,
        "height": 387,
        "unit": "pixel",
        "lines": [
          {
            "text": "PASSPORT",
            "boundingBox": [
              57,
              10,
              120,
              11,
              119,
              22,
              57,
              22
            ],
            "words": [
              {
                "text": "PASSPORT",
                "boundingBox": [
                  57,
                  11,
                  119,
                  11,
                  118,
                  23,
                  57,
                  22
                ],
                "confidence": 0.994
              }
            ],
          ...
      }
    ],
    
     "documentResults": [
      {
        "docType": "prebuilt:idDocument:passport",
        "docTypeConfidence": 0.995,
        "pageRange": [
          1,
          1
        ],
        "fields": {
          "Country": {
            "type": "country",
            "valueCountry": "USA",
            "text": "USA"
          },
          "DateOfBirth": {
            "type": "date",
            "valueDate": "1980-01-01",
            "text": "800101"
          },
          "DateOfExpiration": {
            "type": "date",
            "valueDate": "2019-05-05",
            "text": "190505"
          },
          "DocumentNumber": {
            "type": "string",
            "valueString": "340020013",
            "text": "340020013"
          },
          "FirstName": {
            "type": "string",
            "valueString": "JENNIFER",
            "text": "JENNIFER"
          },
          "LastName": {
            "type": "string",
            "valueString": "BROOKS",
            "text": "BROOKS"
          },
          "Nationality": {
            "type": "country",
            "valueCountry": "USA",
            "text": "USA"
          },
          "Sex": {
            "type": "gender",
            "valueGender": "F",
            "text": "F"
          },
          "MachineReadableZone": {
            "type": "object",
            "text": "P<USABROOKS<<JENNIFER<<<<<<<<<<<<<<<<<<<<<<< 3400200135USA8001014F1905054710000307<715816",
            "boundingBox": [
              16,
              314.1,
              504.2,
              317,
              503.9,
              363,
              15.7,
              360.1
            ],
            "page": 1,
            "confidence": 0.384,
            "elements": [
              "#/readResults/0/lines/33/words/0",
              "#/readResults/0/lines/33/words/1",
              "#/readResults/0/lines/33/words/2",
              "#/readResults/0/lines/33/words/3",
              "#/readResults/0/lines/33/words/4",
              "#/readResults/0/lines/34/words/0"
            ]
          },
          "DocumentType": {
            "type": "string",
            "text": "passport",
            "confidence": 0.995
          }
        }
      }
    ]
  }
}
```


## <a name="next-steps"></a>Nächste Schritte

- Testen Sie eigene Ausweisdokumente und Beispiele in der [Beispielbenutzeroberfläche für die Formularerkennung](https://fott-preview.azurewebsites.net/).
- Sehen Sie sich die Informationen in einem [Schnellstart zur Formularerkennung](quickstarts/client-library.md) an, um eine Anwendung zur Verarbeitung von Ausweisdokumenten mit Formularerkennung in der gewünschten Programmiersprache zu schreiben.

## <a name="see-also"></a>Siehe auch

* [**Was ist die Formularerkennung?**](./overview.md)
* [**REST-API-Referenzdokumente**](https://westus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1-preview-3/operations/AnalyzeWithCustomForm)
