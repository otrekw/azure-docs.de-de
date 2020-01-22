---
title: Erstellen eines privaten Azure Kubernetes Service-Clusters
description: Erfahren Sie, wie Sie einen privaten Azure Kubernetes Service-Cluster (AKS) erstellen.
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 12/10/2019
ms.author: mlearned
ms.openlocfilehash: e01579272878a2436abca2ee50f0f6ea6cf78cbf
ms.sourcegitcommit: 05cdbb71b621c4dcc2ae2d92ca8c20f216ec9bc4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2020
ms.locfileid: "76045594"
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
* USA (Westen)
* USA, Westen 2
* USA (Ost) 2
* Kanada, Mitte
* Nordeuropa
* Europa, Westen
* Australien (Osten)

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

## <a name="connect-to-the-private-cluster"></a>Herstellen einer Verbindung mit dem privaten Cluster
Der Endpunkt des API-Servers weist keine öffentliche IP-Adresse auf. Daher müssen Sie einen virtuellen Azure-Computer (VM) in einem virtuellen Netzwerk erstellen und eine Verbindung mit dem API-Server herstellen. Führen Sie hierzu folgende Schritte aus:

1. Abrufen der Anmeldeinformationen für die Verbindung mit dem Cluster.

   ```azurecli-interactive
   az aks get-credentials --name MyManagedCluster --resource-group MyResourceGroup
   ```

1. Führen Sie einen der folgenden Schritte aus:
   * Erstellen Sie einen virtuellen Computer im selben virtuellen Netzwerk wie dem des AKS-Clusters.  
   * Erstellen Sie einen virtuellen Computer in einem anderen virtuellen Netzwerk, und stellen Sie ein Peering dieses virtuellen Netzwerks mit dem virtuellen Netzwerk des AKS-Clusters her.

     Wenn Sie einen virtuellen Computer in einem anderen virtuellen Netzwerk erstellen, richten Sie eine Verknüpfung zwischen diesem virtuellen Netzwerk und der privaten DNS-Zone ein. Gehen Sie folgendermaßen vor:
    
     a. Wechseln Sie im Azure-Portal zur Ressourcengruppe „MC_*“.  
     b. Wählen Sie die private DNS-Zone aus.   
     c. Wählen Sie im linken Bereich den Link **Virtuelles Netzwerk** aus.  
     d. Erstellen Sie eine neue Verknüpfung, um das virtuelle Netzwerk des virtuellen Computers der privaten DNS-Zone hinzuzufügen. Es dauert ein paar Minuten, bis der DNS-Zonenlink verfügbar wird.  
     e. Wechseln Sie im Azure-Portal zurück zur Ressourcengruppe „MC_*“.  
     f. Wählen Sie im rechten Bereich das virtuelle Netzwerk aus. Der Name des virtuellen Netzwerks hat die Form *aks-vnet-\** .  
     g. Wählen Sie im linken Bereich **Peerings** aus.  
     h. Wählen Sie **Hinzufügen** aus, fügen Sie das virtuelle Netzwerk des virtuellen Computers hinzu, und erstellen Sie dann das Peering.  
     i. Wechseln Sie zu dem virtuellen Netzwerk, in dem sich der virtuelle Computer befindet, wählen Sie **Peerings** und dann das virtuelle AKS-Netzwerk aus, und erstellen Sie dann das Peering. Wenn die Adressbereiche im virtuellen AKS-Netzwerk mit denen im virtuellen Netzwerk des virtuellen Computers in Konflikt stehen, schlägt das Peering fehl. Weitere Informationen finden Sie unter [Peering in virtuellen Netzwerken][virtual-network-peering].

1. Greifen Sie über Secure Shell (SSH) auf den virtuellen Computer zu.
1. Installieren Sie das Kubectl-Tool, und führen Sie die Kubectl-Befehle aus.


## <a name="dependencies"></a>Abhängigkeiten  
* Der Private Link-Dienst wird nur von Load Balancer Standard unterstützt. Der Load Balancer Basic wird nicht unterstützt.  

## <a name="limitations"></a>Einschränkungen 
* Die [Einschränkungen für den Azure Private Link-Dienst][private-link-service] gelten für private Cluster, private Azure-Endpunkte und VNET-Dienstendpunkte, die zurzeit nicht im selben virtuellen Netzwerk unterstützt werden.
* Keine Unterstützung für virtuelle Knoten in einem privaten Cluster zum Wechseln privater Cisco ACI-Instanzen (Cisco Application Centric Infrastructure) in einem privaten virtuellen Azure-Netzwerk.
* Keine standardmäßige Unterstützung für die Azure DevOps-Integration in private Cluster.
* Für Kunden, die Azure Container Registry für die Arbeit mit privatem AKS aktivieren müssen, muss ein Peering des virtuellen Netzwerks der Container Registry mit dem virtuellen Netzwerk des Agent-Clusters ausgeführt werden.
* Keine aktuelle Unterstützung für Azure Dev Spaces.
* Keine Unterstützung für das Konvertieren vorhandener AKS-Cluster in private Cluster.  
* Das Löschen oder Ändern des privaten Endpunkts im Kundensubnetz führt dazu, dass der Cluster nicht mehr funktioniert. 
* Azure Monitor wird für Livedaten von Containern derzeit nicht unterstützt.
* *Das Verwenden eines eigenen DNS (BYOD)* wird derzeit nicht unterstützt.


<!-- LINKS - internal -->
[az-provider-register]: /cli/azure/provider?view=azure-cli-latest#az-provider-register
[az-feature-list]: /cli/azure/feature?view=azure-cli-latest#az-feature-list
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[private-link-service]: https://docs.microsoft.com/azure/private-link/private-link-service-overview
[virtual-network-peering]: ../virtual-network/virtual-network-peering-overview.md

