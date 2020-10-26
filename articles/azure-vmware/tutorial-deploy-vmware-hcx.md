---
title: 'Tutorial: Bereitstellen und Konfigurieren von VMware HCX'
description: Erfahren Sie, wie Sie eine VMware HCX-Lösung für Ihre private Azure VMware Solution-Cloud bereitstellen und konfigurieren.
ms.topic: tutorial
ms.date: 10/16/2020
ms.openlocfilehash: c78eae11497702054bb54b5980228fd0a3962577
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2020
ms.locfileid: "92367770"
---
# <a name="deploy-and-configure-vmware-hcx"></a>Bereitstellen und Konfigurieren von VMware HCX

In diesem Artikel werden die Verfahren zum Bereitstellen und Konfigurieren des lokalen VMware HCX-Connectors für Ihre private Azure VMware Solution-Cloud erläutert. Mit VMware HCX können Sie Ihre VMware-Workloads zu Azure VMware Solution und zu anderen verbundenen Standorten über verschiedene Migrationstypen migrieren. Da Azure VMware Solution den HCX-Cloud-Manager bereitstellt und konfiguriert, müssen Sie den HCX-Connector in Ihr lokales VMware-Rechenzentrum herunterladen und ihn aktivieren und konfigurieren.

VMware HCX Advanced Connector wird in Azure VMware Solution vorab bereitgestellt. Er unterstützt bis zu drei Standortverbindungen (lokal-zu-Cloud oder Cloud-zu-Cloud). Wenn Sie mehr als drei Standortverbindungen benötigen, senden Sie eine [Supportanfrage](https://rc.portal.azure.com/#create/Microsoft.Support), um das Add-On [VMware HCX Enterprise](https://cloud.vmware.com/community/2019/08/08/introducing-hcx-enterprise/) zu aktivieren. Das Add-On befindet sich derzeit in der Vorschauphase. 

>[!NOTE]
>VMware HCX Enterprise Edition (EE) ist mit Azure VMware Solution als Vorschaudienst verfügbar. Der Dienst ist kostenlos und unterliegt den Nutzungsbedingungen für einen Vorschaudienst. Nachdem der VMware HCX EE-Dienst allgemein verfügbar ist, erhalten Sie eine Benachrichtigung, dass in 30 Tagen die Abrechnung umgestellt wird. Sie haben auch die Möglichkeit, den Dienst zu deaktivieren bzw. abzuwählen.

Lesen Sie zunächst die Informationen in den Abschnitten [Voraussetzungen (Vorbereitung)](#before-you-begin), [Erforderliche Softwareversionen](#software-version-requirements) und [Voraussetzungen](#prerequisites) in diesem Artikel. 

Danach werden alle erforderlichen Schritte beschrieben:

> [!div class="checklist"]
> * Bereitstellen der lokalen OVA-Datei für VMware HCX (HCX-Connector).
> * Aktivieren des VMware HCX-Connectors.
> * Koppeln Ihres lokalen HCX-Connectors mit dem Azure VMware Solution HCX-Cloud-Manager.
> * Konfigurieren der Verbindungen (Netzwerkprofil, Computeprofil und Service Mesh).
> * Abschließen des Setups, indem Sie den Gerätestatus überprüfen und prüfen, ob die Migration möglich ist.

Nach Abschluss dieser Schritte können Sie die empfohlenen nächsten Schritte am Ende dieses Artikels ausführen.  

## <a name="before-you-begin"></a>Bevor Sie beginnen
   
* Sehen Sie sich die [Tutorialreihe](tutorial-network-checklist.md) mit Grundlagen zu Azure VMware Solution Software Defined Datacenter (SDDC) an.
* Lesen Sie die [VMware-Dokumentation zu HCX](https://docs.vmware.com/en/VMware-HCX/index.html) einschließlich des HCX-Benutzerhandbuchs.
* Lesen Sie die VMware-Dokumentation zum [Migrieren von Virtual Machines mit VMware HCX](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-D0CD0CC6-3802-42C9-9718-6DA5FEC246C6.html?hWord=N4IghgNiBcIBIGEAaACAtgSwOYCcwBcMB7AOxAF8g).
* Optional: Lesen Sie die [Überlegungen zur Bereitstellung von VMware HCX](https://docs.vmware.com/en/VMware-HCX/services/install-checklist/GUID-C0A0E820-D5D0-4A3D-AD8E-EEAA3229F325.html).
* Optional: Lesen Sie weitere VMware-Ressourcen wie die VMware vSphere-[Blogreihe](https://blogs.vmware.com/vsphere/2019/10/cloud-migration-series-part-2.html). 
* Optional: Fordern Sie eine Aktivierung von Azure VMware Solution HCX Enterprise über die Azure VMware Solution-Supportkanäle an.
* Optional: [Überprüfen Sie die für HCX erforderlichen Netzwerkports](https://ports.vmware.com/home/VMware-HCX)
* Obwohl der Azure VMware Solution HCX-Cloud-Manager vorkonfiguriert aus dem /22-Netzwerk stammt, das für die private Azure VMware Solution-Cloud bereitgestellt wird, müssen Sie für den lokalen HCX-Connector Netzwerkbereiche aus Ihrem lokalen Netzwerk zuweisen. Diese Netzwerke und Bereiche werden weiter unten in diesem Artikel beschrieben.

Die Größenanpassung von Workloads hinsichtlich Compute- und Speicherressourcen ist ein wesentlicher Planungsschritt. Nehmen Sie diese Größenanpassung bei der ersten Planung der Umgebung für die private Cloud vor. 

Sie können Workloads anpassen, indem Sie eine [Azure VMware Solution-Bewertung](../migrate/how-to-create-azure-vmware-solution-assessment.md) im Azure Migrate-Portal ausführen.

## <a name="software-version-requirements"></a>Erforderliche Softwareversionen

Auf Infrastrukturkomponenten muss die erforderliche Mindestversion ausgeführt werden. 
                                                         
| Komponententyp    | Anforderungen für die Quellumgebung    | Anforderungen für die Zielumgebung   |
| --- | --- | --- |
| vCenter Server   | 5,1<br/><br/>Wenn Sie 5.5 U1 oder früher verwenden, verwenden Sie die eigenständige HCX-Benutzeroberfläche für HCX-Vorgänge.  | 6.0 U2 und höher   |
| ESXi   | 5.0    | ESXi 6.0 und höher   |
| NSX    | Für die HCX-Netzwerkerweiterung logischer Switches an der Quelle: NSXv 6.2 oder höher oder NSX-T 2.4 oder höher.   | NSXv 6.2+ oder NSX-T 2.4+<br/><br/>Für HCX-Näherungsrouting: NSXv 6.4 oder höher. (Näherungsrouting wird für NSX-T nicht unterstützt.) |
| vCloud Director   | Nicht erforderlich. Es gibt keine Interoperabilität mit vCloud Director am Quellstandort. | Wenn Sie die Zielumgebung in vCloud Director integrieren, ist mindestens 9.1.0.2 erforderlich.  |

## <a name="prerequisites"></a>Voraussetzungen

### <a name="network-and-ports"></a>Netzwerk und Ports

* Konfigurieren Sie [ExpressRoute Global Reach](tutorial-expressroute-global-reach-private-cloud.md) zwischen lokalen Verbindungen und Azure VMware Solution SDDC ExpressRoute-Verbindungen.

* [Alle erforderlichen Ports](https://ports.vmware.com/home/VMware-HCX) für die Kommunikation zwischen lokalen Komponenten und Azure VMware Solution SDDC müssen geöffnet sein.

Weitere Informationen finden Sie in der [VMware HCX-Dokumentation](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-E456F078-22BE-494B-8E4B-076EF33A9CF4.html).


### <a name="ip-addresses"></a>IP-Adressen

[!INCLUDE [hcx-network-segments](includes/hcx-network-segments.md)]
   
## <a name="deploy-the-vmware-hcx-connector-ova-on-premises"></a>Lokales Bereitstellen der OVA-Datei für den VMware HCX-Connector

> [!NOTE]
> Bevor Sie das virtuelle Gerät (Appliance) in Ihrer lokalen vCenter-Instanz bereitstellen, müssen Sie die OVA-Datei für den VMware HCX-Connector herunterladen. 

1. Öffnen Sie ein Browserfenster, melden Sie sich über `https://x.x.x.9` und Port 443 bei dem Azure VMware Solution HCX-Manager mit den **cloudadmin**-Benutzeranmeldeinformationen an, und navigieren Sie dann zu **Support**.

   > [!TIP]
   > Notieren Sie sich die IP-Adresse des HCX-Cloud-Managers in Azure VMware Solution. Um die IP-Adresse zu ermitteln, navigieren Sie im Bereich „Azure VMware Solution“ zu **Manage** > **Connectivity**, und wählen Sie dann die Registerkarte **HCX** aus. 
   >
   >Das vCenter-Kennwort wurde festgelegt, als Sie die private Cloud eingerichtet haben.

1. Wählen Sie den Link **Download** aus, um die OVA-Datei des VMware HCX-Connectors herunterzuladen.

1. Navigieren Sie zur lokalen vCenter-Instanz. Wählen Sie die OVF-Vorlage (die von Ihnen heruntergeladene OVA-Datei) aus, um den HCX-Connector in Ihrer lokalen vCenter-Instanz bereitzustellen.  

   :::image type="content" source="media/tutorial-vmware-hcx/select-ovf-template.png" alt-text="Screenshot: Navigieren zu einer OVF-Vorlage." lightbox="media/tutorial-vmware-hcx/select-ovf-template.png":::


1. Wählen Sie einen Namen und einen Speicherort aus, und wählen Sie dann eine Ressource bzw. einen Cluster aus, in der oder der HCX-Connector bereitgestellt werden soll. Überprüfen Sie anschließend die Details und die erforderlichen Ressourcen.  

   :::image type="content" source="media/tutorial-vmware-hcx/configure-template.png" alt-text="Screenshot: Navigieren zu einer OVF-Vorlage." lightbox="media/tutorial-vmware-hcx/configure-template.png":::

1. Lesen Sie die Lizenzbedingungen. Wenn Sie diesen zustimmen, wählen Sie den erforderlichen Speicher und das erforderliche Netzwerk aus, und wählen Sie dann **Next** (Weiter) aus.

1. Geben Sie unter **Customize template** (Vorlage anpassen) alle erforderlichen Informationen ein. 

   :::image type="content" source="media/tutorial-vmware-hcx/customize-template.png" alt-text="Screenshot: Navigieren zu einer OVF-Vorlage." lightbox="media/tutorial-vmware-hcx/customize-template.png":::

1. Wählen Sie **Next** (Weiter) aus, überprüfen Sie die Konfiguration, und wählen Sie dann **Finish** (Fertig stellen) aus, um die OVA-Datei für den HCX-Connector bereitzustellen.
     
   > [!NOTE]
   > In der Regel wird der VMware HCX-Connector, den Sie jetzt bereitstellen, im Verwaltungsnetzwerk des Clusters bereitgestellt.  
   
   > [!IMPORTANT]
   > Möglicherweise müssen Sie das virtuelle Gerät manuell einschalten.  Wenn dies der Fall ist, warten Sie 10 bis 15 Minuten, bevor Sie mit dem nächsten Schritt fortfahren.

Eine umfassende Übersicht über diese Vorgehensweise vermittelt das Video [Azure VMware Solution: Bereitstellung der HCX-Appliance](https://www.youtube.com/embed/BwSnQeefnso). 


## <a name="activate-vmware-hcx"></a>Aktivieren von VMware HCX

Nachdem Sie den VMware HCX-Connector lokal bereitgestellt und die Appliance gestartet haben, sind Sie bereit für die Aktivierung. Zunächst müssen Sie einen Lizenzschlüssel aus dem Azure VMware Solution-Portal abrufen.

1. Navigieren Sie im Azure VMware Solution-Portal zu **Manage** > **Connectivity** (Verwalten > Konnektivität), wählen Sie die Registerkarte **HCX** aus, und wählen Sie dann **Add** (Hinzufügen) aus.

1. Verwenden Sie die **Administrator**anmeldeinformationen, um sich beim lokalen VMware-HCX-Manager unter `https://HCXManagerIP:9443` anzumelden. 

   > [!IMPORTANT]
   > Geben Sie die Portnummer `9443` mit der IP-Adresse des VMware HCX Managers an.

1. Geben Sie unter **Licensing** (Lizenzierung) Ihren Schlüssel für den **erweiterten HCX-Schlüssel** ein.  
   
    > [!NOTE]
    > Für den VMware-HCX-Manager muss offener Internetzugriff möglich oder ein Proxy konfiguriert sein.

1. Geben Sie unter **Datacenter Location** (Standort des Rechenzentrums) den nächstgelegenen Standort für die lokale Installation des VMware-HCX-Managers an.

1. Ändern Sie unter **System Name** (Systemname) den Namen, oder übernehmen Sie die Standardeinstellung.
   
1. Wählen Sie **Yes, Continue** (Ja, fortsetzen) aus, um den Vorgang fortzusetzen.
    
1. Geben Sie in **Connect your vCenter** (Mit vCenter verbinden) den FQDN oder die IP-Adresse Ihres vCenter-Servers und die entsprechenden Anmeldeinformationen an, und wählen Sie dann **Continue** aus.
   
1. Geben Sie in **Configure SSO/PSC** (SSO/PSC konfigurieren) den FQDN oder die IP-Adresse Ihres Platform Services Controllers an, und wählen Sie dann **Continue** (Weiter) aus.
   
   >[!NOTE]
   >In der Regel ist dieser Eintrag mit Ihrem vCenter-FQDN oder Ihrer IP-Adresse identisch.

1. Vergewissern Sie sich, dass alle Eingaben richtig sind, und wählen Sie **Restart** (Neu starten) aus.
    
   > [!NOTE]
   > Nach dem Neustart tritt eine Verzögerung auf, bevor Sie zum Wechsel zum nächsten Schritt aufgefordert werden.

Nachdem der Dienst neu gestartet wurde, muss vCenter auf dem angezeigten Bildschirm mit grünem Punkt angezeigt werden. Sowohl vCenter als auch SSO müssen die geeigneten Konfigurationsparameter aufweisen, die mit denen im vorherigen Bildschirm identisch sein sollten.

:::image type="content" source="media/tutorial-vmware-hcx/activation-done.png" alt-text="Screenshot: Navigieren zu einer OVF-Vorlage." lightbox="media/tutorial-vmware-hcx/activation-done.png":::  

Eine umfassende Übersicht über diese Vorgehensweise vermittelt das Video [Azure VMware Solution: Aktivieren von HCX](https://www.youtube.com/embed/BkAV_TNYxdE).


## <a name="configure-the-vmware-hcx-connector"></a>Konfigurieren des VMware HCX-Connectors

Nun können Sie eine Standortkopplung (Site Pairing) hinzufügen, ein Netzwerk- und Computeprofil erstellen und Dienste wie Migration, Netzwerkerweiterung (Network Extension) oder Notfallwiederherstellung (Disaster Recovery) aktivieren. 

### <a name="add-a-site-pairing"></a>Hinzufügen einer Standortkopplung

Sie können den VMware HCX-Cloud-Manager in Azure VMware Solution mit dem VMware HCX-Connector in Ihrem Rechenzentrum verbinden (koppeln). 

1. Melden Sie sich bei Ihrem lokalen vCenter an, und wählen Sie unter **Home** die Option **HCX** aus.

   :::image type="content" source="media/tutorial-vmware-hcx/vcenter-vmware-hcx.png" alt-text="Screenshot: Navigieren zu einer OVF-Vorlage." lightbox="media/tutorial-vmware-hcx/vcenter-vmware-hcx.png":::

1. Wählen Sie unter **Infrastructure** die Option **Site Pairing** (Standortkopplung) aus, und wählen Sie dann die Option **Connect To Remote Site** (Verbindung mit Remotestandort herstellen) aus (in der Mitte des Bildschirms). 

   :::image type="content" source="media/tutorial-vmware-hcx/connect-remote-site.png" alt-text="Screenshot: Navigieren zu einer OVF-Vorlage." lightbox="media/tutorial-vmware-hcx/connect-remote-site.png":::

1. Geben Sie die zuvor notierte Remote-HCX-URL oder IP-Adresse, den cloudadmin@vsphere.local-Benutzernamen von Azure VMware Solution und das Kennwort ein. Wählen Sie dann **Verbinden** aus.

   > [!NOTE]
   > Zum erfolgreichen Einrichten eines Standortpaars muss Ihr HCX-Connector in der Lage sein, über Port 443 Routing an Ihre HCX-Cloud-Manager-IP-Adresse durchzuführen.
   >
   > Das Kennwort ist mit dem Kennwort identisch, mit dem Sie sich bei vCenter angemeldet haben. Sie haben dieses Kennwort auf dem ersten Bereitstellungsbildschirm angegeben.

   Ein Bildschirm wird angezeigt, auf dem der HCX-CLoud-Manager in Azure VMware Solution mit dem lokalen HCX-Connector verbunden (gekoppelt) ist.

   :::image type="content" source="media/tutorial-vmware-hcx/site-pairing-complete.png" alt-text="Screenshot: Navigieren zu einer OVF-Vorlage.":::

Eine umfassende Übersicht über diese Vorgehensweise vermittelt das Video [Azure VMware Solution: HCX-Standortkopplung](https://www.youtube.com/embed/sKizDCRHOko).



### <a name="create-network-profiles"></a>Erstellen von Netzwerkprofilen

VMware HCX stellt einen Teil der virtuellen Geräte bereit (automatisiert). Dafür sind mehrere IP-Segmente erforderlich. Wenn Sie Ihre Netzwerkprofile erstellen, definieren Sie die IP-Segmente, die Sie während der [Vorbereitungs- und Planungsphase für die Bereitstellung der VMware HCX-Netzwerksegmente](production-ready-deployment-steps.md#vmware-hcx-network-segments) ermittelt haben.

Sie erstellen vier Netzwerkprofile:

   - Verwaltung
   - vMotion
   - Replikation
   - Uplink

1. Wählen Sie unter **Infrastruktur** die Option **Interconnect** > **Multi-Site Service Mesh (Dienstnetz mit mehreren Standorten)**  > **Network Profiles (Netzwerkprofile)**  > **Create Network Profile (Netzwerkprofil erstellen)** aus.

   :::image type="content" source="media/tutorial-vmware-hcx/network-profile-start.png" alt-text="Screenshot: Navigieren zu einer OVF-Vorlage." lightbox="media/tutorial-vmware-hcx/network-profile-start.png":::

1. Wählen Sie für jedes Netzwerkprofil das Netzwerk und die Portgruppe aus, geben Sie einen Namen an, und erstellen Sie den IP-Pool für dieses Segment. Klicken Sie anschließend auf **Erstellen**. 

   :::image type="content" source="media/tutorial-vmware-hcx/example-configurations-network-profile.png" alt-text="Screenshot: Navigieren zu einer OVF-Vorlage.":::

Eine umfassende Übersicht über diese Vorgehensweise vermittelt das Video [Azure VMware Solution: HCX-Netzwerkprofil](https://www.youtube.com/embed/NhyEcLco4JY).


### <a name="create-a-compute-profile"></a>Erstellen eines Computeprofils

1. Wählen Sie **Compute Profiles** (Compute-Profile) > **Create Compute Profile** (Compute-Profil erstellen) aus.

   :::image type="content" source="media/tutorial-vmware-hcx/compute-profile-create.png" alt-text="Screenshot: Navigieren zu einer OVF-Vorlage." lightbox="media/tutorial-vmware-hcx/compute-profile-create.png":::

1. Geben Sie einen Namen für das Profil ein, und wählen Sie **Continue** (Weiter) aus.  

   :::image type="content" source="media/tutorial-vmware-hcx/name-compute-profile.png" alt-text="Screenshot: Navigieren zu einer OVF-Vorlage." lightbox="media/tutorial-vmware-hcx/name-compute-profile.png":::

1. Wählen Sie die Dienste aus, die aktiviert werden sollen, z. B. für Migration, Netzwerkerweiterung oder Notfallwiederherstellung, und wählen Sie dann **Continue** (Weiter) aus.
  
   > [!NOTE]
   > Im Allgemeinen ändert sich hier nichts.

1. Wählen Sie unter **Select Service Resources** (Dienstressourcen auswählen) mindestens eine Dienstressource (Cluster) aus, um die ausgewählten VMware HCX-Dienste zu aktivieren.  

1. Wenn Sie die Cluster in Ihrem lokalen Rechenzentrum sehen, wählen Sie **Continue** aus.

   :::image type="content" source="media/tutorial-vmware-hcx/select-service-resource.png" alt-text="Screenshot: Navigieren zu einer OVF-Vorlage." lightbox="media/tutorial-vmware-hcx/select-service-resource.png":::

1. Wählen Sie unter **Select Datastore** (Datenspeicher auswählen) die Datenspeicher-Speicherressource für die Bereitstellung der VMware HCX Interconnect-Appliances aus. Klicken Sie anschließend auf **Weiter**.

   Wenn mehrere Ressourcen ausgewählt sind, verwendet VMware HCX die erste ausgewählte Ressource, bis deren Kapazität erschöpft ist.   

   :::image type="content" source="media/tutorial-vmware-hcx/deployment-resources-and-reservations.png" alt-text="Screenshot: Navigieren zu einer OVF-Vorlage." lightbox="media/tutorial-vmware-hcx/deployment-resources-and-reservations.png":::  

1. Wählen Sie unter **Select Management Network Profile** (Verwaltungsnetzwerkprofil auswählen) das Verwaltungsnetzwerkprofil aus, das Sie in den vorherigen Schritten erstellt haben. Klicken Sie anschließend auf **Weiter**.  

   :::image type="content" source="media/tutorial-vmware-hcx/select-management-network-profile.png" alt-text="Screenshot: Navigieren zu einer OVF-Vorlage." lightbox="media/tutorial-vmware-hcx/select-management-network-profile.png":::

   > [!NOTE]
   > Das Verwaltungsnetzwerkprofil ermöglicht es den VMware HCX-Appliances, mit vCenter zu kommunizieren. Die ESXi-Hosts können über dieses Profil erreicht werden.

1. Wählen Sie unter **Select Uplink Network Profile** (Uplinknetzwerkprofil auswählen) das Uplinknetzwerkprofil aus, das Sie in den vorherigen Schritten erstellt haben. Klicken Sie anschließend auf **Weiter**.

   :::image type="content" source="media/tutorial-vmware-hcx/select-uplink-network-profile.png" alt-text="Screenshot: Navigieren zu einer OVF-Vorlage." lightbox="media/tutorial-vmware-hcx/select-uplink-network-profile.png":::

1. Wählen Sie unter **Select vMotion Network Profile** (vMotion-Netzwerkprofil auswählen) das vMotion-Netzwerkprofil aus, das Sie in den vorherigen Schritten erstellt haben. Klicken Sie anschließend auf **Weiter**.

   :::image type="content" source="media/tutorial-vmware-hcx/select-vmotion-network-profile.png" alt-text="Screenshot: Navigieren zu einer OVF-Vorlage." lightbox="media/tutorial-vmware-hcx/select-vmotion-network-profile.png":::

1. Wählen Sie unter **Select vSphere Replication Network Profile** (vSphere-Replikationsnetzwerkprofil auswählen) das vMotion-Replikationsnetzwerkprofil aus, das Sie in den vorherigen Schritten erstellt haben. Klicken Sie anschließend auf **Weiter**.

   In den meisten Fällen ist das Replikationsnetzwerkprofil mit dem Verwaltungsnetzwerkprofil identisch.  

   :::image type="content" source="media/tutorial-vmware-hcx/select-replication-network-profile.png" alt-text="Screenshot: Navigieren zu einer OVF-Vorlage." lightbox="media/tutorial-vmware-hcx/select-replication-network-profile.png":::

1. Wählen Sie unter **Select Distributed Switches for Network Extensions** (Verteilte Switches für Network Extensions auswählen) die verteilten virtuellen Switches aus, die die virtuellen Computer, die zu Azure VMware Solution migriert werden sollen, in einem erweiterten Schicht-2-Netzwerk enthalten. Klicken Sie anschließend auf **Weiter**.

   :::image type=" content" source="media/tutorial-vmware-hcx/select-layer-2-distributed-virtual-switch.png" alt-text="Screenshot: Navigieren zu einer OVF-Vorlage." lightbox="media/tutorial-vmware-hcx/select-layer-2-distributed-virtual-switch.png":::

1. Überprüfen Sie die Verbindungsregeln, und wählen Sie **Continue** (Weiter) aus.  

   :::image type="content" source="media/tutorial-vmware-hcx/review-connection-rules.png" alt-text="Screenshot: Navigieren zu einer OVF-Vorlage." lightbox="media/tutorial-vmware-hcx/review-connection-rules.png":::

1. Wählen Sie **Finish** (Fertig stellen) aus, um das Compute-Profil zu erstellen.


   :::image type="content" source="media/tutorial-vmware-hcx/compute-profile-done.png" alt-text="Screenshot: Navigieren zu einer OVF-Vorlage." lightbox="media/tutorial-vmware-hcx/compute-profile-done.png":::

Eine umfassende Übersicht über diese Vorgehensweise vermittelt das Video [Azure VMware Solution: Computeprofil](https://www.youtube.com/embed/qASXi5xrFzM).

### <a name="create-a-service-mesh"></a>Erstellen eines Service Mesh

Im nächsten Schritt konfigurieren Sie ein Service Mesh zwischen dem lokalen Standort und Azure VMware Solution SDDC.

   > [!NOTE]
   > So richten Sie erfolgreich ein Service Mesh mit Azure VMware Solution ein:
   >
   > Die Ports UDP 500/4500 sind zwischen den durch Ihren lokalen HCC-Connector definierten Uplink-Netzwerkprofiladressen und den Azure VMware Solution HCX-Cloudnetzwerk-Profiladressen geöffnet.
   >
   > Überprüfen Sie unbedingt die [Ports, die für HCX erforderlich sind](https://ports.vmware.com/home/VMware-HCX).

1. Wählen Sie unter **Infrastructure** (Infrastruktur) die Option **Interconnect** (Verbinden) > **Service Mesh** (Dienstnetz) > **Create Service Mesh** (Dienstnetz erstellen) aus.    

   :::image type="content" source="media/tutorial-vmware-hcx/create-service-mesh.png" alt-text="Screenshot: Navigieren zu einer OVF-Vorlage." lightbox="media/tutorial-vmware-hcx/create-service-mesh.png":::

1. Überprüfen Sie die vorab aufgefüllten Websites, und wählen Sie dann **Continue** aus. 

   >[!NOTE]
   >Wenn dies Ihre erste Dienstnetzkonfiguration ist, müssen Sie diesen Bildschirm nicht ändern.  

1. Wählen Sie das Quellcomputeprofil und das Remotecomputeprofil aus den Dropdownlisten aus, und wählen Sie dann **Continue** (Weiter) aus.  

   Mit der Auswahl werden die Ressourcen definiert, in denen VMs VMware HCX-Dienste nutzen können.  

   :::image type="content" source="media/tutorial-vmware-hcx/select-compute-profile-source.png" alt-text="Screenshot: Navigieren zu einer OVF-Vorlage." lightbox="media/tutorial-vmware-hcx/select-compute-profile-source.png":::

   :::image type="content" source="media/tutorial-vmware-hcx/select-compute-profile-remote.png" alt-text="Screenshot: Navigieren zu einer OVF-Vorlage." lightbox="media/tutorial-vmware-hcx/select-compute-profile-remote.png":::

1. Überprüfen Sie die Dienste, die aktiviert werden, und wählen Sie **Continue** (Weiter) aus.  

1. Wählen Sie unter **Advanced Configuration - Override Uplink Network profiles** (Erweiterte Konfiguration – Uplinknetzwerkprofile überschreiben) die Option **Continue** (Weiter) aus.  

   Uplinknetzwerkprofile stellen Verbindungen mit dem Netzwerk her, über das die Interconnect-Appliances des Remotestandorts erreicht werden können.  
  
1. Überprüfen Sie die Einstellungen in **Advanced Configuration – Network Extension Appliance Scale Out** (Erweiterte Konfiguration – Aufskalierung für Network Extension-Appliance konfigurieren), und wählen Sie **Continue** aus. 

1. Überprüfen Sie die Einstellungen in **Advanced Configuration – Traffic Engineering** (Erweiterte Konfiguration – Verwaltung des Datenverkehrs), nehmen Sie alle erforderlichen Änderungen vor, und wählen Sie dann **Continue** (Weiter) aus.

1. Überprüfen Sie die Topologievorschau, und wählen Sie **Continue** (Weiter) aus.

1. Geben Sie einen benutzerfreundlichen Namen für dieses Dienstnetz ein, und wählen Sie **Finish** (Fertig stellen) aus, um den Vorgang abzuschließen.  

1. Wählen Sie **View Tasks** (Aufgaben anzeigen) aus, um die Bereitstellung zu überwachen. 

   :::image type="content" source="media/tutorial-vmware-hcx/monitor-service-mesh.png" alt-text="Screenshot: Navigieren zu einer OVF-Vorlage.":::

   Wenn die Service Mesh-Bereitstellung erfolgreich abgeschlossen wurde, werden die Dienste grün angezeigt.

   :::image type="content" source="media/tutorial-vmware-hcx/service-mesh-green.png" alt-text="Screenshot: Navigieren zu einer OVF-Vorlage." lightbox="media/tutorial-vmware-hcx/service-mesh-green.png":::

1. Überprüfen Sie die Integrität des Dienstnetzes, indem Sie den Appliancestatus überprüfen. 
1. Wählen Sie **Interconnect** > **Appliances** aus.

   :::image type="content" source="media/tutorial-vmware-hcx/interconnect-appliance-state.png" alt-text="Screenshot: Navigieren zu einer OVF-Vorlage." lightbox="media/tutorial-vmware-hcx/interconnect-appliance-state.png":::

Eine umfassende Übersicht über diese Vorgehensweise vermittelt das Video [Azure VMware Solution: Service Mesh](https://www.youtube.com/embed/FyZ0d3P_T24).



### <a name="optional-create-a-network-extension"></a>(Optional) Erstellen einer Netzwerkerweiterung

Wenn Sie Netzwerke aus Ihrer lokalen Umgebung um Azure VMware Solution erweitern möchten, führen Sie die folgenden Schritte aus:

1. Wählen Sie unter **Services** (Dienste) die Option **Network Extension** (Netzwerkerweiterung) aus, und wählen Sie dann **Create a Network Extension** (Netzwerkerweiterung erstellen) aus.

   :::image type="content" source="media/tutorial-vmware-hcx/create-network-extension.png" alt-text="Screenshot: Navigieren zu einer OVF-Vorlage." lightbox="media/tutorial-vmware-hcx/create-network-extension.png":::

1. Wählen Sie jedes der Netzwerke aus, die Sie um Azure VMware Solution erweitern möchten, und wählen Sie dann **Next** (Weiter) aus.

   :::image type="content" source="media/tutorial-vmware-hcx/select-extend-networks.png" alt-text="Screenshot: Navigieren zu einer OVF-Vorlage.":::

1. Geben Sie die lokale Gateway-IP-Adresse für jedes der Netzwerke ein, die Sie erweitern möchten, ein, und wählen Sie dann **Submit** (Senden) aus. 

   :::image type="content" source="media/tutorial-vmware-hcx/extend-networks-gateway.png" alt-text="Screenshot: Navigieren zu einer OVF-Vorlage.":::

   Es dauert einige Minuten, bis die Netzwerkerweiterung abgeschlossen ist. Sobald dies der Fall ist, erfolgt die Statusänderung in **Extension complete** (Erweiterung abgeschlossen).

   :::image type="content" source="media/tutorial-vmware-hcx/extension-complete.png" alt-text="Screenshot: Navigieren zu einer OVF-Vorlage." lightbox="media/tutorial-vmware-hcx/extension-complete.png":::

Eine umfassende Übersicht über diesen Schritt vermittelt das Video [Azure VMware Solution: Netzwerkerweiterung](https://www.youtube.com/embed/cNlp0f_tTr0).


## <a name="next-steps"></a>Nächste Schritte

Wenn Sie diesen Punkt erreicht haben und der „Tunnel Status“ (Tunnelstatus) der Appliance-Verbindung als **UP** und grün angezeigt wird, können Sie Azure VMware Solution-VMs mit VMware HCX migrieren und schützen. Azure VMware Solution unterstützt Workloadmigrationen (mit oder ohne Netzwerkerweiterung). Sie können weiterhin Workloads in ihrer vSphere-Umgebung migrieren sowie Netzwerke lokal erstellen und VMs in diesen Netzwerken bereitstellen.  

Weitere Informationen zur Verwendung von HCx finden Sie in der technischen VMware-Dokumentation:

* [VMware HCX-Dokumentation](https://docs.vmware.com/en/VMware-HCX/index.html)
* [Migrieren von Virtual Machines mit VMware HCX](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-D0CD0CC6-3802-42C9-9718-6DA5FEC246C6.html?hWord=N4IghgNiBcIBIGEAaACAtgSwOYCcwBcMB7AOxAF8g).
* [Für HCX erforderliche Ports](https://ports.vmware.com/home/VMware-HCX)
