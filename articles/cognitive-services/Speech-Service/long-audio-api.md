---
title: API für lange Audioinhalte (Vorschau) – Spracherkennungsdienst
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie die API für lange Audioinhalte für die asynchrone Synthese von langen Sprachsynthesen konzipiert ist.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 01/30/2020
ms.author: trbye
ms.openlocfilehash: b7cca314ec59e46cf17751b1aec28b5c3ea029ed
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81401073"
---
# <a name="long-audio-api-preview"></a>API für lange Audioinhalte (Vorschau)

Die API für lange Audioinhalte ist für die asynchrone Synthese von langen Sprachsynthesen (z. B. Hörbücher) konzipiert. Diese API gibt keine synthetisierten Audiodaten in Echtzeit zurück, sondern erwartet, dass Sie die Antworten abrufen und die Ausgaben so verwenden, wie sie vom Dienst bereitgestellt werden. Im Gegensatz zur Text-to-Speech-API, die vom Speech SDK verwendet wird, kann die API für lange Audioinhalte synthetisierte Audiodaten erzeugen, die länger als 10 Minuten sind, wodurch sie ideal für Herausgeber und Plattformen für Audioinhalte geeignet ist.

Weitere Vorteile der API für lange Audioinhalte:

* Die vom Dienst zurückgegebene synthetisierte Sprache verwendet neuronale Stimmen, was eine High-Fidelity-Audioausgabe sicherstellt.
* Da Echtzeitantworten nicht unterstützt werden, ist es nicht erforderlich, einen Sprachendpunkt bereitzustellen.

> [!NOTE]
> Die API für lange Audioinhalte unterstützt jetzt nur noch die [benutzerdefinierte neuronale Stimme](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-custom-voice#custom-neural-voices).

## <a name="workflow"></a>Workflow

Wenn Sie die API für lange Audioinhalte verwenden, senden Sie typischerweise eine Textdatei oder zu synthetisierende Dateien, fragen den Status ab und können die Audioausgabe herunterladen, wenn der Status „Erfolgreich“ ist.

Dieses Diagramm bietet eine allgemeine Übersicht über den Workflow.

![Diagramm zum Workflow der API für lange Audioinhalte](media/long-audio-api/long-audio-api-workflow.png)

## <a name="prepare-content-for-synthesis"></a>Vorbereiten der Inhalte für die Synthese

Stellen Sie bei der Vorbereitung Ihrer Textdatei Folgendes sicher:

* Es handelt sich entweder um Nur-Text (.txt) oder SSML-Text (.txt).
* Die Verschlüsselung ist [UTF-8 mit Bytereihenfolge-Marke (BOM)](https://www.w3.org/International/questions/qa-utf8-bom.en#bom).
* Es handelt sich um eine einzelne Datei, nicht um ein ZIP-Archiv.
* Sie enthält mehr als 400 Zeichen für Nur-Text oder 400 [abrechenbare Zeichen](https://docs.microsoft.com/azure/cognitive-services/speech-service/text-to-speech#pricing-note) für SSML-Text und weniger als 10.000 Absätze.
  * Bei Nur-Text wird jeder Absatz durch Drücken der **EINGABETASTE** getrennt – [Beispiel für die Eingabe von Nur-Text](https://github.com/Azure-Samples/Cognitive-Speech-TTS/blob/master/CustomVoice-API-Samples/Java/en-US.txt) anzeigen
  * Bei SSML-Text wird jede SSML-Komponente als Absatz betrachtet. SSML-Elemente sollen durch verschiedene Absätze getrennt werden – [Beispiel für die Eingabe von SSML-Text](https://github.com/Azure-Samples/Cognitive-Speech-TTS/blob/master/CustomVoice-API-Samples/Java/SSMLTextInputSample.txt) anzeigen.
> [!NOTE]
> Bei Chinesisch (Festland), Chinesisch (Hongkong), Chinesisch (Taiwan), Japanisch und Koreanisch zählt jedes Wort als zwei Zeichen. 

## <a name="submit-synthesis-requests"></a>Übermitteln von Syntheseanforderungen

Nach dem Vorbereiten des Eingabeinhalts befolgen Sie die Anweisungen unter [Schnellstart: Asynchrone Synthese für lange Audioinhalte in Python (Vorschau)](https://aka.ms/long-audio-python), um die Anforderung zu übermitteln. Wenn Sie über mehr als eine Eingabedatei verfügen, müssen Sie mehrere Anforderungen übermitteln. Es gelten jedoch einige Einschränkungen, die Sie beachten müssen: 
* Ein Client darf für jedes Azure-Abonnementkonto bis zu fünf Anforderungen pro Sekunde an den Server übermitteln. Wenn dieses Limit überschritten wird, wird der Fehlercode 429 (zu viele Anforderungen) an den Client gesendet. Reduzieren Sie die Menge an Anforderungen pro Sekunde.
* Auf dem Server dürfen bis zu 120 Anforderungen für jedes Azure-Abonnementkonto ausgeführt und in die Warteschlange eingereiht werden. Wenn dieses Limit überschritten wird, gibt der Server den Fehlercode 429 (zu viele Anforderungen) zurück. Warten Sie, und senden Sie erst dann wieder neue Anforderungen, wenn einige Anforderungen abgeschlossen sind.
* Der Server speichert bis zu 20.000 Anforderungen für jedes Azure-Abonnementkonto. Wenn dieses Limit überschritten wird, löschen Sie einige Anforderungen, bevor Sie neue übermitteln.

## <a name="audio-output-formats"></a>Audioausgabeformate

Wir unterstützen flexible Audioausgabeformate. Sie können Audioausgaben pro Absatz generieren oder mit dem Parameter „concatenateResult“ die Einzelausgaben zu einer einzigen Ausgabe verketten. Die folgenden Audioausgabeformate werden von der API für lange Audioinhalte unterstützt:

> [!NOTE]
> Das Standardaudioformat ist riff-16khz-16bit-mono-pcm.

* riff-8khz-16bit-mono-pcm
* riff-16khz-16bit-mono-pcm
* riff-24khz-16bit-mono-pcm
* riff-48khz-16bit-mono-pcm
* audio-16khz-32kbitrate-mono-mp3
* audio-16khz-64kbitrate-mono-mp3
* audio-16khz-128kbitrate-mono-mp3
* audio-24khz-48kbitrate-mono-mp3
* audio-24khz-96kbitrate-mono-mp3
* audio-24khz-160kbitrate-mono-mp3

## <a name="quickstarts"></a>Schnellstarts

Wir bieten Schnellstarts, die Ihnen helfen sollen, die API für lange Audioinhalte erfolgreich zu verwenden. In dieser Tabelle werden Schnellstarts für die API für lange Audioinhalte nach Sprache aufgelistet.

* [Schnellstart: Python](https://aka.ms/long-audio-python)

## <a name="sample-code"></a>Beispielcode
Beispielcode für die API für lange Audioinhalte finden Sie auf GitHub.

* [Beispielcode: Python](https://github.com/Azure-Samples/Cognitive-Speech-TTS/tree/master/CustomVoice-API-Samples/Python)
* [Beispielcode: C#](https://github.com/Azure-Samples/Cognitive-Speech-TTS/tree/master/CustomVoice-API-Samples/CSharp)
* [Beispielcode: Java](https://github.com/Azure-Samples/Cognitive-Speech-TTS/blob/master/CustomVoice-API-Samples/Java/)
