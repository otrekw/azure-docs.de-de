---
title: Verbindung mit Azure-VMs kann nicht hergestellt werden, da der RDP-Port in der NSG nicht aktiviert ist | Microsoft-Dokumentation
description: Beschreibung der Behandlung eines Problems, bei dem für RDP aufgrund der NSG-Konfiguration im Azure-Portal ein Fehler auftritt | Microsoft-Dokumentation
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: dcscontentpm
editor: v-jesits
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 11/20/2018
ms.author: genli
ms.openlocfilehash: 6c877690cf27edd73f1d828b8a1dda6f4f34e780
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "77918171"
---
#  <a name="cannot-connect-remotely-to-a-vm-because-rdp-port-is-not-enabled-in-nsg"></a>Remoteverbindung mit einer VM kann nicht hergestellt werden, da der RDP-Port in der NSG nicht aktiviert ist

In diesem Artikel wird die Lösung eines Problems beschrieben, bei dem Sie keine Verbindung mit einem virtuellen Azure-Computer (VM) unter Windows herstellen können, weil der RDP-Port (Remotedesktopprotokoll) in der Netzwerksicherheitsgruppe (NSG) nicht aktiviert ist.


## <a name="symptom"></a>Symptom

Sie können keine RDP-Verbindung mit einer VM in Azure herstellen, weil der RDP-Port in der Netzwerksicherheitsgruppe nicht geöffnet ist.

## <a name="solution"></a>Lösung 

Wenn Sie einen neuen virtuellen Computer erstellen, wird der gesamte Datenverkehr aus dem Internet standardmäßig blockiert. 

Führen Sie diese Schritte aus, um den RDP-Port in einer NSG zu aktivieren:
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie unter **Virtuelle Computer** die VM aus, für die das Problem besteht. 
3. Wählen Sie unter **Einstellungen** die Option **Netzwerk**. 
4. Überprüfen Sie unter **Regeln für eingehende Ports**, ob der Port für RDP richtig festgelegt ist. Hier ist ein Beispiel für die Konfiguration angegeben: 

    **Priorität:** 300 </br>
    **Name**: Port_3389 </br>
    **Port(Ziel)** : 3389 </br>
    **Protokoll:** TCP </br>
    **Quelle**: Any </br>
    **Ziele:** Any </br>
    **Aktion:** Allow </br>

Wenn Sie die IP-Quelladresse angeben, wird mit dieser Einstellung nur Datenverkehr von einer bestimmten IP-Adresse bzw. einem IP-Adressbereich für die Verbindung mit der VM zugelassen. Stellen Sie sicher, dass sich der Computer, den Sie zum Starten der RDP-Sitzung verwenden, in diesem Bereich befindet.

Weitere Informationen zu Netzwerksicherheitsgruppen finden Sie unter [Netzwerksicherheitsgruppe](../../virtual-network/security-overview.md).

> [!NOTE]
> RDP-Port 3389 wird für das Internet verfügbar gemacht. Daher empfehlen wir Ihnen, diesen Port nur für Testzwecke zu nutzen. Für Produktionsumgebungen empfehlen wir die Verwendung einer VPN- oder privaten Verbindung.

## <a name="next-steps"></a>Nächste Schritte

Falls der RDP-Port in der Netzwerksicherheitsgruppe bereits aktiviert ist, helfen Ihnen die Informationen unter [Beheben eines allgemeinen RDP-Fehlers auf einer Azure-VM](./troubleshoot-rdp-general-error.md) weiter.



