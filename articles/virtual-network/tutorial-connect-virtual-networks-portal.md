---
title: 'Herstellen von Verbindungen zwischen virtuellen Netzwerken mittels VNET-Peering – Tutorial: Azure-Portal'
description: In diesem Tutorial erfahren Sie, wie Sie im Azure-Portal durch Peering virtueller Netzwerke eine Verbindung zwischen virtuellen Netzwerken herstellen.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 07/06/2021
ms.author: kumud
ms.custom: ''
ms.openlocfilehash: ca8829a5f71dabfe33f013c354e083cae08e4566
ms.sourcegitcommit: beff1803eeb28b60482560eee8967122653bc19c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/07/2021
ms.locfileid: "113430630"
---
# <a name="tutorial-connect-virtual-networks-with-virtual-network-peering-using-the-azure-portal"></a>Tutorial: Herstellen einer Verbindung zwischen virtuellen Netzwerken mittels Peering über das Azure-Portal

Sie können durch Peering virtueller Netzwerke Verbindungen zwischen virtuellen Netzwerken herstellen. Diese virtuellen Netzwerke können sich in derselben oder in unterschiedlichen Regionen (auch als globales VNET-Peering bezeichnet) befinden. Sobald ein Peering zwischen virtuellen Netzwerken eingerichtet wurde, können Ressourcen in beiden virtuellen Netzwerken untereinander mit der gleichen Latenz und Bandbreite kommunizieren, als befänden sie sich im selben virtuellen Netzwerk. In diesem Tutorial lernen Sie, wie die folgenden Aufgaben ausgeführt werden:

> [!div class="checklist"]
> * Erstellen zweier virtueller Netzwerke
> * Herstellen einer Verbindung zwischen zwei virtuellen Netzwerken mit einem Peering virtueller Netzwerke
> * Bereitstellen eines virtuellen Computers (VM) in jedem virtuellen Netzwerk
> * Kommunikation zwischen VMs

Dieser Artikel kann auch mit der [Azure-Befehlszeilenschnittstelle](tutorial-connect-virtual-networks-cli.md) oder mit [Azure PowerShell](tutorial-connect-virtual-networks-powershell.md) durchgearbeitet werden.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie beginnen, müssen Sie über ein Azure-Konto mit einem aktiven Abonnement verfügen. Falls Sie über keins verfügen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="create-virtual-networks"></a>Erstellen virtueller Netzwerke

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Wählen Sie im Azure-Portal die Option **+ Ressource erstellen** aus.

1. Suchen Sie nach **Virtuelles Netzwerk** und wählen Sie dann **Erstellen** aus.

    :::image type="content" source="./media/tutorial-connect-virtual-networks-portal/create-vnet.png" alt-text="Screenshot: Erstellen einer Ressource für das virtuelle Netzwerk":::

1. Geben Sie auf der Registerkarte **Grundlagen** die folgenden Informationen ein, oder wählen Sie sie aus, und übernehmen Sie die Standardwerte für die übrigen Einstellungen:

    |Einstellung|Wert|
    |---|---|
    |Subscription| Wählen Sie Ihr Abonnement aus.|
    |Resource group| Klicken Sie auf **Neu erstellen**, und geben Sie *myResourceGroup* ein.|
    |Region| Wählen Sie **USA, Osten** aus.|
    |Name|myVirtualNetwork1|

    :::image type="content" source="./media/tutorial-connect-virtual-networks-portal/create-basic-tab.png" alt-text="Screenshot der Registerkarte „Grundlagen“ zum Erstellen eines virtuellen Netzwerks":::

1. Geben Sie auf der Registerkarte **IP-Adressen** im Feld **Adressbereich** die Adresse *10.0.0.0/16* ein. Klicken Sie unten auf die Schaltfläche **Subnetz hinzufügen**, und geben Sie *Subnet1* für **Subnetzname** und *10.0.0.0/24* für **Subnetzadressbereich** ein.

    :::image type="content" source="./media/tutorial-connect-virtual-networks-portal/ip-addresses-tab.png" alt-text="Screenshot der Registerkarte „IP-Adressen“ zum Erstellen eines virtuellen Netzwerks":::

1. Klicken Sie auf **Überprüfen und erstellen** und dann auf **Erstellen**.
   
1. Wiederholen Sie die Schritte 1 bis 5, um ein zweites virtuelles Netzwerk mit den folgenden Einstellungen zu erstellen:

    | Einstellung | Wert |
    | --- | --- |
    | Name | myVirtualNetwork2 |
    | Adressraum | 10.1.0.0/16 |
    | Ressourcengruppe | Wählen Sie **Vorhandene verwenden** und dann **myResourceGroup**.|
    | Subnetzname | Subnet2 |
    | Subnetzadressbereich | 10.1.0.0/24 |

## <a name="peer-virtual-networks"></a>Einrichten eines Peerings von virtuellen Netzwerken

1. Suchen Sie oben im Azure-Portal über das Suchfeld nach *myVirtualNetwork1*. Wenn **myVirtualNetwork1** in den Suchergebnissen angezeigt wird, wählen Sie es aus.

    :::image type="content" source="./media/tutorial-connect-virtual-networks-portal/search-vnet.png" alt-text="Screenshot der Suche nach myVirtualNetwork1":::

1. Klicken Sie unter **Einstellungen** auf **Peerings**, und wählen Sie dann **+ Hinzufügen** aus, wie in der folgenden Abbildung gezeigt wird:

    :::image type="content" source="./media/tutorial-connect-virtual-networks-portal/create-peering.png" alt-text="Screenshot: Erstellen von Peerings für myVirtualNetwork1":::
    
1. Geben Sie die folgenden Informationen ein, oder wählen Sie sie aus, übernehmen Sie die Standardwerte für die übrigen Einstellungen, und klicken Sie auf **Hinzufügen**.

    | Einstellung | Wert |
    | --- | --- |
    | Dieses virtuelle Netzwerk - Name des Peeringlinks | Name des Peerings von myVirtualNetwork1 zum virtuellen Remotenetzwerk.  Für diese Verbindung wird *myVirtualNetwork1-myVirtualNetwork2* verwendet.|
    | Virtuelles Remote-Netzwerk - Name des Peeringlinks |  Name des Peerings vom virtuellen Remotenetzwerk zu myVirtualNetwork1. Für diese Verbindung wird *myVirtualNetwork2-myVirtualNetwork1* verwendet. |
    | Subscription | Wählen Sie Ihr Abonnement aus.|
    | Virtuelles Netzwerk  | Sie können ein virtuelles Netzwerk in derselben Region oder in einer anderen Region auswählen. Wählen Sie in der Dropdownliste *myVirtualNetwork2* aus. |

    :::image type="content" source="./media/tutorial-connect-virtual-networks-portal/peering-settings-bidirectional.png" alt-text="Screenshot: Konfiguration des Peerings virtueller Netzwerke" lightbox="./media/tutorial-connect-virtual-networks-portal/peering-settings-bidirectional-expanded.png":::

    Der **PEERINGSTATUS** lautet *Verbunden*, wie in der folgenden Abbildung gezeigt wird:

    :::image type="content" source="./media/tutorial-connect-virtual-networks-portal/peering-status-connected.png" alt-text="Screenshot: Verbindungsstatus des Peerings virtueller Netzwerke":::

    Wenn der Status *Verbunden* nicht angezeigt wird, klicken Sie auf die Schaltfläche **Aktualisieren**.

## <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern

Erstellen Sie eine VM in jedem virtuellen Netzwerk, damit Sie die Kommunikation zwischen den VMs testen können.

### <a name="create-the-first-vm"></a>Erstellen des ersten virtuellen Computers

1. Wählen Sie im Azure-Portal die Option **+ Ressource erstellen** aus.

1. Wählen Sie **Compute** und dann unter *Virtueller Computer* die Option **Erstellen** aus.

    :::image type="content" source="./media/tutorial-connect-virtual-networks-portal/create-vm.png" alt-text="Screenshot: Erstellen einer Ressource für VMs":::

1. Geben Sie die folgenden Informationen in der Registerkarte **Grundlagen** ein, oder wählen Sie sie aus, übernehmen Sie die Standardwerte für die übrigen Einstellungen, und klicken Sie auf **Erstellen**:

    | Einstellung | Wert |
    | --- | --- |
    | Ressourcengruppe| Wählen Sie **Vorhandene verwenden** und dann **myResourceGroup**. |
    | Name | myVm1 |
    | Standort | Wählen Sie **USA, Osten** aus. |
    | Image | Wählen Sie ein Betriebssystemimage aus. Für diese VM wurde *Windows Server 2019 Datacenter – Gen1* ausgewählt. |
    | Size | Wählen Sie eine VM-Größe aus. Für diese VM wurde *Standard_D2s_v3* ausgewählt. |
    | Username | Geben Sie einen Benutzernamen ein. Für dieses Beispiel wurde der Benutzername *azure* ausgewählt. |
    | Kennwort | Geben Sie das gewünschte Kennwort ein. Das Kennwort muss mindestens zwölf Zeichen lang sein und die [definierten Anforderungen an die Komplexität](../virtual-machines/windows/faq.yml?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm-) erfüllen. |
   
    :::image type="content" source="./media/tutorial-connect-virtual-networks-portal/create-vm-basic-tab.png" alt-text="Screenshot: Konfiguration der Grundlagen-Registerkarte einer VM" lightbox="./media/tutorial-connect-virtual-networks-portal/create-vm-basic-tab-expanded.png":::

1. Wählen Sie auf der Registerkarte **Netzwerk** die folgenden Werte aus:

    | Einstellung | Wert |
    | --- | --- |
    | Virtuelles Netzwerk | myVirtualNetwork1: Falls dieses Element noch nicht ausgewählt ist, wählen Sie die Option **Virtuelles Netzwerk** und dann **myVirtualNetwork1** aus. |
    | Subnet | Subnet1: Falls dieses Element noch nicht ausgewählt ist, wählen Sie die Option **Subnetz** und dann **Subnet1** aus. |
    | Öffentliche Eingangsports | *Ausgewählte Ports zulassen* |
    | Eingangsports auswählen | *RDP (3389)* |

    :::image type="content" source="./media/tutorial-connect-virtual-networks-portal/create-vm-networking-tab.png" alt-text="Screenshot: Konfiguration der Netzwerk-Registerkarte einer VM" lightbox="./media/tutorial-connect-virtual-networks-portal/create-vm-networking-tab-expanded.png":::

1. Wählen Sie **Überprüfen + Erstellen** und dann **Erstellen** aus, um die VM-Bereitstellung zu starten.

### <a name="create-the-second-vm"></a>Erstellen des zweiten virtuellen Computers

Wiederholen Sie die Schritte 1 bis 6, um eine zweite VM mit den folgenden Änderungen zu erstellen:

| Einstellung | Wert |
| --- | --- |
| Name | myVm2 |
| Virtuelles Netzwerk | myVirtualNetwork2 |

Die Erstellung der VMs kann einige Minuten dauern. Fahren Sie mit den restlichen Schritten erst fort, nachdem beide VMs erstellt wurden.

[!INCLUDE [ephemeral-ip-note.md](../../includes/ephemeral-ip-note.md)]

## <a name="communicate-between-vms"></a>Kommunikation zwischen VMs

1. Suchen Sie über das Suchfeld am oberen Rand des Portals nach *myVm1*. Wenn **myVm1** in den Suchergebnissen angezeigt wird, wählen Sie diese Angabe aus.
    
    :::image type="content" source="./media/tutorial-connect-virtual-networks-portal/search-vm.png" alt-text="Screenshot der Suche nach myVm1":::

1. Wählen Sie **Verbinden** und anschließend in der Dropdownliste **RDP** aus, um eine Verbindung mit der VM herzustellen. Wählen Sie **RDP-Datei herunterladen** aus, um die Remotedesktopdatei herunterzuladen.

    :::image type="content" source="./media/tutorial-connect-virtual-networks-portal/connect-to-virtual-machine.png" alt-text="Screenshot der Schaltfläche „Verbindung mit virtuellem Computer herstellen“"::: 

1. Öffnen Sie die heruntergeladene RDP-Datei, um eine Verbindung mit dem virtuellen Computer herzustellen. Wenn Sie dazu aufgefordert werden, wählen Sie **Verbinden** aus.

    :::image type="content" source="./media/tutorial-connect-virtual-networks-portal/rdp-connect.png" alt-text="Screenshot: Verbindungsbildschirm für Remotedesktop":::

1. Geben Sie den Benutzernamen und das Kennwort ein, wie beim Erstellen des virtuellen Computers angegeben. (Unter Umständen müssen Sie auf **Weitere Optionen** und dann auf **Anderes Konto verwenden** klicken, um die Anmeldeinformationen anzugeben, die Sie beim Erstellen des virtuellen Computers eingegeben haben.) Klicken Sie anschließend auf **OK**.

    :::image type="content" source="./media/tutorial-connect-virtual-networks-portal/rdp-credentials.png" alt-text="Screenshot des Bildschirms „RDP-Anmeldeinformationen“":::

1. Während des Anmeldevorgangs wird unter Umständen eine Zertifikatwarnung angezeigt. Wählen Sie **Ja** aus, um mit dem Herstellen der Verbindung fortzufahren.

1. In einem späteren Schritt wird Ping genutzt, um von der VM *myVm2* aus mit der VM *myVm1* zu kommunizieren. Ping verwendet standardmäßig das Internet Control Message-Protokoll (ICMP), das für die Windows-Firewall verweigert wird. Aktivieren Sie auf der VM *myVm1* ICMP über die Windows-Firewall, damit Sie diese VM in einem späteren Schritt mit PowerShell über *myVm2* per Ping erreichen können:

    ```powershell
    New-NetFirewallRule –DisplayName "Allow ICMPv4-In" –Protocol ICMPv4
    ```

    In diesem Tutorial wird zwar Ping für die Kommunikation zwischen virtuellen Computern verwendet, für Produktionsbereitstellungen wird jedoch davon abgeraten, ICMP-Datenverkehr durch die Windows-Firewall zuzulassen.

1. Um eine Verbindung mit der VM *myVm2* herzustellen, geben Sie an einer Eingabeaufforderung auf der VM *myVm1* den folgenden Befehl ein:

    ```
    mstsc /v:10.1.0.4
    ```
    
1. Da Sie den Ping für *myVm1* aktiviert haben, können Sie jetzt anhand der IP-Adresse pingen:

    ```
    ping 10.0.0.4
    ```
    
    :::image type="content" source="./media/tutorial-connect-virtual-networks-portal/myvm2-ping-myvm1.png" alt-text="Screenshot: MyVM2 pingt myVM1":::

1. Trennen Sie Ihre RDP-Sitzungen auf *myVm1* und *myVm2*.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcengruppe mit allen ihren Ressourcen, wenn Sie sie nicht mehr benötigen: 

1. Geben Sie im oben im Portal im Feld *Suche* die Zeichenfolge **myResourceGroup** ein. Wenn **myResourceGroup** in den Suchergebnissen angezeigt wird, wählen Sie diese Angabe aus.

1. Wählen Sie die Option **Ressourcengruppe löschen**.

1. Geben Sie für **Geben Sie den Ressourcengruppennamen ein:** den Namen *myResourceGroup* ein, und klicken Sie auf **Löschen**.

    :::image type="content" source="./media/tutorial-connect-virtual-networks-portal/delete-resource-group.png" alt-text="Screenshot der Seite „Ressourcengruppe löschen“":::

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> Informieren Sie sich ausführlicher über das [Peering virtueller Netzwerke](virtual-network-peering-overview.md).
