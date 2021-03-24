---
title: 'Schnellstart: Erstellen eines Private Link-Diensts über das Azure-Portal'
titlesuffix: Azure Private Link
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie über das Azure-Portal einen Private Link-Dienst erstellen.
services: private-link
author: asudbring
ms.service: private-link
ms.topic: quickstart
ms.date: 01/18/2021
ms.author: allensu
ms.openlocfilehash: d394a475c5121607f70c03437382e104a5d0cbee
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "98746406"
---
# <a name="quickstart-create-a-private-link-service-by-using-the-azure-portal"></a>Schnellstart: Erstellen eines Private Link-Diensts über das Azure-Portal

Führen Sie die ersten Schritte zum Erstellen eines Private Link-Diensts für Ihren Dienst aus.  Gewähren Sie für Private Link den Zugriff auf Ihren Dienst oder Ihre Ressource hinter Azure Load Balancer Standard.  Benutzer Ihres Diensts verfügen über privaten Zugriff aus ihrem virtuellen Netzwerk.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich unter https://portal.azure.com beim Azure-Portal an.

## <a name="create-an-internal-load-balancer"></a>Erstellen eines internen Load Balancers

In diesem Abschnitt erstellen Sie ein virtuelles Netzwerk und eine interne Azure Load Balancer-Instanz.

### <a name="virtual-network"></a>Virtuelles Netzwerk

In diesem Abschnitt erstellen Sie ein virtuelles Netzwerk und das Subnetz zum Hosten des Lastenausgleichs, der auf Ihren Private Link-Dienst zugreift.

1. Wählen Sie links oben auf dem Bildschirm **Ressource erstellen > Netzwerk > Virtuelles Netzwerk** aus, oder suchen Sie über das Suchfeld nach **Virtuelles Netzwerk**.

2. Geben Sie unter **Virtuelles Netzwerk erstellen** auf der Registerkarte **Grundlegende Einstellungen** die folgenden Informationen ein, oder wählen Sie sie aus:

    | **Einstellung**          | **Wert**                                                           |
    |------------------|-----------------------------------------------------------------|
    | **Projektdetails**  |                                                                 |
    | Subscription     | Auswählen des Azure-Abonnements                                  |
    | Ressourcengruppe   | Wählen Sie **CreatePrivLinkService-rg** aus. |
    | **Instanzendetails** |                                                                 |
    | Name             | Geben Sie **myVNet** ein.                                    |
    | Region           | Wählen Sie **USA, Osten 2** aus. |

3. Wählen Sie die Registerkarte **IP-Adressen** oder die Schaltfläche **Weiter: IP-Adressen** am unteren Seitenrand aus.

4. Geben Sie auf der Registerkarte **IP-Adressen** die folgenden Informationen ein:

    | Einstellung            | Wert                      |
    |--------------------|----------------------------|
    | IPv4-Adressraum | Geben Sie **10.1.0.0/16** ein. |

5. Wählen Sie unter **Subnetzname** das Wort **Standard** aus.

6. Geben Sie unter **Subnetz bearbeiten** die folgenden Informationen ein:

    | Einstellung            | Wert                      |
    |--------------------|----------------------------|
    | Subnetzname | Geben Sie **mySubnet** ein. |
    | Subnetzadressbereich | Geben Sie **10.1.0.0/24** ein. |

7. Wählen Sie **Speichern** aus.

8. Wählen Sie die Registerkarte **Überprüfen + erstellen** oder die Schaltfläche **Überprüfen + erstellen** aus.

9. Klicken Sie auf **Erstellen**.

### <a name="create-a-standard-load-balancer"></a>Schnellstart: Erstellen einer Load Balancer Standard-Instanz mit Azure PowerShell

Erstellen Sie über das Portal einen internen Lastenausgleich im Tarif „Standard“. 

1. Wählens Sie links oben auf dem Bildschirm die Optionen **Ressource erstellen** > **Netzwerk** > **Lastenausgleich** aus.

2. Geben Sie auf der Seite **Lastenausgleich erstellen** auf der Registerkarte **Grundlagen** die folgenden Informationen ein, bzw. wählen Sie sie aus: 

    | Einstellung                 | Wert                                              |
    | ---                     | ---                                                |
    | Subscription               | Wählen Sie Ihr Abonnement aus.    |    
    | Resource group         | Wählen Sie das Element **CreatePrivLinkService-rg** aus, das Sie im vorherigen Schritt erstellt haben.|
    | Name                   | Geben Sie **myLoadBalancer** ein.                                   |
    | Region         | Wählen Sie **USA, Osten 2** aus.                                        |
    | type          | Wählen Sie **Intern** aus.                                        |
    | SKU           | Wählen Sie **Standard** aus. |
    | Virtuelles Netzwerk | Wählen Sie das im vorherigen Schritt erstellte virtuelle Netzwerk **myVNet** aus. |
    | Subnet  | Wählen Sie das Element **mySubnet** aus, das Sie im vorherigen Schritt erstellt haben. |
    | IP-Adresszuweisung | Wählen Sie **Dynamisch** aus. |
    | Verfügbarkeitszone | Wählen Sie **Zonenredundant** aus. |

3. Übernehmen Sie bei den anderen Einstellungen die Standardwerte, und wählen Sie **Überprüfen + erstellen** aus.

4. Wählen Sie auf der Registerkarte **Bewerten + erstellen** die Option **Erstellen** aus.   

## <a name="create-load-balancer-resources"></a>Erstellen von Load Balancer-Ressourcen

In diesem Abschnitt wird Folgendes konfiguriert:

* Lastenausgleichseinstellungen für einen Back-End-Adresspool
* Ein Integritätstest
* Eine Lastenausgleichsregel

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
    | Protocol | Wählen Sie **TCP** aus. |
    | Port | Geben Sie **80** ein.|
    | Intervall | Geben Sie für das **Intervall** den Wert **15** (Sekunden zwischen Testversuchen) ein. |
    | Fehlerhafter Schwellenwert | Wählen Sie **2** als Wert für den **Fehlerschwellenwert** bzw. als Anzahl aufeinander folgender Testfehler aus, die auftreten müssen, damit ein virtueller Computer als fehlerhaft eingestuft wird.|
    | | |

3. Übernehmen Sie die übrigen Standardeinstellungen, und wählen Sie **OK** aus.

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
    | Leerlaufzeitüberschreitung (Minuten) | Bewegen Sie den Schieberegler auf **15 Minuten**. |
    | TCP-Zurücksetzung | Wählen Sie **Aktiviert**. |

4. Übernehmen Sie die übrigen Standardeinstellungen, und wählen Sie dann **OK** aus.

## <a name="create-a-private-link-service"></a>Erstellen eines Private Link-Diensts

In diesem Abschnitt erstellen Sie einen Private Link-Dienst hinter einem Standardlastenausgleich.

1. Wählen Sie im Azure-Portal oben links auf der Seite die Option **Ressource erstellen** aus.

2. Suchen Sie im Feld **Marketplace durchsuchen** nach **Private Link**.

3. Klicken Sie auf **Erstellen**.

4. Wählen Sie in der **Übersicht** unter **Private Link Center** die blaue Schaltfläche **Create private link service** (Private Link-Dienst erstellen) aus.

5. Geben Sie auf der Registerkarte **Grundlagen** unter **Create private link service** (Private Link-Dienst erstellen) die folgenden Informationen ein, bzw. wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | **Projektdetails** |  |
    | Subscription | Wählen Sie Ihr Abonnement aus. |
    | Ressourcengruppe | Wählen Sie **CreatePrivLinkService-rg** aus. |
    | **Instanzendetails** |  |
    | Name | Geben Sie **myPrivateLinkService** ein. |
    | Region | Wählen Sie **USA, Osten 2** aus. |

6. Wählen Sie die Registerkarte **Ausgangseinstellungen** oder unten auf der Seite die Option **Weiter: Ausgangseinstellungen** aus.

7. Geben Sie auf der Registerkarte **Ausgangseinstellungen** die folgenden Informationen ein, bzw. wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | Load Balancer | Wählen Sie **myLoadBalancer** aus. |
    | Front-End-IP-Adresse des Lastenausgleichs | Wählen Sie **LoadBalancerFrontEnd (10.1.0.4)** aus. |
    | NAT-Quellsubnetz | Wählen Sie **mySubnet (10.1.0.0/24)** aus. |
    | TCP-Proxy V2 aktivieren | Übernehmen Sie den Standardwert **Nein**. </br> Wählen Sie **Ja** aus, falls Ihre Anwendung einen TCP-Proxy V2-Header erwartet. |
    | **Einstellungen für private IP-Adressen** |  |
    | Übernehmen Sie die Standardeinstellungen. |  |

8. Wählen Sie die Registerkarte **Zugriffssicherheit** oder unten auf der Seite die Option **Weiter: Zugriffssicherheit** aus.

9. Übernehmen Sie auf der Registerkarte **Zugriffssicherheit** die Standardeinstellung **Nur rollenbasierte Zugriffssteuerung**.

10. Wählen Sie die Registerkarte **Tags** aus, oder **Weiter: Tags** unten auf der Seite.

11. Wählen Sie die Registerkarte **Überprüfen und erstellen** oder unten auf der Seite die Option **Weiter: Überprüfen + erstellen** aus.

12. Wählen Sie auf der Registerkarte **Überprüfen und erstellen** die Option **Erstellen** aus.

Ihr Private Link-Dienst wird erstellt und kann Datenverkehr empfangen. Konfigurieren Sie Ihre Anwendung hinter Ihrer Instanz von Load Balancer Standard, falls Sie den Datenverkehrsfluss anzeigen möchten.


## <a name="create-private-endpoint"></a>Erstellen eines privaten Endpunkts

In diesem Abschnitt ordnen Sie den Private Link-Dienst einem privaten Endpunkt zu. Ein virtuelles Netzwerk enthält den privaten Endpunkt für den Private Link-Dienst. In diesem virtuellen Netzwerk sind die Ressourcen enthalten, die auf Ihren Private Link-Dienst zugreifen.

### <a name="create-private-endpoint-virtual-network"></a>Erstellen eines virtuellen Netzwerks des privaten Endpunkts

1. Wählen Sie links oben auf dem Bildschirm **Ressource erstellen > Netzwerk > Virtuelles Netzwerk** aus, oder suchen Sie über das Suchfeld nach **Virtuelles Netzwerk**.

2. Geben Sie unter **Virtuelles Netzwerk erstellen** auf der Registerkarte **Grundlegende Einstellungen** die folgenden Informationen ein, oder wählen Sie sie aus:

    | **Einstellung**          | **Wert**                                                           |
    |------------------|-----------------------------------------------------------------|
    | **Projektdetails**  |                                                                 |
    | Subscription     | Auswählen des Azure-Abonnements                                  |
    | Ressourcengruppe   | Wählen Sie **CreatePrivLinkService-rg** aus. |
    | **Instanzendetails** |                                                                 |
    | Name             | Geben Sie **myVNetPE** ein.                                    |
    | Region           | Wählen Sie **USA, Osten 2** aus. |

3. Wählen Sie die Registerkarte **IP-Adressen** oder die Schaltfläche **Weiter: IP-Adressen** am unteren Seitenrand aus.

4. Geben Sie auf der Registerkarte **IP-Adressen** die folgenden Informationen ein:

    | Einstellung            | Wert                      |
    |--------------------|----------------------------|
    | IPv4-Adressraum | Geben Sie **11.1.0.0/16** ein. |

5. Wählen Sie unter **Subnetzname** das Wort **Standard** aus.

6. Geben Sie unter **Subnetz bearbeiten** die folgenden Informationen ein:

    | Einstellung            | Wert                      |
    |--------------------|----------------------------|
    | Subnetzname | Geben Sie **mySubnetPE** ein. |
    | Subnetzadressbereich | Geben Sie **11.1.0.0/24** ein. |

7. Wählen Sie **Speichern** aus.

8. Wählen Sie die Registerkarte **Überprüfen + erstellen** oder die Schaltfläche **Überprüfen + erstellen** aus.

9. Klicken Sie auf **Erstellen**.

### <a name="create-private-endpoint"></a>Erstellen eines privaten Endpunkts

1. Wählen Sie auf der oberen linken Seite des Bildschirms im Portal die Option **Ressource erstellen** > **Netzwerk** > **Private Link** aus, oder geben Sie im Suchfeld **Private Link** ein.

2. Wählen Sie **Erstellen** aus.

3. Wählen Sie in **Private Link-Center** im linken Menü **Private Endpunkte** aus.

4. Wählen Sie unter **Private Endpunkte** die Option **+ Hinzufügen** aus.

5. Geben Sie auf der Registerkarte **Grundlagen** unter **Privaten Endpunkt erstellen** die folgenden Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | **Projektdetails** | |
    | Subscription | Wählen Sie Ihr Abonnement aus. |
    | Resource group | Wählen Sie **CreatePrivLinkService-rg** aus. Diese Ressourcengruppe wurde im vorherigen Abschnitt erstellt.|
    | **Instanzendetails** |  |
    | Name  | Geben Sie **myPrivateEndpoint** ein. |
    | Region | Wählen Sie **USA, Osten 2** aus. |

6. Wählen Sie die Registerkarte **Ressource** oder unten auf der Seite **Weiter: Ressource** aus.
    
7. Geben Sie in **Ressource** diese Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | Verbindungsmethode | Wählen Sie **Hiermit wird eine Verbindung mit einer Azure-Ressource im eigenen Verzeichnis hergestellt** aus. |
    | Subscription | Wählen Sie Ihr Abonnement aus. |
    | Ressourcentyp | Wählen Sie **Microsoft.Network/privateLinkServices** aus. |
    | Resource | Wählen Sie **myPrivateLinkService** aus. |

8. Wählen Sie die Registerkarte **Konfiguration** oder die Schaltfläche **Weiter: Konfiguration** am unteren Bildschirmrand aus.

9. Geben Sie diese Informationen in **Konfiguration** ein oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | **Netzwerk** |  |
    | Virtual Network | Wählen Sie **myVNetPE** aus. |
    | Subnet | Wählen Sie **mySubnetPE** aus. |

10. Wählen Sie die Registerkarte **Überprüfen + erstellen** oder am unteren Bildschirmrand die Schaltfläche **Überprüfen + erstellen** aus.

11. Wählen Sie **Erstellen** aus.

### <a name="ip-address-of-private-endpoint"></a>IP-Adresse des privaten Endpunkts

In diesem Abschnitt finden Sie die IP-Adresse des privaten Endpunkts, die zum Lastenausgleich und Private Link-Dienst gehört.

1. Wählen Sie in der linken Spalte des Azure-Portals die Option **Ressourcengruppen** aus.

2. Wählen Sie die Ressourcengruppe **CreatePrivLinkService-rg** aus.

3. Wählen Sie in der Ressourcengruppe **CreatePrivLinkService-rg** die Option **myPrivateEndpoint** aus.

4. Wählen Sie auf der Seite **Übersicht** von **myPrivateEndpoint** den Namen der Netzwerkschnittstelle aus, die dem privaten Endpunkt zugeordnet ist.  Der Name der Netzwerkschnittstelle beginnt mit **myPrivateEndpoint.nic**.

5. Auf der Seite **Übersicht** der NIC des privaten Endpunkts wird unter **Private IP-Adresse** die IP-Adresse des Endpunkts angezeigt.
    

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie den Private Link-Dienst nicht mehr benötigen, löschen Sie die Ressourcengruppe, um die im Rahmen dieser Schnellstartanleitung verwendeten Ressourcen zu bereinigen.

1. Geben Sie im Portal oben im Suchfeld den Suchbegriff **CreatePrivLinkService-rg** ein, und wählen Sie in den Suchergebnissen den Eintrag **CreatePrivLinkService-rg** aus.
1. Wählen Sie die Option **Ressourcengruppe löschen**.
1. Geben Sie unter **GEBEN SIE DEN RESSOURCENGRUPPENNAMEN EIN** den Namen **CreatePrivLinkService-rg** ein.
1. Klicken Sie auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie Folgendes durchgeführt:

* Erstellen eines virtuellen Netzwerks und einer internen Azure Load Balancer-Instanz
* Erstellen eines Private Link-Diensts
* Erstellen eines virtuellen Netzwerks und eines privaten Endpunkts für den Private Link-Dienst

Weitere Informationen zu privaten Azure-Endpunkten finden Sie unter:
> [!div class="nextstepaction"]
> [Schnellstart: Erstellen eines privaten Endpunkts mit dem Azure-Portal](create-private-endpoint-portal.md)