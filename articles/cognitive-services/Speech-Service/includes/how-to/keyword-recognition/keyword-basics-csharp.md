---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 01/04/2021
ms.author: trbye
ms.openlocfilehash: 786f9587ab223cf87a48cd791f366049b94af59b
ms.sourcegitcommit: aeba98c7b85ad435b631d40cbe1f9419727d5884
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/04/2021
ms.locfileid: "97866106"
---
Laden Sie zunächst die Schlüsselwortmodelldatei mithilfe der statischen Funktion `FromFile()`, die ein `KeywordRecognitionModel` zurückgibt. Verwenden Sie den Pfad zu der `.table`-Datei, die Sie von Speech Studio heruntergeladen haben. Zusätzlich erstellen Sie eine `AudioConfig` unter Verwendung des Standardmikrofons und instanziieren dann ein neues `KeywordRecognizer` unter Verwendung der Audiokonfiguration.

```csharp
using Microsoft.CognitiveServices.Speech;
using Microsoft.CognitiveServices.Speech.Audio;

var keywordModel = KeywordRecognitionModel.FromFile("your/path/to/Activate_device.table");
using var audioConfig = AudioConfig.FromDefaultMicrophoneInput();
using var keywordRecognizer = new KeywordRecognizer(audioConfig);
```

Als nächstes erfolgt die Ausführung der Schlüsselworterkennung mit einem Aufruf von `RecognizeOnceAsync()` durch Übergabe Ihres Modellobjekts. Dadurch wird eine Sitzung zur Schlüsselworterkennung gestartet, die so lange dauert, bis das Schlüsselwort erkannt wird. Daher verwenden Sie dieses Entwurfsmuster in der Regel in Multithreadanwendungen oder in Anwendungsfällen, in denen Sie möglicherweise auf unbestimmte Zeit auf ein Aktivierungswort warten.

```csharp
KeywordRecognitionResult result = await keywordRecognizer.RecognizeOnceAsync(keywordModel);
```

> [!NOTE]
> Das hier gezeigte Beispiel verwendet die lokale Schlüsselworterkennung, da es kein `SpeechConfig`-Objekt für den Authentifizierungskontext benötigt und das Back-End nicht kontaktiert. Sie können jedoch sowohl die Schlüsselworterkennung als auch die Überprüfung [unter Verwendung einer direkten Back-End-Verbindung](../../../tutorial-voice-enable-your-bot-speech-sdk.md#view-the-source-code-that-enables-keyword) ausführen.

### <a name="continuous-recognition"></a>Kontinuierliche Erkennung

Andere Klassen im Sprach-SDK unterstützen die kontinuierliche Erkennung (sowohl für Sprach- als auch Absichtserkennung) mit Schlüsselworterkennung. Auf diese Weise können Sie denselben Code verwenden, den Sie normalerweise für die kontinuierliche Erkennung verwenden würden, mit der Möglichkeit, auf eine `.table`-Datei für Ihr Schlüsselwortmodell zu verweisen.

Befolgen Sie zur Spracherkennung das im [Schnellstart](https://docs.microsoft.com/azure/cognitive-services/speech-service/get-started-speech-to-text?tabs=script%2Cbrowser%2Cwindowsinstall&pivots=programming-language-csharp#continuous-recognition) gezeigte Entwurfsmuster, um kontinuierliche Erkennung einzurichten. Ersetzen Sie dann den Aufruf von `recognizer.StartContinuousRecognitionAsync()` durch `recognizer.StartKeywordRecognitionAsync(KeywordRecognitionModel)`, und übergeben Sie das `KeywordRecognitionModel`-Objekt. Verwenden Sie `recognizer.StopKeywordRecognitionAsync()` anstelle von `recognizer.StopContinuousRecognitionAsync()`, um die kontinuierliche Erkennung durch die Schlüsselworterkennung zu beenden.

Die Absichtserkennung verwendet ein mit den Funktionen [`StartKeywordRecognitionAsync`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.intent.intentrecognizer.startkeywordrecognitionasync?view=azure-dotnet#Microsoft_CognitiveServices_Speech_Intent_IntentRecognizer_StartKeywordRecognitionAsync_Microsoft_CognitiveServices_Speech_KeywordRecognitionModel_) und [`StopKeywordRecognitionAsync`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.intent.intentrecognizer.stopkeywordrecognitionasync?view=azure-dotnet#Microsoft_CognitiveServices_Speech_Intent_IntentRecognizer_StopKeywordRecognitionAsync) identisches Muster.