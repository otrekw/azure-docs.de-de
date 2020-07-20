---
title: Verwenden von Näherungsplatzierungsgruppen zur Verringerung der Wartezeit für AKS-Cluster (Azure Kubernetes Service)
description: Hier erfahren Sie, wie Sie mithilfe von Näherungsplatzierungsgruppen die Wartezeit für Ihre AKS-Clusterworkloads verringern.
services: container-service
manager: gwallace
ms.topic: article
ms.date: 06/22/2020
ms.openlocfilehash: 1bcdfb4bb3c910feeac0521308e1e7d733fbd959
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2020
ms.locfileid: "86244071"
---
# <a name="reduce-latency-with-proximity-placement-groups-preview"></a>Verringern der Wartezeit mithilfe von Näherungsplatzierungsgruppen (Vorschauversion)

> [!Note]
> Wenn Sie Näherungsplatzierungsgruppen mit AKS verwenden, betrifft die Colocation nur die Agent-Knoten. Die Wartezeit zwischen Knoten und die entsprechende Wartezeit zwischen gehosteten Pods werden verbessert. Die Colocation hat keine Auswirkung auf die Platzierung der Steuerungsebene eines Clusters.

Wenn Sie Ihre Anwendung in Azure bereitstellen, führt die Verteilung von VM-Instanzen (Virtual Machine, virtueller Computer) auf Regionen oder Verfügbarkeitszonen zu Netzwerkwartezeit, die die Gesamtleistung Ihrer Anwendung beeinträchtigen kann. Eine Näherungsplatzierungsgruppe ist eine logische Gruppierung, die dazu dient, eine geringe physische Entfernung zwischen Azure-Computeressourcen sicherzustellen. Einige Anwendungsbereiche wie Spiele, technische Simulationen und Hochfrequenzhandel (High-Frequency Trading, HFT) erfordern geringe Wartezeiten und eine schnelle Aufgabenausführung. In solchen HPC-Szenarien (High Performance Computing) empfiehlt sich ggf. der Einsatz von [Näherungsplatzierungsgruppen](../virtual-machines/linux/co-location.md#proximity-placement-groups) für die Knotenpools Ihres Clusters.

## <a name="limitations"></a>Einschränkungen

* Die Näherungsplatzierungsgruppe umfasst eine einzelne Verfügbarkeitszone.
* AKS-Cluster, die VM-Verfügbarkeitsgruppen nutzen, werden aktuell nicht unterstützt.
* Bereits vorhandene Knotenpools können nicht für die Verwendung einer Näherungsplatzierungsgruppe geändert werden.

> [!IMPORTANT]
> AKS-Previewfunktionen stehen gemäß dem Self-Service- und Aktivierungsprinzip zur Verfügung. Vorschauversionen werden „wie besehen“ und „wie verfügbar“ bereitgestellt und sind von den Vereinbarungen zum Service Level und der eingeschränkten Garantie ausgeschlossen. AKS-Vorschauversionen werden teilweise vom Kundensupport auf Grundlage der bestmöglichen Leistung abgedeckt. Daher sind diese Funktionen nicht für die Verwendung in der Produktion vorgesehen. Weitere Informationen finden Sie in den folgenden Supportartikeln:
>
> - [Unterstützungsrichtlinien für Azure Kubernetes Service](support-policies.md)
> - [Häufig gestellte Fragen zum Azure-Support](faq.md)

## <a name="before-you-begin"></a>Voraussetzungen

Die folgenden Ressourcen müssen installiert sein:

- Die Erweiterung „aks-preview 0.4.53“

### <a name="set-up-the-preview-feature-for-proximity-placement-groups"></a>Einrichten der Previewfunktion für Näherungsplatzierungsgruppen

> [!IMPORTANT]
> Wenn Sie Näherungsplatzierungsgruppen mit AKS verwenden, betrifft die Colocation nur die Agent-Knoten. Die Wartezeit zwischen Knoten und die entsprechende Wartezeit zwischen gehosteten Pods werden verbessert. Die Colocation hat keine Auswirkung auf die Platzierung der Steuerungsebene eines Clusters.

```azurecli-interactive
# register the preview feature
az feature register --namespace "Microsoft.ContainerService" --name "ProximityPlacementGroupPreview"
```

Die Registrierung dauert ggf. einige Minuten. Verwenden Sie den folgenden Befehl, um zu überprüfen, ob die Funktion registriert ist:

```azurecli-interactive
# Verify the feature is registered:
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/ProximityPlacementGroupPreview')].{Name:name,State:properties.state}"
```

In der Vorschauphase benötigen Sie die CLI-Erweiterung *aks-preview*, um Näherungsplatzierungsgruppen verwenden zu können. Verwenden Sie den Befehl [az extension add][az-extension-add], und suchen Sie dann mit dem Befehl [az extension update][az-extension-update] nach verfügbaren Updates:

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```
## <a name="node-pools-and-proximity-placement-groups"></a>Knotenpools und Näherungsplatzierungsgruppen

Die erste Ressource, die Sie mit einer Näherungsplatzierungsgruppe bereitstellen, wird einem bestimmten Rechenzentrum zugeordnet. Weitere Ressourcen, die mit der gleichen Näherungsplatzierungsgruppe bereitgestellt werden, werden im gleichen Rechenzentrum platziert. Wenn alle Ressourcen, von denen die Näherungsplatzierungsgruppe verwendet wird, beendet (Zuordnung aufgehoben) oder gelöscht wurden, ist sie nicht mehr zugeordnet.

* Viele Knotenpools können einer einzelnen Näherungsplatzierungsgruppe zugeordnet werden.
* Ein Knotenpool darf immer nur einer einzelnen Näherungsplatzierungsgruppe zugeordnet werden.

## <a name="create-a-new-aks-cluster-with-a-proximity-placement-group"></a>Erstellen eines neuen AKS-Clusters mit einer Näherungsplatzierungsgruppe

Im folgenden Beispiel wird der Befehl [az group create][az-group-create] verwendet, um eine Ressourcengruppe namens *myResourceGroup* in der Region *centralus* zu erstellen. Anschließend wird mit dem Befehl [az aks create][az-aks-create] ein AKS-Cluster mit dem Namen *myAKSCluster* erstellt. 

Beschleunigter Netzwerkbetrieb trägt erheblich zur Verbesserung der Netzwerkleistung virtueller Computer bei. Es empfiehlt sich daher, Näherungsplatzierungsgruppen zusammen mit beschleunigtem Netzwerkbetrieb zu verwenden. Beschleunigter Netzwerkbetrieb wird von AKS standardmäßig in [unterstützten VM-Instanzen](../virtual-network/create-vm-accelerated-networking-cli.md?toc=/azure/virtual-machines/linux/toc.json#limitations-and-constraints) verwendet. Hierzu zählen die meisten virtuellen Azure-Computer mit mindestens zwei vCPUs.

Erstellen Sie einen neuen AKS-Cluster mit einer Näherungsplatzierungsgruppe:

```azurecli-interactive
# Create an Azure resource group
az group create --name myResourceGroup --location centralus
```
Führen Sie den folgenden Befehl aus, und speichern Sie die zurückgegebene ID:

```azurecli-interactive
# Create proximity placement group
az ppg create -n myPPG -g myResourceGroup -l centralus -t standard
```

Die Ausgabe des Befehls enthält den für anstehende CLI-Befehle erforderlichen *ID-Wert*:

```output
{
  "availabilitySets": null,
  "colocationStatus": null,
  "id": "/subscriptions/yourSubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.Compute/proximityPlacementGroups/myPPG",
  "location": "centralus",
  "name": "myPPG",
  "proximityPlacementGroupType": "Standard",
  "resourceGroup": "myResourceGroup",
  "tags": {},
  "type": "Microsoft.Compute/proximityPlacementGroups",
  "virtualMachineScaleSets": null,
  "virtualMachines": null
}
```

Verwenden Sie im folgenden Befehl die Ressourcen-ID der Näherungsplatzierungsgruppe für den Wert *myPPGResourceID*:

```azurecli-interactive
# Create an AKS cluster that uses a proximity placement group for the initial node pool
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --ppg myPPGResourceID
```

## <a name="add-a-proximity-placement-group-to-an-existing-cluster"></a>Hinzufügen einer Näherungsplatzierungsgruppe in einen vorhandenen Cluster

Sie können einem vorhandenen Cluster eine Näherungsplatzierungsgruppe hinzufügen, indem Sie einen neuen Knotenpool erstellen. Anschließend können Sie vorhandene Workloads optional zum neuen Knotenpool migrieren und den ursprünglichen Knotenpool löschen.

Verwenden Sie die gleiche Näherungsplatzierungsgruppe, die Sie zuvor erstellt haben, um sicherzustellen, dass sich Agent-Knoten in beiden Knotenpools in Ihrem AKS-Cluster physisch im gleichen Rechenzentrum befinden.

Verwenden Sie die Ressourcen-ID aus der zuvor erstellten Näherungsplatzierungsgruppe, und fügen Sie mithilfe des Befehls [`az aks nodepool add`][az-aks-nodepool-add] einen neuen Knotenpool hinzu:

```azurecli-interactive
# Add a new node pool that uses a proximity placement group, use a --node-count = 1 for testing
az aks nodepool add \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool \
    --node-count 1 \
    --ppg myPPGResourceID
```

## <a name="clean-up"></a>Bereinigung

Wenn Sie den Cluster löschen möchten, löschen Sie die AKS-Ressourcengruppe mithilfe des Befehls [`az group delete`][az-group-delete]:

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu Näherungsplatzierungsgruppen finden Sie [hier][proximity-placement-groups].

<!-- LINKS - Internal -->
[azure-ad-rbac]: azure-ad-rbac.md
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-get-upgrades]: /cli/azure/aks#az-aks-get-upgrades
[az-aks-upgrade]: /cli/azure/aks#az-aks-upgrade
[az-aks-show]: /cli/azure/aks#az-aks-show
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[proximity-placement-groups]: ../virtual-machines/linux/co-location.md#proximity-placement-groups
[az-aks-create]: /cli/azure/aks#az-aks-create
[system-pool]: ./use-system-pools.md
[az-aks-nodepool-add]: /cli/azure/aks/nodepool?view=azure-cli-latest#az-aks-nodepool-add
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-group-create]: /cli/azure/group#az-group-create
[az-group-delete]: /cli/azure/group#az-group-delete
