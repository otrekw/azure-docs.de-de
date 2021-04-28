---
title: Was ist die multivariate Anomalieerkennungs-API?
titleSuffix: Azure Cognitive Services
description: Übersicht der öffentlichen Vorschau der neuen multivariaten Anomalieerkennungs-APIs.
services: cognitive-services
author: mrbullwinkle
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: conceptual
ms.date: 04/01/2021
ms.author: mbullwin
keywords: Anomalieerkennung, maschinelles Lernen, Algorithmen
ms.openlocfilehash: 63ad63cd57dce5f503e317e8c6330dca0325ba05
ms.sourcegitcommit: b4fbb7a6a0aa93656e8dd29979786069eca567dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2021
ms.locfileid: "107318823"
---
# <a name="multivariate-time-series-anomaly-detection-public-preview"></a>Multivariate Zeitreihen-Anomalieerkennung (öffentliche Vorschau)

Mit der ersten Version der Anomalieerkennung von Azure Cognitive Services konnten Sie Metriküberwachungslösungen mit den einfach zu verwendenden [univariaten Zeitreihen-Anomalieerkennungs-APIs](overview.md) erstellen. Durch die Möglichkeit der individuellen Analyse von Zeitreihen bietet die univariate Anomalieerkennung Einfachheit und Skalierbarkeit.

Die neuen **APIs für die multivariate Anomalieerkennung** erweitern die Möglichkeiten von Entwicklern durch die einfache Integration fortschrittlicher KI zur Erkennung von Anomalien in Metrikgruppen ganz ohne Machine Learning-Kenntnisse oder gekennzeichnete Daten. Abhängigkeiten und gegenseitige Wechselwirkungen zwischen bis zu 300 verschiedenen Signalen werden jetzt automatisch als Schlüsselfaktoren gewertet. Diese neue Funktion hilft Ihnen, Ihre komplexen Systeme wie Softwareanwendungen, Server, Fertigungssysteme, Raumfahrzeuge oder schlicht Ihr Unternehmen proaktiv vor Ausfällen zu schützen.

Stellen Sie sich 20 Sensoren eines Automotors vor, die 20 verschiedene Signale wie Vibration, Temperatur, Kraftstoffdruck usw. generieren. Die Messwerte dieser Signale geben Ihnen einzeln möglicherweise nicht allzu viel Auskunft über Probleme auf Systemebene, aber zusammen können sie die Integrität des Motors darstellen. Wenn die Interaktion dieser Signale außerhalb des üblichen Bereichs abweicht, kann das Feature zur multivariaten Anomalieerkennung die Anomalie wie ein erfahrener Experte erkennen. Die zugrunde liegenden KI-Modelle werden mit Ihren Daten trainiert und angepasst, sodass sie die besonderen Anforderungen Ihres Unternehmens verstehen. Mit den neuen APIs in der Anomalieerkennung können Entwickler jetzt problemlos die Funktionen zur Anomalieerkennung in multivariaten Zeitreihen in Predictive Maintenance-Lösungen, AIOps-Überwachungslösungen für komplexe Unternehmenssoftware oder Business Intelligence-Tools integrieren.

## <a name="when-to-use-multivariate-versus-univariate"></a>Verwendung von **multivariate** gegenüber **univariate**

Verwenden Sie die univariaten Anomalieerkennungs-APIs, wenn Ihr Ziel die Erkennung von Anomalien gegenüber einem normalen Muster für jede einzelne Zeitreihe ist, ausgehend ausschließlich von deren eigenen Verlaufsdaten. Beispiele: Sie möchten Anomalien im täglichen Umsatz im Ausgang von den Umsatzdaten selbst oder eine CPU-Spitze ausschließlich auf der Grundlage von CPU-Daten erkennen.
- `POST /anomalydetector/v1.0/timeseries/last/detect`
- `POST /anomalydetector/v1.0/timeseries/batch/detect`
- `POST /anomalydetector/v1.0/timeseries/changepoint/detect`

![Zeitreihen-Liniendiagramm mit den fluktuierenden Werten einer einzelnen Variable, die durch eine blaue Linie dargestellt werden, und durch orangefarbene Kreise gekennzeichnete Anomalien](./media/anomaly_detection2.png)

Verwenden Sie die multivariaten Anomalieerkennungs-APIs unten, wenn es Ihr Ziel ist, Anomalien auf der Systemebene anhand einer Gruppe von Zeitreihendaten zu erkennen. Insbesondere, wenn eine einzelne Zeitreihe nicht viel verrät und Sie alle Signale (eine Gruppe von Zeitreihen) holistisch betrachten müssen, um ein Problem auf Systemebene zu bestimmen. Beispiel: Sie besitzen ein teures physisches Objekt wie ein Flugzeug, Ausrüstung auf einer Ölplattform oder einen Satelliten. Jedes dieser Objekte verfügt über Dutzende oder Hunderte verschiedener Sensortypen. Sie müssten sich die Zeitreihensignale aller dieser Sensoren ansehen, um zu entscheiden, ob es ein Problem auf Systemebene gibt.

- `POST /anomalydetector/v1.1-preview/multivariate/models`
- `GET /anomalydetector/v1.1-preview/multivariate/models[?$skip][&$top]`
- `GET /anomalydetector/v1.1-preview/multivariate/models/{modelId}`
- `POST/anomalydetector/v1.1-preview/multivariate/models/{modelId}/detect`
- `GET /anomalydetector/v1.1-preview/multivariate/results/{resultId}`
- `DELETE /anomalydetector/v1.1-preview/multivariate/models/{modelId}`
- `GET /anomalydetector/v1.1-preview/multivariate/models/{modelId}/export`

![Liniendiagramme mehrerer Zeitreihen für Variablen von: Vibration, Temperatur, Druck, Geschwindigkeit, Rotationsgeschwindigkeit. Die Anomalien sind orange hervorgehoben.](./media/multivariate-graph.png)

## <a name="region-support"></a>Unterstützung für Regionen

Die öffentliche Vorschauversion der multivariaten Anomalieerkennung ist derzeit in drei Regionen verfügbar: „USA, Westen 2“, „USA, Osten 2“ und „Europa, Westen“.

## <a name="algorithms"></a>Algorithmen

- [Multivariate Zeitreihen-Anomalieerkennung über ein Graph-Aufmerksamkeitsnetzwerk](https://arxiv.org/abs/2009.02040)

## <a name="join-the-anomaly-detector-community"></a>Beitreten zur Anomalieerkennungs-Community

- Beitreten zur [Ratgebergruppe für die Anomalieerkennung auf Microsoft Teams](https://aka.ms/AdAdvisorsJoin)

## <a name="next-steps"></a>Nächste Schritte

- [Schnellstarts](./quickstarts/client-libraries-multivariate.md).
- [Bewährte Methoden:](./concepts/best-practices-multivariate.md)In diesem Artikel werden empfohlene Muster für die Verwendung mit den multivariaten APIs beschrieben.
