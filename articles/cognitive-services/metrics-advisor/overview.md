---
title: Was ist der Azure Metrics Advisor-Dienst?
titleSuffix: Azure Applied AI Services
description: Was ist Metrics Advisor?
services: cognitive-services
author: mrbullwinkle
manager: nitinme
ms.service: cognitive-services
ms.subservice: metrics-advisor
ms.topic: overview
ms.date: 09/14/2020
ms.author: mbullwin
ms.openlocfilehash: 34ab524cfc00cf4f74d632883ffeb18b904342a6
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110376845"
---
# <a name="what-is-azure-metrics-advisor-preview"></a>Was ist Azure Metrics Advisor (Vorschau)? 

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

## <a name="real-time-alerts-through-multiple-channels"></a>Echtzeitwarnungen über mehrere Kanäle

Im Falle einer Anomalieerkennung kann Metrics Advisor über mehrere Kanäle [Echtzeitwarnungen senden](how-tos/alerts.md). Hierzu können Hooks (beispielsweise E-Mail-Hooks, Webhooks und Azure DevOps-Hooks) verwendet werden. Dank flexibler Warnungsregeln können Sie anpassen, welche Warnungen wohin gesendet werden sollen.

## <a name="smart-diagnostic-insights-by-analyzing-anomalies"></a>Analyse von Anomalien für intelligente Diagnoseerkenntnisse

Analysieren Sie Anomalien, die für mehrdimensionale Metriken erkannt wurden, und generieren Sie [intelligente Diagnoseerkenntnisse](how-tos/diagnose-incident.md), die unter anderem Aufschluss über die wahrscheinlichste Grundursache geben sowie Diagnosestrukturen enthalten und die Anzeige von Detailinformationen zu Metriken ermöglichen. Durch Konfigurieren des [Metrikgraphen](how-tos/metrics-graph.md) können Sie die metrikübergreifende Analyse aktivieren, um Sie bei der Visualisierung von Vorfällen zu unterstützen.


## <a name="typical-workflow"></a>Typischer Workflow

Der Workflow ist ganz einfach: Nach dem Onboarding Ihrer Daten können Sie die Anomalieerkennung optimieren und Konfigurationen für Ihr Szenario erstellen.

1. [Erstellen Sie eine Azure-Ressource](https://go.microsoft.com/fwlink/?linkid=2142156) für Metrics Advisor. 
2. Erstellen Sie Ihren ersten Monitor über das Webportal.
    1. Durchführen des Onboardings für Ihre Daten
    2. Optimieren der Anomalieerkennung
    3. Abonnieren von Benachrichtigungen
    4. Anzeigen von Diagnoseerkenntnissen
3. Anpassen Ihrer Instanz mithilfe der REST-API

## <a name="next-steps"></a>Nächste Schritte

* Sehen Sie sich eine Schnellstartanleitung an: [Überwachen Ihrer ersten Metrik im Web](quickstarts/web-portal.md)
* Sehen Sie sich eine Schnellstartanleitung an: [Verwenden der REST-APIs zum Anpassen Ihrer Lösung](./quickstarts/rest-api-and-client-library.md)
