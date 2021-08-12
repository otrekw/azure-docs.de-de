---
title: Konfigurieren Sie die Exportrichtlinie für Azure NetApp Files NFS- oder Dual-Protokoll-Volumes - Azure NetApp Files
description: Beschreibt, wie Sie die Exportrichtlinie zum Steuern des Zugriffs auf ein NFS-Volume mit Azure NetApp Files konfigurieren.
services: azure-netapp-files
author: b-juche
ms.author: b-juche
ms.service: azure-netapp-files
ms.workload: storage
ms.topic: how-to
ms.date: 05/07/2021
ms.openlocfilehash: 35bcff2615ab9b31f077a13c1cae7fbe38518575
ms.sourcegitcommit: c1b0d0b61ef7635d008954a0d247a2c94c1a876f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2021
ms.locfileid: "109628246"
---
# <a name="configure-export-policy-for-nfs-or-dual-protocol-volumes"></a>Konfigurieren Sie die Exportrichtlinie für NFS- oder Dualprotokollvolumes

Sie können die Exportrichtlinie konfigurieren, um den Zugriff auf ein Azure NetApp Files Volume zu steuern, das das NFS-Protokoll (NFSv3 und NFSv4.1) oder das duale Protokoll (NFSv3 und SMB) verwendet. 

Sie können bis zu fünf Exportrichtlinienregeln erstellen.

## <a name="configure-the-policy"></a>Konfigurieren der Richtlinie 

1.  Wählen Sie auf der Seite **Volumes** das Volume aus, für das Sie die Exportrichtlinie konfigurieren möchten, und wählen Sie dann **Exportrichtlinie** aus. Sie können die Exportrichtlinie auch während der Erstellung des Volumes konfigurieren.

2.  Geben Sie die folgenden Informationen an, um eine Exportrichtlinienregel zu erstellen:   
    * **Index**: Geben Sie die Indexnummer für die Regel an.  
      
      Eine Exportrichtlinie kann bis zu fünf Regeln umfassen. Regeln werden gemäß ihrer Reihenfolge in der Indexnummernliste ausgewertet. Regeln mit einer niedrigeren Indexnummer werden zuerst ausgewertet. So wird beispielsweise die Regel mit der Indexnummer 1 vor der Regel mit der Indexnummer 2 ausgewertet. 

    * **Zulässige Clients**: Geben Sie den Wert in einem der folgenden Formate an:  
      * IPv4-Adresse. Ein Beispiel: `10.1.12.24`
      * IPv4-Adresse mit einer Subnetzmaske, ausgedrückt als Anzahl von Bits. Ein Beispiel: `10.1.12.10/4`
      * Durch Trennzeichen getrennte IP-Adressen. Sie können mehrere Host-IP-Adressen in einer einzigen Regel eingeben, indem Sie sie durch Kommas trennen. Maximal 4096 Zeichen sind zulässig. Ein Beispiel: `10.1.12.25,10.1.12.28,10.1.12.29`

    * **Zugriff**: Wählen Sie einen der folgenden Berechtigungstypen aus:  
      * Kein Zugriff 
      * Lesen/Schreiben
      * Nur Leseberechtigung

    * **Nur Leseberechtigung** und **Lesen/Schreiben**: Wenn Sie die Kerberos-Verschlüsselung mit NFSv4.1 verwenden, befolgen Sie die Anweisungen unter [Konfigurieren der NFSv4.1-Kerberos-Verschlüsselung](configure-kerberos-encryption.md).  Informationen zu den Leistungsauswirkungen von Kerberos finden Sie unter [Leistungsauswirkungen von Kerberos auf NFSv4.1-Volumes](performance-impact-kerberos.md). 

      ![Kerberos-Sicherheitsoptionen](../media/azure-netapp-files/kerberos-security-options.png) 

    * **Root-Zugriff**: Geben Sie an, ob das `root`-Konto auf das Volume zugreifen kann.  Standardmäßig ist der Root-Zugriff auf **Ein** festgelegt, und das `root`-Konto hat Zugriff auf das Volume.

      ![Exportrichtlinie](../media/azure-netapp-files/azure-netapp-files-export-policy.png) 

## <a name="next-steps"></a>Nächste Schritte 
* [Mounten oder unmounten eines Volumes](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md)
* [Verwalten von Momentaufnahmen](azure-netapp-files-manage-snapshots.md)
