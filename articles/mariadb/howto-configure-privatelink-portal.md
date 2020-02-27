---
title: Einrichten von Private Link für Azure Database for MariaDB (Vorschau) über das Portal
description: Erfahren Sie, wie Sie Private Link für Azure Database for MariaDB über das Azure-Portal konfigurieren.
author: kummanish
ms.author: manishku
ms.service: mariadb
ms.topic: conceptual
ms.date: 01/09/2020
ms.openlocfilehash: 67b045ff0661e8d0f8e20656a012e85d01e83d7b
ms.sourcegitcommit: b8f2fee3b93436c44f021dff7abe28921da72a6d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/18/2020
ms.locfileid: "77425917"
---
# <a name="create-and-manage-private-link-for-azure-database-for-mariadb-preview-using-portal"></a>Erstellen und Verwalten von Private Link für Azure Database for MariaDB (Vorschau) über das Portal

Ein privater Endpunkt ist der grundlegende Baustein für Private Link in Azure. Mit ihm können Azure-Ressourcen wie virtuelle Computer (VMs) privat mit Private Link-Ressourcen kommunizieren.  In diesem Artikel erfahren Sie, wie Sie einen virtuellen Computer in einem virtuellen Azure-Netzwerk und einen Azure Database for MariaDB-Server mit einem privaten Azure-Endpunkt im Azure-Portal erstellen.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

> [!NOTE]
> Dieses Feature steht in allen Azure-Regionen zur Verfügung, in denen Azure Database for MariaDB die Tarife „Universell“ und „Arbeitsspeicheroptimiert“ unterstützt.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure
Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

## <a name="create-an-azure-vm"></a>Erstellen einer Azure-VM

In diesem Abschnitt erstellen Sie ein virtuelles Netzwerk und das Subnetz zum Hosten des virtuellen Computers, der für den Zugriff auf Ihre Private Link-Ressource verwendet wird (ein MariaDB-Server in Azure).

### <a name="create-the-virtual-network"></a>Erstellen des virtuellen Netzwerks
In diesem Abschnitt erstellen Sie ein virtuelles Netzwerk und das Subnetz zum Hosten des virtuellen Computers, der für den Zugriff auf Ihre Private Link-Ressource verwendet wird.

1. Wählen Sie oben links auf dem Bildschirm **Ressource erstellen** > **Netzwerk** > **Virtuelles Netzwerk** aus.
2. Geben Sie in **Virtuelles Netzwerk erstellen** diese Informationen ein, oder wählen Sie sie aus:

    | Einstellung | value |
    | ------- | ----- |
    | Name | Geben Sie *MyVirtualNetwork* ein. |
    | Adressraum | Geben Sie *10.1.0.0/16* ein. |
    | Subscription | Wählen Sie Ihr Abonnement aus.|
    | Resource group | Wählen Sie **Neue erstellen** aus, geben Sie *myResourceGroup* ein, und wählen Sie **OK** aus. |
    | Location | Wählen Sie **Europa, Westen** aus.|
    | Subnetzname | Geben Sie *mySubnet* ein. |
    | Subnetzadressbereich | Geben Sie *10.1.0.0/24* ein. |
    |||
3. Übernehmen Sie die übrigen Standardeinstellungen, und wählen Sie **Erstellen** aus.

### <a name="create-virtual-machine"></a>Erstellen eines virtuellen Computers

1. Wählen Sie oben links auf dem Bildschirm im Azure-Portal die Option **Ressource erstellen** > **Compute** > **Virtueller Computer** aus.

2. Geben Sie in **Virtuellen Computer erstellen – Grundlagen** diese Informationen ein, oder wählen Sie sie aus:

    | Einstellung | value |
    | ------- | ----- |
    | **PROJEKTDETAILS** | |
    | Subscription | Wählen Sie Ihr Abonnement aus. |
    | Resource group | Wählen Sie **myResourceGroup** aus. Diese haben Sie im vorherigen Abschnitt erstellt.  |
    | **INSTANZDETAILS** |  |
    | Name des virtuellen Computers | Geben Sie *myVm* ein. |
    | Region | Wählen Sie **Europa, Westen** aus. |
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
    |||


1. Klicken Sie auf **Überprüfen + erstellen**. Sie werden zur Seite **Überprüfen und erstellen** weitergeleitet, auf der Azure Ihre Konfiguration überprüft.

1. Wenn die Meldung **Überprüfung erfolgreich** angezeigt wird, wählen Sie **Erstellen** aus.

## <a name="create-an-azure-database-for-mariadb"></a>Erstellen einer Azure Database for MariaDB-Instanz

In diesem Abschnitt erstellen Sie einen Azure Database for MariaDB-Server in Azure. 

1. Wählen Sie links oben auf dem Bildschirm im Azure-Portal die Option **Ressource erstellen** > **Datenbanken** > **Azure Database for MariaDB** aus.

1. Geben Sie in **Azure Database for MariaDB** die folgenden Informationen an:

    | Einstellung | value |
    | ------- | ----- |
    | **Projektdetails** | |
    | Subscription | Wählen Sie Ihr Abonnement aus. |
    | Resource group | Wählen Sie **myResourceGroup** aus. Diese haben Sie im vorherigen Abschnitt erstellt.|
    | **Serverdetails** |  |
    |Servername  | Geben Sie *myserver* ein. Wenn dieser Name vergeben ist, erstellen Sie einen eindeutigen Namen.|
    | Administratorbenutzername| Geben Sie einen Administratornamen Ihrer Wahl ein. |
    | Kennwort | Geben Sie das gewünschte Kennwort ein. Das Kennwort muss mindestens acht Zeichen lang sein und die festgelegten Anforderungen erfüllen. |
    | Location | Wählen Sie eine Azure-Region aus, in der sich Ihr MariaDB-Server befinden soll. |
    |Version  | Wählen Sie die gewünschte Datenbankversion des MariaDB-Servers aus.|
    | Compute und Speicher| Wählen Sie den für den Server erforderlichen Tarif basierend auf der Workload aus. |
    |||
 
7. Klicken Sie auf **OK**. 
8. Klicken Sie auf **Überprüfen + erstellen**. Sie werden zur Seite **Überprüfen und erstellen** weitergeleitet, auf der Azure Ihre Konfiguration überprüft. 
9. Wenn die Meldung „Überprüfung erfolgreich“ angezeigt wird, wählen Sie **Erstellen** aus. 
10. Wenn die Meldung „Überprüfung erfolgreich“ angezeigt wird, wählen Sie „Erstellen“ aus. 

## <a name="create-a-private-endpoint"></a>Erstellen eines privaten Endpunkts

In diesem Abschnitt erstellen Sie einen privaten Endpunkt für den MariaDB-Server. 

1. Wählen Sie oben links auf dem Bildschirm im Azure-Portal die Option **Ressource erstellen** > **Netzwerk** > **Private Link-Center (Vorschau)** aus.
2. Wählen Sie unter **Privat Link-Center – Übersicht** bei der Option **Build a private connection to a service** (Private Verbindung mit einem Dienst herstellen) **Start** aus.

    ![Übersicht über Private Link](media/concepts-data-access-and-security-private-link/privatelink-overview.png)

1. Geben Sie unter **Privaten Endpunkt erstellen (Vorschau) – Grundlagen** diese Informationen ein, oder wählen Sie sie aus:

    | Einstellung | value |
    | ------- | ----- |
    | **Projektdetails** | |
    | Subscription | Wählen Sie Ihr Abonnement aus. |
    | Resource group | Wählen Sie **myResourceGroup** aus. Diese haben Sie im vorherigen Abschnitt erstellt.|
    | **Instanzendetails** |  |
    | Name | Geben Sie *myPrivateEndpoint* ein. Wenn dieser Name vergeben ist, erstellen Sie einen eindeutigen Namen. |
    |Region|Wählen Sie **Europa, Westen** aus.|
    |||
5. Klicken Sie auf **Weiter: Ressource** aus.
6. Geben Sie unter **Privaten Endpunkt erstellen – Ressource** diese Informationen ein, oder wählen Sie sie aus:

    | Einstellung | value |
    | ------- | ----- |
    |Verbindungsmethode  | Wählen Sie das Herstellen einer Verbindung mit einer Azure-Ressource im eigenen Verzeichnis aus.|
    | Subscription| Wählen Sie Ihr Abonnement aus. |
    | Ressourcentyp | Wählen Sie **Microsoft.DBforMariaDB/servers** aus. |
    | Resource |Wählen Sie *myServer* aus.|
    |Zielunterressource |Wählen Sie *mariadbServer* aus.|
    |||
7. Klicken Sie auf **Weiter: Konfiguration** aus.
8. Geben Sie unter **Privaten Endpunkt erstellen (Vorschau) – Konfiguration** diese Informationen ein, oder wählen Sie sie aus:

    | Einstellung | value |
    | ------- | ----- |
    |**NETZWERK**| |
    | Virtuelles Netzwerk| Wählen Sie *MyVirtualNetwork* aus. |
    | Subnet | Wählen Sie *mySubnet* aus. |
    |**PRIVATE DNS-INTEGRATION**||
    |Integration in eine private DNS-Zone |Wählen Sie **Ja** aus. |
    |Private DNS-Zone |Wählen Sie *(New)privatelink.mariadb.database.azure.com* aus. |
    |||

1. Klicken Sie auf **Überprüfen + erstellen**. Sie werden zur Seite **Überprüfen und erstellen** weitergeleitet, auf der Azure Ihre Konfiguration überprüft. 
2. Wenn die Meldung **Überprüfung erfolgreich** angezeigt wird, wählen Sie **Erstellen** aus. 

    ![Erstellter Private Link](media/concepts-data-access-and-security-private-link/show-mariadb-private-link.png)

## <a name="connect-to-a-vm-using-remote-desktop-rdp"></a>Herstellen einer Verbindung mit einem virtuellen Computer mithilfe von Remotedesktop (RDP)


Stellen Sie nach der Erstellung von **myVm** über das Internet eine Verbindung mit diesem virtuellen Computer her: 

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

## <a name="access-the-mariadb-server-privately-from-the-vm"></a>Privates Zugreifen auf den MariaDB-Server über den virtuellen Computer

1. Öffnen Sie PowerShell auf dem Remotedesktop von  *myVM*.

2. Geben Sie `nslookup mydemomserver.privatelink.mariadb.database.azure.com` ein. 

    Sie erhalten eine Meldung wie die folgende:
    ```azurepowershell
    Server:  UnKnown
    Address:  168.63.129.16
    Non-authoritative answer:
    Name:    mydemoMariaDBserver.privatelink.mariadb.database.azure.com
    Address:  10.1.3.4
    ```

3. Testen Sie die Verbindung über den privaten Link für den MariaDB-Server mit einem beliebigen verfügbaren Client. Im folgenden Beispiel wird für diesen Vorgang [MySQL Workbench](https://dev.mysql.com/doc/workbench/en/wb-installing-windows.html) verwendet.


4. Geben Sie in **Neue Verbindung** diese Informationen ein, oder wählen Sie sie aus:

    | Einstellung | value |
    | ------- | ----- |
    | Servertyp| Wählen Sie **MariaDB** aus.|
    | Servername| Wählen Sie *mydemoserver.privateLink.mariadb.database.azure.com* aus. |
    | Benutzername | Geben Sie einen Benutzernamen als username@servername ein, der während der Erstellung des MariaDB-Servers angegeben wird. |
    |Kennwort |Geben Sie ein während der Erstellung des MariaDB-Servers angegebenes Kennwort ein. |
    |SSL|Wählen Sie **Erforderlich** aus.|
    ||

5. Wählen Sie **Verbindung testen** oder **OK** aus.

6. (Optional) Durchsuchen von Datenbanken im linken Menü und Erstellen von Informationen in der MariaDB-Datenbank oder Abfragen von Informationen darin

7. Schließen Sie die Remotedesktopverbindung mit myVm.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Wenn Sie Ihre Arbeit mit dem privaten Endpunkt, dem MariaDB-Server und dem virtuellen Computer abgeschlossen haben, löschen Sie die Ressourcengruppe und alle darin enthaltenen Ressourcen:

1. Geben Sie oben im Portal die Zeichenfolge  *myResourceGroup* im Feld **Suchen** ein, und wählen Sie in den Suchergebnissen *myResourceGroup* aus.
2. Wählen Sie die Option **Ressourcengruppe löschen**.
3. Geben Sie „myResourceGroup“ für **RESSOURCENGRUPPENNAMEN EINGEBEN** ein, und wählen Sie **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

In dieser Anleitung haben Sie einen virtuellen Computer in einem virtuellen Netzwerk, eine Azure Database for MariaDB und einen privaten Endpunkt für den privaten Zugriff erstellt. Sie haben aus dem Internet eine Verbindung mit einem virtuellen Computer hergestellt und über Private Link sicher mit dem MariaDB-Server kommuniziert. Weitere Informationen zu privaten Endpunkten finden Sie unter [Was ist privater Endpunkt in Azure?](https://docs.microsoft.com/azure/private-link/private-endpoint-overview).
