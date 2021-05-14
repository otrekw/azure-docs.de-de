---
title: Planen der Azure VMware Solution-Bereitstellung
description: In diesem Artikel wird der Workflow für die Bereitstellung einer Azure VMware Solution-Instanz beschrieben.  Das Endergebnis ist eine Umgebung, die für die Erstellung und Migration von virtuellen Computern (VMs) vorbereitet ist.
ms.topic: tutorial
ms.custom: contperf-fy21q4
ms.date: 04/27/2021
ms.openlocfilehash: bc78830180f12c947b195b8525ea9b100b123836
ms.sourcegitcommit: 516eb79d62b8dbb2c324dff2048d01ea50715aa1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108179532"
---
# <a name="plan-the-azure-vmware-solution-deployment"></a>Planen der Azure VMware Solution-Bereitstellung

In dieser Schnellstartanleitung wird der Planungsprozess zum Ermitteln und Sammeln der Informationen erläutert, die Sie für Ihre Bereitstellung benötigen. Stellen Sie beim Planen Ihrer Bereitstellung sicher, dass Sie die gesammelten Informationen dokumentieren, damit Sie während des Bereitstellungsvorgangs leicht darauf zugreifen können.

Durch die beschriebenen Schritte erhalten Sie eine produktionsbereite Umgebung zum Erstellen von virtuellen Computern (Virtual Machines, VMs) sowie für die Migration. 

>[!TIP]
>Verwenden Sie zum Nachverfolgen der von Ihnen gesammelten Daten die [VMware HCX-Prüfliste für die Planung](https://www.virtualworkloads.com/2021/04/hcx-planning-checklist/).


## <a name="request-a-host-quota"></a>Anfordern eines Hostkontingents 

Es ist wichtig, frühzeitig ein Hostkontingent anzufordern, wenn Sie sich auf die Erstellung Ihrer Azure VMware Solution-Ressource vorbereiten. Sie können jetzt ein Hostkontingent anfordern. Wenn der Planungsprozess abgeschlossen ist, können Sie direkt die private Azure VMware Solution-Cloud bereitstellen: Nachdem das Supportteam Ihre Anforderung eines Hostkontingents erhalten hat, dauert es bis zu fünf Werktage, bis die Anforderung bestätigt wurde und Ihre Hosts zugewiesen wurden. Wenn Sie über eine vorhandene private Azure VMware Solution-Cloud verfügen und weitere Hosts zugeordnet werden sollen, müssen Sie auf die gleiche Weise vorgehen. Weitere Informationen finden Sie je nach Abonnementtyp unter den folgenden Links:
- [EA-Kunden](enable-azure-vmware-solution.md?tabs=azure-portal#request-host-quota-for-ea-customers)
- [CSP-Kunden](enable-azure-vmware-solution.md?tabs=azure-portal#request-host-quota-for-csp-customers)

## <a name="identify-the-subscription"></a>Identifizieren des Abonnements

Identifizieren Sie das Abonnement, das Sie zum Bereitstellen von Azure VMware Solution verwenden möchten.  Sie können entweder ein neues Abonnement erstellen oder ein vorhandenes wiederverwenden.

>[!NOTE]
>Das Abonnement muss einem Microsoft Enterprise Agreement oder einem Azure-Plan für Cloud Solution Provider zugeordnet sein. Weitere Informationen finden Sie unter [Aktivieren einer Azure VMware Solution-Ressource](enable-azure-vmware-solution.md).

## <a name="identify-the-resource-group"></a>Identifizieren der Ressourcengruppe

Identifizieren Sie die Ressourcengruppe, die Sie für Ihre Azure VMware Solution-Instanz verwenden möchten.  Im Allgemeinen wird eine Ressourcengruppe speziell für Azure VMware Solution erstellt, aber Sie können auch eine vorhandene Ressourcengruppe verwenden.

## <a name="identify-the-region-or-location"></a>Identifizieren der Region oder des Standorts

Identifizieren Sie die Region, in der Azure VMware Solution bereitgestellt werden soll.  Weitere Informationen finden Sie unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/en-us/global-infrastructure/services/?products=azure-vmware).

## <a name="identify-the-resource-name"></a>Identifizieren des Ressourcennamens

Legen Sie den Ressourcennamen fest, den Sie während der Bereitstellung verwenden möchten.  Der Ressourcenname ist ein benutzerfreundlicher und beschreibender Name, in dem Sie Ihre private Azure VMware Solution-Cloud angeben.

>[!IMPORTANT]
>Der Name darf maximal 40 Zeichen lang sein. Wenn der Name diesen Grenzwert überschreitet, können Sie keine öffentlichen IP-Adressen für die Verwendung mit der privaten Cloud erstellen. 

## <a name="identify-the-size-hosts"></a>Identifizieren der Größenhosts

Identifizieren Sie die Größenhosts, die Sie beim Bereitstellen von Azure VMware Solution verwenden möchten.  

[!INCLUDE [disk-capabilities-of-the-host](includes/disk-capabilities-of-the-host.md)]

## <a name="determine-the-number-of-clusters-and-hosts"></a>Bestimmen der Anzahl von Clustern und Hosts

Ihre erste Azure VMware Solution-Bereitstellung umfasst eine private Cloud mit einem einzelnen Cluster. Für Ihre Bereitstellung müssen Sie die Anzahl von Hosts definieren, die Sie im ersten Cluster bereitstellen möchten.

[!INCLUDE [hosts-minimum-initial-deployment-statement](includes/hosts-minimum-initial-deployment-statement.md)]

## <a name="define-the-ip-address-segment-for-private-cloud-management"></a>Definieren des IP-Adressensegments für die Verwaltung der privaten Cloud

Der erste Schritt bei der Planung der Bereitstellung ist das Planen der IP-Segmentierung. Für Azure VMware Solution wird ein CIDR-Netzwerk vom Typ „/22“ benötigt. Dieser Adressraum wird in kleinere Netzwerksegmente (Subnetze) unterteilt und für Azure VMware Solution-Verwaltungssegmente (u. a. vCenter-, VMware HCX-, NSX-T- und vMotion-Funktionen) verwendet. Die Visualisierung weiter unten zeigt, wo dieses Segment verwendet wird.

Dieser CIDR-Netzwerkadressblock vom Typ „/22“ darf sich mit keinem Netzwerksegment überschneiden, das bereits lokal oder in Azure vorhanden ist.

**Beispiel:** 10.0.0.0/22

Eine detaillierte Aufschlüsselung des CIDR-Netzwerks vom Typ „/22“ nach privater Cloud finden Sie unter [Checkliste für die Netzwerkplanung für Azure VMware Solution](tutorial-network-checklist.md#routing-and-subnet-considerations).

:::image type="content" source="media/pre-deployment/management-vmotion-vsan-network-ip-diagram.png" alt-text="Identifizieren: IP-Adressensegment" border="false":::  

## <a name="define-the-ip-address-segment-for-vm-workloads"></a>Definieren des IP-Adressensegments für VM-Workloads

Wie bei jeder VMware-Umgebung müssen die virtuellen Computer eine Verbindung mit einem Netzwerksegment herstellen. In Azure VMware Solution gibt es zwei Arten von Segmenten: erweiterte L2-Segmente (weiter unten behandelt) und NSX-T-Netzwerksegmente. Wenn die Produktionsbereitstellung von Azure VMware Solution erweitert wird, gibt es häufig eine Kombination aus erweiterten L2-Segmenten aus der lokalen Umgebung und lokalen NSX-T-Netzwerksegmenten. Geben Sie zum Planen der ersten Bereitstellung in Azure VMware Solution ein einzelnes Netzwerksegment (IP-Netzwerk) an. Dieses Netzwerk darf sich nicht mit lokalen Netzwerksegmenten oder innerhalb des restlichen Azure-Netzwerks überlappen und darf nicht innerhalb des zuvor definierten Netzwerksegments vom Typ „/22“ liegen.

Dieses Netzwerksegment wird hauptsächlich zu Testzwecken während der ersten Bereitstellung verwendet.

>[!NOTE]
>Diese Netzwerke werden während der Bereitstellung nicht benötigt. Sie werden in einem Schritt nach der Bereitstellung erstellt.
  
**Beispiel:** 10.0.4.0/24

:::image type="content" source="media/pre-deployment/nsx-segment-diagram.png" alt-text="Identifizieren: IP-Adressensegment für VM-Workloads" border="false":::     

## <a name="define-the-virtual-network-gateway"></a>Definieren des VNET-Gateways

>[!IMPORTANT]
>Sie können eine Verbindung mit einem VNET-Gateway in Azure Virtual WAN herstellen, dies wird in dieser Schnellstartanleitung jedoch nicht erläutert.

Eine private Azure VMware Solution-Cloud erfordert ein virtuelles Azure-Netzwerk und eine ExpressRoute-Leitung.  

Legen Sie fest, ob Sie ein *vorhandenes* ODER ein *neues* ExpressRoute-VNET-Gateway verwenden möchten.  Wenn Sie sich für die Verwendung eines *neuen* VNET-Gateways entscheiden, erstellen Sie dieses nach der Erstellung der privaten Cloud. Die Verwendung eines vorhandenen ExpressRoute-VNET-Gateways ist akzeptabel. Notieren Sie sich zu Planungszwecken, welches ExpressRoute-VNET-Gateway Sie verwenden. 

:::image type="content" source="media/pre-deployment/azure-vmware-solution-expressroute-diagram.png" alt-text="Diagramm: An Azure VMware Solution angefügtes virtuelles Azure-Netzwerk" border="false":::



## <a name="define-vmware-hcx-network-segments"></a>Definieren der VMware HCX-Netzwerksegmente

VMware HCX ist eine Technologie, die Teil des Azure VMware Solution-Pakets ist. Die primären Anwendungsfälle für VMware HCX sind Workloadmigrationen und Notfallwiederherstellung. Wenn Sie beides durchführen möchten, ist es ratsam, das Netzwerk jetzt zu planen. Andernfalls können Sie diesen Schritt überspringen und mit dem nächsten fortfahren.

[!INCLUDE [hcx-network-segments](includes/hcx-network-segments.md)]

## <a name="determine-whether-to-extend-your-networks"></a>Bestimmen, ob Ihre Netzwerke erweitert werden sollen

Sie können Netzwerksegmente optional aus der lokalen Umgebung auf Azure VMware Solution erweitern. 

In diesem Fall sollten Sie diese Netzwerke jetzt gemäß der folgenden Richtlinien identifizieren:

- Für die Netzwerke muss eine Verbindung mit einem [vSphere Distributed Switch (vDS)](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.networking.doc/GUID-B15C6A13-797E-4BCB-B9D9-5CBC5A60C3A6.html) in Ihrer lokalen VMware-Umgebung hergestellt werden.  
- Netzwerke, die sich auf einem [vSphere Standard Switch](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.networking.doc/GUID-350344DE-483A-42ED-B0E2-C811EE927D59.html) befinden, können nicht erweitert werden.

>[!IMPORTANT]
>Diese Netzwerke werden im letzten Schritt der Konfiguration erweitert, nicht während der Bereitstellung.


## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie die erforderlichen Informationen nun gesammelt und dokumentiert haben, können Sie mit dem nächsten Abschnitt fortfahren, um Ihre private Azure VMware Solution-Cloud zu erstellen.

> [!div class="nextstepaction"]
> [Bereitstellen von Azure VMware Solution](deploy-azure-vmware-solution.md)> 
