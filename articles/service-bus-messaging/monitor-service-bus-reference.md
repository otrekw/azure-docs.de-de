---
title: Referenz zum Überwachen von Azure Service Bus-Daten
description: Wichtiges Referenzmaterial für die Überwachung von Azure Service Bus.
ms.topic: reference
ms.custom: subject-monitoring
ms.date: 05/18/2021
ms.openlocfilehash: a32972b6400831a4045082fa5d0255b06ed66210
ms.sourcegitcommit: 23040f695dd0785409ab964613fabca1645cef90
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/14/2021
ms.locfileid: "112061288"
---
# <a name="monitoring-azure-service-bus-data-reference"></a>Referenz zum Überwachen von Azure Service Bus-Daten
Ausführliche Informationen zum Erfassen und Analysieren von Überwachungsdaten für Azure Service Bus finden Sie unter [Überwachen von Azure Service Bus](monitor-service-bus.md).

## <a name="metrics"></a>Metriken
In diesem Abschnitt werden alle automatisch erfassten Plattformmetriken aufgeführt, die für Azure Service Bus erfasst werden. Der Ressourcenanbieter für diese Metriken ist **Microsoft.ServiceBus/namespaces**.

### <a name="request-metrics"></a>Anforderungsmetriken
Zählt die Anzahl der Anforderungen von Daten und Verwaltungsvorgängen

| Metrikname |  Über Diagnoseeinstellungen exportierbar | Einheit | Aggregationstyp |  BESCHREIBUNG | Dimensionen | 
| ---------- | ---------- | ----- | --- | --- | --- | 
| Eingehende Anforderungen| Ja | Anzahl | Gesamt | Die Anzahl der Anforderungen an den Service Bus-Dienst in einem bestimmten Zeitraum | EntityName | 
|Erfolgreiche Anforderungen| Nein | Anzahl | Gesamt | Die Anzahl der erfolgreichen Anforderungen an den Service Bus-Dienst in einem bestimmten Zeitraum | Name der Entität<br/>OperationResult|
|Serverfehler| Nein | Anzahl | Gesamt | Die Anzahl der aufgrund eines Fehlers nicht verarbeiteten Anforderungen an den Service Bus-Dienst in einem bestimmten Zeitraum | Name der Entität<br/>OperationResult|
|Benutzerfehler | Nein | Anzahl | Gesamt | Die Anzahl der aufgrund von Benutzerfehlern nicht verarbeiteten Anforderungen in einem bestimmten Zeitraum | Name der Entität|
|Gedrosselte Anforderungen| Nein | Anzahl | Gesamt | Die Anzahl der Anforderungen, die aufgrund eines überschrittenen Verbrauchs gedrosselt wurden |  Name der Entität|

Die beiden folgenden Arten von Fehlern werden als **Benutzerfehler** klassifiziert:

1. Clientseitige Fehler (in HTTP: Fehler vom Typ 400)
2. Fehler bei der Nachrichtenverarbeitung (beispielsweise [MessageLockLostException](/dotnet/api/microsoft.azure.servicebus.messagelocklostexception))


### <a name="message-metrics"></a>Nachrichtenmetriken

| Metrikname |  Über Diagnoseeinstellungen exportierbar | Einheit | Aggregationstyp |  BESCHREIBUNG | Dimensionen | 
| ---------- | ---------- | ----- | --- | --- | --- | 
|Eingehende Nachrichten| Ja | Anzahl | Gesamt | Die Anzahl von Ereignissen oder Nachrichten, die in einem bestimmten Zeitraum an Service Bus gesendet wurden Diese Metrik enthält keine Nachrichten, die automatisch weitergeleitet werden. | Name der Entität|
|Ausgehende Nachrichten| Ja | Anzahl | Gesamt | Die Anzahl von Ereignissen oder Nachrichten, die in einem bestimmten Zeitraum von Service Bus empfangen wurden | Name der Entität|
| Meldungen| Nein | Anzahl | Average | Anzahl von Nachrichten in einer Warteschlange/einem Thema | Name der Entität |
| Aktive Nachrichten| Nein | Anzahl | Average | Anzahl von aktiven Nachrichten in einer Warteschlange/einem Thema | Name der Entität |
| Unzustellbare Nachrichten| Nein | Anzahl | Average | Anzahl von unzustellbaren Nachrichten in einer Warteschlange/einem Thema.  | Name der Entität |
| Geplante Nachrichten| Nein | Anzahl | Average | Anzahl von geplanten Nachrichten in einer Warteschlange/einem Thema. | Name der Entität |
| Size | Nein | Byte | Average | Größe einer Entität (Warteschlange oder Thema) in Byte | Name der Entität | 

> [!NOTE]
> Werte für Nachrichten, aktive Nachrichten, unzustellbare Nachrichten, geplante Nachrichten, abgeschlossene Nachrichten und abgebrochene Nachrichten sind Zeitpunktwerte. Eingehende Nachrichten, die unmittelbar nach diesem Zeitpunkt verarbeitet wurden, werden in diesen Metriken möglicherweise nicht berücksichtigt. 

### <a name="connection-metrics"></a>Verbindungsmetriken

| Metrikname |  Über Diagnoseeinstellungen exportierbar | Einheit | Aggregationstyp |  BESCHREIBUNG | Dimensionen | 
| ---------- | ---------- | ----- | --- | --- | --- | 
|Die aktiven Verbindungen.| Nein | Anzahl | Gesamt | Die Anzahl der aktiven Verbindungen für einen Namespace sowie für eine Entität im Namespace. Der Wert für diese Metrik ist ein Zeitpunktwert. Verbindungen, die unmittelbar nach diesem Zeitpunkt aktiv waren, werden in der Metrik möglicherweise nicht berücksichtigt. | |
|Geöffnete Verbindungen | Nein | Anzahl | Average | Die Anzahl der geöffneten Verbindungen | Name der Entität|
|Geschlossene Verbindungen | Nein | Anzahl | Average | Die Anzahl der geschlossenen Verbindungen | Name der Entität|

### <a name="resource-usage-metrics"></a>Metriken zur Ressourcennutzung

> [!NOTE] 
> Die folgenden Metriken sind nur mit dem **Premium**-Tarif verfügbar. 
> 
> Wichtige Metriken, die bei Ausfällen für einen Namespace im Premium-Tarif überwacht werden müssen: **CPU-Auslastung pro Namespace** und **Arbeitsspeichergröße pro Namespace**. [Richten Sie mit Azure Monitor Warnungen](../azure-monitor/alerts/alerts-metric.md) für diese Metriken ein.
> 
> Die andere Metrik, die Sie überwachen können, ist **Gedrosselt Anforderungen**. Es sollte jedoch kein Problem vorliegen, solange der Namespace innerhalb der Grenzwerte für Arbeitsspeicher, CPU und vermittelte Verbindungen bleibt. Weitere Informationen finden Sie unter [Drosselung im Tarif „Premium“ von Azure Service Bus](service-bus-throttling.md#throttling-in-azure-service-bus-premium-tier)

| Metrikname |  Über Diagnoseeinstellungen exportierbar | Einheit | Aggregationstyp |  BESCHREIBUNG | Dimensionen | 
| ---------- | ---------- | ----- | --- | --- | --- | 
|CPU-Auslastung pro Namespace| Nein | CPU | Percent | Der Prozentsatz der CPU-Auslastung des Namespace | Replikat |
|Auslastung der Arbeitsspeichergröße pro Namespace| Nein | Speicherauslastung | Percent | Der Prozentsatz der Arbeitsspeichernutzung des Namespace | Replikat |

### <a name="error-metrics"></a>Fehlermetriken
| Metrikname |  Über Diagnoseeinstellungen exportierbar | Einheit | Aggregationstyp |  BESCHREIBUNG | Dimensionen |
| ------------------- | ----------------- | --- | --- | --- | --- | 
|Serverfehler| Nein | Anzahl | Gesamt | Die Anzahl der aufgrund eines Fehlers nicht verarbeiteten Anforderungen an den Service Bus-Dienst in einem bestimmten Zeitraum | Name der Entität<br/><br/>Ergebnis des Vorgangs |
|Benutzerfehler | Nein | Anzahl | Gesamt | Die Anzahl der aufgrund von Benutzerfehlern nicht verarbeiteten Anforderungen in einem bestimmten Zeitraum | Name der Entität<br/><br/>Ergebnis des Vorgangs|

## <a name="metric-dimensions"></a>Metrikdimensionen

Azure Service Bus unterstützt folgende Dimensionen für Metriken in Azure Monitor. Das Hinzufügen von Dimensionen zu Ihren Metriken ist optional. Wenn Sie keine Dimensionen hinzufügen, werden Metriken auf Namespaceebene angegeben. 

|Dimensionsname|BESCHREIBUNG|
| ------------------- | ----------------- |
|Entitätsname| Service Bus unterstützt Messagingentitäten unter dem Namespace.|

## <a name="resource-logs"></a>Ressourcenprotokolle
In diesem Abschnitt werden die Typen der Ressourcenprotokolle aufgeführt, die für Azure Service Bus erfasst werden können.

- Betriebsprotokolle
- Protokolle zum Filtern virtueller Netzwerke und IP-Adressen

### <a name="operational-logs"></a>Betriebsprotokolle
Einträge im Betriebsprotokoll enthalten Elemente, die in der folgenden Tabelle aufgeführt werden:

| Name | BESCHREIBUNG |
| ------- | ------- |
| ActivityId | Interne ID, die zum Identifizieren der angegebenen Aktivität verwendet wird |
| EventName | Vorgangsname |
| resourceId | Azure Resource Manager-Ressourcen-ID |
| SubscriptionId | Abonnement-ID |
| EventTimeString | Vorgangsdauer |
| EventProperties | Vorgangseigenschaften |
| Status | Vorgangsstatus |
| Caller | Aufrufer des Vorgangs (Azure-Portal oder Verwaltungsclient) |
| Category | OperationalLogs |

Hier ein Beispiel für eine JSON-Zeichenfolge im Betriebsprotokoll:

```json
{
  "ActivityId": "0000000000-0000-0000-0000-00000000000000",
  "EventName": "Create Queue",
  "resourceId": "/SUBSCRIPTIONS/<AZURE SUBSCRPTION ID>/RESOURCEGROUPS/<RESOURCE GROUP NAME>/PROVIDERS/MICROSOFT.SERVICEBUS/NAMESPACES/<SERVICE BUS NAMESPACE NAME>",
  "SubscriptionId": "0000000000-0000-0000-0000-00000000000000",
  "EventTimeString": "9/28/2016 8:40:06 PM +00:00",
  "EventProperties": "{\"SubscriptionId\":\"0000000000-0000-0000-0000-00000000000000\",\"Namespace\":\"mynamespace\",\"Via\":\"https://mynamespace.servicebus.windows.net/f8096791adb448579ee83d30e006a13e/?api-version=2016-07\",\"TrackingId\":\"5ee74c9e-72b5-4e98-97c4-08a62e56e221_G1\"}",
  "Status": "Succeeded",
  "Caller": "ServiceBus Client",
  "category": "OperationalLogs"
}
```

### <a name="events-and-operations-captured-in-operational-logs"></a>In Betriebsprotokollen erfasste Ereignisse und Vorgänge
Betriebsprotokolle erfassen alle Verwaltungsvorgänge, die für den Azure Service Bus-Namespace ausgeführt werden. Datenvorgänge werden aufgrund der hohen Menge an Datenvorgängen, die für Azure Service Bus durchgeführt werden, nicht erfasst.

> [!NOTE]
> Damit Sie Datenvorgänge besser nachverfolgen können, empfehlen wir Ihnen die clientseitige Nachverfolgung.

Die folgenden Verwaltungsvorgänge werden in Betriebsprotokollen erfasst: 

| `Scope` | Vorgang|
|-------| -------- |
| Namespace | <ul> <li> Erstellen des Namespaces</li> <li> Aktualisieren des Namespace </li> <li> Löschen des Namespace </li> <li> Aktualisieren des Namespace: SharedAccess-Richtlinie </li> </ul> | 
| Warteschlange | <ul> <li> Erstellen einer Warteschlange</li> <li> Aktualisieren einer Warteschlange</li> <li> Löschen einer Warteschlange </li> <li> Automatisches Löschen: Löschen einer Warteschlange </li> </ul> | 
| Thema | <ul> <li> Erstellen eines Themas </li> <li> Aktualisieren eines Themas </li> <li> Löschen eines Themas </li> <li> Automatisches Löschen: Löschen eines Themas </li> </ul> |
| Subscription | <ul> <li> Erstellen von Abonnements </li> <li> Aktualisieren des Abonnements </li> <li> Löschen des Abonnements </li> <li> Automatisches Löschen: Löschen eines Abonnements </li> </ul> |

> [!NOTE]
> *Lesevorgänge* werden in Betriebsprotokollen derzeit nicht nachverfolgt.

## <a name="azure-monitor-logs-tables"></a>Tabellen in Azure Monitor-Protokollen
Azure Service Bus verwendet Kusto-Tabellen aus Azure Monitor-Protokollen. Sie können diese Tabellen mit Log Analytics abfragen. Eine Liste der Kusto-Tabellen, die der Dienst verwendet, finden Sie unter [Referenz zur Azure Monitor-Protokolltabelle](/azure/azure-monitor/reference/tables/tables-resourcetype#service-bus).


## <a name="next-steps"></a>Nächste Schritte
- Ausführliche Informationen zum Überwachen von Azure Service Bus finden Sie unter [Überwachen von Azure Service Bus](monitor-service-bus.md).
- Ausführliche Informationen zur Überwachung von Azure-Ressourcen finden Sie unter [Überwachen von Azure-Ressourcen mit Azure Monitor](../azure-monitor/essentials/monitor-azure-resource.md).
