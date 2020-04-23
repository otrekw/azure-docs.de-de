---
title: 'Anwendungseinstellungen: LUIS'
description: Anwendungseinstellungen für Language Understanding-Apps von Azure Cognitive Services werden in der App und im Portal gespeichert.
ms.topic: reference
ms.date: 04/14/2020
ms.openlocfilehash: 9e17736cd6ff5074a6eab76a6cf5bdb8acedc185
ms.sourcegitcommit: ea006cd8e62888271b2601d5ed4ec78fb40e8427
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/14/2020
ms.locfileid: "81382203"
---
# <a name="application-settings"></a>Anwendungseinstellungen

Diese Anwendungseinstellungen werden in der [exportierten](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c40) App gespeichert und mit den REST-APIs [aktualisiert](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/versions-update-application-version-settings). Wenn Sie die Einstellungen Ihrer App-Version ändern, wird der Trainingsstatus Ihrer App auf „Untrainiert“ zurückgesetzt.

Machen Sie sich mit den [Konzepten](luis-concept-utterance.md#utterance-normalization-for-diacritics-and-punctuation) von diakritischen Zeichen und Interpunktion vertraut.

|Einstellung|Standardwert|Notizen|
|--|--|--|
|NormalizePunctuation|True|Entfernt die Interpunktion.|
|NormalizeDiacritics|True|Entfernt diakritische Zeichen.|

## <a name="diacritics-normalization"></a>Normalisierung von diakritischen Zeichen

Aktivieren Sie die Äußerungsnormalisierung für diakritische Zeichen für Ihre LUIS-JSON-App-Datei im Parameter `settings`.

```JSON
"settings": [
    {"name": "NormalizeDiacritics", "value": "true"}
]
```

Anhand der folgenden Äußerungen wird veranschaulicht, wie sich die Normalisierung von diakritischen Zeichen auf Äußerungen auswirkt:

|Einstellung „false“ für diakritische Zeichen|Einstellung „true“ für diakritische Zeichen|
|--|--|
|`quiero tomar una piña colada`|`quiero tomar una pina colada`|
|||

### <a name="language-support-for-diacritics"></a>Sprachunterstützung für diakritische Zeichen

#### <a name="brazilian-portuguese-pt-br-diacritics"></a>Diakritische Zeichen: Portugiesisch (Brasilien) `pt-br`

|Einstellung „false“ für diakritische Zeichen|Einstellung „true“ für diakritische Zeichen|
|-|-|
|`á`|`a`|
|`â`|`a`|
|`ã`|`a`|
|`à`|`a`|
|`ç`|`c`|
|`é`|`e`|
|`ê`|`e`|
|`í`|`i`|
|`ó`|`o`|
|`ô`|`o`|
|`õ`|`o`|
|`ú`|`u`|
|||

#### <a name="dutch-nl-nl-diacritics"></a>Diakritische Zeichen: Niederländisch `nl-nl`

|Einstellung „false“ für diakritische Zeichen|Einstellung „true“ für diakritische Zeichen|
|-|-|
|`á`|`a`|
|`à`|`a`|
|`é`|`e`|
|`ë`|`e`|
|`è`|`e`|
|`ï`|`i`|
|`í`|`i`|
|`ó`|`o`|
|`ö`|`o`|
|`ú`|`u`|
|`ü`|`u`|
|||

#### <a name="french-fr--diacritics"></a>Diakritische Zeichen: Französisch `fr-`

Gilt sowohl für den französischen als auch für den kanadischen Unterkulturbereich.

|Einstellung „false“ für diakritische Zeichen|Einstellung „true“ für diakritische Zeichen|
|--|--|
|`é`|`e`|
|`à`|`a`|
|`è`|`e`|
|`ù`|`u`|
|`â`|`a`|
|`ê`|`e`|
|`î`|`i`|
|`ô`|`o`|
|`û`|`u`|
|`ç`|`c`|
|`ë`|`e`|
|`ï`|`i`|
|`ü`|`u`|
|`ÿ`|`y`|

#### <a name="german-de-de-diacritics"></a>Diakritische Zeichen: Deutsch `de-de`

|Einstellung „false“ für diakritische Zeichen|Einstellung „true“ für diakritische Zeichen|
|--|--|
|`ä`|`a`|
|`ö`|`o`|
|`ü`|`u`|

#### <a name="italian-it-it-diacritics"></a>Diakritische Zeichen: Italienisch `it-it`

|Einstellung „false“ für diakritische Zeichen|Einstellung „true“ für diakritische Zeichen|
|--|--|
|`à`|`a`|
|`è`|`e`|
|`é`|`e`|
|`ì`|`i`|
|`í`|`i`|
|`î`|`i`|
|`ò`|`o`|
|`ó`|`o`|
|`ù`|`u`|
|`ú`|`u`|

#### <a name="spanish-es--diacritics"></a>Diakritische Zeichen: Spanisch `es-`

Gilt sowohl für Spanisch als auch für Mexikanisch (Kanada).

|Einstellung „false“ für diakritische Zeichen|Einstellung „true“ für diakritische Zeichen|
|-|-|
|`á`|`a`|
|`é`|`e`|
|`í`|`i`|
|`ó`|`o`|
|`ú`|`u`|
|`ü`|`u`|
|`ñ`|`u`|


## <a name="punctuation-normalization"></a>Normalisierung der Interpunktion

Aktivieren Sie die Äußerungsnormalisierung für die Interpunktion für Ihre LUIS-JSON-App-Datei im Parameter `settings`.

```JSON
"settings": [
    {"name": "NormalizePunctuation", "value": "true"}
]
```

Anhand der folgenden Äußerungen wird veranschaulicht, wie sich die Interpunktion auf Äußerungen auswirkt:

|Wenn die Interpunktion auf „False“ festgelegt ist|Wenn die Interpunktion auf „True“ festgelegt ist|
|--|--|
|`Hmm..... I will take the cappuccino`|`Hmm I will take the cappuccino`|
|||

### <a name="punctuation-removed"></a>Entfernte Interpunktion

Die folgende Interpunktion wird entfernt, wenn `NormalizePunctuation` auf „true“ festgelegt ist.

|Interpunktion|
|--|
|`-`|
|`.`|
|`'`|
|`"`|
|`\`|
|`/`|
|`?`|
|`!`|
|`_`|
|`,`|
|`;`|
|`:`|
|`(`|
|`)`|
|`[`|
|`]`|
|`{`|
|`}`|
|`+`|
|`¡`|

## <a name="next-steps"></a>Nächste Schritte

* Machen Sie sich mit den [Konzepten](luis-concept-utterance.md#utterance-normalization-for-diacritics-and-punctuation) von diakritischen Zeichen und Interpunktion vertraut.
