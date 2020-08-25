---
title: include file
description: include file
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.date: 08/13/2020
ms.topic: include
ms.custom: include file
ms.author: diberry
ms.openlocfilehash: eac827097f892de81bc5820994f3e2a4e0696dcb
ms.sourcegitcommit: c293217e2d829b752771dab52b96529a5442a190
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/15/2020
ms.locfileid: "88246102"
---
Verwenden Sie die LUIS-Clientbibliotheken (Language Understanding) für Python für Folgendes:

* Erstellen einer App
* Hinzufügen einer Absicht (eine durch maschinelles Lernen erworbene Entität) mit einer Beispieläußerung
* Trainieren und Veröffentlichen der App
* Abfragen der Vorhersagelaufzeit

[Referenzdokumentation](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/index?view=azure-python) | Quellcode für die [Erstellungs-](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-language-luis/azure/cognitiveservices/language/luis/authoring) und [Vorhersagebibliothek](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-language-luis/azure/cognitiveservices/language/luis/runtime) | [Paket (Pypi)](https://pypi.org/project/azure-cognitiveservices-language-luis/) | [Beispiele](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/LUIS/sdk-3x/authoring_and_predict.py)

## <a name="prerequisites"></a>Voraussetzungen

* Die aktuelle Version von [Python 3.x](https://www.python.org/). 
* Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/cognitive-services)
* Sobald Sie über Ihr Azure-Abonnement verfügen, [erstellen Sie eine Language Understanding-Erstellungsressource](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesLUISAllInOne) im Azure-Portal, um an Ihren Schlüssel und Ihren Endpunkt zu gelangen. Warten Sie ihre Bereitstellung ab, und klicken Sie auf die Schaltfläche **Zu Ressource wechseln**.
    * Sie benötigen den Schlüssel und Endpunkt der von Ihnen [erstellten](../luis-how-to-azure-subscription.md#create-luis-resources-in-azure-portal) Ressource, um Ihre Anwendung mit der Language Understanding-Erstellung zu verbinden. Der Schlüssel und der Endpunkt werden weiter unten in der Schnellstartanleitung in den Code eingefügt. Sie können den kostenlosen Tarif (`F0`) verwenden, um den Dienst auszuprobieren.

## <a name="setting-up"></a>Einrichten

### <a name="create-a-new-python-application"></a>Erstellen einer neuen Python-Anwendung

1. Erstellen Sie in einem Konsolenfenster ein neues Verzeichnis für Ihre Anwendung, und wechseln Sie in dieses Verzeichnis.

    ```console
    mkdir quickstart-sdk && cd quickstart-sdk
    ```

1. Erstellen Sie eine Datei mit dem Namen `authoring_and_predict.py` für Ihren Python-Code. 

    ```console
    touch authoring_and_predict.py
    ```

### <a name="install-the-client-library-with-pip"></a>Installieren der Clientbibliothek mit Pip

Installieren Sie innerhalb des Anwendungsverzeichnisses die LUIS-Clientbibliothek (Language Understanding) für Python mithilfe des folgenden Befehls:

```console
pip install azure-cognitiveservices-language-luis
```

## <a name="authoring-object-model"></a>Erstellungsobjektmodell

Der LUIS-Erstellungsclient (Language Understanding) ist ein Objekt vom Typ [LUISAuthoringClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.luisauthoringclient?view=azure-python), das sich bei Azure authentifiziert und Ihren Erstellungsschlüssel enthält.

Nach der Erstellung des Clients können Sie damit unter anderem auf folgende Funktionen zugreifen:

* Apps: [Erstellen](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.appsoperations?view=azure-python#add-application-create-object--custom-headers-none--raw-false----operation-config-), [Löschen](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.appsoperations?view=azure-python#delete-app-id--force-false--custom-headers-none--raw-false----operation-config-), [Veröffentlichen](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.appsoperations?view=azure-python#publish-app-id--version-id-none--is-staging-false--custom-headers-none--raw-false----operation-config-)
* Beispieläußerungen: [Hinzufügen per Batch](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.examplesoperations?view=azure-python#batch-app-id--version-id--example-label-object-array--custom-headers-none--raw-false----operation-config-), [Löschen anhand der ID](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.examplesoperations?view=azure-python#delete-app-id--version-id--example-id--custom-headers-none--raw-false----operation-config-)
* Features: Verwalten von [Ausdruckslisten](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.featuresoperations?view=azure-python#add-phrase-list-app-id--version-id--phraselist-create-object--custom-headers-none--raw-false----operation-config-)
* Modell: Verwalten von [Absichten](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.modeloperations?view=azure-python#add-intent-app-id--version-id--name-none--custom-headers-none--raw-false----operation-config-) und Entitäten
* Muster: Verwalten von [Mustern](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.patternoperations?view=azure-python#add-pattern-app-id--version-id--pattern-none--intent-none--custom-headers-none--raw-false----operation-config-)
* Trainieren: [Trainieren](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.trainoperations?view=azure-python#train-version-app-id--version-id--custom-headers-none--raw-false----operation-config-) der App und Abfragen des [Trainingsstatus](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.trainoperations?view=azure-python#get-status-app-id--version-id--custom-headers-none--raw-false----operation-config-)
* [Versionen:](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.versionsoperations?view=azure-python) Verwalten mittels Klonen, Exportieren und Löschen


## <a name="prediction-object-model"></a>Vorhersageobjektmodell

Der LUIS-Vorhersagelaufzeit-Client (Language Understanding) ist ein Objekt vom Typ [LUISRuntimeClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.luisruntimeclient?view=azure-python), das sich bei Azure authentifiziert und Ihren Ressourcenschlüssel enthält.

Nach der Erstellung des Clients können Sie damit unter anderem auf folgende Funktionen zugreifen:

* Vorhersage nach [Staging- oder Produktionsslot](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.operations.predictionoperations?view=azure-python#get-slot-prediction-app-id--slot-name--prediction-request--verbose-none--show-all-intents-none--log-none--custom-headers-none--raw-false----operation-config-)
* Vorhersage durch [Version](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.operations.predictionoperations?view=azure-python#get-version-prediction-app-id--version-id--prediction-request--verbose-none--show-all-intents-none--log-none--custom-headers-none--raw-false----operation-config-)

[!INCLUDE [Bookmark links to same article](sdk-code-examples.md)]

## <a name="add-the-dependencies"></a>Hinzufügen der Abhängigkeiten

Fügen Sie der Python-Datei die Clientbibliotheken hinzu. 

[!code-python[Add python libraries to code file](~/cognitive-services-quickstart-code/python/LUIS/sdk-3x/authoring_and_predict.py?name=Dependencies)]


## <a name="add-boilerplate-code"></a>Hinzufügen von Codebausteinen 

1. Fügen Sie die `quickstart`-Methode und Ihren Aufruf hinzu. Diese Methode enthält den Großteil des restlichen Codes. Diese Methode wird am Ende der Datei aufgerufen. 

    ```python
    def quickstart(): 

        # add calls here, remember to indent properly
    
    quickstart()
    ```

1. Fügen Sie den verbleibenden Code in der quickstart-Methode hinzu, sofern nicht anders angegeben. 

## <a name="create-variables-for-the-app"></a>Erstellen von Variablen für die App

1. Erstellen Sie Variablen für Ihren Erstellungsschlüssel und Ihre Ressourcennamen.

    [!code-python[Variables you need to change](~/cognitive-services-quickstart-code/python/LUIS/sdk-3x/authoring_and_predict.py?name=VariablesYouChange)]

1. Erstellen Sie Variablen für Ihre Endpunkte, den App-Namen, die Version und den Namen der Absicht.

    [!code-python[Variables you don't need to change](~/cognitive-services-quickstart-code/python/LUIS/sdk-3x/authoring_and_predict.py?name=VariablesYouDontNeedToChangeChange)]

## <a name="authenticate-the-client"></a>Authentifizieren des Clients

Erstellen Sie ein Objekt vom Typ [CognitiveServicesCredentials](https://docs.microsoft.com/python/api/msrest/msrest.authentication.cognitiveservicescredentials?view=azure-python) mit Ihrem Schlüssel, und verwenden Sie es mit Ihrem Endpunkt, um ein Objekt vom Typ [LUISAuthoringClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.luisauthoringclient?view=azure-python) zu erstellen.

[!code-python[Create LUIS authoring client object](~/cognitive-services-quickstart-code/python/LUIS/sdk-3x/authoring_and_predict.py?name=AuthoringCreateClient)]

## <a name="create-a-luis-app"></a>Erstellen einer LUIS-App

Eine LUIS-App enthält das NLP-Modell (Natural Language Processing, Verarbeitung natürlicher Sprache) mit Absichten, Entitäten und Beispieläußerungen.

Erstellen Sie zum Generieren der App eine Methode vom Typ [add](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.appsoperations?view=azure-python#add-application-create-object--custom-headers-none--raw-false----operation-config-) für das [AppsOperation](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.appsoperations?view=azure-python)-Objekt. Name und Sprachkultur sind erforderliche Eigenschaften.

[!code-python[Create LUIS client app](~/cognitive-services-quickstart-code/python/LUIS/sdk-3x/authoring_and_predict.py?name=AuthoringCreateApplication)]


## <a name="create-intent-for-the-app"></a>Erstellen der Absicht für die App
Das primäre Objekt im Modell einer LUIS-App ist die Absicht. Die Absicht orientiert sich an einer Gruppierung der _Absichten_ von Benutzeräußerungen. Ein Benutzer kann eine Frage stellen oder etwas sagen, um eine bestimmte _beabsichtigte_ Antwort von einem Bot (oder von einer anderen Clientanwendung) zu erhalten. Beispiele für Absichten wären etwa das Buchen eines Flugs, das Einholen von Wetterinformationen für eine Zielstadt oder das Erfragen von Kontaktinformationen für den Kundendienst.

Verwenden Sie die Methode [model.add_intent](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.modeloperations?view=azure-python#add-intent-app-id--version-id--name-none--custom-headers-none--raw-false----operation-config-) mit dem Namen der eindeutigen Absicht, und übergeben Sie anschließend die App-ID, die Versions-ID und den Namen der neuen Absicht.

Der Wert `intentName` wird im Abschnitt [Erstellen von Variablen für die App](#create-variables-for-the-app) als `OrderPizzaIntent` hartcodiert und ist Teil der Variablen.

[!code-python[Create intent](~/cognitive-services-quickstart-code/python/LUIS/sdk-3x/authoring_and_predict.py?name=AddIntent)]

## <a name="create-entities-for-the-app"></a>Erstellen von Entitäten für die App

Entitäten sind zwar nicht erforderlich, aber in den meisten Apps vorhanden. Die Entität extrahiert Informationen aus der Benutzeräußerung, die zur Erfüllung der Absicht des Benutzers erforderlich sind. Es gibt mehrere Arten von [vordefinierten](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.modeloperations?view=azure-python#add-prebuilt-app-id--version-id--prebuilt-extractor-names--custom-headers-none--raw-false----operation-config-) und benutzerdefinierten Entitäten, die jeweils über ein eigenes DTO-Modell (Data Transformation Object) verfügen.  Zu den gängigen vordefinierten Entitäten, die Sie Ihrer App hinzufügen können, zählen etwa [number](../luis-reference-prebuilt-number.md), [datetimeV2](../luis-reference-prebuilt-datetimev2.md), [geographyV2](../luis-reference-prebuilt-geographyv2.md) und [ordinal](../luis-reference-prebuilt-ordinal.md).

Wichtig: Entitäten sind nicht mit einer Absicht gekennzeichnet. Sie gelten in der Regel für mehrere Absichten. Nur exemplarische Benutzeräußerungen sind für eine bestimmte, einzelne Absicht gekennzeichnet.

Erstellungsmethoden für Entitäten sind Teil der Klasse [ModelOperations](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.modeloperations?view=azure-python). Jeder Entitätstyp verfügt über ein eigenes DTO-Modell (Data Transformation Object).

Mit dem Code für die Entitätserstellung wird eine Machine Learning-Entität mit untergeordneten Entitäten und Features erstellt, die auf die untergeordneten Entitäten vom Typ `Quantity` angewendet werden.

:::image type="content" source="../media/quickstart-sdk/machine-learned-entity.png" alt-text="Mit dem Code für die Entitätserstellung wird eine Machine Learning-Entität mit untergeordneten Entitäten und Features erstellt, die auf die untergeordneten Entitäten vom Typ „Quantity“ angewendet werden.":::

[!code-python[Create entities](~/cognitive-services-quickstart-code/python/LUIS/sdk-3x/authoring_and_predict.py?name=AuthoringAddEntities)]

Fügen Sie die folgende Methode oberhalb der `quickstart`-Methode ein, um die ID der untergeordneten Entität vom Typ „Quantity“ zu ermitteln und dieser untergeordneten Entität die entsprechenden Features zuzuweisen.

[!code-python[Find subentity id](~/cognitive-services-quickstart-code/python/LUIS/sdk-3x/authoring_and_predict.py?name=AuthoringSortModelObject)]

## <a name="add-example-utterance-to-intent"></a>Hinzufügen einer Beispieläußerung zu einer Absicht

Um die Absicht einer Äußerung ermitteln und Entitäten extrahieren zu können, benötigt die App Beispiele für Äußerungen. Die Beispiele müssen auf eine bestimmte, einzelne Absicht ausgerichtet sein und alle benutzerdefinierten Entitäten markieren. Vordefinierte Entitäten müssen nicht markiert werden.

Fügen Sie Beispieläußerungen hinzu, indem Sie eine Liste mit Objekten vom Typ [ExampleLabelObject](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.models.examplelabelobject?view=azure-python) erstellen (jeweils ein Objekt pro Beispieläußerung). Jedes Beispiel muss alle Entitäten mit einem Wörterbuch mit Name-Wert-Paaren (Entitätsname und Entitätswert) markieren. Der Entitätswert muss exakt dem Vorkommen im Text der Beispieläußerung entsprechen.

:::image type="content" source="../media/quickstart-sdk/labeled-example-machine-learned-entity.png" alt-text="Der Entitätswert muss exakt dem Vorkommen im Text der Beispieläußerung entsprechen.":::

Rufen Sie [examples.add](https://docs.microsoft.com//python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.examplesoperations?view=azure-python#add-app-id--version-id--example-label-object--enable-nested-children-false--custom-headers-none--raw-false----operation-config-) mit der App-ID, der Versions-ID und dem Beispiel auf. 

[!code-python[Add example utterance to a specific intent](~/cognitive-services-quickstart-code/python/LUIS/sdk-3x/authoring_and_predict.py?name=AuthoringAddLabeledExamples)]

## <a name="train-the-app"></a>Trainieren der App

Nach Erstellung des Modells muss die LUIS-App für diese Version des Modells trainiert werden. Ein trainiertes Modell kann in einem [Container](../luis-container-howto.md) verwendet oder in den Staging- oder Produktslots [veröffentlicht](../luis-how-to-publish-app.md) werden.

Für die Methode [train.train_version](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.trainoperations?view=azure-python#train-version-app-id--version-id--custom-headers-none--raw-false----operation-config-) müssen die App-ID und die Versions-ID angegeben werden.

Bei sehr kleinen Modellen (wie etwa in dieser Schnellstartanleitung) dauert das Training nicht sehr lang. Bei Produktionsanwendungen muss das App-Training auch einen Abfrageaufruf für die Methode [get_status](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.trainoperations?view=azure-python#get-status-app-id--version-id--custom-headers-none--raw-false----operation-config-) enthalten, um zu bestimmen, wann bzw. ob das Training erfolgreich war. Die Antwort ist eine Liste mit Objekten vom Typ [ModelTrainingInfo](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.models.modeltraininginfo?view=azure-python), die jeweils einen separaten Status für die einzelnen Objekte besitzen. Alle Objekte müssen erfolgreich sein, damit das Training als abgeschlossen gilt.

[!code-python[Train the app's version](~/cognitive-services-quickstart-code/python/LUIS/sdk-3x/authoring_and_predict.py?name=TrainAppVersion)]

## <a name="publish-a-language-understanding-app"></a>Veröffentlichen einer Language Understanding-App

Veröffentlichen Sie die LUIS-App mithilfe der Methode [app.publish](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.appsoperations?view=azure-python#publish-app-id--version-id-none--is-staging-false--custom-headers-none--raw-false----operation-config-). Dadurch wird die aktuelle trainierte Version im angegebenen Slot am Endpunkt veröffentlicht. Ihre Clientanwendung verwendet diesen Endpunkt zum Senden von Benutzeräußerungen für die Absichtsvorhersage und die Entitätsextraktion.

[!code-python[Publish app to production slot](~/cognitive-services-quickstart-code/python/LUIS/sdk-3x/authoring_and_predict.py?name=PublishVersion)]

## <a name="authenticate-the-prediction-runtime-client"></a>Authentifizieren des Vorhersageruntimeclients

Verwenden Sie das Anmeldeinformationsobjekt mit Ihrem Schlüssel und mit Ihrem Endpunkt, um ein Objekt vom Typ [LUISRuntimeClientConfiguration](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.luisruntimeclientconfiguration?view=azure-python) zu erstellen.

[!INCLUDE [Caution about using authoring key](caution-authoring-key.md)]

[!code-python[Create LUIS runtime client object](~/cognitive-services-quickstart-code/python/LUIS/sdk-3x/authoring_and_predict.py?name=PredictionCreateClient)]

## <a name="get-prediction-from-runtime"></a>Abrufen der Vorhersage aus der Laufzeit

Fügen Sie den folgenden Code hinzu, um die Anforderung an die Vorhersageruntime zu erstellen.

Die Benutzeräußerung ist Teil des Objekts [prediction_request](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.models.predictionrequest?view=azure-python).

Die Methode **[get_slot_prediction](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.operations.predictionoperations?view=azure-python#get-slot-prediction-app-id--slot-name--prediction-request--verbose-none--show-all-intents-none--log-none--custom-headers-none--raw-false----operation-config-)** benötigt mehrere Parameter, um die Anforderung erfüllen zu können. Hierzu zählen beispielsweise die App-ID, der Slotname und das Vorhersageanforderungsobjekt. Die anderen Optionen, wie z. B. „Ausführlich“, „Alle Absichten anzeigen“ und „Protokollieren“, sind optional. Von der Anforderung wird ein Objekt vom Typ [PredictionResponse](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.models.predictionresponse?view=azure-python) zurückgegeben.

[!code-python[Get prediction based on query](~/cognitive-services-quickstart-code/python/LUIS/sdk-3x/authoring_and_predict.py?name=QueryPredictionEndpoint)]

[!INCLUDE [Prediction JSON response](sdk-json.md)]

## <a name="run-the-application"></a>Ausführen der Anwendung

Führen Sie die Anwendung mit dem Befehl `python` für die Schnellstartdatei aus.

```console
python authoring_and_predict.py
```