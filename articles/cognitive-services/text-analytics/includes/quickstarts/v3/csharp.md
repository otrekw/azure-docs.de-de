---
title: 'Schnellstart: Textanalyse-Clientbibliothek v3 für C# | Microsoft-Dokumentation'
description: Hier finden Sie Informationen zu den ersten Schritten mit der Textanalyse-Clientbibliothek v3 für C#.
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 01/15/2020
ms.author: aahi
ms.reviewer: assafi
ms.openlocfilehash: 2cf9a006e1f6f1edb996aa9beaf8934a14af29df
ms.sourcegitcommit: d9ec6e731e7508d02850c9e05d98d26c4b6f13e6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/20/2020
ms.locfileid: "76281153"
---
<a name="HOLTop"></a>

[Referenzdokumentation](https://aka.ms/azsdk-net-textanalytics-ref-docs) | [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/textanalytics) | [Paket (NuGet)](https://www.nuget.org/packages/Azure.AI.TextAnalytics) | [Beispiele](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/textanalytics/Azure.AI.TextAnalytics/samples)

> [!NOTE]
> * In dieser Schnellstartanleitung wird die Version `3.0-preview` der Textanalyse-Clientbibliothek verwendet. Diese beinhaltet eine Public Preview für verbesserte [Standpunktanalyse](../../../how-tos/text-analytics-how-to-sentiment-analysis.md#sentiment-analysis-versions-and-features) und [Erkennung benannter Entitäten (Named Entity Recognition, NER)](../../../how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-versions-and-features).
> * Im Code dieses Artikels werden der Einfachheit halber synchrone Methoden und ein ungeschützter Anmeldeinformationsspeicher verwendet. Für Produktionsszenarien wird aus Leistungs- und Skalierbarkeitsgründen die Verwendung der asynchronen Batchmethoden empfohlen. Dazu muss beispielsweise `AnalyzeSentimentAsync()` anstelle von `AnalyzeSentiment()`aufgerufen werden.

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/)
* [Visual Studio-IDE](https://visualstudio.microsoft.com/vs/)

## <a name="setting-up"></a>Einrichten

### <a name="create-a-text-analytics-azure-resource"></a>Erstellen einer Azure-Ressource für die Textanalyse

[!INCLUDE [text-analytics-resource-creation](../resource-creation.md)]

### <a name="create-a-new-net-core-application"></a>Erstellen einer neuen .NET Core-Anwendung

Erstellen Sie über die Visual Studio-IDE eine neue .NET Core-Konsolenanwendung. Dadurch wird ein Projekt vom Typ „Hallo Welt“ mit einer einzelnen C#-Quelldatei (*program.cs*) erstellt.

Installieren Sie die Clientbibliothek, indem Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf die Projektmappe klicken und **NuGet-Pakete verwalten** auswählen. Wählen Sie im daraufhin geöffneten Paket-Manager die Option **Durchsuchen** aus, aktivieren Sie das Kontrollkästchen **Vorabversion einbeziehen**, und suchen Sie nach `Azure.AI.TextAnalytics`. Klicken Sie darauf, und klicken Sie dann auf **Installieren**. Sie können auch die [Paket-Manager-Konsole](https://docs.microsoft.com/nuget/consume-packages/install-use-packages-powershell#find-and-install-a-package) verwenden.

Öffnen Sie die Datei *program.cs*, und fügen Sie die folgenden `using`-Anweisungen hinzu:

```csharp
using System;
using Azure.AI.TextAnalytics;
```

Erstellen Sie in der `Program`-Klasse der Anwendung Variablen für den Schlüssel und Endpunkt Ihrer Ressource.

[!INCLUDE [text-analytics-find-resource-information](../../find-azure-resource-info.md)]

```csharp
private static readonly string key = "<replace-with-your-text-analytics-key-here>";
private static readonly Uri endpoint = new Uri("<replace-with-your-text-analytics-endpoint-here>");
```

Ersetzen Sie die `Main`-Methode der Anwendung. Die hier aufgerufenen Methoden definieren Sie später.

```csharp
static void Main(string[] args)
{
    var client = new TextAnalyticsClient(endpoint, key);

    SentimentAnalysisExample(client);
    LanguageDetectionExample(client);
    EntityRecognitionExample(client);
    EntityPIIExample(client);
    EntityLinkingExample(client);
    KeyPhraseExtractionExample(client);

    Console.Write("Press any key to exit.");
    Console.ReadKey();
}
```

## <a name="object-model"></a>Objektmodell

Der Textanalyse-Client ist ein Objekt vom Typ `TextAnalyticsClient`, das sich mit Ihrem Schlüssel bei Azure authentifiziert und Funktionen bereitstellt, um Text als einzelne Zeichenfolgen oder als Batch zu akzeptieren. Sie können Text synchron oder asynchron an die API senden. Das Antwortobjekt enthält die Analyseinformationen für die einzelnen gesendeten Dokumente. Eine optionale `TextAnalyticsClientOptions`-Instanz kann verwendet werden, um den Client mit verschiedenen Standardeinstellungen wie Standardsprache oder Länderhinweis zu initialisieren.

Weitere Beispiele (einschließlich AAD-Authentifizierung und Verwendung von Clientstandardeinstellungen) finden Sie [hier](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples).

## <a name="code-examples"></a>Codebeispiele

* [Standpunktanalyse](#sentiment-analysis) (Public Preview)
* [Sprachenerkennung](#language-detection)
* [Erkennung benannter Entitäten](#named-entity-recognition-public-preview) (Public Preview)
* [Erkennung benannter Entitäten – persönliche Informationen](#named-entity-recognition---personal-information-public-preview) (Public Preview)
* [Entitätsverknüpfung](#entity-linking)
* [Schlüsselbegriffserkennung](#key-phrase-extraction)

Rufen Sie in der `main()`-Methode des Programms die Authentifizierungsmethode auf, um den Client zu instanziieren.

## <a name="sentiment-analysis-public-preview"></a>Standpunktanalyse (Public Preview)

> [!NOTE]
> Der folgende Code gilt für Version 3 der Standpunktanalyse, die sich momentan in der Public Preview-Phase befindet.

Erstellen Sie eine neue Funktion namens `SentimentAnalysisExample()`, die den zuvor erstellten Client verwendet, und rufen Sie die zugehörige Funktion `AnalyzeSentiment()` auf. Das zurückgegebene Objekt vom Typ `Response<AnalyzeSentimentResult>` enthält die Stimmungsbezeichnung und den Stimmungswert des gesamten Eingabedokuments sowie eine Standpunktanalyse für jeden Satz (sofern erfolgreich). Andernfalls enthält es eine Fehlermeldung (`Value.ErrorMessage`).

```csharp
static void SentimentAnalysisExample(TextAnalyticsClient client)
{
    var response = client.AnalyzeSentiment("I had the best day of my life. I wish you were there with me.");
    Console.WriteLine($"Document sentiment: {response.Value.DocumentSentiment.SentimentClass}\n");
    foreach (var sentence in response.Value.SentenceSentiments)
    {
        Console.WriteLine($"\tSentence [offset {sentence.Offset}, length {sentence.Length}]");
        Console.WriteLine($"\tSentence sentiment: {sentence.SentimentClass}");
        Console.WriteLine($"\tPositive score: {sentence.PositiveScore:0.00}");
        Console.WriteLine($"\tNegative score: {sentence.NegativeScore:0.00}");
        Console.WriteLine($"\tNeutral score: {sentence.NeutralScore:0.00}\n");
    }
}
```

### <a name="output"></a>Ausgabe

```console
Document sentiment: Positive

        Sentence [offset 0, length 30]
        Sentence sentiment: Positive
        Positive score: 1.00
        Negative score: 0.00
        Neutral score: 0.00

        Sentence [offset 31, length 30]
        Sentence sentiment: Neutral
        Positive score: 0.21
        Negative score: 0.02
        Neutral score: 0.77
```

## <a name="language-detection"></a>Spracherkennung

Erstellen Sie eine neue Funktion namens `LanguageDetectionExample()`, die den zuvor erstellten Client verwendet, und rufen Sie die zugehörige Funktion `DetectLanguage()` auf. Das zurückgegebene Objekt vom Typ `Response<DetectLanguageResult>` enthält die erkannte Sprache in `Value.PrimaryLanguage` (sofern erfolgreich). Andernfalls enthält es eine Fehlermeldung (`Value.ErrorMessage`).

> [!Tip]
> In einigen Fällen ist es unter Umständen schwierig, die Sprachen basierend auf den Eingaben eindeutig zu bestimmen. Mithilfe des Parameters `countryHint` können Sie einen zweistelligen Ländercode angeben. Die API verwendet standardmäßig „US“ als Wert für „countryHint“. Wenn Sie dieses Verhalten entfernen möchten, können Sie diesen Parameter zurücksetzen, indem Sie seinen Wert auf eine leere Zeichenfolge (`countryHint = ""`) festlegen. Wenn Sie einen anderen Standardwert festlegen möchten, legen Sie die Eigenschaft `TextAnalyticsClientOptions.DefaultCountryHint` fest, und übergeben Sie sie während der Initialisierung des Clients.

```csharp
static void LanguageDetectionExample(TextAnalyticsClient client)
{
    var response = client.DetectLanguage("Ce document est rédigé en Français.");
    var detectedLanguage = response.Value.PrimaryLanguage;
    Console.WriteLine("Language:");
    Console.WriteLine($"\t{detectedLanguage.Name},\tISO-6391: {detectedLanguage.Iso6391Name}\n");
}
```

### <a name="output"></a>Ausgabe

```console
Language:
        French, ISO-6391: fr
```

## <a name="named-entity-recognition-public-preview"></a>Erkennung benannter Entitäten (Public Preview)

> [!NOTE]
> Der folgende Code gilt für Version 3 der Erkennung benannter Entitäten, die sich momentan in der Public Preview-Phase befindet.

Erstellen Sie eine neue Funktion namens `EntityRecognitionExample()`, die den zuvor erstellten Client verwendet, rufen Sie die zugehörige Funktion `RecognizeEntities()` auf, und durchlaufen Sie die Ergebnisse. Das zurückgegebene Objekt vom Typ `Response<RecognizeEntitiesResult>` enthält die Liste erkannter Entitäten in `Value.NamedEntities` (sofern erfolgreich). Andernfalls enthält es eine Fehlermeldung (`Value.ErrorMessage`). Geben Sie für die erkannten Entitäten jeweils den Typ und den Untertyp aus (sofern vorhanden).

```csharp
static void EntityRecognitionExample(TextAnalyticsClient client)
{
    var response = client.RecognizeEntities("I had a wonderful trip to Seattle last week.");
    Console.WriteLine("Named Entities:");
    foreach(var entity in response.Value.NamedEntities)
    {
        Console.WriteLine($"\tText: {entity.Text},\tType: {entity.Type},\tSub-Type: {entity.SubType ?? "N/A"}");
        Console.WriteLine($"\t\tOffset: {entity.Offset},\tLength: {entity.Length},\tScore: {entity.Score:F3}\n");
    }
}
```

### <a name="output"></a>Ausgabe

```console
Named Entities:
        Text: Seattle,  Type: Location, Sub-Type: N/A
                Offset: 26,     Length: 7,      Score: 0.806

        Text: last week,        Type: DateTime, Sub-Type: N/A
                Offset: 34,     Length: 9,      Score: 0.800
```

## <a name="named-entity-recognition---personal-information-public-preview"></a>Erkennung benannter Entitäten – persönliche Informationen (Public Preview)

> [!NOTE]
> Der folgende Code gilt für die Erkennung persönlicher Informationen mithilfe von Version 3 der Erkennung benannter Entitäten, die sich momentan in der Public Preview-Phase befindet.

Erstellen Sie eine neue Funktion namens `EntityPIIExample()`, die den zuvor erstellten Client verwendet, rufen Sie die zugehörige Funktion `RecognizePiiEntities()` auf, und durchlaufen Sie die Ergebnisse. Das zurückgegebene Objekt vom Typ `Response<RecognizeEntitiesResult>` enthält ähnlich wie bei der vorherigen Funktion die Liste erkannter Entitäten in `Value.NamedEntities` (sofern erfolgreich). Andernfalls enthält es eine Fehlermeldung (`Value.ErrorMessage`).

```csharp
static void EntityPIIExample(TextAnalyticsClient client)
{
    var response = client.RecognizePiiEntities("Insurance policy for SSN on file 123-12-1234 is here by approved.");
    Console.WriteLine("Personally Identifiable Information Entities:");
    foreach(var entity in response.Value.NamedEntities)
    {
        Console.WriteLine($"\tText: {entity.Text},\tType: {entity.Type},\tSub-Type: {entity.SubType ?? "N/A"}");
        Console.WriteLine($"\t\tOffset: {entity.Offset},\tLength: {entity.Length},\tScore: {entity.Score:F3}\n");
    }
}
```

### <a name="output"></a>Ausgabe

```console
Personally Identifiable Information Entities:
        Text: 123-12-1234,      Type: U.S. Social Security Number (SSN),        Sub-Type: N/A
                Offset: 33,     Length: 11,     Score: 0.850
```

## <a name="entity-linking"></a>Entitätsverknüpfung

Erstellen Sie eine neue Funktion namens `EntityLinkingExample()`, die den zuvor erstellten Client verwendet, rufen Sie die zugehörige Funktion `RecognizeLinkedEntities()` auf, und durchlaufen Sie die Ergebnisse. Das zurückgegebene Objekt vom Typ `Response<RecognizeLinkedEntitiesResult>` enthält die Liste erkannter Entitäten in `Value.LinkedEntities` (sofern erfolgreich). Andernfalls enthält es eine Fehlermeldung (`Value.ErrorMessage`). Da verknüpfte Entitäten eindeutig identifiziert werden, werden Vorkommen der gleichen Entität unter einem Objekt vom Typ `LinkedEntity` als Liste von Objekten des Typs `LinkedEntityMatch` gruppiert.

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
    foreach (var entity in response.Value.LinkedEntities)
    {
        Console.WriteLine($"\tName: {entity.Name},\tID: {entity.Id},\tURL: {entity.Uri}\tData Source: {entity.DataSource}");
        Console.WriteLine("\tMatches:");
        foreach (var match in entity.Matches)
        {
            Console.WriteLine($"\t\tText: {match.Text}");
            Console.WriteLine($"\t\tOffset: {match.Offset},\tLength: {match.Length},\tScore: {match.Score:F3}\n");
        }
    }
}
```

### <a name="output"></a>Ausgabe

```console
Linked Entities:
        Name: Altair 8800,      ID: Altair 8800,        URL: https://en.wikipedia.org/wiki/Altair_8800  Data Source: Wikipedia
        Matches:
                Text: Altair 8800
                Offset: 116,    Length: 11,     Score: 0.777

        Name: Bill Gates,       ID: Bill Gates, URL: https://en.wikipedia.org/wiki/Bill_Gates   Data Source: Wikipedia
        Matches:
                Text: Bill Gates
                Offset: 25,     Length: 10,     Score: 0.555

                Text: Gates
                Offset: 161,    Length: 5,      Score: 0.555

        Name: Paul Allen,       ID: Paul Allen, URL: https://en.wikipedia.org/wiki/Paul_Allen   Data Source: Wikipedia
        Matches:
                Text: Paul Allen
                Offset: 40,     Length: 10,     Score: 0.533

        Name: Microsoft,        ID: Microsoft,  URL: https://en.wikipedia.org/wiki/Microsoft    Data Source: Wikipedia
        Matches:
                Text: Microsoft
                Offset: 0,      Length: 9,      Score: 0.469

                Text: Microsoft
                Offset: 150,    Length: 9,      Score: 0.469

        Name: April 4,  ID: April 4,    URL: https://en.wikipedia.org/wiki/April_4      Data Source: Wikipedia
        Matches:
                Text: April 4
                Offset: 54,     Length: 7,      Score: 0.248

        Name: BASIC,    ID: BASIC,      URL: https://en.wikipedia.org/wiki/BASIC        Data Source: Wikipedia
        Matches:
                Text: BASIC
                Offset: 89,     Length: 5,      Score: 0.281
```

## <a name="key-phrase-extraction"></a>Schlüsselwortextraktion

Erstellen Sie eine neue Funktion namens `KeyPhraseExtractionExample()`, die den zuvor erstellten Client verwendet, und rufen Sie die zugehörige Funktion `ExtractKeyPhrases()` auf. Das Ergebnis enthält die Liste erkannter Schlüsselbegriffe in `Value.KeyPhrases` (sofern erfolgreich). Andernfalls enthält es eine Fehlermeldung (`Value.ErrorMessage`). Geben Sie alle erkannten Schlüsselbegriffe aus.

```csharp
static void KeyPhraseExtractionExample(TextAnalyticsClient client)
{
    var response = client.ExtractKeyPhrases("My cat might need to see a veterinarian.");

    // Printing key phrases
    Console.WriteLine("Key phrases:");

    foreach (string keyphrase in response.Value.KeyPhrases)
    {
        Console.WriteLine($"\t{keyphrase}");
    }
}
```

### <a name="output"></a>Ausgabe

```console
Key phrases:
    cat
    veterinarian
```
