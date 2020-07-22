---
title: 'Schnellstart: Textanalyse-Clientbibliothek für Go | Microsoft-Dokumentation'
titleSuffix: Azure Cognitive Services
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie die Textanalyse-Clientbibliothek für Go von Azure Cognitive Services für die Spracherkennung verwenden.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: quickstart
ms.date: 02/26/2020
ms.author: aahi
ms.openlocfilehash: ff1b35ec036996dfa452d92632c8fc6ede22d82f
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2020
ms.locfileid: "86203242"
---
# <a name="quickstart-use-the-text-analytics-client-library-for-go"></a>Schnellstart: Verwenden der Textanalyse-Clientbibliothek für Go

- [Referenzdokumentation](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics)
- [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-go/blob/090dc0ee4d8d2d60e2a9525774d967a4111a2b0c/services/cognitiveservices/v2.1/textanalytics/client.go)
- [Paket (GitHub)](https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices/v2.1/textanalytics)
- [Beispiele](https://github.com/Azure-Samples/cognitive-services-quickstart-code)

> [!NOTE]
> Diese Schnellstartanleitung gilt nur für die Version 2.1 der Textanalyse. Für Go steht derzeit keine V3-Clientbibliothek zur Verfügung.

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement: [Kostenloses Azure-Konto](https://azure.microsoft.com/free/)
* Aktuelle Version von [Go](https://golang.org/dl/)
* Sobald Sie über Ihr Azure-Abonnement verfügen, <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics"  title="Erstellen einer Textanalyseressource"  target="_blank"> erstellen Sie eine Textanalyseressource <span class="docon docon-navigate-external x-hidden-focus"></span></a> im Azure-Portal, um Ihren Schlüssel und Endpunkt zu erhalten. 
    * Sie benötigen den Schlüssel und den Endpunkt der von Ihnen erstellten Ressource, um Ihre Anwendung mit der Textanalyse-API zu verbinden. Dies geschieht später in der Schnellstartanleitung.
    * Sie können den kostenlosen Tarif verwenden, um den Dienst zu testen, und später für die Produktion ein Upgrade auf einen kostenpflichtigen Tarif durchführen.

## <a name="setting-up"></a>Einrichten

### <a name="create-a-new-go-project"></a>Erstellen eines neuen Go-Projekts

Erstellen Sie in einem Konsolenfenster (cmd, PowerShell, Terminal, Bash) einen neuen Arbeitsbereich für Ihr Go-Projekt, und navigieren Sie zu diesem Projekt. Ihr Arbeitsbereich enthält drei Ordner: 

* **src**: Dieses Verzeichnis enthält Quellcode und Pakete. Alle Pakete, die mit dem Befehl `go get` installiert werden, befinden sich in diesem Verzeichnis.
* **pkg**: Dieses Verzeichnis enthält die kompilierten Go-Paketobjekte. Diese Dateien haben alle die Erweiterung `.a`.
* **bin**: Dieses Verzeichnis enthält die ausführbaren Binärdateien, die beim Ausführen von `go install` erstellt werden.

> [!TIP]
> Informieren Sie sich über die Struktur eines [Go-Arbeitsbereichs](https://golang.org/doc/code.html#Workspaces). Dieses Handbuch enthält Informationen zum Festlegen von `$GOPATH` und `$GOROOT`.

Erstellen Sie einen Arbeitsbereich mit dem Namen `my-app` und den erforderlichen Unterverzeichnissen für `src`, `pkg` und `bin`:

```console
$ mkdir -p my-app/{src, bin, pkg}  
$ cd my-app
```

### <a name="install-the-text-analytics-client-library-for-go"></a>Installieren der Textanalyse-Clientbibliothek für Go

Installieren Sie die Clientbibliothek für Go: 

```console
$ go get -u <https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices/v2.1/textanalytics>
```

Bei Verwendung von DEP können Sie alternativ Folgendes in Ihrem Repository ausführen:

```console
$ dep ensure -add <https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices/v2.1/textanalytics>
```

### <a name="create-your-go-application"></a>Erstellen Ihrer Go-Anwendung

Erstellen Sie als Nächstes eine Datei mit dem Namen `src/quickstart.go`:

```bash
$ cd src
$ touch quickstart.go
```

Öffnen Sie `quickstart.go` in einer IDE oder einem Text-Editor Ihrer Wahl. Fügen Sie dann den Paketnamen hinzu, und importieren Sie die folgenden Bibliotheken:

[!code-go[Import statements](~/azure-sdk-for-go-samples/cognitiveservices/textanalytics.go?name=imports)]

## <a name="object-model"></a>Objektmodell 

Der Textanalyse-Client ist ein [BaseClient](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics#New)-Objekt, für das die Authentifizierung für Azure mit Ihrem Schlüssel durchgeführt wird. Der Client verfügt über verschiedene Methoden zum Analysieren von Text als einzelne Zeichenfolge oder als Batch. 

Text wird als `documents`-Liste an die API gesendet. Hierbei handelt es sich um `dictionary`-Objekte, die je nach verwendeter Methode eine Kombination aus den Attributen `id`, `text` und `language` enthalten. Das `text`-Attribut speichert den zu analysierenden Text in der Ausgangssprache (`language`), und `id` kann ein beliebiger Wert sein. 

Das Antwortobjekt ist eine Liste mit den Analyseinformationen für jedes Dokument. 

## <a name="code-examples"></a>Codebeispiele

Mit den Codeausschnitten wird veranschaulicht, wie die folgenden Vorgänge mit der Textanalyse-Clientbibliothek für Go durchgeführt werden:

* [Authentifizieren des Clients](#authenticate-the-client)
* [Standpunktanalyse](#sentiment-analysis)
* [Sprachenerkennung](#language-detection)
* [Entitätserkennung](#entity-recognition)
* [Schlüsselbegriffserkennung](#key-phrase-extraction)

## <a name="authenticate-the-client"></a>Authentifizieren des Clients


Erstellen Sie in einer neuen Funktion Variablen für den Azure-Endpunkt und den Abonnementschlüssel Ihrer Ressource.

[!INCLUDE [text-analytics-find-resource-information](../includes/find-azure-resource-info.md)]

Erstellen Sie ein neues [BaseClient](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics#New)-Objekt. Übergeben Sie Ihren Schlüssel an die Funktion [autorest.NewCognitiveServicesAuthorizer()](https://godoc.org/github.com/Azure/go-autorest/autorest#NewCognitiveServicesAuthorizer), die dann an die `authorizer`-Eigenschaft des Clients übergeben wird.

```go
func GetTextAnalyticsClient() textanalytics.BaseClient {
    var key string = "<paste-your-text-analytics-key-here>"
    var endpoint string = "<paste-your-text-analytics-endpoint-here>"

    textAnalyticsClient := textanalytics.New(endpoint)
    textAnalyticsClient.Authorizer = autorest.NewCognitiveServicesAuthorizer(key)

    return textAnalyticsClient
}
```

## <a name="sentiment-analysis"></a>Stimmungsanalyse

Erstellen Sie eine neue Funktion mit dem Namen `SentimentAnalysis()`, und erstellen Sie mithilfe der zuvor erstellten `GetTextAnalyticsClient()`-Methode einen Client. Erstellen Sie eine Liste mit [MultiLanguageInput](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics#MultiLanguageBatchInput)-Objekten, die die zu analysierenden Dokumente enthalten. Jedes Objekt enthält die Attribute `id`, `Language` und `text`. Das `text`-Attribut speichert den Text, der analysiert werden soll, `language` ist die Sprache des Dokuments, und `id` kann ein beliebiger Wert sein. 

Rufen Sie die Funktion [sentiment()](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics#BaseClient.Sentiment) des Clients auf, um das Ergebnis abzurufen. Durchlaufen Sie dann die Ergebnisse, und drucken Sie die ID und den Stimmungswert jedes Dokuments. Eine gegen 0 tendierende Punktzahl deutet auf eine negative Stimmung hin, eine gegen 1 tendierende Punktzahl auf eine positive.

[!code-go[Sentiment analysis sample](~/azure-sdk-for-go-samples/cognitiveservices/textanalytics.go?name=sentimentAnalysis)]

Rufen Sie `SentimentAnalysis()` in Ihrem Projekt auf.

### <a name="output"></a>Output

```console
Document ID: 1 , Sentiment Score: 0.87
Document ID: 2 , Sentiment Score: 0.11
Document ID: 3 , Sentiment Score: 0.44
Document ID: 4 , Sentiment Score: 1.00
```

## <a name="language-detection"></a>Spracherkennung

Erstellen Sie eine neue Funktion mit dem Namen `LanguageDetection()`, und erstellen Sie mithilfe der zuvor erstellten `GetTextAnalyticsClient()`-Methode einen Client. Erstellen Sie eine Liste mit [LanguageInput](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics#LanguageInput)-Objekten, die die zu analysierenden Dokumente enthalten. Jedes Objekt enthält die Attribute `id` und `text`. Das `text`-Attribut speichert den Text, der analysiert werden soll, und die `id` kann ein beliebiger Wert sein. 

Rufen Sie die [DetectLanguage()](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics#BaseClient.DetectLanguage)-Funktion des Clients auf und das Ergebnis ab. Durchlaufen Sie dann die Ergebnisse, und geben Sie für jedes Dokument die ID und die ermittelte Sprache aus.

[!code-go[Language detection sample](~/azure-sdk-for-go-samples/cognitiveservices/textanalytics.go?name=languageDetection)]

Rufen Sie `LanguageDetection()` in Ihrem Projekt auf.

### <a name="output"></a>Output

```console
Document ID: 0 , Language: English 
Document ID: 1 , Language: Spanish
Document ID: 2 , Language: Chinese_Simplified
```

## <a name="entity-recognition"></a>Entitätserkennung

Erstellen Sie eine neue Funktion mit dem Namen `ExtractEntities()`, und erstellen Sie mithilfe der zuvor erstellten `GetTextAnalyticsClient()`-Methode einen Client. Erstellen Sie eine Liste mit [MultiLanguageInput](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics#MultiLanguageBatchInput)-Objekten, die die zu analysierenden Dokumente enthalten. Jedes Objekt enthält die Attribute `id`, `language` und `text`. Das `text`-Attribut speichert den Text, der analysiert werden soll, `language` ist die Sprache des Dokuments, und `id` kann ein beliebiger Wert sein. 

Rufen Sie die [Entities()](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics#BaseClient.Entities)-Funktion des Clients auf und das Ergebnis ab. Durchlaufen Sie dann die Ergebnisse, und geben Sie für jedes Dokument die ID und die Bewertung der extrahierten Entitäten aus.

[!code-go[entity recognition sample](~/azure-sdk-for-go-samples/cognitiveservices/textanalytics.go?name=entityRecognition)]

Rufen Sie `ExtractEntities()` in Ihrem Projekt auf.

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

## <a name="key-phrase-extraction"></a>Schlüsselwortextraktion

Erstellen Sie eine neue Funktion mit dem Namen `ExtractKeyPhrases()`, und erstellen Sie mithilfe der zuvor erstellten `GetTextAnalyticsClient()`-Methode einen Client. Erstellen Sie eine Liste mit [MultiLanguageInput](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics#MultiLanguageBatchInput)-Objekten, die die zu analysierenden Dokumente enthalten. Jedes Objekt enthält die Attribute `id`, `language` und `text`. Das `text`-Attribut speichert den Text, der analysiert werden soll, `language` ist die Sprache des Dokuments, und `id` kann ein beliebiger Wert sein.

Rufen Sie die [KeyPhrases()](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics#BaseClient.KeyPhrases)-Funktion des Clients auf und das Ergebnis ab. Durchlaufen Sie dann die Ergebnisse, und geben Sie für jedes Dokument die ID und die extrahierten Schlüsselbegriffe aus.

[!code-go[key phrase extraction sample](~/azure-sdk-for-go-samples/cognitiveservices/textanalytics.go?name=keyPhrases)]

Rufen Sie `ExtractKeyPhrases()` in Ihrem Projekt auf.

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
