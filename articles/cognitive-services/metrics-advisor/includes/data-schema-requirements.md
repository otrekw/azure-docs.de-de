---
title: Datenschemaanforderungen
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: metrics-advisor
ms.topic: include
ms.date: 09/10/2020
ms.author: aahi
ms.openlocfilehash: 33bc52d3c334919a9e93d9666a24d85e3fe158b4
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91377600"
---
Metrics Monitor ist ein Dienst für die Anomalieerkennung in Zeitreihen sowie für die Diagnose und Analyse von Zeitreihen. Da es sich hierbei um einen KI-gestützten Dienst handelt, wird das verwendete Modell mit Ihren Daten trainiert. Der Dienst akzeptiert Tabellen mit aggregierten Daten und folgenden Spalten:

* **Measure** (erforderlich): Mindestens eine Spalte mit numerischen Werten.
* **Zeitstempel** (optional): Maximal eine Spalte vom Typ `DateTime` oder `String`. Wird diese Spalte nicht festgelegt, wird der Zeitstempel als Startzeit des jeweiligen Erfassungszeitraums festgelegt. Formatieren Sie den Zeitstempel wie folgt: `yyyy-MM-ddTHH:mm:ssZ`. 
  * **Der Zeitstempel muss der Granularität der Metrik entsprechen. Bei einer Tagesmetrik muss der Zeitstempel also beispielsweise die Stunde, Minute und Sekunde (`00:00:00`) enthalten.**
* **Dimension** (optional): Spalten können einen beliebigen Datentyp aufweisen. Achten Sie bei Verwendung großer Mengen von Spalten und Werten darauf, dass nicht übermäßig viele Dimensionen verarbeitet werden.

> [!Note]
> Für jede Metrik darf immer nur ein einzelner Zeitstempel pro Measure vorhanden sein, der einer Dimensionskombination entspricht. Aggregieren Sie Ihre Daten vor dem Onboarding, oder verwenden Sie die Abfrage, um die zu erfassenden Daten anzugeben.