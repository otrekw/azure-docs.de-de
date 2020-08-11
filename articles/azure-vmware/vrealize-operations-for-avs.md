---
title: Einrichten von vRealize Operations für Azure VMware Solution (AVS)
description: ''
ms.topic: how-to
ms.date: 08/06/2020
ms.openlocfilehash: 57b513b681c4d2522b1c92946aab80fe8ba95746
ms.sourcegitcommit: 5f7b75e32222fe20ac68a053d141a0adbd16b347
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2020
ms.locfileid: "87475845"
---
# <a name="set-up-vrealize-operations-for-azure-vmware-solution-avs"></a>Einrichten von vRealize Operations für Azure VMware Solution (AVS)


vRealize Operations Manager ist eine Betriebsverwaltungsplattform, mit der VMware-Infrastrukturadministratoren Systemressourcen überwachen können. Diese Systemressourcen können (sowohl physische als auch virtuelle) Objekte auf Anwendungs- oder Infrastrukturebene sein. In der Vergangenheit haben die meisten VMware-Administratoren vRealize Operations zum Überwachen und Verwalten der Private Cloud-Komponenten von VMware verwendet: vCenter, ESXi, NSX-T, vSAN und Hybrid Cloud Extension (HCX). Jede AVS Private Cloud wird mit einer dedizierten vCenter-, NSX-T-, vSAN- und HCX-Bereitstellung bereitgestellt. 

Lesen Sie zuerst gründlich die Informationen unter [Voraussetzungen (Vorbereitung)](#before-you-begin) und [Voraussetzungen](#prerequisites). Anschließend führen wir Sie durch die zwei typischen Bereitstellungstopologien für vRealize Operations Manager mit AVS:

> [!div class="checklist"]
> * [Lokale vRealize Operations-Verwaltung von AVS-Bereitstellung](#on-premises-vrealize-operations-managing-avs-deployment)
> * [Ausführung von vRealize Operations in einer AVS-Bereitstellung](#vrealize-operations-running-on-avs-deployment)

## <a name="before-you-begin"></a>Voraussetzungen
* Weitere Informationen zur Bereitstellung von vRealize Operations finden Sie in der [vRealize Operations Manager-Produktdokumentation](https://docs.vmware.com/en/vRealize-Operations-Manager/8.1/com.vmware.vcom.vapp.doc/GUID-7FFC61A0-7562-465C-A0DC-46D092533984.html). 
* Sehen Sie sich die [Tutorialreihe](tutorial-network-checklist.md) mit Grundlagen zu AVS Software Defined Datacenter (SDDC) an.
* Lesen Sie optional die Produktdokumentation zum [vRealize Operations Remote Controller](https://docs.vmware.com/en/vRealize-Operations-Manager/8.1/com.vmware.vcom.vapp.doc/GUID-263F9219-E801-4383-8A59-E84F3D01ED6B.html) für die Option „lokale vRealize Operations-Verwaltung von AVS-Bereitstellung“. 



## <a name="prerequisites"></a>Voraussetzungen
* VPN oder Azure ExpressRoute sollten zwischen lokalem und AVS SDDC konfiguriert sein.
* Eine AVS Private Cloud wurde in Azure bereitgestellt.



## <a name="on-premises-vrealize-operations-managing-avs-deployment"></a>Lokale vRealize Operations-Verwaltung von AVS-Bereitstellung
Die meisten Kunden verfügen über eine vorhandene lokale Bereitstellung von vRealize Operations, die zum Verwalten von einer oder mehreren lokalen vCenter-Domänen verwendet wird. Wenn Kunden eine neue AVS Private Cloud in Azure bereitstellen, verbinden sie in der Regel ihre lokale Umgebung mit AVS, indem Sie entweder eine Azure ExpressRoute- oder eine Layer 3-VPN-Lösung verwenden – wie unten gezeigt.   

:::image type="content" source="media/vrealize-operations-manager/vrealize-operations-deployment-option-1.png" alt-text="Lokale vRealize Operations-Verwaltung von AVS-Bereitstellung"  border="false":::

Um die Funktionen von vRealize Operations auf die AVS Private Cloud zu erweitern, erstellen Sie eine Adapter[instanz für die AVS Private Cloud-Ressourcen](https://docs.vmware.com/en/vRealize-Operations-Manager/8.1/com.vmware.vcom.config.doc/GUID-640AD750-301E-4D36-8293-1BFEB67E2600.html), um Daten aus der AVS Private Cloud zu sammeln und sie in lokale vRealize Operations zu übernehmen. Die lokale vRealize Operations Manager-Instanz kann direkt eine Verbindung mit dem vCenter und NSX-T-Manager in AVS herstellen, oder Sie können optional einen vRealize Operations Remote Collector in der AVS-Private Cloud bereitstellen. Ein vRealize Operations Remote Collector komprimiert und verschlüsselt die aus der AVS Private Collector gesammelten Daten, bevor sie über das ExpressRoute- oder VPN-Netzwerk an den lokal ausgeführten vRealize Operations Manager gesendet werden. 

> [!TIP]
> Eine schrittweise Anleitung für die Installation von vRealize Operations Manager finden Sie in der [VMware-Dokumentation](https://docs.vmware.com/en/vRealize-Operations-Manager/8.1/com.vmware.vcom.vapp.doc/GUID-7FFC61A0-7562-465C-A0DC-46D092533984.html). 



## <a name="vrealize-operations-running-on-avs-deployment"></a>Ausführung von vRealize Operations in einer AVS-Bereitstellung

Eine andere Bereitstellungsoption besteht darin, eine Instanz von vRealize Operations Manager auf einem der vSphere-Cluster in der AVS Private Cloud bereitzustellen – wie unten gezeigt. 

:::image type="content" source="media/vrealize-operations-manager/vrealize-operations-deployment-option-2.png" alt-text="Ausführung von vRealize Operations in AVS" border="false":::

Nach dem Bereitstellen der AVS-Instanz von vRealize Operations können Sie vRealize Operations für das Sammeln von Daten aus vCenter, ESXi, NSX-T, vSAN und HCX konfigurieren. 

> [!TIP]
> Eine schrittweise Anleitung für die Installation von vRealize Operations Manager finden Sie in der [VMware-Dokumentation](https://docs.vmware.com/en/vRealize-Operations-Manager/8.1/com.vmware.vcom.vapp.doc/GUID-7FFC61A0-7562-465C-A0DC-46D092533984.html).




<!-- LINKS - external -->


<!-- LINKS - internal -->




