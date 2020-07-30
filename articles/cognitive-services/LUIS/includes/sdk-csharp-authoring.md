---
title: include file
description: include file
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.date: 07/28/2020
ms.topic: include
ms.custom: include file
ms.author: diberry
ms.openlocfilehash: 671d6a18ca4c6b9ae15453d23d8188fbb4f7d957
ms.sourcegitcommit: f353fe5acd9698aa31631f38dd32790d889b4dbb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2020
ms.locfileid: "87369424"
---
Verwenden Sie die LUIS-Erstellungsclientbibliothek (Language Understanding) für .NET für Folgendes:

* Erstellen einer App
* Hinzufügen von Absichten, Entitäten und Beispieläußerungen
* Hinzufügen von Features (beispielsweise eine Ausdrucksliste)
* Trainieren und Veröffentlichen der App

[Referenzdokumentation](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/languageunderstanding?view=azure-dotnet) | [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Language.LUIS.Authoring) | [Erstellungspaket (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.LUIS.Authoring/) | [C#-Beispiele](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/dotnet/LanguageUnderstanding/authoring/authoring-with-sdk.cs)

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/)
* Aktuelle Version von [.NET Core](https://dotnet.microsoft.com/download/dotnet-core).
* Sobald Sie über Ihr Azure-Abonnement verfügen, [erstellen Sie eine Language Understanding-Erstellungsressource](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesLUISAllInOne) im Azure-Portal, um an Ihren Schlüssel und Ihren Endpunkt zu gelangen. Warten Sie ihre Bereitstellung ab, und klicken Sie auf die Schaltfläche **Zu Ressource wechseln**.
    * Sie benötigen den Schlüssel und Endpunkt der von Ihnen [erstellten](../luis-how-to-azure-subscription.md#create-luis-resources-in-azure-portal) Ressource, um Ihre Anwendung mit der Language Understanding-Erstellung zu verbinden. Der Schlüssel und der Endpunkt werden weiter unten in der Schnellstartanleitung in den Code eingefügt. Sie können den kostenlosen Tarif (`F0`) verwenden, um den Dienst auszuprobieren.

## <a name="setting-up"></a>Einrichten

### <a name="create-a-new-c-application"></a>Erstellen einer neuen C#-Anwendung

Erstellen Sie eine neue .NET Core-Anwendung in Ihrem bevorzugten Editor oder Ihrer bevorzugten IDE.

1. Verwenden Sie in einem Konsolenfenster (z. B. cmd, PowerShell oder Bash) den Befehl dotnet `new` zum Erstellen einer neuen Konsolen-App mit dem Namen `language-understanding-quickstart`. Dieser Befehl erstellt ein einfaches „Hallo Welt“-C#-Projekt mit einer einzigen Quelldatei: `Program.cs`.

    ```dotnetcli
    dotnet new console -n language-understanding-quickstart
    ```

1. Wechseln Sie zum Ordner der neu erstellten App.

1. Sie können die Anwendung mit folgendem Befehl erstellen:

    ```dotnetcli
    dotnet build
    ```

    Die Buildausgabe sollte keine Warnungen oder Fehler enthalten.

    ```console
    ...
    Build succeeded.
     0 Warning(s)
     0 Error(s)
    ...
    ```


### <a name="install-the-sdk"></a>Installieren des SDKs

Installieren Sie innerhalb des Anwendungsverzeichnisses die LUIS-Erstellungsclientbibliothek (Language Understanding) für .NET mithilfe des folgenden Befehls:

```dotnetcli
dotnet add package Microsoft.Azure.CognitiveServices.Language.LUIS.Authoring --version 3.0.0
```

Bei Verwendung der Visual Studio-IDE ist die Clientbibliothek als herunterladbares NuGet-Paket verfügbar.


## <a name="object-model"></a>Objektmodell

Der LUIS-Erstellungsclient (Language Understanding) ist ein Objekt vom Typ [LUISAuthoringClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.luisauthoringclient?view=azure-dotnet), das sich bei Azure authentifiziert und Ihren Erstellungsschlüssel enthält.

Nach der Erstellung des Clients können Sie damit unter anderem auf folgende Funktionen zugreifen:

* Apps: [Erstellen](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.appsextensions.addasync?view=azure-dotnet), [Löschen](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.appsextensions.deleteasync?view=azure-dotnet), [Veröffentlichen](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.appsextensions.publishasync?view=azure-dotnet)
* Beispieläußerungen: [Hinzufügen per Batch](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.examplesextensions.batchasync?view=azure-dotnet), [Löschen anhand der ID](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.examplesextensions.deleteasync?view=azure-dotnet)
* Features: Verwalten von [Ausdruckslisten](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.featuresextensions.addphraselistasync?view=azure-dotnet)
* Modell: Verwalten von [Absichten](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.modelextensions?view=azure-dotnet) und Entitäten
* Muster: Verwalten von [Mustern](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.patternextensions?view=azure-dotnet)
* Trainieren: [Trainieren](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.trainextensions.trainversionasync?view=azure-dotnet) der App und Abfragen des [Trainingsstatus](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.trainextensions.getstatusasync?view=azure-dotnet)
* [Versionen:](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.versionsextensions?view=azure-dotnet) Verwalten mittels Klonen, Exportieren und Löschen


## <a name="code-examples"></a>Codebeispiele

In den bereitgestellten Codeausschnitten wird veranschaulicht, wie Sie die folgenden Vorgänge mit der LUIS-Erstellungsclientbibliothek (Language Understanding) für .NET ausführen:

* [Erstellen einer App](#create-a-luis-app)
* [Hinzufügen von Entitäten](#create-entities-for-the-app)
* [Hinzufügen von Absichten](#create-intent-for-the-app)
* [Hinzufügen von Beispieläußerungen](#add-example-utterance-to-intent)
* [Trainieren der App](#train-the-app)
* [Veröffentlichen der App](#publish-a-language-understanding-app)

## <a name="add-the-dependencies"></a>Hinzufügen der Abhängigkeiten

Öffnen Sie aus dem Projektverzeichnis die Datei *Program.cs* in Ihrem bevorzugten Editor oder Ihrer bevorzugten IDE. Ersetzen Sie den vorhandenen `using`-Code durch die folgenden `using`-Anweisungen:

[!code-csharp[Using statements](~/cognitive-services-quickstart-code/dotnet/LanguageUnderstanding/authoring/authoring-with-sdk.cs?name=Dependencies)]

## <a name="authenticate-the-client"></a>Authentifizieren des Clients

1. Erstellen Sie eine Variable zur Aufnahme Ihres Erstellungsschlüssels und des Erstellungsendpunkts.

    [!code-csharp[Authorization to resource key](~/cognitive-services-quickstart-code/dotnet/LanguageUnderstanding/authoring/authoring-with-sdk.cs?name=Variables)]

1. Erstellen Sie ein Objekt vom Typ [ApiKeyServiceClientCredentials](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.apikeyserviceclientcredentials?view=azure-dotnet) mit Ihrem Schlüssel, und verwenden Sie es mit Ihrem Endpunkt, um ein Objekt vom Typ [LUISAuthoringClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.luisauthoringclient?view=azure-dotnet) zu erstellen.

    [!code-csharp[Create LUIS client object](~/cognitive-services-quickstart-code/dotnet/LanguageUnderstanding/authoring/authoring-with-sdk.cs?name=AuthoringCreateClient)]

## <a name="create-a-luis-app"></a>Erstellen einer LUIS-App

1. Erstellen Sie eine LUIS-App für das NLP-Modell (Natural Language Processing, Verarbeitung natürlicher Sprache) mit Absichten, Entitäten und Beispieläußerungen.

1. Erstellen Sie ein Objekt vom Typ [ApplicationCreateObject](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.models.applicationcreateobject?view=azure-dotnet). Name und Sprachkultur sind erforderliche Eigenschaften.

1. Rufen Sie die Methode [Apps.AddAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.appsextensions.addasync?view=azure-dotnet) auf. Die Antwort ist die App-ID.

    [!code-csharp[Create a LUIS app](~/cognitive-services-quickstart-code/dotnet/LanguageUnderstanding/authoring/authoring-with-sdk.cs?name=AuthoringCreateApplication)]

## <a name="create-intent-for-the-app"></a>Erstellen der Absicht für die App
Das primäre Objekt im Modell einer LUIS-App ist die Absicht. Die Absicht orientiert sich an einer Gruppierung der _Absichten_ von Benutzeräußerungen. Ein Benutzer kann eine Frage stellen oder etwas sagen, um eine bestimmte _beabsichtigte_ Antwort von einem Bot (oder von einer anderen Clientanwendung) zu erhalten. Beispiele für Absichten wären etwa das Buchen eines Flugs, das Einholen von Wetterinformationen für eine Zielstadt oder das Erfragen von Kontaktinformationen für den Kundendienst.

Erstellen Sie ein Objekt vom Typ [ModelCreateObject](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.models.modelcreateobject?view=azure-dotnet) mit dem Namen der eindeutigen Absicht, und übergeben Sie dann die App-ID, die Versions-ID und das ModelCreateObject-Objekt an die Methode [Model.AddIntentAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.modelextensions.addintentasync?view=azure-dotnet). Die Antwort ist die Absichts-ID.

[!code-csharp[Create intent](~/cognitive-services-quickstart-code/dotnet/LanguageUnderstanding/authoring/authoring-with-sdk.cs?name=AuthoringAddIntents)]

## <a name="create-entities-for-the-app"></a>Erstellen von Entitäten für die App

Entitäten sind zwar nicht erforderlich, aber in den meisten Apps vorhanden. Die Entität extrahiert Informationen aus der Benutzeräußerung, die zur Erfüllung der Absicht des Benutzers erforderlich sind. Es gibt mehrere Arten von [vordefinierten](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.modelextensions.addprebuiltasync?view=azure-dotnet) und benutzerdefinierten Entitäten, die jeweils über ein eigenes DTO-Modell (Data Transformation Object) verfügen.  Zu den gängigen vordefinierten Entitäten, die Sie Ihrer App hinzufügen können, zählen etwa [number](../luis-reference-prebuilt-number.md), [datetimeV2](../luis-reference-prebuilt-datetimev2.md), [geographyV2](../luis-reference-prebuilt-geographyv2.md) und [ordinal](../luis-reference-prebuilt-ordinal.md).

Diese Methode vom Typ **AddEntities** hat die einfache Entität `Location` mit zwei Rollen, die einfache Entität `Class` und die zusammengesetzte Entität `Flight` erstellt und fügt mehrere vordefinierte Entitäten hinzu.

Wichtig: Entitäten sind nicht mit einer Absicht gekennzeichnet. Sie gelten in der Regel für mehrere Absichten. Nur exemplarische Benutzeräußerungen sind für eine bestimmte, einzelne Absicht gekennzeichnet.

Erstellungsmethoden für Entitäten sind Teil der Klasse [Model](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.modelextensions?view=azure-dotnet). Jeder Entitätstyp verfügt über ein eigenes DTO-Modell (Data Transformation Object), das in der Regel das Wort `model` im Namespace [Models](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.models?view=azure-dotnet) enthält.

[!code-csharp[Create entities](~/cognitive-services-quickstart-code/dotnet/LanguageUnderstanding/authoring/authoring-with-sdk.cs?name=AuthoringAddEntities)]

## <a name="add-example-utterance-to-intent"></a>Hinzufügen einer Beispieläußerung zu einer Absicht

Um die Absicht einer Äußerung ermitteln und Entitäten extrahieren zu können, benötigt die App Beispiele für Äußerungen. Die Beispiele müssen auf eine bestimmte, einzelne Absicht ausgerichtet sein und alle benutzerdefinierten Entitäten markieren. Vordefinierte Entitäten müssen nicht markiert werden.

Fügen Sie Beispieläußerungen hinzu, indem Sie eine Liste mit Objekten vom Typ [ExampleLabelObject](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.models.examplelabelobject?view=azure-dotnet) erstellen (jeweils ein Objekt pro Beispieläußerung). Jedes Beispiel muss alle Entitäten mit einem Wörterbuch mit Name-Wert-Paaren (Entitätsname und Entitätswert) markieren. Der Entitätswert muss exakt dem Vorkommen im Text der Beispieläußerung entsprechen.

Rufen Sie [Examples.BatchAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.examplesextensions.batchasync?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Language_LUIS_Authoring_ExamplesExtensions_BatchAsync_Microsoft_Azure_CognitiveServices_Language_LUIS_Authoring_IExamples_System_Guid_System_String_System_Collections_Generic_IList_Microsoft_Azure_CognitiveServices_Language_LUIS_Authoring_Models_ExampleLabelObject__System_Threading_CancellationToken_) mit der App-ID, der Versions-ID und der Liste mit den Beispielen auf. Daraufhin wird eine Liste mit Ergebnissen zurückgegeben. Überprüfen Sie das Ergebnis der einzelnen Beispiele, um sich zu vergewissern, dass es dem Modell erfolgreich hinzugefügt wurde.

[!code-csharp[Add example utterances to a specific intent](~/cognitive-services-quickstart-code/dotnet/LanguageUnderstanding/authoring/authoring-with-sdk.cs?name=AuthoringBatchAddUtterancesForIntent)]

Die Methoden **CreateUtterance** und **CreateLabel** sind Hilfsmethoden, die Sie bei der Objekterstellung unterstützen.

## <a name="train-the-app"></a>Trainieren der App

Nach Erstellung des Modells muss die LUIS-App für diese Version des Modells trainiert werden. Ein trainiertes Modell kann in einem [Container](../luis-container-howto.md) verwendet oder in den Staging- oder Produktslots [veröffentlicht](../luis-how-to-publish-app.md) werden.

Für die Methode [Train.TrainVersionAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.trainextensions?view=azure-dotnet) müssen die App-ID und die Versions-ID angegeben werden.

Bei sehr kleinen Modellen (wie etwa in dieser Schnellstartanleitung) dauert das Training nicht sehr lang. Bei Produktionsanwendungen muss das App-Training auch einen Abfrageaufruf für die Methode [GetStatusAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.trainextensions.getstatusasync?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Language_LUIS_Authoring_TrainExtensions_GetStatusAsync_Microsoft_Azure_CognitiveServices_Language_LUIS_Authoring_ITrain_System_Guid_System_String_System_Threading_CancellationToken_) enthalten, um zu bestimmen, wann bzw. ob das Training erfolgreich war. Die Antwort ist eine Liste mit Objekten vom Typ [ModelTrainingInfo](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.models.modeltraininginfo?view=azure-dotnet), die jeweils einen separaten Status für die einzelnen Objekte besitzen. Alle Objekte müssen erfolgreich sein, damit das Training als abgeschlossen gilt.

[!code-csharp[Train the app's version](~/cognitive-services-quickstart-code/dotnet/LanguageUnderstanding/authoring/authoring-with-sdk.cs?name=AuthoringTrainVersion)]

## <a name="publish-a-language-understanding-app"></a>Veröffentlichen einer Language Understanding-App

Veröffentlichen Sie die LUIS-App mithilfe der Methode [PublishAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.appsextensions.publishasync?view=azure-dotnet). Dadurch wird die aktuelle trainierte Version im angegebenen Slot am Endpunkt veröffentlicht. Ihre Clientanwendung verwendet diesen Endpunkt zum Senden von Benutzeräußerungen für die Absichtsvorhersage und die Entitätsextraktion.

[!code-csharp[Create entities](~/cognitive-services-quickstart-code/dotnet/LanguageUnderstanding/authoring/authoring-with-sdk.cs?name=AuthoringPublishVersionAndSlot)]

## <a name="run-the-application"></a>Ausführen der Anwendung

Führen Sie die Anwendung mit dem Befehl `dotnet run` aus dem Anwendungsverzeichnis aus.

```dotnetcli
dotnet run
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Zur Bereinigung können Sie die LUIS-App löschen. Zum Löschen der App wird die Methode [Apps.DeleteAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.appsextensions.deleteasync?view=azure-dotnet) verwendet. Sie können die App aber auch über das [LUIS-Portal](https://www.luis.ai) löschen.