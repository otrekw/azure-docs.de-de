---
title: Spracherkennung – Speech-Dienst
titleSuffix: Azure Cognitive Services
description: Die Spracherkennung im Speech-Dienst ermöglicht die Echtzeittranskription von Audiostreams in Text, den Ihre Anwendungen, Tools oder Geräte nutzen, anzeigen und als Befehlseingabe verarbeiten können. Dieser Dienst arbeitet problemlos mit den Features Sprachsynthese (Text-to-Speech) und Sprachübersetzung zusammen.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/03/2019
ms.author: erhopf
ms.openlocfilehash: 651a30bbcba7b4d4f5d00765c651be73953cd748
ms.sourcegitcommit: 6c01e4f82e19f9e423c3aaeaf801a29a517e97a0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2019
ms.locfileid: "74815501"
---
# <a name="what-is-speech-to-text"></a>Was ist die Spracherkennung?

Die Spracherkennung im Speech-Dienst, auch als Sprache-in-Text bezeichnet, ermöglicht die Echtzeittranskription von Audiostreams in Text, den Ihre Anwendungen, Tools oder Geräte nutzen, anzeigen und als Befehlseingabe verarbeiten können. Dieser Dienst nutzt dieselbe Erkennungstechnologie, die Microsoft auch bei Cortana und Office-Produkten einsetzt, und funktioniert nahtlos mit Übersetzungen und Sprachsynthese. Eine vollständige Liste der Spracherkennungssprachen finden Sie unter [Unterstützte Sprachen](https://docs.microsoft.com/azure/cognitive-services/speech-service/language-support#speech-to-text).

Der Spracherkennungsdienst verwendet standardmäßig das sogenannte Universal Language Model. Dieses Modell wurde mit Microsoft-Daten trainiert, und es wird in der Cloud bereitgestellt. Es eignet sich besonders für Gesprächs- oder Diktatszenarios. Wenn Sie die Spracherkennung für die Erkennung und Transkription in einer individuellen Umgebung verwenden, können Sie benutzerdefinierte Akustik-, Sprach- und Aussprachemodelle erstellen, um Umgebungsgeräusche zu kompensieren oder branchenspezifisches Vokabular zu berücksichtigen.

Mit dem Speech SDK und den REST-APIs können Sie unkompliziert Tonaufnahmen eines Mikrofons erfassen, aus einem Stream lesen und auf Audiodateien in einem Speicher zugreifen. Das Speech SDK unterstützt für die Spracherkennung 16-Bit-WAV/PCM-Einkanalaudiodateien mit einer Frequenz von 16 kHz/8 kHz. Zusätzliche Audioformate werden mit dem [Spracherkennungs-REST-Endpunkt](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) oder dem [Batchtranskriptionsdienst](https://docs.microsoft.com/azure/cognitive-services/speech-service/batch-transcription#supported-formats) unterstützt.

## <a name="core-features"></a>Wichtige Funktionen

In der folgenden Tabelle sind die Funktionen des Speech SDK und der REST-APIs aufgelistet:

| Anwendungsfall | SDK | REST |
|--------- | --- | ---- |
| Transkribieren einer kurzen Äußerung (< 15 Sekunden). Unterstützt nur ein endgültiges Transkriptionsergebnis. | Ja | Ja\* |
| Kontinuierliche Transkription einer langen Äußerungen und das Streamen von Audio (> 15 Sekunden). Unterstützt zeitweilige und endgültige Transkriptionsergebnisse. | Ja | Nein |
| Ableiten von Absichten von Erkennungsergebnissen mit dem [LUIS](https://docs.microsoft.com/azure/cognitive-services/luis/what-is-luis). | Ja | Nein\*\* |
| Asynchrone Batchtranskription langer Audiodateien. | Nein  | Ja\*\*\* |
| Erstellen und Verwalten von Sprachmodellen. | Nein | Ja\*\*\* |
| Erstellen und Verwalten benutzerdefinierter Modellimplementierungen. | Nein  | Ja\*\*\* |
| Erstellen von Genauigkeitsprüfungen zum Messen der Genauigkeit eines einfachen Modells im Vergleich zu einem benutzerdefinierten Modell. | Nein  | Ja\*\*\* |
| Verwalten von Abonnements. | Nein  | Ja\*\*\* |

\*_Mit der REST-Funktionalität können Sie Audiodaten mit einer Länge von bis zu 60 Sekunden übertragen und erhalten anschließend ein Aufzeichnungsergebnis._

\*\*_Sie können die LUIS-Absichten und -Entitäten mithilfe eines separaten LUIS-Abonnements ableiten. Über dieses Abonnement ruft das SDK den LUIS auf und gibt Ergebnisse für Absichten und Entitäten aus. Mithilfe der REST-API können Sie LUIS selbst aufrufen, um Absichten und Entitäten mithilfe Ihres LUIS-Abonnements abzuleiten._

\*\*\*_Diese Dienste sind über den Endpunkt „cris.ai“ verfügbar. Weitere Informationen finden Sie in der [Swagger-Referenz](https://westus.cris.ai/swagger/ui/index)._

## <a name="get-started-with-speech-to-text"></a>Erste Schritte mit der Spracherkennung

Wir stellen Schnellstartanleitungen für die gängigsten Programmiersprachen bereit. Diese sind jeweils so konzipiert, dass Sie in weniger als zehn Minuten über ausführbaren Code verfügen. In [dieser Tabelle](https://aka.ms/csspeech#5-minute-quickstarts) sind alle Schnellstarts für das Speech SDK nach Plattform und Sprache aufgelistet. Einen API-Verweis finden Sie auch [hier](https://aka.ms/csspeech#reference).

Wenn Sie den Spracherkennungs-REST-Dienst verwenden möchten, finden Sie weitere Informationen unter [REST-APIs](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis).

## <a name="tutorials-and-sample-code"></a>Tutorials und Beispielcode

Nachdem Sie Gelegenheit hatten, den Speech-Dienst zu verwenden, erfahren Sie in unserem Tutorial, wie Sie mithilfe des Speech SDK und LUIS Absichten in gesprochener Sprache erkennen.

- [Tutorial: Erkennen von Absichten anhand von gesprochener Sprache mit dem Speech SDK für C#](how-to-recognize-intents-from-speech-csharp.md)

Beispielcode für das Speech SDK finden Sie auf GitHub. In den Beispielen werden gängige Szenarios wie etwa das Lesen von Audiodaten aus einer Datei oder einem Stream, die kontinuierliche und einmalige Erkennung oder die Verwendung benutzerdefinierter Modelle behandelt.

- [Speech-to-text samples (SDK) (Spracherkennungsbeispiele (SDK))](https://github.com/Azure-Samples/cognitive-services-speech-sdk)
- [Batch transcription samples (REST) (Batchtranskriptionsbeispiele (REST))](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/batch)

## <a name="customization"></a>Anpassung

Zusätzlich zum Standardbasismodell, das vom Speech-Dienst verwendet wird, können Sie die Modelle mit verfügbaren Daten an Ihre Bedürfnisse anpassen, um Hindernisse bei der Spracherkennung wie Sprachstil, Wortschatz und Hintergrundgeräusche zu überwinden. Weitere Informationen finden Sie unter [Custom Speech](how-to-custom-speech.md).

> [!NOTE]
> Die Anpassungsoptionen unterscheiden sich je nach Sprache/Gebietsschema. Weitere Informationen erhalten Sie unter [Unterstützte Sprachen](supported-languages.md).

## <a name="migration-guides"></a>Migrationsleitfäden

> [!WARNING]
> Die Bing-Spracheingabe wurde am 15. Oktober 2019 eingestellt.

Wenn Ihre Anwendungen, Tools oder Produkte die Bing-Spracheingabe-APIs oder Custom Speech verwenden, finden Sie in den nachfolgend aufgelisteten Leitfäden Informationen zur Migration zum Speech-Dienst.

- [Migrieren von der Bing-Spracheingabe zum Speech-Dienst](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-migrate-from-bing-speech)
- [Migrieren von Custom Speech zum Speech-Dienst](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-migrate-from-custom-speech-service)

## <a name="reference-docs"></a>Referenz

- [Speech SDK](https://aka.ms/csspeech)
- [Speech-Geräte-SDK](speech-devices-sdk.md)
- [REST-API: Spracherkennung](rest-speech-to-text.md)
- [REST-API: Sprachsynthese](rest-text-to-speech.md)
- [REST-API: Batchtranskription und Anpassung](https://westus.cris.ai/swagger/ui/index)

## <a name="next-steps"></a>Nächste Schritte

- [Kostenloses Testen des Speech-Diensts](get-started.md)
- [Abrufen des Speech SDK](speech-sdk.md)
