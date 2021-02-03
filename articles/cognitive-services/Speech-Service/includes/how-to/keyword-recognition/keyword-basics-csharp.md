---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 01/04/2021
ms.author: trbye
ms.openlocfilehash: 4ec9c847158c9b8a1160cb6f4510912ed83a9fae
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2021
ms.locfileid: "98948492"
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

Befolgen Sie zur Spracherkennung das im [Schnellstart](../../../get-started-speech-to-text.md?pivots=programming-language-csharp&tabs=script%2cbrowser%2cwindowsinstall#continuous-recognition) gezeigte Entwurfsmuster, um kontinuierliche Erkennung einzurichten. Ersetzen Sie dann den Aufruf von `recognizer.StartContinuousRecognitionAsync()` durch `recognizer.StartKeywordRecognitionAsync(KeywordRecognitionModel)`, und übergeben Sie das `KeywordRecognitionModel`-Objekt. Verwenden Sie `recognizer.StopKeywordRecognitionAsync()` anstelle von `recognizer.StopContinuousRecognitionAsync()`, um die kontinuierliche Erkennung durch die Schlüsselworterkennung zu beenden.

Die Absichtserkennung verwendet ein mit den Funktionen [`StartKeywordRecognitionAsync`](/dotnet/api/microsoft.cognitiveservices.speech.intent.intentrecognizer.startkeywordrecognitionasync#Microsoft_CognitiveServices_Speech_Intent_IntentRecognizer_StartKeywordRecognitionAsync_Microsoft_CognitiveServices_Speech_KeywordRecognitionModel_) und [`StopKeywordRecognitionAsync`](/dotnet/api/microsoft.cognitiveservices.speech.intent.intentrecognizer.stopkeywordrecognitionasync#Microsoft_CognitiveServices_Speech_Intent_IntentRecognizer_StopKeywordRecognitionAsync) identisches Muster.
