---
title: Auswählen eines Spracherkennungsmodus mit dem Sprach-SDK
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie den besten Erkennungsmodus auswählen, wenn Sie das Sprach-SDK verwenden.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/10/2020
ms.author: dapine
zone_pivot_groups: programming-languages-set-two
ms.openlocfilehash: d997cb592d9d648998f2b44d9f61f465f05faeb0
ms.sourcegitcommit: 72c2da0def8aa7ebe0691612a89bb70cd0c5a436
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "79079818"
---
# <a name="choose-a-speech-recognition-mode"></a>Auswählen eines Spracherkennungsmodus

Wenn Sie Spracherkennungsvorgänge in Erwägung ziehen, stehen Ihnen im [Sprach-SDK](speech-sdk.md) mehrere Modi für die Verarbeitung von Sprache zur Verfügung. Vom Konzept her wird oft die Bezeichnung *Erkennungsmodus* verwendet. In diesem Artikel werden die verschiedenen Erkennungsmodi verglichen.

## <a name="recognize-once"></a>Einmal erkennen

Wenn Sie jeweils jede Äußerung als einen „Satz“ verarbeiten möchten, verwenden Sie die „Einmal erkennen“-Funktion. Diese Methode untersucht eine erkannte Äußerung von der Eingabe ab dem Anfang der erkannten Sprache bis zur nächsten Pause. Normalerweise markiert eine Pause das Ende eines Satzes oder eines Gedankens.

Am Ende einer erkannten Äußerung hält der Dienst die Verarbeitung von Audiodaten aus dieser Anforderung an. Der Höchstwert für die Erkennung ist eine Satzdauer von 20 Sekunden.

::: zone pivot="programming-language-csharp"

Weitere Informationen zur Verwendung der `RecognizeOnceAsync`-Funktion finden Sie in der [Dokumentation zum .NET-Sprach-SDK](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechrecognizer.recognizeonceasync?view=azure-dotnet#Microsoft_CognitiveServices_Speech_SpeechRecognizer_RecognizeOnceAsync).

```csharp
var result = await recognizer.RecognizeOnceAsync();
```

::: zone-end
::: zone pivot="programming-language-cpp"

Weitere Informationen zur Verwendung der `RecognizeOnceAsync`-Funktion finden Sie in der [Dokumentation zum C++-Sprach-SDK](https://docs.microsoft.com/cpp/cognitive-services/speech/asyncrecognizer#recognizeonceasync).

```cpp
auto result = recognize->RecognizeOnceAsync().get();
```

::: zone-end
::: zone pivot="programming-language-java"

Weitere Informationen zur Verwendung der `recognizeOnceAsync`-Funktion finden Sie in der [Dokumentation zum Java-Sprach-SDK](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.SpeechRecognizer.recognizeOnceAsync?view=azure-java-stable).

```java
SpeechRecognitionResult result = recognizer.recognizeOnceAsync().get();
```

::: zone-end
::: zone pivot="programming-language-python"

Weitere Informationen zur Verwendung der `recognize_once`-Funktion finden Sie in der [Dokumentation zum Python-Sprach-SDK](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechrecognizer?view=azure-python#recognize-once------azure-cognitiveservices-speech-speechrecognitionresult).

```python
result = speech_recognizer.recognize_once()
```

::: zone-end
::: zone pivot="programming-language-more"

Weitere Sprachen finden Sie in der [Referenzdokumentation für das Sprach-SDK](speech-to-text.md#speech-sdk-reference-docs).

::: zone-end

## <a name="continuous"></a>Fortlaufend

Wenn Sie eine langfristige Erkennung benötigen, verwenden Sie die Start- und entsprechenden Stoppfunktionen für fortlaufende Erkennung. Die Startfunktion wird gestartet und verarbeitet fortlaufend alle Äußerungen, bis Sie die Stoppfunktion aufrufen oder eine zu lange Schweigephase aufgetreten ist. Wenn Sie den kontinuierlichen Modus verwenden, stellen Sie sicher, dass Sie sich bei den verschiedenen Ereignissen registrieren, die ausgelöst werden können. Beispielsweise wird das „Erkannt“-Ereignis ausgelöst, wenn Spracherkennung auftritt. Um die Erkennung zu verarbeiten, muss ein Ereignishandler vorhanden sein.

::: zone pivot="programming-language-csharp"

```csharp
// Subscribe to event
recognizer.Recognized += (s, e) => 
{
    if (e.Result.Reason == ResultReason.RecognizedSpeech)
    {
        // Do something with the recognized text
        // e.Result.Text
    }
};

// Start continuous speech recognition
await recognizer.StartContinuousRecognitionAsync();

// Stop continuous speech recognition
await recognizer.StopContinuousRecognitionAsync();
```

::: zone-end
::: zone pivot="programming-language-cpp"

```cpp
// Connect to event
recognizer->Recognized.Connect([] (const SpeechRecognitionEventArgs& e)
{
    if (e.Result->Reason == ResultReason::RecognizedSpeech)
    {
        // Do something with the recognized text
        // e.Result->Text
    }
});

// Start continuous speech recognition
recognizer->StartContinuousRecognitionAsync().get();

// Stop continuous speech recognition
recognizer->StopContinuousRecognitionAsync().get();
```

::: zone-end
::: zone pivot="programming-language-java"

```java
recognizer.recognized.addEventListener((s, e) -> {
    if (e.getResult().getReason() == ResultReason.RecognizedSpeech) {
        // Do something with the recognized text
        // e.getResult().getText()
    }
});

// Start continuous speech recognition
recognizer.startContinuousRecognitionAsync().get();

// Stop continuous speech recognition
recognizer.stopContinuousRecognitionAsync().get();
```

::: zone-end
::: zone pivot="programming-language-python"

```python
def recognized_cb(evt):
    if evt.result.reason == speechsdk.ResultReason.RecognizedSpeech:
        # Do something with the recognized text
        # evt.result.text

speech_recognizer.recognized.connect(recognized_cb)

# Start continuous speech recognition
speech_recognizer.start_continuous_recognition()

# Stop continuous speech recognition
speech_recognizer.stop_continuous_recognition()
```

::: zone-end
::: zone pivot="programming-language-more"

Weitere Sprachen finden Sie in der [Referenzdokumentation für das Sprach-SDK](speech-to-text.md#speech-sdk-reference-docs).

::: zone-end

## <a name="dictation"></a>Diktieren

Bei Verwendung der kontinuierlichen Erkennung können Sie die Diktatverarbeitung mithilfe der entsprechenden Funktion „Diktat aktivieren“ aktivieren. Dieser Modus bewirkt, dass die Sprachkonfigurationsinstanz Wortbeschreibungen von Satzstrukturen wie z. B. Interpunktion interpretiert. Beispielsweise würde die Äußerung „Leben Sie in der Stadt Fragezeichen“ als Text „Leben Sie in der Stadt?“ interpretiert.

::: zone pivot="programming-language-csharp"

Weitere Informationen zur Verwendung der `EnableDictation`-Funktion finden Sie in der [Dokumentation zum .NET-Sprach-SDK](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig.enabledictation?view=azure-dotnet#Microsoft_CognitiveServices_Speech_SpeechConfig_EnableDictation).

```csharp
// Enable diction
SpeechConfig.EnableDictation();
```

::: zone-end
::: zone pivot="programming-language-cpp"

Weitere Informationen zur Verwendung der `EnableDictation`-Funktion finden Sie in der [Dokumentation zum C++-Sprach-SDK](https://docs.microsoft.com/cpp/cognitive-services/speech/speechconfig#enabledictation).

```cpp
// Enable diction
SpeechConfig->EnableDictation();
```

::: zone-end
::: zone pivot="programming-language-java"

Weitere Informationen zur Verwendung der `enableDictation`-Funktion finden Sie in der [Dokumentation zum Java-Sprach-SDK](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.SpeechConfig.enableDictation?view=azure-java-stable).

```java
// Enable diction
SpeechConfig.enableDictation();
```

::: zone-end
::: zone pivot="programming-language-python"

Weitere Informationen zur Verwendung der `enable_dictation`-Funktion finden Sie in der [Dokumentation zum Python-Sprach-SDK](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig?view=azure-python#enable-dictation--).

```python
# Enable diction
SpeechConfig.enable_dictation()
```

::: zone-end
::: zone pivot="programming-language-more"

Weitere Sprachen finden Sie in der [Referenzdokumentation für das Sprach-SDK](speech-to-text.md#speech-sdk-reference-docs).

::: zone-end

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Lernen Sie weitere Sprach-SDK-Beispiele auf GitHub kennen.](https://aka.ms/csspeech/samples)
