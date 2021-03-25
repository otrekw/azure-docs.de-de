---
title: Abfrageanforderungen
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: mrbullwinkle
manager: nitinme
ms.service: cognitive-services
ms.subservice: metrics-advisor
ms.topic: include
ms.date: 09/30/2020
ms.author: mbullwin
ms.openlocfilehash: 777e3261bd6f842861ed6622b78589eedaaf65d7
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "92043198"
---
Verwenden Sie in der Abfrage den Parameter `@StartTime`, um Metrikdaten für einen einzelnen Zeitstempel zu erhalten. Metrics Advisor ersetzt den Parameter durch eine Formatzeichenfolge vom Typ `yyyy-MM-ddTHH:mm:ss`, wenn die Abfrage ausgeführt wird.

> [!IMPORTANT]
> Die Abfrage sollte zu jedem Zeitstempel höchstens einen Datensatz für jede Dimensionskombination zurückgeben. Alle von der Abfrage zurückgegebenen Datensätze müssen dieselben Zeitstempel aufweisen. Metrics Advisor führt diese Abfrage für jeden Zeitstempel aus, um die Daten zu erfassen. Weitere Informationen und Beispiele finden Sie im Abschnitt mit [häufig gestellten Fragen zu Abfragen](../faq.md#how-do-i-write-a-valid-query-for-ingesting-my-data). 