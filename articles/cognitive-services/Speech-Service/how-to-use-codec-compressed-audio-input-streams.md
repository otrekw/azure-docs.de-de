---
title: Streamen von per Codec komprimierten Audiodaten mit dem Speech SDK – Speech-Dienst
titleSuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie Sie mit dem Speech SDK komprimierte Audiodaten an den Speech-Dienst streamen. Verfügbar für C++, C# und Java für Linux, Java in Android und Objective-C in iOS.
services: cognitive-services
author: amitkumarshukla
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: amishu
ms.custom: devx-track-csharp
zone_pivot_groups: programming-languages-set-twenty-two
ms.openlocfilehash: 7d6a0388507d836050550a3a98255deabb826cde
ms.sourcegitcommit: 857859267e0820d0c555f5438dc415fc861d9a6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93128323"
---
# <a name="use-codec-compressed-audio-input-with-the-speech-sdk"></a>Verwenden von per Codec komprimierter Audioeingabe mit dem Speech SDK

Die Speech Service SDK-API für **komprimierte Audioeingabestreams** ermöglicht das Streamen komprimierter Audiodaten an den Speech-Dienst mittels `PullStream` oder `PushStream`.

Plattform | Sprachen | Unterstützte GStreamer-Version
| :--- | ---: | :---:
Windows (ohne universelle Windows-Plattform)  | C++, C#, Java, Python | [1.15.1](https://gstreamer.freedesktop.org/data/pkg/windows/1.15.1/)
Linux  | C++, C#, Java, Python | [Unterstützte Linux-Distributionen und Zielarchitekturen](~/articles/cognitive-services/speech-service/speech-sdk.md)
Android  | Java | [1.14.4](https://gstreamer.freedesktop.org/data/pkg/android/1.14.4/)

## <a name="speech-sdk-version-required-for-compressed-audio-input"></a>Erforderliche Speech SDK-Version für komprimierte Audioeingaben
* Das Speech SDK, Version 1.10.0 oder höher ist für RHEL 8 und CentOS 8 erforderlich.
* Das Speech SDK, Version 1.11.0 oder höher ist für Windows erforderlich.

[!INCLUDE [supported-audio-formats](includes/supported-audio-formats.md)]

## <a name="prerequisites"></a>Voraussetzungen

::: zone pivot="programming-language-csharp"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/csharp/prerequisites.md)]
::: zone-end

::: zone pivot="programming-language-cpp"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/cpp/prerequisites.md)]
::: zone-end

::: zone pivot="programming-language-java"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/java/prerequisites.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/python/prerequisites.md)]
::: zone-end

## <a name="example-code-using-codec-compressed-audio-input"></a>Beispielcode für die Verwendung von per Codec komprimierter Audioeingabe

::: zone pivot="programming-language-csharp"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/csharp/examples.md)]
::: zone-end

::: zone pivot="programming-language-cpp"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/cpp/examples.md)]
::: zone-end

::: zone pivot="programming-language-java"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/java/examples.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/python/examples.md)]
::: zone-end

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Schnellstart: Erkennen von Spracheingaben per Mikrofon](quickstarts/speech-to-text-from-microphone.md)
