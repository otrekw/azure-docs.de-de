---
title: Beispiel einer benutzerdefinierten Qualifikation (Python)
titleSuffix: Azure Cognitive Search
description: 'Für Python-Entwickler: Erlernen Sie die Tools und Techniken für die Erstellung einer benutzerdefinierten Qualifikation mit Azure Functions und Visual Studio. Benutzerdefinierte Qualifikationen enthalten benutzerdefinierte Modelle oder Logik, die Sie einer mit KI angereicherten Indizierungspipeline in Azure Cognitive Search hinzufügen können.'
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 01/15/2020
ms.custom: devx-track-python
ms.openlocfilehash: aac184b9dc7eae8afcf796376d2f7dca684b4367
ms.sourcegitcommit: 7fe8df79526a0067be4651ce6fa96fa9d4f21355
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2020
ms.locfileid: "87852785"
---
# <a name="example-create-a-custom-skill-using-python"></a>Beispiel: Erstellen einer benutzerdefinierten Qualifikation mit Python

In diesem Beispiel für ein Azure Cognitive Search-Skillset lernen Sie, wie Sie mit Python- und Visual Studio-Code eine benutzerdefinierte Web-API-Qualifikation erstellen können. Das Beispiel verwendet eine [Azure Function](https://azure.microsoft.com/services/functions/), die die [Schnittstelle der benutzerdefinierten Qualifikation](cognitive-search-custom-skill-interface.md) implementiert.

Die benutzerdefinierte Qualifikation ist vom Design her einfach (sie verknüpft zwei Zeichenfolgen), sodass Sie sich auf die Tools und Technologien konzentrieren können, die für die Entwicklung benutzerdefinierter Qualifikationen in Python verwendet werden. Sobald Sie mit einer einfachen Qualifikation erfolgreich sind, können Sie sich an komplexere Szenarien wagen.

## <a name="prerequisites"></a>Voraussetzungen

+ Unter [Schnittstelle für benutzerdefinierte Qualifikationen](cognitive-search-custom-skill-interface.md) finden Sie eine Einführung in die Eingabe/Ausgabe-Schnittstelle, die eine benutzerdefinierte Qualifikation implementieren sollte.

+ Einrichten Ihrer Umgebung. Wir sind [diesem Tutorial von Anfang bis Ende](https://docs.microsoft.com/azure/python/tutorial-vs-code-serverless-python-01) gefolgt, um die serverlose Azure Function unter Verwendung von Visual Studio-Code und Python-Erweiterungen einzurichten. Das Tutorial führt Sie durch die Installation der folgenden Tools und Komponenten: 

  + [Python 3.75](https://www.python.org/downloads/release/python-375/)
  + [Visual Studio Code](https://code.visualstudio.com/)
  + [Python-Erweiterung für Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-python.python)
  + [Azure Functions Core Tools](https://docs.microsoft.com/azure/azure-functions/functions-run-local#v2)
  + [Azure Functions-Erweiterung für Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)

## <a name="create-an-azure-function"></a>Erstellen einer Azure Function

In diesem Beispiel wird eine Azure Function verwendet, um das Konzept des Hostings einer Web-API zu demonstrieren, aber es sind auch andere Ansätze möglich. Solange Sie die [Schnittstellenanforderungen für einen kognitiven Skill](cognitive-search-custom-skill-interface.md) erfüllen, ist der gewählte Ansatz unerheblich. Mit Azure Functions ist das Erstellen eines benutzerdefinierten Skills jedoch einfacher.

### <a name="create-a-function-app"></a>Erstellen einer Funktionen-App

Mit der Azure Functions-Projektvorlage in Visual Studio Code wird ein Projekt erstellt, das in einer Funktions-App in Azure veröffentlicht werden kann. Sie können mit einer Funktionen-App Funktionen zu logischen Einheiten gruppieren, um die Verwaltung, Bereitstellung und Freigabe von Ressourcen zu ermöglichen.

1. Drücken Sie in Visual Studio Code die F1-Taste, um die Befehlspalette zu öffnen. Suchen Sie in der Befehlspalette den Befehl `Azure Functions: Create new project...`, und wählen Sie ihn aus.

1. Wählen Sie einen Verzeichnisspeicherort für Ihren Projektarbeitsbereich und anschließend **Auswählen** aus.

    > [!NOTE]
    > Diese Schritte sollten außerhalb eines Arbeitsbereichs ausgeführt werden. Wählen Sie aus diesem Grund keinen Projektordner aus, der Teil eines Arbeitsbereichs ist.

1. Auswählen einer Sprache für Ihr Funktions-App-Projekt. Wählen Sie für dieses Tutorial die Option **Python** aus.
1. Wählen Sie die Python-Version aus (Version 3.7.5 wird von Azure Functions unterstützt).
1. Wählen Sie die Vorlage für die erste Funktion Ihres Projekts aus. Wählen Sie **HTTP-Trigger** aus, um eine durch HTTP ausgelöste Funktion in der neuen Funktions-App zu erstellen.
1. Angeben eines Funktionsnamens. Verwenden Sie in diesem Fall **Concatenator**. 
1. Wählen Sie **Funktion** als Autorisierungsstufe aus. Das bedeutet, dass wir einen [Funktionsschlüssel](../azure-functions/functions-bindings-http-webhook-trigger.md#authorization-keys) bereitstellen, um den HTTP-Endpunkt der Funktion aufzurufen. 
1. Auswählen, wie Sie Ihr Projekt öffnen möchten. Wählen Sie für diesen Schritt **Zu Arbeitsbereich hinzufügen** aus, um die Funktions-App im aktuellen Arbeitsbereich zu erstellen.

Visual Studio Code erstellt das Funktions-App-Projekt in einem neuen Arbeitsbereich. Dieses Projekt enthält die Konfigurationsdateien [host.json](../azure-functions/functions-host-json.md) und [local.settings.json](../azure-functions/functions-run-local.md#local-settings-file) sowie sprachspezifische Projektdateien. 

Eine neue HTTP-gesteuerte Funktion wird ebenfalls im **Concatenator**-Ordner des Funktions-App-Projekts erstellt. Darin ist eine Datei namens „\_\_init__.py“ mit diesem Inhalt enthalten:

```py
import logging

import azure.functions as func


def main(req: func.HttpRequest) -> func.HttpResponse:
    logging.info('Python HTTP trigger function processed a request.')

    name = req.params.get('name')
    if not name:
        try:
            req_body = req.get_json()
        except ValueError:
            pass
        else:
            name = req_body.get('name')

    if name:
        return func.HttpResponse(f"Hello {name}!")
    else:
        return func.HttpResponse(
             "Please pass a name on the query string or in the request body",
             status_code=400
        )

```

Nun wollen wir diesen Code so entsprechend der [Schnittstelle für benutzerdefinierte Qualifikationen](cognitive-search-custom-skill-interface.md)) modifizieren. Ändern Sie den Code mit dem folgenden Inhalt:

```py
import logging
import azure.functions as func
import json

def main(req: func.HttpRequest) -> func.HttpResponse:
    logging.info('Python HTTP trigger function processed a request.')

    try:
        body = json.dumps(req.get_json())
    except ValueError:
        return func.HttpResponse(
             "Invalid body",
             status_code=400
        )
    
    if body:
        result = compose_response(body)
        return func.HttpResponse(result, mimetype="application/json")
    else:
        return func.HttpResponse(
             "Invalid body",
             status_code=400
        )


def compose_response(json_data):
    values = json.loads(json_data)['values']
    
    # Prepare the Output before the loop
    results = {}
    results["values"] = []
    
    for value in values:
        output_record = transform_value(value)
        if output_record != None:
            results["values"].append(output_record)
    return json.dumps(results, ensure_ascii=False)

## Perform an operation on a record
def transform_value(value):
    try:
        recordId = value['recordId']
    except AssertionError  as error:
        return None

    # Validate the inputs
    try:         
        assert ('data' in value), "'data' field is required."
        data = value['data']        
        assert ('text1' in data), "'text1' field is required in 'data' object."
        assert ('text2' in data), "'text2' field is required in 'data' object."
    except AssertionError  as error:
        return (
            {
            "recordId": recordId,
            "errors": [ { "message": "Error:" + error.args[0] }   ]       
            })

    try:                
        concatenated_string = value['data']['text1'] + " " + value['data']['text2']  
        # Here you could do something more interesting with the inputs

    except:
        return (
            {
            "recordId": recordId,
            "errors": [ { "message": "Could not complete operation for record." }   ]       
            })

    return ({
            "recordId": recordId,
            "data": {
                "text": concatenated_string
                    }
            })
```

Die Methode **transform_value** führt eine Operation für einen einzelnen Datensatz aus. Sie können die Methode an Ihre spezifischen Bedürfnisse anpassen. Vergessen Sie nicht, die erforderliche Eingabevalidierung durchzuführen und alle Fehler und Warnungen zurückzugeben, die erzeugt werden, wenn der Vorgang für den Datensatz nicht abgeschlossen werden konnte.

### <a name="debug-your-code-locally"></a>Lokales Debuggen Ihres Codes

Mit Visual Studio Code lässt sich der Code leicht debuggen. Drücken Sie F5, oder wählen Sie im Menü **Debuggen** die Option **Debuggen starten** aus.

Sie können beliebige Breakpoints im Code setzen, indem Sie auf der betreffenden Zeile „F9“ drücken.

Sobald Sie das Debuggen gestartet haben, wird Ihre Funktion lokal ausgeführt. Sie können ein Tool wie Postman oder Fiddler verwenden, um die Anforderung an „localhost“ zu stellen. Notieren Sie die Position Ihres lokalen Endpunkts im Terminalfenster. 

## <a name="publish-your-function"></a>Veröffentlichen der Funktion

Wenn Sie mit dem Funktionsverhalten zufrieden sind, können Sie sie veröffentlichen.

1. Drücken Sie in Visual Studio Code die F1-Taste, um die Befehlspalette zu öffnen. Suchen Sie in der Befehlspalette nach **In Funktions-App bereitstellen...** , und führen Sie den Befehl aus. 

1. Wählen Sie das Azure-Abonnement aus, in dem Sie Ihre Anwendung bereitstellen möchten.

1. Wählen Sie **+ Neue Funktions-App in Azure erstellen** aus.

1. Geben Sie einen global eindeutigen Namen für die Funktions-App ein.

1. Wählen Sie die Python-Version aus (Python 3.7.x kann für diese Funktion verwendet werden).

1. Wählen Sie einen Standort für die neue Ressource aus (z. B. USA, Westen 2).

Zu diesem Zeitpunkt werden in Ihrem Azure-Abonnement die notwendigen Ressourcen geschaffen, um die neue Azure Function auf Azure zu hosten. Warten Sie, bis die Bereitstellung abgeschlossen ist. Das Ausgabefenster zeigt den Status des Bereitstellungsvorgangs an.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu **Alle Ressourcen**, und suchen Sie nach der von Ihnen veröffentlichten Funktion anhand ihres Namens. Wenn Sie sie **Concatenator** genannt haben, wählen Sie die Ressource aus.

1. Klicken Sie auf die Schaltfläche **</> Funktions-URL abrufen** aus. Dadurch können Sie die URL kopieren, um die Funktion aufzurufen.

## <a name="test-the-function-in-azure"></a>Testen der Funktion in Azure

Nachdem Sie nun den Standardhostschlüssel haben, testen Sie Ihre Funktion wie folgt:

```http
POST [Function URL you copied above]
```

### <a name="request-body"></a>Anforderungstext
```json
{
    "values": [
        {
            "recordId": "e1",
            "data":
            {
                "text1":  "Hello",
                "text2":  "World"
            }
        },
        {
            "recordId": "e2",
            "data": "This is an invalid input"
        }
    ]
}
```

Dieses Beispiel sollte das gleiche Ergebnis liefern wie die Ausführung der Funktion in der lokalen Umgebung.

## <a name="connect-to-your-pipeline"></a>Herstellen einer Verbindung mit Ihrer Pipeline

Nun, da Sie über einen neuen benutzerdefinierten Skill verfügen, können Sie diesen zu Ihrem Skillset hinzufügen. Das folgende Beispiel zeigt, wie Sie die Qualifikation aufrufen, den Titel und den Autor des Dokuments in einem einzigen Feld namens „merged_title_author“ zu verketten. Ersetzen Sie `[your-function-url-here]` durch die URL Ihrer neuen Azure Function.

```json
{
    "skills": [
      "[... your existing skills remain here]",  
      {
        "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
        "description": "Our new search custom skill",
        "uri": "https://[your-function-url-here]",        
          "context": "/document/merged_content/organizations/*",
          "inputs": [
            {
              "name": "text1",
              "source": "/document/metadata_title"
            },
            {
              "name": "text2",
              "source": "/document/metadata_author"
            },
          ],
          "outputs": [
            {
              "name": "text",
              "targetName": "merged_title_author"
            }
          ]
      }
  ]
}
```

## <a name="next-steps"></a>Nächste Schritte
Glückwunsch! Sie haben Ihre erste benutzerdefinierte Qualifikation erstellt. Nun können Sie Ihre eigene benutzerdefinierte Funktionalität nach dem gleichen Muster hinzufügen. Klicken Sie auf die folgenden Links, um weitere Informationen zu erhalten.

+ [Power Skills: ein Repository benutzerdefinierter Skills](https://github.com/Azure-Samples/azure-search-power-skills)
+ [Hinzufügen einer benutzerdefinierten Qualifikation zu einer KI-Anreicherungspipeline](cognitive-search-custom-skill-interface.md)
+ [Definieren eines Skillsets](cognitive-search-defining-skillset.md)
+ [Erstellen eines Skillsets (REST)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
+ [Zuordnen angereicherter Felder](cognitive-search-output-field-mapping.md)
