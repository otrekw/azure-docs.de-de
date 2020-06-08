---
title: Verwenden der automatischen Spracherkennung zur Umwandlung von Sprache in Text
titleSuffix: Azure Cognitive Services
description: Das Sprach-SDK unterstützt die automatische Spracherkennung für die Umwandlung von Sprache in Text. Mit diesem Feature wird die Audioeingabe mit einer bereitgestellten Liste von Sprachen verglichen und die wahrscheinlichste Übereinstimmung bestimmt. Der zurückgegebene Wert kann dann verwendet werden, um das Sprachmodell auszuwählen, das für die Spracherkennung verwendet wird.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/19/2020
ms.author: trbye
zone_pivot_groups: programming-languages-set-nineteen
ms.openlocfilehash: 311c85e254711a219ac93424b77f35c2662008b7
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83658452"
---
# <a name="automatic-language-detection-for-speech-to-text"></a>Automatische Spracherkennung zur Umwandlung von Sprache in Text

Die automatische Spracherkennung wird verwendet, um aufgrund eines Vergleichs anhand einer Liste bereitgestellter Sprachen die wahrscheinlichste Übereinstimmung mit der an das Sprach-SDK übergebenen Audioeingabe zu ermitteln. Der von der automatischen Spracherkennung zurückgegebene Wert wird dann verwendet, um das Sprachmodell zur Umwandlung von Sprache in Text auszuwählen und Ihnen eine genauere Transkription zu bieten. Welche Sprachen verfügbar sind, erfahren Sie unter [Sprachunterstützung](language-support.md).

In diesem Artikel erfahren Sie, wie Sie `AutoDetectSourceLanguageConfig` zum Erstellen eines `SpeechRecognizer`-Objekts und zum Abrufen der erkannten Sprache verwenden.

> [!IMPORTANT]
> Dieses Feature ist nur für die Speech SDKs für C#, C++, Java, Python und Objective-C verfügbar.

## <a name="automatic-language-detection-with-the-speech-sdk"></a>Automatische Spracherkennung mit dem Sprach-SDK

Die automatische Spracherkennung weist zurzeit eine dienstseitige Einschränkung auf zwei Sprachen pro Erkennung auf. Beachten Sie diese Einschränkung bei der Erstellung des `AudoDetectSourceLanguageConfig`-Objekts. In den folgenden Beispielen erstellen Sie eine `AutoDetectSourceLanguageConfig` und verwenden Sie dann, um einen `SpeechRecognizer` zu erstellen.

> [!TIP]
> Sie können auch ein benutzerdefiniertes Modell angeben, das beim Durchführen der Spracherkennung verwendet werden soll. Weitere Informationen finden Sie unter [Verwenden eines benutzerdefinierten Modells für die automatische Spracherkennung](#use-a-custom-model-for-automatic-language-detection).

Die folgenden Codeausschnitte veranschaulichen die Verwendung der automatischen Spracherkennung in ihren Apps:

::: zone pivot="programming-language-csharp"

```csharp
var autoDetectSourceLanguageConfig =
    AutoDetectSourceLanguageConfig.FromLanguages(
        new string[] { "en-US", "de-DE" });

using (var recognizer = new SpeechRecognizer(
    speechConfig,
    autoDetectSourceLanguageConfig,
    audioConfig))
{
    var speechRecognitionResult = await recognizer.RecognizeOnceAsync();
    var autoDetectSourceLanguageResult =
        AutoDetectSourceLanguageResult.FromResult(speechRecognitionResult);
    var detectedLanguage = autoDetectSourceLanguageResult.Language;
}
```

::: zone-end

::: zone pivot="programming-language-cpp"

```cpp
auto autoDetectSourceLanguageConfig =
    AutoDetectSourceLanguageConfig::FromLanguages({ "en-US", "de-DE" });

auto recognizer = SpeechRecognizer::FromConfig(
    speechConfig,
    autoDetectSourceLanguageConfig,
    audioConfig);

speechRecognitionResult = recognizer->RecognizeOnceAsync().get();
auto autoDetectSourceLanguageResult =
    AutoDetectSourceLanguageResult::FromResult(speechRecognitionResult);
auto detectedLanguage = autoDetectSourceLanguageResult->Language;
```

::: zone-end

::: zone pivot="programming-language-java"

```java
AutoDetectSourceLanguageConfig autoDetectSourceLanguageConfig =
    AutoDetectSourceLanguageConfig.fromLanguages(Arrays.asList("en-US", "de-DE"));

SpeechRecognizer recognizer = new SpeechRecognizer(
    speechConfig,
    autoDetectSourceLanguageConfig,
    audioConfig);

Future<SpeechRecognitionResult> future = recognizer.recognizeOnceAsync();
SpeechRecognitionResult result = future.get(30, TimeUnit.SECONDS);
AutoDetectSourceLanguageResult autoDetectSourceLanguageResult =
    AutoDetectSourceLanguageResult.fromResult(result);
String detectedLanguage = autoDetectSourceLanguageResult.getLanguage();

recognizer.close();
speechConfig.close();
autoDetectSourceLanguageConfig.close();
audioConfig.close();
result.close();
```

::: zone-end

::: zone pivot="programming-language-python"

```Python
auto_detect_source_language_config = \
        speechsdk.languageconfig.AutoDetectSourceLanguageConfig(languages=["en-US", "de-DE"])
speech_recognizer = speechsdk.SpeechRecognizer(
        speech_config=speech_config, 
        auto_detect_source_language_config=auto_detect_source_language_config, 
        audio_config=audio_config)
result = speech_recognizer.recognize_once()
auto_detect_source_language_result = speechsdk.AutoDetectSourceLanguageResult(result)
detected_language = auto_detect_source_language_result.language
```

::: zone-end

::: zone pivot="programming-language-objectivec"

```Objective-C
NSArray *languages = @[@"zh-CN", @"de-DE"];
SPXAutoDetectSourceLanguageConfiguration* autoDetectSourceLanguageConfig = \
        [[SPXAutoDetectSourceLanguageConfiguration alloc]init:languages];
SPXSpeechRecognizer* speechRecognizer = \
        [[SPXSpeechRecognizer alloc] initWithSpeechConfiguration:speechConfig
                           autoDetectSourceLanguageConfiguration:autoDetectSourceLanguageConfig
                                              audioConfiguration:audioConfig];
SPXSpeechRecognitionResult *result = [speechRecognizer recognizeOnce];
SPXAutoDetectSourceLanguageResult *languageDetectionResult = [[SPXAutoDetectSourceLanguageResult alloc] init:result];
NSString *detectedLanguage = [languageDetectionResult language];
```

::: zone-end

## <a name="use-a-custom-model-for-automatic-language-detection"></a>Verwenden eines benutzerdefinierten Modells für die automatische Spracherkennung

Zur Ergänzung der Spracherkennung mit Spracherkennungsdienst-Modellen können Sie ein benutzerdefiniertes Modell zur verbesserten Erkennung angeben. Wenn kein benutzerdefiniertes Modell bereitgestellt wird, verwendet der Dienst das Standardsprachmodell.

Die folgenden Codeausschnitte veranschaulichen, wie Sie beim Aufruf des Spracherkennungsdiensts ein benutzerdefiniertes Modell angeben. Wenn die erkannte Sprache `en-US` ist, wird das Standardmodell verwendet. Wenn die erkannte Sprache `fr-FR` ist, wird der Endpunkt für das benutzerdefinierte Modell verwendet:

::: zone pivot="programming-language-csharp"

```csharp
var sourceLanguageConfigs = new SourceLanguageConfig[]
{
    SourceLanguageConfig.FromLanguage("en-US"),
    SourceLanguageConfig.FromLanguage("fr-FR", "The Endpoint Id for custom model of fr-FR")
};
var autoDetectSourceLanguageConfig =
    AutoDetectSourceLanguageConfig.FromSourceLanguageConfigs(
        sourceLanguageConfigs);
```

::: zone-end

::: zone pivot="programming-language-cpp"

```cpp
std::vector<std::shared_ptr<SourceLanguageConfig>> sourceLanguageConfigs;
sourceLanguageConfigs.push_back(
    SourceLanguageConfig::FromLanguage("en-US"));
sourceLanguageConfigs.push_back(
    SourceLanguageConfig::FromLanguage("fr-FR", "The Endpoint Id for custom model of fr-FR"));

auto autoDetectSourceLanguageConfig =
    AutoDetectSourceLanguageConfig::FromSourceLanguageConfigs(
        sourceLanguageConfigs);
```

::: zone-end

::: zone pivot="programming-language-java"

```java
List sourceLanguageConfigs = new ArrayList<SourceLanguageConfig>();
sourceLanguageConfigs.add(
    SourceLanguageConfig.fromLanguage("en-US"));
sourceLanguageConfigs.add(
    SourceLanguageConfig.fromLanguage("fr-FR", "The Endpoint Id for custom model of fr-FR"));

AutoDetectSourceLanguageConfig autoDetectSourceLanguageConfig =
    AutoDetectSourceLanguageConfig.fromSourceLanguageConfigs(
        sourceLanguageConfigs);
```

::: zone-end

::: zone pivot="programming-language-python"

```Python
 en_language_config = speechsdk.languageconfig.SourceLanguageConfig("en-US")
 fr_language_config = speechsdk.languageconfig.SourceLanguageConfig("fr-FR", "The Endpoint Id for custom model of fr-FR")
 auto_detect_source_language_config = speechsdk.languageconfig.AutoDetectSourceLanguageConfig(
        sourceLanguageConfigs=[en_language_config, fr_language_config])
```

::: zone-end

::: zone pivot="programming-language-objectivec"

```Objective-C
SPXSourceLanguageConfiguration* enLanguageConfig = [[SPXSourceLanguageConfiguration alloc]init:@"en-US"];
SPXSourceLanguageConfiguration* frLanguageConfig = \
        [[SPXSourceLanguageConfiguration alloc]initWithLanguage:@"fr-FR"
                                                     endpointId:@"The Endpoint Id for custom model of fr-FR"];
NSArray *languageConfigs = @[enLanguageConfig, frLanguageConfig];
SPXAutoDetectSourceLanguageConfiguration* autoDetectSourceLanguageConfig = \
        [[SPXAutoDetectSourceLanguageConfiguration alloc]initWithSourceLanguageConfigurations:languageConfigs];
```

::: zone-end

## <a name="next-steps"></a>Nächste Schritte

- [Speech SDK-Referenzdokumentation](speech-sdk.md)