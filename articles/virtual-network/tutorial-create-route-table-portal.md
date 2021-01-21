---
title: 'Weiterleiten von Netzwerkdatenverkehr: Tutorial – Azure-Portal'
titlesuffix: Azure Virtual Network
description: In diesem Tutorial erfahren Sie, wie Sie unter Verwendung des Azure-Portals Netzwerkdatenverkehr mit einer Routingtabelle weiterleiten.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
Customer intent: I want to route traffic from one subnet, to a different subnet, through a network virtual appliance.
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/13/2020
ms.author: kumud
ms.openlocfilehash: e047f46e110e1f7b1d544545c80bd1097ae65167
ms.sourcegitcommit: d59abc5bfad604909a107d05c5dc1b9a193214a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/14/2021
ms.locfileid: "98221917"
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

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="create-an-nva"></a>Erstellen eines virtuellen Netzwerkgeräts

Virtuelle Netzwerkgeräte (NVAs) sind VMs, die Netzwerkfunktionen unterstützen, z. B. Routing- und Firewalloptimierung. In diesem Tutorial wird vorausgesetzt, dass Sie **Windows Server 2016 Datacenter** verwenden. Sie können ein anderes Betriebssystem auswählen, wenn Sie möchten.

1. Wählen Sie im Menü des [Azure-Portals](https://portal.azure.com) oder auf der **Startseite** die Option **Ressource erstellen** aus.

1. Wählen Sie **Sicherheit** > **Windows Server 2016 Datacenter** aus.

    ![Windows Server 2016 Datacenter, VM erstellen, Azure-Portal](./media/tutorial-create-route-table-portal/vm-ws2016-datacenter.png)

1. Geben Sie auf der Seite **Virtuellen Computer erstellen** unter **Grundlagen** die folgenden Informationen ein, oder wählen Sie sie aus:

    | `Section` | Einstellung | Aktion |
    | ------- | ------- | ----- |
    | **Projektdetails** | Subscription | Wählen Sie Ihr Abonnement aus. |
    | | Resource group | Wählen Sie **Neue erstellen** aus, geben Sie *myResourceGroup* ein, und wählen Sie **OK** aus. |
    | **Instanzendetails** | Name des virtuellen Computers | Geben Sie *MyVmNva* ein. |
    | | Region | Wählen Sie **(USA) USA, Osten** aus. |
    | | Verfügbarkeitsoptionen | Wählen Sie **Keine Infrastrukturredundanz erforderlich** aus. |
    | | Image | Wählen Sie **Windows Server 2016 Datacenter** aus. |
    | | Size | Übernehmen Sie den Standardwert **Standard DS1 v2**. |
    | **Administratorkonto** | Username | Geben Sie den gewünschten Benutzernamen ein. |
    | | Kennwort | Geben Sie ein Kennwort Ihrer Wahl ein, das mindestens 12 Zeichen lang ist und den [Anforderungen an die Komplexität von Kennwörtern](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) entspricht. |
    | | Kennwort bestätigen | Geben Sie das Kennwort erneut ein. |
    | **Regeln für eingehende Ports** | Öffentliche Eingangsports | Wählen Sie **Keine** aus. |
    | **Sparen Sie Geld** | Sie verfügen bereits über eine Windows Server-Lizenz? | Wählen Sie **Nein** aus. |

    ![Grundlagen, VM erstellen, Azure-Portal](./media/tutorial-create-route-table-portal/basics-create-virtual-machine.png)

    Wählen Sie anschließend **Weiter: Datenträger >** .

1. Wählen Sie unter **Datenträger** die entsprechenden Einstellungen für Ihre Anforderungen aus, und wählen Sie dann **Weiter: Netzwerk >** .

1. Unter **Netzwerk**:

    1. Wählen Sie unter **Virtuelles Netzwerk** die Option **Neu erstellen** aus.
    
    1. Geben Sie im Dialogfeld **Virtuelles Netzwerk erstellen** unter **Name** den Namen *myVirtualNetwork* ein.

    1. Ersetzen Sie unter **Adressraum** den vorhandenen Adressbereich durch *10.0.0.0/16*.

    1. Klicken Sie unter **Subnetze** auf das Symbol **Löschen**, um das vorhandene Subnetz zu löschen, und geben Sie dann die folgenden Kombinationen aus **Subnetzname** und **Adressbereich** ein. Sobald Sie einen gültigen Namen und Bereich eingegeben haben, wird darunter eine neue leere Zeile angezeigt.

        | Subnetzname | Adressbereich |
        | ----------- | ------------- |
        | *Öffentlich* | *10.0.0.0/24* |
        | *Privat* | *10.0.1.0/24* |
        | *DMZ* | *10.0.2.0/24* |

    1. Klicken Sie auf **OK**, um das Dialogfeld zu schließen.

    1. Wählen Sie unter **Subnetz** die Option **DMZ (10.0.2.0/24)** aus.

    1. Wählen Sie unter **Öffentliche IP** die Option **Keine** aus, da diese VM keine Verbindung über das Internet herstellt.

    1. Wählen Sie **Weiter: Verwaltung >** .

1. Unter **Verwaltung**:

    1. Wählen Sie unter **Diagnosespeicherkonto** die Option **Neu erstellen** aus.
    
    1. Geben Sie im Dialogfeld **Speicherkonto erstellen** die folgenden Informationen ein, oder wählen Sie sie aus:

        | Einstellung | Wert |
        | ------- | ----- |
        | Name | *mynvastorageaccount* |
        | Kontoart | **Storage (Universell V1)** |
        | Leistung | **Standard** |
        | Replikation | **Lokal redundanter Speicher (LRS)** |
    
    1. Klicken Sie auf **OK**, um das Dialogfeld zu schließen.

    1. Klicken Sie auf **Überprüfen + erstellen**. Sie gelangen auf die Seite **Bewerten + erstellen**, und Azure überprüft Ihre Konfiguration.

1. Wenn die Meldung **Überprüfung erfolgreich** angezeigt wird, wählen Sie **Erstellen** aus.

    Die Erstellung des virtuellen Computers dauert einige Minuten. Warten Sie, bis Azure die Erstellung der VM abgeschlossen hat. Auf der Seite **Ihre Bereitstellung wird ausgeführt** werden Bereitstellungsdetails angezeigt.

1. Wenn Ihr virtueller Computer bereit ist, wählen Sie **Zu Ressource wechseln** aus.

## <a name="create-a-route-table"></a>Erstellen einer Routingtabelle

1. Wählen Sie im Menü des [Azure-Portals](https://portal.azure.com) oder auf der **Startseite** die Option **Ressource erstellen** aus.

2. Geben Sie *Routingtabelle* in das Suchfeld ein. Wählen Sie **Routingtabelle** aus, wenn diese Option in den Suchergebnissen angezeigt wird.

3. Wählen Sie auf der Seite **Routingtabelle** die Option **Erstellen** aus.

4. Geben Sie in **Routingtabelle erstellen** diese Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | Name | *myRouteTablePublic* |
    | Subscription | Ihr Abonnement |
    | Resource group | **myResourceGroup** |
    | Standort | **(USA) USA, Osten** |
    | Routenverteilung des Gateways für virtuelle Netzwerke | **Aktiviert** |

    ![Routingtabelle erstellen, Azure-Portal](./media/tutorial-create-route-table-portal/create-route-table.png)

5. Klicken Sie auf **Erstellen**.

## <a name="create-a-route"></a>Erstellen einer Route

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com), um Ihre Routingtabelle zu verwalten. Suchen Sie nach **Routingtabellen**, und wählen Sie diese Option aus.

1. Wählen Sie den Namen Ihrer Routingtabelle (**myRouteTablePublic**) aus.

1. Wählen Sie **Routen** > **Hinzufügen** aus.

    ![Route hinzufügen, Routingtabelle, Azure-Portal](./media/tutorial-create-route-table-portal/add-route.png)

1. Geben Sie in **Route hinzufügen** diese Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | Routenname | *ToPrivateSubnet* |
    | Adresspräfix | *10.0.1.0/24* (der Adressbereich des zuvor erstellten Subnetzes *Privat*) |
    | Typ des nächsten Hops | **Virtuelles Gerät** |
    | Adresse des nächsten Hops | *10.0.2.4* (eine Adresse im Adressbereich des Subnetzes *DMZ*) |

1. Klicken Sie auf **OK**.

## <a name="associate-a-route-table-to-a-subnet"></a>Zuordnen einer Routingtabelle zu einem Subnetz

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com), um Ihr virtuelles Netzwerk zu verwalten. Suchen Sie nach **Virtuelle Netzwerke**, und wählen Sie diese Option aus.

1. Wählen Sie den Namen Ihres virtuellen Netzwerks (**myVirtualNetwork**) aus.

1. Wählen Sie in der Menüleiste des virtuellen Netzwerks **Subnetze** aus.

1. Wählen Sie in der Subnetzliste des virtuellen Netzwerks **Öffentlich** aus.

1. Wählen Sie unter **Routingtabelle** die von Ihnen erstellte Routingtabelle (**myRouteTablePublic**) aus, und klicken Sie dann auf **Speichern**, um die Routingtabelle dem Subnetz *Öffentlich* zuzuordnen.

    ![Routingtabelle zuordnen, Subnetzliste, virtuelles Netzwerk, Azure-Portal](./media/tutorial-create-route-table-portal/associate-route-table.png)

## <a name="turn-on-ip-forwarding"></a>Aktivieren der IP-Weiterleitung

Aktivieren Sie als Nächstes die IP-Weiterleitung für die neue NVA-VM (*myVmNva*). Wenn Azure Netzwerkdatenverkehr an *myVmNva* sendet und der Datenverkehr für eine andere IP-Adresse bestimmt ist, sendet die IP-Weiterleitung den Datenverkehr an die richtige Adresse.

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com), um Ihre VM zu verwalten. Suchen Sie nach **Virtuelle Computer**, und wählen Sie diese Option aus.

1. Wählen Sie den Namen Ihrer VM (**myVmNva**) aus.

1. Wählen Sie in der Menüleiste Ihrer NVA-VM **Netzwerk** aus.

1. Wählen Sie **myvmnva123** aus. Dies ist die Netzwerkschnittstelle, die Azure für Ihren virtuellen Computer erstellt hat. Azure fügt Zahlen hinzu, damit der Name eindeutig ist.

    ![Netzwerk, VM des virtuellen Netzwerkgeräts (Network Virtual Appliance, NVA), Azure-Portal](./media/tutorial-create-route-table-portal/virtual-machine-networking.png)

1. Wählen Sie in der Menüleiste der Netzwerkschnittstelle **IP-Konfigurationen** aus.

1. Legen Sie auf der Seite **IP-Konfigurationen** die Option **IP-Weiterleitung** auf **Aktiviert** fest, und wählen Sie **Speichern** aus.

    ![IP-Weiterleitung aktivieren, IP-Konfigurationen, Netzwerkschnittstelle, VM des virtuellen Netzwerkgeräts (Network Virtual Appliance, NVA), Azure-Portal](./media/tutorial-create-route-table-portal/enable-ip-forwarding.png)

## <a name="create-public-and-private-virtual-machines"></a>Erstellen von öffentlichen und privaten virtuellen Computern

Erstellen Sie einen öffentlichen und einem privaten virtuellen Computer im virtuellen Netzwerk. Später werden Sie sie verwenden, um anzuzeigen, dass Azure den *öffentlichen* Subnetzverkehr durch das virtuelle Netzwerkgerät zum *privaten* Subnetz leitet.

Um die öffentliche und private VM zu erstellen, führen Sie die Schritte im Abschnitt [Erstellen eines virtuellen Netzwerkgeräts](#create-an-nva) weiter oben aus. Sie müssen nicht warten, bis die Bereitstellung abgeschlossen ist, oder zur VM-Ressource wechseln. Abgesehen von den unten beschriebenen Ausnahmen verwenden Sie die gleichen Einstellungen.

In den folgenden beiden Unterabschnitten ([Öffentliche VM](#public-vm) und [Private VM](#private-vm)) finden Sie die Werte, die anders lauten müssen. Legen Sie diese Werte fest, bevor Sie auf **Erstellen** klicken, um die öffentliche oder private VM zu erstellen. Wenn Azure die Bereitstellung beider VMs abgeschlossen hat, können Sie mit dem nächsten Abschnitt fortfahren ([Weiterleiten von Datenverkehr über ein virtuelles Netzwerkgerät](#route-traffic-through-an-nva)).

### <a name="public-vm"></a>Öffentliche VM

| Registerkarte | Einstellung | Wert |
| --- | ------- | ----- |
| Grundlagen | Resource group | **myResourceGroup** |
| | Name des virtuellen Computers | *myVmPublic* |
| | Öffentliche Eingangsports | **Ausgewählte Ports zulassen** |
| | Eingangsports auswählen | **RDP** |
| Netzwerk | Virtuelles Netzwerk | **myVirtualNetwork** |
| | Subnet | **Öffentlich (10.0.0.0/24)** |
| | Öffentliche IP-Adresse | Standardwert |
| Verwaltung | Diagnosespeicherkonto | **mynvastorageaccount** |

### <a name="private-vm"></a>Private VM

| Registerkarte | Einstellung | Wert |
| --- | ------- | ----- |
| Grundlagen | Resource group | **myResourceGroup** |
| | Name des virtuellen Computers | *myVmPrivate* |
| | Öffentliche Eingangsports | **Ausgewählte Ports zulassen** |
| | Eingangsports auswählen | **RDP** |
| Netzwerk | Virtuelles Netzwerk | **myVirtualNetwork** |
| | Subnet | **Privat (10.0.1.0/24)** |
| | Öffentliche IP-Adresse | Standardwert |
| Verwaltung | Diagnosespeicherkonto | **mynvastorageaccount** |

## <a name="route-traffic-through-an-nva"></a>Weiterleiten von Datenverkehr über ein virtuelles Netzwerkgerät

### <a name="sign-in-to-myvmprivate-over-remote-desktop"></a>Melden bei „myVmPrivate“ per Remotedesktop

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com), um Ihre private VM zu verwalten. Suchen Sie nach **Virtuelle Computer**, und wählen Sie diese Option aus.

1. Wählen Sie den Namen Ihrer privaten VM (**myVmPrivate**) aus.

1. Wählen Sie in der Menüleiste der VM **Verbinden** aus, um eine Remotedesktopverbindung mit der privaten VM herzustellen.

1. Wählen Sie auf der Seite **Verbindung mit RDP herstellen** die Option **RDP-Datei herunterladen** aus. Azure erstellt eine Remotedesktopprotokoll-Datei (*RDP*) und lädt sie auf Ihren Computer herunter.

1. Öffnen Sie die heruntergeladene *RDP*-Datei. Wenn Sie dazu aufgefordert werden, wählen Sie **Verbinden** aus. Wählen Sie **Weitere Optionen** > **Anderes Konto verwenden** aus, und geben Sie dann den Benutzernamen und das Kennwort ein, die Sie beim Erstellen der privaten VM angegeben haben.

1. Klicken Sie auf **OK**.

1. Wenn während des Anmeldevorgangs eine Zertifikatwarnung angezeigt wird, klicken Sie auf **Ja**, um die Verbindung mit der VM herzustellen.

### <a name="enable-icmp-through-the-windows-firewall"></a>Aktivieren Sie ICMP über die Windows-Firewall.

In einem späteren Schritt wird das Routing mithilfe des Traceroute-Tools getestet. Traceroute verwendet das Internet Control Message-Protokoll (ICMP). Dieses Protokoll wird durch die Windows-Firewall standardmäßig blockiert. Aktivieren Sie ICMP über die Windows-Firewall.

1. Öffnen Sie auf dem Remotedesktop von *myVmPrivate* PowerShell.

1. Geben Sie diesen Befehl ein:

    ```powershell
    New-NetFirewallRule –DisplayName "Allow ICMPv4-In" –Protocol ICMPv4
    ```

    Sie verwenden Traceroute, um das Routing in diesem Tutorial zu testen. Für Produktionsumgebungen empfehlen wir nicht, ICMP über die Windows-Firewall zuzulassen.

### <a name="turn-on-ip-forwarding-within-myvmnva"></a>Aktivieren der IP-Weiterleitung in myVmNva

Sie haben [die IP-Weiterleitung](#turn-on-ip-forwarding) für die Netzwerkschnittstelle des virtuellen Computers mit Azure aktiviert. Das Betriebssystem des virtuellen Computers muss Netzwerkdatenverkehr ebenfalls weiterleiten. Mit diesen Befehlen aktivieren Sie die IP-Weiterleitung für das Betriebssystem des virtuellen Computers *myVmNva*.

1. Öffnen Sie über eine Eingabeaufforderung auf dem virtuellen Computer *myVmPrivate* eine Remotedesktopverbindung mit dem virtuellen Computer *myVmNva*:

    ```cmd
    mstsc /v:myvmnva
    ```

1. Geben Sie in PowerShell auf der VM *myVmNva* den folgenden Befehl ein, um die IP-Weiterleitung zu aktivieren:

    ```powershell
    Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters -Name IpEnableRouter -Value 1
    ```

1. Starten Sie die VM *myVmNva* neu: Wählen Sie in der Taskleiste **Start** > **Netzschaltersymbol**, **Andere (Geplant)**  > **Fortsetzen** aus.

    Dadurch wird auch die Remotedesktopsitzung getrennt.

1. Nachdem der virtuelle Computer *myVmNva* neu gestartet wurde, erstellen Sie eine Remotedesktopsitzung für den virtuellen Computer *myVmPublic*. Öffnen Sie, solange Sie noch mit der *myVmPrivate* verbunden sind, eine Eingabeaufforderung und führen Sie diesen Befehl aus:

    ```cmd
    mstsc /v:myVmPublic
    ```
1. Öffnen Sie auf dem Remotedesktop von *myVmPublic* PowerShell.

1. Aktivieren Sie ICMP für die Windows-Firewall, indem Sie den folgenden Befehl eingeben:

    ```powershell
    New-NetFirewallRule –DisplayName "Allow ICMPv4-In" –Protocol ICMPv4
    ```

## <a name="test-the-routing-of-network-traffic"></a>Testen des Routings von Netzwerkdatenverkehr

Zunächst testen wir das Routing des Netzwerkdatenverkehrs von *myVmPublic* zu *myVmPrivate*.

1. Geben Sie von PowerShell auf *myVmPublic* diesen Befehl ein:

    ```powershell
    tracert myVmPrivate
    ```

    Die Antwort ähnelt diesem Beispiel:

    ```powershell
    Tracing route to myVmPrivate.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.1.4]
    over a maximum of 30 hops:

    1    <1 ms     *        1 ms  10.0.2.4
    2     1 ms     1 ms     1 ms  10.0.1.4

    Trace complete.
    ```

    Wie Sie sehen, ist der erste Hop 10.0.2.4, d. h. die private IP-Adresse des virtuellen Netzwerksgeräts. Der zweite Hop ist die private IP-Adresse der VM *myVmPrivate*: 10.0.1.4. Zuvor haben Sie die Route der Routingtabelle *myRouteTablePublic* hinzugefügt und dem Subnetz *Öffentlich* zugeordnet. Infolgedessen sendete Azure den Netzwerkdatenverkehr über das Netzwerkgerät und nicht direkt an das Subnetz *Privat*.

1. Schließen Sie die Remotedesktopsitzung mit der VM *myVmPublic*, sodass Sie nur noch mit der VM *myVmPrivate* verbunden sind.

1. Geben Sie über eine Eingabeaufforderung auf der VM *myVmPrivate* den folgenden Befehl ein:

    ```cmd
    tracert myVmPublic
    ```

    Dieser Befehl testet das Routing des Netzwerkdatenverkehrs von der VM *myVmPrivate* zur VM *myVmPublic*. Die Antwort ähnelt diesem Beispiel:

    ```cmd
    Tracing route to myVmPublic.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.0.4]
    over a maximum of 30 hops:

    1     1 ms     1 ms     1 ms  10.0.0.4

    Trace complete.
    ```

    Wie Sie sehen, leitet Azure Netzwerkdatenverkehr von *myVmPrivate* direkt zu *myVmPublic* weiter. Azure-Routen leiten standardmäßig Datenverkehr direkt durch Subnetze weiter.

1. Schließen Sie die Remotedesktopsitzung mit der VM *myVmPrivate*.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcengruppe *myResourceGroup* mit allen zugehörigen Ressourcen, wenn Sie sie nicht mehr benötigen:

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com), um Ihre Ressourcengruppe zu verwalten. Suchen Sie nach **Ressourcengruppen**, und wählen Sie diese Option aus.

1. Wählen Sie den Namen Ihrer Ressourcengruppe (**myResourceGroup**) aus.

1. Wählen Sie die Option **Ressourcengruppe löschen**.

1. Geben Sie im Bestätigungsdialogfeld *myResourceGroup* in das Feld **Geben Sie den Ressourcengruppennamen ein** ein, und klicken Sie dann auf **Löschen**. Azure löscht *myResourceGroup* und alle Ressourcen, die an diese Ressourcengruppe gebunden sind, einschließlich Ihrer Routingtabellen, Speicherkonten, virtuellen Netzwerke, VMs, Netzwerkschnittstellen und öffentlichen IP-Adressen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine Routingtabelle erstellt und einem Subnetz zugeordnet. Sie haben ein einfaches virtuelles Netzwerkgerät erstellt, das Datenverkehr von einem öffentlichen Subnetz an ein privates Subnetz weiterleitet. Nun können Sie verschiedene vorkonfigurierte NVAs mit vielen nützlichen Netzwerkfunktionen aus dem [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking) bereitstellen. Weitere Informationen zum Routing finden Sie unter [Routing von Datenverkehr für virtuelle Netzwerke](virtual-networks-udr-overview.md) und [Erstellen, Ändern oder Löschen einer Routingtabelle](manage-route-table.md).

Sie können zwar viele Azure-Ressourcen in einem virtuellen Netzwerk bereitstellen, für einige kann Azure jedoch keine Ressourcen für einige PaaS-Dienste in einem virtuellen Netzwerk bereitstellen. Sie können den Zugriff auf die Ressourcen einiger Azure-PaaS-Dienste einschränken, dies ist jedoch nur für Datenverkehr aus dem Subnetz eines virtuellen Netzwerks möglich. Im nächsten Tutorial erfahren Sie, wie Sie den Netzwerkzugriff auf Azure-PaaS-Ressourcen einschränken.

> [!div class="nextstepaction"]
> [Beschränken des Netzwerkzugriffs auf PaaS-Ressourcen](tutorial-restrict-network-access-to-resources.md)

> [!NOTE] 
> Azure-Dienste kosten Geld. Mit Azure Cost Management können Sie Budgets festlegen und Warnungen konfigurieren, um die Kontrolle über Ihre Ausgaben zu behalten. Analysieren, verwalten und optimieren Sie Ihre Azure-Kosten mit Cost Management. Weitere Informationen finden Sie in der [Schnellstartanleitung zur Kostenanalyse](../cost-management-billing/costs/quick-acm-cost-analysis.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn).