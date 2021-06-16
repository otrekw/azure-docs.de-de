---
title: Abfrageanforderungen
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: mrbullwinkle
manager: nitinme
ms.service: applied-ai-services
ms.subservice: metrics-advisor
ms.topic: include
ms.date: 09/30/2020
ms.author: mbullwin
ms.openlocfilehash: c7d038153385c2d36d48f660a9a4e2bab29a3c45
ms.sourcegitcommit: 34feb2a5bdba1351d9fc375c46e62aa40bbd5a1f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111893996"
---
Verwenden Sie in der Abfrage den Parameter `@StartTime`, um Metrikdaten für einen einzelnen Zeitstempel zu erhalten. Metrics Advisor ersetzt den Parameter durch eine Formatzeichenfolge vom Typ `yyyy-MM-ddTHH:mm:ss`, wenn die Abfrage ausgeführt wird.

> [!IMPORTANT]
> Die Abfrage sollte zu jedem Zeitstempel höchstens einen Datensatz für jede Dimensionskombination zurückgeben. Alle von der Abfrage zurückgegebenen Datensätze müssen dieselben Zeitstempel aufweisen. Metrics Advisor führt diese Abfrage für jeden Zeitstempel aus, um die Daten zu erfassen. Weitere Informationen und Beispiele finden Sie im Abschnitt mit [häufig gestellten Fragen zu Abfragen](../faq.md#how-do-i-write-a-valid-query-for-ingesting-my-data). 