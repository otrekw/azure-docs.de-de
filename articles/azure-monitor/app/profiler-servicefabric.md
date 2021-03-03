---
title: Erstellen von Profilen für Azure Service Fabric-Live-Apps mit Application Insights
description: Aktivieren des Profilers für eine Service Fabric-Anwendung
ms.topic: conceptual
ms.custom: devx-track-dotnet
author: cweining
ms.author: cweining
ms.date: 08/06/2018
ms.reviewer: mbullwin
ms.openlocfilehash: 67e7765a1f46c2be5790c11687e06ea624702b9b
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/17/2021
ms.locfileid: "100589565"
---
# <a name="profile-live-azure-service-fabric-applications-with-application-insights"></a>Erstellen von Profilen für Azure Service Fabric-Liveanwendungen mit Application Insights

Application Insights Profiler kann auch für diese Dienste bereitgestellt werden:
* [Azure App Service](profiler.md?toc=/azure/azure-monitor/toc.json)
* [Azure Cloud Services](profiler-cloudservice.md?toc=/azure/azure-monitor/toc.json)
* [Dokumentation zu virtuellen Computern](profiler-vm.md?toc=/azure/azure-monitor/toc.json)

## <a name="set-up-the-environment-deployment-definition"></a>Einrichten der Definition für die Umgebungsbereitstellung

Application Insights Profiler ist in Azure-Diagnose enthalten. Sie können die Azure-Diagnoseerweiterung mithilfe einer Azure Resource Manager-Vorlage für Ihren Service Fabric-Cluster installieren. Rufen Sie eine [Vorlage zur Installation von Azure-Diagnose auf einem Service Fabric-Cluster](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/ServiceFabricCluster.json) ab.

Zum Einrichten Ihrer Umgebung führen Sie folgende Schritte aus:

1. Der Profiler unterstützt .NET Framework und .Net Core. Wenn Sie .NET Framework verwenden, stellen Sie sicher, dass Sie [.NET Framework 4.6.1](/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) oder höher verwenden. Es ist ausreichend zu bestätigen, dass das bereitgestellte Betriebssystem `Windows Server 2012 R2` oder höher ist. Der Profiler unterstützt .NET Core 2.1 und neuere Anwendungen.

1. Suchen Sie nach der [Azure-Diagnoseerweiterung](../agents/diagnostics-extension-overview.md) in der Bereitstellungsvorlagendatei.

1. Fügen Sie den folgenden `SinksConfig`-Abschnitt als untergeordnetes Element von `WadCfg` hinzu. Ersetzen Sie den `ApplicationInsightsProfiler`-Eigenschaftswert durch Ihren eigenen Application Insights-Instrumentierungsschlüssel:  

      ```json
      "SinksConfig": {
        "Sink": [
          {
            "name": "MyApplicationInsightsProfilerSink",
            "ApplicationInsightsProfiler": "00000000-0000-0000-0000-000000000000"
          }
        ]
      }
      ```

      Informationen zum Hinzufügen der Diagnoseerweiterung zu Ihrer Bereitstellungsvorlage finden Sie unter [Verwenden von Überwachung und Diagnose bei einer Windows-VM und Azure Resource Manager-Vorlagen](../../virtual-machines/extensions/diagnostics-template.md?toc=/azure/virtual-machines/windows/toc.json).

1. Stellen Sie Ihren Service Fabric-Cluster über Ihre Azure Resource Manager-Vorlage bereit.  
  Wenn Ihre Einstellungen richtig sind, wird Application Insights Profiler installiert und aktiviert, sobald die Azure-Diagnoseerweiterung installiert wird. 

1. Fügen Sie Application Insights Ihrer Service Fabric-Anwendung hinzu.  
  Damit Profiler Profile für Ihre Anforderungen erfasst, muss Ihre Anwendung Vorgänge mit Application Insights nachverfolgen. Für zustandslose APIs können Sie sich auf die Anweisungen für [Nachverfolgen von Anforderungen für die Profilerstellung](profiler-trackrequests.md?toc=/azure/azure-monitor/toc.json) beziehen. Weitere Informationen zum Nachverfolgen benutzerdefinierter Vorgänge in anderen Arten von Anwendungen finden Sie unter [Nachverfolgen benutzerdefinierter Vorgänge mit dem Application Insights .NET SDK](custom-operations-tracking.md?toc=/azure/azure-monitor/toc.json).

1. Stellen Sie Ihre Anwendung erneut bereit.


## <a name="next-steps"></a>Nächste Schritte

* Generieren Sie Datenverkehr zu Ihrer Anwendung (starten Sie z.B. einen [Verfügbarkeitstest](monitor-web-app-availability.md)). Warten Sie dann 10 bis 15 Minuten, bis das Senden der Ablaufverfolgungen an die Application Insights-Instanz beginnt.
* Weitere Informationen finden Sie unter [Profiler-Ablaufverfolgungen](profiler-overview.md?toc=/azure/azure-monitor/toc.json) im Azure-Portal.
* Hilfe bei der Behandlung von Profiler-Problemen finden Sie unter [Problembehandlung für den Profiler](profiler-troubleshooting.md?toc=/azure/azure-monitor/toc.json).
