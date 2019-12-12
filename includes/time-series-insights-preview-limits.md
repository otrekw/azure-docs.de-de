---
title: include file
description: include file
services: digital-twins
ms.service: digital-twins
ms.topic: include
ms.date: 12/06/2019
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.custom: include file
ms.openlocfilehash: be46a0dda24dc990e895a3e983c730337f9cf31d
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2019
ms.locfileid: "74980972"
---
### <a name="general-availability-and-preview-comparison"></a>Vergleich der allgemeinen Verfügbarkeit und der Vorschauversion

In der folgenden Tabelle sind einige wesentliche Unterschiede zwischen Azure Time Series Insights GA (allgemeine Verfügbarkeit) und Instanzen der Vorschauversion zusammengefasst.

| | Allgemein verfügbar | Vorschau |
| --- | --- | ---|
| Bürger erster Klasse | Ereigniszentriert | Zeitreihenzentriert |
| Semantische Argumentation | Low-Level (Verweisdaten) | Allgemein (Modelle) |
| Datenkontextualisierung | Nicht-Geräteebene | Geräte- und Nicht-Geräteebene |
| Computelogikspeicher | Nein | In Typvariablen gespeichert, die Teil des Modells sind |
| Speicher und Zugriffssteuerung | Nein | Über das Modell aktiviert |
| Aggregationen/Sampling | Nein | Ereignisgewichtet und zeitgewichtet |
| Signalwiederherstellung | Nein | Interpolation |
| Produktion abgeleiteter Zeitreihen | Nein | Ja, Zusammenführungen und Joins |
| Sprachflexibilität | Nicht zusammensetzbar | Zusammensetzbar |
| Ausdruckssprache | Prädikatszeichenfolge | Zeitreihenausdrücke (Prädikatszeichenfolgen, Werte, Ausdrücke und Funktionen) |

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

### <a name="api-limits"></a>API-Grenzwerte

REST-API-Grenzwerte für Time Series Insights Preview sind in der [Referenzdokumentation zur REST-API](https://docs.microsoft.com/rest/api/time-series-insights/preview-query#limits) angegeben.