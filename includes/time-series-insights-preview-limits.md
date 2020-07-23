---
title: include file
description: include file
services: digital-twins
ms.service: digital-twins
ms.topic: include
ms.date: 02/07/2020
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.custom: include file
ms.openlocfilehash: 1171c07a754b056fb9df2cee73ddf224427037cc
ms.sourcegitcommit: f7e160c820c1e2eb57dc480b2a8fd6bef7053e91
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2020
ms.locfileid: "86277954"
---
### <a name="general-availability-and-preview-comparison"></a>Vergleich der allgemeinen Verfügbarkeit und der Vorschauversion

In der folgenden Tabelle sind einige wesentliche Unterschiede zwischen Azure Time Series Insights GA (allgemeine Verfügbarkeit) und Instanzen der Vorschauversion zusammengefasst.

| | Allgemein verfügbar | Vorschau |
| --- | --- | ---|
| **Bürger erster Klasse** | Ereigniszentriert | Zeitreihenzentriert |
| **Semantische Argumentation** | Low-Level (Verweisdaten) | Allgemein (Modelle) |
| **Datenkontextualisierung** | Nicht-Geräteebene | Geräte- und Nicht-Geräteebene |
| **Computelogikspeicher** | Nein | In Typvariablen gespeichert, die Teil des Modells sind |
| **Speicher und Zugriffssteuerung** | Nein | Über das Modell aktiviert |
| **Aggregationen/Sampling** | Nein | Ereignisgewichtet und zeitgewichtet |
| **Signalnachbildung** | Nein | Interpolation |
| **Produktion abgeleiteter Zeitreihen** | Nein | Ja, Zusammenführungen und Joins |
| **Sprachflexibilität** | Nicht zusammensetzbar | Zusammensetzbar |
| **Ausdruckssprache** | Prädikatszeichenfolge | Zeitreihenausdrücke (Prädikatszeichenfolgen, Werte, Ausdrücke und Funktionen) |

### <a name="property-limits"></a>Eigenschaftsgrenzwerte

Time Series Insights-Eigenschaftsgrenzwerte wurden von einer maximalen Obergrenze von 800 in GA (allgemeine Verfügbarkeit) auf 1.000 erhöht. Die bereitgestellten Ereigniseigenschaften verfügen über entsprechende JSON-, CSV- und Diagrammspalten, die Sie im [Time Series Insights Preview-Explorer](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-update-quickstart) anzeigen können.

| SKU | Maximale Eigenschaften |
| --- | --- |
| PAYG-Vorschau | 1\.000 Eigenschaften (Spalten) |
| GA S1 | 600 Eigenschaften (Spalten) |
| GA S2 | 800 Eigenschaften (Spalten) |

### <a name="event-sources"></a>Ereignisquellen

Maximal zwei Ereignisquellen werden pro Instanz unterstützt. 

* Informieren Sie sich über das [Hinzufügen einer Event Hub-Quelle](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-eventhub).
* Konfigurieren [einer IoT-Hub-Quelle](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-iothub).

Standardmäßig unterstützen [Vorschau-Umgebungen](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-update-storage-ingress#ingress-scale-and-preview-limitations) Eingangsraten von bis zu **1 Megabyte pro Sekunde (MB/s) pro Umgebung**. Kunden können Ihre Vorschau-Umgebungen bei Bedarf auf bis zu **16 MB/s** Durchsatz skalieren. Darüber hinaus besteht pro Partition ein Grenzwert von **0,5 MB/s**. 

### <a name="api-limits"></a>API-Grenzwerte

REST-API-Grenzwerte für Time Series Insights Preview sind in der [Referenzdokumentation zur REST-API](https://docs.microsoft.com/rest/api/time-series-insights/preview-query#limits) angegeben.
