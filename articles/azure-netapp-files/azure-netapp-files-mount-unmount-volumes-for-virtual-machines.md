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
ms.openlocfilehash: 4cd3bc7e4f95869d3efd2d92a7cdf1addc7ce5b2
ms.sourcegitcommit: 44c2a964fb8521f9961928f6f7457ae3ed362694
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2019
ms.locfileid: "73953096"
---
# <a name="mount-or-unmount-a-volume-for-windows-or-linux-virtual-machines"></a>Einbinden oder Aufheben der Einbindung eines Volumes auf virtuellen Windows- oder Linux-Computern 

Volumes für virtuelle Windows- oder Linux-Computer lassen sich nach Bedarf einbinden. Dies kann jederzeit wieder rückgängig gemacht werden.  Die Einbindungsanweisungen für virtuelle Linux-Computer stehen in Azure NetApp Files zur Verfügung.  

1. Klicken Sie auf das Blatt **Volumes**, und wählen Sie dann das Volume aus, für das die Einbindung erfolgen soll. 
2. Klicken Sie für das ausgewählte Volume auf **Einbindungsanweisungen**, und führen Sie die Schritte aus, um das Volume einzubinden. 

    ![Einbindungsanweisungen: NFS](../media/azure-netapp-files/azure-netapp-files-mount-instructions-nfs.png)

    ![Einbindungsanweisungen: SMB](../media/azure-netapp-files/azure-netapp-files-mount-instructions-smb.png)
    
Wenn Sie mit NFSv4.1 arbeiten, binden Sie Ihr Dateisystem mit dem folgenden Befehl ein:  

`sudo mount -t nfs -o rw,hard,rsize=65536,wsize=65536,vers=4.1,tcp,sec=sys $MOUNTTARGETIPADDRESS:/$VOLUMENAME $MOUNTPOINT`  

## <a name="next-steps"></a>Nächste Schritte

* [Konfigurieren der NFSv4.1-Fehlerdomäne für Azure NetApp Files](azure-netapp-files-configure-nfsv41-domain.md)
* Informationen dazu, wie Sie ein NFS-Volume automatisch bereitstellen können, wenn eine Azure-VM gestartet oder neu gestartet wird, finden Sie unter [NFS FAQs](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-faqs#nfs-faqs).
