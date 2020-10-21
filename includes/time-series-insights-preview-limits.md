---
title: include file
description: include file
services: digital-twins
ms.service: digital-twins
ms.topic: include
ms.date: 07/09/2020
author: deepakpalled
ms.author: dpalled
manager: diviso
ms.custom: include file
ms.openlocfilehash: 7259e1981f873c8385a02fe4f353dcdda495f823
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91287411"
---
### <a name="property-limits"></a>Eigenschaftsgrenzwerte

Azure Time Series Insights-Eigenschaftsgrenzwerte wurden von einer maximalen Obergrenze von 800 in Gen1 auf 1.000 erhöht. Die bereitgestellten Ereigniseigenschaften verfügen über entsprechende JSON-, CSV- und Diagrammspalten, die Sie im [Azure Time Series Insights Gen2-Explorer](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-update-quickstart) anzeigen können.

| SKU | Maximale Eigenschaften |
| --- | --- |
| Gen2 (L1) | 1\.000 Eigenschaften (Spalten) |
| Gen1 (S1) | 600 Eigenschaften (Spalten) |
| Gen1 (S2) | 800 Eigenschaften (Spalten) |

### <a name="streaming-ingestion"></a>Streamingerfassung

* Es gibt maximal zwei [Ereignisquellen](../articles/time-series-insights/concepts-streaming-ingestion-event-sources.md) pro Umgebung.

* Die bewährten Methoden und den allgemeinen Leitfaden für Ereignisquellen finden Sie [hier](../articles/time-series-insights/concepts-streaming-ingestion-event-sources.md#streaming-ingestion-best-practices).

* Standardmäßig kann Azure Time Series Insights Gen2 eingehende Daten mit einer Rate von **bis zu 1 MB pro Sekunde (MBit/s) pro Azure Time Series Insights Gen2-Umgebung** erfassen. Es gelten zusätzliche Einschränkungen [pro Hubpartition](../articles/time-series-insights/concepts-streaming-ingress-throughput-limits.md#hub-partitions-and-per-partition-limits). Raten von bis zu 8 MBit/s können durch Übermitteln eines Supporttickets über das Azure-Portal bereitgestellt werden. Weitere Informationen finden Sie unter [Durchsatzlimits für Datenstromerfassung](../articles/time-series-insights/concepts-streaming-ingress-throughput-limits.md).

### <a name="api-limits"></a>API-Grenzwerte

REST-API-Grenzwerte für Azure Time Series Insights Gen2 sind in der [Referenzdokumentation zur REST-API](https://docs.microsoft.com/rest/api/time-series-insights/preview#limits-1) angegeben.
