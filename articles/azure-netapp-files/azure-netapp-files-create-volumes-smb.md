---
title: Erstellen eines SMB-Volumes für Azure NetApp Files | Microsoft-Dokumentation
description: Beschreibt die Vorgehensweise zum Erstellen eines SMB-Volumes für Azure NetApp Files.
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
ms.date: 04/03/2020
ms.author: b-juche
ms.openlocfilehash: c4e7566eeb28bc5709acd60ced9fcdffb7e8a725
ms.sourcegitcommit: 67addb783644bafce5713e3ed10b7599a1d5c151
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/05/2020
ms.locfileid: "80668003"
---
# <a name="create-an-smb-volume-for-azure-netapp-files"></a>Erstellen eines SMB-Volumes für Azure NetApp Files

Azure NetApp Files unterstützt NFS- und SMBv3-Volumes. Der Kapazitätsverbrauch eines Volumes wird mit der bereitgestellten Kapazität des dazugehörigen Pools verrechnet. In diesem Artikel wird veranschaulicht, wie Sie ein SMB v3-Volume erstellen. Wenn Sie ein NFS-Volume erstellen möchten, lesen Sie unter [Erstellen eines NFS-Volumes für Azure NetApp Files](azure-netapp-files-create-volumes.md) nach. 

## <a name="before-you-begin"></a>Voraussetzungen 
Sie müssen bereits einen Kapazitätspool eingerichtet haben.   
[Einrichten eines Kapazitätspools](azure-netapp-files-set-up-capacity-pool.md)   
Ein Subnetz muss an Azure NetApp Files delegiert werden.  
[Delegieren eines Subnetzes für Azure NetApp Files](azure-netapp-files-delegate-subnet.md)

## <a name="requirements-for-active-directory-connections"></a>Anforderungen an Active Directory-Verbindungen

 Bevor Sie ein SMB-Volume erstellen, müssen Sie zunächst Active Directory-Verbindungen erstellen. Die Anforderungen für Active Directory-Verbindungen lauten wie folgt: 

* Es muss mit dem verwendeten Administratorkonto möglich sein, Computerkonten im angegebenen Pfad der Organisationseinheit (OU) zu erstellen.  

* Auf dem entsprechenden Windows Active Directory-Server (AD) müssen die richtigen Ports geöffnet sein.  
    Die erforderlichen Ports lauten wie folgt: 

    |     Dienst           |     Port     |     Protocol     |
    |-----------------------|--------------|------------------|
    |    AD-Webdienste    |    9389      |    TCP           |
    |    DNS                |    53        |    TCP           |
    |    DNS                |    53        |    UDP           |
    |    ICMPv4             |    –       |    Echo Reply    |
    |    Kerberos           |    464       |    TCP           |
    |    Kerberos           |    464       |    UDP           |
    |    Kerberos           |    88        |    TCP           |
    |    Kerberos           |    88        |    UDP           |
    |    LDAP               |    389       |    TCP           |
    |    LDAP               |    389       |    UDP           |
    |    LDAP               |    3268      |    TCP           |
    |    NetBIOS-Name       |    138       |    UDP           |
    |    SAM/LSA            |    445       |    TCP           |
    |    SAM/LSA            |    445       |    UDP           |
    |    w32time            |    123       |    UDP           |

* Die Standorttopologie für die Ziel-Active Directory Domain Services muss den bewährten Methoden entsprechen. Dies gilt insbesondere für das Azure-VNET, in dem Azure NetApp Files bereitgestellt wird.  

    Der Adressraum für das virtuelle Netzwerk, in dem Azure NetApp Files bereitgestellt wird, muss einem neuen oder vorhandenen Active Directory-Standort hinzugefügt werden (in dem sich ein Domänencontroller befindet, der über Azure NetApp Files erreichbar ist). 

* Die angegebenen DNS-Server müssen über das [delegierte Subnetz](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-delegate-subnet) von Azure NetApp Files erreichbar sein.  

    Informationen zu unterstützten Netzwerktopologien finden Sie unter [Richtlinien für die Azure NetApp Files-Netzwerkplanung](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-network-topologies).

    Die Netzwerksicherheitsgruppen (NSGs) und Firewalls müssen ordnungsgemäß konfigurierte Regeln aufweisen, um Active Directory- und DNS-Datenverkehrsanforderungen zuzulassen. 

* Das delegierte Azure NetApp Files-Subnetz muss in der Lage sein, alle Active Directory Domain Services-Domänencontroller (ADDS) in der Domäne zu erreichen, einschließlich aller lokalen und Remotedomänencontroller. Andernfalls kann es zu Dienstunterbrechungen kommen.  

    Wenn Sie über Domänencontroller verfügen, die über das delegierte Azure NetApp Files-Subnetz nicht erreichbar sind, können Sie während der Erstellung der Active Directory-Verbindung einen Active Directory-Standort angeben.  Azure NetApp Files muss nur mit Domänencontrollern an dem Standort kommunizieren, an dem sich der Adressraum des delegierten Azure NetApp Files-Subnetzes befindet.

    Weitere Informationen zu AD-Standorten und -Diensten finden Sie unter [Entwerfen der Standorttopologie](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/designing-the-site-topology). 
    
<!--
* Azure NetApp Files supports DES, Kerberos AES 128, and Kerberos AES 256 encryption types (from the least secure to the most secure). The user credentials used to join Active Directory must have the highest corresponding account option enabled that matches the capabilities enabled for your Active Directory.   

    For example, if your Active Directory has only the AES-128 capability, you must enable the AES-128 account option for the user credentials. If your Active Directory has the AES-256 capability, you must enable the AES-256 account option (which also supports AES-128). If your Active Directory does not have any Kerberos encryption capability, Azure NetApp Files uses DES by default.  

    You can enable the account options in the properties of the Active Directory Users and Computers MMC console:   

    ![Active Directory Users and Computers MMC](../media/azure-netapp-files/ad-users-computers-mmc.png)
-->

Weitere Informationen zu zusätzlichen AD-Informationen finden Sie in den [häufig gestellten Fragen zu SMB](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-faqs#smb-faqs) für Azure NetApp Files. 

## <a name="decide-which-domain-services-to-use"></a>Festlegen der zu verwendenden Domänendienste 

Azure NetApp Files unterstützt sowohl [Active Directory Domain Services](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/understanding-active-directory-site-topology) (ADDS) als auch Azure Active Directory Domain Services (AADDS) für AD-Verbindungen.  Vor der Erstellung einer AD-Verbindung müssen Sie festlegen, ob ADDS oder AADDS verwendet werden soll.  

Weitere Informationen finden Sie unter [Vergleichen von selbstverwalteten Active Directory Domain Services, Azure Active Directory und verwalteten Azure Active Directory Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/compare-identity-solutions). 

### <a name="active-directory-domain-services"></a>Active Directory Domain Services

Sie können den Bereich Ihrer bevorzugten [Active Directory-Standorte und -Dienste](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/understanding-active-directory-site-topology) für Azure NetApp Files verwenden. Mit dieser Option werden Lese- und Schreibvorgänge für Active Directory Domain Services-Domänencontroller (ADDS) ermöglicht, auf die über [Azure NetApp Files](azure-netapp-files-network-topologies.md) zugegriffen werden kann. Außerdem wird verhindert, dass der Dienst mit Domänencontrollern kommuniziert, die sich nicht am angegebenen Standort für Active Directory-Standorte und -Dienste befinden. 

Um bei Verwendung von ADDS den Standortnamen zu ermitteln, können Sie sich an die administrative Gruppe Ihrer Organisation wenden, die für Active Directory Domain Services zuständig ist. Im folgenden Beispiel wird das Plug-In „Active Directory-Standorte und -Dienste“ gezeigt, in dem der Standortname angezeigt wird: 

![Active Directory-Standorte und -Dienste](../media/azure-netapp-files/azure-netapp-files-active-directory-sites-and-services.png)

Wenn Sie eine AD-Verbindung für Azure NetApp Files konfigurieren, geben Sie den Standortnamen im Bereich für das Feld **Active Directory-Standortname** an.

### <a name="azure-active-directory-domain-services"></a>Azure Active Directory Domain Services 

Informationen zur Konfiguration und Anleitungen für Azure Active Directory Domain Services (AADDS) finden Sie in der [Dokumentation zu Azure AD Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/).

Weitere Überlegungen zu Azure NetApp Files im Hinblick auf AADDS: 

* Stellen Sie sicher, dass sich das VNET oder Subnetz, in dem AADDS bereitgestellt wird, in der gleichen Azure-Region wie die Azure NetApp Files-Bereitstellung befindet.
* Wenn Sie ein anderes VNET in der Region verwenden, in der Azure NetApp Files bereitgestellt wird, sollten Sie ein Peering zwischen den beiden VNETs erstellen.
* Azure NetApp Files unterstützt die Typen `user` und `resource forest`.
* Für den Synchronisierungstyp können Sie `All` oder `Scoped` auswählen.   
    Wenn Sie `Scoped` auswählen, stellen Sie sicher, dass die richtige Azure AD-Gruppe für den Zugriff auf SMB-Freigaben ausgewählt ist.  Wenn Sie unsicher sind, können Sie den Synchronisierungstyp `All` verwenden.
* Es muss die Enterprise- oder Premium-SKU verwendet werden. Die Standard-SKU wird nicht unterstützt.

Beachten Sie beim Erstellen einer Active Directory-Verbindung die folgenden Besonderheiten für AADDS:

* Im Menü „AADDS“ finden Sie Informationen zu **Primäres DNS**, **Sekundäres DNS** und **AD-DNS-Domänenname**.  
Für DNS-Server werden zwei IP-Adressen für die Konfiguration der Active Directory-Verbindung verwendet. 
* Der **Pfad der Organisationseinheit** lautet `OU=AADDC Computers`.  
Diese Einstellung wird in **Active Directory-Verbindungen** unter **NetApp-Konto** konfiguriert:

  ![Pfad der Organisationseinheit](../media/azure-netapp-files/azure-netapp-files-org-unit-path.png)

* Als Anmeldeinformationen kann für **Benutzername** ein beliebiger Benutzer angegeben werden, der Mitglied der Azure AD-Gruppe **Azure AD DC-Administratoren** ist.


## <a name="create-an-active-directory-connection"></a>Erstellen einer Active Directory-Verbindung

1. Klicken Sie über Ihr NetApp-Konto auf **Active Directory-Verbindungen**, und klicken Sie dann auf **Verbinden**.  

    ![Active Directory-Verbindungen](../media/azure-netapp-files/azure-netapp-files-active-directory-connections.png)

2. Geben Sie im Fenster „Active Directory beitreten“ basierend auf den zu verwendenden Domänendiensten die folgenden Informationen an:  

    Spezifische Informationen zu den verwendeten Domänendiensten finden Sie unter [Festlegen der zu verwendenden Domänendienste](#decide-which-domain-services-to-use). 

    * **Primäres DNS**  
        Das DNS, das für den Active Directory-Domänenbeitritt und SMB-Authentifizierungsvorgänge erforderlich ist 
    * **Sekundäres DNS**   
        Der sekundäre DNS-Server für das Sicherstellen redundanter Namensdienste 
    * **AD-DNS-Domänenname**  
        Dies ist der Domänenname Ihrer Active Directory Domain Services, denen Sie beitreten möchten.
    * **AD-Standortname**  
        Dies ist der Name des Standorts, auf den die Domänencontrollerermittlung beschränkt wird.
    * **Präfix des SMB-Servers (Computerkonto)**  
        Dies ist das Namenspräfix für das Computerkonto in Active Directory, das Azure NetApp Files für die Erstellung von neuen Konten verwendet.

        Wenn beispielsweise in Ihrer Organisation der Benennungsstandard für Dateiserver „NAS-01“, „NAS-02“ … „NAS-045“ verwendet wird, geben Sie „NAS“ als Präfix ein. 

        Der Dienst erstellt dann bei Bedarf zusätzliche Computerkonten in Active Directory.

    * **Pfad der Organisationseinheit**  
        Dies ist der LDAP-Pfad für die Organisationseinheit, in der Computerkonten für SMB-Server erstellt werden. Das bedeutet, Organisationseinheit = zweite Ebene, Organisationseinheit = erste Ebene. 

        Bei Verwendung von Azure NetApp Files mit Azure Active Directory Domain Services lautet der Pfad der Organisationseinheit `OU=AADDC Computers`, wenn Sie Active Directory für Ihr NetApp-Konto konfigurieren.
        
    * Anmeldeinformationen, einschließlich **Benutzername** und **Kennwort**

    ![Beitreten zu Active Directory](../media/azure-netapp-files/azure-netapp-files-join-active-directory.png)

3. Klicken Sie auf **Verknüpfen**.  

    Die von Ihnen erstellte Active Directory-Verbindung wird angezeigt.

    ![Active Directory-Verbindungen](../media/azure-netapp-files/azure-netapp-files-active-directory-connections-created.png)

> [!NOTE] 
> Die Felder für Benutzername und Kennwort können nach dem Speichern der Active Directory-Verbindung bearbeitet werden. Andere Werte können nach dem Speichern der Verbindung nicht mehr bearbeitet werden. Wenn Sie andere Werte ändern möchten, müssen Sie zuerst alle bereitgestellten SMB-Volumes löschen und anschließend die Active Directory-Verbindung löschen und neu erstellen.

## <a name="add-an-smb-volume"></a>Hinzufügen eines SMB-Volumes

1. Klicken Sie auf dem Blatt „Kapazitätspools“ auf das Blatt **Volumes**. 

    ![Navigieren zu Volumes](../media/azure-netapp-files/azure-netapp-files-navigate-to-volumes.png)

2. Klicken Sie auf **+ Volume hinzufügen**, um ein Volume zu erstellen.  
    Das Fenster „Volume erstellen“ wird angezeigt.

3. Klicken Sie im Fenster „Volume erstellen“ auf **Erstellen**, und geben Sie Informationen für die folgenden Felder an:   
    * **Volumename**      
        Geben Sie den Namen für das Volume an, das Sie erstellen möchten.   

        Ein Volumename muss innerhalb der einzelnen Kapazitätspools eindeutig sein. Er muss mindestens drei Zeichen lang sein. Sie dürfen beliebige alphanumerische Zeichen verwenden.   

        `default` kann nicht als Volumename verwendet werden.

    * **Kapazitätspool**  
        Geben Sie den Kapazitätspool an, in dem das Volume erstellt werden soll.

    * **Kontingent**  
        Geben Sie die Menge an logischem Speicherplatz an, die dem Volume zugewiesen wird.  

        Das Feld **Verfügbares Kontingent** zeigt den ungenutzten Speicherplatz im ausgewählten Kapazitätspool an, den Sie beim Erstellen eines neuen Volumes verwenden können. Die Größe des neuen Volumes darf das verfügbare Kontingent nicht überschreiten.  

    * **Virtuelles Netzwerk**  
        Geben Sie das virtuelle Azure-Netzwerk (VNET) an, von dem aus Sie auf das Volume zugreifen möchten.  

        Das von Ihnen angegebene VNET muss über ein an Azure NetApp Files delegiertes Subnetz verfügen. Auf den Azure NetApp Files-Dienst kann nur vom gleichen VNET aus oder per VNET-Peering von einem VNET aus zugegriffen werden, das sich in der gleichen Region befindet wie das Volume. Sie können auch über ExpressRoute von Ihrem lokalen Netzwerk aus auf das Volume zugreifen.   

    * **Subnetz**  
        Geben Sie das Subnetz an, das Sie für das Volume verwenden möchten.  
        Das von Ihnen angegebene Subnetz muss an Azure NetApp Files delegiert werden. 
        
        Wenn Sie kein Subnetz delegiert haben, klicken Sie auf der Seite „Volume erstellen“ auf **Neu erstellen**. Geben Sie dann auf der Seite „Subnetz erstellen“ die Subnetzinformationen an, und wählen Sie **Microsoft.NetApp/volumes** aus, um das Subnetz für Azure NetApp Files zu delegieren. In jedem VNET kann nur ein Subnetz an Azure NetApp Files delegiert werden.   
 
        ![Erstellen eines Volumes](../media/azure-netapp-files/azure-netapp-files-new-volume.png)
    
        ![Erstellen eines Subnetzes](../media/azure-netapp-files/azure-netapp-files-create-subnet.png)

4. Klicken Sie auf **Protokoll**, und geben Sie die folgenden Informationen an:  
    * Wählen Sie **SMB** als Protokolltyp für das Volume aus. 
    * Wählen Sie in der Dropdownliste Ihre **Active Directory**-Verbindung aus.
    * Geben Sie in **Freigabename** den Namen des freigegebenen Volumes ein.

    ![Angeben des SMB-Protokolls](../media/azure-netapp-files/azure-netapp-files-protocol-smb.png)

5. Klicken Sie auf **Bewerten + erstellen**, um die Volumedetails zu überprüfen.  Klicken Sie dann auf **Erstellen**, um das SMB-Volume zu erstellen.

    Das von Ihnen erstellte Volume wird auf der Seite „Volumes“ angezeigt. 
 
    Ein Volume erbt Abonnement-, Ressourcengruppen- und Standortattribute aus dem Kapazitätspool. Den Volumebereitstellungsstatus können Sie auf der Benachrichtigungsregisterkarte überwachen.

## <a name="next-steps"></a>Nächste Schritte  

* [Einbinden oder Aufheben der Einbindung eines Volumes auf virtuellen Windows- oder Linux-Computern](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md)
* [Ressourcenlimits für Azure NetApp Files](azure-netapp-files-resource-limits.md)
* [Häufig gestellte Fragen zu SMB](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-faqs#smb-faqs)
* [Erfahren Sie mehr über die Integration virtueller Netzwerke für Azure-Dienste](https://docs.microsoft.com/azure/virtual-network/virtual-network-for-azure-services)
* [Installieren einer neuen Active Directory-Gesamtstruktur mit der Azure CLI](https://docs.microsoft.com/windows-server/identity/ad-ds/deploy/virtual-dc/adds-on-azure-vm)
