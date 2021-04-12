---
title: Konfigurieren der beiden Routingpräferenzoptionen für einen virtuellen Computer – Azure-Portal
description: Erfahren Sie, wie Sie beide Routingpräferenzoptionen aktivieren.
services: virtual-network
documentationcenter: na
author: KumudD
manager: mtillman
ms.service: virtual-network
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2021
ms.author: mnayak
ms.openlocfilehash: d9bb00077b1d96d13e15861cac477f913a002030
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "101678516"
---
# <a name="configure-both-routing-preference-options-for-a-virtual-machine"></a>Konfigurieren der beiden Routingpräferenzoptionen für einen virtuellen Computer

In diesem Artikel erfahren Sie, wie Sie die beiden [Routingpräferenzoptionen](./routing-preference-overview.md) (Internet und globales Microsoft-Netzwerk) für einen virtuellen Computer (VM) konfigurieren. Dies erfolgt durch die Verwendung von zwei virtuellen Netzwerkschnittstellen: eine Netzwerkschnittstelle mit einer öffentlichen IP-Adresse und Routing über das globale Microsoft-Netzwerk und eine Netzwerkschnittstelle mit einer öffentlichen IP-Adresse und Routing über das ISP-Netzwerk.

## <a name="prerequisites"></a>Voraussetzungen

Erstellen Sie im Azure-Portal einen virtuellen Computer mit einer öffentlichen IP-Adresse gemäß den Anweisungen in [Erstellen eines virtuellen Computers mit einer statischen öffentlichen IP-Adresse mithilfe des Azure-Portals](./virtual-network-deploy-static-pip-arm-portal.md). Die standardmäßige Routingpräferenzoption für die öffentliche IP-Adresse ist die Weiterleitung über das **globale Microsoft-Netzwerk**. Nachdem Sie einen virtuellen Computer mit einer öffentlichen IP-Adresse erstellt haben, fügen Sie ihm eine zweite öffentliche IP-Adresse hinzu, die den Datenverkehr über das ISP-Transitnetzwerk mit der als **Internet** konfigurierten Routingpräferenz weiterleitet.

## <a name="create-a-public-ip-address-with-a-routing-preference-choice-internet"></a>Erstellen einer öffentlichen IP-Adresse mit der Routingpräferenz „Internet“
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2. Wählen Sie **Ressource erstellen**. 
3. Geben Sie in das Suchfeld *Öffentliche IP-Adresse* ein.
4. Wählen Sie in den Suchergebnissen **Öffentliche IP-Adresse** aus. Wählen Sie anschließend auf der Seite **Öffentliche IP-Adressen** die Option **Erstellen** aus.
5. Wählen Sie **Internet** aus den Optionen für **Routingpräferenz** aus.

      ![Erstellen einer öffentlichen IP-Adresse](./media/routing-preference-portal/public-ip-new.png)

    > [!NOTE]
    > Öffentliche IP-Adressen werden mit einer IPv4- oder IPv6-Adresse erstellt. Die Routingpräferenz unterstützt derzeit jedoch nur IPv4.

## <a name="create-a-network-interface-and-associate-the-public-ip"></a>Erstellen einer Netzwerkschnittstelle und Zuordnen der öffentlichen IP-Adresse

1. Erstellen Sie im Azure-Portal eine [Netzwerkschnittstelle](./routing-preference-overview.md) mit Standardeinstellungen. 
1. [Ordnen Sie die öffentliche IP-Adresse der Netzwerkschnittstelle zu](./associate-public-ip-address-vm.md), die im vorherigen Abschnitt erstellt wurde. Möglicherweise dauert es einige Sekunden, bis eine IP-Adresse angezeigt wird. Zeigen Sie die öffentliche IP-Adresse an, die der IP-Konfiguration zugewiesen ist, wie in der folgenden Abbildung dargestellt:

    ![Zuordnen der öffentlichen IP-Adresse](./media/routing-preference-mixed-network-adapter-portal/public-ip.png)

## <a name="attach-network-interface-to-the-vm"></a>Anfügen der Netzwerkschnittstelle an den virtuellen Computer

1. [Fügen Sie die im vorherigen Abschnitt erstellte Netzwerkschnittstelle an den virtuellen Computer an](./virtual-network-network-interface-vm.md).
2. Sie können jetzt zwei virtuelle Netzwerkschnittstellen anzeigen, die dem virtuellen Computer zugeordnet sind: eine mit einer öffentlichen IP-Adresse, die über das globale Netzwerk von Microsoft weitergeleitet wird, und eine, die über ein ISP-Netzwerk weitergeleitet wird, wie unten dargestellt: ![Anfügen einer Netzwerkschnittstelle an eine VM](./media/routing-preference-mixed-network-adapter-portal/mixed-network-adapter.png) 

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über [öffentliche IP-Adressen mit Routingpräferenz](routing-preference-overview.md).
- [Konfigurieren der Routingpräferenz für einen virtuellen Computer](tutorial-routing-preference-virtual-machine-portal.md)
- [Konfigurieren der Routingpräferenz für eine öffentliche IP-Adresse mithilfe von PowerShell](routing-preference-powershell.md)
- Lesen Sie mehr über [öffentliche IP-Adressen](./public-ip-addresses.md#public-ip-addresses) in Azure.
- Erfahren Sie mehr über alle [Einstellungen für öffentliche IP-Adressen](virtual-network-public-ip-address.md#create-a-public-ip-address).