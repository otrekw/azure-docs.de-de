---
title: 'Entwerfen für eine Sprache: QnA Maker'
description: Die QnA Maker-Ressource und alle Wissensdatenbanken innerhalb dieser Ressource unterstützen nur eine einzelne Sprache. Diese einzelne Sprache ist erforderlich, um bestmögliche Antwortergebnisse für eine Abfrage zu erzielen.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 01/27/2020
ms.openlocfilehash: e09e15f71b0574a5612e7f9bacd8aaa4739a441c
ms.sourcegitcommit: ef69245ca06aa16775d4232b790b142b53a0c248
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/06/2020
ms.locfileid: "91777002"
---
# <a name="design-knowledge-base-for-content-language"></a>Entwerfen einer Wissensdatenbank für die Inhaltssprache

Die QnA Maker-Ressource und alle Wissensdatenbanken innerhalb dieser Ressource unterstützen nur eine einzelne Sprache. Diese einzelne Sprache ist erforderlich, um bestmögliche Antwortergebnisse für eine Abfrage zu erzielen.

## <a name="single-language-per-resource"></a>Einzelne Sprache pro Ressource

QnA Maker-Überlegungen zur Sprachunterstützung:

* Ein QnA Maker-Dienst und alle seine Wissensdatenbanken unterstützen nur eine Sprache
* Die Sprache wird explizit festgelegt, wenn die erste Wissensdatenbank des Diensts erstellt wird
* Die Sprache wird durch die Dateien und URLs bestimmt, die beim Erstellen der Wissensdatenbank hinzugefügt werden
* Die Sprache kann für andere Wissensdatenbanken im Dienst nicht geändert werden
* Die Sprache wird vom Cognitive Search-Dienst (Bewerter Nr. 1) und dem QnA Maker-Dienst (Bewerter Nr. 2) verwendet, um die beste Antwort auf eine Abfrage zu generieren

## <a name="supporting-multiple-languages"></a>Unterstützung mehrerer Sprachen

Wenn Sie ein Wissensdatenbank-System unterstützen müssen, das mehrere Sprachen umfasst, stehen die folgenden Methoden zur Wahl:

* Verwenden Sie den [Textübersetzungsdienst](../../translator/translator-info-overview.md), um eine Frage in eine einzelne Sprache zu übersetzen, bevor die Frage an Ihre Wissensdatenbank gesendet wird. Dies erlaubt es Ihnen, sich auf die Qualität einer einzelnen Sprache und die Qualität der alternativen Fragen und Antworten zu konzentrieren.
* Erstellen Sie für jede Sprache eine QnA Maker-Ressource und eine Wissensdatenbank innerhalb dieser Ressource. Auf diese Weise können Sie für jede Sprache separate alternative Frage- und Antworttexte mit stärkerer Nuancierung verwenden. Dadurch erhalten Sie eine viel größere Flexibilität, allerdings um den Preis eines deutlich höheren Verwaltungsaufwands, wenn sich die Fragen oder Antworten über die Sprachen hinweg unterscheiden.

Mehr dazu finden Sie bei den [unterstützten Sprachen](../overview/language-support.md) für QnA Maker.

### <a name="support-each-language-with-a-qna-maker-resource"></a>Unterstützen Sie jede Sprache mit einer QnA Maker-Ressource

* Erstellen Sie für jede Sprache eine QnA Maker-Ressource
* Fügen Sie nur Dateien und URLs für diese Sprache hinzu
* Verwenden Sie eine Benennungskonvention zum Identifizieren der Sprache. Ein Beispiel ist `qna-maker-fr` für alle Wissensdatenbanken für französische Dokumente

## <a name="next-steps"></a>Nächste Schritte

Lernen Sie [Konzepte](query-knowledge-base.md) zum Abfragen der Wissensdatenbank nach einer Antwort kennen.