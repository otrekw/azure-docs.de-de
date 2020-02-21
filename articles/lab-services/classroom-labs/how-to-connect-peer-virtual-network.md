---
title: Herstellen einer Verbindung mit einem Peernetzwerk in Azure Lab Services | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie eine Peerverbindung zwischen Ihrem Labnetzwerk und einem anderen Netzwerk herstellen. Sie können z.B. Ihr lokales Schul- oder Universitätsnetzwerk mit dem virtuellen Netzwerk eines Labs in Azure verbinden.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/14/2020
ms.author: spelluru
ms.openlocfilehash: 5e013011f81542aa279ba9276a6a1aac01eb9e41
ms.sourcegitcommit: 6e87ddc3cc961945c2269b4c0c6edd39ea6a5414
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/18/2020
ms.locfileid: "77443187"
---
# <a name="connect-your-labs-network-with-a-peer-virtual-network-in-azure-lab-services"></a>Verbinden des Netzwerks Ihres Labs mit einem virtuellen Peernetzwerk in Azure Lab Services 
Dieser Artikel bietet Informationen dazu, wie Sie eine Peerverbindung zwischen dem Netzwerk Ihres Labs und einem anderen Netzwerk herstellen. 

## <a name="overview"></a>Übersicht
Das Peering virtueller Netzwerke ermöglicht es, virtuelle Azure-Netzwerke nahtlos miteinander zu verbinden. Nach dem Peering werden die virtuellen Netzwerke für Verbindungszwecke als einzelnes Element angezeigt. Der Datenverkehr zwischen virtuellen Computern in den virtuellen Netzwerken, die mittels Peering miteinander verbunden sind, wird ähnlich wie der Datenverkehr zwischen virtuellen Computern im gleichen Netzwerk nur über private IP-Adressen über die Microsoft-Backbone-Infrastruktur geleitet. Weitere Informationen finden Sie unter [Peering in virtuellen Netzwerken](../../virtual-network/virtual-network-peering-overview.md).

In einigen Szenarien, wie beispielsweise den folgenden, müssen Sie das Netzwerk Ihres Labs mit einem virtuellen Peernetzwerk verbinden:

- Auf den virtuellen Computern im Lab ist Software installiert, die eine Verbindung mit lokalen Lizenzservern herstellen muss, um eine Lizenz zu erhalten.
- Die virtuellen Computer im Lab benötigen Zugriff auf Datasets (oder andere Dateien) auf Freigaben im Netzwerk einer Universität. 

Einige lokale Netzwerke sind über [ExpressRoute](../../expressroute/expressroute-introduction.md) oder ein [Gateway für virtuelle Netzwerke](../../vpn-gateway/vpn-gateway-about-vpngateways.md) mit Azure Virtual Network verbunden. Diese Dienste müssen außerhalb von Azure Lab Services eingerichtet werden. Weitere Informationen zum Herstellen einer Verbindung zwischen einem lokalen Netzwerk und Azure über ExpressRoute finden Sie unter [Übersicht über ExpressRoute](../../expressroute/expressroute-introduction.md). Für die lokale Verbindung über ein Gateway für virtuelle Netzwerke müssen sich das Gateway, das angegebene virtuelle Netzwerk und das Labkonto in der gleichen Region befinden.

## <a name="configure-at-the-time-of-lab-account-creation"></a>Konfigurieren zum Zeitpunkt der Labkontoerstellung
Während Sie ein neues Labkonto erstellen, können Sie ein vorhandenes virtuelles Netzwerk auswählen, das in der Dropdownliste **Virtuelles Peernetzwerk** auf der Registerkarte **Erweitert** angezeigt wird. Das ausgewählte virtuelle Netzwerk ist (mittels Peering) mit Labs verbunden, die im Labkonto erstellt wurden. Alle virtuellen Computer in Labs, die nach Durchführen dieser Änderung erstellt werden, haben Zugriff auf die Ressourcen im virtuellen Peernetzwerk. 

![Auswählen des virtuellen Netzwerks für das Peering](../media/how-to-connect-peer-virtual-network/select-vnet-to-peer.png)

> [!NOTE]
> Detaillierte Schrittanleitungen zum Erstellen eines Labkontos finden Sie unter [Einrichten eines Labkontos](tutorial-setup-lab-account.md).


## <a name="configure-after-the-lab-is-created"></a>Konfigurieren nach Erstellen des Labs
Die gleiche Eigenschaft kann über die Registerkarte **Labkonfiguration** der Seite **Labkonto** aktiviert werden, wenn Sie zum Zeitpunkt der Erstellung des Labkontos kein Peernetzwerk eingerichtet haben. Änderungen an dieser Einstellung gelten nur für die Labs, die nach der Änderung erstellt werden. Wie Sie in der Abbildung sehen, können Sie die Option **Virtuelles Peernetzwerk** für Labs im Labkonto aktivieren oder deaktivieren. 

![Aktivieren oder Deaktivieren des VNET-Peerings nach dem Erstellen des Labs](../media/how-to-connect-peer-virtual-network/select-vnet-to-peer-existing-lab.png) 

Wenn Sie ein virtuelles Netzwerk für das Feld **Virtuelles Peernetzwerk** auswählen, ist die Option **Auswahl des Labspeicherorts durch Labersteller zulassen** deaktiviert. Der Grund dafür ist, dass sich die Labs im Labkonto in der gleichen Region befinden müssen wie das Labkonto, damit sie sich mit Ressourcen im virtuellen Peernetzwerk verbinden können. 

> [!IMPORTANT]
> Diese Einstellungsänderung gilt nur für Labs, die nach dem Durchführen der Änderung erstellt werden, nicht für vorhandene Labs. 


## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in folgenden Artikeln:

- [Zulassen der Auswahl des Labstandorts durch den Lab-Ersteller](allow-lab-creator-pick-lab-location.md)
- [Anfügen eines Katalogs mit freigegebenen Images an ein Lab](how-to-attach-detach-shared-image-gallery.md)
- [Hinzufügen eines Benutzers als Labbesitzer](how-to-add-user-lab-owner.md)
- [Anzeigen von Firewalleinstellungen für ein Lab](how-to-configure-firewall-settings.md)
- [Konfigurieren von anderen Einstellungen für ein Lab](how-to-configure-lab-accounts.md)