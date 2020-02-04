---
title: include file
description: include file
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: luis
ms.topic: include
ms.custom: include file
ms.date: 01/23/2020
ms.author: diberry
ms.openlocfilehash: 31280b1efa3db22e8008f401a25ca627dda4153f
ms.sourcegitcommit: 984c5b53851be35c7c3148dcd4dfd2a93cebe49f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2020
ms.locfileid: "76772390"
---
Verwenden Sie die LUIS-Vorhersageclientbibliothek (Language Understanding) für Python für Folgendes:

* Abrufen einer Vorhersage nach Slot
* Abrufen einer Vorhersage nach Version

[Referenzdokumentation](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/index?view=azure-python) | [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-language-luis/azure/cognitiveservices/language/luis) | [Vorhersageruntimepaket (PyPi)](https://pypi.org/project/azure-cognitiveservices-language-luis/) | [Beispiele](https://github.com/Azure-Samples/cognitive-services-quickstart-code/tree/master/python/LUIS)

## <a name="prerequisites"></a>Voraussetzungen

* Konto für das LUIS-Portal (Language Understanding): [Erstellen Sie ein kostenloses Konto.](https://www.luis.ai)
* [Python 3.x](https://www.python.org/)

## <a name="setting-up"></a>Einrichten

### <a name="get-your-language-understanding-luis-runtime-key"></a>Abrufen Ihres LUIS-Runtimeschlüssels (Language Understanding)

Erstellen Sie eine LUIS-Runtimeressource, um Ihren [Runtimeschlüssel](../luis-how-to-azure-subscription.md) zu erhalten. Den Schlüssel und den Endpunkt des Schlüssels benötigen Sie im nächsten Schritt.

[!INCLUDE [Set up environment variables for prediction quickstart](sdk-prediction-environment-variables.md)]

### <a name="create-a-new-python-file"></a>Erstellen einer neuen Python-Datei

Erstellen Sie in Ihrem bevorzugten Editor oder in Ihrer bevorzugten IDE eine neue Python-Datei namens `prediction_quickstart.py`.

### <a name="install-the-sdk"></a>Installieren des SDKs

Installieren Sie innerhalb des Anwendungsverzeichnisses die LUIS-Vorhersageruntime-Clientbibliothek (Language Understanding) für Python mithilfe des folgenden Befehls:

```python
python -m pip install azure-cognitiveservices-language-luis
```

## <a name="object-model"></a>Objektmodell

Der LUIS-Vorhersagelaufzeit-Client (Language Understanding) ist ein Objekt vom Typ [LUISRuntimeClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.luisruntimeclient?view=azure-python), das sich bei Azure authentifiziert und Ihren Ressourcenschlüssel enthält.

Nach der Erstellung des Clients können Sie damit unter anderem auf folgende Funktionen zugreifen:

* Vorhersage nach [Staging- oder Produktionsslot](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.operations.predictionoperations?view=azure-python#get-slot-prediction-app-id--slot-name--prediction-request--verbose-none--show-all-intents-none--log-none--custom-headers-none--raw-false----operation-config-)
* Vorhersage nach [Version](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.operations.predictionoperations?view=azure-python#get-version-prediction-app-id--version-id--prediction-request--verbose-none--show-all-intents-none--log-none--custom-headers-none--raw-false----operation-config-)

## <a name="code-examples"></a>Codebeispiele

In den bereitgestellten Codeausschnitten wird veranschaulicht, wie Sie die folgenden Vorgänge mit der LUIS-Vorhersageruntime-Clientbibliothek (Language Understanding) für Python ausführen:

* [Vorhersage nach Slot](#get-prediction-from-runtime)

## <a name="add-the-dependencies"></a>Hinzufügen der Abhängigkeiten

Öffnen Sie über das Projektverzeichnis die Datei `prediction_quickstart.py` in Ihrem bevorzugten Editor oder in Ihrer bevorzugten IDE. Fügen Sie die folgenden Abhängigkeiten hinzu:

[!code-python[Dependency statements](~/cognitive-services-quickstart-code/python/LUIS/prediction_quickstart.py?name=Dependencies)]

## <a name="authenticate-the-client"></a>Authentifizieren des Clients

1. Erstellen Sie Variablen für die benötigten LUIS-Informationen:

    Fügen Sie Variablen zur Verwaltung Ihres Vorhersageschlüssels hinzu, der aus einer Umgebungsvariablen namens `LUIS_RUNTIME_KEY` gepullt wird. Wenn Sie die Umgebungsvariable nach dem Start der Anwendung erstellen, müssen der Editor, die IDE oder die Shell, in denen sie ausgeführt wird, geschlossen und erneut geladen werden, damit der Zugriff auf die Variable möglich ist. Die Methoden werden später erstellt.

    Erstellen Sie eine Variable zum Speichern Ihres Ressourcennamens `LUIS_RUNTIME_ENDPOINT`.

    [!code-python[Dependency statements](~/cognitive-services-quickstart-code/python/LUIS/prediction_quickstart.py?name=AuthorizationVariables)]

1. Erstellen Sie eine Variable für die App-ID als Umgebungsvariable mit dem Namen `LUIS_APP_ID`. Legen Sie die Umgebungsvariable auf die öffentliche IoT-App fest ( **`df67dcdb-c37d-46af-88e1-8b97951ca1c2`** ). Erstellen Sie eine Variable, um den veröffentlichten Slot `production` festzulegen.

    [!code-python[Dependency statements](~/cognitive-services-quickstart-code/python/LUIS/prediction_quickstart.py?name=OtherVariables)]


1. Erstellen Sie ein Anmeldeinformationsobjekt mit Ihrem Schlüssel, und verwenden Sie es mit Ihrem Endpunkt, um ein Objekt vom Typ [LUISRuntimeClientConfiguration]https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.luisruntimeclientconfiguration?view=azure-python() zu erstellen.

    [!code-python[Dependency statements](~/cognitive-services-quickstart-code/python/LUIS/prediction_quickstart.py?name=Client)]

## <a name="get-prediction-from-runtime"></a>Abrufen der Vorhersage aus der Laufzeit

Fügen Sie die folgende Methode hinzu, um die Anforderung an die Vorhersagelaufzeit zu erstellen.

Die Benutzeräußerung ist Teil des Objekts [prediction_request](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.models.predictionrequest?view=azure-python).

Die Methode **[get_slot_prediction](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.operations.predictionoperations?view=azure-python#get-slot-prediction-app-id--slot-name--prediction-request--verbose-none--show-all-intents-none--log-none--custom-headers-none--raw-false----operation-config-)** benötigt mehrere Parameter, um die Anforderung erfüllen zu können. Hierzu zählen beispielsweise die App-ID, der Slotname und das Vorhersageanforderungsobjekt. Die anderen Optionen, wie z. B. „Ausführlich“, „Alle Absichten anzeigen“ und „Protokollieren“, sind optional. Von der Anforderung wird ein Objekt vom Typ [PredictionResponse](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.models.predictionresponse?view=azure-python) zurückgegeben.

[!code-python[Dependency statements](~/cognitive-services-quickstart-code/python/LUIS/prediction_quickstart.py?name=predict)]

## <a name="main-code-for-the-prediction"></a>Hauptcode für die Vorhersage

Verwenden Sie die folgende Hauptmethode, um die Variablen und Methoden zu verknüpfen, um die Vorhersage zu erhalten.

```python
predict(luisAppID, luisSlotName)
```
## <a name="run-the-application"></a>Ausführen der Anwendung

Führen Sie die Anwendung mit dem Befehl `python prediction_quickstart.py` aus dem Anwendungsverzeichnis aus.

```console
python prediction_quickstart.py
```

Die Ausgabe wird in der Schnellstartkonsole angezeigt:

```console
Top intent: HomeAutomation.TurnOn
Sentiment: None
Intents:
        "HomeAutomation.TurnOn"
Entities: {'HomeAutomation.Operation': ['on']}
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie mit den Vorhersagen fertig sind, bereinigen Sie die Elemente aus dieser Schnellstartanleitung, indem Sie die Datei und deren Unterverzeichnisse löschen.
