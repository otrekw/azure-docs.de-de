---
title: Azure VMware Solution-Netzwerkbetrieb und -Konnektivität
description: Beschreibung von Azure VMware Solution-Netzwerkbetrieb und -Konnektivität
ms.topic: include
ms.date: 09/28/2020
ms.openlocfilehash: 1d43a8a49fc56a6a5b534a89fd1dfc2f5ae2d478
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91574454"
---
<!-- Used in introduction.md and concepts-networking.md -->

Azure VMware Solution stellt eine private Cloud bereit, auf die aus lokalen und Azure-basierten Umgebungen oder Ressourcen aus zugegriffen werden kann. Dienste wie Azure ExpressRoute und VPN-Verbindungen stellen die Konnektivität bereit. Diese Dienste benötigen bestimmte Netzwerkadressbereiche und Firewallports für die Aktivierung der Dienste.

Wenn eine private Cloud bereitgestellt wird, werden private Netzwerke für Verwaltung, Bereitstellung und vMotion erstellt. Verwenden Sie diese privaten Netzwerke, um auf vCenter und NSX-T Manager zuzugreifen sowie vMotion oder Bereitstellung virtueller Computer umzusetzen.  ExpressRoute Global Reach wird verwendet, um private Clouds mit lokalen Umgebungen zu verbinden. Die Verbindung erfordert ein virtuelles Netzwerk mit einer ExpressRoute-Leitung in Ihrem Abonnement.



>[!NOTE]
>Zugriff auf das Internet und Azure-Dienste werden bereitgestellt, damit VMs in Produktionsnetzwerken diese nutzen können, wenn eine private Cloud bereitgestellt wird.  Der Internetzugriff ist für neue private Clouds standardmäßig deaktiviert und kann jederzeit aktiviert oder deaktiviert werden.