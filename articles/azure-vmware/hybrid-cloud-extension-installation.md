---
title: Installieren von Hybrid Cloud Extension (HCX)
description: Einrichten der VMware-HCX-Lösung (Hybrid Cloud Extension) für die private AVS-Cloud (Azure VMware Solution)
ms.topic: how-to
ms.date: 05/19/2020
ms.openlocfilehash: dc5f7f82b83c82538b2d5a7b4c87131afb3fcc20
ms.sourcegitcommit: 64fc70f6c145e14d605db0c2a0f407b72401f5eb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2020
ms.locfileid: "83873649"
---
# <a name="install-hcx-for-azure-vmware-solution"></a>Installieren von HCX für eine Azure-VMware-Lösung

In diesem Artikel werden die Vorgehensweisen zum Einrichten der VMware-HCX-Lösung (Hybrid Cloud Extension) für Ihre private AVS-Cloud (Azure VMware Solution) beschrieben. HCX Advanced (Standardinstallation) unterstützt bis zu drei externe Standorte. Dabei muss für jeden externen Standort ein HCX Enterprise-Manager oder -Connector installiert und aktiviert sein.
HCX ermöglicht die Migration Ihrer VMware-Workloads in die Cloud und zu anderen verbundenen Standorten mithilfe verschiedener integrierter, von HCX unterstützter Migrationstypen. Wenn mehr als drei Standorte erforderlich sind, können die Kunden das HCX Enterprise-Add-on über den Support aktivieren. Für HCX Enterprise fallen über die allgemeine Verfügbarkeit (GA) hinaus zusätzliche Gebühren für Kunden an, die Version bietet jedoch [zusätzliche Funktionen](https://cloud.vmware.com/community/2019/08/08/introducing-hcx-enterprise/).

Lesen Sie zuerst die Informationen unter [Voraussetzungen (Vorbereitung)](#before-you-begin), [Erforderliche Softwareversionen](#software-version-requirements) und [Voraussetzungen](#prerequisites). 

Danach werden die folgenden Aufgaben beschrieben:

> [!div class="checklist"]
> * Bereitstellen der lokalen HCX-OVA-Datei
> * Aktivieren und Konfigurieren von HCX
> * Konfigurieren des Netzwerkuplinks und des Dienstnetzes
> * Ausführen des Setups durch Überprüfen des Appliance-Status

Nach Abschluss des Setups werden die empfohlenen nächsten Schritte beschrieben.

## <a name="before-you-begin"></a>Voraussetzungen
    
* Sehen Sie sich die [Tutorialreihe](tutorial-network-checklist.md) mit Grundlagen zu AVS Software Defined Datacenter (SDDC) an.
* Lesen Sie die [VMware-Dokumentation zu HCX](https://docs.vmware.com/en/VMware-HCX/index.html) einschließlich des HCX-Benutzerhandbuchs.
* Lesen Sie die VMware-Dokumentation zum [Migrieren von Virtual Machines mit VMware HCX](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-D0CD0CC6-3802-42C9-9718-6DA5FEC246C6.html?hWord=N4IghgNiBcIBIGEAaACAtgSwOYCcwBcMB7AOxAF8g)
* Optional: Lesen Sie die [Überlegungen zur Bereitstellung von VMware HCX](https://docs.vmware.com/en/VMware-HCX/services/install-checklist/GUID-C0A0E820-D5D0-4A3D-AD8E-EEAA3229F325.html)
* Optional: Lesen Sie weitere VMware-Ressourcen wie die VMware vSphere-[Blogreihe](https://blogs.vmware.com/vsphere/2019/10/cloud-migration-series-part-2.html) zu HCX. 
* Fordern Sie die Aktivierung von AVS HCX Enterprise über die AVS-Supportkanäle an.

Die Größenanpassung von Workloads für Compute-und Speicherressourcen ist ein wichtiger Planungsschritt beim Vorbereiten der Verwendung einer HCX-Lösung für private AVS-Clouds. Diese Größenanpassung sollte bei der ersten Planung der Umgebung für die private Cloud vorgenommen werden. 

## <a name="software-version-requirements"></a>Erforderliche Softwareversionen
Auf Infrastrukturkomponenten muss die erforderliche Mindestversion ausgeführt werden. 
                                                         
| Komponententyp                                                          | Anforderungen für die Quellumgebung                                                                   | Anforderungen für die Zielumgebung                                                                      |
| --- | --- | --- |
| vCenter Server                                                          | 5,1<br/><br/>Wenn Sie 5.5 U1 oder früher verwenden, verwenden Sie die eigenständige HCX-Benutzeroberfläche für HCX-Vorgänge.         | 6.0 U2 und höher                                                                                          |
| ESXi                                                                    | 5.0                                                                                               | ESXi 6.0 und höher                                                                                        |
| NSX                                                                     | Für die HCX-Netzwerkerweiterung logischer Switches an der Quelle: NSXv 6.2+ oder NSX-T 2.4+              | NSXv 6.2+ oder NSX-T 2.4+<br/><br/>Für HCX-Näherungsrouting: NSXv 6.4+ (Näherungsrouting wird für NSX-T nicht unterstützt) |
| vCloud Director                                                         | Nicht erforderlich, keine Interoperabilität mit vCloud Director am Quellstandort | Wenn die Zielumgebung in vCloud Director integriert ist, wird mindestens 9.1.0.2. benötigt.              |

## <a name="prerequisites"></a>Voraussetzungen

* Zwischen lokalen und AVS SDDC ER-Verbindungen sollte eine globale Reichweite konfiguriert werden.

* Alle erforderlichen Ports zwischen dem lokalen Netzwerk und AVS SDDC müssen geöffnet sein (siehe [VMware-Dokumentation zu HCX](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-E456F078-22BE-494B-8E4B-076EF33A9CF4.html)).

* Es werden eine IP-Adresse für den lokalen HCX Manager und mindestens zwei IP-Adressen für die Appliances Interconnect (IX) und Network Extension (NE) benötigt.

* Lokale HCX-IX-und NE-Appliances sollten die vCenter- und ESXi-Infrastruktur erreichen können.

* Zum Bereitstellen der WAN-Interconnect-Appliance, zusätzlich zum/22 CIDR-Netzwerk-Adressblock, der für die SDDC-Bereitstellung im Azure-Portal verwendet wird, ist für HCX ein /29-Block erforderlich. Sie sollten dies bei der Netzwerkplanung berücksichtigen.

## <a name="deploy-the-vmware-hcx-ova-on-premises"></a>Lokales Bereitstellen der OVA-Datei für VMware-HCX

1. Melden Sie sich bei AVS SDDC vCenter an, und wählen Sie **HCX**aus.

    ![„HCX“ in AVS vCenter auswählen](./media/hybrid-cloud-extension-installation/avs-vsphere-client.png)

1. Wählen Sie zum Herunterladen der HCX-OVA-Datei für VMware **Administration** (Verwaltung) > **System Updates** (Systemupdates) aus.

    ![Systemupdates abrufen](./media/hybrid-cloud-extension-installation/administration-updates.png)

1. Wählen Sie eine OVF-Vorlage aus, die für das lokale vCenter bereitgestellt werden soll.  

    ![OVF-Vorlage auswählen](./media/hybrid-cloud-extension-installation/select-template.png)

1. Wählen Sie einen Namen und einen Speicherort aus, und wählen Sie dann eine Ressource bzw. einen Cluster aus, in der oder dem HCX bereitgestellt werden soll. Überprüfen Sie anschließend die Details und die erforderlichen Ressourcen.  

    ![Vorlagendetails überprüfen](./media/hybrid-cloud-extension-installation/configure-template.png)

1. Lesen Sie die Lizenzbedingungen, stimmen Sie zu, und wählen Sie den erforderlichen Speicher und das Netzwerk aus. Wählen Sie **Weiter**aus.

1. Geben Sie unter **Customize template** (Vorlage anpassen) alle erforderlichen Informationen ein. 

    ![Anpassen der Vorlage](./media/hybrid-cloud-extension-installation/customize-template.png)  

1. Wählen Sie **Continue** (Weiter) aus, überprüfen Sie die Konfiguration, und wählen Sie **Finish** (Fertig stellen) aus, um die OVA-Datei für HCX bereitzustellen.

## <a name="activate-hcx"></a>Aktivieren von HCX

Führen Sie nach der Installation die folgenden Schritte aus:

1. Öffnen Sie den HCX Manager unter `https://HCXManagerIP:9443`, und melden Sie sich mit Ihrem Benutzernamen und Ihrem Kennwort an. 

1. Geben Sie unter **Licensing** (Lizenzierung) **den erweiterten HCX-Schlüssel** ein.  

    ![HCX-Schlüssel eingeben](./media/hybrid-cloud-extension-installation/hcx-key.png)  
    
    > [!NOTE]
    > Für den HCX Manager muss ein offener Internetzugriff oder ein Proxy konfiguriert sein.

1. Konfigurieren Sie vCenter.

    ![vCenter konfigurieren](./media/hybrid-cloud-extension-installation/configure-vcenter.png)

1. Bearbeiten Sie bei Bedarf unter **Datacenter Location** (Standort des Rechenzentrums) den Standort.

    ![Standort der Datenbank](./media/hybrid-cloud-extension-installation/system-location.png)

## <a name="configure-hcx"></a>Konfigurieren von HCX 

1. Melden Sie sich beim lokalen vCenter an, und wählen Sie dann **Home** > **HCX**aus.

    ![HCX in vCenter](./media/hybrid-cloud-extension-installation/hcx-vcenter.png)

1. Wählen Sie **Infrastructure** (Infrastruktur) > **Site Pairing** (Standortverbindung) > **Add a site pairing** (Standortverbindung hinzufügen) aus.

    ![Standortverbindung hinzufügen](./media/hybrid-cloud-extension-installation/site-pairing.png)

1. Geben Sie die Werte für die **Remote-HCX-URL**, Benutzernamen und Kennwort ein. Wählen Sie dann **Verbinden** aus.

   Das System zeigt den verbundenen Standort an.
   
    ![Standortverbindung](./media/hybrid-cloud-extension-installation/site-connection.png)

1. Wählen Sie **Interconnect** > **Multi-Site Service Mesh** (Dienstnetz mit mehreren Standorten) > **Network Profiles** (Netzwerkprofile) > **Create Network Profile** (Netzwerkprofil erstellen) aus.

    ![Netzwerkprofil erstellen](./media/hybrid-cloud-extension-installation/create-network-profile.png)

1. Geben Sie die IP-Adressbereiche für HCX IX und NE ein (für IX-und NE-Appliances sind mindestens zwei IP-Adressen erforderlich).
    
   ![IP-Adressbereiche eingeben](./media/hybrid-cloud-extension-installation/enter-address-ranges.png)
  
   > [!NOTE]
   > Die Network Extension-Appliance (HCX-NE) verfügt über eine eins-zu-eins-Beziehung mit einem verteilten virtuellen Switch (Distributed Virtual Switch, DVS).  

1. Wählen Sie jetzt **Compute profile** (Compute-Profil) > **Create compute profile** (Compute-Profil erstellen) aus.

1. Geben Sie einen Namen für das Compute-Profil ein, und wählen Sie **Continue** (Weiter) aus.  

    ![Compute-Profil erstellen](./media/hybrid-cloud-extension-installation/create-compute-profile.png)

1. Wählen Sie Dienste aus, die aktiviert werden sollen, z. B. für Migration, Netzwerkerweiterung oder Notfallwiederherstellung. Wählen Sie **Weiter**.

    ![Auswählen von Diensten](./media/hybrid-cloud-extension-installation/select-services.png)

1. Wählen Sie unter **Select Service Resources** (Dienstressourcen auswählen) eine oder mehrere Dienstressourcen aus, für die die ausgewählten HCX-Dienste aktiviert werden sollen. Wählen Sie **Weiter**.
    
   ![Dienstressourcen auswählen](./media/hybrid-cloud-extension-installation/select-service-resources.png)
  
   > [!NOTE]
   > Wählen Sie bestimmte Cluster mit VMs aus, die als Ziel für die Migration mit HCX verwendet werden sollen.

1. Wählen Sie **Datastore** (Datenspeicher) und dann **Continue** (Weiter) aus. 
      
    Wählen Sie alle Compute-und Speicherressourcen für die Bereitstellung der HCX Interconnect-Appliances aus. Wenn mehrere Ressourcen ausgewählt werden, verwendet HCX die erste ausgewählte Ressource, bis deren Kapazität erschöpft ist.  
    
    ![Bereitstellungsressourcen auswählen](./media/hybrid-cloud-extension-installation/deployment-resources.png)

1. Wählen Sie das unter **Network Profiles** (Netzwerkprofile) erstellte Netzwerkprofil für die Verwaltung aus, und wählen Sie **Continue** (Weiter) aus.  
      
    Wählen Sie das Netzwerkprofil aus, über das die Verwaltungsschnittstelle von vCenter und die ESXi-Hosts erreicht werden können. Wenn Sie ein solches Netzwerkprofil nicht bereits definiert haben, können Sie es hier erstellen.  
    
    ![Netzwerkprofil für die Verwaltung auswählen](./media/hybrid-cloud-extension-installation/management-network-profile.png)

1. Wählen Sie **Network Uplink** (Netzwerkuplink) und dann **Continue** (Weiter) aus.
      
    Wählen Sie mindestens ein Netzwerkprofil aus, sodass eine der folgenden Bedingungen erfüllt wird:  
    * Die Interconnect-Appliances am Remote-Standort können über dieses Netzwerk erreicht werden.  
    * Die Appliances am Remote-Standort können die lokalen Interconnect-Appliances über dieses Netzwerk erreichen.  
    
    Wenn Sie Punkt-zu-Punkt-Netzwerke wie Direct Connect verwenden, die nicht für mehrere Standorte verwendet werden, können Sie diesen Schritt überspringen, da Compute-Profile für mehrere Standorte verwendet werden. In solchen Fällen können Uplink-Netzwerkprofile beim Erstellen des Interconnect-Dienstnetzes überschrieben und angegeben werden.  
    
    ![Uplink-Netzwerkprofil auswählen](./media/hybrid-cloud-extension-installation/uplink-network-profile.png)

1. Wählen Sie **vMotion Network Profile** (vMotion-Netzwerkprofil) und dann **Continue** (Weiter) aus.
      
    Wählen Sie das Netzwerkprofil aus, über das die vMotion-Schnittstelle der ESXi-Hosts erreicht werden kann. Wenn Sie ein solches Netzwerkprofil nicht bereits definiert haben, können Sie es hier erstellen. Wenn Sie kein vMotion-Netzwerk verwenden, wählen Sie **Management Network Profile** (Netzwerkprofil für die Verwaltung) aus.  
    
    ![vMotion-Netzwerkprofil auswählen](./media/hybrid-cloud-extension-installation/vmotion-network-profile.png)

1. Wählen Sie **vSphere Replication Network Profile** (Netzwerkprofil für vSphere-Replikation) und dann **Continue** (Weiter) aus.
      
    Wählen Sie ein Netzwerkprofil aus, über das die vSphere-Replikationsschnittstelle der ESXi-Hosts erreicht werden kann. In den meisten Fällen ist dieses Profil mit dem Netzwerkprofil für die Verwaltung identisch.  
    
    ![Netzwerkprofil für die vSphere-Replikation auswählen](./media/hybrid-cloud-extension-installation/vsphere-replication-network-profile.png)

1. Wählen Sie **Distributed Switches for Network Extensions** (Verteilte Switches für Network Extensions) und dann **Continue** (Weiter) aus.  
      
    Wählen Sie die verteilten virtuellen Switches mit den Netzwerken aus, mit denen die VMs, die migriert werden, verbunden sind.

    ![Verteilte virtuelle Switches auswählen](./media/hybrid-cloud-extension-installation/distributed-switches.png)

1. Überprüfen Sie die Verbindungsregeln, und wählen Sie **Continue** (Weiter) aus. Wählen Sie **Finish** (Fertig stellen) aus, um das Compute-Profil zu erstellen.  

    ![Compute-Profil erstellen](./media/hybrid-cloud-extension-installation/complete-compute-profile.png)

## <a name="configure-network-uplink"></a>Konfigurieren des Netzwerkuplinks

Konfigurieren Sie nun die Netzwerkprofiländerung in AVS SDDC für den Netzwerkuplink.

1. Melden Sie sich bei SDDC NSX-T an, um einen neuen logischen Switch zu erstellen, oder verwenden Sie einen vorhandenen logischen Switch, der zwischen dem lokalen Standort und AVS SDDC für den Netzwerkuplink verwendet werden kann.

1. Erstellen Sie ein Netzwerkprofil für den HCX-Uplink in AVS SDDC, das für die Kommunikation zwischen dem lokalen Standort und AVS SDDC verwendet werden kann.  
    
   ![Netzwerkprofil für Uplink erstellen](./media/hybrid-cloud-extension-installation/network-profile-uplink.png)

1. Geben Sie einen Namen für das Netzwerkprofil und mindestens 4 bis 5 freie IP-Adressen ein (basierend auf der erforderlichen L2-Netzwerkerweiterung).  
    
   ![Netzwerkprofil für Uplink konfigurieren](./media/hybrid-cloud-extension-installation/configure-uplink-profile.png)

1. Wählen Sie **Create** (Erstellen) aus, um die AVS SDDC-Konfiguration abzuschließen.

## <a name="configure-service-mesh"></a>Konfigurieren des Dienstnetzes

Konfigurieren Sie nun das Dienstnetz zwischen dem lokalen Standort und AVS SDDC.

1. Melden Sie sich bei AVS SDDC vCenter an, und wählen Sie **HCX**aus.

1. Wählen Sie **Infrastructure** (Infrastruktur) > **Interconnect** > **Dienstnetz** > **Create Service Mesh** (Dienstnetz erstellen) aus.  Konfigurieren Sie die Netzwerk-und Compute-Profile, die Sie in den vorherigen Schritten erstellt haben.    
      
    ![Konfigurieren des Dienstnetzes](./media/hybrid-cloud-extension-installation/configure-service-mesh.png)

1. Wählen Sie **Create Service Mesh** (Dienstnetz erstellen) und dann **Continue** (Weiter) aus.  
      
    Wählen Sie verbundene Standorte aus, zwischen denen Hybrid-Mobilität aktiviert werden soll.  
    
    ![Verbundene Standorte auswählen](./media/hybrid-cloud-extension-installation/select-paired-sites.png)

1. Wählen Sie **Compute profile** (Compute-Profil) und dann **Continue** (Weiter) aus.
      
    Wählen Sie jeweils ein Compute-Profil am Quell- und am Remote-Standort aus, um Hybriddienste zu aktivieren. Durch die Auswahl werden die Ressourcen definiert, durch die VMs HCX-Dienste nutzen können.  
      
    ![Hybriddienste aktivieren](./media/hybrid-cloud-extension-installation/enable-hybridity.png)

1. Wählen Sie die Dienste, die für HCX aktiviert werden sollen, und dann **Continue** (Weiter) aus.  
      
    ![HCX-Dienste auswählen](./media/hybrid-cloud-extension-installation/hcx-services.png)

1. Wählen Sie unter **Advanced Configuration - Override Uplink Network profiles** (Erweiterte Konfiguration – Uplink-Netzwerkprofile überschreiben)die Option **Continue** (Weiter) aus.  
      
    Uplink-Netzwerkprofile werden verwendet, um eine Verbindung mit dem Netzwerk herzustellen, über das die Interconnect-Appliances des Remote-Standorts erreicht werden können.  
      
    ![Uplink-Profile überschreiben](./media/hybrid-cloud-extension-installation/override-uplink-profiles.png)

1. Wählen Sie unter **Advanced Configuration – Network Extension Appliance Scale Out** (Erweiterte Konfiguration – Aufskalierung für Network Extension-Appliance) **Configure the Network Extension Appliance Scale Out** (Aufskalierung für Network Extension-Appliance konfigurieren) aus. 
      
    ![Aufskalierung für Network Extension-Appliance](./media/hybrid-cloud-extension-installation/network-extension-scale-out.png)

1. Geben Sie die Anzahl der Appliances entsprechend der Anzahl der DVS-Switches an.  
      
    ![Appliance-Anzahl konfigurieren](./media/hybrid-cloud-extension-installation/appliance-scale.png)

1. Wählen Sie unter **Advanced Configuration - Traffic Engineering** (Erweiterte Konfiguration – Verwaltung des Datenverkehrs) **Continue** (Weiter) aus.  
      
    ![Verwaltung des Datenverkehrs konfigurieren](./media/hybrid-cloud-extension-installation/traffic-engineering.png)

1. Überprüfen Sie die Topologievorschau, und wählen Sie **Continue** (Weiter) aus. Geben Sie dann einen benutzerfreundlichen Namen für dieses Dienstnetz ein, und wählen Sie **Finish** (Fertig stellen) aus, um den Vorgang abzuschließen.  
      
    ![Dienstnetz fertig stellen](./media/hybrid-cloud-extension-installation/complete-service-mesh.png)

    Das Dienstnetz ist jetzt bereitgestellt und konfiguriert.  
      
    ![Bereitgestelltes Dienstnetz](./media/hybrid-cloud-extension-installation/deployed-service-mesh.png)

## <a name="check-appliance-status"></a>Überprüfen des Appliance-Status
Um den Status der Appliance zu überprüfen, wählen Sie **Interconnect** > **Appliances** aus. 
      
![Appliance-Status](./media/hybrid-cloud-extension-installation/appliance-status.png)

## <a name="next-steps"></a>Nächste Schritte

Wenn der Wert unter **Tunnel Status** (Tunnelstatus) für die Interconnect-Appliance **UP** lautet und grün angezeigt wird, können Sie AVS-VMs mit HCX migrieren und schützen. Weitere Informationen finden Sie in der [VMware-Dokumentation zu HCX](https://docs.vmware.com/en/VMware-HCX/index.html) und in der technischen Dokumentation zu VMware unter [Migrating Virtual Machines with VMware HCX](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-D0CD0CC6-3802-42C9-9718-6DA5FEC246C6.html?hWord=N4IghgNiBcIBIGEAaACAtgSwOYCcwBcMB7AOxAF8g) (Migrieren von VMs mit VMware HCX).
