---
title: Datenschemaanforderungen
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: mrbullwinkle
manager: nitinme
ms.service: applied-ai-services
ms.subservice: metrics-advisor
ms.topic: include
ms.date: 09/10/2020
ms.author: mbullwin
ms.openlocfilehash: 90a175c4c92f2ea3b0d3f9ec8015dd3ef27036d7
ms.sourcegitcommit: 34feb2a5bdba1351d9fc375c46e62aa40bbd5a1f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111893994"
---
Metrics Advisor ist ein Dienst für die Anomalieerkennung in Zeitreihen sowie für die Diagnose und Analyse von Zeitreihen. Da es sich hierbei um einen KI-gestützten Dienst handelt, wird das verwendete Modell mit Ihren Daten trainiert. Der Dienst akzeptiert Tabellen mit aggregierten Daten und folgenden Spalten:

* **Measure** (erforderlich): Mindestens eine Spalte mit numerischen Werten.
* **Zeitstempel** (optional): Maximal eine Spalte vom Typ `DateTime` oder `String`. Wird diese Spalte nicht festgelegt, wird der Zeitstempel als Startzeit des jeweiligen Erfassungszeitraums festgelegt. Formatieren Sie den Zeitstempel wie folgt: `yyyy-MM-ddTHH:mm:ssZ`. 
  * **Der Zeitstempel muss der Granularität der Metrik entsprechen. Bei einer Tagesmetrik muss der Zeitstempel also beispielsweise die Stunde, Minute und Sekunde (`00:00:00`) enthalten.**
* **Dimension** (optional): Spalten können einen beliebigen Datentyp aufweisen. Achten Sie bei Verwendung großer Mengen von Spalten und Werten darauf, dass nicht übermäßig viele Dimensionen verarbeitet werden.

> [!Note]
> Für jede Metrik darf immer nur ein einzelner Zeitstempel pro Measure vorhanden sein, der einer Dimensionskombination entspricht. Aggregieren Sie Ihre Daten vor dem Onboarding, oder verwenden Sie die Abfrage, um die zu erfassenden Daten anzugeben.