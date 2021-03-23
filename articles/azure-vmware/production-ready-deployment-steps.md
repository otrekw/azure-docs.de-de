---
title: Planen der Azure VMware Solution-Bereitstellung
description: In diesem Artikel wird der Workflow für die Bereitstellung einer Azure VMware Solution-Instanz beschrieben.  Das Endergebnis ist eine Umgebung, die für die Erstellung und Migration von virtuellen Computern (VMs) vorbereitet ist.
ms.topic: tutorial
ms.date: 03/13/2021
ms.openlocfilehash: f1895f14361b7121ae0d78950cdf8eca3cf7eb52
ms.sourcegitcommit: afb9e9d0b0c7e37166b9d1de6b71cd0e2fb9abf5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/14/2021
ms.locfileid: "103462421"
---
# <a name="planning-the-azure-vmware-solution-deployment"></a>Planen der Azure VMware Solution-Bereitstellung

In diesem Artikel wird der Planungsprozess zum Identifizieren und Sammeln der Daten beschrieben, die im Rahmen der Bereitstellung verwendet werden. Stellen Sie beim Planen Ihrer Bereitstellung sicher, dass Sie die gesammelten Informationen dokumentieren, damit Sie während des Bereitstellungsvorgangs leicht darauf zugreifen können.

Durch die in dieser Schnellstartanleitung beschriebenen Schritte erhalten Sie eine produktionsbereite Umgebung zum Erstellen von virtuellen Computern (virtual machines, VMs) sowie für die Migration. 

>[!IMPORTANT]
>Halten Sie sich vor dem Erstellen Ihrer Azure VMware Solution-Ressource an die Informationen im Artikel [Aktivieren einer Azure VMware Solution-Ressource](enable-azure-vmware-solution.md), um ein Supportticket für die Zuordnung Ihrer Hosts zu übermitteln. Nachdem das Supportteam Ihre Anforderung erhalten hat, dauert es bis zu fünf Werktage, bis die Anforderung bestätigt wurde und Ihre Hosts zugewiesen wurden. Wenn Sie über eine vorhandene private Azure VMware Solution-Cloud verfügen und weitere Hosts zugeordnet werden sollen, müssen Sie den gleichen Prozess durchlaufen. 


## <a name="subscription"></a>Subscription

Identifizieren Sie das Abonnement, das Sie zum Bereitstellen von Azure VMware Solution verwenden möchten.  Sie können entweder ein neues Abonnement erstellen oder ein vorhandenes wiederverwenden.

>[!NOTE]
>Das Abonnement muss einem Microsoft Enterprise Agreement oder einem Azure-Plan für Cloud Solution Provider zugeordnet sein. Weitere Informationen finden Sie unter [Aktivieren einer Azure VMware Solution-Ressource](enable-azure-vmware-solution.md).

## <a name="resource-group"></a>Resource group

Identifizieren Sie die Ressourcengruppe, die Sie für Ihre Azure VMware Solution-Instanz verwenden möchten.  Im Allgemeinen wird eine Ressourcengruppe speziell für Azure VMware Solution erstellt, aber Sie können auch eine vorhandene Ressourcengruppe verwenden.

## <a name="region"></a>Region

Identifizieren Sie die Region, in der Azure VMware Solution bereitgestellt werden soll.  Weitere Informationen finden Sie unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/en-us/global-infrastructure/services/?products=azure-vmware).

## <a name="resource-name"></a>Ressourcenname

Legen Sie den Ressourcennamen fest, den Sie während der Bereitstellung verwenden möchten.  Der Ressourcenname ist ein benutzerfreundlicher und beschreibender Name, in dem Sie Ihre private Azure VMware Solution-Cloud angeben.

>[!IMPORTANT]
>Der Name darf maximal 40 Zeichen lang sein. Wenn der Name diesen Grenzwert überschreitet, können Sie keine öffentlichen IP-Adressen für die Verwendung mit der privaten Cloud erstellen. 

## <a name="size-hosts"></a>Größenhosts

Identifizieren Sie die Größenhosts, die Sie beim Bereitstellen von Azure VMware Solution verwenden möchten.  Eine vollständige Liste finden Sie in der Dokumentation zu [privaten Clouds und Clustern unter Azure VMware Solution](concepts-private-clouds-clusters.md#hosts).

## <a name="number-of-clusters-and-hosts"></a>Anzahl von Clustern und Hosts

In Azure VMware Solution stellen Sie eine private Cloud bereit und erstellen mehrere Cluster. Für Ihre Bereitstellung müssen Sie die Anzahl von Clustern sowie die Hosts definieren, die Sie in den einzelnen Clustern bereitstellen möchten. Sie können zwischen drei und 16 Cluster (jeweils einschließlich) verwenden. Pro privater Cloud sind maximal vier Cluster möglich. Die maximale Anzahl von Knoten pro privater Cloud beträgt 64.

Weitere Informationen finden Sie unter [Azure VMware Solution (Vorschau): Konzepte – Private Clouds und Cluster](concepts-private-clouds-clusters.md#clusters).

>[!TIP]
>Sie können den Cluster später jederzeit erweitern, wenn Sie die anfängliche Bereitstellungsanzahl erhöhen möchten.

## <a name="vcenter-admin-password"></a>vCenter admin password (vCenter-Administratorkennwort)
Definieren Sie das vCenter-Administratorkennwort. Während der Bereitstellung erstellen Sie ein vCenter-Administratorkennwort. Das Kennwort wird dem Administratorkonto cloudadmin@vsphere.local im Rahmen des vCenter-Buildvorgangs zugewiesen. Diese Anmeldeinformationen werden für die Anmeldung bei vCenter verwendet.

## <a name="nsx-t-admin-password"></a>NSX-T-Administratorkennwort
Definieren Sie das NSX-T-Administratorkennwort. Während der Bereitstellung erstellen Sie ein NSX-T-Administratorkennwort. Das Kennwort wird dem Administratorbenutzer im NSX-Konto während des NSX-Buildvorgangs zugewiesen. Diese Anmeldeinformationen werden für die Anmeldung bei NSX-T Manager verwendet.

## <a name="ip-address-segment-for-private-cloud-management"></a>IP-Adressensegment für die Verwaltung der privaten Cloud

Der erste Schritt bei der Planung der Bereitstellung ist das Planen der IP-Segmentierung. Für Azure VMware Solution wird ein CIDR-Netzwerk vom Typ „/22“ benötigt. Durch diesen Adressraum wird es in kleinere Netzwerksegmente (Subnetze) unterteilt und für vCenter-, VMware HCX-, NSX-T- und vMotion-Funktionen verwendet.

Dieser CIDR-Netzwerkadressblock vom Typ „/22“ darf sich mit keinem Netzwerksegment überschneiden, das ggf. bereits lokal oder in Azure vorhanden ist.

**Beispiel:** 10.0.0.0/22

Azure VMware Solution stellt über eine interne ExpressRoute Global Reach-Leitung (D-MSEE in der Darstellung weiter unten) eine Verbindung mit Ihrer Microsoft Azure Virtual Network-Instanz her. Diese Funktion ist Teil des Azure VMware Solution-Diensts und wird nicht in Rechnung gestellt.

Weitere Informationen finden Sie in der [Checkliste für die Netzwerkplanung](tutorial-network-checklist.md#routing-and-subnet-considerations).

:::image type="content" source="media/pre-deployment/management-vmotion-vsan-network-ip-diagram.png" alt-text="Identifizieren: IP-Adressensegment" border="false":::  

## <a name="ip-address-segment-for-virtual-machine-workloads"></a>IP-Adressensegment für VM-Workloads

Identifizieren Sie ein IP-Segment, um Ihr erstes Netzwerk für Workloads (NSX-Segment) in Ihrer privaten Cloud zu erstellen. Anders ausgedrückt: Sie müssen ein Netzwerksegment in Azure VMware Solution erstellen, damit Sie virtuelle Computer in Azure VMware Solution bereitstellen können.

Auch wenn Sie planen, lokale Netzwerke in Azure VMware Solution (L2) zu erweitern, müssen Sie ein Netzwerksegment zur Validierung der Umgebung erstellen.

Beachten Sie, dass alle erstellten IP-Segmente in Ihrer Azure- und lokalen Umgebung eindeutig sein müssen.
  
**Beispiel:** 10.0.4.0/24

:::image type="content" source="media/pre-deployment/nsx-segment-diagram.png" alt-text="Identifizieren: IP-Adressensegment für VM-Workloads" border="false":::     

## <a name="optional-extend-networks"></a>(Optional) Erweitern von Netzwerken

Sie können Netzwerksegmente aus der lokalen Umgebung auf Azure VMware Solution erweitern. Wenn Sie dies tun, sollten Sie diese Netzwerke jetzt identifizieren.  

Bedenken Sie Folgendes:

- Wenn Sie die Erweiterung von Netzwerken aus der lokalen Umgebung planen, muss für diese Netzwerke eine Verbindung mit einem [vSphere Distributed Switch (vDS)](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.networking.doc/GUID-B15C6A13-797E-4BCB-B9D9-5CBC5A60C3A6.html) in Ihrer lokalen VMware-Umgebung bestehen.  
- Falls die zu erweiternden Netzwerke auf einem [vSphere Standard Switch](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.networking.doc/GUID-350344DE-483A-42ED-B0E2-C811EE927D59.html) angeordnet sind, können sie nicht erweitert werden.

## <a name="attach-azure-virtual-network-to-azure-vmware-solution"></a>Anfügen von Azure VMware Solution an eine Azure Virtual Network-Instanz

In diesem Schritt identifizieren Sie ein ExpressRoute-Gateway für virtuelle Netzwerke sowie die unterstützende Azure Virtual Network-Instanz, die zum Herstellen einer Verbindung mit der ExpressRoute-Leitung von Azure VMware Solution verwendet wird.  Die ExpressRoute-Leitung ermöglicht Konnektivität zwischen der privaten Azure VMware Solution-Cloud und anderen Azure-Diensten, Azure-Ressourcen und lokalen Umgebungen.

Sie können ein *vorhandenes* ODER ein *neues* ExpressRoute-Gateway für virtuelle Netzwerke verwenden.

:::image type="content" source="media/pre-deployment/azure-vmware-solution-expressroute-diagram.png" alt-text="Identifizieren: Azure Virtual Network zum Anfügen von Azure VMware Solution" border="false":::

### <a name="use-an-existing-expressroute-virtual-network-gateway"></a>Verwenden eines vorhandenen ExpressRoute-Gateways für virtuelle Netzwerke

Wenn Sie ein *vorhandenes* ExpressRoute-Gateway für virtuelle Netzwerke verwenden, wird die ExpressRoute-Leitung von Azure VMware Solution nach dem Bereitstellen der privaten Cloud eingerichtet. Lassen Sie in diesem Fall das Feld **Virtual Network** leer.  

Notieren Sie sich, welches ExpressRoute-Gateway für virtuelle Netzwerke verwendet wird, und fahren Sie mit dem nächsten Schritt fort.

### <a name="create-a-new-expressroute-virtual-network-gateway"></a>Erstellen eines neuen ExpressRoute-Gateways für virtuelle Netzwerke

Wenn Sie ein *neues* ExpressRoute-Gateway für virtuelle Netzwerke erstellen, können Sie eine vorhandene Azure Virtual Network-Instanz verwenden oder eine neue erstellen.  

- Vorhandene Azure Virtual Network-Instanz:
   1. Vergewissern Sie sich, dass im virtuellen Netzwerk nicht bereits ExpressRoute-Gateways für virtuelle Netzwerke vorhanden sind. 
   1. Wählen Sie in der Liste **Virtual Network** die vorhandene Azure Virtual Network-Instanz aus.

- Für eine neue Azure Virtual Network-Instanz können Sie diese im Voraus oder während der Bereitstellung erstellen. Wählen Sie unter der Liste **Virtual Network** den Link **Neu erstellen** aus.

Die folgende Abbildung zeigt den Bereitstellungsbildschirm **Erstellen einer privaten Cloud**, auf dem das Feld **Virtual Network** hervorgehoben ist.

:::image type="content" source="media/pre-deployment/azure-vmware-solution-deployment-screen-vnet-circle.png" alt-text="Screenshot: Azure VMware Solution-Bereitstellungsbildschirm mit hervorgehobenem Feld „Virtual Network“":::

>[!NOTE]
>Ein virtuelles Netzwerk, das verwendet oder erstellt wird, kann von Ihrer lokalen Umgebung und von Azure VMware Solution erkannt werden. Stellen Sie daher sicher, dass sich das in diesem virtuellen Netzwerk verwendete IP-Segment und die Subnetze nicht überlappen.

## <a name="vmware-hcx-network-segments"></a>VMware HCX-Netzwerksegmente

VMware HCX ist eine Technologie, die Teil des Azure VMware Solution-Pakets ist. Die primären Anwendungsfälle für VMware HCX sind Workloadmigrationen und Notfallwiederherstellung. Wenn Sie beides durchführen möchten, ist es ratsam, das Netzwerk jetzt zu planen.   Andernfalls können Sie diesen Schritt überspringen und mit dem nächsten fortfahren.

[!INCLUDE [hcx-network-segments](includes/hcx-network-segments.md)]

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie die erforderlichen Informationen nun gesammelt und dokumentiert haben, können Sie mit dem nächsten Abschnitt fortfahren, um Ihre private Azure VMware Solution-Cloud zu erstellen.

> [!div class="nextstepaction"]
> [Bereitstellen von Azure VMware Solution](deploy-azure-vmware-solution.md)
