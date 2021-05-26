---
title: 'Tutorial: Hinzufügen eines NSX-T-Netzwerksegments in Azure VMware Solution'
description: Hier erfahren Sie, wie Sie ein NSX-T-Netzwerksegment für die Verwendung für virtuelle Computer in vCenter erstellen.
ms.topic: tutorial
ms.date: 03/13/2021
ms.openlocfilehash: 70a0602b037aaebd4b9776c3a8dafafcb173fa23
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110064454"
---
# <a name="tutorial-add-a-network-segment-in-azure-vmware-solution"></a>Tutorial: Hinzufügen eines Netzwerksegments in Azure VMware Solution 

Die in vCenter erstellten virtuellen Computer (VMs) werden in den unter NSX-T erstellten Netzwerksegmenten platziert und sind in vCenter sichtbar.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Navigieren in NSX-T Manager zum Hinzufügen von Netzwerksegmenten
> * Hinzufügen eines neuen Netzwerksegments
> * Beobachten des neuen Netzwerksegments in vCenter

## <a name="prerequisites"></a>Voraussetzungen

Eine private Azure VMware Solution-Cloud mit Zugriff auf die vCenter- und NSX-T Manager-Verwaltungsschnittstellen Weitere Informationen finden Sie im Tutorial zum [Konfigurieren des Netzwerks](tutorial-configure-networking.md).

## <a name="add-a-network-segment"></a>Hinzufügen eines Netzwerksegments

[!INCLUDE [add-network-segment-steps](includes/add-network-segment-steps.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie ein NSX-T-Netzwerksegment erstellt, das für virtuelle Computer in vCenter verwendet wird. 

Sie können nun Folgendes: 

- [Erstellen und Verwalten von DHCP für Azure VMware Solution](configure-dhcp-azure-vmware-solution.md)
- [Erstellen einer Inhaltsbibliothek zum Bereitstellen von VMs in Azure VMware Solution](deploy-vm-content-library.md) 
- [Zusammenfassen lokaler Umgebungen mittels Peering zu einer privaten Cloud](tutorial-expressroute-global-reach-private-cloud.md)


<!-- LINKS - external-->

<!-- LINKS - internal -->
