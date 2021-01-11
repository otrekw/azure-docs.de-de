---
title: Übersicht über die Vorschauversion von BareMetal Infrastructure in Azure
description: Erfahren Sie, wie Sie BareMetal Infrastructure in Azure bereitstellen.
ms.custom: references_regions
ms.topic: conceptual
ms.date: 12/31/2020
ms.openlocfilehash: db974d9260344d1f6050235bb2a9fbaa0420659b
ms.sourcegitcommit: 42922af070f7edf3639a79b1a60565d90bb801c0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/31/2020
ms.locfileid: "97829055"
---
#  <a name="what-is-baremetal-infrastructure-preview-on-azure"></a>Was ist BareMetal Infrastructure (Vorschau) in Azure?

Azure BareMetal Infrastructure bietet eine sichere Lösung für das Migrieren von benutzerdefinierten Unternehmensworkloads. Die BareMetal-Instanzen bestehen aus nicht freigegebener Host-/Serverhardware, die Ihnen zugewiesen ist. Damit können Sie Ihre lokale Lösung mit spezialisierten Workloads portieren, die zertifizierte Hardware, Lizenzierung und Supportvereinbarungen erfordern. Azure übernimmt die Überwachung und Wartung der Infrastruktur für Sie, während die Überwachung von Gastbetriebssystem und Anwendungen Ihrer Verantwortung unterliegt.

BareMetal Infrastructure bietet einen Weg, Ihre Infrastrukturumgebung zu modernisieren und gleichzeitig bestehende Investitionen und Architektur weiter zu nutzen. Mit BareMetal Infrastructure können Sie spezialisierte Workloads zu Azure übertragen, sodass Sie mit geringer Latenz auf Azure-Dienste zugreifen und diese integrieren können.

## <a name="sku-availability-in-azure-regions"></a>SKU-Verfügbarkeit nach Azure-Regionen
BareMetal Infrastructure ist für spezialisierte und allgemeine Workloads zunächst in vier Regionen mit Stempeln von Revision 4.2 (Rev 4.2) verfügbar:
- Europa, Westen
- Nordeuropa
- USA (Ost) 2
- USA Süd Mitte

>[!NOTE]
>**Rev 4.2** ist die neueste BareMetal Infrastructure-Revision mit neuem Branding, die die vorhandene Architektur von Rev 4 verwendet.  Rev 4 bietet eine größere Nähe zu den Hosts von Azure-VMs (Virtual Machines, virtuelle Computer) und verringert damit die Latenz zwischen Azure-VMs und BareMetal-Instanzeinheiten. Sie können über das Azure-Portal auf Ihre BareMetal-Instanzen zugreifen und diese verwalten. 

## <a name="support"></a>Support
BareMetal Infrastructure ist mit ISO 27001, ISO 27017, SOC 1 und SOC 2 konform.  Außerdem wird das BYOL-Modell (Bring-Your-Own-License, Verwendung Ihrer eigenen Lizenz) für Betriebssystem, spezialisierte Workloads und Anwendungen von Drittanbietern unterstützt.  

Wenn Sie Root-Zugriff und vollständige Kontrolle erhalten, sind Sie für Folgendes verantwortlich:
- Entwerfen und Implementieren von Sicherungs- und Wiederherstellungslösungen, Hochverfügbarkeit und Notfallwiederherstellung
- Lizenzierung, Sicherheit und Unterstützung für Betriebssystem und Drittanbietersoftware

Microsoft ist verantwortlich für:
- Bereitstellen zertifizierter Hardware für spezialisierte Workloads 
- Bereitstellen des Betriebssystems

:::image type="content" source="media/baremetal-support-model.png" alt-text="Supportmodell für BareMetal Infrastructure" border="false":::

## <a name="compute"></a>Compute
BareMetal Infrastructure bietet verschiedene SKUs, die für spezialisierte Workloads zertifiziert sind. Die verfügbaren SKUs reichen vom kleineren System mit zwei Sockets bis zum System mit 24 Sockets. Verwenden Sie die für die jeweilige Workload zertifizierten SKUs für Ihre spezialisierte Workload.

Im BareMetal-Instanzstempel selbst sind die folgenden Komponenten kombiniert:

- **Computing:** Server, die auf verschiedenen Generationen von Intel Xeon-Prozessoren basieren, die die erforderliche Rechenleistung bereitstellen und für die spezialisierte Workload zertifiziert sind

- **Netzwerk**: Ein einheitliches Hochgeschwindigkeits-Netzwerkfabric, das die Computing-, Speicher- und LAN-Komponenten miteinander verbindet

- **Speicher:** Eine Infrastruktur, auf die über ein einheitliches Netzwerkfabric zugegriffen wird

Innerhalb der mehrinstanzenfähigen Infrastruktur des BareMetal-Stempels werden Kunden als isolierte Mandanten bereitgestellt. Bei der Bereitstellung des Mandanten benennen Sie ein Azure-Abonnement in Ihrer Azure-Registrierung. An dieses Azure-Abonnement erfolgt die Abrechnung der BareMetal-Instanzen.

>[!NOTE]
>Ein in der BareMetal-Instanz bereitgestellter Kunde wird in einem Mandanten isoliert. Ein Mandant wird in der Schicht der Netzwerke, Speicher und von Compute von anderen Mandanten isoliert. Speicher- und Compute-Einheiten, die unterschiedlichen Mandanten zugewiesen wurden, können einander in den BareMetal-Instanzen nicht sehen oder miteinander kommunizieren.

## <a name="os"></a>OS
Während der Bereitstellung der BareMetal-Instanz können Sie das Betriebssystem auswählen, das auf den Computern installiert werden soll. 

>[!NOTE]
>Beachten Sie dabei, dass BareMetal Infrastructure ein BYOL-Modell nutzt.

Folgende Linux-Betriebssystemversionen sind verfügbar:
- Red Hat Enterprise Linux (RHEL) 7.6
- SUSE Linux Enterprise Server (SLES)
   - SLES 12 SP2
   - SLES 12 SP3
   - SLES 12 SP4
   - SLES 12 SP5
   - SLES 15 SP1

## <a name="storage"></a>Storage
Auf einem bestimmten SKU-Typ basierende BareMetal-Instanzen verfügen über einen vordefinierten NFS-Speicher, der auf einem bestimmten Workloadtyp basiert. Wenn Sie BareMetal bereitstellen, können Sie basierend auf dem geschätzten Wachstum zusätzlichen Speicher bereitstellen, indem Sie eine Supportanfrage übermitteln. Der gesamte Speicher wird in Revision 4.2 als Flashspeicher mit Unterstützung für NFSv3 und NFSv4 bereitgestellt. Bei der neueren Revision 4.5 sind auch NVMe-SSDs verfügbar. Weitere Informationen zu Speichergrößen finden Sie im Abschnitt zum [BareMetal-Workloadtyp](../../../virtual-machines/workloads/sap/get-started.md).

>[!NOTE]
>Der für BareMetal verwendete Speicher entspricht den Sicherheitsanforderungen von FIPS 140-2 und bietet standardmäßig die Verschlüsselung ruhender Daten. Die Daten werden sicher auf den Datenträgern gespeichert.

## <a name="networking"></a>Netzwerk
Die Architektur von Azure-Netzwerkdiensten ist ein wichtiger Aspekt einer erfolgreichen Bereitstellung spezialisierter Workloads auf BareMetal-Instanzen. Die Wahrscheinlichkeit ist hoch, dass nicht alle IT-Systeme bereits in Azure zu finden sind. Azure verfügt über Netzwerktechnologie, damit Azure für Ihre lokalen Softwarebereitstellungen wie ein virtuelles Rechenzentrum aussieht. Für BareMetal-Instanzen sind die folgenden Azure-Netzwerkfunktionen erforderlich:

- Virtuelle Azure-Netzwerke werden mit der ExpressRoute-Leitung verbunden, die als Verbindung mit Ihren lokalen Netzwerkressourcen dient.
- Eine ExpressRoute-Leitung, die lokale Systeme mit Azure verbindet, sollte über eine Mindestbandbreite von 1 GBit/s verfügen.
- Active Directory und DNS sind auf Azure ausgedehnt oder werden vollständig in Azure ausgeführt.

Mit ExpressRoute können Sie Ihr lokales Netzwerk über eine private Verbindung, die von einem Konnektivitätsanbieter bereitgestellt wird, auf die Cloud von Microsoft ausdehnen. Sie können **ExpressRoute Premium** aktivieren, um die Konnektivität über geopolitische Grenzen hinweg zu erweitern, oder Sie verwenden **ExpressRoute Local** für kostengünstige Datenübertragungen von Ihrem Standort in der Nähe der gewünschten Azure-Region.

BareMetal-Instanzen werden im IP-Adressbereich Ihres Azure-VNET-Servers bereitgestellt.

:::image type="content" source="media/baremetal-infrastructure-portal/baremetal-infrastructure-diagram.png" alt-text="Diagramm von Azure BareMetal Infrastructure" lightbox="media/baremetal-infrastructure-portal/baremetal-infrastructure-diagram.png" border="false":::

Die dargestellte Architektur ist in drei Abschnitte unterteilt:
- **Links:** Zeigt die lokale Infrastruktur des Kunden, in der verschiedene Anwendungen ausgeführt werden. Die Verbindung erfolgt über den Partner oder einen lokalen Edgerouter wie Equinix. Weitere Informationen finden Sie unter [Konnektivitätsanbieter und Standorte: Azure ExpressRoute](../../../expressroute/expressroute-locations.md).
- **Mitte:** Zeigt [ExpressRoute](../../../expressroute/expressroute-introduction.md), das mithilfe Ihres Azure-Abonnements bereitgestellt wurde und Konnektivität mit dem Azure-Edgenetzwerk bereitstellt.
- **Rechts:** Zeigt Azure IaaS und in diesem Fall die Verwendung von VMs zum Hosten Ihrer Anwendungen, die in Ihrem virtuellen Azure-Netzwerk bereitgestellt werden.
- **Unten:** Zeigt die Verwendung des ExpressRoute-Gateways, auf dem [ExpressRoute FastPath](../../../expressroute/about-fastpath.md) aktiviert wurde, sodass es für die BareMetal-Konnektivität eine sehr niedrige Latenz ermöglicht.   
   >[!TIP]
   >Um dies zu unterstützen, sollte das ExpressRoute-Gateway UltraPerformance aufweisen.  Weitere Informationen finden Sie unter [Informationen zu ExpressRoute-Gateways für virtuelle Netzwerke](../../../expressroute/expressroute-about-virtual-network-gateways.md).

## <a name="next-steps"></a>Nächste Schritte

Im nächsten Schritt erfahren Sie, wie Sie BareMetal-Instanzeinheiten über das Azure-Portal identifizieren und mit ihnen interagieren.

> [!div class="nextstepaction"]
> [Verwalten von BareMetal-Instanzen über das Azure-Portal](baremetal-infrastructure-portal.md)