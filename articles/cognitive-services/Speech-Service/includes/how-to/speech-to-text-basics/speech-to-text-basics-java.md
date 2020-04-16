---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/11/2020
ms.author: trbye
ms.openlocfilehash: 2de836a63eed79b7d166db220218e0c11ef11e70
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81399781"
---
## <a name="prerequisites"></a>Voraussetzungen

In diesem Artikel wird vorausgesetzt, dass Sie über ein Azure-Konto und über ein Abonnement für den Speech-Dienst verfügen. Falls nicht, können Sie [den Speech-Dienst kostenlos testen](../../../get-started.md).

## <a name="install-the-speech-sdk"></a>Installieren des Speech SDK

Zuallererst muss das Speech SDK installiert werden. Verwenden Sie dazu die folgenden plattformspezifischen Anleitungen:

* <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/setup-platform?tabs=jre&pivots=programming-language-java" target="_blank">Java Runtime <span class="docon docon-navigate-external x-hidden-focus"></span></a>
* <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/setup-platform?tabs=android&pivots=programming-language-java" target="_blank">Android <span class="docon docon-navigate-external x-hidden-focus"></span></a>

## <a name="create-a-speech-configuration"></a>Erstellen einer Sprachkonfiguration

Um den Speech-Dienst über das Speech SDK aufrufen zu können, muss eine Sprachkonfiguration ([`SpeechConfig`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechconfig?view=azure-java-stable)) erstellt werden. Diese Klasse enthält Informationen zu Ihrem Abonnement. Hierzu zählen etwa Ihr Schlüssel und die zugeordnete Region, der Endpunkt, der Host oder das Autorisierungstoken.

> [!NOTE]
> Eine Konfiguration ist immer erforderlich. Dabei spielt es keine Rolle, ob Sie eine Spracherkennung, eine Sprachsynthese, eine Übersetzung oder eine Absichtserkennung durchführen möchten.

Eine Sprachkonfiguration ([`SpeechConfig`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechconfig?view=azure-java-stable)) kann auf unterschiedliche Weise initialisiert werden:

* Mit einem Abonnement: Übergeben Sie einen Schlüssel und die zugeordnete Region.
* Mit einem Endpunkt: Übergeben Sie einen Endpunkt für den Speech-Dienst. Ein Schlüssel oder Autorisierungstoken ist optional.
* Mit einem Host: Übergeben Sie eine Hostadresse. Ein Schlüssel oder Autorisierungstoken ist optional.
* Mit einem Autorisierungstoken: Übergeben Sie ein Autorisierungstoken und die zugeordnete Region.

Hier sehen Sie, wie eine Sprachkonfiguration ([`SpeechConfig`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechconfig?view=azure-java-stable)) mit einem Schlüssel und einer Region erstellt wird. Informationen zur Ermittlung Ihres Regionsbezeichners finden Sie auf der Seite zur [Unterstützung von Regionen](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#speech-sdk).

```java
SpeechConfig config = SpeechConfig.fromSubscription("YourSubscriptionKey", "YourServiceRegion");
```

## <a name="initialize-a-recognizer"></a>Initialisieren einer Erkennung

Nach der Erstellung einer Sprachkonfiguration ([`SpeechConfig`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechconfig?view=azure-java-stable)) muss eine Spracherkennung ([`SpeechRecognizer`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechrecognizer?view=azure-java-stable)) initialisiert werden. Wenn Sie eine Spracherkennung ([`SpeechRecognizer`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechrecognizer?view=azure-java-stable)) initialisieren, müssen Sie Ihre Sprachkonfiguration (`config`) übergeben. Dadurch werden die Anmeldeinformationen bereitgestellt, die der Speech-Dienst zur Überprüfung Ihrer Anforderung benötigt.

Wenn Sie für die Spracherkennung das Standardmikrofon Ihres Geräts verwenden, sollte [`SpeechRecognizer`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechrecognizer?view=azure-java-stable) wie folgt aussehen:

```java
SpeechRecognizer recognizer = new SpeechRecognizer(config);
```

Wenn Sie das Audioeingabegerät angeben möchten, müssen Sie eine Audiokonfiguration ([`AudioConfig`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.audio.audioconfig?view=azure-java-stable)) erstellen und beim Initialisieren Ihrer Spracherkennung ([`SpeechRecognizer`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechrecognizer?view=azure-java-stable)) den Parameter `audioConfig` angeben.

> [!TIP]
> Informationen zum Abrufen der Geräte-ID für Ihr Audioeingabegerät finden Sie [hier](../../../how-to-select-audio-input-devices.md).

Fügen Sie zunächst die folgenden `import`-Anweisungen hinzu.

```java
import java.util.concurrent.Future;
import com.microsoft.cognitiveservices.speech.*;
```

Anschließend können Sie wie folgt auf das Objekt `AudioConfig` verweisen:

```java
AudioConfig audioConfig = AudioConfig.fromDefaultMicrophoneInput();
SpeechRecognizer recognizer = new SpeechRecognizer(config, audioConfig);
```

Eine Audiokonfiguration (`audioConfig`) ist auch erforderlich, wenn Sie anstelle eines Mikrofons eine Audiodatei verwenden möchten. In diesem Fall muss beim Erstellen einer Audiokonfiguration ([`AudioConfig`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.audio.audioconfig?view=azure-java-stable)) allerdings `fromWavFileOutput` (anstelle von `fromDefaultMicrophoneInput`) aufgerufen und der Parameter `filename` übergeben werden.

```java
AudioConfig audioConfig = AudioConfig.fromWavFileInput("YourAudioFile.wav");
SpeechRecognizer recognizer = new SpeechRecognizer(config, audioConfig);
```

## <a name="recognize-speech"></a>Erkennen von Sprache

Von der Klasse [SpeechRecognizer](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechrecognizer?view=azure-java-stable) für das Speech SDK für Java werden verschiedene Methoden für die Spracherkennung verfügbar gemacht.

* Einzelerkennung (asynchron): Führt die Erkennung in einem nicht blockierenden (asynchronen) Modus durch. Dient zum Erkennen einer einzelnen Äußerung. Zur Erkennung des Endes einer einzelnen Äußerung wird auf Stille am Ende gelauscht oder gewartet, bis maximal 15 Sekunden an Audiodaten verarbeitet wurden.
* Kontinuierliche Erkennung (asynchron): Initialisiert asynchron einen kontinuierlichen Erkennungsvorgang. Eine Audiokonfiguration ist auch erforderlich, wenn Sie anstelle eines Mikrofons eine Audiodatei verwenden möchten. Zum Beenden der asynchronen kontinuierlichen Erkennung muss [stopContinuousRecognitionAsync](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechrecognizer.stopcontinuousrecognitionasync) aufgerufen werden.

> [!NOTE]
> Weitere Informationen zum Auswählen eines Spracherkennungsmodus finden Sie [hier](../../../how-to-choose-recognition-mode.md).

### <a name="single-shot-recognition"></a>Einzelerkennung

Das folgende Beispiel zeigt eine asynchrone Einzelerkennung mit [`recognizeOnceAsync`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechrecognizer.recognizeonceasync?view=azure-java-stable):

```java
Future<SpeechRecognitionResult> task = recognizer.recognizeOnceAsync();
SpeechRecognitionResult result = task.get();
```

Sie müssen etwas Code schreiben, um das Ergebnis zu behandeln. In diesem Beispiel wird [`result.getReason()`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.resultreason?view=azure-java-stable) ausgewertet:

* Das Erkennungsergebnis wird ausgegeben: `ResultReason.RecognizedSpeech`
* Wurde kein Erkennungstreffer gefunden, wird der Benutzer davon in Kenntnis gesetzt: `ResultReason.NoMatch`
* Ist ein Fehler aufgetreten, wird die Fehlermeldung ausgegeben: `ResultReason.Canceled`

```java
switch (result.getReason()) {
    case ResultReason.RecognizedSpeech:
        System.out.println("We recognized: " + result.getText());
        exitCode = 0;
        break;
    case ResultReason.NoMatch:
        System.out.println("NOMATCH: Speech could not be recognized.");
        break;
    case ResultReason.Canceled: {
            CancellationDetails cancellation = CancellationDetails.fromResult(result);
            System.out.println("CANCELED: Reason=" + cancellation.getReason());

            if (cancellation.getReason() == CancellationReason.Error) {
                System.out.println("CANCELED: ErrorCode=" + cancellation.getErrorCode());
                System.out.println("CANCELED: ErrorDetails=" + cancellation.getErrorDetails());
                System.out.println("CANCELED: Did you update the subscription info?");
            }
        }
        break;
}
```

### <a name="continuous-recognition"></a>Kontinuierliche Erkennung

Die kontinuierliche Erkennung ist etwas komplexer als die Einzelerkennung. Für die kontinuierliche Erkennung müssen die Ereignisse `recognizing`, `recognized` und `canceled` abonniert werden, um die Erkennungsergebnisse zu erhalten. Zum Beenden der Erkennung muss [`stopContinuousRecognitionAsync`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechrecognizer.stopcontinuousrecognitionasync) aufgerufen werden. Im folgenden Beispiel wird eine kontinuierliche Erkennung für eine Audioeingabedatei durchgeführt.

Als Erstes wird die Eingabe definiert und eine Spracherkennung ([`SpeechRecognizer`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechrecognizer?view=azure-java-stable)) initialisiert:

```java
AudioConfig audioConfig = AudioConfig.fromWavFileInput("YourAudioFile.wav");
SpeechRecognizer recognizer = new SpeechRecognizer(config, audioConfig);
```

Als Nächstes wird eine Variable erstellt, um den Zustand der Spracherkennung zu verwalten. Zunächst deklarieren wir einen Semaphor (`Semaphore`) für den Klassenbereich.

```java
private static Semaphore stopTranslationWithFileSemaphore;
```

Wir abonnieren die von [`SpeechRecognizer`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechrecognizer?view=azure-java-stable) gesendeten Ereignisse:

* [`recognizing`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechrecognizer.recognizing?view=azure-java-stable): Signal für Ereignisse mit Zwischenergebnissen der Erkennung.
* [`recognized`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechrecognizer.recognized?view=azure-java-stable): Signal für Ereignisse mit Endergebnissen der Erkennung. (Der Erkennungsversuch war also erfolgreich.)
* [`sessionStopped`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.recognizer.sessionstopped?view=azure-java-stable): Signal für Ereignisse, die das Ende einer Erkennungssitzung (bzw. eines Erkennungsvorgangs) angeben.
* [`canceled`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechrecognizer.canceled?view=azure-java-stable): Signal für Ereignisse mit Ergebnissen einer abgebrochenen Erkennung. (Der Erkennungsversuch wurde also aufgrund einer direkten Abbruchanforderungen oder infolge eines Transport- oder Protokollfehlers abgebrochen.)

```java
// First initialize the semaphore.
stopTranslationWithFileSemaphore = new Semaphore(0);

recognizer.recognizing.addEventListener((s, e) -> {
    System.out.println("RECOGNIZING: Text=" + e.getResult().getText());
});

recognizer.recognized.addEventListener((s, e) -> {
    if (e.getResult().getReason() == ResultReason.RecognizedSpeech) {
        System.out.println("RECOGNIZED: Text=" + e.getResult().getText());
    }
    else if (e.getResult().getReason() == ResultReason.NoMatch) {
        System.out.println("NOMATCH: Speech could not be recognized.");
    }
});

recognizer.canceled.addEventListener((s, e) -> {
    System.out.println("CANCELED: Reason=" + e.getReason());

    if (e.getReason() == CancellationReason.Error) {
        System.out.println("CANCELED: ErrorCode=" + e.getErrorCode());
        System.out.println("CANCELED: ErrorDetails=" + e.getErrorDetails());
        System.out.println("CANCELED: Did you update the subscription info?");
    }

    stopTranslationWithFileSemaphore.release();
});

recognizer.sessionStopped.addEventListener((s, e) -> {
    System.out.println("\n    Session stopped event.");
    stopTranslationWithFileSemaphore.release();
});
```

Nach Abschluss der Einrichtung kann [`stopContinuousRecognitionAsync`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechrecognizer.stopcontinuousrecognitionasync) aufgerufen werden.

```java
// Starts continuous recognition. Uses StopContinuousRecognitionAsync() to stop recognition.
recognizer.startContinuousRecognitionAsync().get();

// Waits for completion.
stopTranslationWithFileSemaphore.acquire();

// Stops recognition.
recognizer.stopContinuousRecognitionAsync().get();
```

### <a name="dictation-mode"></a>Diktiermodus

Bei Verwendung der kontinuierlichen Erkennung können Sie die Diktatverarbeitung mithilfe der entsprechenden Funktion „Diktat aktivieren“ aktivieren. Dieser Modus bewirkt, dass die Sprachkonfigurationsinstanz Wortbeschreibungen von Satzstrukturen wie z. B. Interpunktion interpretiert. Beispielsweise würde die Äußerung „Leben Sie in der Stadt Fragezeichen“ als Text „Leben Sie in der Stadt?“ interpretiert.

Verwenden Sie zum Aktivieren des Diktiermodus die Methode [`enableDictation`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechconfig.enabledictation?view=azure-java-stable) in Ihrer Sprachkonfiguration ([`SpeechConfig`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechconfig?view=azure-java-stable)).

```java
config.enableDictation();
```

## <a name="change-source-language"></a>Ändern der Ausgangssprache

Eine gängige Aufgabe für die Spracherkennung ist die Angabe der Eingabesprache (Ausgangssprache). Im folgenden Beispiel wird gezeigt, wie Sie die Eingabesprache auf Französisch festlegen. Navigieren Sie in Ihrem Code zu [`SpeechConfig`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechconfig?view=azure-java-stable), und fügen Sie direkt darunter die folgende Zeile hinzu:

```java
config.setSpeechRecognitionLanguage("fr-FR");
```

[`setSpeechRecognitionLanguage`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechconfig.setspeechrecognitionlanguage?view=azure-java-stable) ist ein Parameter, der eine Zeichenfolge als Argument akzeptiert. Sie können einen beliebigen Wert aus der Liste der unterstützten [Gebietsschemas/Sprachen](../../../language-support.md) angeben.

## <a name="improve-recognition-accuracy"></a>Verbessern der Erkennungsgenauigkeit

Es gibt mehrere Möglichkeiten, die Erkennungsgenauigkeit mit dem Speech SDK zu verbessern. Eine dieser Möglichkeiten sind Begriffslisten: Begriffslisten dienen zur Identifizierung bekannter Begriffe in Audiodaten, wie beispielsweise den Namen einer Person oder einen bestimmten Ort. Einer Begriffsliste können einzelne Wörter oder ganze Phrasen hinzugefügt werden. Bei der Erkennung wird ein Eintrag aus einer Begriffsliste verwendet, wenn in den Audiodaten eine exakte Übereinstimmung für den gesamten Begriff enthalten ist. Wenn keine genaue Übereinstimmung für den Ausdruck gefunden wird, wird die Erkennung nicht unterstützt.

> [!IMPORTANT]
> Das Begriffslistenfeature ist nur für Englisch verfügbar.

Wenn Sie eine Begriffsliste verwenden möchten, erstellen Sie zunächst ein Objekt vom Typ [`PhraseListGrammar`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.phraselistgrammar?view=azure-java-stable), und fügen Sie anschließend mithilfe von [`AddPhrase`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.phraselistgrammar.addphrase?view=azure-java-stable#com_microsoft_cognitiveservices_speech_PhraseListGrammar_addPhrase_String_) bestimmte Wörter und Begriffe hinzu.

Änderungen an [`PhraseListGrammar`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.phraselistgrammar?view=azure-java-stable) werden bei der nächsten Erkennung oder nach erneuter Verbindungsherstellung mit dem Speech-Dienst wirksam.

```java
PhraseListGrammar phraseList = PhraseListGrammar.fromRecognizer(recognizer);
phraseList.addPhrase("Supercalifragilisticexpialidocious");
```

Wenn Sie Ihre Begriffsliste löschen möchten, verwenden Sie Folgendes: 

```java
phraseList.clear();
```

### <a name="other-options-to-improve-recognition-accuracy"></a>Weitere Optionen zum Verbessern der Erkennungsgenauigkeit

Begriffslisten sind nur eine von mehreren Optionen zur Verbesserung der Erkennungsgenauigkeit. Sie können außerdem: 

* [Verbessern der Genauigkeit mithilfe von Custom Speech](../../../how-to-custom-speech.md)
* [Verbessern der Genauigkeit mithilfe von Mandantenmodellen](../../../tutorial-tenant-model.md)