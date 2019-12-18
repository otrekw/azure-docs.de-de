---
title: Azure Application Insights – unterstützte Features von Azure Functions
description: In Application Insights unterstützte Features von Azure Functions
ms.service: azure-monitor
ms.subservice: application-insights
ms.topic: reference
author: TimothyMothra
ms.author: tilee
ms.date: 4/23/2019
ms.reviewer: mbullwin
ms.openlocfilehash: 5688bd92e63016e195726488bd9ff9ed039c5795
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2019
ms.locfileid: "74928420"
---
# <a name="application-insights-for-azure-functions-supported-features"></a>Unterstützte Features in Application Insights für Azure Functions

Azure Functions bietet [von Haus aus Integration](../../azure-functions/functions-monitoring.md) in Application Insights, die über die ILogger-Schnittstelle verfügbar ist. Unten folgt die Liste der aktuell unterstützten Features. Arbeiten Sie zum [Einstieg](../../azure-functions/functions-monitoring.md#enable-application-insights-integration) das Azure Functions-Handbuch durch.

## <a name="supported-features"></a>Unterstützte Features

| Azure-Funktionen                       | V1                | V2 (Ignite 2018)  | 
|-----------------------------------    |---------------    |------------------ |
| **Application Insights .NET SDK**   | **2.5.0**       | **2.9.1**         |
| | | | 
| **Automatische Sammlung von**        |                 |                   |               
| &bull;Anforderungen                     | Ja             | Ja               | 
| &bull;Ausnahmen                   | Ja             | Ja               | 
| &bull; Leistungsindikatoren         | Ja             | Ja               |
| &bull;Abhängigkeiten                   |                   |                   |               
| &nbsp;&nbsp;&nbsp;&mdash; HTTP      |                 | Ja               | 
| &nbsp;&nbsp;&nbsp;&mdash; ServiceBus|                 | Ja               | 
| &nbsp;&nbsp;&nbsp;&mdash; EventHub  |                 | Ja               | 
| &nbsp;&nbsp;&nbsp;&mdash; SQL       |                 | Ja               | 
| | | | 
| **Unterstützte Features**                |                   |                   |               
| &bull; QuickPulse/LiveMetrics       | Ja             | Ja               | 
| &nbsp;&nbsp;&nbsp;&mdash; Sicherer Steuerkanal|                 | Ja               | 
| &bull; Stichprobenentnahme                     | Ja             | Ja               | 
| &bull; Heartbeats                   |                 | Ja               | 
| | | | 
| **Korrelation**                       |                   |                   |               
| &bull; ServiceBus                     |                   | Ja               | 
| &bull; EventHub                       |                   | Ja               | 
| | | | 
| **Konfigurierbar**                      |                   |                   |           
| &bull;Vollständig konfigurierbar.<br/>Anweisungen finden Sie unter [Azure Functions](https://github.com/Microsoft/ApplicationInsights-aspnetcore/issues/759#issuecomment-426687852).<br/>Alle Optionen finden Sie unter [Asp.NET Core](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Custom-Configuration).               |                   | Ja                   | 


## <a name="performance-counters"></a>Leistungsindikatoren

Automatische Erfassung von Leistungsindikatoren kann nur für Windows-Computer verwendet werden.


## <a name="live-metrics--secure-control-channel"></a>Livemetriken und sicherer Steuerkanal

Die von Ihnen angegebenen benutzerdefinierten Filterkriterien werden an die Livemetrikkomponente des Application Insights SDK zurückgesendet. Der Filter können potenziell vertrauliche Informationen wie z.B. Kunden-IDs enthalten. Sie können den Kanal mit einem geheimen API-Schlüssel sicher machen. Anleitungen dazu finden Sie unter [Sichern des Steuerkanals](https://docs.microsoft.com/azure/azure-monitor/app/live-stream#secure-the-control-channel).

## <a name="sampling"></a>Stichproben

Azure Functions aktiviert die Stichprobenentnahme in der Konfiguration standardmäßig. Weitere Informationen finden Sie unter [Configure Sampling](https://docs.microsoft.com/azure/azure-functions/functions-monitoring#configure-sampling) (Konfigurieren der Stichprobenentnahme).

Wenn Ihr Projekt zur manuellen Nachverfolgung der Telemetrie vom Application Insights SDK abhängig ist, kann ein unerwartetes Verhalten auftreten, wenn Ihre Konfiguration der Stichprobenentnahme von der Konfiguration in Functions abweicht. 

Es wird empfohlen, die gleiche Konfiguration wie Functions zu verwenden. Bei **Functions v2** können Sie mithilfe der Abhängigkeitsinjektion in Ihrem Konstruktor die gleiche Konfiguration erzielen:

```csharp
using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.Extensibility;

public class Function1 
{

    private readonly TelemetryClient telemetryClient;

    public Function1(TelemetryConfiguration configuration)
    {
        this.telemetryClient = new TelemetryClient(configuration);
    }

    [FunctionName("Function1")]
    public async Task<IActionResult> Run(
        [HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = null)] HttpRequest req, ILogger logger)
    {
        this.telemetryClient.TrackTrace("C# HTTP trigger function processed a request.");
    }
}
```
