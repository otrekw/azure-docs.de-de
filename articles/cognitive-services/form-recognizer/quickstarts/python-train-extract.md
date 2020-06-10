---
title: 'Schnellstart: Trainieren eines Modells und Extrahieren von Formulardaten unter Verwendung der REST-API mit Python: Formularerkennung'
titleSuffix: Azure Cognitive Services
description: In diesem Schnellstart verwenden Sie die REST-API für die Formularerkennung mit Python, um ein Modell zu trainieren und Daten aus Formularen zu extrahieren.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: quickstart
ms.date: 05/27/2020
ms.author: pafarley
ms.custom: tracking-python
ms.openlocfilehash: 948c30c641b68f7966831a3324c1bfab5dbd3b03
ms.sourcegitcommit: 1de57529ab349341447d77a0717f6ced5335074e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84610734"
---
# <a name="quickstart-train-a-form-recognizer-model-and-extract-form-data-by-using-the-rest-api-with-python"></a>Schnellstart: Trainieren eines Modells zur Formularerkennung und Extrahieren von Formulardaten unter Verwendung der REST-API mit Python

In diesem Schnellstart verwenden Sie die REST-API der Azure-Formularerkennung mit Python, um Formulare zu trainieren und zu bewerten und um Schlüssel-Wert-Paare und Tabellen zu extrahieren.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Für diesen Schnellstart benötigen Sie Folgendes:
- Wenn Sie das Beispiel lokal ausführen möchten, muss [Python](https://www.python.org/downloads/) installiert sein.
- Einen Satz von mindestens fünf Formularen des gleichen Typs. Zum Trainieren des Modells verwenden Sie diese Daten. Die Dateitypen Ihrer Formulare können unterschiedlich sein, sie müssen jedoch den gleichen Dokumenttyp aufweisen. Für diesen Schnellstart können Sie ein [Beispieldataset](https://go.microsoft.com/fwlink/?linkid=2090451) verwenden. Laden Sie die Trainingsdateien in das Stammverzeichnis eines Blobspeichercontainers in einem Azure Storage-Konto hoch.

> [!NOTE]
> In dieser Schnellstartanleitung werden Remotedokumente verwendet, auf die über eine URL zugegriffen wird. Informationen zum Verwenden von lokalen Dateien finden Sie in der [Referenzdokumentation](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-preview/operations/TrainCustomModelAsync).


## <a name="create-a-form-recognizer-resource"></a>Erstellen einer Formularerkennungsressource

[!INCLUDE [create resource](../includes/create-resource.md)]

## <a name="train-a-form-recognizer-model"></a>Trainieren eines Formularerkennungsmodells

Zunächst benötigen Sie Trainingsdaten in einem Azure Storage-Blobcontainer. Als Haupteingabedaten sollten Sie mindestens fünf ausgefüllte Formulare (PDF-Dokumente und/oder Bilder) desselben Typs/mit der gleichen Struktur haben. Alternativ können Sie ein einzelnes leeres Formular mit zwei ausgefüllten Formularen verwenden. Der Dateiname des leeren Formulars muss das Wort „empty“ enthalten. Unter [Erstellen eines Schulungsdatensatzes für ein benutzerdefiniertes Modell](../build-training-data-set.md) finden Sie Tipps und Optionen für das Zusammenstellen von Trainingsdaten.

> [!NOTE]
> Sie können das Feature „beschriftete Daten“ verwenden, um einige oder alle Ihrer Trainingsdaten vorab manuell zu beschriften. Dies ist ein komplexerer Prozess, der jedoch ein besser trainiertes Modell hervorbringt. Weitere Informationen finden Sie im Abschnitt [Trainieren mit Beschriftungen](../overview.md#train-with-labels) der Übersicht.

Um ein Formularerkennungsmodell mit den Dokumenten in Ihrem Azure-Blobcontainer zu trainieren, rufen Sie die **[Train Custom Model](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-preview/operations/TrainCustomModelAsync)** -API (Trainieren des benutzerdefinierten Modells) auf, indem Sie den folgenden Python-Code ausführen. Nehmen Sie die folgenden Änderungen vor, bevor Sie den Code ausführen:

1. Ersetzen Sie `<SAS URL>` mit der Shared Access Signature-URL (SAS) des Azure Blob Storage-Containers. Um die SAS-URL abzurufen, öffnen Sie den Microsoft Azure Storage-Explorer, klicken mit der rechten Maustaste auf Ihren Container und wählen **Abrufen der Shared Access Signature** aus. Stellen Sie sicher, dass die Berechtigungen **Lesen** und **Auflisten**  aktiviert sind, und klicken Sie auf **Erstellen**. Kopieren Sie den Wert im **URL**-Abschnitt. Er muss das Format `https://<storage account>.blob.core.windows.net/<container name>?<SAS value>` aufweisen.
1. Ersetzen Sie `<subscription key>` durch den Abonnementschlüssel, den Sie im vorherigen Schritt kopiert haben.
1. Ersetzen Sie `<endpoint>` durch die Endpunkt-URL für Ihre Formularerkennungsressource.
1. Ersetzen Sie `<Blob folder name>` durch den Pfad zu dem Ordner im Blobspeicher, in dem sich die Formulare befinden. Wenn sich Ihre Formulare im Stammverzeichnis Ihres Containers befinden, lassen Sie diese Zeichenfolge leer.

    ```python
    ########### Python Form Recognizer Labeled Async Train #############
    import json
    import time
    from requests import get, post
    
    # Endpoint URL
    endpoint = r"<endpoint>"
    post_url = endpoint + r"/formrecognizer/v2.0-preview/custom/models"
    source = r"<SAS URL>"
    prefix = "<Blob folder name>"
    includeSubFolders = False
    useLabelFile = False
    
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
1. Speichern Sie den Code in einer Datei mit der Erweiterung „.py“. Beispiel: *form-recognizer-train.py*.
1. Öffnen Sie ein Eingabeaufforderungsfenster.
1. Verwenden Sie an der Eingabeaufforderung den Befehl `python`, um das Beispiel auszuführen. Beispiel: `python form-recognizer-train.py`.

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

Wenn der Trainingsvorgang abgeschlossen ist, erhalten Sie eine `201 (Success)`-Antwort mit JSON-Inhalt ähnlich dem folgenden:

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

Wenn der Vorgang abgeschlossen ist, erhalten Sie eine `200 (Success)`-Antwort mit JSON-Inhalt im folgenden Format. Die Antwort wurde zur Vereinfachung gekürzt. Die wichtigsten Schlüssel-Wert-Paar-Zuordnungen und Tabellen befinden sich im Knoten `"pageResults"`. Wenn Sie auch die Nur-Text-Extraktion über den *includeTextDetails*-URL-Parameter angegeben haben, zeigt der Knoten `"readResults"` den Inhalt und die Positionen des gesamten Texts im Dokument an.

```bash
{
  "analyzeResult":{ 
    "readResults":[ 
      { 
        "page":1,
        "width":8.5,
        "height":11.0,
        "angle":0,
        "unit":"inch",
        "lines":[ 
          { 
            "text":"Contoso",
            "boundingBox":[ 
              0.5278,
              1.0597,
              1.4569,
              1.0597,
              1.4569,
              1.4347,
              0.5278,
              1.4347
            ],
            "words":[ 
              { 
                "text":"Contoso",
                "boundingBox":[ 
                  0.5278,
                  1.0597,
                  1.4569,
                  1.0597,
                  1.4569,
                  1.4347,
                  0.5278,
                  1.4347
                ]
              }
            ]
          },
          ...
          { 
            "text":"PT",
            "boundingBox":[ 
              6.2181,
              3.3528,
              6.3944,
              3.3528,
              6.3944,
              3.5417,
              6.2181,
              3.5417
            ],
            "words":[ 
              { 
                "text":"PT",
                "boundingBox":[ 
                  6.2181,
                  3.3528,
                  6.3944,
                  3.3528,
                  6.3944,
                  3.5417,
                  6.2181,
                  3.5417
                ]
              }
            ]
          }
        ]
      }
    ],
    "version":"2.0.0",
    "errors":[ 

    ],
    "documentResults":[ 

    ],
    "pageResults":[ 
      { 
        "page":1,
        "clusterId":1,
        "keyValuePairs":[ 
          { 
            "key":{ 
              "text":"Address:",
              "boundingBox":[ 
                0.7972,
                1.5125,
                1.3958,
                1.5125,
                1.3958,
                1.6431,
                0.7972,
                1.6431
              ],
              "elements":[ 
                "#/readResults/0/lines/1/words/0"
              ]
            },
            "value":{ 
              "text":"1 Redmond way Suite 6000 Redmond, WA 99243",
              "boundingBox":[ 
                0.7972,
                1.6764,
                2.15,
                1.6764,
                2.15,
                2.2181,
                0.7972,
                2.2181
              ],
              "elements":[ 
                "#/readResults/0/lines/4/words/0",
                "#/readResults/0/lines/4/words/1",
                "#/readResults/0/lines/4/words/2",
                "#/readResults/0/lines/4/words/3",
                "#/readResults/0/lines/6/words/0",
                "#/readResults/0/lines/6/words/1",
                "#/readResults/0/lines/6/words/2",
                "#/readResults/0/lines/8/words/0"
              ]
            },
            "confidence":0.86
          },
          { 
            "key":{ 
              "text":"Invoice For:",
              "boundingBox":[ 
                4.3903,
                1.5125,
                5.1139,
                1.5125,
                5.1139,
                1.6431,
                4.3903,
                1.6431
              ],
              "elements":[ 
                "#/readResults/0/lines/2/words/0",
                "#/readResults/0/lines/2/words/1"
              ]
            },
            "value":{ 
              "text":"Microsoft 1020 Enterprise Way Sunnayvale, CA 87659",
              "boundingBox":[ 
                5.1917,
                1.4458,
                6.6583,
                1.4458,
                6.6583,
                2.0347,
                5.1917,
                2.0347
              ],
              "elements":[ 
                "#/readResults/0/lines/3/words/0",
                "#/readResults/0/lines/5/words/0",
                "#/readResults/0/lines/5/words/1",
                "#/readResults/0/lines/5/words/2",
                "#/readResults/0/lines/7/words/0",
                "#/readResults/0/lines/7/words/1",
                "#/readResults/0/lines/7/words/2"
              ]
            },
            "confidence":0.86
          },
          ...
        ],
        "tables":[ 
          { 
            "caption":null,
            "rows":2,
            "columns":5,
            "cells":[ 
              { 
                "rowIndex":0,
                "colIndex":0,
                "header":true,
                "text":"Invoice Number",
                "boundingBox":[ 
                  0.5347,
                  2.8722,
                  1.575,
                  2.8722,
                  1.575,
                  3.0028,
                  0.5347,
                  3.0028
                ],
                "elements":[ 
                  "#/readResults/0/lines/9/words/0",
                  "#/readResults/0/lines/9/words/1"
                ]
              },
              { 
                "rowIndex":0,
                "colIndex":1,
                "header":true,
                "text":"Invoice Date",
                "boundingBox":[ 
                  1.9403,
                  2.8722,
                  2.7569,
                  2.8722,
                  2.7569,
                  3.0028,
                  1.9403,
                  3.0028
                ],
                "elements":[ 
                  "#/readResults/0/lines/10/words/0",
                  "#/readResults/0/lines/10/words/1"
                ]
              },
              { 
                "rowIndex":0,
                "colIndex":2,
                "header":true,
                "text":"Invoice Due Date",
                "boundingBox":[ 
                  3.3403,
                  2.8722,
                  4.4583,
                  2.8722,
                  4.4583,
                  3.0028,
                  3.3403,
                  3.0028
                ],
                "elements":[ 
                  "#/readResults/0/lines/11/words/0",
                  "#/readResults/0/lines/11/words/1",
                  "#/readResults/0/lines/11/words/2"
                ]
              },
              ...
            ]
          }
        ]
      }
    ]
  },
  "lastUpdatedDateTime":"2019-10-07T19:32:18+00:00",
  "status":"succeeded",
  "createdDateTime":"2019-10-07T19:32:15+00:00"
}
```

## <a name="improve-results"></a>Verbessern der Ergebnisse

[!INCLUDE [improve results](../includes/improve-results-unlabeled.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie die REST-API der Formularerkennung mit Python verwendet, um ein Modell zu trainieren und es in einem Beispielszenario auszuführen. Lesen Sie als Nächstes die Referenzdokumentation, um die Formularerkennungs-API eingehender zu erkunden.

> [!div class="nextstepaction"]
> [Referenzdokumentation zur Rest-API](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-preview/operations/AnalyzeWithCustomForm)
