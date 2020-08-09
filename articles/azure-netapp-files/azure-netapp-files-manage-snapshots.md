---
title: Verwalten von Momentaufnahmen mithilfe von Azure NetApp Files | Microsoft-Dokumentation
description: Hier wird das Erstellen und Verwalten von Momentaufnahmen mithilfe von Azure NetApp Files beschrieben.
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
ms.date: 07/24/2020
ms.author: b-juche
ms.openlocfilehash: 271c3c9f63ee3f761826e214f3bf32a8df5f1cbe
ms.sourcegitcommit: 3d56d25d9cf9d3d42600db3e9364a5730e80fa4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/03/2020
ms.locfileid: "87533290"
---
# <a name="manage-snapshots-by-using-azure-netapp-files"></a>Verwalten von Momentaufnahmen mithilfe von Azure NetApp Files

Azure NetApp Files unterstützt das Erstellen bedarfsgesteuerter Momentaufnahmen und das Verwenden von Momentaufnahmenrichtlinien zum Planen der automatischen Erstellung von Momentaufnahmen.  Sie können eine Momentaufnahme auch für ein neues Volume wiederherstellen.  

## <a name="create-an-on-demand-snapshot-for-a-volume"></a>Erstellen einer Momentaufnahme bei Bedarf für ein Volume

Sie können bei Bedarf Volumemomentaufnahmen erstellen. 

1.  Wechseln Sie zu dem Volume, für das Sie eine Momentaufnahme erstellen möchten. Klicken Sie auf **Momentaufnahmen**.

    ![Zu Momentaufnahmen navigieren](../media/azure-netapp-files/azure-netapp-files-navigate-to-snapshots.png)

2.  Klicken Sie auf **+ Momentaufnahme hinzufügen**, um feine Momentaufnahme bei Bedarf für ein Volume zu erstellen.

    ![Momentaufnahme hinzufügen](../media/azure-netapp-files/azure-netapp-files-add-snapshot.png)

3.  Geben Sie im Fenster „Neue Momentaufnahme“ der neu erstellten Momentaufnahme einen Namen an.   

    ![Neue Momentaufnahme](../media/azure-netapp-files/azure-netapp-files-new-snapshot.png)

4. Klicken Sie auf **OK**. 

## <a name="manage-snapshot-policies"></a>Verwalten von Momentaufnahmenrichtlinien

Mithilfe von Momentaufnahmenrichtlinien können Sie planen, dass automatisch Volumemomentaufnahmen erstellt werden. Sie können eine Momentaufnahmenrichtlinie nach Bedarf auch ändern oder sogar löschen, wenn Sie sie nicht mehr benötigen.  

### <a name="register-the-feature"></a>Registrieren der Funktion

Die Funktion **Momentaufnahmenrichtlinie** steht derzeit als Vorschau zur Verfügung. Wenn Sie diese Funktion zum ersten Mal verwenden, müssen Sie sie zuerst registrieren. 

1. Registrieren Sie die Funktion: 

    ```azurepowershell-interactive
    Register-AzProviderFeature -ProviderNamespace Microsoft.NetApp -FeatureName ANFSnapshotPolicy
    ```

2. Überprüfen Sie den Status der Funktionsregistrierung: 

    > [!NOTE]
    > Der **RegistrationState** kann einige Minuten lang den Status `Registering` aufweisen, bevor der Wechsel in `Registered` erfolgt. Warten Sie, bis der Status **Registriert** lautet, bevor Sie fortfahren.

    ```azurepowershell-interactive
    Get-AzProviderFeature -ProviderNamespace Microsoft.NetApp -FeatureName ANFSnapshotPolicy
    ```

### <a name="create-a-snapshot-policy"></a>Erstellen einer Momentaufnahmenrichtlinie 

Eine Momentaufnahmenrichtlinie ermöglicht es Ihnen, die Häufigkeit der Erstellung von Momentaufnahmen in stündlichen, täglichen, wöchentlichen oder monatlichen Zyklen anzugeben. Außerdem müssen Sie die maximale Anzahl von Momentaufnahmen angeben, die für das Volume beibehalten werden soll.  

1.  Klicken Sie in der Ansicht für das NetApp-Konto auf **	Momentaufnahmenrichtlinie**.

    ![Momentaufnahmenrichtliniennavigation](../media/azure-netapp-files/snapshot-policy-navigation.png)

2.  Legen Sie die Option „Richtlinienstatus“ im Fenster „Momentaufnahmenrichtlinie“ auf **Aktiviert** fest. 

3.  Klicken Sie auf die Registerkarte **Stündlich**, **Täglich**, **Wöchentlich** oder **Monatlich**, um Richtlinien für stündliche, tägliche, wöchentliche oder monatliche Momentaufnahmen zu erstellen. Bestimmen Sie die **Anzahl von Momentaufnahmen, die beibehalten werden sollen**.  

    Informationen zur maximalen Anzahl von Momentaufnahmen, die für ein Volume zulässig sind, finden Sie unter [Ressourcenlimits für Azure NetApp Files](azure-netapp-files-resource-limits.md). 

    Das folgende Beispiel zeigt die Konfiguration der Richtlinie für stündliche Momentaufnahmen. 

    ![Richtlinie für stündliche Momentaufnahme](../media/azure-netapp-files/snapshot-policy-hourly.png)

    Das folgende Beispiel zeigt die Konfiguration der Richtlinie für tägliche Momentaufnahmen.

    ![Richtlinie für tägliche Momentaufnahme](../media/azure-netapp-files/snapshot-policy-daily.png)

    Das folgende Beispiel zeigt die Konfiguration der Richtlinie für wöchentliche Momentaufnahmen.

    ![Richtlinie für wöchentliche Momentaufnahme](../media/azure-netapp-files/snapshot-policy-weekly.png)

    Das folgende Beispiel zeigt die Konfiguration der Richtlinie für monatliche Momentaufnahmen.

    ![Richtlinie für monatliche Momentaufnahme](../media/azure-netapp-files/snapshot-policy-monthly.png) 

4.  Klicken Sie auf **Speichern**.  

Wiederholen Sie Schritt 3, wenn Sie zusätzliche Momentaufnahmenrichtlinien erstellen müssen.
Die von Ihnen erstellten Richtlinien werden auf der Seite „Momentaufnahmenrichtlinie“ angezeigt.

Wenn Sie möchten, dass die Richtlinie von einem Volume verwendet wird, müssen Sie [die Richtlinie auf das Volume anwenden](azure-netapp-files-manage-snapshots.md#apply-a-snapshot-policy-to-a-volume). 

### <a name="apply-a-snapshot-policy-to-a-volume"></a>Anwenden einer Momentaufnahmenrichtlinie auf ein Volume

Wenn Sie möchten, dass ein Volume eine von Ihnen erstellte Momentaufnahmenrichtlinie verwendet, müssen Sie die Richtlinie auf das Volume anwenden. 

1.  Navigieren Sie zur Seite **Volumes**, klicken Sie mit der rechten Maustaste auf das Volume, auf das Sie eine Momentaufnahmenrichtlinie anwenden möchten, und klicken Sie dann auf **Bearbeiten**.

    ![Rechtsklicken auf Volumes im Menü](../media/azure-netapp-files/volume-right-cick-menu.png) 

2.  Wählen Sie unter **Momentaufnahmenrichtlinie** im Fenster „Bearbeiten“ eine Richtlinie aus, die für das Volume verwendet werden soll.  Klicken Sie auf **OK**, um die Richtlinie anzuwenden.  

    ![Bearbeiten einer Momentaufnahmenrichtlinie](../media/azure-netapp-files/snapshot-policy-edit.png) 

### <a name="modify-a-snapshot-policy"></a>Ändern einer Momentaufnahmenrichtlinie 

Sie können eine vorhandene Momentaufnahmenrichtlinie ändern, um den Richtlinienstatus, die Häufigkeit der Momentaufnahmen (stündlich, täglich, wöchentlich oder monatlich) oder die Anzahl von Momentaufnahmen zu ändern, die aufbewahrt werden sollen.  
 
1.  Klicken Sie in der Ansicht für das NetApp-Konto auf **	Momentaufnahmenrichtlinie**.

2.  Klicken Sie mit der rechten Maustaste auf die Momentaufnahmenrichtlinie, die Sie ändern möchten, und klicken Sie dann auf **Bearbeiten**.

    ![Rechtsklicken für Menü zum Bearbeiten der Momentaufnahmenrichtlinie](../media/azure-netapp-files/snapshot-policy-right-click-menu.png) 

3.  Nehmen Sie die Änderungen im Fenster „Momentaufnahmenrichtlinie“ vor, das angezeigt wird, und klicken Sie dann auf **Speichern**. 

### <a name="delete-a-snapshot-policy"></a>Löschen einer Momentaufnahmenrichtlinie 

Sie können eine Momentaufnahmenrichtlinie löschen, die Sie nicht mehr beibehalten möchten.   

1.  Klicken Sie in der Ansicht für das NetApp-Konto auf **	Momentaufnahmenrichtlinie**.

2.  Klicken Sie mit der rechten Maustaste auf die Momentaufnahmenrichtlinie, die Sie ändern möchten, und klicken Sie dann auf **Löschen**.

    ![Rechtsklicken für Menü zum Bearbeiten der Momentaufnahmenrichtlinie](../media/azure-netapp-files/snapshot-policy-right-click-menu.png) 

3.  Klicken Sie auf **Ja**, um zu bestätigen, dass Sie die Momentaufnahmenrichtlinie löschen möchten.   

    ![Bestätigung zum Löschen der Momentaufnahmenrichtlinie](../media/azure-netapp-files/snapshot-policy-delete-confirm.png) 

## <a name="restore-a-snapshot-to-a-new-volume"></a>Wiederherstellen einer Momentaufnahme auf einem neuen Volume

Derzeit können Sie eine Momentaufnahme nur auf einem neuen Volume wiederherstellen. 
1. Klicken Sie im Bereich „Volume“ auf **Momentaufnahmen**, um die Liste der Momentaufnahmen anzuzeigen. 
2. Klicken Sie mit der rechten Maustaste auf die Momentaufnahme, die Sie wiederherstellen möchten, und klicken Sie dann im Menü auf die Option **Auf neuem Volume wiederherstellen**.  

    ![Wiederherstellen von Momentaufnahme auf neuem Volume](../media/azure-netapp-files/azure-netapp-files-snapshot-restore-to-new-volume.png)

3. Geben Sie im Fenster „Volume erstellen“ Informationen für das neue Volume an:  
    * **Name**   
        Geben Sie den Namen für das Volume an, das Sie erstellen möchten.  
        
        Der Name muss innerhalb einer Ressourcengruppe eindeutig sein. Er muss mindestens drei Zeichen lang sein.  Er darf beliebige alphanumerische Zeichen enthalten.

    * **Kontingent**  
        Geben Sie die Menge an logischem Speicherplatz an, den Sie dem Volume zuordnen möchten.  

    ![Wiederherstellen auf neuem Volume](../media/azure-netapp-files/snapshot-restore-new-volume.png) 

4. Klicken Sie auf **Überprüfen und erstellen**.  Klicken Sie auf **Erstellen**.   
    Das neue Volume verwendet das gleiche Protokoll wie die Momentaufnahme.   
    Das neue Volume, auf dem die Momentaufnahme wiederhergestellt wurde, wird auf dem Blatt „Volumes“ angezeigt.

## <a name="next-steps"></a>Nächste Schritte

* [Grundlegendes zur Speicherhierarchie von Azure NetApp Files](azure-netapp-files-understand-storage-hierarchy.md)
* [Ressourcenlimits für Azure NetApp Files](azure-netapp-files-resource-limits.md)
