---
title: include file
description: include file
services: digital-twins
ms.service: digital-twins
ms.topic: include
ms.date: 02/03/2020
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.custom: include file
ms.openlocfilehash: 11469d992e0f5669cd3fc1e3864627dd0b8ae23d
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "81263341"
---
Im Folgenden werden die wichtigsten Grenzwerte für die allgemeine Verfügbarkeit zusammengefasst.

### <a name="sku-ingress-rates-and-capacities"></a>SKU: Eingangsraten und Kapazitäten

Eingangsraten und Kapazitäten der S1 und S2 SKU bieten Flexibilität bei der Konfiguration einer neuen Time Series Insights-Umgebung. Die SKU-Kapazität gibt die tägliche Eingangsrate basierend auf der Anzahl der Ereignisse oder der gespeicherten Bytes an – je nachdem, welcher Wert zuerst erreicht wird. Beachten Sie, dass der Eingang *pro Minute* gemessen wird und dass mithilfe des Tokenbucketalgorithmus eine **Drosselung** angewendet wird. Der Eingang wird in Blöcken von je 1 KB gemessen. Ein Ereignis mit einer tatsächlichen Größe von 0,8 KB wird als ein Ereignis gemessen, ein 2,6 KB großes Ereignis zählt als drei Ereignisse.

| Kapazität der SKU „S1“ | Eingangsrate | Maximale Speicherkapazität
| --- | --- | --- |
| 1 | 1 GB (1 Mio. Ereignisse) pro Tag | 30 GB (30 Mio. Ereignisse) pro Monat |
| 10 | 10 GB (10 Mio. Ereignisse) pro Tag | 300 GB (300 Mio. Ereignisse) pro Monat |

| Kapazität der SKU „S2“ | Eingangsrate | Maximale Speicherkapazität
| --- | --- | --- |
| 1 | 10 GB (10 Mio. Ereignisse) pro Tag | 300 GB (300 Mio. Ereignisse) pro Monat |
| 10 | 100 GB (100 Mio. Ereignisse) pro Tag | 3 TB (3 Mrd. Ereignisse) pro Monat |

> [!NOTE]
> Kapazitäten werden linear skaliert, sodass eine S1-SKU mit der Kapazität „2“ eine Erfassungsrate von 2 GB (2 Mio. Ereignisse) pro Tag und 60 GB (60 Mio. Ereignisse) pro Monat unterstützt.

S2 SKU-Umgebungen unterstützen wesentlich mehr Ereignisse pro Monat und haben eine deutlich höhere Eingangskapazität.

| SKU  | Ereignisanzahl pro Monat  | Ereignisanzahl pro Minute | Ereignisgröße pro Minute  |
|---------|---------|---------|---------|---------|
| S1     |   30 Millionen   |  720    |  720 KB   |
 |S2     |   300 Millionen   | 7\.200   | 7\.200 KB  |

### <a name="property-limits"></a>Eigenschaftsgrenzwerte

Eigenschaftsgrenzwerte der allgemeinen Verfügbarkeit (GA) sind von der ausgewählten SKU-Umgebung abhängig. Die bereitgestellten Ereigniseigenschaften verfügen über entsprechende JSON-, CSV- und Diagrammspalten, die im [Time Series Insights-Explorer](https://docs.microsoft.com/azure/time-series-insights/time-series-quickstart) angezeigt werden können.

| SKU | Maximale Eigenschaften |
| --- | --- |
| S1 | 600 Eigenschaften (Spalten) |
| S2 | 800 Eigenschaften (Spalten) |

### <a name="event-sources"></a>Ereignisquellen

Maximal zwei Ereignisquellen werden pro Instanz unterstützt. 

* Informieren Sie sich über das [Hinzufügen einer Event Hub-Quelle](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-eventhub).
* Konfigurieren [einer IoT-Hub-Quelle](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-iothub).

### <a name="api-limits"></a>API-Grenzwerte

REST-API-Grenzwerte für Time Series Insights GA (allgemeine Verfügbarkeit) sind in der [Referenzdokumentation zur REST-API](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api#limits) angegeben.