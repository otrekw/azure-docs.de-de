---
title: Konfigurieren der NFSv4.1-Standarddomäne für Azure NetApp Files | Microsoft-Dokumentation
description: Beschreibt, wie Sie den NFS-Client für die Verwendung von NFSv4.1 mit Azure NetApp Files konfigurieren.
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 11/08/2019
ms.author: b-juche
ms.openlocfilehash: e749f27875612136c50938712fded6a371f8c7ab
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91325622"
---
# <a name="configure-nfsv41-default-domain-for-azure-netapp-files"></a>Konfigurieren der NFSv4.1-Fehlerdomäne für Azure NetApp Files

NFSv4 führt das Konzept einer Authentifizierungsdomäne ein. Azure NetApp Files unterstützt zurzeit nur die root-Benutzerzuordnung vom Dienst zum NFS-Client. Um die NFSv4.1-Funktionalität mit Azure NetApp Files zu verwenden, müssen Sie den NFS-Client aktualisieren.

## <a name="default-behavior-of-usergroup-mapping"></a>Standardverhalten der Benutzer-/Gruppenzuordnung

Die Stammzuordnung erfolgt standardmäßig mit dem Benutzer `nobody`, da die NFSv4-Domäne auf `localdomain` festgelegt ist. Wenn Sie ein Azure NetApp Files NFSv4.1-Volume als root einbinden, werden die Dateiberechtigungen wie folgt angezeigt:  

![Standardverhalten der Benutzer-/Gruppenzuordnung für NFSv4.1](../media/azure-netapp-files/azure-netapp-files-nfsv41-default-behavior-user-group-mapping.png)

Wie das obige Beispiel zeigt, muss der Benutzer für `file1``root` sein, doch er wird standardmäßig `nobody` zugeordnet.  Dieser Artikel beschreibt, wie Sie den Benutzer `file1` auf `root` festlegen.  

## <a name="steps"></a>Schritte 

1. Bearbeiten Sie die Datei `/etc/idmapd.conf` auf dem NFS-Client.   
    Heben Sie die Auskommentierung der Zeile `#Domain` auf (d. h., entfernen Sie das `#` aus der Zeile), und ändern Sie den Wert `localdomain` in `defaultv4iddomain.com`. 

    Anfängliche Konfiguration: 
    
    ![Anfängliche Konfiguration für NFSv4.1](../media/azure-netapp-files/azure-netapp-files-nfsv41-initial-config.png)

    Aktualisierte Konfiguration:
    
    ![Aktualisierte Konfiguration für NFSv4.1](../media/azure-netapp-files/azure-netapp-files-nfsv41-updated-config.png)

2. Heben Sie die Bereitstellung aller aktuell bereitgestellten NFS-Volumes auf.
3. Aktualisieren Sie die `/etc/idmapd.conf`-Datei.
4. Starten Sie den `rpcbind`-Dienst auf Ihrem Host neu (`service rpcbind restart`), oder starten Sie den Host einfach neu.
5. Binden Sie die NFS-Volumes nach Bedarf ein.   

    Weitere Informationen finden Sie unter [Einbinden oder Aufheben der Einbindung eines Volumes auf virtuellen Windows- oder Linux-Computern](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md). 

Das folgende Beispiel zeigt die resultierende Benutzer-/Gruppenänderung: 

![Screenshot, der ein Beispiel für die sich ergebende Benutzer-/Gruppenänderung zeigt.](../media/azure-netapp-files/azure-netapp-files-nfsv41-resulting-config.png)

Wie das Beispiel zeigt, hat sich der Benutzer/die Gruppe nun von `nobody` in `root` geändert.

## <a name="behavior-of-other-non-root-users-and-groups"></a>Verhalten anderer (nicht-root) Benutzer und Gruppen

Azure NetApp Files unterstützt lokale Benutzer (lokal auf einem Host erstellte Benutzer), die über Berechtigungen verfügen, die Dateien oder Ordnern auf NFSv4.1-Volumes zugeordnet sind. Der Dienst unterstützt derzeit jedoch keine Zuordnung der Benutzer/Gruppen über mehrere Knoten hinweg. Daher werden Benutzer, die auf einem Host erstellt werden, nicht standardmäßig den Benutzern zugeordnet, die auf einem anderen Host erstellt wurden. 

Im folgenden Beispiel verfügt `Host1` über drei vorhandene Testbenutzerkonten (`testuser01`, `testuser02`, `testuser03`): 

![Screenshot, der zeigt, dass Host1 drei bestehende Testbenutzerkonten besitzt.](../media/azure-netapp-files/azure-netapp-files-nfsv41-host1-users.png)

Beachten Sie, dass die Testbenutzerkonten auf `Host2` nicht erstellt wurden, dass aber dasselbe Volume auf beiden Hosts bereitgestellt ist:

![Resultierende Konfiguration für NFSv4.1](../media/azure-netapp-files/azure-netapp-files-nfsv41-host2-users.png)

## <a name="next-step"></a>Nächster Schritt 

[Einbinden oder Aufheben der Einbindung eines Volumes auf virtuellen Windows- oder Linux-Computern](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md)

