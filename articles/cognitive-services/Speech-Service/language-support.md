---
title: Sprachunterstützung – Speech-Dienst
titleSuffix: Azure Cognitive Services
description: Der Speech-Dienst unterstützt neben der Sprachübersetzung zahlreiche Sprachen für die Konvertierung von Sprache in Text und Text in Sprache. Dieser Artikel enthält eine umfassende Liste zur Sprachunterstützung der einzelnen Dienstfunktionen.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/26/2020
ms.author: trbye
ms.custom: seodec18
ms.openlocfilehash: 7d5dd7d2d0aa3d10f439b5cf3604e2d595456f86
ms.sourcegitcommit: 2989396c328c70832dcadc8f435270522c113229
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2020
ms.locfileid: "92172264"
---
# <a name="language-and-voice-support-for-the-speech-service"></a>Sprach- und Stimmunterstützung für den Speech-Dienst

Die Sprachunterstützung ist abhängig von der Funktion des Speech-Diensts. In den folgenden Tabellen ist die Sprachunterstützung für die Dienstangebote [Spracherkennung](#speech-to-text), [Text-zu-Sprache](#text-to-speech), und [Sprachübersetzung](#speech-translation) zusammengefasst.

## <a name="speech-to-text"></a>Spracherkennung

Sowohl das Microsoft Speech SDK als auch die REST-API unterstützen die folgenden Sprachen (Gebietsschemas). 

Um die Genauigkeit zu erhöhen, wird die Anpassung für eine Teilmenge der Sprachen durch das Hochladen von **Audio und menschenmarkierten Transkripten** oder **zugehörigem Text (Sätze)** angeboten. Weitere Informationen zur Anpassung finden Sie unter [Was ist Custom Speech?](how-to-custom-speech.md).

<!--
To get the AM and ML bits:
https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0/operations/GetSupportedLocalesForModels

To get pronunciation bits:
https://cris.ai -> Click on Adaptation Data -> scroll down to section "Pronunciation Datasets" -> Click on Import -> Locale: the list of locales there correspond to the supported locales
-->

| Gebietsschema  | Sprache                          | Anpassungen                                    |
|---------|-----------------------------------|---------------------------------------------------|
| `ar-AE` | Arabisch (Vereinigte Arabische Emirate)     | Sprachmodell                                    |
| `ar-BH` | Arabisch (Bahrain), modernes Hocharabisch | Sprachmodell                                    |
| `ar-EG` | Arabisch (Ägypten)                    | Sprachmodell                                    |
| `ar-IQ` | Arabisch (Irak)                     | Sprachmodell                                    |
| `ar-JO` | Arabisch (Jordanien)                   | Sprachmodell                                    |
| `ar-KW` | Arabisch (Kuwait)                   | Sprachmodell                                    |
| `ar-LB` | Arabisch (Libanon)                  | Sprachmodell                                    |
| `ar-OM` | Arabisch (Oman)                     | Sprachmodell                                    |
| `ar-QA` | Arabisch (Katar)                    | Sprachmodell                                    |
| `ar-SA` | Arabisch (Saudi-Arabien)             | Sprachmodell                                    |
| `ar-SY` | Arabisch (Syrien)                    | Sprachmodell                                    |
| `bg-BG` | Bulgarisch (Bulgarien)              | Sprachmodell                                    |
| `ca-ES` | Katalanisch (Spanien)                   | Sprachmodell                                    |
| `cs-CZ` | Tschechisch (Tschechische Republik)            | Sprachmodell                                    | 
| `da-DK` | Dänisch (Dänemark)                  | Sprachmodell                                    |
| `de-DE` | Deutsch (Deutschland)                  | Akustische Modell<br>Sprachmodell<br>Aussprache |
| `el-GR` | Griechisch (Griechenland)                    | Sprachmodell                                    |
| `en-AU` | Englisch (Australien)               | Akustische Modell<br>Sprachmodell                  |
| `en-CA` | Englisch (Kanada)                  | Akustische Modell<br>Sprachmodell                  |
| `en-GB` | Walisisch (Großbritannien)          | Akustische Modell<br>Sprachmodell<br>Aussprache |
| `en-HK` | Englisch (Hongkong)               | Sprachmodell                                    | 
| `en-IE` | Englisch (Irland)                 | Sprachmodell                                    | 
| `en-IN` | Englisch (Indien)                   | Akustische Modell<br>Sprachmodell                  |
| `en-NZ` | Englisch (Neuseeland)             | Akustische Modell<br>Sprachmodell                  |
| `en-PH` | Englisch (Philippinen)             | Sprachmodell                                    | 
| `en-SG` | Englisch (Singapur)               | Sprachmodell                                    | 
| `en-US` | Englisch (USA)           | Akustische Modell<br>Sprachmodell<br>Aussprache |
| `en-ZA` | Englisch (Südafrika)            | Sprachmodell                                    | 
| `es-AR` | Spanisch (Argentinien)               | Sprachmodell                                    | 
| `es-BO` | Spanisch (Bolivien)                 | Sprachmodell                                    | 
| `es-CL` | Spanisch (Chile)                   | Sprachmodell                                    | 
| `es-CO` | Spanisch (Kolumbien)                | Sprachmodell                                    | 
| `es-CR` | Spanisch (Costa Rica)              | Sprachmodell                                    | 
| `es-CU` | Spanisch (Kuba)                    | Sprachmodell                                    | 
| `es-DO` | Spanisch (Dominikanische Republik)      | Sprachmodell                                    | 
| `es-EC` | Spanisch (Ecuador)                 | Sprachmodell                                    | 
| `es-ES` | Spanisch (Spanien)                   | Akustische Modell<br>Sprachmodell                  |
| `es-GT` | Spanisch (Guatemala)               | Sprachmodell                                    | 
| `es-HN` | Spanisch (Honduras)                | Sprachmodell                                    | 
| `es-MX` | Spanisch (Mexiko)                  | Akustische Modell<br>Sprachmodell                  |
| `es-NI` | Spanisch (Nicaragua)               | Sprachmodell                                    | 
| `es-PA` | Spanisch (Panama)                  | Sprachmodell                                    | 
| `es-PE` | Spanisch (Peru)                    | Sprachmodell                                    | 
| `es-PR` | Spanisch (Puerto Rico)             | Sprachmodell                                    | 
| `es-PY` | Spanisch (Paraguay)                | Sprachmodell                                    | 
| `es-SV` | Spanisch (El Salvador)             | Sprachmodell                                    | 
| `es-US` | Spanisch (USA)                     | Sprachmodell                                    | 
| `es-UY` | Spanisch (Uruguay)                 | Sprachmodell                                    | 
| `es-VE` | Spanisch (Venezuela)               | Sprachmodell                                    |
| `et-EE` | Estnisch (Estland)                 | Sprachmodell                                    | 
| `fi-FI` | Finnisch (Finnland)                 | Sprachmodell                                    |
| `fr-CA` | Französisch (Kanada)                   | Akustische Modell<br>Sprachmodell                  |
| `fr-FR` | Französisch (Frankreich)                   | Akustische Modell<br>Sprachmodell<br>Aussprache |
| `ga-IE` | Irisch (Irland)                    | Sprachmodell                                    |
| `gu-IN` | Gujarati (Indien)                 | Sprachmodell                                    |
| `hi-IN` | Hindi (Indien)                     | Akustische Modell<br>Sprachmodell                  |
| `hr-HR` | Kroatisch (Kroatien)                | Sprachmodell                                    |
| `hu-HU` | Ungarisch (Ungarn)               | Sprachmodell                                    | 
| `it-IT` | Italienisch (Italien)                   | Akustische Modell<br>Sprachmodell<br>Aussprache |
| `ja-JP` | Japanisch (Japan)                  | Sprachmodell                                    |
| `ko-KR` | Koreanisch (Korea)                    | Sprachmodell                                    |
| `lt-LT` | Litauisch (Litauen)            | Sprachmodell                                    |
| `lv-LV` | Lettisch (Lettland)                  | Sprachmodell                                    |
| `mr-IN` | Marathi (Indien)                   | Sprachmodell                                    |
| `mt-MT` | Maltesisch (Malta)                    | Sprachmodell                                    |
| `nb-NO` | Norwegisch, Bokmål (Norwegen)       | Sprachmodell                                    |
| `nl-NL` | Niederländisch (Niederlande)               | Sprachmodell                                    |
| `pl-PL` | Polnisch (Polen)                   | Sprachmodell                                    |
| `pt-BR` | Portugiesisch (Brasilien)               | Akustische Modell<br>Sprachmodell<br>Aussprache |
| `pt-PT` | Portugiesisch (Portugal)             | Sprachmodell                                    |
| `ro-RO` | Rumänisch (Rumänien)                | Sprachmodell                                    |
| `ru-RU` | Russisch (Russische Föderation)                  | Akustische Modell<br>Sprachmodell                  |
| `sk-SK` | Slowakisch (Slowakei)                 | Sprachmodell                                    |
| `sl-SI` | Slowenisch (Slowenien)              | Sprachmodell                                    |
| `sv-SE` | Schwedisch (Schweden)                  | Sprachmodell                                    |
| `ta-IN` | Tamil (Indien)                     | Sprachmodell                                    |
| `te-IN` | Telugu (Indien)                    | Sprachmodell                                    |
| `th-TH` | Thai (Thailand)                   | Sprachmodell                                    |
| `tr-TR` | Türkisch (Türkei)                  | Sprachmodell                                    |
| `zh-CN` | Chinesisch (Mandarin, vereinfacht)    | Akustische Modell<br>Sprachmodell                  |
| `zh-HK` | Chinesisch (Kantonesisch, traditionell)  | Sprachmodell                                    |
| `zh-TW` | Chinesisch (Taiwanesisch, Mandarin)      | Sprachmodell                                    |

## <a name="text-to-speech"></a>Text-zu-Sprache

Sowohl das Microsoft Speech SDK als auch die REST-API unterstützen diese Stimmen. Jede dieser Stimmen steht für eine bestimmte Sprache und einen bestimmten Dialekt und wird durch das Gebietsschema identifiziert. Über die [API „voices/list“](rest-text-to-speech.md#get-a-list-of-voices) können Sie auch eine vollständige Liste der Sprachen und Stimmen abrufen, die für die jeweilige Region/den jeweiligen Endpunkt unterstützt werden. 

> [!IMPORTANT]
> Die Preise variieren für Standardstimmen, benutzerdefinierte und neuronale Stimmen. Weitere Informationen finden Sie auf der Seite [Preise](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

### <a name="neural-voices"></a>Neuronale Stimmen

Die neuronale Sprachsynthese ist eine neue Art der Sprachsynthese mithilfe von Deep Neural Networks. Wenn Sie eine neuronale Stimme verwenden, kann die synthetisierte Sprache kaum von menschlichen Aufzeichnungen unterschieden werden.

Gestalten Sie mit neuronalen Stimmen Interaktionen mit Chatbots und Sprachassistenten noch natürlicher und einladender, wandeln Sie digitale Texte wie E-Books in Audiobooks um, und verpassen Sie Ihrem Navigationssystem im Auto ein Upgrade. Durch natürliche, menschenähnliche Intonation und klare Aussprache von Wörtern können neuronale Stimmen die Hörermüdung bei der Interaktion mit KI-Systemen erheblich verringern.

Weitere Informationen zur regionalen Verfügbarkeit finden Sie unter [Regionen](regions.md#standard-and-neural-voices).

|Gebietsschema  | Sprache            | Geschlecht | Name der Stimme | Stilunterstützung |
|--|--|--|--|--|
| `ar-EG` | Arabisch (Ägypten) | Female | `ar-EG-SalmaNeural` | Allgemein |
| `ar-SA` | Arabisch (Saudi-Arabien) | Female | `ar-SA-ZariyahNeural` | Allgemein |
| `bg-BG` <sup>Neu</sup> | Bulgarisch (Bulgarien) | Female | `bg-BG-KalinaNeural` | Allgemein |
| `ca-ES` | Katalanisch (Spanien) | Female | `ca-ES-AlbaNeural` | Allgemein |
| `cs-CZ` <sup>Neu</sup> | Tschechisch (Tschechische Republik)  | Female | `cs-CZ-VlastaNeural` | Allgemein |
| `da-DK` | Dänisch (Dänemark) | Female | `da-DK-ChristelNeural` | Allgemein |
| `de-AT` <sup>Neu</sup> | Deutsch (Österreich) | Female | `de-AT-IngridNeural` | Allgemein |
| `de-CH` <sup>Neu</sup> | Deutsch (Schweiz) | Female | `de-CH-LeniNeural` | Allgemein |
| `de-DE` | Deutsch (Deutschland) | Female | `de-DE-KatjaNeural` | Allgemein |
| `de-DE` <sup>Neu</sup> | Deutsch (Deutschland) | Male | `de-DE-ConradNeural` | Allgemein |
| `el-GR` <sup>Neu</sup> | Griechisch (Griechenland) | Female | `el-GR-AthinaNeural` | Allgemein |
| `en-AU` | Englisch (Australien) | Female | `en-AU-NatashaNeural` | Allgemein |
| `en-AU` <sup>Neu</sup> | Australisch (Australien) | Male | `en-AU-WilliamNeural` | Allgemein |
| `en-CA` | Englisch (Kanada) | Female | `en-CA-ClaraNeural` | Allgemein |
| `en-GB` | Englisch (Vereinigtes Königreich) | Female | `en-GB-LibbyNeural` | Allgemein |
| `en-GB` | Englisch (Vereinigtes Königreich) | Female | `en-GB-MiaNeural` | Allgemein |
| `en-GB` <sup>Neu</sup> | Walisisch (Großbritannien) | Male | `en-GB-RyanNeural` | Allgemein |
| `en-IE` <sup>Neu</sup> | Irisches Englisch (Irland) | Female | `en-IE-EmilyNeural` | Allgemein |
| `en-IN` | Englisch (Indien) | Female | `en-IN-NeerjaNeural` | Allgemein |
| `en-US` | Englisch (Nordamerika) | Female | `en-US-AriaNeural` | Allgemein, mehrere Stimmstile verfügbar |
| `en-US` | Englisch (Nordamerika) | Male | `en-US-GuyNeural` | Allgemein |
| `en-US` <sup>Neu</sup> | Englisch (Nordamerika) | Female | `en-US-JennyNeural` | Allgemein, mehrere Stimmstile verfügbar |
| `es-ES` | Spanisch (Spanien) | Female | `es-ES-ElviraNeural` | Allgemein |
| `es-ES` <sup>Neu</sup> | Spanisch (Spanien) | Male | `es-ES-AlvaroNeural` | Allgemein |
| `es-MX` | Spanisch (Mexiko) | Female | `es-MX-DaliaNeural` | Allgemein |
| `es-MX` <sup>Neu</sup> | Spanisch (Mexiko) | Male | `es-MX-JorgeNeural` | Allgemein |
| `fi-FI` | Finnisch (Finnland) | Female | `fi-FI-NooraNeural` | Allgemein |
| `fr-CA` | Französisch (Kanada) | Female | `fr-CA-SylvieNeural` | Allgemein |
| `fr-CA` <sup>Neu</sup> | Französisch (Kanada) | Male | `fr-CA-JeanNeural` | Allgemein |
| `fr-CH` <sup>Neu</sup> | Französisch (Schweiz) | Female | `fr-CH-ArianeNeural` | Allgemein |
| `fr-FR` | Französisch (Frankreich) | Female | `fr-FR-DeniseNeural` | Allgemein |
| `fr-FR` <sup>Neu</sup> | Französisch (Frankreich) | Male | `fr-FR-HenriNeural` | Allgemein |
| `he-IL` <sup>Neu</sup> | Hebräisch (Israel) | Female | `he-IL-HilaNeural` | Allgemein |
| `hi-IN` | Hindi (Indien) | Female | `hi-IN-SwaraNeural` | Allgemein |
| `hr-HR` <sup>Neu</sup> | Kroatisch (Kroatien) | Female | `hr-HR-GabrijelaNeural` | Allgemein |
| `hu-HU` <sup>Neu</sup> | Ungarisch (Ungarn) | Female | `hu-HU-NoemiNeural` | Allgemein |
| `id-ID` <sup>Neu</sup> | Bahasa Indonesisch (Indonesien) | Male | `id-ID-ArdiNeural` | Allgemein |
| `it-IT` | Italienisch (Italien) | Female | `it-IT-ElsaNeural` | Allgemein |
| `it-IT` <sup>Neu</sup> | Italienisch (Italien) | Female | `it-IT-IsabellaNeural` | Allgemein |
| `it-IT` <sup>Neu</sup> | Italienisch (Italien) | Male | `it-IT-DiegoNeural` | Allgemein |
| `ja-JP` | Japanisch (Japan) | Female | `ja-JP-NanamiNeural` | Allgemein |
| `ja-JP` <sup>Neu</sup> | Japanisch (Japan) | Male | `ja-JP-KeitaNeural` | Allgemein |
| `ko-KR` | Koreanisch (Korea) | Female | `ko-KR-SunHiNeural` | Allgemein |
| `ko-KR` <sup>Neu</sup> | Koreanisch (Korea) | Male | `ko-KR-InJoonNeural` | Allgemein |
| `ms-MY` <sup>Neu</sup> | Malaiisch (Malaysia) | Female | `ms-MY-YasminNeural` | Allgemein |
| `nb-NO` | Norwegisch, Bokmål (Norwegen) | Female | `nb-NO-IselinNeural` | Allgemein |
| `nl-NL` | Niederländisch (Niederlande) | Female | `nl-NL-ColetteNeural` | Allgemein |
| `pl-PL` | Polnisch (Polen) | Female | `pl-PL-ZofiaNeural` | Allgemein |
| `pt-BR` | Portugiesisch (Brasilien) | Female | `pt-BR-FranciscaNeural` | Allgemein, mehrere Stimmstile verfügbar |
| `pt-BR` <sup>Neu</sup> | Brasilianisches Portugiesisch (Brasilien) | Male | `pt-BR-AntonioNeural` | Allgemein |
| `pt-PT` | Portugiesisch (Portugal) | Female | `pt-PT-FernandaNeural` | Allgemein |
| `ro-RO` <sup>Neu</sup> | Rumänisch (Rumänien) | Female | `ro-RO-AlinaNeural` | Allgemein |
| `ru-RU` | Russisch (Russische Föderation) | Female | `ru-RU-DariyaNeural` | Allgemein |
| `sk-SK` <sup>Neu</sup> | Slowakisch (Slowakei) | Female | `sk-SK-ViktoriaNeural` | Allgemein |
| `sl-SI` <sup>Neu</sup> | Slowenisch (Slowenien) | Female | `sl-SI-PetraNeural` | Allgemein |
| `sv-SE` | Schwedisch (Schweden) | Female | `sv-SE-HilleviNeural` | Allgemein |
| `ta-IN` <sup>Neu</sup> | Tamil (Indien) | Female | `ta-IN-PallaviNeural` | Allgemein |
| `te-IN` <sup>Neu</sup> | Telugu (Indien) | Female | `te-IN-ShrutiNeural` | Allgemein |
| `th-TH` | Thai (Thailand) | Female | `th-TH-AcharaNeural` | Allgemein |
| `th-TH` <sup>Neu</sup> | Thai (Thailand) | Female | `th-TH-PremwadeeNeural` | Allgemein |
| `tr-TR` | Türkisch (Türkei) | Female | `tr-TR-EmelNeural` | Allgemein |
| `vi-VN` <sup>Neu</sup> | Vietnamesisch (Vietnam) | Female | `vi-VN-HoaiMyNeural` | Allgemein |
| `zh-CN` | Mandarin (Chinesisch, vereinfacht, China) | Female | `zh-CN-XiaoxiaoNeural` | Allgemein, mehrere Stimmstile verfügbar |
| `zh-CN` | Mandarin (Chinesisch, vereinfacht, China) | Female | `zh-CN-XiaoyouNeural` | Kinderstimme, optimiert für das Erzählen von Geschichten |
| `zh-CN` | Mandarin (Chinesisch, vereinfacht, China) | Male | `zh-CN-YunyangNeural` | Optimiert für das Vorlesen von Nachrichten, mehrere Stimmstile verfügbar |
| `zh-CN` | Mandarin (Chinesisch, vereinfacht, China) | Male | `zh-CN-YunyeNeural` | Optimiert für das Erzählen von Geschichten |
| `zh-HK` | Kantonesisch (Chinesisch, traditionell, Hongkong) | Female | `zh-HK-HiuGaaiNeural` | Allgemein |
| `zh-TW` | Mandarin (Chinesisch, traditionell, Taiwan) | Female | `zh-TW-HsiaoYuNeural` | Allgemein |


> [!IMPORTANT]
> Die Stimme `en-US-JessaNeural` wurde in `en-US-AriaNeural` geändert. Wenn Sie zuvor „Jessa“ verwendet haben, wechseln Sie zu „Aria“.

Informationen dazu, wie Sie neuronale Stimmen konfigurieren und anpassen können, finden Sie unter [Anpassen von Sprechweisen](speech-synthesis-markup.md#adjust-speaking-styles).

> [!TIP]
> In Sprachsyntheseanforderungen kann weiterhin die vollständige Dienstnamenzuordnung wie etwa „Microsoft Server Speech Text to Speech Voice (en-US, AriaNeural)“ verwendet werden.

### <a name="standard-voices"></a>Standardstimmen

Es stehen mehr als 75 Standardstimmen in mehr als 45 Sprachen und Gebietsschemas zur Verfügung, mit denen Sie Text in Sprache umwandeln können. Weitere Informationen zur regionalen Verfügbarkeit finden Sie unter [Regionen](regions.md#standard-and-neural-voices).

| Gebietsschema | Sprache | Geschlecht | Name der Stimme |
|--|--|--|--|
| `ar-EG` | Arabisch (Arabisch) | Female | `ar-EG-Hoda` |
| `ar-SA` | Arabisch (Saudi-Arabien) | Male | `ar-SA-Naayf` |
| `bg-BG` | Bulgarisch (Bulgarien) | Male | `bg-BG-Ivan` |
| `ca-ES` | Katalanisch (Spanien) | Female | `ca-ES-HerenaRUS` |
| `cs-CZ` | Tschechisch (Tschechische Republik) | Male | `cs-CZ-Jakub` |
| `da-DK` | Dänisch (Dänemark) | Female | `da-DK-HelleRUS` |
| `de-AT` | Deutsch (Österreich) | Male | `de-AT-Michael` |
| `de-CH` | Deutsch (Schweiz) | Male | `de-CH-Karsten` |
| `de-DE` | Deutsch (Deutschland) | Female | `de-DE-HeddaRUS` |
| `de-DE` | Deutsch (Deutschland) | Male | `de-DE-Stefan` |
| `el-GR` | Griechisch (Griechenland) | Male | `el-GR-Stefanos` |
| `en-AU` | Englisch (Australien) | Female | `en-AU-Catherine` |
| `en-AU` | Englisch (Australien) | Female | `en-AU-HayleyRUS` |
| `en-CA` | Englisch (Kanada) | Female | `en-CA-HeatherRUS` |
| `en-CA` | Englisch (Kanada) | Female | `en-CA-Linda` |
| `en-GB` | Walisisch (Großbritannien) | Male | `en-GB-George` |
| `en-GB` | Walisisch (Großbritannien) | Female | `en-GB-HazelRUS` |
| `en-GB` | Walisisch (Großbritannien) | Female | `en-GB-Susan` |
| `en-IE` | Englisch (Irland) | Male | `en-IE-Sean` |
| `en-IN` | Englisch (Indien) | Female | `en-IN-Heera` |
| `en-IN` | Englisch (Indien) | Female | `en-IN-PriyaRUS` |
| `en-IN` | Englisch (Indien) | Male | `en-IN-Ravi` |
| `en-US` | Englisch (USA) | Male | `en-US-BenjaminRUS` |
| `en-US` | Englisch (USA) | Male | `en-US-GuyRUS` |
| `en-US` | Englisch (USA) | Female | `en-US-JessaRUS` |
| `en-US` | Englisch (USA) | Female | `en-US-ZiraRUS` |
| `es-ES` | Spanisch (Spanien) | Female | `es-ES-HelenaRUS` |
| `es-ES` | Spanisch (Spanien) | Female | `es-ES-Laura` |
| `es-ES` | Spanisch (Spanien) | Male | `es-ES-Pablo` |
| `es-MX` | Spanisch (Mexiko) | Female | `es-MX-HildaRUS` |
| `es-MX` | Spanisch (Mexiko) | Male | `es-MX-Raul` |
| `fi-FI` | Finnisch (Finnland) | Female | `fi-FI-HeidiRUS` |
| `fr-CA` | Französisch (Kanada) | Female | `fr-CA-Caroline` |
| `fr-CA` | Französisch (Kanada) | Female | `fr-CA-HarmonieRUS` |
| `fr-CH` | Französisch (Schweiz) | Male | `fr-CH-Guillaume` |
| `fr-FR` | Französisch (Frankreich) | Female | `fr-FR-HortenseRUS` |
| `fr-FR` | Französisch (Frankreich) | Female | `fr-FR-Julie` |
| `fr-FR` | Französisch (Frankreich) | Male | `fr-FR-Paul` |
| `he-IL` | Hebräisch (Israel) | Male | `he-IL-Asaf` |
| `hi-IN` | Hindi (Indien) | Male | `hi-IN-Hemant` |
| `hi-IN` | Hindi (Indien) | Female | `hi-IN-Kalpana` |
| `hr-HR` | Kroatisch (Kroatien) | Male | `hr-HR-Matej` |
| `hu-HU` | Ungarisch (Ungarn) | Male | `hu-HU-Szabolcs` |
| `id-ID` | Indonesisch (Indonesien) | Male | `id-ID-Andika` |
| `it-IT` | Italienisch (Italien) | Male | `it-IT-Cosimo` |
| `it-IT` | Italienisch (Italien) | Female | `it-IT-LuciaRUS` |
| `ja-JP` | Japanisch (Japan) | Female | `ja-JP-Ayumi` |
| `ja-JP` | Japanisch (Japan) | Female | `ja-JP-HarukaRUS` |
| `ja-JP` | Japanisch (Japan) | Male | `ja-JP-Ichiro` |
| `ko-KR` | Koreanisch (Korea) | Female | `ko-KR-HeamiRUS` |
| `ms-MY` | Malaiisch (Malaysia) | Male | `ms-MY-Rizwan` |
| `nb-NO` | Norwegisch, Bokmål (Norwegen) | Female | `nb-NO-HuldaRUS` |
| `nl-NL` | Niederländisch (Niederlande) | Female | `nl-NL-HannaRUS` |
| `pl-PL` | Polnisch (Polen) | Female | `pl-PL-PaulinaRUS` |
| `pt-BR` | Portugiesisch (Brasilien) | Male | `pt-BR-Daniel` |
| `pt-BR` | Portugiesisch (Brasilien) | Female | `pt-BR-HeloisaRUS` |
| `pt-PT` | Portugiesisch (Portugal) | Female | `pt-PT-HeliaRUS` |
| `ro-RO` | Rumänisch (Rumänien) | Male | `ro-RO-Andrei` |
| `ru-RU` | Russisch (Russische Föderation) | Female | `ru-RU-EkaterinaRUS` |
| `ru-RU` | Russisch (Russische Föderation) | Female | `ru-RU-Irina` |
| `ru-RU` | Russisch (Russische Föderation) | Male | `ru-RU-Pavel` |
| `sk-SK` | Slowakisch (Slowakei) | Male | `sk-SK-Filip` |
| `sl-SI` | Slowenisch (Slowenien) | Male | `sl-SI-Lado` |
| `sv-SE` | Schwedisch (Schweden) | Female | `sv-SE-HedvigRUS` |
| `ta-IN` | Tamil (Indien) | Male | `ta-IN-Valluvar` |
| `te-IN` | Telugu (Indien) | Female | `te-IN-Chitra` |
| `th-TH` | Thai (Thailand) | Male | `th-TH-Pattara` |
| `tr-TR` | Türkisch (Türkei) | Female | `tr-TR-SedaRUS` |
| `vi-VN` | Vietnamesisch (Vietnam) | Male | `vi-VN-An` |
| `zh-CN` | Mandarin (Chinesisch, vereinfacht, China) | Female | `zh-CN-HuihuiRUS` |
| `zh-CN` | Mandarin (Chinesisch, vereinfacht, China) | Male | `zh-CN-Kangkang` |
| `zh-CN` | Mandarin (Chinesisch, vereinfacht, China) | Female | `zh-CN-Yaoyao` |
| `zh-HK` | Kantonesisch (Chinesisch, traditionell, Hongkong) | Male | `zh-HK-Danny` |
| `zh-HK` | Kantonesisch (Chinesisch, traditionell, Hongkong) | Female | `zh-HK-TracyRUS` |
| `zh-TW` | Mandarin (Chinesisch, traditionell, Taiwan) | Female | `zh-TW-HanHanRUS` |
| `zh-TW` | Mandarin (Chinesisch, traditionell, Taiwan) | Female | `zh-TW-Yating` |
| `zh-TW` | Mandarin (Chinesisch, traditionell, Taiwan) | Male | `zh-TW-Zhiwei` |

> [!IMPORTANT]
> Die Stimme `en-US-Jessa` wurde in `en-US-Aria` geändert. Wenn Sie zuvor „Jessa“ verwendet haben, wechseln Sie zu „Aria“.

> [!TIP]
> In Sprachsyntheseanforderungen kann weiterhin die vollständige Dienstnamenzuordnung wie etwa „Microsoft Server Speech Text to Speech Voice (en-US, AriaRUS)“ verwendet werden.

### <a name="customization"></a>Anpassung

Die Sprachanpassung ist für `de-DE`, `en-GB`, `en-IN`, `en-US`, `es-MX`, `fr-FR`, `it-IT`, `pt-BR`und `zh-CN` verfügbar. Wählen Sie das richtige Gebietsschema, das den Trainingsdaten entspricht, die Sie für das Training eines benutzerdefinierten Sprachmodells benötigen. Wenn die von Ihnen aufgenommenen Daten beispielsweise auf Englisch mit britischem Akzent gesprochen werden, wählen Sie `en-GB`.

> [!NOTE]
> Wir unterstützen kein zweisprachiges Modelltraining in Custom Voice, mit Ausnahme des zweisprachigen Chinesisch-Englisch. Wählen Sie „Chinesisch-Englisch zweisprachig“, wenn Sie eine chinesische Stimme trainieren möchten, die auch Englisch sprechen kann. Das Sprachtraining in allen Gebietsschemas beginnt mit einem Datenset mit mehr als 2.000 Äußerungen, mit Ausnahme von `en-US` und `zh-CN`, bei denen Sie mit einer beliebigen Größe von Trainingsdaten beginnen können.

## <a name="speech-translation"></a>Sprachübersetzung

Die **Sprachübersetzungs** -API unterstützt verschiedene Sprachen für die Übersetzung von Sprache in Sprache und Sprache in Text. Die Ausgangssprache muss immer eine der Sprachen aus der Sprachentabelle für die Spracherkennung sein. Welche Zielsprachen verfügbar sind, richtet sich danach, ob es sich beim Übersetzungsziel um Sprache oder Text handelt. Sie können Spracheingaben in mehr als [60 Sprachen](https://www.microsoft.com/translator/business/languages/) übersetzen. Eine Teilmenge der Sprachen steht für die [Sprachsynthese](language-support.md#text-languages) zur Verfügung.

### <a name="text-languages"></a>Sprachen für Textausgabe

| Sprache für Textausgabe           | Sprachcode |
|:------------------------|:-------------:|
| Afrikaans               | `af`          |
| Arabisch                  | `ar`          |
| Bengalisch                  | `bn`          |
| Bosnisch (Lateinisch)         | `bs`          |
| Bulgarisch               | `bg`          |
| Chinesisch (traditionell) | `yue`         |
| Katalanisch                 | `ca`          |
| Chinesisch (vereinfacht)      | `zh-Hans`     |
| Chinesisch (traditionell)     | `zh-Hant`     |
| Kroatisch                | `hr`          |
| Tschechisch                   | `cs`          |
| Dänisch                  | `da`          |
| Niederländisch                   | `nl`          |
| Englisch                 | `en`          |
| Estnisch                | `et`          |
| Fidschi                  | `fj`          |
| Filipino                | `fil`         |
| Finnisch                 | `fi`          |
| Französisch                  | `fr`          |
| Deutsch                  | `de`          |
| Griechisch                   | `el`          |
| Gujarati                | `gu`          |
| Haitianisches Kreolisch          | `ht`          |
| Hebräisch                  | `he`          |
| Hindi                   | `hi`          |
| Hmong Daw               | `mww`         |
| Ungarisch               | `hu`          |
| Indonesisch              | `id`          |
| Irisch                   | `ga`          |
| Italienisch                 | `it`          |
| Japanisch                | `ja`          |
| Kannada                 | `kn`          |
| Suaheli               | `sw`          |
| Klingonisch                 | `tlh-Latn`    |
| Klingonisch (plqaD)         | `tlh-Piqd`    |
| Koreanisch                  | `ko`          |
| Lettisch                 | `lv`          |
| Litauisch              | `lt`          |
| Madagassisch                | `mg`          |
| Malaiisch                   | `ms`          |
| Malayalam               | `ml`          |
| Maltesisch                 | `mt`          |
| Maori                   | `mi`          |
| Marathi                 | `mr`          |
| Norwegisch               | `nb`          |
| Persisch                 | `fa`          |
| Polnisch                  | `pl`          |
| Portugiesisch (Brasilien)     | `pt-br`       |
| Portugiesisch (Portugal)   | `pt-pt`       |
| Pandschabi                 | `pa`          |
| Queretaro-Otomi         | `otq`         |
| Rumänisch                | `ro`          |
| Russisch                 | `ru`          |
| Samoanisch                  | `sm`          |
| Serbisch (Kyrillisch)      | `sr-Cyrl`     |
| Serbisch (Lateinisch)         | `sr-Latn`     |
| Slowakisch                  | `sk`          |
| Slowenisch               | `sl`          |
| Spanisch                 | `es`          |
| Schwedisch                 | `sv`          |
| Tahitisch                | `ty`          |
| Tamilisch                   | `ta`          |
| Telugu                  | `te`          |
| Thailändisch                    | `th`          |
| Tongaisch                  | `to`          |
| Türkisch                 | `tr`          |
| Ukrainisch               | `uk`          |
| Urdu                    | `ur`          |
| Vietnamesisch              | `vi`          |
| Walisisch                   | `cy`          |
| Yukatekisches Maya            | `yua`         |

## <a name="speaker-recognition"></a>Sprechererkennung

In der folgenden Tabelle finden Sie die unterstützten Sprachen für die verschiedenen Sprechererkennungs-APIs. Weitere Informationen zur Sprechererkennung finden Sie in der [Übersicht](speaker-recognition-overview.md).

| Gebietsschema | Sprache | Textabhängige Überprüfung | Textunabhängige Überprüfung | Textunabhängige Identifikation |
|----|----|----|----|----|
| de-DE | Englisch (USA) | ja | ja | ja |
|zh-CN    |Chinesisch (Mandarin, vereinfacht)|    –|    ja|    ja|
|de-DE    |Deutsch (Deutschland)    |–    |ja    |ja|
|en-GB    |English (UK)    |–    |ja    |ja|
|fr-FR    |Französisch (Frankreich)    |–    |ja    |ja|
|en-AU    |Englisch (Australien)    |–    |ja    |ja|
|en-CA    |Englisch (Kanada)    |–|    ja|    ja|
|fr-CA    |Französisch (Kanada)    |–    |ja|    ja|
|it-IT    |Italienisch|    –    |ja|    ja|
|es-ES|    Spanisch (Spanien)    |–    |ja|    ja|
|es-MX    |Spanisch (Mexiko)    |–|    ja|    ja|
|ja-JP|    Japanisch    |–    |ja    |ja|
|pt-BR|    Portugiesisch (Brasilien)|    –|    ja|    ja|

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen Sie ein kostenloses Azure-Konto.](https://azure.microsoft.com/free/cognitive-services/)
* [Erkennen von Sprache in C#](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-chsarp)
