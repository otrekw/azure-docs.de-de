---
title: Analysieren der Leistung
titleSuffix: Azure Cognitive Search
description: TBD
author: LiamCavanagh
ms.author: liamca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 04/06/2021
ms.openlocfilehash: 298508f8068b47cbfc720f1d1e8ddf370323ed28
ms.sourcegitcommit: d63f15674f74d908f4017176f8eddf0283f3fac8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/07/2021
ms.locfileid: "106582071"
---
# <a name="analyze-performance-in-azure-cognitive-search"></a>Analysieren der Leistung in Azure Cognitive Search

In diesem Artikel werden die Tools, Verhaltensweisen und Ansätze zum Analysieren der Abfrage- und Indizierungsleistung in Cognitive Search beschrieben.

## <a name="develop-baseline-numbers"></a>Entwickeln von Baselinewerten

Bei jeder großen Implementierung ist es wichtig, einen Benchmarkleistungstest für Ihren Cognitive Search-Dienst zu durchführen, bevor Sie ihn in der Produktionsumgebung einsetzen. Sie sollten sowohl die erwartete Auslastung der Suchabfrage als auch die erwarteten Datenerfassungsworkloads testen (führen Sie diese nach Möglichkeit gleichzeitig aus). Benchmarkzahlen helfen dabei, die richtige [Suchebene](search-sku-tier.md), die [Dienstkonfiguration](search-capacity-planning.md) und die erwartete [Abfragelatenz](search-performance-analysis.md#average-query-latency) zu ermitteln.

Zur Erarbeitung von Benchmarks empfehlen wir das Tool [azure-search-performance-testing (GitHub)](https://github.com/Azure-Samples/azure-search-performance-testing).

Testen Sie die Dienstkonfigurationen eines Replikats und einer Partition, um die Auswirkungen einer verteilten Dienstarchitektur zu isolieren.

> [!NOTE]
> Für die Tarife vom Typ „Storage Optimized“ (L1 und L2) sollten Sie einen geringeren Abfragedurchsatz und eine höhere Latenz als für die Tarife vom Typ „Standard“ erwarten.
>

## <a name="use-diagnostic-logging"></a>Verwenden der Diagnoseprotokollierung

Das wichtigste Tool, das ein Administrator bei der Diagnose potenzieller Leistungsprobleme hat, ist die [Diagnoseprotokollierung](search-monitor-logs.md), die operative Daten und Metriken zu Ihrem Suchdienst sammelt. Die Diagnoseprotokollierung wird über [Azure Monitor](../azure-monitor/overview.md) aktiviert. Mit der Verwendung von Azure Monitor und dem Speichern von Daten sind Kosten verbunden. Wenn Sie sie aber für Ihren Dienst aktivieren, kann dies bei der Untersuchung von Leistungsproblemen äußerst nützlich sein.

Latenzen können in verschiedenen Situationen auftreten, sei es bei einer Datenübertragung im Netzwerk, bei der Verarbeitung von Inhalten in der App-Dienstebene oder bei einem Suchdienst. Ein wichtiger Vorteil der Diagnoseprotokollierung ist, dass Aktivitäten aus Sicht des Suchdiensts protokolliert werden. Das bedeutet, dass Sie mithilfe des Protokolls bestimmen können, ob Leistungseinbußen auf Probleme mit der Abfrage oder Indizierung oder auf eine andere Fehlerquelle zurückgehen.

:::image type="content" source="media/search-performance/perf-log-event-chain.png" alt-text="Kette protokollierter Ereignisse" border="true":::

Die Diagnoseprotokollierung bietet Ihnen Optionen zum Speichern protokollierter Informationen. Es wird empfohlen, [Log Analytics](../azure-monitor/logs/log-analytics-overview.md) zu verwenden, damit Sie erweiterte Kusto-Abfragen für die Daten ausführen können, um viele Fragen zur Nutzung und Leistung zu beantworten. 

Auf den Portalseiten ihres Suchdiensts können Sie die Protokollierung über **Diagnoseeinstellungen** aktivieren und dann Kusto-Abfragen für Log Analytics ausstellen, indem Sie **Protokolle** auswählen. Weitere Informationen finden Sie unter [Sammeln und Analysieren von Protokolldaten](search-monitor-logs.md).

:::image type="content" source="media/search-performance/perf-log-menu-options.png" alt-text="Menüoptionen für die Protokollierung" border="true":::

## <a name="throttling-behaviors"></a>Drosselungsverhalten

Eine Drosselung tritt auf, wenn der Suchdienst den Grenzwert erreicht hat, den er aus Ressourcensicht verarbeiten kann. Zu einer Drosselung kann es während einer Abfrage oder einer Indizierung kommen. Auf Clientseite führt ein API-Aufruf zu einer 503-HTTP-Antwort, wenn er gedrosselt wurde. Während der Indizierung besteht auch die Möglichkeit, eine 207-HTTP-Antwort zu erhalten, die angibt, dass mindestens ein Element nicht indiziert werden konnte. Dieser Fehler zeigt an, dass sich der Suchdienst der Kapazität nähert. 

Als Faustregel empfiehlt es sich, die angezeigte Drosselung zu quantifizieren. Wenn beispielsweise eine von 500.000 Suchabfragen gedrosselt wird, ist dies möglicherweise kein so großes Problem. Wenn jedoch ein großer Prozentsatz der Abfragen über einen bestimmten Zeitraum gedrosselt wird, ist dies ein ernstes Problem. Die Betrachtung der Drosselung über einen bestimmten Zeitraum hilft auch dabei, Zeiträume zu identifizieren, in denen eine Drosselung wahrscheinlicher ist. Zudem können Sie so entscheiden, wie Sie diese am besten berücksichtigen.

Eine einfache Lösung für die meisten Drosselungsprobleme besteht darin, dem Suchdienst mehr Ressourcen zur Verfügung zu stellen (typischerweise Replikate für abfragebasierte Drosselung oder Partitionen für indizierungsbasierte Drosselung). Das Erhöhen von Replikaten oder Partitionen verursacht jedoch zusätzliche Kosten, weshalb es wichtig ist, den Grund zu kennen, warum die Drosselung überhaupt auftritt. Die Untersuchung der Ursachen für eine Drosselung wird in den nächsten Abschnitten erläutert. 

Im Folgenden finden Sie ein Beispiel für eine Kusto-Abfrage, die die Aufschlüsselung der HTTP-Antworten des ausgelasteten Suchdiensts identifizieren kann. Bei der Abfrage über einen Zeitraum von 7 Tagen zeigt das gerenderte Balkendiagramm, dass ein relativ großer Prozentsatz der Suchabfragen gedrosselt wurde, im Vergleich zur Anzahl der erfolgreichen (200) Antworten.

```kusto
AzureDiagnostics
| where TimeGenerated > ago(7d)
| summarize count() by resultSignature_d 
| render barchart 
```

:::image type="content" source="media/search-performance/perf-bar-chart-http-errors.png" alt-text="Balkendiagramm der HTTP-Fehleranzahl" border="true":::

Durch die Untersuchung der Drosselung über einen bestimmten Zeitraum lassen sich die Zeiten ermitteln, in denen die Drosselung möglicherweise häufiger auftritt. Im folgenden Beispiel wird ein Zeitreihendiagramm verwendet, um die Anzahl der gedrosselten Abfragen darzustellen, die innerhalb eines festgelegten Zeitraums aufgetreten sind. In diesem Fall entsprachen die gedrosselten Abfragen den Zeiten, in denen das Leistungsbenchmarking durchgeführt wurde.

```kusto
let ['_startTime']=datetime('2021-02-25T20:45:07Z');
let ['_endTime']=datetime('2021-03-03T20:45:07Z');
let intervalsize = 1m; 
AzureDiagnostics 
| where TimeGenerated > ago(7d)
| where resultSignature_d != 403 and resultSignature_d != 404 and OperationName in ("Query.Search", "Query.Suggest", "Query.Lookup", "Query.Autocomplete")
| summarize 
  ThrottledQueriesPerMinute=bin(countif(OperationName in ("Query.Search", "Query.Suggest", "Query.Lookup", "Query.Autocomplete") and resultSignature_d == 503)/(intervalsize/1m), 0.01)
  by bin(TimeGenerated, intervalsize)
| render timechart   
```

:::image type="content" source="media/search-performance/perf-line-chart-throttled-queries.png" alt-text="Liniendiagramm der gedrosselten Abfragen" border="true":::

## <a name="measure-individual-queries"></a>Messen einzelner Abfragen

In einigen Fällen kann es hilfreich sein, die Leistung einzelner Abfragen testen. Dazu muss ersichtlich sein, wie lange der Suchdienst für die Bearbeitung benötigt und wie lange die Roundtripanforderung vom Client und zurück zum Client dauert. Die Diagnoseprotokolle können verwendet werden, um einzelne Vorgänge zu suchen, aber es ist möglicherweise einfacher, all dies über ein Clienttool wie Postman durchzuführen.

Im folgenden Beispiel wurde eine REST-basierte Suchabfrage ausgeführt. Cognitive Search schließt in jeder Antwort die Anzahl von Millisekunden ein, die zum Abschließen der Abfrage benötigt wird (sichtbar auf der Registerkarte „Header“ in „verstrichene Zeit“). Oben in der Antwort finden Sie neben „Status“ die Roundtripdauer. In diesem Fall 418 Millisekunden. Im Ergebnisabschnitt wurde die Registerkarte „Header“ ausgewählt. Anhand dieser beiden Werte, die in der Abbildung unten mit einem roten Kästchen hervorgehoben sind, lässt sich erkennen, dass der Suchdienst 21 Millisekunden für die Suchabfrage benötigte und die gesamte Roundtripanforderung des Clients 125 Millisekunden in Anspruch nahm. Durch Subtraktion dieser beiden Zahlen können wir feststellen, dass die Übertragung der Suchabfrage an den Suchdienst und die Rückübertragung der Suchergebnisse an den Client 104 ms länger dauerte.

Dies kann sehr hilfreich sein, um festzustellen, ob es möglicherweise Netzwerklatenzen oder andere Faktoren gibt, die die Abfrageleistung beeinflussen.

:::image type="content" source="media/search-performance/perf-elapsed-time.png" alt-text="Metriken zur Abfragedauer" border="true":::

## <a name="query-rates"></a>Abfrageraten

Ein möglicher Grund für die Drosselung von Anforderungen durch Ihren Suchdienst ist die schieren Anzahl von Abfragen, bei denen das Volume als Abfragen pro Sekunde (QPS) oder Abfragen pro Minute (QPM) erfasst wird. Wenn Ihr Suchdienst mehr QPS empfängt, dauert es in der Regel immer länger, um auf diese Abfragen zu antworten, bis er nicht mehr mithalten kann und eine drosselnde 503-HTTP-Antwort zurücksendet. 

Die folgende Kusto-Abfrage zeigt die in QPM gemessene Abfragemenge zusammen mit der durchschnittlichen Dauer einer Abfrage in Millisekunden (AvgDurationMS) und der durchschnittlichen Anzahl von Dokumenten (AvgDocCountReturned), die jeweils zurückgegeben werden.

```kusto
AzureDiagnostics
| where OperationName == "Query.Search" and TimeGenerated > ago(1d)
| extend MinuteOfDay = substring(TimeGenerated, 0, 16) 
| project MinuteOfDay, DurationMs, Documents_d, IndexName_s
| summarize QPM=count(), AvgDuractionMs=avg(DurationMs), AvgDocCountReturned=avg(Documents_d)  by MinuteOfDay
| order by MinuteOfDay desc 
| render timechart
```

:::image type="content" source="media/search-performance/perf-line-chart-queries-per-minute.png" alt-text="Diagramm mit Abfragen pro Minute" border="true":::

> [!TIP]
> Um die Daten hinter diesem Diagramm anzuzeigen, entfernen Sie die Zeile `| render timechart`, und starten Sie dann die Abfrage erneut.

## <a name="impact-of-indexing-on-queries"></a>Auswirkungen der Indizierung auf Abfragen

Ein wichtiger Faktor, der bei der Betrachtung der Leistung zu berücksichtigen ist, besteht darin, dass die Indizierung dieselben Ressourcen verwendet wie die Suchabfragen. Wenn Sie eine große Menge an Inhalten indizieren, können Sie davon ausgehen, dass die Latenz steigt, wenn der Dienst versucht, beide Workloads zu bewältigen.

Wenn Abfragen langsamer ausgeführt werden, sehen Sie sich den Zeitpunkt der Indizierungsaktivität an, um zu sehen, ob er mit der Verschlechterung der Abfrageleistung zusammenfällt. Zum Beispiel könnte ein Indexer einen täglichen oder stündlichen Auftrag ausführen, der mit der verminderten Leistung der Suchabfragen korreliert. 

Dieser Abschnitt enthält eine Reihe von Abfragen, mit denen Sie die Such- und Indizierungsraten visualisieren können. In diesen Beispielen wird der Zeitbereich in der Abfrage festgelegt. Achten Sie beim Ausführen der Abfragen in Azure-Portal auf **In Abfrage festlegen**.

:::image type="content" source="media/search-performance/perf-set-query-time-range.png" alt-text="Festlegen von Zeitbereichen im Abfragetool" border="true":::

<a name="average-query-latency"></a>

### <a name="average-query-latency"></a>Durchschnittliche Abfragelatenz

In der folgenden Abfrage wird eine Intervallgröße von 1 Minute verwendet, um die durchschnittliche Latenz bei Suchabfragen anzuzeigen. Im Diagramm sehen Sie, dass die durchschnittliche Latenz 17:45 Uhr niedrig war und bis 17:53 Uhr gedauert hat.

```kusto
let intervalsize = 1m; 
let _startTime = datetime('2021-02-23 17:40');
let _endTime = datetime('2021-02-23 18:00');
AzureDiagnostics
| where TimeGenerated between(['_startTime']..['_endTime']) // Time range filtering
| summarize AverageQueryLatency = avgif(DurationMs, OperationName in ("Query.Search", "Query.Suggest", "Query.Lookup", "Query.Autocomplete"))
    by bin(TimeGenerated, intervalsize)
| render timechart
```

:::image type="content" source="media/search-performance/perf-line-chart-average-query-latency.png" alt-text="Diagramm mit durchschnittlicher Abfragelatenz" border="true":::

### <a name="average-queries-per-minute-qpm"></a>Durchschnittliche Abfragen pro Minute (QPM)

Die folgende Abfrage ermöglicht es, die durchschnittliche Anzahl von Abfragen pro Minute zu untersuchen. So können Sie sicherstellen, dass es keine Spitzen bei Suchanforderungen gab, die sich möglicherweise auf die Latenz ausgewirkt haben. Aus dem Diagramm ist ersichtlich, dass es eine gewisse Varianz gibt, aber nichts, was auf eine Spitze bei der Anforderungsanzahl hindeutet.

```kusto
let intervalsize = 1m; 
let _startTime = datetime('2021-02-23 17:40');
let _endTime = datetime('2021-02-23 18:00');
AzureDiagnostics
| where TimeGenerated between(['_startTime'] .. ['_endTime']) // Time range filtering
| summarize QueriesPerMinute=bin(countif(OperationName in ("Query.Search", "Query.Suggest", "Query.Lookup", "Query.Autocomplete"))/(intervalsize/1m), 0.01)
  by bin(TimeGenerated, intervalsize)
| render timechart
```

:::image type="content" source="media/search-performance/perf-line-chart-average-queries-per-minute.png" alt-text="Diagramm mit durchschnittlichen Abfragen pro Minute" border="true":::

### <a name="indexing-operations-per-minute-opm"></a>Indizierungsvorgänge pro Minute (OPM)

Hier sehen Sie die Anzahl der Indizierungsvorgänge pro Minute. Das Diagramm zeigt, dass die Indizierung einer großen Datenmenge um 17:42 Uhr begann und um 17:50 Uhr endete. Diese Indizierung begann 3 Minuten, bevor es Latenzen bei den Suchabfragen gab, und endete 3 Minuten, bevor die Suchabfragen keine Latenzen mehr aufwiesen.

Daraus ist ersichtlich, dass es etwa 3 Minuten dauerte, bis der Suchdienst durch die Indizierung so stark ausgelastet war, dass sich dies auf die Latenz der Suchabfrage auswirkte. Wir sehen auch, dass es nach Abschluss der Indizierung weitere 3 Minuten gedauert hat, bis der Suchdienst alle Vorgänge ab dem neu indizierten Inhalt abgeschlossen hat, wonach die Suchabfragen ohne Latenzen ausgeführt wurden.

```kusto
let intervalsize = 1m; 
let _startTime = datetime('2021-02-23 17:40');
let _endTime = datetime('2021-02-23 18:00');
AzureDiagnostics
| where TimeGenerated between(['_startTime'] .. ['_endTime']) // Time range filtering
| summarize IndexingOperationsPerSecond=bin(countif(OperationName == "Indexing.Index")/ (intervalsize/1m), 0.01)
  by bin(TimeGenerated, intervalsize)
| render timechart 
```

:::image type="content" source="media/search-performance/perf-line-chart-indexing-operations.png" alt-text="Diagramm mit Indizierungsvorgängen pro Minute" border="true":::

## <a name="background-service-processing"></a>Hintergrunddienstverarbeitung

Regelmäßige Spitzen bei der Abfrage- oder Indizierungslatenz sind nicht ungewöhnlich. Spitzen können als Reaktion auf die Indizierung oder hohe Abfrageraten auftreten, aber auch während Zusammenführungsvorgängen. Suchindizes werden in Blöcken Shards gespeichert. In regelmäßigen Abständen führt das System kleinere Shards zu großen Shards zusammen, um die Leistung des Diensts zu optimieren. Bei dieser Zusammenführung werden auch Dokumente bereinigt, die zuvor zum Löschen aus dem Index markiert wurden, wodurch wieder mehr Speicherplatz zur Verfügung steht. 

Das Zusammenführen von Shards ist zwar schnell, aber auch ressourcenintensiv. Daher kann dadurch die Leistung des Diensts beeinträchtigt werden. Wenn Sie daher kurze Spitzen von Abfragelatenzen sehen und diese mit aktuellen Änderungen an indizierten Inhalten zusammenfallen, ist es wahrscheinlich, dass Sie diese Latenzen auf Zusammenführungsvorgänge von Shards zurückführen. 

## <a name="next-steps"></a>Nächste Schritte

Die folgenden Artikel enthalten weitere Informationen zur Analyse der Leistung des Diensts.

+ [Leistungstipps](search-performance-tips.md)
+ [Auswählen eines Tarifs für die kognitive Azure-Suche](search-sku-tier.md)
+ [Verwalten der Kapazität](search-capacity-planning.md)
