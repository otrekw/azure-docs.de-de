---
title: 'Tutorial: Hinzufügen eines NSX-T-Netzwerksegments in Azure VMware Solution'
description: Hier erfahren Sie, wie Sie ein NSX-T-Netzwerksegment für die Verwendung für virtuelle Computer in vCenter erstellen.
ms.topic: tutorial
ms.date: 03/13/2021
ms.openlocfilehash: 9125e552f9641a2d26b9584b66a4447f9c152161
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "103462121"
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

- [Erstellen und Verwalten von DHCP für Azure VMware Solution](manage-dhcp.md)
- [Erstellen einer Inhaltsbibliothek zum Bereitstellen von VMs in Azure VMware Solution](deploy-vm-content-library.md) 
- [Zusammenfassen lokaler Umgebungen mittels Peering zu einer privaten Cloud](tutorial-expressroute-global-reach-private-cloud.md)


<!-- LINKS - external-->

<!-- LINKS - internal -->
