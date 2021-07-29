---
title: Datenreferenz zur Überwachung von Azure App Configuration
description: Wichtige Referenzmaterialien für die Überwachung von App Configuration
services: azure-app-configuration
author: AlexandraKemperMS
ms.author: alkemper
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 05/05/2021
ms.openlocfilehash: ec6515403574af29c47b03b55eb3cbc42ecd4a7a
ms.sourcegitcommit: c072eefdba1fc1f582005cdd549218863d1e149e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111968341"
---
# <a name="monitoring-app-configuration-data-reference"></a>Datenreferenz zur Überwachung von App Configuration

Bei diesem Artikel handelt es sich um einen Referenzartikel für die von App Configuration gesammelten Überwachungsdaten. Unter [Überwachen von App Configuration](monitor-app-configuration.md) finden Sie eine Anleitung zum Sammeln und Analysieren von Überwachungsdaten für App Configuration.

## <a name="metrics"></a>Metriken 
Ressourcenanbieter und -typ: [Plattformmetriken von App Configuration](../azure-monitor/essentials/metrics-supported.md#microsoftappconfigurationconfigurationstores)

| Metrik | Einheit | BESCHREIBUNG |
|-------|-----| ----- |
| Anzahl eingehender HTTP-Anforderungen   | Anzahl | Gesamtanzahl eingehender HTTP-Anforderungen |
|Dauer eingehender HTTP-Anforderungen | Millisekunden | Serverseitige Dauer einer HTTP-Anforderung |
| Anzahl gedrosselter HTTP-Anforderungen | Anzahl |    Gedrosselte Anforderungen sind HTTP-Anforderungen, die den Statuscode 429 (zu viele Anforderungen) zurückgeben. |

Weitere Informationen finden Sie in der Liste [aller in Azure Monitor unterstützten Plattformmetriken](../azure-monitor/essentials/metrics-supported.md).


## <a name="metric-dimensions"></a>Metrikdimensionen
Den Metriken in App Configuration sind die folgenden Dimensionen zugeordnet.

| Metrikname | Beschreibung der Dimension |
|-------|-----|
| Anzahl eingehender HTTP-Anforderungen | Die Anzahl aller HTTP-Anforderungen. Die unterstützten Dimensionen sind **HttpStatusCode** oder **AuthenticationScheme** jeder Anforderung. **AuthenticationScheme** kann nach AAD- oder HMAC-Authentifizierung gefiltert werden.   |
| Dauer eingehender HTTP-Anforderungen | Die serverseitige Dauer jeder Anforderung. Die unterstützten Dimensionen sind **HttpStatusCode** oder **AuthenticationScheme** jeder Anforderung. **AuthenticationScheme** kann nach AAD- oder HMAC-Authentifizierung gefiltert werden. |
| Anzahl gedrosselter HTTP-Anforderungen | Diese Metrik hat keine Dimensionen |

 Weitere Informationen zu Metrikdimensionen finden Sie unter [Mehrdimensionale Metriken](../azure-monitor/essentials/data-platform-metrics.md#multi-dimensional-metrics).

## <a name="resource-logs"></a>Ressourcenprotokolle
In diesem Abschnitt werden die Kategorietypen von Ressourcenprotokollen aufgelistet, die für App Configuration gesammelt werden. 

| Ressourcenprotokolltyp | Weitere Informationen|
|-------|-----|
| HttpRequest | [Informationen zur Ressourcenprotokollkategorie „App Configuration“](../azure-monitor/essentials/resource-logs-categories.md) |

Weitere Informationen finden Sie in der Liste [aller Typen von Ressourcenprotokollkategorien, die in Azure Monitor unterstützt werden](../azure-monitor/essentials/resource-logs-schema.md).
 
## <a name="azure-monitor-logs-tables"></a>Tabellen in Azure Monitor-Protokollen

Dieser Abschnitt bezieht sich auf alle Kusto-Tabellen in Azure Monitor-Protokollen, die für App Configuration relevant und für die Abfrage durch Log Analytics verfügbar sind.

|Ressourcentyp | Hinweise |
|-------|-----|
| [AACHttpRequest](/azure/azure-monitor/reference/tables/aachttprequest) | Einträge jeder HTTP-Anforderung, die an eine ausgewählte App Configuration-Ressource gesendet wird |
| [AzureActivity](/azure/azure-monitor/reference/tables/azureactivity) | Einträge aus dem Azure-Aktivitätsprotokoll, das Erkenntnisse zu Ereignissen auf Abonnement- oder Verwaltungsgruppenebene bietet, die in Azure aufgetreten sind |

Eine Referenz zu allen Azure Monitor-Protokollen und Log Analytics-Tabellen finden Sie in der [Referenz zu Tabellen in Azure Monitor-Protokollen](/azure/azure-monitor/reference/tables/tables-resourcetype).

### <a name="diagnostics-tables"></a>Diagnosetabellen

App Configuration verwendet die [AACHttpRequest-Tabelle](/azure/azure-monitor/reference/tables/aachttprequest), um Ressourcenprotokollinformationen zu speichern.

**HTTP-Anforderungen**

|Eigenschaft | Typ | BESCHREIBUNG |
|-------|-----| ----- |
|Category   |Zeichenfolge |Die Protokollkategorie des Ereignisses, immer HttpRequest 
|ClientIPAddress |  Zeichenfolge| Die IP-Adresse des Clients, der die Anforderung gesendet hat
|ClientRequestId|   Zeichenfolge| Vom Client bereitgestellte Anforderungs-ID
|CorrelationId| Zeichenfolge| GUID für korrelierte Protokolle
|DurationMs|    INT |Die Dauer des Vorgangs in Millisekunden.
|Methode (string)| HTTP| HTTP-Anforderungsmethode („get“ oder „post“)
|RequestId| Zeichenfolge| Eindeutige Anforderungs-ID, die vom Server generiert wird
|RequestLength| INT |Länge der HTTP-Anforderung in Bytes
|RequestURI|    Zeichenfolge| Der URI der Anforderung kann Schlüssel- und Bezeichnungsname enthalten. 
|_ResourceId|   Zeichenfolge| Ein eindeutiger Bezeichner für die Ressource, der der Datensatz zugeordnet ist.
|ResponseLength|    INT|    Länge der HTTP-Antwort in Bytes
|SourceSystem| Zeichenfolge|  
|StatusCode|    INT |Der HTTP-Statuscode der Anforderung
|TenantId|  Zeichenfolge  |WorkspaceId der Anforderung 
|TimeGenerated| datetime|   Zeitstempel (UTC) zum Zeitpunkt der Erstellung des Protokolls aufgrund einer gesendeten Anforderung
|type   |Zeichenfolge|    Der Name der Tabelle.
|UserAgent| Zeichenfolge| Vom Client bereitgestellter Benutzer-Agent


## <a name="see-also"></a>Weitere Informationen

* Eine Beschreibung der Überwachung von Azure App Configuration finden Sie unter [Überwachen von Azure App Configuration](monitor-app-configuration.md).
* Ausführliche Informationen zur Überwachung von Azure-Ressourcen finden Sie unter [Überwachen von Azure-Ressourcen mit Azure Monitor](../azure-monitor/essentials/monitor-azure-resource.md).