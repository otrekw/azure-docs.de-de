---
title: 'Tutorial: Erstellen eines öffentlichen Lastenausgleichs mit einem IP-basierten Back-End – Azure-Portal'
titleSuffix: Azure Load Balancer
description: In diesem Tutorial erfahren Sie, wie Sie einen öffentlichen Lastenausgleich mit einem IP-basierten Back-End-Pool erstellen.
author: asudbring
ms.author: allensu
ms.service: load-balancer
ms.topic: tutorial
ms.date: 3/31/2021
ms.custom: template-tutorial
ms.openlocfilehash: f8174d46d1674e0cf81e36e89f6fb6180091a9b9
ms.sourcegitcommit: 9f4510cb67e566d8dad9a7908fd8b58ade9da3b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/01/2021
ms.locfileid: "106123015"
---
# <a name="tutorial-create-a-public-load-balancer-with-an-ip-based-backend-using-the-azure-portal"></a>Tutorial: Erstellen eines öffentlichen Lastenausgleichs mit einem IP-basierten Back-End über das Azure-Portal

In diesem Tutorial erfahren Sie, wie Sie einen öffentlichen Lastenausgleich mit einem IP-basierten Back-End-Pool erstellen. 

Bei einer herkömmlichen Bereitstellung von Azure Load Balancer wird die Netzwerkschnittstelle der virtuellen Computer verwendet. Bei einem IP-basierten Back-End werden die virtuellen Computer dem Back-End nach IP-Adresse hinzugefügt.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines virtuellen Netzwerks
> * Erstellen eines NAT-Gateways für ausgehende Konnektivität
> * Erstellen einer Azure Load Balancer-Instanz
> * Erstellen eines IP-basierten Back-End-Pools
> * Erstellen von zwei virtuellen Computern
> * Testen des Lastenausgleichs
## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

In diesem Abschnitt erstellen Sie ein virtuelles Netzwerk für den Lastenausgleich sowie ein NAT-Gateway und virtuelle Computer.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Wählen Sie links oben auf dem Bildschirm **Ressource erstellen > Netzwerk > Virtuelles Netzwerk** aus, oder suchen Sie über das Suchfeld nach **Virtuelles Netzwerk**.

2. Klicken Sie auf **Erstellen**. 

3. Geben Sie unter **Virtuelles Netzwerk erstellen** auf der Registerkarte **Grundlegende Einstellungen** die folgenden Informationen ein, oder wählen Sie sie aus:

    | **Einstellung**          | **Wert**                                                           |
    |------------------|-----------------------------------------------------------------|
    | **Projektdetails**  |                                                                 |
    | Subscription     | Auswählen des Azure-Abonnements                                  |
    | Ressourcengruppe   | Wählen Sie **TutorPubLBIP-rg** aus. |
    | **Instanzendetails** |                                                                 |
    | Name             | Geben Sie **myVNet** ein.                                    |
    | Region           | Wählen Sie **USA, Osten** aus. |

4. Wählen Sie die Registerkarte **IP-Adressen** oder die Schaltfläche **Weiter: IP-Adressen** am unteren Seitenrand aus.

5. Geben Sie auf der Registerkarte **IP-Adressen** die folgenden Informationen ein:

    | Einstellung            | Wert                      |
    |--------------------|----------------------------|
    | IPv4-Adressraum | Geben Sie **10.1.0.0/16** ein. |

6. Wählen Sie unter **Subnetzname** das Wort **Standard** aus.

7. Geben Sie unter **Subnetz bearbeiten** die folgenden Informationen ein:

    | Einstellung            | Wert                      |
    |--------------------|----------------------------|
    | Subnetzname | Geben Sie **myBackendSubnet** ein. |
    | Subnetzadressbereich | Geben Sie **10.1.0.0/24** ein. |

8. Wählen Sie **Speichern** aus.

9. Wählen Sie die Registerkarte **Sicherheit** .

10. Wählen Sie unter **BastionHost** die Option **Aktivieren** aus. Geben Sie die folgenden Informationen ein:

    | Einstellung            | Wert                      |
    |--------------------|----------------------------|
    | Bastion-Name | Geben Sie **myBastionHost** ein. |
    | AzureBastionSubnet-Adressraum | Geben Sie **10.1.1.0/27** ein. |
    | Öffentliche IP-Adresse | Wählen Sie **Neu erstellen**. </br> Geben Sie **myBastionIP** als **Name** ein. </br> Klicken Sie auf **OK**. |


11. Wählen Sie die Registerkarte **Überprüfen + erstellen** oder die Schaltfläche **Überprüfen + erstellen** aus.

12. Klicken Sie auf **Erstellen**.
## <a name="create-nat-gateway"></a>Erstellen eines NAT Gateways

In diesem Abschnitt wird ein NAT-Gateway erstellt und dem Subnetz in dem virtuellen Netzwerk zugewiesen, das Sie zuvor erstellt haben.

1. Wählen Sie links oben auf dem Bildschirm **Ressource erstellen > Netzwerk> NAT-Gateway** aus, oder suchen Sie über das Suchfeld nach **NAT-Gateway**.

2. Klicken Sie auf **Erstellen**. 

3. Geben Sie auf der Registerkarte **Grundlagen** unter **Gateway für die Netzwerkadressübersetzung (NAT) erstellen** die folgenden Informationen ein, oder wählen Sie sie aus:

    | **Einstellung**          | **Wert**                                                           |
    |------------------|-----------------------------------------------------------------|
    | **Projektdetails**  |                                                                 |
    | Subscription     | Wählen Sie Ihr Azure-Abonnement.                                  |
    | Ressourcengruppe   | Wählen Sie **Neu erstellen** aus, und geben Sie **TutorPubLBIP-rg** in das Textfeld ein. </br> Klicken Sie auf **OK**. |
    | **Instanzendetails** |                                                                 |
    | Name             | Geben Sie **myNATgateway** ein.                                    |
    | Region           | Wählen Sie **USA, Osten** aus.  |
    | Verfügbarkeitszone | Wählen Sie **Keine**. |
    | Leerlaufzeitüberschreitung (Minuten) | Geben Sie **10** ein. |

4. Wählen Sie die Registerkarte **Ausgehende IP-Adresse** oder unten auf der Seite die Schaltfläche **Weiter: Ausgehende IP-Adresse** aus.

5. Geben Sie auf der Registerkarte **Ausgehende IP-Adresse** die folgenden Informationen ein, bzw. wählen Sie sie aus:

    | **Einstellung** | **Wert** |
    | ----------- | --------- |
    | Öffentliche IP-Adressen | Wählen Sie **Neue öffentliche IP-Adresse erstellen** aus. </br> Geben Sie unter **Name** den Namen **myPublicIP-NAT** ein. </br> Klicken Sie auf **OK**. |

6. Wählen Sie die Registerkarte **Subnetz** oder unten auf der Seite die Schaltfläche **Weiter: Subnetz** aus.

7. Wählen Sie auf der Registerkarte **Subnetz** in der Dropdownliste **Virtuelles Netzwerk** die Option **myVNet** aus.

8. Aktivieren Sie das Kontrollkästchen neben **myBackendSubnet**.

9. Wählen Sie die Registerkarte **Überprüfen + erstellen** oder unten auf der Seite die blaue Schaltfläche **Überprüfen + erstellen** aus.

10. Klicken Sie auf **Erstellen**.
## <a name="create-load-balancer"></a>Erstellen eines Load Balancers

In diesem Abschnitt wird eine Azure Load Balancer Standard-Instanz erstellt. 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie **Ressource erstellen**. 
3. Geben Sie im Suchfeld **Load Balancer** ein. Wählen Sie in den Suchergebnissen **Load Balancer** aus.
4. Wählen Sie auf der Seite **Load Balancer** die Option **Erstellen** aus.
5. Geben Sie auf der Seite **Lastenausgleich erstellen** die folgenden Informationen ein, bzw. wählen Sie sie aus: 

    | Einstellung                 | Wert                                              |
    | ---                     | ---                                                |
    | **Projektdetails** |   |
    | Subscription               | Wählen Sie Ihr Abonnement aus.    |    
    | Ressourcengruppe         | Wählen Sie **TutorPubLBIP-rg** aus.|
    | **Instanzendetails** |   |
    | Name                   | Geben Sie **myLoadBalancer** ein.                                   |
    | Region         | Wählen Sie **(USA) USA, Osten** aus.                                        |
    | type          | Wählen Sie **Öffentlich** aus.                                        |
    | SKU           | Übernehmen Sie den Standardwert **Standard**. |
    | Tarif          | Übernehmen Sie den Standardwert für **Region**. |
    | **Öffentliche IP-Adresse** |   |
    | Öffentliche IP-Adresse | Wählen Sie **Neu erstellen**. </br> Wenn Sie über eine vorhandene öffentliche IP-Adresse verfügen, die Sie verwenden möchten, wählen Sie **Vorhandene verwenden** aus. |
    | Name der öffentlichen IP-Adresse | Geben Sie **myPublicIP-LB** in das Textfeld ein.|
    | Verfügbarkeitszone | Wählen Sie **Zonenredundant** aus, um einen resilienten Lastenausgleich zu erstellen. Wählen Sie zum Erstellen eines zonalen Lastenausgleichs eine bestimmte Zone aus 1, 2 oder 3 aus. |
    | Öffentliche IPv6-Adresse hinzufügen | Wählen Sie **Nein** aus. </br> Weitere Informationen zu IPv6-Adressen und zum Lastenausgleich finden Sie unter [Was ist IPv6 für Azure Virtual Network?](../virtual-network/ipv6-overview.md).  |
    | Routingpräferenz | Übernehmen Sie die Standardeinstellung **Microsoft-Netzwerk**. </br> Weitere Informationen zu den Routingpräferenzen finden Sie unter [Was ist Routingpräferenz (Vorschau)?](../virtual-network/routing-preference-overview.md) |

6. Übernehmen Sie bei den anderen Einstellungen die Standardwerte, und wählen Sie **Überprüfen + erstellen** aus.

7. Wählen Sie auf der Registerkarte **Bewerten + erstellen** die Option **Erstellen** aus.

## <a name="create-load-balancer-resources"></a>Erstellen von Load Balancer-Ressourcen

In diesem Abschnitt wird Folgendes konfiguriert:

* Lastenausgleichseinstellungen für einen Back-End-Adresspool
* Ein Integritätstest
* Eine Lastenausgleichsregel

### <a name="create-a-backend-pool"></a>Erstellen eines Back-End-Pools

Der Back-End-Adresspool enthält die IP-Adressen der virtuellen NICs, die mit dem Lastenausgleich verbunden sind. 

Erstellen Sie den Back-End-Adresspool **myBackendPool**, um virtuelle Computer für den Lastenausgleich von Internetdatenverkehr einzubeziehen.

1. Wählen Sie im linken Menü **Alle Dienste** > **Alle Ressourcen** und anschließend in der Ressourcenliste den Eintrag **myLoadBalancer** aus.

2. Wählen Sie unter **Einstellungen** die Option **Back-End-Pools** und dann **+ Hinzufügen** aus.

3. Geben Sie auf der Seite **Back-End-Pool hinzufügen** die folgenden Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | Name | Geben Sie **myBackendPool** ein. |
    | Virtuelles Netzwerk | Wählen Sie **myVNet** aus. |
    | Konfiguration des Back-End-Pools | Wählen Sie **IP-Adresse** aus. |
    | IP-Version | Wählen Sie **IPv4** aus. |

4. Wählen Sie **Hinzufügen**.

### <a name="create-a-health-probe"></a>Erstellen eines Integritätstests

Der Status Ihrer App wird vom Lastenausgleich mithilfe eines Integritätstests überwacht. 

Abhängig von der Reaktion auf Integritätsüberprüfungen werden der Load Balancer-Instanz durch den Integritätstest virtuelle Computer hinzugefügt oder daraus entfernt. 

Erstellen Sie zur Überwachung der Integrität der virtuellen Computer einen Integritätstest mit dem Namen **myHealthProbe**.

1. Wählen Sie im linken Menü **Alle Dienste** > **Alle Ressourcen** und anschließend in der Ressourcenliste den Eintrag **myLoadBalancer** aus.

2. Wählen Sie unter **Einstellungen** die Option **Integritätstests** und dann **+ Hinzufügen** aus.
    
    | Einstellung | Wert |
    | ------- | ----- |
    | Name | Geben Sie **myHealthProbe** ein. |
    | Protocol | Wählen Sie **TCP** aus. |
    | Port | Geben Sie **80** ein.|
    | Intervall | Geben Sie für das **Intervall** den Wert **15** (Sekunden zwischen Testversuchen) ein. |
    | Fehlerhafter Schwellenwert | Wählen Sie **2**. |
   
3. Lassen Sie die restlichen Einstellungen unverändert, und wählen Sie **OK** aus.

### <a name="create-a-load-balancer-rule"></a>Erstellen einer Load Balancer-Regel

Mithilfe einer Load Balancer-Regel wird definiert, wie Datenverkehr auf die virtuellen Computer verteilt werden soll. Sie definieren die Front-End-IP-Konfiguration für den eingehenden Datenverkehr und den Back-End-IP-Pool für den Empfang des Datenverkehrs. Quell- und Zielport werden in der Regel definiert. 

In diesem Abschnitt wird eine Lastenausgleichsregel mit folgenden Merkmalen erstellt:

* Sie heißt **myHTTPRule**.
* Sie befindet sich im Front-End **LoadBalancerFrontEnd**.
* Sie lauscht a **Port 80**.
* Sie leitet vom Lastenausgleich verteilten Datenverkehr an den **Port 80** des Back-Ends **myBackendPool** weiter.

1. Wählen Sie im linken Menü **Alle Dienste** > **Alle Ressourcen** und anschließend in der Ressourcenliste den Eintrag **myLoadBalancer** aus.

2. Wählen Sie unter **Einstellungen** die Option **Lastenausgleichsregeln** und dann **+ Hinzufügen** aus.

3. Geben Sie die folgenden Informationen für die Lastenausgleichsregel ein, oder wählen Sie sie aus:
    
    | Einstellung | Wert |
    | ------- | ----- |
    | Name | Geben Sie **myHTTPRule** ein. |
    | IP-Version | Wählen Sie **IPv4** aus. |
    | Front-End-IP-Adresse | Wählen Sie **LoadBalancerFrontEnd** aus. |
    | Protocol | Wählen Sie **TCP** aus. |
    | Port | Geben Sie **80** ein.|
    | Back-End-Port | Geben Sie **80** ein. |
    | Back-End-Pool | Wählen Sie **myBackendPool** aus.|
    | Integritätstest | Wählen Sie **myHealthProbe** aus. |
    | Sitzungspersistenz | Übernehmen Sie den Standardwert **Keine**. |
    | Leerlaufzeitüberschreitung (Minuten) | Geben Sie **15** Minuten ein. |
    | TCP-Zurücksetzung | Wählen Sie **Aktiviert**. |
    | Unverankerte IP | Wählen Sie **Deaktiviert** aus. |
    | Übersetzung der Quellnetzwerkadresse (SNAT) für ausgehenden Datenverkehr | Wählen Sie **(Empfohlen) Verwenden Sie Ausgangsregeln, um Back-End-Poolmitgliedern Zugriff auf das Internet zu gewähren** aus. |

4. Übernehmen Sie die übrigen Standardeinstellungen, und wählen Sie **Hinzufügen** aus.

## <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern

In diesem Abschnitt werden zwei virtuelle Computer (**myVM1** und **myVM2**) in zwei verschiedenen Zonen (**Zone 1** und **Zone 2**) erstellt.

Diese virtuellen Computer werden dem Back-End-Pool des zuvor erstellten Lastenausgleichs hinzugefügt.

1. Navigieren Sie links oben im Portal zu **Ressource erstellen** > **Compute** > **Virtueller Computer**. 
   
2. Geben Sie unter **Virtuellen Computer erstellen** auf der Registerkarte **Grundlagen** die folgenden Werte ein, oder wählen Sie sie aus:

    | Einstellung | Wert                                          |
    |-----------------------|----------------------------------|
    | **Projektdetails** |  |
    | Subscription | Auswählen des Azure-Abonnements |
    | Ressourcengruppe | Wählen Sie **TutorPubLBIP-rg** aus. |
    | **Instanzendetails** |  |
    | Name des virtuellen Computers | Geben Sie **myVM1** ein. |
    | Region | Wählen Sie **USA, Osten** aus. |
    | Verfügbarkeitsoptionen | Wählen Sie **Verfügbarkeitszonen** aus. |
    | Verfügbarkeitszone | Wählen Sie **1** aus. |
    | Image | Wählen Sie **Windows Server 2019 Datacenter** aus. |
    | Azure Spot-Instanz | Behalten Sie die Standardwerte bei. |
    | Size | Wählen Sie eine VM-Größe aus, oder übernehmen Sie die Standardeinstellung. |
    | **Administratorkonto** |  |
    | Username | Geben Sie einen Benutzernamen ein. |
    | Kennwort | Geben Sie ein Kennwort ein. |
    | Kennwort bestätigen | Geben Sie das Kennwort erneut ein. |
    | **Regeln für eingehende Ports** |  |
    | Öffentliche Eingangsports | Wählen Sie **Keine** aus. |

3. Wählen Sie die Registerkarte **Netzwerk** aus, oder wählen Sie **Weiter: Datenträger** und anschließend **Weiter: Netzwerk** aus.
  
4. Wählen Sie auf der Registerkarte „Netzwerk“ die folgenden Werte aus, oder geben Sie sie ein:

    | Einstellung | Wert |
    |-|-|
    | **Netzwerkschnittstelle** |  |
    | Virtuelles Netzwerk | **myVNet** |
    | Subnet | **myBackendSubnet** |
    | Öffentliche IP-Adresse | Wählen Sie **Keine** aus. |
    | NIC-Netzwerksicherheitsgruppe | Wählen Sie **Erweitert** aus.|
    | Konfigurieren von Netzwerksicherheitsgruppen | Wählen Sie **Neu erstellen**. </br> Geben Sie unter **Netzwerksicherheitsgruppe erstellen** als **Name** die Zeichenfolge **myNSG** ein. </br> Wählen Sie unter **Regeln für eingehenden Datenverkehr** die Option **+ Eingangsregel hinzufügen** aus. </br> Wählen Sie unter **Dienst** die Option **HTTP** aus. </br> Geben Sie unter **Priorität** den Wert **100** ein. </br> Geben Sie unter **Name** die Zeichenfolge **myHTTPRule** ein. </br> Wählen Sie **Hinzufügen** aus. </br> Klicken Sie auf **OK**. |
    | **Lastenausgleich**  |
    | Diese VM hinter einer vorhandenen Lastenausgleichslösung platzieren? | Aktivieren Sie das Kontrollkästchen.|
    | **Lastenausgleichseinstellungen** |
    | Optionen für den Lastenausgleich | Wählen Sie **Azure-Lastenausgleich** aus. |
    | Wählen Sie einen Lastenausgleich aus. | Wählen Sie **myLoadBalancer** aus.  |
    | Wählen Sie einen Back-End-Pool aus. | Wählen Sie **myBackendPool** aus. |
   
5. Klicken Sie auf **Überprüfen + erstellen**. 
  
6. Überprüfen Sie die Einstellungen, und wählen Sie dann die Option **Erstellen**.

7. Führen Sie die Schritte 1 bis 6 aus, um einen virtuellen Computer mit den unten angegebenen Werten zu erstellen. Verwenden Sie für alle übrigen Einstellungen die Werte von **myVM1**:

    | Einstellung | VM 2 |
    | ------- | ----- |
    | Name |  **myVM2** |
    | Verfügbarkeitszone | **2** |
    | Netzwerksicherheitsgruppe | Wählen Sie die vorhandene Netzwerksicherheitsgruppe (**myNSG**) aus.| 

## <a name="install-iis"></a>Installieren von IIS

1. Wählen Sie im linken Menü **Alle Dienste** > **Alle Ressourcen** und anschließend in der Ressourcenliste den Eintrag **myVM1** in der Ressourcengruppe **TutorPubLBIP-rg** aus.

2. Wählen Sie auf der Seite **Übersicht** die Option **Verbinden** und dann **Bastion** aus.

3. Wählen Sie die Schaltfläche **Bastion verwenden** aus.

4. Geben Sie den Benutzernamen und das Kennwort ein, die Sie bei der VM-Erstellung verwendet haben.

5. Wählen Sie **Verbinden**.

6. Navigieren Sie auf dem Serverdesktop zu **Windows-Verwaltungsprogramme** > **Windows PowerShell**.

7. Führen Sie im PowerShell-Fenster die im Anschluss angegebenen Befehle aus. Diese bewirken Folgendes:

    * Installieren des IIS-Servers
    * Entfernen der Standarddatei „iisstart.htm“
    * Hinzufügen einer neuen Datei namens „iisstart.htm“ zum Anzeigen des Namens des virtuellen Computers

   ```powershell
    # install IIS server role
    Install-WindowsFeature -name Web-Server -IncludeManagementTools
    
    # remove default htm file
    Remove-Item C:\inetpub\wwwroot\iisstart.htm
    
    # Add a new htm file that displays server name
    Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from " + $env:computername)
   ```
8. Schließen Sie die Bastion-Sitzung mit **myVM1**.

9. Wiederholen Sie die Schritte 1 bis 7, um IIS und die aktualisierte Datei „iisstart.htm“ auf **myVM2** zu installieren.

## <a name="test-the-load-balancer"></a>Testen des Lastenausgleichs

1. Ermitteln Sie auf dem Bildschirm **Übersicht** die öffentliche IP-Adresse für den Load Balancer. Wählen Sie im linken Menü **Alle Dienste** > **Alle Ressourcen** und anschließend **myPublicIP-LB** aus.

2. Kopieren Sie die öffentliche IP-Adresse, und fügen Sie sie in die Adressleiste des Browsers ein. Die Standardseite des IIS-Webservers wird im Browser angezeigt.

   ![IIS-Webserver](./media/tutorial-load-balancer-standard-zonal-portal/load-balancer-test.png)

Führen Sie vom Clientcomputer aus eine erzwungene Aktualisierung Ihres Webbrowsers durch, um zu verfolgen, wie der Lastenausgleich den Datenverkehr auf myVM2 verteilt.
## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Falls Sie diese Anwendung nicht mehr benötigen, löschen Sie die das virtuelle Netzwerk, den virtuellen Computer und das NAT-Gateway wie folgt:

1. Wählen Sie im linken Menü die Option **Ressourcengruppen** aus.

2. Wählen Sie die Ressourcengruppe **TutorPubLBIP-rg** aus.

3. Wählen Sie die Option **Ressourcengruppe löschen**.

4. Geben Sie **TutorPubLBIP-rg** ein, und wählen Sie **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial führen Sie Folgendes durch:

* Erstellen eines virtuellen Netzwerks
* Erstellen eines NAT-Gateways
* Erstellen eines Lastenausgleichs mit einem IP-basierten Back-End-Pool
* Testen des Lastenausgleichs

Fahren Sie mit dem nächsten Artikel fort, um sich über das Erstellen eines regionsübergreifenden Lastenausgleichs zu informieren:
> [!div class="nextstepaction"]
> [Erstellen einer regionsübergreifenden Azure Load Balancer-Instanz mithilfe des Azure-Portals](tutorial-cross-region-portal.md)
