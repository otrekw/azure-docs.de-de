---
title: 'Weiterleiten von Netzwerkdatenverkehr: Tutorial – Azure-Portal'
titlesuffix: Azure Virtual Network
description: In diesem Tutorial erfahren Sie, wie Sie unter Verwendung des Azure-Portals Netzwerkdatenverkehr mit einer Routingtabelle weiterleiten.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/16/2021
ms.author: kumud
ms.openlocfilehash: 1d8a3bf8a17d04d98f605be3bcc01f47a61fdc89
ms.sourcegitcommit: b4032c9266effb0bf7eb87379f011c36d7340c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2021
ms.locfileid: "107906060"
---
# <a name="tutorial-route-network-traffic-with-a-route-table-using-the-azure-portal"></a>Tutorial: Weiterleiten von Netzwerkdatenverkehr mithilfe des Azure-Portals

Azure leitet den Netzwerkdatenverkehr standardmäßig durch alle Subnetze des virtuellen Netzwerks. Sie können eigene Routen erstellen, um das Azure-Standardrouting außer Kraft zu setzen. Benutzerdefinierte Routen sind beispielsweise hilfreich, wenn Sie über ein virtuelles Netzwerkgerät (Network Virtual Appliance, NVA) Datenverkehr zwischen Subnetzen weiterleiten möchten. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines virtuellen Netzwerkgeräts, das Datenverkehr weiterleitet
> * Erstellen einer Routingtabelle
> * Erstellen einer Route
> * Zuordnen einer Routingtabelle zu einem Subnetz
> * Bereitstellen von VMs in unterschiedlichen Subnetzen
> * Weiterleiten von Datenverkehr aus einem Subnetz zu einem anderen über ein virtuelles Netzwerkgerät

In diesem Tutorial wird das [Azure-Portal](https://portal.azure.com) verwendet. Sie können auch die [Azure-Befehlszeilenschnittstelle (Azure CLI)](tutorial-create-route-table-cli.md) oder [Azure PowerShell](tutorial-create-route-table-powershell.md) verwenden.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie beginnen, müssen Sie über ein Azure-Konto mit einem aktiven Abonnement verfügen. Falls Sie über keins verfügen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter https://portal.azure.com beim Azure-Portal an.

## <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

1. Wählen Sie im Menü des Azure-Portals die Option **Ressource erstellen** aus. Wählen Sie im Azure Marketplace die Option **Netzwerk**  >  **virtuelles Netzwerk** aus, oder suchen Sie im Suchfeld nach **Virtual Network**.

2. Wählen Sie **Erstellen** aus.

2. Geben Sie in **Virtuelles Netzwerk erstellen** diese Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | Subscription | Wählen Sie Ihr Abonnement aus.|
    | Ressourcengruppe | Wählen Sie **Neue erstellen** aus, und geben Sie **myResourceGroup** ein. </br> Klicken Sie auf **OK**. |
    | Name | Geben Sie **myVirtualNetwork** ein. |
    | Standort | Wählen Sie **(USA) USA, Osten** aus.|

3. Wählen Sie die Registerkarte **IP-Adressen** oder unten auf der Seite die Schaltfläche **Weiter: IP-Adressen** aus.

4. Wählen Sie unter **IPv4-Adressraum** den vorhandenen Adressraum aus, und ändern Sie ihn in **10.0.0.0/16**.

4. Wählen Sie **+ Subnetz hinzufügen** aus, und geben Sie unter **Subnetzname** den Namen **Public Subnetzname** und unter **Subnetzadressbereich** den Bereich **10.0.0.0/24** ein.

5. Wählen Sie **Hinzufügen**.

6. Wählen Sie **+ Subnetz hinzufügen** aus, und geben Sie unter **Subnetzname** den Namen **Private Subnetzname** und unter **Subnetzadressbereich** den Bereich **10.0.1.0/24** ein.

7. Wählen Sie **Hinzufügen**.

8. Wählen Sie **+ Subnetz hinzufügen** aus, und geben Sie unter **Subnetzname** den Namen **DMZ Subnetzname** und unter **Subnetzadressbereich** den Bereich **10.0.2.0/24** ein.

9. Wählen Sie **Hinzufügen**.

10. Wählen Sie die Registerkarte **Sicherheit** oder unten auf der Seite die Schaltfläche **Weiter: Sicherheit** aus.

11. Wählen Sie unter **BastionHost** die Option **Aktivieren** aus. Geben Sie die folgenden Informationen ein:

    | Einstellung            | Wert                      |
    |--------------------|----------------------------|
    | Bastion-Name | Geben Sie **myBastionHost** ein. |
    | AzureBastionSubnet-Adressraum | Geben Sie **10.0.3.0/24** ein. |
    | Öffentliche IP-Adresse | Wählen Sie **Neu erstellen**. </br> Geben Sie **myBastionIP** als **Name** ein. </br> Klicken Sie auf **OK**. |

12. Wählen Sie die Registerkarte **Überprüfen + erstellen** oder die Schaltfläche **Überprüfen + erstellen** aus.

13. Klicken Sie auf **Erstellen**.

## <a name="create-an-nva"></a>Erstellen eines virtuellen Netzwerkgeräts

Virtuelle Netzwerkgeräte (NVAs) sind VMs, die Netzwerkfunktionen unterstützen, z. B. Routing- und Firewalloptimierung. In diesem Tutorial wird vorausgesetzt, dass Sie **Windows Server 2019 Datacenter** verwenden. Sie können ein anderes Betriebssystem auswählen, wenn Sie möchten.

1. Navigieren Sie links oben im Portal zu **Ressource erstellen** > **Compute** > **Virtueller Computer**. 
   
2. Geben Sie unter **Virtuellen Computer erstellen** auf der Registerkarte **Grundlagen** die folgenden Werte ein, oder wählen Sie sie aus:

    | Einstellung | Wert                                          |
    |-----------------------|----------------------------------|
    | **Projektdetails** |  |
    | Subscription | Auswählen des Azure-Abonnements |
    | Ressourcengruppe | Wählen Sie **myResourceGroup** aus. |
    | **Instanzendetails** |  |
    | Name des virtuellen Computers | Geben Sie **MyVmNva** ein. |
    | Region | Wählen Sie **USA, Osten** aus. |
    | Verfügbarkeitsoptionen | Wählen Sie **Keine Infrastrukturredundanz erforderlich** aus. |
    | Image | Wählen Sie **Windows Server 2019 Datacenter** aus. |
    | Azure Spot-Instanz | Wählen Sie **Nein** aus. |
    | Size | Wählen Sie eine VM-Größe aus, oder übernehmen Sie die Standardeinstellung. |
    | **Administratorkonto** |  |
    | Username | Geben Sie einen Benutzernamen ein. |
    | Kennwort | Geben Sie ein Kennwort ein. |
    | Kennwort bestätigen | Geben Sie das Kennwort erneut ein. |
    | **Regeln für eingehende Ports** |    |
    | Öffentliche Eingangsports | Wählen Sie **Keine**. |
    |

3. Wählen Sie die Registerkarte **Netzwerk** aus, oder wählen Sie **Weiter: Datenträger** und anschließend **Weiter: Netzwerk** aus.
  
4. Wählen Sie auf der Registerkarte „Netzwerk“ die folgenden Werte aus, oder geben Sie sie ein:

    | Einstellung | Wert |
    |-|-|
    | **Netzwerkschnittstelle** |  |
    | Virtuelles Netzwerk | Wählen Sie **myVirtualNetwork** aus. |
    | Subnet | Wählen Sie **DMZ** aus. |
    | Öffentliche IP-Adresse | Wählen Sie **Keine** aus. |
    | NIC-Netzwerksicherheitsgruppe | Wählen Sie **Basic** aus.|
    | Public inbound ports network (Netzwerk mit öffentlichen Eingangsports) | Wählen Sie **Keine**. |
   
5. Wählen Sie die Registerkarte **Überprüfen + erstellen** oder unten auf der Seite die blaue Schaltfläche **Überprüfen + erstellen** aus.
  
6. Überprüfen Sie die Einstellungen, und wählen Sie dann die Option **Erstellen**.

## <a name="create-a-route-table"></a>Erstellen einer Routingtabelle

1. Wählen Sie im Menü des [Azure-Portals](https://portal.azure.com) oder auf der **Startseite** die Option **Ressource erstellen** aus.

2. Geben Sie **Routingtabelle** in das Suchfeld ein. Wählen Sie **Routingtabelle** aus, wenn diese Option in den Suchergebnissen angezeigt wird.

3. Wählen Sie auf der Seite **Routingtabelle** die Option **Erstellen** aus.

4. Geben Sie in der Registerkarte **Grundlagen** unter **Routentabelle** erstellen die folgenden Informationen ein oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | **Projektdetails** |   |
    | Subscription | Wählen Sie Ihr Abonnement aus.|
    | Resource group | Wählen Sie **myResourceGroup** aus. |
    | **Instanzendetails** |    |
    | Region | Wählen Sie **USA, Osten** aus. |
    | Name | Geben Sie **myRouteTablePublic** ein. |
    | Gatewayrouten verteilen | Wählen Sie **Ja** aus. |

    :::image type="content" source="./media/tutorial-create-route-table-portal/create-route-table.png" alt-text="Routingtabelle erstellen, Azure-Portal." border="true":::

5. Wählen Sie die Registerkarte **Überprüfen + erstellen** oder unten auf der Seite die blaue Schaltfläche **Überprüfen + erstellen** aus.

## <a name="create-a-route"></a>Erstellen einer Route

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com), um Ihre Routingtabelle zu verwalten. Suchen Sie nach **Routingtabellen**, und wählen Sie diese Option aus.

2. Wählen Sie den Namen Ihrer Routingtabelle (**myRouteTablePublic**) aus.

3. Wählen Sie auf der Seite **myroutetablepublic** im Abschnitt **Einstellungen** die Option **Routen** aus.

4. Wählen Sie auf der Seite Routen die Schaltfläche **+ Hinzufügen** aus.

5. Geben Sie in **Route hinzufügen** diese Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | Routenname | Geben Sie **ToPrivateSubnet** ein. |
    | Adresspräfix | Geben Sie **10.0.1.0/24** (der Adressbereich des zuvor erstellten Subnetzes **Privat**) ein |
    | Typ des nächsten Hops | Wählen Sie **Virtuelles Gerät** aus. |
    | Adresse des nächsten Hops | Geben Sie **10.0.2.4** (eine Adresse im Adressbereich des Subnetzes **DMZ**) ein |

6. Klicken Sie auf **OK**.

## <a name="associate-a-route-table-to-a-subnet"></a>Zuordnen einer Routingtabelle zu einem Subnetz

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com), um Ihr virtuelles Netzwerk zu verwalten. Suchen Sie nach **Virtuelle Netzwerke**, und wählen Sie diese Option aus.

2. Wählen Sie den Namen Ihres virtuellen Netzwerks (**myVirtualNetwork**) aus.

3. Wählen Sie auf der Seite **myvirtualnetwork** im Abschnitt **Einstellungen** die Option **Subnetze** aus.

4. Wählen Sie in der Subnetzliste des virtuellen Netzwerks **Öffentlich** aus.

5. Wählen Sie in der Routing **Tabelle** die erstellte Routing Tabelle **myroutetablepublic** aus. 

6. Wählen Sie **Speichern** aus, um die Routing Tabelle dem **öffentlichen** Subnetz zuzuordnen.

    :::image type="content" source="./media/tutorial-create-route-table-portal/associate-route-table.png" alt-text="Routingtabelle zuordnen, Subnetzliste, virtuelles Netzwerk, Azure-Portal." border="true":::

## <a name="turn-on-ip-forwarding"></a>Aktivieren der IP-Weiterleitung

Aktivieren Sie als Nächstes die IP-Weiterleitung für die neue NVA-VM (**myVmNva**). Wenn Azure Netzwerkdatenverkehr an **myVmNva** sendet und der Datenverkehr für eine andere IP-Adresse bestimmt ist, sendet die IP-Weiterleitung den Datenverkehr an die richtige Adresse.

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com), um Ihre VM zu verwalten. Suchen Sie nach **Virtuelle Computer**, und wählen Sie diese Option aus.

2. Wählen Sie den Namen des virtuellen Computers **myvmnva** aus.

3. Wählen Sie auf der Seite Übersicht von **myvmnva** unter **Einstellungen** die Option **Netzwerk** aus.

4. Wählen Sie auf der Seite **Netzwerk** von **myvmnva** die Netzwerkschnittstelle neben **Netzwerkschnittstelle** aus.  Der Name der Schnittstelle beginnt mit **myvmnva**.

    :::image type="content" source="./media/tutorial-create-route-table-portal/virtual-machine-networking.png" alt-text="Netzwerk, VM des virtuellen Netzwerkgeräts (Network Virtual Appliance, NVA), Azure-Portal" border="true":::

5. Wählen Sie auf der Seite Übersicht über die Netzwerkschnittstelle unter **Einstellungen** die Option **IP-Konfigurationen** aus.

6. Legen Sie auf der Seite **IP-Konfigurationen** die Option **IP-Weiterleitung** auf **Aktiviert** fest, und wählen Sie **Speichern** aus.

    :::image type="content" source="./media/tutorial-create-route-table-portal/enable-ip-forwarding.png" alt-text="Aktivieren der IP-Weiterleitung" border="true":::

## <a name="create-public-and-private-virtual-machines"></a>Erstellen von öffentlichen und privaten virtuellen Computern

Erstellen Sie einen öffentlichen und einem privaten virtuellen Computer im virtuellen Netzwerk. Später werden Sie sie verwenden, um anzuzeigen, dass Azure den **öffentlichen** Subnetzverkehr durch das virtuelle Netzwerkgerät zum **privaten** Subnetz leitet.


### <a name="public-vm"></a>Öffentliche VM

1. Navigieren Sie links oben im Portal zu **Ressource erstellen** > **Compute** > **Virtueller Computer**. 
   
2. Geben Sie unter **Virtuellen Computer erstellen** auf der Registerkarte **Grundlagen** die folgenden Werte ein, oder wählen Sie sie aus:

    | Einstellung | Wert                                          |
    |-----------------------|----------------------------------|
    | **Projektdetails** |  |
    | Subscription | Auswählen des Azure-Abonnements |
    | Ressourcengruppe | Wählen Sie **myResourceGroup** aus. |
    | **Instanzendetails** |  |
    | Name des virtuellen Computers | Geben Sie **myVmPublic** ein. |
    | Region | Wählen Sie **USA, Osten** aus. |
    | Verfügbarkeitsoptionen | Wählen Sie **Keine Infrastrukturredundanz erforderlich** aus. |
    | Image | Wählen Sie **Windows Server 2019 Datacenter** aus. |
    | Azure Spot-Instanz | Wählen Sie **Nein** aus. |
    | Size | Wählen Sie eine VM-Größe aus, oder übernehmen Sie die Standardeinstellung. |
    | **Administratorkonto** |  |
    | Username | Geben Sie einen Benutzernamen ein. |
    | Kennwort | Geben Sie ein Kennwort ein. |
    | Kennwort bestätigen | Geben Sie das Kennwort erneut ein. |
    | **Regeln für eingehende Ports** |    |
    | Öffentliche Eingangsports | Wählen Sie **Keine**. |
    |

3. Wählen Sie die Registerkarte **Netzwerk** aus, oder wählen Sie **Weiter: Datenträger** und anschließend **Weiter: Netzwerk** aus.
  
4. Wählen Sie auf der Registerkarte „Netzwerk“ die folgenden Werte aus, oder geben Sie sie ein:

    | Einstellung | Wert |
    |-|-|
    | **Netzwerkschnittstelle** |  |
    | Virtuelles Netzwerk | Wählen Sie **myVirtualNetwork** aus. |
    | Subnet | Wählen Sie **Öffentlich (10.0.0.0/24)** . |
    | Öffentliche IP-Adresse | Wählen Sie **Keine** aus. |
    | NIC-Netzwerksicherheitsgruppe | Wählen Sie **Basic** aus.|
    | Public inbound ports network (Netzwerk mit öffentlichen Eingangsports) | Wählen Sie **Keine**. |
   
5. Wählen Sie die Registerkarte **Überprüfen + erstellen** oder unten auf der Seite die blaue Schaltfläche **Überprüfen + erstellen** aus.
  
6. Überprüfen Sie die Einstellungen, und wählen Sie dann die Option **Erstellen**.

### <a name="private-vm"></a>Private VM

1. Navigieren Sie links oben im Portal zu **Ressource erstellen** > **Compute** > **Virtueller Computer**. 
   
2. Geben Sie unter **Virtuellen Computer erstellen** auf der Registerkarte **Grundlagen** die folgenden Werte ein, oder wählen Sie sie aus:

    | Einstellung | Wert                                          |
    |-----------------------|----------------------------------|
    | **Projektdetails** |  |
    | Subscription | Auswählen des Azure-Abonnements |
    | Ressourcengruppe | Wählen Sie **myResourceGroup** aus. |
    | **Instanzendetails** |  |
    | Name des virtuellen Computers | Geben Sie **myVmPrivate** ein. |
    | Region | Wählen Sie **USA, Osten** aus. |
    | Verfügbarkeitsoptionen | Wählen Sie **Keine Infrastrukturredundanz erforderlich** aus. |
    | Image | Wählen Sie **Windows Server 2019 Datacenter** aus. |
    | Azure Spot-Instanz | Wählen Sie **Nein** aus. |
    | Size | Wählen Sie eine VM-Größe aus, oder übernehmen Sie die Standardeinstellung. |
    | **Administratorkonto** |  |
    | Username | Geben Sie einen Benutzernamen ein. |
    | Kennwort | Geben Sie ein Kennwort ein. |
    | Kennwort bestätigen | Geben Sie das Kennwort erneut ein. |
    | **Regeln für eingehende Ports** |    |
    | Öffentliche Eingangsports | Wählen Sie **Keine**. |
    |

3. Wählen Sie die Registerkarte **Netzwerk** aus, oder wählen Sie **Weiter: Datenträger** und anschließend **Weiter: Netzwerk** aus.
  
4. Wählen Sie auf der Registerkarte „Netzwerk“ die folgenden Werte aus, oder geben Sie sie ein:

    | Einstellung | Wert |
    |-|-|
    | **Netzwerkschnittstelle** |  |
    | Virtuelles Netzwerk | Wählen Sie **myVirtualNetwork** aus. |
    | Subnet | Wählen Sie **Privat** aus. |
    | Öffentliche IP-Adresse | Wählen Sie **Keine** aus. |
    | NIC-Netzwerksicherheitsgruppe | Wählen Sie **Basic** aus.|
    | Public inbound ports network (Netzwerk mit öffentlichen Eingangsports) | Wählen Sie **Keine**. |
   
5. Wählen Sie die Registerkarte **Überprüfen + erstellen** oder unten auf der Seite die blaue Schaltfläche **Überprüfen + erstellen** aus.
  
6. Überprüfen Sie die Einstellungen, und wählen Sie dann die Option **Erstellen**.

## <a name="route-traffic-through-an-nva"></a>Weiterleiten von Datenverkehr über ein virtuelles Netzwerkgerät

### <a name="sign-in-to-private-vm"></a>Anmelden bei privater VM

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com), um Ihre private VM zu verwalten. Suchen Sie nach **Virtuelle Computer**, und wählen Sie diese Option aus.

2. Wählen Sie den Namen Ihres privaten virtuellen Computers **myVMPrivate** aus.

3. Wählen Sie auf der Menüleiste des virtuellen Computers die Option **Verbinden** und dann **Bastion** aus.

4. Wählen Sie auf der Seite **Verbinden** die blaue Schaltfläche **Bastion verwenden** aus.

5. Geben Sie auf der Seite **Bastion** den Benutzernamen und das Kennwort ein, die Sie zuvor für den virtuellen Computer erstellt haben.

6. Wählen Sie **Verbinden** aus.

### <a name="configure-firewall"></a>Konfigurieren der Firewall

In einem späteren Schritt wird das Routing mithilfe des Traceroute-Tools getestet. Traceroute verwendet das Internet Control Message-Protokoll (ICMP). Dieses Protokoll wird durch die Windows-Firewall standardmäßig blockiert. 

Aktivieren Sie ICMP über die Windows-Firewall.

1. Öffnen Sie in der geschützten Verbindung von **myVMprivate** PowerShell mit Administratorrechten.

2. Geben Sie diesen Befehl ein:

    ```powershell
    New-NetFirewallRule –DisplayName "Allow ICMPv4-In" –Protocol ICMPv4
    ```

    Sie verwenden Traceroute, um das Routing in diesem Tutorial zu testen. Für Produktionsumgebungen empfehlen wir nicht, ICMP über die Windows-Firewall zuzulassen.

### <a name="turn-on-ip-forwarding-within-myvmnva"></a>Aktivieren der IP-Weiterleitung in myVMNVA

Sie haben [die IP-Weiterleitung](#turn-on-ip-forwarding) für die Netzwerkschnittstelle des virtuellen Computers mit Azure aktiviert. Das Betriebssystem des virtuellen Computers muss Netzwerkdatenverkehr ebenfalls weiterleiten. 

Schalten Sie mit diesen Befehlen die IP-Weiterleitung für **myVMNVA** ein.

1. Öffnen Sie über PowerShell auf der **myVmPrivate** eine Remotedesktopverbindung zu **myVMNVA** VM:

    ```powershell
    mstsc /v:myvmnva
    ```

2. Geben Sie in PowerShell auf der VM **myVMNVA** den folgenden Befehl ein, um die IP-Weiterleitung zu aktivieren:

    ```powershell
    Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters -Name IpEnableRouter -Value 1
    ```

3. Starten Sie **myVMNVA** neu.

    ```powershell
    Restart-Computer
    ```

4. Nachdem **myVMNVA** neu gestartet wurde, erstellen Sie eine Remote Desktop Sitzung mit **myvmpublic**. 
    
    Solange Sie noch mit der **myVmPrivate** verbunden sind, öffnen Sie PowerShell und führen Sie diesen Befehl aus:

    ```powershell
    mstsc /v:myvmpublic
    ```
5. Öffnen Sie auf dem Remotedesktop von **myVmPublic** PowerShell.

6. Aktivieren Sie ICMP für die Windows-Firewall, indem Sie den folgenden Befehl eingeben:

    ```powershell
    New-NetFirewallRule –DisplayName "Allow ICMPv4-In" –Protocol ICMPv4
    ```

## <a name="test-the-routing-of-network-traffic"></a>Testen des Routings von Netzwerkdatenverkehr

Zunächst testen wir das Routing des Netzwerkdatenverkehrs von **myVmPublic** zu **myVmPrivate**.

1. Geben Sie von PowerShell auf **myVmPublic** diesen Befehl ein:

    ```powershell
    tracert myvmprivate
    ```

    Die Antwort ähnelt diesem Beispiel:

    ```powershell
    Tracing route to myvmprivate.q04q2hv50taerlrtdyjz5nza1f.bx.internal.cloudapp.net [10.0.1.4]
    over a maximum of 30 hops:

      1     1 ms     *        2 ms  myvmnva.internal.cloudapp.net [10.0.2.4]
      2     2 ms     1 ms     1 ms  myvmprivate.internal.cloudapp.net [10.0.1.4]

    Trace complete.
    ```

    * Wie Sie sehen, ist der erste Hop 10.0.2.4, d. h **myVMNVA's** private IP Adresse. 

    * Der zweite Hop ist die private IP-Adresse der VM **myVmPrivate**:10.0.1.4 

    Zuvor haben Sie die Route der Routingtabelle **myRouteTablePublic** hinzugefügt und dem Subnetz *Öffentlich* zugeordnet. Infolgedessen sendete Azure den Netzwerkdatenverkehr über das Netzwerkgerät und nicht direkt an das Subnetz *Privat*.

2. Schließen Sie die Remotedesktopsitzung mit der VM **myVmPublic**, sodass Sie nur noch mit der VM **myVmPrivate** verbunden sind.

3. Öffnen Sie PowerShell auf **myVmprivate**, und geben Sie den folgenden Befehl ein:

    ```powershell
    tracert myvmpublic
    ```

    Dieser Befehl testet das Routing des Netzwerkdatenverkehrs von der VM *myVmPrivate* zur VM *myVmPublic*. Die Antwort ähnelt diesem Beispiel:

    ```cmd
    Tracing route to myvmpublic.q04q2hv50taerlrtdyjz5nza1f.bx.internal.cloudapp.net [10.0.0.4]
    over a maximum of 30 hops:

      1     1 ms     1 ms     1 ms  myvmpublic.internal.cloudapp.net [10.0.0.4]

    Trace complete.
    ```

    Wie Sie sehen, leitet Azure Netzwerkdatenverkehr von *myVmPrivate* direkt zu *myVmPublic* weiter. Azure-Routen leiten standardmäßig Datenverkehr direkt durch Subnetze weiter.

4. Schließen Sie die geschützte Sitzung mit **myvmprivate**.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcengruppe **myResourceGroup** mit allen zugehörigen Ressourcen, wenn Sie sie nicht mehr benötigen:

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com), um Ihre Ressourcengruppe zu verwalten. Suchen Sie nach **Ressourcengruppen**, und wählen Sie diese Option aus.

2. Wählen Sie den Namen Ihrer Ressourcengruppe (**myResourceGroup**) aus.

3. Wählen Sie die Option **Ressourcengruppe löschen**.

4. Geben Sie im Bestätigungsdialogfeld **myResourceGroup** in das Feld **Geben Sie den Ressourcengruppennamen ein** ein, und klicken Sie dann auf **Löschen**. 


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial:

* Sie eine Routingtabelle erstellt und einem Subnetz zugeordnet.
* Sie haben ein einfaches virtuelles Netzwerkgerät erstellt, das Datenverkehr von einem öffentlichen Subnetz an ein privates Subnetz weiterleitet. 

Nun können Sie verschiedene vorkonfigurierte NVAs mit vielen nützlichen Netzwerkfunktionen aus dem [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking) bereitstellen. 

Weitere Informationen zum Routing finden Sie unter [Routing von Datenverkehr für virtuelle Netzwerke](virtual-networks-udr-overview.md) und [Erstellen, Ändern oder Löschen einer Routingtabelle](manage-route-table.md).

Filtern von Netzwerkdatenverkehr mit Netzwerksicherheitsgruppen
> [!div class="nextstepaction"]
> [Tutorial: Filtern von Netzwerkdatenverkehr mithilfe einer Netzwerksicherheitsgruppe über das Azure-Portal](tutorial-filter-network-traffic.md)
