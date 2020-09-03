---
title: Überwachung von Metriken und Protokollen in Azure Front Door | Microsoft-Dokumentation
description: Dieser Artikel beschreibt die verschiedenen Metriken und Zugriffsprotokolle, die Azure Front Door unterstützt.
services: frontdoor
documentationcenter: ''
author: duongau
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/18/2018
ms.author: duau
ms.openlocfilehash: 6f5051dd7dedcc49320557f17148bcdc9bf539ab
ms.sourcegitcommit: 5a3b9f35d47355d026ee39d398c614ca4dae51c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89399751"
---
# <a name="monitoring-metrics-and-logs-in-azure-front-door"></a>Überwachung von Metriken und Protokollen in Azure Front Door

Mit Azure Front Door können Sie Ressourcen auf die folgenden Arten überwachen:

- **Metriken:** Azure Front Door verfügt derzeit über sieben Metriken, um Leistungsindikatoren anzuzeigen.
- **Protokolle**. Protokolle ermöglichen das Speichern und Nutzen von Leistungs-, Zugriffs- und anderen Daten einer Ressource zu Überwachungszwecken.

### <a name="metrics"></a>Metriken

Metriken sind ein Feature für bestimmte Azure-Ressourcen, mit dem Sie die Leistungsindikatoren im Portal anzeigen können. Die folgenden Front Door-Metriken stehen zur Verfügung:

| Metrik | Metrikanzeigename | Einheit | Dimensionen | BESCHREIBUNG |
| --- | --- | --- | --- | --- |
| RequestCount | Anzahl der Anforderungen | Anzahl | HttpStatus</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | Die Anzahl der von Front Door Service bereitgestellten Clientanforderungen.  |
| RequestSize | Anforderungsgröße | Byte | HttpStatus</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | Die Anzahl der von Clients als Anforderungen an Front Door Service gesendeten Bytes. |
| ResponseSize | Antwortgröße | Byte | HttpStatus</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | Die Anzahl der von Front Door Service als Antworten an Clients gesendeten Bytes. |
| TotalLatency | Gesamtlatenz | Millisekunden | HttpStatus</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | Die Zeitspanne zwischen dem Empfang der Clientanforderung durch Front Door und der Bestätigung des letzten Antwortbytes von Front Door durch den Client. |
| BackendRequestCount | Back-End-Anforderungsanzahl | Anzahl | HttpStatus</br>HttpStatusGroup</br>Back-End | Die Anzahl der von Front Door Service an Back-Ends gesendeten Anforderungen. |
| BackendRequestLatency | Latenz der Back-End-Anforderung | Millisekunden | Back-End | Die Zeitspanne zwischen dem Senden der Anforderung durch Front Door Service an das Back-End und dem Empfang des letzten Antwortbytes durch Front Door Service vom Back-End. |
| BackendHealthPercentage | Prozentsatz der Back-End-Integrität | Percent | Back-End</br>BackendPool | Der Prozentsatz der erfolgreichen Integritätstests von Front Door Service zu Back-Ends. |
| WebApplicationFirewallRequestCount | Anforderungsanzahl für die Web Application Firewall | Anzahl | PolicyName</br>RuleName</br>Aktion | Die Anzahl der Clientanforderungen, die von der Anwendungssicherheitsschicht von Front Door Service verarbeitet werden. |

## <a name="activity-logs"></a><a name="activity-log"></a>Aktivitätsprotokolle

Aktivitätsprotokolle geben Informationen zu den Front Door-Vorgängen. Damit können Sie auch die Antworten auf die Fragen „Was“, „Wer“ und „Wann“ für alle Schreibvorgänge (put, post oder delete) in Ihrer Front Door-Instanz ermitteln.

>[!NOTE]
>Aktivitätsprotokolle umfassen keine Lesevorgänge (get). Sie enthalten auch keine Vorgänge, die Sie entweder mithilfe des Azure-Portals oder der ursprünglichen Verwaltungs-API ausführen.

Greifen Sie auf Aktivitätsprotokolle in Ihrer Front Door-Instanz oder alle Protokolle Ihrer Azure-Ressourcen in Azure Monitor zu. So zeigen Sie Aktivitätsprotokolle an:

1. Wählen Sie Ihre Front Door-Instanz aus.
2. Wählen Sie **Aktivitätsprotokoll** aus.

    ![Aktivitätsprotokoll](./media/front-door-diagnostics/activity-log.png)

3. Wählen Sie einen Filterbereich und dann **Übernehmen** aus.

## <a name="diagnostic-logs"></a><a name="diagnostic-logging"></a>Diagnoseprotokolle
Diagnoseprotokolle bieten umfassende Informationen zu Vorgängen und Fehlern, die zur Überwachung und Problembehandlung relevant sind. Diagnoseprotokolle unterscheiden sich von Aktivitätsprotokollen.

Aktivitätsprotokolle geben Einblick in die Vorgänge, die für Azure-Ressourcen ausgeführt wurden. Diagnoseprotokolle bieten Einblicke in Vorgänge, die Ihre Ressource ausgeführt hat. Weitere Informationen finden Sie unter [Erfassen und Nutzen von Protokolldaten aus Ihren Azure-Ressourcen](../azure-monitor/platform/platform-logs-overview.md).

![Diagnoseprotokolle](./media/front-door-diagnostics/diagnostic-log.png)

So konfigurieren Sie Diagnoseprotokolle für Ihre Front Door-Instanz

1. Wählen Sie Ihre Azure Front Door-Instanz aus.

2. Wählen Sie **Azure-Diagnoseeinstellungen** aus.

3. Wählen Sie **Diagnose aktivieren**. Archivieren Sie Diagnoseprotokolle zusammen mit Metriken in einem Speicherkonto, streamen Sie sie an einen Event Hub, oder senden Sie sie an Azure Monitor-Protokolle.

Front Door stellt derzeit Diagnoseprotokolle bereit (stündlicher Batch). Diagnoseprotokolle enthalten einzelne API-Anforderungen, wobei jeder Eintrag folgendem Schema entspricht:

| Eigenschaft  | BESCHREIBUNG |
| ------------- | ------------- |
| BackendHostname | Wenn die Anforderung an ein Back-End weitergeleitet wurde, stellt dieses Feld den Hostnamen des Back-Ends dar. Dieses Feld ist leer, wenn die Anforderung umgeleitet oder an einen regionalen Cache weitergeleitet wurde (sofern das Zwischenspeichern für die Routingregel aktiviert ist). |
| CacheStatus | Bei Zwischenspeicherungsszenarien definiert dieses Feld den Cachetreffer/Cachefehler im POP. |
| ClientIp | Die IP-Adresse des Clients, der die Anforderung gestellt hat. Wenn in der Anforderung ein X-Forwarded-For-Header vorhanden ist, wird die Client-IP daraus ausgewählt. |
| ClientPort | Der IP-Port des Clients, der die Anforderung gestellt hat. |
| HttpMethod | Von der Anforderung verwendete HTTP-Methode |
| HttpStatusCode | Der HTTP-Statuscode, der vom Proxy zurückgegeben wurde. |
| HttpStatusDetails | Resultierender Status in der Anforderung. Informationen zur Bedeutung dieses Zeichenfolgenwerts finden Sie in der Verweistabelle zum Status. |
| HttpVersion | Typ der Anforderung oder Verbindung. |
| POP | Kurzname des Edges, in dem die Anforderung gelandet ist. |
| RequestBytes | Die Größe der HTTP-Anforderungsnachricht in Byte, einschließlich Anforderungsheader und -text. |
| RequestUri | URI der empfangenen Anforderung |
| ResponseBytes | Die vom Back-End-Server als Antwort gesendeten Bytes.  |
| RoutingRuleName | Der Name der Routingregel, der die Anforderung entspricht. |
| RulesEngineMatchNames | Die Namen der Regeln, mit denen die Anforderung übereinstimmte. |
| SecurityProtocol | Die TLS-/SSL-Protokollversion, die von der Anforderung verwendet wird, oder NULL, wenn keine Verschlüsselung verwendet wird. |
| SentToOriginShield | Boolesches Feld, das angibt, ob in der ersten Umgebung ein Cachefehler aufgetreten ist und die Anforderung an den regionalen Cache gesendet wurde. Ignorieren Sie dieses Feld, wenn die Routingregel eine Umleitung ist oder wenn das Zwischenspeichern nicht aktiviert ist. |
| TimeTaken | Die Dauer vom ersten Byte der Anforderung an Front Door bis zum letzten Byte der Antwort in Sekunden. |
| TrackingReference | Die eindeutige Verweiszeichenfolge zur Identifizierung einer Anforderung, die von der Front Door-Instanz verarbeitet wird. Diese wird auch als X-Azure-Ref-Header an den Client gesendet. Sie ist für die Suche nach Informationen in den Zugriffsprotokollen für eine bestimmte Anforderung erforderlich. |
| UserAgent | Der vom Client verwendete Browsertyp |

**Hinweis:** Für verschiedene Routingkonfigurationen und Datenverkehrsverhalten können einige der Felder wie „backendHostname“, „cacheStatus“, „sentToOriginShield“ und „POP“ mit unterschiedlichen Werten antworten. In der folgenden Tabelle werden die unterschiedlichen Werte erläutert, die diese Felder in verschiedenen Szenarien enthalten können:

| Szenarien | Anzahl der Protokolleinträge | POP | BackendHostname | SentToOriginShield | CacheStatus |
| ------------- | ------------- | ------------- | ------------- | ------------- | ------------- |
| Routingregel ohne aktiviertes Zwischenspeichern | 1 | Edge-POP-Code | Das Back-End, an das die Anforderung weitergeleitet wurde | False | CONFIG_NOCACHE |
| Routingregel mit aktiviertem Zwischenspeichern. Cachetreffer beim Edge-POP | 1 | Edge-POP-Code | Leer | False | HIT |
| Routingregel mit aktiviertem Zwischenspeichern. Cachefehler bei Edge-POP, aber Cachetreffer beim übergeordneten Cache-POP | 2 | 1. Edge-POP-Code</br>2. POP-Code des übergeordneten Caches | 1. POP-Hostname des übergeordneten Caches</br>2. Leer | 1. True</br>2. False | 1. MISS</br>2. PARTIAL_HIT |
| Routingregel mit aktiviertem Zwischenspeichern. Cachefehler beim Edge-POP und übergeordnetem Cache-POP | 2 | 1. Edge-POP-Code</br>2. POP-Code des übergeordneten Caches | 1. POP-Hostname des übergeordneten Caches</br>2. Back-End zum Auffüllen des Caches | 1. True</br>2. False | 1. MISS</br>2. MISS |


## <a name="next-steps"></a>Nächste Schritte

- [Schnellstart: Erstellen einer Front Door-Instanz](quickstart-create-front-door.md)
- [Übersicht über die Routingarchitektur](front-door-routing-architecture.md)
