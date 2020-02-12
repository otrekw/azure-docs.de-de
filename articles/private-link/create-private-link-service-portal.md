---
title: 'Schnellstart: Erstellen eines Private Link-Diensts über das Azure-Portal'
titlesuffix: Azure Private Link
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie über das Azure-Portal einen Private Link-Dienst erstellen.
services: private-link
author: malopMSFT
ms.service: private-link
ms.topic: quickstart
ms.date: 02/03/2020
ms.author: allensu
ms.openlocfilehash: 4dd27d3fb4321c2eecf222136376bfd4b49790bb
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77026203"
---
# <a name="quickstart-create-a-private-link-service-using-the-azure-portal"></a>Schnellstart: Erstellen eines Private Link-Diensts über das Azure-Portal

Der Azure Private Link-Dienst ist der Verweis auf Ihren eigenen Dienst, der von Azure Private Link unterstützt wird. Für den Dienst oder die Ressource hinter Azure Load Balancer kann Private Link-Zugriff aktiviert werden. Consumer Ihres Diensts können von ihrem eigenen VNET aus privat auf den Dienst zugreifen. In dieser Schnellstartanleitung erfahren Sie, wie Sie über das Azure-Portal einen Private Link-Dienst erstellen.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.


## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter https://portal.azure.com beim Azure-Portal an.

## <a name="create-an-internal-load-balancer"></a>Erstellen eines internen Lastenausgleichs

Erstellen Sie zunächst ein virtuelles Netzwerk und anschließend einen internen Lastenausgleich für die Verwendung mit dem Azure Private Link-Dienst.

### <a name="create-the-virtual-network"></a>Erstellen des virtuellen Netzwerks

In diesem Abschnitt erstellen Sie ein virtuelles Netzwerk und das Subnetz zum Hosten des Lastenausgleichs, der für den Zugriff auf Ihren Private Link-Dienst verwendet wird.


1. Wählen Sie oben links im Portal **Ressource erstellen** > **Netzwerk** > **Virtuelles Netzwerk**.
   
1. Geben Sie im Bereich **Virtuelles Netzwerk erstellen** diese Werte ein (bzw. wählen Sie sie aus):
   
   - **Name**: Geben Sie **MyVNet** ein.
   - **Ressourcengruppe**: Wählen Sie **Neu erstellen** aus, geben Sie **MyResourceGroupLB** ein, und wählen Sie anschließend **OK** aus. 
   - **Subnetz** > **Name**: Geben Sie **MyBackendSubnet** ein.
   
1. Klicken Sie auf **Erstellen**.

   ![Erstellen eines virtuellen Netzwerks](../load-balancer/media/tutorial-load-balancer-basic-internal-portal/2-load-balancer-virtual-network.png)

### <a name="create-a-standard-load-balancer"></a>Erstellen eines Load Balancers im Tarif „Standard“

Erstellen Sie über das Portal einen internen Lastenausgleich im Tarif „Standard“. Der von Ihnen erstellte Name und die IP-Adresse werden automatisch als Front-End des Load Balancers konfiguriert.

1. Wählen Sie oben links im Portal **Ressource erstellen** > **Netzwerk** > **Load Balancer**.
   
2. Geben Sie auf der Seite **Lastenausgleich erstellen** auf der Registerkarte **Grundlagen** die folgenden Informationen ein, oder wählen Sie sie aus, übernehmen Sie die Standardwerte für die übrigen Einstellungen, und klicken Sie auf **Überprüfen + erstellen**:

    | Einstellung                 | Wert                                              |
    | ---                     | ---                                                |
    | Subscription               | Wählen Sie Ihr Abonnement aus.    |    
    | Resource group         | Wählen Sie im Pulldownfeld die Option *MyResourceGroupLB* aus.|
    | Name                   | *myLoadBalancer*                                   |
    | Region         | Wählen Sie **USA, Osten 2** aus.                                        |
    | type          | Wählen Sie **Intern** aus.                                        |
    | SKU           | Wählen Sie **Standard** aus.                          |
    | Virtuelles Netzwerk           | Wählen Sie *MyVNet* aus.                          |    
    | IP-Adresszuweisung              | Wählen Sie **Statisch** aus.   |
    | Private IP-Adresse|Geben Sie eine Adresse ein, die im Adressraum Ihres virtuellen Netzwerks und Subnetzes enthalten ist (beispielsweise *10.3.0.7*).  |

3. Klicken Sie auf der Registerkarte **Überprüfen + erstellen** auf **Erstellen**. 
   

### <a name="create-standard-load-balancer-resources"></a>Erstellen von Lastenausgleichsressourcen vom Typ „Standard“

In diesem Abschnitt konfigurieren Sie Lastenausgleichseinstellungen für einen Back-End-Adresspool und einen Integritätstest. Außerdem geben Sie Lastenausgleichsregeln an.

#### <a name="create-a-backend-pool"></a>Erstellen eines Back-End-Pools

Ein Back-End-Adresspool enthält die IP-Adressen der virtuellen NICs, die mit der Load Balancer-Instanz verbunden sind, um Datenverkehr an Ihre Ressourcen verteilen zu können. Erstellen Sie den Back-End-Adresspool *myBackendPool* für Ressourcen im Zusammenhang mit dem Lastenausgleich für Datenverkehr.

1. Wählen Sie im linken Menü **Alle Dienste** > **Alle Ressourcen** und anschließend in der Ressourcenliste den Eintrag **myLoadBalancer** aus.
2. Wählen Sie unter **Einstellungen** die Option **Back-End-Pools** und dann **Hinzufügen** aus.
3. Geben Sie auf der Seite **Back-End-Pool hinzufügen** die Zeichenfolge *myBackendPool* als Name für Ihren Back-End-Pool ein, und wählen Sie anschließend **Hinzufügen** aus.



#### <a name="create-a-health-probe"></a>Erstellen eines Integritätstests

Mit einem Integritätstest kann der Lastenausgleich den Ressourcenstatus überwachen. Abhängig von der Reaktion auf Integritätsüberprüfungen werden der Lastenausgleichsrotation durch den Integritätstest dynamisch Ressourcen hinzugefügt oder daraus entfernt. 

**So erstellen Sie einen Integritätstest zur Überwachung der Ressourcenintegrität:**

1. Wählen Sie im Menü auf der linken Seite die Option **Alle Ressourcen** und dann in der Ressourcenliste die Option **MyLoadBalancer**.
   
1. Wählen Sie unter **Einstellungen** die Option **Integritätstests** und dann **Hinzufügen**.
   
1. Geben Sie auf der Seite **Integritätstest hinzufügen** die folgenden Werte ein (bzw. wählen Sie sie aus):
   
   - **Name**: Geben Sie **MyHealthProbe** ein.
   - **Protokoll:** Wählen Sie in der Dropdownliste die Option **TCP** aus. 
   - **Port:** Geben Sie **80** ein. 
   - **Intervall**: Geben Sie **15** ein. Das Intervall ist die Anzahl von Sekunden zwischen Testversuchen.
   - **Fehlerschwellenwert**: Geben Sie **2** ein. Dieser Wert gibt die Anzahl aufeinander folgender Testfehler an, die auftreten müssen, damit ein virtueller Computer als fehlerhaft eingestuft wird.
   
1. Klicken Sie auf **OK**.

#### <a name="create-a-load-balancer-rule"></a>Erstellen einer Load Balancer-Regel

Eine Lastenausgleichsregel definiert, wie Datenverkehr an Ressourcen verteilt wird. Die Regel definiert die Front-End-IP-Konfiguration für eingehenden Datenverkehr und den Back-End-IP-Pool zum Empfangen des Datenverkehrs sowie die erforderlichen Quell- und Zielports. 

Mit der Lastenausgleichsregel **MyLoadBalancerRule** wird über Port 80 des Front-Ends **LoadBalancerFrontEnd** gelauscht. Die Regel sendet Netzwerkdatenverkehr an den Back-End-Adresspool **MyBackendPool** (ebenfalls unter Port 80). 

**Erstellen Sie die Lastenausgleichsregel wie folgt:**

1. Wählen Sie im Menü auf der linken Seite die Option **Alle Ressourcen** und dann in der Ressourcenliste die Option **MyLoadBalancer**.
   
1. Wählen Sie unter **Einstellungen** die Option **Lastenausgleichsregeln** und dann **Hinzufügen** aus.
   
1. Geben Sie auf der Seite **Lastenausgleichsregel hinzufügen** bei Bedarf die folgenden Werte ein (bzw. wählen Sie sie aus):
   
   - **Name**: Geben Sie **MyLoadBalancerRule** ein.
   - **Front-End-IP-Adresse**: Geben Sie **LoadBalancerFrontEnd** ein (sofern noch nicht vorhanden).
   - **Protokoll:** Wählen Sie **TCP** aus.
   - **Port:** Geben Sie **80** ein.
   - **Back-End-Port**: Geben Sie **80** ein.
   - **Back-End-Pool**: Wählen Sie **MyBackendPool** aus.
   - **Integritätstest**: Wählen Sie **MyHealthProbe** aus. 
   
1. Klicken Sie auf **OK**.
   
## <a name="create-a-private-link-service"></a>Erstellen eines Private Link-Diensts

In diesem Abschnitt wird ein Private Link-Dienst hinter einer Load Balancer Standard-Instanz erstellt.

1. Wählen Sie oben links auf dem Bildschirm im Azure-Portal die Option **Ressource erstellen** > **Netzwerk** > **Private Link-Center (Vorschau)** aus. Sie können auch über die Suchfunktion des Portals nach „Private Link“ suchen.

2. Wählen Sie in **Private Link-Center – Übersicht** unter **Machen Sie Ihren eigenen Dienst verfügbar, damit andere Benutzer eine Verbindung herstellen können.** die Option „Starten“ aus.

3. Geben Sie unter **Private Link-Dienst erstellen (Vorschau) – Grundlegende Einstellungen** die folgenden Informationen ein, oder wählen Sie sie aus:

    | Einstellung           | Wert                                                                        |
    |-------------------|------------------------------------------------------------------------------|
    | **Projektdetails:**  |                                                                              |
    | Abonnement      | Wählen Sie Ihr Abonnement aus.                                                     |
    | Ressourcengruppe    | Wählen Sie *MyResourceGroupLB* aus.                                                     |
    | **INSTANZDETAILS:** |                                                                              |
    | Name              | Geben Sie *myPrivateLinkService* ein. |
    | Region            | Wählen Sie *USA, Osten 2* aus.                                                         |

4. Wählen Sie **Weiter: Ausgangseinstellungen** aus.

5. Geben Sie unter **Private Link-Dienst erstellen (Vorschau) – Ausgangseinstellungen** die folgenden Informationen ein, oder wählen Sie sie aus:


    | Einstellung                           | Wert                                                                           |
    |-----------------------------------|---------------------------------------------------------------------------------|
    | Load Balancer                     | Wählen Sie *MyLoadBalancer* aus.                                                           |
    | Front-End-IP-Adresse des Lastenausgleichs | Wählen Sie die Front-End-IP-Adresse von „MyLoadBalancer“ aus.                                |
    | Virtuelles Quellnetzwerk für NAT        | Wählen Sie *myVNET* aus.                                                                   |
    | NAT-Quellsubnetz                 | Wählen Sie *myBackendSubnet* aus.                                                          |
    | TCP-Proxy V2 aktivieren               | Wählen Sie abhängig davon, ob Ihre Anwendung TCP-Proxy V2-Header erwartet, entweder „JA“ oder „NEIN“ aus. |
    | Einstellungen für private IP-Adressen       | Konfigurieren Sie die Zuordnungsmethode und die IP-Adresse für die einzelnen NAT-IP-Adressen.                  |

6. Wählen Sie **Weiter: Zugriffssicherheit** aus.

7. Geben Sie unter **Private Link-Dienst erstellen (Vorschau) – Zugriffssicherheit** die folgenden Informationen ein, oder wählen Sie sie aus:

    | Einstellung                                     | Wert                                                             |
    |---------------------------------------------|-------------------------------------------------------------------|
    | Sichtbarkeit                                  | Wählen Sie *Nur rollenbasierte Zugriffssteuerung* aus.         |
  
8. Wählen Sie **Weiter: Tags** und anschließend **Überprüfen + erstellen** aus, oder wählen Sie im oberen Seitenbereich die Registerkarte **Überprüfen + erstellen** aus.

9. Überprüfen Sie Ihre Angaben, und wählen Sie **Erstellen** aus.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen 
Wenn Sie den Private Link-Dienst nicht mehr benötigen, löschen Sie die Ressourcengruppe, um die im Rahmen dieser Schnellstartanleitung verwendeten Ressourcen zu bereinigen:

1. Geben Sie oben im Portal im Feld **Suchen** die Zeichenfolge *myResourceGroupLB* ein, und wählen Sie in den Suchergebnissen den Eintrag *myResourceGroupLB* aus. 
2. Wählen Sie die Option **Ressourcengruppe löschen**. 
3. Geben Sie „myResourceGroup“ für **RESSOURCENGRUPPENNAMEN EINGEBEN** ein, und wählen Sie **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie eine interne Azure Load Balancer-Instanz und einen Private Link-Dienst erstellt. Weitere Informationen zur Erstellung privater Endpunkte finden Sie unter [Schnellstart: Erstellen eines privaten Endpunkts mit dem Azure-Portal](https://docs.microsoft.com/azure/private-link/create-private-endpoint-portal).
