---
title: Semantische Suche
titleSuffix: Azure Cognitive Search
description: Hier finden Sie Informationen zur Verwendung von Deep Learning-Modellen für die semantische Suche von Bing in Cognitive Search, um intuitivere Suchergebnisse zu erhalten.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 04/01/2021
ms.custom: references_regions
ms.openlocfilehash: 04300b8d148bb22bf585aa81481c475b347ad462
ms.sourcegitcommit: 3f684a803cd0ccd6f0fb1b87744644a45ace750d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/02/2021
ms.locfileid: "106222043"
---
# <a name="semantic-search-in-azure-cognitive-search"></a>Semantische Suche in Azure Cognitive Search

> [!IMPORTANT]
> Die semantische Suche befindet sich in der öffentlichen Vorschauphase und ist über die Vorschau-REST-API und das Portal verfügbar. Previewfunktionen werden im Ist-Zustand gemäß den [ergänzenden Nutzungsbedingungen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) angeboten, und es ist nicht garantiert, dass dieselbe Implementierung bei allgemeiner Verfügbarkeit verwendet wird. Diese Features sind abrechenbar. Weitere Informationen finden Sie unter [Verfügbarkeit und Preise](semantic-search-overview.md#availability-and-pricing).

Bei der semantischen Suche handelt es sich um eine Sammlung abfragebezogener Funktionen, die den Suchergebnissen semantische Relevanz und Sprachverständnis (Language Understanding) hinzufügen. Die *semantische Rangfolge* sucht nach Kontext und Verwandtschaft zwischen Begriffen und stuft die Übereinstimmungen hoch, die gemäß Abfrage sinnvoller sind. Language Understanding findet *Titel* und *Antworten* innerhalb Ihrer Inhalte, die das übereinstimmende Dokument zusammenfassen oder eine Frage beantworten. Die Ergebnisse werden anschließend auf einer Suchergebnisseite dargestellt, um eine produktivere Suchoberfläche bereitzustellen.

Moderne vorab trainierte Modelle werden für Zusammenfassung und Rangfolge verwendet. Um die hohe Leistung zu gewährleisten, die Benutzer von der Suche erwarten, werden semantische Zusammenfassung und Rangfolge auf die ersten 50 Ergebnisse gemäß der Bewertung durch den standardmäßigen [Ähnlichkeitsalgorithmus für die Rangfolge](index-similarity-and-scoring.md#similarity-ranking-algorithms) angewendet. Wenn diese Ergebnisse als Dokumentkorpus verwendet werden, bewertet die semantische Rangfolge diese Ergebnisse basierend auf der semantischen Stärke der Entsprechung neu.

Die zugrunde liegende Technologie stammt von Bing und Microsoft Research und ist als Add-On-Feature in die Cognitive Search-Infrastruktur integriert. Weitere Informationen zu den Forschungs- und KI-Investitionen, die hinter der semantischen Suche stehen, finden Sie unter [How AI from Bing is powering Azure Cognitive Search (Microsoft Research Blog)](https://www.microsoft.com/research/blog/the-science-behind-semantic-search-how-ai-from-bing-is-powering-azure-cognitive-search/) (Wie die KI von Bing die Azure Cognitive Search unterstützt).

Im folgenden Video erhalten Sie eine Übersicht zu den Funktionen.

> [!VIDEO https://www.youtube.com/embed/yOf0WfVd_V0]

## <a name="components-and-workflow"></a>Komponenten und Workflow

Die semantische Suche verbessert Genauigkeit und Abruf durch Hinzufügen folgender Funktionen:

| Funktion | BESCHREIBUNG |
|---------|-------------|
| [Rechtschreibprüfung](speller-how-to-add.md) | Korrektur von Tippfehlern, bevor die Abfragebegriffe die Suchmaschine erreichen. |
| [Semantische Rangfolge](semantic-ranking.md) | Verwendung von Kontext oder semantischer Bedeutung, um eine Relevanzbewertung zu berechnen. |
| [Semantische Titel und Markierungen](semantic-how-to-query-request.md) | Sätze und Ausdrücke aus einem Dokument, die den Inhalt am besten zusammenfassen, mit Hervorhebungen wichtiger Passagen zum einfachen Scannen. Titel, die ein Ergebnis zusammenfassen, sind nützlich, wenn einzelne Inhaltsfelder für die Ergebnisseite zu dicht sind. Der markierte Text hebt die relevantesten Begriffe und Ausdrücke hervor, sodass Benutzer schnell ermitteln können, warum eine Entsprechung als relevant eingestuft wurde. |
| [Semantische Antworten](semantic-answers.md) | Eine optionale und zusätzliche Unterstruktur, die von einer Semantikabfrage zurückgegeben wird. Sie bietet eine direkte Antwort auf eine Abfrage, die wie eine Frage aussieht. |

### <a name="order-of-operations"></a>Reihenfolge der Vorgänge

Komponenten der semantischen Suche erweitern die vorhandene Pipeline zur Abfrageausführung in beide Richtungen. Wenn Sie die Rechtschreibkorrektur aktivieren, korrigiert die [Rechtschreibprüfung](speller-how-to-add.md) die Tippfehler zu Beginn der Abfrage, bevor die Begriffe das Suchmodul erreichen.

:::image type="content" source="media/semantic-search-overview/semantic-workflow.png" alt-text="Semantikkomponenten in der Abfrageausführung" border="true":::

Die Ausführung der Abfrage verläuft wie gewohnt, mit Begriffsanalyse, Analyse und Scans über die invertierten Indizes. Die Engine ruft Dokumente mithilfe der Tokenübereinstimmung ab und bewertet die Ergebnisse mithilfe des standardmäßigen [Ähnlichkeitsalgorithmus für die Rangfolge](index-similarity-and-scoring.md#similarity-ranking-algorithms). Die Ergebnisse werden basierend auf dem Grad der linguistischen Ähnlichkeit zwischen Abfragebegriffen und übereinstimmenden Begriffen im Index berechnet. Bewertungsprofile werden in dieser Phase ebenfalls angewandt, sofern Sie sie definiert haben. Die Ergebnisse werden dann an das semantische Suchsubsystem weitergeleitet.

Im Vorbereitungsschritt wird der vom ursprünglichen Resultset zurückgegebene Dokumentkorpus auf Satz- und Absatzebene analysiert, um Passagen zu finden, die die einzelnen Dokumente zusammenfassen. Anders als bei der Schlüsselwortsuche verwendet dieser Schritt maschinelles Lesen und Verstehen, um den Inhalt auszuwerten. In dieser Phase der Inhaltsverarbeitung gibt eine semantische Abfrage [Titel](semantic-how-to-query-request.md) und [Antworten](semantic-answers.md) zurück. Um sie zu formulieren, nutzt die semantische Suche die Sprachrepräsentation, um Schlüsselpassagen zu extrahieren und hervorzuheben, die ein Ergebnis am besten zusammenfassen. Wenn die Suchabfrage eine Frage ist und Antworten angefordert wurden, enthält das Ergebnis auch eine Textpassage, die die von der Suchabfrage ausgedrückte Frage am besten beantwortet. 

Für Titel und Antworten wird vorhandener Text in der Formulierung verwendet. Die Semantikmodelle erstellen keine neuen Sätze oder Ausdrücke aus dem verfügbaren Inhalt und wenden auch keine Logik an, um neue Schlussfolgerungen zu treffen. Kurz gesagt, gibt das System niemals Inhalt zurück, der nicht bereits vorhanden ist.

Die Ergebnisse werden dann basierend auf der [konzeptionellen Ähnlichkeit](semantic-ranking.md) der Abfragebegriffe neu bewertet.

Um semantische Funktionen in Abfragen zu verwenden, müssen Sie geringfügige Änderungen an der [Suchanforderung](semantic-how-to-query-request.md) vornehmen. Es ist aber keine zusätzliche Konfiguration oder Neuindizierung erforderlich.

## <a name="availability-and-pricing"></a>Verfügbarkeit und Preismodell

Semantische Funktionen sind über die [Registrierung](https://aka.ms/SemanticSearchPreviewSignup) für im Tarif „Standard“ (S1, S2, S3) erstellte Suchdienste in folgenden Regionen verfügbar: „USA, Norden-Mitte“, „USA, Westen“, „USA, Westen 2“, „USA, Osten 2“, „Europa, Norden“ und „Europa, Westen“. 

Die Rechtschreibkorrektur ist in den gleichen Regionen verfügbar, aber auf keinen bestimmten Tarif beschränkt. Wenn Sie bereits einen Dienst nutzen, der den Kriterien für Tarif und Region entspricht, ist nur die Registrierung erforderlich.

Ab dem Start der Vorschauversion am 2. März bis weit in den April werden Rechtschreibkorrektur und semantische Rangfolge kostenlos angeboten. Später im April wird der Rechenaufwand für die Nutzung dieser Funktionen in Rechnung gestellt. Die Kosten liegen voraussichtlich bei etwa 500 USD/Monat für 250.000 Abfragen. Ausführliche Kosteninformationen finden Sie unter [Azure Cognitive Search – Preise](https://azure.microsoft.com/pricing/details/search/) sowie unter [Schätzen und Verwalten von Kosten](search-sku-manage-costs.md).

## <a name="next-steps"></a>Nächste Schritte

Ein neuer Abfragetyp ermöglicht die Rangfolge nach Relevanz sowie Antwortstrukturen für die semantische Suche.

[Erstellen Sie eine Semantikabfrage](semantic-how-to-query-request.md), um mit der Verwendung zu beginnen. Suchen Sie alternativ in den folgenden Artikeln nach entsprechenden Informationen.

+ [Hinzufügen der Rechtschreibprüfung für Abfragebegriffe](speller-how-to-add.md)
+ [Zurückgeben einer semantischen Antwort](semantic-answers.md)
+ [Semantische Rangfolge](semantic-ranking.md)
+ [Einführung in die semantische Suche (Blogbeitrag)](https://techcommunity.microsoft.com/t5/azure-ai/introducing-semantic-search-bringing-more-meaningful-results-to/ba-p/2175636)
+ [Gewinnen aussagekräftiger Erkenntnisse mithilfe von Semantikfunktionen (AI Show-Video)](https://channel9.msdn.com/Shows/AI-Show/Find-meaningful-insights-using-semantic-capabilities-in-Azure-Cognitive-Search)