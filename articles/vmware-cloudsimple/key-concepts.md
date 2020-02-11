---
title: Wichtige Konzepte für die Verwaltung von Azure VMware Solutions (AVS)
description: Enthält eine Beschreibung der wichtigen Konzepte für die Verwaltung von Azure VMware Solutions (AVS).
titleSuffix: Azure VMware Solutions (AVS)
author: sharaths-cs
ms.author: b-shsury
ms.date: 04/24/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: c581ffbba7fbaefc8fbd0ffc7e92350bd69802b8
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77025230"
---
# <a name="key-concepts-for-administration-of-azure-vmware-solutions-avs"></a>Wichtige Konzepte für die Verwaltung von Azure VMware Solutions (AVS)

Für die Verwaltung von Azure VMware Solutions (AVS) müssen Sie mit den folgenden Konzepten vertraut sein:

* AVS-Dienst (Anzeige als „Azure VMware Solutions (AVS): Dienst“)
* AVS-Knoten (Anzeige als „Azure VMware Solutions (AVS): Knoten“)
* Private AVS-Cloud
* Dienstnetzwerk
* Virtueller AVS-Computer (Anzeige als „Azure VMware Solutions (AVS): Virtueller Computer“)

## <a name="avs-service"></a>AVS-Dienst

Mit dem AVS-Dienst können Sie sämtliche Ressourcen für Azure VMware Solutions (AVS) über das Azure-Portal erstellen und verwalten. Erstellen Sie in jeder Region, in der Sie den Dienst verwenden möchten, eine Dienstressource.

Lesen Sie weitere Informationen zum [AVS-Dienst](cloudsimple-service.md).

## <a name="avs-node"></a>AVS-Knoten

Bei einem AVS-Knoten handelt es sich um einen dedizierten, hyperkonvergenten Bare-Metal-Compute- und Speicherhost, auf dem der VMware ESXi-Hypervisor bereitgestellt wird. Dieser Knoten wird dann in die VMware vSphere-, vCenter-, vSAN- und NSX-Plattform integriert. AVS-Netzwerkdienste und Umkreisnetzwerkdienste werden ebenfalls aktiviert. Jeder Knoten fungiert als Compute- und Speicherkapazitätseinheit, die Sie zur Erstellung [privater AVS-Clouds](cloudsimple-private-cloud.md) bereitstellen können. Sie stellen Knoten in einer Region bereit, in der der AVS-Dienst verfügbar ist, oder reservieren sie.

Erfahren Sie mehr über [AVS-Knoten](cloudsimple-node.md).

## <a name="avs-private-cloud"></a>Private AVS-Cloud

Eine private AVS-Cloud ist eine isolierte VMware-Stapelumgebung, die von einem vCenter-Server in einer eigenen Verwaltungsdomäne verwaltet wird. Der VMware-Stapel beinhaltet ESXi-Hosts, vSphere, vCenter, vSAN und NSX. Der Stapel wird auf dedizierten Knoten (dedizierte und isolierte Bare-Metal-Hardware) ausgeführt und von Benutzern über native VMware-Tools (einschließlich vCenter und NSX Manager) genutzt. Dedizierte Knoten werden an Azure-Standorten bereitgestellt und von Azure verwaltet. Jede private AVS-Cloud kann mithilfe von Netzwerkdiensten wie VLANs und Subnetzen sowie Firewalltabellen segmentiert und geschützt werden. Für die Verbindungsherstellung mit Ihrer lokalen Umgebung und dem Azure-Netzwerk werden ein sicheres, privates VPN sowie Azure ExpressRoute-Verbindungen verwendet.

Informieren Sie sich über die [private AVS-Cloud](cloudsimple-private-cloud.md).

## <a name="service-networking"></a>Dienstnetzwerk

Der AVS-Dienst verfügt über ein Netzwerk pro Region, in der Ihr AVS-Dienst bereitgestellt wurde. Das Netzwerk ist ein einzelner TCP-Schicht-3-Adressraum mit standardmäßig aktiviertem Routing. Alle privaten AVS-Clouds und Subnetze, die in dieser Region erstellt werden, kommunizieren ohne zusätzliche Konfiguration miteinander. Verteilte Portgruppen in vCenter werden unter Verwendung von VLANs erstellt. Die Workloadressourcen in Ihrer privaten AVS-Cloud können mithilfe der folgenden Netzwerkfeatures konfiguriert und geschützt werden:

* [VLANs und Subnetze](cloudsimple-vlans-subnets.md)
* [Firewalltabellen](cloudsimple-firewall-tables.md)
* [VPN-Gateways](cloudsimple-vpn-gateways.md)
* [Öffentliche IP-Adresse](cloudsimple-public-ip-address.md)
* [Azure-Netzwerkverbindung](cloudsimple-azure-network-connection.md)

## <a name="avs-virtual-machine"></a>Virtueller AVS-Computer

Mit dem AVS-Dienst können Sie virtuelle VMware-Computer über das Azure-Portal verwalten. Cluster oder Ressourcenpools aus Ihrer vSphere-Umgebung können dem Abonnement zugeordnet werden, für das der Dienst erstellt wird.

Weitere Informationen:

* [Virtuelle AVS-Computer](cloudsimple-virtual-machines.md)
* [Azure-Abonnementzuordnung](https://docs.azure.cloudsimple.com/azure-subscription-mapping/)
