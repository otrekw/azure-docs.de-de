---
title: Abrufen von Gesichtsausdrucksereignissen für die Lippensynchronisierung
titleSuffix: Azure Cognitive Services
description: Das Speech SDK unterstützt Visem-Ereignisse während der Sprachsynthese. Diese stellen wichtige Gesichtsausdrücke beim Sprechen dar, z. B. die Position der Lippen, des Kiefers und der Zunge beim Erzeugen eines bestimmten Phonems.
services: cognitive-services
author: yulin-li
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/03/2021
ms.author: yulili
ms.custom: references_regions
zone_pivot_groups: programming-languages-speech-services-nomore-variant
ms.openlocfilehash: a7808b3b5db9d01d6a1a2cd352a912d44a4e7908
ms.sourcegitcommit: b4032c9266effb0bf7eb87379f011c36d7340c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2021
ms.locfileid: "107904494"
---
# <a name="get-facial-pose-events"></a>Abrufen von Gesichtsausdrucksereignissen

> [!NOTE]
> Visem-Ereignisse sind derzeit nur für die Stimme `en-US-AriaNeural` verfügbar.

Ein _Visem_ ist die visuelle Beschreibung eines Phonems in der gesprochenen Sprache.
Es beschreibt die Position des Gesichts und Munds beim Sprechen eines Worts.
Jedes Visem stellt die wichtigsten Gesichtsausdrücke für einen bestimmten Satz von Phonemen dar.
Es besteht keine 1:1-Entsprechung zwischen Visemen und Phonemen.
Oftmals entsprechen mehrere Phoneme einem einzelnen Visem, da mehrere Phoneme, z. B. `s` und `z`, mit dem gleichen Mundbild einhergehen.
Weitere Informationen finden Sie in der [Tabelle der Zuordnungen zwischen Visemen und Phonemen](#map-phonemes-to-visemes).

Mit Visemen können Sie natürlicher wirkende und intelligentere Nachrichtensprecher, interaktivere Figuren in Spielen und Cartoons sowie intuitivere Sprachunterrichtsvideos erstellen. Hörgeschädigte können Töne auch visuell wahrnehmen und Sprachinhalte aus den Visemen in einem animierten Gesicht ableiten.

## <a name="get-viseme-events-with-the-speech-sdk"></a>Abrufen von Visemereignissen mit dem Speech SDK

Zum Erstellen von Visemereignissen konvertieren wir den Eingabetext in einen Satz von Phonemsequenzen und den entsprechenden Visemsequenzen. Wir schätzen die Startzeit jedes Visems in der Sprachausgabe. Visemereignisse enthalten eine Sequenz von Visem-IDs mit jeweils einem Offset zu der Sprachausgabe, in der das Visem vorhanden ist. Diese Ereignisse können Mundanimationen steuern, die eine Person simulieren, die den Eingabetext spricht.

| Parameter | BESCHREIBUNG |
|-----------|-------------|
| VisemeId | Eine ganze Zahl, die ein Visem angibt. In der Sprache „Englisch (USA)“ werden 22 verschiedene Viseme geboten, um die Mundformen für einen bestimmten Satz von Phonemen darzustellen. Weitere Informationen finden Sie in der [Tabelle der Zuordnungen zwischen Visem-IDs und Phonemen](#map-phonemes-to-visemes).  |
| AudioOffset | Die Startzeit jedes Visems in Takten (100 Nanosekunden) |

Abonnieren Sie das `VisemeReceived`-Ereignis im Speech SDK, um Visemereignisse abzurufen.
In den folgenden Codeausschnitten wird gezeigt, wie Sie das Visemereignis abonnieren.

::: zone pivot="programming-language-csharp"

```csharp
using (var synthesizer = new SpeechSynthesizer(speechConfig, audioConfig))
{
    // Subscribes to viseme received event
    synthesizer.VisemeReceived += (s, e) =>
    {
        Console.WriteLine($"Viseme event received. Audio offset: " +
            $"{e.AudioOffset / 10000}ms, viseme id: {e.VisemeId}.");
    };

    var result = await synthesizer.SpeakSsmlAsync(ssml));
}

```

::: zone-end

::: zone pivot="programming-language-cpp"

```cpp
auto synthesizer = SpeechSynthesizer::FromConfig(speechConfig, audioConfig);

// Subscribes to viseme received event
synthesizer->VisemeReceived += [](const SpeechSynthesisVisemeEventArgs& e)
{
    cout << "viseme event received. "
        // The unit of e.AudioOffset is tick (1 tick = 100 nanoseconds), divide by 10,000 to convert to milliseconds.
        << "Audio offset: " << e.AudioOffset / 10000 << "ms, "
        << "viseme id: " << e.VisemeId << "." << endl;
};

auto result = synthesizer->SpeakSsmlAsync(ssml).get();
```

::: zone-end

::: zone pivot="programming-language-java"

```java
SpeechSynthesizer synthesizer = new SpeechSynthesizer(speechConfig, audioConfig);

// Subscribes to viseme received event
synthesizer.VisemeReceived.addEventListener((o, e) -> {
    // The unit of e.AudioOffset is tick (1 tick = 100 nanoseconds), divide by 10,000 to convert to milliseconds.
    System.out.print("Viseme event received. Audio offset: " + e.getAudioOffset() / 10000 + "ms, ");
    System.out.println("viseme id: " + e.getVisemeId() + ".");
});

SpeechSynthesisResult result = synthesizer.SpeakSsmlAsync(ssml).get();
```

::: zone-end

::: zone pivot="programming-language-python"

```Python
speech_synthesizer = speechsdk.SpeechSynthesizer(speech_config=speech_config, audio_config=audio_config)

# Subscribes to viseme received event
speech_synthesizer.viseme_received.connect(lambda evt: print(
    "Viseme event received: audio offset: {}ms, viseme id: {}.".format(evt.audio_offset / 10000, evt.viseme_id)))

result = speech_synthesizer.speak_ssml_async(ssml).get()
```

::: zone-end

::: zone pivot="programming-language-javascript"

```Javascript
var synthesizer = new SpeechSDK.SpeechSynthesizer(speechConfig, audioConfig);

// Subscribes to viseme received event
synthesizer.visemeReceived = function (s, e) {
    window.console.log("(Viseme), Audio offset: " + e.audioOffset / 10000 + "ms. Viseme ID: " + e.visemeId);
}

synthesizer.speakSsmlAsync(ssml);
```

::: zone-end

::: zone pivot="programming-language-objectivec"

```Objective-C
SPXSpeechSynthesizer *synthesizer =
    [[SPXSpeechSynthesizer alloc] initWithSpeechConfiguration:speechConfig
                                           audioConfiguration:audioConfig];

// Subscribes to viseme received event
[synthesizer addVisemeReceivedEventHandler: ^ (SPXSpeechSynthesizer *synthesizer, SPXSpeechSynthesisVisemeEventArgs *eventArgs) {
    NSLog(@"Viseme event received. Audio offset: %fms, viseme id: %lu.", eventArgs.audioOffset/10000., eventArgs.visemeId);
}];

[synthesizer speakSsml:ssml];
```

::: zone-end

## <a name="map-phonemes-to-visemes"></a>Zuordnen von Phonemen zu Visemen

Viseme variieren je nach Sprache. Jede Sprache verfügt über einen Satz von Visemen, die den jeweiligen Phonemen entsprechen. In der folgenden Tabelle wird die Entsprechung zwischen Visemen nach dem internationalen phonetischen Alphabet (IPA) und Visem-IDs für „Englisch (USA)“ gezeigt.

| IPA | Beispiel | Visem-ID |
|-----|---------|-----------|
| i   | **ea** t   | 6 |
| ɪ   | **i** f | 6 |
| eɪ  | **a** te | 4 |
| ɛ | **e** very | 4 |
|æ  |   **a** ctive        |1|
|ɑ  |   **o** bstinate     |2|
|ɔ  |   c **au** se         |3|
|ʊ  |   b **oo** k          |4|
|oʊ |   **o** ld           |8|
|u  |   **U** ber          |7|
|ʌ  |   **u** ncle         |1|
|aɪ |   **i** ce           |11|
|aʊ |   **ou** t           |9|
|ɔɪ |   **oi** l           |10|
|ju |   **Yu** ma          |[6, 7]|
|ə  |   **a** go           |1|
|ɪɹ |   **ear** s          |[6, 13]|
|ɛɹ |   **air** plane      |[4, 13]|
|ʊɹ |   c **ur** e          |[4, 13]|
|aɪ(ə)ɹ |   **Ire** land   |[11, 13]|
|aʊ(ə)ɹ |   **hour** s     |[9, 13]|
|ɔɹ |   **or** ange        |[3, 13]|
|ɑɹ |   **ar** tist        |[2, 13]|
|ɝ  |   **ear** th         |[5, 13]|
|ɚ  |   all **er** gy       |[1, 13]|
|w  |   **w** ith, s **ue** de   |7|
|j  |   **y** ard, f **e** w     |6|
|p  |   **p** ut           |21|
|b  |   **b** ig           |21|
|t  |   **t** alk          |19|
|T  |   **d** ig           |19|
|k  |   **c** ut           |20|
|g  |   **g** o            |20|
|m  |   **m** at, s **m** ash    |21|
|n  |   **n** o, s **n** ow      |19|
|ŋ  |   li **n** k          |20|
|f  |   **f** ork          |18|
|v  |   **v** alue         |18|
|θ  |   **th** in          |17|
|ð  |   **th** en          |17|
|s  |   **s** it           |15|
|z  |   **z** ap           |15|
|ʃ  |   **sh** e           |16|
|ʒ  |   **J** acques       |16|
|h  |   **h** elp          |12|
|tʃ |   **ch** in          |16|
|dʒ |   **j** oy           |16|
|l  |   **l** id, g **l** ad     |14|
|ɹ  |   **r** ed, b **r** ing    |13|


## <a name="next-steps"></a>Nächste Schritte

* [Speech SDK-Referenzdokumentation](speech-sdk.md)
