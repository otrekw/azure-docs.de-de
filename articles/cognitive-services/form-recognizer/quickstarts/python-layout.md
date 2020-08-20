---
title: 'Schnellstart: Extrahieren von Text und Layoutinformationen mit Python: Formularerkennung'
titleSuffix: Azure Cognitive Services
description: In dieser Schnellstartanleitung verwenden Sie die Layout-REST-API der Formularerkennung mit Python, um Text- und Tabellendaten in Ihren Formularen zu lesen.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: quickstart
ms.date: 05/27/2020
ms.author: pafarley
ms.custom: devx-track-python
ms.openlocfilehash: 1777d4d09dfb12f490245f726715aed4eefd6227
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/18/2020
ms.locfileid: "88517794"
---
# <a name="quickstart-extract-text-and-layout-information-using-the-form-recognizer-rest-api-with-python"></a>Schnellstart: Extrahieren von Text und Layoutinformationen mit der Formularerkennungs-REST-API und Python

In dieser Schnellstartanleitung verwenden Sie die REST-API der Azure-Formularerkennung mit Python, um Textlayoutinformationen und Tabellendaten aus Formulardokumenten zu extrahieren.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/cognitive-services/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Für diesen Schnellstart benötigen Sie Folgendes:
- Wenn Sie das Beispiel lokal ausführen möchten, muss [Python](https://www.python.org/downloads/) installiert sein.
- Ein Formulardokument. Sie können für diesen Schnellstart Bilddateien aus dem [Beispieldataset](https://go.microsoft.com/fwlink/?linkid=2090451) herunterladen.

> [!NOTE]
> In dieser Schnellstartanleitung wird ein lokal gespeichertes Dokument verwendet. Informationen zum Verwenden von Remotedateien, auf die über URLs zugegriffen wird, finden Sie in der [Referenzdokumentation](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/AnalyzeLayoutAsync).


## <a name="create-a-form-recognizer-resource"></a>Erstellen einer Formularerkennungsressource

[!INCLUDE [create resource](../includes/create-resource.md)]

## <a name="analyze-the-form-layout"></a>Analysieren des Formularlayouts

Um mit der Analyse des Layouts zu beginnen, rufen Sie die **[Analyze Layout](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/AnalyzeLayoutAsync)** -API mit dem folgenden Python-Skript auf. Nehmen Sie die folgenden Änderungen vor, bevor Sie das Skript ausführen:

1. Ersetzen Sie `<Endpoint>` durch den Endpunkt, den Sie mit Ihrem Abonnement für die Formularerkennung erhalten haben.
1. Ersetzen Sie `<path to your form>` durch den Pfad zu Ihrem lokalen Formulardokument.
1. Ersetzen Sie `<subscription key>` durch den Abonnementschlüssel, den Sie im vorherigen Schritt kopiert haben.

    ```python
    ########### Python Form Recognizer Async Layout #############

    import json
    import time
    from requests import get, post
    
    # Endpoint URL
    endpoint = r"<Endpoint>"
    apim_key = "<Subscription Key>"
    post_url = endpoint + "/formrecognizer/v2.0/Layout/analyze"
    source = r"<path to your form>"
    
    headers = {
        # Request headers
        'Content-Type': 'application/json',
        'Ocp-Apim-Subscription-Key': apim_key,
    }
    with open(source, "rb") as f:
        data_bytes = f.read()
    
    try:
        resp = post(url = post_url, data = data_bytes, headers = headers)
        if resp.status_code != 202:
            print("POST analyze failed:\n%s" % resp.text)
            quit()
        print("POST analyze succeeded:\n%s" % resp.headers)
        get_url = resp.headers["operation-location"]
    except Exception as e:
        print("POST analyze failed:\n%s" % str(e))
        quit()
    ```

1. Speichern Sie den Code in einer Datei mit der Erweiterung „.py“. Beispiel: *form-recognizer-layout.py*.
1. Öffnen Sie ein Eingabeaufforderungsfenster.
1. Verwenden Sie an der Eingabeaufforderung den Befehl `python`, um das Beispiel auszuführen. Beispiel: `python form-recognizer-layout.py`.

Sie erhalten eine `202 (Success)`-Antwort, die einen **Operation-Location**-Header umfasst, den das Skript an der Konsole ausgibt. Dieser Header enthält eine Vorgangs-ID, mit der Sie den Status des asynchronen Vorgangs abfragen und die Ergebnisse abrufen können. Im folgenden Beispielwert ist die Zeichenfolge nach `operations/` die Vorgangs-ID.

```console
https://cognitiveservice/formrecognizer/v2.0/layout/operations/54f0b076-4e38-43e5-81bd-b85b8835fdfb
```

## <a name="get-the-layout-results"></a>Abrufen der Layoutergebnisse

Nachdem Sie die **Analyze Layout**-API aufgerufen haben, rufen Sie die **[Get Analyze Layout Result](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/GetAnalyzeLayoutResult)** -API auf, um den Status des Vorgangs und die extrahierten Daten abzurufen. Fügen Sie am Ende des Python-Skripts den folgenden Code hinzu. Dieser Code verwendet den Vorgangs-ID-Wert in einem neuen API-Aufruf. Dieses Skript ruft die API in regelmäßigen Abständen auf, bis die Ergebnisse vorliegen. Ein Intervall von mindestens einer Sekunde wird empfohlen.

```python
n_tries = 10
n_try = 0
wait_sec = 6
while n_try < n_tries:
    try:
        resp = get(url = get_url, headers = {"Ocp-Apim-Subscription-Key": apim_key})
        resp_json = json.loads(resp.text)
        if resp.status_code != 200:
            print("GET Layout results failed:\n%s" % resp_json)
            quit()
        status = resp_json["status"]
        if status == "succeeded":
            print("Layout Analysis succeeded:\n%s" % resp_json)
            quit()
        if status == "failed":
            print("Layout Analysis failed:\n%s" % resp_json)
            quit()
        # Analysis still running. Wait and retry.
        time.sleep(wait_sec)
        n_try += 1     
    except Exception as e:
        msg = "GET analyze results failed:\n%s" % str(e)
        print(msg)
        quit()
```

1. Speichern Sie das Skript.
1. Verwenden Sie erneut den Befehl `python`, um das Beispiel auszuführen. Beispiel: `python form-recognizer-layout.py`.

### <a name="examine-the-response"></a>Untersuchen der Antwort

Das Skript gibt Antworten an der Konsole aus, bis der **Analyze Layout**-Vorgang abgeschlossen ist. Anschließend werden die extrahierten Daten im JSON-Format ausgegeben. Der Knoten `"readResults"` enthält jede Textzeile mit der Platzierung des zugehörigen Begrenzungsrahmens auf der Seite. Das Feld `"pageResults"` zeigt jedes Textelement in Tabellen, jeweils mit zugehöriger Spalten-/Zeilenkoordinate.

Vergleichen Sie das folgende Bild einer Rechnung mit der entsprechenden JSON-Ausgabe. Die Ausgabe wurde zur Vereinfachung gekürzt.

> [!div class="mx-imgBorder"]
> ![Contoso-Rechnungsdokument mit einer Tabelle](../media/contoso-invoice.png)

```json
{ 
  "status":"succeeded",
  "createdDateTime":"2019-11-12T19:55:36Z",
  "lastUpdatedDateTime":"2019-11-12T19:55:43Z",
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
              0.5384,
              1.1583,
              1.4466,
              1.1583,
              1.4466,
              1.3534,
              0.5384,
              1.3534
            ],
            "text":"Contoso",
            "words":[ 
              { 
                "boundingBox":[ 
                  0.5384,
                  1.1583,
                  1.4466,
                  1.1583,
                  1.4466,
                  1.3534,
                  0.5384,
                  1.3534
                ],
                "text":"Contoso",
                "confidence":1
              }
            ]
          },
          { 
            "language":"en",
            "boundingBox":[ 
              0.7994,
              1.5143,
              1.3836,
              1.5143,
              1.3836,
              1.6154,
              0.7994,
              1.6154
            ],
            "text":"Address:",
            "words":[ 
              { 
                "boundingBox":[ 
                  0.7994,
                  1.5143,
                  1.3836,
                  1.5143,
                  1.3836,
                  1.6154,
                  0.7994,
                  1.6154
                ],
                "text":"Address:",
                "confidence":1
              }
            ]
          },
          ...
          { 
            "language":"en",
            "boundingBox":[ 
              6.2285,
              3.4114,
              6.3919,
              3.4114,
              6.3919,
              3.5119,
              6.2285,
              3.5119
            ],
            "text":"PT",
            "words":[ 
              { 
                "boundingBox":[ 
                  6.2285,
                  3.4114,
                  6.3919,
                  3.4114,
                  6.3919,
                  3.5119,
                  6.2285,
                  3.5119
                ],
                "text":"PT",
                "confidence":1
              }
            ]
          }
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
                  "#/readResults/0/lines/8/words/0",
                  "#/readResults/0/lines/8/words/1"
                ]
              },
              ...
            ]
          }
        ]
      }
    ]
  }
}
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie die Formularerkennungs-REST-API mit Python verwendet, um das Textlayout einer Rechnung zu extrahieren. Lesen Sie als Nächstes die Referenzdokumentation, um die Formularerkennungs-API eingehender zu erkunden.

> [!div class="nextstepaction"]
> [Referenzdokumentation zur Rest-API](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/AnalyzeLayoutAsync)
