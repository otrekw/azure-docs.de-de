---
title: Herstellen einer privaten Verbindung mit einem Speicherkonto mithilfe eines privaten Azure-Endpunkts
description: Erfahren Sie, wie Sie eine private Verbindung mit einem Speicherkonto in Azure mithilfe eines privaten Endpunkts herstellen.
services: private-link
author: malopMSFT
ms.service: private-link
ms.topic: article
ms.date: 09/16/2019
ms.author: allensu
ms.openlocfilehash: 8c76333d5a2be8a2c589dbe54389b023fef34854
ms.sourcegitcommit: e4c33439642cf05682af7f28db1dbdb5cf273cc6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2020
ms.locfileid: "78252526"
---
# <a name="connect-privately-to-a-storage-account-using-azure-private-endpoint"></a>Herstellen einer privaten Verbindung mit einem Speicherkonto mithilfe eines privaten Azure-Endpunkts
Der private Azure-Endpunkt ist der grundlegende Baustein für Private Link in Azure. Mit ihm können Azure-Ressourcen wie virtuelle Computer (VMs) privat mit Private Link-Ressourcen kommunizieren.

In diesem Schnellstart erfahren Sie, wie Sie einen virtuellen Computer in einem virtuellen Azure-Netzwerk und ein Speicherkonto mit einem privaten Endpunkt im Azure-Portal erstellen. Anschließend können Sie vom virtuellen Computer sicher auf das Speicherkonto zugreifen.

> [!NOTE]
> Private Endpunkte sind in Verbindung mit Dienstendpunkten im selben Subnetz unzulässig!

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter https://portal.azure.com beim Azure-Portal an.

## <a name="create-a-vm"></a>Erstellen einer VM
In diesem Abschnitt erstellen Sie ein virtuelles Netzwerk und das Subnetz zum Hosten des virtuellen Computers, der für den Zugriff auf Ihre Private Link-Ressource verwendet wird (in diesem Beispiel ein Speicherkonto).

## <a name="virtual-network-and-parameters"></a>Virtuelles Netzwerk und Parameter

In diesem Abschnitt erstellen Sie ein virtuelles Netzwerk und das Subnetz zum Hosten des virtuellen Computers, der für den Zugriff auf Ihre Private Link-Ressource verwendet wird.

In diesem Abschnitt müssen Sie die folgenden Parameter in den Schritten unten wie folgt ersetzen:

| Parameter                   | value                |
|-----------------------------|----------------------|
| **\<resource-group-name>**  | myResourceGroup |
| **\<virtual-network-name>** | myVirtualNetwork          |
| **\<region-name>**          | USA, Westen-Mitte      |
| **\<IPv4-address-space>**   | 10.1.0.0\16          |
| **\<subnet-name>**          | mySubnet        |
| **\<subnet-address-range>** | 10.1.0.0\24          |

[!INCLUDE [virtual-networks-create-new](../../includes/virtual-networks-create-new.md)]


### <a name="create-virtual-machine"></a>Erstellen eines virtuellen Computers

1. Wählen Sie oben links auf dem Bildschirm im Azure-Portal die Option **Ressource erstellen** > **Compute** > **Virtueller Computer** aus.

1. Geben Sie in **Virtuellen Computer erstellen – Grundlagen** diese Informationen ein, oder wählen Sie sie aus:

    | Einstellung | value |
    | ------- | ----- |
    | **PROJEKTDETAILS** | |
    | Subscription | Wählen Sie Ihr Abonnement aus. |
    | Resource group | Wählen Sie **myResourceGroup** aus. Diese haben Sie im vorherigen Abschnitt erstellt.  |
    | **INSTANZDETAILS** |  |
    | Name des virtuellen Computers | Geben Sie *myVm* ein. |
    | Region | Wählen Sie **WestCentralUS** aus. |
    | Verfügbarkeitsoptionen | Übernehmen Sie den Standardwert **Keine Infrastrukturredundanz erforderlich**. |
    | Image | Wählen Sie **Windows Server 2019 Datacenter** aus. |
    | Size | Übernehmen Sie den Standardwert **Standard DS1 v2**. |
    | **ADMINISTRATORKONTO** |  |
    | Username | Geben Sie einen Benutzernamen Ihrer Wahl ein. |
    | Kennwort | Geben Sie das gewünschte Kennwort ein. Das Kennwort muss mindestens zwölf Zeichen lang sein und die [definierten Anforderungen an die Komplexität](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) erfüllen.|
    | Kennwort bestätigen | Geben Sie das Kennwort erneut ein. |
    | **REGELN FÜR EINGEHENDE PORTS** |  |
    | Öffentliche Eingangsports | Übernehmen Sie den Standardwert **Keine**. |
    | **SPAREN SIE GELD** |  |
    | Windows-Lizenz bereits vorhanden? | Übernehmen Sie den Standardwert **Nein**. |
    |||

1. Klicken Sie auf **Weiter: Datenträger**.

1. Übernehmen Sie unter **Virtuellen Computer erstellen – Datenträger** die Standardwerte, und wählen Sie **Weiter: Netzwerk**.

1. Wählen Sie in **Virtuellen Computer erstellen – Netzwerk** diese Informationen aus:

    | Einstellung | value |
    | ------- | ----- |
    | Virtuelles Netzwerk | Übernehmen Sie den Standardwert **MyVirtualNetwork**.  |
    | Adressraum | Übernehmen Sie den Standardwert **10.1.0.0/24**.|
    | Subnet | Übernehmen Sie den Standardwert **mySubnet (10.1.0.0/24)** .|
    | Öffentliche IP-Adresse | Übernehmen Sie den Standardwert **(neu) myVm-ip**. |
    | Öffentliche Eingangsports | Wählen Sie **Ausgewählte Ports zulassen** aus. |
    | Eingangsports auswählen | Wählen Sie **HTTP** und **RDP** aus.|
    ||

1. Klicken Sie auf **Überprüfen + erstellen**. Sie werden zur Seite **Überprüfen und erstellen** weitergeleitet, auf der Azure Ihre Konfiguration überprüft.

1. Wenn die Meldung **Überprüfung erfolgreich** angezeigt wird, wählen Sie **Erstellen** aus.

## <a name="create-your-private-endpoint"></a>Erstellen Ihres privaten Endpunkts
In diesem Abschnitt erstellen Sie ein privates Speicherkonto und verwenden für das Speicherkonto einen privaten Endpunkt. 

1. Wählen Sie oben links auf dem Bildschirm im Azure-Portal die Option **Ressource erstellen** > **Speicher** > **Speicherkonto** aus.

1. Geben Sie in **Speicherkonto erstellen – Grundlagen** diese Informationen ein, oder wählen Sie sie aus:

    | Einstellung | value |
    | ------- | ----- |
    | **PROJEKTDETAILS** | |
    | Subscription | Wählen Sie Ihr Abonnement aus. |
    | Resource group | Wählen Sie **myResourceGroup** aus. Diese haben Sie im vorherigen Abschnitt erstellt.|
    | **INSTANZDETAILS** |  |
    | Speicherkontoname  | Geben Sie *mystorageaccount* ein. Wenn dieser Name vergeben ist, erstellen Sie einen eindeutigen Namen. |
    | Region | Wählen Sie **WestCentralUS** aus. |
    | Leistung| Übernehmen Sie den Standardwert **Standard**. |
    | Kontoart | Übernehmen Sie den Standardwert **Storage (universell, Version 2)** . |
    | Replikation | Wählen Sie **Georedundanter Speicher mit Lesezugriff (RA-GRS)** aus. |
    |||
  
3. Klicken Sie auf **Weiter: Netzwerk** aus.
4. Wählen Sie in **Speicherkonto erstellen – Netzwerke** als Verbindungsmethode die Option **Privater Endpunkt** aus.
5. Wählen Sie in **Speicherkonto erstellen – Netzwerke** die Option **Privaten Endpunkt hinzufügen** aus. 
6. Geben Sie unter **Privaten Endpunkt erstellen** diese Informationen ein, oder wählen Sie sie aus:

    | Einstellung | value |
    | ------- | ----- |
    | **PROJEKTDETAILS** | |
    | Subscription | Wählen Sie Ihr Abonnement aus. |
    | Resource group | Wählen Sie **myResourceGroup** aus. Diese haben Sie im vorherigen Abschnitt erstellt.|
    |Location|Wählen Sie **WestCentralUS** aus.|
    |Name|Geben Sie *myPrivateEndpoint* ein.  |
    |Speicherunterressource|Übernehmen Sie den Standardwert **Blob**. |
    | **NETZWERK** |  |
    | Virtuelles Netzwerk  | Wählen Sie *MyVirtualNetwork* in der Ressourcengruppe *myResourceGroup* aus. |
    | Subnet | Wählen Sie *mySubnet* aus. |
    | **PRIVATE DNS-INTEGRATION**|  |
    | Integration in eine private DNS-Zone  | Übernehmen Sie den Standardwert **Ja**. |
    | Private DNS-Zone  | Übernehmen Sie den Standardwert **(Neu) privatelink.blob.core.windows.net**. |
    |||
7. Klicken Sie auf **OK**. 
8. Klicken Sie auf **Überprüfen + erstellen**. Sie werden zur Seite **Überprüfen und erstellen** weitergeleitet, auf der Azure Ihre Konfiguration überprüft. 
9. Wenn die Meldung **Überprüfung erfolgreich** angezeigt wird, wählen Sie **Erstellen** aus. 
10. Navigieren Sie zur gerade erstellten Speicherkontoressource.
11. Wählen Sie im Inhaltsmenü auf der linken Seite **Zugriffsschlüssel** aus.
12. Wählen Sie **Kopieren** für die Verbindungszeichenfolge für „key1“ aus.
 
## <a name="connect-to-a-vm-from-the-internet"></a>Herstellen einer Verbindung mit einem virtuellen Computer über das Internet

Stellen Sie wie folgt eine Verbindung mit dem virtuellen Computer *myVm* aus dem Internet her:

1. Geben Sie in der Suchleiste des Portals *myVm* ein.

1. Wählen Sie die Schaltfläche **Verbinden** aus. Nach dem Auswählen der Schaltfläche **Verbinden** wird **Verbindung mit virtuellem Computer herstellen** geöffnet.

1. Wählen Sie **RDP-Datei herunterladen** aus. Azure erstellt eine Remotedesktopprotokoll-Datei (*RDP*) und lädt sie auf Ihren Computer herunter.

1. Öffnen Sie die Datei *downloaded.rdp*.

    1. Wenn Sie dazu aufgefordert werden, wählen Sie **Verbinden** aus.

    1. Geben Sie den Benutzernamen und das Kennwort ein, den/das Sie beim Erstellen des virtuellen Computers angegeben haben.

        > [!NOTE]
        > Unter Umständen müssen Sie **Weitere Optionen** > **Anderes Konto verwenden** auswählen, um die Anmeldeinformationen anzugeben, die Sie beim Erstellen des virtuellen Computers eingegeben haben.

1. Klicken Sie auf **OK**.

1. Während des Anmeldevorgangs wird unter Umständen eine Zertifikatwarnung angezeigt. Wenn Sie eine Zertifikatwarnung erhalten, wählen Sie **Ja** oder **Weiter** aus.

1. Sobald der VM-Desktop angezeigt wird, minimieren Sie ihn, um zu Ihrem lokalen Desktop zurückzukehren.  

## <a name="access-storage-account-privately-from-the-vm"></a>Privates Zugreifen auf das Speicherkonto von der VM

In diesem Abschnitt stellen Sie unter Verwendung des privaten Endpunkts eine private Verbindung mit dem Speicherkonto her.

1. Öffnen Sie auf dem Remotedesktop von *myVM* PowerShell.
2. Geben Sie`nslookup mystorageaccount.blob.core.windows.net` ein. Sie erhalten eine Meldung wie die folgende:
    ```azurepowershell
    Server:  UnKnown
    Address:  168.63.129.16
    Non-authoritative answer:
    Name:    mystorageaccount123123.privatelink.blob.core.windows.net
    Address:  10.0.0.5
    Aliases:  mystorageaccount.blob.core.windows.net
    ```
3. Installieren Sie den [Microsoft Azure Storage-Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json&tabs=windows).
4. Klicken Sie mit der rechten Maustaste auf **Speicherkonten**.
5. Wählen Sie **Connect to an azure storage** (Mit Azure-Speicher verbinden) aus.
6. Wählen Sie **Verbindungszeichenfolge verwenden** aus.
7. Wählen Sie **Weiter** aus.
8. Geben Sie die Verbindungszeichenfolge ein, indem Sie die zuvor kopierten Informationen einfügen.
9. Wählen Sie **Weiter** aus.
10. Wählen Sie **Verbinden**.
11. Durchsuchen Sie die Blobcontainer aus „mystorageaccount“. 
12. (Optional) Erstellen Sie Ordner, und/oder laden Sie Dateien in *mystorageaccount* hoch. 
13. Schließen Sie die Remotedesktopverbindung mit *myVM*. 

Weitere Optionen zum Zugreifen auf das Speicherkonto:
- Bei Microsoft Azure Storage-Explorer handelt es sich um eine kostenlose eigenständige App von Microsoft, über die Sie ganz einfach visuell mit Azure-Speicherdaten arbeiten können – unter Windows, MacOS und Linux. Sie können die Anwendung installieren, um den Inhalt des Speicherkontos privat zu durchsuchen. 
 
- Das Dienstprogramm „AzCopy“ ist eine weitere Option für eine leistungsstarke, skriptfähige Datenübertragung für Azure-Speicher. Verwenden Sie AzCopy, um Daten in und aus Blob-, Datei- und Tabellenspeicher zu übertragen. 


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen 
Wenn Sie Ihre Arbeit mit dem privaten Endpunkt, dem Speicherkonto und dem virtuellen Computer abgeschlossen haben, löschen Sie die Ressourcengruppe und alle darin enthaltenen Ressourcen: 
1. Geben Sie oben im Portal im Feld **Suchen** die Zeichenfolge *myResourceGroup* ein, und wählen Sie in den Suchergebnissen den Eintrag *myResourceGroup* aus. 
2. Wählen Sie die Option **Ressourcengruppe löschen**. 
3. Geben Sie *myResourceGroup* für **RESSOURCENGRUPPENNAMEN EINGEBEN** ein, und wählen Sie **Löschen** aus. 

## <a name="next-steps"></a>Nächste Schritte
In diesem Schnellstart haben Sie einen virtuellen Computer in einem virtuellen Netzwerk, ein Speicherkonto und einen privaten Endpunkt erstellt. Sie haben über das Internet eine Verbindung mit einem virtuellen Computer hergestellt und über Private Link sicher mit dem Speicherkonto kommuniziert. Weitere Informationen zu privaten Endpunkten finden Sie unter [Was ist ein privater Endpunkt in Azure?](private-endpoint-overview.md).
