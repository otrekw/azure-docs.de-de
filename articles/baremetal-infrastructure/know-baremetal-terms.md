---
title: Wichtige Begriffe in der Azure BareMetal-Infrastruktur
description: Machen Sie sich mit wichtigen Begriffen in der Azure BareMetal-Infrastruktur vertraut.
ms.topic: conceptual
ms.date: 1/4/2021
ms.openlocfilehash: b22a6cecb2647df3878cb8fd4ade93d9a7d963fd
ms.sourcegitcommit: 2c1b93301174fccea00798df08e08872f53f669c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2021
ms.locfileid: "104770885"
---
# <a name="know-the-terms-for-baremetal-infrastructure"></a>Wichtige Begriffe in der BareMetal-Infrastruktur

In diesem Artikel werden einige wichtige BareMetal-Begriffe behandelt.

- **Revision**: Es gibt eine ursprüngliche Stamprevision, die als Revision 3 (Rev 3) bezeichnet wird, sowie zwei unterschiedliche Stamprevisionen für BareMetal-Instanz-Stamps. Die Stamps unterscheiden sich in Architektur und Nähe zu Hosts für Azure-VMs:
    - **Revision 4** (Rev 4): ein neueres Design, das eine größere Nähe zu den Hosts von Azure-VMs bietet und damit die Wartezeit zwischen Azure-VMs und BareMetal-Instanzeinheiten verkürzt. 
    - **Revision 4.2** (Rev 4.2): die neueste BareMetal Infrastructure-Revision mit neuem Branding, die die vorhandene Architektur von Rev 4 verwendet. Rev 4 befindet sich näher an den Azure-VM-Hosts (virtuelle Computer). Sie bietet wesentliche Verbesserungen bei der Netzwerklatenz zwischen Azure-VMs und BareMetal-Instanzeinheiten, die in Rev 4-Stamps oder -Reihen bereitgestellt werden. Sie können über das Azure-Portal auf Ihre BareMetal-Instanzen zugreifen und diese verwalten.    

- **Stamp**: Definiert die interne Microsoft-Bereitstellungsgröße von BareMetal-Instanzen. Bevor Instanzeinheiten bereitgestellt werden können, muss ein Stamp von BareMetal-Instanzen, der aus Compute-, Netzwerk- und Speicherracks besteht, an einem Rechenzentrumsstandort bereitgestellt werden. Eine solche Bereitstellung wird als Stamp einer BareMetal-Instanz oder mit Revision 4.2 bezeichnet.

- **Tenant:** Die Bereitstellung einer BareMetal-Instanz eines Kunden in einem Stamp wird in einem *Mandanten* isoliert. Ein Mandant wird in der Schicht der Netzwerke, Speicher und von Compute von anderen Mandanten isoliert. Speicher- und Compute-Einheiten, die unterschiedlichen Mandanten zugewiesen sind, können einander auf der Ebene des Stamps der BareMetal-Instanz nicht sehen und nicht miteinander kommunizieren. Ein Kunde kann Bereitstellungen in unterschiedlichen Mandanten durchführen. Auch dann ist die Kommunikation auf der Ebene des Stamps der BareMetal-Instanz nicht möglich.

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über die [BareMetal-Infrastruktur](concepts-baremetal-infrastructure-overview.md) oder das [Verwalten von BareMetal-Instanzen über das Azure-Portal](connect-baremetal-infrastructure.md). 