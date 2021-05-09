---
title: Ethernetkonfiguration von Bare-Metal für Oracle
description: Hier finden Sie Informationen zur Konfiguration von Ethernetschnittstellen in Bare-Metal-Instanzen für Oracle-Workloads.
ms.topic: reference
ms.subservice: workloads
ms.date: 04/14/2021
ms.openlocfilehash: c57cbc86d17090d6960a334c2790d80b43420aca
ms.sourcegitcommit: 272351402a140422205ff50b59f80d3c6758f6f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2021
ms.locfileid: "107588886"
---
# <a name="ethernet-configuration-of-baremetal-for-oracle"></a>Ethernetkonfiguration von Bare-Metal für Oracle

Dieser Artikel enthält Informationen zur Konfiguration von Ethernetschnittstellen in Bare-Metal-Instanzen für Oracle-Workloads.

Jede bereitgestellte Bare-Metal-Instanz für Oracle ist mit Ethernetschnittstellen vorkonfiguriert. Die Ethernetschnittstellen lassen sich in vier Kategorien unterteilen:

- Wird für oder durch Clientzugriff verwendet.
- Wird für die Kommunikation zwischen Knoten verwendet. Diese Schnittstelle ist auf allen Servern konfiguriert (unabhängig von der angeforderten Topologie). Sie wird nur für Szenarien mit horizontaler Skalierung verwendet.
- Wird für die Konnektivität zwischen Knoten und Speicher verwendet.
- Wird für die Einrichtung der Notfallwiederherstellung (Disaster Recovery, DR) und der Global Reach-Konnektivität für regionsübergreifende Konnektivität verwendet.

## <a name="architecture"></a>Aufbau

Im folgenden Diagramm wird die Architektur der vorkonfigurierten Ethernetschnittstellen der Bare-Metal-Infrastruktur veranschaulicht: 

[![Diagramm: Architektur der vorkonfigurierten Ethernetschnittstellen für Oracle-Workloads](media/oracle-baremetal-ethernet/architecture-ethernet.png)](media/oracle-baremetal-ethernet/architecture-ethernet.png#lightbox)

Die Standardkonfiguration beinhaltet eine einzelne Client-IP-Schnittstelle (eth1) für die Verbindungsherstellung aus Ihrem virtuellen Azure-Netzwerk (VNET), um die Verwendung von Secure Shell (SSH) für den Zugriff auf eine Bare-Metal-Instanz zu ermöglichen.

> [!NOTE]
> Sollten Sie eine weitere Clientschnittstelle (eth10) für ein anderes Azure-VNET benötigen, wenden Sie sich an Ihren Microsoft-CSA, um eine entsprechende Serviceanfrage zu übermitteln. Dies kann beispielsweise erforderlich sein, wenn Sie sowohl Entwicklungs-/Testumgebungen als auch Produktions-/DR-Umgebungen benötigen.

| **Logische Netzwerkkarten-Schnittstelle** | **Name mit dem Betriebssystem RHEL** | **Anwendungsfall** |
| --- | --- | --- |
| A | net1.tenant | Client zu Bare-Metal-Instanz |
| C | net2.tenant | Knoten zu Speicher; unterstützt die Koordination und den Zugriff auf die Speichercontroller für die Verwaltung der Speicherumgebung. |
| B | net3.tenant | Knoten zu Knoten (private Verbindung) |
| C | net4.tenant | Reserviert/iSCSI |
| C | net5.tenant | Reserviert/Protokollsicherung |
| C | net6.tenant | Knoten zu Speicher: Datensicherung (RMAN, Momentaufnahme) |
| C | net7.tenant | Knoten zu Speicher (dNFS, primär); sorgt für Konnektivität mit dem NetApp-Speicherarray. |
| C | net8.tenant | Knoten zu Speicher (dNFS, sekundär); sorgt für Konnektivität mit dem NetApp-Speicherarray. |
| D | net9.tenant | DR-Konnektivität für die Einrichtung von Global Reach für den Zugriff auf BMI in einer anderen Region. |
| A | \*net10.tenant | \* Client zu Bare-Metal-Instanz
 |

Bei Bedarf können Sie selbst weitere NIC-Karten (Network Interface Controller) definieren. Die Konfigurationen vorhandener Netzwerkkarten *können jedoch nicht* geändert werden.

## <a name="usage-rules"></a>Nutzungsregeln

Für Bare-Metal-Instanzen stehen standardmäßig neun zugewiesene IP-Adressen auf den vier logischen NICs zur Verfügung. Folgende Nutzungsregeln gelten:

- Ethernet „A“ muss über eine zugewiesene IP-Adresse verfügen, die außerhalb des Adressbereichs des Server-IP-Pools liegt, den Sie an Microsoft übermittelt haben. Diese IP-Adresse darf nicht im Verzeichnis „etc/hosts“ des Betriebssystems verwaltet werden.
- Ethernet „B“ muss für die Kommunikation zwischen den verschiedenen Instanzen ausschließlich im Verzeichnis „etc/hosts“ verwaltet werden. Verwenden Sie diese IP-Adressen in Oracle RAC-Konfigurationen (Real Application Clusters) mit horizontaler Skalierung als die IP-Adressen für die knotenübergreifende Konfiguration.
- Ethernet „C“ muss über eine zugewiesene IP-Adresse verfügen, die für die Kommunikation mit NFS-Speicher verwendet wird. Diese Art von Adresse darf nicht im Verzeichnis „etc/hosts“ verwaltet werden.
- Ethernet „D“ darf ausschließlich für die Global Reach-Einrichtung verwendet werden, um den Zugriff auf Bare-Metal-Instanzen in Ihrer DR-Region zu ermöglichen.

## <a name="next-step"></a>Nächster Schritt

Erfahren Sie mehr über die Architektur der Bare-Metal-Infrastruktur für Oracle:

> [!div class="nextstepaction"]
> [Architektur der Bare-Metal-Infrastruktur für Oracle](oracle-baremetal-architecture.md)
