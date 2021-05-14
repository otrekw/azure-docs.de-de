---
title: Abfrage eines Containerendpunkts für die Sprachsynthese
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 08/31/2020
ms.author: aahi
ms.openlocfilehash: 4571acfc46d9b336be23eaa3287dce5696c707b2
ms.sourcegitcommit: bd1a4e4df613ff24e954eb3876aebff533b317ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2021
ms.locfileid: "107930214"
---
Der Container stellt [REST-basierte Endpunkt-APIs](../rest-text-to-speech.md) bereit. Es gibt viele [Beispielquellcodeprojekte](https://github.com/Azure-Samples/Cognitive-Speech-TTS) für Plattform-, Framework- und Sprachvarianten.

Mit den Containern für die Standard- oder Neuronalsprachsynthese sollten Sie sich auf das Gebietsschema und die Sprache des heruntergeladenen Imagetags verlassen. Wenn Sie z. B. das `latest`-Tag heruntergeladen haben, ist das Standardgebietsschema `en-US` und `AriaNeural` die Stimme. Das `{VOICE_NAME}`-Argument wäre dann [`en-US-AriaNeural`](../language-support.md#neural-voices). Betrachten Sie das folgende SSML-Beispiel:

```xml
<speak version="1.0" xml:lang="en-US">
    <voice name="en-US-AriaNeural">
        This text will get converted into synthesized speech.
    </voice>
</speak>
```
