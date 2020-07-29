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
ms.openlocfilehash: cd6ea6d4967e024ddf88fb9572d5efae8b7a7815
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86495334"
---
### <a name="property-limits"></a>Eigenschaftsgrenzwerte

Azure Time Series Insights-Eigenschaftsgrenzwerte wurden von einer maximalen Obergrenze von 800 in Gen1 auf 1.000 erhöht. Die bereitgestellten Ereigniseigenschaften verfügen über entsprechende JSON-, CSV- und Diagrammspalten, die Sie im [Azure Time Series Insights Gen2-Explorer](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-update-quickstart) anzeigen können.

| SKU | Maximale Eigenschaften |
| --- | --- |
| Gen2 (L1) | 1\.000 Eigenschaften (Spalten) |
| Gen1 (S1) | 600 Eigenschaften (Spalten) |
| Gen1 (S2) | 800 Eigenschaften (Spalten) |

### <a name="event-sources"></a>Ereignisquellen

Maximal zwei Ereignisquellen werden pro Instanz unterstützt.

* Informieren Sie sich über das [Hinzufügen einer Event Hub-Quelle](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-eventhub).
* Konfigurieren [einer IoT-Hub-Quelle](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-iothub).

Standardmäßig [unterstützen Gen2-Umgebungen Eingangsraten](https://docs.microsoft.com/azure/time-series-insights/concepts-streaming-throughput-limitations) von bis zu **1 Megabyte pro Sekunde (MB/s) pro Umgebung**. Kunden können ihre Umgebungen bei Bedarf auf bis zu **16 MB/s** Durchsatz skalieren. Darüber hinaus besteht pro Partition ein Grenzwert von **0,5 MB/s**.

### <a name="api-limits"></a>API-Grenzwerte

REST-API-Grenzwerte für Azure Time Series Insights Gen2 sind in der [Referenzdokumentation zur REST-API](https://docs.microsoft.com/rest/api/time-series-insights/preview#limits-1) angegeben.
