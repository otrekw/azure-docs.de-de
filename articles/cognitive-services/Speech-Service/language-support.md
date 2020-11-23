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
ms.custom: references_regions
ms.openlocfilehash: 569808d8a3c84d7d5455ebcdca6d2701cc14db5c
ms.sourcegitcommit: c157b830430f9937a7fa7a3a6666dcb66caa338b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/17/2020
ms.locfileid: "94685527"
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

| Sprache                          | Gebietsschema (BCP-47) | Anpassungen                                   |
|-----------------------------------|--------|--------------------------------------------------|
|Arabisch (Bahrain), modernes Hocharabisch  |`ar-BH` | Sprachmodell                                   |
|Arabisch (Ägypten)                     |`ar-EG` | Sprachmodell                                   |
|Arabisch (Irak)                      |`ar-IQ` | Sprachmodell                                   |
|Arabisch (Jordanien)                    |`ar-JO` | Sprachmodell                                   |
|Arabisch (Kuwait)                    |`ar-KW` | Sprachmodell                                   |
|Arabisch (Libanon)                   |`ar-LB` | Sprachmodell                                   |
|Arabisch (Oman)                      |`ar-OM` | Sprachmodell                                   |
|Arabisch (Katar)                     |`ar-QA` | Sprachmodell                                   |
|Arabisch (Saudi-Arabien)              |`ar-SA` | Sprachmodell                                   |
|Arabisch (Syrien)                     |`ar-SY` | Sprachmodell                                   |
|Arabisch (Vereinigte Arabische Emirate)      |`ar-AE` | Sprachmodell                                   |
|Bulgarisch (Bulgarien)               |`bg-BG` | Sprachmodell                                   |
|Katalanisch (Spanien)                    |`ca-ES` | Sprachmodell                                   |
|Chinesisch (Kantonesisch, traditionell)   |`zh-HK` | Sprachmodell                                   |
|Chinesisch (Mandarin, vereinfacht)     |`zh-CN` | Akustische Modell<br>Sprachmodell                 |
|Chinesisch (Taiwanesisch, Mandarin)       |`zh-TW` | Sprachmodell                                   |
|Kroatisch (Kroatien)                 |`hr-HR` | Sprachmodell                                   |
|Tschechisch (Tschechische Republik)             |`cs-CZ` | Sprachmodell                                   |
|Dänisch (Dänemark)                   |`da-DK` | Sprachmodell                                   |
|Niederländisch (Niederlande)                |`nl-NL` | Sprachmodell                                   |
|Englisch (Australien)                |`en-AU` | Akustische Modell<br>Sprachmodell                 |
|Englisch (Kanada)                   |`en-CA` | Akustische Modell<br>Sprachmodell                 |
|Englisch (Hongkong)                |`en-HK` | Sprachmodell                                   |
|Englisch (Indien)                    |`en-IN` | Akustische Modell<br>Sprachmodell                 |
|Englisch (Irland)                  |`en-IE` | Sprachmodell                                   |
|Englisch (Neuseeland)              |`en-NZ` | Akustische Modell<br>Sprachmodell                 |
|Englisch (Philippinen)              |`en-PH` | Sprachmodell                                   |
|Englisch (Singapur)                |`en-SG` | Sprachmodell                                   |
|Englisch (Südafrika)             |`en-ZA` | Sprachmodell                                   |
|Walisisch (Großbritannien)           |`en-GB` | Akustische Modell<br>Sprachmodell<br>Aussprache|
|Englisch (USA)            |`en-US` | Akustische Modell<br>Sprachmodell<br>Aussprache|
|Estnisch (Estland)                  |`et-EE` | Sprachmodell                                   |
|Finnisch (Finnland)                  |`fi-FI` | Sprachmodell                                   |
|Französisch (Kanada)                    |`fr-CA` | Akustische Modell<br>Sprachmodell                 |
|Französisch (Frankreich)                    |`fr-FR` | Akustische Modell<br>Sprachmodell<br>Aussprache|
|Deutsch (Deutschland)                   |`de-DE` | Akustische Modell<br>Sprachmodell<br>Aussprache|
|Griechisch (Griechenland)                     |`el-GR` | Sprachmodell                                   |
|Gujarati (Indien)                  |`gu-IN` | Sprachmodell                                   |
|Hindi (Indien)                      |`hi-IN` | Akustische Modell<br>Sprachmodell                 |
|Ungarisch (Ungarn)                |`hu-HU` | Sprachmodell                                   |
|Irisch (Irland)                     |`ga-IE` | Sprachmodell                                   |
|Italienisch (Italien)                    |`it-IT` | Akustische Modell<br>Sprachmodell<br>Aussprache|
|Japanisch (Japan)                   |`ja-JP` | Sprachmodell                                   |
|Koreanisch (Korea)                     |`ko-KR` | Sprachmodell                                   |
|Lettisch (Lettland)                   |`lv-LV` | Sprachmodell                                   |
|Litauisch (Litauen)             |`lt-LT` | Sprachmodell                                   |
|Maltesisch (Malta)                     |`mt-MT` | Sprachmodell                                   |
|Marathi (Indien)                    |`mr-IN` | Sprachmodell                                   |
|Norwegisch, Bokmål (Norwegen)         |`nb-NO` | Sprachmodell                                   |
|Polnisch (Polen)                    |`pl-PL` | Sprachmodell                                   |
|Portugiesisch (Brasilien)                |`pt-BR` | Akustische Modell<br>Sprachmodell<br>Aussprache|
|Portugiesisch (Portugal)              |`pt-PT` | Sprachmodell                                   |
|Rumänisch (Rumänien)                 |`ro-RO` | Sprachmodell                                   |
|Russisch (Russische Föderation)                   |`ru-RU` | Akustische Modell<br>Sprachmodell                 |
|Slowakisch (Slowakei)                  |`sk-SK` | Sprachmodell                                   |
|Slowenisch (Slowenien)               |`sl-SI` | Sprachmodell                                   |
|Spanisch (Argentinien)                |`es-AR` | Sprachmodell                                   |
|Spanisch (Bolivien)                  |`es-BO` | Sprachmodell                                   |
|Spanisch (Chile)                    |`es-CL` | Sprachmodell                                   |
|Spanisch (Kolumbien)                 |`es-CO` | Sprachmodell                                   |
|Spanisch (Costa Rica)               |`es-CR` | Sprachmodell                                   |
|Spanisch (Kuba)                     |`es-CU` | Sprachmodell                                   |
|Spanisch (Dominikanische Republik)       |`es-DO` | Sprachmodell                                   |
|Spanisch (Ecuador)                  |`es-EC` | Sprachmodell                                   |
|Spanisch (El Salvador)              |`es-SV` | Sprachmodell                                   |
|Spanisch (Guatemala)                |`es-GT` | Sprachmodell                                   |
|Spanisch (Honduras)                 |`es-HN` | Sprachmodell                                   |
|Spanisch (Mexiko)                   |`es-MX` | Akustische Modell<br>Sprachmodell                 |
|Spanisch (Nicaragua)                |`es-NI` | Sprachmodell                                   |
|Spanisch (Panama)                   |`es-PA` | Sprachmodell                                   |
|Spanisch (Paraguay)                 |`es-PY` | Sprachmodell                                   |
|Spanisch (Peru)                     |`es-PE` | Sprachmodell                                   |
|Spanisch (Puerto Rico)              |`es-PR` | Sprachmodell                                   |
|Spanisch (Spanien)                    |`es-ES` | Akustische Modell<br>Sprachmodell                 |
|Spanisch (Uruguay)                  |`es-UY` | Sprachmodell                                   |
|Spanisch (USA)                      |`es-US` | Sprachmodell                                   |
|Spanisch (Venezuela)                |`es-VE` | Sprachmodell                                   |
|Schwedisch (Schweden)                   |`sv-SE` | Sprachmodell                                   |
|Tamil (Indien)                      |`ta-IN` | Sprachmodell                                   |
|Telugu (Indien)                     |`te-IN` | Sprachmodell                                   |
|Thai (Thailand)                    |`th-TH` | Sprachmodell                                   |
|Türkisch (Türkei)                   |`tr-TR` | Sprachmodell                                   |

## <a name="text-to-speech"></a>Text-zu-Sprache

Sowohl das Microsoft Speech SDK als auch die REST-API unterstützen diese Stimmen. Jede dieser Stimmen steht für eine bestimmte Sprache und einen bestimmten Dialekt und wird durch das Gebietsschema identifiziert. Über die [API „voices/list“](rest-text-to-speech.md#get-a-list-of-voices) können Sie auch eine vollständige Liste der Sprachen und Stimmen abrufen, die für die jeweilige Region/den jeweiligen Endpunkt unterstützt werden. 

> [!IMPORTANT]
> Die Preise variieren für Standardstimmen, benutzerdefinierte und neuronale Stimmen. Weitere Informationen finden Sie auf der Seite [Preise](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

### <a name="neural-voices"></a>Neuronale Stimmen

Die neuronale Sprachsynthese ist eine neue Art der Sprachsynthese mithilfe von Deep Neural Networks. Wenn Sie eine neuronale Stimme verwenden, kann die synthetisierte Sprache kaum von menschlichen Aufzeichnungen unterschieden werden.

Gestalten Sie mit neuronalen Stimmen Interaktionen mit Chatbots und Sprachassistenten noch natürlicher und einladender, wandeln Sie digitale Texte wie E-Books in Audiobooks um, und verpassen Sie Ihrem Navigationssystem im Auto ein Upgrade. Durch natürliche, menschenähnliche Intonation und klare Aussprache von Wörtern können neuronale Stimmen die Hörermüdung bei der Interaktion mit KI-Systemen erheblich verringern.

| Sprache | Gebietsschema | Geschlecht | Name der Stimme | Stilunterstützung |
|---|---|---|---|---|
| Arabisch (Ägypten) | `ar-EG` | Female | `ar-EG-SalmaNeural` | Allgemein |
| Arabisch (Saudi-Arabien) | `ar-SA` | Female | `ar-SA-ZariyahNeural` | Allgemein |
| Bulgarisch (Bulgarien) | `bg-BG` | Female | `bg-BG-KalinaNeural` <sup>Neu</sup> | Allgemein |
| Katalanisch (Spanien) | `ca-ES` | Female | `ca-ES-AlbaNeural` | Allgemein |
| Chinesisch (Kantonesisch, traditionell) | `zh-HK` | Female | `zh-HK-HiuGaaiNeural` | Allgemein |
| Chinesisch (Mandarin, vereinfacht) | `zh-CN` | Female | `zh-CN-XiaoxiaoNeural` | Allgemein, mehrere Stimmstile verfügbar |
| Chinesisch (Mandarin, vereinfacht) | `zh-CN` | Female | `zh-CN-XiaoyouNeural` | Kinderstimme, optimiert für das Erzählen von Geschichten |
| Chinesisch (Mandarin, vereinfacht) | `zh-CN` | Male   | `zh-CN-YunyangNeural` | Optimiert für die Ansage von Nachrichten,<br /> mehrere Stimmen und Stile verfügbar |
| Chinesisch (Mandarin, vereinfacht) | `zh-CN` | Male   | `zh-CN-YunyeNeural` | Optimiert für das Erzählen von Geschichten |
| Chinesisch (Taiwanesisch, Mandarin) | `zh-TW` | Female | `zh-TW-HsiaoYuNeural` | Allgemein |
| Kroatisch (Kroatien) | `hr-HR` | Female | `hr-HR-GabrijelaNeural` <sup>Neu</sup> | Allgemein |
| Tschechisch (Tschechische Republik) | `cs-CZ` | Female | `cs-CZ-VlastaNeural` <sup>Neu</sup>    | Allgemein |
| Dänisch (Dänemark) | `da-DK` | Female | `da-DK-ChristelNeural` | Allgemein |
| Niederländisch (Niederlande) | `nl-NL` | Female | `nl-NL-ColetteNeural` | Allgemein |
| Englisch (Australien) | `en-AU` | Female | `en-AU-NatashaNeural` | Allgemein |
| Englisch (Australien) | `en-AU` | Male   | `en-AU-WilliamNeural` <sup>Neu</sup>  | Allgemein |
| Englisch (Kanada) | `en-CA` | Female | `en-CA-ClaraNeural` | Allgemein |
| Englisch (Indien) | `en-IN` | Female | `en-IN-NeerjaNeural` | Allgemein |
| Englisch (Irland) | `en-IE` | Female | `en-IE-EmilyNeural` <sup>Neu</sup> | Allgemein |
| Walisisch (Großbritannien) | `en-GB` | Female | `en-GB-LibbyNeural` | Allgemein |
| Walisisch (Großbritannien) | `en-GB` | Female | `en-GB-MiaNeural` | Allgemein |
| Walisisch (Großbritannien) | `en-GB` | Male | `en-GB-RyanNeural` <sup>Neu</sup> | Allgemein |
| Englisch (USA) | `en-US` | Female | `en-US-AriaNeural` | Allgemein, mehrere Stimmstile verfügbar |
| Englisch (USA) | `en-US` | Male | `en-US-GuyNeural` | Allgemein |
| Englisch (USA) | `en-US` | Female | `en-US-JennyNeural` <sup>Neu</sup> | Allgemein, mehrere Stimmstile verfügbar |
| Finnisch (Finnland) | `fi-FI` | Female | `fi-FI-NooraNeural` | Allgemein |
| Französisch (Kanada) | `fr-CA` | Female | `fr-CA-SylvieNeural` | Allgemein |
| Französisch (Kanada) | `fr-CA` | Male | `fr-CA-JeanNeural` <sup>Neu</sup> | Allgemein |
| Französisch (Frankreich) | `fr-FR` | Female | `fr-FR-DeniseNeural` | Allgemein |
| Französisch (Frankreich) | `fr-FR` | Male | `fr-FR-HenriNeural` <sup>Neu</sup> | Allgemein |
| Französisch (Schweiz) | `fr-CH` | Female | `fr-CH-ArianeNeural` <sup>Neu</sup> | Allgemein |
| Deutsch (Österreich) | `de-AT` | Female | `de-AT-IngridNeural` <sup>Neu</sup> | Allgemein |
| Deutsch (Deutschland) | `de-DE` | Female | `de-DE-KatjaNeural` | Allgemein |
| Deutsch (Deutschland) | `de-DE` | Male | `de-DE-ConradNeural` <sup>Neu</sup> | Allgemein |
| Deutsch (Schweiz) | `de-CH` | Female | `de-CH-LeniNeural` <sup>Neu</sup> | Allgemein |
| Griechisch (Griechenland) | `el-GR` | Female | `el-GR-AthinaNeural` <sup>Neu</sup> | Allgemein |
| Hebräisch (Israel) | `he-IL` | Female | `he-IL-HilaNeural` <sup>Neu</sup> | Allgemein |
| Hindi (Indien) | `hi-IN` | Female | `hi-IN-SwaraNeural` | Allgemein |
| Ungarisch (Ungarn) | `hu-HU` | Female | `hu-HU-NoemiNeural` <sup>Neu</sup> | Allgemein |
| Indonesisch (Indonesien) | `id-ID` | Male | `id-ID-ArdiNeural` <sup>Neu</sup> | Allgemein |
| Italienisch (Italien) | `it-IT` | Female | `it-IT-ElsaNeural` | Allgemein |
| Italienisch (Italien) | `it-IT` | Female | `it-IT-IsabellaNeural` <sup>Neu</sup> | Allgemein |
| Italienisch (Italien) | `it-IT` | Male | `it-IT-DiegoNeural` <sup>Neu</sup> | Allgemein |
| Japanisch (Japan) | `ja-JP` | Female | `ja-JP-NanamiNeural` | Allgemein |
| Japanisch (Japan) | `ja-JP` | Male | `ja-JP-KeitaNeural` <sup>Neu</sup> | Allgemein |
| Koreanisch (Korea) | `ko-KR` | Female | `ko-KR-SunHiNeural` | Allgemein |
| Koreanisch (Korea) | `ko-KR` | Male | `ko-KR-InJoonNeural` <sup>Neu</sup> | Allgemein |
| Malaiisch (Malaysia) | `ms-MY` | Female | `ms-MY-YasminNeural` <sup>Neu</sup> | Allgemein |
| Norwegisch, Bokmål (Norwegen) | `nb-NO` | Female | `nb-NO-IselinNeural` | Allgemein |
| Polnisch (Polen) | `pl-PL` | Female | `pl-PL-ZofiaNeural` | Allgemein |
| Portugiesisch (Brasilien) | `pt-BR` | Female | `pt-BR-FranciscaNeural` | Allgemein, mehrere Stimmstile verfügbar |
| Portugiesisch (Brasilien) | `pt-BR` | Male | `pt-BR-AntonioNeural` <sup>Neu</sup> | Allgemein |
| Portugiesisch (Portugal) | `pt-PT` | Female | `pt-PT-FernandaNeural` | Allgemein |
| Rumänisch (Rumänien) | `ro-RO` | Female | `ro-RO-AlinaNeural` <sup>Neu</sup> | Allgemein |
| Russisch (Russische Föderation) | `ru-RU` | Female | `ru-RU-DariyaNeural` | Allgemein |
| Slowakisch (Slowakei) | `sk-SK` | Female | `sk-SK-ViktoriaNeural` <sup>Neu</sup> | Allgemein |
| Slowenisch (Slowenien) | `sl-SI` | Female | `sl-SI-PetraNeural` <sup>Neu</sup> | Allgemein |
| Spanisch (Mexiko) | `es-MX` | Female | `es-MX-DaliaNeural` | Allgemein |
| Spanisch (Mexiko) | `es-MX` | Male | `es-MX-JorgeNeural` <sup>Neu</sup> | Allgemein |
| Spanisch (Spanien) | `es-ES` | Female | `es-ES-ElviraNeural` | Allgemein |
| Spanisch (Spanien) | `es-ES` | Male | `es-ES-AlvaroNeural` <sup>Neu</sup> | Allgemein |
| Schwedisch (Schweden) | `sv-SE` | Female | `sv-SE-HilleviNeural` | Allgemein |
| Tamil (Indien) | `ta-IN` | Female | `ta-IN-PallaviNeural` <sup>Neu</sup> | Allgemein |
| Telugu (Indien) | `te-IN` | Female | `te-IN-ShrutiNeural` <sup>Neu</sup> | Allgemein |
| Thai (Thailand) | `th-TH` | Female | `th-TH-AcharaNeural` | Allgemein |
| Thai (Thailand) | `th-TH` | Female | `th-TH-PremwadeeNeural` <sup>Neu</sup> | Allgemein |
| Türkisch (Türkei) | `tr-TR` | Female | `tr-TR-EmelNeural` | Allgemein |
| Vietnamesisch (Vietnam) | `vi-VN` <sup>Neu</sup> | Female | `vi-VN-HoaiMyNeural` | Allgemein|

#### <a name="neural-voices-in-preview"></a>Neuronale Stimmen in der Vorschau

Die folgenden neuronalen Stimmen sind in der öffentlichen Vorschau verfügbar. 

| Sprache                         | Gebietsschema  | Geschlecht | Name der Stimme                             | Stilunterstützung |
|----------------------------------|---------|--------|----------------------------------------|---------------|
| Chinesisch (Mandarin, vereinfacht) | `zh-CN` | Male   | `zh-CN-YunxiNeural` <sup>Neu</sup> | Allgemein, mehrere Stimmen und Stile verfügbar |
| Chinesisch (Mandarin, vereinfacht) | `zh-CN` | Female | `zh-CN-XiaohanNeural` <sup>Neu</sup> | Allgemein, mehrere Stimmen und Stile verfügbar |
| Chinesisch (Mandarin, vereinfacht) | `zh-CN` | Female | `zh-CN-XiaoxuanNeural` <sup>Neu</sup> | Allgemein, mehrere Rollen und Stile verfügbar |
| Chinesisch (Mandarin, vereinfacht) | `zh-CN` | Female | `zh-CN-XiaomoNeural` <sup>Neu</sup> | Allgemein, mehrere Rollen und Stile verfügbar |
| Chinesisch (Mandarin, vereinfacht) | `zh-CN` | Female | `zh-CN-XiaoruiNeural` <sup>Neu</sup> | Erwachsene Stimme, mehrere Stile verfügbar |
| Estnisch (Estland) | `et-EE` | Female | `et-EE-AnuNeural` <sup>Neu</sup> | Allgemein |
| Gälisch (Irland) | `ga-IE` | Female | `ga-IE-OrlaNeural` <sup>Neu</sup> | Allgemein |
| Litauisch (Litauen) | `lt-LT` | Female | `lt-LT-OnaNeural` <sup>Neu</sup> | Allgemein |
| Lettisch (Lettland) | `lv-LV` | Female | `lv-LV-EveritaNeural` <sup>Neu</sup> | Allgemein |
| Maltesisch (Malta) | `mt-MT` | Female | `mt-MT-GraceNeural` <sup>Neu</sup> | Allgemein |

> [!IMPORTANT]
> Stimmen in der öffentlichen Vorschau sind nur in drei Dienstregionen verfügbar: „USA, Osten“, „Europa, Westen“ und „Asien, Südosten“.

Weitere Informationen zur regionalen Verfügbarkeit finden Sie unter [Regionen](regions.md#standard-and-neural-voices).

Informationen dazu, wie Sie neuronale Stimmen konfigurieren und anpassen können (z. B. den Sprechstil), finden Sie unter [Anpassen von Sprechweisen](speech-synthesis-markup.md#adjust-speaking-styles).

> [!IMPORTANT]
> Die Stimme `en-US-JessaNeural` wurde in `en-US-AriaNeural` geändert. Wenn Sie zuvor „Jessa“ verwendet haben, wechseln Sie zu „Aria“.

> [!TIP]
> In Sprachsyntheseanforderungen kann weiterhin die vollständige Dienstnamenzuordnung wie etwa „Microsoft Server Speech Text to Speech Voice (en-US, AriaNeural)“ verwendet werden.

### <a name="standard-voices"></a>Standardstimmen

Es stehen mehr als 75 Standardstimmen in mehr als 45 Sprachen und Gebietsschemas zur Verfügung, mit denen Sie Text in Sprache umwandeln können. Weitere Informationen zur regionalen Verfügbarkeit finden Sie unter [Regionen](regions.md#standard-and-neural-voices).

| Sprache | Gebietsschema (BCP-47) | Geschlecht | Name der Stimme |
|--|--|--|--|
| Arabisch (Arabisch) | `ar-EG` | Female | `ar-EG-Hoda`|
| Arabisch (Saudi-Arabien) | `ar-SA` | Male | `ar-SA-Naayf`|
| Bulgarisch (Bulgarien) | `bg-BG` | Male | `bg-BG-Ivan`|
| Katalanisch (Spanien) | `ca-ES` | Female | `ca-ES-HerenaRUS`|
| Chinesisch (Kantonesisch, traditionell) | `zh-HK` | Male | `zh-HK-Danny`|
| Chinesisch (Kantonesisch, traditionell) | `zh-HK` | Female | `zh-HK-TracyRUS`|
| Chinesisch (Mandarin, vereinfacht) | `zh-CN` | Female | `zh-CN-HuihuiRUS`|
| Chinesisch (Mandarin, vereinfacht) | `zh-CN` | Male | `zh-CN-Kangkang`|
| Chinesisch (Mandarin, vereinfacht) | `zh-CN` | Female | `zh-CN-Yaoyao`|
| Chinesisch (Taiwanesisch, Mandarin) |  `zh-TW` | Female | `zh-TW-HanHanRUS`|
| Chinesisch (Taiwanesisch, Mandarin) |  `zh-TW` | Female | `zh-TW-Yating`|
| Chinesisch (Taiwanesisch, Mandarin) |  `zh-TW` | Male | `zh-TW-Zhiwei`|
| Kroatisch (Kroatien) | `hr-HR` | Male | `hr-HR-Matej`|
| Tschechisch (Tschechische Republik) | `cs-CZ` | Male | `cs-CZ-Jakub`|
| Dänisch (Dänemark) | `da-DK` | Female | `da-DK-HelleRUS`|
| Niederländisch (Niederlande) | `nl-NL` | Female | `nl-NL-HannaRUS`|
| Englisch (Australien) | `en-AU` | Female | `en-AU-Catherine`|
| Englisch (Australien) | `en-AU` | Female | `en-AU-HayleyRUS`|
| Englisch (Kanada) | `en-CA` | Female | `en-CA-HeatherRUS`|
| Englisch (Kanada) | `en-CA` | Female | `en-CA-Linda`|
| Englisch (Indien) | `en-IN` | Female | `en-IN-Heera`|
| Englisch (Indien) | `en-IN` | Female | `en-IN-PriyaRUS`|
| Englisch (Indien) | `en-IN` | Male | `en-IN-Ravi`|
| Englisch (Irland) | `en-IE` | Male | `en-IE-Sean`|
| Walisisch (Großbritannien) | `en-GB` | Male | `en-GB-George`|
| Walisisch (Großbritannien) | `en-GB` | Female | `en-GB-HazelRUS`|
| Walisisch (Großbritannien) | `en-GB` | Female | `en-GB-Susan`|
| Englisch (USA) | `en-US` | Male | `en-US-BenjaminRUS`|
| Englisch (USA) | `en-US` | Male | `en-US-GuyRUS`|
| Englisch (USA) | `en-US` | Female | `en-US-JessaRUS`|
| Englisch (USA) | `en-US` | Female | `en-US-ZiraRUS`|
| Finnisch (Finnland) | `fi-FI` | Female | `fi-FI-HeidiRUS`|
| Französisch (Kanada) | `fr-CA` | Female | `fr-CA-Caroline`|
| Französisch (Kanada) | `fr-CA` | Female | `fr-CA-HarmonieRUS`|
| Französisch (Frankreich) | `fr-FR` | Female | `fr-FR-HortenseRUS`|
| Französisch (Frankreich) | `fr-FR` | Female | `fr-FR-Julie`|
| Französisch (Frankreich) | `fr-FR` | Male | `fr-FR-Paul`|
| Französisch (Schweiz) | `fr-CH` | Male | `fr-CH-Guillaume`|
| Deutsch (Österreich) | `de-AT` | Male | `de-AT-Michael`|
| Deutsch (Deutschland) | `de-DE` | Female | `de-DE-HeddaRUS`|
| Deutsch (Deutschland) | `de-DE` | Male | `de-DE-Stefan`|
| Deutsch (Schweiz) | `de-CH` | Male | `de-CH-Karsten`|
| Griechisch (Griechenland) | `el-GR` | Male | `el-GR-Stefanos`|
| Hebräisch (Israel) | `he-IL` | Male | `he-IL-Asaf`|
| Hindi (Indien) | `hi-IN` | Male | `hi-IN-Hemant`|
| Hindi (Indien) | `hi-IN` | Female | `hi-IN-Kalpana`|
| Ungarisch (Ungarn) | `hu-HU` | Male | `hu-HU-Szabolcs`|
| Indonesisch (Indonesien) | `id-ID` | Male | `id-ID-Andika`|
| Italienisch (Italien) | `it-IT` | Male | `it-IT-Cosimo`|
| Italienisch (Italien) | `it-IT` | Female | `it-IT-LuciaRUS`|
| Japanisch (Japan) | `ja-JP` | Female | `ja-JP-Ayumi`|
| Japanisch (Japan) | `ja-JP` | Female | `ja-JP-HarukaRUS`|
| Japanisch (Japan) | `ja-JP` | Male | `ja-JP-Ichiro`|
| Koreanisch (Korea) | `ko-KR` | Female | `ko-KR-HeamiRUS`|
| Malaiisch (Malaysia) | `ms-MY` | Male | `ms-MY-Rizwan`|
| Norwegisch, Bokmål (Norwegen) | `nb-NO` | Female | `nb-NO-HuldaRUS`|
| Polnisch (Polen) | `pl-PL` | Female | `pl-PL-PaulinaRUS`|
| Portugiesisch (Brasilien) | `pt-BR` | Male | `pt-BR-Daniel`|
| Portugiesisch (Brasilien) | `pt-BR` | Female | `pt-BR-HeloisaRUS`|
| Portugiesisch (Portugal) | `pt-PT` | Female | `pt-PT-HeliaRUS`|
| Rumänisch (Rumänien) | `ro-RO` | Male | `ro-RO-Andrei`|
| Russisch (Russische Föderation) | `ru-RU` | Female | `ru-RU-EkaterinaRUS`|
| Russisch (Russische Föderation) | `ru-RU` | Female | `ru-RU-Irina`|
| Russisch (Russische Föderation) | `ru-RU` | Male | `ru-RU-Pavel`|
| Slowakisch (Slowakei) | `sk-SK` | Male | `sk-SK-Filip`|
| Slowenisch (Slowenien) | `sl-SI` | Male | `sl-SI-Lado`|
| Spanisch (Mexiko) | `es-MX` | Female | `es-MX-HildaRUS`|
| Spanisch (Mexiko) | `es-MX` | Male | `es-MX-Raul`|
| Spanisch (Spanien) | `es-ES` | Female | `es-ES-HelenaRUS`|
| Spanisch (Spanien) | `es-ES` | Female | `es-ES-Laura`|
| Spanisch (Spanien) | `es-ES` | Male | `es-ES-Pablo`|
| Schwedisch (Schweden) | `sv-SE` | Female | `sv-SE-HedvigRUS`|
| Tamil (Indien) | `ta-IN` | Male | `ta-IN-Valluvar`|
| Telugu (Indien) | `te-IN` | Female | `te-IN-Chitra`|
| Thai (Thailand) | `th-TH` | Male | `th-TH-Pattara`|
| Türkisch (Türkei) | `tr-TR` | Female | `tr-TR-SedaRUS`|
| Vietnamesisch (Vietnam) | `vi-VN` | Male | `vi-VN-An` |

> [!IMPORTANT]
> Die Stimme `en-US-Jessa` wurde in `en-US-Aria` geändert. Wenn Sie zuvor „Jessa“ verwendet haben, wechseln Sie zu „Aria“.

> [!TIP]
> In Sprachsyntheseanforderungen kann weiterhin die vollständige Dienstnamenzuordnung wie etwa „Microsoft Server Speech Text to Speech Voice (en-US, AriaRUS)“ verwendet werden.

### <a name="customization"></a>Anpassung

Die Sprachanpassung ist für `de-DE`, `en-GB`, `en-IN`, `en-US`, `es-MX`, `fr-FR`, `it-IT`, `pt-BR`und `zh-CN` verfügbar. Wählen Sie das richtige Gebietsschema, das den Trainingsdaten entspricht, die Sie für das Training eines benutzerdefinierten Sprachmodells benötigen. Wenn die von Ihnen aufgenommenen Daten beispielsweise auf Englisch mit britischem Akzent gesprochen werden, wählen Sie `en-GB`.

> [!NOTE]
> Wir unterstützen kein zweisprachiges Modelltraining in Custom Voice, mit Ausnahme des zweisprachigen Chinesisch-Englisch. Wählen Sie „Chinesisch-Englisch zweisprachig“, wenn Sie eine chinesische Stimme trainieren möchten, die auch Englisch sprechen kann. Das Sprachtraining in allen Gebietsschemas beginnt mit einem Datenset mit mehr als 2.000 Äußerungen, mit Ausnahme von `en-US` und `zh-CN`, bei denen Sie mit einer beliebigen Größe von Trainingsdaten beginnen können.

## <a name="speech-translation"></a>Sprachübersetzung

Die **Sprachübersetzungs**-API unterstützt verschiedene Sprachen für die Übersetzung von Sprache in Sprache und Sprache in Text. Die Ausgangssprache muss immer eine der Sprachen aus der Sprachentabelle für die Spracherkennung sein. Welche Zielsprachen verfügbar sind, richtet sich danach, ob es sich beim Übersetzungsziel um Sprache oder Text handelt. Sie können Spracheingaben in mehr als [60 Sprachen](https://www.microsoft.com/translator/business/languages/) übersetzen. Eine Teilmenge der Sprachen steht für die [Sprachsynthese](language-support.md#text-languages) zur Verfügung.

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

| Sprache | Gebietsschema (BCP-47) | Textabhängige Überprüfung | Textunabhängige Überprüfung | Textunabhängige Identifikation |
|----|----|----|----|----|
|Englisch (USA)  |  en-US  |  ja  |  ja  |  ja |
|Chinesisch (Mandarin, vereinfacht) | zh-CN     |     – |     ja |     ja|
|Englisch (Australien)     | en-AU     | –     | ja     | ja|
|Englisch (Kanada)     | en-CA     | – |     ja |     ja|
|English (UK)     | en-GB     | –     | ja     | ja|
|Französisch (Kanada)     | fr-CA     | –     | ja |     ja|
|Französisch (Frankreich)     | fr-FR     | –     | ja     | ja|
|Deutsch (Deutschland)     | de-DE     | –     | ja     | ja|
|Italienisch | it-IT     |     –     | ja |     ja|
|Japanisch     | ja-JP | –     | ja     | ja|
|Portugiesisch (Brasilien) | pt-BR |     – |     ja |     ja|
|Spanisch (Mexiko)     | es-MX     | – |     ja |     ja|
|Spanisch (Spanien)     | es-ES | –     | ja |     ja|

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen Sie ein kostenloses Azure-Konto.](https://azure.microsoft.com/free/cognitive-services/)
* [Erkennen von Sprache in C#](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-chsarp)
