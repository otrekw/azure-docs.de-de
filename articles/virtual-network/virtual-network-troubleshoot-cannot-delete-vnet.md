---
title: Ein virtuelles Netzwerk in Azure kann nicht gelöscht werden | Microsoft-Dokumentationen
description: In diesem Artikel erfahren Sie, was Sie tun können, wenn ein virtuelles Netzwerk in Azure nicht gelöscht werden kann.
services: virtual-network
documentationcenter: na
author: chadmath
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: c2152b872c82c224c786e56db0318c9df994ac25
ms.sourcegitcommit: c8a0fbfa74ef7d1fd4d5b2f88521c5b619eb25f8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/05/2020
ms.locfileid: "82801585"
---
# <a name="troubleshooting-failed-to-delete-a-virtual-network-in-azure"></a>Problembehandlung: Fehler beim Löschen eines virtuellen Netzwerks in Azure.
<p class="alert is-flex is-primary"><span class="has-padding-left-medium has-padding-top-extra-small"><a class="button is-primary" href="https://azurevirtualsupportagent.services.microsoft.com?content=a13a0908-28e0-f9f7-c6c7-301fcd187560" target='_blank'>Start</a></span><span class="has-padding-small">Lösen Sie Ihr Problem schnell mithilfe unseres virtuellen Agents zum Ausführen der <b>automatisierten Diagnose</b>.</span><span class="has-padding-small"><a href="https://privacy.microsoft.com/privacystatement" target='_blank'><div align="right"><sub>Datenschutzerklärung</sub></div></a></span></p>
Wenn Sie versuchen, in Microsoft Azure ein virtuelles Netzwerk zu löschen, können Fehler auftreten. Dieser Artikel enthält Schritte, mit denen Sie dieses Problem beheben können. 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="troubleshooting-guidance"></a>Anleitungen zur Problembehandlung 

1. [Überprüfen, ob im virtuellen Netzwerk ein virtuelles Netzwerkgateway ausgeführt wird](#check-whether-a-virtual-network-gateway-is-running-in-the-virtual-network)
2. [Überprüfen, ob im virtuellen Netzwerk ein Anwendungsgateway ausgeführt wird](#check-whether-an-application-gateway-is-running-in-the-virtual-network)
3. [Überprüfen, ob im virtuellen Netzwerk Azure Active Directory Domain Services aktiviert ist](#check-whether-azure-active-directory-domain-service-is-enabled-in-the-virtual-network)
4. [Überprüfen, ob das virtuelle Netzwerk mit anderen Ressourcen verbunden ist](#check-whether-the-virtual-network-is-connected-to-other-resource)
5. [Überprüfen, ob im virtuellen Netzwerk noch ein virtueller Computer ausgeführt wird](#check-whether-a-virtual-machine-is-still-running-in-the-virtual-network)
6. [Überprüfen, ob das virtuelle Netzwerk im Migrationsprozess hängengeblieben ist](#check-whether-the-virtual-network-is-stuck-in-migration)

## <a name="troubleshooting-steps"></a>Schritte zur Problembehandlung

### <a name="check-whether-a-virtual-network-gateway-is-running-in-the-virtual-network"></a>Überprüfen, ob im virtuellen Netzwerk ein virtuelles Netzwerkgateway ausgeführt wird

Um das virtuelle Netzwerk zu entfernen, müssen Sie zunächst das Gateway des virtuellen Netzwerks entfernen.

Für klassische virtuelle Netzwerke wechseln Sie im Azure-Portal auf die **Übersichtsseite** des klassischen virtuellen Netzwerks. Wenn das Gateway im virtuellen Netzwerk ausgeführt wird, wird im Abschnitt **VPN-Verbindungen** die IP-Adresse des Gateways angezeigt. 

![Überprüfen Sie, ob das Gateway ausgeführt wird](media/virtual-network-troubleshoot-cannot-delete-vnet/classic-gateway.png)

Für virtuelle Netzwerke wechseln Sie zur **Übersichtsseite** des virtuellen Netzwerks. Überprüfen Sie **Verbundene Geräte** für das Gateway des virtuellen Netzwerks.

![Überprüfen des verbundenen Geräts](media/virtual-network-troubleshoot-cannot-delete-vnet/vnet-gateway.png)

Bevor Sie das Gateway entfernen können, müssen Sie zunächst alle im Gateway vorhandenen **Verbindungsobjekte** entfernen. 

### <a name="check-whether-an-application-gateway-is-running-in-the-virtual-network"></a>Überprüfen, ob im virtuellen Netzwerk ein Anwendungsgateway ausgeführt wird

Wechseln Sie zur **Übersichtsseite** des virtuellen Netzwerks. Überprüfen Sie **Verbundene Geräte** für das Anwendungsgateway.

![Überprüfen des verbundenen Geräts](media/virtual-network-troubleshoot-cannot-delete-vnet/app-gateway.png)

Wenn ein Anwendungsgateway vorhanden ist, müssen Sie dieses entfernen, bevor Sie das virtuelle Netzwerk löschen können.

### <a name="check-whether-azure-active-directory-domain-service-is-enabled-in-the-virtual-network"></a>Überprüfen, ob im virtuellen Netzwerk Azure Active Directory Domain Services aktiviert ist

Wenn Active Directory Domain Services aktiviert und mit dem virtuellen Netzwerk verbunden ist, können sie dieses virtuelle Netzwerk nicht löschen. 

![Überprüfen des verbundenen Geräts](media/virtual-network-troubleshoot-cannot-delete-vnet/enable-domain-services.png)

Informationen zum Deaktivieren des Service finden Sie unter [Deaktivieren von Azure Active Directory Domain Services mithilfe des Azure-Portals](../active-directory-domain-services/delete-aadds.md).

### <a name="check-whether-the-virtual-network-is-connected-to-other-resource"></a>Überprüfen, ob das virtuelle Netzwerk mit anderen Ressourcen verbunden ist

Suchen Sie nach Verbindungslinks, Verbindungen und virtuelles Netzwerk über Peerings. Dies können die Ursachen dafür sein, dass ein virtuelles Netzwerk nicht gelöscht werden kann. 

Die empfohlene Löschreihenfolge lautet wie folgt:

1. Gatewayverbindungen
2. Gateways
3. IP-Adressen
4. Virtuelles Netzwerk über Peerings
5. App Service-Umgebung (ASE)

### <a name="check-whether-a-virtual-machine-is-still-running-in-the-virtual-network"></a>Überprüfen, ob im virtuellen Netzwerk noch ein virtueller Computer ausgeführt wird

Stellen Sie sicher, dass sich kein virtueller Computer im virtuellen Netzwerk befindet.

### <a name="check-whether-the-virtual-network-is-stuck-in-migration"></a>Überprüfen, ob das virtuelle Netzwerk im Migrationsprozess hängengeblieben ist

Wenn das virtuelle Netzwerk im Migrationszustand hängengeblieben ist, kann es nicht gelöscht werden. Führen Sie den folgenden Befehl aus, um die Migration abzubrechen, und löschen Sie dann das virtuelle Netzwerk.

    Move-AzureVirtualNetwork -VirtualNetworkName "Name" -Abort

## <a name="next-steps"></a>Nächste Schritte

- [Azure Virtual Network](virtual-networks-overview.md)
- [Azure Virtual Network – häufig gestellte Fragen](virtual-networks-faq.md)
