---
title: Starten und Beenden einer AKS-Instanz (Azure Kubernetes Service)
description: Hier erfahren Sie, wie Sie eine AKS-Instanz (Azure Kubernetes Service) starten und beenden.
services: container-service
ms.topic: article
ms.date: 09/24/2020
author: palma21
ms.openlocfilehash: 734986d2c9b372214a54c1308e4ca445940c5f65
ms.sourcegitcommit: a434cfeee5f4ed01d6df897d01e569e213ad1e6f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111808923"
---
# <a name="stop-and-start-an-azure-kubernetes-service-aks-cluster"></a>Beenden und Starten eines AKS-Clusters (Azure Kubernetes Service)

Ihre AKS-Workloads müssen möglicherweise nicht kontinuierlich ausgeführt werden – beispielsweise bei einem Entwicklungscluster, der nur während der Geschäftszeiten verwendet wird. In diesem Fall können Zeiten auftreten, in denen sich Ihr AKS-Cluster (Azure Kubernetes Service) im Leerlauf befindet und nur die Systemkomponenten ausgeführt werden. Dann können Sie den Speicherbedarf des Clusters reduzieren, indem Sie [alle `User`-Knotenpools auf 0 skalieren](scale-cluster.md#scale-user-node-pools-to-0). Der [`System`-Pool](use-system-pools.md) wird jedoch weiterhin für die Ausführung der Systemkomponenten benötigt, solange der Cluster ausgeführt wird. Um Ihre Kosten während dieser Zeiträume weiter zu optimieren, können Sie Ihren Cluster vollständig ausschalten (beenden). Dadurch werden Ihre Steuerungsebene und Agentknoten vollständig angehalten, sodass Sie bei allen Computekosten sparen können. Gleichzeitig werden alle Objekte und der Clusterstatus gespeichert und so lange beibehalten, bis Sie sie wieder starten. So können Sie nach einem Wochenende genau dort weitermachen, wo Sie aufgehört haben. Sie haben auch die Möglichkeit, Ihren Cluster immer nur dann auszuführen, wenn Sie Ihre Batchaufträge ausführen.

## <a name="before-you-begin"></a>Voraussetzungen

Es wird vorausgesetzt, dass Sie über ein AKS-Cluster verfügen. Wenn Sie einen AKS-Cluster benötigen, erhalten Sie weitere Informationen im AKS-Schnellstart. Verwenden Sie dafür entweder die [Azure CLI][aks-quickstart-cli] oder das [Azure-Portal][aks-quickstart-portal].

### <a name="limitations"></a>Einschränkungen

Wenn Sie das Feature zum Starten/Beenden von Clustern verwenden, gelten die folgenden Einschränkungen:

- Diese Funktion wird nur für Back-End-Cluster für Virtual Machine Scale Sets unterstützt.
- Der Clusterstatus eines beendeten AKS-Clusters wird bis zu 12 Monate beibehalten. Wenn Ihr Cluster länger als 12 Monate angehalten wird, kann der Clusterstatus danach nicht mehr wiederhergestellt werden. Weitere Informationen finden Sie unter [Unterstützungsrichtlinien für Azure Kubernetes Service](support-policies.md).
- Nun ein angehaltener AKS-Cluster kann gestartet oder gelöscht werden. Wenn Sie einen Vorgang wie eine Skalierung oder ein Upgrade ausführen möchten, müssen Sie zuerst den Cluster starten.
- Die vom Kunden bereitgestellten PrivateEndpoints, die mit dem privaten Cluster verknüpft sind, müssen gelöscht und erneut erstellt werden, wenn Sie einen beendeten AKS-Cluster starten.

## <a name="stop-an-aks-cluster"></a>Beenden eines AKS-Clusters

Mit dem Befehl `az aks stop` können Sie die Knoten und die Steuerungsebene eines AKS-Clusters beenden, der gerade ausgeführt wird. Im folgenden Beispiel wird ein Cluster mit dem Namen *myAKSCluster* beendet.

```azurecli-interactive
az aks stop --name myAKSCluster --resource-group myResourceGroup
```

Mit dem Befehl [az aks show][az-aks-show] können Sie überprüfen, wann Ihr Cluster beendet wurde, und sicherstellen, dass für `powerState` wie in der folgenden Ausgabe `Stopped` angezeigt wird:

```json
{
[...]
  "nodeResourceGroup": "MC_myResourceGroup_myAKSCluster_westus2",
  "powerState":{
    "code":"Stopped"
  },
  "privateFqdn": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
[...]
}
```

Wenn für `provisioningState` `Stopping` angezeigt wird, bedeutet das, dass Ihr Cluster noch nicht vollständig beendet wurde.

> [!IMPORTANT]
> Wenn Sie [Budgets für die Unterbrechung von Pods](https://kubernetes.io/docs/concepts/workloads/pods/disruptions/) verwenden, kann der Beendigungsvorgang länger dauern, da der Ausgleichsprozess mehr Zeit in Anspruch nimmt.

## <a name="start-an-aks-cluster"></a>Starten eines AKS-Clusters

Mit dem Befehl `az aks start` können Sie die Knoten und die Steuerungsebene eines beendeten AKS-Clusters starten. Der Cluster wird mit dem vorherigen Status der Steuerungsebene und der vorherigen Anzahl von Agentknoten neu gestartet.  
Im folgenden Beispiel wird ein Cluster mit dem Namen *myAKSCluster* gestartet.

```azurecli-interactive
az aks start --name myAKSCluster --resource-group myResourceGroup
```

Mit dem Befehl [az aks show][az-aks-show] können Sie überprüfen, wann Ihr Cluster gestartet wurde, und sicherstellen, dass für `powerState` wie in der folgenden Ausgabe `Running` angezeigt wird:

```json
{
[...]
  "nodeResourceGroup": "MC_myResourceGroup_myAKSCluster_westus2",
  "powerState":{
    "code":"Running"
  },
  "privateFqdn": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
[...]
}
```

Wenn für `provisioningState` `Starting` angezeigt wird, bedeutet das, dass Ihr Cluster noch nicht vollständig gestartet wurde.

> [!NOTE]
> Wenn Sie automatische Clusterskalierung verwenden, liegt Ihre aktuelle Knotenanzahl beim Starten des Clusters möglicherweise nicht zwischen den von Ihnen festgelegten Mindest- und Höchstwerten für den Bereich. Dies ist das erwartete Verhalten. Der Cluster beginnt mit der Anzahl von Knoten, die er zum Ausführen seiner Workloads benötigt. Dies hat keine Auswirkungen auf Ihre Einstellungen für automatische Skalierung. Wenn Ihr Cluster Skalierungsvorgänge ausführt, wirken sich die Mindest- und Höchstwerte auf Ihre aktuelle Knotenanzahl aus. Der Cluster wird schließlich in den gewünschten Bereich gelangen und dort verbleiben, bis Sie ihn beenden.

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Skalieren von `User`-Pools auf 0 finden Sie unter [Skalieren von `User`-Pools auf 0](scale-cluster.md#scale-user-node-pools-to-0).
- Informationen zum Einsparen von Kosten mithilfe von Spot-Instanzen finden Sie unter [Hinzufügen eines Spot-Knotenpools zu AKS](spot-node-pool.md).
- Weitere Informationen zu den Unterstützungsrichtlinien für AKS finden Sie unter [Unterstützungsrichtlinien für AKS](support-policies.md).

<!-- LINKS - external -->

<!-- LINKS - internal -->
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[az-extension-add]: /cli/azure/extension#az_extension_add
[az-extension-update]: /cli/azure/extension#az_extension_update
[az-feature-register]: /cli/azure/feature#az_feature_register
[az-feature-list]: /cli/azure/feature#az_feature_list
[az-provider-register]: /cli/azure/provider#az_provider_register
[az-aks-show]: /cli/azure/aks#az_aks_show
