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
ms.openlocfilehash: 0e18fd0c52fd4090477599f53cd0ef0bc05855f2
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83587339"
---
# <a name="long-audio-api-preview"></a>API für lange Audioinhalte (Vorschau)

Die API für lange Audioinhalte ist für asynchrone lange Sprachsynthesen konzipiert. Beispiele wären etwa Hörbücher, Zeitungsartikel und Dokumente. Diese API gibt keine synthetisierten Audiodaten in Echtzeit zurück, sondern erwartet, dass Sie die Antworten abrufen und die Ausgaben so verwenden, wie sie vom Dienst bereitgestellt werden. Im Gegensatz zur Text-to-Speech-API, die vom Speech SDK verwendet wird, kann die API für lange Audioinhalte synthetisierte Audiodaten erzeugen, die länger als 10 Minuten sind, wodurch sie ideal für Herausgeber und Plattformen für Audioinhalte geeignet ist.

Weitere Vorteile der API für lange Audioinhalte:

* Die vom Dienst zurückgegebene Stimmensynthese verwendet die besten neuronalen Stimmen.
* Es muss kein Stimmendpunkt bereitgestellt werden, da Stimmen in einem nicht echtzeitbasierten Batchmodus synthetisiert werden.

> [!NOTE]
> Die API für lange Audioinhalte unterstützt jetzt sowohl [öffentliche neuronale Stimmen](https://docs.microsoft.com/azure/cognitive-services/speech-service/language-support#neural-voices) als auch [benutzerdefinierte neuronale Stimmen](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-custom-voice#custom-neural-voices).

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

Nach dem Vorbereiten des Eingabeinhalts befolgen Sie die Anweisungen unter [Schnellstart: Asynchrone Synthese für lange Audioinhalte in Python (Vorschau)](https://aka.ms/long-audio-python), um die Anforderung zu übermitteln. Wenn Sie über mehr als eine Eingabedatei verfügen, müssen Sie mehrere Anforderungen übermitteln. 

Die **HTTP-Statuscodes** weisen auf häufige Fehler hin.

| API | HTTP-Statuscode | Beschreibung | Vorschlag |
|-----|------------------|-------------|----------|
| Erstellen | 400 | Die Stimmensynthese ist in dieser Region nicht aktiviert. | Verwenden Sie einen Speech-Abonnementschlüssel mit einer unterstützten Region. |
|        | 400 | Nur das **Standard**-Speech-Abonnement für diese Region ist gültig. | Ändern Sie den Speech-Abonnementschlüssel in den Tarif „Standard“. |
|        | 400 | Für das Azure-Konto wurde das Anforderungslimit von 20.000 Anforderungen überschritten. Entfernen Sie einige Anforderungen, bevor Sie neue Anforderungen übermitteln. | Der Server speichert für jedes Azure-Konto bis zu 20.000 Anforderungen. Löschen Sie einige Anforderungen, bevor Sie neue Anforderungen übermitteln. |
|        | 400 | Das folgende Modell kann nicht in der Stimmensynthese verwendet werden: {modelID}. | Stellen Sie sicher, dass der Zustand von {modelID} korrekt ist. |
|        | 400 | Die Region für die Anforderung entspricht nicht der Region für das Modell: {modelID}. | Stellen Sie sicher, dass die Region von {modelID} der Region der Anforderung entspricht. |
|        | 400 | Die Textdatei wird von der Stimmensynthese nur mit UTF-8-Codierung und Bytereihenfolge-Marke unterstützt. | Stellen Sie sicher, dass die Eingabedatei über UTF-8-Codierung und Bytereihenfolge-Marke verfügen. |
|        | 400 | In der Stimmensyntheseanforderung sind nur gültige SSML-Eingaben zulässig. | Stellen Sie sicher, dass die SSML-Eingabeausdrücke korrekt sind. |
|        | 400 | Der Stimmname „{voiceName}“ wurde in der Eingabedatei nicht gefunden. | Der SSML-Stimmname der Eingabe ist nicht auf die Modell-ID abgestimmt. |
|        | 400 | Der Absatz in der Eingabedatei muss weniger als 10.000 Zeichen enthalten. | Stellen Sie sicher, dass der Absatz in der Datei weniger als 10.000 Zeichen enthält. |
|        | 400 | Die Eingabedatei muss mehr als 400 Zeichen enthalten. | Stellen Sie sicher, dass die Eingabedatei mehr als 400 Zeichen enthält. |
|        | 404 | Das in der Stimmensynthesedefinition deklarierte Modell wurde nicht gefunden: {modelID}. | Stellen Sie sicher, dass die Modell-ID ({modelID}) korrekt ist. |
|        | 429 | Der Grenzwert für die aktive Stimmensynthese wurde überschritten. Warten Sie, bis einige Anforderungen abgeschlossen wurden. | Der Server kann für jedes Azure-Konto bis zu 120 Anforderungen ausführen und in die Warteschlange einreihen. Warten Sie, und senden Sie erst dann wieder neue Anforderungen, wenn einige Anforderungen abgeschlossen wurden. |
| All       | 429 | Zu viele Anforderungen. | Der Client kann für jedes Azure-Konto pro Sekunde bis zu fünf Anforderungen an den Server übermitteln. Verringern Sie die Anforderungen pro Sekunde. |
| Löschen    | 400 | Die Stimmensyntheseaufgabe wird noch verwendet. | Nur Anforderungen mit dem Status **Abgeschlossen** oder **Fehler** können gelöscht werden. |
| GetByID   | 404 | Die angegebene Entität wurde nicht gefunden. | Stellen Sie sicher, dass die Synthese-ID korrekt ist. |

## <a name="regions-and-endpoints"></a>Regionen und Endpunkte

Die API für lange Audioinhalte ist in mehreren Regionen mit eindeutigen Endpunkten verfügbar:

| Region | Endpunkt |
|--------|----------|
| Australien (Osten) | `https://australiaeast.customvoice.api.speech.microsoft.com` |
| Kanada, Mitte | `https://canadacentral.customvoice.api.speech.microsoft.com` |
| East US | `https://eastus.customvoice.api.speech.microsoft.com` |
| Indien, Mitte | `https://centralindia.customvoice.api.speech.microsoft.com` |
| USA Süd Mitte | `https://southcentralus.customvoice.api.speech.microsoft.com` |
| Asien, Südosten | `https://southeastasia.customvoice.api.speech.microsoft.com` |
| UK, Süden | `https://uksouth.customvoice.api.speech.microsoft.com` |
| Europa, Westen | `https://westeurope.customvoice.api.speech.microsoft.com` |
| USA, Westen 2 | `https://westus2.customvoice.api.speech.microsoft.com` |

## <a name="audio-output-formats"></a>Audioausgabeformate

Wir unterstützen flexible Audioausgabeformate. Sie können Audioausgaben pro Absatz generieren oder mit dem Parameter „concatenateResult“ zu einer einzelnen Ausgabe verketten. Die folgenden Audioausgabeformate werden von der API für lange Audioinhalte unterstützt:

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
