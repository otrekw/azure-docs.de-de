---
title: Erstellen eines SMB-Volumes für Azure NetApp Files | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie ein SMB3-Volume in Azure NetApp Files erstellen. Sie erhalten Informationen zu den Anforderungen für Active Directory-Verbindungen und Active Directory Domain Services.
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
ms.date: 03/29/2021
ms.author: b-juche
ms.openlocfilehash: d386b504475b308c2fb5146b47d3977cb87510f8
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105935673"
---
# <a name="create-an-smb-volume-for-azure-netapp-files"></a>Erstellen eines SMB-Volumes für Azure NetApp Files

Azure NetApp Files unterstützt das Erstellen von Volumes mithilfe von NFS (NFSv3 und NFSv4.1), SMB3 oder einem dualen Protokoll (NFSv3 und SMB). Der Kapazitätsverbrauch eines Volumes wird mit der bereitgestellten Kapazität des dazugehörigen Pools verrechnet. In diesem Artikel wird veranschaulicht, wie Sie ein SMB3-Volume erstellen.

## <a name="before-you-begin"></a>Voraussetzungen 

* Sie müssen bereits einen Kapazitätspool eingerichtet haben. Siehe [Einrichten eines Kapazitätspools](azure-netapp-files-set-up-capacity-pool.md).     
* Ein Subnetz muss an Azure NetApp Files delegiert werden. Siehe [Delegieren eines Subnetzes an Azure NetApp Files](azure-netapp-files-delegate-subnet.md).

## <a name="configure-active-directory-connections"></a>Konfigurieren von Active Directory-Verbindungen 

Bevor Sie ein SMB-Volume erstellen, müssen Sie zunächst eine Active Directory-Verbindung erstellen. Wenn Sie keine Active Directory Verbindungen für Azure-NetApp-Dateien konfiguriert haben, befolgen Sie die Anweisungen unter [Erstellen und Verwalten von Active Directory-Verbindungen](create-active-directory-connections.md).

## <a name="add-an-smb-volume"></a>Hinzufügen eines SMB-Volumes

1. Klicken Sie auf dem Blatt „Kapazitätspools“ auf das Blatt **Volumes**. 

    ![Navigieren zu Volumes](../media/azure-netapp-files/azure-netapp-files-navigate-to-volumes.png)

2. Klicken Sie auf **+ Volume hinzufügen**, um ein Volume zu erstellen.  
    Das Fenster „Volume erstellen“ wird angezeigt.

3. Klicken Sie im Fenster „Volume erstellen“ auf **Erstellen**, und geben Sie auf der Registerkarte „Grundlagen“ Informationen in den folgenden Feldern an:   
    * **Volumename**      
        Geben Sie den Namen für das Volume an, das Sie erstellen möchten.   

        Ein Volumename muss innerhalb der einzelnen Kapazitätspools eindeutig sein. Er muss mindestens drei Zeichen lang sein. Sie dürfen beliebige alphanumerische Zeichen verwenden.   

        Sie können `default` oder `bin` nicht als Volumename verwenden.

    * **Kapazitätspool**  
        Geben Sie den Kapazitätspool an, in dem das Volume erstellt werden soll.

    * **Kontingent**  
        Geben Sie die Menge an logischem Speicherplatz an, die dem Volume zugewiesen wird.  

        Das Feld **Verfügbares Kontingent** zeigt den ungenutzten Speicherplatz im ausgewählten Kapazitätspool an, den Sie beim Erstellen eines neuen Volumes verwenden können. Die Größe des neuen Volumes darf das verfügbare Kontingent nicht überschreiten.  

    * **Durchsatz (MiB/s)**    
        Wenn das Volume in einem manuellen QoS-Kapazitätspool erstellt wird, geben Sie den für das Volume gewünschten Durchsatz an.   

        Wenn das Volume in einem automatischen QoS-Kapazitätspool erstellt wird, lautet der in diesem Feld angezeigte Wert (Kontingent x Serviceleveldurchsatz).   

    * **Virtuelles Netzwerk**  
        Geben Sie das virtuelle Azure-Netzwerk (VNET) an, von dem aus Sie auf das Volume zugreifen möchten.  

        Das von Ihnen angegebene VNET muss über ein an Azure NetApp Files delegiertes Subnetz verfügen. Auf den Azure NetApp Files-Dienst kann nur vom gleichen VNET aus oder per VNET-Peering von einem VNET aus zugegriffen werden, das sich in der gleichen Region befindet wie das Volume. Sie können auch über ExpressRoute von Ihrem lokalen Netzwerk aus auf das Volume zugreifen.   

    * **Subnetz**  
        Geben Sie das Subnetz an, das Sie für das Volume verwenden möchten.  
        Das von Ihnen angegebene Subnetz muss an Azure NetApp Files delegiert werden. 
        
        Wenn Sie kein Subnetz delegiert haben, klicken Sie auf der Seite „Volume erstellen“ auf **Neu erstellen**. Geben Sie dann auf der Seite „Subnetz erstellen“ die Subnetzinformationen an, und wählen Sie **Microsoft.NetApp/volumes** aus, um das Subnetz für Azure NetApp Files zu delegieren. In jedem VNET kann nur ein Subnetz an Azure NetApp Files delegiert werden.   
 
        ![Erstellen eines Volumes](../media/azure-netapp-files/azure-netapp-files-new-volume.png)
    
        ![Erstellen eines Subnetzes](../media/azure-netapp-files/azure-netapp-files-create-subnet.png)

    * Wenn Sie eine vorhandene Momentaufnahmerichtlinie auf das Volume anwenden möchten, klicken Sie auf **Abschnitt „Erweitert“ anzeigen**, um den Bereich zu erweitern, geben Sie an, ob Sie den Momentaufnahmepfad ausblenden möchten, und wählen Sie im Pulldownmenü eine Momentaufnahmerichtlinie aus. 

        Informationen zum Erstellen einer Momentaufnahmenrichtlinie finden Sie unter [Verwalten von Momentaufnahmenrichtlinien](azure-netapp-files-manage-snapshots.md#manage-snapshot-policies).

        ![Abschnitt „Erweitert“ anzeigen](../media/azure-netapp-files/volume-create-advanced-selection.png)

4. Klicken Sie auf **Protokoll**, und geben Sie die folgenden Informationen an:  
    * Wählen Sie **SMB** als Protokolltyp für das Volume aus. 
    * Wählen Sie in der Dropdownliste Ihre **Active Directory**-Verbindung aus.
    * Geben Sie in **Freigabename** den Namen des freigegebenen Volumes ein.
    * Wenn Sie die fortlaufende Verfügbarkeit für das SMB-Volume aktivieren möchten, wählen Sie **Fortlaufende Verfügbarkeit aktivieren** aus.    

        > [!IMPORTANT]   
        > Das SMB-Feature „Fortlaufende Verfügbarkeit“ ist derzeit als öffentliche Vorschauversion verfügbar. Sie müssen eine Wartelistenanforderung für den Zugriff auf das Feature über die Seite **[Azure NetApp Files SMB Continuous Availability Shares Public Preview](https://aka.ms/anfsmbcasharespreviewsignup)** übermitteln. Warten Sie auf eine offizielle Bestätigungs-E-Mail des Azure NetApp Files-Teams, bevor Sie das Feature für die fortlaufende Verfügbarkeit verwenden.   
        > 
        > Sie sollten die fortlaufende Verfügbarkeit nur für SQL-Workloads aktivieren. Die Verwendung von SMB-Freigaben der fortlaufenden Verfügbarkeit für andere Workloads als SQL Server wird *nicht* unterstützt. Dieses Feature wird derzeit für SQL Server unter Windows unterstützt. SQL Server unter Linux wird zurzeit nicht unterstützt. Wenn Sie für die Installation von SQL Server ein Nicht-Administratorkonto bzw. ein Nicht-Administratordomänenkonto verwenden, stellen Sie sicher, dass dem Konto die erforderlichen Sicherheitsberechtigungen zugewiesen sind. Wenn das Domänenkonto nicht über die erforderliche Sicherheitsberechtigung (`SeSecurityPrivilege`) verfügt und die Berechtigung nicht auf Domänenebene festgelegt werden kann, können Sie dem Konto die Berechtigung über das Feld **Benutzer mit Sicherheitsberechtigungen** der Active Directory-Verbindungen zuweisen. Weitere Informationen finden Sie unter [Erstellen einer Active Directory Domain Services-Verbindung](create-active-directory-connections.md#create-an-active-directory-connection).

    <!-- [1/13/21] Commenting out command-based steps below, because the plan is to use form-based (URL) registration, similar to CRR feature registration -->
    <!-- 
        ```azurepowershell-interactive
        Register-AzProviderFeature -ProviderNamespace Microsoft.NetApp -FeatureName ANFSMBCAShare
        ```

        Check the status of the feature registration: 

        > [!NOTE]
        > The **RegistrationState** may be in the `Registering` state for up to 60 minutes before changing to`Registered`. Wait until the status is `Registered` before continuing.

        ```azurepowershell-interactive
        Get-AzProviderFeature -ProviderNamespace Microsoft.NetApp -FeatureName ANFSMBCAShare
        ```
        
        You can also use [Azure CLI commands](/cli/azure/feature?preserve-view=true&view=azure-cli-latest) `az feature register` and `az feature show` to register the feature and display the registration status. 
    --> 

    ![Screenshot, der die Registerkarte „Protokoll“ für die Erstellung eines SMB-Volumes zeigt.](../media/azure-netapp-files/azure-netapp-files-protocol-smb.png)

5. Klicken Sie auf **Bewerten + erstellen**, um die Volumedetails zu überprüfen.  Klicken Sie dann auf **Erstellen**, um das SMB-Volume zu erstellen.

    Das von Ihnen erstellte Volume wird auf der Seite „Volumes“ angezeigt. 
 
    Ein Volume erbt Abonnement-, Ressourcengruppen- und Standortattribute aus dem Kapazitätspool. Den Volumebereitstellungsstatus können Sie auf der Benachrichtigungsregisterkarte überwachen.

## <a name="control-access-to-an-smb-volume"></a>Steuern des Zugriffs auf ein SMB-Volume  

Der Zugriff auf ein SMB-Volume wird durch Berechtigungen verwaltet.  

### <a name="share-permissions"></a>Freigabeberechtigungen  

Ein neues Volume verfügt standardmäßig über die Freigabeberechtigungen **Jeder/Vollzugriff**. Mitglieder der Gruppe „Domänenadministratoren“ können die Freigabeberechtigungen wie folgt ändern:  

1. Die Freigabe einem Laufwerk zuordnen.  
2. Klicken Sie mit der rechten Maustaste auf das Laufwerk, wählen Sie **Eigenschaften** aus, und wechseln Sie dann zur Registerkarte **Sicherheit**.

[![Freigabeberechtigungen festlegen](../media/azure-netapp-files/set-share-permissions.png)](../media/azure-netapp-files/set-share-permissions.png#lightbox)

### <a name="ntfs-file-and-folder-permissions"></a>NTFS-Datei- und -Ordnerberechtigungen  

Sie können Berechtigungen für eine Datei oder einen Ordner festlegen, indem Sie die Registerkarte **Sicherheit** in den Eigenschaften des Objekts im Windows-SMB-Client verwenden.
 
![Festlegen von Datei- und Ordnerberechtigungen](../media/azure-netapp-files/set-file-folder-permissions.png) 

## <a name="next-steps"></a>Nächste Schritte  

* [Einbinden oder Aufheben der Einbindung eines Volumes auf virtuellen Windows- oder Linux-Computern](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md)
* [Ressourcenlimits für Azure NetApp Files](azure-netapp-files-resource-limits.md)
* [Häufig gestellte Fragen zu SMB](./azure-netapp-files-faqs.md#smb-faqs)
* [Problembehandlung für SMB-Volumes und Volumes mit dualem Protokoll](troubleshoot-dual-protocol-volumes.md)
* [Erfahren Sie mehr über die Integration virtueller Netzwerke für Azure-Dienste](../virtual-network/virtual-network-for-azure-services.md)
* [Installieren einer neuen Active Directory-Gesamtstruktur mit der Azure CLI](/windows-server/identity/ad-ds/deploy/virtual-dc/adds-on-azure-vm)
