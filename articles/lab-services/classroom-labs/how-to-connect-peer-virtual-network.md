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
ms.date: 03/31/2020
ms.author: spelluru
ms.openlocfilehash: 9e53b6bdb041bfac5a82ed607b75b25ab0513f57
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82188003"
---
# <a name="connect-your-labs-network-with-a-peer-virtual-network-in-azure-lab-services"></a>Verbinden des Netzwerks Ihres Labs mit einem virtuellen Peernetzwerk in Azure Lab Services

Dieser Artikel bietet Informationen dazu, wie Sie eine Peerverbindung zwischen dem Netzwerk Ihres Labs und einem anderen Netzwerk herstellen.

## <a name="overview"></a>Übersicht

Das Peering virtueller Netzwerke ermöglicht es, virtuelle Azure-Netzwerke nahtlos miteinander zu verbinden. Nach dem Peering werden die virtuellen Netzwerke für Verbindungszwecke als einzelnes Element angezeigt. Der Datenverkehr zwischen virtuellen Computern in den virtuellen Netzwerken, die mittels Peering miteinander verbunden sind, wird ähnlich wie der Datenverkehr zwischen virtuellen Computern im gleichen Netzwerk nur über private IP-Adressen über die Microsoft-Backbone-Infrastruktur geleitet. Weitere Informationen finden Sie unter [Peering in virtuellen Netzwerken](../../virtual-network/virtual-network-peering-overview.md).

In einigen Szenarien, wie beispielsweise den folgenden, müssen Sie das Netzwerk Ihres Labs mit einem virtuellen Peernetzwerk verbinden:

- Auf den virtuellen Computern im Lab ist Software installiert, die eine Verbindung mit lokalen Lizenzservern herstellen muss, um eine Lizenz zu erhalten.
- Die virtuellen Computer im Lab benötigen Zugriff auf Datasets (oder andere Dateien) auf Freigaben im Netzwerk einer Universität.

Einige lokale Netzwerke sind über [ExpressRoute](../../expressroute/expressroute-introduction.md) oder ein [Gateway für virtuelle Netzwerke](../../vpn-gateway/vpn-gateway-about-vpngateways.md) mit Azure Virtual Network verbunden. Diese Dienste müssen außerhalb von Azure Lab Services eingerichtet werden. Weitere Informationen zum Herstellen einer Verbindung zwischen einem lokalen Netzwerk und Azure über ExpressRoute finden Sie unter [Übersicht über ExpressRoute](../../expressroute/expressroute-introduction.md). Für die lokale Verbindung über ein Gateway für virtuelle Netzwerke müssen sich das Gateway, das angegebene virtuelle Netzwerk und das Labkonto in der gleichen Region befinden.

> [!NOTE]
> Beim Erstellen eines Azure Virtual Network, für das Peering mit einem Lab-Konto verwendet wird, ist es wichtig zu verstehen, wie sich die Region des virtuellen Netzwerks auf den Ort der Erstellung von Classroom-Labs auswirkt.  Weitere Informationen finden Sie im Administratorleitfaden im Abschnitt zu [Regionen/Speicherorten](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#regionslocations).

## <a name="configure-at-the-time-of-lab-account-creation"></a>Konfigurieren zum Zeitpunkt der Labkontoerstellung

Während Sie ein [neues Labkonto erstellen](tutorial-setup-lab-account.md), können Sie ein vorhandenes virtuelles Netzwerk auswählen, das in der Dropdownliste **Virtuelles Peernetzwerk** auf der Registerkarte **Erweitert** angezeigt wird.  In der Liste werden ausschließlich virtuelle Netzwerke angezeigt, die sich in derselben Region befinden wie das Labkonto. Das ausgewählte virtuelle Netzwerk ist (mittels Peering) mit Labs verbunden, die im Labkonto erstellt wurden.  Alle virtuellen Computer in Labs, die nach Durchführen dieser Änderung erstellt werden, haben Zugriff auf die Ressourcen im virtuellen Peernetzwerk.

![Auswählen des virtuellen Netzwerks für das Peering](../media/how-to-connect-peer-virtual-network/select-vnet-to-peer.png)

### <a name="address-range"></a>Adressbereich

Es gibt auch eine Option, um einen **Adressbereich** für virtuelle Computer für die Labs bereitzustellen.  Die Eigenschaft **Adressbereich** ist nur verfügbar, wenn **Virtuelles Peernetzwerk** für das Lab aktiviert ist.  Wenn der Adressbereich bereitgestellt wird, werden alle virtuellen Computer in den Labs unter dem Lab-Konto in diesem Adressbereich erstellt. Der Adressbereich muss in CIDR-Notation (z. B. 10.20.0.0/20) angegeben werden und darf sich nicht mit vorhandenen Adressbereichen überschneiden.  Wenn Sie einen Adressbereich angeben, ist es wichtig, die Anzahl von *Labs* zu berücksichtigen, die erstellt werden sollen. Wählen Sie einen Adressbereich, der für die geplante Anzahl von Labs geeignet ist. Lab Services geht von maximal 512 virtuellen Computern pro Lab aus.  Bei einem IP-Bereich mit „/23“ kann beispielsweise nur ein Lab erstellt werden.  Ein Bereich mit „/21“ ermöglicht das Erstellen von vier Labs.

Wenn kein **Adressbereich** angegeben ist, verwendet Lab Services den standardmäßigen Adressbereich, der von Azure beim Erstellen des virtuellen Netzwerks bereitgestellt wird, das als Peernetzwerk Ihres virtuellen Netzwerks konfiguriert werden soll.  Für den Bereich ist häufig etwas wie „10.x.0.0/16“ angegeben.  Dies kann zu überlappenden IP-Bereichen führen. Geben Sie daher entweder einen Adressbereich in den Labeinstellungen an, oder überprüfen Sie den Adressbereich Ihres virtuellen Netzwerks, für das das Peering durchgeführt werden soll.

## <a name="configure-after-the-lab-is-created"></a>Konfigurieren nach Erstellen des Labs

Die gleiche Eigenschaft kann über die Registerkarte **Labkonfiguration** der Seite **Labkonto** aktiviert werden, wenn Sie zum Zeitpunkt der Erstellung des Labkontos kein Peernetzwerk eingerichtet haben. Änderungen an dieser Einstellung gelten nur für die Labs, die nach der Änderung erstellt werden. Wie Sie in der Abbildung sehen, können Sie die Option **Virtuelles Peernetzwerk** für Labs im Labkonto aktivieren oder deaktivieren.

![Aktivieren oder Deaktivieren des VNET-Peerings nach dem Erstellen des Labs](../media/how-to-connect-peer-virtual-network/select-vnet-to-peer-existing-lab.png)

Wenn Sie ein virtuelles Netzwerk für das Feld **Virtuelles Peernetzwerk** auswählen, ist die Option **Auswahl des Labspeicherorts durch Labersteller zulassen** deaktiviert. Der Grund dafür ist, dass sich die Labs im Labkonto in der gleichen Region befinden müssen wie das Labkonto, damit sie sich mit Ressourcen im virtuellen Peernetzwerk verbinden können.

> [!IMPORTANT]
> Die Einstellung für das virtuelle Peernetzwerk gilt nur für Labs, die nach dem Durchführen der Änderung erstellt werden, nicht für vorhandene Labs.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in folgenden Artikeln:

- [Zulassen der Auswahl des Labstandorts durch den Lab-Ersteller](allow-lab-creator-pick-lab-location.md)
- [Anfügen eines Katalogs mit freigegebenen Images an ein Lab](how-to-attach-detach-shared-image-gallery.md)
- [Hinzufügen eines Benutzers als Labbesitzer](how-to-add-user-lab-owner.md)
- [Anzeigen von Firewalleinstellungen für ein Lab](how-to-configure-firewall-settings.md)
- [Konfigurieren von anderen Einstellungen für ein Lab](how-to-configure-lab-accounts.md)
