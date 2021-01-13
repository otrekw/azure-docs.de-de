---
title: Überwachung und Protokollierung von Azure Web Application Firewall
description: Informationen zu Web Application Firewall (WAF) mit Front Door-Überwachung und -Protokollierung
author: vhorne
ms.service: web-application-firewall
ms.topic: article
services: web-application-firewall
ms.date: 06/09/2020
ms.author: victorh
ms.openlocfilehash: 596374d4f3f188e08a10bd25b36b178cc79a6e57
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "84808955"
---
# <a name="azure-web-application-firewall-monitoring-and-logging"></a>Überwachung und Protokollierung von Azure Web Application Firewall

Überwachung und Protokollierung von Azure Web Application Firewall (WAF) werden mittels Protokollierung und Integration in Azure Monitor und Azure Monitor-Protokolle ermöglicht.

## <a name="azure-monitor"></a>Azure Monitor

WAF mit Front Door-Protokoll ist in [Azure Monitor](../../azure-monitor/overview.md) integriert. Über Azure Monitor können Sie Diagnoseinformationen einschließlich WAF-Warnungen und -Protokolle nachverfolgen. Sie können die WAF-Überwachung in der Front Door-Ressource im Portal auf der Registerkarte **Diagnose** oder direkt über den Azure Monitor-Dienst konfigurieren.

Navigieren Sie im Azure-Portal zum Front Door-Ressourcentyp. Auf der Registerkarte **Metriken** unter **Überwachung** können Sie **WebApplicationFirewallRequestCount** hinzufügen, um die Anzahl der mit WAF-Regeln übereinstimmenden Anforderungen nachzuverfolgen. Benutzerdefinierte Filter können basierend auf Aktionstypen und Regelnamen erstellt werden.

:::image type="content" source="../media/waf-frontdoor-monitor/waf-frontdoor-metrics.png" alt-text="WAFMetrics ":::

## <a name="logs-and-diagnostics"></a>Protokolle und Diagnose

WAF mit Front Door bietet detaillierte Berichte zu jeder erkannten Bedrohung. Die Protokollierung ist in Azure Diagnostics-Protokolle integriert, und Warnungen werden in einem JSON-Format erfasst. Diese Protokolle können in [Azure Monitor-Protokolle](../../azure-monitor/insights/azure-networking-analytics.md) integriert werden.

![WAFDiag](../media/waf-frontdoor-monitor/waf-frontdoor-diagnostics.png)

In [FrontdoorAccessLog](../../frontdoor/front-door-diagnostics.md) werden alle Anforderungen protokolliert. In FrontdoorWebApplicationFirewallLog wird jede Anforderung protokolliert, die mit einer WAF-Regel im nachstehenden Schema übereinstimmt:

| Eigenschaft  | BESCHREIBUNG |
| ------------- | ------------- |
|Aktion|Aktion, die für die Anforderung durchgeführt wird|
| ClientIp | Die IP-Adresse des Clients, der die Anforderung gestellt hat. Wenn in der Anforderung ein X-Forwarded-For-Header vorhanden ist, wird die Client-IP-Adresse aus dem Headerfeld ausgewählt. |
| ClientPort | Der IP-Port des Clients, der die Anforderung gestellt hat. |
| Details|Weitere Details zur übereinstimmenden Anforderung |
|| matchVariableName: der HTTP-Parametername der übereinstimmenden Anforderung, z. B. Headernamen|
|| matchVariableValue: Werte, die die Übereinstimmung ausgelöst haben|
| Host | Der Hostheader der übereinstimmenden Anforderung |
| Richtlinie | Der Name der WAF-Richtlinie, mit der die Anforderung übereinstimmt |
| PolicyMode | Der Betriebsmodus der WAF-Richtlinie. Mögliche Werte sind „Prevention“ und „Detection“. |
| RequestUri | Vollständiger URI der übereinstimmenden Anforderung |
| RuleName | Der Name der WAF-Regel, mit der die Anforderung übereinstimmt |
| SocketIp | Die für WAF angegebene IP-Quelladresse. Diese IP-Adresse basiert unabhängig von den Anforderungsheadern auf der TCP-Sitzung.|
| TrackingReference | Die eindeutige Verweiszeichenfolge zur Identifizierung einer Anforderung, die von der Front Door-Instanz verarbeitet wird. Diese wird auch als X-Azure-Ref-Header an den Client gesendet. Sie ist für die Suche nach Informationen in den Zugriffsprotokollen für eine bestimmte Anforderung erforderlich. |

Die folgende Beispielabfrage gibt WAF-Protokolle für blockierte Anforderungen zurück:

``` WAFlogQuery
AzureDiagnostics
| where ResourceType == "FRONTDOORS" and Category == "FrontdoorWebApplicationFirewallLog"
| where action_s == "Block"

```

Das folgende Beispiel ist eine protokollierte Anforderung im WAF-Protokoll:

``` WAFlogQuerySample
{
    "time":  "2020-06-09T22:32:17.8376810Z",
    "category": "FrontdoorWebApplicationFirewallLog",
    "operationName": "Microsoft.Network/FrontDoorWebApplicationFirewallLog/Write",
    "properties":
    {
        "clientIP":"xxx.xxx.xxx.xxx",
        "clientPort":"52097",
        "socketIP":"xxx.xxx.xxx.xxx",
        "requestUri":"https://wafdemofrontdoorwebapp.azurefd.net:443/?q=%27%20or%201=1",
        "ruleName":"Microsoft_DefaultRuleSet-1.1-SQLI-942100",
        "policy":"WafDemoCustomPolicy",
        "action":"Block",
        "host":"wafdemofrontdoorwebapp.azurefd.net",
        "trackingReference":"08Q3gXgAAAAAe0s71BET/QYwmqtpHO7uAU0pDRURHRTA1MDgANjMxNTAwZDAtOTRiNS00YzIwLTljY2YtNjFhNzMyOWQyYTgy",
        "policyMode":"prevention",
        "details":
            {
            "matches":
                [{
                "matchVariableName":"QueryParamValue:q",
                "matchVariableValue":"' or 1=1"
                }]
            }
     }
}
```

Die folgende Beispielabfrage gibt AccessLogs-Einträge zurück:

``` AccessLogQuery
AzureDiagnostics
| where ResourceType == "FRONTDOORS" and Category == "FrontdoorAccessLog"

```

Das folgende Beispiel ist eine protokollierte Anforderung im Access-Protokoll:

``` AccessLogSample
{
"time": "2020-06-09T22:32:17.8383427Z",
"category": "FrontdoorAccessLog",
"operationName": "Microsoft.Network/FrontDoor/AccessLog/Write",
 "properties":
    {
    "trackingReference":"08Q3gXgAAAAAe0s71BET/QYwmqtpHO7uAU0pDRURHRTA1MDgANjMxNTAwZDAtOTRiNS00YzIwLTljY2YtNjFhNzMyOWQyYTgy",
    "httpMethod":"GET",
    "httpVersion":"2.0",
    "requestUri":"https://wafdemofrontdoorwebapp.azurefd.net:443/?q=%27%20or%201=1",
    "requestBytes":"715",
    "responseBytes":"380",
    "userAgent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/85.0.4157.0 Safari/537.36 Edg/85.0.531.1",
    "clientIp":"xxx.xxx.xxx.xxx",
    "socketIp":"xxx.xxx.xxx.xxx",
    "clientPort":"52097",
    "timeTaken":"0.003",
    "securityProtocol":"TLS 1.2",
    "routingRuleName":"WAFdemoWebAppRouting",
    "rulesEngineMatchNames":[],
    "backendHostname":"wafdemowebappuscentral.azurewebsites.net:443",
    "sentToOriginShield":false,
    "httpStatusCode":"403",
    "httpStatusDetails":"403",
    "pop":"SJC",
    "cacheStatus":"CONFIG_NOCACHE"
    }
}

```

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu [Front Door](../../frontdoor/front-door-overview.md)
