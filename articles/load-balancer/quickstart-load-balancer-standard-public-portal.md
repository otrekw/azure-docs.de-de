---
title: 'Schnellstart: Erstellen eines öffentlichen Lastenausgleichs: Azure-Portal'
titleSuffix: Azure Load Balancer
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie einen Lastenausgleich über das Azure-Portal erstellen.
services: load-balancer
documentationcenter: na
author: asudbring
manager: KumudD
Customer intent: I want to create a load balancer so that I can load balance internet traffic to VMs.
ms.service: load-balancer
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/17/2020
ms.author: allensu
ms.custom: mvc
ms.openlocfilehash: eb23f1e703c2e447c484ccb366914cb4b23c5bf7
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86536544"
---
# <a name="quickstart-create-a-load-balancer-to-load-balance-vms-using-the-azure-portal"></a>Schnellstart: Erstellen eines Lastenausgleichs für virtuelle Computer über das Azure-Portal

Beginnen Sie mit der Verwendung von Azure Load Balancer, indem Sie einen öffentlichen Lastenausgleich und drei virtuelle Computer über das Azure-Portal erstellen.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.

---

# <a name="option-1-default-create-a-load-balancer-standard-sku"></a>[Option 1 (Standard): Erstellen eines Lastenausgleichs (Standard-SKU)](#tab/option-1-create-load-balancer-standard)

>[!NOTE]
>Ein Lastenausgleich mit Standard-SKU wird für Produktionsworkloads empfohlen.  Weitere Informationen zu SKUs finden Sie unter **[Azure Load Balancer-SKUs](skus.md)** .

In diesem Abschnitt wird ein Lastenausgleich für virtuelle Computer erstellt. 

Sie können eine öffentliche oder eine interne Load Balancer-Instanz erstellen. 

Wenn Sie einen öffentlichen Lastenausgleich erstellen, muss für den Lastenausgleich auch eine neue, als Front-End konfigurierte öffentliche IP-Adresse erstellt werden. Dieses Front-End hat standardmäßig den Namen **LoadBalancerFrontend**.

1. Wählens Sie links oben auf dem Bildschirm die Optionen **Ressource erstellen** > **Netzwerk** > **Lastenausgleich** aus.

2. Geben Sie auf der Seite **Lastenausgleich erstellen** auf der Registerkarte **Grundlagen** die folgenden Informationen ein, bzw. wählen Sie sie aus: 

    | Einstellung                 | Wert                                              |
    | ---                     | ---                                                |
    | Subscription               | Wählen Sie Ihr Abonnement aus.    |    
    | Resource group         | Wählen Sie **Neu erstellen** aus, und geben Sie **myResourceGroupLB** in das Textfeld ein.|
    | Name                   | Geben Sie **myLoadBalancer** ein.                                   |
    | Region         | Wählen Sie **Europa, Westen** aus.                                        |
    | type          | Wählen Sie **Öffentlich** aus.                                        |
    | SKU           | Wählen Sie **Standard** aus. |
    | Öffentliche IP-Adresse | Wählen Sie **Neu erstellen**. Wenn Sie über eine vorhandene öffentliche IP-Adresse verfügen, die Sie verwenden möchten, wählen Sie **Vorhandene verwenden** aus. |
    | Name der öffentlichen IP-Adresse | Geben Sie **myPublicIP** in das Textfeld ein.|
    | Verfügbarkeitszone | Wählen Sie **Zonenredundant** aus, um einen resilienten Lastenausgleich zu erstellen. Wählen Sie zum Erstellen eines zonalen Lastenausgleichs eine bestimmte Zone aus 1, 2 oder 3 aus. |
    | Öffentliche IPv6-Adresse hinzufügen | Wählen Sie **Nein** aus. </br> Weitere Informationen zu IPv6-Adressen und zum Lastenausgleich finden Sie unter [Was ist IPv6 für Azure Virtual Network?](https://docs.microsoft.com/azure/virtual-network/ipv6-overview).  |

3. Übernehmen Sie bei den anderen Einstellungen die Standardwerte, und wählen Sie **Überprüfen + erstellen** aus.

4. Wählen Sie auf der Registerkarte **Bewerten + erstellen** die Option **Erstellen** aus.   
    
    :::image type="content" source="./media/quickstart-load-balancer-standard-public-portal/create-standard-load-balancer.png" alt-text="Erstellen einer Load Balancer Standard-Instanz" border="true":::
 
## <a name="create-load-balancer-resources"></a>Erstellen von Load Balancer-Ressourcen

In diesem Abschnitt wird Folgendes konfiguriert:

* Lastenausgleichseinstellungen für einen Back-End-Adresspool
* Ein Integritätstest
* Eine Lastenausgleichsregel und eine automatische Ausgangsregel

### <a name="create-a-backend-pool"></a>Erstellen eines Back-End-Pools

Der Back-End-Adresspool enthält die IP-Adressen der virtuellen NICs, die mit dem Lastenausgleich verbunden sind. 

Erstellen Sie den Back-End-Adresspool **myBackendPool**, um virtuelle Computer für den Lastenausgleich von Internetdatenverkehr einzubeziehen.

1. Wählen Sie im linken Menü **Alle Dienste** > **Alle Ressourcen** und anschließend in der Ressourcenliste den Eintrag **myLoadBalancer** aus.

2. Wählen Sie unter **Einstellungen** die Option **Back-End-Pools** und dann **Hinzufügen** aus.

3. Geben Sie auf der Seite **Back-End-Pool hinzufügen** die Zeichenfolge **myBackendPool** als Name für Ihren Back-End-Pool ein, und wählen Sie anschließend **Hinzufügen** aus.

### <a name="create-a-health-probe"></a>Erstellen eines Integritätstests

Der Status Ihrer App wird vom Lastenausgleich mithilfe eines Integritätstests überwacht. 

Abhängig von der Reaktion auf Integritätsüberprüfungen werden der Load Balancer-Instanz durch den Integritätstest virtuelle Computer hinzugefügt oder daraus entfernt. 

Erstellen Sie zur Überwachung der Integrität der virtuellen Computer einen Integritätstest mit dem Namen **myHealthProbe**.

1. Wählen Sie im linken Menü **Alle Dienste** > **Alle Ressourcen** und anschließend in der Ressourcenliste den Eintrag **myLoadBalancer** aus.

2. Wählen Sie unter **Einstellungen** die Option **Integritätstests** und dann **Hinzufügen** aus.
    
    | Einstellung | Wert |
    | ------- | ----- |
    | Name | Geben Sie **myHealthProbe** ein. |
    | Protocol | Wählen Sie **HTTP** aus. |
    | Port | Geben Sie **80** ein.|
    | Intervall | Geben Sie für das **Intervall** den Wert **15** (Sekunden zwischen Testversuchen) ein. |
    | Fehlerhafter Schwellenwert | Wählen Sie **2** als Wert für den **Fehlerschwellenwert** bzw. als Anzahl aufeinander folgender Testfehler aus, die auftreten müssen, damit ein virtueller Computer als fehlerhaft eingestuft wird.|
    | | |

3. Klicken Sie auf **OK**.

### <a name="create-a-load-balancer-rule"></a>Erstellen einer Load Balancer-Regel

Mithilfe einer Load Balancer-Regel wird definiert, wie Datenverkehr auf die virtuellen Computer verteilt werden soll. Sie definieren die Front-End-IP-Konfiguration für den eingehenden Datenverkehr und den Back-End-IP-Pool für den Empfang des Datenverkehrs. Quell- und Zielport werden in der Regel definiert. 

In diesem Abschnitt wird eine Lastenausgleichsregel mit folgenden Merkmalen erstellt:

* Sie heißt **myHTTPRule**.
* Sie befindet sich im Front-End **LoadBalancerFrontEnd**.
* Sie lauscht a **Port 80**.
* Sie leitet vom Lastenausgleich verteilten Datenverkehr an den **Port 80** des Back-Ends **myBackendPool** weiter.

1. Wählen Sie im linken Menü **Alle Dienste** > **Alle Ressourcen** und anschließend in der Ressourcenliste den Eintrag **myLoadBalancer** aus.

2. Wählen Sie unter **Einstellungen** die Option **Lastenausgleichsregeln** und dann **Hinzufügen** aus.

3. Konfigurieren Sie die Lastenausgleichsregel mit folgenden Werten:
    
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
    | Erstellen impliziter Ausgangsregeln | Wählen Sie **Ja** aus. </br> Weitere Informationen sowie eine erweiterte Ausgangsregelkonfiguration finden Sie hier: </br> [Ausgehende Verbindungen in Azure](load-balancer-outbound-connections.md) </br> [Konfigurieren von Lastenausgleichs- und Ausgangsregeln in Load Balancer Standard mithilfe des Azure-Portals](configure-load-balancer-outbound-portal.md)

4. Übernehmen Sie die übrigen Standardeinstellungen, und wählen Sie dann **OK** aus.

## <a name="create-backend-servers"></a>Erstellen von Back-End-Servern

In diesem Abschnitt führen Sie folgende Schritte aus:

* Erstellen eines virtuellen Netzwerks
* Erstellen von drei virtuellen Computern für den Back-End-Pool des Lastenausgleichs
* Installieren von IIS auf den virtuellen Computern, um den Lastenausgleich zu testen

## <a name="virtual-network-and-parameters"></a>Virtuelles Netzwerk und Parameter

In diesem Abschnitt müssen die Parameter in den Schritten durch die folgenden Informationen ersetzt werden:

| Parameter                   | Wert                |
|-----------------------------|----------------------|
| **\<resource-group-name>**  | myResourceGroupLB |
| **\<virtual-network-name>** | myVNet          |
| **\<region-name>**          | Europa, Westen      |
| **\<IPv4-address-space>**   | 10.1.0.0\16          |
| **\<subnet-name>**          | myBackendSubnet        |
| **\<subnet-address-range>** | 10.1.0.0\24          |

[!INCLUDE [virtual-networks-create-new](../../includes/virtual-networks-create-new.md)]

### <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern

Die SKUs für öffentliche IP-Adressen und die Lastenausgleichs-SKUs müssen übereinstimmen. Verwenden Sie für einen Standardlastenausgleich virtuelle Computer mit Standard-IP-Adressen im Back-End-Pool. 

In diesem Abschnitt werden drei virtuelle Computer (**myVM1**, **myVM2** und **myVM3**) mit einer öffentlichen Standard-IP-Adresse in drei verschiedenen Zonen (**Zone 1**, **Zone 2** und **Zone 3**) erstellt. 

Diese virtuellen Computer werden dem Back-End-Pool des zuvor erstellten Lastenausgleichs hinzugefügt.

1. Navigieren Sie links oben im Portal zu **Ressource erstellen** > **Compute** > **Virtueller Computer**. 
   
2. Geben Sie unter **Virtuellen Computer erstellen** auf der Registerkarte **Grundlagen** die folgenden Werte ein, oder wählen Sie sie aus:

    | Einstellung | Wert                                          |
    |-----------------------|----------------------------------|
    | **Projektdetails** |  |
    | Subscription | Auswählen des Azure-Abonnements |
    | Ressourcengruppe | Wählen Sie **myResourceGroupLB** aus. |
    | **Instanzendetails** |  |
    | Name des virtuellen Computers | Geben Sie **myVM1** ein. |
    | Region | Wählen Sie **Europa, Westen** aus. |
    | Verfügbarkeitsoptionen | Wählen Sie **Verfügbarkeitszonen** aus. |
    | Verfügbarkeitszone | Wählen Sie **1** aus. |
    | Image | Wählen Sie **Windows Server 2019 Datacenter** aus. |
    | Azure Spot-Instanz | Wählen Sie **Nein** aus. |
    | Size | Wählen Sie eine VM-Größe aus, oder übernehmen Sie die Standardeinstellung. |
    | **Administratorkonto** |  |
    | Username | Geben Sie einen Benutzernamen ein. |
    | Kennwort | Geben Sie ein Kennwort ein. |
    | Kennwort bestätigen | Geben Sie das Kennwort erneut ein. |

3. Wählen Sie die Registerkarte **Netzwerk** aus, oder wählen Sie **Weiter: Datenträger** und anschließend **Weiter: Netzwerk** aus.
  
4. Wählen Sie auf der Registerkarte „Netzwerk“ die folgenden Werte aus, oder geben Sie sie ein:

    | Einstellung | Wert |
    |-|-|
    | **Netzwerkschnittstelle** |  |
    | Virtuelles Netzwerk | **myVNet** |
    | Subnet | **myBackendSubnet** |
    | Öffentliche IP-Adresse | Übernehmen Sie den Standardwert **myVM-ip**. </br> Die IP-Adresse ist automatisch eine IP-Adresse der SKU „Standard“ in Zone 1. |
    | NIC-Netzwerksicherheitsgruppe | Wählen Sie **Erweitert** aus.|
    | Konfigurieren von Netzwerksicherheitsgruppen | Wählen Sie **Neu erstellen**. </br> Geben Sie unter **Netzwerksicherheitsgruppe erstellen** als **Name** die Zeichenfolge **myNSG** ein. </br> Wählen Sie unter **Regeln für eingehenden Datenverkehr** die Option **+ Eingangsregel hinzufügen** aus. </br> Geben Sie unter **Zielportbereiche** den Wert **80** ein. </br> Geben Sie unter **Priorität** den Wert **100** ein. </br> Geben Sie unter **Name** die Zeichenfolge **myHTTPRule** ein. </br> Wählen Sie **Hinzufügen** aus. </br> Klicken Sie auf **OK**. |
    | **Lastenausgleich**  |
    | Diese VM hinter einer vorhandenen Lastenausgleichslösung platzieren? | Klicken Sie auf **Ja**. |
    | **Lastenausgleichseinstellungen** |
    | Optionen für den Lastenausgleich | Wählen Sie **Azure-Lastenausgleich** aus. |
    | Wählen Sie einen Lastenausgleich aus. | Wählen Sie **myLoadBalancer** aus.  |
    | Wählen Sie einen Back-End-Pool aus. | Wählen Sie **myBackendPool** aus. |

5. Wählen Sie die Registerkarte **Verwaltung** oder **Weiter** > **Verwaltung**.

6. Wählen Sie auf der Registerkarte **Verwaltung** die folgenden Werte aus, oder geben Sie sie ein:
    
    | Einstellung | Wert |
    |-|-|
    | **Überwachung** |  |
    | Startdiagnose | Wählen Sie **Aus** aus. |
   
7. Klicken Sie auf **Überprüfen + erstellen**. 
  
8. Überprüfen Sie die Einstellungen, und wählen Sie dann die Option **Erstellen**.

9. Führen Sie die Schritte 1 bis 8 aus, um zwei weitere virtuelle Computer mit folgenden Werten zu erstellen. Verwenden Sie für alle übrigen Einstellungen die Werte von **myVM1**:

    | Einstellung | VM 2| VM 3|
    | ------- | ----- |---|
    | Name |  **myVM2** |**myVM3**|
    | Verfügbarkeitszone | **2** |**3**|
    | Netzwerksicherheitsgruppe | Wählen Sie die vorhandene Netzwerksicherheitsgruppe (**myNSG**) aus.| Wählen Sie die vorhandene Netzwerksicherheitsgruppe (**myNSG**) aus.|


# <a name="option-2-create-a-load-balancer-basic-sku"></a>[Option 2: Erstellen eines Lastenausgleichs (Basic-SKU)](#tab/option-1-create-load-balancer-basic)

>[!NOTE]
>Ein Lastenausgleich mit Standard-SKU wird für Produktionsworkloads empfohlen.  Weitere Informationen zu SKUs finden Sie unter **[Azure Load Balancer-SKUs](skus.md)** .

In diesem Abschnitt wird ein Lastenausgleich für virtuelle Computer erstellt. 

Sie können eine öffentliche oder eine interne Load Balancer-Instanz erstellen. 

Wenn Sie einen öffentlichen Lastenausgleich erstellen, muss für den Lastenausgleich auch eine neue, als Front-End konfigurierte öffentliche IP-Adresse erstellt werden. Dieses Front-End hat standardmäßig den Namen **LoadBalancerFrontend**.

1. Wählens Sie links oben auf dem Bildschirm die Optionen **Ressource erstellen** > **Netzwerk** > **Lastenausgleich** aus.

2. Geben Sie auf der Seite **Lastenausgleich erstellen** auf der Registerkarte **Grundlagen** die folgenden Informationen ein, bzw. wählen Sie sie aus: 

    | Einstellung                 | Wert                                              |
    | ---                     | ---                                                |
    | Subscription               | Wählen Sie Ihr Abonnement aus.    |    
    | Resource group         | Wählen Sie **Neu erstellen** aus, und geben Sie **myResourceGroupLB** in das Textfeld ein.|
    | Name                   | Geben Sie **myLoadBalancer** ein.                                   |
    | Region         | Wählen Sie **Europa, Westen** aus.                                        |
    | type          | Wählen Sie **Öffentlich** aus.                                        |
    | SKU           | Wählen Sie **Basic** aus. |
    | Öffentliche IP-Adresse | Wählen Sie **Neu erstellen**. Wenn Sie über eine vorhandene öffentliche IP-Adresse verfügen, die Sie verwenden möchten, wählen Sie **Vorhandene verwenden** aus. |
    | Name der öffentlichen IP-Adresse | Geben Sie **myPublicIP** in das Textfeld ein.|
    | Zuweisung | Wählen Sie **Dynamisch** aus. |
    | Öffentliche IPv6-Adresse hinzufügen | Wählen Sie **Nein** aus. </br> Weitere Informationen zu IPv6-Adressen und zum Lastenausgleich finden Sie unter [Was ist IPv6 für Azure Virtual Network?](https://docs.microsoft.com/azure/virtual-network/ipv6-overview).  |

3. Übernehmen Sie bei den anderen Einstellungen die Standardwerte, und wählen Sie **Überprüfen + erstellen** aus.

4. Wählen Sie auf der Registerkarte **Bewerten + erstellen** die Option **Erstellen** aus.   

    :::image type="content" source="./media/quickstart-load-balancer-standard-public-portal/create-basic-load-balancer.png" alt-text="Erstellen eines Lastenausgleichs im Basic-Tarif" border="true":::

## <a name="create-load-balancer-resources"></a>Erstellen von Load Balancer-Ressourcen

In diesem Abschnitt wird Folgendes konfiguriert:

* Erstellen Sie ein virtuelles Netzwerk.
* Lastenausgleichseinstellungen für einen Back-End-Adresspool
* Ein Integritätstest
* Eine Lastenausgleichsregel

## <a name="virtual-network-and-parameters"></a>Virtuelles Netzwerk und Parameter

In diesem Abschnitt müssen die Parameter in den Schritten durch die folgenden Informationen ersetzt werden:

| Parameter                   | Wert                |
|-----------------------------|----------------------|
| **\<resource-group-name>**  | myResourceGroupLB |
| **\<virtual-network-name>** | myVNet          |
| **\<region-name>**          | Europa, Westen      |
| **\<IPv4-address-space>**   | 10.1.0.0\16          |
| **\<subnet-name>**          | myBackendSubnet        |
| **\<subnet-address-range>** | 10.1.0.0\24          |

[!INCLUDE [virtual-networks-create-new](../../includes/virtual-networks-create-new.md)]

### <a name="create-a-backend-pool"></a>Erstellen eines Back-End-Pools

Der Back-End-Adresspool enthält die IP-Adressen der virtuellen NICs, die mit dem Lastenausgleich verbunden sind. 

Erstellen Sie den Back-End-Adresspool **myBackendPool**, um virtuelle Computer für den Lastenausgleich von Internetdatenverkehr einzubeziehen.

1. Wählen Sie im linken Menü **Alle Dienste** > **Alle Ressourcen** und anschließend in der Ressourcenliste den Eintrag **myLoadBalancer** aus.

2. Wählen Sie unter **Einstellungen** die Option **Back-End-Pools** und dann **Hinzufügen** aus.

3. Geben Sie auf der Seite **Back-End-Pool hinzufügen** die folgenden Werte ein, oder wählen Sie sie aus:
    
    | Einstellung | Wert |
    | ------- | ----- |
    | Name | Geben Sie **myBackendPool** ein. |
    | Virtuelles Netzwerk | Wählen Sie **myVNet** aus. |
    | Zugeordnet zu | Wählen Sie **Virtuelle Computer** aus. |

### <a name="create-a-health-probe"></a>Erstellen eines Integritätstests

Der Status Ihrer App wird vom Lastenausgleich mithilfe eines Integritätstests überwacht. 

Abhängig von der Reaktion auf Integritätsüberprüfungen werden der Load Balancer-Instanz durch den Integritätstest virtuelle Computer hinzugefügt oder daraus entfernt. 

Erstellen Sie zur Überwachung der Integrität der virtuellen Computer einen Integritätstest mit dem Namen **myHealthProbe**.

1. Wählen Sie im linken Menü **Alle Dienste** > **Alle Ressourcen** und anschließend in der Ressourcenliste den Eintrag **myLoadBalancer** aus.

2. Wählen Sie unter **Einstellungen** die Option **Integritätstests** und dann **Hinzufügen** aus.
    
    | Einstellung | Wert |
    | ------- | ----- |
    | Name | Geben Sie **myHealthProbe** ein. |
    | Protocol | Wählen Sie **HTTP** aus. |
    | Port | Geben Sie **80** ein.|
    | `Path` | Eingeben von **/** |
    | Intervall | Geben Sie für das **Intervall** den Wert **15** (Sekunden zwischen Testversuchen) ein. |
    | Fehlerhafter Schwellenwert | Wählen Sie **2** als Wert für den **Fehlerschwellenwert** bzw. als Anzahl aufeinander folgender Testfehler aus, die auftreten müssen, damit ein virtueller Computer als fehlerhaft eingestuft wird.|

3. Klicken Sie auf **OK**.

### <a name="create-a-load-balancer-rule"></a>Erstellen einer Load Balancer-Regel

Mithilfe einer Load Balancer-Regel wird definiert, wie Datenverkehr auf die virtuellen Computer verteilt werden soll. Sie definieren die Front-End-IP-Konfiguration für den eingehenden Datenverkehr und den Back-End-IP-Pool für den Empfang des Datenverkehrs. Quell- und Zielport werden in der Regel definiert. 

In diesem Abschnitt wird eine Lastenausgleichsregel mit folgenden Merkmalen erstellt:

* Sie heißt **myHTTPRule**.
* Sie befindet sich im Front-End **LoadBalancerFrontEnd**.
* Sie lauscht a **Port 80**.
* Sie leitet vom Lastenausgleich verteilten Datenverkehr an den **Port 80** des Back-Ends **myBackendPool** weiter.

1. Wählen Sie im linken Menü **Alle Dienste** > **Alle Ressourcen** und anschließend in der Ressourcenliste den Eintrag **myLoadBalancer** aus.

2. Wählen Sie unter **Einstellungen** die Option **Lastenausgleichsregeln** und dann **Hinzufügen** aus.

3. Konfigurieren Sie die Lastenausgleichsregel mit folgenden Werten:
    
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
 
4. Übernehmen Sie die übrigen Standardeinstellungen, und wählen Sie dann **OK** aus.

## <a name="create-backend-servers"></a>Erstellen von Back-End-Servern

In diesem Abschnitt führen Sie folgende Schritte aus:

* Erstellen von drei virtuellen Computern für den Back-End-Pool des Lastenausgleichs
* Erstellen einer Verfügbarkeitsgruppe für die virtuellen Computer
* Installieren von IIS auf den virtuellen Computern, um den Lastenausgleich zu testen

### <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern

Die SKUs für öffentliche IP-Adressen und die Lastenausgleichs-SKUs müssen übereinstimmen. Verwenden Sie für den Lastenausgleich im Basic-Tarif virtuelle Computer mit Basic-IP-Adressen im Back-End-Pool. 

In diesem Abschnitt werden drei virtuelle Computer (**myVM1**, **myVM2** und **myVM3**) mit einer öffentlichen IP-Adresse vom Typ „Basic“ erstellt.  

Die drei virtuellen Computer werden einer Verfügbarkeitsgruppe namens **myAvailabilitySet** hinzugefügt.

Diese virtuellen Computer werden dem Back-End-Pool des zuvor erstellten Lastenausgleichs hinzugefügt.

1. Navigieren Sie links oben im Portal zu **Ressource erstellen** > **Compute** > **Virtueller Computer**. 
   
2. Geben Sie unter **Virtuellen Computer erstellen** auf der Registerkarte **Grundlagen** die folgenden Werte ein, oder wählen Sie sie aus:

    | Einstellung | Wert                                          |
    |-----------------------|----------------------------------|
    | **Projektdetails** |  |
    | Subscription | Auswählen des Azure-Abonnements |
    | Ressourcengruppe | Wählen Sie **myResourceGroupLB** aus. |
    | **Instanzendetails** |  |
    | Name des virtuellen Computers | Geben Sie **myVM1** ein. |
    | Region | Wählen Sie **Europa, Westen** aus. |
    | Verfügbarkeitsoptionen | Wählen Sie **Verfügbarkeitsgruppe** aus. |
    | Verfügbarkeitsgruppe | Wählen Sie **Neu erstellen**. </br> Geben Sie unter **Name** die Zeichenfolge **myAvailabilitySet** ein. </br> Klicken Sie auf **OK**. |
    | Image | **Windows Server 2019 Datacenter** |
    | Azure Spot-Instanz | Wählen Sie **Nein** aus. |
    | Size | Wählen Sie eine VM-Größe aus, oder übernehmen Sie die Standardeinstellung. |
    | **Administratorkonto** |  |
    | Username | Geben Sie einen Benutzernamen ein. |
    | Kennwort | Geben Sie ein Kennwort ein. |
    | Kennwort bestätigen | Geben Sie das Kennwort erneut ein. |

3. Wählen Sie die Registerkarte **Netzwerk** aus, oder wählen Sie **Weiter: Datenträger** und anschließend **Weiter: Netzwerk** aus.
  
4. Wählen Sie auf der Registerkarte „Netzwerk“ die folgenden Werte aus, oder geben Sie sie ein:

    | Einstellung | Wert |
    |-|-|
    | **Netzwerkschnittstelle** |  |
    | Virtuelles Netzwerk | Wählen Sie **myVNet** aus. |
    | Subnet | Wählen Sie **myBackendSubnet** aus. |
    | Öffentliche IP-Adresse | Wählen Sie **Neu erstellen** aus. </br> Geben Sie **myVM-ip** als Name ein. </br> Klicken Sie auf **OK**. |
    | NIC-Netzwerksicherheitsgruppe | Wählen Sie **Erweitert** aus.|
    | Konfigurieren von Netzwerksicherheitsgruppen | Wählen Sie **Neu erstellen**. </br> Geben Sie unter **Netzwerksicherheitsgruppe erstellen** als **Name** die Zeichenfolge **myNSG** ein. </br> Wählen Sie unter **Regeln für eingehenden Datenverkehr** die Option **+ Eingangsregel hinzufügen** aus. </br> Geben Sie unter **Zielportbereiche** den Wert **80** ein. </br> Geben Sie unter **Priorität** den Wert **100** ein. </br> Geben Sie unter **Name** die Zeichenfolge **myHTTPRule** ein. </br> Wählen Sie **Hinzufügen** aus. </br> Klicken Sie auf **OK**. |
    | **Lastenausgleich**  |
    | Diese VM hinter einer vorhandenen Lastenausgleichslösung platzieren? | Wählen Sie **Nein** aus. |
 
5. Wählen Sie die Registerkarte **Verwaltung** oder **Weiter** > **Verwaltung**.

6. Wählen Sie auf der Registerkarte **Verwaltung** die folgenden Werte aus, oder geben Sie sie ein:
    | Einstellung | Wert |
    |-|-|
    | **Überwachung** | |
    | Startdiagnose | Wählen Sie **Aus** aus. |

7. Klicken Sie auf **Überprüfen + erstellen**. 
  
8. Überprüfen Sie die Einstellungen, und wählen Sie dann die Option **Erstellen**.

9. Führen Sie die Schritte 1 bis 8 aus, um zwei weitere virtuelle Computer mit folgenden Werten zu erstellen. Verwenden Sie für alle übrigen Einstellungen die Werte von **myVM1**:

    | Einstellung | VM 2| VM 3|
    | ------- | ----- |---|
    | Name |  **myVM2** |**myVM3**|
    | Verfügbarkeitsgruppe| Wählen Sie **myAvailabilitySet** aus. | Wählen Sie **myAvailabilitySet** aus.|
    | Netzwerksicherheitsgruppe | Wählen Sie die vorhandene Netzwerksicherheitsgruppe (**myNSG**) aus.| Wählen Sie die vorhandene Netzwerksicherheitsgruppe (**myNSG**) aus.|
---

### <a name="install-iis"></a>Installieren von IIS

1. Wählen Sie im linken Menü **Alle Dienste** > **Alle Ressourcen** und anschließend in der Ressourcenliste den Eintrag **myVM1** (in der Ressourcengruppe **myResourceGroupLB**) aus.

2. Wählen Sie auf der Seite **Übersicht** die Option **Verbinden** aus, um die RDP-Datei für den virtuellen Computer herunterzuladen.

3. Öffnen Sie die RDP-Datei.

4. Melden Sie sich mit den Anmeldeinformationen, die Sie während der Erstellung dieses virtuellen Computers bereitgestellt haben, beim virtuellen Computer an.

5. Navigieren Sie auf dem Serverdesktop zu **Windows-Verwaltungsprogramme**>**Windows PowerShell**.

6. Führen Sie im PowerShell-Fenster die im Anschluss angegebenen Befehle aus. Diese bewirken Folgendes:

    * Installieren des IIS-Servers
    * Entfernen der Standarddatei „iisstart.htm“
    * Hinzufügen einer neuen Datei namens „iisstart.htm“ zum Anzeigen des Namens des virtuellen Computers

   ```powershell
    
    # install IIS server role
    Install-WindowsFeature -name Web-Server -IncludeManagementTools
    
    # remove default htm file
     remove-item  C:\inetpub\wwwroot\iisstart.htm
    
    # Add a new htm file that displays server name
     Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from " + $env:computername)
   ```
7. Schließen Sie die RDP-Sitzung mit **myVM1**.

8. Wiederholen Sie die Schritte 1 bis 6, um IIS und die aktualisierte Datei „iisstart.htm“ auf **myVM2** und **myVM3** zu installieren.

## <a name="test-the-load-balancer"></a>Testen des Lastenausgleichs

1. Ermitteln Sie auf dem Bildschirm **Übersicht** die öffentliche IP-Adresse für den Load Balancer. Wählen Sie im linken Menü **Alle Dienste** > **Alle Ressourcen** und anschließend **myPublicIP** aus.

2. Kopieren Sie die öffentliche IP-Adresse, und fügen Sie sie in die Adressleiste des Browsers ein. Die Standardseite des IIS-Webservers wird im Browser angezeigt.

   ![IIS-Webserver](./media/tutorial-load-balancer-standard-zonal-portal/load-balancer-test.png)

Um zu sehen, wie Datenverkehr durch den Lastenausgleich auf alle drei virtuellen Computer verteilt wird, können Sie die Standardseite des IIS-Webservers der einzelnen virtuellen Computer anpassen und dann über den Clientcomputer eine Aktualisierung Ihres Webbrowsers erzwingen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcengruppe, den Lastenausgleich und alle zugehörigen Ressourcen, wenn Sie sie nicht mehr benötigen. Wählen Sie hierzu die Ressourcengruppe **myResourceGroupLB** aus, die die Ressourcen enthält, und wählen Sie anschließend **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung führen Sie die folgenden Schritte aus:

* Erstellen einer Azure Load Balancer-Instanz (Standard oder Basic)
* Anfügen von drei virtuellen Computern an den Lastenausgleich
* Konfigurieren der Lastenausgleichsregel für den Datenverkehr sowie eines Integritätstests und anschließendes Testen des Lastenausgleichs 

Weitere Informationen zu Azure Load Balancer finden Sie unter [Was versteht man unter Azure Load Balancer?](load-balancer-overview.md) sowie unter [Häufig gestellte Fragen zu Load Balancer](load-balancer-faqs.md).

Weitere Informationen zu Load Balancer und Verfügbarkeitszonen finden Sie [hier](load-balancer-standard-availability-zones.md).
