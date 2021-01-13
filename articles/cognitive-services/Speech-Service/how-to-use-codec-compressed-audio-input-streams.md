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
ms.openlocfilehash: 410c0942b9040a6707a51e4ff9f375b9d4728668
ms.sourcegitcommit: 28c93f364c51774e8fbde9afb5aa62f1299e649e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/30/2020
ms.locfileid: "97821569"
---
# <a name="use-codec-compressed-audio-input-with-the-speech-sdk"></a>Verwenden von per Codec komprimierter Audioeingabe mit dem Speech SDK

Die Speech Service SDK-API für **komprimierte Audioeingabestreams** ermöglicht das Streamen komprimierter Audiodaten an den Speech-Dienst mittels `PullStream` oder `PushStream`.

Plattform | Sprachen | Unterstützte GStreamer-Version
| :--- | ---: | :---:
Windows (ohne universelle Windows-Plattform)  | C++, C#, Java, Python | [1.15.1](https://gstreamer.freedesktop.org/releases/gstreamer/1.5.1.html)
Linux  | C++, C#, Java, Python | [Unterstützte Linux-Distributionen und Zielarchitekturen](~/articles/cognitive-services/speech-service/speech-sdk.md)
Android  | Java | [1.14.4](https://gstreamer.freedesktop.org/data/pkg/android/1.14.4/)

## <a name="speech-sdk-version-required-for-compressed-audio-input"></a>Erforderliche Speech SDK-Version für komprimierte Audioeingaben
* Das Speech SDK, Version 1.10.0 oder höher ist für RHEL 8 und CentOS 8 erforderlich.
* Das Speech SDK, Version 1.11.0 oder höher ist für Windows erforderlich.

[!INCLUDE [supported-audio-formats](includes/supported-audio-formats.md)]

## <a name="gstreamer-required-to-handle-compressed-audio"></a>GStreamer ist zum Verarbeiten komprimierter Audiodaten erforderlich.

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
> [Schnellstart: Erkennen von Spracheingaben per Mikrofon](./get-started-speech-to-text.md)