---
title: 'Schnellstart: Erstellen einer öffentlichen Load Balancer-Instanz – Azure-Portal'
titleSuffix: Azure Load Balancer
description: In diesem Schnellstart wird gezeigt, wie Sie über das Azure-Portal eine Load Balancer-Instanz erstellen.
services: load-balancer
documentationcenter: na
author: asudbring
manager: twooley
Customer intent: I want to create a Load Balancer so that I can load balance internet traffic to VMs.
ms.service: load-balancer
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/08/2020
ms.author: allensu
ms.custom: mvc
ms.openlocfilehash: b424fe315737b84479283eed2d77398c8ce4f148
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/26/2020
ms.locfileid: "78898830"
---
# <a name="quickstart-create-a-load-balancer-to-load-balance-vms-using-the-azure-portal"></a>Schnellstart: Erstellen einer Load Balancer-Instanz für den Lastenausgleich virtueller Computer über das Azure-Portal

Durch die Verteilung der eingehenden Anforderungen auf mehrere virtuelle Computer bietet ein Lastenausgleich ein höheres Maß an Verfügbarkeit und Skalierbarkeit. Sie können das Azure-Portal verwenden, um einen Load Balancer für den Lastenausgleich virtueller Computer (Virtual Machines, VMs) zu erstellen. In dieser Schnellstartanleitung erfahren Sie, wie Sie für den Lastenausgleich virtueller Computer eine öffentliche Load Balancer-Instanz verwenden.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen. 

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.

## <a name="create-a-load-balancer"></a>Einrichten einer Load Balancer-Instanz

In diesem Abschnitt erstellen Sie eine Load Balancer-Instanz für den Lastenausgleich virtueller Computer. Sie können eine öffentliche oder eine interne Load Balancer-Instanz erstellen. Wenn Sie eine öffentliche Load Balancer-Instanz erstellen, müssen Sie für diese auch eine neue öffentliche IP-Adresse erstellen, die als Front-End konfiguriert ist. Dieses hat standardmäßig den Namen *LoadBalancerFrontend*.

1. Wählens Sie links oben auf dem Bildschirm die Optionen **Ressource erstellen** > **Netzwerk** > **Lastenausgleich** aus.
2. Geben Sie auf der Seite **Lastenausgleich erstellen** auf der Registerkarte **Grundlagen** die folgenden Informationen ein, oder wählen Sie sie aus, übernehmen Sie die Standardwerte für die übrigen Einstellungen, und klicken Sie auf **Überprüfen + erstellen**:

    | Einstellung                 | Wert                                              |
    | ---                     | ---                                                |
    | Subscription               | Wählen Sie Ihr Abonnement aus.    |    
    | Resource group         | Wählen Sie **Neu erstellen** aus, und geben Sie *myResourceGroupSLB* in das Textfeld ein.|
    | Name                   | *myLoadBalancer*                                   |
    | Region         | Wählen Sie **Europa, Westen** aus.                                        |
    | type          | Wählen Sie **Öffentlich** aus.                                        |
    | SKU           | Wählen Sie **Standard** oder **Basic** aus. Microsoft empfiehlt für Produktionsworkloads die Option „Standard“. |
    | Öffentliche IP-Adresse | Wählen Sie **Neu erstellen**. Wenn Sie über eine vorhandene öffentliche IP-Adresse verfügen, die Sie verwenden möchten, wählen Sie **Vorhandene verwenden** aus. |
    | Name der öffentlichen IP-Adresse              | Geben Sie *myPublicIP* in das Textfeld ein.   Verwenden Sie ```-SKU Basic```, um eine öffentliche IP-Adresse vom Typ „Basic“ zu erstellen. Öffentliche IP-Adressen des Typs „Basic“ sind nicht mit einem Lastenausgleich des Typs **Standard** kompatibel. Microsoft empfiehlt für Produktionsworkloads die Option **Standard**.|
    | Verfügbarkeitszone | Geben Sie *Zonenredundant* ein, um eine resiliente Load Balancer-Instanz zu erstellen. Wählen Sie zum Erstellen einer zonalen Load Balancer-Instanz eine bestimmte Zone aus 1, 2 oder 3 aus. |

> [!IMPORTANT]
> Im weiteren Verlauf dieser Schnellstartanleitung wird davon ausgegangen, dass bei der SKU-Auswahl oben die SKU **Standard** ausgewählt wurde.


3. Wählen Sie auf der Registerkarte **Bewerten + erstellen** die Option **Erstellen** aus.   

    ![Einrichten eines Load Balancers im Tarif „Standard“](./media/quickstart-load-balancer-standard-public-portal/create-standard-load-balancer.png)

## <a name="create-load-balancer-resources"></a>Erstellen von Load Balancer-Ressourcen

In diesem Abschnitt konfigurieren Sie Load Balancer-Einstellungen für einen Back-End-Adresspool und einen Integritätstest. Außerdem legen Sie eine Lastenausgleichsregel fest.

### <a name="create-a-backend-pool"></a>Erstellen eines Back-End-Pools

Ein Back-End-Adresspool enthält die IP-Adressen der virtuellen NICs, die mit dem Load Balancer verbunden sind, um Datenverkehr an die virtuellen Computer verteilen zu können. Erstellen Sie den Back-End-Adresspool *myBackendPool*, um virtuelle Computer für den Lastenausgleich von Internetdatenverkehr einzubeziehen.

1. Wählen Sie im linken Menü **Alle Dienste** > **Alle Ressourcen** und anschließend in der Ressourcenliste den Eintrag **myLoadBalancer** aus.
2. Wählen Sie unter **Einstellungen** die Option **Back-End-Pools** und dann **Hinzufügen** aus.
3. Geben Sie auf der Seite **Back-End-Pool hinzufügen** die Zeichenfolge *myBackendPool* als Name für Ihren Back-End-Pool ein, und wählen Sie anschließend **Hinzufügen** aus.

### <a name="create-a-health-probe"></a>Erstellen eines Integritätstests

Damit der Load Balancer den Status Ihrer App überwachen kann, verwenden Sie einen Integritätstest. Abhängig von der Reaktion auf Integritätsüberprüfungen werden der Load Balancer-Rotation durch den Integritätstest dynamisch virtuelle Computer hinzugefügt oder daraus entfernt. Erstellen Sie zur Überwachung der Integrität der virtuellen Computer einen Integritätstest namens *myHealthProbe*.

1. Wählen Sie im linken Menü **Alle Dienste** > **Alle Ressourcen** und anschließend in der Ressourcenliste den Eintrag **myLoadBalancer** aus.
2. Wählen Sie unter **Einstellungen** die Option **Integritätstests** und dann **Hinzufügen** aus.
    
    | Einstellung | Wert |
    | ------- | ----- |
    | Name | Geben Sie *myHealthProbe* ein. |
    | Protocol | Wählen Sie **HTTP** aus. |
    | Port | Geben Sie *80* ein.|
    | Intervall | Geben Sie für das **Intervall** den Wert *15* (Sekunden zwischen Testversuchen) ein. |
    | Fehlerhafter Schwellenwert | Wählen Sie **2** als Wert für den **Fehlerschwellenwert** bzw. als Anzahl aufeinander folgender Testfehler aus, die auftreten müssen, damit ein virtueller Computer als fehlerhaft eingestuft wird.|
    | | |
4. Klicken Sie auf **OK**.

### <a name="create-a-load-balancer-rule"></a>Erstellen einer Load Balancer-Regel
Mithilfe einer Load Balancer-Regel wird definiert, wie Datenverkehr auf die virtuellen Computer verteilt werden soll. Sie definieren die Front-End-IP-Konfiguration für den eingehenden Datenverkehr und den Back-End-IP-Pool zum Empfangen des Datenverkehrs zusammen mit dem erforderlichen Quell- und Zielport. Erstellen Sie eine Load Balancer-Regel namens *myLoadBalancerRuleWeb*, die am Port 80 des Front-Ends *FrontendLoadBalancer* lauscht und den Netzwerkdatenverkehr nach erfolgtem Lastenausgleich an den Back-End-Adresspool *myBackEndPool* sendet, wobei ebenfalls der Port 80 verwendet wird. 

1. Wählen Sie im linken Menü **Alle Dienste** > **Alle Ressourcen** und anschließend in der Ressourcenliste den Eintrag **myLoadBalancer** aus.
2. Wählen Sie unter **Einstellungen** die Option **Lastenausgleichsregeln** und dann **Hinzufügen** aus.
3. Konfigurieren Sie die Lastenausgleichsregel mit folgenden Werten:
    
    | Einstellung | Wert |
    | ------- | ----- |
    | Name | Geben Sie *myHTTPRule* ein. |
    | Protocol | Wählen Sie **TCP** aus. |
    | Port | Geben Sie *80* ein.|
    | Back-End-Port | Geben Sie *80* ein. |
    | Back-End-Pool | Wählen Sie *myBackendPool* aus.|
    | Integritätstest | Wählen Sie *myHealthProbe* aus. |
4. Übernehmen Sie die übrigen Standardeinstellungen, und wählen Sie dann **OK** aus.


## <a name="create-backend-servers"></a>Erstellen von Back-End-Servern

In diesem Abschnitt erstellen Sie ein virtuelles Netzwerk sowie drei virtuelle Computer für den Back-End-Pool des Load Balancers und installieren anschließend IIS auf den virtuellen Computern, um den Load Balancer zu testen.

## <a name="virtual-network-and-parameters"></a>Virtuelles Netzwerk und Parameter

In den Schritten dieses Abschnitts müssen die folgenden Parameter wie folgt ersetzt werden:

| Parameter                   | Wert                |
|-----------------------------|----------------------|
| **\<Ressourcengruppenname>**  | myResourceGroupSLB |
| **\<VNET-Name>** | myVNet          |
| **\<Regionsname>**          | Europa, Westen      |
| **\<IPv4-Adressraum>**   | 10.1.0.0\16          |
| **\<Subnetzname>**          | myBackendSubnet        |
| **\<Subnetzadressbereich>** | 10.1.0.0\24          |

[!INCLUDE [virtual-networks-create-new](../../includes/virtual-networks-create-new.md)]

### <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern
Die SKUs für öffentliche IP-Adressen und die Load Balancer-SKUs müssen übereinstimmen. Verwenden Sie für Load Balancer Standard virtuelle Computer mit Standard-IP-Adressen im Back-End-Pool. In diesem Abschnitt werden drei virtuelle Computer (*myVM1*, *myVM2* und *myVM3*) mit einer öffentlichen Standard-IP-Adresse in drei verschiedenen Zonen (*Zone 1*, *Zone 2* und *Zone 3*) erstellt, die später dem Back-End-Pool der zuvor erstellten Load Balancer-Instanz hinzugefügt werden. Wenn Sie „Basic“ ausgewählt haben, verwenden Sie virtuelle Computer mit IP-Adressen vom Typ „Basic“.

1. Wählen Sie oben links im Portal die Option **Ressource erstellen** > **Compute** > **Windows Server 2019 Datacenter**. 
   
1. Geben Sie unter **Virtuellen Computer erstellen** auf der Registerkarte **Grundlagen** die folgenden Werte ein (bzw. wählen Sie sie aus):
   - **Abonnement** > **Ressourcengruppe**: Wählen Sie **myResourceGroupSLB** aus.
   - **Instanzendetails** > **Name des virtuellen Computers**: Geben Sie *myVM1* ein.
   - **Instanzendetails** > **Region**: Wählen Sie **Europa, Westen** aus.
   - **Instanzendetails** > **Verfügbarkeitsoptionen**: Wählen Sie **Verfügbarkeitszonen** aus. 
   - **Instanzendetails** > **Verfügbarkeitszone**: Wählen Sie **1** aus.
   - **Administratorkonto**> Geben Sie Informationen für **Benutzernamen**, **Kennwort** und **Kennwort bestätigen** ein.
   - Wählen Sie die Registerkarte **Netzwerk** aus, oder wählen Sie **Weiter: Datenträger** und anschließend **Weiter: Netzwerk** aus.
  
1. Stellen Sie auf der Registerkarte **Netzwerk** sicher, dass Folgendes ausgewählt ist:
   - **Virtuelles Netzwerk:** *myVnet*
   - **Subnetz:** *myBackendSubnet*
   - **Öffentliche IP-Adresse:** Wählen Sie **Neu erstellen** und anschließend im Fenster **Öffentliche IP-Adresse erstellen** unter **SKU** die Option **Standard** und unter **Verfügbarkeitszone** die Option **Zonenredundant** und dann **OK** aus. Wenn Sie eine Load Balancer-Instanz vom Typ „Basic“ erstellt haben, wählen Sie „Basic“ aus. Microsoft empfiehlt für Produktionsworkloads die Verwendung der Standard-SKU.
   - Wählen Sie zum Erstellen einer neuen Netzwerksicherheitsgruppe (NSG) – einer Art Firewall – unter **Netzwerksicherheitsgruppe** die Option **Erweitert**. 
       1. Wählen Sie im Feld **Netzwerksicherheitsgruppe konfigurieren** die Option **Neu erstellen**. 
       1. Geben Sie *myNetworkSecurityGroup* ein, und wählen Sie **OK** aus.
   - Führen Sie die folgenden Schritte aus, um den virtuellen Computer dem Back-End-Pool des Load Balancers hinzuzufügen:
        - Wählen Sie unter **Lastenausgleich** für **Diese VM hinter einer vorhandenen Lastenausgleichslösung platzieren?** die Option **Ja** aus.
        - Wählen Sie in den **Lastenausgleichseinstellungen** unter **Optionen für den Lastenausgleich** die Option **Azure Load Balancer** aus.
        - Wählen Sie unter **Load Balancer auswählen** die Option *myLoadBalancer* aus.
        - Wählen Sie die Registerkarte **Verwaltung** oder **Weiter** > **Verwaltung**.
2. Legen Sie auf der Registerkarte **Verwaltung** unter **Überwachung** die Option **Startdiagnose** auf **Aus** fest. 
1. Klicken Sie auf **Überprüfen + erstellen**.   
1. Überprüfen Sie die Einstellungen, und wählen Sie dann die Option **Erstellen**.
1. Führen Sie die Schritte 2 bis 6 aus, um zwei zusätzliche VMs mit folgenden Werten zu erstellen. Verwenden Sie für alle übrigen Einstellungen die Werte von *myVM1*:

    | Einstellung | VM 2| VM 3|
    | ------- | ----- |---|
    | Name |  *myVM2* |*myVM3*|
    | Verfügbarkeitszone | 2 |3|
    |Öffentliche IP-Adresse| **Standard**-SKU:|**Standard**-SKU:|
    | Öffentliche IP-Adresse – Verfügbarkeitszone| **Zonenredundant** |**Zonenredundant**|
    | Netzwerksicherheitsgruppe | Wählen Sie die vorhandene *myNetworkSecurity-Gruppe* aus.| Wählen Sie die vorhandene *myNetworkSecurity-Gruppe* aus.|

 ### <a name="create-nsg-rule"></a>Erstellen der NSG-Regel

In diesem Abschnitt erstellen Sie eine Netzwerksicherheitsgruppen-Regel, um eingehende Verbindungen über HTTP zuzulassen.

1. Wählen Sie im linken Menü **Alle Dienste** > **Alle Ressourcen** und anschließend in der Ressourcenliste den Eintrag **myNetworkSecurityGroup** (in der Ressourcengruppe **myResourceGroupSLB**) aus.
2. Wählen Sie unter **Einstellungen** die Option **Eingangssicherheitsregeln** und dann **Hinzufügen**.
3. Geben Sie für die Eingangssicherheitsregel *myHTTPRule* folgende Werte ein, um eine eingehende HTTP-Verbindung über den Port 80 zuzulassen:
    - **Quelle**: *Diensttag*
    -  **Quelldiensttag**: *Internet*
    - **Zielportbereiche**: *80*
    - **Protokoll:** *TCP*
    - **Aktion:** *Zulassen*
    - **Priorität:** *100*
    - **Name**: *myHTTPRule* 
    - **Beschreibung**: *HTTP zulassen* 
4. Wählen Sie **Hinzufügen**.
5. Wiederholen Sie ggf. die Schritte für die RDP-Eingangsregel mit den folgenden abweichenden Werten:
   - **Zielportbereiche**: Geben Sie *3389* ein.
   - **Priorität:** Geben Sie *200* ein. 
   - **Name**: Geben Sie *MyRDPRule* ein. 
   - **Beschreibung**: Geben Sie *Allow RDP* ein. 
 
### <a name="install-iis"></a>Installieren von IIS

1. Wählen Sie im linken Menü **Alle Dienste** > **Alle Ressourcen** und anschließend in der Ressourcenliste den Eintrag **myVM1** (in der Ressourcengruppe *myResourceGroupSLB*) aus.
2. Wählen Sie auf der Seite **Übersicht** die Option **Verbinden**, um eine RDP-Verbindung mit dem virtuellen Computer herzustellen.
5. Melden Sie sich mit den Anmeldeinformationen, die Sie während der Erstellung dieses virtuellen Computers bereitgestellt haben, beim virtuellen Computer an. Dadurch wird eine Remotedesktopsitzung mit dem virtuellen Computer *myVM1* gestartet.
6. Navigieren Sie auf dem Serverdesktop zu **Windows-Verwaltungsprogramme**>**Windows PowerShell**.
7. Führen Sie im PowerShell-Fenster die folgenden Befehle aus, um den IIS-Server zu installieren, die Standarddatei „iisstart.htm“ zu entfernen und dann eine neue Datei „iisstart.htm“ hinzuzufügen, die den Namen des virtuellen Computers enthält:

   ```azurepowershell-interactive
    
    # install IIS server role
    Install-WindowsFeature -name Web-Server -IncludeManagementTools
    
    # remove default htm file
     remove-item  C:\inetpub\wwwroot\iisstart.htm
    
    # Add a new htm file that displays server name
     Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from " + $env:computername)
   ```
6. Schließen Sie die RDP-Sitzung mit *myVM1*.
7. Wiederholen Sie die Schritte 1 bis 6, um IIS und die aktualisierte Datei „iisstart.htm“ auf *myVM2* und *myVM3* zu installieren.

## <a name="test-the-load-balancer"></a>Testen des Load Balancers
1. Ermitteln Sie auf dem Bildschirm **Übersicht** die öffentliche IP-Adresse für den Load Balancer. Wählen Sie im linken Menü **Alle Dienste** > **Alle Ressourcen** und anschließend **myPublicIP** aus.

2. Kopieren Sie die öffentliche IP-Adresse, und fügen Sie sie in die Adressleiste des Browsers ein. Die Standardseite des IIS-Webservers wird im Browser angezeigt.

   ![IIS-Webserver](./media/tutorial-load-balancer-standard-zonal-portal/load-balancer-test.png)

Um den verteilten Load Balancer-Datenverkehr auf allen drei virtuellen Computern anzuzeigen, können Sie die Standardseite des IIS-Webservers einzelner virtueller Computer anpassen und dann die Aktualisierung Ihres Webbrowsers auf dem Clientcomputer durchsetzen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcengruppe, den Load Balancer und alle zugehörigen Ressourcen, wenn Sie sie nicht mehr benötigen. Wählen Sie hierzu die Ressourcengruppe (*myResourceGroupSLB*) aus, die den Load Balancer enthält, und wählen Sie anschließend **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie eine Load Balancer Standard-Instanz erstellt, virtuelle Computer angefügt, die Datenverkehrsregel für den Load Balancer sowie einen Integritätstest konfiguriert und den Load Balancer getestet. Weitere Informationen zu Azure Load Balancer finden Sie in den [Tutorials zu Azure Load Balancer](tutorial-load-balancer-standard-public-zone-redundant-portal.md).

Weitere Informationen zu Load Balancer und Verfügbarkeitszonen finden Sie [hier](load-balancer-standard-availability-zones.md).
