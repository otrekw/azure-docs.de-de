---
title: 'Schnellstart: Trainieren eines Modells und Extrahieren von Formulardaten mit cURL – Formularerkennung'
titleSuffix: Azure Cognitive Services
description: In diesem Schnellstart verwenden Sie die Formularerkennungs-REST-API mit cURL, um ein Modell zu trainieren und Daten aus Formularen zu extrahieren.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: quickstart
ms.date: 10/05/2020
ms.author: pafarley
ms.openlocfilehash: b0532007ff03cd9dcf253824a1158fd0b8661120
ms.sourcegitcommit: 6a4687b86b7aabaeb6aacdfa6c2a1229073254de
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/06/2020
ms.locfileid: "91760460"
---
# <a name="quickstart-train-a-form-recognizer-model-and-extract-form-data-by-using-the-rest-api-with-curl"></a>Schnellstart: Trainieren eines Modells zur Formularerkennung und Extrahieren von Formulardaten unter Verwendung der REST-API mit cURL

In diesem Schnellstart verwenden Sie die REST-API der Azure-Formularerkennung mit cURL, um Formulare zu trainieren und zu bewerten und um Schlüssel-Wert-Paare und Tabellen zu extrahieren.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/cognitive-services/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Für diesen Schnellstart benötigen Sie Folgendes:
- [cURL](https://curl.haxx.se/windows/) muss installiert sein.
- Einen Satz mit mindestens sechs Formularen desselben Typs. Sie verwenden fünf dieser Methoden, um das Modell zu trainieren, und testen es dann mit dem sechsten Formular. Die Dateitypen Ihrer Formulare können unterschiedlich sein, sie müssen jedoch den gleichen Dokumenttyp aufweisen. Für diesen Schnellstart können Sie ein [Beispieldataset](https://go.microsoft.com/fwlink/?linkid=2090451) verwenden. Laden Sie die Trainingsdateien in das Stammverzeichnis eines Blobspeichercontainers in einem Azure Storage-Konto mit der Leistungsstufe „Standard“ hoch. Sie können die Testdateien in einem separaten Ordner speichern.

## <a name="create-a-form-recognizer-resource"></a>Erstellen einer Formularerkennungsressource

[!INCLUDE [create resource](../includes/create-resource.md)]

## <a name="train-a-form-recognizer-model"></a>Trainieren eines Formularerkennungsmodells

Zunächst benötigen Sie Trainingsdaten in einem Azure Storage-Blob. Als Haupteingabedaten sollten Sie mindestens fünf ausgefüllte Formulare (PDF-Dokumente und/oder Bilder) desselben Typs/mit der gleichen Struktur haben. Alternativ können Sie ein einzelnes leeres Formular mit zwei ausgefüllten Formularen verwenden. Der Dateiname des leeren Formulars muss das Wort „empty“ enthalten. Unter [Erstellen eines Schulungsdatensatzes für ein benutzerdefiniertes Modell](../build-training-data-set.md) finden Sie Tipps und Optionen für das Zusammenstellen von Trainingsdaten.

> [!NOTE]
> Sie können das Feature „beschriftete Daten“ verwenden, um einige oder alle Ihrer Trainingsdaten vorab manuell zu beschriften. Dies ist ein komplexerer Prozess, der jedoch ein besser trainiertes Modell hervorbringt. Weitere Informationen zu diesem Feature finden Sie im Abschnitt [Trainieren mit Beschriftungen](../overview.md#train-with-labels) der Übersicht.



Um ein Formularerkennungsmodell mit den Dokumenten in Ihrem Azure-Blobcontainer zu trainieren, rufen Sie die **[Train Custom Model](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/TrainCustomModelAsync)** -API (Trainieren des benutzerdefinierten Modells) auf, indem Sie den folgenden cURL-Befehl ausführen. Nehmen Sie die folgenden Änderungen vor, bevor Sie den Befehl ausführen:

1. Ersetzen Sie `<Endpoint>` durch den Endpunkt, den Sie mit Ihrem Abonnement für die Formularerkennung erhalten haben.
1. Ersetzen Sie `<subscription key>` durch den Abonnementschlüssel, den Sie im vorherigen Schritt kopiert haben.
1. Ersetzen Sie `<SAS URL>` mit der Shared Access Signature-URL (SAS) des Azure Blob Storage-Containers. Um die SAS-URL abzurufen, öffnen Sie den Microsoft Azure Storage-Explorer, klicken mit der rechten Maustaste auf Ihren Container und wählen **Abrufen der Shared Access Signature** aus. Stellen Sie sicher, dass die Berechtigungen **Lesen** und **Auflisten**  aktiviert sind, und klicken Sie auf **Erstellen**. Kopieren Sie den Wert im **URL**-Abschnitt. Er muss das Format `https://<storage account>.blob.core.windows.net/<container name>?<SAS value>` aufweisen.

    # <a name="v20"></a>[v2.0](#tab/v2-0)    
    ```bash
    curl -i -X POST "https://<Endpoint>/formrecognizer/v2.0/custom/models" -H "Content-Type: application/json" -H "Ocp-Apim-Subscription-Key: <subscription key>" --data-ascii "{ \"source\": \""<SAS URL>"\"}"
    ```
    # <a name="v21-preview"></a>[Vorschauversion v2.1](#tab/v2-1)    
    ```bash
    curl -i -X POST "https://<Endpoint>/formrecognizer/v2.1-preview.1/custom/models" -H "Content-Type: application/json" -H "Ocp-Apim-Subscription-Key: <subscription key>" --data-ascii "{ \"source\": \""<SAS URL>"\"}"
    ```
    
    ---



Sie erhalten eine `201 (Success)`-Antwort mit einem **Location**-Header. Der Wert dieses Headers ist die ID des neuen Modells, das trainiert wird.

## <a name="get-training-results"></a>Abrufen der Trainingsergebnisse

Nachdem Sie das Training gestartet haben, verwenden Sie einen neuen Vorgang, **[Get Custom Model](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/GetCustomModel)** (Benutzerdefiniertes Modell abrufen), um den Trainingsstatus zu überprüfen. Übergeben Sie die Modell-ID an diesen API-Aufruf, um den Trainingsstatus zu überprüfen:

1. Ersetzen Sie `<Endpoint>` durch den Endpunkt, den Sie mit Ihrem Abonnementschlüssel für die Formularerkennung erhalten haben.
1. Ersetzen Sie `<subscription key>` durch Ihren Abonnementschlüssel.
1. Ersetzen Sie `<model ID>` durch die Modell-ID, die Sie im vorherigen Schritt erhalten haben.



# <a name="v20"></a>[v2.0](#tab/v2-0)    
```bash
curl -X GET "https://<Endpoint>/formrecognizer/v2.0/custom/models/<model ID>" -H "Content-Type: application/json" -H "Ocp-Apim-Subscription-Key: <subscription key>"
```
# <a name="v21-preview"></a>[Vorschauversion v2.1](#tab/v2-1)    
```bash
curl -X GET "https://<Endpoint>/formrecognizer/v2.1-preview.1/custom/models/<model ID>" -H "Content-Type: application/json" -H "Ocp-Apim-Subscription-Key: <subscription key>"
```ce\": \""<SAS URL>"\"}"
```
    
---

Sie erhalten eine `200 (Success)`-Antwort mit einem JSON-Text im folgenden Format. Beachten Sie das `"status"`-Feld. Dies hat nach Abschluss des Trainings den Wert `"ready"`. Wenn das Training des Modells noch nicht abgeschlossen ist, müssen Sie den Dienst erneut abfragen, indem Sie den Befehl erneut ausführen. Ein Intervall von mindestens einer Sekunde zwischen den Aufrufen wird empfohlen.

Das `"modelId"`-Feld enthält die ID des Modells, das Sie trainieren. Sie benötigen sie im nächsten Schritt.

```json
{
  "modelInfo":{
    "status":"ready",
    "createdDateTime":"2019-10-08T10:20:31.957784",
    "lastUpdatedDateTime":"2019-10-08T14:20:41+00:00",
    "modelId":"1cfb372bab404ba3aa59481ab2c63da5"
  },
  "trainResult":{
    "trainingDocuments":[
      {
        "documentName":"invoices\\Invoice_1.pdf",
        "pages":1,
        "errors":[

        ],
        "status":"succeeded"
      },
      {
        "documentName":"invoices\\Invoice_2.pdf",
        "pages":1,
        "errors":[

        ],
        "status":"succeeded"
      },
      {
        "documentName":"invoices\\Invoice_3.pdf",
        "pages":1,
        "errors":[

        ],
        "status":"succeeded"
      },
      {
        "documentName":"invoices\\Invoice_4.pdf",
        "pages":1,
        "errors":[

        ],
        "status":"succeeded"
      },
      {
        "documentName":"invoices\\Invoice_5.pdf",
        "pages":1,
        "errors":[

        ],
        "status":"succeeded"
      }
    ],
    "errors":[

    ]
  },
  "keys":{
    "0":[
      "Address:",
      "Invoice For:",
      "Microsoft",
      "Page"
    ]
  }
}
```

## <a name="analyze-forms-for-key-value-pairs-and-tables"></a>Analysieren von Formularen für Schlüssel-Wert-Paare und Tabellen

Als Nächstes verwenden Sie Ihr gerade trainiertes Modell, um ein Dokument zu analysieren und Schlüssel-Werte-Paare und Tabellen daraus zu extrahieren. Rufen Sie die **[Analyze Form](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/AnalyzeWithCustomForm)** -API (Formular analysieren) auf, indem Sie den folgenden cURL-Befehl ausführen. Nehmen Sie die folgenden Änderungen vor, bevor Sie den Befehl ausführen:

1. Ersetzen Sie `<Endpoint>` durch den Endpunkt, den Sie mit Ihrem Abonnementschlüssel für die Formularerkennung erhalten haben. Sie finden ihn auf der Registerkarte **Übersicht** der Formularerkennungsressource.
1. Ersetzen Sie `<model ID>` durch die Modell-ID, die Sie im vorherigen Abschnitt erhalten haben.
1. Ersetzen Sie `<SAS URL>` durch eine SAS-URL zu Ihrer Datei in Azure Storage. Befolgen Sie die Schritte im Abschnitt „Training“. Rufen Sie jedoch anstelle einer SAS-URL für den gesamten Blobcontainer eine URL für die zu analysierende Datei ab.
1. Ersetzen Sie `<subscription key>` durch Ihren Abonnementschlüssel.

# <a name="v20"></a>[v2.0](#tab/v2-0)    
```bash
curl -v "https://<Endpoint>/formrecognizer/v2.0/custom/models/<model ID>/analyze" -H "Content-Type: application/json" -H "Ocp-Apim-Subscription-Key: <subscription key>" -d "{ \"source\": \""<SAS URL>"\" } "
```
# <a name="v21-preview"></a>[Vorschauversion v2.1](#tab/v2-1)    
```bash
```bash
curl -v "https://<Endpoint>/formrecognizer/v2.1-preview.1/custom/models/<model ID>/analyze" -H "Content-Type: application/json" -H "Ocp-Apim-Subscription-Key: <subscription key>" -d "{ \"source\": \""<SAS URL>"\" } "
```
    
---



Sie erhalten eine `202 (Success)`-Antwort mit einem **Operation-Location**-Header. Der Wert dieses Headers enthält eine Ergebnis-ID, mit der Sie die Ergebnisse des Analysevorgangs nachverfolgen können. Speichern Sie diese Ergebnis-ID für den nächsten Schritt.

## <a name="get-the-analyze-results"></a>Abrufen der Analyseergebnisse

Verwenden Sie die folgende API, um die Ergebnisse des Analysevorgangs abzufragen.

1. Ersetzen Sie `<Endpoint>` durch den Endpunkt, den Sie mit Ihrem Abonnementschlüssel für die Formularerkennung erhalten haben. Sie finden ihn auf der Registerkarte **Übersicht** der Formularerkennungsressource.
1. Ersetzen Sie `<result ID>` durch die ID, die Sie im vorherigen Abschnitt erhalten haben.
1. Ersetzen Sie `<subscription key>` durch Ihren Abonnementschlüssel.


# <a name="v20"></a>[v2.0](#tab/v2-0)    
```bash
curl -X GET "https://<Endpoint>/formrecognizer/v2.0/custom/models/<model ID>/analyzeResults/<result ID>" -H "Ocp-Apim-Subscription-Key: <subscription key>"
```
# <a name="v21-preview"></a>[Vorschauversion v2.1](#tab/v2-1)    
```bash
curl -X GET "https://<Endpoint>/formrecognizer/v2.1-preview/custom/models/<model ID>/analyzeResults/<result ID>" -H "Ocp-Apim-Subscription-Key: <subscription key>"
```
    
---

Sie erhalten eine `200 (Success)`-Antwort mit einem JSON-Text im folgenden Format. Die Ausgabe wurde zur Vereinfachung gekürzt. Beachten Sie das `"status"`-Feld im unteren Bereich. Dies hat den Wert `"succeeded"`, wenn der Analysevorgang beendet ist. Wenn der Analysevorgang noch nicht abgeschlossen ist, müssen Sie den Dienst erneut abfragen, indem Sie den Befehl erneut ausführen. Ein Intervall von mindestens einer Sekunde zwischen den Aufrufen wird empfohlen.

Die wichtigsten Schlüssel-Wert-Paar-Zuordnungen und Tabellen befinden sich im Knoten `"pageResults"`. Wenn Sie auch die Nur-Text-Extraktion über den *includeTextDetails*-URL-Parameter angegeben haben, zeigt der Knoten `"readResults"` den Inhalt und die Positionen des gesamten Texts im Dokument an.

Diese JSON-Beispielausgabe wurde zur Vereinfachung gekürzt.

# <a name="v20"></a>[v2.0](#tab/v2-0)
```JSON
{
  "status": "succeeded",
  "createdDateTime": "2020-08-21T00:46:25Z",
  "lastUpdatedDateTime": "2020-08-21T00:46:32Z",
  "analyzeResult": {
    "version": "2.0.0",
    "readResults": [
      {
        "page": 1,
        "angle": 0,
        "width": 8.5,
        "height": 11,
        "unit": "inch",
        "lines": [
          {
            "text": "Project Statement",
            "boundingBox": [
              5.0153,
              0.275,
              8.0944,
              0.275,
              8.0944,
              0.7125,
              5.0153,
              0.7125
            ],
            "words": [
              {
                "text": "Project",
                "boundingBox": [
                  5.0153,
                  0.275,
                  6.2278,
                  0.275,
                  6.2278,
                  0.7125,
                  5.0153,
                  0.7125
                ]
              },
              {
                "text": "Statement",
                "boundingBox": [
                  6.3292,
                  0.275,
                  8.0944,
                  0.275,
                  8.0944,
                  0.7125,
                  6.3292,
                  0.7125
                ]
              }
            ]
          }, 
        ...
        ]
      }
    ],
    "pageResults": [
      {
        "page": 1,
        "keyValuePairs": [
          {
            "key": {
              "text": "Date:",
              "boundingBox": [
                6.9722,
                1.0264,
                7.3417,
                1.0264,
                7.3417,
                1.1931,
                6.9722,
                1.1931
              ],
              "elements": [
                "#/readResults/0/lines/2/words/0"
              ]
            },
            "confidence": 1
          },
         ...
        ],
        "tables": [
          {
            "rows": 4,
            "columns": 5,
            "cells": [
              {
                "text": "Training Date",
                "rowIndex": 0,
                "columnIndex": 0,
                "boundingBox": [
                  0.6931,
                  4.2444,
                  1.5681,
                  4.2444,
                  1.5681,
                  4.4125,
                  0.6931,
                  4.4125
                ],
                "confidence": 1,
                "rowSpan": 1,
                "columnSpan": 1,
                "elements": [
                  "#/readResults/0/lines/15/words/0",
                  "#/readResults/0/lines/15/words/1"
                ],
                "isHeader": true,
                "isFooter": false
              },
              ...
            ]
          }
        ], 
        "clusterId": 0
      }
    ],
    "documentResults": [],
    "errors": []
  }
}
```    
# <a name="v21-preview"></a>[Vorschauversion v2.1](#tab/v2-1)    
```JSON
{
  "status": "succeeded",
  "createdDateTime": "2020-08-21T01:13:28Z",
  "lastUpdatedDateTime": "2020-08-21T01:13:42Z",
  "analyzeResult": {
    "version": "2.1.0",
    "readResults": [
      {
        "page": 1,
        "angle": 0,
        "width": 8.5,
        "height": 11,
        "unit": "inch",
        "lines": [
          {
            "text": "Project Statement",
            "boundingBox": [
              5.0444,
              0.3613,
              8.0917,
              0.3613,
              8.0917,
              0.6718,
              5.0444,
              0.6718
            ],
            "words": [
              {
                "text": "Project",
                "boundingBox": [
                  5.0444,
                  0.3587,
                  6.2264,
                  0.3587,
                  6.2264,
                  0.708,
                  5.0444,
                  0.708
                ]
              },
              {
                "text": "Statement",
                "boundingBox": [
                  6.3361,
                  0.3635,
                  8.0917,
                  0.3635,
                  8.0917,
                  0.6396,
                  6.3361,
                  0.6396
                ]
              }
            ]
          }, 
          ...
        ] 
      }
    ],
    "pageResults": [
      {
        "page": 1,
        "keyValuePairs": [
          {
            "key": {
              "text": "Date:",
              "boundingBox": [
                6.9833,
                1.0615,
                7.3333,
                1.0615,
                7.3333,
                1.1649,
                6.9833,
                1.1649
              ],
              "elements": [
                "#/readResults/0/lines/2/words/0"
              ]
            },
            "value": {
              "text": "9/10/2020",
              "boundingBox": [
                7.3833,
                1.0802,
                7.925,
                1.0802,
                7.925,
                1.174,
                7.3833,
                1.174
              ],
              "elements": [
                "#/readResults/0/lines/3/words/0"
              ]
            },
            "confidence": 1
          },
          ...
        ], 
        "tables": [
          {
            "rows": 5,
            "columns": 5,
            "cells": [
              {
                "text": "Training Date",
                "rowIndex": 0,
                "columnIndex": 0,
                "boundingBox": [
                  0.6944,
                  4.2779,
                  1.5625,
                  4.2779,
                  1.5625,
                  4.4005,
                  0.6944,
                  4.4005
                ],
                "confidence": 1,
                "rowSpan": 1,
                "columnSpan": 1,
                "elements": [
                  "#/readResults/0/lines/15/words/0",
                  "#/readResults/0/lines/15/words/1"
                ],
                "isHeader": true,
                "isFooter": false
              },
              ...
            ]
          }
        ], 
        "clusterId": 0
      }
    ], 
    "documentResults": [],
    "errors": []
  }
}
``` 

---


## <a name="improve-results"></a>Verbessern der Ergebnisse

[!INCLUDE [improve results](../includes/improve-results-unlabeled.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie die REST-API der Formularerkennung mit cURL verwendet, um ein Modell zu trainieren und es in einem Beispielszenario auszuführen. Lesen Sie als Nächstes die Referenzdokumentation, um die Formularerkennungs-API eingehender zu erkunden.

> [!div class="nextstepaction"]
> [Referenzdokumentation zur Rest-API](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/AnalyzeWithCustomForm)
