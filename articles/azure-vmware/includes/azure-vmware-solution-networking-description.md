---
title: Azure VMware Solution-Netzwerkbetrieb und -Konnektivität
description: Beschreibung von Azure VMware Solution-Netzwerkbetrieb und -Konnektivität
ms.topic: include
ms.date: 05/28/2021
ms.openlocfilehash: 66ea12565aa84661a4019e5685a5544792591b0c
ms.sourcegitcommit: 7f59e3b79a12395d37d569c250285a15df7a1077
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/02/2021
ms.locfileid: "111350711"
---
<!-- Used in introduction.md and concepts-networking.md -->

Azure VMware Solution stellt eine private Cloudumgebung bereit, auf die aus lokalen und Azure-basierten Ressourcen aus zugegriffen werden kann. Dienste wie Azure ExpressRoute, VPN-Verbindungen oder Azure Virtual WAN stellen die Konnektivität bereit. Diese Dienste benötigen bestimmte Netzwerkadressbereiche und Firewallports für die Aktivierung der Dienste.

Wenn eine private Cloud bereitgestellt wird, werden private Netzwerke für Verwaltung, Bereitstellung und vMotion erstellt. Verwenden Sie diese privaten Netzwerke, um auf vCenter und NSX-T Manager zuzugreifen sowie vMotion oder Bereitstellung virtueller Computer umzusetzen.  

ExpressRoute Global Reach wird verwendet, um private Clouds mit lokalen Umgebungen zu verbinden. Die Verbindung erfordert ein virtuelles Netzwerk mit einer ExpressRoute-Leitung zur lokalen Umgebung in Ihrem Abonnement.

In der privaten Cloud bereitgestellte virtuelle Computer sind über die öffentliche IP-Adresse von Azure Virtual WAN für das Internet zugänglich.  Der Internetzugriff ist für neue private Clouds standardmäßig deaktiviert. Weitere Informationen finden Sie unter [Aktivieren des öffentlichen Internetzugriffs in Azure VMware Solution](../enable-public-internet-access.md).



