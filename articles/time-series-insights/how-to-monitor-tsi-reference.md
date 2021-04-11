---
title: Referenz zur Überwachung von Azure Time Series Insights-Daten | Microsoft-Dokumentation
description: Referenzdokumentation zur Überwachung von Azure Time Series Insights.
author: deepakpalled
ms.author: dpalled
manager: diviso
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/10/2020
ms.custom: lyrana
ms.openlocfilehash: 20d376ee501edf16711617847c685dfd531f2c80
ms.sourcegitcommit: c2a41648315a95aa6340e67e600a52801af69ec7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2021
ms.locfileid: "106504582"
---
# <a name="monitoring-azure-time-series-insights-data-reference"></a>Referenz zur Überwachung von Azure Time Series Insights-Daten

Erfahren Sie mehr über die Daten und Ressourcen, die von Azure Monitor aus Ihrer Azure Time Series Insights-Umgebung gesammelt werden. Ausführliche Informationen über das Sammeln und Analysieren von Überwachungsdaten finden Sie unter [Überwachen von Time Series Insights]( ./how-to-monitor-tsi.md).

## <a name="metrics"></a>Metriken

In diesem Abschnitt werden alle automatisch erfassten Plattformmetriken aufgeführt, die für Azure Time Series Insights gesammelt werden. Eine Liste aller von Azure Monitor unterstützten Metriken (einschließlich Azure Time Series Insights) finden Sie unter [Unterstützte Metriken von Azure Monitor](../azure-monitor/essentials/metrics-supported.md).
Die Ressourcenanbieter für diese Metriken sind [Microsoft.TimeSeriesInsights/environments/eventsources](../azure-monitor/essentials/metrics-supported.md#microsofttimeseriesinsightsenvironmentseventsources) und [Microsoft.TimeSeriesInsights/environments](../azure-monitor/essentials/metrics-supported.md#microsofttimeseriesinsightsenvironments).


### <a name="ingress"></a>Eingehende Daten

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|BESCHREIBUNG|
|---|---|---|---|---|
|IngressReceivedBytes|Eingang empfangene Bytes|Byte|Gesamt|Die Anzahl der von der Ereignisquelle gelesenen Bytes|
|IngressReceivedInvalidMessages|Eingang empfangene ungültige Nachrichten|Anzahl|Gesamt|Die Anzahl der von der Ereignisquelle gelesenen ungültigen Nachrichten|
|IngressReceivedMessages|Eingang empfangene Nachrichten|Anzahl|Gesamt|Die Anzahl der von der Ereignisquelle gelesenen Nachrichten|
|IngressReceivedMessagesCountLag|Ingress-Anzahlrückstand empfangener Nachrichten|Anzahl|Average|Unterschied zwischen der Sequenznummer der Nachricht, die in der Ereignisquelle zuletzt in die Warteschlange eingereiht wurde, und der Sequenznummer der am Eingang verarbeiteten Nachricht|
|IngressReceivedMessagesTimeLag|Ingress-Zeitabstand empfangener Nachrichten|Sekunden|Maximum|Der Unterschied zwischen dem Zeitpunkt, zu dem die Nachricht in der Ereignisquelle in die Warteschlange eingereiht wird, und dem Zeitpunkt der Verarbeitung in Ingress|
|IngressStoredBytes|Eingang gespeicherte Bytes|Byte|Gesamt|Die Gesamtgröße der erfolgreich verarbeiteten und für Abfragen verfügbaren Ereignisse|
|IngressStoredEvents|Gespeicherte Ingress-Ereignisse|Anzahl|Gesamt|Die Gesamtgröße der vereinfachten und für Abfragen verfügbaren Ereignisse|

### <a name="storage"></a>Storage

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|BESCHREIBUNG|
|---|---|---|---|---|
|WarmStorageMaxProperties|Maximale Eigenschaften von Warm Storage|Anzahl|Maximum|Maximale Anzahl von Eigenschaften, die von der Umgebung für S1/S2-SKU zugelassen werden, und maximale Anzahl von Eigenschaften, die von Warm Store für PAYG-SKU zugelassen werden|
|WarmStorageUsedProperties|Verwendete Eigenschaften von Warm Storage |Anzahl|Maximum|Anzahl von Eigenschaften, die von der Umgebung für S1/S2-SKU verwendet werden, und Anzahl von Eigenschaften, die von Warm Store für PAYG-SKU verwendet werden|

## <a name="resource-logs"></a>Ressourcenprotokolle

In diesem Abschnitt werden die Arten von Ressourcenprotokollen aufgeführt, die Sie für Ihre Azure Time Series Insights-Umgebung sammeln können.

| Kategorie | Anzeigename | BESCHREIBUNG |
|----- |----- |----- |
| Eingehende Daten | TSIIngress | In der Kategorie „Eingehende Daten“ werden Fehler nachverfolgt, die in der Eingangspipeline auftreten. Diese Kategorie umfasst Fehler beim Empfangen von Ereignissen (z. B. Fehler beim Herstellen einer Verbindung mit einer Ereignisquelle) sowie beim Verarbeiten von Ereignissen (z. B. Fehler beim Analysieren von Ereignisnutzdaten). |

## <a name="schemas"></a>Schemas
Die folgenden Schemas werden von Azure Time Series Insights verwendet.

### <a name="tsiingress-table"></a>TSIIngress-Tabelle

| Eigenschaft | BESCHREIBUNG |
|----- |----- |
| TimeGenerated | Uhrzeit (UTC), zu der dieses Ereignis generiert wurde. |
| Speicherort | Der Speicherort der Ressource. |
| Category | Kategorie des Protokollereignisses. |
| Vorgangsname | Vorgangsname des Ereignisses. |
| CorrelationId | Korrelations-ID der Anforderung. |
| Ebene | Der Schweregrad des Ereignisses. |
| ResultDescription | Beschreibung des Ergebnisses des Vorgangs, z. B. „Fehler empfangen“. |
| `Message` | Die mit dem Fehler verknüpfte Meldung. Enthält Details zum Fehler und dessen Behebung. |
| ErrorCode | Der dem Fehler zugeordnete Code. |
| EventSourceType | Der Typ der Ereignisquelle. Dies kann ein Event Hub oder ein IoT-Hub sein. |
| EventSourceProperties | Eine Sammlung von für die Ereignisquelle spezifischen Eigenschaften. Enthält Details wie z. B. Consumergruppe und Zugriffsschlüsselname. |
