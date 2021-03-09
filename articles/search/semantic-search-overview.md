---
title: Semantische Suche
titleSuffix: Azure Cognitive Search
description: Hier finden Sie Informationen zur Verwendung von Deep Learning-Modellen für die semantische Suche von Bing in Cognitive Search, um intuitivere Suchergebnisse zu erhalten.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 03/02/2021
ms.custom: references_regions
ms.openlocfilehash: e9cbb7daf61397064bd79f30d851d96fdf63f5a0
ms.sourcegitcommit: dda0d51d3d0e34d07faf231033d744ca4f2bbf4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102203231"
---
# <a name="semantic-search-in-azure-cognitive-search"></a>Semantische Suche in Azure Cognitive Search

> [!IMPORTANT]
> Die Features der semantischen Suche befinden sich in der Public Preview-Phase und stehen nur über die Vorschau-REST-API zur Verfügung. Vorschaufeatures werden wie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) beschrieben im Ist-Zustand angeboten.

Bei der semantischen Suche handelt es sich um eine Sammlung von abfragespezifischen Features, die hochwertigere und natürlichere Abfragen unterstützen. Zu den Features zählen das semantische Priorisieren von Suchergebnissen sowie die Generierung von Beschriftungen und Antworten mit semantischer Hervorhebung. Die 50 besten Ergebnisse der [Engine für die Volltextsuche](search-lucene-query-architecture.md) werden neu bewertet, um die relevantesten Treffer zu finden.

Die zugrunde liegende Technologie nutzt die Investitionen von Bing und Microsoft Research und ist in die Cognitive Search-Infrastruktur integriert. Um sie zu verwenden, sind geringfügige Änderungen an der Suchabfrage notwendig. Es ist aber keine zusätzliche Konfiguration oder Neuindizierung erforderlich.

Die Public Preview-Features umfassen Folgendes:

+ Ein semantisches Bewertungsmodell, dass die Ergebnisse basierend auf dem Kontext oder der semantischen Bedeutung von Suchabfragebegriffen bewertet
+ Semantische Beschriftungen zur Hervorhebung relevanter Passagen
+ Semantische Antworten auf die Abfrage (auch auf der Grundlage von Ergebnissen formuliert)
+ Rechtschreibprüfung, um Tippfehler zu korrigieren, bevor die Abfragebegriffe die Suchmaschine erreichen

## <a name="semantic-search-architecture"></a>Architektur der semantischen Suche

Komponenten der semantischen Suche sind in einer Schicht über der vorhandenen Pipeline für die Abfrageausführung angeordnet. Die Rechtschreibkorrektur (im Diagramm nicht dargestellt) verbessert den Rückruf, indem Tippfehler in den einzelnen Abfragebegriffen korrigiert werden. Nachdem alle Analyseschritte durchgeführt wurden, ruft die Suchmaschine die Dokumente ab, die der Abfrage entsprechen, und bewertet sie mithilfe des [Standardbewertungsalgorithmus](index-similarity-and-scoring.md#similarity-ranking-algorithms). Dies ist entweder BM25 oder klassisch – je nachdem, wann der Dienst erstellt wurde. Bewertungsprofile werden in dieser Phase ebenfalls angewandt. 

Nachdem die besten 50 Übereinstimmungen empfangen wurden, wertet der [Algorithmus für die semantische Rangfolge](semantic-how-to-query-response.md) den Dokumentkorpus erneut aus. Die Ergebnisse können mehr als 50 Übereinstimmungen umfassen, es werden aber nur die ersten 50 erneut bewertet. Für die Priorisierung wendet der Algorithmus maschinelles Lernen und Lerntransfer an, um die Dokumente auf der Grundlage der Frage, wie gut das jeweilige Dokument zur Absicht der Abfrage passt, neu zu bewerten.

Zum Erstellen von Beschriftungen und Antworten werden Sprachdarstellungsmodelle verwendet. Innerhalb einer Beschriftung verwendet der Algorithmus ein Sprachmodell, das von Bing entwickelt wurde, um Beschriftungen mit Hervorhebungen zu extrahieren, mit denen die Abfrageergebnisse am besten zusammengefasst werden. Wenn die Suchabfrage eine Frage ist und Antworten angefordert wurden, identifiziert ein ähnliches Sprachmodell Textpassagen, die die Frage am besten beantworten, die von der Suchabfrage ausgedrückt wird.

:::image type="content" source="media/semantic-search-overview/semantic-query-architecture.png" alt-text="Semantikkomponenten in einer Abfragepipeline" border="true":::

## <a name="availability-and-pricing"></a>Verfügbarkeit und Preismodell

Die semantische Rangfolge ist über die [Registrierung](https://aka.ms/SemanticSearchPreviewSignup) für im Tarif „Standard“ (S1, S2, S3) erstellte Suchdienste in folgenden Regionen verfügbar: „USA, Norden-Mitte“, „USA, Westen“, „USA, Westen 2“, „USA, Osten 2“, „Europa, Norden“ und „Europa, Westen“. Ein vorhandener Suchdienst mit S1 oder höher in den angegebenen Regionen ist für die Vorschau zugelassen (es muss kein neuer Dienst erstellt werden).

Die Rechtschreibkorrektur ist in den gleichen Regionen verfügbar, aber auf keinen bestimmten Tarif beschränkt. Es ist auch keine Registrierung erforderlich. 

Zwischen dem Start der Vorschauversion am 2. März und dem 1. April werden Rechtschreibkorrektur und semantische Rangfolge kostenlos angeboten. Nach dem 1. April wird der Rechenaufwand für die Nutzung dieser Funktionen in Rechnung gestellt. Die Kosten liegen voraussichtlich bei etwa 500 USD/Monat für 250.000 Abfragen. Ausführliche Kosteninformationen finden Sie unter [Azure Cognitive Search – Preise](https://azure.microsoft.com/pricing/details/search/) sowie unter [Schätzen und Verwalten von Kosten](search-sku-manage-costs.md).

## <a name="next-steps"></a>Nächste Schritte

Ein neuer Abfragetyp ermöglicht die Rangfolge nach Relevanz sowie Antwortstrukturen für die semantische Suche. [Erstellen Sie eine Semantikabfrage](semantic-how-to-query-request.md), um mit der Verwendung zu beginnen. Weitere Informationen finden Sie auch in den folgenden Artikeln:

+ [Hinzufügen der Rechtschreibprüfung für Abfragebegriffe](speller-how-to-add.md)
+ [Semantische Rangfolge und Antworten](semantic-how-to-query-response.md)