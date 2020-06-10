---
title: 'Grundlagen der Sprachsynthese: Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie Sie das Speech SDK verwenden, um Text in Sprache zu konvertieren. Dieser Artikel enthält Informationen zur Objektkonstruktion, zu unterstützten Audioausgabeformaten und zu Konfigurationsoptionen für die Sprachsynthese.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 04/14/2020
ms.author: trbye
ms.custom: tracking-python
zone_pivot_groups: programming-languages-set-two-with-js
ms.openlocfilehash: 2bba1536c9d0990eb771f62d27b852ae1eb3b45c
ms.sourcegitcommit: 1de57529ab349341447d77a0717f6ced5335074e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84604614"
---
# <a name="learn-the-basics-of-speech-synthesis"></a>Grundlegendes zur Sprachsynthese

In diesem Artikel werden gängige Entwurfsmuster für die Sprachsynthese mithilfe des Speech SDK vermittelt. Hierzu werden zunächst eine grundlegende Konfiguration und eine einfache Synthese durchgeführt, gefolgt von komplexeren Beispielen für die Entwicklung benutzerdefinierter Anwendungen:

* Erhalten von Antworten als In-Memory-Datenstrom
* Anpassen der Abtast- und Bitrate der Ausgabe
* Übermitteln von Syntheseanforderungen mithilfe von SSML (Speech Synthesis Markup Language, Markupsprache für Sprachsynthese)
* Verwenden neuronaler Stimmen

> [!TIP]
> Sollten Sie noch keine unserer Schnellstartanleitungen absolviert haben, empfehlen wir Ihnen, sich selbst ein Bild von der Spracherkennung zu machen.
> * [Schnellstart: Erkennen von Spracheingaben per Mikrofon](quickstarts/text-to-speech.md)

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

::: zone pivot="programming-language-more"
[!INCLUDE [More languages include](./includes/how-to/speech-to-text-basics/more.md)]
::: zone-end

## <a name="next-steps"></a>Nächste Schritte

* [Erste Schritte mit Custom Voice](how-to-custom-voice.md)
* [Verbessern der Synthese mit Markupsprache für Sprachsynthese (Speech Synthesis Markup Language, SSML)](speech-synthesis-markup.md)
