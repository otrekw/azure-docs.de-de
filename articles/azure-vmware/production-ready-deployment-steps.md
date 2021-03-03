---
title: Planen der Azure VMware Solution-Bereitstellung
description: In diesem Artikel wird der Workflow für die Bereitstellung einer Azure VMware Solution-Instanz beschrieben.  Das Endergebnis ist eine Umgebung, die für die Erstellung und Migration von virtuellen Computern (VMs) vorbereitet ist.
ms.topic: tutorial
ms.date: 02/22/2021
ms.openlocfilehash: f9d49d7ff8109364c9fc1eee4388b30ccc1a61b6
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101733658"
---
# <a name="planning-the-azure-vmware-solution-deployment"></a>Planen der Azure VMware Solution-Bereitstellung

In diesem Artikel wird der Planungsprozess zum Identifizieren und Sammeln von Daten beschrieben, die während der Bereitstellung verwendet werden. Stellen Sie beim Planen Ihrer Bereitstellung sicher, dass Sie die gesammelten Informationen dokumentieren, damit Sie während des Bereitstellungsvorgangs leicht darauf zugreifen können.

Die Prozesse dieser Schnellstartanleitung führen zu einer für die Produktion vorbereiteten Umgebung zum Erstellen von virtuellen Computern (VMs) und Durchführen der Migration. 

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

## <a name="number-of-hosts"></a>Anzahl von Hosts

Definieren Sie die Anzahl von Hosts, die Sie in der privaten Azure VMware Solution-Cloud bereitstellen möchten.  Die Mindestanzahl von Hosts beträgt 3, und der Höchstwert beträgt  16 pro Cluster.  Weitere Informationen finden Sie unter [Azure VMware Solution (Vorschau): Konzepte – Private Clouds und Cluster](concepts-private-clouds-clusters.md#clusters).

Sie können den Cluster später jederzeit erweitern, wenn Sie die anfängliche Bereitstellungsanzahl erhöhen möchten.

## <a name="ip-address-segment"></a>IP-Adressensegment

Der erste Schritt bei der Planung der Bereitstellung ist das Planen der IP-Segmentierung.  Azure VMware Solution erfasst ein von Ihnen bereitgestelltes Netzwerk vom Typ „/22“. Anschließend wird es in kleinere IP-Segmente unterteilt, die dann für vCenter, VMware HCX, NSX-T und vMotion verwendet werden.

Azure VMware Solution stellt über eine interne ExpressRoute-Leitung eine Verbindung mit Ihrer Microsoft Azure Virtual Network-Instanz her. In den meisten Fällen wird eine Verbindung mit Ihrem Rechenzentrum per ExpressRoute Global Reach hergestellt. 

Azure VMware Solution, Ihre vorhandene Azure-Umgebung und Ihre lokale Umgebung tauschen (normalerweise) untereinander Routen aus. Daher sollte sich der CIDR-Netzwerkadressblock vom Typ „/22“, den Sie in diesem Schritt definieren, nicht mit Komponenten überlappen, die bei Ihnen lokal oder in Azure bereits vorhanden sind.

**Beispiel:** 10.0.0.0/22

Weitere Informationen finden Sie in der [Checkliste für die Netzwerkplanung](tutorial-network-checklist.md#routing-and-subnet-considerations).

:::image type="content" source="media/pre-deployment/management-vmotion-vsan-network-ip-diagram.png" alt-text="Identifizieren: IP-Adressensegment" border="false":::  

## <a name="ip-address-segment-for-virtual-machine-workloads"></a>IP-Adressensegment für VM-Workloads

Identifizieren Sie ein IP-Segment, um Ihr erstes Netzwerk (NSX-Segment) in Ihrer privaten Cloud zu erstellen.  Anders ausgedrückt: Sie möchten ein Netzwerksegment in Azure VMware Solution erstellen, damit Sie virtuelle Computer unter Azure VMware Solution bereitstellen können.   

Auch wenn Sie eine Erweiterung von L2-Netzwerken planen, sollten Sie ein Netzwerksegment erstellen, das die Umgebung überprüft.

Beachten Sie, dass alle erstellten IP-Segmente in Ihrer Azure- und lokalen Umgebung eindeutig sein müssen.  

**Beispiel:** 10.0.4.0/24

:::image type="content" source="media/pre-deployment/nsx-segment-diagram.png" alt-text="Identifizieren: IP-Adressensegment für VM-Workloads" border="false":::     

## <a name="optional-extend-networks"></a>(Optional) Erweitern von Netzwerken

Sie können Netzwerksegmente aus der lokalen Umgebung auf Azure VMware Solution erweitern. Wenn Sie dies tun, sollten Sie diese Netzwerke jetzt identifizieren.  

Bedenken Sie Folgendes:

- Wenn Sie die Erweiterung von Netzwerken aus der lokalen Umgebung planen, muss für diese Netzwerke eine Verbindung mit einem [vSphere Distributed Switch (vDS)](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.networking.doc/GUID-B15C6A13-797E-4BCB-B9D9-5CBC5A60C3A6.html) in Ihrer lokalen VMware-Umgebung bestehen.  
- Falls die zu erweiternden Netzwerke auf einem [vSphere Standard Switch](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.networking.doc/GUID-350344DE-483A-42ED-B0E2-C811EE927D59.html) angeordnet sind, können sie nicht erweitert werden.

## <a name="attach-virtual-network-to-azure-vmware-solution"></a>Anfügen eines virtuellen Netzwerks an Azure VMware Solution

In diesem Schritt identifizieren Sie ein ExpressRoute-Gateway für virtuelle Netzwerke und eine unterstützende Azure Virtual Network-Instanz, die zum Herstellen einer Verbindung mit der ExpressRoute-Leitung von Azure VMware Solution verwendet wird.  Die ExpressRoute-Leitung ermöglicht Konnektivität zwischen der privaten Azure VMware Solution-Cloud und anderen Azure-Diensten, Azure-Ressourcen und lokalen Umgebungen.

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
