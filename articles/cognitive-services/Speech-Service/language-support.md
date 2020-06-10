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
ms.openlocfilehash: 43476f3e3f125bc338ba25e8cce0e96baa76262b
ms.sourcegitcommit: ce44069e729fce0cf67c8f3c0c932342c350d890
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84636103"
---
# <a name="language-and-voice-support-for-the-speech-service"></a>Sprach- und Stimmunterstützung für den Speech-Dienst

Die Sprachunterstützung ist abhängig von der Funktion des Speech-Diensts. In den folgenden Tabellen ist die Sprachunterstützung für die Dienstangebote [Spracherkennung](#speech-to-text), [Text-zu-Sprache](#text-to-speech), und [Sprachübersetzung](#speech-translation) zusammengefasst.

## <a name="speech-to-text"></a>Spracherkennung

Sowohl das Microsoft Speech SDK als auch die REST-API unterstützen die folgenden Sprachen (Gebietsschemas). 

Um die Genauigkeit zu erhöhen, wird die Anpassung für eine Teilmenge der Sprachen durch das Hochladen von **Audio und menschenmarkierten Transkripten** oder **zugehörigem Text (Sätze)** angeboten. Weitere Informationen zur Anpassung finden Sie unter [Was ist Custom Speech?](how-to-custom-speech.md).

Weitere Informationen zur Verbesserung der Aussprache finden Sie unter [Verbessern der Custom Speech-Genauigkeit](how-to-custom-speech-improve-accuracy.md#add-new-words-with-pronunciation).

<!--
To get the AM and ML bits:
https://westus.cris.ai/swagger/ui/index#/Custom%20Speech%20models%3A/GetSupportedLocalesForModels

To get pronunciation bits:
https://cris.ai -> Click on Adaptation Data -> scroll down to section "Pronunciation Datasets" -> Click on Import -> Locale: the list of locales there correspond to the supported locales
-->

| Gebietsschema  | Sprache                          | Unterstützt | Anpassungen                                    |
|---------|-----------------------------------|-----------|---------------------------------------------------|
| `ar-AE` | Arabisch (VAE)                      | Ja       | Nein                                                |
| `ar-BH` | Arabisch (Bahrain), modernes Hocharabisch | Ja       | Sprachmodell                                    |
| `ar-EG` | Arabisch (Ägypten)                    | Ja       | Sprachmodell                                    |
| `ar-IL` | Arabisch (Israel)                   | Ja       | Nein                                                |
| `ar-JO` | Arabisch (Jordanien)                   | Ja       | Nein                                                |
| `ar-KW` | Arabisch (Kuwait)                   | Ja       | Nein                                                |
| `ar-LB` | Arabisch (Libanon)                  | Ja       | Nein                                                |
| `ar-PS` | Arabisch (Palästina)                | Ja       | Nein                                                |
| `ar-QA` | Arabisch (Katar)                    | Ja       | Nein                                                |
| `ar-SA` | Arabisch (Saudi-Arabien)             | Ja       | Nein                                                |
| `ar-SY` | Arabisch (Syrien)                    | Ja       | Sprachmodell                                    |
| `ca-ES` | Katalanisch                           | Ja       | Sprachmodell                                    |
| `da-DK` | Dänisch (Dänemark)                  | Ja       | Sprachmodell                                    |
| `de-DE` | Deutsch (Deutschland)                  | Ja       | Akustische Modell<br>Sprachmodell<br>Aussprache |
| `en-AU` | Englisch (Australien)               | Ja       | Akustische Modell<br>Sprachmodell                  |
| `en-CA` | Englisch (Kanada)                  | Ja       | Akustische Modell<br>Sprachmodell                  |
| `en-GB` | Walisisch (Großbritannien)          | Ja       | Akustische Modell<br>Sprachmodell<br>Aussprache |
| `en-IN` | Englisch (Indien)                   | Ja       | Akustische Modell<br>Sprachmodell                  |
| `en-NZ` | Englisch (Neuseeland)             | Ja       | Akustische Modell<br>Sprachmodell                  |
| `en-US` | Englisch (USA)           | Ja       | Akustische Modell<br>Sprachmodell<br>Aussprache |
| `es-ES` | Spanisch (Spanien)                   | Ja       | Akustische Modell<br>Sprachmodell                  |
| `es-MX` | Spanisch (Mexiko)                  | Ja       | Akustische Modell<br>Sprachmodell                  |
| `fi-FI` | Finnisch (Finnland)                 | Ja       | Sprachmodell                                    |
| `fr-CA` | Französisch (Kanada)                   | Ja       | Akustische Modell<br>Sprachmodell                  |
| `fr-FR` | Französisch (Frankreich)                   | Ja       | Akustische Modell<br>Sprachmodell<br>Aussprache |
| `gu-IN` | Gujarati (Indien)                 | Ja       | Sprachmodell                                    |
| `hi-IN` | Hindi (Indien)                     | Ja       | Akustische Modell<br>Sprachmodell                  |
| `it-IT` | Italienisch (Italien)                   | Ja       | Akustische Modell<br>Sprachmodell<br>Aussprache |
| `ja-JP` | Japanisch (Japan)                  | Ja       | Sprachmodell                                    |
| `ko-KR` | Koreanisch (Korea)                    | Ja       | Sprachmodell                                    |
| `mr-IN` | Marathi (Indien)                   | Ja       | Sprachmodell                                    |
| `nb-NO` | Norwegisch, Bokmål (Norwegen)       | Ja       | Sprachmodell                                    |
| `nl-NL` | Niederländisch (Niederlande)               | Ja       | Sprachmodell                                    |
| `pl-PL` | Polnisch (Polen)                   | Ja       | Sprachmodell                                    |
| `pt-BR` | Portugiesisch (Brasilien)               | Ja       | Akustische Modell<br>Sprachmodell<br>Aussprache |
| `pt-PT` | Portugiesisch (Portugal)             | Ja       | Sprachmodell                                    |
| `ru-RU` | Russisch (Russische Föderation)                  | Ja       | Akustische Modell<br>Sprachmodell                  |
| `sv-SE` | Schwedisch (Schweden)                  | Ja       | Sprachmodell                                    |
| `ta-IN` | Tamil (Indien)                     | Ja       | Sprachmodell                                    |
| `te-IN` | Telugu (Indien)                    | Ja       | Sprachmodell                                    |
| `th-TH` | Thai (Thailand)                   | Ja       | Nein                                                |
| `tr-TR` | Türkisch (Türkei)                  | Ja       | Sprachmodell                                    |
| `zh-CN` | Chinesisch (Mandarin, vereinfacht)    | Ja       | Akustische Modell<br>Sprachmodell                  |
| `zh-HK` | Chinesisch (Kantonesisch, traditionell)  | Ja       | Sprachmodell                                    |
| `zh-TW` | Chinesisch (Taiwanesisch, Mandarin)      | Ja       | Sprachmodell                                    |

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
| `de-DE` | Deutsch (Deutschland)                | Female | „de-DE-KatjaNeural"      | Allgemein |
| `en-AU` | Englisch (Australien)             | Female | „en-AU-NatashaNeural“    | Allgemein |
| `en-CA` | Englisch (Kanada)                | Female | „en-CA-ClaraNeural“      | Allgemein |
| `en-GB` | English (UK)                    | Female | „en-GB-LibbyNeural“      | Allgemein |
|         |                                 | Female | „en-GB-MiaNeural“        | Allgemein |
| `en-US` | Englisch (USA)                    | Female | „en-US-AriaNeural“       | Allgemein, mehrere Stimmstile verfügbar |
|         |                                 | Male   | "en-US-GuyNeural"        | Allgemein |
| `es-ES` | Spanisch (Spanien)                 | Female | „es-ES-ElviraNeural“     | Allgemein |
| `es-MX` | Spanisch (Mexiko)                | Female | „es-MX-DaliaNeural“      | Allgemein |
| `fr-CA` | Französisch (Kanada)                 | Female | „fr-CA-SylvieNeural“     | Allgemein |
| `fr-FR` | Französisch (Frankreich)                 | Female | „fr-FR-DeniseNeural“     | Allgemein |
| `it-IT` | Italienisch (Italien)                 | Female | "it-IT-ElsaNeural"       | Allgemein |
| `ja-JP` | Japanisch                        | Female | „ja-JP-NanamiNeural“     | Allgemein |
| `ko-KR` | Koreanisch                          | Female | „ko-KR-SunHiNeural“      | Allgemein |
| `nb-NO` | Norwegisch                       | Female | „nb-NO-IselinNeural“     | Allgemein |
| `pt-BR` | Portugiesisch (Brasilien)             | Female | „pt-BR-FranciscaNeural“  | Allgemein |
| `tr-TR` | Türkisch                         | Female | „tr-TR-EmelNeural“       | Allgemein |
| `zh-CN` | Chinesisch (Mandarin, vereinfacht)  | Female | „zh-CN-XiaoxiaoNeural"   | Allgemein, mehrere Stimmstile verfügbar |
|         |                                 | Female | „zh-CN-XiaoyouNeural“    | Kinderstimme, optimiert für das Erzählen von Geschichten |
|         |                                 | Male   | „zh-CN-YunyangNeural“    | Optimiert für das Vorlesen von Nachrichten, mehrere Stimmstile verfügbar |
|         |                                 | Male   | „zh-CN-YunyeNeural“      | Optimiert für das Erzählen von Geschichten |

> [!IMPORTANT]
> Die Stimme `en-US-JessaNeural` wurde in `en-US-AriaNeural` geändert. Wenn Sie zuvor „Jessa“ verwendet haben, wechseln Sie zu „Aria“.

Informationen dazu, wie Sie neuronale Stimmen konfigurieren und anpassen können, finden Sie unter [Anpassen von Sprechweisen](speech-synthesis-markup.md#adjust-speaking-styles).

> [!TIP]
> In Sprachsyntheseanforderungen kann weiterhin die vollständige Dienstnamenzuordnung wie etwa „Microsoft Server Speech Text to Speech Voice (en-US, AriaNeural)“ verwendet werden.

### <a name="standard-voices"></a>Standardstimmen

Es stehen mehr als 75 Standardstimmen in mehr als 45 Sprachen und Gebietsschemas zur Verfügung, mit denen Sie Text in Sprache umwandeln können. Weitere Informationen zur regionalen Verfügbarkeit finden Sie unter [Regionen](regions.md#standard-and-neural-voices).

| Gebietsschema | Sprache | Geschlecht | Name der Stimme |
|--|--|--|--|
| <sup>1</sup>`ar-EG` | Arabisch (Ägypten) | Female | "ar-EG-Hoda" |
| `ar-SA` | Arabisch (Saudi-Arabien) | Male | "ar-SA-Naayf" |
| `bg-BG` | Bulgarisch | Male |  "bg-BG-Ivan" |
| `ca-ES` | Katalanisch | Female |  "ca-ES-HerenaRUS" |
| `cs-CZ` | Tschechisch | Male | "cs-CZ-Jakub" |
| `da-DK` | Dänisch | Female |  "da-DK-HelleRUS" |
| `de-AT` | Deutsch (Österreich) | Male | "de-AT-Michael" |
| `de-CH` | Deutsch (Schweiz) | Male |  "de-CH-Karsten" |
| `de-DE` | Deutsch (Deutschland) | Female |  "de-DE-Hedda" |
|  |  | Female | "de-DE-HeddaRUS" |
|  |  | Male |  "de-DE-Stefan-Apollo" |
| `el-GR` | Griechisch | Male | "el-GR-Stefanos" |
| `en-AU` | Englisch (Australien) | Female |  "en-AU-Catherine" |
|  |  | Female |  "en-AU-HayleyRUS" |
| `en-CA` | Englisch (Kanada) | Female |  "en-CA-Linda" |
|  |  | Female |  "en-CA-HeatherRUS" |
| `en-GB` | English (UK) | Female |  "en-GB-Susan-Apollo" |
|  |  | Female |  "en-GB-HazelRUS" |
|  |  | Male |  "en-GB-George-Apollo" |
| `en-IE` | Englisch (Irland) | Male | "en-IE-Sean" |
| `en-IN` | Englisch (Indien) | Female | "en-IN-Heera-Apollo" |
|  |  | Female |  "en-IN-PriyaRUS" |
|  |  | Male |  "en-IN-Ravi-Apollo" |
| `en-US` | Englisch (USA) | Female |  "en-US-ZiraRUS" |
|  |  | Female | „en-US-AriaRUS“ |
|  |  | Male | "en-US-BenjaminRUS" |
|  |  | Male |  "en-US-Guy24kRUS" |
| `es-ES` | Spanisch (Spanien) | Female |  "es-ES-Laura-Apollo" |
|  |  | Female | "es-ES-HelenaRUS" |
|  |  | Male | "es-ES-Pablo-Apollo" |
| `es-MX` | Spanisch (Mexiko) | Female |  "es-MX-HildaRUS" |
|  |  | Male | "es-MX-Raul-Apollo" |
| `fi-FI` | Finnisch | Female | "fi-FI-HeidiRUS" |
| `fr-CA` | Französisch (Kanada) | Female | "fr-CA-Caroline" |
|  |  | Female | "fr-CA-HarmonieRUS" |
| `fr-CH` | Französisch (Schweiz) | Male | "fr-CH-Guillaume" |
| `fr-FR` | Französisch (Frankreich) | Female |  "fr-FR-Julie-Apollo" |
|  |  | Female |"fr-FR-HortenseRUS" |
|  |  | Male |  "fr-FR-Paul-Apollo" |
| `he-IL` | Hebräisch (Israel) | Male |  "he-IL-Asaf" |
| `hi-IN` | Hindi (Indien) | Female | "hi-IN-Kalpana-Apollo" |
|  |  | Female |  "hi-IN-Kalpana" |
|  |  | Male |  "hi-IN-Hemant" |
| `hr-HR` | Kroatisch | Male | "hr-HR-Matej" |
| `hu-HU` | Ungarisch | Male |  "hu-HU-Szabolcs" |
| `id-ID` | Indonesisch | Male | "id-ID-Andika" |
| `it-IT` | Italienisch | Male |  "it-IT-Cosimo-Apollo" |
|  |  | Female |  "it-IT-LuciaRUS" |
| `ja-JP` | Japanisch | Female |  "ja-JP-Ayumi-Apollo" |
|  |  | Male | "ja-JP-Ichiro-Apollo" |
|  |  | Female |  "ja-JP-HarukaRUS" |
| `ko-KR` | Koreanisch | Female | "ko-KR-HeamiRUS" |
| `ms-MY` | Malaiisch | Male |  "ms-MY-Rizwan" |
| `nb-NO` | Norwegisch | Female |  "nb-NO-HuldaRUS" |
| `nl-NL` | Niederländisch | Female |  "nl-NL-HannaRUS" |
| `pl-PL` | Polnisch | Female |  "pl-PL-PaulinaRUS" |
| `pt-BR` | Portugiesisch (Brasilien) | Female | "pt-BR-HeloisaRUS" |
|  |  | Male |  "pt-BR-Daniel-Apollo" |
| `pt-PT` | Portugiesisch (Portugal) | Female | "pt-PT-HeliaRUS" |
| `ro-RO` | Rumänisch | Male | "ro-RO-Andrei" |
| `ru-RU` | Russisch | Female |  "ru-RU-Irina-Apollo" |
|  |  | Male | "ru-RU-Pavel-Apollo" |
|  |  | Female |  ru-RU-EkaterinaRUS |
| `sk-SK` | Slowakisch | Male | "sk-SK-Filip" |
| `sl-SI` | Slowenisch | Male |  "sl-SI-Lado" |
| `sv-SE` | Schwedisch | Female | "sv-SE-HedvigRUS" |
| `ta-IN` | Tamil (Indien) | Male |  "ta-IN-Valluvar" |
| `te-IN` | Telugu (Indien) | Female |  "te-IN-Chitra" |
| `th-TH` | Thailändisch | Male |  "th-TH-Pattara" |
| `tr-TR` | Türkisch (Türkei) | Female | "tr-TR-SedaRUS" |
| `vi-VN` | Vietnamesisch | Male |  "vi-VN-An" |
| `zh-CN` | Chinesisch (Mandarin, vereinfacht) | Female |  "zh-CN-HuihuiRUS" |
|  |  | Female | "zh-CN-Yaoyao-Apollo" |
|  |  | Male | "zh-CN-Kangkang-Apollo" |
| `zh-HK` | Chinesisch (Kantonesisch, traditionell) | Female |  "zh-HK-Tracy-Apollo" |
|  |  | Female | "zh-HK-TracyRUS" |
|  |  | Male |  "zh-HK-Danny-Apollo" |
| `zh-TW` | Chinesisch (Taiwanesisch, Mandarin) | Female |  "zh-TW-Yating-Apollo" |
|  |  | Female | "zh-TW-HanHanRUS" |
|  |  | Male |  "zh-TW-Zhiwei-Apollo" |

**1** *ar-EG unterstützt modernes Hocharabisch (Modern Standard Arabic, MSA).*

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
| Klingonisch                 | `tlh`         |
| Klingonisch (plqaD)         | `tlh-Qaak`    |
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

## <a name="next-steps"></a>Nächste Schritte

* [Cognitive Services ausprobieren](https://azure.microsoft.com/try/cognitive-services/)
* [Erkennen von Sprache in C#](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-chsarp)
