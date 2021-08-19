---
title: Was ist der Azure Metrics Advisor-Dienst?
titleSuffix: Azure Applied AI Services
description: Was ist Metrics Advisor?
services: cognitive-services
author: mrbullwinkle
manager: nitinme
ms.service: applied-ai-services
ms.subservice: metrics-advisor
ms.topic: overview
ms.date: 07/06/2021
ms.author: mbullwin
ms.openlocfilehash: 295c166ec80eba638798dd452e27de945e41e9a6
ms.sourcegitcommit: 192444210a0bd040008ef01babd140b23a95541b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/15/2021
ms.locfileid: "114341858"
---
# <a name="what-is-azure-metrics-advisor"></a>Was ist Azure Metrics Advisor? 

Metrics Advisor ist Teil von [Azure Applied AI Services](../../applied-ai-services/what-are-applied-ai-services.md) und nutzt künstliche Intelligenz zur Datenüberwachung und Anomalieerkennung in Zeitreihendaten. Der Dienst automatisiert die Anwendung von Modellen auf Ihre Daten und bietet eine Reihe von APIs sowie einen webbasierten Arbeitsbereich für die Datenerfassung, Anomalieerkennung und Diagnose, die ganz ohne Maschinen-Lernkenntnisse verwendet werden können. Entwickler können zusätzlich zum Dienst aiops-, präative Wartungs-und Business Monitor-Anwendungen erstellen. Metrics Advisor ermöglicht Folgendes:

* Analysieren mehrdimensionaler Daten aus mehreren Datenquellen
* Identifizieren und Korrelieren von Anomalien
* Konfigurieren und Optimieren des für Ihre Daten verwendeten Anomalieerkennungsmodells
* Diagnostizieren von Anomalien und Bereitstellen von Unterstützung bei der Ursachenanalyse

:::image type="content" source="media/metrics-advisor-overview.png" alt-text="Übersicht über Metrics Advisor":::

Diese Dokumentation enthält die folgenden Arten von Artikeln:
* In den [Schnellstarts](./Quickstarts/web-portal.md) finden Sie Schritt-für-Schritt-Anleitungen, mit denen Sie Aufrufe an den Dienst senden können und in kurzer Zeit Ergebnisse erhalten. 
* Die [Anleitungen](./how-tos/onboard-your-data.md) enthalten Anweisungen zur spezifischeren oder individuelleren Verwendung des Diensts.
* Die [konzeptionellen Artikel](glossary.md) enthalten ausführliche Erläuterungen der Funktionen und Features eines Diensts.

## <a name="connect-to-a-variety-of-data-sources"></a>Herstellen einer Verbindung mit unterschiedlichen Datenquellen

Metrics Advisor kann eine Verbindung mit zahlreichen Datenspeichern herstellen und daraus [mehrdimensionale Metrikdaten erfassen](how-tos/onboard-your-data.md). Zu diesen Datenspeichern zählen unter anderem folgende: SQL Server, Azure Blob Storage und MongoDB.

## <a name="easy-to-use-and-customizable-anomaly-detection"></a>Benutzerfreundliche und anpassbare Anomalieerkennung

* Metrics Advisor wählt automatisch das beste Modell für Ihre Daten aus. Dazu sind keinerlei Machine Learning-Kenntnisse erforderlich.
* Überwachen Sie automatisch alle Zeitreihen in [mehrdimensionalen Metriken](glossary.md#multi-dimensional-metric).
* Nutzen Sie die [Parameteroptimierung](how-tos/configure-metrics.md) und [interaktives Feedback](how-tos/anomaly-feedback.md), um das auf Ihre Daten angewendete Modell sowie zukünftige Anomalieerkennungsergebnisse anzupassen.

## <a name="real-time-notification-through-multiple-channels"></a>Echtzeitbenachrichtigungen über mehrere Kanäle

Im Falle einer Anomalieerkennung kann Metrics Advisor über mehrere Kanäle [Echtzeitbenachrichtigungen senden](how-tos/alerts.md). Hierzu können Hooks (beispielsweise E-Mail-Hooks, Webhooks, Teams-Hooks und Azure DevOps-Hooks) verwendet werden. Mit der flexiblen Warnungskonfiguration können Sie anpassen, wann und wo eine Benachrichtigung gesendet werden soll.

## <a name="smart-diagnostic-insights-by-analyzing-anomalies"></a>Analyse von Anomalien für intelligente Diagnoseerkenntnisse

### <a name="analyze-root-cause-into-specific-dimension"></a>Ursachenanalyse für eine bestimmte Dimension 

Metrics Advisor kombiniert Anomalien, die für dieselbe mehrdimensionale Metrik erkannt wurden, in einer Diagnosestruktur, damit Sie die Ursache für eine bestimmte Dimension analysieren können. Außerdem sind automatisierte analysierte Erkenntnisse verfügbar, indem der größte Beitrag der einzelnen Dimensionen analysiert wird. 

### <a name="cross-metrics-analysis-using-metrics-graph"></a>Metrikübergreifende Analyse mithilfe des Metrikdiagramms

Anhand eines [Metrikdiagramms](./how-tos/metrics-graph.md) kann die Beziehung zwischen Metriken abgelesen werden. Die metrikübergreifende Analyse kann aktiviert werden, um einen ungewöhnlichen Status aller zugehörigen Metriken in einer ganzheitlichen Ansicht zu ermitteln. Dadurch lässt sich letztendlich die Ursache eines Problems bestimmen.

Nähere Informationen finden Sie im Abschnitt zur [Diagnose eines Incidents](./how-tos/diagnose-an-incident.md).

## <a name="typical-workflow"></a>Typischer Workflow

Der Workflow ist ganz einfach: Nach dem Onboarding Ihrer Daten können Sie die Anomalieerkennung optimieren und Konfigurationen für Ihr Szenario erstellen.

1. [Erstellen Sie eine Azure-Ressource](https://go.microsoft.com/fwlink/?linkid=2142156) für Metrics Advisor. 
2. Erstellen Sie Ihren ersten Monitor über das Webportal.
    1. [Durchführen des Onboardings für Ihre Daten](./how-tos/onboard-your-data.md)
    2. [Optimieren der Konfiguration zur Anomalieerkennung](./how-tos/configure-metrics.md)
    3. [Abonnieren von Anomalien für Benachrichtigungen](./how-tos/alerts.md)
    4. [Anzeigen von Diagnoseerkenntnissen](./how-tos/diagnose-an-incident.md)
3. Anpassen Ihrer Instanz mithilfe der REST-API

## <a name="video"></a>Video
* [Einführung in Metrics Advisor](https://www.youtube.com/watch?v=0Y26cJqZMIM)
* [Neu bei Cognitive Services](https://www.youtube.com/watch?v=7tCLJHdBZgM)

## <a name="data-retention--limitation"></a>Datenaufbewahrung und Einschränkung: 

Bei Metrics Advisor werden höchstens **10.000** Zeitintervalle ([was ist ein Intervall?](tutorials/write-a-valid-query.md#what-is-an-interval)) ab dem aktuellen Zeitstempel beibehalten. Dies gilt unabhängig davon, ob Daten verfügbar sind oder nicht. Daten außerhalb dieses Zeitfensters werden gelöscht.  Zuordnung der Datenaufbewahrung zur Anzahl von Tagen für unterschiedliche Metrikgranularität: 

| Granularität (Min.) |    Aufbewahrung (Tage) |
|------------------| ------------------|
|  1 | 6,94 |
|  5 | 34,72|
| 15 | 104,1|
| 60 (=stündlich) | 416,67 |
| 1440 (=täglich)|10000,00|

Zudem müssen weitere Einschränkungen berücksichtigt werden. Nähere Einzelheiten finden Sie in den [FAQ](faq.yml#what-are-the-data-retention-and-limitations-of-metrics-advisor-). 

## <a name="next-steps"></a>Nächste Schritte

* Sehen Sie sich eine Schnellstartanleitung an: [Überwachen Ihrer ersten Metrik im Web](quickstarts/web-portal.md)
* Sehen Sie sich eine Schnellstartanleitung an: [Verwenden der REST-APIs zum Anpassen Ihrer Lösung](./quickstarts/rest-api-and-client-library.md)