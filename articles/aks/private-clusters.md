---
title: Private Azure Kubernetes Service-Cluster
description: Erfahren Sie, wie Sie einen privaten Azure Kubernetes Service-Cluster (AKS) erstellen.
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 12/10/2019
ms.author: mlearned
ms.openlocfilehash: 8af0f998df2a92e51078a2e23806cca07ff08ca3
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75475016"
---
# <a name="public-preview---private-azure-kubernetes-service-cluster"></a>Public Preview: private Azure Kubernetes Service-Cluster

In einem privaten Cluster werden auf der Steuerungsebene bzw. dem API-Server interne IP-Adressen gemäß [RFC1918](https://tools.ietf.org/html/rfc1918) definiert.  Durch die Verwendung eines privaten Clusters können Sie sicherstellen, dass der Netzwerkdatenverkehr zwischen Ihrem API-Server und den Knotenpools ausschließlich im privaten Netzwerk verbleibt.

Die Kommunikation zwischen der Steuerungsebene bzw. dem API-Server in einem von AKS verwalteten Azure-Abonnement und dem Cluster/Knotenpool des Kunden, der sich in einem Kundenabonnement befindet, kann über den [privaten Linkdienst][private-link-service] im API-Server-VNET und einen privaten Endpunkt erfolgen, der im Subnetz des AKS-Clusters des Kunden verfügbar gemacht wird.

> [!IMPORTANT]
> AKS-Previewfunktionen stehen gemäß dem Self-Service- und Aktivierungsprinzip zur Verfügung. Vorschauversionen werden „wie besehen“ und „wie verfügbar“ bereitgestellt und sind von den Vereinbarungen zum Service Level und der eingeschränkten Garantie ausgeschlossen. AKS-Vorschauen werden teilweise vom Kundensupport auf der Grundlage der bestmöglichen Leistung abgedeckt. Daher sind diese Funktionen nicht für die Verwendung in der Produktion vorgesehen. Weitere Informationen finden Sie in den folgenden Supportartikeln:
>
> * [Unterstützungsrichtlinien für Azure Kubernetes Service](support-policies.md)
> * [Häufig gestellte Fragen zum Azure-Support](faq.md)

## <a name="before-you-begin"></a>Voraussetzungen

* Sie benötigen Version 2.0.77 oder höher der Azure-Befehlszeilenschnittstelle sowie die Erweiterung „aks-preview 0.4.18“.

## <a name="current-supported-regions"></a>Derzeit unterstützte Regionen
* USA (Westen)
* USA, Westen 2
* USA (Ost) 2
* Kanada, Mitte
* Nordeuropa
* Europa, Westen
* Australien (Osten)

## <a name="install-latest-aks-cli-preview-extension"></a>Installieren der neuesten AKS-CLI-Vorschauerweiterung

Um private Cluster zu verwenden, benötigen Sie mindestens Version 0.4.18 der CLI-Erweiterung *aks-preview*. Installieren Sie die Azure CLI-Erweiterung *aks-preview* mit dem Befehl [az extension add][az-extension-add], und suchen Sie dann mit dem Befehl [az extension update][az-extension-update] nach verfügbaren Updates:

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```
> [!CAUTION]
> Wenn Sie ein Feature für ein Abonnement registrieren, können Sie die Registrierung dieses Features momentan nicht mehr aufheben. Nachdem Sie einige Vorschaufeatures aktiviert haben, können Standardwerte für alle AKS-Cluster verwendet werden, die dann im Abonnement erstellt werden. Aktivieren Sie keine Vorschaufeatures für Produktionsabonnements. Verwenden Sie ein separates Abonnement, um Vorschaufeatures zu testen und Feedback zu sammeln.

```azurecli-interactive
az feature register --name AKSPrivateLinkPreview --namespace Microsoft.ContainerService
```

Es kann einige Minuten dauern, bis der Status *Registriert* angezeigt wird. Sie können den Registrierungsstatus mithilfe des Befehls [az feature list][az-feature-list] überprüfen:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/AKSPrivateLinkPreview')].{Name:name,State:properties.state}"
```

Wenn der Zustand registriert ist, aktualisieren Sie die Registrierung des *Microsoft.ContainerService*-Ressourcenanbieters mit dem Befehl [az provider register][az-provider-register]:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
az provider register --namespace Microsoft.Network
```
## <a name="create-a-private-aks-cluster"></a>Erstellen eines privaten AKS-Clusters

#### <a name="default-basic-networking"></a>Standardnetzwerke (Basic) 

```azurecli-interactive
az aks create -n <private-cluster-name> -g <private-cluster-resource-group> --load-balancer-sku standard --enable-private-cluster  
```
Dabei gilt: enable-private-cluster ist ein obligatorisches Flag für einen privaten Cluster. 

#### <a name="advanced-networking"></a>Erweiterte Netzwerke  

```azurecli-interactive
az aks create \ 
    --resource-group <private-cluster-resource-group>\ 
    --name <private-cluster-name> \ 
    --load-balancer-sku standard
    --enable-private-cluster 
    --network-plugin azure \ 
    --vnet-subnet-id <subnet-id> \ 
    --docker-bridge-address 172.17.0.1/16 \ 
    --dns-service-ip 10.2.0.10 \ 
    --service-cidr 10.2.0.0/24 \ 
```
Dabei gilt: enable-private-cluster ist ein obligatorisches Flag für einen privaten Cluster. 

## <a name="steps-to-connect-to-the-private-cluster"></a>Schritte für das Herstellen einer Verbindung mit dem privaten Cluster
Der Endpunkt des API-Servers weist keine öffentliche IP-Adresse auf. Daher müssen Benutzer einen virtuellen Azure-Computer in einem virtuellen Netzwerk erstellen und eine Verbindung mit dem API-Server herstellen. Führen Sie dazu folgende Schritte aus:

* Abrufen der Anmeldeinformationen für die Verbindung mit dem Cluster

   ```azurecli-interactive
   az aks get-credentials --name MyManagedCluster --resource-group MyResourceGroup
   ```
* Erstellen Sie einen virtuellen Computer im selben VNET wie der AKS-Cluster, oder erstellen Sie einen virtuellen Computer in einem anderen VNET, und führen Sie ein Peering dieses VNET mit dem VNET des AKS-Clusters durch.
* Wenn Sie einen virtuellen Computer in einem anderen VNET erstellen, müssen Sie eine Verknüpfung zwischen diesem VNET und der privaten DNS-Zone einrichten.
    * Navigieren Sie im Portal zur Ressourcengruppe „MC_*“. 
    * Klicken Sie auf die private DNS-Zone. 
    * Wählen Sie im linken Bereich die Verknüpfung mit dem virtuellen Netzwerk aus.
    * Erstellen Sie eine neue Verknüpfung, um das VNET der VM der privaten DNS-Zone hinzuzufügen. *(Es dauert einige Minuten, bis die Verknüpfung mit der DNS-Zone verfügbar wird.)*
* Stellen Sie eine SSH-Verbindung mit der VM her.
* Installieren Sie das Tool kubectl, und führen Sie kubectl-Befehle aus.

## <a name="dependencies"></a>Abhängigkeiten  
* Nur Standard-Lastenausgleich: Ein Lastenausgleich mit Basic-SKU wird nicht unterstützt.  

## <a name="limitations"></a>Einschränkungen 
* Für private Cluster gelten ebenfalls die [Einschränkungen für Azure Private Link][private-link-service], sodass private Azure-Endpunkte und VNET-Dienstendpunkte derzeit nicht im selben VNET unterstützt werden.
* Keine Unterstützung für virtuelle Knoten in einem privaten Cluster zum Wechseln privater ACI-Instanzen in einem privaten Azure-VNET
* Keine standardmäßige Unterstützung für die Azure DevOps-Integration mit privaten Clustern
* Wenn Kunden ACR für die Zusammenarbeit mit dem privaten AKS aktivieren müssen, muss ein Peering des ACR-VNET mit dem VNET des Agent-Clusters durchgeführt werden.
* Keine aktuelle Unterstützung für Azure Dev Spaces
* Keine Unterstützung für das Konvertieren vorhandener AKS-Cluster in private Cluster  
* Das Löschen oder Ändern des privaten Endpunkts im Kundensubnetz führt dazu, dass der Cluster nicht mehr funktioniert. 
* Azure Monitor wird für Livedaten von Containern derzeit nicht unterstützt.
* Das Verwenden eines eigenen DNS wird derzeit nicht unterstützt.


<!-- LINKS - internal -->
[az-provider-register]: /cli/azure/provider?view=azure-cli-latest#az-provider-register
[az-feature-list]: /cli/azure/feature?view=azure-cli-latest#az-feature-list
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[private-link-service]: https://docs.microsoft.com/azure/private-link/private-link-service-overview
