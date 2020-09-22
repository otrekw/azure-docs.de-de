---
title: Abfrageanforderungen
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 09/10/2020
ms.author: aahi
ms.openlocfilehash: 23da214f4ffc9e589523f7ada00a77494183eb15
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90931668"
---
Verwenden Sie in der Abfrage den Parameter `@StartTime`, um Metrikdaten für einen bestimmten Zeitstempel zu erhalten. Er wird durch eine Zeichenfolge im Format `yyyy-MM-ddTHH:mm:ss` ersetzt. 

> [!IMPORTANT]
> Achten Sie darauf, dass von der Abfrage nur Metrikdaten **eines einzelnen Zeitstempels** zurückgegeben werden. Metrics Advisor führt die Abfrage für jeden Zeitstempel aus, um die entsprechenden Metrikdaten zu erhalten. So sollte beispielsweise eine Abfrage für eine Metrik mit *tagesbezogener* Granularität nur einen einzelnen Zeitstempel (etwa `2020-06-21T00:00:00Z`) enthalten, wenn die Abfrage einmal ausgeführt wird. 
