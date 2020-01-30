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
ms.date: 01/14/2020
ms.author: diberry
ms.openlocfilehash: fa0537b4009cf38b95cedff93850e5dac61baf34
ms.sourcegitcommit: 984c5b53851be35c7c3148dcd4dfd2a93cebe49f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2020
ms.locfileid: "76772385"
---
Verwenden Sie die LUIS-Vorhersageclientbibliothek (Language Understanding) für .NET für Folgendes:

* Abrufen einer Vorhersage nach Slot

[Referenzdokumentation](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/languageunderstanding?view=azure-dotnet) | [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Language.LUIS.Runtime) | [Vorhersagelaufzeitpaket (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.LUIS.Runtime/) | [C#-Beispiele](https://github.com/Azure-Samples/cognitive-services-quickstart-code/tree/master/dotnet/LanguageUnderstanding/predict-with-sdk-3x)

## <a name="prerequisites"></a>Voraussetzungen

* Konto für das LUIS-Portal (Language Understanding): [Erstellen Sie ein kostenloses Konto.](https://www.luis.ai)
* Aktuelle Version von [.NET Core](https://dotnet.microsoft.com/download/dotnet-core).

Weitere Dokumentationen:

 * [SDK-Referenzdokumentation](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/languageunderstanding?view=azure-dotnet)

## <a name="setting-up"></a>Einrichten

### <a name="create-an-environment-variable"></a>Erstellen einer Umgebungsvariablen

Erstellen Sie unter Verwendung des Schlüssels und des Namens Ihrer Ressource zwei Umgebungsvariablen für die Authentifizierung:

* `LUIS_PREDICTION_KEY`: Der Ressourcenschlüssel zum Authentifizieren Ihrer Anforderungen.
* `LUIS_ENDPOINT_NAME`: Der Ressourcenname, der Ihrem Schlüssel zugeordnet ist.

Führen Sie die Schritte für Ihr Betriebssystem aus:

#### <a name="windowstabwindows"></a>[Windows](#tab/windows)

```console
setx LUIS_PREDICTION_KEY <replace-with-your-resource-key>
setx LUIS_ENDPOINT_NAME <replace-with-your-resource-name>
```

Starten Sie das Konsolenfenster neu, nachdem Sie die Umgebungsvariable hinzugefügt haben.

#### <a name="linuxtablinux"></a>[Linux](#tab/linux)

```bash
export LUIS_PREDICTION_KEY=<replace-with-your-resource-key>
export LUIS_ENDPOINT_NAME=<replace-with-your-resource-name>
```

Führen Sie nach dem Hinzufügen der Umgebungsvariablen im Konsolenfenster `source ~/.bashrc` aus, damit die Änderungen wirksam werden.

#### <a name="macostabunix"></a>[macOS](#tab/unix)

Bearbeiten Sie `.bash_profile`, und fügen Sie die Umgebungsvariable hinzu:

```bash
export LUIS_PREDICTION_KEY=<replace-with-your-resource-key>
export LUIS_ENDPOINT_NAME=<replace-with-your-resource-name>
```

Führen Sie nach dem Hinzufügen der Umgebungsvariablen im Konsolenfenster `source .bash_profile` aus, damit die Änderungen wirksam werden.

---

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

Installieren Sie innerhalb des Anwendungsverzeichnisses die LUIS-Vorhersagelaufzeit-Clientbibliothek (Language Understanding) für .NET mithilfe des folgenden Befehls:

```dotnetcli
dotnet add package Microsoft.Azure.CognitiveServices.Language.LUIS.Runtime --version 3.0.0
```

Bei Verwendung der Visual Studio-IDE ist die Clientbibliothek als herunterladbares NuGet-Paket verfügbar.

## <a name="object-model"></a>Objektmodell

Der LUIS-Vorhersagelaufzeit-Client (Language Understanding) ist ein Objekt vom Typ [LUISRuntimeClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.runtime.luisruntimeclient?view=azure-dotnet), das sich bei Azure authentifiziert und Ihren Ressourcenschlüssel enthält.

Nach der Erstellung des Clients können Sie damit unter anderem auf folgende Funktionen zugreifen:

* Vorhersage durch [Staging oder Produktslot](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.runtime.predictionoperationsextensions.getslotpredictionasync?view=azure-dotnet)
* Vorhersage durch [Version](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.runtime.predictionoperationsextensions.getversionpredictionasync?view=azure-dotnet)


## <a name="code-examples"></a>Codebeispiele

In den bereitgestellten Codeausschnitten wird veranschaulicht, wie Sie die folgenden Vorgänge mit der LUIS-Vorhersagelaufzeit-Clientbibliothek (Language Understanding) für .NET ausführen:

* [Vorhersage nach Slot](#get-prediction-from-runtime)

## <a name="add-the-dependencies"></a>Hinzufügen der Abhängigkeiten

Öffnen Sie aus dem Projektverzeichnis die Datei *Program.cs* in Ihrem bevorzugten Editor oder Ihrer bevorzugten IDE. Ersetzen Sie den vorhandenen `using`-Code durch die folgenden `using`-Anweisungen:

[!code-csharp[Using statements](~/cognitive-services-quickstart-code/dotnet/LanguageUnderstanding/predict-with-sdk-3x/Program.cs?name=snippet_using)]

## <a name="authenticate-the-client"></a>Authentifizieren des Clients

1. Erstellen Sie Variablen für den Schlüssel, den Namen und die App-ID:

    Variablen zur Verwaltung Ihres Vorhersageschlüssels, der aus einer Umgebungsvariablen namens `LUIS_PREDICTION_KEY` gepullt wird. Wenn Sie die Umgebungsvariable nach dem Start der Anwendung erstellen, müssen der Editor, die IDE oder die Shell, in denen sie ausgeführt wird, geschlossen und erneut geladen werden, damit der Zugriff auf die Variable möglich ist. Die Methoden werden später erstellt.

    Erstellen Sie eine Variable zum Speichern Ihres Ressourcennamens `LUIS_ENDPOINT_NAME`.

    Erstellen Sie eine Variable für die App-ID als Umgebungsvariable mit dem Namen `LUIS_APP_ID`. Legen Sie die Umgebungsvariable für die öffentliche IoT-App fest:

    **`df67dcdb-c37d-46af-88e1-8b97951ca1c2`**

    [!code-csharp[Create variables](~/cognitive-services-quickstart-code/dotnet/LanguageUnderstanding/predict-with-sdk-3x/Program.cs?name=snippet_variables)]

1. Erstellen Sie ein Objekt vom Typ [ApiKeyServiceClientCredentials](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.runtime.apikeyserviceclientcredentials?view=azure-dotnet) mit Ihrem Schlüssel, und verwenden Sie es mit Ihrem Endpunkt, um ein Objekt vom Typ [LUISRuntimeClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.runtime.luisruntimeclient?view=azure-dotnet) zu erstellen.

    [!code-csharp[Create LUIS client object](~/cognitive-services-quickstart-code/dotnet/LanguageUnderstanding/predict-with-sdk-3x/Program.cs?name=snippet_create_client)]

## <a name="get-prediction-from-runtime"></a>Abrufen der Vorhersage aus der Laufzeit

Fügen Sie die folgende Methode hinzu, um die Anforderung an die Vorhersagelaufzeit zu erstellen.

Die Benutzeräußerung ist Teil des [PredictionRequest](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.runtime.models.predictionrequest?view=azure-dotnet)-Objekts.

Die **GetSlotPredictionAsync**-Methode benötigt mehrere Parameter, wie z. B. die App-ID, den Slotnamen, das Vorhersageanforderungsobjekt, um die Anforderung zu erfüllen. Die anderen Optionen, wie z. B. „Ausführlich“, „Alle Absichten anzeigen“ und „Protokollieren“, sind optional.

[!code-csharp[Create method to get prediction runtime](~/cognitive-services-quickstart-code/dotnet/LanguageUnderstanding/predict-with-sdk-3x/Program.cs?name=snippet_maintask)]

## <a name="main-code-for-the-prediction"></a>Hauptcode für die Vorhersage

Verwenden Sie die folgende Hauptmethode, um die Variablen und Methoden zu verknüpfen, um die Vorhersage zu erhalten.

[!code-csharp[Create method to get prediction runtime](~/cognitive-services-quickstart-code/dotnet/LanguageUnderstanding/predict-with-sdk-3x/Program.cs?name=snippet_main)]

## <a name="run-the-application"></a>Ausführen der Anwendung

Führen Sie die Anwendung mit dem Befehl `dotnet run` aus dem Anwendungsverzeichnis aus.

```dotnetcli
dotnet run
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie Ihre Vorhersagen abgeschlossen haben, bereinigen Sie die Arbeit aus dieser Schnellstartanleitung, indem Sie die program.cs-Datei und deren Unterverzeichnisse löschen.
