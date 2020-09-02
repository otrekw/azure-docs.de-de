---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/11/2020
ms.author: trbye
ms.custom: devx-track-csharp
ms.openlocfilehash: 8a8647e7f19b55547bbb7eff6f1f3bc1f5282c89
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/26/2020
ms.locfileid: "88934536"
---
## <a name="prerequisites"></a>Voraussetzungen

In diesem Artikel wird vorausgesetzt, dass Sie über ein Azure-Konto und über ein Abonnement für den Speech-Dienst verfügen. Falls nicht, können Sie [den Speech-Dienst kostenlos testen](../../../get-started.md).

## <a name="install-the-speech-sdk"></a>Installieren des Speech SDK

Zuallererst muss das Speech SDK installiert werden. Verwenden Sie dazu die folgenden plattformspezifischen Anleitungen:

* <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/setup-platform?tabs=dotnet&pivots=programming-language-csharp" target="_blank">.NET Framework <span class="docon docon-navigate-external x-hidden-focus"></span></a>
* <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/setup-platform?tabs=dotnetcore&pivots=programming-language-csharp" target="_blank">.NET Core <span class="docon docon-navigate-external x-hidden-focus"></span></a>
* <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/setup-platform?tabs=unity&pivots=programming-language-csharp" target="_blank">Unity <span class="docon docon-navigate-external x-hidden-focus"></span></a>
* <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/setup-platform?tabs=uwps&pivots=programming-language-csharp" target="_blank">UWP <span class="docon docon-navigate-external x-hidden-focus"></span></a>
* <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/setup-platform?tabs=xaml&pivots=programming-language-csharp" target="_blank">Xamarin <span class="docon docon-navigate-external x-hidden-focus"></span></a>

## <a name="create-a-speech-configuration"></a>Erstellen einer Sprachkonfiguration

Um den Speech-Dienst über das Speech SDK aufrufen zu können, muss eine Sprachkonfiguration ([`SpeechConfig`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig?view=azure-dotnet)) erstellt werden. Diese Klasse enthält Informationen zu Ihrem Abonnement. Hierzu zählen etwa Ihr Schlüssel und die zugeordnete Region, der Endpunkt, der Host oder das Autorisierungstoken.

> [!NOTE]
> Eine Konfiguration ist immer erforderlich. Dabei spielt es keine Rolle, ob Sie eine Spracherkennung, eine Sprachsynthese, eine Übersetzung oder eine Absichtserkennung durchführen möchten.

Eine Sprachkonfiguration ([`SpeechConfig`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig?view=azure-dotnet)) kann auf unterschiedliche Weise initialisiert werden:

* Mit einem Abonnement: Übergeben Sie einen Schlüssel und die zugeordnete Region.
* Mit einem Endpunkt: Übergeben Sie einen Endpunkt für den Speech-Dienst. Ein Schlüssel oder Autorisierungstoken ist optional.
* Mit einem Host: Übergeben Sie eine Hostadresse. Ein Schlüssel oder Autorisierungstoken ist optional.
* Mit einem Autorisierungstoken: Übergeben Sie ein Autorisierungstoken und die zugeordnete Region.

Hier sehen Sie, wie eine Sprachkonfiguration ([`SpeechConfig`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig?view=azure-dotnet)) mit einem Schlüssel und einer Region erstellt wird. Informationen zur Ermittlung Ihres Regionsbezeichners finden Sie auf der Seite zur [Unterstützung von Regionen](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#speech-sdk).

```csharp
var speechConfig = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");
```

## <a name="initialize-a-recognizer"></a>Initialisieren einer Erkennung

Nach der Erstellung einer Sprachkonfiguration ([`SpeechConfig`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig?view=azure-dotnet)) muss eine Spracherkennung ([`SpeechRecognizer`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechrecognizer?view=azure-dotnet)) initialisiert werden. Wenn Sie eine Spracherkennung ([`SpeechRecognizer`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechrecognizer?view=azure-dotnet)) initialisieren, müssen Sie Ihre Sprachkonfiguration (`speechConfig`) übergeben. Dadurch werden die Anmeldeinformationen bereitgestellt, die der Speech-Dienst zur Überprüfung Ihrer Anforderung benötigt.

Wenn Sie für die Spracherkennung das Standardmikrofon Ihres Geräts verwenden, sollte [`SpeechRecognizer`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechrecognizer?view=azure-dotnet) wie folgt aussehen:

```csharp
using var recognizer = new SpeechRecognizer(speechConfig);
```

Wenn Sie das Audioeingabegerät angeben möchten, müssen Sie eine Audiokonfiguration ([`AudioConfig`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.audio.audioconfig?view=azure-dotnet)) erstellen und beim Initialisieren Ihrer Spracherkennung ([`SpeechRecognizer`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechrecognizer?view=azure-dotnet)) den Parameter `audioConfig` angeben.

> [!TIP]
> Informationen zum Abrufen der Geräte-ID für Ihr Audioeingabegerät finden Sie [hier](../../../how-to-select-audio-input-devices.md).

Fügen Sie zunächst die folgende `using`-Anweisung hinzu.

```csharp
using Microsoft.CognitiveServices.Speech.Audio;
```

Anschließend können Sie wie folgt auf das Objekt `AudioConfig` verweisen:

```csharp
using var audioConfig = AudioConfig.FromDefaultMicrophoneInput();
using var recognizer = new SpeechRecognizer(speechConfig, audioConfig);
```

Eine Audiokonfiguration (`audioConfig`) ist auch erforderlich, wenn Sie anstelle eines Mikrofons eine Audiodatei verwenden möchten. In diesem Fall muss beim Erstellen einer Audiokonfiguration ([`AudioConfig`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.audio.audioconfig?view=azure-dotnet)) allerdings `FromWavFileOutput` (anstelle von `FromDefaultMicrophoneInput`) aufgerufen und der Parameter `filename` übergeben werden.


```csharp
using var audioConfig = AudioConfig.FromWavFileInput("YourAudioFile.wav");
using var recognizer = new SpeechRecognizer(speechConfig, audioConfig);
```

## <a name="recognize-speech"></a>Erkennen von Sprache

Von der Klasse [SpeechRecognizer](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechrecognizer?view=azure-dotnet) für das Speech SDK für C# werden verschiedene Methoden für die Spracherkennung verfügbar gemacht.

* Einzelerkennung (asynchron): Führt die Erkennung in einem nicht blockierenden (asynchronen) Modus durch. Dient zum Erkennen einer einzelnen Äußerung. Zur Erkennung des Endes einer einzelnen Äußerung wird auf Stille am Ende gelauscht oder gewartet, bis maximal 15 Sekunden an Audiodaten verarbeitet wurden.
* Kontinuierliche Erkennung (asynchron): Initialisiert asynchron einen kontinuierlichen Erkennungsvorgang. Der Benutzer registriert sich für Ereignisse und behandelt verschiedene Anwendungszustände. Zum Beenden der asynchronen kontinuierlichen Erkennung muss [`StopContinuousRecognitionAsync`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechrecognizer.stopcontinuousrecognitionasync?view=azure-dotnet) aufgerufen werden.

> [!NOTE]
> Weitere Informationen zum Auswählen eines Spracherkennungsmodus finden Sie [hier](../../../how-to-choose-recognition-mode.md).

### <a name="single-shot-recognition"></a>Einzelerkennung

Das folgende Beispiel zeigt eine asynchrone Einzelerkennung mit [`RecognizeOnceAsync`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechrecognizer.recognizeonceasync?view=azure-dotnet):

```csharp
var result = await recognizer.RecognizeOnceAsync();
```

Sie müssen etwas Code schreiben, um das Ergebnis zu behandeln. In diesem Beispiel wird [`result.Reason`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.recognitionresult.reason?view=azure-dotnet) ausgewertet:

* Das Erkennungsergebnis wird ausgegeben: `ResultReason.RecognizedSpeech`
* Wurde kein Erkennungstreffer gefunden, wird der Benutzer davon in Kenntnis gesetzt: `ResultReason.NoMatch`
* Ist ein Fehler aufgetreten, wird die Fehlermeldung ausgegeben: `ResultReason.Canceled`

```csharp
switch (result.Reason)
{
    case ResultReason.RecognizedSpeech:
        Console.WriteLine($"RECOGNIZED: Text={result.Text}");
        Console.WriteLine($"    Intent not recognized.");
        break;
    case ResultReason.NoMatch:
        Console.WriteLine($"NOMATCH: Speech could not be recognized.");
        break;
    case ResultReason.Canceled:
        var cancellation = CancellationDetails.FromResult(result);
        Console.WriteLine($"CANCELED: Reason={cancellation.Reason}");

        if (cancellation.Reason == CancellationReason.Error)
        {
            Console.WriteLine($"CANCELED: ErrorCode={cancellation.ErrorCode}");
            Console.WriteLine($"CANCELED: ErrorDetails={cancellation.ErrorDetails}");
            Console.WriteLine($"CANCELED: Did you update the subscription info?");
        }
        break;
}
```

### <a name="continuous-recognition"></a>Kontinuierliche Erkennung

Die kontinuierliche Erkennung ist etwas komplexer als die Einzelerkennung. Für die kontinuierliche Erkennung müssen die Ereignisse `Recognizing`, `Recognized` und `Canceled` abonniert werden, um die Erkennungsergebnisse zu erhalten. Zum Beenden der Erkennung muss [`StopContinuousRecognitionAsync`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechrecognizer.stopcontinuousrecognitionasync?view=azure-dotnet) aufgerufen werden. Im folgenden Beispiel wird eine kontinuierliche Erkennung für eine Audioeingabedatei durchgeführt.

Als Erstes wird die Eingabe definiert und eine Spracherkennung ([`SpeechRecognizer`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechrecognizer?view=azure-dotnet)) initialisiert:

```csharp
using var audioConfig = AudioConfig.FromWavFileInput("YourAudioFile.wav");
using var recognizer = new SpeechRecognizer(speechConfig, audioConfig);
```
Als Nächstes wird eine Variable erstellt, um den Zustand der Spracherkennung zu verwalten. Im Anschluss an die vorherigen Deklarationen deklarieren wir zunächst Folgendes: `TaskCompletionSource<int>`.

```csharp
var stopRecognition = new TaskCompletionSource<int>();
```

Wir abonnieren die von [`SpeechRecognizer`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechrecognizer?view=azure-dotnet) gesendeten Ereignisse:

* [`Recognizing`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechrecognizer.recognizing?view=azure-dotnet): Signal für Ereignisse mit Zwischenergebnissen der Erkennung.
* [`Recognized`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechrecognizer.recognized?view=azure-dotnet): Signal für Ereignisse mit Endergebnissen der Erkennung. (Der Erkennungsversuch war also erfolgreich.)
* [`SessionStopped`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.recognizer.sessionstopped?view=azure-dotnet): Signal für Ereignisse, die das Ende einer Erkennungssitzung (bzw. eines Erkennungsvorgangs) angeben.
* [`Canceled`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechrecognizer.canceled?view=azure-dotnet): Signal für Ereignisse mit Ergebnissen einer abgebrochenen Erkennung. (Der Erkennungsversuch wurde also aufgrund einer direkten Abbruchanforderungen oder infolge eines Transport- oder Protokollfehlers abgebrochen.)

```csharp
recognizer.Recognizing += (s, e) =>
{
    Console.WriteLine($"RECOGNIZING: Text={e.Result.Text}");
};

recognizer.Recognized += (s, e) =>
{
    if (e.Result.Reason == ResultReason.RecognizedSpeech)
    {
        Console.WriteLine($"RECOGNIZED: Text={e.Result.Text}");
    }
    else if (e.Result.Reason == ResultReason.NoMatch)
    {
        Console.WriteLine($"NOMATCH: Speech could not be recognized.");
    }
};

recognizer.Canceled += (s, e) =>
{
    Console.WriteLine($"CANCELED: Reason={e.Reason}");

    if (e.Reason == CancellationReason.Error)
    {
        Console.WriteLine($"CANCELED: ErrorCode={e.ErrorCode}");
        Console.WriteLine($"CANCELED: ErrorDetails={e.ErrorDetails}");
        Console.WriteLine($"CANCELED: Did you update the subscription info?");
    }

    stopRecognition.TrySetResult(0);
};

recognizer.SessionStopped += (s, e) =>
{
    Console.WriteLine("\n    Session stopped event.");
    stopRecognition.TrySetResult(0);
};
```

Nach Abschluss der Einrichtung kann [`StopContinuousRecognitionAsync`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechrecognizer.stopcontinuousrecognitionasync?view=azure-dotnet) aufgerufen werden.

```csharp
// Starts continuous recognition. Uses StopContinuousRecognitionAsync() to stop recognition.
await recognizer.StartContinuousRecognitionAsync();

// Waits for completion. Use Task.WaitAny to keep the task rooted.
Task.WaitAny(new[] { stopRecognition.Task });

// Stops recognition.
await recognizer.StopContinuousRecognitionAsync();
```

### <a name="dictation-mode"></a>Diktiermodus

Bei Verwendung der kontinuierlichen Erkennung können Sie die Diktatverarbeitung mithilfe der entsprechenden Funktion „Diktat aktivieren“ aktivieren. Dieser Modus bewirkt, dass die Sprachkonfigurationsinstanz Wortbeschreibungen von Satzstrukturen wie z. B. Interpunktion interpretiert. Beispielsweise würde die Äußerung „Leben Sie in der Stadt Fragezeichen“ als Text „Leben Sie in der Stadt?“ interpretiert.

Verwenden Sie zum Aktivieren des Diktiermodus die Methode [`EnableDictation`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig.enabledictation?view=azure-dotnet) in Ihrer Sprachkonfiguration ([`SpeechConfig`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig?view=azure-dotnet)).

```csharp
speechConfig.EnableDictation();
```

## <a name="change-source-language"></a>Ändern der Ausgangssprache

Eine gängige Aufgabe für die Spracherkennung ist die Angabe der Eingabesprache (Ausgangssprache). Im folgenden Beispiel wird gezeigt, wie Sie die Eingabesprache auf Italienisch festlegen. Navigieren Sie in Ihrem Code zu [`SpeechConfig`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig?view=azure-dotnet), und fügen Sie direkt darunter die folgende Zeile hinzu:

```csharp
speechConfig.SpeechRecognitionLanguage = "it-IT";
```

Von der Eigenschaft [`SpeechRecognitionLanguage`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig.speechrecognitionlanguage?view=azure-dotnet) wird eine Zeichenfolge im Format „Sprache-Gebietsschema“ erwartet. Sie können einen beliebigen Wert aus der Spalte **Gebietsschema** der Liste mit den unterstützten [Gebietsschemas/Sprachen](../../../language-support.md) angeben.

## <a name="improve-recognition-accuracy"></a>Verbessern der Erkennungsgenauigkeit

Es gibt mehrere Möglichkeiten, die Erkennungsgenauigkeit mit dem Speech SDK zu verbessern. Eine dieser Möglichkeiten sind Begriffslisten: Begriffslisten dienen zur Identifizierung bekannter Begriffe in Audiodaten, wie beispielsweise den Namen einer Person oder einen bestimmten Ort. Einer Begriffsliste können einzelne Wörter oder ganze Phrasen hinzugefügt werden. Bei der Erkennung wird ein Eintrag aus einer Begriffsliste verwendet, wenn in den Audiodaten eine exakte Übereinstimmung für den gesamten Begriff enthalten ist. Wenn keine genaue Übereinstimmung für den Ausdruck gefunden wird, wird die Erkennung nicht unterstützt.

> [!IMPORTANT]
> Das Begriffslistenfeature ist nur für Englisch verfügbar.

Wenn Sie eine Begriffsliste verwenden möchten, erstellen Sie zunächst ein Objekt vom Typ [`PhraseListGrammar`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.phraselistgrammar?view=azure-dotnet), und fügen Sie anschließend mithilfe von [`AddPhrase`](https://docs.microsoft.com//dotnet/api/microsoft.cognitiveservices.speech.phraselistgrammar.addphrase?view=azure-dotnet) bestimmte Wörter und Begriffe hinzu.

Änderungen an [`PhraseListGrammar`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.phraselistgrammar?view=azure-dotnet) werden bei der nächsten Erkennung oder nach erneuter Verbindungsherstellung mit dem Speech-Dienst wirksam.

```csharp
var phraseList = PhraseListGrammar.FromRecognizer(recognizer);
phraseList.AddPhrase("Supercalifragilisticexpialidocious");
```

Wenn Sie Ihre Begriffsliste löschen möchten, verwenden Sie Folgendes: 

```csharp
phraseList.Clear();
```

### <a name="other-options-to-improve-recognition-accuracy"></a>Weitere Optionen zum Verbessern der Erkennungsgenauigkeit

Begriffslisten sind nur eine von mehreren Optionen zur Verbesserung der Erkennungsgenauigkeit. Sie können außerdem: 

* [Verbessern der Genauigkeit mithilfe von Custom Speech](../../../how-to-custom-speech.md)
* [Verbessern der Genauigkeit mithilfe von Mandantenmodellen](../../../tutorial-tenant-model.md)