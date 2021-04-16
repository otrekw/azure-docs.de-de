---
title: Bewährte Methoden für Azure Service Fabric-Überwachung
description: Bewährte Methoden und Entwurfsüberlegungen zum Überwachen von Clustern und Anwendungen mithilfe von Azure Service Fabric.
author: peterpogorski
ms.topic: conceptual
ms.date: 01/23/2019
ms.author: pepogors
ms.openlocfilehash: 0eb9ce24f9ead44b7ba5a4d28d24177e62cb7757
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104950517"
---
# <a name="monitoring-and-diagnostic-best-practices-for-azure-service-fabric"></a>Best Practices für Überwachung und Diagnose für Azure Service Fabric

Die [Überwachung und die Diagnose](./service-fabric-diagnostics-overview.md) sind wichtig für die Entwicklung und Durchführung von Tests sowie für die Bereitstellung von Workloads in beliebigen Cloudumgebungen. So können Sie beispielsweise die Nutzung Ihrer Anwendungen, die Aktionen der Service Fabric-Plattform, die Ressourcenverwendung (mithilfe von Leistungsindikatoren) sowie die allgemeine Integrität Ihres Clusters nachverfolgen. Auf der Grundlage dieser Informationen können Sie Probleme diagnostizieren, beheben und in Zukunft vermeiden.

## <a name="application-monitoring"></a>Anwendungsüberwachung

Bei der Anwendungsüberwachung wird nachverfolgt, wie die Funktionen und Komponenten Ihrer Anwendung genutzt werden. Überwachen Sie Ihre Anwendungen, um sicherzustellen, dass die für Benutzer relevanten Probleme erkannt werden. Für die Anwendungsüberwachung sind die Personen zuständig, die eine Anwendung und die zugehörigen Dienste entwickeln, da die Überwachung stark von der Geschäftslogik Ihrer Anwendung abhängig ist. Es wird empfohlen, dass Sie die Anwendungsüberwachung mit [Application Insights](./service-fabric-tutorial-monitoring-aspnet.md) einrichten. Dies ist das Tool, das in Azure für die Anwendungsüberwachung vorgesehen ist.

## <a name="cluster-monitoring"></a>Clusterüberwachung

Ein Ziel von Service Fabric besteht darin, für Anwendungen die Resilienz in Bezug auf Hardwarefehler sicherzustellen. Dies ist möglich, weil die Systemdienste der Plattform in der Lage sind, Infrastrukturprobleme zu erkennen und für Workloads schnell ein Failover auf andere Knoten im Cluster durchzuführen. Aber was passiert, wenn für die Systemdienste selbst Probleme auftreten? Oder was geschieht, wenn beim Bereitstellen oder Verschieben einer Workload Regeln für die Platzierung von Diensten verletzt werden? Service Fabric ermöglicht eine Diagnose dieser und anderer Probleme, um sicherzustellen, dass Sie darüber informiert sind, wie die Service Fabric-Plattform mit Ihren Anwendungen, Diensten, Containern und Knoten interagiert.

Für Windows-Cluster wird empfohlen, die Clusterüberwachung mit dem [Diagnose-Agent](./service-fabric-diagnostics-event-aggregation-wad.md) und mit [Azure Monitor-Protokolle](./service-fabric-diagnostics-oms-setup.md) einzurichten.

Für Linux-Cluster ist Azure Monitor-Protokolle ebenfalls das empfohlene Tool für die Überwachung der Azure-Plattform und -Infrastruktur. Für die Diagnose von Linux-Plattformen ist eine andere Konfiguration erforderlich. Dies ist unter [Service Fabric-Linux-Clusterereignisse in Syslog](./service-fabric-diagnostics-oms-syslog.md) beschrieben.

## <a name="infrastructure-monitoring"></a>Infrastrukturüberwachung

[Azure Monitor-Protokolle](./service-fabric-diagnostics-oms-agent.md) wird empfohlen, um Ereignisse auf Clusterebene zu überwachen. Nachdem Sie den Log Analytics-Agent wie unter dem obigen Link beschrieben für Ihren Arbeitsbereich konfiguriert haben, können Sie Leistungsmetriken erfassen. Beispiele hierfür sind CPU-Auslastung, .NET-Leistungsindikatoren (z.B. CPU-Auslastung auf Prozessebene), Service Fabric-Leistungsindikatoren (z.B. Anzahl von Ausnahmen für einen zuverlässigen Dienst) und Containermetriken (z.B. CPU-Auslastung).  Sie müssen Containerprotokolle in stdout oder stderr schreiben, damit sie in Azure Monitor-Protokolle verfügbar sind.

## <a name="watchdogs"></a>Watchdogs

Ein Watchdog ist im Allgemeinen ein separater Dienst, der die Integrität und Last dienstübergreifend überwacht, Pings an Endpunkte sendet und unerwartete Integritätsereignisse im Cluster meldet. So können Fehler verhindert werden, die anhand der Leistung eines einzelnen Diensts allein nicht erkannt werden können. Watchdogs sind auch ein guter Ort zum Hosten von Code, mit dem Maßnahmen zur Problembehandlung durchgeführt werden, ohne dass eine Benutzeraktion erforderlich ist (z. B. Bereinigen von Protokolldateien im Speicher nach bestimmten Zeitintervallen). Informieren Sie sich über das [FabricObserver](https://aka.ms/sf/FabricObserver)-Projekt, wenn Sie einen vollständig implementierten Open-Source-Dienst für den SF-Watchdog benötigen, der ein benutzerfreundliches Watchdogerweiterungsmodell umfasst und sowohl in Windows- als auch in Linux-Clustern ausgeführt wird. Bei FabricObserver handelt es sich um produktionsbereite Software. Wir empfehlen, FabricObserver in Ihren Test- und Produktionsclustern bereitzustellen und zu erweitern, um Ihre Anforderungen entweder mithilfe des Plug-In-Modells oder durch Forken der Software und Schreiben Ihres eigenen integrierten Beobachters zu erfüllen. Ersteres (Plug-Ins) stellt den empfohlenen Ansatz dar.

## <a name="next-steps"></a>Nächste Schritte

* Einstieg in die Instrumentierung Ihrer Anwendungen: [Ereignis- und Protokollgenerierung auf Anwendungsebene](service-fabric-diagnostics-event-generation-app.md).
* Führen Sie die Schritte unter [Überwachen und Diagnostizieren einer ASP.NET Core-Anwendung in Service Fabric](service-fabric-tutorial-monitoring-aspnet.md) aus, um Application Insights für Ihre Anwendung einzurichten.
* Weitere Informationen zum Überwachen der Plattform und der Ereignisse, die von Service Fabric für Sie bereitgestellt werden: [Ereignis- und Protokollgenerierung auf Plattformebene](service-fabric-diagnostics-event-generation-infra.md).
* Konfigurieren der Integration von Azure Monitor-Protokolle in Service Fabric: [Einrichten von Azure Monitor-Protokolle für den Cluster](service-fabric-diagnostics-oms-setup.md)
* Erfahren Sie, wie Sie Azure Monitor-Protokolle für Überwachungscontainer einrichten: [Überwachung und Diagnose für Windows-Container in Azure Service Fabric](service-fabric-tutorial-monitoring-wincontainers.md)
* Beispiele für Diagnoseprobleme und die entsprechenden Lösungen mit Service Fabric finden Sie unter [Diagnostizieren häufiger Szenarien mit Service Fabric](service-fabric-diagnostics-common-scenarios.md)
* Informieren Sie sich über die Empfehlungen zur allgemeinen Überwachung für Azure-Ressourcen: [Bewährte Methoden: Überwachung und Diagnose](/azure/architecture/best-practices/monitoring).
