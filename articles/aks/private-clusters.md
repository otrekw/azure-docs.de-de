---
title: Erstellen eines privaten Azure Kubernetes Service-Clusters
description: Erfahren Sie, wie Sie einen privaten Azure Kubernetes Service-Cluster (AKS) erstellen.
services: container-service
ms.topic: article
ms.date: 2/21/2020
ms.openlocfilehash: 0a05bd15fff97d4f0020f6ce82ee90a2fe995edf
ms.sourcegitcommit: 8f4d54218f9b3dccc2a701ffcacf608bbcd393a6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2020
ms.locfileid: "78944200"
---
# <a name="create-a-private-azure-kubernetes-service-cluster-preview"></a>Erstellen eines privaten Azure Kubernetes Service-Clusters (Preview)

In einem privaten Cluster besitzt die Steuerungsebene oder der API-Server interne IP-Adressen, die im Dokument [RFC1918 – -Allocation for Private Internets](https://tools.ietf.org/html/rfc1918) definiert sind. Durch die Verwendung eines privaten Clusters können Sie sicherstellen, dass der Netzwerkdatenverkehr zwischen Ihrem API-Server und den Knotenpools ausschließlich im privaten Netzwerk verbleibt.

Die Steuerungsebene oder der API-Server befindet sich in einem von Azure Kubernetes Service (AKS) verwalteten Azure-Abonnement. Der Cluster oder Knotenpool eines Kunden befindet sich im Abonnement des Kunden. Der Server und der Cluster oder Knotenpool können über den [Azure Private Link-Dienst][private-link-service] im virtuellen Netzwerk des API-Servers und einen privaten Endpunkt, der im Subnetz des AKS-Clusters des Kunden verfügbar gemacht wird, miteinander kommunizieren.

> [!IMPORTANT]
> AKS-Previewfunktionen stehen gemäß dem Self-Service-Prinzip und auf Aktivierungsbasis zur Verfügung. Vorschauversionen werden *wie besehen* und *wie verfügbar* bereitgestellt und sind von den Vereinbarungen zum Service Level (SLA) und der eingeschränkten Garantie ausgeschlossen. AKS-Vorschauversionen werden teilweise vom Kundensupport auf Grundlage der *bestmöglichen Leistung* abgedeckt. Daher sind die Funktionen nicht für den Einsatz in der Produktion vorgesehen. Weitere Informationen finden Sie in den folgenden Supportartikeln:
>
> * [Unterstützungsrichtlinien für Azure Kubernetes Service](support-policies.md)
> * [Häufig gestellte Fragen zum Azure-Support](faq.md)

## <a name="prerequisites"></a>Voraussetzungen

* Die Version 2.0.77 oder höher der Azure CLI sowie die Version 0.4.18 der AKS-Vorschauerweiterung für die Azure CLI.

## <a name="currently-supported-regions"></a>Derzeit unterstützte Regionen

* Australien (Osten)
* Australien, Südosten
* Brasilien Süd
* Kanada, Mitte
* Kanada, Osten
* USA, Mitte
* Asien, Osten
* East US
* USA (Ost) 2
* USA, Osten 2 (EUAP)
* Frankreich, Mitte
* Deutschland, Norden
* Japan, Osten
* Japan, Westen
* Korea, Mitte
* Korea, Süden
* USA Nord Mitte
* Nordeuropa
* Nordeuropa
* USA Süd Mitte
* UK, Süden
* Europa, Westen
* USA (Westen)
* USA, Westen 2
* USA (Ost) 2

## <a name="currently-supported-availability-zones"></a>Derzeit unterstützte Verfügbarkeitszonen

* USA (Mitte)
* East US
* USA (Ost) 2
* Frankreich, Mitte
* Japan, Osten
* Nordeuropa
* Asien, Südosten
* UK, Süden
* Europa, Westen
* USA, Westen 2

## <a name="install-the-latest-azure-cli-aks-preview-extension"></a>Installieren der neuesten AKS-Vorschauerweiterung für die Azure CLI

Um private Cluster zu verwenden, benötigen Sie die Version 0.4.18 oder höher der AKS-Vorschauerweiterung für die Azure CLI. Installieren Sie die AKS-Vorschauerweiterung für die Azure CLI, indem Sie den Befehl [az extension add][az-extension-add] verwenden und dann mit dem folgenden Befehl [az extension update][az-extension-update] nach verfügbaren Updates suchen:

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```
> [!CAUTION]
> Wenn Sie ein Feature für ein Abonnement registrieren, können Sie die Registrierung dieses Features momentan nicht mehr aufheben. Nachdem Sie einige Previewfunktionen aktiviert haben, können Sie Standardeinstellungen für alle AKS-Cluster verwenden, die in dem Abonnement erstellt wurden. Aktivieren Sie keine Vorschaufeatures für Produktionsabonnements. Verwenden Sie ein separates Abonnement, um Vorschaufeatures zu testen und Feedback zu sammeln.

```azurecli-interactive
az feature register --name AKSPrivateLinkPreview --namespace Microsoft.ContainerService
```

Es kann einige Minuten dauern, bis der Registrierungsstatus als *Registriert* angezeigt wird. Sie können den Status mithilfe des folgenden Befehls [az feature list][az-feature-list] überprüfen:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/AKSPrivateLinkPreview')].{Name:name,State:properties.state}"
```

Wenn der Zustand „Registriert“ ist, aktualisieren Sie die Registrierung des *Microsoft.ContainerService*-Ressourcenanbieters mit dem folgenden Befehl [az provider register][az-provider-register]:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
az provider register --namespace Microsoft.Network
```
## <a name="create-a-private-aks-cluster"></a>Erstellen eines privaten AKS-Clusters

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie eine Ressourcengruppe, oder verwenden Sie eine vorhandene Ressourcengruppe für Ihren AKS-Cluster.

```azurecli-interactive
az group create -l westus -n MyResourceGroup
```

### <a name="default-basic-networking"></a>Standardnetzwerke (Basic) 

```azurecli-interactive
az aks create -n <private-cluster-name> -g <private-cluster-resource-group> --load-balancer-sku standard --enable-private-cluster  
```
Dabei gilt: *--enable-private-cluster* ist ein obligatorisches Flag für einen privaten Cluster. 

### <a name="advanced-networking"></a>Erweiterte Netzwerke  

```azurecli-interactive
az aks create \
    --resource-group <private-cluster-resource-group> \
    --name <private-cluster-name> \
    --load-balancer-sku standard \
    --enable-private-cluster \
    --network-plugin azure \
    --vnet-subnet-id <subnet-id> \
    --docker-bridge-address 172.17.0.1/16 \
    --dns-service-ip 10.2.0.10 \
    --service-cidr 10.2.0.0/24 
```
Dabei gilt: *--enable-private-cluster* ist ein obligatorisches Flag für einen privaten Cluster. 

> [!NOTE]
> Wenn CIDR der Docker-Bridge-Adresse (172.17.0.1/16) mit der Subnetz-CIDR in Konflikt steht, ändern Sie die Docker-Bridge-Adresse entsprechend.

## <a name="options-for-connecting-to-the-private-cluster"></a>Optionen zum Herstellen einer Verbindung mit dem privaten Cluster

Der Endpunkt des API-Servers weist keine öffentliche IP-Adresse auf. Zum Verwalten des API-Servers müssen Sie einen virtuellen Computer verwenden, der Zugriff auf das Azure Virtual Network (VNET) des AKS-Clusters hat. Es gibt mehrere Optionen zum Einrichten der Netzwerkkonnektivität mit dem privaten Cluster.

* Erstellen Sie einen virtuellen Computer in demselben Azure Virtual Network (VNET) wie dem des AKS-Clusters.
* Verwenden Sie einen virtuellen Computer in einem separaten Netzwerk, und richten Sie ein [Peering virtueller Netzwerke][virtual-network-peering] ein.  Weitere Informationen zu dieser Option finden Sie im nachstehenden Abschnitt.
* Verwenden Sie eine [ExpressRoute- oder VPN][express-route-or-VPN]-Verbindung.

Das Erstellen eines virtuellen Computers in demselben VNET wie dem des AKS-Clusters ist die einfachste Option.  ExpressRoute und VPNs (virtuelle private Netzwerke) erhöhen die Kosten und erfordern zusätzliche Netzwerkkomplexität.  Beim Peering virtueller Netzwerke müssen Sie Ihre Netzwerk-CIDR-Bereiche planen, um sicherzustellen, dass es keine überlappenden Bereiche gibt.

## <a name="virtual-network-peering"></a>Peering in virtuellen Netzwerken

Wie bereits erwähnt, ist VNET-Peering eine Möglichkeit für den Zugriff auf Ihren privaten Cluster. Wenn Sie VNET-Peering verwenden möchten, müssen Sie eine Verknüpfung zwischen dem virtuellen Netzwerk und der privaten DNS-Zone einrichten.
    
1. Wechseln Sie im Azure-Portal zur Ressourcengruppe „MC_*“.  
2. Wählen Sie die private DNS-Zone aus.   
3. Wählen Sie im linken Bereich den Link **Virtuelles Netzwerk** aus.  
4. Erstellen Sie eine neue Verknüpfung, um das virtuelle Netzwerk des virtuellen Computers der privaten DNS-Zone hinzuzufügen. Es dauert ein paar Minuten, bis der DNS-Zonenlink verfügbar wird.  
5. Wechseln Sie im Azure-Portal zurück zur Ressourcengruppe „MC_*“.  
6. Wählen Sie im rechten Bereich das virtuelle Netzwerk aus. Der Name des virtuellen Netzwerks hat die Form *aks-vnet-\** .  
7. Wählen Sie im linken Bereich **Peerings** aus.  
8. Wählen Sie **Hinzufügen** aus, fügen Sie das virtuelle Netzwerk des virtuellen Computers hinzu, und erstellen Sie dann das Peering.  
9. Wechseln Sie zu dem virtuellen Netzwerk, in dem sich der virtuelle Computer befindet, wählen Sie **Peerings** und dann das virtuelle AKS-Netzwerk aus, und erstellen Sie dann das Peering. Wenn die Adressbereiche im virtuellen AKS-Netzwerk mit denen im virtuellen Netzwerk des virtuellen Computers in Konflikt stehen, schlägt das Peering fehl. Weitere Informationen finden Sie unter [Peering in virtuellen Netzwerken][virtual-network-peering].

## <a name="dependencies"></a>Abhängigkeiten  
* Der Private Link-Dienst wird nur von Load Balancer Standard unterstützt. Der Load Balancer Basic wird nicht unterstützt.  
* Um einen benutzerdefinierten DNS-Server zu verwenden, stellen Sie einen AD-Server mit DNS für die Weiterleitung an die IP-Adresse 168.63.129.16 bereit

## <a name="limitations"></a>Einschränkungen 
* IP-autorisierte Bereiche können nicht auf den privaten API-Serverendpunkt angewandt werden, sie gelten nur für den öffentlichen API-Server.
* Verfügbarkeitszonen werden derzeit für bestimmte Regionen unterstützt, wie am Anfang dieses Dokuments beschrieben. 
* Die [Einschränkungen für den Azure Private Link-Dienst][private-link-service] gelten für private Cluster, private Azure-Endpunkte und VNET-Dienstendpunkte, die zurzeit nicht im selben virtuellen Netzwerk unterstützt werden.
* Keine Unterstützung für virtuelle Knoten in einem privaten Cluster zum Wechseln privater Azure Container Instances (ACI) in einem privaten virtuellen Azure-Netzwerk
* Keine standardmäßige Unterstützung für die Azure DevOps-Integration mit privaten Clustern
* Für Kunden, die Azure Container Registry für die Arbeit mit privatem AKS aktivieren müssen, muss ein Peering des virtuellen Netzwerks der Container Registry mit dem virtuellen Netzwerk des Agent-Clusters ausgeführt werden.
* Keine aktuelle Unterstützung für Azure Dev Spaces
* Keine Unterstützung für das Konvertieren vorhandener AKS-Cluster in private Cluster
* Das Löschen oder Ändern des privaten Endpunkts im Kundensubnetz führt dazu, dass der Cluster nicht mehr funktioniert. 
* Azure Monitor wird für Livedaten von Containern derzeit nicht unterstützt.
* *Das Verwenden eines eigenen DNS (BYOD)* wird derzeit nicht unterstützt.


<!-- LINKS - internal -->
[az-provider-register]: /cli/azure/provider?view=azure-cli-latest#az-provider-register
[az-feature-list]: /cli/azure/feature?view=azure-cli-latest#az-feature-list
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[private-link-service]: /private-link/private-link-service-overview
[virtual-network-peering]: ../virtual-network/virtual-network-peering-overview.md
[azure-bastion]: ../bastion/bastion-create-host-portal.md
[express-route-or-vpn]: ../expressroute/expressroute-about-virtual-network-gateways.md

