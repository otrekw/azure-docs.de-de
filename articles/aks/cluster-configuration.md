---
title: Clusterkonfiguration in Azure Kubernetes Services (AKS)
description: Erfahren Sie, wie Sie in Azure Kubernetes Service (AKS) einen Cluster konfigurieren.
services: container-service
ms.topic: conceptual
ms.date: 03/12/2020
ms.openlocfilehash: fe5ce13d9db8f2bc2231f87de7e602e63d239bfa
ms.sourcegitcommit: 6fd8dbeee587fd7633571dfea46424f3c7e65169
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2020
ms.locfileid: "83725145"
---
# <a name="configure-an-aks-cluster"></a>Konfigurieren eines AKS-Clusters

Im Rahmen der Erstellung eines AKS-Clusters müssen Sie möglicherweise Ihre Clusterkonfiguration an Ihre Anforderungen anpassen. In diesem Artikel werden einige Optionen zur Anpassung Ihres AKS-Clusters vorgestellt.

## <a name="os-configuration-preview"></a>Betriebssystemkonfiguration (Vorschau)

AKS unterstützt jetzt in der Vorschau Ubuntu 18.04 als Knotenbetriebssystem. In der Vorschauphase sind sowohl Ubuntu 16.04 als auch Ubuntu 18.04 verfügbar.

Die folgenden Ressourcen müssen installiert sein:

- Azure CLI ab Version 2.2.0
- Die Erweiterung aks-preview 0.4.35

Verwenden Sie zum Installieren der Erweiterung aks-preview 0.4.35 oder höher die folgenden Azure CLI-Befehle:

```azurecli
az extension add --name aks-preview
az extension list
```

Registrieren Sie das Feature `UseCustomizedUbuntuPreview`:

```azurecli
az feature register --name UseCustomizedUbuntuPreview --namespace Microsoft.ContainerService
```

Es kann einige Minuten dauern, bis der Status als **Registriert** angezeigt wird. Sie können den Registrierungsstatus mithilfe des Befehls [az feature list](https://docs.microsoft.com/cli/azure/feature?view=azure-cli-latest#az-feature-list) überprüfen:

```azurecli
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/UseCustomizedUbuntuPreview')].{Name:name,State:properties.state}"
```

Wenn der Status als registriert angezeigt wird, können Sie die Registrierung des `Microsoft.ContainerService`-Ressourcenanbieters mit dem Befehl [az provider register](https://docs.microsoft.com/cli/azure/provider?view=azure-cli-latest#az-provider-register) aktualisieren:

```azurecli
az provider register --namespace Microsoft.ContainerService
```

### <a name="new-clusters"></a>Neue Cluster

Konfigurieren Sie den Cluster für die Verwendung von Ubuntu 18,04, wenn der Cluster erstellt wird. Verwenden Sie das Flag `--aks-custom-headers`, um Ubuntu 18.04 als Standardbetriebssystem festzulegen.

```azure-cli
az aks create --name myAKSCluster --resource-group myResourceGroup --aks-custom-headers CustomizedUbuntu=aks-ubuntu-1804
```

Wenn Sie einen herkömmlichen Ubuntu 16.04-Cluster erstellen möchten, lassen Sie das benutzerdefinierte Tag `--aks-custom-headers` weg.

### <a name="existing-clusters"></a>Vorhandene Cluster

Konfigurieren Sie einen neuen Knotenpool zur Verwendung von Ubuntu 18.04. Verwenden Sie das Flag `--aks-custom-headers`, um Ubuntu 18.04 als Standardbetriebssystem für diesen Knotenpool festzulegen.

```azure-cli
az aks nodepool add --name ubuntu1804 --cluster-name myAKSCluster --resource-group myResourceGroup --aks-custom-headers CustomizedUbuntu=aks-ubuntu-1804
```

Wenn Sie einen herkömmlichen Ubuntu 16.04-Knotenpool erstellen möchten, lassen Sie das benutzerdefinierte Tag `--aks-custom-headers` weg.


## <a name="custom-resource-group-name"></a>Name der benutzerdefinierten Ressourcengruppe

Wenn Sie in Azure einen Azure Kubernetes Service-Cluster bereitstellen, wird für die Workerknoten eine zweite Ressourcengruppe erstellt. In AKS wird der Knotenressourcengruppe standardmäßig der Name `MC_resourcegroupname_clustername_location` zugewiesen. Sie können jedoch auch einen eigenen Namen angeben.

Installieren Sie die aks-preview-Erweiterungsversion 0.3.2 oder höher der Azure CLI, wenn Sie einen eigenen Ressourcengruppennamen angeben möchten. Verwenden Sie in der Azure CLI den Parameter `--node-resource-group` des Befehls `az aks create`, um einen benutzerdefinierten Namen für die Ressourcengruppe anzugeben. Wenn Sie eine Azure Resource Manager-Vorlage verwenden, um einen AKS-Cluster bereitzustellen, können Sie die `nodeResourceGroup`-Eigenschaft verwenden, um den Namen der Ressourcengruppe zu definieren.

```azurecli
az aks create --name myAKSCluster --resource-group myResourceGroup --node-resource-group myNodeResourceGroup
```

Die sekundäre Ressourcengruppe wird automatisch vom Azure-Ressourcenanbieter in Ihrem eigenen Abonnement erstellt. Sie können einen benutzerdefinierten Namen für die Ressourcengruppe nur dann angeben, wenn der Cluster erstellt wird. 

Denken Sie bei der Arbeit mit der Knotenressourcengruppe daran, dass Folgendes nicht möglich ist:

- Angeben einer vorhandenen Ressourcengruppe als Knotenressourcengruppe
- Angeben eines anderen Abonnements für die Knotenressourcengruppe
- Ändern des Namens der Knotenressourcengruppe, nachdem der Cluster erstellt wurde
- Angeben von Namen für die verwalteten Ressourcen innerhalb der Knotenressourcengruppe
- Ändern oder Löschen von Azure erstellter Tags für verwaltete Ressourcen innerhalb der Knotenressourcengruppe

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie mit `Kured` in Ihrem Cluster [Sicherheits- und Kernelupdates auf Linux-Knoten anwenden können](node-updates-kured.md).
- Unter [Upgrade eines Azure Kubernetes Service-Clusters (AKS)](upgrade-cluster.md) erfahren Sie, wie Sie Ihren Cluster auf die neueste Version von Kubernetes aktualisieren können.
- In der Liste der [Häufig gestellten Fragen zu AKS](faq.md) finden Sie Antworten auf einige häufig gestellte Fragen zu AKS.