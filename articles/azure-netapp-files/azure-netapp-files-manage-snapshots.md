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
ms.date: 09/04/2020
ms.author: b-juche
ms.openlocfilehash: e9f2a1f9125d25caa9506e954cab3b94dfcb5c24
ms.sourcegitcommit: 50802bffd56155f3b01bfb4ed009b70045131750
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91932276"
---
# <a name="manage-snapshots-by-using-azure-netapp-files"></a>Verwalten von Momentaufnahmen mithilfe von Azure NetApp Files

Azure NetApp Files unterstützt das Erstellen bedarfsgesteuerter Momentaufnahmen und das Verwenden von Momentaufnahmenrichtlinien zum Planen der automatischen Erstellung von Momentaufnahmen.  Sie können auch eine Momentaufnahme auf einem neuen Volume wiederherstellen oder eine einzelne Datei mithilfe eines Clients wiederherstellen.  

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
    > Der **RegistrationState** kann für bis zu 60 Minuten den Status `Registering` aufweisen, bevor der Wechsel in `Registered` erfolgt. Warten Sie, bis der Status **Registriert** lautet, bevor Sie fortfahren.

    ```azurepowershell-interactive
    Get-AzProviderFeature -ProviderNamespace Microsoft.NetApp -FeatureName ANFSnapshotPolicy
    ```
Sie können auch die [Azure CLI-Befehle](/cli/azure/feature?preserve-view=true&view=azure-cli-latest) `az feature register` und `az feature show` verwenden, um das Feature zu registrieren und den Registrierungsstatus anzuzeigen. 

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

2.  Klicken Sie mit der rechten Maustaste auf die Momentaufnahmenrichtlinie, die Sie ändern möchten, und wählen Sie dann **Bearbeiten** aus.

    ![Rechtsklicken für Menü zum Bearbeiten der Momentaufnahmenrichtlinie](../media/azure-netapp-files/snapshot-policy-right-click-menu.png) 

3.  Nehmen Sie die Änderungen im Fenster „Momentaufnahmenrichtlinie“ vor, das angezeigt wird, und klicken Sie dann auf **Speichern**. 

### <a name="delete-a-snapshot-policy"></a>Löschen einer Momentaufnahmenrichtlinie 

Sie können eine Momentaufnahmenrichtlinie löschen, die Sie nicht mehr beibehalten möchten.   

1.  Klicken Sie in der Ansicht für das NetApp-Konto auf **	Momentaufnahmenrichtlinie**.

2.  Klicken Sie mit der rechten Maustaste auf die Momentaufnahmenrichtlinie, die Sie ändern möchten, und wählen Sie dann **Löschen** aus.

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

## <a name="restore-a-file-from-a-snapshot-using-a-client"></a>Wiederherstellen einer Datei aus einer Momentaufnahme mithilfe eines Clients

Wenn Sie nicht [die gesamte Momentaufnahme auf einem Volume wiederherstellen](#restore-a-snapshot-to-a-new-volume) möchten, haben Sie die Möglichkeit, eine Datei aus einer Momentaufnahme wiederherzustellen, und zwar mithilfe eines Clients, der das Volume eingebunden hat.  

Das eingebundene Volume enthält ein Momentaufnahmeverzeichnis namens `.snapshot` (in NFS-Clients) oder `~snapshot` (bei SMB-Clients), auf das der Client Zugriff hat. Das Momentaufnahmeverzeichnis enthält Unterverzeichnisse, die den Momentaufnahmen des Volumes entsprechen. Jedes Unterverzeichnis enthält die Dateien der Momentaufnahme. Wenn Sie versehentlich eine Datei löschen oder überschreiben, können Sie die Datei im übergeordneten Lese-/Schreibverzeichnis wiederherstellen, indem Sie die Datei aus einem Unterverzeichnis der Momentaufnahme in das Lese-/Schreibverzeichnis kopieren. 

Wenn Sie das beim Erstellen des Volumes Kontrollkästchen „Momentaufnahmepfad ausblenden“ aktiviert haben, wird das Momentaufnahmeverzeichnis ausgeblendet. Sie können den Status für „Momentaufnahmepfad ausblenden“ des Volumes einsehen, indem Sie das Volume auswählen. Sie können die Option „Momentaufnahmepfad ausblenden“ bearbeiten, indem Sie auf der Seite des Volumes auf **Bearbeiten** klicken.  

![Optionen zum Bearbeiten von Volumemomentaufnahmen](../media/azure-netapp-files/volume-edit-snapshot-options.png) 

### <a name="restore-a-file-by-using-a-linux-nfs-client"></a>Wiederherstellen einer Datei mithilfe eines Linux-NFS-Clients 

1. Führen Sie den Linux-Befehl `ls` aus, um die Datei aufzulisten, die Sie aus dem Verzeichnis `.snapshot` wiederherstellen möchten. 

    Beispiel:

    `$ ls my.txt`   
    `ls: my.txt: No such file or directory`   

    `$ ls .snapshot`   
    `daily.2020-05-14_0013/              hourly.2020-05-15_1106/`   
    `daily.2020-05-15_0012/              hourly.2020-05-15_1206/`   
    `hourly.2020-05-15_1006/             hourly.2020-05-15_1306/`   

    `$ ls .snapshot/hourly.2020-05-15_1306/my.txt`   
    `my.txt`

2. Führen Sie den Befehl `cp` aus, um die Datei in das übergeordnete Verzeichnis zu kopieren.  

    Beispiel: 

    `$ cp .snapshot/hourly.2020-05-15_1306/my.txt .`   

    `$ ls my.txt`   
    `my.txt`   

### <a name="restore-a-file-by-using-a-windows-client"></a>Wiederherstellen einer Datei mithilfe eines Windows-Clients 

1. Wenn das Verzeichnis `~snapshot` des Volumes ausgeblendet ist, [blenden Sie ausgeblendete Elemente](https://support.microsoft.com/help/4028316/windows-view-hidden-files-and-folders-in-windows-10) im übergeordneten Verzeichnis ein, um `~snapshot` anzuzeigen.

    ![Ausgeblendete Elemente anzeigen](../media/azure-netapp-files/snapshot-show-hidden.png) 

2. Navigieren Sie zum Unterverzeichnis innerhalb von `~snapshot`, um die Datei zu finden, die Sie wiederherstellen möchten.  Klicken Sie mit der rechten Maustaste auf die Datei. Wählen Sie **Kopieren** aus.  

    ![Kopieren der wiederherzustellenden Datei](../media/azure-netapp-files/snapshot-copy-file-restore.png) 

3. Kehren Sie zum übergeordneten Verzeichnis zurück. Klicken Sie mit der rechten Maustaste in das übergeordnete Verzeichnis, und wählen Sie `Paste` aus, um die Datei in das Verzeichnis einzufügen.

    ![Einfügen der wiederherzustellenden Datei](../media/azure-netapp-files/snapshot-paste-file-restore.png) 

4. Sie können auch mit der rechten Maustaste auf das übergeordnete Verzeichnis klicken, **Eigenschaften** auswählen, auf die Registerkarte **Vorherige Versionen** klicken, um die Liste der Momentaufnahmen anzuzeigen, und **Wiederherstellen** auswählen, um eine Datei wiederherzustellen.  

    ![Eigenschaften vorheriger Versionen](../media/azure-netapp-files/snapshot-properties-previous-version.png) 

## <a name="next-steps"></a>Nächste Schritte

* [Problembehandlung für Momentaufnahmenrichtlinien](troubleshoot-snapshot-policies.md)
* [Ressourcenlimits für Azure NetApp Files](azure-netapp-files-resource-limits.md)
* [Video „Azure NetApp Files Snapshots 101“](https://www.youtube.com/watch?v=uxbTXhtXCkw&feature=youtu.be)