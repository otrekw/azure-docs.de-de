---
title: 'Häufig gestellte Fragen: Textübersetzung'
titleSuffix: Azure Cognitive Services
description: Erhalten Sie Antworten auf häufig gestellte Fragen zur Textübersetzungs-API in Azure Cognitive Services.
services: cognitive-services
author: laujan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 05/18/2021
ms.author: lajanuar
ms.openlocfilehash: db1d981305ee62267f72f3050475c88dbcd5cc54
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110116583"
---
# <a name="frequently-asked-questionstranslator-api"></a>Häufig gestellte Fragen: Textübersetzungs-API

## <a name="how-does-translator-count-characters"></a>Wie zählt die Textübersetzung die Zeichen?

Die Textübersetzung zählt jeden in Unicode definierten Codepunkt als Zeichen. Jede Übersetzung gilt als separate Übersetzung, auch wenn die Anforderung in einem einzelnen API-Aufruf zur Übersetzung in mehrere Sprachen gestellt wurde. Die Länge der Antwort spielt keine Rolle, und die Anzahl der Anforderungen, Wörter, Bytes oder Sätze ist für die Zeichenanzahl nicht relevant.

Die Textübersetzung zählt die folgende Eingabe:

* Text, der im Hauptteil einer Anforderung an die Textübersetzung übergeben wurde.
  * `Text` bei Verwendung der Methoden [Übersetzen](reference/v3-0-translate.md), [Transkribieren](reference/v3-0-transliterate.md) und [Wörterbuchsuche](reference/v3-0-dictionary-lookup.md).
  * `Text` und `Translation` bei Verwendung der Methode [Wörterbuchbeispiele](reference/v3-0-dictionary-examples.md).

* Das gesamte Markup: HTML-, XML-Tags usw. im Textfeld des Anforderungstexts. Die JSON-Notation, die für die Erstellung der Anforderung verwendet wird (z. B. der Schlüssel „Text:“), wird **nicht** gezählt.
* Ein einzelner Buchstabe.
* Interpunktion.
* Ein Leerzeichen, Tabulatorzeichen, Markup oder beliebiges Leerraumzeichen.
* Eine Übersetzungswiederholung, selbst wenn Sie zuvor den gleichen Text übersetzt haben. Jedes Zeichen, das an die translate-Funktion übermittelt wird, wird gezählt, auch wenn der Inhalt nicht geändert wird oder Quell- und Zielsprache identisch sind.

Für Skripts, die auf grafischen Symbolen basieren, z. B. geschriebenes Chinesisch und japanische Kanji, zählt der Textübersetzungsdienst die Anzahl der Unicode-Codepunkte. Ein Zeichen pro Symbol. Ausnahme: Unicode-Ersatzzeichenpaare zählen als zwei Zeichen.

Aufrufe der Methoden **Detect** und **BreakSentence** werden nicht in den Zeichenverbrauch eingerechnet. Wir erwarten jedoch, dass die Aufrufe der Methoden „Detect“ und „BreakSentence“ in einem angemessenen Verhältnis zur Verwendung anderer gezählten Funktionen stehen. Wenn die Anzahl der erfolgten Aufrufe von „Detect“ oder „BreakSentence“ die Anzahl der anderen gezählten Methoden um das 100-fache übersteigt, behält sich Microsoft das Recht vor, Ihre Verwendung dieser Methoden einzuschränken.

## <a name="where-can-i-see-my-monthly-usage"></a>Wo kann ich meinen monatlichen Verbrauch sehen?

Sie können den [Azure-Preisrechner](https://azure.microsoft.com/pricing/calculator/) verwenden, um Ihre Kosten zu schätzen. Sie können Azure-Warnungen für Ihre Azure-Dienste auch in Ihrem Benutzerkonto im Azure-Portal überwachen, anzeigen und hinzufügen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Navigieren Sie zur Übersichtsseite Ihrer Textübersetzungsressource.
1. Wählen Sie das **Abonnement** für Ihre Textübersetzungsressource aus.

    :::image type="content" source="media/azure-portal-overview.png" alt-text="Screenshot: Abonnementlink auf der Übersichtsseite im Azure-Portal":::

2. Treffen Sie in der linken Schiene unter **Cost Management** Ihre Auswahl:

    :::image type="content" source="media/azure-portal-cost-management.png" alt-text="Screenshot: Links zu Cost Management-Ressourcen im Azure-Portal":::

## <a name="is-attribution-required-when-using-translator"></a>Ist bei der Verwendung der Textübersetzung eine Zuordnung erforderlich?

Die Zuordnung ist nicht erforderlich, wenn die Textübersetzung für die Text- und Sprachübersetzung verwendet wird. Es wird empfohlen, Benutzer darüber zu informieren, dass die angezeigten Inhalte maschinell übersetzt sind.

Wenn eine Zuordnung vorliegt, muss sie den [Zuordnungsrichtlinien für Übersetzer](https://www.microsoft.com/translator/business/attribution/) entsprechen.

## <a name="is-translator-a-replacement-for-human-translator"></a>Ist die Textübersetzung (Translator) ein Ersatz für menschliche Übersetzer?

Nein, beide haben ihren Platz als wichtige Kommunikationstools. Verwenden Sie die maschinelle Übersetzung, bei der die Menge an Inhalten, die Geschwindigkeit der Erstellung und Budgeteinschränkungen die Verwendung menschlicher Übersetzungen unmöglich machen.

Die maschinelle Übersetzung wurde von mehreren [LSP-Partnern](https://www.microsoft.com/translator/business/partners/) (Language Service Provider) als erster Durchlauf verwendet, bevor menschliche Übersetzungen verwendet wurden, und sie kann die Produktivität um bis zu 50 Prozent steigern. Eine Liste der LSP-Partner finden Sie auf der Seite der Partner für Textübersetzungen.

---
> [!TIP]
> Wenn Sie in diesen FAQs keine Antwort auf Ihre Frage finden, können Sie die Fragen unter [StackOverflow](https://stackoverflow.com/search?q=%5Bmicrosoft-cognitive%5D+or+%5Bmicrosoft-cognitive%5D+translator&s=34bf0ce2-b6b3-4355-86a6-d45a1121fe27) an die Community der Textübersetzungs-API stellen.