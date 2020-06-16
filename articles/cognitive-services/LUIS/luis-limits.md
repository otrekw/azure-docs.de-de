---
title: Grenzwerte – LUIS
description: Dieser Artikel behandelt die bekannten Einschränkungen von Azure Cognitive Services Language Understanding (LUIS). LUIS weist Begrenzungen in mehreren Bereichen auf. Modellbegrenzungen steuern Absichten, Entitäten und Features in LUIS. Kontingentgrenzen basierend auf dem Schlüsseltyp. Tastenkombinationen steuern die LUIS-Website.
ms.topic: reference
ms.date: 06/04/2020
ms.openlocfilehash: aa4362fba09834758d47f3ef063068c1854b9280
ms.sourcegitcommit: 0a5bb9622ee6a20d96db07cc6dd45d8e23d5554a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2020
ms.locfileid: "84449497"
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
| [Machine Learning-Entitäten und Rollen](./luis-concept-entity-types.md):<br> Zusammengesetzt,<br>Einfach,<br>Entitätsrolle|Maximal 100 übergeordnete Entitäten oder 330 Entitäten, je nachdem, welches Limit der Benutzer zuerst erreicht. Eine Rolle zählt für diese Begrenzung als eine Entität. Ein Beispiel für eine zusammengesetzte Entität mit einer einfachen Entität, die über zwei Rollen verfügt: 1 zusammengesetzte + 1 einfache + 2 Rollen = 4 der 330 Entitäten.<br>Untergeordnete Entitäten können auf bis zu fünf Ebenen geschachtelt werden.|
|Modell als Feature| Die maximale Anzahl von Modellen, die als Feature für ein bestimmtes Modell verwendet werden können, liegt bei zehn Modellen. Die maximale Anzahl von Ausdruckslisten, die als Feature für ein bestimmtes Modell verwendet werden können, liegt bei zehn Ausdruckslisten.|
| [Vorschau – Entitäten vom Typ „dynamische Liste“](https://aka.ms/luis-api-v3-doc#dynamic-lists-passed-in-at-prediction-time)|2 Listen mit ca. 1.000 pro Anforderung für den Endpunkt der Abfragevorhersage|
| [Muster](luis-concept-patterns.md)|500 Muster pro Anwendung.<br>Muster dürfen maximal 400 Zeichen lang sein.<br>3 Pattern.any-Entitäten pro Muster<br>Maximal 2 geschachtelte optionale Texte im Muster|
| [Pattern.any-Entitäten](./luis-concept-entity-types.md)|100 pro Anwendung, 3 Pattern.any-Entitäten pro Muster |
| [Begriffsliste][phrase-list]|500 Ausdruckslisten 10 globale Ausdruckslisten aufgrund der Funktionsbeschränkung des Modells. Nicht austauschbare Ausdrucksliste mit maximal 5.000 Ausdrücken. Austauschbare Ausdrucksliste mit maximal 50.000 Ausdrücken. Die maximale Anzahl von Ausdrücken pro Anwendung beträgt 500.000 Ausdrücke.|
| [Vordefinierte Entitäten](./luis-prebuilt-entities.md) | Keine Begrenzung|
| [RegEx-Entitäten](./luis-concept-entity-types.md)|20 Entitäten<br>max. 500 Zeichen pro RegEx-Entitätsmuster|
| [Rollen](luis-concept-roles.md)|300 Rollen pro Anwendung. 10 Rollen pro Entität|
| [Äußerung][utterances] | 500 Zeichen<br><br>Wenn Text länger als dieses Zeichenlimit ist, müssen Sie die Äußerung vor der Eingabe in LUIS segmentieren und die Antworten entsprechend zusammenführen. Es gibt offensichtliche Unterbrechungen, die Sie nutzen können, z. B. Satzzeichen und lange Pausen in der Sprache.|
| [Beispiele für Äußerungen][utterances] | 15.000 pro Anwendung – es gibt keine Beschränkung für die Anzahl von Äußerungen pro Absicht<br><br>Wenn Sie die Anwendung mit weiteren Beispielen trainieren möchten, verwenden Sie einen verteilten Modellansatz ([Dispatch](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/Dispatch)). Sie trainieren einzelne LUIS-Apps (als untergeordnete Apps der übergeordneten Dispatch-App bezeichnet) mit einer oder mehreren Absichten und abschließend eine Dispatch-App mit Stichproben aus den Äußerungen der einzelnen untergeordneten LUIS-Apps, um die Vorhersageanforderung an die richtige untergeordnete App weiterzuleiten. |
| [Versionen](luis-concept-version.md)| 100 Versionen pro Anwendung |
| [Versionsname][luis-how-to-manage-versions] | 128 Zeichen |

\* Das Standardzeichenlimit beträgt 50 Zeichen.

<a name="intent-and-entity-naming"></a>

## <a name="name-uniqueness"></a>Eindeutigkeit des Namens

Objektnamen müssen eindeutig sein, wenn sie mit anderen Objekten der gleichen Ebene verglichen werden.

|Objekte|Beschränkungen|
|--|--|
|Absicht, Entität|Alle Absichts- und Entitätsnamen müssen in einer Version einer App eindeutig sein.|
|ML-Entitätskomponenten|Alle Komponenten von Machine Learning-Entitäten (untergeordnete Entitäten) müssen innerhalb dieser Entität für Komponenten auf der gleichen Ebene eindeutig sein.|
|Features | Alle benannten Features (beispielsweise Ausdruckslisten) müssen innerhalb App-Version eindeutig sein.|
|Entitätsrollen|Alle Rollen in einer Entität oder Entitätskomponente müssen eindeutig sein, wenn sie sich auf der gleichen Entitätsebene befinden (übergeordnet, untergeordnet, auf der zweiten Unterebene usw.).|

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
