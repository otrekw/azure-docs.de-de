---
title: include file
description: include file
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.date: 02/14/2020
ms.topic: include
ms.custom: include file
ms.author: diberry
ms.openlocfilehash: 9c15e4217c5331346c5a95329bae7e2a4f0e0841
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/21/2020
ms.locfileid: "81732080"
---
Verwenden Sie die LUIS-Runtime-Clientbibliothek (Language Understanding) für Node.js für Folgendes:

* Vorhersage nach Slot
* Vorhersage nach Version

[Referenzdokumentation](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-runtime/?view=azure-node-latest) | [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/cognitiveservices/cognitiveservices-luis-runtime) | [Runtimepaket (NPM)](https://www.npmjs.com/package/@azure/cognitiveservices-luis-runtime) | [Beispiele](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/LUIS/luis_prediction.js)

## <a name="prerequisites"></a>Voraussetzungen

* Language Understanding-Runtimeressource: [Im Azure-Portal erstellen](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesLUISAllInOne)
* [Node.js](https://nodejs.org)
* Eine LUIS-App-ID: Verwenden Sie die öffentliche IoT-App-ID `df67dcdb-c37d-46af-88e1-8b97951ca1c2`. Die im Schnellstartcode verwendete Benutzerabfrage ist spezifisch für diese App.

## <a name="setting-up"></a>Einrichten

### <a name="get-your-language-understanding-luis-runtime-key"></a>Abrufen Ihres LUIS-Runtimeschlüssels (Language Understanding)

Erstellen Sie eine LUIS-Runtimeressource, um Ihren [Runtimeschlüssel](../luis-how-to-azure-subscription.md) zu erhalten. Den Schlüssel und den Endpunkt des Schlüssels benötigen Sie im nächsten Schritt.

[!INCLUDE [Set up environment variables for prediction quickstart](sdk-prediction-environment-variables.md)]

### <a name="create-a-new-javascript-nodejs-file"></a>Erstellen einer neuen JavaScript-Datei (Node.js)

Erstellen Sie in Ihrem bevorzugten Editor oder in Ihrer bevorzugten IDE eine neue JavaScript-Datei namens `luis_prediction.js`.

### <a name="install-the-npm-library-for-the-luis-runtime"></a>Installieren der NPM-Bibliothek für die LUIS-Runtime

Installieren Sie die Abhängigkeiten mithilfe des folgenden Befehls in Ihrem Anwendungsverzeichnis:

```console
npm install @azure/cognitiveservices-luis-runtime @azure/ms-rest-js
```

## <a name="object-model"></a>Objektmodell

Der LUIS-Erstellungsclient (Language Understanding) ist ein Objekt vom Typ [LUISAuthoringClient](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-runtime/luisruntimeclient?view=azure-node-latest), das sich bei Azure authentifiziert und Ihren Erstellungsschlüssel enthält.

Nach der Erstellung des Clients können Sie damit unter anderem auf folgende Funktionen zugreifen:

* [Vorhersage](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-runtime/predictionoperations?view=azure-node-latest#getslotprediction-string--string--predictionrequest--models-predictiongetslotpredictionoptionalparams-) nach Slot (`staging` oder `production`)
* [Vorhersage nach Version](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-runtime/predictionoperations?view=azure-node-latest#getversionprediction-string--string--predictionrequest--models-predictiongetversionpredictionoptionalparams-)

## <a name="code-examples"></a>Codebeispiele

In den bereitgestellten Codeausschnitten wird veranschaulicht, wie Sie die folgenden Vorgänge mit der LUIS-Vorhersageruntime-Clientbibliothek (Language Understanding) ausführen:

* [Vorhersage nach Slot](#get-prediction-from-runtime)

## <a name="add-the-dependencies"></a>Hinzufügen der Abhängigkeiten

Öffnen Sie über das Projektverzeichnis die Datei `luis_prediction.js` in Ihrem bevorzugten Editor oder in Ihrer bevorzugten IDE. Fügen Sie die folgenden Abhängigkeiten hinzu:

[!code-javascript [Dependencies](~/cognitive-services-quickstart-code/javascript/LUIS/luis_prediction.js?name=Dependencies)]

## <a name="authenticate-the-client"></a>Authentifizieren des Clients

1. Erstellen Sie Variablen für die benötigten LUIS-Informationen:

    Fügen Sie Variablen zur Verwaltung Ihres Vorhersageschlüssels hinzu, der aus einer Umgebungsvariablen namens `LUIS_RUNTIME_KEY` gepullt wird. Wenn Sie die Umgebungsvariable nach dem Start der Anwendung erstellen, müssen der Editor, die IDE oder die Shell, in denen sie ausgeführt wird, geschlossen und erneut geladen werden, damit der Zugriff auf die Variable möglich ist. Die Methoden werden später erstellt.

    Erstellen Sie eine Variable zum Speichern Ihres Ressourcennamens `LUIS_RUNTIME_ENDPOINT`.

    [!code-javascript [Azure resource variables](~/cognitive-services-quickstart-code/javascript/LUIS/luis_prediction.js?name=Variables)]

1. Erstellen Sie eine Variable für die App-ID als Umgebungsvariable mit dem Namen `LUIS_APP_ID`. Legen Sie die Umgebungsvariable auf die öffentliche IoT-App fest ( **`df67dcdb-c37d-46af-88e1-8b97951ca1c2`** ). Erstellen Sie eine Variable, um den veröffentlichten Slot `production` festzulegen.

    [!code-javascript [LUIS app variables](~/cognitive-services-quickstart-code/javascript/LUIS/luis_prediction.js?name=OtherVariables)]


1. Erstellen Sie ein Objekt vom Typ „msRest.ApiKeyCredentials“ mit Ihrem Schlüssel, und verwenden Sie es mit Ihrem Endpunkt, um ein Objekt vom Typ [LUIS.LUISRuntimeClient](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-runtime/luisruntimeclient?view=azure-node-latest) zu erstellen.

    [!code-javascript [LUIS Runtime client is required to access predictions for LUIS apps](~/cognitive-services-quickstart-code/javascript/LUIS/luis_prediction.js?name=AuthoringCreateClient)]

## <a name="get-prediction-from-runtime"></a>Abrufen der Vorhersage aus der Laufzeit

Fügen Sie die folgende Methode hinzu, um die Anforderung an die Vorhersagelaufzeit zu erstellen.

Die Benutzeräußerung ist Teil des Objekts [predictionRequest](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-runtime/predictionrequest?view=azure-node-latest).

Die Methode **[luisRuntimeClient.prediction.getSlotPrediction](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-runtime/predictionoperations?view=azure-node-latest#getslotprediction-string--string--predictionrequest--models-predictiongetslotpredictionoptionalparams-)** benötigt mehrere Parameter, um die Anforderung erfüllen zu können. Hierzu zählen beispielsweise die App-ID, der Slotname und das Vorhersageanforderungsobjekt. Die anderen Optionen, wie z. B. „Ausführlich“, „Alle Absichten anzeigen“ und „Protokollieren“, sind optional.

[!code-javascript [LUIS prediction request and response in Node.js NPM SDK](~/cognitive-services-quickstart-code/javascript/LUIS/luis_prediction.js?name=predict)]

## <a name="main-code-for-the-prediction"></a>Hauptcode für die Vorhersage

Verwenden Sie die folgende Hauptmethode, um die Variablen und Methoden zu verknüpfen, um die Vorhersage zu erhalten.

[!code-javascript [Main method and main call](~/cognitive-services-quickstart-code/javascript/LUIS/luis_prediction.js?name=Main)]

## <a name="run-the-application"></a>Ausführen der Anwendung

Führen Sie die Anwendung mit dem Befehl `node luis_prediction.js` aus dem Anwendungsverzeichnis aus.

```console
node luis_prediction.js
```

Im Vorhersageergebnis wird ein JSON-Objekt zurückgegeben:

```console
{
   "query":"turn on all lights",
   "prediction":{
      "topIntent":"HomeAutomation.TurnOn",
      "intents":{
         "HomeAutomation.TurnOn":{
            "score":0.5375382
         },
         "None":{
            "score":0.08687421
         },
         "HomeAutomation.TurnOff":{
            "score":0.0207554
         }
      },
      "entities":{
         "HomeAutomation.Operation":[
            "on"
         ],
         "$instance":{
            "HomeAutomation.Operation":[
               {
                  "type":"HomeAutomation.Operation",
                  "text":"on",
                  "startIndex":5,
                  "length":2,
                  "score":0.724984169,
                  "modelTypeId":-1,
                  "modelType":"Unknown",
                  "recognitionSources":[
                     "model"
                  ]
               }
            ]
         }
      }
   }
}
```


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie mit den Vorhersagen fertig sind, bereinigen Sie die Elemente aus dieser Schnellstartanleitung, indem Sie die Datei und deren Unterverzeichnisse löschen.
