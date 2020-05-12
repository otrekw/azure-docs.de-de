---
title: Verwenden von Systemknotenpools in Azure Kubernetes Service (AKS)
description: Informationen zum Erstellen und Verwalten von Systemknotenpools in Azure Kubernetes Service (AKS)
services: container-service
ms.topic: article
ms.date: 04/28/2020
ms.openlocfilehash: 04322bdaa2e0e72c5fbdbadb07f2608ee360e1e3
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/05/2020
ms.locfileid: "82790557"
---
# <a name="manage-system-node-pools-in-azure-kubernetes-service-aks"></a>Verwalten von Systemknotenpools in Azure Kubernetes Service (AKS)

Im Azure Kubernetes Service (AKS) werden Knoten derselben Konfiguration zu *Knotenpools* zusammengefasst. Knotenpools enthalten die zugrunde liegenden virtuellen Computer, auf denen Ihre Anwendungen ausgeführt werden. Systemknotenpools und Benutzerknotenpools sind zwei verschiedene Knotenpoolmodi für AKS-Cluster. Systemknotenpools dienen dem primären Zweck, kritische Systempods wie CoreDNS und tunnelfront zu hosten. Benutzerknotenpools dienen dem primären Zweck, Ihre Anwendungspods zu hosten. Anwendungspods lassen sich jedoch auf Systemknotenpools planen, wenn Sie nur einen Pool in Ihrem AKS-Cluster haben möchten. Jeder AKS-Cluster muss mindestens einen Systemknotenpool mit mindestens einem Knoten enthalten. 

> [!Important]
> Wenn Sie für Ihren AKS-Cluster nur einen Systemknotenpool in einer Produktionsumgebung ausführen, sollten Sie für den Knotenpool mindestens drei Knoten verwenden.

## <a name="before-you-begin"></a>Voraussetzungen

* Azure CLI-Version 2.3.1 oder höher muss installiert und konfiguriert sein. Führen Sie `az --version` aus, um die Version zu ermitteln. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sie bei Bedarf unter [Installieren der Azure CLI][install-azure-cli].

## <a name="limitations"></a>Einschränkungen

Die folgenden Einschränkungen gelten für die Erstellung und Verwaltung von AKS-Clustern, die Systemknotenpools unterstützen.

* Siehe [Kontingente, Größeneinschränkungen für virtuelle Computer und regionale Verfügbarkeit in Azure Kubernetes Service (AKS)][quotas-skus-regions].
* Der AKS-Cluster muss mit VM-Skalierungsgruppen als VM-Typ erstellt werden.
* Der Name eines Knotenpools darf nur Kleinbuchstaben und Ziffern enthalten und muss mit einem Kleinbuchstaben beginnen. Bei Linux-Knotenpools muss die Länge zwischen 1 und 12 Zeichen betragen. Bei Windows-Knotenpools muss die Länge zwischen 1 und 6 Zeichen betragen.
* Zum Festlegen des Modus für einen Knotenpool muss eine API der Version 2020-03-01 oder höher verwendet werden.
* Der Modus eines Knotenpools ist eine erforderliche Eigenschaft und muss explizit festgelegt werden, wenn ARM-Vorlagen oder direkte API-Aufrufe verwendet werden.

## <a name="system-and-user-node-pools"></a>System- und Benutzerknotenpools

Systemknotenpoolknoten weisen jeweils die Bezeichnung **kubernetes.azure.com/mode: system** auf. Jeder AKS-Cluster enthält mindestens einen Systemknotenpool. Für Systemknotenpools gelten folgende Einschränkungen:

* Bei Systempools muss als „osType“ Linux angegeben werden.
* Bei Benutzerknotenpools kann als „osType“ Linux oder Windows angegeben werden.
* Systempools müssen mindestens einen Knoten enthalten, während Benutzerknotenpools keine oder mehrere Knoten enthalten können.
* Für Systemknotenpools ist eine VM SKU mit mindestens zwei vCPUs und 4 GB Arbeitsspeicher erforderlich.
* Systemknotenpools müssen gemäß der [Formel für die mindestens erforderliche und maximal zulässige Anzahl von Pods][maximum-pods] mindestens 30 Pods unterstützen.
* Für Spot-Knotenpools sind Benutzerknotenpools erforderlich.

Mit Knotenpools sind folgende Vorgänge möglich:

* Sie können aus einem Systemknotenpool einen Benutzerknotenpool machen, sofern im AKS-Cluster ein anderer Systemknotenpool als Ersatz vorhanden ist.
* Sie können aus einem Benutzerknotenpool einen Systemknotenpool machen.
* Sie können Benutzerknotenpools löschen.
* Systemknotenpools können Sie löschen, wenn im AKS-Cluster ein anderer Systemknotenpool als Ersatz vorhanden ist.
* Ein AKS-Cluster kann mehrere Systemknotenpools enthalten, benötigt jedoch mindestens einen Systemknotenpool.

## <a name="create-a-new-aks-cluster-with-a-system-node-pool"></a>Erstellen eines neuen AKS-Clusters mit einem Systemknotenpool

Beim Erstellen eines neuen AKS-Clusters wird automatisch ein Systemknotenpool mit einem Knoten erstellt. Für den anfänglichen Knotenpool ist standardmäßig ein Modus vom Typ „system“ festgelegt. Neue Knotenpools, die mit „az aks nodepool add“ erstellt werden, sind Benutzerknotenpools, außer der Modusparameter wird explizit angegeben.

Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen *myResourceGroup* in der Region *eastus* erstellt.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Erstellen Sie mithilfe des Befehls [az aks create][az-aks-create] einen AKS-Cluster. Im folgenden Beispiel wird ein Cluster mit dem Namen *myAKSCluster* mit einem Systempool erstellt, der einen Knoten enthält. Verwenden Sie in Ihren Produktionsworkloads Systemknotenpools mit mindestens drei Knoten. Dieser Vorgang kann mehrere Minuten dauern.

```azurecli-interactive
az aks create -g myResourceGroup --name myAKSCluster --node-count 1 --generate-ssh-keys
```

## <a name="add-a-system-node-pool-to-an-existing-aks-cluster"></a>Hinzufügen eines Systemknotenpools zu einem vorhandenen AKS-Cluster

Vorhandenen AKS-Clustern kann mindestens ein Systemknotenpool hinzugefügt werden. Mit dem folgenden Befehl wird ein Knotenpool mit dem Modustyp „system“ und der standardmäßigen Anzahl von drei Knoten hinzugefügt.

```azurecli-interactive
az aks nodepool add -g myResourceGroup --cluster-name myAKSCluster -n mynodepool --mode system
```
## <a name="show-details-for-your-node-pool"></a>Anzeigen von Details zum Knotenpool

Mit dem folgenden Befehl können Sie die Details zu Ihrem Knotenpool anzeigen.  

```azurecli-interactive
az aks nodepool show -g myResourceGroup --cluster-name myAKSCluster -n mynodepool
```

Für Systemknotenpools wurde ein Modus vom Typ **system** definiert, für Benutzerknotenpools ein Modus vom Typ **user**.

```output
{
  "agentPoolType": "VirtualMachineScaleSets",
  "availabilityZones": null,
  "count": 3,
  "enableAutoScaling": null,
  "enableNodePublicIp": false,
  "id": "/subscriptions/666d66d8-1e43-4136-be25-f25bb5de5883/resourcegroups/myResourceGroup/providers/Microsoft.ContainerService/managedClusters/myAKSCluster/agentPools/mynodepool",
  "maxCount": null,
  "maxPods": 110,
  "minCount": null,
  "mode": "System",
  "name": "mynodepool",
  "nodeLabels": {},
  "nodeTaints": null,
  "orchestratorVersion": "1.15.10",
  "osDiskSizeGb": 100,
  "osType": "Linux",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "scaleSetEvictionPolicy": null,
  "scaleSetPriority": null,
  "spotMaxPrice": null,
  "tags": null,
  "type": "Microsoft.ContainerService/managedClusters/agentPools",
  "vmSize": "Standard_DS2_v2",
  "vnetSubnetId": null
}
```

## <a name="update-system-and-user-node-pools"></a>Aktualisieren von System- und Benutzerknotenpools

Der Modus für System- und Benutzerknotenpools kann geändert werden. Sie können aus einem Systemknotenpool nur dann einen Benutzerpool machen, wenn im AKS-Cluster bereits ein anderer Systemknotenpool vorhanden ist.

Mit diesem Befehl wird aus einem Systemknotenpool ein Benutzerknotenpool.

```azurecli-interactive
az aks nodepool update -g myResourceGroup --cluster-name myAKSCluster -n mynodepool --mode user
```

Mit diesem Befehl wird aus einem Benutzerknotenpool ein Systemknotenpool.

```azurecli-interactive
az aks nodepool update -g myResourceGroup --cluster-name myAKSCluster -n mynodepool --mode system
```

## <a name="delete-a-system-node-pool"></a>Löschen eines Systemknotenpools

> [!Note]
> Wenn Sie in AKS-Clustern älter als API-Version 2020-03-02 Systemknotenpools verwenden möchten, fügen Sie einen neuen Systemknotenpool hinzu, und löschen Sie anschließend den ursprünglichen Standardknotenpool.

Bisher konnte der Systemknotenpool, der der ursprüngliche Standardknotenpool in einem AKS-Cluster war, nicht gelöscht werden. Ab sofort können Sie jeden Knotenpool in Ihren Clustern flexibel löschen. Da in AKS-Clustern mindestens ein Systemknotenpool vorhanden sein muss, müssen im AKS-Cluster mindestens zwei Systemknotenpools vorhanden sein, damit Sie einen davon löschen können.

```azurecli-interactive
az aks nodepool delete -g myResourceGroup --cluster-name myAKSCluster -n mynodepool
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie Sie Systemknotenpools in einem AKS-Cluster erstellen und verwalten. Weitere Informationen zum Verwenden mehrerer Knotenpools finden Sie unter [Verwenden mehrerer Knotenpools][use-multiple-node-pools].

<!-- EXTERNAL LINKS -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-taint]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#taint
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe
[kubernetes-labels]: https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/
[kubernetes-label-syntax]: https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/#syntax-and-character-set

<!-- INTERNAL LINKS -->
[aks-windows]: windows-container-cli.md
[az-aks-get-credentials]: /cli/azure/aks#az-aks-get-credentials
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-aks-nodepool-add]: /cli/azure/aks/nodepool?view=azure-cli-latest#az-aks-nodepool-add
[az-aks-nodepool-list]: /cli/azure/aks/nodepool?view=azure-cli-latest#az-aks-nodepool-list
[az-aks-nodepool-update]: /cli/azure/aks/nodepool?view=azure-cli-latest#az-aks-nodepool-update
[az-aks-nodepool-upgrade]: /cli/azure/aks/nodepool?view=azure-cli-latest#az-aks-nodepool-upgrade
[az-aks-nodepool-scale]: /cli/azure/aks/nodepool?view=azure-cli-latest#az-aks-nodepool-scale
[az-aks-nodepool-delete]: /cli/azure/aks/nodepool?view=azure-cli-latest#az-aks-nodepool-delete
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[az-group-create]: /cli/azure/group#az-group-create
[az-group-delete]: /cli/azure/group#az-group-delete
[az-group-deployment-create]: /cli/azure/group/deployment#az-group-deployment-create
[gpu-cluster]: gpu-cluster.md
[install-azure-cli]: /cli/azure/install-azure-cli
[operator-best-practices-advanced-scheduler]: operator-best-practices-advanced-scheduler.md
[quotas-skus-regions]: quotas-skus-regions.md
[supported-versions]: supported-kubernetes-versions.md
[tag-limitation]: ../azure-resource-manager/resource-group-using-tags.md
[taints-tolerations]: operator-best-practices-advanced-scheduler.md#provide-dedicated-nodes-using-taints-and-tolerations
[vm-sizes]: ../virtual-machines/linux/sizes.md
[use-multiple-node-pools]: use-multiple-node-pools.md
[maximum-pods]: configure-azure-cni.md#maximum-pods-per-node