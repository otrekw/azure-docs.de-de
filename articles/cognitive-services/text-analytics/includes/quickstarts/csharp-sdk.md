---
title: 'Schnellstart: Textanalyse-Clientbibliothek für C# | Microsoft-Dokumentation'
description: Steigen Sie in die Textanalyse-Clientbibliothek für C# ein.
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 07/27/2020
ms.author: aahi
ms.reviewer: assafi
ms.openlocfilehash: aa182f27fcd3ab2984bbd9f960b2b828880c36f5
ms.sourcegitcommit: f353fe5acd9698aa31631f38dd32790d889b4dbb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2020
ms.locfileid: "87375193"
---
<a name="HOLTop"></a>

#### <a name="version-30"></a>[Version 3.0](#tab/version-3)

[v3-Referenzdokumentation](https://aka.ms/azsdk-net-textanalytics-ref-docs) | [Quellcode der v3-Bibliothek](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/textanalytics) | [v3-Paket (NuGet)](https://www.nuget.org/packages/Azure.AI.TextAnalytics) | [v3-Beispiele](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/textanalytics/Azure.AI.TextAnalytics/samples)

#### <a name="version-21"></a>[Version 2.1](#tab/version-2)

[v2-Referenzdokumentation](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/textanalytics?view=azure-dotnet-preview) | [Quellcode der v2-Bibliothek](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Language.TextAnalytics) | [v2-Paket (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.TextAnalytics/) | [v2-Beispiele](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples)

---

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/cognitive-services)
* [Visual Studio-IDE](https://visualstudio.microsoft.com/vs/)
* Sobald Sie über Ihr Azure-Abonnement verfügen, <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics"  title="Erstellen einer Textanalyseressource"  target="_blank"> erstellen Sie eine Textanalyseressource <span class="docon docon-navigate-external x-hidden-focus"></span></a> im Azure-Portal, um Ihren Schlüssel und Endpunkt zu erhalten.  Klicken Sie nach Abschluss der Bereitstellung auf **Zu Ressource wechseln**.
    * Sie benötigen den Schlüssel und den Endpunkt der von Ihnen erstellten Ressource, um Ihre Anwendung mit der Textanalyse-API zu verbinden. Der Schlüssel und der Endpunkt werden weiter unten in der Schnellstartanleitung in den Code eingefügt.
    * Sie können den kostenlosen Tarif (`F0`) verwenden, um den Dienst zu testen, und später für die Produktion auf einen kostenpflichtigen Tarif upgraden.

## <a name="setting-up"></a>Einrichten

### <a name="create-a-new-net-core-application"></a>Erstellen einer neuen .NET Core-Anwendung

Erstellen Sie über die Visual Studio-IDE eine neue .NET Core-Konsolenanwendung. Dadurch wird ein Projekt vom Typ „Hallo Welt“ mit einer einzelnen C#-Quelldatei (*program.cs*) erstellt.

#### <a name="version-30"></a>[Version 3.0](#tab/version-3)

Installieren Sie die Clientbibliothek, indem Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf die Projektmappe klicken und **NuGet-Pakete verwalten** auswählen. Wählen Sie im daraufhin geöffneten Paket-Manager die Option **Durchsuchen** aus, und suchen Sie nach `Azure.AI.TextAnalytics`. Wählen Sie die Version `5.0.0` und anschließend **Installieren** aus. Sie können auch die [Paket-Manager-Konsole](https://docs.microsoft.com/nuget/consume-packages/install-use-packages-powershell#find-and-install-a-package) verwenden.


> [!TIP]
> Möchten Sie sich sofort die gesamte Codedatei für die Schnellstartanleitung ansehen? Die Datei steht [auf GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/dotnet/TextAnalytics/program.cs) zur Verfügung. Dort finden Sie die Codebeispiele aus dieser Schnellstartanleitung. 

#### <a name="version-21"></a>[Version 2.1](#tab/version-2)

Installieren Sie die Clientbibliothek, indem Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf die Projektmappe klicken und **NuGet-Pakete verwalten** auswählen. Wählen Sie im daraufhin geöffneten Paket-Manager die Option **Durchsuchen** aus, und suchen Sie nach `Microsoft.Azure.CognitiveServices.Language.TextAnalytics`. Klicken Sie darauf, und klicken Sie dann auf **Installieren**. Sie können auch die [Paket-Manager-Konsole](https://docs.microsoft.com/nuget/consume-packages/install-use-packages-powershell#find-and-install-a-package) verwenden.

> [!TIP]
> Möchten Sie sich sofort die gesamte Codedatei für die Schnellstartanleitung ansehen? Die Datei steht [auf GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/samples/TextAnalytics/synchronous/Program.cs) zur Verfügung. Dort finden Sie die Codebeispiele aus dieser Schnellstartanleitung. 

---

#### <a name="version-30"></a>[Version 3.0](#tab/version-3)

Öffnen Sie die Datei *program.cs*, und fügen Sie die folgenden `using`-Anweisungen hinzu:

```csharp
using Azure;
using System;
using System.Globalization;
using Azure.AI.TextAnalytics;
```

Erstellen Sie in der `Program`-Klasse der Anwendung Variablen für den Schlüssel und Endpunkt Ihrer Ressource.

[!INCLUDE [text-analytics-find-resource-information](../find-azure-resource-info.md)]

```csharp
private static readonly AzureKeyCredential credentials = new AzureKeyCredential("<replace-with-your-text-analytics-key-here>");
private static readonly Uri endpoint = new Uri("<replace-with-your-text-analytics-endpoint-here>");
```

Ersetzen Sie die `Main`-Methode der Anwendung. Die hier aufgerufenen Methoden definieren Sie später.

```csharp
static void Main(string[] args)
{
    var client = new TextAnalyticsClient(endpoint, credentials);
    // You will implement these methods later in the quickstart.
    SentimentAnalysisExample(client);
    LanguageDetectionExample(client);
    EntityRecognitionExample(client);
    EntityLinkingExample(client);
    KeyPhraseExtractionExample(client);

    Console.Write("Press any key to exit.");
    Console.ReadKey();
}
```

#### <a name="version-21"></a>[Version 2.1](#tab/version-2)

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

---

## <a name="object-model"></a>Objektmodell

Der Textanalyse-Client ist ein Objekt vom Typ `TextAnalyticsClient`, das sich mit Ihrem Schlüssel bei Azure authentifiziert und Funktionen bereitstellt, um Text als einzelne Zeichenfolgen oder als Batch zu akzeptieren. Sie können Text synchron oder asynchron an die API senden. Das Antwortobjekt enthält die Analyseinformationen für die einzelnen gesendeten Dokumente. 

Wenn Sie Version `3.0` des Diensts verwenden, können Sie eine optionale `TextAnalyticsClientOptions`-Instanz nutzen, um den Client mit verschiedenen Standardeinstellungen wie Standardsprache oder Länder-/Regionshinweis zu initialisieren. Sie können für die Authentifizieren auch ein Azure Active Directory-Token verwenden. 

## <a name="code-examples"></a>Codebeispiele

* [Standpunktanalyse](#sentiment-analysis)
* [Sprachenerkennung](#language-detection)
* [Erkennung benannter Entitäten](#named-entity-recognition-ner)
* [Entitätsverknüpfung](#entity-linking)
* [Schlüsselbegriffserkennung](#key-phrase-extraction)

## <a name="authenticate-the-client"></a>Authentifizieren des Clients

#### <a name="version-30"></a>[Version 3.0](#tab/version-3)

Stellen Sie sicher, dass mit Ihrer zuvor erstellten Main-Methode ein neues Clientobjekt mit Ihrem Endpunkt und Ihren Anmeldeinformationen erstellt wird.

```csharp
var client = new TextAnalyticsClient(endpoint, credentials);
```

#### <a name="version-21"></a>[Version 2.1](#tab/version-2)

Erstellen Sie eine neue Klasse vom Typ `ApiKeyServiceClientCredentials`, um die Anmeldeinformationen zu speichern und den Anforderungen des Clients hinzuzufügen. Erstellen Sie darin eine Außerkraftsetzung für `ProcessHttpRequestAsync()`, um Ihren Schlüssel dem Header `Ocp-Apim-Subscription-Key` hinzuzufügen.

[!code-csharp[Client class](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=clientClass)]

Erstellen Sie eine Methode, um das [TextAnalyticsClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclient?view=azure-dotnet)-Objekt mit Ihrem Endpunkt und einem `ApiKeyServiceClientCredentials`-Objekt zu instanziieren, das Ihren Schlüssel enthält.

[!code-csharp[Client authentication](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=authentication)]

---

## <a name="sentiment-analysis"></a>Stimmungsanalyse

#### <a name="version-30"></a>[Version 3.0](#tab/version-3)

Erstellen Sie eine neue Funktion namens `SentimentAnalysisExample()`, die den zuvor erstellten Client verwendet, und rufen Sie die zugehörige Funktion `AnalyzeSentiment()` auf. Das zurückgegebene Objekt vom Typ `Response<DocumentSentiment>` enthält die Stimmungsbezeichnung und den Stimmungswert des gesamten Eingabedokuments sowie eine Standpunktanalyse für jeden Satz (sofern erfolgreich). Im Falle eines Fehlers wird ein Fehler vom Typ `RequestFailedException` ausgelöst.

```csharp
static void SentimentAnalysisExample(TextAnalyticsClient client)
{
    string inputText = "I had the best day of my life. I wish you were there with me.";
    DocumentSentiment documentSentiment = client.AnalyzeSentiment(inputText);
    Console.WriteLine($"Document sentiment: {documentSentiment.Sentiment}\n");

    foreach (var sentence in documentSentiment.Sentences)
    {
        Console.WriteLine($"\tText: \"{sentence.Text}\"");
        Console.WriteLine($"\tSentence sentiment: {sentence.Sentiment}");
        Console.WriteLine($"\tPositive score: {sentence.ConfidenceScores.Positive:0.00}");
        Console.WriteLine($"\tNegative score: {sentence.ConfidenceScores.Negative:0.00}");
        Console.WriteLine($"\tNeutral score: {sentence.ConfidenceScores.Neutral:0.00}\n");
    }
}
```

### <a name="output"></a>Output

```console
Document sentiment: Positive

        Text: "I had the best day of my life."
        Sentence sentiment: Positive
        Positive score: 1.00
        Negative score: 0.00
        Neutral score: 0.00

        Text: "I wish you were there with me."
        Sentence sentiment: Neutral
        Positive score: 0.21
        Negative score: 0.02
        Neutral score: 0.77
```

#### <a name="version-21"></a>[Version 2.1](#tab/version-2)

Erstellen Sie eine neue Funktion namens `SentimentAnalysisExample()`, die den zuvor erstellten Client verwendet, und rufen Sie die zugehörige Funktion [Sentiment()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.sentiment?view=azure-dotnet) auf. Das zurückgegebene Objekt vom Typ [SentimentResult](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.models.sentimentresult?view=azure-dotnet) enthält bei erfolgreicher Ausführung die Stimmung `Score` und andernfalls `errorMessage`. 

Eine gegen 0 tendierende Punktzahl deutet auf eine negative Stimmung hin, eine gegen 1 tendierende Punktzahl auf eine positive.

[!code-csharp[Sentiment analysis](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=sentiment)]

```console
Sentiment Score: 0.87
```

---

## <a name="language-detection"></a>Spracherkennung

#### <a name="version-30"></a>[Version 3.0](#tab/version-3)


Erstellen Sie eine neue Funktion namens `LanguageDetectionExample()`, die den zuvor erstellten Client verwendet, und rufen Sie die zugehörige Funktion `DetectLanguage()` auf. Das zurückgegebene Objekt vom Typ `Response<DetectedLanguage>` enthält die erkannte Sprache sowie den zugehörigen Namen und ISO-6391-Code. Im Falle eines Fehlers wird ein Fehler vom Typ `RequestFailedException` ausgelöst.

> [!Tip]
> In einigen Fällen ist es unter Umständen schwierig, die Sprachen basierend auf den Eingaben eindeutig zu bestimmen. Mithilfe des Parameters `countryHint` können Sie einen zweistelligen Länder-/Regionscode angeben. Die API verwendet standardmäßig „US“ als Wert für „countryHint“. Wenn Sie dieses Verhalten entfernen möchten, können Sie diesen Parameter zurücksetzen, indem Sie seinen Wert auf eine leere Zeichenfolge (`countryHint = ""`) festlegen. Wenn Sie einen anderen Standardwert festlegen möchten, legen Sie die Eigenschaft `TextAnalyticsClientOptions.DefaultCountryHint` fest, und übergeben Sie sie während der Initialisierung des Clients.

```csharp
static void LanguageDetectionExample(TextAnalyticsClient client)
{
    DetectedLanguage detectedLanguage = client.DetectLanguage("Ce document est rédigé en Français.");
    Console.WriteLine("Language:");
    Console.WriteLine($"\t{detectedLanguage.Name},\tISO-6391: {detectedLanguage.Iso6391Name}\n");
}
```

### <a name="output"></a>Output

```console
Language:
        French, ISO-6391: fr
```

#### <a name="version-21"></a>[Version 2.1](#tab/version-2)

Erstellen Sie eine neue Funktion namens `languageDetectionExample()`, die den zuvor erstellten Client verwendet, und rufen Sie die zugehörige Funktion [DetectLanguage()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.detectlanguage?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Language_TextAnalytics_TextAnalyticsClientExtensions_DetectLanguage_Microsoft_Azure_CognitiveServices_Language_TextAnalytics_ITextAnalyticsClient_System_String_System_String_System_Nullable_System_Boolean__System_Threading_CancellationToken_) auf. Das zurückgegebene Objekt vom Typ [LanguageResult](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.models.languageresult?view=azure-dotnet) enthält bei erfolgreicher Ausführung die Liste der erkannten Sprachen in `DetectedLanguages` und andernfalls `errorMessage`. Geben Sie die erste zurückgegebene Sprache aus.

> [!Tip]
> In einigen Fällen ist es unter Umständen schwierig, die Sprachen basierend auf den Eingaben eindeutig zu bestimmen. Mithilfe des Parameters `countryHint` können Sie einen zweistelligen Länder-/Regionscode angeben. Die API verwendet standardmäßig „US“ als Wert für „countryHint“. Wenn Sie dieses Verhalten entfernen möchten, können Sie diesen Parameter zurücksetzen, indem Sie seinen Wert auf eine leere Zeichenfolge (`countryHint = ""`) festlegen.

[!code-csharp[Language Detection example](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=languageDetection)]

### <a name="output"></a>Output

```console
Language: English
```

---

## <a name="named-entity-recognition-ner"></a>Erkennung benannter Entitäten (NER)

#### <a name="version-30"></a>[Version 3.0](#tab/version-3)


> [!NOTE]
> Neu in Version `3.0`:
> * Die Entitätsverknüpfung ist nun von der Entitätserkennung getrennt.


Erstellen Sie eine neue Funktion namens `EntityRecognitionExample()`, die den zuvor erstellten Client verwendet, rufen Sie die zugehörige Funktion `RecognizeEntities()` auf, und durchlaufen Sie die Ergebnisse. Das zurückgegebene Objekt vom Typ `Response<IReadOnlyCollection<CategorizedEntity>>` enthält die Liste erkannter Entitäten. Im Falle eines Fehlers wird ein Fehler vom Typ `RequestFailedException` ausgelöst.

```csharp
static void EntityRecognitionExample(TextAnalyticsClient client)
{
    var response = client.RecognizeEntities("I had a wonderful trip to Seattle last week.");
    Console.WriteLine("Named Entities:");
    foreach (var entity in response.Value)
    {
        Console.WriteLine($"\tText: {entity.Text},\tCategory: {entity.Category},\tSub-Category: {entity.SubCategory}");
        Console.WriteLine($"\t\tScore: {entity.ConfidenceScore:F2}\n");
    }
}
```

### <a name="output"></a>Output

```console
Named Entities:
        Text: trip,     Category: Event,        Sub-Category:
                Score: 0.61

        Text: Seattle,  Category: Location,     Sub-Category: GPE
                Score: 0.82

        Text: last week,        Category: DateTime,     Sub-Category: DateRange
                Score: 0.80
```

## <a name="entity-linking"></a>Entitätsverknüpfung

Erstellen Sie eine neue Funktion namens `EntityLinkingExample()`, die den zuvor erstellten Client verwendet, rufen Sie die zugehörige Funktion `RecognizeLinkedEntities()` auf, und durchlaufen Sie die Ergebnisse. Die Rückgabe (`Response<IReadOnlyCollection<LinkedEntity>>`) stellt die Liste der erkannten Entitäten dar. Im Falle eines Fehlers wird ein Fehler vom Typ `RequestFailedException` ausgelöst. Da verknüpfte Entitäten eindeutig identifiziert werden, werden Vorkommen der gleichen Entität unter einem Objekt vom Typ `LinkedEntity` als Liste von Objekten des Typs `LinkedEntityMatch` gruppiert.

```csharp
static void EntityLinkingExample(TextAnalyticsClient client)
{
    var response = client.RecognizeLinkedEntities(
        "Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, " +
        "to develop and sell BASIC interpreters for the Altair 8800. " +
        "During his career at Microsoft, Gates held the positions of chairman, " +
        "chief executive officer, president and chief software architect, " +
        "while also being the largest individual shareholder until May 2014.");
    Console.WriteLine("Linked Entities:");
    foreach (var entity in response.Value)
    {
        Console.WriteLine($"\tName: {entity.Name},\tID: {entity.DataSourceEntityId},\tURL: {entity.Url}\tData Source: {entity.DataSource}");
        Console.WriteLine("\tMatches:");
        foreach (var match in entity.Matches)
        {
            Console.WriteLine($"\t\tText: {match.Text}");
            Console.WriteLine($"\t\tScore: {match.ConfidenceScore:F2}\n");
        }
    }
}
```

### <a name="output"></a>Output

```console
Linked Entities:
        Name: Altair 8800,      ID: Altair 8800,        URL: https://en.wikipedia.org/wiki/Altair_8800  Data Source: Wikipedia
        Matches:
                Text: Altair 8800
                Score: 0.88

        Name: Bill Gates,       ID: Bill Gates, URL: https://en.wikipedia.org/wiki/Bill_Gates   Data Source: Wikipedia
        Matches:
                Text: Bill Gates
                Score: 0.63

                Text: Gates
                Score: 0.63

        Name: Paul Allen,       ID: Paul Allen, URL: https://en.wikipedia.org/wiki/Paul_Allen   Data Source: Wikipedia
        Matches:
                Text: Paul Allen
                Score: 0.60

        Name: Microsoft,        ID: Microsoft,  URL: https://en.wikipedia.org/wiki/Microsoft    Data Source: Wikipedia
        Matches:
                Text: Microsoft
                Score: 0.55

                Text: Microsoft
                Score: 0.55

        Name: April 4,  ID: April 4,    URL: https://en.wikipedia.org/wiki/April_4      Data Source: Wikipedia
        Matches:
                Text: April 4
                Score: 0.32

        Name: BASIC,    ID: BASIC,      URL: https://en.wikipedia.org/wiki/BASIC        Data Source: Wikipedia
        Matches:
                Text: BASIC
                Score: 0.33
```

#### <a name="version-21"></a>[Version 2.1](#tab/version-2)

> [!NOTE]
> In Version 2.1 ist die Entitätsverknüpfung in der NER-Antwort enthalten.

Erstellen Sie eine neue Funktion namens `RecognizeEntitiesExample()`, die den zuvor erstellten Client verwendet, und rufen Sie die zugehörige Funktion [Entities()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.entities?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Language_TextAnalytics_TextAnalyticsClientExtensions_Entities_Microsoft_Azure_CognitiveServices_Language_TextAnalytics_ITextAnalyticsClient_System_String_System_String_System_Nullable_System_Boolean__System_Threading_CancellationToken_) auf. Durchlaufen Sie die Ergebnisse. Das zurückgegebene Objekt vom Typ [EntitiesResult](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.models.entitiesresult?view=azure-dotnet) enthält bei erfolgreicher Ausführung die Liste der erkannten Entitäten in `Entities` und andernfalls `errorMessage`. Geben Sie für jede erkannte Entität ihren Typ, Untertyp, Wikipedia-Namen (sofern vorhanden) sowie die Fundorte im Originaltext aus.

[!code-csharp[Entity Recognition example](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=entityRecognition)]

--- 


## <a name="key-phrase-extraction"></a>Schlüsselwortextraktion

#### <a name="version-30"></a>[Version 3.0](#tab/version-3)

Erstellen Sie eine neue Funktion namens `KeyPhraseExtractionExample()`, die den zuvor erstellten Client verwendet, und rufen Sie die zugehörige Funktion `ExtractKeyPhrases()` auf. Das zurückgegebene Objekt vom Typ `<Response<IReadOnlyCollection<string>>` enthält die Liste erkannter Schlüsselbegriffe. Im Falle eines Fehlers wird ein Fehler vom Typ `RequestFailedException` ausgelöst.

```csharp
static void KeyPhraseExtractionExample(TextAnalyticsClient client)
{
    var response = client.ExtractKeyPhrases("My cat might need to see a veterinarian.");

    // Printing key phrases
    Console.WriteLine("Key phrases:");

    foreach (string keyphrase in response.Value)
    {
        Console.WriteLine($"\t{keyphrase}");
    }
}
```

### <a name="output"></a>Output

```console
Key phrases:
    cat
    veterinarian
```

#### <a name="version-21"></a>[Version 2.1](#tab/version-2)

Erstellen Sie eine neue Funktion namens `KeyPhraseExtractionExample()`, die den zuvor erstellten Client verwendet, und rufen Sie die zugehörige Funktion [KeyPhrases()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.keyphrases?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Language_TextAnalytics_TextAnalyticsClientExtensions_KeyPhrases_Microsoft_Azure_CognitiveServices_Language_TextAnalytics_ITextAnalyticsClient_System_String_System_String_System_Nullable_System_Boolean__System_Threading_CancellationToken_) auf. Das Ergebnis enthält bei erfolgreicher Ausführung die Liste der erkannten Schlüsselbegriffe in `KeyPhrases` und andernfalls `errorMessage`. Geben Sie alle erkannten Schlüsselbegriffe aus.

[!code-csharp[Key phrase extraction example](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=keyPhraseExtraction)]


### <a name="output"></a>Output

```console
Key phrases:
    cat
    veterinarian
```

---
