---
title: Konzepte – Private Clouds und Cluster
description: Erfahren Sie mehr über die wichtigsten Funktionen von softwaredefinierten Azure-VMware-Rechenzentren und vSphere-Clustern in Azure VMware Solution von VMware.
ms.topic: conceptual
ms.date: 05/04/2020
ms.openlocfilehash: f51b76b654a43d690aa5c97fa9df99fad1f47d7a
ms.sourcegitcommit: d9cd51c3a7ac46f256db575c1dfe1303b6460d04
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/04/2020
ms.locfileid: "82739790"
---
# <a name="azure-vmware-solution-avs-preview-private-cloud-and-cluster-concepts"></a>Azure VMware Solution (Vorschau): Konzepte – Private Clouds und Cluster

Azure VMware Solution (AVS) bietet VMware-basierte private Clouds in Azure. Die privaten Clouds werden aus Clustern dedizierter Bare-Metal-Hosts erstellt und über das Azure-Portal bereitgestellt und verwaltet. Cluster in privaten Clouds werden mit VMware vSphere-, vCenter-, vSAN- und NSX-Software bereitgestellt. Hardware- und Softwarebereitstellungen für private AVS-Clouds sind vollständig in Azure integriert und automatisiert.

Es besteht eine logische Beziehung zwischen Azure-Abonnements, privaten AVS-Clouds, vSAN-Clustern und Hosts. Im Diagramm sind zwei private Clouds in einem einzelnen Azure-Abonnement dargestellt. Private Clouds stellen eine Entwicklungs- und Produktionsumgebung dar, die jeweils über eine eigene private Cloud verfügen. In jeder dieser privaten Clouds gibt es zwei Cluster. Um die geringeren potenziellen Anforderungen einer Entwicklungsumgebung aufzuzeigen, werden kleinere Cluster und Hosts mit geringeren Kapazitäten verwendet. All diese Konzepte werden in den folgenden Abschnitten beschrieben.

![Abbildung von zwei privaten Clouds in einem Kundenabonnement](./media/hosts-clusters-private-clouds-final.png)

## <a name="private-clouds"></a>Private Clouds

Private Clouds enthalten vSAN-Cluster, die mit dedizierten Azure-Bare-Metal-Hosts erstellt werden. Jede private Cloud kann über mehrere Cluster verfügen, die alle vom selben vCenter Server und NSX-T Manager verwaltet werden. Sie können private Clouds im Portal, über die CLI oder mit PowerShell bereitstellen und verwalten. Private Clouds werden wie andere Ressourcen innerhalb eines Azure-Abonnements installiert und verwaltet.

Die Anzahl der privaten Clouds innerhalb eines Abonnements ist skalierbar. Anfänglich besteht eine Begrenzung von einer privaten Cloud pro Abonnement.

## <a name="clusters"></a>Cluster

In jeder privaten Cloud erstellen Sie mindestens einen vSAN-Cluster. Wenn Sie eine private Cloud erstellen, ist standardmäßig ein Cluster vorhanden. Sie können einer privaten Cloud im Azure-Portal oder über die API weitere Cluster hinzufügen. Alle Cluster verfügen über eine Standardgröße von drei Hosts und können von 3 bis 16 Hosts skaliert werden. Die in einem Cluster verwendeten Hosts müssen denselben Typ aufweisen. Die Hosttypen werden im nächsten Abschnitt beschrieben.

Zu Evaluierungszwecken sind Testcluster verfügbar, die auf drei Hosts und einen einzelnen Testcluster pro private Cloud beschränkt sind. Sie können einen Testcluster während des Evaluierungszeitraums unter Verwendung eines einzelnen Hosts skalieren.

Sie können Cluster über das Portal oder die API erstellen, löschen und skalieren. Sie verwenden weiterhin vSphere und NSX-T Manager, um die meisten weiteren Aspekte der Clusterkonfiguration oder des Clusterbetriebs zu verwalten. Der gesamte lokale Speicher jedes Hosts in einem Cluster befindet sich unter der Kontrolle von vSAN.

## <a name="hosts"></a>Hosts

In Clustern privater AVS-Clouds werden hyperkonvergente Bare-Metal-Infrastrukturknoten verwendet. Die RAM-, CPU- und Datenträgerkapazitäten des Hosts sind in der folgenden Tabelle angegeben. 

| Hosttyp              |             CPU             |   RAM (GB)   |  vSAN NVMe-Cacheebene (TB, Raw)  |  vSAN SSD-Kapazitätsebene (TB, Raw)  |
| :---                   |            :---:            |    :---:     |               :---:              |                :---:               |
| High-End (HE)          |  Intel Dual Core, 18 Kerne 2,3 GHz  |     576      |                3.2               |                15,20               |

Zum Erstellen oder Skalieren von Clustern verwendete Hosts werden aus einem isolierten Pool von Hosts abgerufen. Diese Hosts wurden Hardwaretests unterzogen. Darüber hinaus wurden alle Daten sicher von den Flashlaufwerken gelöscht. Wenn Sie einen Host aus einem Cluster entfernen, werden die internen Datenträger auf sichere Weise bereinigt und die Hosts im isolierten Pool von Hosts platziert. Wenn Sie einem Cluster einen Host hinzufügen, wird ein bereinigter Host aus dem isolierten Pool verwendet.

## <a name="vmware-software-versions"></a>Versionen von VMware-Software

Im Folgenden die aktuellen in Clustern privater AVS-Clouds verwendeten VMware-Softwareversionen:

| Software              |    Version   |
| :---                  |     :---:    |
| VCSA/vSphere/ESXi |    6.7 U2    | 
| ESXi                  |    6.7 U2    | 
| vSAN                  |    6.7 U2    |
| NSX-T                 |      2.5     |

Bei jedem neuen Cluster in einer privaten Cloud entspricht die Softwareversion derjenigen, die derzeit in der privaten Cloud ausgeführt wird. Für alle neuen private Clouds in einem Kundenabonnement wird die neueste Version des Softwarestapels installiert.

Die allgemeinen Upgraderichtlinien und -prozesse für die AVS-Plattformsoftware werden im Dokument zu Upgradekonzepten beschrieben.

## <a name="host-maintenance-and-lifecycle-management"></a>Hostwartung und Lebenszyklusverwaltung

Die Hostwartung und Lebenszyklusverwaltung haben keine Auswirkungen auf die Kapazität oder Leistung von Clustern in privaten Clouds. Beispiele für die automatisierte Hostwartung sind Firmwareupgrades und Hardwarereparaturen oder der Austausch von Hardware.

Microsoft ist für die Lebenszyklusverwaltung von NSX-T-Appliances wie NSX-T Manager und NSX-T Edges verantwortlich. Microsoft ist auch für das Bootstrapping der Netzwerkkonfiguration zuständig, wie die Erstellung des Tier-0-Gateways und die Aktivierung des Nord-Süd-Routings. Als Administrator der privaten AVS-Cloud sind Sie für die NSX-T-SDN-Konfiguration verantwortlich, z. B. Netzwerksegmente, Regeln für verteilte Firewalls, Tier-1-Gateways und Lastenausgleiche.

> [!IMPORTANT]
> Die Konfiguration von NSX-T Edges oder Tier-0-Gateways darf von einem AVS-Administrator nicht geändert werden. Dies kann zu einer Beeinträchtigung des Diensts führen.

## <a name="backup-and-restoration"></a>Sichern und Wiederherstellen

Die vCenter- und NSX-T-Konfigurationen der privaten Cloud werden stündlich gesichert. Sicherungen werden drei Tage lang aufbewahrt. Die Wiederherstellung aus einer Sicherung wird durch eine Serviceanfrage im Azure-Portal angefordert.

## <a name="next-steps"></a>Nächste Schritte

Im nächsten Schritt erfahren Sie mehr über [Netzwerk- und Interkonnektivitätskonzepte](concepts-networking.md).

<!-- LINKS - internal -->

<!-- LINKS - external-->
[VCSA versions]: https://kb.vmware.com/s/article/2143838
[ESXi versions]: https://kb.vmware.com/s/article/2143832
[vSAN versions]: https://kb.vmware.com/s/article/2150753

