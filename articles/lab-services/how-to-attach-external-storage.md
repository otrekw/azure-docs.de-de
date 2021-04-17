---
title: Verwenden von externem Dateispeicher in Lab Services | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie ein Lab einrichten, das externen Dateispeicher in Lab Services verwendet.
author: emaher
ms.topic: article
ms.date: 03/30/2021
ms.author: enewman
ms.openlocfilehash: 888e04db76567051f8c5eae7cf94c77e684cb146
ms.sourcegitcommit: 5fd1f72a96f4f343543072eadd7cdec52e86511e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/01/2021
ms.locfileid: "106111100"
---
# <a name="using-external-file-storage-in-lab-services"></a>Verwenden von externem Dateispeicher in Lab Services

In diesem Artikel werden einige der Optionen für die Speicherung externer Dateien behandelt, wenn Sie Azure Lab Services verwenden.  [Azure Files](https://azure.microsoft.com/services/storage/files/) bietet vollständig verwaltete Dateifreigaben in der Cloud, [auf die über SMB 2.1. und SMB 3.0](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-windows) zugegriffen werden kann.  Eine Azure Files-Freigabe kann entweder öffentlich oder privat innerhalb eines virtuellen Netzwerks verbunden werden.  Außerdem kann sie so konfiguriert werden, dass sie die AD-Anmeldeinformationen des Kursteilnehmers für die Verbindung mit der Dateifreigabe verwendet.  Die Verwendung von Azure NetApp Files mit NFS-Volumes für Linux-Computer ist eine weitere Option für die Speicherung externer Dateien mit Azure Lab Services.  

## <a name="deciding-which-solution-to-use"></a>Entscheiden über die zu verwendende Lösung

Jede Lösung hat unterschiedliche Anforderungen und Funktionen.  In der folgenden Tabelle sind wichtige Punkte aufgeführt, die bei jeder Lösung zu beachten sind.  

|     Lösung    |    Wichtige Information    |
| -------------- | ------------------------ |
| [Azure Files-Freigabe mit öffentlichem Endpunkt](#azure-files-share) | <ul><li>Jeder verfügt über Lese-/Schreibzugriff.</li><li>Kein VNET-Peering erforderlich.</li><li>Zugriff für alle virtuellen Computer möglich, nicht nur für virtuelle Lab-Computer.</li><li>Bei Verwendung von Linux haben Kursteilnehmer Zugriff auf den Speicherkontoschlüssel.</li></ul> |
| [Azure Files-Freigabe mit privatem Endpunkt](#azure-files-share) | <ul><li>Jeder verfügt über Lese-/Schreibzugriff.</li><li>VNET-Peering erforderlich.</li><li>Zugriff nur für virtuelle Computer im selben Netzwerk (oder Peernetzwerk) wie das Speicherkonto möglich.</li><li>Bei Verwendung von Linux haben Kursteilnehmer Zugriff auf den Speicherkontoschlüssel.</li></ul> |
| [Azure Files mit identitätsbasierter Autorisierung](#azure-files-with-identity-base-authorization) | <ul><li>Entweder Lese- oder Lese-/Schreibzugriffsberechtigungen können für einen Ordner oder eine Datei festgelegt werden.</li><li>VNET-Peering erforderlich.</li><li>Das Speicherkonto muss mit Active Directory verbunden sein.</li><li>Lab-VMs müssen in eine Domäne eingebunden sein.</li><li>Der Speicherkontoschlüssel wird für Kursteilnehmer nicht zum Herstellen einer Verbindung mit der Dateifreigabe verwendet.</li></ul> |
| [NetApp Files mit NFS-Volumes](#netapp-files-with-nfs-volumes) | <ul><li>Entweder Lese- oder Lese-/Schreibzugriff kann für Volumes festgelegt werden.</li><li>Berechtigungen werden mithilfe der IP-Adresse des virtuellen Computers des Kursteilnehmers festgelegt.</li><li>VNET-Peering erforderlich.</li><li>Muss sich möglicherweise für die Verwendung des NetApp Files-Diensts registrieren.</li><li>Nur Linux.</li></ul>

Die Kosten für die Verwendung von externem Speicher sind nicht in den Kosten für die Verwendung von Azure Lab Services enthalten.  Weitere Informationen zu den Preisen finden Sie unter [Azure Files – Preise](https://azure.microsoft.com/pricing/details/storage/files/) und [Azure NetApp Files – Preise](https://azure.microsoft.com/pricing/details/netapp/).

## <a name="azure-files-share"></a>Azure Files-Freigabe

Der Zugriff auf Azure Files-Freigaben erfolgt über einen öffentlichen oder privaten Endpunkt.  Azure Files-Freigaben werden mithilfe des Speicherkontoschlüssels als Kennwort eingebunden.  Bei diesem Ansatz besitzt jeder Lese-/Schreibzugriff auf die Dateifreigabe.

Wenn Sie einen öffentlichen Endpunkt für die Azure Files-Freigabe verwenden, ist es wichtig, Folgendes zu beachten:

- Das virtuelle Netzwerk für das Speicherkonto muss nicht mit dem Lab-Konto gepeert sein.  Die Dateifreigabe kann jederzeit vor der Veröffentlichung der Vorlage für virtuelle Computer erstellt werden.
- Der Zugriff auf die Dateifreigabe kann von jedem beliebigen Computer aus erfolgen, wenn ein Benutzer über den Speicherkontoschlüssel verfügt.  
- Linux-Kursteilnehmer können den Speicherkontoschlüssel sehen.  Anmeldeinformationen für das Einbinden einer Azure Files-Freigabe werden auf virtuellen Linux-Computern in `{file-share-name}.cred` gespeichert und können von sudo gelesen werden.  Da Kursteilnehmer standardmäßig sudo-Zugriff auf Azure Lab Services-VMs erhalten, können Sie den Speicherkontoschlüssel lesen.  Wenn der Endpunkt des Speicherkontos öffentlich ist, können Kursteilnehmer Zugriff auf die Dateifreigabe außerhalb Ihres virtuellen Kursteilnehmer-Computers erhalten.  Erwägen Sie, den Speicherkontoschlüssel im Anschluss an das Ende des Kurses zu rotieren und private Dateifreigaben zu verwenden.

Wenn Sie einen privaten Endpunkt für die Azure Files-Freigabe verwenden, ist es wichtig, Folgendes zu beachten:

- Der Zugriff ist auf Datenverkehr beschränkt, der aus dem privaten Netzwerk stammt, und es kann den nicht über das öffentliche Internet darauf zugegriffen werden.  Nur VMs im privaten virtuellen Netzwerk, virtuelle Computer in einem Netzwerk, das mit dem privaten virtuellen Netzwerk gepeert ist, oder Computer, die mit einem VPN für das private Netzwerk verbunden sind, können auf die Dateifreigabe zugreifen.  
- Linux-Kursteilnehmer können den Speicherkontoschlüssel sehen.  Anmeldeinformationen für das Einbinden einer Azure Files-Freigabe werden auf virtuellen Linux-Computern in `{file-share-name}.cred` gespeichert und können von sudo gelesen werden.  Da Kursteilnehmer standardmäßig sudo-Zugriff auf Azure Lab Services-VMs erhalten, können Sie den Speicherkontoschlüssel lesen.  Erwägen Sie, den Speicherkontoschlüssel im Anschluss an das Ende des Kurses zu rotieren.
- Dieser Ansatz erfordert, dass das virtuelle Netzwerk der Dateifreigabe mit dem Lab-Konto gepeert ist.  Das virtuelle Netzwerk für das Azure Storage-Konto muss mit dem virtuellen Netzwerk für das Lab-Konto gepeert werden, **bevor** das Lab erstellt wird.

> [!NOTE]
> Dateifreigaben mit einer Größe von mehr als 5 TB sind nur für [[lokal redundante Speicherkonten (LRS)]](/azure/storage/files/storage-files-how-to-create-large-file-share#restrictions) verfügbar.

Führen Sie die folgenden Schritte aus, um eine mit einer Azure Files-Freigabe verbundene VM zu erstellen.

1. Erstellen Sie ein [Azure Storage-Konto](/azure/storage/files/storage-how-to-create-file-share). Wählen Sie auf der Seite „Verbindungsmethode“ öffentlicher Endpunkt oder privater Endpunkt aus.
2. Wenn Sie einen [privaten Endpunkt](/azure/private-link/create-private-endpoint-storage-portal) verwenden, erstellen Sie einen, damit aus dem virtuellen Netzwerk auf die Dateifreigaben zugegriffen werden kann.  Erstellen Sie eine [private DNS-Zone](/azure/dns/private-dns-privatednszone), oder verwenden Sie eine vorhandene. Private Azure DNS-Zonen bieten Namensauflösung in einem virtuellen Netzwerk.
3. [Erstellen Sie eine Azure-Dateifreigabe](/azure/storage/files/storage-how-to-create-file-share). Die Dateifreigabe ist über den öffentlichen Hostnamen des Speicherkontos erreichbar.
4. Binden Sie die Azure-Dateifreigabe in die Vorlage für virtuelle Computer ein:
    - [[Windows]](/azure/storage/files/storage-how-to-use-files-windows)
    - [[Linux]](/azure/storage/files/storage-how-to-use-files-linux).  Informationen, wie Sie Probleme beim Einbinden auf VMs von Kursteilnehmern vermeiden können, finden Sie unter [Verwenden von Azure Files mit Linux](#using-azure-files-with-linux).
5. [Veröffentlichen](how-to-create-manage-template.md#publish-the-template-vm) Sie die Vorlage für virtuelle Computer.

> [!IMPORTANT]
> Stellen Sie sicher, dass die Firewall ausgehende SMB-Verbindungen über Port 445 nicht blockiert. SMB ist standardmäßig für Azure-VMs zulässig.

### <a name="using-azure-files-with-linux"></a>Verwenden von Azure Files mit Linux

Wenn Sie die _Standardanweisungen_ zum Einbinden einer Azure-Dateifreigabe verwenden, scheint die Dateifreigabe auf VMs von Kursteilnehmern zu verschwinden, sobald die Vorlage veröffentlicht wurde.  Im Folgenden finden Sie ein modifiziertes Skript, das dieses Problem behandelt.  

```bash
#!/bin/bash

# Assign variables values for your storage account and file share
storage_account_name=""
storage_account_key=""
fileshare_name=""

# Do not use 'mnt' for mount directory.
# Using ‘mnt’ will cause issues on student VMs.
mount_directory="prm-mnt" 

sudo mkdir /$mount_directory/$fileshare_name
if [ ! -d "/etc/smbcredentials" ]; then
    sudo mkdir /etc/smbcredentials
fi
if [ ! -f "/etc/smbcredentials/$storage_account_name.cred" ]; then
    sudo bash -c "echo ""username=$storage_account_name"" >> /etc/smbcredentials/$storage_account_name.cred"
    sudo bash -c "echo ""password=$storage_account_key"" >> /etc/smbcredentials/$storage_account_name.cred"
fi
sudo chmod 600 /etc/smbcredentials/$storage_account_name.cred

sudo bash -c "echo ""//$storage_account_name.file.core.windows.net/$fileshare_name /$mount_directory/$fileshare_name cifs nofail,vers=3.0,credentials=/etc/smbcredentials/$storage_account_name.cred,dir_mode=0777,file_mode=0777,serverino"" >> /etc/fstab"
sudo mount -t cifs //$storage_account_name.file.core.windows.net/$fileshare_name /$mount_directory/$fileshare_name -o vers=3.0,credentials=/etc/smbcredentials/$storage_account_name.cred,dir_mode=0777,file_mode=0777,serverino
```

Wenn die Vorlage für virtuelle Computer, die die Azure-Dateifreigabe in das Verzeichnis `/mnt` einbindet, bereits veröffentlicht wurde, kann der Kursteilnehmer eine der folgenden Aktionen ausführen.

- Verschieben Sie die Anweisung zum Einbinden von `/mnt` den Anfang der Datei `/etc/fstab`.  
- Ändern Sie in der Anweisung zum Einbinden von `/mnt/{file-share-name}` das Zielverzeichnis, z. B. in `/prm-mnt/{file-share-name}`.

Kursteilnehmer sollten `mount -a` ausführen, um Verzeichnisse erneut einzubinden.

Weitere allgemeine Informationen zur Verwendung von Dateifreigaben mit Linux finden Sie unter [Verwenden von Azure Files mit Linux](/azure/storage/files/storage-how-to-use-files-linux).

## <a name="azure-files-with-identity-base-authorization"></a>Azure Files mit identitätsbasierter Autorisierung

Auf Azure Files-Freigaben kann auch mithilfe der AD-Authentifizierung zugegriffen werden, wenn Folgendes erfüllt ist:

1. Die VM des Kursteilnehmers ist in eine Domäne eingebunden.
2. AD-Authentifizierung [ist in dem Azure Storage-Konto aktiviert](/azure/storage/files/storage-files-active-directory-overview), das die Dateifreigabe hostet.  

Das Netzwerklaufwerk wurde unter Verwendung der Identität des Benutzers auf dem virtuellen Computer eingebunden, nicht mit dem Schlüssel für das Speicherkonto.  Der Zugriff auf das Speicherkonto kann öffentliche oder private Endpunkte verwenden.

Besprechen wir ein paar wichtige Punkte dieses Ansatzes.

- Berechtigungen können auf Verzeichnis- oder Dateiebene festgelegt werden.
- Die aktuellen Benutzeranmeldeinformationen werden verwendet, um sich bei der Dateifreigabe zu authentifizieren.

Wenn Sie einen öffentlichen Endpunkt für die Azure Files-Freigabe verwenden, ist es wichtig, Folgendes zu beachten:

- Das virtuelle Netzwerk für das Speicherkonto muss nicht mit dem Lab-Konto gepeert sein.  Die Dateifreigabe kann jederzeit vor der Veröffentlichung der Vorlage für virtuelle Computer erstellt werden.

Wenn Sie einen privaten Endpunkt für die Azure Files-Freigabe verwenden, ist es wichtig, Folgendes zu beachten:

- Der Zugriff ist auf Datenverkehr beschränkt, der aus dem privaten Netzwerk stammt, und es kann den nicht über das öffentliche Internet darauf zugegriffen werden.  Nur VMs im privaten virtuellen Netzwerk, virtuelle Computer in einem Netzwerk, das mit dem privaten virtuellen Netzwerk gepeert ist, oder Computer, die mit einem VPN für das private Netzwerk verbunden sind, können auf die Dateifreigabe zugreifen.  
- Dieser Ansatz erfordert, dass das virtuelle Netzwerk der Dateifreigabe mit dem Lab-Konto gepeert ist.  Das virtuelle Netzwerk für das Azure Storage-Konto muss mit dem virtuellen Netzwerk für das Lab-Konto gepeert werden, **bevor** das Lab erstellt wird.

Führen Sie die folgenden Schritte aus, um eine Azure Files-Freigabe zu erstellen, die für AD-Authentifizierung aktiviert ist, sowie um die Lab-VMs in eine Domäne einzubinden.

1. Erstellen Sie ein [Azure Storage-Konto](/azure/storage/files/storage-how-to-create-file-share).
2. Wenn Sie einen [privaten Endpunkt](/azure/private-link/create-private-endpoint-storage-portal) verwenden, erstellen Sie einen, damit aus dem virtuellen Netzwerk auf die Dateifreigaben zugegriffen werden kann.  Erstellen Sie eine [private DNS-Zone](/azure/dns/private-dns-privatednszone), oder verwenden Sie eine vorhandene. Private Azure DNS-Zonen bieten Namensauflösung in einem virtuellen Netzwerk.
3. [Erstellen Sie eine Azure-Dateifreigabe](/azure/storage/files/storage-how-to-create-file-share).
4. Führen Sie die Schritte zum Aktivieren der identitätsbasierten Autorisierung aus.  Wenn Sie ein lokales AD verwenden, das mit Azure AD synchronisiert wird, befolgen Sie die Schritte für die [lokale Active Directory Domain Services-Authentifizierung über SMB für Azure-Dateifreigaben](/azure/storage/files/storage-files-identity-auth-active-directory-enable).  Wenn Sie nur Azure AD verwenden, befolgen Sie die Schritte zum [Aktivieren der Azure Active Directory Domain Services-Authentifizierung in Azure Files](/azure/storage/files/storage-files-identity-auth-active-directory-domain-service-enable).
    >[!IMPORTANT]
    >Sprechen Sie mit dem Team, das Ihr AD verwaltet, um sicherzustellen, dass alle in den Anweisungen aufgeführten Voraussetzungen erfüllt sind.
5. Weisen Sie Rollen für die SMB-Freigabeberechtigung in Azure zu.  Ausführliche Informationen zu den Berechtigungen, die jeder Rolle gewährt werden, finden Sie unter [Berechtigungen auf Freigabeebene](/azure/storage/files/storage-files-identity-ad-ds-assign-permissions).
    1. Die Rolle „Mitwirkender mit erhöhten Rechten für Speicherdateidaten-SMB-Freigabe“ muss der Person oder Gruppe zugewiesen werden, die Berechtigungen für den Inhalt der Dateifreigabe einrichtet.
    2. Die Rolle „Mitwirkender für Speicherdateidaten-SMB-Freigabe“ sollte Kursteilnehmern zugewiesen werden, die Dateien auf der Dateifreigabe hinzufügen oder bearbeiten müssen.
    3. Die Rolle „Leser für Speicherdateidaten-SMB-Freigabe“ sollte Kursteilnehmern zugewiesen werden, die die Dateien auf der Dateifreigabe nur lesen müssen.
6. Richten Sie Berechtigungen auf Verzeichnis-/Dateiebene für die Dateifreigabe ein.  Berechtigungen müssen von einem in die Domäne eingebundenen Computer aus eingerichtet werden, der über Netzwerkzugriff auf die Dateifreigabe verfügt.  **Um Berechtigungen auf Verzeichnis-/Dateiebene zu ändern, binden Sie die Dateifreigabe mit dem Speicherschlüssel ein, nicht mit Ihren AAD-Anmeldeinformationen.**  Der PowerShell-Befehl [Set-Acl](/powershell/module/microsoft.powershell.security/set-acl) oder [icacls](/windows-server/administration/windows-commands/icacls) sind die empfohlenen Tools, die Sie beim Zuweisen von Berechtigungen verwenden sollten.
7. [Peeren Sie das virtuelle Netzwerk](how-to-connect-peer-virtual-network.md) für das Speicherkonto mit dem Lab-Konto.
8. [Erstellen Sie das Classroom-Lab](how-to-manage-classroom-labs.md).
9. Speichern Sie ein Skript auf der Vorlage für virtuelle Computer, das Kursteilnehmer ausführen können, um eine Verbindung mit dem Netzwerklaufwerk herzustellen.  So rufen Sie das Beispielskript ab
    1. Öffnen Sie das Speicherkonto im Azure-Portal.
    1. Wählen Sie im Menü **Dateidienst** den Eintrag **Dateifreigaben** aus.
    1. Suchen Sie die Freigabe, mit der Sie eine Verbindung herstellen möchten, wählen Sie ganz rechts die Schaltfläche mit den Auslassungspunkten aus, und wählen Sie **Verbinden** aus.
    1. Das **Verbinden**-Flyout wird mit Anweisungen für Windows, Linux und macOS angezeigt.  Wenn Sie Windows verwenden, legen Sie die **Authentifizierungsmethode** auf **Active Directory** fest.
    1. Kopieren Sie den Code in dem Beispiel, und speichern Sie ihn auf dem Vorlagencomputer in einer `.ps1`-Datei für Windows oder in einer `.sh`-Datei für Linux.
10. Laden Sie das Skript auf den Vorlagencomputer herunter, und führen Sie es aus, um [Computer von Kursteilnehmern in die Domäne einzubinden](https://github.com/Azure/azure-devtestlab/blob/master/samples/ClassroomLabs/Scripts/ActiveDirectoryJoin/README.md#usage).  Das `Join-AzLabADTemplate`-Skript [veröffentlicht die Vorlage für virtuelle Computer](how-to-create-manage-template.md#publish-the-template-vm) automatisch.  
    > [!NOTE]
    > Der Vorlagencomputer ist in keine Domäne eingebunden. Kursleiter sollten sich selbst eine veröffentlichte Kursteilnehmer-VM zuweisen, um Dateien auf der Freigabe anzuzeigen.
11. Kursteilnehmer, die Windows verwenden, können über den [Datei-Explorer](/azure/storage/files/storage-how-to-use-files-windows) mit Ihren Anmeldeinformationen eine Verbindung mit der Azure Files-Freigabe herstellen, sobald sie den Pfad zur Dateifreigabe erhalten haben.  Alternativ können Kursteilnehmer das oben erstellte Skript ausführen, um eine Verbindung mit dem Netzwerklaufwerk herzustellen.  Führen Sie für Kursteilnehmer, die Linux verwenden, das oben erstellte Skript aus.

## <a name="netapp-files-with-nfs-volumes"></a>NetApp Files mit NFS-Volumes

[Azure NetApp Files](https://azure.microsoft.com/services/netapp/) ist ein leistungsstarker Dateispeicherdienst auf Unternehmensniveau.

- Zugriffsrichtlinien können pro Volume festgelegt werden.
- Berechtigungsrichtlinien sind für jedes Volume IP-basiert.
- Wenn Kursteilnehmer ein eigenes Volume benötigen, auf das andere Kursteilnehmer nicht zugreifen können, müssen Sie nach der Veröffentlichung des Labs Berechtigungsrichtlinien zuweisen.
- Im Zusammenhang mit Azure Lab Services werden nur Linux-Computer unterstützt.
- Das virtuelle Netzwerk für den Azure NetApp Files-Kapazitätspool muss mit dem virtuellen Netzwerk für das Lab-Konto gepeert werden, **bevor** das Lab erstellt wird.

Führen Sie die folgenden Schritte aus, um eine Azure NetApp Files-Freigabe in Azure Lab Services zu verwenden.

1. Führen Sie bei Bedarf ein Onboarding in [Azure NetApp Files](https://aka.ms/azurenetappfiles) durch.
2. Informationen zum Erstellen eines NetApp Files-Kapazitätspools und von NFS-Volumes finden Sie unter [Einrichten von Azure NetApp Files und NFS-Volume](/azure/azure-netapp-files/azure-netapp-files-quickstart-set-up-account-create-volumes).  Informationen zu Servicelevels finden Sie unter [Servicelevels für Azure NetApp Files](/azure/azure-netapp-files/azure-netapp-files-service-levels).
3. [Peeren Sie das virtuelle Netzwerk](how-to-connect-peer-virtual-network.md) für den NetApp Files-Kapazitätspool mit dem Lab-Konto.
4. [Erstellen Sie das Classroom-Lab](how-to-manage-classroom-labs.md).
5. Installieren Sie auf der Vorlage für virtuelle Computer die erforderlichen Komponenten, um NFS-Dateifreigaben zu verwenden.
    - Ubuntu:

        ```bash
        sudo apt update
        sudo apt install nfs-common
        ```

    - CentOS

        ```bash
        sudo yum install nfs-utils
        ```

6. Speichern Sie auf der Vorlage für virtuelle Computer das folgende Skript als `mount_fileshare.sh`, um [die NetApp Files-Freigabe einzubinden](/azure/azure-netapp-files/azure-netapp-files-mount-unmount-volumes-for-virtual-machines).  Weisen Sie der Variablen `capacity_pool_ipaddress` die IP-Adresse des Einbindungsziels für den Kapazitätspool zu.  Sehen Sie in den Anweisungen zum Einbinden für das Volume nach, um den entsprechenden Wert zu ermitteln.  Das Skript erwartet den Pfad/Namen des NetApp Files-Volumes.  Vergessen Sie nicht, `chmod u+x mount_fileshare.sh` auszuführen, um sicherzustellen, dass das Skript von Benutzern ausgeführt werden kann.

    ```bash
    #!/bin/bash
    
    if [ $# -eq 0 ];  then
        echo "Must provide volume name."
        exit 1
    fi
    
    volume_name=$1
    capacity_pool_ipaddress=0.0.0.0 # IP address of capacity pool
    
    # Do not use 'mnt' for mount directory.
    # Using ‘mnt’ may cause issues on student VMs.
    mount_directory="prm-mnt" 
    
    sudo mkdir -p /$mount_directory
    sudo mkdir /$mount_directory/$folder_name
    
    sudo mount -t nfs -o rw,hard,rsize=65536,wsize=65536,vers=3,tcp $capacity_pool_ipaddress:/$volume_name /$mount_directory/$volume_name
    sudo bash -c "echo ""$capacity_pool_ipaddress:/$volume_name /$mount_directory/$volume_name nfs bg,rw,hard,noatime,nolock,rsize=65536,wsize=65536,vers=3,tcp,_netdev 0 0"" >> /etc/fstab"
    ```

7. Wenn alle Kursteilnehmer gemeinsam Zugriff auf dasselbe NetApp Files-Volume haben, kann das Skript `mount_fileshare.sh` vor der Veröffentlichung auf der Vorlage für virtuelle Computer ausgeführt werden.  Wenn Kursteilnehmer jeweils ein eigenes Volume erhalten, speichern Sie das Skript, damit es später vom jeweiligen Kursteilnehmer ausgeführt werden kann.
8. [Veröffentlichen](how-to-create-manage-template.md#publish-the-template-vm) Sie die Vorlage für virtuelle Computer.
9. [Konfigurieren Sie die Richtlinie](/azure/azure-netapp-files/azure-netapp-files-configure-export-policy) für die Dateifreigabe.  Die Exportrichtlinie kann zulassen, dass ein einzelner virtueller Computer oder mehrere VMs auf ein Volume zugreifen können.  Reiner Lese- oder Lese-/Schreibzugriff kann gewährt werden.
10. Kursteilnehmer müssen Ihre VMs starten und das Skript ausführen, um die Dateifreigabe einzubinden.  Sie müssen das Skript nur einmal ausführen.  Der Befehl sieht wie folgt aus: `./mount_fileshare.sh myvolumename`.

## <a name="next-steps"></a>Nächste Schritte

Die nächsten Einrichtungsschritte sind für alle Labs gleich.

- [Erstellen und Verwalten einer Vorlage](how-to-create-manage-template.md)
- [Hinzufügen von Benutzern](tutorial-setup-classroom-lab.md#add-users-to-the-lab)
- [Kontingent festlegen](how-to-configure-student-usage.md#set-quotas-for-users)
- [Zeitplan festlegen](tutorial-setup-classroom-lab.md#set-a-schedule-for-the-lab)
- [Registrierungslinks per E-Mail an Kursteilnehmer senden](how-to-configure-student-usage.md#send-invitations-to-users)