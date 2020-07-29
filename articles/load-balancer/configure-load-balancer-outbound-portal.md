---
title: Konfigurieren von Lastenausgleichs- und Ausgangsregeln mithilfe des Azure-Portals
titleSuffix: Azure Load Balancer
description: In diesem Artikel wird gezeigt, wie Sie Lastenausgleichs- und Ausgangsregeln in Load Balancer Standard mithilfe des Azure-Portals konfigurieren.
services: load-balancer
author: asudbring
ms.service: load-balancer
ms.topic: how-to
ms.date: 09/24/2019
ms.author: allensu
ms.openlocfilehash: 0484bc393ac97dc88fed5858f736f01fc41b507a
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86521059"
---
# <a name="configure-load-balancing-and-outbound-rules-in-standard-load-balancer-by-using-the-azure-portal"></a>Konfigurieren von Lastenausgleichs- und Ausgangsregeln in Load Balancer Standard mithilfe des Azure-Portals

In diesem Artikel erfahren Sie, wie Sie Ausgangsregeln in Load Balancer Standard mithilfe des Azure-Portals konfigurieren.  

Die Load Balancer-Ressource enthält zwei Front-Ends und die ihnen zugeordneten Regeln. Sie haben ein Front-End für eingehenden und ein weiteres Front-End für ausgehenden Datenverkehr.  

Jedes Front-End verweist auf eine öffentliche IP-Adresse. In diesem Szenario unterscheidet sich die öffentliche IP-Adresse für eingehenden Datenverkehr von der Adresse für ausgehenden Datenverkehr.   Die Lastenausgleichsregel bietet nur eingehenden Lastenausgleich. Die Ausgangsregel steuert die ausgehende Netzwerkadressenübersetzung (NAT) für die VM.  

Das Szenario verwendet zwei Back-End-Pools: einen für eingehenden Datenverkehr und einen für ausgehenden Datenverkehr. Diese Pools veranschaulichen die Funktion und stellen dem Szenario Flexibilität zur Verfügung.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen. 

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.

## <a name="create-a-load-balancer"></a>Einrichten eines Load Balancers

In diesem Abschnitt erstellen Sie eine Load Balancer-Instanz für den Lastenausgleich virtueller Computer. Sie können eine öffentliche oder eine interne Load Balancer-Instanz erstellen. Wenn Sie eine öffentliche Load Balancer-Instanz erstellen, erstellen Sie eine neue öffentliche IP-Adresse, die als Front-End für die Load Balancer-Instanz konfiguriert ist. Das Front-End erhält standardmäßig den Namen **LoadBalancerFrontEnd**.

1. Wählens Sie links oben auf dem Bildschirm die Optionen **Ressource erstellen** > **Netzwerk** > **Lastenausgleich** aus.
2. Geben Sie auf der Seite **Lastenausgleich erstellen** auf der Registerkarte **Grundlagen** die folgenden Informationen ein, bzw. wählen Sie sie aus:

    | Einstellung                 | Wert                                              |
    | ---                     | ---                                                |
    | Subscription               | Wählen Sie Ihr Abonnement aus.    |    
    | Resource group         | Wählen Sie **Neu erstellen** aus, und geben Sie **myResourceGroupSLB** in das Textfeld ein.|
    | Name                   | **myLoadBalancer**                                   |
    | Region         | Wählen Sie **Europa, Westen** aus.                                        |
    | type          | Wählen Sie **Öffentlich** aus.                                        |
    | SKU           | Wählen Sie **Standard** aus. |
    | Öffentliche IP-Adresse | Wählen Sie **Neu erstellen**. Wenn Sie über eine vorhandene öffentliche IP-Adresse verfügen, die Sie verwenden möchten, wählen Sie **Vorhandene verwenden** aus.  Die vorhandene öffentliche IP-Adresse muss eine **Standard**-SKU sein.  Öffentliche IP-Adressen des Typs „Basic“ sind nicht mit einem Lastenausgleich des Typs **Standard**-SKU kompatibel.  |
    | Name der öffentlichen IP-Adresse              | Geben Sie **myPublicIP** in das Textfeld ein.|
    | Verfügbarkeitszone | Wählen Sie **Zonenredundant** aus, um eine resiliente Load Balancer-Instanz zu erstellen. Wählen Sie zum Erstellen einer zonalen Load Balancer-Instanz eine bestimmte Zone aus 1, 2 oder 3 aus. |

3. Übernehmen Sie für den Rest der Konfiguration die Standardwerte.
4. Klicken Sie auf **Überprüfen + erstellen**.

    > [!IMPORTANT]
    > Im weiteren Verlauf dieser Schnellstartanleitung wird davon ausgegangen, dass bei der SKU-Auswahl oben die SKU **Standard** ausgewählt wurde.

5. Wählen Sie auf der Registerkarte **Bewerten + erstellen** die Option **Erstellen** aus.   

    ![Einrichten eines Load Balancers im Tarif „Standard“](./media/quickstart-load-balancer-standard-public-portal/create-standard-load-balancer.png)

## <a name="create-load-balancer-resources"></a>Erstellen von Load Balancer-Ressourcen

In diesem Abschnitt konfigurieren Sie Lastenausgleichseinstellungen für einen Back-End-Adresspool und einen Integritätstest. Außerdem legen Sie eine Lastenausgleichsregel fest.

### <a name="create-a-backend-pool"></a>Erstellen eines Back-End-Pools

Ein Back-End-Adresspool enthält die IP-Adressen der virtuellen NICs im Back-End-Pool. Erstellen Sie den Back-End-Adresspool **myBackendPool**, um virtuelle Computer für den Lastenausgleich von Internetdatenverkehr einzubeziehen.

1. Wählen Sie im linken Menü **Alle Dienste** > **Alle Ressourcen** und anschließend in der Ressourcenliste den Eintrag **myLoadBalancer** aus.
2. Wählen Sie unter **Einstellungen** die Option **Back-End-Pools** und dann **Hinzufügen** aus.
3. Geben Sie auf der Seite **Back-End-Pool hinzufügen** die Zeichenfolge **myBackendPool** als Name für Ihren Back-End-Pool ein, und wählen Sie anschließend **Hinzufügen** aus.

### <a name="create-a-health-probe"></a>Erstellen eines Integritätstests

Ein Integritätstest wird verwendet, um den Status Ihrer App zu überwachen. Abhängig von der Reaktion auf Integritätsüberprüfungen werden der Load Balancer-Instanz durch den Integritätstest virtuelle Computer hinzugefügt oder daraus entfernt. Erstellen Sie zur Überwachung der Integrität der virtuellen Computer einen Integritätstest namens **myHealthProbe**.

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
4. Klicken Sie auf **OK**.

### <a name="create-a-load-balancer-rule"></a>Erstellen einer Load Balancer-Regel
Mithilfe einer Load Balancer-Regel wird definiert, wie Datenverkehr auf die virtuellen Computer verteilt werden soll. 

Sie definieren Folgendes:
 - Front-End-IP-Konfiguration für den eingehenden Datenverkehr.
 - Back-End-IP-Pool zum Empfangen des Datenverkehrs.
 - Erforderliche Quell- und Zielports. 

Im folgenden Abschnitt erstellen Sie Folgendes:
 - Die Lastenausgleichsregel **myHTTPRule** für das Lauschen an Port 80.
 - Front-End-**LoadBalancerFrontEnd**.
 - Back-End-Adresspool **myBackEndPool**, der ebenfalls Port 80 verwendet. 

1. Wählen Sie im linken Menü **Alle Dienste** > **Alle Ressourcen** und anschließend in der Ressourcenliste den Eintrag **myLoadBalancer** aus.
2. Wählen Sie unter **Einstellungen** die Option **Lastenausgleichsregeln** und dann **Hinzufügen** aus.
3. Konfigurieren Sie die Lastenausgleichsregel mit folgenden Werten:
    
    | Einstellung | Wert |
    | ------- | ----- |
    | Name | Geben Sie **myHTTPRule** ein. |
    | Protocol | Wählen Sie **TCP** aus. |
    | Port | Geben Sie **80** ein.|
    | Back-End-Port | Geben Sie **80** ein. |
    | Back-End-Pool | Wählen Sie **myBackendPool** aus.|
    | Integritätstest | Wählen Sie **myHealthProbe** aus. |
    | Erstellen impliziter Ausgangsregeln | Wählen Sie also **Nein**. Wir werden in einem späteren Abschnitt Ausgangsregeln unter Verwendung einer dedizierten öffentlichen IP-Adresse erstellen. |
4. Übernehmen Sie die übrigen Standardeinstellungen, und wählen Sie dann **OK** aus.

## <a name="create-outbound-rule-configuration"></a>Erstellen der Konfiguration für Ausgangsregeln
Ausgangsregeln für den Lastenausgleich konfigurieren das ausgehende SNAT für virtuelle Computer im Back-End-Pool. 

### <a name="create-an-outbound-public-ip-address-and-frontend"></a>Erstellen einer öffentlichen IP-Ausgangsadresse und eines Front-Ends

1. Wählen Sie im linken Menü **Alle Dienste** > **Alle Ressourcen** und anschließend in der Ressourcenliste den Eintrag **myLoadBalancer** aus.

2. Klicken Sie unter **Einstellungen** auf **Front-End-IP-Konfiguration**, und wählen Sie dann **Hinzufügen** aus.

3. Verwenden Sie diese Werte zum Konfigurieren der Front-End-IP-Konfiguration für ausgehenden Datenverkehr:

    | Einstellung | Wert |
    | ------- | ----- |
    | Name | Geben Sie **LoadBalancerFrontEndOutbound** ein. |
    | IP-Version | Wählen Sie **IPv4** aus. |
    | IP-Typ | Wählen Sie **IP-Adresse** oder **IP-Präfix** aus.|
    | Öffentliche IP-Adresse | Wählen Sie **Neu erstellen**. </br> Geben Sie im Feld **Öffentliche IP-Adresse hinzufügen** den Eintrag **myPublicIPOutbound** ein. </br> Wenn Sie „IP-Präfix“ verwenden, geben Sie unter **Präfix für öffentliche IP-Adressen hinzufügen** die Zeichenfolge **myPublicIPPrefixOutbound** ein. Wählen Sie unter **Präfixgröße** enie Größe für das öffentliche IP-Präfix aus. </br> Klicken Sie auf **OK**.  |

4. Wählen Sie **Hinzufügen**.

### <a name="create-an-outbound-backend-pool"></a>Erstellen eines Back-End-Ausgangspools

1. Wählen Sie im linken Menü **Alle Dienste** > **Alle Ressourcen** und anschließend in der Ressourcenliste den Eintrag **myLoadBalancer** aus.

2. Wählen Sie unter **Einstellungen** die Option **Back-End-Pools** und dann **Hinzufügen** aus.

3. Geben Sie auf der Seite **Back-End-Pool hinzufügen** die Zeichenfolge **myBackendPoolOutbound** als Name für Ihren Back-End-Pool ein, und wählen Sie anschließend **Hinzufügen** aus.

### <a name="create-outbound-rule"></a>Erstellen einer Ausgangsregel

1. Wählen Sie im linken Menü **Alle Dienste** > **Alle Ressourcen** und anschließend in der Ressourcenliste den Eintrag **myLoadBalancer** aus.

2. Wählen Sie unter **Einstellungen** die Option **Ausgangsregeln** und dann **Hinzufügen** aus.

3. Verwenden Sie diese Werte zur Konfiguration der Ausgangsregeln:

    | Einstellung | Wert |
    | ------- | ----- |
    | Name | Geben Sie **myOutboundRule** ein. |
    | Front-End-IP-Adresse | Wählen Sie **LoadBalancerFrontEndOutbound** aus. |
    | Leerlaufzeitüberschreitung (Minuten) | Bewegen Sie den Schieberegler auf **15 Minuten**.|
    | TCP-Zurücksetzung | Wählen Sie **Aktiviert**.|
    | Back-End-Pool | Wählen Sie **myBackendPoolOutbound** aus. |
    | Portzuordnung -> Portzuordnung | Wählen Sie **Anzahl ausgehender Ports manuell auswählen** aus. |
    | Ausgehende Ports -> Auswählen nach | Wählen Sie **Ports pro Instanz** aus. |
    | Ausgehende Ports -> Ports pro Instanz | Geben Sie **10.000** ein. |

4. Wählen Sie **Hinzufügen**.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcengruppe, den Lastenausgleich und alle dazugehörigen Ressourcen, wenn Sie sie nicht mehr benötigen. Wählen Sie die Ressourcengruppe **myResourceGroupSLB** aus, die den Lastenausgleich enthält, und wählen Sie anschließend **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

Inhalt dieses Artikels:
 - Sie haben eine Load Balancer Standard-Instanz erstellt.
 - Es wurden Regeln für eingehenden als auch ausgehenden Datenverkehr für den Lastenausgleich konfiguriert.
 - Es wurde ein Integritätstest für die virtuellen Computer im Back-End-Pool konfiguriert. 

Weitere Informationen zum Lastenausgleich finden Sie in den [Tutorials zum Azure Load Balancer](tutorial-load-balancer-standard-public-zone-redundant-portal.md).
