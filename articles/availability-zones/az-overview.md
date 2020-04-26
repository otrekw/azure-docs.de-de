---
title: Was sind Azure-Verfügbarkeitszonen?
description: Zur Erstellung hoch verfügbarer und robuster Anwendungen in Azure bieten Verfügbarkeitszonen physisch voneinander getrennte Orte für die Ressourcenausführung.
author: cynthn
ms.service: azure
ms.topic: article
ms.date: 10/17/2019
ms.author: cynthn
ms.custom: fasttrack-edit, mvc
ms.openlocfilehash: e7ba62a96a6fc76ad63960cfe57f5b1f85589c32
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2020
ms.locfileid: "82085041"
---
# <a name="what-are-availability-zones-in-azure"></a>Was sind Verfügbarkeitszonen in Azure?
Verfügbarkeitszonen sind ein Hochverfügbarkeitsangebot, das Anwendungen und Daten vor Ausfällen von Rechenzentren schützt. Verfügbarkeitszonen sind eindeutige physische Standorte in einer Azure-Region. Jede Zone besteht aus mindestens einem Rechenzentrum, dessen Stromversorgung, Kühlung und Netzwerkbetrieb unabhängig funktionieren. Zur Gewährleistung der Resilienz sind in allen aktivierten Regionen mindestens drei separate Zonen vorhanden. Die physische Trennung von Verfügbarkeitszonen innerhalb einer Region schützt Anwendungen und Daten vor Ausfällen von Rechenzentren. Zonenredundante Dienste replizieren Ihre Anwendungen und Daten zum Schutz vor einzelnen Fehlerquellen über Verfügbarkeitszonen hinweg. Mit Verfügbarkeitszonen bietet Azure die branchenweit beste Betriebszeit-SLA von 99,99 % für VMs. Die vollständige [Azure-SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) erläutert die garantierte Verfügbarkeit von Azure insgesamt.

Eine Verfügbarkeitszone in einer Azure-Region ist eine Kombination aus einer Fehlerdomäne und einer Updatedomäne. Wenn Sie z.B. drei oder mehr virtuelle Computer über drei Zonen verteilt in einer Azure-Region erstellen, werden Ihre virtuellen Computer effektiv auf drei Fehlerdomänen und drei Updatedomänen verteilt. Die Azure-Plattform erkennt diese Updatedomänen übergreifende Verteilung, um sicherzustellen, dass virtuelle Computer in unterschiedlichen Zonen nicht gleichzeitig aktualisiert werden.

Integrieren Sie hohe Verfügbarkeit in Ihre Anwendungsarchitektur, indem Sie Ihre Compute-, Speicher-, Netzwerk- und Datenressourcen in eine Zone aufnehmen und in anderen Zonen replizieren. Azure-Dienste, die Verfügbarkeitszonen unterstützen, können in zwei Kategorien unterteilt werden:

- **Zonendienste**: Sie binden die Ressource in eine bestimmte Zone ein (z. B. virtuelle Computer, verwaltete Datenträger, Standard-IP-Adressen).
- **Zonenredundante Dienste**: Die Plattform repliziert automatisch zonenübergreifend (z.B. zonenredundanter Speicher, SQL-Datenbank).

Um eine umfassende Geschäftskontinuität in Azure zu erreichen, erstellen Sie die Anwendungsarchitektur über die Kombination von Verfügbarkeitszonen mit Azure-Regionspaaren. Sie können Anwendungen und Daten mithilfe von Verfügbarkeitszonen für hohe Verfügbarkeit innerhalb einer Azure-Region synchron replizieren und für Notfallwiederherstellungen über Azure-Regionen asynchron replizieren.
 
![Konzeptionelle Ansicht einer Zone, die in einer Region ausfällt](./media/az-overview/az-graphic-two.png)

> [!IMPORTANT]
> Die Verfügbarkeitszonenbezeichner (die Zahlen 1, 2 und 3 in der vorherigen Abbildung) werden den tatsächlichen physischen Zonen der einzelnen Abonnements unabhängig logisch zugeordnet. Dies bedeutet, dass die Verfügbarkeitszone 1 in einem bestimmten Abonnement möglicherweise auf eine andere physische Zone als die Verfügbarkeitszone 1 in einem anderen Abonnement verweist. Folglich empfiehlt es sich, in Verfügbarkeitszonen-IDs nicht über verschiedene Abonnements hinweg bei der Platzierung virtueller Computer ein Relay durchzuführen.

## <a name="services-support-by-region"></a>Unterstützung der Dienste nach Region

Die folgenden Kombinationen von Azure-Diensten und -Regionen unterstützen Verfügbarkeitszonen:


|                                 |Amerika |              |           |           | Europa |              |          |              | Asien-Pazifik |                 |                |
|----------------------------|----------|----------|---------|---------|--------------|------------|--------|----------|----------|-------------|-------------|
|          |USA (Mitte)|East US|USA (Ost) 2|USA, Westen 2|Frankreich, Mitte|Nordeuropa|UK, Süden|Europa, Westen|Japan, Osten|Asien, Südosten|Australien (Osten)|
| **Compute**                         |            |              |           |           |                |              |          |             |            |                |                |
| Virtuelle Linux-Computer          | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |                |
| Virtuelle Windows-Computer        | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |                |
| Virtual Machine Scale Sets      | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |                |
| Azure App Service-Umgebungen ILB | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |                |
| Azure Kubernetes Service        | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |                |
| **Storage**   |            |              |           |           |                |              |          |             |            |                |                |
| Managed Disks                   | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |                |
| Zonenredundanter Speicher          | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |                |
| **Netzwerk**                     |            |              |           |           |                |              |          |             |            |                |                |
| Standard-IP-Adresse        | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |                |
| Load Balancer Standard     | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |                |
| VPN Gateway            | &#10003;   |  &#10003;    | &#10003;  | &#10003;  | &#10003;       | &#10003;     |  &#10003;  | &#10003;    |  &#10003;   | &#10003;       |                |
| ExpressRoute-Gateway   | &#10003;   |  &#10003;    | &#10003;  | &#10003;  | &#10003;       | &#10003;     |  &#10003;  | &#10003;    |  &#10003;   | &#10003;       |                |
| Application Gateway(V2)    | &#10003;   |  &#10003;    | &#10003;  | &#10003;  | &#10003;       | &#10003;     |  &#10003;  | &#10003;    |  &#10003;   | &#10003;       |                |
| Azure Firewall           | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    |  &#10003;       | &#10003;       |                |
| **Datenbanken**                     |            |              |           |           |                |              |          |             |            |                |                |
| Azure-Daten-Explorer                   | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;        | &#10003;       |                |
| SQL-Datenbank                    | &#10003;   | &#10003;     | &#10003;  | &#10003;(Vorschau) | &#10003;       | &#10003;(Vorschau)     | &#10003; | &#10003;    | &#10003;       | &#10003;       |&#10003;        |
| Azure Cache for Redis           | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    |  &#10003;       | &#10003;       |                |
| Azure Cosmos DB                    | &#10003;   |  &#10003;  |  &#10003; | &#10003; |       |     | &#10003; |  &#10003;   |            | &#10003;       |                |
| **Analyse**                       |            |              |           |           |                |              |          |             |            |                |                |
| Event Hubs                      | &#10003;   |   &#10003; | &#10003;  | &#10003;  | &#10003; | &#10003; | &#10003; | &#10003; | &#10003; | &#10003;       |                |
| **Integration**                     |            |              |           |           |                |              |          |             |            |                |                |
| Service Bus (nur Premium-Tarif) | &#10003;   |  &#10003;  | &#10003;  | &#10003;  | &#10003;  | &#10003;     |&#10003;   | &#10003;    |&#10003;      | &#10003;       |                |
| Event Grid | &#10003;   |  &#10003;  | &#10003;  | &#10003;  | &#10003;  | &#10003;     |&#10003;   | &#10003;    |&#10003;      | &#10003;       |                |
| **Identität**                     |            |              |           |           |                |              |          |             |            |                |                |
| Azure AD Domain Services | &#10003;   |  &#10003;  | &#10003;  | &#10003;  | &#10003;  | &#10003;     |&#10003;   | &#10003;    |&#10003;      | &#10003;       |                |

## <a name="services-resiliency"></a>Resilienz von Diensten
Alle Azure-Verwaltungsdienste sind so konstruiert, dass Sie gegenüber regionsweiten Ausfällen resilient sind. Im Spektrum möglicher Ausfälle besitzen ein oder mehrere Verfügbarkeitszonenausfälle innerhalb einer Region einen geringeren Ausfallradius als der Ausfall einer ganzen Region. Azure kann von einem zonenweiten Ausfall von Verwaltungsdiensten innerhalb der Region oder aus einer anderen Azure-Region wiederhergestellt werden. Azure führt wichtige Wartungsaufgaben immer zonenweise innerhalb einer Region durch, um Ausfälle zu verhindern, die sich auf Kundenressourcen auswirken, die über Verfügbarkeitszonen innerhalb einer Region hinweg bereitgestellt sind.

## <a name="pricing"></a>Preise
Es fallen keine zusätzlichen Kosten für virtuelle Computer an, die in einer Verfügbarkeitszone bereitgestellt werden. Die Betriebszeit-SLA von 99,99% für VMs wird angeboten, wenn mindestens zwei virtuelle Computer in zwei oder mehr Verfügbarkeitszonen innerhalb einer Azure-Region bereitgestellt werden. Es fallen zusätzliche Gebühren für Datenübertragungen zwischen VMs in unterschiedlichen Verfügbarkeitszonen an. Weitere Informationen finden Sie auf der Seite [Bandbreite – Preisdetails](https://azure.microsoft.com/pricing/details/bandwidth/).


## <a name="get-started-with-availability-zones"></a>Erste Schritte mit Verfügbarkeitszonen
- [Erstellen eines virtuellen Computers](../virtual-machines/windows/create-portal-availability-zone.md)
- [Anfügen eines Datenträgers an einen virtuellen Windows-Computer mithilfe von PowerShell](../virtual-machines/windows/attach-disk-ps.md#add-an-empty-data-disk-to-a-virtual-machine)
- [Erstellen einer zonenredundanten VM-Skalierungsgruppe](../virtual-machine-scale-sets/virtual-machine-scale-sets-use-availability-zones.md)
- [Zonenübergreifender Lastenausgleich für virtuelle Computer mithilfe eines Standardlastenausgleichs mit einem zonenredundanten Front-End](../load-balancer/load-balancer-standard-public-zone-redundant-cli.md)
- [Lastenausgleich für virtuelle Computer innerhalb einer Zone mithilfe eines Standardlastenausgleichs mit einem zonalen Front-End](../load-balancer/load-balancer-standard-public-zonal-cli.md)
- [Zonenredundanter Speicher](../storage/common/storage-redundancy-zrs.md)
- [SQL-Datenbank](../sql-database/sql-database-high-availability.md#zone-redundant-configuration)
- [Georedundante Notfallwiederherstellung in Event Hubs](../event-hubs/event-hubs-geo-dr.md#availability-zones)
- [Georedundante Notfallwiederherstellung in Service Bus](../service-bus-messaging/service-bus-geo-dr.md#availability-zones)
- [Erstellen eines zonenredundanten Gateways für virtuelle Netzwerke](../vpn-gateway/create-zone-redundant-vnet-gateway.md)
- [Hinzufügen einer zonenredundanten Region für Azure Cosmos DB](../cosmos-db/high-availability.md#availability-zone-support)
- [Erste Schritte mit Azure Cache für Redis-Verfügbarkeitszonen](https://aka.ms/redis/az/getstarted)
- [Erstellen einer Azure Active Directory Domain Services-Instanz](../active-directory-domain-services/tutorial-create-instance.md)
- [Erstellen eines Azure Kubernetes Service-Clusters (AKS), der Verfügbarkeitszonen verwendet](../aks/availability-zones.md)

## <a name="next-steps"></a>Nächste Schritte
- [Schnellstartvorlagen](https://aka.ms/azqs)
