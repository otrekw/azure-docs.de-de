---
title: Tutorial zu Kubernetes in Azure – Upgrade eines Clusters
description: In diesem Azure Kubernetes Service-Tutorial (AKS) erfahren Sie, wie Sie ein Upgrade eines vorhandenen AKS-Clusters auf die neueste verfügbare Kubernetes-Version durchführen.
services: container-service
ms.topic: tutorial
ms.date: 05/24/2021
ms.custom: mvc, devx-track-azurepowershell
ms.openlocfilehash: 4751081d628f3ea1bff411b5e391377e8b771939
ms.sourcegitcommit: 20acb9ad4700559ca0d98c7c622770a0499dd7ba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2021
ms.locfileid: "110697746"
---
# <a name="tutorial-upgrade-kubernetes-in-azure-kubernetes-service-aks"></a>Tutorial: Durchführen eines Upgrades für Kubernetes in Azure Kubernetes Service (AKS)

Im Rahmen des Anwendungs- und Clusterlebenszyklus möchten Sie möglicherweise ein Upgrade auf die neueste verfügbare Version von Kubernetes durchführen und neue Funktionen nutzen. Ein Upgrade für den Azure Kubernetes Service-Cluster (AKS) kann über die Azure CLI durchgeführt werden.

In diesem Tutorial (Teil 7 von 7) wird ein Upgrade für einen Kubernetes-Cluster durchgeführt. Folgendes wird vermittelt:

> [!div class="checklist"]
> * Identifizieren der aktuellen und verfügbaren Kubernetes-Versionen
> * Aktualisieren der Kubernetes-Knoten
> * Überprüfen des erfolgreichen Upgrades

## <a name="before-you-begin"></a>Voraussetzungen

In den vorherigen Tutorials wurde eine Anwendung als Containerimage verpackt. Dieses Image wurde in Azure Container Registry hochgeladen, und Sie haben einen AKS-Cluster erstellt. Die Anwendung wurde dann für den AKS-Cluster bereitgestellt. Falls Sie diese Schritte nicht ausgeführt haben und dies jetzt nachholen möchten, sollten Sie mit [Tutorial 1 – Erstellen von Containerimages][aks-tutorial-prepare-app] beginnen.

### <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Für dieses Tutorial müssen Sie mindestens Version 2.0.53 der Azure CLI ausführen. Führen Sie `az --version` aus, um die Version zu ermitteln. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sie bei Bedarf unter [Installieren der Azure CLI][azure-cli-install].

### <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

Für dieses Tutorial müssen Sie mindestens die Azure PowerShell-Version 5.9.0 ausführen. Führen Sie `Get-InstalledModule -Name Az` aus, um die Version zu ermitteln. Falls Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure PowerShell][azure-powershell-install] weitere Informationen.

---

## <a name="get-available-cluster-versions"></a>Abrufen verfügbarer Clusterversionen

### <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Verwenden Sie vor dem Upgrade eines Clusters den Befehl [az aks get-upgrades][], um zu überprüfen, welche Kubernetes-Versionen als Upgrade verfügbar sind:

```azurecli
az aks get-upgrades --resource-group myResourceGroup --name myAKSCluster
```

Im folgenden Beispiel lautet die aktuelle Version *1.18.10*, und die verfügbaren Versionen werden unter *Upgrades* angezeigt.

```json
{
  "agentPoolProfiles": null,
  "controlPlaneProfile": {
    "kubernetesVersion": "1.18.10",
    ...
    "upgrades": [
      {
        "isPreview": null,
        "kubernetesVersion": "1.19.1"
      },
      {
        "isPreview": null,
        "kubernetesVersion": "1.19.3"
      }
    ]
  },
  ...
}
```

### <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

Bevor Sie einen Cluster upgraden, verwenden Sie das Cmdlet [Get-AzAksCluster][get-azakscluster], um zu bestimmen, welche Kubernetes-Version Sie ausführen und in welcher Region sie sich befindet:

```azurepowershell
Get-AzAksCluster -ResourceGroupName myResourceGroup -Name myAKSCluster |
  Select-Object -Property Name, KubernetesVersion, Location
```

Im folgenden Beispiel lautet die aktuelle Version *1.19.9*:

```output
Name         KubernetesVersion Location
----         ----------------- --------
myAKSCluster 1.19.9            eastus
```

Verwenden Sie das Cmdlet [Get-AzAksVersion][get-azaksversion], um zu bestimmen, welche Upgradeversionen für Kubernetes in der Region verfügbar sind, in der sich der AKS-Cluster befindet:

```azurepowershell
Get-AzAksVersion -Location eastus | Where-Object OrchestratorVersion -gt 1.19.9
```

Die verfügbaren Versionen werden unter *OrchestratorVersion* angezeigt.

```output
OrchestratorType    : Kubernetes
OrchestratorVersion : 1.20.2
DefaultProperty     :
IsPreview           :
Upgrades            : {Microsoft.Azure.Commands.Aks.Models.PSOrchestratorProfile}

OrchestratorType    : Kubernetes
OrchestratorVersion : 1.20.5
DefaultProperty     :
IsPreview           :
Upgrades            : {}
```

---

## <a name="upgrade-a-cluster"></a>Durchführen eines Clusterupgrades

AKS-Knoten werden sorgfältig isoliert und ausgeglichen, um ausgeführte Anwendungen möglichst wenig zu beeinträchtigen. Bei diesem Prozess werden die folgenden Schritte ausgeführt:

1. Der Kubernetes-Planer verhindert, dass weitere Pods für einen Knoten geplant werden, für den ein Upgrade durchgeführt werden soll.
1. Ausgeführte Pods auf dem Knoten werden auf anderen Knoten im Cluster eingeplant.
1. Es wird ein Knoten erstellt, auf dem die aktuellen Kubernetes-Komponenten ausgeführt werden.
1. Wenn der neue Knoten bereit ist und in den Cluster eingebunden wurde, beginnt der Kubernetes-Planer damit, Pods darauf auszuführen.
1. Der alte Knoten wird gelöscht, und der nächste Knoten im Cluster beginnt mit dem Prozess des Absperrens und Ausgleichens.

### <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Verwenden Sie den Befehl [az aks upgrade][], um ein Upgrade des AKS-Clusters durchzuführen.

```azurecli
az aks upgrade \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --kubernetes-version KUBERNETES_VERSION
```

> [!NOTE]
> Upgrades sind jeweils nur auf die nächste Nebenversion möglich. Beispielsweise können Sie ein Upgrade von *1.14.x* auf *1.15.x* durchführen, aber nicht direkt von *1.14.x* auf *1.16.x*. Für ein Upgrade von *1.14.x* auf *1.16.x* müssen Sie zuerst ein Upgrade von *1.14.x* auf *1.15.x* und dann ein weiteres Upgrade von  *1.15.x* auf *1.16.x* durchführen.

In der folgenden verkürzten Beispielausgabe wird das Ergebnis des Upgrades auf *1.19.1* gezeigt. Beachten Sie, dass *kubernetesVersion* jetzt *1.19.1* lautet:

```json
{
  "agentPoolProfiles": [
    {
      "count": 3,
      "maxPods": 110,
      "name": "nodepool1",
      "osType": "Linux",
      "storageProfile": "ManagedDisks",
      "vmSize": "Standard_DS1_v2",
    }
  ],
  "dnsPrefix": "myAKSClust-myResourceGroup-19da35",
  "enableRbac": false,
  "fqdn": "myaksclust-myresourcegroup-19da35-bd54a4be.hcp.eastus.azmk8s.io",
  "id": "/subscriptions/<Subscription ID>/resourcegroups/myResourceGroup/providers/Microsoft.ContainerService/managedClusters/myAKSCluster",
  "kubernetesVersion": "1.19.1",
  "location": "eastus",
  "name": "myAKSCluster",
  "type": "Microsoft.ContainerService/ManagedClusters"
}
```

### <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

Verwenden Sie das Cmdlet [Set-AzAksCluster][set-azakscluster], um ein Upgrade des AKS-Clusters durchzuführen.

```azurepowershell
Set-AzAksCluster -ResourceGroupName myResourceGroup -Name myAKSCluster -KubernetesVersion <KUBERNETES_VERSION>
```

> [!NOTE]
> Upgrades sind jeweils nur auf die nächste Nebenversion möglich. Beispielsweise können Sie ein Upgrade von *1.14.x* auf *1.15.x* durchführen, aber nicht direkt von *1.14.x* auf *1.16.x*. Für ein Upgrade von *1.14.x* auf *1.16.x* müssen Sie zuerst ein Upgrade von *1.14.x* auf *1.15.x* und dann ein weiteres Upgrade von  *1.15.x* auf *1.16.x* durchführen.

In der folgenden verkürzten Beispielausgabe wird das Ergebnis des Upgrades auf *1.19.9* gezeigt. Beachten Sie, dass *kubernetesVersion* jetzt *1.20.2* lautet:

```output
ProvisioningState       : Succeeded
MaxAgentPools           : 100
KubernetesVersion       : 1.20.2
PrivateFQDN             :
AgentPoolProfiles       : {default}
Name                    : myAKSCluster
Type                    : Microsoft.ContainerService/ManagedClusters
Location                : eastus
Tags                    : {}
```

---

## <a name="validate-an-upgrade"></a>Überprüfen eines Upgrades

### <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Überprüfen mit dem Befehl [az aks show][] folgendermaßen, ob das Upgrade erfolgreich durchgeführt wurde:

```azurecli
az aks show --resource-group myResourceGroup --name myAKSCluster --output table
```

Die folgende Beispielausgabe zeigt, dass der AKS-Cluster *KubernetesVersion 1.19.1* ausführt:

```output
Name          Location    ResourceGroup    KubernetesVersion    ProvisioningState    Fqdn
------------  ----------  ---------------  -------------------  -------------------  ----------------------------------------------------------------
myAKSCluster  eastus      myResourceGroup  1.19.1               Succeeded            myaksclust-myresourcegroup-19da35-bd54a4be.hcp.eastus.azmk8s.io
```

### <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

Überprüfen Sie, ob das Upgrade erfolgreich durchgeführt wurde, indem Sie das Cmdlet [Get-AzAksCluster][get-azakscluster] folgendermaßen verwenden:

```azurepowershell
Get-AzAksCluster -ResourceGroupName myResourceGroup -Name myAKSCluster |
  Select-Object -Property Name, Location, KubernetesVersion, ProvisioningState
```

Die folgende Beispielausgabe zeigt, dass der AKS-Cluster *KubernetesVersion 1.20.2* ausführt:

```output
Name         Location KubernetesVersion ProvisioningState
----         -------- ----------------- -----------------
myAKSCluster eastus   1.20.2            Succeeded
```

---

## <a name="delete-the-cluster"></a>Löschen des Clusters

### <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Da dieses Tutorial der letzte Teil der Reihe ist, ist es ratsam, den AKS-Cluster zu löschen. Da die Kubernetes-Knoten auf virtuellen Azure-Computern (VMs) ausgeführt werden, fallen dafür auch dann weiter Gebühren an, wenn Sie den Cluster nicht nutzen. Führen Sie den Befehl [az group delete][az-group-delete] aus, um die Ressourcengruppe, den Containerdienst und alle dazugehörigen Ressourcen zu entfernen.

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```
### <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

Da dieses Tutorial der letzte Teil der Reihe ist, ist es ratsam, den AKS-Cluster zu löschen. Da die Kubernetes-Knoten auf virtuellen Azure-Computern (VMs) ausgeführt werden, fallen dafür auch dann weiter Gebühren an, wenn Sie den Cluster nicht nutzen. Entfernen Sie die Ressourcengruppe, den Containerdienst und alle zugehörigen Ressourcen mit dem Cmdlet [Remove-AzResourceGroup][remove-azresourcegroup].

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup
```

---

> [!NOTE]
> Wenn Sie den Cluster löschen, wird der vom AKS-Cluster verwendete Azure Active Directory-Dienstprinzipal nicht entfernt. Schritte zum Entfernen des Dienstprinzipals finden Sie unter den [Überlegungen zum AKS-Dienstprinzipal und dessen Löschung][sp-delete]. Wenn Sie eine verwaltete Identität verwendet haben, wird die Identität von der Plattform verwaltet, und Sie müssen keine Geheimnisse bereitstellen oder rotieren.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Kubernetes in einem AKS-Cluster aktualisiert. Sie haben Folgendes gelernt:

> [!div class="checklist"]
> * Identifizieren der aktuellen und verfügbaren Kubernetes-Versionen
> * Aktualisieren der Kubernetes-Knoten
> * Überprüfen des erfolgreichen Upgrades

Weitere Informationen zu AKS finden Sie unter [Übersicht über AKS][aks-intro]. Anleitungen zum Erstellen vollständiger Lösungen mit AKS finden Sie unter [AKS-Lösungsleitfaden][aks-solution-guidance].

<!-- LINKS - external -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/

<!-- LINKS - internal -->
[aks-intro]: ./intro-kubernetes.md
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[az aks show]: /cli/azure/aks#az_aks_show
[az aks get-upgrades]: /cli/azure/aks#az_aks_get_upgrades
[az aks upgrade]: /cli/azure/aks#az_aks_upgrade
[azure-cli-install]: /cli/azure/install-azure-cli
[az-group-delete]: /cli/azure/group#az_group_delete
[sp-delete]: kubernetes-service-principal.md#additional-considerations
[aks-solution-guidance]: /azure/architecture/reference-architectures/containers/aks-start-here?WT.mc_id=AKSDOCSPAGE
[azure-powershell-install]: /powershell/azure/install-az-ps
[get-azakscluster]: /powershell/module/az.aks/get-azakscluster
[get-azaksversion]: /powershell/module/az.aks/get-azaksversion
[set-azakscluster]: /powershell/module/az.aks/set-azakscluster
[remove-azresourcegroup]: /powershell/module/az.resources/remove-azresourcegroup
