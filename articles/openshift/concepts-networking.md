---
title: 'Konzepte: Netzwerkdiagramm für Azure Red Hat OpenShift 4'
description: Netzwerkdiagramm und Übersicht für Azure Red Hat OpenShift-Netzwerke
author: sakthi-vetrivel
ms.author: suvetriv
ms.topic: tutorial
ms.service: container-service
ms.date: 06/22/2020
ms.openlocfilehash: fb81405e85d6e2653e0cf6c007c363493992161a
ms.sourcegitcommit: e71da24cc108efc2c194007f976f74dd596ab013
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2020
ms.locfileid: "87419970"
---
# <a name="networking-in-azure-red-hat-on-openshift-4"></a>Netzwerk in Azure Red Hat OpenShift 4

Dieser Leitfaden enthält eine Übersicht über Netzwerke in Clustern mit Azure Red Hat OpenShift 4 sowie ein Diagramm und eine Liste wichtiger Endpunkte.

Weitere ausführliche Informationen zu wichtigen OpenShift-Netzwerkkonzepten finden Sie in der [Netzwerkdokumentation für Azure Red Hat OpenShift 4](https://docs.openshift.com/aro/4/networking/understanding-networking.html).

## <a name="networking-concepts-in-azure-red-hat-openshift"></a>Netzwerkkonzepte in Azure Red Hat OpenShift

![Azure Red Hat OpenShift 4: Netzwerkdiagramm](./media/concepts-networking/aro4-networking-diagram.png)

Wenn Sie Azure Red Hat OpenShift 4 bereitstellen, befindet sich Ihr gesamter Cluster in einem virtuellen Netzwerk. Innerhalb dieses virtuellen Netzwerks befinden sich Ihre Master- und Workerknoten jeweils in einem eigenen Subnetz. Von den einzelnen Subnetzen wird jeweils ein öffentlicher und interner Lastenausgleich verwendet.

## <a name="explanation-of-endpoints"></a>Erläuterungen zu Endpunkten

Die folgende Liste enthält wichtige Endpunkte in einem Azure Red Hat OpenShift-Cluster:

* **aro-pls**
    * Hierbei handelt es sich um einen Azure Private Link-Endpunkt, der von Websitezuverlässigkeits-Technikern (Site Reliability Engineers, SREs) von Microsoft und Red Hat genutzt wird, um die Clusterverwaltung zu erleichtern.
* **aro-internal-lb**
    * Dieser Endpunkt gleicht den Datenverkehr für den API-Server aus. Für diesen Lastenausgleich sind die Masterknoten im Back-End-Pool.
* **aro-public-lb**
    * Wenn die API öffentlich ist, wird Datenverkehr durch diesen Endpunkt an den API-Server weitergeleitet und ausgeglichen. Von diesem Endpunkt wird eine öffentliche ausgehende IP-Adresse zugewiesen, damit Masterknoten auf Azure Resource Manager zugreifen und Informationen zur Clusterintegrität zurückgeben können.
* **aro-internal**
    * Dieser Endpunkt gleicht internen Dienstdatenverkehr aus. Für diesen Lastenausgleich sind die Workerknoten im Back-End-Pool.
    * Dieser Lastenausgleich wird nicht standardmäßig erstellt. Dieser Lastenausgleich wird erstellt, wenn Sie einen Dienst vom Typ „LoadBalancer“ mit den korrekten Anmerkungen erstellen. Beispiel: service.beta.kubernetes.io/azure-load-balancer-internal: "true".
* **Netzwerkrichtlinien (eingehend)**
    * Unterstützt im Rahmen von OpenShift SDN
    * Standardmäßig aktiviert, kundenseitige Erzwingung
    * Kompatibel mit V1-Netzwerkrichtlinie (NetworkPolicy); „Egress“ (Ausgehend) und „IPBlock“ (IP-Block) werden jedoch noch nicht unterstützt.
    * **aro**
    * Dieser Endpunkt gleicht den Datenverkehr für den API-Server aus. Für diesen Lastenausgleich sind die Masterknoten im Back-End-Pool.
  * **aro-internal-lb**
    * Dieser Endpunkt wird für öffentlichen Datenverkehr verwendet. Wenn Sie eine Anwendung und eine Route erstellen, ist dies der Pfad für eingehenden Datenverkehr.
    * Dieser Lastenausgleich deckt auch ausgehende Internetkonnektivität beliebiger Pods ab, die in den Workerknoten ausgeführt werden (über Azure Load Balancer-Ausgangsregeln).
        * Ausgangsregeln können aktuell nicht konfiguriert werden. Durch sie werden jedem Knoten 1.024 TCP-Ports zugewiesen.
        * Da „DisableOutboundSnat“ in den LB-Regeln nicht konfiguriert ist, können Pods als IP-Adresse für ausgehenden Datenverkehr eine beliebige öffentliche IP-Adresse erhalten, die in dieser ALB-Instanz konfiguriert ist.
        * Aufgrund der beiden vorherigen Punkte müssen ARO öffentliche Dienste vom Typ „LoadBalancer“ hinzugefügt werden, wenn Sie kurzlebige SNAT-Ports hinzufügen möchten.
* **Netzwerkrichtlinien (ausgehend)**
    * Richtlinien für ausgehenden Datenverkehr werden mithilfe der Firewall für ausgehenden Datenverkehr in OpenShift unterstützt.
    * Pro Namespace/Projekt ist nur jeweils eine Instanz zulässig.
    * Richtlinien für ausgehenden Datenverkehr werden im Standardnamespace nicht unterstützt.
    * Regeln von Richtlinien für ausgehenden Datenverkehr werden von der ersten bis zur letzten Regel ausgewertet.
    * **aro-outbound-pip**
        * Dieser Endpunkt fungiert als öffentliche IP-Adresse (Public IP, PIP) für die Workerknoten.
        * Dieser Endpunkt ermöglicht es Diensten, eine bestimmte IP-Adresse aus einem Azure Red Hat OpenShift-Cluster einer Zulassungsliste hinzuzufügen.
* **aro-node-nsg**
    * Wenn Sie einen Dienst verfügbar machen, wird von der API eine Regel in dieser Netzwerksicherheitsgruppe erstellt, damit Datenverkehr weitergeleitet wird und die Knoten erreichen kann.
    * Durch diese Netzwerksicherheitsgruppe wird standardmäßig der gesamte ausgehende Datenverkehr zugelassen. Ausgehender Datenverkehr kann aktuell nur auf die Azure Red Hat OpenShift-Steuerungsebene beschränkt werden.
* **aro-controlplane-nsg**
    * Durch diesen Endpunkt wird eingehender Datenverkehr nur über den Port 6443 für die Masterknoten zugelassen.
* **Azure Container Registry**
    * Hierbei handelt es sich um eine von Microsoft intern bereitgestellte und verwendete Containerregistrierung.
        * Durch diese Registrierung werden Hostplattformimages und Clusterkomponenten bereitgestellt. Beispiele wären etwa Container für die Überwachung oder Protokollierung.
        * Nicht für die Verwendung durch Azure Red Hat OpenShift-Kunden vorgesehen  
        * Schreibgeschützt
        * Verbindungen mit dieser Registrierung werden über den Dienstendpunkt hergestellt (interne Konnektivität zwischen Azure-Diensten).
        * Diese interne Registrierung steht außerhalb des Clusters standardmäßig nicht zur Verfügung.
* **Private Link**
    * Ermöglicht die Clusterverwaltung über eine Netzwerkverbindung zwischen der Verwaltungsebene und einem Cluster.
    * Websitezuverlässigkeits-Techniker (Site Reliability Engineers, SREs) von Microsoft und Red Hat zur Unterstützung bei der Clusterverwaltung

## <a name="networking-basics-in-openshift"></a>Netzwerkgrundlagen in OpenShift

OpenShift SDN (Software-Defined Networking) wird verwendet, um ein Overlaynetzwerk mithilfe von Open vSwitch (OVS) zu konfigurieren. Hierbei handelt es sich um eine OpenFlow-Implementierung, die auf der CNI-Spezifikation (Container Network Interface, Containernetzwerkschnittstelle) basiert. Von SDN werden verschiedene Plug-Ins unterstützt, und in Azure Red Hat OpenShift 4 wird das Plug-In „Network Policy“ (Netzwerkrichtlinie) verwendet. Da die gesamte Netzwerkkommunikation durch SDN verwaltet wird, sind für die Kommunikation zwischen Pods keine zusätzlichen Routen in Ihren virtuellen Netzwerken erforderlich.

## <a name="azure-red-hat-openshift-networking-specifics"></a>Spezielle Netzwerkfeatures von Azure Red Hat OpenShift

Bei den folgenden Punkten handelt es sich um spezielle Features von Azure Red Hat OpenShift:
* Die Verwendung eines eigenen virtuellen Netzwerks wird unterstützt.
* Pod- und Dienstnetzwerk-CIDRs sind konfigurierbar.
* Knoten und Master befinden sich in unterschiedlichen Subnetzen.
* Die VNET-Subnetze von Knoten und Mastern müssen eine Mindestgröße von /27 haben.
* Pod-CIDR muss eine Mindestgröße von /18 haben. (Beim Podnetzwerk handelt es sich um nicht routingfähige IP-Adressen, und es wird ausschließlich innerhalb von OpenShift SDN verwendet.)
* Jedem Knoten wird ein Subnetz vom Typ „/23“ (512 IP-Adressen) für die zugehörigen Pods zugewiesen. Dieser Wert kann nicht geändert werden.
* Ein Pod kann nicht an mehrere Netzwerke angefügt werden.
* Es kann keine statische IP-Adresse für ausgehenden Datenverkehr konfiguriert werden. (Hierbei handelt es sich um ein OpenShift-Feature. Weitere Informationen finden Sie unter [Konfigurieren von IP-Adressen für ausgehenden Datenverkehr](https://docs.openshift.com/aro/4/networking/openshift_sdn/assigning-egress-ips.html).)

## <a name="network-settings"></a>Netzwerkeinstellungen

In Azure Red Hat OpenShift 4 stehen folgende Netzwerkeinstellungen zur Verfügung:

* **API Visibility** (API-Sichtbarkeit): Legen Sie die API-Sichtbarkeit beim Ausführen des Befehls [az aro create](tutorial-create-cluster.md#create-the-cluster) fest.
    * „Public“ (Öffentlich): Auf den API-Server kann von externen Netzwerken zugegriffen werden.
    * „Private“ (Privat): Vom API-Server wurde eine private IP-Adresse aus dem Mastersubnetz zugewiesen, und der Zugriff ist nur über verbundene Netzwerke (mittels Peering verbundene virtuelle Netzwerke, andere Subnetze im Cluster) möglich. Für den Kunden wird eine private DNS-Zone erstellt.
* **Ingress Visibility** (Sichtbarkeit für eingehenden Datenverkehr): Legen Sie die API-Sichtbarkeit beim Ausführen des Befehls [az aro create](tutorial-create-cluster.md#create-the-cluster) fest.
    * „Public“ (Öffentlich): Für Routen wird standardmäßig die öffentliche Load Balancer Standard-Instanz von Azure verwendet. (Kann geändert werden.)
    * „Private“ (Privat): Für Routen wird standardmäßig der interne Lastenausgleich verwendet. (Kann geändert werden.)

## <a name="network-security-groups"></a>Netzwerksicherheitsgruppen
Netzwerksicherheitsgruppen werden in der (gesperrten) Ressourcengruppe des Knotens erstellt. Die Netzwerksicherheitsgruppen werden direkt den Subnetzen zugewiesen und nicht den NICs des Knotens. Die Netzwerksicherheitsgruppen sind unveränderlich. Das bedeutet, dass Sie nicht berechtigt sind, sie zu ändern. 

Mit einem öffentlich sichtbaren API-Server können Sie jedoch keine Netzwerksicherheitsgruppen erstellen und sie den NICs zuweisen.

## <a name="domain-forwarding"></a>Domänenweiterleitung
Von Azure Red Hat OpenShift wird CoreDNS verwendet. Die Domänenweiterleitung kann konfiguriert werden. (Weitere Informationen finden Sie in der [Dokumentation zur Verwendung der DNS-Weiterleitung](https://docs.openshift.com/aro/4/networking/dns-operator.html#nw-dns-forward_dns-operator).)

Aktuell ist es nicht möglich, Ihr eigenes DNS in Ihren virtuellen Netzwerken zu verwenden.


Weitere Informationen zu ausgehendem Datenverkehr und zur Unterstützung von Azure Red Hat OpenShift in diesem Zusammenhang finden Sie unter [Unterstützungsrichtlinien in Azure Red Hat OpenShift](support-policies-v4.md).
