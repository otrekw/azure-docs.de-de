---
title: 'Tutorial: Erstellen eines Lastenausgleichs mit mehreren Verfügbarkeitsgruppen im Back-End-Pool – Azure-Portal'
titleSuffix: Azure Load Balancer
description: In diesem Tutorial stellen Sie eine Azure Load Balancer-Instanz mit mehreren Verfügbarkeitsgruppen im Back-End-Pool bereit.
author: asudbring
ms.author: allensu
ms.service: load-balancer
ms.topic: tutorial
ms.date: 04/21/2021
ms.custom: template-tutorial
ms.openlocfilehash: 71115da01f47572d77243f25204d5b1127db22cd
ms.sourcegitcommit: 5ce88326f2b02fda54dad05df94cf0b440da284b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2021
ms.locfileid: "107887160"
---
# <a name="tutorial-create-a-load-balancer-with-more-than-one-availability-set-in-the-backend-pool-using-the-azure-portal"></a>Tutorial: Erstellen eines Lastenausgleichs mit mehreren Verfügbarkeitsgruppen im Back-End-Pool mithilfe des Azure-Portals

Im Rahmen einer Hochverfügbarkeitsbereitstellung werden virtuelle Computer häufig in mehrere Verfügbarkeitsgruppen gruppiert. 

Load Balancer unterstützt mehrere Verfügbarkeitsgruppen bei virtuellen Computern im Back-End-Pool.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines virtuellen Netzwerks
> * Erstellen eines NAT-Gateways für ausgehende Konnektivität
> * Erstellen einer Azure Load Balancer-Instanz mit der Standard-SKU
> * Erstellen von vier virtuellen Computern und zwei Verfügbarkeitsgruppen
> * Hinzufügen virtueller Computer in Verfügbarkeitsgruppen zum Back-End-Pool des Lastenausgleichs
> * Testen des Lastenausgleichs

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

In diesem Abschnitt erstellen Sie ein virtuelles Netzwerk für den Lastenausgleich und die anderen in diesem Tutorial verwendeten Ressourcen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Geben Sie im Suchfeld oben im Portal den Suchbegriff **Virtuelles Netzwerk** ein.

3. Wählen Sie in den Suchergebnissen **Virtuelle Netzwerke** aus.

4. Wählen Sie **+ Erstellen** aus.

5. Geben Sie unter **Virtuelles Netzwerkerstellen** auf der Registerkarte **Grundlagen** die folgenden Informationen an, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ------|
    | **Projektdetails** |   |
    | Subscription | Wählen Sie Ihr Abonnement aus. |
    | Ressourcengruppe | Wählen Sie **Neu erstellen**. </br> Geben Sie unter **Name** den Namen **TutorLBmultiAVS-rg** ein. |
    | **Instanzendetails** |   |
    | Name | Geben Sie **myVNet** ein. |
    | Region | Wählen Sie **USA, Westen 2** aus. |

6. Wählen Sie die Registerkarte **IP-Adressen** oder unten auf der Seite die Schaltfläche **Weiter: IP-Adressen** aus.

7. Wählen Sie auf der Registerkarte **IP-Adressen** unter **Subnetzname** die Einstellung **Standard** aus.

8. Geben Sie im Bereich **Subnetz bearbeiten** unter **Subnetzname** den Namen **myBackendSubnet** ein.

9. Wählen Sie **Speichern**.

10. Wählen Sie die Registerkarte **Sicherheit** oder unten auf der Seite die Schaltfläche **Weiter: Sicherheit** aus.

11. Wählen Sie unter **BastionHost** auf der Registerkarte **Sicherheit** die Option **Aktivieren** aus.

12. Geben Sie die folgenden Informationen an:

    | Einstellung | Wert |
    | ------- | ----- |
    | Bastion-Name | Geben Sie **MyBastionHost** ein. |
    | AzureBastionSubnet-Adressraum | Geben Sie **10.1.1.0/27** ein. |
    | Öffentliche IP-Adresse | Wählen Sie **Neu erstellen**. </br> Geben Sie unter **Name** den Namen **myBastionIP** ein. |

13. Wählen Sie die Registerkarte **Überprüfen + erstellen** oder unten auf der Seite die blaue Schaltfläche **Überprüfen + erstellen** aus.

14. Klicken Sie auf **Erstellen**.

## <a name="create-nat-gateway"></a>Erstellen eines NAT Gateways 

In diesem Abschnitt erstellen Sie ein NAT-Gateway für ausgehende Konnektivität der virtuellen Computer.

1. Geben Sie am oberen Rand des Portals den Suchbegriff **NAT-Gateway** in das Suchfeld ein.

2. Wählen Sie in den Suchergebnissen **NAT-Gateways** aus.

3. Wählen Sie **+ Erstellen** aus.

4. Geben Sie unter **Gateway für die Netzwerkadressübersetzung (NAT) erstellen** auf der Registerkarte **Grundlagen** die folgenden Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | **Projektdetails** |   |
    | Subscription | Wählen Sie Ihr Abonnement aus. |
    | Ressourcengruppe | Wählen Sie **TutorLBmultiAVS-rg** aus. |
    | **Instanzendetails** |   |
    | Name des NAT-Gateways | Geben Sie **myNATgateway** ein. |
    | Region | Wählen Sie **USA, Westen 2** aus. |
    | Verfügbarkeitszone | Wählen Sie **Keine**. |
    | Leerlaufzeitüberschreitung (Minuten) | Geben Sie **15** ein. |

5. Wählen Sie die Registerkarte **Ausgehende IP-Adresse** oder unten auf der Seite die Schaltfläche **Weiter: Ausgehende IP-Adresse** aus.

6. Wählen Sie auf der Registerkarte **Ausgehende IP-Adresse** neben **Öffentliche IP-Adressen** die Option **Neue öffentliche IP-Adresse erstellen** aus.

7. Geben Sie unter **Name** den Namen **myPublicIP-nat** ein.

8. Klicken Sie auf **OK**.

9. Wählen Sie die Registerkarte **Subnetz** oder unten auf der Seite die Schaltfläche **Weiter: Subnetz** aus.

10. Wählen Sie unter **Virtuelles Netzwerk** im Pulldownmenü den Eintrag **myVNet** aus.

11. Aktivieren Sie das Kontrollkästchen neben **myBackendSubnet**.

12. Wählen Sie die Registerkarte **Überprüfen + erstellen** oder unten auf der Seite die blaue Schaltfläche **Überprüfen + erstellen** aus.

13. Klicken Sie auf **Erstellen**.

## <a name="create-load-balancer"></a>Erstellen eines Load Balancers

In diesem Abschnitt wird ein Lastenausgleich für die virtuellen Computer erstellt.

1. Geben Sie am oberen Rand des Portals den Suchbegriff **Lastenausgleich** in das Suchfeld ein.

2. Wählen Sie in den Suchergebnissen **Lastenausgleichsmodule** aus.

3. Wählen Sie **+ Erstellen** aus.

4. Geben Sie unter **Lastenausgleich erstellen** auf der Registerkarte **Grundlagen** die folgenden Informationen ein, bzw. wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | **Projektdetails** |   |
    | Subscription | Wählen Sie Ihr Abonnement aus. |
    | Ressourcengruppe | Wählen Sie **TutorLBmultiAVS-rg** aus. |
    | **Instanzendetails** |   |
    | Name | Geben Sie **myLoadBalancer** ein. |
    | Region | Wählen Sie **USA, Westen 2** aus. |
    | type | Übernehmen Sie den Standardwert **Öffentlich**. |
    | SKU | Übernehmen Sie den Standardwert **Standard**. |
    | Tarif | Übernehmen Sie den Standardwert **Regional**. |
    | **Öffentliche IP-Adresse** |   |
    | Öffentliche IP-Adresse | Übernehmen Sie die Standardeinstellung **Neu erstellen**. |
    | Name der öffentlichen IP-Adresse | Geben Sie **myPublicIP-lb** ein. |
    | Verfügbarkeitszone | Wählen Sie **Zonenredundant** aus. |
    | Öffentliche IPv6-Adresse hinzufügen | Übernehmen Sie den Standardwert **Nein**. |
    | Routingpräferenz | Übernehmen Sie die Standardeinstellung **Microsoft-Netzwerk**. |

5. Wählen Sie die Registerkarte **Überprüfen + erstellen** oder unten auf der Seite die blaue Schaltfläche **Überprüfen + erstellen** aus.

6. Klicken Sie auf **Erstellen**.

### <a name="configure-load-balancer-settings"></a>Konfigurieren der Lastenausgleichseinstellungen

In diesem Abschnitt erstellen Sie einen Back-End-Pool für **myLoadBalancer**.

Sie erstellen einen Integritätstest, um **HTTP** und **Port 80** zu überwachen. Der Integritätstest überwacht die Integrität der virtuellen Computer im Back-End-Pool. 

Sie erstellen eine Lastenausgleichsregel für **Port 80** mit deaktivierter SNAT für ausgehenden Datenverkehr. Das zuvor von Ihnen erstellte NAT-Gateway verarbeitet die ausgehende Konnektivität der virtuellen Computer.

1. Geben Sie am oberen Rand des Portals den Suchbegriff **Lastenausgleich** in das Suchfeld ein.

2. Wählen Sie in den Suchergebnissen **Lastenausgleichsmodule** aus.

3. Wählen Sie **myLoadBalancer** aus.

4. Wählen Sie unter **myLoadBalancer** unter **Einstellungen** die Option **Back-End-Pools** aus.

5. Wählen Sie unter **Back-End-Pools** die Option **+ Hinzufügen** aus.

6. Geben Sie unter **Back-End-Pool hinzufügen** die folgenden Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | Name | Geben Sie **myBackendPool** ein. |
    | Virtuelles Netzwerk | Wählen Sie **myVNet** aus. |
    | Konfiguration des Back-End-Pools | Übernehmen Sie den Standardwert **NIC**. |
    | IP-Version | Übernehmen Sie den Standardwert **IPv4**. |

7. Wählen Sie **Hinzufügen** aus.

8. Wählen Sie **Integritätstests** aus.

9. Wählen Sie **+ Hinzufügen**.

10. Geben Sie unter **Integritätstest hinzufügen** die folgenden Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | Name | Geben Sie **myHTTPProbe** ein. |
    | Protocol | Wählen Sie **HTTP** aus. |
    | Port | Übernehmen Sie den Standardwert **80**. |
    | Pfad | Übernehmen Sie den Standardwert **/** . |
    | Intervall | Übernehmen Sie den Standardwert **5** Sekunden. |
    | Fehlerhafter Schwellenwert | Übernehmen Sie den Standardwert **2** für aufeinanderfolgende Fehler. |

11. Wählen Sie **Hinzufügen** aus.

12. Wählen Sie **Lastenausgleichsregeln** aus. 

13. Wählen Sie **+ Hinzufügen**.

14. Geben Sie unter **Lastenausgleichsregel hinzufügen** die folgenden Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | Name | Geben Sie **myHTTPRule** ein. |
    | IP-Version | Übernehmen Sie den Standardwert **IPv4**. |
    | Front-End-IP-Adresse | Wählen Sie **LoadBalancerFrontEnd** aus. |
    | Protokoll | Wählen Sie die Standardoption **TCP** aus. |
    | Port | Geben Sie **80** ein. |
    | Back-End-Port | Geben Sie **80** ein. |
    | Back-End-Pool | Wählen Sie **myBackendPool** aus. |
    | Integritätstest | Wählen Sie **myHTTPProbe** aus. |
    | Sitzungspersistenz | Übernehmen Sie den Standardwert **Keine**. |
    | Leerlaufzeitüberschreitung (Minuten) | Schieben Sie den Schieberegler auf **15**. |
    | TCP-Zurücksetzung | Wählen Sie **Aktiviert**. |
    | Unverankerte IP | Übernehmen Sie den Standardwert **Deaktiviert**. |
    | Übersetzung der Quellnetzwerkadresse (SNAT) für ausgehenden Datenverkehr | Übernehmen Sie die Standardeinstellung **(Empfohlen) Verwenden Sie Ausgangsregeln, um Back-End-Poolmitgliedern Zugriff auf das Internet zu gewähren**. |

5. Wählen Sie **Hinzufügen** aus.

## <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern

In diesem Abschnitt erstellen Sie zwei Verfügbarkeitsgruppen mit zwei virtuellen Computern pro Gruppe. Diese Computer werden während der Erstellung dem Back-End-Pool des Lastenausgleichs hinzugefügt. 

### <a name="create-first-set-of-vms"></a>Erstellen der ersten Gruppe virtueller Computer

1. Wählen Sie im Portal im Abschnitt oben links die Option **+ Ressource erstellen** aus.

2. Wählen Sie unter **Neu** die Optionen **Compute** > **Virtueller Computer** aus.

3. Geben Sie unter **Virtuellen Computer erstellen** auf der Registerkarte **Grundlagen** die folgenden Informationen ein, bzw. wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | **Projektdetails** |   |
    | Subscription | Wählen Sie Ihr Abonnement aus. |
    | Ressourcengruppe | Wählen Sie **TutorLBmultiAVS-rg** aus. |
    | **Instanzendetails** |   |
    | Name des virtuellen Computers | Geben Sie **myVM1** ein. |
    | Region | Wählen Sie **USA, Westen 2** aus. |
    | Verfügbarkeitsoptionen | Wählen Sie **Verfügbarkeitsgruppe** aus. |
    | Verfügbarkeitsgruppe | Wählen Sie **Neu erstellen**. </br> Geben Sie unter **Name** die Zeichenfolge **myAvailabilitySet1** ein. </br> Klicken Sie auf **OK**. |
    | Image | Wählen Sie **Windows Server 2019 Datacenter – Gen1** aus. |
    | Azure Spot-Instanz | Übernehmen Sie die Standardeinstellung (deaktiviert). |
    | Size | Wählen Sie eine Größe für den virtuellen Computer aus. |
    | **Administratorkonto** |   |
    | Username | Geben Sie einen Benutzernamen ein. |
    | Kennwort | Geben Sie ein Kennwort ein. |
    | **Regeln für eingehende Ports** |   |
    | Öffentliche Eingangsports | Wählen Sie **Keine**. |

4. Wählen Sie die Registerkarte **Netzwerk** oder unten auf der Seite die Schaltfläche **Weiter: Datenträger** und dann **Weiter: Netzwerk** aus.

5. Geben Sie auf der Registerkarte **Netzwerk** die folgenden Informationen ein, bzw. wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | **Netzwerkschnittstelle** |   |
    | Virtuelles Netzwerk | Wählen Sie **myVNet** aus. |
    | Subnet | Wählen Sie **myBackendSubnet** aus. |
    | Öffentliche IP-Adresse | Wählen Sie **Keine** aus. |
    | NIC-Netzwerksicherheitsgruppe | Wählen Sie **Erweitert** aus. |
    | Konfigurieren von Netzwerksicherheitsgruppen | Wählen Sie **Neu erstellen**. </br> Geben Sie unter **Name** den Namen **myNSG** ein. </br> Wählen Sie unter **Regeln für eingehenden Datenverkehr** die Option **+ Eingangsregel hinzufügen** aus. </br> Wählen Sie unter **Dienst** die Option **HTTP** aus. </br> Geben Sie unter **Name** den Namen **myHTTPrule** ein. </br> Wählen Sie **Hinzufügen** aus. </br> Klicken Sie auf **OK**. | 
    | **Lastenausgleich** |   |
    | Diese VM hinter einer vorhandenen Lastenausgleichslösung platzieren? | Aktivieren Sie das Kontrollkästchen. |
    | **Einstellungen für den Lastenausgleich** |   |
    | Optionen für den Lastenausgleich | Wählen Sie **Azure-Lastenausgleich** aus. |
    | Wählen Sie einen Lastenausgleich aus. | Wählen Sie **myLoadBalancer** aus. |
    | Wählen Sie einen Back-End-Pool aus. | Wählen Sie **myBackendPool** aus. |

6. Wählen Sie die Registerkarte **Überprüfen + erstellen** oder unten auf der Seite die blaue Schaltfläche **Überprüfen + erstellen** aus.

7. Klicken Sie auf **Erstellen**.

8. Wiederholen Sie die Schritte 1 bis 7, um den zweiten virtuellen Computer der Gruppe zu erstellen. Ersetzen Sie die Einstellungen für den virtuellen Computer durch die folgenden Informationen:

    | Einstellung | Wert |
    | ------- | ----- |
    | Name | Geben Sie **myVM2** ein. |
    | Verfügbarkeitsgruppe | Wählen Sie **myAvailabilitySet1** aus. |
    | Virtual Network | Wählen Sie **myVNet** aus. |
    | Subnet | Wählen Sie **myBackendSubnet** aus. |
    | Öffentliche IP-Adresse | Wählen Sie **Keine** aus. |
    | Netzwerksicherheitsgruppe | Wählen Sie **myNSG** aus. |
    | Optionen für den Lastenausgleich | Wählen Sie **Azure-Lastenausgleich** aus. |
    | Wählen Sie einen Lastenausgleich aus. | Wählen Sie **myLoadBalancer** aus. |
    | Wählen Sie einen Back-End-Pool aus. | Wählen Sie **myBackendPool** aus. |

### <a name="create-second-set-of-vms"></a>Erstellen einer zweiten Gruppe virtueller Computer

1. Wählen Sie im Portal im Abschnitt oben links die Option **+ Ressource erstellen** aus.

2. Wählen Sie unter **Neu** die Optionen **Compute** > **Virtueller Computer** aus.

3. Geben Sie unter **Virtuellen Computer erstellen** auf der Registerkarte **Grundlagen** die folgenden Informationen ein, bzw. wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | **Projektdetails** |   |
    | Subscription | Wählen Sie Ihr Abonnement aus. |
    | Ressourcengruppe | Wählen Sie **TutorLBmultiAVS-rg** aus. |
    | **Instanzendetails** |   |
    | Name des virtuellen Computers | Geben Sie **myVM3** ein. |
    | Region | Wählen Sie **USA, Westen 2** aus. |
    | Verfügbarkeitsoptionen | Wählen Sie **Verfügbarkeitsgruppe** aus. |
    | Verfügbarkeitsgruppe | Wählen Sie **Neu erstellen**. </br> Geben Sie unter **Name** die Zeichenfolge **myAvailabilitySet2** ein. </br> Klicken Sie auf **OK**. |
    | Image | Wählen Sie **Windows Server 2019 Datacenter – Gen1** aus. |
    | Azure Spot-Instanz | Übernehmen Sie die Standardeinstellung (deaktiviert). |
    | Size | Wählen Sie eine Größe für den virtuellen Computer aus. |
    | **Administratorkonto** |   |
    | Username | Geben Sie einen Benutzernamen ein. |
    | Kennwort | Geben Sie ein Kennwort ein. |
    | **Regeln für eingehende Ports** |   |
    | Öffentliche Eingangsports | Wählen Sie **Keine**. |

4. Wählen Sie die Registerkarte **Netzwerk** oder unten auf der Seite die Schaltfläche **Weiter: Datenträger** und dann **Weiter: Netzwerk** aus.

5. Geben Sie auf der Registerkarte **Netzwerk** die folgenden Informationen ein, bzw. wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | **Netzwerkschnittstelle** |   |
    | Virtuelles Netzwerk | Wählen Sie **myVNet** aus. |
    | Subnet | Wählen Sie **myBackendSubnet** aus. |
    | Öffentliche IP-Adresse | Wählen Sie **Keine** aus. |
    | NIC-Netzwerksicherheitsgruppe | Wählen Sie **Erweitert** aus. |
    | Konfigurieren von Netzwerksicherheitsgruppen | Wählen Sie **myNSG** aus. | 
    | **Lastenausgleich** |   |
    | Diese VM hinter einer vorhandenen Lastenausgleichslösung platzieren? | Aktivieren Sie das Kontrollkästchen. |
    | **Einstellungen für den Lastenausgleich** |   |
    | Optionen für den Lastenausgleich | Wählen Sie **Azure-Lastenausgleich** aus. |
    | Wählen Sie einen Lastenausgleich aus. | Wählen Sie **myLoadBalancer** aus. |
    | Wählen Sie einen Back-End-Pool aus. | Wählen Sie **myBackendPool** aus. |

6. Wählen Sie die Registerkarte **Überprüfen + erstellen** oder unten auf der Seite die blaue Schaltfläche **Überprüfen + erstellen** aus.

7. Klicken Sie auf **Erstellen**.

8. Wiederholen Sie die Schritte 1 bis 7, um den zweiten virtuellen Computer der Gruppe zu erstellen. Ersetzen Sie die Einstellungen für den virtuellen Computer durch die folgenden Informationen:

    | Einstellung | Wert |
    | ------- | ----- |
    | Name | Geben Sie **myVM4** ein. |
    | Verfügbarkeitsgruppe | Wählen Sie **myAvailabilitySet2** aus. |
    | Virtual Network | Wählen Sie **myVNet** aus. |
    | Netzwerksicherheitsgruppe | Wählen Sie **myNSG** aus. |
    | Optionen für den Lastenausgleich | Wählen Sie **Azure-Lastenausgleich** aus. |
    | Wählen Sie einen Lastenausgleich aus. | Wählen Sie **myLoadBalancer** aus. |
    | Wählen Sie einen Back-End-Pool aus. | Wählen Sie **myBackendPool** aus. |

## <a name="install-iis"></a>Installieren von IIS

In diesem Abschnitt verwenden Sie den zuvor erstellten Azure Bastion-Host, um eine Verbindung mit den virtuellen Computern herzustellen und IIS zu installieren.

1. Geben Sie im Suchfeld oben im Portal den Suchbegriff **Virtueller Computer** ein.

2. Wählen Sie in den Suchergebnissen **Virtuelle Computer** aus.

3. Wählen Sie **myVM1** aus.

4. Wählen Sie auf der Seite **Übersicht** von myVM1 die Option **Verbinden** > **Bastion** aus.

5. Klicken Sie auf **Bastion verwenden**.

6. Geben Sie die Werte für **Benutzername** und **Kennwort** an, die Sie beim Erstellen des virtuellen Computers erstellt haben.

7. Wählen Sie **Verbinden**.

7. Navigieren Sie auf dem Serverdesktop zu **Windows-Verwaltungsprogramme** > **Windows PowerShell**.

8. Führen Sie im PowerShell-Fenster die im Anschluss angegebenen Befehle aus. Diese bewirken Folgendes:

    * Installieren des IIS-Servers
    * Entfernen der Standarddatei „iisstart.htm“
    * Hinzufügen einer neuen Datei namens „iisstart.htm“ zum Anzeigen des Namens des virtuellen Computers

   ```powershell
    # Install IIS server role
    Install-WindowsFeature -name Web-Server -IncludeManagementTools
    
    # Remove default htm file
    Remove-Item  C:\inetpub\wwwroot\iisstart.htm
    
    # Add a new htm file that displays server name
    Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from " + $env:computername)
   ```
8. Schließen Sie die Bastion-Sitzung mit **myVM1**.

9. Wiederholen Sie die Schritte 1 bis 8 für **myVM2**, **myVM3** und **myVM4**.

## <a name="test-the-load-balancer"></a>Testen des Lastenausgleichs

In diesem Abschnitt ermitteln Sie die öffentliche IP-Adresse des Lastenausgleichs. Sie verwenden die IP-Adresse, um den Lastenausgleich zu testen.

1. Geben Sie am oberen Rand des Portals den Suchbegriff **Öffentliche IP-Adresse** in das Suchfeld ein.

2. Wählen Sie in den Suchergebnissen **Öffentliche IP-Adressen** aus.

3. Wählen Sie **myPublicIP-lb** aus.

4. Notieren Sie sich die öffentliche IP-Adresse, die auf der Seite **Übersicht** von **myPublicIP-lb** unter **IP-Adresse** aufgeführt ist:

    :::image type="content" source="./media/tutorial-multi-availability-sets-portal/find-public-ip.png" alt-text="Ermitteln der öffentlichen IP-Adresse des Lastenausgleichs" border="true":::

5. Öffnen Sie einen Webbrowser, und geben Sie die öffentliche IP-Adresse in die Adressleiste ein:

    :::image type="content" source="./media/tutorial-multi-availability-sets-portal/verify-load-balancer.png" alt-text="Testen des Lastenausgleichs mit dem Webbrowser" border="true":::

6. Wählen Sie im Browser „Aktualisieren“ aus, damit ein Lastenausgleich für den Datenverkehr auf die anderen virtuellen Computer im Back-End-Pool durchgeführt wird.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Falls Sie diese Anwendung nicht weiterverwenden möchten, sollten Sie den Lastenausgleich und die unterstützenden Ressourcen wie folgt löschen:

1. Geben Sie oben im Portal im Suchfeld **Ressourcengruppe** ein.

2. Wählen Sie in den Suchergebnissen **Ressourcengruppen** aus.

3. Wählen Sie **TutorLBmultiAVS-rg** aus.

4. Wählen Sie auf der Seite „Übersicht“ von **TutorLBmultiAVS-rg** die Option **Ressourcengruppe löschen** aus.

5. Geben Sie unter **RESSOURCENGRUPPENNAMEN EINGEBEN** den Namen **TutorLBmultiAVS-rg** ein.

6. Klicken Sie auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial führen Sie Folgendes durch:

* Erstellen eines virtuellen Netzwerks und eines Azure Bastion-Hosts
* Erstellen einer Azure Load Balancer Standard-Instanz
* Erstellen von zwei Verfügbarkeitsgruppen mit zwei virtuellen Computern pro Gruppe
* Installieren von IIS und Testen des Lastenausgleichs

Fahren Sie mit dem nächsten Artikel fort, um sich über das Erstellen einer regionsübergreifenden Azure Load Balancer-Instanz zu informieren:
> [!div class="nextstepaction"]
> [Erstellen eines regionsübergreifenden Lastenausgleichs](tutorial-cross-region-portal.md)

