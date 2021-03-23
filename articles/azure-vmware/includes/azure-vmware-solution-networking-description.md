---
title: Azure VMware Solution-Netzwerkbetrieb und -Konnektivität
description: Beschreibung von Azure VMware Solution-Netzwerkbetrieb und -Konnektivität
ms.topic: include
ms.date: 03/13/2021
ms.openlocfilehash: 96dd93f1db5dc3ddcbb883313e19c6aed8a256da
ms.sourcegitcommit: afb9e9d0b0c7e37166b9d1de6b71cd0e2fb9abf5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/14/2021
ms.locfileid: "103462580"
---
<!-- Used in introduction.md and concepts-networking.md -->

Azure VMware Solution stellt eine private Cloudumgebung bereit, auf die aus lokalen und Azure-basierten Ressourcen aus zugegriffen werden kann. Dienste wie Azure ExpressRoute, VPN-Verbindungen oder Azure Virtual WAN stellen die Konnektivität bereit. Diese Dienste benötigen bestimmte Netzwerkadressbereiche und Firewallports für die Aktivierung der Dienste.

Wenn eine private Cloud bereitgestellt wird, werden private Netzwerke für Verwaltung, Bereitstellung und vMotion erstellt. Verwenden Sie diese privaten Netzwerke, um auf vCenter und NSX-T Manager zuzugreifen sowie vMotion oder Bereitstellung virtueller Computer umzusetzen.  

ExpressRoute Global Reach wird verwendet, um private Clouds mit lokalen Umgebungen zu verbinden. Die Verbindung erfordert ein virtuelles Netzwerk mit einer ExpressRoute-Leitung zur lokalen Umgebung in Ihrem Abonnement.

In der privaten Cloud bereitgestellte virtuelle Computer sind über die öffentliche IP-Adresse von Azure Virtual WAN für das Internet zugänglich.  Der Internetzugriff ist für neue private Clouds standardmäßig deaktiviert. Weitere Informationen finden Sie unter [Verwenden der Funktion „öffentliche IP-Adresse“ in Azure VMware Solution](../public-ip-usage.md).

