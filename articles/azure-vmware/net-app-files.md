---
title: NetApp Files für Azure VMware Solution
description: Verwenden Sie Azure NetApp Files mit Azure VMware-VMs, um Daten zwischen lokalen Servern, Azure VMware Solution-VMs und Cloudinfrastrukturen zu migrieren und zu synchronisieren.
ms.topic: how-to
ms.date: 09/17/2020
ms.openlocfilehash: 0adf137db358d3561365a3a1ed2def338eeeb158
ms.sourcegitcommit: 7374b41bb1469f2e3ef119ffaf735f03f5fad484
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/16/2020
ms.locfileid: "90708416"
---
# <a name="netapp-files-for-azure-vmware-solution"></a>NetApp Files für Azure VMware Solution

In diesem Artikel werden die Schritte erläutert, mit denen Azure NetApp Files in Azure VMware Solution-basierte Workloads eingebunden wird. [Azure NetApp Files](../azure-netapp-files/azure-netapp-files-introduction.md) ist ein Azure-Dienst zum Migrieren und Ausführen von Unternehmensdateiworkloads in der Cloud, ohne dass Codeänderungen erforderlich sind. Der Dienst ermöglicht einfache Migration von Daten zwischen lokalen und cloudbasierten Infrastrukturen. Eine schnelle und sicherheitsoptimierte Datensynchronisierung vereinfacht Migrations- und DevOps-Szenarien mit Funktionalitäten wie sofortige Momentaufnahme, Wiederherstellung und Active Directory-Integration.

## <a name="topology"></a>Topologie

In diesem Szenario wird, um die Azure NetApp Files-Poolfreigabe zu testen und zu überprüfen, Azure NetApp Files mit Kapazitätspool, Volumepool und Subnetz konfiguriert. Es wurde das NFS-Protokoll verwendet. Sowohl Azure NetApp Files als auch Azure VMware Solution werden in derselben Azure-Region (USA, Osten) erstellt. Die Verbindung mit Azure VMware Solution erfolgt über Azure ExpressRoute.

![NetApp Files für Azure VMware Solution-Topologie](media/net-app-files/net-app-files-topology.png)

## <a name="prerequisites"></a>Voraussetzungen 

> [!div class="checklist"]
> * Azure-Abonnement mit Azure NetApp Files
> * Subnetz für Azure NetApp Files
> * Linux-VM auf Azure VMware Solution
> * Windows-VM auf Azure VMware Solution

## <a name="verify-configuration-of-azure-netapp-files"></a>Überprüfen der Konfiguration der Azure NetApp Files-Umgebung 

Überprüfen Sie zunächst die Konfiguration der Azure NetApp Files-Umgebung, die in Azure auf einem Premium-Datenträger erstellt wurde.

1. Wählen Sie im Azure-Portal unter **Speicher** die Option **Azure NetApp Files** aus. Eine Liste der konfigurierten Azure NetApp Files-Konten wird angezeigt.

    :::image type="content" source="media/net-app-files/azure-net-app-files-list.png" alt-text="Liste der Azure NetApp Files-Konten"::: 

2. Wenn Sie ein NetApp-Konto auswählen, können Sie verschiedene Einstellungen anzeigen. Wenn Sie z. B. **Contoso-anf2** auswählen, werden die Einstellungen dieses Kontos angezeigt. 

    :::image type="content" source="media/net-app-files/net-app-settings.png" alt-text="Liste der Azure NetApp Files-Konten"::: 

3. Wählen Sie **Kapazitätspools** aus, um den konfigurierten Pool zu überprüfen. 

    :::image type="content" source="media/net-app-files/capacity-pools.png" alt-text="Liste der Azure NetApp Files-Konten":::

    Sie können sehen, dass der Speicherpool als 4 TiB mit Premium-Datenträger konfiguriert ist.

4. Wählen Sie **Volumes** aus (siehe Screenshot aus Schritt 2), um die im Kapazitätspool erstellten Volumes anzuzeigen.  

    :::image type="content" source="media/net-app-files/azure-net-app-volumes.png" alt-text="Liste der Azure NetApp Files-Konten":::

5. Wählen Sie ein Volume aus, um dessen Konfiguration anzuzeigen.  

    :::image type="content" source="media/net-app-files/configuration-of-volume.png" alt-text="Liste der Azure NetApp Files-Konten":::

    Sie können sehen, dass der Volumepool „anfpool“, mit 200 GiB, als NetApp Files-Freigabe erstellt wurde. Es wurde ein privates virtuelles IP-Netzwerk für die Azure NetApp Files-Umgebung erstellt, das den NFS-Pfad angibt, mit dem das Einbinden auf den Azure VMware Solution-VMs erfolgt. 

## <a name="protocols-supported-by-azure-netapp-files"></a>Protokolle, die von Azure NetApp Files unterstützt werden  

Azure NetApp Files unterstützt die Zuordnung von SMB- (Server Message Block) und NFS-Freigaben (Network File System). 

- **SMB-Freigabe**: Damit Sie ein SMB-Volume bereitstellen können, müssen Sie zunächst eine Active Directory-Verbindung erstellen. Damit erfolgreich eine Verbindung hergestellt werden kann, muss über das delegierte Subnetz von Azure NetApp Files Zugriff auf die angegebenen Domänencontroller möglich sein. Nachdem die Active Directory-Verbindung im Azure NetApp Files Konto konfiguriert wurde, wird sie als auswählbares Element angezeigt, wenn ein SMB-Volume erstellt wird. 

- **NFS-Freigabe**: Azure NetApp Files unterstützt das Erstellen von Volumes über NFS (NFSv3 und NFSv4.1), SMBv3 oder ein duales Protokoll (NFSv3 und SMB). Der Kapazitätsverbrauch eines Volumes wird mit der bereitgestellten Kapazität des dazugehörigen Pools verrechnet. NFS kann über eine Befehlszeile in einen Linux-Server eingebunden werden.

## <a name="create-azure-netapp-files"></a>Erstellen eines Azure NetApp Files-Volumes 

1. Melden Sie sich beim [Azure-Portal](https://rc.portal.azure.com/#home)an. Wählen Sie unter „Azure-Dienste“ die Option **Azure NetApp Files** aus. 

2. Wählen Sie **+ Hinzufügen** aus, um ein neues Azure NetApp Files-Volume zu erstellen. 

3. Füllen Sie die Pflichtfelder (Name, Abonnement, Ressourcengruppe und Speicherort) aus, und wählen Sie **Erstellen** aus. 

## <a name="add-capacity-pools-into-azure-netapp-files"></a>Hinzufügen von Kapazitätspools in Azure NetApp Files 

1. Wählen Sie im Azure-Portal die Option **Azure NetApp Files** aus, wählen Sie **Kapazitätspools** aus, und wählen Sie **+ Pool hinzufügen** aus. 

2. Geben Sie die erforderlichen Details für den Volumenpoolnamen, das Servicelevel und die Datenträgergröße (FQDN) oder die IP-Adresse und die Gewichtung ein. Wählen Sie **Hinzufügen**.

    :::image type="content" source="media/net-app-files/add-capacity-pool.png" alt-text="Liste der Azure NetApp Files-Konten":::

3. Um Volumes unter dem Kapazitätspool zu erstellen, wählen Sie **Volumes** im Suchbereich aus, und wählen Sie **+ Volume hinzufügen** aus. 
 
4. Füllen Sie die Pflichtfelder aus, wie im folgenden Screenshot gezeigt.

    :::image type="content" source="media/net-app-files/create-a-volume.png" alt-text="Liste der Azure NetApp Files-Konten":::

5. Wählen Sie auf der nächsten Seite die Protokolltypfreigabe aus. Wählen Sie die Versionen aus, wenn das Volume eine NFS-Freigabe ist, oder wählen Sie die Active Directory-Domäne aus, wenn das Volume eine SMB-Freigabe ist.  

6. Ist das Volume eine NFS-Freigabe, fügen Sie die Quell-IP-Adresse hinzu, von der aus auf die Protokolltypenfreigabe zugegriffen wird. Klicken Sie auf **Überprüfen + erstellen**. 

    :::image type="content" source="media/net-app-files/select-volume-details.png" alt-text="Liste der Azure NetApp Files-Konten":::
 
    Die NFS-Freigabe ist jetzt im Azure-Portal unter „Azure NetApp Files“ verfügbar.

    :::image type="content" source="media/net-app-files/share-ready.png" alt-text="Liste der Azure NetApp Files-Konten":::

## <a name="mount-an-nfs-file-share-on-your-azure-vmware-solution-vms"></a>Einbinden einer NFS-Dateifreigabe in Ihre Azure VMware Solution-VMs

### <a name="mount-an-nfs-file-share-on-an-azure-vmware-solution-windows-vm"></a>Einbinden einer NFS-Dateifreigabe in eine Azure VMware Solution-Windows-VM

- Öffnen Sie eine Eingabeaufforderung, und führen Sie den Befehl aus, mit dem die NFS-Dateifreigabe zugeordnet wird. Die folgenden Screenshots zeigen die Dateifreigabe, die einer Windows-VM in Azure VMware Solution zugeordnet ist.  

    :::image type="content" source="media/net-app-files/share-mapped-to-windows-vm.png" alt-text="Liste der Azure NetApp Files-Konten":::
 
    :::image type="content" source="media/net-app-files/mapped-to-windows-drive.png" alt-text="Liste der Azure NetApp Files-Konten":::

### <a name="mount-an-nfs-file-share-on-an-azure-vmware-solution-linux-vm"></a>Einbinden einer NFS-Dateifreigabe in eine Azure VMware Solution-Linux-VM

#### <a name="setting-up-your-azure-instance"></a>Einrichten Ihrer Azure-Instanz

1. Ordnen Sie im Azure-Portal Ihre Azure-Instanz einem Subnetz zu, das im selben virtuellen Netzwerk wie Ihr Volume definiert ist.

    > [!NOTE]
    > Für das Subnetz ist eine Netzwerksicherheitsgruppenregel erforderlich, die Datenverkehr an den NFS-Ports (2049, 111), UDP und TCP, zulässt.

2. Öffnen Sie einen SSH-Client, und stellen Sie eine Verbindung mit ihrer Azure-Instanz her. Weitere Informationen finden Sie unter [Verwenden von SSH-Schlüsseln mit Windows in Azure](../virtual-machines/linux/ssh-from-windows.md).

3. Installieren Sie den NFS-Client in Ihrer Azure-Instanz:
   - In Red Hat Enterprise Linux- oder SuSE Linux-Instanz: `sudo yum install -y nfs-utils`
   - In einer Ubuntu- oder Debian-Instanz: `sudo apt-get install nfs-common` 

#### <a name="mounting-your-file-system"></a>Einbinden Ihres Dateisystems

1. Erstellen Sie ein neues Verzeichnis in Ihrer Azure-Instanz: `sudo mkdir ANFPOOL`

2. Wählen Sie eine Ziel-IP-Adresse für das Einbinden aus.

3. Binden Sie Ihr Dateisystem ein: `sudo mount -t nfs -o rw,hard,rsize=1048576,wsize=1048576,vers=3,tcp 10.22.3.4:/ANFPOOL ANFPOOL`

    :::image type="content" source="media/net-app-files/root-test.png" alt-text="Liste der Azure NetApp Files-Konten":::

## <a name="create-an-active-directory-connection-for-an-smb-share"></a>Erstellen einer Active Directory-Verbindung für eine SMB-Freigabe

1. Wählen Sie im Azure-Portal ein NetApp-Konto aus.

2. Wählen Sie unter „Azure NetApp Files“ die Option **Active Directory-Verbindungen** aus.

    :::image type="content" source="media/net-app-files/active-directory-connections.png" alt-text="Liste der Azure NetApp Files-Konten"::: 

3. Wählen Sie **Verknüpfen** aus, um die SMB-Freigabe mit Active Directory zu verknüpfen. Der folgende Screenshot zeigt die Domänendetails der SMB-Freigabe.

    :::image type="content" source="media/net-app-files/active-directory-connect-details.png" alt-text="Liste der Azure NetApp Files-Konten"::: 

    Ihre Azure SMB-Dateifreigabe ist einsatzbereit.  

    :::image type="content" source="media/net-app-files/smb-file-share-details.png" alt-text="Liste der Azure NetApp Files-Konten"::: 

4. Ordnen Sie die SMB-Freigabe Ihrer Azure VMware Solution-Windows-VM zu. Verwenden Sie den SMB-Einbindungspfad (wie im vorherigen Screenshot gezeigt). Weitere Informationen finden Sie unter [Netzlaufwerk in Windows 10 zuordnen](https://support.microsoft.com/help/4026635/windows-10-map-a-network-drive).

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über die [Speicherhierarchie von Azure NetApp Files](../azure-netapp-files/azure-netapp-files-understand-storage-hierarchy.md).
- Lesen Sie [Lebenszyklusverwaltung von Azure VMware Solution-VMs](lifecycle-management-of-azure-vmware-solution-vms.md).
- Lesen Sie [Integrieren von Azure VMware Solution in eine Hub-and-Spoke-Architektur](concepts-avs-hub-and-spoke-integration.md).
