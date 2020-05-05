---
title: 'Erstellen eines virtuellen Netzwerks: Schnellstart – Azure-Portal'
titleSuffix: Azure Virtual Network
description: 'Schnellstart: Erstellen Sie ein virtuelles Netzwerk über das Azure-Portal. Über diese Netzwerke können Azure-Ressourcen wie etwa virtuelle Computer sicher miteinander und mit dem Internet kommunizieren.'
author: KumudD
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/05/2020
ms.author: kumud
ms.openlocfilehash: 1a1593566b8bdb72f322d64c1ee99c7018f49329
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "79214788"
---
# <a name="quickstart-create-a-virtual-network-using-the-azure-portal"></a>Schnellstart: Erstellen eines virtuellen Netzwerks im Azure-Portal

In dieser Schnellstartanleitung erfahren Sie, wie Sie über das Azure-Portal ein virtuelles Netzwerk erstellen. Dazu stellen Sie zwei virtuelle Computer (virtual machines, VMs) bereit. Anschließend nutzen Sie die sichere Kommunikation zwischen virtuellen Computern und stellen über das Internet eine Verbindung mit virtuellen Computern her. Ein virtuelles Netzwerk ist der grundlegende Baustein für Ihr privates Netzwerk in Azure. Über dieses Netzwerk können Azure-Ressourcen wie etwa virtuelle Computer sicher miteinander und mit dem Internet kommunizieren.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Konto mit einem aktiven Abonnement. [Erstellen Sie ein kostenloses Konto.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

## <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

1. Wählen Sie im Menü des Azure-Portals die Option **Ressource erstellen** aus. Wählen Sie im Azure Marketplace die Option **Netzwerk** > **Virtuelles Netzwerk** aus.

1. Geben Sie in **Virtuelles Netzwerk erstellen** diese Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | Subscription | Wählen Sie Ihr Abonnement aus.|
    | Resource group | Wählen Sie **Neue erstellen** aus, geben Sie *myResourceGroup* ein, und wählen Sie **OK** aus. |
    | Name | Geben Sie *myVirtualNetwork* ein. |
    | Standort | Wählen Sie **USA, Osten** aus.|

1. Klicken Sie auf **Weiter: IP-Adressen** aus, und geben Sie unter **IPv4-Adressraum** den Adressraum *10.1.0.0/16* ein.

1. Wählen Sie **Subnetz hinzufügen** aus, und geben Sie unter **Subnetzname** den Namen *myVirtualSubnet* und unter **Subnetzadressbereich** den Bereich *10.1.0.0/24* ein.

1. Wählen Sie **Hinzufügen** und anschließend **Überprüfen + erstellen** aus. Übernehmen Sie die übrigen Standardeinstellungen, und wählen Sie **Erstellen** aus.

1. Wählen Sie unter **Virtuelles Netzwerk erstellen** die Option **Erstellen** aus.

## <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern

Erstellen Sie zwei virtuelle Computer im virtuellen Netzwerk:

### <a name="create-the-first-vm"></a>Erstellen des ersten virtuellen Computers

1. Wählen Sie im Menü des Azure-Portals die Option **Ressource erstellen** aus.

1. Wählen Sie im Azure Marketplace die Option **Compute** > **Windows Server 2019 Datacenter** aus. Klicken Sie auf **Erstellen**.

1. Geben Sie in **Virtuellen Computer erstellen – Grundlagen** diese Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | **Projektdetails** | |
    | Subscription | Wählen Sie Ihr Abonnement aus. |
    | Resource group | Wählen Sie **myResourceGroup** aus. Diese Ressourcengruppe wurde im vorherigen Abschnitt erstellt. |
    | **Instanzendetails** |  |
    | Name des virtuellen Computers | Geben Sie *myVm1* ein. |
    | Region | Wählen Sie **USA, Osten** aus. |
    | Verfügbarkeitsoptionen | Verwenden Sie die Standardeinstellung **Keine Infrastrukturredundanz erforderlich**. |
    | Image | Verwenden Sie die Standardeinstellung **Windows Server 2019 Datacenter**. |
    | Size | Verwenden Sie die Standardeinstellung **Standard DS1 v2**. |
    | **Administratorkonto** |  |
    | Username | Geben Sie einen Benutzernamen Ihrer Wahl ein. |
    | Kennwort | Geben Sie das gewünschte Kennwort ein. Das Kennwort muss mindestens zwölf Zeichen lang sein und die [definierten Anforderungen an die Komplexität](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) erfüllen.|
    | Kennwort bestätigen | Geben Sie das Kennwort erneut ein. |
    | **Regeln für eingehende Ports** |  |
    | Öffentliche Eingangsports | Wählen Sie **Ausgewählte Ports zulassen** aus. |
    | Eingangsports auswählen | Geben Sie *HTTP (80)* und *RDP (3389)* ein. |
    | **Sparen Sie Geld** |  |
    | Windows-Lizenz bereits vorhanden? | Verwenden Sie die Standardeinstellung **Nein**. |

1. Klicken Sie auf **Weiter: Datenträger**.

1. Übernehmen Sie unter **Virtuellen Computer erstellen – Datenträger** die Standardwerte, und wählen Sie **Weiter: Netzwerk**.

1. Wählen Sie in **Virtuellen Computer erstellen – Netzwerk** diese Informationen aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | Virtuelles Netzwerk | Verwenden Sie die Standardeinstellung **myVirtualNetwork**. |
    | Subnet | Verwenden Sie die Standardeinstellung **myVirtualSubnet (10.1.0.0/24)** . |
    | Öffentliche IP-Adresse | Verwenden Sie die Standardeinstellung **(neu) myVm-ip**. |
    | NIC-Netzwerksicherheitsgruppe | Verwenden Sie die Standardeinstellung **Basic**. |
    | Öffentliche Eingangsports | Verwenden Sie die Standardeinstellung **Ausgewählte Ports zulassen**. |
    | Eingangsports auswählen | Verwenden Sie die Standardeinstellungen **HTTP** und **RDP**.

1. Klicken Sie auf **Weiter: Verwaltung** aus.

1. Wählen Sie in **Virtuellen Computer erstellen – Verwaltung** für **Diagnosespeicherkonto** **Neu erstellen** aus.

1. Geben Sie in **Speicherkonto erstellen** diese Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | Name | Geben Sie *myvmstorageaccount* ein. Wenn dieser Name vergeben ist, erstellen Sie einen eindeutigen Namen.|
    | Kontoart | Verwenden Sie die Standardeinstellung **Storage (universell, Version 1)** . |
    | Leistung | Verwenden Sie die Standardeinstellung **Standard**. |
    | Replikation | Verwenden Sie die Standardeinstellung **Lokal redundanter Speicher (LRS)** . |

1. Wählen Sie **OK** und anschließend **Überprüfen + erstellen** aus. Sie werden zur Seite **Überprüfen und erstellen** weitergeleitet, auf der Azure Ihre Konfiguration überprüft.

1. Wenn die Meldung **Überprüfung erfolgreich** angezeigt wird, wählen Sie **Erstellen** aus.

### <a name="create-the-second-vm"></a>Erstellen des zweiten virtuellen Computers

Wiederholen Sie die Schritte des vorherigen Abschnitts, um einen weiteren virtuellen Computer zu erstellen.

> [!IMPORTANT]
> Geben Sie als **Name des virtuellen Computers** den Namen *myVm2* ein.
>
> Wählen Sie unter **Diagnosespeicherkonto** die Option **myvmstorageaccount** aus, anstatt eines zu erstellen.

## <a name="connect-to-a-vm-from-the-internet"></a>Herstellen einer Verbindung mit einem virtuellen Computer über das Internet

Nachdem Sie *myVm1* erstellt haben, stellen Sie eine Verbindung mit dem Internet her.

1. Suchen Sie im Azure-Portal nach dem virtuellen Computer *myVm1*, und wählen Sie ihn aus.

1. Wählen Sie **Verbinden** und anschließend **RDP** aus.

    ![Herstellen einer Verbindung mit einem virtuellen Computer](./media/quick-create-portal/connect-to-virtual-machine.png)

    Die Seite **Verbinden** wird geöffnet.

1. Wählen Sie **RDP-Datei herunterladen** aus. Azure erstellt eine Remotedesktopprotokoll-Datei (*RDP*) und lädt sie auf Ihren Computer herunter.

1. Öffnen Sie die RDP-Datei. Wenn Sie dazu aufgefordert werden, wählen Sie **Verbinden** aus.

1. Geben Sie den Benutzernamen und das Kennwort ein, den/das Sie beim Erstellen des virtuellen Computers angegeben haben.

    > [!NOTE]
    > Unter Umständen müssen Sie **Weitere Optionen** > **Anderes Konto verwenden** auswählen, um die Anmeldeinformationen anzugeben, die Sie beim Erstellen des virtuellen Computers eingegeben haben.

1. Klicken Sie auf **OK**.

1. Bei der Anmeldung wird ggf. eine Zertifikatwarnung angezeigt. Wenn Sie eine Zertifikatwarnung erhalten, wählen Sie **Ja** oder **Weiter** aus.

1. Sobald der VM-Desktop angezeigt wird, minimieren Sie ihn, um zu Ihrem lokalen Desktop zurückzukehren.

## <a name="communicate-between-vms"></a>Kommunikation zwischen VMs

1. Öffnen Sie auf dem Remote Desktop von *myVm1* PowerShell.

1. Geben Sie `ping myVm2` ein.

    Sie erhalten eine Meldung wie die folgende:

    ```output
    Pinging myVm2.0v0zze1s0uiedpvtxz5z0r0cxg.bx.internal.clouda
    Request timed out.
    Request timed out.
    Request timed out.
    Request timed out.

    Ping statistics for 10.1.0.5:
    Packets: Sent = 4, Received = 0, Lost = 4 (100% loss),
    ```

    Bei `ping` tritt ein Fehler auf, weil `ping` das Internet Control Message-Protokoll (ICMP) verwendet. Standardmäßig kann ICMP nicht durch die Windows-Firewall ausgeführt werden.

1. Damit *myVm2* in einem späteren Schritt ein Pingsignal an *myVm1* senden kann, geben Sie diesen Befehl ein:

    ```powershell
    New-NetFirewallRule –DisplayName "Allow ICMPv4-In" –Protocol ICMPv4
    ```

    Dieser Befehl lässt eingehenden ICMP-Datenverkehr durch die Windows-Firewall zu:

1. Schließen Sie die Remotedesktopverbindung mit *myVm1*.

1. Führen Sie die Schritte unter [Herstellen einer Verbindung mit einem virtuellen Computer über das Internet](#connect-to-a-vm-from-the-internet) erneut aus, stellen Sie jedoch die Verbindung mit *myVm2* her.

1. Geben Sie an einer Befehlszeile `ping myvm1` ein.

    Es wird etwa diese Meldung angezeigt:

    ```output
    Pinging myVm1.0v0zze1s0uiedpvtxz5z0r0cxg.bx.internal.cloudapp.net [10.1.0.4] with 32 bytes of data:
    Reply from 10.1.0.4: bytes=32 time=1ms TTL=128
    Reply from 10.1.0.4: bytes=32 time<1ms TTL=128
    Reply from 10.1.0.4: bytes=32 time<1ms TTL=128
    Reply from 10.1.0.4: bytes=32 time<1ms TTL=128

    Ping statistics for 10.1.0.4:
        Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 0ms, Maximum = 1ms, Average = 0ms
    ```

    Sie erhalten Antworten von *myVm1*, weil Sie in Schritt 3 ICMP-Datenverkehr durch die Windows-Firewall auf dem virtuellen Computer *myVm1* zugelassen haben.

1. Schließen Sie die Remotedesktopverbindung mit *myVm2*.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

In dieser Schnellstartanleitung haben Sie ein virtuelles Standardnetzwerk und zwei virtuelle Computer erstellt. Sie haben über das Internet eine Verbindung mit einem der virtuellen Computer hergestellt und sicher zwischen den beiden virtuellen Computern kommuniziert.

Wenn Sie Ihre Arbeit mit dem virtuellen Netzwerk und den virtuellen Computern abgeschlossen haben, löschen Sie die Ressourcengruppe und alle darin enthaltenen Ressourcen:

1. Suchen Sie nach der Ressourcengruppe *myResourceGroup*, und wählen Sie sie aus.

1. Wählen Sie die Option **Ressourcengruppe löschen**.

1. Geben Sie *myResourceGroup* für **RESSOURCENGRUPPENNAMEN EINGEBEN** ein, und wählen Sie **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu VNET-Einstellungen finden Sie unter [Erstellen, Ändern oder Löschen eines virtuellen Netzwerks](manage-virtual-network.md).

Azure ermöglicht standardmäßig eine sichere Kommunikation zwischen virtuellen Computern. Azure lässt nur eingehende Remotedesktopverbindungen mit virtuellen Windows-Computern über das Internet zu. Weitere Informationen zu verschiedenen Arten der VM-Netzwerkkommunikation finden Sie unter [Tutorial: Filtern von Netzwerkdatenverkehr mithilfe einer Netzwerksicherheitsgruppe über das Azure-Portal](tutorial-filter-network-traffic.md).
