---
title: 'Schnellstart: Trainieren eines Modells und Extrahieren von Formulardaten mit cURL – Formularerkennung'
titleSuffix: Azure Cognitive Services
description: In diesem Schnellstart verwenden Sie die Formularerkennungs-REST-API mit cURL, um ein Modell zu trainieren und Daten aus Formularen zu extrahieren.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: quickstart
ms.date: 05/27/2020
ms.author: pafarley
ms.openlocfilehash: ea38b7351d2ba512261de94ac00a06eec9ba9946
ms.sourcegitcommit: 6fd28c1e5cf6872fb28691c7dd307a5e4bc71228
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2020
ms.locfileid: "85206251"
---
# <a name="quickstart-train-a-form-recognizer-model-and-extract-form-data-by-using-the-rest-api-with-curl"></a>Schnellstart: Trainieren eines Modells zur Formularerkennung und Extrahieren von Formulardaten unter Verwendung der REST-API mit cURL

In diesem Schnellstart verwenden Sie die REST-API der Azure-Formularerkennung mit cURL, um Formulare zu trainieren und zu bewerten und um Schlüssel-Wert-Paare und Tabellen zu extrahieren.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Für diesen Schnellstart benötigen Sie Folgendes:
- [cURL](https://curl.haxx.se/windows/) muss installiert sein.
- Einen Satz mit mindestens sechs Formularen desselben Typs. Sie verwenden fünf dieser Methoden, um das Modell zu trainieren, und testen es dann mit dem sechsten Formular. Die Dateitypen Ihrer Formulare können unterschiedlich sein, sie müssen jedoch den gleichen Dokumenttyp aufweisen. Für diesen Schnellstart können Sie ein [Beispieldataset](https://go.microsoft.com/fwlink/?linkid=2090451) verwenden. Laden Sie die Trainingsdateien in das Stammverzeichnis eines Blobspeichercontainers in einem Azure Storage-Konto hoch. Sie können die Testdateien in einem separaten Ordner speichern.

## <a name="create-a-form-recognizer-resource"></a>Erstellen einer Formularerkennungsressource

[!INCLUDE [create resource](../includes/create-resource.md)]

## <a name="train-a-form-recognizer-model"></a>Trainieren eines Formularerkennungsmodells

Zunächst benötigen Sie Trainingsdaten in einem Azure Storage-Blob. Als Haupteingabedaten sollten Sie mindestens fünf ausgefüllte Formulare (PDF-Dokumente und/oder Bilder) desselben Typs/mit der gleichen Struktur haben. Alternativ können Sie ein einzelnes leeres Formular mit zwei ausgefüllten Formularen verwenden. Der Dateiname des leeren Formulars muss das Wort „empty“ enthalten. Unter [Erstellen eines Schulungsdatensatzes für ein benutzerdefiniertes Modell](../build-training-data-set.md) finden Sie Tipps und Optionen für das Zusammenstellen von Trainingsdaten.

> [!NOTE]
> Sie können das Feature „beschriftete Daten“ verwenden, um einige oder alle Ihrer Trainingsdaten vorab manuell zu beschriften. Dies ist ein komplexerer Prozess, der jedoch ein besser trainiertes Modell hervorbringt. Weitere Informationen zu diesem Feature finden Sie im Abschnitt [Trainieren mit Beschriftungen](../overview.md#train-with-labels) der Übersicht.

Um ein Formularerkennungsmodell mit den Dokumenten in Ihrem Azure-Blobcontainer zu trainieren, rufen Sie die **[Train Custom Model](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-previewoperations/TrainCustomModelAsync)** -API (Trainieren des benutzerdefinierten Modells) auf, indem Sie den folgenden cURL-Befehl ausführen. Nehmen Sie die folgenden Änderungen vor, bevor Sie den Befehl ausführen:

1. Ersetzen Sie `<Endpoint>` durch den Endpunkt, den Sie mit Ihrem Abonnement für die Formularerkennung erhalten haben.
1. Ersetzen Sie `<subscription key>` durch den Abonnementschlüssel, den Sie im vorherigen Schritt kopiert haben.
1. Ersetzen Sie `<SAS URL>` mit der Shared Access Signature-URL (SAS) des Azure Blob Storage-Containers. Um die SAS-URL abzurufen, öffnen Sie den Microsoft Azure Storage-Explorer, klicken mit der rechten Maustaste auf Ihren Container und wählen **Abrufen der Shared Access Signature** aus. Stellen Sie sicher, dass die Berechtigungen **Lesen** und **Auflisten**  aktiviert sind, und klicken Sie auf **Erstellen**. Kopieren Sie den Wert im **URL**-Abschnitt. Er muss das Format `https://<storage account>.blob.core.windows.net/<container name>?<SAS value>` aufweisen.

```bash
curl -i -X POST "https://<Endpoint>/formrecognizer/v2.0/custom/models" -H "Content-Type: application/json" -H "Ocp-Apim-Subscription-Key: <subscription key>" --data-ascii "{ \"source\": \""<SAS URL>"\"}"
```

Sie erhalten eine `201 (Success)`-Antwort mit einem **Location**-Header. Der Wert dieses Headers ist die ID des neuen Modells, das trainiert wird. 

## <a name="get-training-results"></a>Abrufen der Trainingsergebnisse

Nachdem Sie das Training gestartet haben, verwenden Sie einen neuen Vorgang, **[Get Custom Model](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-previewoperations/GetCustomModel)** (Benutzerdefiniertes Modell abrufen), um den Trainingsstatus zu überprüfen. Übergeben Sie die Modell-ID an diesen API-Aufruf, um den Trainingsstatus zu überprüfen:

1. Ersetzen Sie `<Endpoint>` durch den Endpunkt, den Sie mit Ihrem Abonnementschlüssel für die Formularerkennung erhalten haben.
1. Ersetzen Sie `<subscription key>` durch Ihren Abonnementschlüssel.
1. Ersetzen Sie `<model ID>` durch die Modell-ID, die Sie im vorherigen Schritt erhalten haben.

```bash
curl -X GET "https://<Endpoint>/formrecognizer/v2.0/custom/models/<model ID>" -H "Content-Type: application/json" -H "Ocp-Apim-Subscription-Key: <subscription key>"
```

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

Als Nächstes verwenden Sie Ihr gerade trainiertes Modell, um ein Dokument zu analysieren und Schlüssel-Werte-Paare und Tabellen daraus zu extrahieren. Rufen Sie die **[Analyze Form](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-previewoperations/AnalyzeWithCustomForm)** -API (Formular analysieren) auf, indem Sie den folgenden cURL-Befehl ausführen. Nehmen Sie die folgenden Änderungen vor, bevor Sie den Befehl ausführen:

1. Ersetzen Sie `<Endpoint>` durch den Endpunkt, den Sie mit Ihrem Abonnementschlüssel für die Formularerkennung erhalten haben. Sie finden ihn auf der Registerkarte **Übersicht** der Formularerkennungsressource.
1. Ersetzen Sie `<model ID>` durch die Modell-ID, die Sie im vorherigen Abschnitt erhalten haben.
1. Ersetzen Sie `<SAS URL>` durch eine SAS-URL zu Ihrer Datei in Azure Storage. Befolgen Sie die Schritte im Abschnitt „Training“. Rufen Sie jedoch anstelle einer SAS-URL für den gesamten Blobcontainer eine URL für die zu analysierende Datei ab.
1. Ersetzen Sie `<subscription key>` durch Ihren Abonnementschlüssel.

```bash
curl -v "https://<Endpoint>/formrecognizer/v2.0/custom/models/<model ID>/analyze" -H "Content-Type: application/json" -H "Ocp-Apim-Subscription-Key: <subscription key>" -d "{ \"source\": \""<SAS URL>"\" } "
```

Sie erhalten eine `202 (Success)`-Antwort mit einem **Operation-Location**-Header. Der Wert dieses Headers enthält eine Ergebnis-ID, mit der Sie die Ergebnisse des Analysevorgangs nachverfolgen können. Speichern Sie diese Ergebnis-ID für den nächsten Schritt.

## <a name="get-the-analyze-results"></a>Abrufen der Analyseergebnisse

Verwenden Sie die folgende API, um die Ergebnisse des Analysevorgangs abzufragen.

1. Ersetzen Sie `<Endpoint>` durch den Endpunkt, den Sie mit Ihrem Abonnementschlüssel für die Formularerkennung erhalten haben. Sie finden ihn auf der Registerkarte **Übersicht** der Formularerkennungsressource.
1. Ersetzen Sie `<result ID>` durch die ID, die Sie im vorherigen Abschnitt erhalten haben.
1. Ersetzen Sie `<subscription key>` durch Ihren Abonnementschlüssel.

```bash
curl -X GET "https://<Endpoint>/formrecognizer/v2.0/custom/models/<model ID>/analyzeResults/<result ID>" -H "Ocp-Apim-Subscription-Key: <subscription key>"
```

Sie erhalten eine `200 (Success)`-Antwort mit einem JSON-Text im folgenden Format. Die Ausgabe wurde zur Vereinfachung gekürzt. Beachten Sie das `"status"`-Feld im unteren Bereich. Dies hat den Wert `"succeeded"`, wenn der Analysevorgang beendet ist. Wenn der Analysevorgang noch nicht abgeschlossen ist, müssen Sie den Dienst erneut abfragen, indem Sie den Befehl erneut ausführen. Ein Intervall von mindestens einer Sekunde zwischen den Aufrufen wird empfohlen.

Die wichtigsten Schlüssel-Wert-Paar-Zuordnungen und Tabellen befinden sich im Knoten `"pageResults"`. Wenn Sie auch die Nur-Text-Extraktion über den *includeTextDetails*-URL-Parameter angegeben haben, zeigt der Knoten `"readResults"` den Inhalt und die Positionen des gesamten Texts im Dokument an.

```json
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

In diesem Schnellstart haben Sie die REST-API der Formularerkennung mit cURL verwendet, um ein Modell zu trainieren und es in einem Beispielszenario auszuführen. Lesen Sie als Nächstes die Referenzdokumentation, um die Formularerkennungs-API eingehender zu erkunden.

> [!div class="nextstepaction"]
> [Referenzdokumentation zur Rest-API](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-previewoperations/AnalyzeWithCustomForm)
