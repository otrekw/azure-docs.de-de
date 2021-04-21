---
title: Skalieren eines Azure Kubernetes Service-Clusters (AKS)
description: Erfahren Sie, wie Sie die Anzahl der Knoten in einem Azure Kubernetes Service-Cluster (AKS) skalieren.
services: container-service
ms.topic: article
ms.date: 09/16/2020
ms.openlocfilehash: 1468f9a0a23935022ed14488dfb65d789828d310
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/20/2021
ms.locfileid: "107782885"
---
# <a name="scale-the-node-count-in-an-azure-kubernetes-service-aks-cluster"></a>Skalieren der Anzahl der Knoten in einem Azure Kubernetes Service-Cluster (AKS)

Wenn sich die Ressourcenanforderungen Ihrer Anwendungen ändern, können Sie einen AKS-Cluster manuell zur Ausführung einer anderen Anzahl von Knoten skalieren. Beim zentralen Herunterskalieren werden die Knoten sorgfältig [gesperrt und ausgeglichen][kubernetes-drain], um die Unterbrechung ausgeführter Anwendungen zu minimieren. Beim Hochskalieren wartet AKS, bis die Knoten vom Kubernetes-Cluster als `Ready` markiert wurden, bevor Pods für diese Knoten geplant werden.

## <a name="scale-the-cluster-nodes"></a>Skalieren der Clusterknoten

Rufen Sie zuerst mithilfe des Befehls [az aks show][az-aks-show] den *Namen* Ihres Knotenpools ab. Das folgende Beispiel ruft den Knotenpoolnamen für den Cluster namens *myAKSCluster* in der Ressourcengruppe *myResourceGroup* ab:

```azurecli-interactive
az aks show --resource-group myResourceGroup --name myAKSCluster --query agentPoolProfiles
```

Die folgende Beispielausgabe zeigt, dass der *Name* *nodepool1* lautet:

```output
[
  {
    "count": 1,
    "maxPods": 110,
    "name": "nodepool1",
    "osDiskSizeGb": 30,
    "osType": "Linux",
    "storageProfile": "ManagedDisks",
    "vmSize": "Standard_DS2_v2"
  }
]
```

Verwenden Sie den Befehl [az aks scale][az-aks-scale], um die Clusterknoten zu skalieren. Das folgende Beispiel skaliert einen Cluster namens *myAKSCluster* auf einen einzelnen Knoten. Stellen Sie Ihren eigenen `--nodepool-name` aus dem vorherigen Befehl bereit, z.B. *nodepool1*:

```azurecli-interactive
az aks scale --resource-group myResourceGroup --name myAKSCluster --node-count 1 --nodepool-name <your node pool name>
```

Die folgende Beispielausgabe zeigt, dass der Cluster erfolgreich auf einen einzigen Knoten skaliert wurde, wie im Abschnitt *agentPoolProfiles* gezeigt:

```json
{
  "aadProfile": null,
  "addonProfiles": null,
  "agentPoolProfiles": [
    {
      "count": 1,
      "maxPods": 110,
      "name": "nodepool1",
      "osDiskSizeGb": 30,
      "osType": "Linux",
      "storageProfile": "ManagedDisks",
      "vmSize": "Standard_DS2_v2",
      "vnetSubnetId": null
    }
  ],
  [...]
}
```


## <a name="scale-user-node-pools-to-0"></a>Skalieren von `User`-Knotenpools auf 0 (null)

Im Unterschied zu `System`-Knotenpools, die immer ausgeführte Knoten erfordern, können Sie `User`-Knotenpools auf 0 (null) skalieren. Weitere Informationen zu den Unterschieden zwischen System- und Benutzerknotenpools finden Sie unter [System- und Benutzerknotenpools](use-system-pools.md).

Um einen Benutzerpool auf 0 zu skalieren, können Sie den Befehl [az aks nodepool scale][az-aks-nodepool-scale] als Alternative zum obigen Befehl `az aks scale` verwenden und als Knotenanzahl 0 (null) festlegen.


```azurecli-interactive
az aks nodepool scale --name <your node pool name> --cluster-name myAKSCluster --resource-group myResourceGroup  --node-count 0 
```

Sie können für `User`-Knotenpools auch eine Autoskalierung auf 0 Knoten durchführen, indem Sie den Parameter `--min-count` der [automatischen Clusterskalierung](cluster-autoscaler.md) auf 0 (null) festlegen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie einen AKS-Cluster manuell skaliert, um die Anzahl von Knoten zu erhöhen oder zu verringern. Sie können auch die [Autoskalierung für Cluster][cluster-autoscaler] verwenden, um den Cluster automatisch zu skalieren.

<!-- LINKS - external -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/

<!-- LINKS - internal -->
[aks-tutorial]: ./tutorial-kubernetes-prepare-app.md
[az-aks-show]: /cli/azure/aks#az_aks_show
[az-aks-scale]: /cli/azure/aks#az_aks_scale
[cluster-autoscaler]: cluster-autoscaler.md
[az-aks-nodepool-scale]: /cli/azure/aks/nodepool#az_aks_nodepool_scale