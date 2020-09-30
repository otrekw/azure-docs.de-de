---
title: Abfrageanforderungen
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: metrics-advisor
ms.topic: include
ms.date: 09/10/2020
ms.author: aahi
ms.openlocfilehash: 68b5946cb21bef1c8979a94c8780682cfe9defa6
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91377448"
---
Verwenden Sie in der Abfrage den Parameter `@StartTime`, um Metrikdaten für einen bestimmten Zeitstempel zu erhalten. Er wird durch eine Zeichenfolge im Format `yyyy-MM-ddTHH:mm:ss` ersetzt. 

> [!IMPORTANT]
> Achten Sie darauf, dass von der Abfrage nur Metrikdaten **eines einzelnen Zeitstempels** zurückgegeben werden. Metrics Advisor führt die Abfrage für jeden Zeitstempel aus, um die entsprechenden Metrikdaten zu erhalten. So sollte beispielsweise eine Abfrage für eine Metrik mit *tagesbezogener* Granularität nur einen einzelnen Zeitstempel (etwa `2020-06-21T00:00:00Z`) enthalten, wenn die Abfrage einmal ausgeführt wird. 
