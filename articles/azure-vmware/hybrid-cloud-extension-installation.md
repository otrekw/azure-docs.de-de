---
title: Installieren von Hybrid Cloud Extension (HCX)
description: Einrichten der VMware-HCX-Lösung (Hybrid Cloud Extension) für die private Azure VMware Solution-Cloud
ms.topic: how-to
ms.date: 07/15/2020
ms.openlocfilehash: fb8497af33b364c1d2ab475233bd2a83ef1befad
ms.sourcegitcommit: 62717591c3ab871365a783b7221851758f4ec9a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/22/2020
ms.locfileid: "88752317"
---
# <a name="install-hcx-for-azure-vmware-solution"></a>Installieren von HCX für eine Azure-VMware-Lösung

In diesem Artikel werden die Vorgehensweisen zum Einrichten der VMware-HCX-Lösung (Hybrid Cloud Extension) für Ihre private Azure VMware Solution-Cloud beschrieben. HCX ermöglicht die Migration Ihrer VMware-Workloads zur Cloud und zu anderen verbundenen Standorten mithilfe verschiedener integrierter, von HCX unterstützter Migrationstypen.

Die Standardinstallation HCX Advanced unterstützt bis zu drei Standortverbindungen (lokal oder Cloud-zu-Cloud). Wenn mehr als drei Standortverbindungen erforderlich sind, können Kunden das HCX Enterprise-Add-on, das sich zurzeit in der Vorschauversion befindet, über den Support aktivieren. Für HCX Enterprise fallen über die allgemeine Verfügbarkeit (GA) hinaus zusätzliche Gebühren für Kunden an, die Version bietet jedoch [zusätzliche Funktionen](https://cloud.vmware.com/community/2019/08/08/introducing-hcx-enterprise/).


Lesen Sie zuerst die Informationen unter [Voraussetzungen (Vorbereitung)](#before-you-begin), [Erforderliche Softwareversionen](#software-version-requirements) und [Voraussetzungen](#prerequisites). 

Danach werden die folgenden Aufgaben beschrieben:

> [!div class="checklist"]
> * Bereitstellen der lokalen HCX-OVA-Datei (Connector)
> * Aktivieren und Konfigurieren von HCX
> * Konfigurieren des Netzwerkuplinks und des Dienstnetzes
> * Ausführen des Setups durch Überprüfen des Appliance-Status

Nach Abschluss des Setups können Sie die empfohlenen nächsten Schritte am Ende dieses Artikels durchführen.  

## <a name="before-you-begin"></a>Voraussetzungen
    
* Sehen Sie sich die [Tutorialreihe](tutorial-network-checklist.md) mit Grundlagen zu Azure VMware Solution Software Defined Datacenter (SDDC) an.
* Lesen Sie die [VMware-Dokumentation zu HCX](https://docs.vmware.com/en/VMware-HCX/index.html) einschließlich des HCX-Benutzerhandbuchs.
* Lesen Sie die VMware-Dokumentation zum [Migrieren von Virtual Machines mit VMware HCX](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-D0CD0CC6-3802-42C9-9718-6DA5FEC246C6.html?hWord=N4IghgNiBcIBIGEAaACAtgSwOYCcwBcMB7AOxAF8g).
* Optional: Lesen Sie die [Überlegungen zur Bereitstellung von VMware HCX](https://docs.vmware.com/en/VMware-HCX/services/install-checklist/GUID-C0A0E820-D5D0-4A3D-AD8E-EEAA3229F325.html).
* Optional: Lesen Sie weitere VMware-Ressourcen wie die VMware vSphere-[Blogreihe](https://blogs.vmware.com/vsphere/2019/10/cloud-migration-series-part-2.html) zu HCX. 
* Fordern Sie eine Aktivierung von Azure VMware Solution HCX Enterprise über die Azure VMware Solution-Supportkanäle an.

Die Größenanpassung von Workloads für Compute- und Speicherressourcen ist ein wichtiger Planungsschritt beim Vorbereiten der Verwendung einer HCX-Lösung für private Azure VMware Solution-Clouds. Nehmen Sie diese Größenanpassung bei der ersten Planung der Umgebung für die private Cloud vor. 

Sie können Workloads auch anpassen, indem Sie eine Azure VMware Solution-Bewertung im Azure Migrate-Portal ausführen (https://docs.microsoft.com/azure/migrate/how-to-create-azure-vmware-solution-assessment) ).

## <a name="software-version-requirements"></a>Erforderliche Softwareversionen

Auf Infrastrukturkomponenten muss die erforderliche Mindestversion ausgeführt werden. 
                                                         
| Komponententyp    | Anforderungen für die Quellumgebung    | Anforderungen für die Zielumgebung   |
| --- | --- | --- |
| vCenter Server   | 5,1<br/><br/>Wenn Sie 5.5 U1 oder früher verwenden, verwenden Sie die eigenständige HCX-Benutzeroberfläche für HCX-Vorgänge.  | 6.0 U2 und höher   |
| ESXi   | 5.0    | ESXi 6.0 und höher   |
| NSX    | Für die HCX-Netzwerkerweiterung logischer Switches an der Quelle: NSXv 6.2+ oder NSX-T 2.4+   | NSXv 6.2+ oder NSX-T 2.4+<br/><br/>Für HCX-Näherungsrouting: NSXv 6.4+ (Näherungsrouting wird für NSX-T nicht unterstützt.) |
| vCloud Director   | Nicht erforderlich, keine Interoperabilität mit vCloud Director am Quellstandort | Bei der Integration der Zielumgebung in vCloud Director ist mindestens 9.1.0.2 erforderlich.  |

## <a name="prerequisites"></a>Voraussetzungen

* Zwischen lokalen Verbindungen und Azure VMware Solution SDDC ExpressRoute-Verbindungen muss ExpressRoute Global Reach konfiguriert werden.

* Alle erforderlichen Ports zwischen dem lokalen Netzwerk und Azure VMware Solution SDDC müssen geöffnet sein (weitere Informationen finden Sie in der [VMware-Dokumentation zu HCX](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-E456F078-22BE-494B-8E4B-076EF33A9CF4.html)).

* Es werden eine IP-Adresse für den lokalen HCX Manager und mindestens zwei IP-Adressen für die Appliances Interconnect (IX) und Network Extension (NE) benötigt.

* Lokale HCX-IX-und NE-Appliances sollten die vCenter- und ESXi-Infrastruktur erreichen können.

* Zum Bereitstellen der WAN-Interconnect-Appliance, zusätzlich zum/22 CIDR-Netzwerk-Adressblock, der für die SDDC-Bereitstellung im Azure-Portal verwendet wird, ist für HCX ein /29-Block erforderlich. Berücksichtigen Sie diese Anforderung bei Ihrer Netzwerkplanung.

## <a name="deploy-the-vmware-hcx-ova-on-premises"></a>Lokales Bereitstellen der OVA-Datei für VMware-HCX

1. Melden Sie sich bei Azure VMware Solution SDDC vCenter an, und wählen Sie **HCX** aus.

   :::image type="content" source="media/hybrid-cloud-extension-installation/avs-vsphere-client.png" alt-text="Melden Sie sich bei Azure VMware Solution SDDC vCenter an, und wählen Sie HCX aus.":::

1. Wählen Sie unter **Administration** (Verwaltung) die Option **System Updates** (Systemupdates) und dann **Request download link** (Downloadlink anfordern), um die HCX-OVA-Datei für VMware herunterzuladen.

   :::image type="content" source="media/hybrid-cloud-extension-installation/administration-updates.png" alt-text="Wählen Sie unter „Administration“ (Verwaltung) „System Updates“ (Systemupdates) und dann „Request download link“ (Downloadlink anfordern), um die HCX-OVA-Datei für VMware herunterzuladen.":::

1. Navigieren Sie als Nächstes zum lokalen vCenter, und wählen Sie eine OVF-Vorlage aus, die Sie in Ihrem lokalen vCenter bereitstellen.  

   :::image type="content" source="media/hybrid-cloud-extension-installation/select-template.png" alt-text="Navigieren Sie als Nächstes zum lokalen vCenter, und wählen Sie eine OVF-Vorlage aus, die Sie in Ihrem lokalen vCenter bereitstellen.":::

1. Wählen Sie einen Namen und einen Speicherort aus, und wählen Sie dann eine Ressource bzw. einen Cluster aus, in der oder dem HCX bereitgestellt werden soll. Überprüfen Sie anschließend die Details und die erforderlichen Ressourcen.  

   :::image type="content" source="media/hybrid-cloud-extension-installation/configure-template.png" alt-text=" Wählen Sie einen Namen und einen Speicherort aus, und wählen Sie dann eine Ressource bzw. einen Cluster aus, in der oder dem HCX bereitgestellt werden soll. Überprüfen Sie anschließend die Details und die erforderlichen Ressourcen.":::

1. Lesen Sie die Lizenzbedingungen, stimmen Sie zu, und wählen Sie den erforderlichen Speicher und das Netzwerk aus. Wählen Sie **Weiter**aus.

1. Geben Sie unter **Customize template** (Vorlage anpassen) alle erforderlichen Informationen ein. 

   :::image type="content" source="media/hybrid-cloud-extension-installation/customize-template.png" alt-text="Geben Sie unter „Customize template“ (Vorlage anpassen) alle erforderlichen Informationen ein.":::

1. Wählen Sie **Continue** (Weiter) aus, überprüfen Sie die Konfiguration, und wählen Sie **Finish** (Fertig stellen) aus, um die OVA-Datei für HCX bereitzustellen.

## <a name="activate-hcx"></a>Aktivieren von HCX

Führen Sie nach der Installation die folgenden Schritte aus:

1. Melden Sie sich beim lokalen HCX Manager unter `https://HCXManagerIP:9443` mit Ihrem Benutzernamen und Ihrem Kennwort an. 

   > [!IMPORTANT]
   > Geben Sie die Portnummer `9443` mit der IP-Adresse des HCX Managers an.

1. Geben Sie unter **Licensing** (Lizenzierung) **den erweiterten HCX-Schlüssel** ein.  

   :::image type="content" source="media/hybrid-cloud-extension-installation/hcx-key.png" alt-text="Geben Sie unter „Licensing“ (Lizenzierung) den erweiterten HCX-Schlüssel ein.":::
    
    > [!NOTE]
    > Für den HCX Manager muss ein offener Internetzugriff oder ein Proxy konfiguriert sein.

1. Bearbeiten Sie im **vCenter** ggf. die vCenter-Informationen.

   :::image type="content" source="media/hybrid-cloud-extension-installation/configure-vcenter.png" alt-text="Bearbeiten Sie im vCenter ggf. die vCenter-Informationen.":::

1. Bearbeiten Sie bei Bedarf unter **Datacenter Location** (Standort des Rechenzentrums) den Standort.

   :::image type="content" source="media/hybrid-cloud-extension-installation/system-location.png" alt-text="Bearbeiten Sie bei Bedarf unter „Datacenter Location“ (Standort des Rechenzentrums) den Standort.":::

## <a name="configure-hcx"></a>Konfigurieren von HCX 

1. Melden Sie sich beim lokalen vCenter an, und wählen Sie unter **Home** die Option **HCX** aus.

   :::image type="content" source="media/hybrid-cloud-extension-installation/hcx-vcenter.png" alt-text="Melden Sie sich beim lokalen vCenter an, und wählen Sie unter „Home“ die Option „HCX“ aus.":::

1. Wählen Sie unter **Infrastruktur** die Option **Standortverbindung** > **Standortverbindung hinzufügen** aus.

   :::image type="content" source="media/hybrid-cloud-extension-installation/site-pairing.png" alt-text="Wählen Sie unter „Infrastruktur“ die Option „Standortverbindung“ > „Standortverbindung hinzufügen“ aus.":::

1. Geben Sie die Remote-HCX-URL oder IP-Adresse und den Benutzernamen sowie das Kennwort für den die Azure VMware Solution-Rolle „cloudadmin“ ein, und wählen Sie **Verbinden** aus.

   Das System zeigt den verbundenen Standort an.

   :::image type="content" source="media/hybrid-cloud-extension-installation/site-connection.png" alt-text="Das System zeigt den verbundenen Standort an.":::

1. Wählen Sie unter **Infrastruktur** die Option **Interconnect** > **Multi-Site Service Mesh (Dienstnetz mit mehreren Standorten)**  > **Network Profiles (Netzwerkprofile)**  > **Create Network Profile (Netzwerkprofil erstellen)** aus.

   :::image type="content" source="media/hybrid-cloud-extension-installation/create-network-profile.png" alt-text="Wählen Sie unter „Infrastructure“ (Infrastruktur) die Option „Interconnect (Anbindung) > „Multi-Site Service Mesh“ (Dienstnetz mit mehreren Standorten) > „Network Profiles“ (Netzwerkprofile) > „Create Network Profile“ (Netzwerkprofil erstellen) aus.":::

1. Geben Sie für das neue Netzwerkprofil die IP-Adressbereiche für HCX IX und NE ein (für IX-und NE-Appliances sind mindestens zwei IP-Adressen erforderlich).

   :::image type="content" source="media/hybrid-cloud-extension-installation/enter-address-ranges.png" alt-text="Geben Sie für das neue Netzwerkprofil die IP-Adressbereiche für HCX IX und NE ein (für IX-und NE-Appliances sind mindestens zwei IP-Adressen erforderlich).":::
  
   > [!NOTE]
   > Die Network Extension-Appliance (HCX-NE) verfügt über eine eins-zu-eins-Beziehung mit einem verteilten virtuellen Switch (Distributed Virtual Switch, DVS).  

1. Wählen Sie jetzt **Compute profile** (Compute-Profil) > **Create compute profile** (Compute-Profil erstellen) aus.

1. Geben Sie einen Namen für das Compute-Profil ein, und wählen Sie **Continue** (Weiter) aus.  

   :::image type="content" source="media/hybrid-cloud-extension-installation/create-compute-profile.png" alt-text="Geben Sie einen Namen für das Computeprofil ein, und wählen Sie „Continue“ (Weiter) aus.":::

1. Wählen Sie die Dienste aus, die aktiviert werden sollen, z. B. für Migration, Netzwerkerweiterung oder Notfallwiederherstellung, und wählen Sie **Weiter** aus.

   :::image type="content" source="media/hybrid-cloud-extension-installation/select-services.png" alt-text="Wählen Sie die Dienste aus, die aktiviert werden sollen, z. B. für Migration, Netzwerkerweiterung oder Notfallwiederherstellung, und wählen Sie „Weiter“ aus.":::

1. Wählen Sie unter **Select Service Resources** (Dienstressourcen auswählen) eine oder mehrere Dienstressourcen aus, für die die ausgewählten HCX-Dienste aktiviert werden sollen. Wählen Sie **Weiter**.

   :::image type="content" source="media/hybrid-cloud-extension-installation/select-service-resources.png" alt-text="Wählen Sie unter „Select Service Resources“ (Dienstressourcen auswählen) eine oder mehrere Dienstressourcen aus, für die die ausgewählten HCX-Dienste aktiviert werden sollen. Wählen Sie „Weiter“ aus.":::
  
   > [!NOTE]
   > Wählen Sie bestimmte Cluster mit VMs aus, die als Ziel für die Migration mit HCX verwendet werden sollen.

1. Wählen Sie **Datastore** (Datenspeicher) und dann **Continue** (Weiter) aus. 
      
   Wählen Sie alle Compute-und Speicherressourcen für die Bereitstellung der HCX Interconnect-Appliances aus. Wenn mehrere Ressourcen ausgewählt werden, verwendet HCX die erste ausgewählte Ressource, bis deren Kapazität erschöpft ist.  

   :::image type="content" source="media/hybrid-cloud-extension-installation/deployment-resources.png" alt-text="Wählen Sie alle Compute-und Speicherressourcen für die Bereitstellung der HCX Interconnect-Appliances aus. Wenn mehrere Ressourcen ausgewählt werden, verwendet HCX die erste ausgewählte Ressource, bis deren Kapazität erschöpft ist.":::

1. Wählen Sie das unter **Network Profiles** (Netzwerkprofile) erstellte Netzwerkprofil für die Verwaltung aus, und wählen Sie **Continue** (Weiter) aus.  
      
   Wählen Sie das Netzwerkprofil aus, über das die Verwaltungsschnittstelle von vCenter und die ESXi-Hosts erreicht werden können. Wenn Sie ein solches Netzwerkprofil nicht bereits definiert haben, können Sie es hier erstellen.  

   :::image type="content" source="media/hybrid-cloud-extension-installation/management-network-profile.png" alt-text="Wählen Sie das Netzwerkprofil aus, über das die Verwaltungsschnittstelle von vCenter und die ESXi-Hosts erreicht werden können. Wenn Sie ein solches Netzwerkprofil nicht bereits definiert haben, können Sie es hier erstellen.":::

1. Wählen Sie **Network Uplink** (Netzwerkuplink) und dann **Continue** (Weiter) aus.
      
   Wählen Sie mindestens ein Netzwerkprofil aus, sodass eine der folgenden Bedingungen erfüllt wird:  
   * Die Interconnect-Appliances am Remote-Standort können über dieses Netzwerk erreicht werden.  
   * Die Appliances am Remote-Standort können die lokalen Interconnect-Appliances über dieses Netzwerk erreichen.  
    
   Wenn Sie Punkt-zu-Punkt-Netzwerke wie Direct Connect verwenden, die nicht für mehrere Standorte verwendet werden, können Sie diesen Schritt überspringen, da Compute-Profile für mehrere Standorte verwendet werden. In solchen Fällen können Uplink-Netzwerkprofile beim Erstellen des Interconnect-Dienstnetzes überschrieben und angegeben werden.  

   :::image type="content" source="media/hybrid-cloud-extension-installation/uplink-network-profile.png" alt-text="Wählen Sie „Network Uplink“ (Netzwerkuplink) und dann „Continue“ (Weiter) aus.":::

1. Wählen Sie **vMotion Network Profile** (vMotion-Netzwerkprofil) und dann **Continue** (Weiter) aus.
      
   Wählen Sie das Netzwerkprofil aus, über das die vMotion-Schnittstelle der ESXi-Hosts erreicht werden kann. Wenn Sie ein solches Netzwerkprofil nicht bereits definiert haben, können Sie es hier erstellen. Wenn Sie kein vMotion-Netzwerk verwenden, wählen Sie **Management Network Profile** (Netzwerkprofil für die Verwaltung) aus.  

   :::image type="content" source="media/hybrid-cloud-extension-installation/vmotion-network-profile.png" alt-text="Wählen Sie „vMotion Network Profile“ (vMotion-Netzwerkprofil) und dann „Continue“ (Weiter) aus.":::

1. Wählen Sie unter **Netzwerkprofil für die vSphere-Replikation auswählen** ein Netzwerkprofil für die vSphere-Replikationsschnittstelle der ESXi-Hosts und dann **Weiter** aus.
      
   In den meisten Fällen ist dieses Profil mit dem Netzwerkprofil für die Verwaltung identisch.  

   :::image type="content" source="media/hybrid-cloud-extension-installation/vsphere-replication-network-profile.png" alt-text="Wählen Sie unter „Netzwerkprofil für die vSphere-Replikation auswählen“ ein Netzwerkprofil für die vSphere-Replikationsschnittstelle der ESXi-Hosts und dann „Weiter“ aus.":::

1. Wählen Sie unter **Select Distributed Switches for Network Extensions (Verteilte Switches für Network Extensions)** den DVS aus, der Netzwerke enthält, die integriert und verbunden werden.  Wählen Sie **Weiter**.  

   :::image type="content" source="media/hybrid-cloud-extension-installation/distributed-switches.png" alt-text="Wählen Sie unter „Select Distributed Switches for Network Extensions“ (Verteilte Switches für Network Extensions) den DVS aus, der Netzwerke enthält, die integriert und verbunden werden.  Wählen Sie „Weiter“ aus.":::

1. Überprüfen Sie die Verbindungsregeln, und wählen Sie **Continue** (Weiter) aus.  

   :::image type="content" source="media/hybrid-cloud-extension-installation/complete-compute-profile.png" alt-text="Überprüfen Sie die Verbindungsregeln, und wählen Sie „Continue“ (Weiter) aus.":::

1.  Wählen Sie **Finish** (Fertig stellen) aus, um das Compute-Profil zu erstellen.

## <a name="configure-network-uplink"></a>Konfigurieren des Netzwerkuplinks

Konfigurieren Sie nun die Netzwerkprofiländerung in Azure VMware Solution SDDC für den Netzwerkuplink.

1. Melden Sie sich bei SDDC NSX-T an, um einen neuen logischen Switch zu erstellen, oder verwenden Sie einen vorhandenen logischen Switch, der zwischen dem lokalen Standort und Azure VMware Solution SDDC für den Netzwerkuplink verwendet werden kann.

1. Erstellen Sie ein Netzwerkprofil für den HCX-Uplink in Azure VMware Solution SDDC, das für die Kommunikation zwischen dem lokalen Standort und Azure VMware Solution SDDC verwendet werden kann.  

   :::image type="content" source="media/hybrid-cloud-extension-installation/network-profile-uplink.png" alt-text="Erstellen Sie ein Netzwerkprofil für den HCX-Uplink in Azure VMware Solution SDDC, das für die Kommunikation zwischen dem lokalen Standort und Azure VMware Solution SDDC verwendet werden kann.":::

1. Geben Sie einen Namen für das Netzwerkprofil und mindestens 4 bis 5 freie IP-Adressen ein (basierend auf der erforderlichen L2-Netzwerkerweiterung).  

   :::image type="content" source="media/hybrid-cloud-extension-installation/configure-uplink-profile.png" alt-text="Geben Sie einen Namen für das Netzwerkprofil und mindestens 4 bis 5 freie IP-Adressen ein (basierend auf der erforderlichen L2-Netzwerkerweiterung).":::

1. Wählen Sie **Create** (Erstellen) aus, um die Azure VMware Solution SDDC-Konfiguration abzuschließen.

## <a name="configure-service-mesh"></a>Konfigurieren des Dienstnetzes

Konfigurieren Sie nun das Dienstnetz zwischen dem lokalen Standort und Azure VMware Solution SDDC.

1. Melden Sie sich bei Azure VMware Solution SDDC vCenter an, und wählen Sie **HCX** aus.

2. Wählen Sie unter **Infrastructure** (Infrastruktur) die Option **Interconnect** > **Service Mesh (Dienstnetz)**  > **Create Service Mesh (Dienstnetz erstellen)** aus, um die in den vorherigen Schritten erstellten Netzwerk- und Computeprofile zu konfigurieren.    

   :::image type="content" source="media/hybrid-cloud-extension-installation/configure-service-mesh.png" alt-text="Wählen Sie unter „Infrastructure“ (Infrastruktur) die Option „Interconnect > „Service Mesh“ (Dienstnetz) > „Create Service Mesh“ (Dienstnetz erstellen) aus, um die in den vorherigen Schritten erstellten Netzwerk- und Computeprofile zu konfigurieren.":::

3. Wählen Sie verbundene Standorte aus, um die Hybridfunktion zu aktivieren, und wählen Sie dann **Continue** (Weiter) aus.   

   :::image type="content" source="media/hybrid-cloud-extension-installation/select-paired-sites.png" alt-text="Wählen Sie verbundene Standorte aus, um die Hybridfunktion zu aktivieren, und wählen Sie dann „Continue“ (Weiter) aus.":::

4. Wählen Sie das Quellcomputeprofil und das Remotecomputeprofil aus, um Hybriddienste zu aktivieren, und wählen Sie dann **Continue** (Weiter) aus.
      
   Mit der Auswahl werden die Ressourcen definiert, in denen VMs HCX-Dienste nutzen können.  

   :::image type="content" source="media/hybrid-cloud-extension-installation/enable-hybridity.png" alt-text="Mit der Auswahl werden die Ressourcen definiert, in denen VMs HCX-Dienste nutzen können.":::

5. Wählen Sie die zu aktivierenden Dienste und dann **Continue** (Weiter) aus.  

   :::image type="content" source="media/hybrid-cloud-extension-installation/hcx-services.png" alt-text="Wählen Sie die zu aktivierenden Dienste und dann „Continue“ (Weiter) aus.":::

6. Wählen Sie unter **Advanced Configuration - Override Uplink Network profiles** (Erweiterte Konfiguration – Uplink-Netzwerkprofile überschreiben)die Option **Continue** (Weiter) aus.  
      
   Uplink-Netzwerkprofile werden verwendet, um eine Verbindung mit dem Netzwerk herzustellen, über das die Interconnect-Appliances des Remote-Standorts erreicht werden können.  

   :::image type="content" source="media/hybrid-cloud-extension-installation/override-uplink-profiles.png" alt-text="Uplink-Netzwerkprofile werden verwendet, um eine Verbindung mit dem Netzwerk herzustellen, über das die Interconnect-Appliances des Remote-Standorts erreicht werden können.":::

7. Wählen Sie **Configure the Network Extension Appliance Scale Out** (Aufskalierung für Network Extension-Appliance konfigurieren) aus. 

   :::image type="content" source="media/hybrid-cloud-extension-installation/network-extension-scale-out.png" alt-text="Wählen Sie „Configure the Network Extension Appliance Scale Out“ (Aufskalierung für Network Extension-Appliance konfigurieren) aus.":::

8. Geben Sie die Anzahl der Appliances entsprechend der Anzahl der DVS-Switches an.  

   :::image type="content" source="media/hybrid-cloud-extension-installation/appliance-scale.png" alt-text="Geben Sie die Anzahl der Appliances entsprechend der Anzahl der DVS-Switches an.":::

9. Wählen Sie **Continue** (Weiter) aus, um den Schritt zu überspringen.  

   :::image type="content" source="media/hybrid-cloud-extension-installation/traffic-engineering.png" alt-text="Wählen Sie „Continue“ (Weiter) aus, um den Schritt zu überspringen.":::

10. Überprüfen Sie die Topologievorschau, und wählen Sie **Continue** (Weiter) aus. 

11. Geben Sie einen benutzerfreundlichen Namen für dieses Dienstnetz ein, und wählen Sie **Finish** (Fertig stellen) aus, um den Vorgang abzuschließen.  

   :::image type="content" source="media/hybrid-cloud-extension-installation/complete-service-mesh.png" alt-text="Dienstnetz fertig stellen":::

   Das Dienstnetz ist jetzt bereitgestellt und konfiguriert.  

   :::image type="content" source="media/hybrid-cloud-extension-installation/deployed-service-mesh.png" alt-text="Bereitgestelltes Dienstnetz":::

## <a name="check-appliance-status"></a>Überprüfen des Appliance-Status
Um den Status der Appliance zu überprüfen, wählen Sie **Interconnect** > **Appliances** aus. 

:::image type="content" source="media/hybrid-cloud-extension-installation/appliance-status.png" alt-text="Überprüfen Sie den Status der Appliance.":::

## <a name="next-steps"></a>Nächste Schritte

Wenn der Wert unter **Tunnel Status** (Tunnelstatus) für die Interconnect-Appliance **UP** lautet und grün angezeigt wird, können Sie Azure VMware Solution-VMs mit HCX migrieren und schützen. Weitere Informationen finden Sie in der [VMware-Dokumentation zu HCX](https://docs.vmware.com/en/VMware-HCX/index.html) und in der technischen Dokumentation zu VMware unter [Migrating Virtual Machines with VMware HCX](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-D0CD0CC6-3802-42C9-9718-6DA5FEC246C6.html?hWord=N4IghgNiBcIBIGEAaACAtgSwOYCcwBcMB7AOxAF8g) (Migrieren von VMs mit VMware HCX).
