---
title: Planen der Azure VMware Solution-Bereitstellung
description: In diesem Artikel wird der Workflow für die Bereitstellung einer Azure VMware Solution-Instanz beschrieben.  Das Endergebnis ist eine Umgebung, die für die Erstellung und Migration von virtuellen Computern (VMs) vorbereitet ist.
ms.topic: tutorial
ms.date: 03/17/2021
ms.openlocfilehash: 60e0a4083c0253d322b2e10472d0df7496722c10
ms.sourcegitcommit: 5f482220a6d994c33c7920f4e4d67d2a450f7f08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2021
ms.locfileid: "107107243"
---
# <a name="planning-the-azure-vmware-solution-deployment"></a>Planen der Azure VMware Solution-Bereitstellung

In diesem Artikel wird der Planungsprozess zum Identifizieren und Sammeln der Daten beschrieben, die im Rahmen der Bereitstellung verwendet werden. Stellen Sie beim Planen Ihrer Bereitstellung sicher, dass Sie die gesammelten Informationen dokumentieren, damit Sie während des Bereitstellungsvorgangs leicht darauf zugreifen können.

Durch die in dieser Schnellstartanleitung beschriebenen Schritte erhalten Sie eine produktionsbereite Umgebung zum Erstellen von virtuellen Computern (virtual machines, VMs) sowie für die Migration. 

Laden Sie zum Nachverfolgen der von Ihnen gesammelten Daten die [HCX-Prüfliste für die Planung](https://www.virtualworkloads.com/2021/04/hcx-planning-checklist/) herunter.

> [!IMPORTANT]
> Es ist wichtig, frühzeitig ein Hostkontingent anzufordern, wenn Sie sich auf die Erstellung Ihrer Azure VMware Solution-Ressource vorbereiten. Sie können jetzt ein Hostkontingent anfordern. Wenn der Planungsprozess abgeschlossen ist, können Sie direkt die private Azure VMware Solution-Cloud bereitstellen: Nachdem das Supportteam Ihre Anforderung eines Hostkontingents erhalten hat, dauert es bis zu fünf Werktage, bis die Anforderung bestätigt wurde und Ihre Hosts zugewiesen wurden. Wenn Sie über eine vorhandene private Azure VMware Solution-Cloud verfügen und weitere Hosts zugeordnet werden sollen, müssen Sie auf die gleiche Weise vorgehen. Weitere Informationen finden Sie je nach Abonnementtyp unter den folgenden Links:
> - [EA-Kunden](enable-azure-vmware-solution.md?tabs=azure-portal#request-host-quota-for-ea-customers)
> - [CSP-Kunden](enable-azure-vmware-solution.md?tabs=azure-portal#request-host-quota-for-csp-customers)

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

Die erste Azure VMware Solution-Bereitstellung umfasst eine private Cloud mit einem einzelnen Cluster. Für Ihre Bereitstellung müssen Sie die Anzahl von Hosts definieren, die Sie im ersten Cluster bereitstellen möchten.

>[!NOTE]
>Sie können zwischen drei und 16 Cluster (jeweils einschließlich) verwenden. Pro privater Cloud sind maximal vier Cluster möglich. 

Weitere Informationen finden Sie unter [Azure VMware Solution (Vorschau): Konzepte – Private Clouds und Cluster](concepts-private-clouds-clusters.md#clusters).

>[!TIP]
>Sie können den Cluster später jederzeit erweitern und zusätzliche Cluster hinzufügen, wenn Sie die anfängliche Bereitstellungsanzahl erhöhen möchten.

## <a name="ip-address-segment-for-private-cloud-management"></a>IP-Adressensegment für die Verwaltung der privaten Cloud

Der erste Schritt bei der Planung der Bereitstellung ist das Planen der IP-Segmentierung. Für Azure VMware Solution wird ein CIDR-Netzwerk vom Typ „/22“ benötigt. Dieser Adressraum wird in kleinere Netzwerksegmente (Subnetze) unterteilt und für Azure VMware Solution-Verwaltungssegmente (u. a. vCenter-, VMware HCX-, NSX-T- und vMotion-Funktionen) verwendet. Die Visualisierung weiter unten zeigt, wo dieses Segment verwendet wird.

Dieser CIDR-Netzwerkadressblock vom Typ „/22“ darf sich mit keinem Netzwerksegment überschneiden, das bereits lokal oder in Azure vorhanden ist.

**Beispiel:** 10.0.0.0/22

Eine detaillierte Aufschlüsselung des CIDR-Netzwerks vom Typ „/22“ nach privater Cloud finden Sie unter [Checkliste für die Netzwerkplanung für Azure VMware Solution](tutorial-network-checklist.md#routing-and-subnet-considerations).

:::image type="content" source="media/pre-deployment/management-vmotion-vsan-network-ip-diagram.png" alt-text="Identifizieren: IP-Adressensegment" border="false":::  

## <a name="ip-address-segment-for-virtual-machine-workloads"></a>IP-Adressensegment für VM-Workloads

Wie bei jeder beliebigen VMware-Umgebung müssen die virtuellen Computer eine Verbindung mit einem Netzwerksegment herstellen. In Azure VMware Solution gibt es zwei Arten von Segmenten: erweiterte L2-Segmente (weiter unten behandelt) und NSX-T-Netzwerksegmente. Wenn die Produktionsbereitstellung von Azure VMware Solution erweitert wird, gibt es häufig eine Kombination aus erweiterten L2-Segmenten aus der lokalen Umgebung und lokalen NSX-T-Netzwerksegmenten. Geben Sie zum Planen der ersten Bereitstellung in Azure VMware Solution ein einzelnes Netzwerksegment (IP-Netzwerk) an. Dieses Netzwerk darf sich nicht mit lokalen Netzwerksegmenten oder innerhalb des restlichen Azure-Netzwerks überlappen und darf nicht innerhalb des zuvor definierten Netzwerksegments vom Typ „/22“ liegen.

Dieses Netzwerksegment wird hauptsächlich zu Testzwecken während der ersten Bereitstellung verwendet.

>[!NOTE]
>Diese Netzwerke werden während der Bereitstellung nicht benötigt. Sie werden in einem Schritt nach der Bereitstellung erstellt.
  
**Beispiel:** 10.0.4.0/24

:::image type="content" source="media/pre-deployment/nsx-segment-diagram.png" alt-text="Identifizieren: IP-Adressensegment für VM-Workloads" border="false":::     

## <a name="attach-azure-virtual-network-to-azure-vmware-solution"></a>Anfügen von Azure VMware Solution an eine Azure Virtual Network-Instanz

Zum Bereitstellen von Konnektivität mit Azure VMware Solution wird eine ExpressRoute-Leitung zwischen der privaten Azure VMware Solution-Cloud und einem ExpressRoute-Gateway für virtuelle Netzwerke eingerichtet.

Sie können ein *vorhandenes* ODER ein *neues* ExpressRoute-Gateway für virtuelle Netzwerke verwenden.

:::image type="content" source="media/pre-deployment/azure-vmware-solution-expressroute-diagram.png" alt-text="Identifizieren: Azure Virtual Network zum Anfügen von Azure VMware Solution" border="false":::

### <a name="use-an-existing-expressroute-virtual-network-gateway"></a>Verwenden eines vorhandenen ExpressRoute-Gateways für virtuelle Netzwerke

Wenn Sie ein *vorhandenes* ExpressRoute-Gateway für virtuelle Netzwerke verwenden möchten, wird die ExpressRoute-Leitung von Azure VMware Solution nach dem Bereitstellen der privaten Cloud eingerichtet. Lassen Sie in diesem Fall das Feld **Virtual Network** leer.

Allgemeine Empfehlung: Es ist in Ordnung, ein bereits vorhandenes ExpressRoute-Gateway für virtuelle Netzwerke zu verwenden. Notieren Sie sich zu Planungszwecken, welches ExpressRoute-Gateway für virtuelle Netzwerke verwendet wird, und fahren Sie mit dem [nächsten Schritt](#vmware-hcx-network-segments) fort.

### <a name="create-a-new-expressroute-virtual-network-gateway"></a>Erstellen eines neuen ExpressRoute-Gateways für virtuelle Netzwerke

Wenn Sie ein *neues* ExpressRoute-Gateway für virtuelle Netzwerke erstellen, können Sie eine vorhandene Azure Virtual Network-Instanz verwenden oder eine neue erstellen.  

- Vorhandene Azure Virtual Network-Instanz:
   1. Ermitteln Sie eine Azure Virtual Network-Instanz, die keine bereits vorhandenen ExpressRoute-Gateways für virtuelle Netzwerke enthält.
   2. Erstellen Sie vor der Bereitstellung ein [GatewaySubnet](../expressroute/expressroute-howto-add-gateway-portal-resource-manager.md#create-the-gateway-subnet)-Element in der Azure Virtual Network-Instanz

- Für eine neue Azure Virtual Network-Instanz und ein neues VNET-Gateway erstellen Sie es während der Bereitstellung, indem Sie in der Liste **Virtual Network** den Link **Neu erstellen** auswählen.  Es ist wichtig, den Adressraum und die Subnetze vor der Bereitstellung zu definieren, damit Sie diese Informationen beim Ausführen der Bereitstellungsschritte eingeben können.

Die folgende Abbildung zeigt den Bereitstellungsbildschirm **Erstellen einer privaten Cloud**, auf dem das Feld **Virtual Network** hervorgehoben ist.

:::image type="content" source="media/pre-deployment/azure-vmware-solution-deployment-screen-vnet-circle.png" alt-text="Screenshot: Azure VMware Solution-Bereitstellungsbildschirm mit hervorgehobenem Feld „Virtual Network“":::

> [!NOTE]
> Ein virtuelles Netzwerk, das verwendet oder erstellt wird, kann von Ihrer lokalen Umgebung und von Azure VMware Solution erkannt werden. Stellen Sie daher sicher, dass sich das in diesem virtuellen Netzwerk verwendete IP-Segment und die Subnetze nicht überlappen.

## <a name="vmware-hcx-network-segments"></a>VMware HCX-Netzwerksegmente

VMware HCX ist eine Technologie, die Teil des Azure VMware Solution-Pakets ist. Die primären Anwendungsfälle für VMware HCX sind Workloadmigrationen und Notfallwiederherstellung. Wenn Sie beides durchführen möchten, ist es ratsam, das Netzwerk jetzt zu planen. Andernfalls können Sie diesen Schritt überspringen und mit dem nächsten fortfahren.

[!INCLUDE [hcx-network-segments](includes/hcx-network-segments.md)]

## <a name="optional-extend-your-networks"></a>(Optional) Erweitern von Netzwerken

Sie können Netzwerksegmente aus der lokalen Umgebung auf Azure VMware Solution erweitern. Wenn Sie dies tun, sollten Sie diese Netzwerke jetzt identifizieren.  

Hier sind einige Faktoren zu beachten:

- Wenn Sie die Erweiterung von Netzwerken aus der lokalen Umgebung planen, muss für diese Netzwerke eine Verbindung mit einem [vSphere Distributed Switch (vDS)](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.networking.doc/GUID-B15C6A13-797E-4BCB-B9D9-5CBC5A60C3A6.html) in Ihrer lokalen VMware-Umgebung bestehen.  
- Netzwerke, die sich auf einem [vSphere Standard Switch](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.networking.doc/GUID-350344DE-483A-42ED-B0E2-C811EE927D59.html) befinden, können nicht erweitert werden.

>[!NOTE]
>Diese Netzwerke werden im letzten Schritt der Konfiguration erweitert, nicht während der Bereitstellung.
>
## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie die erforderlichen Informationen nun gesammelt und dokumentiert haben, können Sie mit dem nächsten Abschnitt fortfahren, um Ihre private Azure VMware Solution-Cloud zu erstellen.

> [!div class="nextstepaction"]
> [Bereitstellen von Azure VMware Solution](deploy-azure-vmware-solution.md)
> 
