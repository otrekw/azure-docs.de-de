---
title: Skalieren eines Azure Kubernetes Service-Clusters (AKS)
description: Erfahren Sie, wie Sie die Anzahl der Knoten in einem Azure Kubernetes Service-Cluster (AKS) skalieren.
services: container-service
author: iainfoulds
ms.topic: article
ms.date: 05/31/2019
ms.author: iainfou
ms.openlocfilehash: 2071ba93e826fabc4778426e2b4404b7475d1bd2
ms.sourcegitcommit: 99ac4a0150898ce9d3c6905cbd8b3a5537dd097e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/25/2020
ms.locfileid: "77594396"
---
# <a name="scale-the-node-count-in-an-azure-kubernetes-service-aks-cluster"></a>Skalieren der Anzahl der Knoten in einem Azure Kubernetes Service-Cluster (AKS)

Wenn sich die Ressourcenanforderungen Ihrer Anwendungen ändern, können Sie einen AKS-Cluster manuell zur Ausführung einer anderen Anzahl von Knoten skalieren. Beim zentralen Herunterskalieren werden die Knoten sorgfältig [gesperrt und ausgeglichen][kubernetes-drain], um die Unterbrechung ausgeführter Anwendungen zu minimieren. Beim zentralen Hochskalieren wartet AKS, bis die Knoten vom Kubernetes-Cluster als `Ready` markiert wurden, bevor Pods für diese Knoten geplant werden.

## <a name="scale-the-cluster-nodes"></a>Skalieren der Clusterknoten

Rufen Sie zuerst mithilfe des Befehls [az aks show][az-aks-show] den *Namen* Ihres Knotenpools ab. Das folgende Beispiel ruft den Knotenpoolnamen für den Cluster namens *myAKSCluster* in der Ressourcengruppe *myResourceGroup* ab:

```azurecli-interactive
az aks show --resource-group myResourceGroup --name myAKSCluster --query agentPoolProfiles
```

Die folgende Beispielausgabe zeigt, dass der *Name* *nodepool1* lautet:

```console
$ az aks show --resource-group myResourceGroup --name myAKSCluster --query agentPoolProfiles

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

Verwenden Sie den Befehl [az aks scale][az-aks-scale], um die Clusterknoten zu skalieren. Das folgende Beispiel skaliert einen Cluster namens *myAKSCluster* auf einen einzelnen Knoten. Stellen Sie Ihren eigenen *--nodepool-name* aus dem vorherigen Befehl bereit, z.B. *nodepool1*:

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

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie einen AKS-Cluster manuell skaliert, um die Anzahl von Knoten zu erhöhen oder zu verringern. Sie können auch die [Autoskalierung für Cluster][cluster-autoscaler] verwenden, um den Cluster automatisch zu skalieren.

<!-- LINKS - external -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/

<!-- LINKS - internal -->
[aks-tutorial]: ./tutorial-kubernetes-prepare-app.md
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-aks-scale]: /cli/azure/aks#az-aks-scale
[cluster-autoscaler]: cluster-autoscaler.md
