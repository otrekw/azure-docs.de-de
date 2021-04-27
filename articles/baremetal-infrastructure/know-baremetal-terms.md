---
title: Wichtige Begriffe in der Azure BareMetal-Infrastruktur
description: Machen Sie sich mit wichtigen Begriffen in der Azure BareMetal-Infrastruktur vertraut.
ms.topic: conceptual
ms.subservice: workloads
ms.date: 04/06/2021
ms.openlocfilehash: 53a601cc4556198479d8ca5d7495942d4dc2762c
ms.sourcegitcommit: d63f15674f74d908f4017176f8eddf0283f3fac8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/07/2021
ms.locfileid: "106580140"
---
# <a name="know-the-terms-for-baremetal-infrastructure"></a>Wichtige Begriffe in der BareMetal-Infrastruktur

In diesem Artikel werden einige wichtige Begriffe im Zusammenhang mit der BareMetal-Infrastruktur behandelt.

- **Revision**: Es gibt eine ursprüngliche Stamprevision, die als Revision 3 (Rev 3) bezeichnet wird, sowie zwei zusätzliche Stamprevisionen für BareMetal-Instanz-Stamps. Die Stamps unterscheiden sich in Architektur und Nähe zu Hosts für Azure-VMs:
    - **Revision 4** (Rev 4): ein neueres Design, das eine größere Nähe zu den Hosts von Azure-VMs bietet und damit die Wartezeit zwischen Azure-VMs und SAP HANA-Instanzen verkürzt. 
    - **Revision 4.2** (Rev 4.2): die neueste BareMetal Infrastructure-Revision mit neuem Branding, die die vorhandene Architektur von Rev 4 verwendet. Rev 4 befindet sich näher an den Azure-VM-Hosts (virtuelle Computer). Sie bietet wesentliche Verbesserungen bei der Netzwerklatenz zwischen Azure-VMs und BareMetal-Instanzen, die in Rev 4-Stamps oder -Reihen bereitgestellt werden. Sie können über das Azure-Portal auf Ihre BareMetal-Instanzen zugreifen und diese verwalten.    

- **Stamp**: Definiert die interne Microsoft-Bereitstellungsgröße von BareMetal-Instanzen. Bevor Instanzen bereitgestellt werden können, muss ein Stamp von BareMetal-Instanzen, der aus Compute-, Netzwerk- und Speicherracks besteht, an einem Rechenzentrumsstandort bereitgestellt werden. Eine solche Bereitstellung wird als Stamp einer BareMetal-Instanz bezeichnet.

- **Mandant**: Ein Kunde, der einen BareMetal-Instanzstamp bereitstellt, wird als *Mandant* isoliert. Ein Mandant wird in der Schicht der Netzwerke, Speicher und von Compute von anderen Mandanten isoliert. Speicher- und Compute-Einheiten, die unterschiedlichen Mandanten zugewiesen sind, können einander auf der Ebene des Stamps der BareMetal-Instanz nicht sehen und nicht miteinander kommunizieren. Ein Kunde kann Bereitstellungen in unterschiedlichen Mandanten durchführen. Auch dann ist die Kommunikation auf der Ebene des Stamps der BareMetal-Instanz nicht möglich.

## <a name="next-steps"></a>Nächste Schritte
Nach dieser Einführung in die wichtige Terminologie der BareMetal-Infrastruktur, möchten Sie sich vielleicht über Folgendes informieren:
- Weitere Details der [BareMetal-Infrastruktur](concepts-baremetal-infrastructure-overview.md)
- [Verbinden von BareMetal-Infrastrukturinstanzen in Azure](connect-baremetal-infrastructure.md)

