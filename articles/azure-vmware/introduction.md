---
title: Einführung
description: Hier finden Sie Informationen zu den Features und Vorteilen von Azure VMware Solution für die Bereitstellung und Verwaltung VMware-basierter Workloads in Azure.
ms.topic: overview
ms.date: 05/04/2020
ms.openlocfilehash: 3d6801a6cdec7600cc6003711a08e6af8c86483e
ms.sourcegitcommit: 419cf179f9597936378ed5098ef77437dbf16295
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2020
ms.locfileid: "89005053"
---
# <a name="what-is-azure-vmware-solution-preview"></a>Was ist Azure VMware Solution (Vorschauversion)?

Von Azure VMware Solution werden private Clouds in Azure bereitgestellt. Die privaten Clouds enthalten vSphere-Cluster, die auf dedizierter Bare-Metal-Azure-Infrastruktur basieren. Private Cloudcluster können von mindestens drei Hosts auf maximal 16 Hosts skaliert werden, und eine einzelne private Cloud kann mehrere Cluster enthalten. Alle privaten Clouds werden mit vCenter Server, vSAN, vSphere und NSX-T bereitgestellt. Sie können Workloads aus Ihren lokalen Umgebungen migrieren, neue virtuelle Computer erstellen oder bereitstellen und Azure-Dienste von Ihren privaten Clouds aus nutzen.

Bei Azure VMware Solution handelt es sich um eine von VMware geprüfte Lösung, deren Erweiterungen und Upgrades kontinuierlich geprüft und getestet werden. Die private Cloudinfrastruktur und die Software werden von Microsoft verwaltet und gepflegt, sodass Sie sich ganz auf die Entwicklung und Ausführung von Workloads in Ihren privaten Clouds konzentrieren können.

Das folgende Diagramm zeigt die Adjazenz zwischen privaten Clouds und VNETs in Azure, Azure-Diensten und lokalen Umgebungen. Der Netzwerkzugriff von privaten Clouds auf Azure-Dienste oder VNETs ermöglicht eine SLA-basierte Integration von Azure-Dienstendpunkten. Für den Zugriff privater Clouds aus lokalen Umgebungen wird ExpressRoute Global Reach verwendet, um eine private und sichere Verbindung herzustellen.

![Abbildung: Adjazenz zwischen privater Azure VMware Solution-Cloud, Azure und lokaler Umgebung](./media/adjacency-overview-drawing-final.png)

## <a name="hosts-clusters-and-private-clouds"></a>Hosts, Cluster und private Clouds

Private Azure VMware Solution-Clouds und Cluster basieren auf einem hyperkonvergenten Bare-Metal-Azure-Infrastrukturhost. Die High-End-Hosts verfügen über 576 GB RAM sowie über duale Intel-Prozessoren mit 18 Kernen und 2,3 GHz. Die HE-Hosts sind mit zwei vSAN-Datenträgergruppen mit einer vSAN-Rohkapazitätsebene von insgesamt 15,36 TB (SSD) und einer vSAN-Cacheebene mit 3,2 TB (NVMe) ausgestattet.

Neue private Clouds werden über das Azure-Portal oder mithilfe der Azure-Befehlszeilenschnittstelle bereitgestellt.

## <a name="networking"></a>Netzwerk

Wenn eine private Cloud bereitgestellt wird, werden private Netzwerke für Verwaltung, Bereitstellung und vMotion erstellt. Diese privaten Netzwerke werden für den Zugriff auf vCenter und NSX-T Manager sowie für vMotion oder die Bereitstellung virtueller Computer verwendet. Auf die privaten Netzwerke kann über ein VNET in Azure oder über lokale Umgebungen zugegriffen werden. ExpressRoute Global Reach wird verwendet, um private Clouds mit lokalen Umgebungen zu verbinden. Für diese Verbindung ist ein VNET mit einer ExpressRoute-Leitung in Ihrem Abonnement erforderlich.

Internetzugriff und Azure-Dienste werden beim Bereitstellen einer privaten Cloud bereitgestellt. Dadurch können virtuelle Computer in Netzwerken mit Produktionsworkloads Azure- oder internetbasierte Dienste nutzen. Der Internetzugriff ist für neue private Clouds standardmäßig deaktiviert und kann jederzeit aktiviert oder deaktiviert werden.

Weitere Informationen zu Netzwerken und zur Interkonnektivität finden Sie im [Artikel zu Netzwerkkonzepten](concepts-networking.md).

## <a name="access-and-security"></a>Zugriff und Sicherheit

Zur Verbesserung der Sicherheit wird von privaten Azure VMware Solution-Clouds die rollenbasierte Zugriffssteuerung von vSphere verwendet. vSphere-SSO-LDAP-Funktionen können in Azure Active Directory integriert werden. Weitere Informationen zu Identitäten und Berechtigungen finden Sie im [Artikel zu Zugriffs- und Identitätskonzepten](concepts-identity.md).

Die vSAN-Verschlüsselung ruhender Daten ist standardmäßig aktiviert und dient zum Schutz des vSAN-Datenspeichers. Ausführlichere Informationen finden Sie im [Artikel zu Speicherkonzepten](concepts-storage.md).

## <a name="host-and-software-lifecycle-maintenance"></a>Verwaltung des Host- und Softwarelebenszyklus

Durch regelmäßige Upgrades der privaten Azure VMware Solution-Cloud und der VMware-Software wird gewährleistet, dass die Sicherheit, Stabilität und Features Ihrer privaten Clouds immer auf dem neuesten Stand sind. Weitere Informationen zur Plattformwartung und zu Upgrades finden Sie im [Artikel zu Upgradekonzepten](concepts-upgrades.md).

## <a name="monitoring-your-private-cloud"></a>Überwachen Ihrer privaten Cloud

Sie können [Protokolle in Azure Monitor](../azure-monitor/overview.md) verwenden, um Protokolle auf Ihren virtuellen Computern zu sammeln, die in Ihrer privaten Azure VMware Solution-Cloud ausgeführt werden. Sie können den [MMA-Agent auf virtuellen Linux- und Windows-Computern, die in Ihren privaten Azure VMware Solution-Clouds ausgeführt werden, herunterladen und installieren](../azure-monitor/platform/log-analytics-agent.md#installation-options) und die gleichen Abfragen verwenden, die Sie auch auf Ihren lokalen virtuellen Computern ausführen. Nichtsdestotrotz können Sie die gleichen Abfragen ausführen, die Sie normalerweise auf Ihren virtuellen Computern ausführen würden. Weitere Informationen zum Erstellen von Abfragen finden Sie unter [Wie erfahre ich, wie ich Abfragen schreiben kann?](../azure-monitor/log-query/log-query-overview.md#how-can-i-learn-how-to-write-queries).

## <a name="next-steps"></a>Nächste Schritte

Machen Sie sich als Nächstes mit zentralen [Konzepten privater Clouds und Cluster](concepts-private-clouds-clusters.md) vertraut.

<!-- LINKS - external -->

<!-- LINKS - internal -->
[concepts-private-clouds-clusters]: ./concepts-private-clouds-clusters.md
