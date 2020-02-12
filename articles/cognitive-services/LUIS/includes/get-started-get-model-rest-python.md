---
title: Abrufen eines Modells mit einem REST-Aufruf in C#
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 01/31/2020
ms.author: diberry
ms.openlocfilehash: a21834cec456ded82c15dc916cc4991a196ea22c
ms.sourcegitcommit: 42517355cc32890b1686de996c7913c98634e348
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/02/2020
ms.locfileid: "76966760"
---
## <a name="prerequisites"></a>Voraussetzungen

* Azure Language Understanding: Ressourcenschlüssel mit 32 Zeichen und Endpunkt-URL für die Erstellung. Führen Sie die Erstellung mit dem [Azure-Portal](../luis-how-to-azure-subscription.md#create-resources-in-the-azure-portal) oder der [Azure CLI](../luis-how-to-azure-subscription.md#create-resources-in-azure-cli) durch.
* Importieren Sie die App [TravelAgent](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/quickstarts/change-model/TravelAgent.json) aus dem GitHub-Repository „cognitive-services-language-understanding“.
* Die LUIS-Anwendungs-ID für die importierte TravelAgent-App. Die Anwendungs-ID wird auf dem Anwendungsdashboard angezeigt.
* Die Versions-ID der Anwendung, die die Äußerungen empfängt. Die Standard-ID lautet „0.1“.
* [Python 3.6](https://www.python.org/downloads/) oder höher
* [Visual Studio Code](https://code.visualstudio.com/)

## <a name="example-utterances-json-file"></a>JSON-Datei mit Beispieläußerungen

[!INCLUDE [Quickstart explanation of example utterance JSON file](get-started-get-model-json-example-utterances.md)]


## <a name="change-model-programmatically"></a>Programmgesteuertes Ändern des Modells

Verwenden Sie Go, um der Anwendung eine [API](https://aka.ms/luis-apim-v3-authoring) für eine durch maschinelles Lernen erworbene Entität hinzuzufügen.

1. Erstellen Sie eine neue Datei mit dem Namen `model.py`. Fügen Sie den folgenden Code hinzu:

    ```python
    ########### Python 3.6 #############
    import requests

    # 32 character Authoring key
    LUIS_authoringKey  = "YOUR-KEY"

    LUIS_APP_ID = "YOUR-APP-ID"

    # Authoring endpoint, example: your-resource-name.api.cognitive.microsoft.com
    LUIS_ENDPOINT = "YOUR-ENDPOINT"

    # The version number of your LUIS app
    LUIS_APP_VERSION = "0.1"

    URI_AddUtterances = f'https://{LUIS_ENDPOINT}/luis/authoring/v3.0-preview/apps/{LUIS_APP_ID}/versions/{LUIS_APP_VERSION}/examples'
    URI_Train = f'https://{LUIS_ENDPOINT}/luis/authoring/v3.0-preview/apps/{LUIS_APP_ID}/versions/{LUIS_APP_VERSION}/train'

    HEADERS = {'Ocp-Apim-Subscription-Key': LUIS_authoringKey}

    def addUtterances():
        r = requests.post(URI_AddUtterances,headers=HEADERS)
        print(r.json())

    def train():
        r = requests.post(URI_Train,headers=HEADERS)
        print(r.json())

    def trainStatus():
        r = requests.get(URI_Train,headers=HEADERS)
        print(r.json())

    addUtterances()
    train()
    trainStatus()
    ```

1. Ersetzen Sie die Werte, die mit `YOUR-` beginnen, durch Ihre eigenen Werte.

    |Information|Zweck|
    |--|--|
    |`YOUR-KEY`|Ihr Erstellungsschlüssel mit 32 Zeichen.|
    |`YOUR-ENDPOINT`| Ihr URL-Endpunkt für die Erstellung. Beispiel: `replace-with-your-resource-name.api.cognitive.microsoft.com`. Sie haben Ihren Ressourcennamen festgelegt, als Sie die Ressource erstellt haben.|
    |`YOUR-APP-ID`| Ihre LUIS-App-ID. |

    Zugewiesene Schlüssel und Ressourcen werden im LUIS-Portal auf der Seite **Azure-Ressourcen** im Abschnitt „Verwalten“ angezeigt. Die App-ID wird auf der Seite **Anwendungseinstellungen** ebenfalls im Abschnitt „Verwalten“ angezeigt.

1. Geben Sie in dem Verzeichnis, in dem Sie die Datei erstellt haben, an einer Eingabeaufforderung den folgenden Befehl ein, um die Datei auszuführen:

    ```console
    python model.py
    ```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Datei aus dem Dateisystem, nachdem Sie diese Schnellstartanleitung durchgearbeitet haben.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Bewährte Methoden für eine App](../luis-concept-best-practices.md)
