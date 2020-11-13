---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/06/2020
ms.author: trbye
ms.openlocfilehash: 434548d7d00468605ad0f1a52af99fbc4278adc1
ms.sourcegitcommit: 6109f1d9f0acd8e5d1c1775bc9aa7c61ca076c45
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2020
ms.locfileid: "94482652"
---
Die Funktion zum Erkennen und Transkribieren von menschlicher Sprache (Spracherkennung) ist eines der zentralen Features des Speech-Diensts. In diesem Schnellstart erfahren Sie, wie Sie das Speech SDK in Ihren Apps und Produkten verwenden, um hochwertige Spracherkennungen durchzuführen.

## <a name="skip-to-samples-on-github"></a>Mit den Beispielen auf GitHub fortfahren

Greifen Sie auf GitHub auf die [C++-Schnellstartbeispiele](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/cpp/windows) zu, falls Sie direkt zum Beispielcode springen möchten.

## <a name="prerequisites"></a>Voraussetzungen

In diesem Artikel wird vorausgesetzt, dass Sie über ein Azure-Konto und über ein Abonnement für den Speech-Dienst verfügen. Falls nicht, können Sie [den Speech-Dienst kostenlos testen](../../../overview.md#try-the-speech-service-for-free).

## <a name="install-the-speech-sdk"></a>Installieren des Speech SDK

Zuallererst muss das Speech SDK installiert werden. Verwenden Sie dazu die folgenden plattformspezifischen Anleitungen:

* <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/setup-platform?tabs=linux&pivots=programming-language-cpp" target="_blank">Linux <span class="docon docon-navigate-external x-hidden-focus"></span></a>
* <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/setup-platform?tabs=macos&pivots=programming-language-cpp" target="_blank">macOS <span class="docon docon-navigate-external x-hidden-focus"></span></a>
* <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/setup-platform?tabs=windows&pivots=programming-language-cpp" target="_blank">Windows <span class="docon docon-navigate-external x-hidden-focus"></span></a>

## <a name="create-a-speech-configuration"></a>Erstellen einer Sprachkonfiguration

Um den Speech-Dienst über das Speech SDK aufrufen zu können, muss eine Sprachkonfiguration ([`SpeechConfig`](/cpp/cognitive-services/speech/speechconfig)) erstellt werden. Diese Klasse enthält Informationen zu Ihrem Abonnement. Hierzu zählen etwa Ihr Schlüssel und die zugeordnete Region, der Endpunkt, der Host oder das Autorisierungstoken. Erstellen Sie mithilfe des Schlüssels und der Region eine Sprachkonfiguration ([`SpeechConfig`](/cpp/cognitive-services/speech/speechconfig)). Ihr Schlüssel-Regions-Paar finden Sie auf der Seite [Ermitteln von Schlüsseln und Region](../../../overview.md#find-keys-and-region).

```cpp
using namespace std;
using namespace Microsoft::CognitiveServices::Speech;

auto config = SpeechConfig::FromSubscription("<paste-your-subscription-key>", "<paste-your-region>");
```

Es gibt noch einige andere Möglichkeiten, eine Sprachkonfiguration ([`SpeechConfig`](/cpp/cognitive-services/speech/speechconfig)) zu initialisieren:

* Mit einem Endpunkt: Übergeben Sie einen Endpunkt für den Speech-Dienst. Ein Schlüssel oder Autorisierungstoken ist optional.
* Mit einem Host: Übergeben Sie eine Hostadresse. Ein Schlüssel oder Autorisierungstoken ist optional.
* Mit einem Autorisierungstoken: Übergeben Sie ein Autorisierungstoken und die zugeordnete Region.

> [!NOTE]
> Eine Konfiguration ist immer erforderlich. Dabei spielt es keine Rolle, ob Sie eine Spracherkennung, eine Sprachsynthese, eine Übersetzung oder eine Absichtserkennung durchführen möchten.

## <a name="recognize-from-microphone"></a>Erkennen über Mikrofon

Um Sprache über das Mikrofon Ihres Geräts zu erkennen, erstellen Sie mithilfe von `FromDefaultMicrophoneInput()` eine Audiokonfiguration (`AudioConfig`). Initialisieren Sie dann [`SpeechRecognizer`](/cpp/cognitive-services/speech/speechrecognizer), und übergeben Sie `audioConfig` und `config`.

```cpp
using namespace Microsoft::CognitiveServices::Speech::Audio;

auto audioConfig = AudioConfig::FromDefaultMicrophoneInput();
auto recognizer = SpeechRecognizer::FromConfig(config, audioConfig);

cout << "Speak into your microphone." << std::endl;
auto result = recognizer->RecognizeOnceAsync().get();
cout << "RECOGNIZED: Text=" << result->Text << std::endl;
```

Wenn Sie ein *bestimmtes* Audioeingabegerät verwenden möchten, müssen Sie in `AudioConfig` die Geräte-ID angeben. Informationen zum Abrufen der Geräte-ID für Ihr Audioeingabegerät finden Sie [hier](../../../how-to-select-audio-input-devices.md).

## <a name="recognize-from-file"></a>Erkennen aus Datei

Wenn Sie Sprache aus einer Audiodatei erkennen möchten, anstatt ein Mikrofon zu verwenden, müssen Sie trotzdem eine Audiokonfiguration (`AudioConfig`) erstellen. In diesem Fall rufen Sie beim Erstellen der Audiokonfiguration ([`AudioConfig`](/cpp/cognitive-services/speech/audio-audioconfig)) allerdings `FromWavFileInput()` (anstelle von `FromDefaultMicrophoneInput()`) auf und übergeben den Dateipfad.

```cpp
using namespace Microsoft::CognitiveServices::Speech::Audio;

auto audioInput = AudioConfig::FromWavFileInput("YourAudioFile.wav");
auto recognizer = SpeechRecognizer::FromConfig(config, audioInput);

auto result = recognizer->RecognizeOnceAsync().get();
cout << "RECOGNIZED: Text=" << result->Text << std::endl;
```

## <a name="recognize-speech"></a>Erkennen von Sprache

Von der Klasse [SpeechRecognizer](/cpp/cognitive-services/speech/speechrecognizer) für das Speech SDK für C++ werden verschiedene Methoden für die Spracherkennung verfügbar gemacht.

### <a name="single-shot-recognition"></a>Einzelerkennung

Bei der Einzelerkennung wird eine einzelne Äußerung asynchron erkannt. Zur Erkennung des Endes einer einzelnen Äußerung wird auf Stille am Ende gelauscht oder gewartet, bis maximal 15 Sekunden an Audiodaten verarbeitet wurden. Das folgende Beispiel zeigt eine asynchrone Einzelerkennung mit [`RecognizeOnceAsync`](/cpp/cognitive-services/speech/speechrecognizer#recognizeonceasync):

```cpp
auto result = recognizer->RecognizeOnceAsync().get();
```

Sie müssen etwas Code schreiben, um das Ergebnis zu behandeln. In diesem Beispiel wird [`result->Reason`](/cpp/cognitive-services/speech/recognitionresult#reason) ausgewertet:

* Das Erkennungsergebnis wird ausgegeben: `ResultReason::RecognizedSpeech`
* Wurde kein Erkennungstreffer gefunden, wird der Benutzer davon in Kenntnis gesetzt: `ResultReason::NoMatch`
* Ist ein Fehler aufgetreten, wird die Fehlermeldung ausgegeben: `ResultReason::Canceled`

```cpp
switch (result->Reason)
{
    case ResultReason::RecognizedSpeech:
        cout << "We recognized: " << result->Text << std::endl;
        break;
    case ResultReason::NoMatch:
        cout << "NOMATCH: Speech could not be recognized." << std::endl;
        break;
    case ResultReason::Canceled:
        {
            auto cancellation = CancellationDetails::FromResult(result);
            cout << "CANCELED: Reason=" << (int)cancellation->Reason << std::endl;
    
            if (cancellation->Reason == CancellationReason::Error) {
                cout << "CANCELED: ErrorCode= " << (int)cancellation->ErrorCode << std::endl;
                cout << "CANCELED: ErrorDetails=" << cancellation->ErrorDetails << std::endl;
                cout << "CANCELED: Did you update the subscription info?" << std::endl;
            }
        }
        break;
    default:
        break;
}
```

### <a name="continuous-recognition"></a>Kontinuierliche Erkennung

Die kontinuierliche Erkennung ist etwas komplexer als die Einzelerkennung. Für die kontinuierliche Erkennung müssen die Ereignisse `Recognizing`, `Recognized` und `Canceled` abonniert werden, um die Erkennungsergebnisse zu erhalten. Zum Beenden der Erkennung muss [StopContinuousRecognitionAsync](/cpp/cognitive-services/speech/speechrecognizer#stopcontinuousrecognitionasync) aufgerufen werden. Im folgenden Beispiel wird eine kontinuierliche Erkennung für eine Audioeingabedatei durchgeführt.

Als Erstes wird die Eingabe definiert und eine Spracherkennung ([`SpeechRecognizer`](/cpp/cognitive-services/speech/speechrecognizer)) initialisiert:

```cpp
auto audioInput = AudioConfig::FromWavFileInput("YourAudioFile.wav");
auto recognizer = SpeechRecognizer::FromConfig(config, audioInput);
```

Als Nächstes wird eine Variable erstellt, um den Zustand der Spracherkennung zu verwalten. Für den Anfang wird `promise<void>` deklariert, da wir zu Beginn der Erkennung sicher sein können, dass sie nicht abgeschlossen ist.

```cpp
promise<void> recognitionEnd;
```

Wir abonnieren die von [`SpeechRecognizer`](/cpp/cognitive-services/speech/speechrecognizer) gesendeten Ereignisse:

* [`Recognizing`](/cpp/cognitive-services/speech/asyncrecognizer#recognizing): Signal für Ereignisse mit Zwischenergebnissen der Erkennung.
* [`Recognized`](/cpp/cognitive-services/speech/asyncrecognizer#recognized): Signal für Ereignisse mit Endergebnissen der Erkennung. (Der Erkennungsversuch war also erfolgreich.)
* [`SessionStopped`](/cpp/cognitive-services/speech/asyncrecognizer#sessionstopped): Signal für Ereignisse, die das Ende einer Erkennungssitzung (bzw. eines Erkennungsvorgangs) angeben.
* [`Canceled`](/cpp/cognitive-services/speech/asyncrecognizer#canceled): Signal für Ereignisse mit Ergebnissen einer abgebrochenen Erkennung. (Der Erkennungsversuch wurde also aufgrund einer direkten Abbruchanforderungen oder infolge eines Transport- oder Protokollfehlers abgebrochen.)

```cpp
recognizer->Recognizing.Connect([](const SpeechRecognitionEventArgs& e)
    {
        cout << "Recognizing:" << e.Result->Text << std::endl;
    });

recognizer->Recognized.Connect([](const SpeechRecognitionEventArgs& e)
    {
        if (e.Result->Reason == ResultReason::RecognizedSpeech)
        {
            cout << "RECOGNIZED: Text=" << e.Result->Text 
                 << " (text could not be translated)" << std::endl;
        }
        else if (e.Result->Reason == ResultReason::NoMatch)
        {
            cout << "NOMATCH: Speech could not be recognized." << std::endl;
        }
    });

recognizer->Canceled.Connect([&recognitionEnd](const SpeechRecognitionCanceledEventArgs& e)
    {
        cout << "CANCELED: Reason=" << (int)e.Reason << std::endl;
        if (e.Reason == CancellationReason::Error)
        {
            cout << "CANCELED: ErrorCode=" << (int)e.ErrorCode << "\n"
                 << "CANCELED: ErrorDetails=" << e.ErrorDetails << "\n"
                 << "CANCELED: Did you update the subscription info?" << std::endl;

            recognitionEnd.set_value(); // Notify to stop recognition.
        }
    });

recognizer->SessionStopped.Connect([&recognitionEnd](const SessionEventArgs& e)
    {
        cout << "Session stopped.";
        recognitionEnd.set_value(); // Notify to stop recognition.
    });
```

Nach Abschluss der Einrichtung kann [`StopContinuousRecognitionAsync`](/cpp/cognitive-services/speech/speechrecognizer#startcontinuousrecognitionasync) aufgerufen werden.

```cpp
// Starts continuous recognition. Uses StopContinuousRecognitionAsync() to stop recognition.
recognizer->StartContinuousRecognitionAsync().get();

// Waits for recognition end.
recognitionEnd.get_future().get();

// Stops recognition.
recognizer->StopContinuousRecognitionAsync().get();
```

### <a name="dictation-mode"></a>Diktiermodus

Bei Verwendung der kontinuierlichen Erkennung können Sie die Diktatverarbeitung mithilfe der entsprechenden Funktion „Diktat aktivieren“ aktivieren. Dieser Modus bewirkt, dass die Sprachkonfigurationsinstanz Wortbeschreibungen von Satzstrukturen wie z. B. Interpunktion interpretiert. Beispielsweise würde die Äußerung „Leben Sie in der Stadt Fragezeichen“ als Text „Leben Sie in der Stadt?“ interpretiert.

Verwenden Sie zum Aktivieren des Diktiermodus die Methode [`EnableDictation`](/cpp/cognitive-services/speech/speechconfig#enabledictation) in Ihrer Sprachkonfiguration ([`SpeechConfig`](/cpp/cognitive-services/speech/speechconfig)).

```cpp
config->EnableDictation();
```

## <a name="change-source-language"></a>Ändern der Ausgangssprache

Eine gängige Aufgabe für die Spracherkennung ist die Angabe der Eingabesprache (Ausgangssprache). Im folgenden Beispiel wird gezeigt, wie Sie die Eingabesprache auf Deutsch festlegen. Navigieren Sie in Ihrem Code zu [`SpeechConfig`](/cpp/cognitive-services/speech/speechconfig), und fügen Sie direkt darunter die folgende Zeile hinzu:

```cpp
config->SetSpeechRecognitionLanguage("de-DE");
```

[`SetSpeechRecognitionLanguage`](/cpp/cognitive-services/speech/speechconfig#setspeechrecognitionlanguage) ist ein Parameter, der eine Zeichenfolge als Argument akzeptiert. Sie können einen beliebigen Wert aus der Liste der unterstützten [Gebietsschemas/Sprachen](../../../language-support.md) angeben.

## <a name="improve-recognition-accuracy"></a>Verbessern der Erkennungsgenauigkeit

Es gibt mehrere Möglichkeiten, die Erkennungsgenauigkeit mit dem Speech SDK zu verbessern. Eine dieser Möglichkeiten sind Begriffslisten: Begriffslisten dienen zur Identifizierung bekannter Begriffe in Audiodaten, wie beispielsweise den Namen einer Person oder einen bestimmten Ort. Einer Begriffsliste können einzelne Wörter oder ganze Phrasen hinzugefügt werden. Bei der Erkennung wird ein Eintrag aus einer Begriffsliste verwendet, wenn in den Audiodaten eine exakte Übereinstimmung für den gesamten Begriff enthalten ist. Wenn keine genaue Übereinstimmung für den Ausdruck gefunden wird, wird die Erkennung nicht unterstützt.

> [!IMPORTANT]
> Das Begriffslistenfeature ist nur für Englisch verfügbar.

Wenn Sie eine Begriffsliste verwenden möchten, erstellen Sie zunächst ein Objekt vom Typ [`PhraseListGrammar`](/cpp/cognitive-services/speech/phraselistgrammar), und fügen Sie anschließend mithilfe von [`AddPhrase`](/cpp/cognitive-services/speech/phraselistgrammar#addphrase) bestimmte Wörter und Begriffe hinzu.

Änderungen an [`PhraseListGrammar`](/cpp/cognitive-services/speech/phraselistgrammar) werden bei der nächsten Erkennung oder nach erneuter Verbindungsherstellung mit dem Speech-Dienst wirksam.

```cpp
auto phraseListGrammar = PhraseListGrammar::FromRecognizer(recognizer);
phraseListGrammar->AddPhrase("Supercalifragilisticexpialidocious");
```

Wenn Sie Ihre Begriffsliste löschen möchten, verwenden Sie Folgendes: 

```cpp
phraseListGrammar->Clear();
```

### <a name="other-options-to-improve-recognition-accuracy"></a>Weitere Optionen zum Verbessern der Erkennungsgenauigkeit

Begriffslisten sind nur eine von mehreren Optionen zur Verbesserung der Erkennungsgenauigkeit. Sie können außerdem: 

* [Verbessern der Genauigkeit mithilfe von Custom Speech](../../../how-to-custom-speech.md)
* [Verbessern der Genauigkeit mithilfe von Mandantenmodellen](../../../tutorial-tenant-model.md)