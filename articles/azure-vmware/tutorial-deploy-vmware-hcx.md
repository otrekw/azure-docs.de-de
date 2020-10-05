---
title: 'Tutorial: Bereitstellen und Konfigurieren von VMware HCX'
description: Erfahren Sie, wie Sie eine VMware HCX-Lösung für Ihre private Azure VMware Solution-Cloud bereitstellen und konfigurieren.
ms.topic: tutorial
ms.date: 10/02/2020
ms.openlocfilehash: 69832d1537f0f1be95d3283f543ef6e54187b58d
ms.sourcegitcommit: a422b86148cba668c7332e15480c5995ad72fa76
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/30/2020
ms.locfileid: "91578781"
---
# <a name="deploy-and-configure-vmware-hcx"></a>Bereitstellen und Konfigurieren von VMware HCX

In diesem Artikel werden die Verfahren zum Bereitstellen und Konfigurieren der VMware HCX-Plattform für Ihre private Azure VMware Solution-Cloud erläutert. VMware HCX ermöglicht die Migration Ihrer VMware-Workloads zu Azure VMware Solution und zu anderen verbundenen Standorten über verschiedene Migrationstypen.

VMware HCX Advanced (vorab bereitgestellt in Azure VMware Solution) unterstützt bis zu drei Standortverbindungen (lokal mit einer Cloud oder Cloud mit Cloud). Sind mehr als drei Standortverbindungen für VMware HCX Enterprise erforderlich, haben Sie die Möglichkeit, das [VMware HCX Enterprise](https://cloud.vmware.com/community/2019/08/08/introducing-hcx-enterprise/)-Add-on (aktuell in *Vorschau*) zu aktivieren, indem Sie eine [Supportanfrage](https://rc.portal.azure.com/#create/Microsoft.Support) senden. VMware HCX Enterprise Edition (EE) ist mit Azure VMware Solution als *Vorschau*-Funktion/Dienst verfügbar. Solange die VMware HCX EE für Azure VMware Solution in *Vorschau* vorliegt, ist sie eine kostenlose Funktion/ein kostenloser Dienst und unterliegt den Bestimmungen eines Diensts in Vorschau. Sobald der VMware HCX EE-Dienst die Phase „Allgemeine Verfügbarkeit“ erreicht hat, erhalten Sie eine Benachrichtigung, dass in 30 Tagen die Abrechnung umgestellt wird. Sie haben auch die Möglichkeit, den Dienst zu deaktivieren bzw. abzuwählen.

Bevor Sie beginnen, sollten Sie die Abschnitte [Bevor Sie beginnen](#before-you-begin), [Erforderliche Softwareversionen](#software-version-requirements) und [Voraussetzungen](#prerequisites) lesen. 

Danach werden alle erforderlichen Schritte beschrieben:

> [!div class="checklist"]
> * Bereitstellen der lokalen OVA-Datei für VMware HCX (Connector)
> * Aktivieren von VMware HCX
> * Koppeln Ihrer lokalen Umgebung mit Ihrer Azure VMware Solution-Umgebung
> * Konfigurieren der Verbindungen (Compute-Profil, Netzwerkprofil(e) und Service Mesh)
> * Abschließen des Setups durch Überprüfen des Status der Appliance

Nach Abschluss dieser Schritte können Sie die empfohlenen nächsten Schritte am Ende dieses Artikels ausführen.  

## <a name="before-you-begin"></a>Bevor Sie beginnen
   
* Sehen Sie sich die [Tutorialreihe](tutorial-network-checklist.md) mit Grundlagen zu Azure VMware Solution Software Defined Datacenter (SDDC) an.
* Lesen Sie die [VMware-Dokumentation zu HCX](https://docs.vmware.com/en/VMware-HCX/index.html) einschließlich des HCX-Benutzerhandbuchs.
* Lesen Sie die VMware-Dokumentation zum [Migrieren von Virtual Machines mit VMware HCX](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-D0CD0CC6-3802-42C9-9718-6DA5FEC246C6.html?hWord=N4IghgNiBcIBIGEAaACAtgSwOYCcwBcMB7AOxAF8g).
* Optional: Lesen Sie die [Überlegungen zur Bereitstellung von VMware HCX](https://docs.vmware.com/en/VMware-HCX/services/install-checklist/GUID-C0A0E820-D5D0-4A3D-AD8E-EEAA3229F325.html).
* Optional: Lesen Sie weitere VMware-Ressourcen wie die VMware vSphere-[Blogreihe](https://blogs.vmware.com/vsphere/2019/10/cloud-migration-series-part-2.html) zu HCX. 
* Optional: Fordern Sie eine Aktivierung von Azure VMware Solution HCX Enterprise über die Azure VMware Solution-Supportkanäle an.
* Um die WAN-Verbindungsappliance bereitzustellen, sind bestimmte CIDR-Bereiche bereits vom Kunden zugeordnet, wozu `\22` für die Erstellung der privaten Cloud vorausgesetzt wird.

Die Größenanpassung von Workloads hinsichtlich Compute- und Speicherressourcen ist ein wesentlicher Planungsschritt. Nehmen Sie diese Größenanpassung bei der ersten Planung der Umgebung für die private Cloud vor. 

Sie können Workloads auch anpassen, indem Sie eine [Azure VMware Solution-Bewertung](../migrate/how-to-create-azure-vmware-solution-assessment.md) im Azure Migrate-Portal ausführen.

## <a name="software-version-requirements"></a>Erforderliche Softwareversionen

Auf Infrastrukturkomponenten muss die erforderliche Mindestversion ausgeführt werden. 
                                                         
| Komponententyp    | Anforderungen für die Quellumgebung    | Anforderungen für die Zielumgebung   |
| --- | --- | --- |
| vCenter Server   | 5,1<br/><br/>Wenn Sie 5.5 U1 oder früher verwenden, verwenden Sie die eigenständige HCX-Benutzeroberfläche für HCX-Vorgänge.  | 6.0 U2 und höher   |
| ESXi   | 5.0    | ESXi 6.0 und höher   |
| NSX    | Für die HCX-Netzwerkerweiterung logischer Switches an der Quelle: NSXv 6.2+ oder NSX-T 2.4+   | NSXv 6.2+ oder NSX-T 2.4+<br/><br/>Für HCX-Näherungsrouting: NSXv 6.4+ (Näherungsrouting wird für NSX-T nicht unterstützt.) |
| vCloud Director   | Nicht erforderlich, keine Interoperabilität mit vCloud Director am Quellstandort | Bei der Integration der Zielumgebung in vCloud Director ist mindestens 9.1.0.2 erforderlich.  |

## <a name="prerequisites"></a>Voraussetzungen

### <a name="network-and-ports"></a>Netzwerk und Ports

* [ExpressRoute Global Reach](tutorial-expressroute-global-reach-private-cloud.md), das zwischen lokalen Verbindungen und Azure VMware Solution SDDC ExpressRoute-Verbindungen konfiguriert ist.

* Alle erforderlichen Ports für Kommunikation zwischen lokalen Komponenten und lokalen Komponenten mit Azure VMware Solution SDDC müssen offen sein (weitere Informationen finden Sie in der [VMware-Dokumentation zu HCX](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-E456F078-22BE-494B-8E4B-076EF33A9CF4.html)).

* Lokale HCX-IX-und NE-Appliances sollten die vCenter- und ESXi-Infrastruktur erreichen können.

### <a name="ip-addresses"></a>IP-Adressen

[!INCLUDE [hcx-network-segments](includes/hcx-network-segments.md)]
   
## <a name="deploy-the-vmware-hcx-ova-on-premises"></a>Lokales Bereitstellen der OVA-Datei für VMware-HCX

>[!NOTE]
>Bevor Sie das virtuelle Gerät (Appliance) in Ihrer lokalen vCenter-Instanz bereitstellen, müssen Sie die OVA-Datei für VMware HCX herunterladen. 

1. Melden Sie sich über `https://x.x.x.9` Port 443 bei dem Azure VMware Solution HCX-Manager mit den **cloudadmin**-Benutzeranmeldeinformationen an, und navigieren Sie dann zu **Support**.

   >[!TIP]
   >Um die IP-Adresse von HCX-Manager zu ermitteln, navigieren Sie auf dem Blatt „Azure VMware Solution“ zu **Manage** > **Connectivity**, und wählen Sie dann die Registerkarte **HCX** aus. 
   >
   >Das vCenter-Kennwort wurde festgelegt, als Sie die private Cloud eingerichtet haben.

1. Wählen Sie den **Download**-Link aus, um die VMware HCX-OVA-Datei herunterzuladen.

1. Navigieren Sie zum lokalen vCenter, und wählen Sie die OVF-Vorlage aus, die Sie in Ihrem lokalen vCenter bereitstellen möchten. In diesem Fall ist dies die OVA-Datei, die Sie heruntergeladen haben.  

   :::image type="content" source="media/tutorial-vmware-hcx/select-ovf-template.png" alt-text="Navigieren Sie zum lokalen vCenter, und wählen Sie eine OVF-Vorlage aus, um diese in Ihrem lokalen vCenter bereitzustellen." lightbox="media/tutorial-vmware-hcx/select-ovf-template.png":::


1. Wählen Sie einen Namen und einen Speicherort (location) aus, wählen Sie eine Ressource/einen Cluster aus, in der/dem Sie HCX bereitstellen, und überprüfen Sie dann die Details und die erforderlichen Ressourcen.  

   :::image type="content" source="media/tutorial-vmware-hcx/configure-template.png" alt-text="Navigieren Sie zum lokalen vCenter, und wählen Sie eine OVF-Vorlage aus, um diese in Ihrem lokalen vCenter bereitzustellen." lightbox="media/tutorial-vmware-hcx/configure-template.png":::

1. Lesen Sie die Lizenzbedingungen, stimmen Sie diesen zu, wählen Sie den erforderlichen Speicher und das erforderliche Netzwerk aus, und wählen Sie dann **Next** (Weiter) aus.

1. Geben Sie unter **Customize template** (Vorlage anpassen) alle erforderlichen Informationen ein. 

   :::image type="content" source="media/tutorial-vmware-hcx/customize-template.png" alt-text="Navigieren Sie zum lokalen vCenter, und wählen Sie eine OVF-Vorlage aus, um diese in Ihrem lokalen vCenter bereitzustellen." lightbox="media/tutorial-vmware-hcx/customize-template.png":::

1. Wählen Sie **Next** (Weiter) aus, überprüfen Sie die Konfiguration, und wählen Sie dann **Finish** (Fertig stellen) aus, um die OVA-Datei für HCX bereitzustellen.
     
   >[!NOTE]
   >Üblicherweise wird der VMware HCX Manager, den Sie jetzt bereitstellen, im Verwaltungsnetzwerk des Clusters bereitgestellt.  
   
   > [!IMPORTANT]
   > Nachdem die Bereitstellung abgeschlossen ist, müssen Sie das virtuelle Gerät möglicherweise manuell einschalten.
   > Warten Sie nach dem Einschalten des HCX-Geräts (Appliance) 10 bis 15 Minuten, bis Sie zum nächsten Schritt wechseln.

Wenn Sie sich eine End-to-End-Übersicht dieses Schritts ansehen möchten, zeigen Sie das Video [Azure VMware Solution: VMware HCX Appliance Deployment](https://www.youtube.com/embed/BwSnQeefnso) an. 


## <a name="activate-vmware-hcx"></a>Aktivieren von VMware HCX

Nachdem Sie die OVA-Datei für VMware HCX lokal bereitgestellt haben, können Sie VMware HCX aktivieren. Bevor Sie VMware HCX aktivieren können, müssen Sie eine Lizenz abrufen.

1. Navigieren Sie in Azure VMware Solution um **Manage** > **Connectivity** (Konnektivität verwalten), wählen Sie die Registerkarte **HCX** aus, und wählen Sie dann **Add**  aus.

1. Melden Sie sich beim lokalen VMware HCX Manager unter `https://HCXManagerIP:9443` an, indem Sie die Benutzeranmeldeinformationen für **admin** eingeben. 

   > [!IMPORTANT]
   > Geben Sie die Portnummer `9443` mit der IP-Adresse des VMware HCX Managers an.

1. Geben Sie unter **Licensing** (Lizenzierung) **den erweiterten HCX-Schlüssel** ein.  
   
    > [!NOTE]
    > Für den VMware HCX Manager muss offener Internetzugriff möglich oder ein Proxy konfiguriert sein.

1. Geben Sie unter **Datacenter Location** (Standort des Rechenzentrums) den nächstgelegenen Standort an, in dem Sie den VMware HCX Manager lokal installieren.

1. Ändern Sie **System Name**, oder übernehmen Sie die Standardeinstellung.
   
1. Sie können den Vorgang entweder später beenden (Finish Later) oder fortsetzen (Continue). Wählen Sie die Option **Yes, Continue** aus, um den Vorgang fortzusetzen.
    
1. Geben Sie in **Connect your vCenter** (Mit vCenter verbinden) den FQDN oder die IP-Adresse Ihres vCenter-Servers und die entsprechenden Anmeldeinformationen an, und wählen Sie dann **Continue** aus.
   
1. Geben Sie in **Configure SSO/PSC** (SSO/PSC konfigurieren) den FQDN oder die IP-Adresse Ihres PSC an, und wählen Sie dann **Continue** aus.
   
   >[!NOTE]
   >Ist in der Regel mit dem FQDN oder der IP-Adresse Ihres vCenter-Servers identisch.

1. Vergewissern Sie sich, dass alle Eingaben korrekt sind, und wählen Sie **Restart** (Neu starten) aus.
    
   > [!NOTE]
   > Nach dem Neustart tritt eine Verzögerung auf, bevor Sie zum Wechsel zum nächsten Schritt aufgefordert werden.
   >
   >Nachdem der Dienst neu gestartet wurde, muss vCenter auf dem angezeigten Bildschirm mit grünem Punkt angezeigt werden. Sowohl vCenter als auch SSO haben die geeigneten Konfigurationsparameter, die mit denen im vorherigen Bildschirm identisch sein sollten.

   :::image type="content" source="media/tutorial-vmware-hcx/activation-done.png" alt-text="Navigieren Sie zum lokalen vCenter, und wählen Sie eine OVF-Vorlage aus, um diese in Ihrem lokalen vCenter bereitzustellen." lightbox="media/tutorial-vmware-hcx/activation-done.png":::  

Wenn Sie sich eine End-to-End-Übersicht dieses Schritts ansehen möchten, zeigen Sie das Video [Azure VMware Solution: Activate HCX](https://www.youtube.com/embed/BkAV_TNYxdE) an.


## <a name="configure-vmware-hcx"></a>Konfigurieren von VMware HCX

Nun können Sie eine Standortkopplung (Site Pairing) hinzufügen, ein Netzwerk- und Compute-Profil erstellen und Dienste wie Migration, Netzwerkerweiterung (Network Extension) oder Notfallwiederherstellung (Disaster Recovery) aktivieren. 

### <a name="add-a-site-pairing"></a>Hinzufügen einer Standortkopplung

Sie können den VMware HCX Manager in Azure VMware Solution mit dem VMware HCX Manager in Ihrem Rechenzentrum verbinden (koppeln). 

1. Melden Sie sich bei Ihrem lokalen vCenter an, und wählen Sie unter **Home** die Option **HCX** aus.

   :::image type="content" source="media/tutorial-vmware-hcx/vcenter-vmware-hcx.png" alt-text="Navigieren Sie zum lokalen vCenter, und wählen Sie eine OVF-Vorlage aus, um diese in Ihrem lokalen vCenter bereitzustellen." lightbox="media/tutorial-vmware-hcx/vcenter-vmware-hcx.png":::

1. Wählen Sie unter **Infrastructure** die Option **Site Pairing** (Standortkopplung) aus, und wählen Sie dann die Option **Connect To Remote Site** (Verbindung mit Remotestandort herstellen) aus (in der Mitte des Bildschirms). 

   :::image type="content" source="media/tutorial-vmware-hcx/connect-remote-site.png" alt-text="Navigieren Sie zum lokalen vCenter, und wählen Sie eine OVF-Vorlage aus, um diese in Ihrem lokalen vCenter bereitzustellen." lightbox="media/tutorial-vmware-hcx/connect-remote-site.png":::

1. Geben Sie die **Remote HCX URL or IP address** und die Azure VMware Solution-Anmeldeinformationen (cloudadmin@vsphere.local-Benutzername und **password**) ein, und wählen Sie **Connect** aus.

   > [!NOTE]
   > Die **Remote-HCX URL** ist die URL des HCX Managers Ihrer privaten Azure VMware Solution-Cloud, üblicherweise die „.9“-Adresse des Verwaltungsnetzwerks.  Hat Ihr vCenter beispielsweise die IP-Adresse 192.168.4.2, ist Ihre HCX-URL gleich 192.168.4.9.
   >
   > Das **password** ist mit dem Kennwort identisch, mit dem Sie sich bei vCenter angemeldet haben. Sie haben dieses Kennwort auf dem ersten Bereitstellungsbildschirm angegeben.

   Es wird eine Darstellung angezeigt, in der zu sehen ist, dass Ihr HCX Manager in Azure VMware Solution und Ihr lokaler HCX Manager verbunden (gekoppelt) sind.

   :::image type="content" source="media/tutorial-vmware-hcx/site-pairing-complete.png" alt-text="Navigieren Sie zum lokalen vCenter, und wählen Sie eine OVF-Vorlage aus, um diese in Ihrem lokalen vCenter bereitzustellen.":::

Wenn Sie sich eine End-to-End-Übersicht dieses Schritts ansehen möchten, zeigen Sie das Video [Azure VMware Solution: HCX Site Pairing](https://www.youtube.com/embed/sKizDCRHOko) an.



### <a name="create-network-profiles"></a>Erstellen von Netzwerkprofilen

VMware HCX stellt einige virtuelle Geräte bereit (automatisiert), erfordert jedoch mehrere IP-Segmente.  Wenn Sie Ihre Netzwerkprofile erstellen, definieren Sie die IP-Segmente, die Sie während der [Vorbereitungs-und Planungsphase für die Bereitstellung der VMware HCX-Netzwerksegmente](production-ready-deployment-steps.md#vmware-hcx-network-segments) ermittelt haben.

Sie erstellen vier Netzwerkprofile:

   - Verwaltung
   - vMotion
   - Replikation
   - Uplink

1. Wählen Sie unter **Infrastruktur** die Option **Interconnect** > **Multi-Site Service Mesh (Dienstnetz mit mehreren Standorten)**  > **Network Profiles (Netzwerkprofile)**  > **Create Network Profile (Netzwerkprofil erstellen)** aus.

   :::image type="content" source="media/tutorial-vmware-hcx/network-profile-start.png" alt-text="Navigieren Sie zum lokalen vCenter, und wählen Sie eine OVF-Vorlage aus, um diese in Ihrem lokalen vCenter bereitzustellen." lightbox="media/tutorial-vmware-hcx/network-profile-start.png":::

1. Wählen Sie für jedes Netzwerkprofil das Netzwerk und die Portgruppe aus, geben Sie einen Namen an, erstellen Sie den IP-Pool für dieses bestimmte Segment, und wählen Sie dann **Create** aus. 

   :::image type="content" source="media/tutorial-vmware-hcx/example-configurations-network-profile.png" alt-text="Navigieren Sie zum lokalen vCenter, und wählen Sie eine OVF-Vorlage aus, um diese in Ihrem lokalen vCenter bereitzustellen.":::

Wenn Sie sich eine End-to-End-Übersicht dieses Schritts ansehen möchten, zeigen Sie das Video [Azure VMware Solution: HCX Network Profile](https://www.youtube.com/embed/NhyEcLco4JY) an.


### <a name="create-compute-profile"></a>Compute-Profil erstellen

1. Wählen Sie **Compute Profiles** (Compute-Profile) > **Create Compute Profile** (Compute-Profil erstellen) aus.

   :::image type="content" source="media/tutorial-vmware-hcx/compute-profile-create.png" alt-text="Navigieren Sie zum lokalen vCenter, und wählen Sie eine OVF-Vorlage aus, um diese in Ihrem lokalen vCenter bereitzustellen." lightbox="media/tutorial-vmware-hcx/compute-profile-create.png":::

1. Geben Sie einen Namen für das Profil ein, und wählen Sie **Continue** (Weiter) aus.  

   :::image type="content" source="media/tutorial-vmware-hcx/name-compute-profile.png" alt-text="Navigieren Sie zum lokalen vCenter, und wählen Sie eine OVF-Vorlage aus, um diese in Ihrem lokalen vCenter bereitzustellen." lightbox="media/tutorial-vmware-hcx/name-compute-profile.png":::

1. Wählen Sie die Dienste aus, die aktiviert werden sollen, z. B. für Migration, Netzwerkerweiterung oder Notfallwiederherstellung, und wählen Sie **Weiter** aus.
  
   > [!NOTE]
   > Üblicherweise wird hier nichts geändert.

1. Wählen Sie unter **Select Service Resources** (Dienstressourcen auswählen) die Dienstressourcen (Cluster) aus, die für die ausgewählten VMware HCX-Dienste aktiviert werden sollen.  

1. Wenn Sie die Cluster in Ihrem lokalen Rechenzentrum sehen, wählen Sie **Continue** aus.

   :::image type="content" source="media/tutorial-vmware-hcx/select-service-resource.png" alt-text="Navigieren Sie zum lokalen vCenter, und wählen Sie eine OVF-Vorlage aus, um diese in Ihrem lokalen vCenter bereitzustellen." lightbox="media/tutorial-vmware-hcx/select-service-resource.png":::

1. Wählen Sie unter **Select Datastore** (Datenspeicher auswählen) die Datenspeicher-Speicherressource für die Bereitstellung der VMware HCX Interconnect-Appliances aus, und wählen Sie dann **Continue** aus.

   Wenn mehrere Ressourcen ausgewählt sind, verwendet VMware HCX die erste ausgewählte Ressource, bis deren Kapazität erschöpft ist.   

   :::image type="content" source="media/tutorial-vmware-hcx/deployment-resources-and-reservations.png" alt-text="Navigieren Sie zum lokalen vCenter, und wählen Sie eine OVF-Vorlage aus, um diese in Ihrem lokalen vCenter bereitzustellen." lightbox="media/tutorial-vmware-hcx/deployment-resources-and-reservations.png":::  

1. Wählen Sie unter **Select Management Network Profile** (Verwaltungsnetzwerkprofil auswählen) das Verwaltungsnetzwerkprofil aus, das Sie in den vorherigen Schritten erstellt haben, und wählen Sie dann **Continue** aus.  

   :::image type="content" source="media/tutorial-vmware-hcx/select-management-network-profile.png" alt-text="Navigieren Sie zum lokalen vCenter, und wählen Sie eine OVF-Vorlage aus, um diese in Ihrem lokalen vCenter bereitzustellen." lightbox="media/tutorial-vmware-hcx/select-management-network-profile.png":::

   > [!NOTE]
   > Das Verwaltungsnetzwerkprofil ermöglicht es den VMware HCX-Appliances, mit vCenter zu kommunizieren, und die ESXi-Hosts können erreicht werden.

1. Wählen Sie unter **Select Uplink Network Profile** (Uplink-Netzwerkprofil auswählen) das Uplink-Netzwerkprofil aus, das Sie in den vorherigen Schritten erstellt haben, und wählen Sie dann **Continue** aus.

   :::image type="content" source="media/tutorial-vmware-hcx/select-uplink-network-profile.png" alt-text="Navigieren Sie zum lokalen vCenter, und wählen Sie eine OVF-Vorlage aus, um diese in Ihrem lokalen vCenter bereitzustellen." lightbox="media/tutorial-vmware-hcx/select-uplink-network-profile.png":::

1. Wählen Sie unter **Select vMotion Network Profile** (vMotion-Netzwerkprofil auswählen) das vMotion-Netzwerkprofil aus, das Sie in den vorherigen Schritten erstellt haben, und wählen Sie dann **Continue** aus.

   :::image type="content" source="media/tutorial-vmware-hcx/select-vmotion-network-profile.png" alt-text="Navigieren Sie zum lokalen vCenter, und wählen Sie eine OVF-Vorlage aus, um diese in Ihrem lokalen vCenter bereitzustellen." lightbox="media/tutorial-vmware-hcx/select-vmotion-network-profile.png":::

1. Wählen Sie unter **Select vSphere Replication Network Profile** (Netzwerkprofil für die vSphere-Replikation auswählen) das Replikationsnetzwerkprofil aus, das Sie in den vorherigen Schritten erstellt haben, und wählen Sie dann **Continue** aus.

   In den meisten Fällen ist das Replikationsnetzwerkprofil mit dem Verwaltungsnetzwerkprofil identisch.  

   :::image type="content" source="media/tutorial-vmware-hcx/select-replication-network-profile.png" alt-text="Navigieren Sie zum lokalen vCenter, und wählen Sie eine OVF-Vorlage aus, um diese in Ihrem lokalen vCenter bereitzustellen." lightbox="media/tutorial-vmware-hcx/select-replication-network-profile.png":::

1. Wählen Sie unter **Select Distributed Switches for Network Extensions** (Verteilte Switches für Network Extensions auswählen) die verteilten virtuellen Switches, die die virtuellen Computer, die zu Azure VMware Solution migriert werden sollen, in einem erweiterten Schicht-2-Netzwerk enthalten, und wählen Sie dann **Continue** aus.

   :::image type=" content" source="media/tutorial-vmware-hcx/select-layer-2-distributed-virtual-switch.png" alt-text="Navigieren Sie zum lokalen vCenter, und wählen Sie eine OVF-Vorlage aus, um diese in Ihrem lokalen vCenter bereitzustellen." lightbox="media/tutorial-vmware-hcx/select-layer-2-distributed-virtual-switch.png":::

1. Überprüfen Sie die Verbindungsregeln, und wählen Sie **Continue** (Weiter) aus.  

   :::image type="content" source="media/tutorial-vmware-hcx/review-connection-rules.png" alt-text="Navigieren Sie zum lokalen vCenter, und wählen Sie eine OVF-Vorlage aus, um diese in Ihrem lokalen vCenter bereitzustellen." lightbox="media/tutorial-vmware-hcx/review-connection-rules.png":::

1. Wählen Sie **Finish** (Fertig stellen) aus, um das Compute-Profil zu erstellen.

   Daraufhin wird ein Bildschirm angezeigt, der dem nachstehend gezeigten Bildschirm ähnelt.

   :::image type="content" source="media/tutorial-vmware-hcx/compute-profile-done.png" alt-text="Navigieren Sie zum lokalen vCenter, und wählen Sie eine OVF-Vorlage aus, um diese in Ihrem lokalen vCenter bereitzustellen." lightbox="media/tutorial-vmware-hcx/compute-profile-done.png":::

Wenn Sie sich eine End-to-End-Übersicht dieses Schritts ansehen möchten, zeigen Sie das Video [Azure VMware Solution: Compute Profile](https://www.youtube.com/embed/qASXi5xrFzM) an.




### <a name="create-service-mesh"></a>Erstellen des Dienstnetzes (Service Mesh)

Im nächsten Schritt konfigurieren Sie das Dienstnetz zwischen dem lokalen Standort und Azure VMware Solution SDDC.

1. Wählen Sie unter **Infrastructure** (Infrastruktur) die Option **Interconnect** (Verbinden) > **Service Mesh** (Dienstnetz) > **Create Service Mesh** (Dienstnetz erstellen) aus.    

   :::image type="content" source="media/tutorial-vmware-hcx/create-service-mesh.png" alt-text="Navigieren Sie zum lokalen vCenter, und wählen Sie eine OVF-Vorlage aus, um diese in Ihrem lokalen vCenter bereitzustellen." lightbox="media/tutorial-vmware-hcx/create-service-mesh.png":::

1. Überprüfen Sie die vorab aufgefüllten Websites, und wählen Sie dann **Continue** aus. 

   >[!NOTE]
   >Wenn dies Ihre erste Dienstnetzkonfiguration ist, müssen Sie diesen Bildschirm nicht ändern.  

1. Wählen Sie das Dropdownfeld für das Quellcomputeprofil und das Remotecomputeprofil aus, und wählen Sie dann **Continue** (Weiter) aus.  

   Mit der Auswahl werden die Ressourcen definiert, in denen VMs VMware HCX-Dienste nutzen können.  

   :::image type="content" source="media/tutorial-vmware-hcx/select-compute-profile-source.png" alt-text="Navigieren Sie zum lokalen vCenter, und wählen Sie eine OVF-Vorlage aus, um diese in Ihrem lokalen vCenter bereitzustellen." lightbox="media/tutorial-vmware-hcx/select-compute-profile-source.png":::

   :::image type="content" source="media/tutorial-vmware-hcx/select-compute-profile-remote.png" alt-text="Navigieren Sie zum lokalen vCenter, und wählen Sie eine OVF-Vorlage aus, um diese in Ihrem lokalen vCenter bereitzustellen." lightbox="media/tutorial-vmware-hcx/select-compute-profile-remote.png":::

1. Überprüfen Sie die Dienste, die aktiviert werden, und wählen Sie **Continue** aus.  

1. Wählen Sie unter **Advanced Configuration - Override Uplink Network profiles** (Erweiterte Konfiguration – Uplink-Netzwerkprofile überschreiben)die Option **Continue** (Weiter) aus.  Uplink-Netzwerkprofile stellen Verbindungen mit dem Netzwerk her, über das die Interconnect-Appliances des Remote-Standorts erreicht werden können.  
  
1. Überprüfen Sie die Einstellungen in **Advanced Configuration – Network Extension Appliance Scale Out** (Erweiterte Konfiguration – Aufskalierung für Network Extension-Appliance konfigurieren), und wählen Sie **Continue** aus. 

1. Überprüfen Sie die Einstellungen in **Advanced Configuration – Traffic Engineering** (Erweiterte Konfiguration – Verwaltung des Datenverkehrs), nehmen Sie alle erforderlichen Änderungen vor, und wählen Sie dann **Continue** aus.

1. Überprüfen Sie die Topologievorschau, und wählen Sie **Continue** (Weiter) aus.

1. Geben Sie einen benutzerfreundlichen Namen für dieses Dienstnetz ein, und wählen Sie **Finish** (Fertig stellen) aus, um den Vorgang abzuschließen.  

1. Wählen Sie **View Tasks** (Aufgaben anzeigen) aus, um die Bereitstellung zu überwachen. 

   :::image type="content" source="media/tutorial-vmware-hcx/monitor-service-mesh.png" alt-text="Navigieren Sie zum lokalen vCenter, und wählen Sie eine OVF-Vorlage aus, um diese in Ihrem lokalen vCenter bereitzustellen.":::

   Wenn die Dienstnetzbereitstellung erfolgreich abgeschlossen wurde, werden die Dienste mit grüner Kennzeichnung angezeigt.

   :::image type="content" source="media/tutorial-vmware-hcx/service-mesh-green.png" alt-text="Navigieren Sie zum lokalen vCenter, und wählen Sie eine OVF-Vorlage aus, um diese in Ihrem lokalen vCenter bereitzustellen." lightbox="media/tutorial-vmware-hcx/service-mesh-green.png":::

1. Überprüfen Sie die Integrität des Dienstnetzes, indem Sie den Appliancestatus überprüfen. Wählen Sie dazu **Interconnect** (Verbinden) > **Appliances** aus.

   :::image type="content" source="media/tutorial-vmware-hcx/interconnect-appliance-state.png" alt-text="Navigieren Sie zum lokalen vCenter, und wählen Sie eine OVF-Vorlage aus, um diese in Ihrem lokalen vCenter bereitzustellen." lightbox="media/tutorial-vmware-hcx/interconnect-appliance-state.png":::

Wenn Sie sich eine End-to-End-Übersicht dieses Schritts ansehen möchten, zeigen Sie das Video [Azure VMware Solution: Service Mesh](https://www.youtube.com/embed/FyZ0d3P_T24) an.



### <a name="optional-create-a-network-extension"></a>(Optional) Erstellen einer Netzwerkerweiterung

Wenn Sie Netzwerke aus Ihrer lokalen Umgebung um Azure VMware Solution erweitern möchten, führen Sie die folgenden Schritte aus, um diese Netzwerke zu erweitern.

1. Wählen Sie unter **Services** (Dienste) die Option **Network Extension** (Netzwerkerweiterung) aus, und wählen Sie dann **Create a Network Extension** (Netzwerkerweiterung erstellen) aus.

   :::image type="content" source="media/tutorial-vmware-hcx/create-network-extension.png" alt-text="Navigieren Sie zum lokalen vCenter, und wählen Sie eine OVF-Vorlage aus, um diese in Ihrem lokalen vCenter bereitzustellen." lightbox="media/tutorial-vmware-hcx/create-network-extension.png":::

1. Wählen Sie jedes der Netzwerke aus, die Sie um Azure VMware Solution erweitern möchten, und wählen Sie dann **Next** aus.

   :::image type="content" source="media/tutorial-vmware-hcx/select-extend-networks.png" alt-text="Navigieren Sie zum lokalen vCenter, und wählen Sie eine OVF-Vorlage aus, um diese in Ihrem lokalen vCenter bereitzustellen.":::

1. Geben Sie die lokale Gateway-IP-Adresse für jedes der Netzwerke ein, die Sie erweitern möchten, ein, und wählen Sie dann **Submit** (Senden) aus. 

   :::image type="content" source="media/tutorial-vmware-hcx/extend-networks-gateway.png" alt-text="Navigieren Sie zum lokalen vCenter, und wählen Sie eine OVF-Vorlage aus, um diese in Ihrem lokalen vCenter bereitzustellen.":::

   Es dauert einige Minuten, bis die Netzwerkerweiterung abgeschlossen ist. Sobald dies der Fall ist, erfolgt die Statusänderung in **Extension complete** (Erweiterung abgeschlossen).

   :::image type="content" source="media/tutorial-vmware-hcx/extension-complete.png" alt-text="Navigieren Sie zum lokalen vCenter, und wählen Sie eine OVF-Vorlage aus, um diese in Ihrem lokalen vCenter bereitzustellen." lightbox="media/tutorial-vmware-hcx/extension-complete.png":::

Wenn Sie sich eine End-to-End-Übersicht dieses Schritts ansehen möchten, zeigen Sie das Video [Azure VMware Solution: Network Extension](https://www.youtube.com/embed/cNlp0f_tTr0) an.


## <a name="next-steps"></a>Nächste Schritte

Wenn Sie diesen Punkt erreicht haben und der „Tunnel Status“ (Tunnelstatus) der Appliance-Verbindung als **UP** und grün angezeigt wird, können Sie Azure VMware Solution-VMs mit VMware HCX migrieren und schützen.  Azure VMware Solution unterstützt Workloadmigrationen (mit oder ohne Netzwerkerweiterung).  Sie können weiterhin Workloadmigrationen in ihrer vSphere-Umgebung ausführen, Netzwerke lokal erstellen und VMs in diesen Netzwerken bereitstellen.  Weitere Informationen zum Verwenden von HCX finden Sie in der [VMware-Dokumentation zu HCX](https://docs.vmware.com/en/VMware-HCX/index.html) und im Abschnitt zum [Migrieren von VMs mit VMware HCX](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-D0CD0CC6-3802-42C9-9718-6DA5FEC246C6.html?hWord=N4IghgNiBcIBIGEAaACAtgSwOYCcwBcMB7AOxAF8g) in der technischen Dokumentation zu VMware.
