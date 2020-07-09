---
author: IEvangelist
ms.service: cognitive-services
ms.topic: include
ms.date: 04/03/2020
ms.author: trbye
ms.openlocfilehash: 66422657aefa5bca2e7f20852a19faca63db5a15
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/01/2020
ms.locfileid: "85839090"
---
## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie beginnen:

> [!div class="checklist"]
> * <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesSpeechServices" target="_blank">Erstellen einer Azure Speech-Ressource<span class="docon docon-navigate-external x-hidden-focus"></span></a>
> * [Einrichten Ihrer Entwicklungsumgebung und Erstellen eines leeren Projekts](../../../../quickstarts/setup-platform.md)

## <a name="start-with-some-boilerplate-code"></a>Beginnen mit Codebausteinen

Fügen Sie Code hinzu, der als Gerüst für das Projekt fungiert. Erstellen Sie eine index.js-Datei, und fügen sie diesen Code hinzu.

Achten Sie darauf, für `subscriptionKey`, `servcieRegion` und `filename` Ihre Werte einzusetzen.

```JavaScript
(function() {
  // <code>
  "use strict";
  
  // pull in the required packages.
  var sdk = require("microsoft-cognitiveservices-speech-sdk");
  var fs = require("fs");
  
  // replace with your own subscription key,
  // service region (e.g., "westus"), and
  // the name of the file you want to run
  // through the speech recognizer.
  var subscriptionKey = "YourSubscriptionKey";
  var serviceRegion = "YourServiceRegion"; // e.g., "westus"
  var filename = "YourAudioFile.wav"; // 16000 Hz, Mono
 
}());
  
```
## <a name="load-the-file-into-an-pushaudioinputstream"></a>Laden der Datei in einen PushAudioInputStream

Für NodeJS unterstützt das Speech SDK den direkten Dateizugriff nicht nativ, daher laden wir die Datei und setzen sie in einen `PushAudioInputStream` ein. Das funktioniert bei kleineren Dateien gut, bei größeren wäre aber ein `PullAudioInputStream` effizienter.

```JavaScript
// create the push stream we need for the speech sdk.
  var pushStream = sdk.AudioInputStream.createPushStream();
  
  // open the file and push it to the push stream.
  fs.createReadStream(filename).on('data', function(arrayBuffer) {
    pushStream.write(arrayBuffer.slice());
  }).on('end', function() {
    pushStream.close();
  });
```

## <a name="create-a-speech-configuration"></a>Erstellen einer Speech-Konfiguration

Bevor Sie ein `SpeechRecognizer`-Objekt initialisieren können, müssen Sie eine Konfiguration erstellen, die den Abonnementschlüssel und die Abonnementregion verwendet. Fügen Sie diesen Code als Nächstes ein.

> [!NOTE]
> Das Speech SDK verwendet für die Erkennung standardmäßig amerikanisches Englisch (en-us). Informationen zum Auswählen der Ausgangssprache finden Sie unter [Angeben der Ausgangssprache für die Spracherkennung](../../../../how-to-specify-source-language.md).

 ```JavaScript
   // now create the audio-config pointing to our stream and
  // the speech config specifying the language.
  var speechConfig = sdk.SpeechConfig.fromSubscription(subscriptionKey, serviceRegion);
  
  // setting the recognition language to English.
  speechConfig.speechRecognitionLanguage = "en-US";
  
```
## <a name="create-an-audio-configuration"></a>Erstellen einer Audiokonfiguration

Jetzt müssen Sie ein `AudioConfig`-Objekt erstellen, das auf Ihren `PushAudioInputStream` verweist. Fügen Sie den folgenden Code direkt unterhalb Ihrer Speech-Konfiguration ein:

```JavaScript
    var audioConfig = sdk.AudioConfig.fromStreamInput(pushStream);
```

## <a name="initialize-a-speechrecognizer"></a>Initialisieren eines SpeechRecognizer-Elements

Erstellen Sie nun das Objekt `SpeechRecognizer` mithilfe der zuvor erstellten Objekte `SpeechConfig` und `AudioConfig`.

```JavaScript
  // create the speech recognizer.
  var recognizer = new sdk.SpeechRecognizer(speechConfig, audioConfig);
  
```
## <a name="recognize-a-phrase-and-display-results"></a>Erkennen eines Ausdrucks und Anzeigen der Ergebnisse

Rufen Sie die Methode `recognizeOnceAsync()` über das Objekt `SpeechRecognizer` auf. Diese Methode teilt dem Spracherkennungsdienst mit, dass Sie einen einzelnen Ausdruck zur Erkennung senden, und dass die Spracherkennung beendet werden soll, sobald der Ausdruck ermittelt wurde.

Wir schreiben den zurückgegebenen Text oder ggf. aufgetretene Fehler ebenfalls in die Konsole und schließen abschließend die Erkennung.
```JavaScript
 // we are done with the setup
  console.log("Now recognizing from: " + filename);
  
  // start the recognizer and wait for a result.
  recognizer.recognizeOnceAsync(
    function (result) {
      console.log(result);
  
      recognizer.close();
      recognizer = undefined;
    },
    function (err) {
      console.trace("err - " + err);
  
      recognizer.close();
      recognizer = undefined;
    });
```
## <a name="check-your-code"></a>Überprüfen des Codes
```JavaScript
(function() {
  "use strict";
  
  // pull in the required packages.
  var sdk = require("microsoft-cognitiveservices-speech-sdk");
  var fs = require("fs");
  
  // replace with your own subscription key,
  // service region (e.g., "westus"), and
  // the name of the file you want to run
  // through the speech recognizer.
  var subscriptionKey = "YourSubscriptionKey";
  var serviceRegion = "YourServiceRegion"; // e.g., "westus"
  var filename = "YourAudioFile.wav"; // 16000 Hz, Mono
  
  // create the push stream we need for the speech sdk.
  var pushStream = sdk.AudioInputStream.createPushStream();
  
  // open the file and push it to the push stream.
  fs.createReadStream(filename).on('data', function(arrayBuffer) {
    pushStream.write(arrayBuffer.slice());
  }).on('end', function() {
    pushStream.close();
  });
  
  // now create the audio-config pointing to our stream and
  // the speech config specifying the language.
  var speechConfig = sdk.SpeechConfig.fromSubscription(subscriptionKey, serviceRegion);
  
  // setting the recognition language to English.
  speechConfig.speechRecognitionLanguage = "en-US";
  
  var audioConfig = sdk.AudioConfig.fromStreamInput(pushStream);
  
  // create the speech recognizer.
  var recognizer = new sdk.SpeechRecognizer(speechConfig, audioConfig);
  
  // we are done with the setup
  console.log("Now recognizing from: " + filename);
  
  // start the recognizer and wait for a result.
  recognizer.recognizeOnceAsync(
    function (result) {
      console.log(result);
  
      recognizer.close();
      recognizer = undefined;
    },
    function (err) {
      console.trace("err - " + err);
  
      recognizer.close();
      recognizer = undefined;
    });

}());
```
## <a name="run-the-sample-locally"></a>Lokales Ausführen des Beispiels

Ausführen des Codes mithilfe von NodeJs
```bash
node index.js
```

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [Speech recognition basics](../../speech-to-text-next-steps.md)]
