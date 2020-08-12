---
title: Einbinden von Azure NetApp Files-Volumes auf virtuellen Computern
description: Erfahren Sie, wie ein Volume auf virtuellen Windows- oder Linux-Computern eingebunden oder die Einbindung aufgehoben wird.
author: b-juche
ms.author: b-juche
ms.service: azure-netapp-files
ms.workload: storage
ms.topic: how-to
ms.date: 07/06/2020
ms.openlocfilehash: 4ad3800748330d5c3a6a32c6c0824bc72a05d0ef
ms.sourcegitcommit: 3d56d25d9cf9d3d42600db3e9364a5730e80fa4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/03/2020
ms.locfileid: "87533086"
---
# <a name="mount-or-unmount-a-volume-for-windows-or-linux-virtual-machines"></a>Einbinden oder Aufheben der Einbindung eines Volumes auf virtuellen Windows- oder Linux-Computern 

Volumes für virtuelle Windows- oder Linux-Computer lassen sich nach Bedarf einbinden. Dies kann jederzeit wieder rückgängig gemacht werden.  Die Einbindungsanweisungen für virtuelle Linux-Computer stehen in Azure NetApp Files zur Verfügung.  

> [!IMPORTANT] 
> Sie benötigen mindestens eine Exportrichtlinie, um auf ein NFS-Volume zugreifen zu können.

1. Klicken Sie auf das Blatt **Volumes**, und wählen Sie dann das Volume aus, für das die Einbindung erfolgen soll. 
2. Klicken Sie für das ausgewählte Volume auf **Einbindungsanweisungen**, und führen Sie die Schritte aus, um das Volume einzubinden. 

    ![Einbindungsanweisungen: NFS](../media/azure-netapp-files/azure-netapp-files-mount-instructions-nfs.png)

    ![Einbindungsanweisungen: SMB](../media/azure-netapp-files/azure-netapp-files-mount-instructions-smb.png)  
    * Wenn Sie ein NFS-Volume einbinden, stellen Sie sicher, dass Sie die Option `vers` im `mount`-Befehl verwenden, um die NFS-Protokollversion anzugeben, die dem Volume entspricht, das Sie einbinden möchten. 
    * Wenn Sie mit NFSv4.1 arbeiten, binden Sie Ihr Dateisystem mit dem folgenden Befehl ein: `sudo mount -t nfs -o rw,hard,rsize=65536,wsize=65536,vers=4.1,tcp,sec=sys $MOUNTTARGETIPADDRESS:/$VOLUMENAME $MOUNTPOINT`  

3. Wenn Sie möchten, dass ein NFS-Volume automatisch bereitgestellt wird, wenn eine Azure-VM gestartet oder neu gestartet wird, fügen Sie der Datei `/etc/fstab` auf dem Host einen Eintrag hinzu. 

    Beispiel: `$ANFIP:/$FILEPATH        /$MOUNTPOINT    nfs bg,rw,hard,noatime,nolock,rsize=65536,wsize=65536,vers=3,tcp,_netdev 0 0`

    * `$ANFIP` ist die IP-Adresse des Azure NetApp Files-Volumes, die Sie auf dem Eigenschaftenblatt für das Volume finden.
    * `$FILEPATH` ist der Exportpfad zum Azure NetApp Files-Volume.
    * `$MOUNTPOINT` ist das auf dem Linux-Host erstellte Verzeichnis, das zum Einbinden des NFS-Exports verwendet wird.

4. Wenn Sie das Volume mithilfe von NFS in Windows einbinden möchten:

    a. Binden Sie das Volume zuerst auf einer Unix- oder Linux-VM ein.  
    b. Führen Sie einen der Befehle `chmod 777` oder `chmod 775` für das Volume aus.  
    c. Binden Sie das Volume über den NFS-Client unter Windows ein.
    
5. Wenn Sie ein NFS-Kerberos-Volume einbinden möchten, finden Sie weitere Informationen unter [Konfigurieren der NFSv4.1-Kerberos-Verschlüsselung](configure-kerberos-encryption.md). 

## <a name="next-steps"></a>Nächste Schritte

* [Konfigurieren der NFSv4.1-Fehlerdomäne für Azure NetApp Files](azure-netapp-files-configure-nfsv41-domain.md)
* [Häufig gestellte Fragen zu NFS](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-faqs#nfs-faqs)
* [Network File System (Übersicht)](https://docs.microsoft.com/windows-server/storage/nfs/nfs-overview)
* [Einbinden eines NFS-Kerberos-Volumes](configure-kerberos-encryption.md#kerberos_mount)
