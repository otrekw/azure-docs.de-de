---
ms.openlocfilehash: 0bae8946b7d81fbf45698cefe7c8b00bbc94d00c
ms.sourcegitcommit: 419c8c8061c0ff6dc12c66ad6eda1b266d2f40bd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2020
ms.locfileid: "92168108"
---
#### <a name="determine-memory-usage"></a>Arbeitsspeichernutzung ermitteln 

Wählen Sie unter **Überwachung** den Eintrag **Protokolle (Analytics)** aus, kopieren Sie dann die folgende Telemetrieabfrage, fügen Sie sie in das Abfragefenster ein, und wählen Sie **Ausführen** aus. Diese Abfrage gibt die Gesamtarbeitsspeichernutzung zu jedem Stichprobenzeitpunkt zurück.

```
performanceCounters
| where name == "Private Bytes"
| project timestamp, name, value
```

Die Ergebnisse sehen ungefähr wie folgt aus:

| Zeitstempel \[UTC\]          | name          | value       |
|----------------------------|---------------|-------------|
| 12.9.2019, 1:05:14\.947 AM | Private Bytes | 209.932.288 |
| 12.9.2019, 1:06:14\.994 AM | Private Bytes | 212.189.184 |
| 12.9.2019, 1:06:30\.010 AM | Private Bytes | 231.714.816 |
| 12.9.2019, 1:07:15\.040 AM | Private Bytes | 210.591.744 |
| 12.9.2019, 1:12:16\.285 AM | Private Bytes | 216.285.184 |
| 12.9.2019, 1:12:31\.376 AM | Private Bytes | 235.806.720 |

#### <a name="determine-duration"></a>Bestimmen der Dauer 

Azure Monitor erfasst Metriken auf Ressourcenebene, wobei es sich für Functions um die Funktions-App handelt. Application Insights-Integration gibt Metriken pro Funktion aus. Im Folgenden finden Sie ein Beispiel für eine Analytics-Abfrage, mit der die durchschnittliche Dauer einer Funktion abgefragt wird:

```
customMetrics
| where name contains "Duration"
| extend averageDuration = valueSum / valueCount
| summarize averageDurationMilliseconds=avg(averageDuration) by name
```

| name                       | averageDurationMilliseconds |
|----------------------------|-----------------------------|
| QueueTrigger AvgDurationMs | 16\.087                     |
| QueueTrigger MaxDurationMs | 90\.249                     |
| QueueTrigger MinDurationMs | 8\.522                      |
