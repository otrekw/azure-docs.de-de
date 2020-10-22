---
title: Starten und Beenden einer AKS-Instanz (Azure Kubernetes Service)
description: Hier erfahren Sie, wie Sie eine AKS-Instanz (Azure Kubernetes Service) starten und beenden.
services: container-service
ms.topic: article
ms.date: 09/24/2020
author: palma21
ms.openlocfilehash: bc756994cf0f6e12af1c1ad5a6c8db304b4253e3
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2020
ms.locfileid: "91968782"
---
# <a name="stop-and-start-an-azure-kubernetes-service-aks-cluster-preview"></a>Beenden und Starten eines AKS-Clusters (Azure Kubernetes Service) (Vorschau)

Ihre AKS-Workloads müssen möglicherweise nicht kontinuierlich ausgeführt werden – beispielsweise bei einem Entwicklungscluster, der nur während der Geschäftszeiten verwendet wird. In diesem Fall können Zeiten auftreten, in denen sich Ihr AKS-Cluster (Azure Kubernetes Service) im Leerlauf befindet und nur die Systemkomponenten ausgeführt werden. Dann können Sie den Speicherbedarf des Clusters reduzieren, indem Sie [alle `User`-Knotenpools auf 0 skalieren](scale-cluster.md#scale-user-node-pools-to-0). Der [`System`-Pool](use-system-pools.md) wird jedoch weiterhin für die Ausführung der Systemkomponenten benötigt, solange der Cluster ausgeführt wird. Um Ihre Kosten während dieser Zeiträume weiter zu optimieren, können Sie Ihren Cluster vollständig ausschalten (beenden). Dadurch werden Ihre Steuerungsebene und Agentknoten vollständig angehalten, sodass Sie bei allen Computekosten sparen können. Gleichzeitig werden alle Objekte und der Clusterstatus gespeichert und so lange beibehalten, bis Sie sie wieder starten. So können Sie nach einem Wochenende genau dort weitermachen, wo Sie aufgehört haben. Sie haben auch die Möglichkeit, Ihren Cluster immer nur dann auszuführen, wenn Sie Ihre Batchaufträge ausführen.

[!INCLUDE [preview features callout](./includes/preview/preview-callout.md)]

## <a name="before-you-begin"></a>Voraussetzungen

Es wird vorausgesetzt, dass Sie über ein AKS-Cluster verfügen. Wenn Sie einen AKS-Cluster benötigen, erhalten Sie weitere Informationen im AKS-Schnellstart. Verwenden Sie dafür entweder die [Azure CLI][aks-quickstart-cli] oder das [Azure-Portal][aks-quickstart-portal].


### <a name="limitations"></a>Einschränkungen

Wenn Sie das Feature zum Starten/Beenden von Clustern verwenden, gelten die folgenden Einschränkungen:

- Diese Funktion wird nur für Back-End-Cluster für Virtual Machine Scale Sets unterstützt.
- In der Vorschauversion wird dieses Feature für private Cluster nicht unterstützt.
- Der Clusterstatus eines beendeten AKS-Clusters wird bis zu 12 Monate beibehalten. Wenn Ihr Cluster länger als 12 Monate angehalten wird, kann der Clusterstatus danach nicht mehr wiederhergestellt werden. Weitere Informationen finden Sie unter [Unterstützungsrichtlinien für Azure Kubernetes Service](support-policies.md).
- In der Vorschauversion müssen Sie die automatische Clusterskalierung beenden, bevor Sie den Cluster anhalten.
- Nun ein angehaltener AKS-Cluster kann gestartet oder gelöscht werden. Wenn Sie einen Vorgang wie eine Skalierung oder ein Upgrade ausführen möchten, müssen Sie zuerst den Cluster starten.

### <a name="install-the-aks-preview-azure-cli"></a>Installieren der Azure-Befehlszeilenschnittstelle `aks-preview` 

Sie benötigen außerdem die Version 0.4.64 oder höher der Erweiterung für die Azure-Befehlszeilenschnittstelle *aks-preview*. Installieren Sie die Erweiterung *aks-preview* der Azure-Befehlszeilenschnittstelle mithilfe des Befehls [az extension add][az-extension-add]. Alternativ können Sie verfügbare Updates mithilfe des Befehls [az extension update][az-extension-update] installieren.

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
``` 

### <a name="register-the-startstoppreview-preview-feature"></a>Registrieren der Previewfunktion `StartStopPreview`

Wenn Sie das Feature zum Starten/Beenden des Clusters verwenden möchten, müssen Sie das `StartStopPreview`-Featureflag für Ihr Abonnement aktivieren.

Registrieren Sie das `StartStopPreview`-Featureflag mit dem Befehl [az feature register][az-feature-register], wie im folgenden Beispiel gezeigt:

```azurecli-interactive
az feature register --namespace "Microsoft.ContainerService" --name "StartStopPreview"
```

Es dauert einige Minuten, bis der Status *Registered (Registriert)* angezeigt wird. Überprüfen Sie den Registrierungsstatus mithilfe des Befehls [az feature list][az-feature-list]:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/StartStopPreview')].{Name:name,State:properties.state}"
```

Wenn der Vorgang abgeschlossen ist, können Sie die Registrierung des *Microsoft.ContainerService*-Ressourcenanbieters mit dem Befehl [az provider register][az-provider-register] aktualisieren:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

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


## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Skalieren von `User`-Pools auf 0 finden Sie unter [Skalieren von `User`-Pools auf 0](scale-cluster.md#scale-user-node-pools-to-0).
- Informationen zum Einsparen von Kosten mithilfe von Spot-Instanzen finden Sie unter [Hinzufügen eines Spot-Knotenpools zu AKS](spot-node-pool.md).
- Weitere Informationen zu den Unterstützungsrichtlinien für AKS finden Sie unter [Unterstützungsrichtlinien für AKS](support-policies.md).

<!-- LINKS - external -->

<!-- LINKS - internal -->
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli&preserve-view=true
[az-extension-add]: /cli/azure/extension?view=azure-cli-latest#az-extension-add&preserve-view=true
[az-extension-update]: /cli/azure/extension?view=azure-cli-latest#az-extension-update&preserve-view=true
[az-feature-register]: /cli/azure/feature?view=azure-cli-latest#az-feature-register&preserve-view=true
[az-feature-list]: /cli/azure/feature?view=azure-cli-latest#az-feature-list&preserve-view=true
[az-provider-register]: /cli/azure/provider?view=azure-cli-latest#az-provider-register&preserve-view=true
[az-aks-show]: /cli/azure/aks?view=azure-cli-latest#az_aks_show
