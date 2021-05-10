---
title: 'Tutorial: Hinzufügen von Azure Load Balancer zu einer vorhandenen VM-Skalierungsgruppe – Azure-Portal'
description: In diesem Tutorial erfahren Sie, wie Sie mithilfe des Azure-Portals einer vorhandenen VM-Skalierungsgruppe einen Lastenausgleich hinzufügen.
author: asudbring
ms.author: allensu
ms.service: load-balancer
ms.topic: tutorial
ms.date: 4/21/2021
ms.custom: template-tutorial
ms.openlocfilehash: cb87efe8402d49463d10e25aa79c50fc3b9b4c6e
ms.sourcegitcommit: bd1a4e4df613ff24e954eb3876aebff533b317ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2021
ms.locfileid: "107931036"
---
# <a name="tutorial-add-azure-load-balancer-to-an-existing-virtual-machine-scale-set-using-the-azure-portal"></a>Tutorial: Hinzufügen von Azure Load Balancer zu einer vorhandenen VM-Skalierungsgruppe mithilfe des Azure-Portals

Dieser Schritt ist unter Umständen erforderlich, wenn einer VM-Skalierungsgruppe keine Azure Load Balancer-Instanz zugeordnet ist. 

Möglicherweise haben Sie eine vorhandene VM-Skalierungsgruppe mit einer bereitgestellten Azure Load Balancer-Instanz, die aktualisiert werden muss.

Im Azure-Portal können Sie eine Azure Load Balancer-Instanz hinzufügen oder aktualisieren, die einer VM-Skalierungsgruppe zugeordnet ist.  

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines virtuellen Netzwerks
> * Erstellen eines NAT-Gateways für ausgehende Konnektivität
> * Erstellen einer Azure Load Balancer-Instanz mit der Standard-SKU
> * Erstellen einer VM-Skalierungsgruppe ohne Lastenausgleich
> * Hinzufügen einer Azure Load Balancer-Instanz zu einer VM-Skalierungsgruppe

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. [Kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)

## <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

In diesem Abschnitt erstellen Sie ein virtuelles Netzwerk für die Skalierungsgruppe und die anderen in diesem Tutorial verwendeten Ressourcen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Geben Sie im Suchfeld oben im Portal den Suchbegriff **Virtuelles Netzwerk** ein.

3. Wählen Sie in den Suchergebnissen **Virtuelle Netzwerke** aus.

4. Wählen Sie **+ Erstellen** aus.

5. Geben Sie unter **Virtuelles Netzwerk erstellen** auf der Registerkarte **Grundlagen** die folgenden Informationen an, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ------|
    | **Projektdetails** |   |
    | Subscription | Wählen Sie Ihr Abonnement aus. |
    | Ressourcengruppe | Wählen Sie **Neu erstellen**. </br> Geben Sie unter **Name** den Namen **TutorLBVMSS-rg** ein. </br> Klicken Sie auf **OK**. |
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
    | Ressourcengruppe | Wählen Sie **TutorLBVMSS-rg** aus. |
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
    | Ressourcengruppe | Wählen Sie **TutorLBVMSS-rg** aus. |
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

## <a name="create-virtual-machine-scale-set"></a>Erstellen einer VM-Skalierungsgruppe

In diesem Abschnitt erstellen Sie eine VM-Skalierungsgruppe ohne Lastenausgleich. Zu einem späteren Zeitpunkt fügen Sie im Azure-Portal dieser Skalierungsgruppe einen Lastenausgleich hinzu.

1. Geben Sie im Suchfeld oben im Portal den Suchbegriff **VM-Skalierungsgruppen** ein.

2. Wählen Sie in den Suchergebnissen **VM-Skalierungsgruppen** aus.

3. Wählen Sie **+ Hinzufügen**.

4. Geben Sie unter **VM-Skalierungsgruppe erstellen** auf der Registerkarte **Grundlagen** die folgenden Informationen ein, bzw. wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | **Projektdetails** |   |
    | Subscription | Wählen Sie Ihr Abonnement aus. |
    | Ressourcengruppe | Wählen Sie **TutorLBVMSS-rg** aus. |
    | **Details zur Skalierungsgruppe** |   |
    | Name der VM-Skalierungsgruppe | Geben Sie **myVMScaleSet** ein. |
    | Region | Wählen Sie **USA, Westen 2** aus. |
    | Verfügbarkeitszone | Übernehmen Sie den Standardwert **Keine**. |
    | **Orchestrierung** |   |
    | Orchestrierungsmodus | Übernehmen Sie den Standardwert **Einheitlich: Optimiert für umfangreiche statuslose Workloads mit identischen Instanzen**. |
    | **Instanzendetails** |   |
    | Image | Wählen Sie **Windows Server 2019 Datacenter – Gen1** aus. |
    | Azure Spot-Instanz | Übernehmen Sie die Standardeinstellung (Kontrollkästchen deaktiviert). |
    | Size | Wählen Sie eine Größe aus. |
    | **Administratorkonto** |
    | Username | Geben Sie einen Benutzernamen ein. |
    | Kennwort | Geben Sie ein Kennwort ein. |
    | Kennwort bestätigen | Bestätigen Sie das Kennwort. |

5. Wählen Sie die Registerkarte **Netzwerk** aus.

6. Geben Sie auf der Registerkarte **Netzwerk** die folgenden Informationen ein, bzw. wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | **Konfiguration des virtuellen Netzwerks** |   |
    | Virtuelles Netzwerk | Wählen Sie **myVNet** aus. |

7. Wählen Sie die Registerkarte **Überprüfen + erstellen** oder unten auf der Seite die blaue Schaltfläche **Überprüfen + erstellen** aus.

8. Klicken Sie auf **Erstellen**.

## <a name="add-load-balancer-to-scale-set"></a>Hinzufügen eines Lastenausgleichs zur Skalierung gruppe

In diesem Abschnitt navigieren Sie im Azure-Portal zur Skalierungsgruppe und fügen der Skalierungsgruppe einen Lastenausgleich hinzu.

1. Geben Sie im Suchfeld oben im Portal den Suchbegriff **VM-Skalierungsgruppen** ein.

2. Wählen Sie in den Suchergebnissen **VM-Skalierungsgruppen** aus.

3. Wählen Sie **myVMScaleSet** aus.

4. Wählen Sie im Abschnitt **Einstellungen** für **myVMScaleSet** die Option **Netzwerk** aus.

5. Wählen Sie auf der Seite **Übersicht** der **Netzwerkeinstellungen** von **myVMScaleSet** die Registerkarte **Lastenausgleich** aus.

    :::image type="content" source="./media/tutorial-add-lb-existing-scale-set-portal/load-balancing-tab.png" alt-text="Auswählen der Registerkarte „Lastenausgleich“ unter „Netzwerk“" border="true":::

6. Wählen Sie die blaue Schaltfläche **Lastenausgleich hinzufügen** aus.

7. Geben Sie unter **Lastenausgleich hinzufügen** die folgenden Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | Optionen für den Lastenausgleich | Wählen Sie **Azure-Lastenausgleich** aus. |
    | Wählen Sie einen Lastenausgleich aus. | Wählen Sie **myLoadBalancer** aus. |
    | Back-End-Pool | Wählen Sie **Vorhandenes verwenden** aus. |
    | Wählen Sie einen Back-End-Pool aus. | Wählen Sie **myBackendPool** aus. |

8. Wählen Sie **Speichern** aus.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Falls Sie diese Anwendung nicht weiterverwenden möchten, sollten Sie den Lastenausgleich und die unterstützenden Ressourcen wie folgt löschen:

1. Geben Sie oben im Portal im Suchfeld **Ressourcengruppe** ein.

2. Wählen Sie in den Suchergebnissen **Ressourcengruppen** aus.

3. Wählen Sie **TutorLBVMSS-rg** aus.

4. Wählen Sie auf der Seite „Übersicht“ von **TutorLBVMSS-rg** die Option **Ressourcengruppe löschen** aus.

5. Geben Sie unter **RESSOURCENGRUPPENNAMEN EINGEBEN** den Namen **TutorLBVMSS-rg** ein.

6. Klicken Sie auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial führen Sie Folgendes durch:

* Erstellen eines virtuellen Netzwerks und eines Azure Bastion-Hosts
* Erstellen einer Azure Load Balancer Standard-Instanz
* Erstellen einer VM-Skalierungsgruppe
* Hinzufügen eines Lastenausgleichs zu einer VM-Skalierungsgruppe

Fahren Sie mit dem nächsten Artikel fort, um sich über das Erstellen einer regionsübergreifenden Azure Load Balancer-Instanz zu informieren:
> [!div class="nextstepaction"]
> [Erstellen eines regionsübergreifenden Lastenausgleichs](tutorial-cross-region-portal.md)