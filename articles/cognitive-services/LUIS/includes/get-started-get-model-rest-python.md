---
title: Abrufen eines Modells mit einem REST-Aufruf in C#
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 02/14/2020
ms.author: diberry
ms.openlocfilehash: 4d8da7d2bc51c4fc4ebc8d71f230f24f20b3aa24
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "77368477"
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

1. Erstellen Sie eine neue Datei mit dem Namen `model.py`. Fügen Sie den folgenden Code hinzu:

    [!code-python[Add example utterances to Language Understanding in python](~/samples-luis/documentation-samples/quickstarts/change-model/python/3.x/add-utterances-3-6.py)]

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
