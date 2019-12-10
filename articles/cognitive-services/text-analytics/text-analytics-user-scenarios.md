---
title: Beispielbenutzerszenarien für die Textanalyse-API
titleSuffix: Azure Cognitive Services
description: In diesem Artikel erfahren Sie mehr über einige gängige Szenarien für die Integration der Textanalyse-API in Ihre Dienste und Prozesse.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: overview
ms.date: 11/29/2019
ms.author: aahi
ms.openlocfilehash: 027e6ec829e9de9956451e48e5f9e1cdd749f9f7
ms.sourcegitcommit: 48b7a50fc2d19c7382916cb2f591507b1c784ee5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/02/2019
ms.locfileid: "74689335"
---
# <a name="example-user-scenarios-for-the-text-analytics-api"></a>Beispielbenutzerszenarien für die Textanalyse-API

Die Textanalyse-API ist ein Clouddienst für die erweiterte Verarbeitung natürlicher Sprache von Text. Dieser Artikel beschreibt einige Beispiele von Anwendungsfällen für die Integration der API in Ihre Unternehmenslösungen und -prozesse. 

## <a name="analyze-survey-results"></a>Analysieren von Befragungsergebnissen

Gewinnen Sie Erkenntnisse aus den Ergebnissen von Kunden- und Mitarbeiterbefragungen, indem Sie die Rohtextantworten mithilfe der Standpunktanalyse verarbeiten. Aggregieren Sie die Ergebnisse für die Analyse, Nachbereitung und Förderung der Motivation.

![Abbildung mit einer Beschreibung der Standpunktanalyse bei Kunden- und Mitarbeiterbefragungen.](media/use-cases/survey-results.svg)

## <a name="analyze-recorded-inbound-customer-calls"></a>Analysieren aufgezeichneter eingehender Kundenanrufe

Gewinnen Sie Erkenntnisse aus Anrufen beim Kundendienst mithilfe der Sprachsynthese, Standpunktanalyse und Schlüsselbegriffserkennung. Zeigen Sie die Ergebnisse im Power BI-Dashboard oder in einem Portal an, um Kunden besser zu verstehen, Trends im Kundendienst aufzuzeigen und die Kundenbindung zu fördern. Senden Sie API-Anforderungen als Batch für die Berichterstellung oder in Echtzeit für Eingriffe. Sehen Sie sich den Beispielcode auf [GitHub](https://github.com/rlagh2/callcenteranalytics) an.

![Abbildung mit einer Beschreibung, wie das Gewinnen von Erkenntnissen aus Anrufen beim Kundendienst mithilfe der Standpunktanalyse automatisiert werden kann](media/use-cases/azure-inbound.svg)

## <a name="process-and-categorize-support-incidents"></a>Verarbeiten und Kategorisieren von Supportvorfällen

Verwenden Sie die Schlüsselbegriffs- und Entitätserkennung zum Verarbeiten von Supportanfragen, die in einem unstrukturierte Textformat eingehen. Verwenden Sie die extrahierten Begriffe und Entitäten, um die Anforderungen für die Ressourcenplanung und Trendanalyse zu kategorisieren.

![Eine Abbildung mit einer Beschreibung, wie die Schlüsselbegriffs- und Entitätserkennung verwendet werden kann, um Schadensberichte und Trends zu kategorisieren](media/use-cases/support-incidents.svg)

## <a name="monitor-your-products-social-media-feeds"></a>Überwachen der Feeds in sozialen Medien zu Ihrem Produkt

Überwachen Sie auf der Twitter- oder Facebook-Seite Ihres Produkts das Benutzerfeedback. Nutzen Sie die Daten, um die Stimmung der Kunden bei der Einführung neuer Produkte zu analysieren, Schlüsselbegriffe zu Funktionen und Funktionswünschen zu extrahieren oder Kundenbeschwerden sofort zu bearbeiten. Sehen Sie sich die [Microsoft Flow-Beispielvorlage](https://flow.microsoft.com/galleries/public/templates/2680d2227d074c4d901e36c66e68f6f9/run-sentiment-analysis-on-tweets-and-push-results-to-a-power-bi-dataset/) an.

![Abbildung mit einer Beschreibung, wie Sie das Feedback zu Ihrem Produkt und Unternehmen in sozialen Medien mithilfe der Schlüsselbegriffserkennung überwachen können](media/use-cases/social-feed.svg)

## <a name="next-steps"></a>Nächste Schritte

* [Worum handelt es sich bei der Textanalyse-API?](overview.md)
* [Senden einer Anforderung an die Textanalyse-API mit der Clientbibliothek](quickstarts/text-analytics-sdk.md)
