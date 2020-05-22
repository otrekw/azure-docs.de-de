---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 08/06/2019
ms.author: erhopf
ms.openlocfilehash: 5116df10fa6732f1b28ff83dc8854616264222bc
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83586508"
---
[!INCLUDE [Prerequisites](prerequisites-go.md)]

[!INCLUDE [Set up and use environment variables](setup-env-variables.md)]

## <a name="create-a-project-and-import-required-modules"></a>Erstellen eines Projekts und Importieren der erforderlichen Module

Erstellen Sie in Ihrer bevorzugten IDE oder Ihrem bevorzugten Editor ein neues Go-Projekt. Kopieren Sie anschließend den folgenden Codeausschnitt in Ihr Projekt in eine Datei namens `transliterate-text.go`.

```go
package main

import (
    "bytes"
    "encoding/json"
    "fmt"
    "log"
    "net/http"
    "net/url"
    "os"
)
```

## <a name="create-the-main-function"></a>Erstellen der main-Funktion

In diesem Beispiel werden der Translator-Abonnementschlüssel und der Endpunkt aus den Umgebungsvariablen `TRANSLATOR_TEXT_SUBSCRIPTION_KEY` und `TRANSLATOR_TEXT_ENDPOINT` gelesen. Wenn Sie mit Umgebungsvariablen nicht vertraut sind, können Sie `subscriptionKey` und `endpoint` als Zeichenfolge festlegen und die Bedingungsanweisungen auskommentieren.

Kopieren Sie diesen Code in Ihr Projekt:

```go
func main() {
    /*
    * Read your subscription key from an env variable.
    * Please note: You can replace this code block with
    * var subscriptionKey = "YOUR_SUBSCRIPTION_KEY" if you don't
    * want to use env variables. If so, be sure to delete the "os" import.
    */
    if "" == os.Getenv("TRANSLATOR_TEXT_SUBSCRIPTION_KEY") {
      log.Fatal("Please set/export the environment variable TRANSLATOR_TEXT_SUBSCRIPTION_KEY.")
    }
    subscriptionKey := os.Getenv("TRANSLATOR_TEXT_SUBSCRIPTION_KEY")
    if "" == os.Getenv("TRANSLATOR_TEXT_ENDPOINT") {
      log.Fatal("Please set/export the environment variable TRANSLATOR_TEXT_ENDPOINT.")
    }
    endpoint := os.Getenv("TRANSLATOR_TEXT_ENDPOINT")
    uri := endpoint + "/transliterate?api-version=3.0"
    /*
     * This calls our breakSentence function, which we'll
     * create in the next section. It takes a single argument,
     * the subscription key.
     */
    transliterate(subscriptionKey, uri)
}
```

## <a name="create-a-function-to-transliterate-text"></a>Erstellen einer Funktion für die Transliteration von Text

Erstellen wir nun eine Funktion für die Transliteration von Text. Von dieser Funktion wird ein einzelnes Argument (Ihr Translator-Abonnementschlüssel) verwendet.

```go
func transliterate(subscriptionKey string, uri string) {
    /*  
     * In the next few sections, we'll add code to this
     * function to make a request and handle the response.
     */
}
```

Als Nächstes erstellen wir die URL. Die URL wird mit den Methoden `Parse()` und `Query()` erstellt. Mit der Methode `Add()` werden Parameter hinzugefügt. In diesem Beispiel wird Japanisch in das lateinische Alphabet transliteriert.

Kopieren Sie diesen Code in die Funktion `transliterate`.

```go
// Build the request URL. See: https://golang.org/pkg/net/url/#example_URL_Parse
u, _ := url.Parse(uri)
q := u.Query()
q.Add("language", "ja")
q.Add("fromScript", "jpan")
q.Add("toScript", "latn")
u.RawQuery = q.Encode()
```

>[!NOTE]
> Weitere Informationen zu Endpunkten, Routen und Anforderungsparametern finden Sie unter [Translator 3.0: Transliterate](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-transliterate).

## <a name="create-a-struct-for-your-request-body"></a>Erstellen einer Struktur für Ihren Anforderungstext

Erstellen Sie als Nächstes eine anonyme Struktur für den Anforderungstext, und codieren Sie sie im JSON-Format mit `json.Marshal()`. Fügen Sie diesen Code der Funktion `transliterate` hinzu.

```go
// Create an anonymous struct for your request body and encode it to JSON
body := []struct {
  Text string
}{
  {Text: "こんにちは"},
}
b, _ := json.Marshal(body)
```

## <a name="build-the-request"></a>Erstellen der Anforderung

Nachdem Sie nun den Anforderungstext im JSON-Format codiert haben, können Sie Ihre POST-Anforderung erstellen und Translator aufrufen.

```go
// Build the HTTP POST request
req, err := http.NewRequest("POST", u.String(), bytes.NewBuffer(b))
if err != nil {
    log.Fatal(err)
}
// Add required headers to the request
req.Header.Add("Ocp-Apim-Subscription-Key", subscriptionKey)
req.Header.Add("Content-Type", "application/json")

// Call the Translator
res, err := http.DefaultClient.Do(req)
if err != nil {
    log.Fatal(err)
}
```

Wenn Sie ein Cognitive Services-Abonnement mehrerer Dienste verwenden, müssen Sie auch `Ocp-Apim-Subscription-Region` in Ihre Anforderungsparameter aufnehmen. [Erfahren Sie mehr über die Authentifizierung mit dem Abonnement für mehrere Dienste](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication).

## <a name="handle-and-print-the-response"></a>Verarbeiten und Ausgeben der Antwort

Fügen Sie diesen Code der Funktion `transliterate` hinzu, um die JSON-Antwort zu decodieren, zu formatieren und das Ergebnis auszugeben.

```go
// Decode the JSON response
var result interface{}
if err := json.NewDecoder(res.Body).Decode(&result); err != nil {
    log.Fatal(err)
}
// Format and print the response to terminal
prettyJSON, _ := json.MarshalIndent(result, "", "  ")
fmt.Printf("%s\n", prettyJSON)
```

## <a name="put-it-all-together"></a>Korrektes Zusammenfügen

Das war's: Sie haben ein einfaches Programm erstellt, das Translator aufruft und eine JSON-Antwort zurückgibt. Führen Sie das Programm jetzt aus:

```console
go run transliterate-text.go
```

[Auf GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-Go) finden Sie das vollständige Beispiel, falls Sie Ihren Code mit unserem Code vergleichen möchten.

## <a name="sample-response"></a>Beispiel für eine Antwort

```json
[
  {
    "script": "latn",
    "text": "konnichiwa"
  }
]
```

## <a name="next-steps"></a>Nächste Schritte

Machen Sie sich anhand der API-Referenz mit den Möglichkeiten von Translator vertraut.

> [!div class="nextstepaction"]
> [API-Referenz](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference)
