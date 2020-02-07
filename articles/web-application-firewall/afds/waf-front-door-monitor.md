---
title: Überwachung und Protokollierung von Azure Web Application Firewall
description: Informationen zu Web Application Firewall (WAF) mit Front Door-Überwachung und -Protokollierung
author: vhorne
ms.service: web-application-firewall
ms.topic: article
services: web-application-firewall
ms.date: 08/21/2019
ms.author: victorh
ms.openlocfilehash: 3446df6effd85a07beb463c1caa40c5826a9e019
ms.sourcegitcommit: fa6fe765e08aa2e015f2f8dbc2445664d63cc591
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2020
ms.locfileid: "76934708"
---
# <a name="azure-web-application-firewall-monitoring-and-logging"></a>Überwachung und Protokollierung von Azure Web Application Firewall 

Überwachung und Protokollierung von Azure Web Application Firewall (WAF) werden mittels Protokollierung und Integration in Azure Monitor und Azure Monitor-Protokolle ermöglicht.

## <a name="azure-monitor"></a>Azure Monitor

WAF mit Front Door-Protokoll ist in [Azure Monitor](../../azure-monitor/overview.md) integriert. Über Azure Monitor können Sie Diagnoseinformationen einschließlich WAF-Warnungen und -Protokolle nachverfolgen. Sie können die WAF-Überwachung in der Front Door-Ressource im Portal auf der Registerkarte **Diagnose** oder direkt über den Azure Monitor-Dienst konfigurieren.

Navigieren Sie im Azure-Portal zum Front Door-Ressourcentyp. Auf der Registerkarte **Metriken** unter **Überwachung** können Sie **WebApplicationFirewallRequestCount** hinzufügen, um die Anzahl der mit WAF-Regeln übereinstimmenden Anforderungen nachzuverfolgen. Benutzerdefinierte Filter können basierend auf Aktionstypen und Regelnamen erstellt werden.

![WAFMetrics](../media/waf-frontdoor-monitor/waf-frontdoor-metrics.png)

## <a name="logs-and-diagnostics"></a>Protokolle und Diagnose

WAF mit Front Door bietet detaillierte Berichte zu jeder erkannten Bedrohung. Die Protokollierung ist in Azure Diagnostics-Protokolle integriert, und Warnungen werden in einem JSON-Format erfasst. Diese Protokolle können in [Azure Monitor-Protokolle](../../azure-monitor/insights/azure-networking-analytics.md) integriert werden.

![WAFDiag](../media/waf-frontdoor-monitor/waf-frontdoor-diagnostics.png)

FrontdoorAccessLog protokolliert alle Anforderungen, die an Back-Ends von Kunden weitergeleitet werden. FrontdoorWebApplicationFirewallLog protokolliert jede Anforderung, die mit einer WAF-Regel übereinstimmt.

Mit der folgenden Beispielabfrage werden WAF-Protokolle für blockierte Anforderungen abgerufen:

``` WAFlogQuery
AzureDiagnostics
| where ResourceType == "FRONTDOORS" and Category == "FrontdoorWebApplicationFirewallLog"
| where action_s == "Block"

```

Das folgende Beispiel ist eine protokollierte Anforderung im WAF-Protokoll:

``` WAFlogQuerySample
{
    "PreciseTimeStamp": "2020-01-25T00:11:19.3866091Z",
    "time": "2020-01-25T00:11:19.3866091Z",
    "category": "FrontdoorWebApplicationFirewallLog",
    "operationName": "Microsoft.Network/FrontDoor/WebApplicationFirewallLog/Write",
    "properties": {
        "clientIP": "xx.xx.xxx.xxx",
        "socketIP": "xx.xx.xxx.xxx",
        "requestUri": "https://wafdemofrontdoorwebapp.azurefd.net:443/?q=../../x",
        "ruleName": "Microsoft_DefaultRuleSet-1.1-LFI-930100",
        "policy": "WafDemoCustomPolicy",
        "action": "Block",
        "host": "wafdemofrontdoorwebapp.azurefd.net",
        "refString": "0p4crXgAAAABgMq5aIpu0T6AUfCYOroltV1NURURHRTA2MTMANjMxNTAwZDAtOTRiNS00YzIwLTljY2YtNjFhNzMyOWQyYTgy",
        "policyMode": "prevention"
    }
}

``` 

Mit der folgenden Beispielabfrage werden AccessLogs-Einträge abgerufen:

``` AccessLogQuery
AzureDiagnostics
| where ResourceType == "FRONTDOORS" and Category == "FrontdoorAccessLog"

```

Das folgende Beispiel ist eine protokollierte Anforderung im Access-Protokoll:

``` AccessLogSample
{
    "PreciseTimeStamp": "2020-01-25T00:11:12.0160150Z",
    "time": "2020-01-25T00:11:12.0160150Z",
    "category": "FrontdoorAccessLog",
    "operationName": "Microsoft.Network/FrontDoor/AccessLog/Write",
    "properties": {
        "trackingReference": "0n4crXgAAAACnRKbdALbyToAqNfSHssDvV1NURURHRTA2MTMANjMxNTAwZDAtOTRiNS00YzIwLTljY2YtNjFhNzMyOWQyYTgy",
        "httpMethod": "GET",
        "httpVersion": "2.0",
        "requestUri": "https://wafdemofrontdoorwebapp.azurefd.net:443/",
        "requestBytes": "710",
        "responseBytes": "3116",
        "userAgent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/81.0.4017.0 Safari/537.36 Edg/81.0.389.2",
        "clientIp": "xx.xx.xxx.xxx",
        "timeTaken": "0.598",
        "securityProtocol": "TLS 1.2",
        "routingRuleName": "WAFdemoWebAppRouting",
        "backendHostname": "wafdemouksouth.azurewebsites.net:443",
        "sentToOriginShield": false,
        "httpStatusCode": "200",
        "httpStatusDetails": "200"
    }
}

```

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu [Front Door](../../frontdoor/front-door-overview.md)