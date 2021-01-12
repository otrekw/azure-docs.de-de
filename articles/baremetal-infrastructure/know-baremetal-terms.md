---
title: Wichtige Begriffe in der Azure BareMetal-Infrastruktur
description: Machen Sie sich mit wichtigen Begriffen in der Azure BareMetal-Infrastruktur vertraut.
ms.topic: conceptual
ms.date: 12/31/2020
ms.openlocfilehash: f11bc4cfdd463623010ed7b6677235344ec7cd62
ms.sourcegitcommit: 42922af070f7edf3639a79b1a60565d90bb801c0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/31/2020
ms.locfileid: "97829044"
---
# <a name="know-the-terms-for-baremetal-infrastructure"></a>Wichtige Begriffe in der BareMetal-Infrastruktur

In diesem Artikel werden einige wichtige BareMetal-Begriffe behandelt.

- **Revision**: Es gibt zwei unterschiedliche Stamprevisionen für Stamps von BareMetal-Instanzen. Diese unterscheiden sich in Architektur und Nähe zu Hosts für Azure-VMs:
    - **Revision 3** (Rev 3): das ursprüngliche Design.
    - **Revision 4** (Rev 4): das neue Design, das eine größere Nähe zu den Hosts von Azure-VMs bietet und damit die Wartezeit zwischen Azure-VMs und BareMetal-Instanzeinheiten verkürzt. 
    - **Revision 4.2** (Rev 4.2): die neueste BareMetal Infrastructure-Revision mit neuem Branding, die die vorhandene Rev 4-Architektur verwendet. Sie können über das Azure-Portal auf Ihre BareMetal-Instanzen zugreifen und diese verwalten.  

- **Stamp**: Definiert die interne Microsoft-Bereitstellungsgröße von BareMetal-Instanzen. Bevor Instanzeinheiten bereitgestellt werden können, muss ein Stamp von BareMetal-Instanzen, der aus Compute-, Netzwerk- und Speicherschränken besteht, an einem Rechenzentrumsstandort bereitgestellt werden. Eine solche Bereitstellung wird als Stamp einer BareMetal-Instanz oder mit Revision 4.2 bezeichnet.

- **Mandant:** Die Bereitstellung einer BareMetal-Instanz eines Kunden in einem Stamp wird in einem *Mandanten* isoliert. Ein Mandant wird in der Schicht der Netzwerke, Speicher und von Compute von anderen Mandanten isoliert. Speicher- und Compute-Einheiten, die unterschiedlichen Mandanten zugewiesen sind, können einander auf der Ebene des Stamps der BareMetal-Instanz nicht sehen und nicht miteinander kommunizieren. Ein Kunde kann Bereitstellungen in unterschiedlichen Mandanten durchführen. Auch dann ist die Kommunikation auf der Ebene des Stamps der BareMetal-Instanz nicht möglich.

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie, wie Sie BareMetal-Instanzeinheiten im [Azure-Portal](workloads/sap/baremetal-infrastructure-portal.md) bestimmen und mit ihnen interagieren.


 