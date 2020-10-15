---
title: Vermeiden von Dienstunterbrechungen bei Azure Stream Analytics-Aufträgen
description: Dieser Artikel bietet eine Anleitung für die Resilienz von Stream Analytics-Aufträgen bei Upgrades.
author: jseb225
ms.author: sidram
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: how-to
ms.date: 06/21/2019
ms.custom: seodec18
ms.openlocfilehash: 8d62f87a6bf33bbbd85b68c9d2e0c6f4a93adf16
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "86044429"
---
# <a name="guarantee-stream-analytics-job-reliability-during-service-updates"></a>Gewährleisten der Zuverlässigkeit von Stream Analytics-Aufträgen während Dienstupdates

Ein vollständig verwalteter Dienst bietet unter anderem die Möglichkeit, dass er sich im Handumdrehen um neue Dienstfunktionen und Verbesserungen erweitern lässt. Somit kann für Stream Analytics wöchentlich (oder häufiger) eine Bereitstellung von Dienstupdates durchgeführt werden. Trotz vieler und gründlicher Tests lässt sich nicht gänzlich ausschließen, dass ein vorhandener, aktuell ausgeführter Auftrag aufgrund eines Fehlers unter Umständen unterbrochen wird. Wenn Sie unternehmenskritische Aufträge ausführen, darf dieses Risiko nicht bestehen. Sie können es verringern, indem Sie sich an das Azure-Modell für **[Regionspaare](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)** halten. 

## <a name="how-do-azure-paired-regions-address-this-concern"></a>Wie wirken Azure-Regionspaare diesem Risiko entgegen?

Mit Stream Analytics wird gewährleistet, dass Aufträge in Regionspaaren in separaten Batches aktualisiert werden. Dies führt zu einer ausreichenden Zeitspanne zwischen den Updates, sodass mögliche Probleme ermittelt und behoben werden können.

_Mit Ausnahme der Region „Indien, Mitte“_ (deren gekoppelte Region „Indien, Süden“ für Stream Analytics nicht verfügbar ist), erfolgt die Bereitstellung eines Updates für Stream Analytics in einer Gruppe von Regionspaaren nicht gleichzeitig. Bereitstellungen in mehreren Regionen **in derselben Gruppe** können **gleichzeitig** erfolgen.

Der Artikel über **[Verfügbarkeit und Regionspaare](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)** enthält die aktuellsten Informationen darüber, welche Regionen zugeordnet werden.

Es wird empfohlen, identische Aufträge für beide gekoppelten Regionen bereitzustellen. Sie sollten diese Aufträge anschließend [überwachen](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-set-up-alerts#scenarios-to-monitor), damit Sie benachrichtigt werden, sobald etwas Unerwartetes passiert. Wenn einer dieser Aufträge nach einem Dienstupdate für Stream Analytics mit dem Status [Fehler](https://docs.microsoft.com/azure/stream-analytics/job-states) endet, können Sie den Kundendienst kontaktieren, damit dieser Ihnen dabei hilft, die Grundursache für diesen Fehler zu finden. Sie sollten außerdem ein Failover für mögliche Downstreamconsumer auf die fehlerfreie Auftragsausgabe ausführen.

## <a name="next-steps"></a>Nächste Schritte

* [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
* [Erste Schritte mit Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skalieren von Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
* [Referenz zur Stream Analytics-Abfragesprache](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Referenz zur REST-API für die Stream Analytics-Verwaltung](https://msdn.microsoft.com/library/azure/dn835031.aspx)
