---
title: 'Schnellstart: Trainieren mit Beschriftungen mit der REST-API und Python – Formularerkennung'
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie das Formularerkennungsfeature für beschriftete Daten mit der REST-API und Python verwenden, um ein benutzerdefiniertes Modell zu trainieren.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: quickstart
ms.date: 05/27/2020
ms.author: pafarley
ms.custom: tracking-python
ms.openlocfilehash: 12af4c57fd906d687eedfe7c865d36abaa0da18e
ms.sourcegitcommit: 6fd28c1e5cf6872fb28691c7dd307a5e4bc71228
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2020
ms.locfileid: "85209146"
---
# <a name="train-a-form-recognizer-model-with-labels-using-rest-api-and-python"></a>Trainieren eines Formularerkennungsmodells mit Beschriftungen mit der REST-API und Python

In dieser Schnellstartanleitung verwenden Sie die Formularerkennungs-REST-API zusammen mit Python, um ein benutzerdefiniertes Modell mit manuell beschrifteten Daten zu trainieren. Weitere Informationen zu diesem Feature finden Sie im Abschnitt [Trainieren mit Beschriftungen](../overview.md#train-with-labels) der Übersicht.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Für diesen Schnellstart benötigen Sie Folgendes:
- Wenn Sie das Beispiel lokal ausführen möchten, muss [Python](https://www.python.org/downloads/) installiert sein.
- Einen Satz mit mindestens sechs Formularen desselben Typs. Diese Daten verwenden Sie zum Trainieren des Modells und zum Testen eines Formulars. Für diesen Schnellstart können Sie ein [Beispieldataset](https://go.microsoft.com/fwlink/?linkid=2090451) verwenden. Laden Sie die Trainingsdateien in das Stammverzeichnis eines Blobspeichercontainers in einem Azure Storage-Konto hoch.

> [!NOTE]
> In dieser Schnellstartanleitung werden Remotedokumente verwendet, auf die über eine URL zugegriffen wird. Informationen zum Verwenden von lokalen Dateien finden Sie in der [Referenzdokumentation](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-preview/operations/TrainCustomModelAsync).

## <a name="create-a-form-recognizer-resource"></a>Erstellen einer Formularerkennungsressource

[!INCLUDE [create resource](../includes/create-resource.md)]

## <a name="set-up-training-data"></a>Einrichten der Trainingsdaten

Als Nächstes müssen Sie die erforderlichen Eingabedaten einrichten. Für das Feature für beschriftete Daten gelten über die Anforderungen zum Trainieren eines benutzerdefinierten Modells ohne Bezeichnungen hinaus besondere Anforderungen an die Eingabe.

Stellen Sie sicher, dass alle Trainingsdokumente im selben Format vorliegen. Wenn Ihre Formulare unterschiedliche Formate aufweisen, sortieren Sie sie in Unterordner für jeweils ein Format. Beim Trainieren müssen Sie für die API einen der Unterordner angeben.

Um ein Modell mit beschrifteten Daten zu trainieren, benötigen Sie folgende Dateien als Eingaben im Unterordner. Im Folgenden erfahren Sie, wie Sie diese Dateien erstellen.

* **Quellformulare**: Die Formulare, aus denen die Daten extrahiert werden sollen. Unterstützt werden die Typen JPEG, PNG, PDF und TIFF.
* **OCR-Layoutdateien**: Hierbei handelt es sich um JSON-Dateien, die die Größe und Position jedes lesbaren Textelements in jedem Quellformular beschreiben. Zum Generieren dieser Dateien verwenden Sie die Layout-API der Formularerkennung. 
* **Beschriftungsdateien**: Hierbei handelt es sich um JSON-Dateien, die Datenbeschriftungen beschreiben, die von einem Benutzer manuell eingegeben wurden.

All diese Dateien müssen sich im selben Unterordner befinden und das folgende Format aufweisen:

* input_file1.pdf 
* input_file1.pdf.ocr.json
* input_file1.pdf.labels.json 
* input_file2.pdf 
* input_file2.pdf.ocr.json
* input_file2.pdf.labels.json
* ...

> [!TIP]
> Wenn Sie Formulare mit dem [Tool für die Beschriftung von Beispielen](./label-tool.md) der Formularerkennung beschriften, erstellt das Tool diese Beschriftungs- und OCR-Layoutdateien automatisch.

### <a name="create-the-ocr-output-files"></a>Erstellen der OCR-Ausgabedateien

Sie benötigen OCR-Ausgabedateien, damit der Dienst die entsprechenden Eingabedateien für das Training mit Beschriftungen berücksichtigen kann. Um OCR-Ergebnisse für ein bestimmtes Quellformular abzurufen, gehen Sie folgendermaßen vor:

1. Rufen Sie die **[Analyze Layout](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-previewoperations/AnalyzeLayoutAsync)** -API im Layoutcontainer für den Lesevorgang auf, und verwenden Sie dabei die Eingabedatei als Teil des Anforderungstexts. Speichern Sie die ID, die sich im **Operation-Location**-Header der Antwort befindet.
1. Rufen Sie die **[Get Analyze Layout Result](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-previewoperations/GetAnalyzeLayoutResult)** -API auf, und verwenden Sie dabei die Vorgangs-ID aus dem vorherigen Schritt.
1. Rufen Sie die Antwort ab, und schreiben Sie den Inhalt in eine Datei. Für jedes Quellformular muss die entsprechende OCR-Datei den ursprünglichen Dateinamen mit dem Anhang `.ocr.json` aufweisen. Die OCR-JSON-Ausgabe muss im folgenden Format vorliegen. Ein vollständiges Beispiel finden Sie in der [OCR-Beispieldatei](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/curl/form-recognizer/Invoice_1.pdf.ocr.json). 

    ```json
    {
    "status": "succeeded",
    "createdDateTime": "2019-11-12T21:18:12Z",
    "lastUpdatedDateTime": "2019-11-12T21:18:17Z",
    "analyzeResult": {
        "version": "2.0.0",
        "readResults": [
            {
                "page": 1,
                "language": "en",
                "angle": 0,
                "width": 8.5,
                "height": 11,
                "unit": "inch",
                "lines": [
                    {
                        "language": "en",
                        "boundingBox": [
                            0.5384,
                            1.1583,
                            1.4466,
                            1.1583,
                            1.4466,
                            1.3534,
                            0.5384,
                            1.3534
                        ],
                        "text": "Contoso",
                        "words": [
                            {
                                "boundingBox": [
                                    0.5384,
                                    1.1583,
                                    1.4466,
                                    1.1583,
                                    1.4466,
                                    1.3534,
                                    0.5384,
                                    1.3534
                                ],
                                "text": "Contoso",
                                "confidence": 1
                            }
                        ]
                    },
                    ...
    ```

### <a name="create-the-label-files"></a>Erstellen der Beschriftungsdateien

Beschriftungsdateien enthalten Schlüssel-Wert-Zuordnungen, die von einem Benutzer manuell eingegeben wurden. Sie sind für das Trainieren von beschrifteten Daten erforderlich, aber nicht Quelldatei benötigt eine entsprechende Beschriftungsdatei. Quelldateien ohne Beschriftungen werden als gewöhnliche Trainingsdokumente behandelt. Um zuverlässige Trainingsergebnisse zu erzielen, werden mindestens fünf beschriftete Dateien empfohlen. Sie können ein Benutzeroberflächentool wie das [Tool für die Beschriftung von Beispielen](./label-tool.md) verwenden, um diese Dateien zu generieren.

Beim Erstellen einer Beschriftungsdatei können Sie optional Regionen angeben – dies sind exakte Positionen von Werten im Dokument. Dadurch erhöht sich die Genauigkeit des Trainings. Regionen sind als Satz aus acht Werten formatiert, die vier X,Y-Koordinaten entsprechen: oben links, oben rechts, unten links und unten rechts. Koordinatenwerte liegen zwischen 0 (null) und 1 (eins) und sind auf die Maße der Seite skaliert.

Für jedes Quellformular muss die entsprechende Beschriftungsdatei den ursprünglichen Dateinamen mit dem Anhang `.labels.json` aufweisen. Die Beschriftungsdatei muss im folgenden Format vorliegen. Ein vollständiges Beispiel finden Sie in der [Beispielbeschriftungsdatei](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/curl/form-recognizer/Invoice_1.pdf.labels.json).

```json
{
    "document": "Invoice_1.pdf",
    "labels": [
        {
            "label": "Provider",
            "key": null,
            "value": [
                {
                    "page": 1,
                    "text": "Contoso",
                    "boundingBoxes": [
                        [
                            0.06334117647058823,
                            0.1053,
                            0.17018823529411767,
                            0.1053,
                            0.17018823529411767,
                            0.12303636363636362,
                            0.06334117647058823,
                            0.12303636363636362
                        ]
                    ]
                }
            ]
        },
        {
            "label": "For",
            "key": null,
            "value": [
                {
                    "page": 1,
                    "text": "Microsoft",
                    "boundingBoxes": [
                        [
                            0.6122941176470589,
                            0.1374,
                            0.6841764705882353,
                            0.1374,
                            0.6841764705882353,
                            0.14682727272727272,
                            0.6122941176470589,
                            0.14682727272727272
                        ]
                    ]
                },
                {
                    "page": 1,
                    "text": "1020",
                    "boundingBoxes": [
                        [
                            0.6121882352941176,
                            0.156,
                            0.6462941176470588,
                            0.156,
                            0.6462941176470588,
                            0.1653181818181818,
                            0.6121882352941176,
                            0.1653181818181818
                        ]
                    ]
                },
                ...
```

> [!IMPORTANT]
> Sie können jedem Textelement nur eine Beschriftung zuweisen, und jede Beschriftung kann nur ein Mal pro Seite angewendet werden. Die Anwendung einer Beschriftung über mehrere Seiten hinweg ist nicht möglich.


## <a name="train-a-model-using-labeled-data"></a>Trainieren eines Modells mit beschrifteten Daten

Um ein Modell mit beschrifteten Daten zu trainieren, rufen Sie die **[Train Custom Model](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-previewoperations/TrainCustomModelAsync)** -API auf, indem Sie den folgenden Python-Code ausführen. Nehmen Sie die folgenden Änderungen vor, bevor Sie den Code ausführen:

1. Ersetzen Sie `<Endpoint>` durch die Endpunkt-URL für Ihre Formularerkennungsressource.
1. Ersetzen Sie `<SAS URL>` mit der Shared Access Signature-URL (SAS) des Azure Blob Storage-Containers. Um die SAS-URL abzurufen, öffnen Sie den Microsoft Azure Storage-Explorer, klicken mit der rechten Maustaste auf Ihren Container und wählen **Abrufen der Shared Access Signature** aus. Stellen Sie sicher, dass die Berechtigungen **Lesen** und **Auflisten**  aktiviert sind, und klicken Sie auf **Erstellen**. Kopieren Sie den Wert im **URL**-Abschnitt. Er muss das Format `https://<storage account>.blob.core.windows.net/<container name>?<SAS value>` aufweisen.
1. Ersetzen Sie `<Blob folder name>` durch den Ordnernamen in Ihrem Blobcontainer, in dem sich die Eingabedaten befinden. Wenn sich Ihre Daten im Stammverzeichnis befinden, geben Sie dort nichts ein und entfernen das Feld `"prefix"` aus dem Text der HTTP-Anforderung.

```python
########### Python Form Recognizer Labeled Async Train #############
import json
import time
from requests import get, post

# Endpoint URL
endpoint = r"<Endpoint>"
post_url = endpoint + r"/formrecognizer/v2.0/custom/models"
source = r"<SAS URL>"
prefix = "<Blob folder name>"
includeSubFolders = False
useLabelFile = True

headers = {
    # Request headers
    'Content-Type': 'application/json',
    'Ocp-Apim-Subscription-Key': '<subsription key>',
}

body =     {
    "source": source,
    "sourceFilter": {
        "prefix": prefix,
        "includeSubFolders": includeSubFolders
    },
    "useLabelFile": useLabelFile
}

try:
    resp = post(url = post_url, json = body, headers = headers)
    if resp.status_code != 201:
        print("POST model failed (%s):\n%s" % (resp.status_code, json.dumps(resp.json())))
        quit()
    print("POST model succeeded:\n%s" % resp.headers)
    get_url = resp.headers["location"]
except Exception as e:
    print("POST model failed:\n%s" % str(e))
    quit() 
```

## <a name="get-training-results"></a>Abrufen der Trainingsergebnisse

Nachdem Sie den Trainingsvorgang gestartet haben, können Sie die zurückgegebene ID verwenden, um den Status des Vorgangs abzurufen. Fügen Sie am Ende des Python-Skripts den folgenden Code hinzu. Hierbei wird der ID-Wert aus dem Trainingsaufruf in einem neuen API-Aufruf verwendet. Der Trainingsvorgang ist asynchron, sodass dieses Skript die API in regelmäßigen Abständen aufruft, bis die Ergebnisse vorliegen. Ein Intervall von mindestens einer Sekunde wird empfohlen.

```python 
n_tries = 15
n_try = 0
wait_sec = 5
max_wait_sec = 60
while n_try < n_tries:
    try:
        resp = get(url = get_url, headers = headers)
        resp_json = resp.json()
        if resp.status_code != 200:
            print("GET model failed (%s):\n%s" % (resp.status_code, json.dumps(resp_json)))
            quit()
        model_status = resp_json["modelInfo"]["status"]
        if model_status == "ready":
            print("Training succeeded:\n%s" % json.dumps(resp_json))
            quit()
        if model_status == "invalid":
            print("Training failed. Model is invalid:\n%s" % json.dumps(resp_json))
            quit()
        # Training still running. Wait and retry.
        time.sleep(wait_sec)
        n_try += 1
        wait_sec = min(2*wait_sec, max_wait_sec)     
    except Exception as e:
        msg = "GET model failed:\n%s" % str(e)
        print(msg)
        quit()
print("Train operation did not complete within the allocated time.")
```

Wenn der Trainingsvorgang abgeschlossen ist, erhalten Sie eine `201 (Success)`-Antwort mit JSON-Inhalt ähnlich dem folgenden. Die Antwort wurde zur Vereinfachung gekürzt.

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

Kopieren Sie den `"modelId"`-Wert, um ihn in den folgenden Schritten zu verwenden.

[!INCLUDE [analyze forms](../includes/python-custom-analyze.md)]

Wenn der Vorgang abgeschlossen ist, erhalten Sie eine `202 (Success)`-Antwort mit JSON-Inhalt im folgenden Format. Die Antwort wurde zur Vereinfachung gekürzt. Die wichtigsten Schlüssel-Wert-Zuordnungen befinden sich im Knoten `"documentResults"`. Die Ergebnisse der Layout-API (Inhalt und Positionen aller Textelemente im Dokument) befinden sich im Knoten `"readResults"`.

```json
{ 
    "analyzeResult":{ 
      "version":"2.0.0",
      "readResults":[ 
        { 
          "page":1,
          "language":"en",
          "angle":0,
          "width":8.5,
          "height":11,
          "unit":"inch",
          "lines":[ 
            { 
              "language":"en",
              "boundingBox":[ 
                0.5375,
                1.1349,
                2.6064,
                1.1349,
                2.6064,
                1.354,
                0.5375,
                1.354
              ],
              "text":"Contoso Suites",
              "words":[ 
                { 
                  "boundingBox":[ 
                    0.5375,
                    1.1402,
                    1.6595,
                    1.1402,
                    1.6595,
                    1.354,
                    0.5375,
                    1.354
                  ],
                  "text":"Contoso",
                  "confidence":1
                },
                { 
                  "boundingBox":[ 
                    1.758,
                    1.1349,
                    2.6064,
                    1.1349,
                    2.6064,
                    1.3534,
                    1.758,
                    1.3534
                  ],
                  "text":"Suites",
                  "confidence":1
                }
              ]
            },
            ...
          ]
        }
      ],
      "pageResults":[ 
        { 
          "page":1,
          "tables":[ 
            { 
              "rows":2,
              "columns":6,
              "cells":[ 
                { 
                  "rowIndex":0,
                  "columnIndex":0,
                  "text":"Invoice Number",
                  "boundingBox":[ 
                    0.5075,
                    2.8088,
                    1.9061,
                    2.8088,
                    1.9061,
                    3.3219,
                    0.5075,
                    3.3219
                  ],
                  "elements":[ 
                    "#/readResults/0/lines/7/words/0",
                    "#/readResults/0/lines/7/words/1"
                  ]
                },
                { 
                  "rowIndex":0,
                  "columnIndex":1,
                  "text":"Invoice Date",
                  "boundingBox":[ 
                    1.9061,
                    2.8088,
                    3.3074,
                    2.8088,
                    3.3074,
                    3.3219,
                    1.9061,
                    3.3219
                  ],
                  "elements":[ 
                    "#/readResults/0/lines/8/words/0",
                    "#/readResults/0/lines/8/words/1"
                  ]
                },
                ...        
              ]
            }
          ]
        }
      ],
      "documentResults":[ 
        { 
          "docType":"Analyze",
          "pageRange":[ 
            1,
            1
          ],
          "fields":{ 
            "total":{ 
              "type":"string",
              "valueString":"$22,123.24",
              "text":"$22,123.24",
              "boundingBox":[ 
                5.29,
                3.41,
                5.9750000000000009,
                3.41,
                5.9750000000000009,
                3.54,
                5.29,
                3.54
              ],
              "page":1,
              "confidence":1,
              "elements":[ 
                "#/analyzeResult/readResults/0/lines/15/words/0"
              ]
            },
            "invoice #":{ 
              "type":"string",
              "valueString":"7689302",
              "text":"7689302",
              "boundingBox":[ 
                0.54,
                3.41,
                1.065,
                3.41,
                1.065,
                3.515,
                0.54,
                3.515
              ],
              "page":1,
              "confidence":1,
              "elements":[ 
                "#/analyzeResult/readResults/0/lines/12/words/0"
              ]
            },
            "vat":{ 
              "type":"string",
              "valueString":"QR",
              "text":"QR",
              "boundingBox":[ 
                6.2250000000000009,
                3.41,
                6.425,
                3.41,
                6.425,
                3.52,
                6.2250000000000009,
                3.52
              ],
              "page":1,
              "confidence":0.9839357733726502,
              "elements":[ 
                "#/analyzeResult/readResults/0/lines/16/words/0"
              ]
            },
            ...
          }
        }
      ]
    },
    "status":"succeeded",
    "createdDateTime":"2019-11-12T21:26:19+00:00",
    "lastUpdatedDateTime":"2019-11-12T21:27:27.0488571+00:00"
}
```

## <a name="improve-results"></a>Verbessern der Ergebnisse

Überprüfen Sie die `"confidence"`-Werte für jedes Schlüssel-Wert-Ergebnis unter dem Knoten `"documentResults"`. Sie sollten sich auch die Konfidenzscores im Knoten `"readResults"` ansehen, die dem Layoutvorgang entsprechen. Die Konfidenz der Layoutergebnisse wirkt sich nicht auf die Konfidenz der Schlüssel-Wert-Extraktionsergebnisse aus, daher sollten Sie beide Bewertungen überprüfen.
* Wenn die Konfidenzscores für den Layoutvorgang niedrig sind, versuchen Sie, die Qualität Ihrer Eingabedokumente zu verbessern (siehe [Anforderungen an die Eingabe](../overview.md#input-requirements)).
* Wenn die Konfidenzscores für den Schlüssel-Wert-Extraktionsvorgang niedrig sind, stellen Sie sicher, dass die analysierten Dokumente denselben Typ aufweisen wie die Dokumente, die im Trainingsset verwendet wurden. Wenn die Dokumente im Trainingsset Variationen im Aussehen aufweisen, erwägen Sie, die Dokumente auf verschiedene Ordner aufzuteilen und für jede Variation ein eigenes Modell zu trainieren.

### <a name="avoid-cluttered-labels"></a>Vermeiden von sich überschneidenden Beschriftungen

Wenn Sie verschiedene Beschriftungen auf der gleichen Textzeile anwenden, kommt es vor, dass der Dienst diese Beschriftungen in ein Feld zusammenführt. Ein Beispiel: Sie haben in einer Anschrift den Ort, das Bundesland und die Postleitzahl in verschiedenen Feldern beschriftet, aber während der Vorhersage werden diese Felder nicht einzeln erkannt.

Uns ist bewusst, dass ein solches Szenario von großer Bedeutung für unsere Kunden ist, und wir arbeiten daran, dieses Verhalten in Zukunft zu verbessern. Zurzeit empfehlen wir Benutzern, mehrere eng beieinanderstehende Felder als ein Feld zu beschriften, und die Begriffe dann bei der Nachbearbeitung der Extraktionsergebnisse zu trennen.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie erfahren, wie Sie die Formularerkennungs-REST-API zusammen mit Python verwenden, um ein benutzerdefiniertes Modell mit manuell beschrifteten Daten zu trainieren. Lesen Sie als Nächstes die API-Referenzdokumentation, um die Formularerkennungs-API eingehender zu erkunden.

> [!div class="nextstepaction"]
> [Referenzdokumentation zur Rest-API](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-previewoperations/AnalyzeWithCustomForm)
