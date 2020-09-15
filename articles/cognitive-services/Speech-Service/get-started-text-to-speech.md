---
title: Schnellstart für die Sprachsynthese – Speech-Dienst
titleSuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie Sie das Speech SDK verwenden, um Text in Sprache zu konvertieren. In dieser Schnellstartanleitung werden die Objektkonstruktion und Entwurfsmuster, die unterstützten Formate für die Audioausgabe, die Speech-CLI und die benutzerdefinierten Konfigurationsoptionen für die Sprachsynthese beschrieben.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 09/01/2020
ms.author: trbye
ms.custom: devx-track-python, devx-track-javascript, devx-track-csharp, cog-serv-seo-aug-2020
zone_pivot_groups: speech-full-stack
keywords: Sprachsynthese
ms.openlocfilehash: 79409f95d698e015d15d9131dcf1f27b34b03343
ms.sourcegitcommit: 5a3b9f35d47355d026ee39d398c614ca4dae51c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89400903"
---
# <a name="get-started-with-text-to-speech"></a>Erste Schritte bei der Sprachsynthese

In dieser Schnellstartanleitung werden gängige Entwurfsmuster für die Sprachsynthese per Speech SDK vermittelt. Hierzu werden zunächst eine grundlegende Konfiguration und eine einfache Synthese durchgeführt, gefolgt von komplexeren Beispielen für die Entwicklung benutzerdefinierter Anwendungen:

* Erhalten von Antworten als In-Memory-Datenstrom
* Anpassen der Abtast- und Bitrate der Ausgabe
* Übermitteln von Syntheseanforderungen mithilfe von SSML (Speech Synthesis Markup Language, Markupsprache für Sprachsynthese)
* Verwenden neuronaler Stimmen

> [!TIP]
> Greifen Sie auf GitHub auf die [Schnellstartbeispiele](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart) zu, falls Sie direkt zum Beispielcode springen möchten.

::: zone pivot="programming-language-csharp"
[!INCLUDE [C# Basics include](includes/how-to/text-to-speech-basics/text-to-speech-basics-csharp.md)]
::: zone-end

::: zone pivot="programming-language-cpp"
[!INCLUDE [C++ Basics include](includes/how-to/text-to-speech-basics/text-to-speech-basics-cpp.md)]
::: zone-end

::: zone pivot="programming-language-java"
[!INCLUDE [Java Basics include](includes/how-to/text-to-speech-basics/text-to-speech-basics-java.md)]
::: zone-end

::: zone pivot="programming-language-javascript"
[!INCLUDE [JavaScript Basics include](includes/how-to/text-to-speech-basics/text-to-speech-basics-javascript.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [Python Basics include](includes/how-to/text-to-speech-basics/text-to-speech-basics-python.md)]
::: zone-end

::: zone pivot="programming-language-spx"
[!INCLUDE [CLI include](includes/how-to/text-to-speech-basics/text-to-speech-basics-cli.md)]
::: zone-end

## <a name="next-steps"></a>Nächste Schritte

* [Erste Schritte mit Custom Voice](how-to-custom-voice.md)
* [Verbessern der Synthese mit Markupsprache für Sprachsynthese (Speech Synthesis Markup Language, SSML)](speech-synthesis-markup.md)
* Informieren Sie sich darüber, wie Sie die [API für lange Audioinhalte](long-audio-api.md) für längere Texte, z. B. Bücher und Zeitungsartikel, verwenden.
* Sehen Sie sich die [Schnellstartbeispiele](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart) auf GitHub an.
