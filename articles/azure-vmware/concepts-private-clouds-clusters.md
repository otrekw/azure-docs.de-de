---
title: Konzepte – Private Clouds und Cluster
description: Hier erfahren Sie mehr über die wichtigsten Funktionen von softwaredefinierten Azure VMware Solution-Rechenzentren und vSphere-Clustern.
ms.topic: conceptual
ms.date: 03/13/2021
ms.openlocfilehash: aff66e01ae4b056eb082d2000611718b1a5cf66a
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104773918"
---
#  <a name="azure-vmware-solution-private-cloud-and-cluster-concepts"></a>Azure VMware Solution: Konzepte – Private Clouds und Cluster

Azure VMware Solution bietet VMware-basierte private Clouds in Azure. Private Clouds enthalten Cluster, die mit dedizierten Azure-Bare-Metal-Hosts erstellt werden. Sie werden über das Azure-Portal oder die CLI oder mithilfe von PowerShell bereitgestellt und verwaltet.  Cluster in privaten Clouds werden mit VMware vSphere-, vCenter-, vSAN- und NSX-Software bereitgestellt. Hardware- und Softwarebereitstellungen für private Azure VMware Solution-Clouds sind vollständig in Azure integriert und automatisiert.

Es besteht eine logische Beziehung zwischen Azure-Abonnements, privaten Azure VMware Solution-Clouds, vSAN-Clustern und Hosts. Die Abbildung zeigt ein einzelnes Azure-Abonnement mit zwei privaten Clouds, die für die Entwicklungs- und die Produktionsumgebung stehen.  In jeder dieser privaten Clouds gibt es zwei Cluster. 

Dieser Artikel enthält eine Beschreibung aller dieser Konzepte.

![Abbildung von zwei privaten Clouds in einem Kundenabonnement](./media/hosts-clusters-private-clouds-final.png)


## <a name="private-clouds"></a>Private Clouds

Private Clouds enthalten vSAN-Cluster, die mit dedizierten Azure-Bare-Metal-Hosts erstellt werden. Jede private Cloud kann über mehrere Cluster verfügen, die alle vom selben vCenter Server und NSX-T Manager verwaltet werden. Sie können private Clouds im Portal, über die CLI oder mit PowerShell bereitstellen und verwalten. 

Private Clouds werden wie andere Ressourcen innerhalb eines Azure-Abonnements installiert und verwaltet. Die Anzahl der privaten Clouds innerhalb eines Abonnements ist skalierbar. Anfänglich besteht eine Begrenzung von einer privaten Cloud pro Abonnement.

## <a name="clusters"></a>Cluster
Für jede erstellte private Cloud gibt es standardmäßig ein vSAN-Cluster. Sie können Cluster im Azure-Portal oder über die API hinzufügen, löschen und skalieren.  Alle Cluster verfügen über eine Standardgröße von drei Hosts und können auf bis zu 16 Hosts skaliert werden. Sie können pro privater Cloud bis zu vier Cluster verwenden.

Testcluster stehen zu Bewertungszwecken zur Verfügung und sind auf drei Hosts begrenzt. Pro privater Cloud gibt es einen einzelnen Testcluster. Sie können einen Testcluster während des Evaluierungszeitraums unter Verwendung eines einzelnen Hosts skalieren.

Sie verwenden vSphere und NSX-T Manager, um die meisten weiteren Aspekte der Clusterkonfiguration oder des Clusterbetriebs zu verwalten. Der gesamte lokale Speicher jedes Hosts in einem Cluster befindet sich unter der Kontrolle von vSAN.

## <a name="hosts"></a>Hosts

Azure VMware Solution-Cluster basieren auf einer hyperkonvergenten Bare-Metal-Infrastruktur. Die RAM-, CPU- und Datenträgerkapazitäten des Hosts sind in der folgenden Tabelle angegeben.

| Hosttyp              |             CPU             |   RAM (GB)   |  vSAN NVMe-Cacheebene (TB, Raw)  |  vSAN SSD-Kapazitätsebene (TB, Raw)  |
| :---                   |            :---:            |    :---:     |               :---:              |                :---:               |
| AVS36          |  Intel Dual Core, 18 Kerne 2,3 GHz  |     576      |                3.2               |                15,20               |

Hosts, die zum Erstellen oder Skalieren von Clustern verwendet werden, stammen aus einem isolierten Hostpool. Diese Hosts wurden Hardwaretests unterzogen. Darüber hinaus wurden alle Daten sicher gelöscht. 

## <a name="vmware-software-versions"></a>Versionen von VMware-Software

[!INCLUDE [vmware-software-versions](includes/vmware-software-versions.md)]

## <a name="update-frequency"></a>Aktualisierungshäufigkeit

[!INCLUDE [vmware-software-update-frequency](includes/vmware-software-update-frequency.md)]

## <a name="host-maintenance-and-lifecycle-management"></a>Hostwartung und Lebenszyklusverwaltung

Die Hostwartung und Lebenszyklusverwaltung haben keine Auswirkungen auf die Kapazität oder Leistung von Clustern in privaten Clouds.  Beispiele für die automatisierte Hostwartung sind Firmwareupgrades und Hardwarereparaturen oder der Austausch von Hardware.

Microsoft ist für die Lebenszyklusverwaltung von NSX-T-Appliances wie NSX-T Manager und NSX-T Edge verantwortlich. Microsoft ist für das Bootstrapping der Netzwerkkonfiguration zuständig, z. B. die Erstellung des Gateways des Schicht 0 und die Aktivierung des Nord-Süd-Routings. Sie sind für die NSX-T-DSN-Konfiguration verantwortlich. Dazu zählen beispielsweise Netzwerksegmente, Regeln für verteilte Firewalls, Tier-1-Gateways und Lastenausgleichsmodule.

## <a name="backup-and-restoration"></a>Sichern und Wiederherstellen

Die vCenter- und NSX-T-Konfigurationen der privaten Cloud werden stündlich gesichert.  Sicherungen werden drei Tage lang aufbewahrt. Wenn Sie eine Wiederherstellung auf Grundlage einer Sicherung durchführen möchten, öffnen Sie eine [Supportanfrage](https://rc.portal.azure.com/#create/Microsoft.Support) im Azure-Portal, um die Wiederherstellung anzufordern.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie jetzt die Konzepte der privaten Azure VMware Solution-Cloud kennengelernt haben, möchten Sie vielleicht mehr über Folgendes erfahren: 

- [Azure VMware Solution – Netzwerk- und Interkonnektivitätskonzepte](concepts-networking.md)
- [Speicherkonzepte von Azure VMware Solution](concepts-storage.md)
- [Aktivieren einer Azure VMware Solution-Ressource](enable-azure-vmware-solution.md)

<!-- LINKS - internal -->
[concepts-networking]: ./concepts-networking.md

<!-- LINKS - external-->
[VCSA versions]: https://kb.vmware.com/s/article/2143838
[ESXi versions]: https://kb.vmware.com/s/article/2143832
[vSAN versions]: https://kb.vmware.com/s/article/2150753

