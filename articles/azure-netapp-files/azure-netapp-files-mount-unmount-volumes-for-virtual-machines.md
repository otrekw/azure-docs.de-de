---
title: Einbinden oder Aufheben der Einbindung eines Azure NetApp Files-Volumes auf virtuellen Windows- oder Linux-Computern | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie ein Volume auf virtuellen (Linux-)Computern eingebunden oder die Einbindung aufgehoben wird.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/08/2019
ms.author: b-juche
ms.openlocfilehash: 84496fbc8a415171172d0a138f647ecb0310b6c7
ms.sourcegitcommit: 4821b7b644d251593e211b150fcafa430c1accf0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/19/2019
ms.locfileid: "74173582"
---
# <a name="mount-or-unmount-a-volume-for-windows-or-linux-virtual-machines"></a>Einbinden oder Aufheben der Einbindung eines Volumes auf virtuellen Windows- oder Linux-Computern 

Volumes für virtuelle Windows- oder Linux-Computer lassen sich nach Bedarf einbinden. Dies kann jederzeit wieder rückgängig gemacht werden.  Die Einbindungsanweisungen für virtuelle Linux-Computer stehen in Azure NetApp Files zur Verfügung.  

1. Klicken Sie auf das Blatt **Volumes**, und wählen Sie dann das Volume aus, für das die Einbindung erfolgen soll. 
2. Klicken Sie für das ausgewählte Volume auf **Einbindungsanweisungen**, und führen Sie die Schritte aus, um das Volume einzubinden. 

    ![Einbindungsanweisungen: NFS](../media/azure-netapp-files/azure-netapp-files-mount-instructions-nfs.png)

    ![Einbindungsanweisungen: SMB](../media/azure-netapp-files/azure-netapp-files-mount-instructions-smb.png)
    
    Wenn Sie mit NFSv4.1 arbeiten, binden Sie Ihr Dateisystem mit dem folgenden Befehl ein: `sudo mount -t nfs -o rw,hard,rsize=65536,wsize=65536,vers=4.1,tcp,sec=sys $MOUNTTARGETIPADDRESS:/$VOLUMENAME $MOUNTPOINT`  

3. Wenn Sie möchten, dass ein NFS-Volume automatisch bereitgestellt wird, wenn eine Azure-VM gestartet oder neu gestartet wird, fügen Sie der Datei `/etc/fstab` auf dem Host einen Eintrag hinzu. 

    Beispiel: `$ANFIP:/$FILEPATH        /$MOUNTPOINT    nfs bg,rw,hard,noatime,nolock,rsize=65536,wsize=65536,vers=3,tcp,_netdev 0 0`

    * `$ANFIP` ist die IP-Adresse des Azure NetApp Files-Volumes, die Sie auf dem Eigenschaftenblatt für das Volume finden.
    * `$FILEPATH` ist der Exportpfad zum Azure NetApp Files-Volume.
    * `$MOUNTPOINT` ist das auf dem Linux-Host erstellte Verzeichnis, das zum Einbinden des NFS-Exports verwendet wird.

## <a name="next-steps"></a>Nächste Schritte

* [Konfigurieren der NFSv4.1-Fehlerdomäne für Azure NetApp Files](azure-netapp-files-configure-nfsv41-domain.md)
* [Häufig gestellte Fragen zu NFS](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-faqs#nfs-faqs)
