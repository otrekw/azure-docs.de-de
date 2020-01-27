---
title: Verwenden der automatischen Spracherkennung zur Umwandlung von Sprache in Text
titleSuffix: Azure Cognitive Services
description: Das Sprach-SDK unterstützt die automatische Spracherkennung für die Umwandlung von Sprache in Text. Mit diesem Feature wird die Audioeingabe mit einer bereitgestellten Liste von Sprachen verglichen und die wahrscheinlichste Übereinstimmung bestimmt. Der zurückgegebene Wert kann dann verwendet werden, um das Sprachmodell auszuwählen, das für die Spracherkennung verwendet wird.
services: cognitive-services
author: susanhu
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 01/15/2020
ms.author: qiohu
zone_pivot_groups: programming-languages-set-seven
ms.openlocfilehash: bc438c3e606fefc10e9ffbb64c79f7167d9af062
ms.sourcegitcommit: 5bbe87cf121bf99184cc9840c7a07385f0d128ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2020
ms.locfileid: "76122048"
---
# <a name="automatic-language-detection-for-speech-to-text"></a>Automatische Spracherkennung zur Umwandlung von Sprache in Text

Die automatische Spracherkennung wird verwendet, um aufgrund eines Vergleichs anhand einer Liste bereitgestellter Sprachen die wahrscheinlichste Übereinstimmung mit der an das Sprach-SDK übergebenen Audioeingabe zu ermitteln. Der von der automatischen Spracherkennung zurückgegebene Wert wird dann verwendet, um das Sprachmodell zur Umwandlung von Sprache in Text auszuwählen und Ihnen eine genauere Transkription zu bieten. Welche Sprachen verfügbar sind, erfahren Sie unter [Sprachunterstützung](language-support.md).

In diesem Artikel erfahren Sie, wie Sie `AutoDetectSourceLanguageConfig` zum Erstellen eines `SpeechRecognizer`-Objekts und zum Abrufen der erkannten Sprache verwenden.

> [!IMPORTANT]
> Dieses Feature ist nur für die Sprach-SDKs für C#, C++ und Java verfügbar.

## <a name="automatic-language-detection-with-the-speech-sdk"></a>Automatische Spracherkennung mit dem Sprach-SDK

Die automatische Spracherkennung weist zurzeit eine dienstseitige Einschränkung auf zwei Sprachen pro Erkennung auf. Beachten Sie diese Einschränkung bei der Erstellung des `AudoDetectSourceLanguageConfig`-Objekts. In den folgenden Beispielen erstellen Sie eine `AutoDetectSourceLanguageConfig` und verwenden Sie dann, um einen `SpeechRecognizer` zu erstellen.

> [!TIP]
> Sie können auch ein benutzerdefiniertes Modell angeben, das beim Durchführen der Spracherkennung verwendet werden soll. Weitere Informationen finden Sie unter [Verwenden eines benutzerdefinierten Modells für die automatische Spracherkennung](#use-a-custom-model-for-automatic-language-detection).

Die folgenden Codeausschnitte veranschaulichen die Verwendung der automatischen Spracherkennung in ihren Apps:

::: zone pivot="programming-language-csharp"

```csharp
var autoDetectSourceLanguageConfig = AutoDetectSourceLanguageConfig.FromLanguages(new string[] { "en-US", "de-DE" });
using (var recognizer = new SpeechRecognizer(speechConfig, autoDetectSourceLanguageConfig, audioConfig))
{
    var speechRecognitionResult = await recognizer.RecognizeOnceAsync();
    var autoDetectSourceLanguageResult = AutoDetectSourceLanguageResult.FromResult(speechRecognitionResult);
    var detectedLanguage = autoDetectSourceLanguageResult.Language;
}
```

::: zone-end

::: zone pivot="programming-language-cpp"

```C++
auto autoDetectSourceLanguageConfig = AutoDetectSourceLanguageConfig::FromLanguages({ "en-US", "de-DE" });
auto recognizer = SpeechRecognizer::FromConfig(speechConfig, autoDetectSourceLanguageConfig, audioConfig);
speechRecognitionResult = recognizer->RecognizeOnceAsync().get();
auto autoDetectSourceLanguageResult = AutoDetectSourceLanguageResult::FromResult(speechRecognitionResult);
auto detectedLanguage = autoDetectSourceLanguageResult->Language;
```

::: zone-end

::: zone pivot="programming-language-java"

```Java
AutoDetectSourceLanguageConfig autoDetectSourceLanguageConfig = AutoDetectSourceLanguageConfig.fromLanguages(Arrays.asList("en-US", "de-DE"));
SpeechRecognizer recognizer = new SpeechRecognizer(speechConfig, autoDetectSourceLanguageConfig, audioConfig);
Future<SpeechRecognitionResult> future = recognizer.recognizeOnceAsync();
SpeechRecognitionResult result = future.get(30, TimeUnit.SECONDS);
AutoDetectSourceLanguageResult autoDetectSourceLanguageResult = AutoDetectSourceLanguageResult.fromResult(result);
String detectedLanguage = autoDetectSourceLanguageResult.getLanguage();

recognizer.close();
speechConfig.close();
autoDetectSourceLanguageConfig.close();
audioConfig.close();
result.close();
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
var autoDetectSourceLanguageConfig = AutoDetectSourceLanguageConfig.FromSourceLanguageConfigs(sourceLanguageConfigs);
```

::: zone-end

::: zone pivot="programming-language-cpp"

```C++
std::vector<std::shared_ptr<SourceLanguageConfig>> sourceLanguageConfigs;
sourceLanguageConfigs.push_back(SourceLanguageConfig::FromLanguage("en-US"));
sourceLanguageConfigs.push_back(SourceLanguageConfig::FromLanguage("fr-FR", "The Endpoint Id for custom model of fr-FR"));
auto autoDetectSourceLanguageConfig = AutoDetectSourceLanguageConfig::FromSourceLanguageConfigs(sourceLanguageConfigs);
```

::: zone-end

::: zone pivot="programming-language-java"

```Java
List sourceLanguageConfigs = new ArrayList<SourceLanguageConfig>();
sourceLanguageConfigs.add(SourceLanguageConfig.fromLanguage("en-US"));
sourceLanguageConfigs.add(SourceLanguageConfig.fromLanguage("fr-FR", "The Endpoint Id for custom model of fr-FR"));
AutoDetectSourceLanguageConfig autoDetectSourceLanguageConfig = AutoDetectSourceLanguageConfig.fromSourceLanguageConfigs(sourceLanguageConfigs);
```

::: zone-end

## <a name="next-steps"></a>Nächste Schritte

- [Speech SDK-Referenzdokumentation](speech-sdk.md)
