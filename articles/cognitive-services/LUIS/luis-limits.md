---
title: Grenzwerte – LUIS
description: Dieser Artikel behandelt die bekannten Einschränkungen von Azure Cognitive Services Language Understanding (LUIS). LUIS weist Begrenzungen in mehreren Bereichen auf. Modellbegrenzungen steuern Absichten, Entitäten und Features in LUIS. Kontingentgrenzen basierend auf dem Schlüsseltyp. Tastenkombinationen steuern die LUIS-Website.
ms.topic: reference
ms.date: 04/02/2020
ms.openlocfilehash: 0a734091ad2c9812f079d77c97c22872717aa7c9
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "82103580"
---
# <a name="limits-for-your-luis-model-and-keys"></a>Begrenzungen für das LUIS-Modell und die Schlüssel
LUIS weist Begrenzungen in mehreren Bereichen auf. Der erste ist die [Modellbegrenzung](#model-limits), die Absichten, Entitäten und Features in LUIS steuert. Der zweite Bereich sind [Kontingentlimits](#key-limits), die auf dem Schlüsseltyp basieren. Ein dritter Bereich von Begrenzungen umfasst die [Tastenkombinationen](#keyboard-controls) zum Steuern der LUIS-Website. Ein vierter Bereich bezieht sich auf die [Zuordnung der Regionen weltweit](luis-reference-regions.md) zwischen der LUIS-Erstellungswebsite und den LUIS-[Endpunkt](luis-glossary.md#endpoint)-APIs.

<a name="model-boundaries"></a>

## <a name="model-limits"></a>Modellbegrenzungen

Wenn Ihre App die LUIS-Modellbegrenzungen überschreitet, sollten Sie eine [LUIS-Dispatch](luis-concept-enterprise.md#dispatch-tool-and-model)-App oder einen [LUIS-Container](luis-container-howto.md) verwenden.

|Bereich|Begrenzung|
|--|:--|
| [App-Name][luis-get-started-create-app] | \* Standardzeichenlimit |
| Anwendungen| 500 Anwendungen pro Azure-Erstellungsressource |
| [Batchtests][batch-testing]| 10 Datasets, 1.000 Äußerungen pro Dataset|
| Explizite Listen | 50 pro Anwendung|
| Externe Entitäten | Keine Einschränkungen |
| [Absichten][intents]|500 pro Anwendung: 499 benutzerdefinierte Absichten und die erforderliche Absicht _None_.<br>[Dispatchbasierte](https://aka.ms/dispatch-tool) Anwendungen verfügen entsprechend über 500 Dispatchquellen.|
| [List-Entitäten](./luis-concept-entity-types.md) | Übergeordnet: 50, untergeordnet: 20.000 Elemente. Kanonischer Name mit * Standardzeichenlimit. Für Synonymwerte gelten keine Längenbeschränkungen. |
| [Durch maschinelles Lernen erworbene Entitäten und Rollen:](./luis-concept-entity-types.md)<br> Zusammengesetzt,<br>Einfach,<br>Entitätsrolle|Maximal 100 übergeordnete Entitäten oder 330 Entitäten, je nachdem, welches Limit der Benutzer zuerst erreicht. Eine Rolle zählt für diese Begrenzung als eine Entität. Ein Beispiel für eine zusammengesetzte Entität mit einer einfachen Entität, die über zwei Rollen verfügt: 1 zusammengesetzte + 1 einfache + 2 Rollen = 4 der 330 Entitäten.<br>Unterkomponenten können auf bis zu fünf Ebenen geschachtelt werden.|
|Modell als Feature| Die maximale Anzahl von Modellen, die als Deskriptor (Feature) für ein bestimmtes Modell verwendet werden können, beträgt 10 Modelle. Die maximale Anzahl von Ausdruckslisten, die als Deskriptor (Feature) für ein bestimmtes Modell verwendet werden können, beträgt 10 Ausdruckslisten.|
| [Vorschau – Entitäten vom Typ „dynamische Liste“](https://aka.ms/luis-api-v3-doc#dynamic-lists-passed-in-at-prediction-time)|2 Listen mit ca. 1.000 pro Anforderung für den Endpunkt der Abfragevorhersage|
| [Muster](luis-concept-patterns.md)|500 Muster pro Anwendung.<br>Muster dürfen maximal 400 Zeichen lang sein.<br>3 Pattern.any-Entitäten pro Muster<br>Maximal 2 geschachtelte optionale Texte im Muster|
| [Pattern.any-Entitäten](./luis-concept-entity-types.md)|100 pro Anwendung, 3 Pattern.any-Entitäten pro Muster |
| [Begriffsliste][phrase-list]|500 Ausdruckslisten 10 globale Ausdruckslisten aufgrund der Funktionsbeschränkung des Modells. Nicht austauschbare Ausdrucksliste mit maximal 5.000 Ausdrücken. Austauschbare Ausdrucksliste mit maximal 50.000 Ausdrücken. Die maximale Anzahl von Ausdrücken pro Anwendung beträgt 500.000 Ausdrücke.|
| [Vordefinierte Entitäten](./luis-prebuilt-entities.md) | Keine Begrenzung|
| [RegEx-Entitäten](./luis-concept-entity-types.md)|20 Entitäten<br>max. 500 Zeichen pro RegEx-Entitätsmuster|
| [Rollen](luis-concept-roles.md)|300 Rollen pro Anwendung. 10 Rollen pro Entität|
| [Äußerung][utterances] | 500 Zeichen|
| [Äußerungen][utterances] | 15.000 pro Anwendung – es gibt keine Beschränkung für die Anzahl von Äußerungen pro Absicht|
| [Versionen](luis-concept-version.md)| 100 Versionen pro Anwendung |
| [Versionsname][luis-how-to-manage-versions] | 10 Zeichen, ausschließlich alphanumerische Zeichen und Punkt (.) |

\* Das Standardzeichenlimit beträgt 50 Zeichen.

<a name="intent-and-entity-naming"></a>

## <a name="name-uniqueness"></a>Eindeutigkeit des Namens

Verwenden Sie die folgenden Regeln zur Eindeutigkeit bei der Benennung.

Folgendes muss innerhalb einer LUIS-App eindeutig sein:

* Versionsname
* Absicht
* Entität
* roles

Folgendes muss innerhalb des Gültigkeitsbereichs eindeutig sein:

* Ausdrucksliste

## <a name="object-naming"></a>Benennung der Objekte

Verwenden Sie die folgenden Zeichen in den folgenden Namen.

|Object|Zeichen ausschließen|
|--|--|
|Absicht, Entität und Rollennamen|`:`<br>`$` <br> `&`|
|Versionsname|`\`<br> `/`<br> `:`<br> `?`<br> `&`<br> `=`<br> `*`<br> `+`<br> `(`<br> `)`<br> `%`<br> `@`<br> `$`<br> `~`<br> `!`<br> `#`|

## <a name="resource-usage-and-limits"></a>Ressourcennutzung und Limits

Language Understanding verfügt über separate Ressourcen, einen Typ für die Erstellung und einen Typ für die Abfrage des Vorhersageendpunkts. Weitere Informationen zu den Unterschieden zwischen Schlüsseltypen finden Sie unter [Erstellungsschlüssel und Endpunktschlüssel für Vorhersageabfragen in LUIS](luis-concept-keys.md).

<a name="key-limits"></a>

### <a name="authoring-resource-limits"></a>Erstellungsressourcenlimits

Verwenden Sie die _kind_ `LUIS.Authoring`, wenn Sie Ressourcen im Azure-Portal filtern. In LUIS gilt die Beschränkung von 500 Anwendungen pro Azure-Erstellungsressource.

|Erstellungsressource|Erstellungs-TPS|
|--|--|
|Starter|1 Mio./Monat, 5/Sekunde|
|F0: Free-Tarif |1 Mio./Monat, 5/Sekunde|

* TPS = Transaktionen pro Sekunde

[Weitere Informationen zu Preisen][pricing]

### <a name="query-prediction-resource-limits"></a>Limits für Vorhersageabfrageressource

Verwenden Sie die _kind_ `LUIS`, wenn Sie Ressourcen im Azure-Portal filtern. Die LUIS-Ressource für Abfragen des Vorhersageendpunkts, die zur Laufzeit verwendet wird, ist nur für Endpunktabfragen gültig.

|Vorhersageabfrageressource|Abfrage-TPS|
|--|--|
|F0: Free-Tarif |10.000/Monat, 5/Sekunde|
|S0: Standard-Tarif|50/Sekunde|

### <a name="sentiment-analysis"></a>Stimmungsanalyse

[Integration der Standpunktanalyse](luis-how-to-publish-app.md#enable-sentiment-analysis), die Standpunktinformationen liefert, wird bereitgestellt, ohne dass weitere Azure-Ressource erforderlich sind.

### <a name="speech-integration"></a>Speech-Integration

[Speech-Integration](../speech-service/how-to-recognize-intents-from-speech-csharp.md) stellt 1.000 Endpunktanforderungen in Kosten pro Einheit bereit.

[Weitere Informationen zu Preisen][pricing]

## <a name="keyboard-controls"></a>Tastenkombinationen

|Tastatureingabe | BESCHREIBUNG |
|--|--|
|STRG+E|Wechselt in der Liste der Äußerungen zwischen Token und Entitäten|

## <a name="website-sign-in-time-period"></a>Anmeldezeitraum für die Website

Ihr Anmeldezugriff gilt für **60 Minuten**. Nach diesem Zeitraum erhalten Sie diesen Fehler. Sie müssen sich erneut anmelden.

[luis-get-started-create-app]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-get-started-create-app
[batch-testing]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-test#batch-testing
[intents]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-intent
[phrase-list]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-feature
[utterances]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-utterance
[luis-how-to-manage-versions]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-how-to-manage-versions
[pricing]: https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/
<!-- TBD: fix this link -->
[speech-to-intent-pricing]: https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/
