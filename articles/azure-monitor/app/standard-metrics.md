---
title: Azure Application Insights-Standardmetriken | Microsoft-Dokumentation
description: Dieser Artikel listet die Metriken von Azure Application Insights mit unterstützten Aggregationen und Dimensionen auf.
services: azure-monitor
ms.topic: reference
ms.date: 07/03/2019
ms.subservice: application-insights
ms.openlocfilehash: 0a18088fa434efa76007607c067feec107bdae57
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "100572356"
---
# <a name="application-insights-standard-metrics"></a>Application Insights-Standardmetriken

Standardmetriken werden während der Erfassung vorab aggregiert, sodass sie zum Abfragezeitpunkt eine bessere Leistung ermöglichen. Dies macht sie zur besten Option für Dashboards und Echtzeitwarnungen.

## <a name="availability-metrics"></a>Verfügbarkeitsmetriken

Metriken der Kategorie „Verfügbarkeit“ ermöglichen es Ihnen, die Integrität Ihrer Webanwendung so anzuzeigen, wie sie an Punkten auf der ganzen Welt zu beobachten ist. [Konfigurieren Sie die Verfügbarkeitstests](../app/monitor-web-app-availability.md), damit Sie Metriken aus dieser Kategorie verwenden können.

### <a name="availability-availabilityresultsavailabilitypercentage"></a>Verfügbarkeit (availabilityResults/availabilityPercentage)
Die Metrik *Verfügbarkeit* zeigt den Prozentsatz der Webtestläufe, bei denen keine Probleme erkannt wurden. Der kleinstmögliche Wert ist 0 und gibt an, dass bei allen Webtestläufen Fehler aufgetreten sind. Der Wert 100 bedeutet, dass alle Webtestläufe die Überprüfungskriterien erfüllt haben.

|Unit of measure|Unterstützte Aggregationen|Unterstützte Dimensionen|
|---|---|---|---|---|---|
|Prozentwert|Average|`Run location`, `Test name`|


### <a name="availability-test-duration-availabilityresultsduration"></a>Verfügbarkeitstestdauer (availabilityResults/duration)

Die Metrik *Verfügbarkeitstestdauer* gibt an, wie lange die Ausführung des Webtests gedauert hat. Bei den [mehrstufigen Webtests](../app/availability-multistep.md) spiegelt die Metrik die gesamte Ausführungszeit aller Stufen wider.

|Unit of measure|Unterstützte Aggregationen|Unterstützte Dimensionen|
|---|---|---|---|---|---|
|Millisekunden|Durchschnitt, Minimum, Maximum|`Run location`, `Test name`, `Test result`

### <a name="availability-tests-availabilityresultscount"></a>Verfügbarkeitstests (availabilityResults/count)

Die Metrik *Verfügbarkeitstests* spiegelt die Anzahl der von Azure Monitor ausgeführten Webtests wider.

|Unit of measure|Unterstützte Aggregationen|Unterstützte Dimensionen|
|---|---|---|---|---|---|
|Anzahl|Anzahl|`Run location`, `Test name`, `Test result`|


## <a name="browser-metrics"></a>Browsermetriken

Browsermetriken werden vom Application Insights JavaScript SDK aus echten Endbenutzerbrowsern gesammelt. Sie bieten nützliche Einblicke in die Erfahrungen der Benutzer mit Ihrer Webanwendung. Für Browsermetriken werden in der Regel keine Stichproben verwendet, was bedeutet, dass sie eine höhere Genauigkeit der Nutzungszahlen bieten als serverseitige Metriken, die durch die Verwendung von Stichproben verzerrt sein können.

> [!NOTE]
> Um Browsermetriken zu erfassen, muss Ihre Anwendung mit dem [Application Insights JavaScript SDK](../app/javascript.md) ausgestattet sein.

### <a name="browser-page-load-time-browsertimingstotalduration"></a>Browser-Seitenladezeit (browserTimings/totalDuration)

|Unit of measure|Unterstützte Aggregationen| Unterstützte Dimensionen|
|---|---|---|
|Millisekunden|Durchschnitt, Minimum, Maximum|Keine|

### <a name="client-processing-time-browsertimingprocessingduration"></a>Clientverarbeitungszeit (browserTiming/processingDuration)

|Unit of measure|Unterstützte Aggregationen|Unterstützte Dimensionen|
|---|---|---|
|Millisekunden|Durchschnitt, Minimum, Maximum|Keine|

### <a name="page-load-network-connect-time-browsertimingsnetworkduration"></a>Netzwerkverbindungszeit zum Laden der Seite (browserTimings/networkDuration)

|Unit of measure|Unterstützte Aggregationen| Unterstützte Dimensionen|
|---|---|---|
|Millisekunden|Durchschnitt, Minimum, Maximum|Keine|

### <a name="receiving-response-time-browsertimingsreceiveduration"></a>Empfängt Antwortzeit (browserTimings/receiveDuration)

|Unit of measure|Unterstützte Aggregationen| Unterstützte Dimensionen|
|---|---|---|
|Millisekunden|Durchschnitt, Minimum, Maximum|Keine|

### <a name="send-request-time-browsertimingssendduration"></a>Anforderungszeit senden (browserTimings/sendDuration)

|Unit of measure|Unterstützte Aggregationen| Unterstützte Dimensionen|
|---|---|---|
|Millisekunden|Durchschnitt, Minimum, Maximum|Keine|

## <a name="failure-metrics"></a>Fehlermetriken

Die Metriken der Kategorie **Fehler** zeigen Probleme bei der Verarbeitung von Anforderungen, Abhängigkeitsaufrufen und ausgelösten Ausnahmen.

### <a name="browser-exceptions-exceptionsbrowser"></a>Browserausnahmen (exceptions/browser)

Diese Metrik spiegelt die Anzahl der ausgelösten Ausnahmen von Ihrem Anwendungscode im Browser wider. Nur Ausnahmen, die mit einem ```trackException()``` Application Insights API-Aufruf verfolgt werden, sind in der Metrik enthalten.

|Unit of measure|Unterstützte Aggregationen | Unterstützte Dimensionen|
|---|---|---|---|
| Anzahl | Anzahl | `Cloud role name` |

### <a name="dependency-call-failures-dependenciesfailed"></a>Fehler bei Abhängigkeitsaufrufen (dependencies/failed)

Die Anzahl fehlerhafter Abhängigkeitsaufrufe.

|Unit of measure|Unterstützte Aggregationen | Unterstützte Dimensionen |
|---|---|---|---|
|Anzahl|Anzahl| `Cloud role instance`, `Cloud role name`, `Dependency performance`, `Dependency type`, `Is traffic synthetic`, `Result code`, `Rarget of dependency call`.


### <a name="exceptions-exceptionscount"></a>Ausnahmen (exceptions/count)

Jedes Mal, wenn Sie eine Ausnahme bei Application Insights protokollieren, erfolgt ein Aufruf der [trackException()-Methode](../app/api-custom-events-metrics.md#trackexception) des SDK. Die Metrik „Ausnahmen“ zeigt die Anzahl der protokollierten Ausnahmen an.

|Unit of measure|Unterstützte Aggregationen | Unterstützte Dimensionen |
|---|---|---|---|
|Anzahl|Anzahl|`Cloud role instance`, `Cloud role name`, `Device type`|

### <a name="failed-requests-requestsfailed"></a>Fehlerhafte Anforderungen (requests/failed)

Die Anzahl der verfolgten Serveranforderungen, die als *fehlgeschlagen* markiert wurden. Standardmäßig markiert das Application Insights SDK automatisch jede Serveranforderung, die den HTTP-Antwortcode 5xx oder 4xx zurückgegeben hat, als fehlerhafte Anforderung. Sie können diese Logik anpassen, indem Sie die Eigenschaft *Erfolg* des Anforderungstelemetrieelements in einem [benutzerdefinierten Telemetrieinitialisierer](../app/api-filtering-sampling.md#addmodify-properties-itelemetryinitializer) ändern.


|Unit of measure|Unterstützte Aggregationen | Unterstützte Dimensionen |
|---|---|---|---|
|Anzahl|Anzahl|`Cloud role instance`, `Cloud role name`, `Is synthetic traffic`, `Request performance`, `Result code`|


### <a name="server-exceptions-exceptionsserver"></a>Serverausnahmen (exceptions/server)

Diese Metrik zeigt die Anzahl der Serverausnahmen.

|Unit of measure|Unterstützte Aggregationen | Unterstützte Dimensionen |
|---|---|---|---|
|Anzahl|Anzahl|`Cloud role instance`, `Cloud role name`|

## <a name="performance-counters"></a>Leistungsindikatoren

Verwenden Sie Metriken in der Kategorie **Leistungsindikatoren**, um auf [die von Application Insights erfassten Systemleistungsindikatoren](../app/performance-counters.md) zuzugreifen.

### <a name="available-memory-performancecountersavailablememory"></a>Verfügbarer Speicher (performanceCounters/availableMemory)

|Unit of measure|Unterstützte Aggregationen | Unterstützte Dimensionen |
|---|---|---|---|
|Datenabhängig: MB, GB|Durchschnitt, Maximum, Minimum|`Cloud role instance`|


### <a name="exception-rate-performancecountersexceptionrate"></a>Ausnahmerate (performanceCounters/exceptionRate)

|Unit of measure|Unterstützte Aggregationen | Unterstützte Dimensionen |
|---|---|---|---|
| Anzahl | Durchschnitt, Maximum, Minimum | `Cloud role instance` |


### <a name="http-request-execution-time-performancecountersrequestexecutiontime"></a>Ausführungszeit der HTTP-Anforderung (performanceCounters/requestExecutionTime)

|Unit of measure|Unterstützte Aggregationen | Unterstützte Dimensionen |
|---|---|---|---|
| Millisekunden | Durchschnitt, Maximum, Minimum | `Cloud role instance` |

### <a name="http-request-rate-performancecountersrequestspersecond"></a>HTTP-Anforderungsrate (performanceCounters/requestsPerSecond)

|Unit of measure|Unterstützte Aggregationen | Unterstützte Dimensionen |
|---|---|---|---|
| Anforderungen pro Sekunde | Durchschnitt, Maximum, Minimum | `Cloud role instance` |

### <a name="http-requests-in-application-queue-performancecountersrequestsinqueue"></a>HTTP-Anforderungen in der Anwendungswarteschlange (performanceCounters/requestsInQueue)

|Unit of measure|Unterstützte Aggregationen | Unterstützte Dimensionen |
|---|---|---|---|
| Anzahl | Durchschnitt, Maximum, Minimum | `Cloud role instance` |


### <a name="process-cpu-performancecountersprocesscpupercentage"></a>Prozess-CPU (performanceCounters/processCpuPercentage)

Die Metrik zeigt, wie viel der gesamten Prozessorleistung von dem Prozess genutzt wird, der Ihre überwachte App hostet.

|Unit of measure|Unterstützte Aggregationen|Unterstützte Dimensionen|
|---|---|---|
|Prozentwert|Durchschnitt, Maximum, Minimum| `Cloud role instance` |


### <a name="process-io-rate-performancecountersprocessiobytespersecond"></a>E/A-Rate des Prozesses (performanceCounters/processIOBytesPerSecond)

|Unit of measure|Unterstützte Aggregationen|Unterstützte Dimensionen|
|---|---|---|
|Bytes pro Sekunde|Durchschnitt, Minimum, Maximum|`Cloud role instance` |



### <a name="process-private-bytes-performancecountersprocessprivatebytes"></a>Private Bytes des Prozesses (performanceCounters/processPrivateBytes)

Menge des nicht gemeinsam genutzten Arbeitsspeichers, die der überwachte Prozess für seine Daten reserviert hat.

|Unit of measure|Unterstützte Aggregationen|Unterstützte Dimensionen|
|---|---|---|
|Byte|Durchschnitt, Minimum, Maximum|`Cloud role instance` |

### <a name="processor-time-performancecountersprocessorcpupercentage"></a>Prozessorzeit (performanceCounters/processorCpuPercentage)

CPU-Auslastung durch *alle* Prozesse, die auf der überwachten Serverinstanz ausgeführt werden.

|Unit of measure|Unterstützte Aggregationen|Unterstützte Dimensionen|
|---|---|---|
|Prozentwert|Durchschnitt, Minimum, Maximum|`Cloud role instance` |

>[!NOTE]
> Die Prozessorzeitmetrik ist für die in Azure App Services gehosteten Anwendungen nicht verfügbar. Verwenden Sie die [Prozess-CPU](#process-cpu-performancecountersprocesscpupercentage)-Metrik, um die CPU-Auslastung der in App Services gehosteten Webanwendungen zu verfolgen.

## <a name="server-metrics"></a>Servermetriken

### <a name="dependency-calls-dependenciescount"></a>Abhängigkeitsaufrufe (dependencies/count)

Diese Metrik bezieht sich auf die Anzahl der Abhängigkeitsaufrufe.

|Unit of measure|Unterstützte Aggregationen|Unterstützte Dimensionen|
|---|---|---|
| Anzahl | Anzahl | `Cloud role instance`, `Cloud role name`, `Dependency performance`, `Dependency type`, `Is traffic synthetic`, `Result code`, `Successful call`, `Target of a dependency call` |

### <a name="dependency-duration-dependenciesduration"></a>Dauer der Abhängigkeit (dependencies/duration)

Diese Metrik bezieht sich auf die Dauer von Abhängigkeitsaufrufen.

|Unit of measure|Unterstützte Aggregationen|Unterstützte Dimensionen|
|---|---|---|
| Zeit | Durchschnitt, Minimum, Maximum | `Cloud role instance`, `Cloud role name`, `Dependency performance`, `Dependency type`, `Is traffic synthetic`, `Result code`, `Successful call`, `Target of a dependency call` |


### <a name="server-request-rate-requestscount"></a>Serveranforderungsrate (Anforderungen/Anzahl)

Diese Metrik spiegelt die Anzahl der eingehenden Serveranforderungen wider, die von Ihrer Webanwendung empfangen wurden.

|Unit of measure|Unterstützte Aggregationen|Unterstützte Dimensionen|
|---|---|---|
| Anzahl | Average | `Cloud role instance`, `Cloud role name`, `Is traffic synthetic`, `Result performance` `Result code`, `Successful request` |

### <a name="server-requests-requestscount"></a>Serveranforderungen (requests/count)

|Unit of measure|Unterstützte Aggregationen|Unterstützte Dimensionen|
|---|---|---|
| Anzahl | Anzahl | `Cloud role instance`, `Cloud role name`, `Is traffic synthetic`, `Result performance` `Result code`, `Successful request` |

### <a name="server-response-time-requestsduration"></a>Serverantwortzeit (requests/duration)

Diese Metrik spiegelt die Zeit wider, die die Server für die Verarbeitung eingehender Anforderungen benötigt haben.

|Unit of measure|Unterstützte Aggregationen|Unterstützte Dimensionen|
|---|---|---|
| Zeit | Durchschnitt, Minimum, Maximum | `Cloud role instance`, `Cloud role name`, `Is traffic synthetic`, `Result performance` `Result code`, `Successful request` |

## <a name="usage-metrics"></a>Nutzungsmetriken

### <a name="page-view-load-time-pageviewsduration"></a>Ladezeit der Seitenansicht (pageViews/duration)

Diese Metrik bezieht sich auf die Zeit, die für das Laden von PageView-Ereignissen benötigt wurde.

|Unit of measure|Unterstützte Aggregationen|Unterstützte Dimensionen|
|---|---|---|
| Zeit | Durchschnitt, Minimum, Maximum | `Cloud role name`, `Is traffic synthetic` |

### <a name="page-views-pageviewscount"></a>Seitenaufrufe (pageViews/count)

Die Anzahl der PageView-Ereignisse, die mit der TrackPageView() Application Insights API protokolliert wurden.

|Unit of measure|Unterstützte Aggregationen|Unterstützte Dimensionen|
|---|---|---|
| Anzahl | Anzahl | `Cloud role name`, `Is traffic synthetic` |

### <a name="traces-tracescount"></a>Überwachungen (traces/count)

Die Anzahl der mit dem TrackTrace() Application Insights API-Aufruf protokollierten Überwachungsanweisungen.

|Unit of measure|Unterstützte Aggregationen|Unterstützte Dimensionen|
|---|---|---|
| Anzahl | Anzahl | `Cloud role instance`, `Cloud role name`,  `Is traffic synthetic`, `Severity level` |


## <a name="next-steps"></a>Nächste Schritte

* Informieren Sie sich über [Protokollbasierte und vorab aggregierte Metriken](./pre-aggregated-metrics-log-metrics.md).
* [Protokollbasierte Metriken in Abfragen und Metrikdefinitionen](../essentials/app-insights-metrics.md).