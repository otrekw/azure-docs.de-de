---
title: Bewährte Methoden für Azure Service Fabric-Anwendungen und -Cluster
description: Hier finden Sie bewährte Methoden und Entwurfsüberlegungen zur Verwaltung von Clustern, Apps und Diensten mit Azure Service Fabric.
author: peterpogorski
ms.topic: conceptual
ms.date: 06/18/2019
ms.author: pepogors
ms.openlocfilehash: 46601fd91fccb5bd0866bd999cc7643c37c6d582
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "75551776"
---
# <a name="azure-service-fabric-application-and-cluster-best-practices"></a>Bewährte Methoden für Azure Service Fabric-Anwendungen und -Cluster

In diesem Artikel finden Sie Links zu Best Practices für die Verwaltung von Azure Service Fabric-Anwendungen und -Clustern. Es wird dringend empfohlen, diese Best Practices umzusetzen, um die Zuverlässigkeit der Produktionsumgebung zu optimieren. Nutzen Sie für den Entwurf Ihrer Produktionslösung entweder eine der [Service Fabric-Clustervorlagen](https://github.com/Azure-Samples/service-fabric-cluster-templates), oder ergänzen Sie eine bestehende Vorlage um diese Methoden.

## <a name="security"></a>Sicherheit

* [Bewährte Methoden für die Sicherheit](service-fabric-best-practices-security.md)

## <a name="networking"></a>Netzwerk

* [Bewährte Methoden für Netzwerke](service-fabric-best-practices-networking.md)

## <a name="compute-planning-and-scaling"></a>Computeplanung und -skalierung

* [Bewährte Methoden für die Skalierung von Computeressourcen](service-fabric-best-practices-capacity-scaling.md)
* [Planen der Computekapazität](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity)

## <a name="infrastructure-as-code"></a>Infrastructure-as-Code

* [Bewährte Methoden für die Implementierung von Infrastructure-as-Code](service-fabric-best-practices-infrastructure-as-code.md)

## <a name="monitoring-and-diagnostics"></a>Überwachung und Diagnose

* [Bewährte Methoden für die Überwachung von Clustern und die Diagnose](service-fabric-best-practices-monitoring.md)

## <a name="application-design"></a>Anwendungsentwurf

* [Bewährte Methoden für en Anwendungsentwurf](service-fabric-best-practices-applications.md)

## <a name="checklist"></a>Checkliste

Setzen Sie nach der Implementierung der oben aufgeführten Methoden alle Best Practices in der Checkliste für die Produktionsbereitschaft um:
* [Azure Service Fabric: Checkliste für die Produktionsbereitschaft](https://docs.microsoft.com/azure/service-fabric/service-fabric-production-readiness-checklist)

## <a name="next-steps"></a>Nächste Schritte

* Erstellen eines Clusters auf virtuellen Computern oder Computern mit Windows Server: [Erstellen eines Service Fabric-Clusters für Windows Server](service-fabric-cluster-creation-for-windows-server.md)
* Erstellen eines Clusters auf virtuellen Computern oder Computern mit Linux: [Erstellen eines Linux-Clusters](service-fabric-cluster-creation-via-portal.md)
* Problembehandlung für Service Fabric: [Leitfäden zur Problembehandlung](https://github.com/Azure/Service-Fabric-Troubleshooting-Guides)