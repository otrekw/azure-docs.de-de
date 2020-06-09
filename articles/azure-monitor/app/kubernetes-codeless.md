---
title: 'Überwachen von Anwendungen in Azure Kubernetes Service (AKS) mit Application Insights: Azure Monitor | Microsoft-Dokumentation'
description: Azure Monitor kann nahtlos in Ihre in Kubernetes ausgeführte Anwendung integriert werden und ermöglicht eine blitzschnelle Erkennung von Problemen mit Ihren Apps.
ms.topic: conceptual
author: MS-jgol
ms.author: jgol
ms.date: 05/13/2020
ms.openlocfilehash: 76f9f922697ef6be6c959ea7f9bafd0872dba6ff
ms.sourcegitcommit: 318d1bafa70510ea6cdcfa1c3d698b843385c0f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2020
ms.locfileid: "83773772"
---
# <a name="zero-instrumentation-application-monitoring-for-kubernetes---azure-monitor-application-insights"></a>Anwendungsüberwachung ohne Instrumentierung für Kubernetes: Azure Monitor Application Insights

> [!IMPORTANT]
>  Aktuell können Sie den [eigenständigen Java-Agent](https://docs.microsoft.com/azure/azure-monitor/app/java-in-process-agent) verwenden, um die Überwachung für Ihre in Kubernetes ausgeführten Java-Apps zu aktivieren, ohne Ihren Code zu instrumentieren. Bis die Lösung für die nahtlose Aktivierung der Anwendungsüberwachung auch für andere Sprachen zur Verfügung steht, können Sie die SDKs verwenden, um Ihre in AKS ausgeführten Apps zu überwachen: [ASP.NET Core](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core), [ASP.NET](https://docs.microsoft.com/azure/azure-monitor/app/asp-net), [Node.js](https://docs.microsoft.com/azure/azure-monitor/app/nodejs), [JavaScript](https://docs.microsoft.com/azure/azure-monitor/app/javascript) und [Python](https://docs.microsoft.com/azure/azure-monitor/app/opencensus-python).

## <a name="application-monitoring-without-instrumenting-the-code"></a>Anwendungsüberwachung ohne Codeinstrumentierung
Aktuell ist die Aktivierung der Anwendungsüberwachung ohne Codeinstrumentierung nur in Java möglich. Wenn Sie Anwendungen in anderen Sprachen überwachen möchten, verwenden Sie die SDKs. 

## <a name="java"></a>Java
Nach der Aktivierung sammelt der Java-Agent automatisch eine Vielzahl von Anforderungen, Abhängigkeiten, Protokollen und Metriken aus den am häufigsten genutzten Bibliotheken und Frameworks.

Eine ausführliche Anleitung zur Überwachung Ihrer in Kubernetes ausgeführten Java-Apps sowie für andere Umgebungen finden Sie [hier](https://docs.microsoft.com/azure/azure-monitor/app/java-in-process-agent). 

## <a name="other-languages"></a>Andere Sprachen

Für Anwendungen in anderen Sprachen empfehlen wir aktuell die Verwendung der SDKs:
* [ASP.NET Core](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core)
* [ASP.NET](https://docs.microsoft.com/azure/azure-monitor/app/asp-net)
* [Node.js](https://docs.microsoft.com/azure/azure-monitor/app/nodejs) 
* [JavaScript](https://docs.microsoft.com/azure/azure-monitor/app/javascript)
* [Python](https://docs.microsoft.com/azure/azure-monitor/app/opencensus-python)

## <a name="next-steps"></a>Nächste Schritte

* Informieren Sie sich ausführlicher über [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview) und [Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview).
* Verschaffen Sie sich einen Überblick über die [verteilte Ablaufverfolgung](https://docs.microsoft.com/azure/azure-monitor/app/distributed-tracing), und informieren Sie sich über die Möglichkeiten der [Anwendungsübersicht](https://docs.microsoft.com/azure/azure-monitor/app/app-map?tabs=net) für Ihr Unternehmen.