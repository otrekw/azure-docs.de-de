---
author: aahill
ms.service: cognitive-services
ms.topic: include
ms.date: 04/13/2020
ms.author: aahi
ms.openlocfilehash: d58f294195efc393c07ecc3886c29e33dba02e6d
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/15/2020
ms.locfileid: "81422151"
---
<a name="HOLTop"></a>

#### <a name="version-30-preview"></a>[Version 3.0-preview](#tab/version-3)

[v3-Referenzdokumentation](https://aka.ms/azsdk-python-textanalytics-ref-docs) | [Quellcode der v3-Bibliothek](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/textanalytics) | [v3-Paket (PiPy)](https://pypi.org/project/azure-ai-textanalytics/) | [v3-Beispiele](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/textanalytics/azure-ai-textanalytics/samples)

#### <a name="version-21"></a>[Version 2.1](#tab/version-2)

[v2-Referenzdokumentation](https://docs.microsoft.com/python/api/overview/azure/cognitiveservices/textanalytics?view=azure-python) | [Quellcode der v2-Bibliothek](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-language-textanalytics) | [v2-Paket (PiPy)](https://pypi.org/project/azure-cognitiveservices-language-textanalytics/) | [v2-Beispiele](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples)

---

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/)
* [Python 3.x](https://www.python.org/)
* Sobald Sie über Ihr Azure-Abonnement verfügen, <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics"  title="Erstellen einer Textanalyseressource"  target="_blank"> erstellen Sie eine Textanalyseressource <span class="docon docon-navigate-external x-hidden-focus"></span></a> im Azure-Portal, um Ihren Schlüssel und Endpunkt zu erhalten. Klicken Sie nach Abschluss der Bereitstellung auf **Zu Ressource wechseln**.
    * Sie benötigen den Schlüssel und den Endpunkt der von Ihnen erstellten Ressource, um Ihre Anwendung mit der Textanalyse-API zu verbinden. Der Schlüssel und der Endpunkt werden weiter unten in der Schnellstartanleitung in den Code eingefügt.
    * Sie können den kostenlosen Tarif (`F0`) verwenden, um den Dienst zu testen, und später für die Produktion auf einen kostenpflichtigen Tarif upgraden.

## <a name="setting-up"></a>Einrichten

### <a name="install-the-client-library"></a>Installieren der Clientbibliothek

Nach der Installation von Python, können Sie die Clientbibliothek mit Folgendem installieren:

#### <a name="version-30-preview"></a>[Version 3.0-preview](#tab/version-3)

```console
pip install azure-ai-textanalytics
```

> [!TIP]
> Möchten Sie sich sofort die gesamte Codedatei für die Schnellstartanleitung ansehen? Die Datei steht [auf GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/TextAnalytics/python-v3-client-library.py) zur Verfügung. Dort finden Sie die Codebeispiele aus dieser Schnellstartanleitung. 

#### <a name="version-21"></a>[Version 2.1](#tab/version-2)

```console
pip install --upgrade azure-cognitiveservices-language-textanalytics
```

> [!TIP]
> Möchten Sie sich sofort die gesamte Codedatei für die Schnellstartanleitung ansehen? Die Datei steht [auf GitHub](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/language/text_analytics_samples.py) zur Verfügung. Dort finden Sie die Codebeispiele aus dieser Schnellstartanleitung. 

---

### <a name="create-a-new-python-application"></a>Erstellen einer neuen Python-Anwendung

Erstellen Sie eine neue Python-Datei, und erstellen Sie Variablen für den Azure-Endpunkt und den Abonnementschlüssel Ihrer Ressource.

[!INCLUDE [text-analytics-find-resource-information](../find-azure-resource-info.md)]

```python
key = "<paste-your-text-analytics-key-here>"
endpoint = "<paste-your-text-analytics-endpoint-here>"
```


## <a name="object-model"></a>Objektmodell

#### <a name="version-30-preview"></a>[Version 3.0-preview](#tab/version-3)

Der Textanalyseclient ist ein Objekt vom Typ `TextAnalyticsClient`, das sich bei Azure mit Ihrem Schlüssel authentifiziert. Der Client bietet verschiedene Methoden, um Text als Batch zu analysieren. 

Bei der Batchverarbeitung wird Text als Liste vom Typ `documents` an die API gesendet. Hierbei handelt es sich um Objekte vom Typ `dictionary`, die je nach verwendeter Methode eine Kombination aus den Attributen `id`, `text` und `language` enthalten. Das `text`-Attribut speichert den zu analysierenden Text in der Ausgangssprache (`language`), und `id` kann ein beliebiger Wert sein. 

Das Antwortobjekt ist eine Liste mit den Analyseinformationen für jedes Dokument. 

#### <a name="version-21"></a>[Version 2.1](#tab/version-2)

Der Textanalyse-Client ist ein [TextAnalyticsClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-textanalytics/azure.cognitiveservices.language.textanalytics.textanalyticsclient?view=azure-python)-Objekt, das sich mit Ihrem Schlüssel bei Azure authentifiziert. Der Client verfügt über verschiedene Methoden zum Analysieren von Text als einzelne Zeichenfolge oder als Batch. 

Text wird als `documents`-Liste an die API gesendet. Hierbei handelt es sich um `dictionary`-Objekte, die je nach verwendeter Methode eine Kombination aus den Attributen `id`, `text` und `language` enthalten. Das `text`-Attribut speichert den zu analysierenden Text in der Ausgangssprache (`language`), und `id` kann ein beliebiger Wert sein. 

---

## <a name="code-examples"></a>Codebeispiele

Diese Codeausschnitte veranschaulichen, wie die folgenden Aufgaben mit der Textanalyse-Clientbibliothek für Python durchgeführt werden:

* [Authentifizieren des Clients](#authenticate-the-client)
* [Standpunktanalyse](#sentiment-analysis)
* [Sprachenerkennung](#language-detection)
* [Erkennung benannter Entitäten](#named-entity-recognition-ner) 
* [Entitätsverknüpfung](#entity-linking)
* [Schlüsselbegriffserkennung](#key-phrase-extraction)

## <a name="authenticate-the-client"></a>Authentifizieren des Clients

#### <a name="version-30-preview"></a>[Version 3.0-preview](#tab/version-3)

Erstellen Sie eine Funktion, um das Objekt `TextAnalyticsClient` mit dem oben erstellten Schlüssel (`key`) und Endpunkt (`endpoint`) zu instanziieren. Erstellen Sie anschließend einen neuen Client. 

```python
from azure.ai.textanalytics import TextAnalyticsClient
from azure.core.credentials import AzureKeyCredential

def authenticate_client():
    ta_credential = AzureKeyCredential(key)
    text_analytics_client = TextAnalyticsClient(
            endpoint=endpoint, credential=ta_credential)
    return text_analytics_client

client = authenticate_client()
```

#### <a name="version-21"></a>[Version 2.1](#tab/version-2)

[!code-python[imports statements](~/samples-cognitive-services-python-sdk/samples/language/text_analytics_samples.py?name=imports)]

Erstellen Sie eine Funktion, um das Objekt `TextAnalyticsClient` mit dem oben erstellten Schlüssel (`key`) und Endpunkt (`endpoint`) zu instanziieren. Erstellen Sie anschließend einen neuen Client. 

[!code-python[version 2 authentication](~/samples-cognitive-services-python-sdk/samples/language/text_analytics_samples.py?name=authentication)]

--- 

## <a name="sentiment-analysis"></a>Stimmungsanalyse

#### <a name="version-30-preview"></a>[Version 3.0-preview](#tab/version-3)

Erstellen Sie eine neue Funktion namens `sentiment_analysis_example()`, die den Client als Argument akzeptiert und anschließend die Funktion `analyze_sentiment()` aufruft. Das zurückgegebene Antwortobjekt enthält die Stimmungsbezeichnung und den Stimmungswert des gesamten Eingabedokuments sowie eine Standpunktanalyse für jeden Satz.


```python
def sentiment_analysis_example(client):

    documents = ["I had the best day of my life. I wish you were there with me."]
    response = client.analyze_sentiment(documents = documents)[0]
    print("Document Sentiment: {}".format(response.sentiment))
    print("Overall scores: positive={0:.2f}; neutral={1:.2f}; negative={2:.2f} \n".format(
        response.confidence_scores.positive,
        response.confidence_scores.neutral,
        response.confidence_scores.negative,
    ))
    for idx, sentence in enumerate(response.sentences):
        print("[Length: {}]".format(sentence.grapheme_length))
        print("Sentence {} sentiment: {}".format(idx+1, sentence.sentiment))
        print("Sentence score:\nPositive={0:.2f}\nNeutral={1:.2f}\nNegative={2:.2f}\n".format(
            sentence.confidence_scores.positive,
            sentence.confidence_scores.neutral,
            sentence.confidence_scores.negative,
        ))
          
sentiment_analysis_example(client)
```

### <a name="output"></a>Output

```console
Document Sentiment: positive
Overall scores: positive=1.00; neutral=0.00; negative=0.00 

[Length: 30]
Sentence 1 sentiment: positive
Sentence score:
Positive=1.00
Neutral=0.00
Negative=0.00

[Length: 30]
Sentence 2 sentiment: neutral
Sentence score:
Positive=0.21
Neutral=0.77
Negative=0.02
```

#### <a name="version-21"></a>[Version 2.1](#tab/version-2)

Authentifizieren Sie ein Clientobjekt, und rufen Sie die Funktion [sentiment()](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-textanalytics/azure.cognitiveservices.language.textanalytics.textanalyticsclient?view=azure-python#sentiment-show-stats-none--documents-none--custom-headers-none--raw-false----operation-config-) auf. Durchlaufen Sie die Ergebnisse, und drucken Sie die ID und den Stimmungswert jedes Dokuments. Eine gegen 0 tendierende Punktzahl deutet auf eine negative Stimmung hin, eine gegen 1 tendierende Punktzahl auf eine positive.

[!code-python[sentiment analysis](~/samples-cognitive-services-python-sdk/samples/language/text_analytics_samples.py?name=sentimentAnalysis)]

### <a name="output"></a>Output

```console
Document ID: 1 , Sentiment Score: 0.87
Document ID: 2 , Sentiment Score: 0.11
Document ID: 3 , Sentiment Score: 0.44
Document ID: 4 , Sentiment Score: 1.00
```

---

## <a name="language-detection"></a>Spracherkennung

#### <a name="version-30-preview"></a>[Version 3.0-preview](#tab/version-3)

Erstellen Sie eine neue Funktion namens `language_detection_example()`, die den Client als Argument akzeptiert und anschließend die Funktion `detect_language()` aufruft. Das zurückgegebene Antwortobjekt enthält die erkannte Sprache in `primary_language` (sofern erfolgreich). Andernfalls enthält es eine Fehlermeldung (`error`).

> [!Tip]
> In einigen Fällen ist es unter Umständen schwierig, die Sprachen basierend auf den Eingaben eindeutig zu bestimmen. Mithilfe des Parameters `country_hint` können Sie einen zweistelligen Ländercode angeben. Die API verwendet standardmäßig „US“ als Wert für „countryHint“. Wenn Sie dieses Verhalten entfernen möchten, können Sie diesen Parameter zurücksetzen, indem Sie seinen Wert auf eine leere Zeichenfolge (`country_hint : ""`) festlegen. 

```python
def language_detection_example(client):
    try:
        documents = ["Ce document est rédigé en Français."]
        response = client.detect_language(documents = documents, country_hint = 'us')[0]
        print("Language: ", response.primary_language.name)

    except Exception as err:
        print("Encountered exception. {}".format(err))
language_detection_example(client)
```


### <a name="output"></a>Output

```console
Language:  French
```

#### <a name="version-21"></a>[Version 2.1](#tab/version-2)

Rufen Sie mit dem zuvor erstellten Client die Funktion [detect_language()](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-textanalytics/azure.cognitiveservices.language.textanalytics.textanalyticsclient?view=azure-python#detect-language-show-stats-none--documents-none--custom-headers-none--raw-false----operation-config-) auf, um das Ergebnis abzurufen. Durchlaufen Sie dann die Ergebnisse, und drucken Sie die ID und die erste zurückgegebene Sprache.

[!code-python[language detection](~/samples-cognitive-services-python-sdk/samples/language/text_analytics_samples.py?name=languageDetection)]


### <a name="output"></a>Output

```console
Document ID: 1 , Language: English
Document ID: 2 , Language: Spanish
Document ID: 3 , Language: Chinese_Simplified
```

---

## <a name="named-entity-recognition-ner"></a>Erkennung benannter Entitäten (NER)

#### <a name="version-30-preview"></a>[Version 3.0-preview](#tab/version-3)

> [!NOTE]
> In Version `3.0-preview`: 
> * Die Entitätsverknüpfung ist eine separate Anforderung.

Erstellen Sie eine neue Funktion namens `entity_recognition_example`, die den Client als Argument akzeptiert und anschließend die Funktion `recognize_entities()` aufruft und die Ergebnisse durchläuft. Das zurückgegebene Antwortobjekt enthält die Liste erkannter Entitäten in `entity` (sofern erfolgreich). Andernfalls enthält es eine Fehlermeldung (`error`). Geben Sie für die erkannten Entitäten jeweils die Kategorie und Unterkategorie aus (sofern vorhanden).

```python
def entity_recognition_example(client):

    try:
        documents = ["I had a wonderful trip to Seattle last week."]
        result = client.recognize_entities(documents = documents)[0]

        print("Named Entities:\n")
        for entity in result.entities:
            print("\tText: \t", entity.text, "\tCategory: \t", entity.category, "\tSubCategory: \t", entity.subcategory,
                    "\n\tLength: \t", entity.grapheme_length, "\tConfidence Score: \t", round(entity.confidence_score, 2), "\n")

    except Exception as err:
        print("Encountered exception. {}".format(err))
entity_recognition_example(client)
```

### <a name="output"></a>Output

```console
Named Entities:

    Text:    Seattle        Category:        Location       SubCategory:     GPE
    Length:          7      Confidence Score:        0.92

    Text:    last week      Category:        DateTime       SubCategory:     DateRange
    Length:          9      Confidence Score:        0.8
```

## <a name="entity-linking"></a>Entitätsverknüpfung

Erstellen Sie eine neue Funktion namens `entity_linking_example()`, die den Client als Argument akzeptiert und anschließend die Funktion `recognize_linked_entities()` aufruft und die Ergebnisse durchläuft. Das zurückgegebene Antwortobjekt enthält die Liste erkannter Entitäten in `entities` (sofern erfolgreich). Andernfalls enthält es eine Fehlermeldung (`error`). Da verknüpfte Entitäten eindeutig identifiziert werden, werden Vorkommen der gleichen Entität unter einem Objekt vom Typ `entity` als Liste von Objekten des Typs `match` gruppiert.

```python
def entity_linking_example(client):

    try:
        documents = ["""Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, 
        to develop and sell BASIC interpreters for the Altair 8800. 
        During his career at Microsoft, Gates held the positions of chairman,
        chief executive officer, president and chief software architect, 
        while also being the largest individual shareholder until May 2014."""]
        result = client.recognize_linked_entities(documents = documents)[0]

        print("Linked Entities:\n")
        for entity in result.entities:
            print("\tName: ", entity.name, "\tId: ", entity.data_source_entity_id, "\tUrl: ", entity.url,
            "\n\tData Source: ", entity.data_source)
            print("\tMatches:")
            for match in entity.matches:
                print("\t\tText:", match.text)
                print("\t\tConfidence Score: {0:.2f}".format(match.confidence_score), "\tLength: {}\n".format(match.grapheme_length))
            
    except Exception as err:
        print("Encountered exception. {}".format(err))
entity_linking_example(client)
```

### <a name="output"></a>Output

```console
Linked Entities:

    Name:  Altair 8800     Id:  Altair 8800     Url:  https://en.wikipedia.org/wiki/Altair_8800 
    Data Source:  Wikipedia
    Matches:
        Text: Altair 8800
        Confidence Score: 0.00     Length: 11

    Name:  Bill Gates     Id:  Bill Gates     Url:  https://en.wikipedia.org/wiki/Bill_Gates 
    Data Source:  Wikipedia
    Matches:
        Text: Bill Gates
        Confidence Score: 0.00     Length: 10

        Text: Gates
        Confidence Score: 0.00     Length: 5

    Name:  Paul Allen     Id:  Paul Allen     Url:  https://en.wikipedia.org/wiki/Paul_Allen 
    Data Source:  Wikipedia
    Matches:
        Text: Paul Allen
        Confidence Score: 0.00     Length: 10

    Name:  Microsoft     Id:  Microsoft     Url:  https://en.wikipedia.org/wiki/Microsoft 
    Data Source:  Wikipedia
    Matches:
        Text: Microsoft
        Confidence Score: 0.00     Length: 9

        Text: Microsoft
        Confidence Score: 0.00     Length: 9

    Name:  April 4     Id:  April 4     Url:  https://en.wikipedia.org/wiki/April_4 
    Data Source:  Wikipedia
    Matches:
        Text: April 4
        Confidence Score: 0.00     Length: 7

    Name:  BASIC     Id:  BASIC     Url:  https://en.wikipedia.org/wiki/BASIC 
    Data Source:  Wikipedia
    Matches:
        Text: BASIC
        Confidence Score: 0.00     Length: 5
```

#### <a name="version-21"></a>[Version 2.1](#tab/version-2)

> [!NOTE]
> In Version 2.1 ist die Entitätsverknüpfung in der NER-Antwort enthalten.

Rufen Sie mit dem zuvor erstellten Client die Funktion [entities()](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-textanalytics/azure.cognitiveservices.language.textanalytics.textanalyticsclient?view=azure-python#entities-show-stats-none--documents-none--custom-headers-none--raw-false----operation-config-) auf, um das Ergebnis abzurufen. Durchlaufen Sie dann die Ergebnisse, und geben Sie für jedes Dokument die ID und die enthaltenen Entitäten aus.

[!code-python[Entity recognition](~/samples-cognitive-services-python-sdk/samples/language/text_analytics_samples.py?name=entityRecognition)]

### <a name="output"></a>Output

```console
Document ID: 1
        Name: Microsoft,        Type: Organization,     Sub-Type: N/A
        Offset: 0, Length: 9,   Score: 1.0

        Name: Bill Gates,       Type: Person,   Sub-Type: N/A
        Offset: 25, Length: 10, Score: 0.999847412109375

        Name: Paul Allen,       Type: Person,   Sub-Type: N/A
        Offset: 40, Length: 10, Score: 0.9988409876823425

        Name: April 4,  Type: Other,    Sub-Type: N/A
        Offset: 54, Length: 7,  Score: 0.8

        Name: April 4, 1975,    Type: DateTime, Sub-Type: Date
        Offset: 54, Length: 13, Score: 0.8

        Name: BASIC,    Type: Other,    Sub-Type: N/A
        Offset: 89, Length: 5,  Score: 0.8

        Name: Altair 8800,      Type: Other,    Sub-Type: N/A
        Offset: 116, Length: 11,        Score: 0.8

Document ID: 2
        Name: Microsoft,        Type: Organization,     Sub-Type: N/A
        Offset: 21, Length: 9,  Score: 0.999755859375

        Name: Redmond (Washington),     Type: Location, Sub-Type: N/A
        Offset: 60, Length: 7,  Score: 0.9911284446716309

        Name: 21 kilómetros,    Type: Quantity, Sub-Type: Dimension
        Offset: 71, Length: 13, Score: 0.8

        Name: Seattle,  Type: Location, Sub-Type: N/A
        Offset: 88, Length: 7,  Score: 0.9998779296875
```

---

## <a name="key-phrase-extraction"></a>Schlüsselwortextraktion


#### <a name="version-30-preview"></a>[Version 3.0-preview](#tab/version-3)

Erstellen Sie eine neue Funktion namens `key_phrase_extraction_example()`, die den Client als Argument akzeptiert und anschließend die Funktion `extract_key_phrases()` aufruft. Das Ergebnis enthält bei erfolgreicher Ausführung die Liste der erkannten Schlüsselbegriffe in `key_phrases` und andernfalls `error`. Geben Sie alle erkannten Schlüsselbegriffe aus.

```python
def key_phrase_extraction_example(client):

    try:
        documents = ["My cat might need to see a veterinarian."]

        response = client.extract_key_phrases(documents = documents)[0]

        if not response.is_error:
            print("\tKey Phrases:")
            for phrase in response.key_phrases:
                print("\t\t", phrase)
        else:
            print(response.id, response.error)

    except Exception as err:
        print("Encountered exception. {}".format(err))
        
key_phrase_extraction_example(client)
```


### <a name="output"></a>Output

```console
    Key Phrases:
         cat
         veterinarian
```

#### <a name="version-21"></a>[Version 2.1](#tab/version-2)

Rufen Sie mit dem zuvor erstellten Client die Funktion [key_phrases()](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-textanalytics/azure.cognitiveservices.language.textanalytics.textanalyticsclient?view=azure-python#key-phrases-show-stats-none--documents-none--custom-headers-none--raw-false----operation-config-) auf, um das Ergebnis abzurufen. Durchlaufen Sie dann die Ergebnisse, und geben Sie für jedes Dokument die ID und die enthaltenen Schlüsselausdrücke aus.

[!code-python[key phrase extraction](~/samples-cognitive-services-python-sdk/samples/language/text_analytics_samples.py?name=keyPhrases)]


### <a name="output"></a>Output

```console
Document ID: 1
         Key phrases:
                幸せ
Document ID: 2
         Key phrases:
                Stuttgart
                Hotel
                Fahrt
                Fu
Document ID: 3
         Key phrases:
                cat
                veterinarian
Document ID: 4
         Key phrases:
                fútbol
```

---
