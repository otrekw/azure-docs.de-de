---
title: Einrichten von vRealize Operations für Azure VMware Solution
description: Hier erfahren Sie, wie Sie vRealize Operations für Ihre private Azure VMware Solution-Cloud einrichten.
ms.topic: how-to
ms.date: 08/06/2020
ms.openlocfilehash: 729ee5c64776d7d04f702af62451175f7c53421b
ms.sourcegitcommit: 62717591c3ab871365a783b7221851758f4ec9a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/22/2020
ms.locfileid: "88750402"
---
# <a name="set-up-vrealize-operations-for-azure-vmware-solution"></a>Einrichten von vRealize Operations für Azure VMware Solution


vRealize Operations Manager ist eine Betriebsverwaltungsplattform, mit der VMware-Infrastrukturadministratoren Systemressourcen überwachen können. Diese Systemressourcen können (sowohl physische als auch virtuelle) Objekte auf Anwendungs- oder Infrastrukturebene sein. In der Vergangenheit haben die meisten VMware-Administratoren vRealize Operations zum Überwachen und Verwalten der Private Cloud-Komponenten von VMware verwendet: vCenter, ESXi, NSX-T, vSAN und Hybrid Cloud Extension (HCX). Jede private Azure VMware Solution-Cloud wird mit einer dedizierten vCenter-, NSX-T-, vSAN- und HCX-Bereitstellung bereitgestellt. 

Lesen Sie zuerst gründlich die Informationen unter [Voraussetzungen (Vorbereitung)](#before-you-begin) und [Voraussetzungen](#prerequisites). Anschließend führen wir Sie durch die zwei typischen Bereitstellungstopologien für vRealize Operations Manager mit Azure VMware Solution:

> [!div class="checklist"]
> * [Lokale vRealize Operations-Verwaltung von Azure VMware Solution-Bereitstellung](#on-premises-vrealize-operations-managing-azure-vmware-solution-deployment)
> * [Ausführung von vRealize Operations in Azure VMware Solution-Bereitstellung](#vrealize-operations-running-on-azure-vmware-solution-deployment)

## <a name="before-you-begin"></a>Voraussetzungen
* Weitere Informationen zur Bereitstellung von vRealize Operations finden Sie in der [vRealize Operations Manager-Produktdokumentation](https://docs.vmware.com/en/vRealize-Operations-Manager/8.1/com.vmware.vcom.vapp.doc/GUID-7FFC61A0-7562-465C-A0DC-46D092533984.html). 
* Sehen Sie sich die [Tutorialreihe](tutorial-network-checklist.md) mit Grundlagen zu Azure VMware Solution Software Defined Datacenter (SDDC) an.
* Lesen Sie optional die Produktdokumentation zum [vRealize Operations-Remotecontroller](https://docs.vmware.com/en/vRealize-Operations-Manager/8.1/com.vmware.vcom.vapp.doc/GUID-263F9219-E801-4383-8A59-E84F3D01ED6B.html) für die Option „lokale vRealize Operations-Verwaltung von Azure VMware Solution-Bereitstellung“. 



## <a name="prerequisites"></a>Voraussetzungen
* Zwischen lokalen Ressourcen und Azure VMware Solution SDDC sollte VPN oder Azure ExpressRoute konfiguriert sein.
* Eine private Azure VMware Solution-Cloud wurde in Azure bereitgestellt.



## <a name="on-premises-vrealize-operations-managing-azure-vmware-solution-deployment"></a>Lokale vRealize Operations-Verwaltung von Azure VMware Solution-Bereitstellung
Die meisten Kunden verfügen über eine vorhandene lokale Bereitstellung von vRealize Operations, die zum Verwalten von einer oder mehreren lokalen vCenter-Domänen verwendet wird. Wenn Kunden eine neue private Azure VMware Solution-Cloud in Azure bereitstellen, verbinden sie in der Regel ihre lokale Umgebung mit Azure VMware Solution, indem sie entweder eine Azure ExpressRoute- oder eine Layer 3-VPN-Lösung verwenden, wie nachfolgende gezeigt:   

:::image type="content" source="media/vrealize-operations-manager/vrealize-operations-deployment-option-1.png" alt-text="Lokale vRealize Operations-Verwaltung von Azure VMware Solution-Bereitstellung"  border="false":::

Um die Funktionen von vRealize Operations auf die private Azure VMware Solution-Cloud zu erweitern, erstellen Sie eine Adapter[instanz für die Ressourcen der privaten Azure VMware Solution-Cloud](https://docs.vmware.com/en/vRealize-Operations-Manager/8.1/com.vmware.vcom.config.doc/GUID-640AD750-301E-4D36-8293-1BFEB67E2600.html), um Daten aus der privaten Azure VMware Solution-Cloud zu sammeln und sie in die lokale vRealize Operations-Instanz zu übernehmen. Die lokale vRealize Operations Manager-Instanz kann direkt eine Verbindung mit vCenter und NSX-T-Manager in Azure VMware Solution herstellen, oder Sie können optional einen vRealize Operations-Remotecollector in der privaten Azure VMware Solution-Cloud bereitstellen. Ein vRealize Operations-Remotecollector komprimiert und verschlüsselt die aus der privaten Azure VMware Solution-Cloud gesammelten Daten, bevor sie über das ExpressRoute- oder VPN-Netzwerk an die lokal ausgeführte vRealize Operations Manager-Instanz gesendet werden. 

> [!TIP]
> Eine schrittweise Anleitung für die Installation von vRealize Operations Manager finden Sie in der [VMware-Dokumentation](https://docs.vmware.com/en/vRealize-Operations-Manager/8.1/com.vmware.vcom.vapp.doc/GUID-7FFC61A0-7562-465C-A0DC-46D092533984.html). 



## <a name="vrealize-operations-running-on-azure-vmware-solution-deployment"></a>Ausführung von vRealize Operations in Azure VMware Solution-Bereitstellung

Eine andere Bereitstellungsoption besteht darin, eine Instanz von vRealize Operations Manager in einem der vSphere-Cluster in der privaten Azure VMware Solution-Cloud bereitzustellen, wie nachfolgend gezeigt: 

:::image type="content" source="media/vrealize-operations-manager/vrealize-operations-deployment-option-2.png" alt-text="Ausführung von vRealize Operations in Azure VMware Solution" border="false":::

Nach dem Bereitstellen der Azure VMware Solution-Instanz von vRealize Operations können Sie vRealize Operations für das Sammeln von Daten aus vCenter, ESXi, NSX-T, vSAN und HCX konfigurieren. 

> [!TIP]
> Eine schrittweise Anleitung für die Installation von vRealize Operations Manager finden Sie in der [VMware-Dokumentation](https://docs.vmware.com/en/vRealize-Operations-Manager/8.1/com.vmware.vcom.vapp.doc/GUID-7FFC61A0-7562-465C-A0DC-46D092533984.html).




<!-- LINKS - external -->


<!-- LINKS - internal -->




