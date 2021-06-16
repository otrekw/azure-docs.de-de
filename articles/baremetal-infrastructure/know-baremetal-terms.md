---
title: Wichtige Begriffe in der Azure BareMetal-Infrastruktur
description: Machen Sie sich mit wichtigen Begriffen in der Azure BareMetal-Infrastruktur vertraut.
ms.topic: conceptual
ms.date: 04/06/2021
ms.openlocfilehash: 91270b2c16a1b642b697fd4ffb6a9c635608110e
ms.sourcegitcommit: e1d5abd7b8ded7ff649a7e9a2c1a7b70fdc72440
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2021
ms.locfileid: "110576313"
---
# <a name="know-the-terms-for-baremetal-infrastructure"></a>Wichtige Begriffe in der BareMetal-Infrastruktur

In diesem Artikel werden einige wichtige Begriffe im Zusammenhang mit der BareMetal-Infrastruktur behandelt.

- **Revision**: Es gibt zwei unterschiedliche Stamprevisionen für Stamps der Bare-Metal-Infrastruktur (große HANA-Instanz). Diese unterscheiden sich in der Architektur und in der Nähe zu Hosts für virtuelle Computer in Azure:
    - „Revision 3“ (Rev 3): Der ursprüngliche Entwurf, der Mitte 2016 bereitgestellt wurde.
    - „Revision 4.2“ (Rev 4.2): Ein neuer Entwurf, der eine größere Nähe zu Hosts für virtuelle Computer in Azure mit äußerst niedriger Netzwerklatenz zwischen virtuellen Azure-Computern und großen HANA-Instanzen bietet. Ressourcen im Azure-Portal werden als „Bare-Metal-Infrastruktur“ bezeichnet, und Kunden können über das Azure-Portal auf ihre Ressourcen als Bare-Metal-Instanzen zugreifen.

- **Stamp**: Definiert die interne Microsoft-Bereitstellungsgröße von BareMetal-Instanzen. Bevor Instanzen bereitgestellt werden können, muss ein Stamp von BareMetal-Instanzen, der aus Compute-, Netzwerk- und Speicherracks besteht, an einem Rechenzentrumsstandort bereitgestellt werden. Eine solche Bereitstellung wird als Stamp einer BareMetal-Instanz bezeichnet.

- **Mandant**: Ein Kunde, der einen BareMetal-Instanzstamp bereitstellt, wird als *Mandant* isoliert. Ein Mandant wird in der Schicht der Netzwerke, Speicher und von Compute von anderen Mandanten isoliert. Speicher- und Compute-Einheiten, die unterschiedlichen Mandanten zugewiesen sind, können einander auf der Ebene des Stamps der BareMetal-Instanz nicht sehen und nicht miteinander kommunizieren. Ein Kunde kann Bereitstellungen in unterschiedlichen Mandanten durchführen. Auch dann ist die Kommunikation auf der Ebene des Stamps der BareMetal-Instanz nicht möglich.

## <a name="next-steps"></a>Nächste Schritte

Nach dieser Einführung in die wichtige Terminologie der BareMetal-Infrastruktur, möchten Sie sich vielleicht über Folgendes informieren:
- Weitere Details der [BareMetal-Infrastruktur](concepts-baremetal-infrastructure-overview.md)
- [Verbinden von BareMetal-Infrastrukturinstanzen in Azure](connect-baremetal-infrastructure.md)

