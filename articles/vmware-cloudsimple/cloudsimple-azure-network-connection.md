---
title: 'VMware Solution (AVS): Azure-Netzwerkverbindungen'
description: Hier erfahren Sie mehr darüber, wie Sie eine Verbindung zwischen Ihrem virtuellen Azure-Netzwerk und dem AVS-Regionsnetzwerk herstellen.
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: bf11d4e2676179e8b71d3a03f8ed3cbcb4cfba9d
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77025077"
---
# <a name="azure-network-connections-overview"></a>Übersicht über Azure-Netzwerkverbindungen

Wenn Sie einen AVS-Dienst in einer Region sowie Knoten erstellen, können Sie die folgenden Aktionen ausführen:

* Anfordern einer Azure ExpressRoute-Leitung und Anfügen der Leitung and das AVS-Netzwerk in dieser Region
* Verbinden Ihres AVS-Regionsnetzwerks mit Ihrem virtuellen Azure-Netzwerk oder Ihrem lokalen Netzwerk mithilfe von Azure ExpressRoute
* Bereitstellen des Zugriffs aus der privaten Cloudumgebung auf Dienste, die in Ihrem Azure-Abonnement oder lokalen Netzwerk ausgeführt werden.

Die ExpressRoute-Verbindung weist eine hohe Bandbreite mit geringer Latenz auf.

## <a name="benefits"></a>Vorteile

Eine Azure-Netzwerkverbindung ermöglicht Ihnen Folgendes:

* Verwenden von Azure als Sicherungsziel für virtuelle Computer in Ihrer privaten Cloud
* Bereitstellen von KMS-Servern in Ihrem Azure-Abonnement, um den vSAN-Datenspeicher Ihrer privaten Cloud zu verschlüsseln
* Verwenden von Hybridanwendungen, bei denen die Webebene der jeweiligen Anwendung in der öffentlichen AVS-Cloud ausgeführt wird, während die Anwendungs- und die Datenbankebene in Ihrer privaten Cloud ausgeführt werden

## <a name="azure-virtual-network-connection"></a>Verbindung mit virtuellem Azure-Netzwerk

Private AVS-Clouds können mit Ihren Azure-Ressourcen mithilfe von ExpressRoute verbunden werden. Die ExpressRoute-Verbindung ermöglicht Ihnen den Zugriff auf verschiedene Ressourcen, die in Ihrem Azure-Abonnement über Ihre private AVS-Cloud ausgeführt werden. Diese Verbindung ermöglicht es Ihnen, Ihr privates AVS-Cloudnetzwerk auf Ihr virtuelles Azure-Netzwerk zu erweitern. Routen aus einem AVS-Netzwerk werden über BGP mit Ihrem virtuellen Azure-Netzwerk ausgetauscht. Wenn Sie das Peering virtueller Netzwerke konfiguriert haben, ist der Zugriff auf alle über Peering verbundenen virtuellen Netzwerke über Ihr AVS-Netzwerk möglich.

![Azure ExpressRoute-Verbindung mit virtuellem Netzwerk](media/cloudsimple-azure-network-connection.png)

## <a name="expressroute-connection-to-on-premises-network"></a>ExpressRoute-Verbindung mit lokalem Netzwerk

Sie können Ihre vorhandene Azure ExpressRoute-Leitung mit Ihrer AVS-Region verbinden. Über den ExpressRoute-Dienst Global Reach werden die beiden Netzwerke miteinander verbunden. Es wird eine Verbindung zwischen dem lokalen Netzwerk und dem AVS-ExpressRoute-Netzwerk hergestellt. Diese Verbindung ermöglicht es Ihnen, Ihre lokalen Netzwerke auf das private AVS-Cloudnetzwerk zu erweitern. Routen aus dem AVS-Netzwerk werden über BGP mit Ihrem lokalen Netzwerk ausgetauscht.

![Lokale ExpressRoute-Verbindung – Global Reach](media/cloudsimple-global-reach-connection.png)

## <a name="connection-to-on-premises-network-and-azure-virtual-network"></a>Verbindung mit dem lokalem Netzwerk und dem virtuellen Azure-Netzwerk

Verbindungen mit dem lokalen Netzwerk und dem virtuellen Azure-Netzwerk können über Ihr AVS-Netzwerk koexistieren. Die Verbindung verwendet BGP zum Austauschen von Routen zwischen einem lokalen Netzwerk, dem virtuellen Azure-Netzwerk und einem AVS-Netzwerk. Wenn eine Global Reach-Verbindung vorhanden ist und Sie Ihr AVS-Netzwerk mit Ihrem virtuellen Azure-Netzwerk verbinden, werden Routen des virtuellen Azure-Netzwerks in Ihrem lokalen Netzwerk angezeigt. Der Routenaustausch erfolgt in Azure zwischen den Edgeroutern.

![Lokale ExpressRoute-Verbindung mit der Verbindung des virtuellen Azure-Netzwerks](media/cloudsimple-global-reach-and-vnet-connection.png)

### <a name="important-considerations"></a>Wichtige Hinweise

Das Herstellen einer Verbindung mit dem AVS-Netzwerk aus einem lokalen Netzwerk und/oder aus einem virtuellen Azure-Netzwerk ermöglicht den Routenaustausch zwischen allen Netzwerken.

* Das virtuelle Azure-Netzwerk ist im lokalen Netzwerk und im AVS-Netzwerk sichtbar.
* Wenn Sie eine Verbindung mit Ihrem virtuellen Azure-Netzwerk über ein lokales Netzwerk hergestellt haben, ermöglicht die Verbindung mit einem AVS-Netzwerk mithilfe von Global Reach den Zugriff auf virtuelle Netzwerke über das AVS-Netzwerk.
* Subnetzadressen **dürfen sich nicht** zwischen den verbundenen Netzwerken überschneiden.
* AVS kündigt die Standardroute **nicht** für ExpressRoute-Verbindungen an
* Wenn Ihr lokaler Router die Standardroute ankündigt, verwendet der Datenverkehr aus dem AVS-Netzwerk und dem virtuellen Azure-Netzwerk die angekündigte Standardroute. Daher kann auf virtuelle Computer in Azure nicht mit öffentlichen IP-Adressen zugegriffen werden.

## <a name="next-steps"></a>Nächste Schritte

* [Verbinden des virtuellen Azure-Netzwerks mit AVS mithilfe von ExpressRoute](virtual-network-connection.md)
* [Herstellen einer Verbindung aus der lokalen Umgebung mit AVS mithilfe von ExpressRoute](on-premises-connection.md)
