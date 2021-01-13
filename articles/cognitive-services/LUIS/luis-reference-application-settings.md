---
title: 'Anwendungseinstellungen: LUIS'
description: Anwendungseinstellungen für Language Understanding-Apps von Azure Cognitive Services werden in der App und im Portal gespeichert.
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 05/04/2020
ms.openlocfilehash: 0578e3c3c952a475c6beb01ffcf354e19eda6e26
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91319162"
---
# <a name="app-and-version-settings"></a>App- und Versionseinstellungen

Diese Einstellungen werden in der [exportierten](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c40) App gespeichert und mithilfe der REST-APIs oder über das LUIS-Portal aktualisiert.

Wenn Sie die Einstellungen Ihrer App-Version ändern, wird der Trainingsstatus Ihrer App auf „Untrainiert“ zurückgesetzt.

[!INCLUDE [App and version settings](includes/app-version-settings.md)]


Textreferenz und Beispiele enthalten Folgendes:

* [Interpunktion](#punctuation-normalization)
* [Diakritische Zeichen](#diacritics-normalization)

## <a name="diacritics-normalization"></a>Normalisierung von diakritischen Zeichen

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
