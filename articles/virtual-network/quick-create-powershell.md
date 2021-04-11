---
title: 'Erstellen eines virtuellen Netzwerks: Schnellstart – Azure PowerShell'
titlesuffix: Azure Virtual Network
description: In dieser Schnellstartanleitung erstellen Sie über das Azure-Portal ein virtuelles Netzwerk. Ein virtuelles Netzwerk ermöglicht die Kommunikation der Azure-Ressourcen untereinander und mit dem Internet.
author: KumudD
ms.service: virtual-network
ms.topic: quickstart
ms.date: 03/06/2021
ms.author: kumud
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 7ee10327ab95a3e66e5592593ae72d6e5cd8d606
ms.sourcegitcommit: 73fb48074c4c91c3511d5bcdffd6e40854fb46e5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2021
ms.locfileid: "106060600"
---
# <a name="quickstart-create-a-virtual-network-using-powershell"></a>Schnellstart: Erstellen eines virtuellen Netzwerks über PowerShell

In einem virtuellen Netzwerk können Azure-Ressourcen wie virtuelle Computer (VMs) privat miteinander und mit dem Internet kommunizieren. 

In dieser Schnellstartanleitung erfahren Sie, wie Sie ein virtuelles Netzwerk erstellen. Nach dem Erstellen eines virtuellen Netzwerks stellen Sie zwei virtuelle Computer im virtuellen Netzwerk bereit. Anschließend stellen Sie über das Internet eine Verbindung mit den VMs her und kommunizieren privat über das virtuelle Netzwerk.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Azure PowerShell (lokal installiert) oder Azure Cloud Shell

Wenn Sie PowerShell lokal installieren und verwenden möchten, müssen Sie für diesen Artikel mindestens Version 5.4.1 des Azure PowerShell-Moduls verwenden. Führen Sie `Get-Module -ListAvailable Az` aus, um die installierte Version zu ermitteln. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-Az-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Connect-AzAccount` ausführen, um eine Verbindung mit Azure herzustellen.

## <a name="create-a-resource-group-and-a-virtual-network"></a>Erstellen einer Ressourcengruppe und eines virtuellen Netzwerks

Es gibt eine Handvoll Schritte, die Sie durchlaufen müssen, um Ihre Ressourcengruppe und Ihr virtuelles Netzwerk zu konfigurieren.

### <a name="create-the-resource-group"></a>Ressourcengruppe erstellen

Bevor Sie ein virtuelles Netzwerk erstellen können, müssen Sie eine Ressourcengruppe zum Hosten des virtuellen Netzwerks erstellen. Erstellen Sie mit [New-AzResourceGroup](/powershell/module/az.Resources/New-azResourceGroup) eine Ressourcengruppe. In diesem Beispiel wird eine Ressourcengruppe mit dem Namen **CreateVNetQS-rg** am Standort **eastus** (USA, Osten) erstellt:

```azurepowershell-interactive
$rg = @{
    Name = 'CreateVNetQS-rg'
    Location = 'EastUS'
}
New-AzResourceGroup @rg
```

### <a name="create-the-virtual-network"></a>Erstellen des virtuellen Netzwerks

Erstellen Sie mit [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) ein virtuelles Netzwerk. In diesem Beispiel wird ein virtuelles Standardnetzwerk mit dem Namen **myVNet** am Standort **EastUS** erstellt:

```azurepowershell-interactive
$vnet = @{
    Name = 'myVNet'
    ResourceGroupName = 'CreateVNetQS-rg'
    Location = 'EastUS'
    AddressPrefix = '10.0.0.0/16'    
}
$virtualNetwork = New-AzVirtualNetwork @vnet
```

### <a name="add-a-subnet"></a>Hinzufügen eines Subnetzes

Azure stellt Ressourcen innerhalb eines virtuellen Netzwerks in einem Subnetz bereit, daher müssen Sie ein Subnetz erstellen. Erstellen Sie mit [Add-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/add-azvirtualnetworksubnetconfig) eine Subnetzkonfiguration mit der Bezeichnung **default**:

```azurepowershell-interactive
$subnet = @{
    Name = 'default'
    VirtualNetwork = $virtualNetwork
    AddressPrefix = '10.0.0.0/24'
}
$subnetConfig = Add-AzVirtualNetworkSubnetConfig @subnet
```

### <a name="associate-the-subnet-to-the-virtual-network"></a>Zuweisen eines Subnetzes zum virtuellen Netzwerk

Schreiben Sie mit [Set-AzVirtualNetwork](/powershell/module/az.network/Set-azVirtualNetwork) die Subnetzkonfiguration in das virtuelle Netzwerk. Dieser Befehl erstellt das Subnetz:

```azurepowershell-interactive
$virtualNetwork | Set-AzVirtualNetwork
```

## <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern

Erstellen Sie zwei virtuelle Computer im virtuellen Netzwerk.

### <a name="create-the-first-vm"></a>Erstellen des ersten virtuellen Computers

Erstellen Sie mit [New-AzVM](/powershell/module/az.compute/new-azvm) den ersten virtuellen Computer. Wenn Sie den nächsten Befehl ausführen, werden Sie zur Eingabe von Anmeldeinformationen aufgefordert. Geben Sie einen Benutzernamen und ein Kennwort für den virtuellen Computer ein:

```azurepowershell-interactive
$vm1 = @{
    ResourceGroupName = 'CreateVNetQS-rg'
    Location = 'EastUS'
    Name = 'myVM1'
    VirtualNetworkName = 'myVNet'
    SubnetName = 'default'
}
New-AzVM @vm1 -AsJob
```

Die `-AsJob`-Option erstellt den virtuellen Computer im Hintergrund. Sie können mit dem nächsten Schritt fortfahren.

Wenn Azure beginnt, den virtuellen Computer im Hintergrund zu erstellen, wird in etwas folgendes zurückgegeben:

```powershell
Id     Name            PSJobTypeName   State         HasMoreData     Location             Command
--     ----            -------------   -----         -----------     --------             -------
1      Long Running... AzureLongRun... Running       True            localhost            New-AzVM
```

### <a name="create-the-second-vm"></a>Erstellen des zweiten virtuellen Computers

Erstellen Sie den zweiten virtuellen Computer mit diesem folgenden Befehl:

```azurepowershell-interactive
$vm2 = @{
    ResourceGroupName = 'CreateVNetQS-rg'
    Location = 'EastUS'
    Name = 'myVM2'
    VirtualNetworkName = 'myVNet'
    SubnetName = 'default'
}
New-AzVM @vm2
```

Sie müssen einen weiteren Benutzer und ein weiteres Kennwort anlegen. Das Erstellen der VM dauert in Azure einige Minuten.

> [!IMPORTANT]
> Fahren Sie mit dem nächsten Schritt erst fort, wenn Azure die Erstellung abgeschlossen hat.  Sie erkennen den Abschluss der Erstellung daran, dass eine Ausgabe an PowerShell erfolgt.

## <a name="connect-to-a-vm-from-the-internet"></a>Herstellen einer Verbindung mit einem virtuellen Computer über das Internet

Verwenden Sie [Get-AzPublicIpAddress](/powershell/module/az.network/get-azpublicipaddress) zum Abrufen der öffentlichen IP-Adresse des virtuellen Computers:

Das Beispiel gibt die öffentliche IP-Adresse des virtuellen Computers **myVm1** zurück:

```azurepowershell-interactive
$ip = @{
    Name = 'myVM1'
    ResourceGroupName = 'CreateVNetQS-rg'
}
Get-AzPublicIpAddress @ip | select IpAddress
```

Öffnen Sie eine Eingabeaufforderung auf dem lokalen Computer. Führen Sie den Befehl `mstsc` aus. Ersetzen Sie `<publicIpAddress>` durch die im vorherigen Schritt zurückgegebene öffentliche IP-Adresse:

> [!NOTE]
> Wenn Sie diese Befehle über eine PowerShell-Eingabeaufforderung auf Ihrem lokalen Computer ausgeführt haben und mindestens die Azure PowerShell-Modulversion 1.0 verwenden, können Sie mit dieser Schnittstelle fortfahren.

```cmd
mstsc /v:<publicIpAddress>
```
1. Wenn Sie dazu aufgefordert werden, wählen Sie **Verbinden** aus.

1. Geben Sie den Benutzernamen und das Kennwort ein, die Sie beim Erstellen des virtuellen Computers festgelegt haben.

    > [!NOTE]
    > Unter Umständen müssen Sie **Weitere Optionen** > **Anderes Konto verwenden** auswählen, um die Anmeldeinformationen anzugeben, die Sie beim Erstellen des virtuellen Computers eingegeben haben.

1. Klicken Sie auf **OK**.

1. Sie erhalten möglicherweise eine Zertifikatwarnung. Ist dies der Fall, wählen Sie **Ja** oder **Weiter** aus.

## <a name="communicate-between-vms"></a>Kommunikation zwischen VMs

1. Öffnen Sie auf dem Remote Desktop von **myVm1** PowerShell.

1. Geben Sie `ping myVm2` ein.

    Es wird etwa diese Meldung angezeigt:

    ```powershell
    PS C:\Users\myVm1> ping myVm2

    Pinging myVm2.ovvzzdcazhbu5iczfvonhg2zrb.bx.internal.cloudapp.net
    Request timed out.
    Request timed out.
    Request timed out.
    Request timed out.

    Ping statistics for 10.0.0.5:
        Packets: Sent = 4, Received = 0, Lost = 4 (100% loss),
    ```

    Bei Ping tritt ein Fehler auf, weil Internet Control Message-Protokoll (ICMP) verwendet wird. Standardmäßig kann ICMP nicht durch Ihre Windows-Firewall ausgeführt werden.

1. Damit **myVm2** in einem späteren Schritt ein Pingsignal an **myVm1** senden kann, geben Sie diesen Befehl ein:

    ```powershell
    New-NetFirewallRule –DisplayName "Allow ICMPv4-In" –Protocol ICMPv4
    ```

    Dieser Befehl ermöglicht den eingehenden ICMP-Datenverkehr durch die Windows-Firewall.

1. Schließen Sie die Remotedesktopverbindung mit **myVm1**.

1. Wiederholen Sie die Schritte in [Herstellen einer Verbindung mit einem virtuellen Computer über das Internet](#connect-to-a-vm-from-the-internet). Dieses Mal stellen Sie eine Verbindung zu **myVm2** her.

1. Geben Sie an einer Eingabeaufforderung auf dem virtuellen Computer **myVm2** den Befehl `ping myvm1` ein.

    Es wird etwa diese Meldung angezeigt:

    ```cmd
    C:\windows\system32>ping myVm1

    Pinging myVm1.e5p2dibbrqtejhq04lqrusvd4g.bx.internal.cloudapp.net [10.0.0.4] with 32 bytes of data:
    Reply from 10.0.0.4: bytes=32 time=2ms TTL=128
    Reply from 10.0.0.4: bytes=32 time<1ms TTL=128
    Reply from 10.0.0.4: bytes=32 time<1ms TTL=128
    Reply from 10.0.0.4: bytes=32 time<1ms TTL=128

    Ping statistics for 10.0.0.4:
        Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 0ms, Maximum = 2ms, Average = 0ms
    ```

    Sie erhalten Antworten von **myVm1**, da Sie in einem vorherigen Schritt das ICMP über die Windows-Firewall auf dem virtuellen Computer **myVm1** zugelassen haben.

1. Schließen Sie die Remotedesktopverbindung mit **myVm2**.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Bearbeitung des virtuellen Netzwerks und der virtuellen Computer abgeschlossen haben, verwenden Sie [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup), um die Ressourcengruppe und alle darin enthaltenen Ressourcen zu löschen:

```azurepowershell-interactive
Remove-AzResourceGroup -Name 'CreateVNetQS-rg' -Force
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung gilt: 

* Sie haben ein virtuelles Standardnetzwerk und zwei virtuelle Computer erstellt. 
* Sie haben über das Internet eine Verbindung mit einem der virtuellen Computer hergestellt und privat zwischen den beiden virtuellen Computern kommuniziert.

Die private Kommunikation zwischen VMs ist in einem virtuellen Netzwerk uneingeschränkt. 

Fahren Sie mit dem nächsten Artikel fort, um mehr über das Konfigurieren verschiedener Typen der VM-Netzwerkkommunikation zu erfahren:
> [!div class="nextstepaction"]
> [Erstellen von Netzwerksicherheitsgruppen über das Azure-Portal](tutorial-filter-network-traffic.md)
