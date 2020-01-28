---
title: 'Schnellstart: Textanalyse-Clientbibliothek v3 für Java | Microsoft-Dokumentation'
description: Hier finden Sie Informationen zu den ersten Schritten mit der Textanalyse-Clientbibliothek v3 für Java.
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 01/15/2020
ms.author: aahi
ms.reviewer: tasharm, assafi
ms.openlocfilehash: a7882f1337e990f828bea15cdc2b9ad72de44f18
ms.sourcegitcommit: d9ec6e731e7508d02850c9e05d98d26c4b6f13e6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/20/2020
ms.locfileid: "76281155"
---
<a name="HOLTop"></a>

[Referenzdokumentation](https://aka.ms/azsdk-java-textanalytics-ref-docs) | [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/textanalytics/azure-ai-textanalytics) | [Paket (Maven)](https://oss.sonatype.org/#nexus-search;quick~com.azure) | [Beispiele](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/textanalytics/azure-ai-textanalytics/src/samples/java/com/azure/ai/textanalytics)

> [!NOTE]
> * In dieser Schnellstartanleitung wird die Version `3.0-preview` der Textanalyse-Clientbibliothek verwendet. Diese beinhaltet eine Public Preview für verbesserte [Standpunktanalyse](../../../how-tos/text-analytics-how-to-sentiment-analysis.md#sentiment-analysis-versions-and-features) und [Erkennung benannter Entitäten (Named Entity Recognition, NER)](../../../how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-versions-and-features).
> * Im Code dieses Artikels werden der Einfachheit halber synchrone Methoden und ein ungeschützter Anmeldeinformationsspeicher verwendet. Für Produktionsszenarien wird aus Leistungs- und Skalierbarkeitsgründen die Verwendung der asynchronen Batchmethoden empfohlen. Dazu muss beispielsweise `SentimentBatchAsync()` anstelle von `Sentiment()`aufgerufen werden.

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/)
* [Java Development Kit (JDK)](https://www.oracle.com/technetwork/java/javase/downloads/index.html), Version 8 oder höher

## <a name="setting-up"></a>Einrichten

### <a name="create-a-text-analytics-azure-resource"></a>Erstellen einer Azure-Ressource für die Textanalyse

[!INCLUDE [text-analytics-resource-creation](../resource-creation.md)]

### <a name="create-a-new-maven-project"></a>Erstellen eines neuen Maven-Projekts

Fügen Sie Ihrem Projekt die folgende Textanalyseabhängigkeit hinzu. Diese Version der Abhängigkeit verwendet die Version `3.0-preview` der Textanalyse-API. 

```xml
<dependencies>
    <dependency>
        <groupId>com.azure</groupId>
        <artifactId>azure-ai-textanalytics</artifactId>
        <version>1.0.0-beta.1</version>
    </dependency>
</dependencies>
```

Erstellen Sie eine neue Java-Datei im folgenden Verzeichnis: `\src\main\java`.

Öffnen Sie die Java-Datei, und fügen Sie die folgenden `import`-Anweisungen hinzu:

```java
import com.azure.ai.textanalytics.models.AnalyzeSentimentResult;
import com.azure.ai.textanalytics.models.DetectLanguageResult;
import com.azure.ai.textanalytics.models.DetectedLanguage;
import com.azure.ai.textanalytics.models.ExtractKeyPhraseResult;
import com.azure.ai.textanalytics.models.LinkedEntity;
import com.azure.ai.textanalytics.models.LinkedEntityMatch;
import com.azure.ai.textanalytics.models.NamedEntity;
import com.azure.ai.textanalytics.models.RecognizeEntitiesResult;
import com.azure.ai.textanalytics.models.RecognizeLinkedEntitiesResult;
import com.azure.ai.textanalytics.models.RecognizePiiEntitiesResult;
import com.azure.ai.textanalytics.models.TextSentiment;
import com.azure.ai.textanalytics.TextAnalyticsClientBuilder;
import com.azure.ai.textanalytics.TextAnalyticsClient;
import java.util.List;
```

Fügen Sie in der Java-Datei eine neue Klasse sowie den Schlüssel und den Endpunkt Ihrer Azure-Ressource hinzu, wie weiter unten zu sehen.

[!INCLUDE [text-analytics-find-resource-information](../../find-azure-resource-info.md)]

```java
public class TextAnalyticsSample {
    private static String KEY = "<replace-with-your-text-analytics-key-here>";
    private static String ENDPOINT = "<replace-with-your-text-analytics-endpoint-here>";
}
```

Fügen Sie der Klasse die folgende main-Methode hinzu. Die hier aufgerufenen Methoden definieren Sie später.

```java
public static void main(String[] args) {
    
    TextAnalyticsClient client = authenticateClient(KEY, ENDPOINT);
    
    sentimentAnalysisExample(client);
    detectLanguageExample(client);
    recognizeEntitiesExample(client);
    recognizePIIEntitiesExample(client);
    recognizeLinkedEntitiesExample(client);
    extractKeyPhrasesExample(client);
}
```

## <a name="object-model"></a>Objektmodell

Der Textanalyse-Client ist ein Objekt vom Typ `TextAnalyticsClient`, das sich mit Ihrem Schlüssel bei Azure authentifiziert und Funktionen bereitstellt, um Text als einzelne Zeichenfolgen oder als Batch zu akzeptieren. Sie können Text synchron oder asynchron an die API senden. Das Antwortobjekt enthält die Analyseinformationen für die einzelnen gesendeten Dokumente. 

## <a name="code-examples"></a>Codebeispiele

* [Authentifizieren des Clients](#authenticate-the-client)
* [Standpunktanalyse](#sentiment-analysis) (Public Preview)
* [Sprachenerkennung](#language-detection)
* [Erkennung benannter Entitäten](#named-entity-recognition-public-preview) (Public Preview)
* [Erkennung benannter Entitäten – persönliche Informationen](#named-entity-recognition---personal-information-public-preview) (Public Preview)
* [Entitätsverknüpfung](#entity-linking)
* [Schlüsselbegriffserkennung](#key-phrase-extraction)

## <a name="authenticate-the-client"></a>Authentifizieren des Clients

Erstellen Sie eine Methode, um das Objekt `TextAnalyticsClient`mit dem oben erstellten Schlüssel (`KEY`) und Endpunkt (`ENDPOINT`) zu instanziieren.

```java
static TextAnalyticsClient authenticateClient(String subscriptionKey, String endpoint) {
    return new TextAnalyticsClientBuilder()
    .subscriptionKey(subscriptionKey)
    .endpoint(endpoint)
    .buildClient();
}
```

Rufen Sie in der `main()`-Methode des Programms die Authentifizierungsmethode auf, um den Client zu instanziieren.

## <a name="sentiment-analysis"></a>Stimmungsanalyse

> [!NOTE]
> Der folgende Code gilt für Version 3 der Standpunktanalyse, die sich momentan in der Public Preview-Phase befindet.

Erstellen Sie eine neue Funktion namens `sentimentAnalysisExample()`, die den zuvor erstellten Client verwendet, und rufen Sie die zugehörige Funktion `analyzeSentiment()` auf. Das zurückgegebene Objekt `AnalyzeSentimentResult` enthält `documentSentiment` und `sentenceSentiments` (sofern erfolgreich). Andernfalls enthält es eine Fehlermeldung (`errorMessage`). 

```java
static void sentimentAnalysisExample(TextAnalyticsClient client)
{
    // The text that need be analyzed.
    String text = "I had the best day of my life. I wish you were there with me.";

    AnalyzeSentimentResult sentimentResult = client.analyzeSentiment(text);
    TextSentiment documentSentiment = sentimentResult.getDocumentSentiment();
    System.out.printf(
        "Recognized TextSentiment: %s, Positive Score: %.2f, Neutral Score: %.2f, Negative Score: %.2f.%n",
        documentSentiment.getTextSentimentClass(),
        documentSentiment.getPositiveScore(),
        documentSentiment.getNeutralScore(),
        documentSentiment.getNegativeScore());

    List<TextSentiment> sentiments = sentimentResult.getSentenceSentiments();
    for (TextSentiment textSentiment : sentiments) {
        System.out.printf(
            "Recognized Sentence TextSentiment: %s, Positive Score: %.2f, Neutral Score: %.2f, Negative Score: %.2f.%n",
            textSentiment.getTextSentimentClass(),
            textSentiment.getPositiveScore(),
            textSentiment.getNeutralScore(),
            textSentiment.getNegativeScore());
    }
}
```

### <a name="output"></a>Ausgabe

```console
Recognized TextSentiment: positive, Positive Score: 1.00, Neutral Score: 0.00, Negative Score: 0.00.
Recognized Sentence TextSentiment: positive, Positive Score: 1.00, Neutral Score: 0.00, Negative Score: 0.00.
Recognized Sentence TextSentiment: neutral, Positive Score: 0.21, Neutral Score: 0.77, Negative Score: 0.02.
```
## <a name="language-detection"></a>Spracherkennung

Erstellen Sie eine neue Funktion namens `detectLanguageExample()`, die den zuvor erstellten Client verwendet, und rufen Sie die zugehörige Funktion `detectLanguage()` auf. Das zurückgegebene Objekt `DetectLanguageResult` enthält eine primäre erkannte Sprache und eine Liste mit anderen erkannten Sprachen (sofern erfolgreich). Andernfalls enthält es eine Fehlermeldung (`errorMessage`).

> [!Tip]
> In einigen Fällen ist es unter Umständen schwierig, die Sprachen basierend auf den Eingaben eindeutig zu bestimmen. Mithilfe des Parameters `countryHint` können Sie einen zweistelligen Ländercode angeben. Die API verwendet standardmäßig „US“ als Wert für „countryHint“. Wenn Sie dieses Verhalten entfernen möchten, können Sie diesen Parameter zurücksetzen, indem Sie seinen Wert auf eine leere Zeichenfolge (`countryHint = ""`) festlegen. Wenn Sie einen anderen Standardwert festlegen möchten, legen Sie die Eigenschaft `TextAnalyticsClientOptions.DefaultCountryHint` fest, und übergeben Sie sie während der Initialisierung des Clients.

```java
static void detectLanguageExample(TextAnalyticsClient client)
{
    // The text that need be analyzed.
    String text = "Ce document est rédigé en Français.";

    DetectLanguageResult detectLanguageResult = client.detectLanguage(text, "US");
    DetectedLanguage detectedDocumentLanguage = detectLanguageResult.getPrimaryLanguage();
    System.out.printf("Language: %s, ISO 6391 Name: %s, Score: %s.%n",
        detectedDocumentLanguage.getName(),
        detectedDocumentLanguage.getIso6391Name(),
        detectedDocumentLanguage.getScore());
}
```

### <a name="output"></a>Ausgabe

```console
Language: French, ISO 6391 Name: fr, Score: 1.0.
```
## <a name="named-entity-recognition-public-preview"></a>Erkennung benannter Entitäten (Public Preview)

> [!NOTE]
> Der folgende Code gilt für Version 3 der Erkennung benannter Entitäten, die sich momentan in der Public Preview-Phase befindet.

Erstellen Sie eine neue Funktion namens `recognizeEntitiesExample()`, die den zuvor erstellten Client verwendet, und rufen Sie die zugehörige Funktion `recognizeEntities()` auf. Das zurückgegebene Objekt `RecognizeEntitiesResult` enthält eine Liste mit Elementen vom Typ `NamedEntity` (sofern erfolgreich). Andernfalls enthält es eine Fehlermeldung (`errorMessage`).

```java
static void recognizeEntitiesExample(TextAnalyticsClient client)
{
    // The text that need be analysed.
    String text = "I had a wonderful trip to Seattle last week.";
    
    RecognizeEntitiesResult recognizeEntitiesResult = client.recognizeEntities(text);

    for (NamedEntity entity : recognizeEntitiesResult.getNamedEntities()) {
        System.out.printf(
            "Recognized NamedEntity Text: %s, Type: %s, Subtype: %s, Offset: %s, Length: %s, Score: %.3f.%n",
            entity.getText(),
            entity.getType(),
            entity.getSubtype() == null || entity.getSubtype().isEmpty() ? "N/A" : entity.getSubtype(),
            entity.getOffset(),
            entity.getLength(),
            entity.getScore());
    }
}
```

### <a name="output"></a>Ausgabe

```console
Recognized NamedEntity Text: Seattle, Type: Location, Subtype: N/A, Offset: 26, Length: 7, Score: 0.806.
Recognized NamedEntity Text: last week, Type: DateTime, Subtype: DateRange, Offset: 34, Length: 9, Score: 0.800.
```

## <a name="named-entity-recognition---personal-information-public-preview"></a>Erkennung benannter Entitäten – persönliche Informationen (Public Preview)

> [!NOTE]
> Der folgende Code gilt für die Erkennung persönlicher Informationen mithilfe von Version 3 der Erkennung benannter Entitäten, die sich momentan in der Public Preview-Phase befindet.

Erstellen Sie eine neue Funktion namens `recognizePIIEntitiesExample()`, die den zuvor erstellten Client verwendet, und rufen Sie die zugehörige Funktion `recognizePiiEntities()` auf. Das zurückgegebene Objekt `RecognizePiiEntitiesResult` enthält eine Liste mit Elementen vom Typ `NamedEntity` (sofern erfolgreich). Andernfalls enthält es eine Fehlermeldung (`errorMessage`). 

```java
static void recognizePIIEntitiesExample(TextAnalyticsClient client)
{
    // The text that need be analysed.
    String text = "Insurance policy for SSN on file 123-12-1234 is here by approved.";
    
    RecognizePiiEntitiesResult recognizePIIEntitiesResult = client.recognizePiiEntities(text);

    for (NamedEntity entity : recognizePIIEntitiesResult.getNamedEntities()) {
        System.out.printf(
            "Personally Identifiable Information Entities Text: %s, Type: %s, Subtype: %s, Offset: %s, Length: %s, Score: %s.%n",
            entity.getText(),
            entity.getType(),
            entity.getSubtype() == null || entity.getSubtype().isEmpty() ? "N/A" : entity.getSubtype(),
            entity.getOffset(),
            entity.getLength(),
            entity.getScore());
    }
}
```

### <a name="output"></a>Ausgabe

```console
Personally Identifiable Information Entities
Text: 123-12-1234, Type: U.S. Social Security Number (SSN), Subtype: N/A, Offset: 33, Length: 11, Score: 0.85.
```

## <a name="entity-linking"></a>Entitätsverknüpfung

Erstellen Sie eine neue Funktion namens `recognizeLinkedEntitiesExample()`, die den zuvor erstellten Client verwendet, und rufen Sie die zugehörige Funktion `recognizeLinkedEntities()` auf. Das zurückgegebene Objekt `RecognizeLinkedEntitiesResult` enthält eine Liste mit Elementen vom Typ `LinkedEntity` (sofern erfolgreich). Andernfalls enthält es eine Fehlermeldung (`errorMessage`). Da verknüpfte Entitäten eindeutig identifiziert werden, werden Vorkommen der gleichen Entität unter einem Objekt vom Typ `LinkedEntity` als Liste von Objekten des Typs `LinkedEntityMatch` gruppiert.

```java
static void recognizeLinkedEntitiesExample(TextAnalyticsClient client)
{
    // The text that need be analysed.
    String text = "Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, " +
            "to develop and sell BASIC interpreters for the Altair 8800. " +
            "During his career at Microsoft, Gates held the positions of chairman, " +
            "chief executive officer, president and chief software architect, " +
            "while also being the largest individual shareholder until May 2014.";
    
    RecognizeLinkedEntitiesResult recognizeLinkedEntitiesResult = client.recognizeLinkedEntities(text);

    System.out.printf("Linked Entities:%n");
    for (LinkedEntity linkedEntity : recognizeLinkedEntitiesResult.getLinkedEntities()) {
        System.out.printf("Name: %s, ID: %s, URL: %s, Data Source: %s.%n",
            linkedEntity.getName(),
            linkedEntity.getId(),
            linkedEntity.getUrl(),
            linkedEntity.getDataSource());
        System.out.printf("tMatches:%n");
        for (LinkedEntityMatch linkedEntityMatch : linkedEntity.getLinkedEntityMatches()) {
            System.out.printf("Text: %s, Offset: %s, Length: %s, Score: %.2f.%n",
                linkedEntityMatch.getText(),
                linkedEntityMatch.getOffset(),
                linkedEntityMatch.getLength(),
                linkedEntityMatch.getScore());
        }
    }
}
```

### <a name="output"></a>Ausgabe

```console
Linked Entities:
Name: Altair 8800, ID: Altair 8800, URL: https://en.wikipedia.org/wiki/Altair_8800, Data Source: Wikipedia.
tMatches:
Text: Altair 8800, Offset: 11, Length: 116, Score: 0.65.
Name: Bill Gates, ID: Bill Gates, URL: https://en.wikipedia.org/wiki/Bill_Gates, Data Source: Wikipedia.
tMatches:
Text: Bill Gates, Offset: 10, Length: 25, Score: 0.24.
Text: Gates, Offset: 5, Length: 161, Score: 0.24.
Name: Paul Allen, ID: Paul Allen, URL: https://en.wikipedia.org/wiki/Paul_Allen, Data Source: Wikipedia.
tMatches:
Text: Paul Allen, Offset: 10, Length: 40, Score: 0.17.
Name: Microsoft, ID: Microsoft, URL: https://en.wikipedia.org/wiki/Microsoft, Data Source: Wikipedia.
tMatches:
Text: Microsoft, Offset: 9, Length: 0, Score: 0.20.
Text: Microsoft, Offset: 9, Length: 150, Score: 0.20.
Name: April 4, ID: April 4, URL: https://en.wikipedia.org/wiki/April_4, Data Source: Wikipedia.
tMatches:
Text: April 4, Offset: 7, Length: 54, Score: 0.14.
Name: BASIC, ID: BASIC, URL: https://en.wikipedia.org/wiki/BASIC, Data Source: Wikipedia.
tMatches:
Text: BASIC, Offset: 5, Length: 89, Score: 0.05.
```
## <a name="key-phrase-extraction"></a>Schlüsselwortextraktion

Erstellen Sie eine neue Funktion namens `extractKeyPhrasesExample()`, die den zuvor erstellten Client verwendet, und rufen Sie die zugehörige Funktion `extractKeyPhrases()` auf. Das zurückgegebene Objekt `ExtractKeyPhraseResult` enthält eine Liste mit Schlüsselbegriffen (sofern erfolgreich). Andernfalls enthält es eine Fehlermeldung (`errorMessage`).

```java
static void extractKeyPhrasesExample(TextAnalyticsClient client)
{
    // The text that need be analyzed.
    String text = "My cat might need to see a veterinarian.";
    
    ExtractKeyPhraseResult keyPhraseResult = client.extractKeyPhrases(text);

    for (String keyPhrase : keyPhraseResult.getKeyPhrases()) {
        System.out.printf("Recognized Phrases: %s.%n", keyPhrase);
    }
}
```

### <a name="output"></a>Ausgabe

```console
Recognized Phrases: cat.
Recognized Phrases: veterinarian.
```
