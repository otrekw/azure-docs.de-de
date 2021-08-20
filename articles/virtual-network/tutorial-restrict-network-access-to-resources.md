---
title: Einschränken des Zugriffs auf PaaS-Ressourcen – Tutorial – Azure-Portal
description: In diesem Tutorial erfahren Sie, wie Sie mithilfe des Azure-Portals den Netzwerkzugriff auf Azure-Ressourcen wie Azure Storage mit VNET-Dienstendpunkten einschränken.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: mtillman
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 07/16/2021
ms.author: kumud
ms.openlocfilehash: a0d721e847d0e47358bfbeb640b9e9a6e6a551a3
ms.sourcegitcommit: 7d63ce88bfe8188b1ae70c3d006a29068d066287
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2021
ms.locfileid: "114463362"
---
# <a name="tutorial-restrict-network-access-to-paas-resources-with-virtual-network-service-endpoints-using-the-azure-portal"></a>Tutorial: Einschränken des Netzwerkzugriffs auf PaaS-Ressourcen mit VNET-Dienstendpunkten mithilfe des Azure-Portals

VNET-Dienstendpunkte ermöglichen es Ihnen, den Netzwerkzugriff auf einige Azure-Dienstressourcen auf ein Subnetz eines virtuellen Netzwerks einzuschränken. Sie können auch den Internetzugriff auf die Ressourcen entfernen. Dienstendpunkte ermöglichen eine direkte Verbindung zwischen Ihrem virtuellen Netzwerk und unterstützten Azure-Diensten, sodass Sie mithilfe des privaten Adressraums Ihres virtuellen Netzwerks auf die Azure-Dienste zugreifen können. Datenverkehr, der über Dienstendpunkte für Azure-Ressourcen bestimmt ist, verbleibt immer im Microsoft Azure-Backbonenetzwerk. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines virtuellen Netzwerks mit einem Subnetz
> * Hinzufügen eines Subnetzes und Aktivieren eines Dienstendpunkts
> * Erstellen einer Azure-Ressource und Zulassen des Netzwerkzugriffs darauf ausschließlich aus einem Subnetz
> * Bereitstellen eines virtuellen Computers für jedes Subnetz
> * Bestätigen des Zugriffs auf eine Ressource aus einem Subnetz
> * Bestätigen, dass der Zugriff auf eine Ressource aus einem Subnetz und dem Internet verweigert wird

Dieser Artikel kann auch mit der [Azure-Befehlszeilenschnittstelle](tutorial-restrict-network-access-to-resources-cli.md) oder mit [Azure PowerShell](tutorial-restrict-network-access-to-resources-powershell.md) durchgearbeitet werden.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Wählen Sie im Azure-Portal links oben **+ Ressource erstellen** aus. Suchen Sie nach **Virtuelles Netzwerk**, und wählen Sie dann **Erstellen** aus.

    :::image type="content" source="./media/tutorial-restrict-network-access-to-resources/create-resources.png" alt-text="Screenshot der Suche nach einem virtuellen Netzwerk auf der Seite „Ressource erstellen“":::    

1. Geben Sie auf der Registerkarte **Grundlagen** die folgenden Informationen ein, und wählen Sie anschließend **Weiter: IP-Adressen >** aus. 

   | Einstellung | Wert |
   |----|----|
   | Subscription | Wählen Sie Ihr Abonnement aus.|
   | Resource group | Klicken Sie auf **Neu erstellen**, und geben Sie *myResourceGroup* ein.|
   | Name | Geben Sie *myVirtualNetwork* ein. |
   | Region | Wählen Sie **USA, Osten** aus. |

    :::image type="content" source="./media/tutorial-restrict-network-access-to-resources/create-virtual-network.png" alt-text="Screenshot der Registerkarte „Grundlagen“ zum Erstellen eines virtuellen Netzwerks":::  

1. Wählen Sie auf der Registerkarte **IP-Adressen** die folgenden Einstellungen für IP-Adressen und anschließend **Überprüfen und erstellen** aus.
   
   | Einstellung | Wert |
   | --- | --- |
   | IPv4-Adressraum| Behalten Sie den Standard bei. |
   | Subnetzname | Wählen Sie **Standard** aus, und ändern Sie den Subnetznamen in „Public“. |
   | Subnetzadressbereich | Behalten Sie den Standard bei. |

    :::image type="content" source="./media/tutorial-restrict-network-access-to-resources/create-virtual-network-ip-addresses.png" alt-text="Screenshot der Registerkarte „IP-Adressen“ zum Erstellen eines virtuellen Netzwerks":::  

1. Wenn die Validierungsprüfungen erfolgreich waren, wählen Sie **Erstellen** aus.

1. Warten Sie, bis die Bereitstellung beendet ist, und wählen Sie dann **Zu Ressource wechseln** aus, oder fahren Sie mit dem nächsten Abschnitt fort. 

## <a name="enable-a-service-endpoint"></a>Aktivieren eines Dienstendpunkts

Dienstendpunkte werden pro Dienst und pro Subnetz aktiviert. So erstellen Sie ein Subnetz und fügen einen Dienstendpunkt für das Subnetz hinzu

1. Wenn Sie sich noch nicht auf der Seite „Ressource“ des virtuellen Netzwerks befinden, können Sie im Feld oben im Portal nach dem neu erstellten Netzwerk suchen. Geben Sie den Namen *myVirtualNetwork* ein, und wählen Sie ihn in der Liste aus.

1. Wählen Sie unter *Einstellungen* die Option **Subnetze** und dann **+ Subnetz** aus, wie in der folgenden Abbildung gezeigt:

    :::image type="content" source="./media/tutorial-restrict-network-access-to-resources/add-subnet.png" alt-text="Screenshot des Hinzufügens eines Subnetzes zu einem vorhandenen virtuellen Netzwerk":::

1. Wählen Sie auf der Seite **Subnetz hinzufügen** die folgenden Informationen aus, oder geben Sie sie ein, und wählen Sie dann **Speichern** aus:

    | Einstellung |Wert |
    | --- | --- |
    | Name | Privat |
    | Subnetzadressbereich | Behalten Sie den Standard bei.|
    | Dienstendpunkte | Wählen Sie **Microsoft.Storage** aus.|
    | Dienstendpunkt-Richtlinien | Behalten Sie den Standardwert bei. *0 ausgewählt* |

    :::image type="content" source="./media/tutorial-restrict-network-access-to-resources/add-subnet-settings.png" alt-text="Screenshot der Seite „Subnetz hinzufügen“ mit konfigurierten Dienstendpunkten":::  

> [!CAUTION]
> Lesen Sie vor der Aktivierung eines Dienstendpunkts für ein vorhandenes Subnetz, das Ressourcen enthält, die Informationen unter [Ändern von Subnetzeinstellungen](virtual-network-manage-subnet.md#change-subnet-settings).

## <a name="restrict-network-access-for-a-subnet"></a>Einschränken des Netzwerkzugriffs für ein Subnetz

Standardmäßig können alle VM-Instanzen in einem Subnetz mit allen Ressourcen kommunizieren. Sie können die bidirektionale Kommunikation mit allen Ressourcen in einem Subnetz einschränken, indem Sie eine Netzwerksicherheitsgruppe erstellen und dem Subnetz zuordnen:

1. Suchen Sie über das Suchfeld oben im Azure-Portal nach **Netzwerksicherheitsgruppen**.

    :::image type="content" source="./media/tutorial-restrict-network-access-to-resources/search-network-security-groups.png" alt-text="Screenshot der Suche nach Netzwerksicherheitsgruppen":::  

1. Wählen Sie auf der Seite *Netzwerksicherheitsgruppen* die Option **+ Erstellen** aus.

    :::image type="content" source="./media/tutorial-restrict-network-access-to-resources/network-security-groups-page.png" alt-text="Screenshot der Landing Page für Netzwerksicherheitsgruppen"::: 

1. Geben Sie die folgenden Informationen an:

    |Einstellung|Wert|
    |----|----|
    |Subscription| Wählen Sie Ihr Abonnement aus.|
    |Ressourcengruppe | Wählen Sie *myResourceGroup* in der Liste aus.|
    |Name| Geben Sie **myNsgPrivate** ein. |
    |Standort| Wählen Sie **USA, Osten** aus. |

1. Wählen Sie **Überprüfen und erstellen** und nach Abschluss der Validierungsprüfung **Erstellen** aus.

    :::image type="content" source="./media/tutorial-restrict-network-access-to-resources/create-nsg-page.png" alt-text="Screenshot der Seite zur Erstellung einer Netzwerksicherheitsgruppe":::

1. Nachdem die Netzwerksicherheitsgruppe erstellt wurde, wählen Sie **Zu Ressource wechseln** aus, oder suchen Sie oben im Azure-Portal nach *myNsgPrivate*.

1. Wählen Sie unter *Einstellungen* die Option **Sicherheitsregeln für ausgehenden Datenverkehr** und dann **+ Hinzufügen** aus.

    :::image type="content" source="./media/tutorial-restrict-network-access-to-resources/create-outbound-rule.png" alt-text="Screenshot des Hinzufügens einer Sicherheitsregel für ausgehenden Datenverkehr" lightbox="./media/tutorial-restrict-network-access-to-resources/create-outbound-rule-expanded.png":::

1. Erstellen Sie eine Regel, die ausgehende Kommunikation mit dem Azure Storage-Dienst zulässt. Geben Sie die folgenden Informationen ein (oder wählen Sie sie aus), und klicken Sie anschließend auf **Hinzufügen**:

    |Einstellung|Wert|
    |----|----|
    |`Source`| Wählen Sie **VirtualNetwork** aus. |
    |Source port ranges| * |
    |Destination | Wählen Sie **Diensttag** aus.|
    |Zieldiensttag | Wählen Sie **Storage** aus.|
    |Dienst | Lassen Sie den Standardwert *Benutzerdefiniert* unverändert. |
    |Zielportbereiche| Ändern Sie den Wert in *445*. Für die Verbindung mit einer Dateifreigabe in einem späteren Schritt wird das SMB-Protokoll verwendet. |
    |Protocol|Beliebig|
    |Aktion|Allow|
    |Priorität|100|
    |Name|Benennen Sie es in **Allow-Storage-All** um.|

    :::image type="content" source="./media/tutorial-restrict-network-access-to-resources/create-outbound-storage-rule.png" alt-text="Screenshot der Erstellung einer Sicherheitsregel für ausgehenden Datenverkehr für den Speicherzugriff":::

1. Erstellen Sie eine weitere Ausgangssicherheitsregel, die Kommunikation mit dem Internet verweigert. Diese Regel überschreibt eine Standardregel in allen Netzwerksicherheitsgruppen, die ausgehende Internetkommunikation zulässt. Wiederholen Sie die obigen Schritte 6–9 mit folgenden Werten, und wählen Sie dann **Hinzufügen** aus:

    |Einstellung|Wert|
    |----|----|
    |`Source`| Wählen Sie **VirtualNetwork** aus. |
    |Source port ranges| * |
    |Destination | Wählen Sie **Diensttag** aus.|
    |Zieldiensttag| Wählen Sie **Internet** aus.|
    |Dienst| Lassen Sie den Standardwert *Benutzerdefiniert* unverändert. |
    |Zielportbereiche| * |
    |Protocol|Beliebig|
    |Aktion| Ändern Sie den Standardwert in **Ablehnen**. |
    |Priority|110|
    |Name|Ändern Sie den Wert in *Deny-Internet-All*.|

    :::image type="content" source="./media/tutorial-restrict-network-access-to-resources/create-outbound-internet-rule.png" alt-text="Screenshot der Erstellung einer Sicherheitsregel für ausgehenden Datenverkehr zum Blockieren des Internetzugriffs":::

1. Erstellen Sie eine *Eingangssicherheitsregel*, die RDP-Datenverkehr (Remote Desktop Protocol) an das Subnetz von überall erlaubt. Die Regel setzt eine Standardsicherheitsregel außer Kraft, die jeglichen eingehenden Verkehr aus dem Internet abweist. Remotedesktopverbindungen in das Subnetz sind zulässig, sodass die Konnektivität in einem späteren Schritt getestet werden kann. Wählen Sie unter *Einstellungen* die Option **Sicherheitsregeln für eingehenden Datenverkehr** und dann **+ Hinzufügen** aus.

    :::image type="content" source="./media/tutorial-restrict-network-access-to-resources/create-inbound-rule.png" alt-text="Screenshot des Hinzufügens einer Sicherheitsregel für eingehenden Datenverkehr" lightbox="./media/tutorial-restrict-network-access-to-resources/create-inbound-rule-expanded.png":::

1. Geben Sie die folgenden Werte ein, oder wählen Sie sie aus, und wählen Sie dann **Hinzufügen** aus.

    |Einstellung|Wert|
    |----|----|
    |`Source`| Beliebig |
    |Source port ranges| * |
    |Destination | Wählen Sie **VirtualNetwork** aus.|
    |Zielportbereiche| Ändern Sie den Wert in *3389*. |
    |Protocol|Beliebig|
    |Aktion|Allow|
    |Priority|120|
    |Name|Ändern Sie den Wert in *Allow-RDP-All*.|

    :::image type="content" source="./media/tutorial-restrict-network-access-to-resources/create-inbound-rdp-rule.png" alt-text="Screenshot der Erstellung einer Sicherheitsregel für eingehenden Datenverkehr für Remotedesktop":::

   >[!WARNING] 
   > RDP-Port 3389 wird für das Internet verfügbar gemacht. Dies wird nur für Tests empfohlen. Für *Produktionsumgebungen* empfehlen wir die Verwendung einer VPN- oder privaten Verbindung.

1.  Wählen Sie unter *Einstellungen* die Option **Subnetze** und dann **+ Zuordnen** aus.

    :::image type="content" source="./media/tutorial-restrict-network-access-to-resources/associate-subnets-page.png" alt-text="Screenshot der Seite für die Subnetzzuordnung für Netzwerksicherheitsgruppen":::

1.  Wählen Sie unter *Virtuelles Netzwerk* das Netzwerk **myVirtualNetwork** und anschließend unter *Subnetze* die Option **Privat** aus. Wählen Sie **OK** aus, um die Netzwerksicherheitsgruppe dem ausgewählten Subnetz zuzuordnen.

    :::image type="content" source="./media/tutorial-restrict-network-access-to-resources/associate-private-subnet.png" alt-text="Screenshot der Zuordnung einer Netzwerksicherheitsgruppe zu einem privaten Subnetz":::

## <a name="restrict-network-access-to-a-resource"></a>Einschränken des Netzwerkzugriffs auf eine Ressource

Die Schritte, die erforderlich sind, um den Netzwerkzugriff auf Ressourcen einzuschränken, die durch Azure-Dienste erstellt und für Dienstendpunkte aktiviert wurden, sind je nach Dienst unterschiedlich. Informationen zu den Schritten für einzelne Dienste finden Sie in der Dokumentation des jeweiligen Diensts. Im weiteren Verlauf dieses Tutorials wird als Beispiel der Netzwerkzugriff für ein Azure Storage-Konto eingeschränkt.

### <a name="create-a-storage-account"></a>Speicherkonto erstellen

1. Klicken Sie im Azure-Portal links oben auf **+ Ressource erstellen**.

1. Geben Sie „Speicherkonto“ in die Suchleiste ein, und wählen Sie es in der Dropdownliste aus. Klicken Sie anschließend auf **Erstellen**.

1. Geben Sie Folgendes ein:

    |Einstellung|Wert|
    |----|----|
    |Subscription| Wählen Sie Ihr Abonnement aus.|
    |Ressourcengruppe| Wählen Sie *myResourceGroup* aus.|
    |Speicherkontoname| Der eingegebene Name muss für sämtliche Azure-Standorte eindeutig sein. Der Name muss zwischen 3 und 24 Zeichen lang sein und darf nur Zahlen und Kleinbuchstaben enthalten.|
    |Region| Wählen Sie **USA, Osten** aus. |
    |Leistung|Standard|
    |Redundanz| Lokal redundanter Speicher (LRS)|

    :::image type="content" source="./media/tutorial-restrict-network-access-to-resources/create-storage-account.png" alt-text="Screenshot der Erstellung eines neuen Speicherkontos":::

1. Wählen Sie **Erstellen und überprüfen** und nach Abschluss der Validierungsprüfungen **Erstellen** aus. 

    >[!NOTE] 
    > Die Bereitstellung kann einige Minuten in Anspruch nehmen.

1. Wählen Sie nach dem Erstellen des neuen Speicherkontos **Zu Ressource wechseln** aus.

### <a name="create-a-file-share-in-the-storage-account"></a>Erstellen einer Dateifreigabe im Speicherkonto

1. Wählen Sie unter *Dateidienst* die Option **Dateifreigaben** und dann **+ Dateifreigabe** aus.

    :::image type="content" source="./media/tutorial-restrict-network-access-to-resources/file-share-page.png" alt-text="Screenshot der Seite „Dateifreigabe“ in einem Speicherkonto":::

1. Geben Sie die folgenden Werte für die Dateifreigabe ein, oder legen Sie sie fest, und wählen Sie dann **Erstellen** aus:

    |Einstellung|Wert|
    |----|----|
    |Name| my-file-share|
    |Kontingent| Wählen Sie **Set to maximum** (Auf Maximum festlegen) aus. |
    |Tarif| Behalten Sie die Standardeinstellung *Transaction optimized* (Transaktionsoptimiert) bei. |

    :::image type="content" source="./media/tutorial-restrict-network-access-to-resources/create-new-file-share.png" alt-text="Screenshot der Einstellungsseite für das Erstellen einer neuen Dateifreigabe":::

1. Die neue Dateifreigabe sollte auf der Dateifreigabeseite angezeigt werden. Wählen Sie andernfalls oben auf der Seite die Schaltfläche **Aktualisieren** aus.

### <a name="restrict-network-access-to-a-subnet"></a>Einschränken des Netzwerkzugriffs auf ein Subnetz

Standardmäßig akzeptieren Speicherkonten Netzwerkverbindungen von Clients in allen Netzwerken, einschließlich des Internets. Sie können den Zugriff über das Internet sowie alle anderen Subnetze in allen virtuellen Netzwerken einschränken (außer über das Subnetz *Private* im virtuellen Netzwerk *myVirtualNetwork*). So schränken Sie den Netzwerkzugriff auf ein Subnetz ein

1. Wählen Sie unter *Einstellungen* für Ihr (eindeutig benanntes) Speicherkonto die Option **Netzwerkbetrieb** aus.

1. Wählen Sie *Zugriff zulassen über **Ausgewählte Netzwerke** _ und dann _* + Vorhandenes virtuelles Netzwerk hinzufügen** aus.

    :::image type="content" source="./media/tutorial-restrict-network-access-to-resources/storage-network-settings.png" alt-text="Screenshot der Seite mit den Netzwerkeinstellungen des Speicherkontos":::

1. Wählen Sie unter **Netzwerke hinzufügen** die folgenden Werte und dann **Hinzufügen** aus:

    |Einstellung|Wert|
    |----|----|
    |Subscription| Wählen Sie Ihr Abonnement aus.|
    |Virtuelle Netzwerke| **myVirtualNetwork**|
    |Subnetze| **Privat**|

    :::image type="content" source="./media/tutorial-restrict-network-access-to-resources/add-virtual-network.png" alt-text="Screenshot der Seite für das Hinzufügen eines virtuellen Netzwerks zum Speicherkonto":::

1. Wählen Sie die Schaltfläche **Speichern** aus, um die Konfiguration des virtuellen Netzwerks zu speichern.

1. Wählen Sie unter *Sicherheit + Netzwerkbetrieb* für das Speicherkonto die Option **Zugriffsschlüssel** und dann **Schlüssel anzeigen** aus. Notieren Sie den Wert des Schlüssels „key1“, da Sie ihn beim Zuordnen der Dateifreigabe zu einer VM in einem späteren Schritt benötigen.

    :::image type="content" source="./media/tutorial-restrict-network-access-to-resources/storage-access-key.png" alt-text="Screenshot mit Speicherkontoschlüssel und Verbindungszeichenfolgen" lightbox="./media/tutorial-restrict-network-access-to-resources/storage-access-key-expanded.png":::

## <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern

Zum Testen des Netzwerkzugriffs auf ein Speicherkonto stellen Sie einen virtuellen Computer für jedes Subnetz bereit.

### <a name="create-the-first-virtual-machine"></a>Erstellen des ersten virtuellen Computers

1. Wählen Sie im Azure-Portal die Option **+ Ressource erstellen** aus.

1. Wählen Sie **Compute** und dann unter *Virtueller Computer* die Option **Erstellen** aus.

1. Geben Sie auf der Registerkarte *Grundlagen* die folgenden Informationen an, oder wählen Sie sie aus:

   |Einstellung|Wert|
   |----|----|
   |Subscription| Wählen Sie Ihr Abonnement aus.|
   |Resource group| Wählen Sie die zuvor erstellte **myResourceGroup** aus.|
   |Name des virtuellen Computers| Geben Sie *myVmPublic* ein.|
   |Region | (USA) USA, Osten
   |Verfügbarkeitsoptionen| Verfügbarkeitszone|
   |Verfügbarkeitszone | 1 |
   |Image | Wählen Sie ein Betriebssystemimage aus. Für diese VM wurde *Windows Server 2019 Datacenter – Gen1* ausgewählt. |
   |Size | Wählen Sie die Größe der VM-Instanz aus, die Sie verwenden möchten. |
   |Username|Geben Sie den gewünschten Benutzernamen ein.|
   |Kennwort| Geben Sie das gewünschte Kennwort ein. Das Kennwort muss mindestens zwölf Zeichen lang sein und die [definierten Anforderungen an die Komplexität](../virtual-machines/windows/faq.yml?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm-) erfüllen.|
   |Öffentliche Eingangsports | Ausgewählte Ports zulassen |
   |Eingangsports auswählen | Belassen Sie die Standardeinstellung *RDP (3389)* . |

    :::image type="content" source="./media/tutorial-restrict-network-access-to-resources/create-public-vm-settings.png" alt-text="Screenshot der Einstellungen für die Erstellung öffentlicher VMs" lightbox="./media/tutorial-restrict-network-access-to-resources/create-public-vm-settings-expanded.png":::
  
1. Geben Sie auf der Registerkarte **Netzwerkbetrieb** die folgenden Informationen ein, oder wählen Sie sie aus:

    |Einstellung|Wert|
    |----|----|
    | Virtual Network | Wählen Sie **myVirtualNetwork** aus. |
    | Subnet | Wählen Sie **Öffentlich** aus. |
    | NIC-Netzwerksicherheitsgruppe | Wählen Sie **Erweitert** aus. Das Portal erstellt automatisch eine Netzwerksicherheitsgruppe, die den Port 3389 zulässt. Dieser Port muss geöffnet werden, um in einem späteren Schritt eine Verbindung mit der VM herstellen zu können. |

    :::image type="content" source="./media/tutorial-restrict-network-access-to-resources/virtual-machine-networking.png" alt-text="Screenshot der Netzwerkeinstellungen für die Erstellung öffentlicher VMs" lightbox="./media/tutorial-restrict-network-access-to-resources/virtual-machine-networking-expanded.png":::

1. Wählen Sie **Überprüfen und erstellen**, dann **Erstellen** aus und warten Sie, bis die Bereitstellung abgeschlossen ist.

1. Wählen Sie **Zu Ressource wechseln** aus, oder öffnen Sie die Seite **Startseite > Virtuelle Computer**, und wählen Sie die soeben erstellte VM *myVmPublic* aus, die gestartet werden sollte.

### <a name="create-the-second-virtual-machine"></a>Erstellen des zweiten virtuellen Computers

1. Wiederholen Sie die Schritte 1–5, um eine zweite VM zu erstellen. Geben Sie in Schritt 3 der VM den Namen *myVmPrivate*, und legen Sie *NIC-Netzwerksicherheitsgruppe* auf **Keine** fest. Wählen Sie in Schritt 4 das Subnetz **Private** aus.

   :::image type="content" source="./media/tutorial-restrict-network-access-to-resources/virtual-machine-2-networking.png" alt-text="Screenshot der Netzwerkeinstellungen für die Erstellung privater VMs" lightbox="./media/tutorial-restrict-network-access-to-resources/virtual-machine-2-networking-expanded.png":::

1. Wählen Sie **Überprüfen und erstellen**, dann **Erstellen** aus und warten Sie, bis die Bereitstellung abgeschlossen ist. 

    > [!WARNING]
    > Fahren Sie erst mit dem nächsten Schritt fort, wenn die Bereitstellung abgeschlossen ist.

1. Wählen Sie **Zu Ressource wechseln** aus, oder öffnen Sie die Seite **Startseite > Virtuelle Computer**, und wählen Sie die soeben erstellte VM *myVmPrivate* aus, die gestartet werden sollte.

## <a name="confirm-access-to-storage-account"></a>Bestätigen des Zugriffs auf das Speicherkonto

1. Nachdem die VM *myVmPrivate* erstellt wurde, wechseln Sie zur Übersichtsseite der VM. Stellen Sie eine Verbindung mit der VM her, indem Sie die Schaltfläche **Verbinden** auswählen, und wählen Sie dann in der Dropdownliste **RDP** aus.

    :::image type="content" source="./media/tutorial-restrict-network-access-to-resources/connect-private-vm.png" alt-text="Screenshot der Schaltfläche „Verbinden“ für eine private VM":::

1. Wählen Sie **RDP-Datei herunterladen** aus, um die Remotedesktopdatei auf Ihren Computer herunterzuladen.

    :::image type="content" source="./media/tutorial-restrict-network-access-to-resources/download-rdp-file.png" alt-text="Screenshot der Option „RDP-Datei herunterladen“ für eine private VM":::
  
1. Öffnen Sie die heruntergeladene RDP-Datei. Wenn Sie dazu aufgefordert werden, wählen Sie **Connect** aus. 

    :::image type="content" source="./media/tutorial-restrict-network-access-to-resources/rdp-connect.png" alt-text="Screenshot des Bildschirms „Verbindung“ für eine private VM":::

1. Geben Sie den Benutzernamen und das Kennwort ein, die Sie beim Erstellen des virtuellen Computers festgelegt haben. Unter Umständen müssen Sie auf **Weitere Optionen** und anschließend auf **Anderes Konto verwenden** klicken, um die Anmeldeinformationen anzugeben, die Sie beim Erstellen des virtuellen Computers eingegeben haben. Geben Sie in das E-Mail-Feld die Anmeldeinformationen für „Administratorkonto: Benutzername“ ein, die Sie zuvor angegeben haben. Wählen Sie **OK** aus, um sich auf der VM anzumelden.

    :::image type="content" source="./media/tutorial-restrict-network-access-to-resources/credential-screen.png" alt-text="Screenshot des Anmeldeinformationsbildschirms für eine private VM":::

    > [!NOTE] 
    > Während des Anmeldevorgangs wird unter Umständen eine Zertifikatwarnung angezeigt. Wenn eine Warnung angezeigt wird, klicken Sie auf **Ja** bzw. **Weiter**, um mit dem Herstellen der Verbindung fortzufahren.

1. Öffnen Sie nach der Anmeldung Windows PowerShell. Ordnen Sie mithilfe des folgenden Skripts die Azure-Dateifreigabe dem Laufwerk „Z“ zu, indem Sie PowerShell verwenden. Ersetzen Sie `<storage-account-key>` und die beiden Variablen `<storage-account-name>` durch die zuvor in [Erstellen eines Speicherkontos](#create-a-storage-account) angegebenen und abgerufenen Werte.

   ```powershell
   $acctKey = ConvertTo-SecureString -String "<storage-account-key>" -AsPlainText -Force
   $credential = New-Object System.Management.Automation.PSCredential -ArgumentList "Azure\<storage-account-name>", $acctKey
   New-PSDrive -Name Z -PSProvider FileSystem -Root "\\<storage-account-name>.file.core.windows.net\my-file-share" -Credential $credential
   ```

   PowerShell gibt eine Ausgabe ähnlich der folgenden Beispielausgabe zurück:

   ```powershell
   Name        Used (GB)     Free (GB) Provider      Root
   ----        ---------     --------- --------      ----
   Z                                      FileSystem    \\mystorage007.file.core.windows.net\my-f...
   ```

   Die Azure-Dateifreigabe wurde dem Laufwerk Z erfolgreich zugeordnet.

1.   Schließen Sie die Remotedesktopsitzung mit der VM *myVmPrivate*.

## <a name="confirm-access-is-denied-to-storage-account"></a>Bestätigen, dass der Zugriff auf das Speicherkonto verweigert wird

### <a name="from-myvmpublic"></a>Auf myVmPublic:

1. Geben Sie oben im Portal im Feld *Ressourcen, Dienste und Dokumente durchsuchen* **myVmPublic** ein. Wenn **myVmPublic** in den Suchergebnissen angezeigt wird, wählen Sie diese Angabe aus.

1. Wiederholen Sie in [Bestätigen des Zugriffs auf das Speicherkonto](#confirm-access-to-storage-account) für die VM *myVmPublic* die obigen Schritte 1–5.

   Nach einer kurzen Wartezeit tritt ein Fehler vom Typ `New-PSDrive : Access is denied` auf. Der Zugriff wird verweigert, da der virtuelle Computer *myVmPublic* im Subnetz *Public* bereitgestellt wird. Das Subnetz *Public* verfügt nicht über einen Dienstendpunkt, der für Azure Storage aktiviert ist. Das Speicherkonto lässt Netzwerkzugriff nur über das Subnetz *Private*, nicht jedoch über das Subnetz *Public* zu.

    ```powershell
    New-PSDrive : Access is denied
    At line:1 char:1
    + New-PSDrive -Name Z -PSProvider FileSystem -Root "\\mystorage007.file ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : InvalidOperation: (Z:PSDriveInfo) [New-PSDrive],     Win32Exception
        + Fu llyQualifiedErrorId : CouldNotMapNetworkDrive,Microsoft.PowerShell.Commands.NewPSDriveCommand

    ```

4. Schließen Sie die Remotedesktopsitzung für den virtuellen Computer *myVmPublic*.

### <a name="from-a-local-machine"></a>Auf einem lokalen Computer:

1. Wechseln Sie im Azure-Portal zu dem eindeutig benannten Speicherkonto, das Sie zuvor erstellt haben, beispielsweise *mystorage007*.

1. Wählen Sie unter *Datenspeicher* die Option **Dateifreigaben** und dann die zuvor erstellte *my-file-share* aus.

1. Es sollte die folgende Fehlermeldung angezeigt werden:

    :::image type="content" source="./media/tutorial-restrict-network-access-to-resources/access-denied-error.png" alt-text="Screenshot der Fehlermeldung „Zugriff verweigert“":::

>[!NOTE] 
> Der Zugriff wird verweigert, da sich Ihr Computer nicht im Subnetz *Private* des virtuellen Netzwerks *MyVirtualNetwork* befindet.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcengruppe mit allen ihren Ressourcen, wenn Sie sie nicht mehr benötigen:

1. Geben Sie im oben im Portal im Feld *Suche* die Zeichenfolge **myResourceGroup** ein. Wenn **myResourceGroup** in den Suchergebnissen angezeigt wird, wählen Sie diese Angabe aus.

1. Wählen Sie die Option **Ressourcengruppe löschen**.

1. Geben Sie für **Geben Sie den Ressourcengruppennamen ein:** den Namen *myResourceGroup* ein, und klicken Sie auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie einen Dienstendpunkt für ein Subnetz eines virtuellen Netzwerks aktiviert. Sie haben erfahren, dass Dienstendpunkte für Ressourcen aktiviert werden können, die über mehrere Azure-Dienste bereitgestellt werden. Sie haben ein Azure Storage-Konto erstellt und den Netzwerkzugriff auf das Speicherkonto ausschließlich auf Ressourcen im Subnetz eines virtuellen Netzwerks eingeschränkt. Weitere Informationen zu Dienstendpunkten finden Sie unter [Dienstendpunkte im virtuellen Netzwerk](virtual-network-service-endpoints-overview.md) und [Hinzufügen, Ändern oder Löschen von Subnetzen virtueller Netzwerke](virtual-network-manage-subnet.md).

Wenn Sie über mehrere virtuelle Netzwerke in Ihrem Konto verfügen, sollten Sie eine Verbindung zwischen diesen in Erwägung ziehen, damit die Ressourcen untereinander kommunizieren können. Im nächsten Tutorial erfahren Sie, wie Sie Verbindungen zwischen virtuellen Netzwerken herstellen.

> [!div class="nextstepaction"]
> [Herstellen von Verbindungen zwischen virtuellen Netzwerken](./tutorial-connect-virtual-networks-portal.md)
