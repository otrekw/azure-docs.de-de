---
title: 'Übersicht über die Sprachsynthese: Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: Das Feature Text-zu-Sprache in Speech-Dienst ermöglicht es Ihren Anwendungen, Tools oder Geräten, Text in natürliche, menschenähnliche synthetisierte Sprache umzuwandeln. Dieser Artikel bietet einen Überblick über die Vorteile und Funktionen des Sprachsynthesediensts.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 09/01/2020
ms.author: trbye
ms.custom: cog-serv-seo-aug-2020
keywords: Sprachsynthese
ms.openlocfilehash: 73e37fde4b3c2dd1aeb6ab171c3726f1b4353949
ms.sourcegitcommit: 5f482220a6d994c33c7920f4e4d67d2a450f7f08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2021
ms.locfileid: "107106087"
---
# <a name="what-is-text-to-speech"></a>Was ist Text-zu-Sprache?

[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

In dieser Übersicht erfahren Sie mehr über die Vorteile und Funktionen des Sprachsynthesediensts, der Ihren Anwendungen, Tools oder Geräten das Konvertieren von Text in menschenähnliche synthetisierte Sprache ermöglicht. Verwenden Sie menschenähnliche neuronale Stimmen, oder erstellen Sie eine benutzerdefinierte Stimme, die für Ihr Produkt oder Ihre Marke exklusiv ist. Eine vollständige Liste der unterstützten Stimmen, Sprachen und Gebietsschemas finden Sie unter [unterstützte Sprachen](language-support.md#text-to-speech).

Diese Dokumentation enthält die folgenden Arten von Artikeln:

* **Schnellstarts** sind Anleitungen zu den ersten Schritten, die Sie durch das Senden von Anforderungen an den Dienst führen.
* **Anleitungen** enthalten Anweisungen zur spezifischeren oder individuelleren Verwendung des Diensts.
* Die Artikel zu **Konzepten** enthalten ausführliche Erläuterungen der Dienstfunktionen und -features.
* **Tutorials** sind ausführlichere Leitfäden, in denen die Verwendung des Diensts als Komponente in umfassenderen Unternehmenslösungen veranschaulicht wird.

> [!NOTE]
> Die Bing-Spracheingabe wurde am 15. Oktober 2019 eingestellt. Wenn Ihre Anwendungen, Tools oder Produkte die Bing-Spracheingabe-APIs oder Custom Speech verwenden, finden Sie in den nachfolgend aufgelisteten Leitfäden Informationen zur Migration zum Speech-Dienst.
> - [Migrieren von der Bing-Spracheingabe zum Speech-Dienst](how-to-migrate-from-bing-speech.md)

## <a name="core-features"></a>Wichtige Funktionen

* Sprachsynthese: Verwenden Sie das [Speech SDK](./get-started-text-to-speech.md) oder die [REST-API](rest-text-to-speech.md), um mithilfe von Standard-, neuronalen oder benutzerdefinierten Stimmen Text in Sprache zu konvertieren.

* Asynchrone Synthese von langen Audioclips: Verwenden Sie die [API für lange Audioinhalte-](long-audio-api.md), um Text-to-Speech-Dateien von mehr als 10 Minuten Länge (z. B. Audiobücher oder Vorlesungen) asynchron zu synthetisieren. Anders als bei der Synthese mithilfe des Speech SDK oder der Sprache-zu-Text-REST API werden keine Antworten in Echtzeit zurückgegeben. Es wird davon ausgegangen, dass Anforderungen asynchron gesendet werden, Antworten abgerufen werden und dass das synthetisierte Audiosignal heruntergeladen wird, wenn es durch den Dienst zur Verfügung gestellt wird. Es werden nur benutzerdefinierte neuronale Stimmen unterstützt.

* Neuronale Stimmen: Tiefe neuronale Netzwerke werden verwendet, um die Grenzen der herkömmlichen Sprachsynthese in Bezug auf Betonung und Intonation in gesprochener Sprache zu überwinden. Die Vorhersage der Satzgliederung und die Stimmensynthese werden gleichzeitig ausgeführt, sodass eine flüssigere und natürlicher klingende Ausgabe entsteht. Gestalten Sie mit neuronalen Stimmen Interaktionen mit Chatbots und Sprachassistenten noch natürlicher und einladender, wandeln Sie digitale Texte wie E-Books in Audiobooks um, und verpassen Sie Ihrem Navigationssystem im Auto ein Upgrade. Durch natürliche, menschenähnliche Intonation und klare Aussprache von Wörtern können neuronale Stimmen die Hörermüdung bei der Interaktion mit KI-Systemen erheblich verringern. Eine vollständige Liste der neuronalen Stimmen finden Sie unter [Unterstützte Sprachen](language-support.md#text-to-speech).

* Anpassen von Sprachstilen mit SSML: Die Markupsprache für Sprachsynthese (Speech Synthesis Markup Language, SSML) ist eine XML-basierte Markupsprache, mit der Ausgaben der Spracherkennung angepasst werden. Mit SSML können Sie Tonhöhe und Pausen anpassen, die Aussprache verbessern, die Sprechgeschwindigkeit beschleunigen oder verlangsamen, die Lautstärke erhöhen oder verringern und einem einzelnen Dokument mehrere Stimmen zuordnen. Weitere Informationen zum Anpassen von Sprachstilen finden Sie in der [Vorgehensweise](speech-synthesis-markup.md).

* Viseme: [Viseme](how-to-speech-synthesis-viseme.md) (Mundbilder) sind die wichtigsten Gesichtsausdrücke beim Sprechen und umfassen u. a. die Position der Lippen, des Kiefers und der Zunge beim Erzeugen eines bestimmten Phonems. Viseme sind stark mit Stimmen und Phonemen korreliert. Sie können mit Visemereignissen im Speech SDK Gesichtsanimationsdaten generieren. Mit diesen lassen sich Gesichter für die Kommunikation durch Lippenlesen, für Unterricht, Unterhaltung oder Kundendienst animieren.

> [!NOTE]
> Viseme-Ereignisse werden derzeit nur für die Stimme `en-US-AriaNeural` unterstützt.

## <a name="get-started"></a>Erste Schritte

Verwenden Sie die [Schnellstartanleitung](get-started-text-to-speech.md), um mit der Sprachsynthese (Text-zu-Sprache) loszulegen. Der Sprachsynthesedienst ist über das [Speech SDK](speech-sdk.md), die [REST-API](rest-text-to-speech.md) und die [Speech CLI](spx-overview.md) verfügbar.

## <a name="sample-code"></a>Beispielcode

Beispielcode für Text-zu-Sprache finden Sie auf GitHub. Diese Beispiele umfassen die Text-zu-Sprache-Konvertierung in den gängigsten Programmiersprachen.

- [Beispiele für Text-zu-Sprache (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)
- [Beispiele für Text-zu-Sprache (REST)](https://github.com/Azure-Samples/Cognitive-Speech-TTS)

## <a name="customization"></a>Anpassung

Über die neuronalen Stimmen hinaus können Sie benutzerdefinierte Stimmen erstellen und optimieren, die für Ihr Produkt oder Ihre Marke exklusiv sind. Für den Einstieg benötigen Sie lediglich einige Audiodateien und die dazugehörigen Transkriptionen. Weitere Informationen finden Sie unter [Erste Schritte mit Custom Voice](how-to-custom-voice.md).

## <a name="pricing-note"></a>Preishinweis

Bei der Nutzung des Sprachsynthesediensts wird Ihnen einschließlich Interpunktion jedes Zeichen in Rechnung gestellt, das in Sprache umgewandelt wird. Während das SSML-Dokument selbst nicht abrechenbar ist, werden optionale Elemente wie Phoneme und Tonhöhe, mit denen eingestellt wird, wie der Text in Sprache umgewandelt wird, als abrechenbare Zeichen gezählt. Folgendes wird abgerechnet:

- Text, der dem Sprachsynthesedienst im SSML-Text der Anforderung übergeben wird
- Alle Markups im Textfeld des Anforderungstexts im SSML-Format, mit Ausnahme von `<speak>`- und `<voice>`-Tags
- Buchstaben, Satzzeichen, Leerzeichen, Tabulatoren, Markups und alle sonstigen Leerzeichen
- Jeder in Unicode definierte Codepunkt

Ausführliche Informationen finden Sie unter [Cognitive Services-Preise – Sprachdienste](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

> [!IMPORTANT]
> Chinesische, japanische und koreanische Zeichen zählen bei der Abrechnung jeweils als zwei Zeichen.

## <a name="reference-docs"></a>Referenz

- [Speech SDK](speech-sdk.md)
- [REST-API: Sprachsynthese](rest-text-to-speech.md)

## <a name="next-steps"></a>Nächste Schritte

- [Abrufen eines kostenlosen Speech-Dienstabonnements](overview.md#try-the-speech-service-for-free)
- [Abrufen des Speech SDK](speech-sdk.md)
