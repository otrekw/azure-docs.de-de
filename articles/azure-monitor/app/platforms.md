---
title: 'Application Insights: Programmiersprachen, Plattformen und Integrationsmöglichkeiten | Microsoft-Dokumentation'
description: Verfügbare Programmiersprachen, Plattformen und Integrationsmöglichkeiten für Application Insights
ms.topic: conceptual
ms.date: 07/18/2019
ms.reviewer: olegan
ms.openlocfilehash: cbda252fece638393ee7757dc165c4b009a0195e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79136722"
---
# <a name="supported-languages"></a>Unterstützte Sprachen

* [C#|VB (.NET)](../../azure-monitor/app/asp-net.md)
* [Java](../../azure-monitor/app/java-get-started.md)
* [JavaScript](../../azure-monitor/app/javascript.md)
* [Node.JS](../../azure-monitor/app/nodejs.md)
* [Python](../../azure-monitor/app/opencensus-python.md)

## <a name="supported-platforms-and-frameworks"></a>Unterstützte Plattformen und Frameworks

### <a name="instrumentation-for-already-deployed-applications-codeless-agent-based"></a>Instrumentierung für bereits bereitgestellte Anwendungen (ohne Code, Agent-basiert)
* [Virtuelle Azure-Computer und Azure-VM-Skalierungsgruppen](../../azure-monitor/app/azure-vm-vmss-apps.md)
* [Azure App Service](../../azure-monitor/app/azure-web-apps.md)
* [ASP.NET – Für Apps, die bereits aktiv sind.](../../azure-monitor/app/monitor-performance-live-website-now.md)
* [Azure Cloud Services](../../azure-monitor/app/cloudservices.md), einschließlich Web- und Workerrollen
* [Azure-Funktionen](https://docs.microsoft.com/azure/azure-functions/functions-monitoring)
### <a name="instrumentation-through-code-sdks"></a>Instrumentierung durch Code (SDKs)
* [ASP.NET](../../azure-monitor/app/asp-net.md)
* [ASP.NET Core](../../azure-monitor/app/asp-net-core.md)
* [Android](../../azure-monitor/learn/mobile-center-quickstart.md) (App Center)
* [iOS](../../azure-monitor/learn/mobile-center-quickstart.md) (App Center)
* [Java EE](../../azure-monitor/app/java-get-started.md)
* [Node.JS](https://www.npmjs.com/package/applicationinsights)
* [Python](../../azure-monitor/app/opencensus-python.md)
* [Universelle Windows-App](../../azure-monitor/learn/mobile-center-quickstart.md) (App Center)
* [Windows-Desktopanwendungen, -Dienste und -Workerrollen](../../azure-monitor/app/windows-desktop.md)

## <a name="logging-frameworks"></a>Protokollierungsframeworks
* [ILogger](https://docs.microsoft.com/azure/azure-monitor/app/ilogger)
* [Log4Net, NLog oder System.Diagnostics.Trace](../../azure-monitor/app/asp-net-trace-logs.md)
* [Java, Log4J oder Logback](../../azure-monitor/app/java-trace-logs.md)
* [LogStash-Plug-in](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-output-applicationinsights)
* [Azure Monitor](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/)

## <a name="export-and-data-analysis"></a>Export und Datenanalyse
* [Power BI](https://blogs.msdn.com/b/powerbi/archive/2015/11/04/explore-your-application-insights-data-with-power-bi.aspx)
* [Stream Analytics](../../azure-monitor/app/export-power-bi.md)

## <a name="unsupported-sdks"></a>Nicht unterstützte SDKs
Wir sind uns bewusst, dass es noch einige andere von der Community unterstützte SDKs gibt. Für Azure Monitor wird aber nur Support geleistet, wenn die auf dieser Seite aufgeführten unterstützten SDKs verwendet werden. Wir prüfen ständig neue Möglichkeiten zur Unterstützung anderer Sprachen. Verfolgen Sie daher die Seite mit den [GitHub-Ankündigungen](https://github.com/microsoft/ApplicationInsights-Announcements/issues), um die aktuellen SDK-News zu erhalten. 
