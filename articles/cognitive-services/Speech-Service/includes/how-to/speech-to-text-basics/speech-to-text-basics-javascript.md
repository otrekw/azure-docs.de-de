---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 04/15/2020
ms.author: trbye
ms.openlocfilehash: bb7bc07bf7aa24af5ed45a5615457d71b3173868
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87102707"
---
## <a name="prerequisites"></a>Voraussetzungen

In diesem Artikel wird vorausgesetzt, dass Sie über ein Azure-Konto und über ein Abonnement für den Speech-Dienst verfügen. Falls nicht, können Sie [den Speech-Dienst kostenlos testen](../../../get-started.md).

## <a name="install-the-speech-sdk"></a>Installieren des Speech SDK

Zuallererst müssen Sie das <a href="https://www.npmjs.com/package/microsoft-cognitiveservices-speech-sdk" target="_blank">Speech SDK für JavaScript <span class="docon docon-navigate-external x-hidden-focus"></span></a> installieren. Verwenden Sie dazu die folgenden plattformspezifischen Anleitungen:

- <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk?tabs=nodejs#get-the-speech-sdk" target="_blank">Node.js <span 
class="docon docon-navigate-external x-hidden-focus"></span></a>
- <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk?tabs=browser#get-the-speech-sdk" target="_blank">Webbrowser <span class="docon docon-navigate-external x-hidden-focus"></span></a>

Verwenden Sie außerdem abhängig von der Zielumgebung eine der folgenden Funktionen:

# <a name="script"></a>[script](#tab/script)

Laden Sie die Datei *microsoft.cognitiveservices.speech.sdk.bundle.js* des <a href="https://aka.ms/csspeech/jsbrowserpackage" target="_blank">Speech SDK für JavaScript <span class="docon docon-navigate-external x-hidden-focus"></span></a> herunter, und extrahieren Sie sie. Speichern Sie sie in einem Ordner, auf den von der HTML-Datei zugegriffen werden kann.

```html
<script src="microsoft.cognitiveservices.speech.sdk.bundle.js"></script>;
```

> [!TIP]
> Verwenden Sie für einen Webbrowser das Tag `<script>`. Das Präfix `sdk` ist nicht erforderlich. Das Präfix `sdk` ist ein Alias, mit dem das Modul `require` benannt wird.

# <a name="import"></a>[import](#tab/import)

```javascript
import * from "microsoft-cognitiveservices-speech-sdk";
```

Weitere Informationen zu `import` finden Sie unter <a href="https://javascript.info/import-export" target="_blank">Exportieren und Importieren <span class="docon docon-navigate-external x-hidden-focus"></span></a>.

# <a name="require"></a>[require](#tab/require)

```javascript
const sdk = require("microsoft-cognitiveservices-speech-sdk");
```

Weitere Informationen zu `require` finden Sie unter <a href="https://nodejs.org/en/knowledge/getting-started/what-is-require/" target="_blank">Was ist „require“? <span class="docon docon-navigate-external x-hidden-focus"></span></a>.

---

## <a name="create-a-speech-configuration"></a>Erstellen einer Sprachkonfiguration

Um den Speech-Dienst über das Speech SDK aufrufen zu können, muss eine Sprachkonfiguration ([`SpeechConfig`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechconfig?view=azure-node-latest)) erstellt werden. Diese Klasse enthält Informationen zu Ihrem Abonnement. Hierzu zählen etwa Ihr Schlüssel und die zugeordnete Region, der Endpunkt, der Host oder das Autorisierungstoken.

> [!NOTE]
> Eine Konfiguration ist immer erforderlich. Dabei spielt es keine Rolle, ob Sie eine Spracherkennung, eine Sprachsynthese, eine Übersetzung oder eine Absichtserkennung durchführen möchten.

Eine Sprachkonfiguration ([`SpeechConfig`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechconfig?view=azure-node-latest)) kann auf unterschiedliche Weise initialisiert werden:

* Mit einem Abonnement: Übergeben Sie einen Schlüssel und die zugeordnete Region.
* Mit einem Endpunkt: Übergeben Sie einen Endpunkt für den Speech-Dienst. Ein Schlüssel oder Autorisierungstoken ist optional.
* Mit einem Host: Übergeben Sie eine Hostadresse. Ein Schlüssel oder Autorisierungstoken ist optional.
* Mit einem Autorisierungstoken: Übergeben Sie ein Autorisierungstoken und die zugeordnete Region.

Hier sehen Sie, wie eine Sprachkonfiguration ([`SpeechConfig`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechconfig?view=azure-node-latest)) mit einem Schlüssel und einer Region erstellt wird. Informationen zur Ermittlung Ihres Regionsbezeichners finden Sie auf der Seite zur [Unterstützung von Regionen](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#speech-sdk).

```javascript
const speechConfig = SpeechConfig.fromSubscription("YourSubscriptionKey", "YourServiceRegion");
```

## <a name="initialize-a-recognizer"></a>Initialisieren einer Erkennung

Nach der Erstellung einer Sprachkonfiguration ([`SpeechConfig`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechconfig?view=azure-node-latest)) muss eine Spracherkennung ([`SpeechRecognizer`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognizer?view=azure-node-latest)) initialisiert werden. Wenn Sie eine Spracherkennung ([`SpeechRecognizer`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognizer?view=azure-node-latest)) initialisieren, müssen Sie Ihre Sprachkonfiguration (`speechConfig`) übergeben. Dadurch werden die Anmeldeinformationen bereitgestellt, die der Speech-Dienst zur Überprüfung Ihrer Anforderung benötigt.

Wenn Sie für die Spracherkennung das Standardmikrofon Ihres Geräts verwenden, sollte [`SpeechRecognizer`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognizer?view=azure-node-latest) wie folgt aussehen:

```javascript
const recognizer = new SpeechRecognizer(speechConfig);
```

Wenn Sie das Audioeingabegerät angeben möchten, müssen Sie eine Audiokonfiguration ([`AudioConfig`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/audioconfig?view=azure-node-latest)) erstellen und beim Initialisieren Ihrer Spracherkennung ([`SpeechRecognizer`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognizer?view=azure-node-latest)) den Parameter `audioConfig` angeben.

> [!TIP]
> Informationen zum Abrufen der Geräte-ID für Ihr Audioeingabegerät finden Sie [hier](../../../how-to-select-audio-input-devices.md).

Verweisen Sie wie folgt auf das `AudioConfig`-Objekt:

```javascript
const audioConfig = AudioConfig.fromDefaultMicrophoneInput();
const recognizer = new SpeechRecognizer(speechConfig, audioConfig);
```

Eine Audiokonfiguration (`audioConfig`) ist auch erforderlich, wenn Sie anstelle eines Mikrofons eine Audiodatei verwenden möchten. Dies ist jedoch nur möglich, wenn Sie **Node.js** verwenden und beim Erstellen eines [`AudioConfig`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/audioconfig?view=azure-node-latest)-Objekts nicht `fromDefaultMicrophoneInput`, sondern `fromWavFileOutput` aufrufen und den Parameter `filename` übergeben.

```javascript
const audioConfig = AudioConfig.fromWavFileInput("YourAudioFile.wav");
const recognizer = new SpeechRecognizer(speechConfig, audioConfig);
```

## <a name="recognize-speech"></a>Erkennen von Sprache

Von der Klasse [SpeechRecognizer](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognizer?view=azure-node-latest) für das Speech SDK für JavaScript werden verschiedene Methoden für die Spracherkennung verfügbar gemacht.

* Einzelerkennung (asynchron): Führt die Erkennung in einem nicht blockierenden (asynchronen) Modus durch. Dient zum Erkennen einer einzelnen Äußerung. Zur Erkennung des Endes einer einzelnen Äußerung wird auf Stille am Ende gelauscht oder gewartet, bis maximal 15 Sekunden an Audiodaten verarbeitet wurden.
* Kontinuierliche Erkennung (asynchron): Initialisiert asynchron einen kontinuierlichen Erkennungsvorgang. Der Benutzer registriert sich für Ereignisse und behandelt verschiedene Anwendungszustände. Zum Beenden der asynchronen kontinuierlichen Erkennung muss [`stopContinuousRecognitionAsync`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognizer?view=azure-node-latest#stopcontinuousrecognitionasync) aufgerufen werden.

> [!NOTE]
> Weitere Informationen zum Auswählen eines Spracherkennungsmodus finden Sie [hier](../../../how-to-choose-recognition-mode.md).

### <a name="single-shot-recognition"></a>Einzelerkennung

Das folgende Beispiel zeigt eine asynchrone Einzelerkennung mit [`recognizeOnceAsync`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognizer?view=azure-node-latest#recognizeonceasync):

```javascript
recognizer.recognizeOnceAsync(result => {
    // Interact with result
});
```

Sie müssen etwas Code schreiben, um das Ergebnis zu behandeln. In diesem Beispiel wird [`result.reason`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognitionresult?view=azure-node-latest#reason) ausgewertet:

* Das Erkennungsergebnis wird ausgegeben: `ResultReason.RecognizedSpeech`
* Wurde kein Erkennungstreffer gefunden, wird der Benutzer davon in Kenntnis gesetzt: `ResultReason.NoMatch`
* Ist ein Fehler aufgetreten, wird die Fehlermeldung ausgegeben: `ResultReason.Canceled`

```javascript
switch (result.reason) {
    case ResultReason.RecognizedSpeech:
        console.log(`RECOGNIZED: Text=${result.text}`);
        console.log("    Intent not recognized.");
        break;
    case ResultReason.NoMatch:
        console.log("NOMATCH: Speech could not be recognized.");
        break;
    case ResultReason.Canceled:
        const cancellation = CancellationDetails.fromResult(result);
        console.log(`CANCELED: Reason=${cancellation.reason}`);

        if (cancellation.reason == CancellationReason.Error) {
            console.log(`CANCELED: ErrorCode=${cancellation.ErrorCode}`);
            console.log(`CANCELED: ErrorDetails=${cancellation.errorDetails}`);
            console.log("CANCELED: Did you update the subscription info?");
        }
        break;
    }
```

### <a name="continuous-recognition"></a>Kontinuierliche Erkennung

Die kontinuierliche Erkennung ist etwas komplexer als die Einzelerkennung. Für die kontinuierliche Erkennung müssen die Ereignisse `Recognizing`, `Recognized` und `Canceled` abonniert werden, um die Erkennungsergebnisse zu erhalten. Zum Beenden der Erkennung muss [`stopContinuousRecognitionAsync`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognizer?view=azure-node-latest#stopcontinuousrecognitionasync) aufgerufen werden. Im folgenden Beispiel wird eine kontinuierliche Erkennung für eine Audioeingabedatei durchgeführt.

Als Erstes wird die Eingabe definiert und eine Spracherkennung ([`SpeechRecognizer`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognizer?view=azure-node-latest)) initialisiert:

```javascript
const recognizer = new SpeechRecognizer(speechConfig);
```

Wir abonnieren die von [`SpeechRecognizer`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognizer?view=azure-node-latest) gesendeten Ereignisse:

* [`recognizing`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognizer?view=azure-node-latest#recognizing): Signal für Ereignisse mit Zwischenergebnissen der Erkennung.
* [`recognized`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognizer?view=azure-node-latest#recognized): Signal für Ereignisse mit Endergebnissen der Erkennung. (Der Erkennungsversuch war also erfolgreich.)
* [`sessionStopped`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognizer?view=azure-node-latest#sessionstopped): Signal für Ereignisse, die das Ende einer Erkennungssitzung (bzw. eines Erkennungsvorgangs) angeben.
* [`canceled`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognizer?view=azure-node-latest#canceled): Signal für Ereignisse mit Ergebnissen einer abgebrochenen Erkennung. (Der Erkennungsversuch wurde also aufgrund einer direkten Abbruchanforderungen oder infolge eines Transport- oder Protokollfehlers abgebrochen.)

```javascript
recognizer.recognizing = (s, e) => {
    console.log(`RECOGNIZING: Text=${e.result.text}`);
};

recognizer.recognized = (s, e) => {
    if (e.result.reason == ResultReason.RecognizedSpeech) {
        console.log(`RECOGNIZED: Text=${e.result.text}`);
    }
    else if (e.result.reason == ResultReason.NoMatch) {
        console.log("NOMATCH: Speech could not be recognized.");
    }
};

recognizer.canceled = (s, e) => {
    console.log(`CANCELED: Reason=${e.reason}`);

    if (e.reason == CancellationReason.Error) {
        console.log(`"CANCELED: ErrorCode=${e.errorCode}`);
        console.log(`"CANCELED: ErrorDetails=${e.errorDetails}`);
        console.log("CANCELED: Did you update the subscription info?");
    }

    recognizer.stopContinuousRecognitionAsync();
};

recognizer.sessionStopped = (s, e) => {
    console.log("\n    Session stopped event.");
    recognizer.stopContinuousRecognitionAsync();
};
```

Nach Abschluss der Einrichtung kann [`startContinuousRecognitionAsync`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognizer?view=azure-node-latest#startcontinuousrecognitionasync) aufgerufen werden.

```javascript
// Starts continuous recognition. Uses stopContinuousRecognitionAsync() to stop recognition.
recognizer.startContinuousRecognitionAsync();

// Something later can call, stops recognition.
// recognizer.StopContinuousRecognitionAsync();
```

### <a name="dictation-mode"></a>Diktiermodus

Bei Verwendung der kontinuierlichen Erkennung können Sie die Diktatverarbeitung mithilfe der entsprechenden Funktion „Diktat aktivieren“ aktivieren. Dieser Modus bewirkt, dass die Sprachkonfigurationsinstanz Wortbeschreibungen von Satzstrukturen wie z. B. Interpunktion interpretiert. Beispielsweise würde die Äußerung „Leben Sie in der Stadt Fragezeichen“ als Text „Leben Sie in der Stadt?“ interpretiert.

Verwenden Sie zum Aktivieren des Diktiermodus die Methode [`enableDictation`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechconfig?view=azure-node-latest#enabledictation--) in Ihrer Sprachkonfiguration ([`SpeechConfig`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechconfig?view=azure-node-latest)).

```javascript
speechConfig.enableDictation();
```

## <a name="change-source-language"></a>Ändern der Ausgangssprache

Eine gängige Aufgabe für die Spracherkennung ist die Angabe der Eingabesprache (Ausgangssprache). Im folgenden Beispiel wird gezeigt, wie Sie die Eingabesprache auf Italienisch festlegen. Navigieren Sie in Ihrem Code zu [`SpeechConfig`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechconfig?view=azure-node-latest), und fügen Sie direkt darunter die folgende Zeile hinzu:

```javascript
speechConfig.speechRecognitionLanguage = "it-IT";
```

Von der Eigenschaft [`speechRecognitionLanguage`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechconfig?view=azure-node-latest#speechrecognitionlanguage) wird eine Zeichenfolge im Format „Sprache-Gebietsschema“ erwartet. Sie können einen beliebigen Wert aus der Spalte **Gebietsschema** der Liste mit den unterstützten [Gebietsschemas/Sprachen](../../../language-support.md) angeben.

## <a name="improve-recognition-accuracy"></a>Verbessern der Erkennungsgenauigkeit

Es gibt mehrere Möglichkeiten, die Erkennungsgenauigkeit mit dem Speech SDK zu verbessern. Eine dieser Möglichkeiten sind Begriffslisten. Begriffslisten dienen zur Identifizierung bekannter Begriffe in Audiodaten, wie beispielsweise den Namen einer Person oder einen bestimmten Ort. Einer Begriffsliste können einzelne Wörter oder ganze Phrasen hinzugefügt werden. Bei der Erkennung wird ein Eintrag aus einer Begriffsliste verwendet, wenn in den Audiodaten eine exakte Übereinstimmung für den gesamten Begriff enthalten ist. Wenn keine genaue Übereinstimmung für den Ausdruck gefunden wird, wird die Erkennung nicht unterstützt.

> [!IMPORTANT]
> Das Begriffslistenfeature ist nur für Englisch verfügbar.

Wenn Sie eine Begriffsliste verwenden möchten, erstellen Sie zunächst ein Objekt vom Typ [`PhraseListGrammar`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/phraselistgrammar?view=azure-node-latest), und fügen Sie anschließend mithilfe von [`addPhrase`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/phraselistgrammar?view=azure-node-latest#addphrase-string-) bestimmte Wörter und Begriffe hinzu.

Änderungen an [`PhraseListGrammar`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/phraselistgrammar?view=azure-node-latest) werden bei der nächsten Erkennung oder nach erneuter Verbindungsherstellung mit dem Speech-Dienst wirksam.

```javascript
const phraseList = PhraseListGrammar.fromRecognizer(recognizer);
phraseList.addPhrase("Supercalifragilisticexpialidocious");
```

Wenn Sie Ihre Begriffsliste löschen möchten, verwenden Sie Folgendes:

```javascript
phraseList.clear();
```

### <a name="other-options-to-improve-recognition-accuracy"></a>Weitere Optionen zum Verbessern der Erkennungsgenauigkeit

Begriffslisten sind nur eine von mehreren Optionen zur Verbesserung der Erkennungsgenauigkeit. Sie können außerdem: 

* [Verbessern der Genauigkeit mithilfe von Custom Speech](../../../how-to-custom-speech.md)
* [Verbessern der Genauigkeit mithilfe von Mandantenmodellen](../../../tutorial-tenant-model.md)
