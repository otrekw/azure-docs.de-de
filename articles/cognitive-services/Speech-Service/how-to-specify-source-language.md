---
title: Angeben der Quellsprache für die Spracherkennung
titleSuffix: Azure Cognitive Services
description: Mit dem Sprach-SDK können Sie bei der Umwandlung von Sprache in Text die Quellsprache angeben. In diesem Artikel wird beschrieben, wie Sie die Methoden FromConfig und SourceLanguageConfig verwenden, damit der Spracherkennungsdienst die Quellsprache erkennen und ein benutzerdefiniertes Modellziel bereitstellen kann.
services: cognitive-services
author: susanhu
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 01/07/2020
ms.author: qiohu
zone_pivot_groups: programming-languages-set-two
ms.openlocfilehash: e4f4dd3c1e23855a8a1a69dac72c232779206f1d
ms.sourcegitcommit: 5bbe87cf121bf99184cc9840c7a07385f0d128ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2020
ms.locfileid: "76121708"
---
# <a name="specify-source-language-for-speech-to-text"></a>Angeben der Quellsprache für die Spracherkennung

In diesem Artikel erfahren Sie, wie Sie die Quellsprache für eine Audioeingabe angeben, die zur Spracherkennung an das Sprach-SDK weitergeleitet wird. Außerdem wird ein Beispielcode bereitgestellt, um ein benutzerdefiniertes Sprachmodell zur verbesserten Erkennung anzugeben.

::: zone pivot="programming-language-csharp"

## <a name="how-to-specify-source-language-in-c"></a>Vorgehensweise: Angeben der Quellsprache in C#

In diesem Beispiel wird die Ausgangssprache mithilfe eines `SpeechRecognizer`-Konstrukts explizit als Parameter bereitgestellt.

```csharp
var recognizer = new SpeechRecognizer(speechConfig, "de-DE", audioConfig);
```

In diesem Beispiel wird die Quellsprache mithilfe von `SourceLanguageConfig` bereitgestellt. `sourceLanguageConfig` wird dann als Parameter an das `SpeechRecognizer`-Konstrukt übergeben.

```csharp
var sourceLanguageConfig = SourceLanguageConfig.FromLanguage("de-DE");
var recognizer = new SpeechRecognizer(speechConfig, sourceLanguageConfig, audioConfig);
```

In diesem Beispiel werden die Quellsprache und der benutzerdefinierte Endpunkt mithilfe von `SourceLanguageConfig` bereitgestellt. `sourceLanguageConfig` wird dann als Parameter an das `SpeechRecognizer`-Konstrukt übergeben.

```csharp
var sourceLanguageConfig = SourceLanguageConfig.FromLanguage("de-DE", "The Endpoint ID for your custom model.");
var recognizer = new SpeechRecognizer(speechConfig, sourceLanguageConfig, audioConfig);
```

>[!Note]
> Die set-Methoden `SpeechRecognitionLanguage` und `EndpointId` sind veraltete Methoden aus der `SpeechConfig`-Klasse in C#. Die Verwendung dieser Methoden wird nicht empfohlen und sollte beim Erstellen eines `SpeechRecognizer` vermieden werden.

::: zone-end

::: zone pivot="programming-language-cpp"


## <a name="how-to-specify-source-language-in-c"></a>Vorgehensweise: Angeben der Quellsprache in C++

In diesem Beispiel wird die Quellsprache explizit mithilfe der `FromConfig`-Methode bereitgestellt.

```C++
auto recognizer = SpeechRecognizer::FromConfig(speechConfig, "de-DE", audioConfig);
```

In diesem Beispiel wird die Quellsprache mithilfe von `SourceLanguageConfig` bereitgestellt. `sourceLanguageConfig` wird dann als Parameter an `FromConfig` übergeben, wenn der `recognizer` erstellt wird.

```C++
auto sourceLanguageConfig = SourceLanguageConfig::FromLanguage("de-DE");
auto recognizer = SpeechRecognizer::FromConfig(speechConfig, sourceLanguageConfig, audioConfig);
```

In diesem Beispiel werden die Quellsprache und der benutzerdefinierte Endpunkt mithilfe von `SourceLanguageConfig` bereitgestellt. `sourceLanguageConfig` wird als Parameter an `FromConfig` übergeben, wenn der `recognizer` erstellt wird.

```C++
auto sourceLanguageConfig = SourceLanguageConfig::FromLanguage("de-DE", "The Endpoint ID for your custom model.");
auto recognizer = SpeechRecognizer::FromConfig(speechConfig, sourceLanguageConfig, audioConfig);
```

>[!Note]
> `SetSpeechRecognitionLanguage` und `SetEndpointId` sind veraltete Methoden aus der `SpeechConfig`-Klasse in C++ und Java. Die Verwendung dieser Methoden wird nicht empfohlen und sollte beim Erstellen eines `SpeechRecognizer` vermieden werden.

::: zone-end

::: zone pivot="programming-language-java"

## <a name="how-to-specify-source-language-in-java"></a>Vorgehensweise: Angeben der Quellsprache in Java

In diesem Beispiel wird die Quellsprache beim Erstellen einer neuen `SpeechRecognizer`-Methode explizit bereitgestellt.

```Java
SpeechRecognizer recognizer = new SpeechRecognizer(speechConfig, "de-DE", audioConfig);
```

In diesem Beispiel wird die Quellsprache mithilfe von `SourceLanguageConfig` bereitgestellt. `sourceLanguageConfig` wird dann als Parameterübergeben, wenn ein neuer `SpeechRecognizer` erstellt wird.

```Java
SourceLanguageConfig sourceLanguageConfig = SourceLanguageConfig.fromLanguage("de-DE");
SpeechRecognizer recognizer = new SpeechRecognizer(speechConfig, sourceLanguageConfig, audioConfig);
```

In diesem Beispiel werden die Quellsprache und der benutzerdefinierte Endpunkt mithilfe von `SourceLanguageConfig` bereitgestellt. `sourceLanguageConfig` wird dann als Parameterübergeben, wenn ein neuer `SpeechRecognizer` erstellt wird.

```Java
SourceLanguageConfig sourceLanguageConfig = SourceLanguageConfig.fromLanguage("de-DE", "The Endpoint ID for your custom model.");
SpeechRecognizer recognizer = new SpeechRecognizer(speechConfig, sourceLanguageConfig, audioConfig);
```

>[!Note]
> `setSpeechRecognitionLanguage` und `setEndpointId` sind veraltete Methoden aus der `SpeechConfig`-Klasse in C++ und Java. Die Verwendung dieser Methoden wird nicht empfohlen und sollte beim Erstellen eines `SpeechRecognizer` vermieden werden.

::: zone-end

::: zone pivot="programming-language-python"

## <a name="how-to-specify-source-language-in-python"></a>Vorgehensweise: Angeben der Quellsprache in Python

Im ersten Schritt wird ein `speech_config` erstellt:

```Python
speech_key, service_region = "YourSubscriptionKey", "YourServiceRegion"
speech_config = speechsdk.SpeechConfig(subscription=speech_key, region=service_region)
```

Geben Sie als nächstes die Quellsprache Ihrer Audiodatei mit `speech_recognition_language` an:

```Python
speech_config.speech_recognition_language="de-DE"
```

Wenn Sie ein benutzerdefiniertes Modell zur Erkennung verwenden, können Sie den Endpunkt mit `endpoint_id` angeben:

```Python
speech_config.endpoint_id = "The Endpoint ID for your custom model."
```

::: zone-end

::: zone pivot="programming-language-more"

## <a name="how-to-specify-source-language-in-javascript"></a>Vorgehensweise: Angeben der Quellsprache in JavaScript

Im ersten Schritt wird ein `SpeechConfig` erstellt:

```Javascript
var speechConfig = sdk.SpeechConfig.fromSubscription("YourSubscriptionkey", "YourRegion");
```

Geben Sie als nächstes die Quellsprache Ihrer Audiodatei mit `speechRecognitionLanguage` an:

```Javascript
speechConfig.speechRecognitionLanguage = "de-DE";
```

Wenn Sie ein benutzerdefiniertes Modell zur Erkennung verwenden, können Sie den Endpunkt mit `endpointId` angeben:

```Javascript
speechConfig.endpointId = "The Endpoint ID for your custom model.";
```

## <a name="how-to-specify-source-language-in-objective-c"></a>Vorgehensweise: Angeben der Quellsprache in Objective-C

Im ersten Schritt wird ein `speechConfig` erstellt:

```Objective-C
SPXSpeechConfiguration *speechConfig = [[SPXSpeechConfiguration alloc] initWithSubscription:@"YourSubscriptionkey" region:@"YourRegion"];
```

Geben Sie als nächstes die Quellsprache Ihrer Audiodatei mit `speechRecognitionLanguage` an:

```Objective-C
speechConfig.speechRecognitionLanguage = @"de-DE";
```

Wenn Sie ein benutzerdefiniertes Modell zur Erkennung verwenden, können Sie den Endpunkt mit `endpointId` angeben:

```Objective-C
speechConfig.endpointId = @"The Endpoint ID for your custom model.";
```

::: zone-end

## <a name="see-also"></a>Weitere Informationen

* Eine Liste der unterstützten Sprachen und Gebietsschemas für die Spracherkennung finden Sie unter [Sprachunterstützung](language-support.md).

## <a name="next-steps"></a>Nächste Schritte

* [Speech SDK-Referenzdokumentation](speech-sdk.md)
