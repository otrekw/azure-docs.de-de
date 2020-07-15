---
title: Künstliche Intelligenz
description: LUIS verwendet künstliche Intelligenz für das Sprachverständnis Ihrer Daten basierend auf dem definierten Schema.
ms.topic: conceptual
ms.date: 06/29/2020
ms.openlocfilehash: 4f145585e097a3cf6a2338dbab879f6fce07f71d
ms.sourcegitcommit: a989fb89cc5172ddd825556e45359bac15893ab7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/01/2020
ms.locfileid: "85802638"
---
# <a name="artificial-intelligence-in-language-understanding-luis"></a>Künstliche Intelligenz in Language Understanding (LUIS)

LUIS verwendet künstliche Intelligenz für das Verstehen natürlicher Sprache (Natural Language Understanding, NLU) Ihrer Daten basierend auf dem definierten Schema.

## <a name="natural-language-processing"></a>Verarbeitung natürlicher Sprache

Das Verstehen natürlicher Sprache (NLU) ist ein spezifisches Unterthema der Verarbeitung natürlicher Sprache (Natural Language Processing, NLP).

Bei der Verarbeitung natürlicher Sprache handelt es sich um ein umfassenderes Konzept, das jede Form der Verarbeitung von Textdaten betrifft und beispielsweise Folgendes umfasst:

* Tokenisierung
* Satzteilmarkierung
* Segmentierung
* Morphologische Analyse
* Semantische Nähe
* Discourse
* Sprachübersetzung

## <a name="natural-language-processing-in-luis"></a>Verarbeitung natürlicher Sprache in LUIS

Die Verarbeitung natürlicher Sprache steht in der LUIS-App wie folgt zur Verfügung:
* [Verstehen natürlicher Sprache](#natural-language-understanding) (LUIS)
* Konfigurierbare Aspekte der Verarbeitung natürlicher Sprache in LUIS:
    * [Tokenisierung](luis-language-support.md#tokenization)
    * Morphologie durch [API-Einstellungen](luis-reference-application-settings.md) für diakritische Zeichen, Interpunktion und Wortformen
* Vor- und Nachbearbeitung der Abfrageäußerung durch andere [Cognitive Services](../Welcome.md) wie beispielsweise:
    * [Übersetzung](../translator/translator-info-overview.md)

## <a name="natural-language-understanding"></a>Verstehen natürlicher Sprache

Das Verstehen natürlicher Sprache stellt die Möglichkeit dar, eine sprachliche Aussage in eine Darstellung _umzuwandeln_, die es Ihnen ermöglicht, Benutzer auf natürliche Weise zu verstehen. Das Verstehen natürlicher Sprache ist immer noch eine große Herausforderung und ist als Problem des Typs _AI-hard_ definiert.

LUIS soll sich auf die Absicht und Extraktion konzentrieren, sodass Folgendes identifiziert werden kann:
* Was möchte der Benutzer?
* Worüber spricht er?

LUIS umfasst wenig oder gar kein Wissen der umfassenderen Aspekte der _Verarbeitung natürlicher Sprache_, z. B. semantische Nähe, und beinhaltet keine explizite Identifizierung in Beispielen. Beispielsweise unterscheiden sich die folgenden Token (Wörter), bis sie in Beispielen in einem ähnlichen Kontext verwendet werden:

* kaufen
* Kauf
* gekauft

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum [Entwicklungslebenszyklus](luis-concept-app-iteration.md) einer LUIS-App