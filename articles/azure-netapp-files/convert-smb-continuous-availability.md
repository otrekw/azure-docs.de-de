---
title: Konvertieren vorhandener Azure NetApp Files SMB-Volumes zur Verwendung des SMB-Features „Fortlaufende Verfügbarkeit“ | Microsoft-Dokumentation
description: Beschrieben wird das Aktivieren der SMB-Zertifizierungsstelle durch Konvertieren eines vorhandenen SMB-Volumes von Azure NetApp Files.
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
ms.topic: how-to
ms.date: 05/10/2021
ms.author: b-juche
ms.openlocfilehash: b55b7e5632ad852b199121179772fa078129cdb5
ms.sourcegitcommit: 1b19b8d303b3abe4d4d08bfde0fee441159771e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/11/2021
ms.locfileid: "109755086"
---
# <a name="convert-existing-smb-volumes-to-use-continuous-availability"></a>Konvertieren vorhandener SMB-Volumes zur Verwendung der fortlaufenden Verfügbarkeit 

Sie können das SMB-Feature „Fortlaufende Verfügbarkeit“ (Continuous Availability, CA) aktivieren, wenn Sie [ein neues SMB-Volume erstellen](azure-netapp-files-create-volumes-smb.md#add-an-smb-volume). Sie können aber auch ein vorhandenes SMB-Volume konvertieren, um das SMB-Feature „Fortlaufende Verfügbarkeit“ zu aktivieren.  In diesem Artikel erfahren Sie, wie Sie durch Konvertieren eines vorhandenen Volumes das SMB-Feature „Fortlaufende Verfügbarkeit“ aktivieren.  

> [!IMPORTANT]
> Dieses Verfahren beinhaltet eine Übernahme vom ursprünglichen Volume auf das neue Volume, das für Freigaben vom Typ „Fortlaufende Verfügbarkeit“ aktiviert ist. Daher sollten Sie ein Wartungsfenster für diesen Vorgang planen. 

## <a name="steps"></a>Schritte

1. Stellen Sie sicher, dass Sie das Feature [SMB-Freigaben „Fortlaufende Verfügbarkeit“](https://aka.ms/anfsmbcasharespreviewsignup) registriert haben.  
2. Beenden Sie die Anwendung, die das SMB-Volume verwendet. 
3. [Erstellen Sie eine bedarfsbasierte Momentaufnahme](azure-netapp-files-manage-snapshots.md#create-an-on-demand-snapshot-for-a-volume) des vorhandenen Volumes. 
4. Wählen Sie im vorhandenen Volume **Momentaufnahmen** aus, um die Liste der Momentaufnahmen anzuzeigen.
5. Klicken Sie mit der rechten Maustaste auf die Momentaufnahme, die Sie wiederherstellen möchten, und wählen Sie dann im Menü die Option **Auf neuem Volume wiederherstellen** aus.
    
    ![Momentaufnahme mit der Option „Auf neuem Volume wiederherstellen“.](../media/azure-netapp-files/azure-netapp-files-snapshot-restore-to-new-volume.png)

6. Das Fenster „Volume erstellen“ wird angezeigt. Geben Sie hier Informationen für das neue Volume an:  

    * **Volumename**    
    Geben Sie den Namen für das Volume an, das Sie erstellen möchten.   
    Der Name muss innerhalb einer Ressourcengruppe eindeutig sein. Er muss mindestens drei Zeichen lang sein. Er darf beliebige alphanumerische Zeichen enthalten.

    * **Kontingent**   
    Geben Sie die Menge an logischem Speicherplatz an, den Sie dem Volume zuordnen möchten.

    ![Momentaufnahme mit dem Fenster „Volume erstellen“.](../media/azure-netapp-files/snapshot-restore-new-volume.png) 

7. Stellen Sie im Fenster „Volume erstellen“ im Abschnitt „Protokoll“ sicher, dass die Option **Fortlaufende Verfügbarkeit aktivieren** ausgewählt ist.

    ![Momentaufnahme mit der Option „Fortlaufende Verfügbarkeit aktivieren“.](../media/azure-netapp-files/enable-continuous-availability-option.png) 

8. Klicken Sie auf **Überprüfen + erstellen**. Klicken Sie auf **Erstellen**.   
    Das neue Volume verwendet das gleiche Protokoll wie die Momentaufnahme.
   Das neue Volume, auf dem die Momentaufnahme wiederhergestellt wird, wird in der Ansicht „Volumes“ angezeigt.

9. Nachdem das neue Volume erstellt wurde, klicken Sie auf dem ausgewählten Volumeblatt auf **Bereitstellungsanweisungen**. Befolgen Sie dann die Anweisungen zum Bereitstellen des neuen Volumes.    

10. Konfigurieren Sie Ihre Anwendung neu, um den neuen Volume-Bereitstellungspunkt zu verwenden.   

11. Starten Sie die Anwendung neu, die Sie in Schritt 2 beendet haben. 

12. Wenn die Anwendung mit der Verwendung des neuen Volumes begonnen hat und die Wiederherstellung auf dem neuen Volume abgeschlossen ist, können Sie optional das ursprüngliche Volume löschen.  

## <a name="next-steps"></a>Nächste Schritte  

* [Erstellen eines SMB-Volumes für Azure NetApp Files](azure-netapp-files-create-volumes-smb.md)
* [Einbinden oder Aufheben der Einbindung eines Volumes auf virtuellen Windows- oder Linux-Computern](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md)