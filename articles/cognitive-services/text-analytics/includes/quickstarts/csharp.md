---
author: aahill
ms.service: cognitive-services
ms.topic: include
ms.date: 10/28/2019
ms.author: aahi
ms.openlocfilehash: ea526648b1b37919eb41953937d3afa72f7f39e7
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75446279"
---
<a name="HOLTop"></a>

[Referenzdokumentation](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/textanalytics?view=azure-dotnet-preview) | [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Language.TextAnalytics) | [Paket (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.TextAnalytics/) | [Beispiele](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples)

> [!NOTE]
> Der Code in diesem Artikel enthält der Einfachheit halber die synchronen Methoden des .NET SDK für die Textanalyse. Für Produktionsszenarien wird aus Leistungs- und Skalierbarkeitsgründen die Verwendung der asynchronen Batchmethoden empfohlen. Rufen Sie also beispielsweise [SentimentBatchAsync()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.sentimentbatchasync?view=azure-dotnet&viewFallbackFrom=azure-dotnet-preview) anstelle von [Sentiment()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.sentiment?view=azure-dotnet) auf.

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/)
* [Visual Studio-IDE](https://visualstudio.microsoft.com/vs/)

## <a name="setting-up"></a>Einrichten

### <a name="create-a-text-analytics-azure-resource"></a>Erstellen einer Azure-Ressource für die Textanalyse

[!INCLUDE [text-analytics-resource-creation](resource-creation.md)]

### <a name="create-a-new-net-core-application"></a>Erstellen einer neuen .NET Core-Anwendung

Erstellen Sie über die Visual Studio-IDE eine neue .NET Core-Konsolenanwendung. Damit wird ein einfaches „Hallo Welt“-Projekt mit einer einzelnen C#-Quelldatei erstellt: *program.cs*.

Installieren Sie die Clientbibliothek, indem Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf die Projektmappe klicken und **NuGet-Pakete verwalten** auswählen. Wählen Sie im daraufhin geöffneten Paket-Manager die Option **Durchsuchen** aus, und suchen Sie nach `Microsoft.Azure.CognitiveServices.Language.TextAnalytics`. Klicken Sie darauf, und klicken Sie dann auf **Installieren**. Sie können auch die [Paket-Manager-Konsole](https://docs.microsoft.com/nuget/consume-packages/install-use-packages-powershell#find-and-install-a-package) verwenden.

Öffnen Sie die Datei *program.cs*, und fügen Sie die folgenden `using`-Anweisungen hinzu:

[!code-csharp[Import directives](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=imports)]

Erstellen Sie in der `Program`-Klasse der Anwendung Variablen für den Schlüssel und Endpunkt Ihrer Ressource. 

[!INCLUDE [text-analytics-find-resource-information](../find-azure-resource-info.md)]

```csharp
private static readonly string key = "<replace-with-your-text-analytics-key-here>";
private static readonly string endpoint = "<replace-with-your-text-analytics-endpoint-here>";
```

Ersetzen Sie die `Main`-Methode der Anwendung. Die hier aufgerufenen Methoden definieren Sie später.

[!code-csharp[main method](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=main)]

## <a name="object-model"></a>Objektmodell

Der Textanalyse-Client ist ein Objekt vom Typ [TextAnalyticsClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclient?view=azure-dotnet), das sich mit Ihrem Schlüssel bei Azure authentifiziert und Funktionen bereitstellt, um Text als einzelne Zeichenfolgen oder als Batch zu akzeptieren. Sie können Text synchron oder asynchron an die API senden. Das Antwortobjekt enthält die Analyseinformationen für die einzelnen gesendeten Dokumente. 

## <a name="code-examples"></a>Codebeispiele

* [Authentifizieren des Clients](#authenticate-the-client)
* [Standpunktanalyse](#sentiment-analysis)
* [Sprachenerkennung](#language-detection)
* [Entitätserkennung](#entity-recognition)
* [Schlüsselbegriffserkennung](#key-phrase-extraction)

## <a name="authenticate-the-client"></a>Authentifizieren des Clients

Erstellen Sie eine neue Klasse vom Typ `ApiKeyServiceClientCredentials`, um die Anmeldeinformationen zu speichern und den Anforderungen des Clients hinzuzufügen. Erstellen Sie darin eine Außerkraftsetzung für `ProcessHttpRequestAsync()`, um Ihren Schlüssel dem Header `Ocp-Apim-Subscription-Key` hinzuzufügen.

[!code-csharp[Client class](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=clientClass)]

Erstellen Sie eine Methode, um das [TextAnalyticsClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclient?view=azure-dotnet)-Objekt mit Ihrem Endpunkt und einem `ApiKeyServiceClientCredentials`-Objekt zu instanziieren, das Ihren Schlüssel enthält.

[!code-csharp[Client authentication](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=authentication)]

Rufen Sie in der `main()`-Methode des Programms die Authentifizierungsmethode auf, um den Client zu instanziieren.

## <a name="sentiment-analysis"></a>Stimmungsanalyse

Erstellen Sie eine neue Funktion namens `SentimentAnalysisExample()`, die den zuvor erstellten Client verwendet, und rufen Sie die zugehörige Funktion [Sentiment()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.sentiment?view=azure-dotnet) auf. Das zurückgegebene Objekt vom Typ [SentimentResult](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.models.sentimentresult?view=azure-dotnet) enthält bei erfolgreicher Ausführung die Stimmung `Score` und andernfalls `errorMessage`. 

Eine gegen 0 tendierende Punktzahl deutet auf eine negative Stimmung hin, eine gegen 1 tendierende Punktzahl auf eine positive.

[!code-csharp[Sentiment analysis](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=sentiment)]

### <a name="output"></a>Output

```console
Sentiment Score: 0.87
```

## <a name="language-detection"></a>Spracherkennung

Erstellen Sie eine neue Funktion namens `languageDetectionExample()`, die den zuvor erstellten Client verwendet, und rufen Sie die zugehörige Funktion [DetectLanguage()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.detectlanguage?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Language_TextAnalytics_TextAnalyticsClientExtensions_DetectLanguage_Microsoft_Azure_CognitiveServices_Language_TextAnalytics_ITextAnalyticsClient_System_String_System_String_System_Nullable_System_Boolean__System_Threading_CancellationToken_) auf. Das zurückgegebene Objekt vom Typ [LanguageResult](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.models.languageresult?view=azure-dotnet) enthält bei erfolgreicher Ausführung die Liste der erkannten Sprachen in `DetectedLanguages` und andernfalls `errorMessage`.  Geben Sie die erste zurückgegebene Sprache aus.

> [!Tip]
> In einigen Fällen ist es unter Umständen schwierig, die Sprachen basierend auf den Eingaben eindeutig zu bestimmen. Mithilfe des Parameters `countryHint` können Sie einen zweistelligen Ländercode angeben. Die API verwendet standardmäßig „US“ als Wert für „countryHint“. Wenn Sie dieses Verhalten entfernen möchten, können Sie diesen Parameter zurücksetzen, indem Sie seinen Wert auf eine leere Zeichenfolge (`countryHint = ""`) festlegen.

[!code-csharp[Language Detection example](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=languageDetection)]

### <a name="output"></a>Output

```console
Language: English
```

## <a name="entity-recognition"></a>Entitätserkennung

Erstellen Sie eine neue Funktion namens `RecognizeEntitiesExample()`, die den zuvor erstellten Client verwendet, und rufen Sie die zugehörige Funktion [Entities()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.entities?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Language_TextAnalytics_TextAnalyticsClientExtensions_Entities_Microsoft_Azure_CognitiveServices_Language_TextAnalytics_ITextAnalyticsClient_System_String_System_String_System_Nullable_System_Boolean__System_Threading_CancellationToken_) auf. Durchlaufen Sie die Ergebnisse. Das zurückgegebene Objekt vom Typ [EntitiesResult](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.models.entitiesresult?view=azure-dotnet) enthält bei erfolgreicher Ausführung die Liste der erkannten Entitäten in `Entities` und andernfalls `errorMessage`. Geben Sie für jede erkannte Entität ihren Typ, Untertyp, Wikipedia-Namen (sofern vorhanden) sowie die Fundorte im Originaltext aus.

[!code-csharp[Entity Recognition example](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=entityRecognition)]


### <a name="output"></a>Output

```console
Entities:
    Name: Microsoft,        Type: Organization,     Sub-Type: N/A
        Offset: 0,      Length: 9,      Score: 1.000
    Name: Bill Gates,       Type: Person,   Sub-Type: N/A
        Offset: 25,     Length: 10,     Score: 1.000
    Name: Paul Allen,       Type: Person,   Sub-Type: N/A
        Offset: 40,     Length: 10,     Score: 0.999
    Name: April 4,  Type: Other,    Sub-Type: N/A
        Offset: 54,     Length: 7,      Score: 0.800
    Name: April 4, 1975,    Type: DateTime, Sub-Type: Date
        Offset: 54,     Length: 13,     Score: 0.800
    Name: BASIC,    Type: Other,    Sub-Type: N/A
        Offset: 89,     Length: 5,      Score: 0.800
    Name: Altair 8800,      Type: Other,    Sub-Type: N/A
        Offset: 116,    Length: 11,     Score: 0.800
```

## <a name="key-phrase-extraction"></a>Schlüsselwortextraktion

Erstellen Sie eine neue Funktion namens `KeyPhraseExtractionExample()`, die den zuvor erstellten Client verwendet, und rufen Sie die zugehörige Funktion [KeyPhrases()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.keyphrases?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Language_TextAnalytics_TextAnalyticsClientExtensions_KeyPhrases_Microsoft_Azure_CognitiveServices_Language_TextAnalytics_ITextAnalyticsClient_System_String_System_String_System_Nullable_System_Boolean__System_Threading_CancellationToken_) auf. Das Ergebnis enthält bei erfolgreicher Ausführung die Liste der erkannten Schlüsselbegriffe in `KeyPhrases` und andernfalls `errorMessage`. Geben Sie alle erkannten Schlüsselbegriffe aus.

[!code-csharp[Key phrase extraction example](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=keyPhraseExtraction)]


### <a name="output"></a>Output

```console
Key phrases:
    cat
    veterinarian
```
