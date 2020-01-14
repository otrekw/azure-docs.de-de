---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 11/14/2019
ms.author: pafarley
ms.openlocfilehash: 3c6059e131eadf1144fd189c47691b2352176745
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75446438"
---
## <a name="analyze-forms-for-key-value-pairs-and-tables"></a>Analysieren von Formularen für Schlüssel-Wert-Paare und Tabellen

Als Nächstes verwenden Sie Ihr gerade trainiertes Modell, um ein Dokument zu analysieren und Schlüssel-Werte-Paare und Tabellen daraus zu extrahieren. Rufen Sie die **[Analyze Form](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-preview/operations/AnalyzeWithCustomForm)** -API auf, indem Sie den folgenden Code in einem neuen Python-Skript ausführen. Nehmen Sie die folgenden Änderungen vor, bevor Sie das Skript ausführen:

1. Ersetzen Sie `<file path>` mit dem Dateipfad des Formulars (z.B. „C:\temp\file.pdf“). Hierbei kann es sich auch um die URL einer Remotedatei handeln. In dieser Schnellstartanleitung können Sie die Dateien im Ordner **Test** des [Beispieldatasets](https://go.microsoft.com/fwlink/?linkid=2090451) verwenden.
1. Ersetzen Sie `<model_id>` durch die Modell-ID, die Sie im vorherigen Abschnitt erhalten haben.
1. Ersetzen Sie `<endpoint>` durch den Endpunkt, den Sie mit Ihrem Abonnementschlüssel für die Formularerkennung erhalten haben. Sie finden ihn auf der Registerkarte **Übersicht** der Formularerkennungsressource.
1. Ersetzen Sie `<file type>` durch den Dateityp. Unterstützte Typen: `application/pdf`, `image/jpeg`, `image/png`, `image/tiff`.
1. Ersetzen Sie `<subscription key>` durch Ihren Abonnementschlüssel.

    ```python
    ########### Python Form Recognizer Async Analyze #############
    import json
    import time
    from requests import get, post
    
    # Endpoint URL
    endpoint = r"<endpoint>"
    apim_key = "<subsription key>"
    model_id = "<model_id>"
    post_url = endpoint + "/formrecognizer/v2.0-preview/custom/models/%s/analyze" % model_id
    source = r"<file path>"
    params = {
        "includeTextDetails": True
    }
    
    headers = {
        # Request headers
        'Content-Type': '<file type>',
        'Ocp-Apim-Subscription-Key': apim_key,
    }
    with open(source, "rb") as f:
        data_bytes = f.read()
    
    try:
        resp = post(url = post_url, data = data_bytes, headers = headers, params = params)
        if resp.status_code != 202:
            print("POST analyze failed:\n%s" % json.dumps(resp.json()))
            quit()
        print("POST analyze succeeded:\n%s" % resp.headers)
        get_url = resp.headers["operation-location"]
    except Exception as e:
        print("POST analyze failed:\n%s" % str(e))
        quit() 
    ```

1. Speichern Sie den Code in einer Datei mit der Erweiterung „.py“. Beispiel: *form-recognizer-analyze.py*.
1. Öffnen Sie ein Eingabeaufforderungsfenster.
1. Verwenden Sie an der Eingabeaufforderung den Befehl `python`, um das Beispiel auszuführen. Beispiel: `python form-recognizer-analyze.py`.

Wenn Sie die **Analyze Form**-API aufrufen, erhalten Sie eine `201 (Success)`-Antwort mit einem **Operation-Location**-Header. Der Wert dieses Headers ist eine ID, mit der Sie die Ergebnisse des Analysevorgangs nachverfolgen können. Das Skript oben gibt den Wert dieses Headers an die Konsole aus.

## <a name="get-the-analyze-results"></a>Abrufen der Analyseergebnisse

Fügen Sie am Ende des Python-Skripts den folgenden Code hinzu. Hierbei wird der ID-Wert aus dem vorherigen Aufruf in einem neuen API-Aufruf verwendet, mit dem die Analyseergebnisse abgerufen werden. Der **Analyze Form**-Vorgang ist asynchron, sodass dieses Skript die API in regelmäßigen Abständen aufruft, bis die Ergebnisse vorliegen. Ein Intervall von mindestens einer Sekunde wird empfohlen.

```python 
n_tries = 15
n_try = 0
wait_sec = 5
max_wait_sec = 60
while n_try < n_tries:
    try:
        resp = get(url = get_url, headers = {"Ocp-Apim-Subscription-Key": apim_key})
        resp_json = resp.json()
        if resp.status_code != 200:
            print("GET analyze results failed:\n%s" % json.dumps(resp_json))
            quit()
        status = resp_json["status"]
        if status == "succeeded":
            print("Analysis succeeded:\n%s" % json.dumps(resp_json))
            quit()
        if status == "failed":
            print("Analysis failed:\n%s" % json.dumps(resp_json))
            quit()
        # Analysis still running. Wait and retry.
        time.sleep(wait_sec)
        n_try += 1
        wait_sec = min(2*wait_sec, max_wait_sec)     
    except Exception as e:
        msg = "GET analyze results failed:\n%s" % str(e)
        print(msg)
        quit()
print("Analyze operation did not complete within the allocated time.")
```
