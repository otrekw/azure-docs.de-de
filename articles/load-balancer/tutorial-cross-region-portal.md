---
title: 'Tutorial: Erstellen eines regionsübergreifenden Lastenausgleichs mithilfe des Azure-Portals'
titleSuffix: Azure Load Balancer
description: In diesem Tutorial stellen Sie zunächst eine regionsübergreifende Azure Load Balancer-Instanz mit dem Azure-Portal bereit.
author: asudbring
ms.author: allensu
ms.service: load-balancer
ms.topic: tutorial
ms.date: 02/24/2021
ms.openlocfilehash: 65d85f51afef36aa618868e2fda1d2bbf583ea21
ms.sourcegitcommit: 3f684a803cd0ccd6f0fb1b87744644a45ace750d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/02/2021
ms.locfileid: "106221125"
---
# <a name="tutorial-create-a-cross-region-azure-load-balancer-using-the-azure-portal"></a>Tutorial: Erstellen einer regionsübergreifenden Azure Load Balancer-Instanz mithilfe des Azure-Portals

Ein regionsübergreifender Lastenausgleich stellt sicher, dass ein Dienst global in mehreren Azure-Regionen verfügbar ist. Wenn eine Region ausfällt, wird der Datenverkehr an den nächstgelegenen fehlerfreien Lastenausgleich weitergeleitet.  

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines regionsübergreifenden Lastenausgleichs
> * Erstellen eines Back-End-Pools mit zwei regionalen Lastenausgleichsmodulen
> * Erstellen Sie eine Load Balancer-Regel.
> * Testen des Lastenausgleichs

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

> [!IMPORTANT]
> Die regionsübergreifende Azure Load Balancer-Instanz befindet sich derzeit in der öffentlichen Vorschauphase (Public Preview).
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Abonnement.
- Zwei Azure Load Balancer-Instanzen der SKU **Standard** mit Back-End-Pool in zwei verschiedenen Azure-Regionen
    - Informationen zum Erstellen einer regionalen Load Balancer Standard-Instanz virtueller Computer für Back-End-Pools finden Sie unter [Schnellstart: Erstellen eines öffentlichen Lastenausgleichs für den Lastenausgleich virtueller Computer über das Azure-Portal](quickstart-load-balancer-standard-public-portal.md).
        - Fügen Sie den Namen der Lastenausgleichsmodule, virtuellen Computer und anderen Ressourcen in jeder Region mit **-R1** und **-R2** an. 

## <a name="sign-in-to-azure-portal"></a>Anmelden beim Azure-Portal

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

## <a name="create-cross-region-load-balancer"></a>Erstellen eines regionsübergreifenden Lastenausgleichs

In diesem Abschnitt erstellen Sie einen regionsübergreifenden Lastenausgleich und eine öffentliche IP-Adresse.

1. Wählen Sie **Ressource erstellen**. 
2. Geben Sie im Suchfeld **Load Balancer** ein. Wählen Sie in den Suchergebnissen **Load Balancer** aus.
3. Wählen Sie auf der Seite **Load Balancer** die Option **Erstellen** aus.
4. Geben Sie auf der Seite **Lastenausgleich erstellen** auf der Registerkarte **Grundlagen** die folgenden Informationen ein, bzw. wählen Sie sie aus: 

    | Einstellung                 | Wert                                              |
    | ---                     | ---                                                |
    | Subscription               | Wählen Sie Ihr Abonnement aus.    |    
    | Resource group         | Wählen Sie die Option **Neu erstellen** aus, und geben Sie im Textfeld **CreateCRLBTutorial-rg** ein.|
    | Name                   | Geben Sie **myLoadBalancer-CR** ein.                                   |
    | Region         | Wählen Sie **USA, Westen** aus.                                        |
    | type          | Wählen Sie **Öffentlich** aus.                                        |
    | SKU           | Übernehmen Sie den Standardwert **Standard**. |
    | Tarif           | Wählen Sie **Global** aus. |
    | Öffentliche IP-Adresse | Wählen Sie **Neu erstellen**.|
    | Name der öffentlichen IP-Adresse | Geben Sie **myPublicIP-CR** in das Textfeld ein.|
    | Routingpräferenz| Wählen Sie **Microsoft-Netzwerk** aus. </br> Weitere Informationen zu den Routingpräferenzen finden Sie unter [Was ist Routingpräferenz (Vorschau)?](../virtual-network/routing-preference-overview.md). |

    > [!NOTE]
    > Regionsübergreifender Lastenausgleich kann nur in den folgenden Startregionen bereitgestellt werden: **USA, Osten 2; USA, Westen, Europa, Westen; Asien, Südosten; USA, Mitte; Europa, Norden; Asien, Osten**. Weitere Informationen finden Sie unter **https://aka.ms/homeregionforglb**.


3. Übernehmen Sie bei den anderen Einstellungen die Standardwerte, und wählen Sie **Überprüfen + erstellen** aus.

4. Wählen Sie auf der Registerkarte **Bewerten + erstellen** die Option **Erstellen** aus.   

    :::image type="content" source="./media/tutorial-cross-region-portal/create-cross-region.png" alt-text="Erstellen eines regionsübergreifenden Lastenausgleichs" border="true":::

## <a name="create-backend-pool"></a>Erstellen eines Back-End-Pools

In diesem Abschnitt fügen Sie dem Back-End-Pool des regionsübergreifenden Lastenausgleichs zwei regionale Load Balancer Standard-Instanzen hinzu.

> [!IMPORTANT]
> Vergewissern Sie sich vor dem Ausführen dieser Schritte, dass zwei regionale Lastenausgleichsmodule mit Back-End-Pools in Ihrem Abonnement bereitgestellt wurden.  Weitere Informationen finden Sie unter **[Schnellstart: Erstellen eines öffentlichen Lastenausgleichs für den Lastenausgleich virtueller Computer über das Azure-Portal](quickstart-load-balancer-standard-public-portal.md)** .

Erstellen Sie den Back-End-Adresspool **myBackendPool-CR**, der die regionalen Lastenausgleichsmodule enthalten soll.

1. Wählen Sie im linken Menü **Alle Dienste** > **Alle Ressourcen** und anschließend in der Ressourcenliste den Eintrag **myLoadBalancer-CR** aus.

2. Wählen Sie unter **Einstellungen** die Option **Back-End-Pools** und dann **Hinzufügen** aus.

3. Geben Sie auf der Seite **Back-End-Pool hinzufügen** unter als Name den Namen **myBackendPool-CR** ein.

4. Wählen Sie **Hinzufügen**.

4. Wählen Sie **myBackendPool-CR** aus.

5. Wählen Sie unter **Lastenausgleichsmodule** das Pulldownfeld unter **Lastenausgleich** aus.

5. Wählen Sie **myLoadBalancer-R1** oder den Namen des Lastenausgleichs in Region 1 aus.

6. Wählen Sie unter **Front-End-IP-Konfiguration** das Pulldownfeld aus. Wählen Sie **LoadBalancerFrontEnd**.

7. Wiederholen Sie die Schritte 4 bis 6, um **myLoadBalancer-R2** hinzuzufügen.

8. Wählen Sie **Hinzufügen**.

    :::image type="content" source="./media/tutorial-cross-region-portal/add-to-backendpool.png" alt-text="Hinzufügen von regionalen Lastenausgleichsmodulen zum Back-End-Pool" border="true":::

## <a name="create-a-load-balancer-rule"></a>Erstellen einer Load Balancer-Regel

In diesem Abschnitt wird eine Lastenausgleichsregel mit folgenden Merkmalen erstellt:

* Sie heißt **myHTTPRule**.
* Sie befindet sich im Front-End **LoadBalancerFrontEnd**.
* Sie lauscht a **Port 80**.
* Sie leitet vom Lastenausgleich verteilten Datenverkehr an den **Port 80** des Back-Ends **myBackendPool-CR** weiter.

    > [!NOTE]
    > Der Front-End-Port muss dem Back-End-Port und dem Front-End-Port der regionalen Lastenausgleichsmodule im Back-End-Pool entsprechen.

1. Wählen Sie im linken Menü **Alle Dienste** > **Alle Ressourcen** und anschließend in der Ressourcenliste den Eintrag **myLoadBalancer-CR** aus.

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
    | Leerlaufzeitüberschreitung (Minuten) | Bewegen Sie den Schieberegler auf **15**. |
    | TCP-Zurücksetzung | Wählen Sie **Aktiviert**. |

4. Übernehmen Sie die übrigen Standardeinstellungen, und wählen Sie dann **OK** aus.

    :::image type="content" source="./media/tutorial-cross-region-portal/create-lb-rule.png" alt-text="Erstellen einer Load Balancer-Regel" border="true":::

## <a name="test-the-load-balancer"></a>Testen des Lastenausgleichs

In diesem Abschnitt testen Sie den regionsübergreifenden Lastenausgleich. Sie stellen in einem Webbrowser eine Verbindung mit der öffentlichen IP-Adresse her.  Sie halten die virtuellen Computer in den Back-End-Pools eines der regionalen Lastenausgleiche an und beobachten das Failover.

1. Ermitteln Sie auf dem Bildschirm **Übersicht** die öffentliche IP-Adresse für den Load Balancer. Wählen Sie im linken Menü **Alle Dienste** > **Alle Ressourcen** und anschließend **myPublicIP-CR** aus.

2. Kopieren Sie die öffentliche IP-Adresse, und fügen Sie sie in die Adressleiste des Browsers ein. Die Standardseite des IIS-Webservers wird im Browser angezeigt.

    :::image type="content" source="./media/tutorial-cross-region-portal/test-cr-lb-1.png" alt-text="Testen des Load Balancers" border="true":::

3. Beenden Sie die virtuellen Computer im Back-End-Pool eines der regionalen Lastenausgleichsmodule.

4. Aktualisieren Sie den Webbrowser, und beobachten Sie das Failover der Verbindung zum anderen regionalen Lastenausgleich.

    :::image type="content" source="./media/tutorial-cross-region-portal/test-cr-lb-2.png" alt-text="Testen des Lastenausgleichs nach einem Failover" border="true":::

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcengruppe, den Lastenausgleich und alle dazugehörigen Ressourcen, wenn Sie sie nicht mehr benötigen. 

Wählen Sie hierzu die Ressourcengruppe **CreateCRLBTutorial-rg** aus, die die Ressourcen enthält, und wählen Sie anschließend **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie:

* einen regionsübergreifenden Lastenausgleich erstellt
* dem Back-End-Pool des regionsübergreifenden Lastenausgleichs regionale Lastenausgleichsmodule hinzugefügt
* eine Lastenausgleichsregel erstellt
* den Lastenausgleich getestet

Weitere Informationen zum regionsübergreifenden Lastenausgleich finden Sie unter folgendem Link:
> [!div class="nextstepaction"]
> [Regionsübergreifender Lastenausgleich (Vorschau)](cross-region-overview.md)
