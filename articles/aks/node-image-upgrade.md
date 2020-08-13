---
title: Durchführen von Upgrades für AKS-Knotenimages (Azure Kubernetes Service)
description: Erfahren Sie, wie Sie für die Images auf AKS-Clusterknoten und Knotenpools ein Upgrade durchführen.
author: laurenhughes
ms.author: lahugh
ms.service: container-service
ms.topic: conceptual
ms.date: 07/13/2020
ms.openlocfilehash: 040f4378e01c3696b9a74bfcc27230503828f19a
ms.sourcegitcommit: 97a0d868b9d36072ec5e872b3c77fa33b9ce7194
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/04/2020
ms.locfileid: "87562786"
---
# <a name="preview---azure-kubernetes-service-aks-node-image-upgrades"></a>Vorschau – Upgrades für AKS-Knotenimages (Azure Kubernetes Service)

AKS unterstützt Upgrades für Images auf einem Knoten, sodass Sie mit den aktuellsten Betriebssystem- und Laufzeitupdates auf dem neuesten Stand sind. AKS stellt ein neues Image pro Woche mit den neuesten Updates zur Verfügung. Es ist daher von Vorteil, die Images Ihres Knotens regelmäßig auf die neuesten Features, einschließlich Linux- oder Windows-Patches, zu aktualisieren. In diesem Artikel erfahren Sie, wie Sie ein Upgrade für AKS-Clusterknotenimages durchführen und Knotenpoolimages aktualisieren können, ohne ein Upgrade der Version von Kubernetes durchzuführen.

Wenn Sie an Informationen zu den neuesten von AKS bereitgestellten Images interessiert sind, finden Sie weitere Einzelheiten in den [AKS-Versionshinweisen](https://github.com/Azure/AKS/releases).

Informationen zum Upgrade der Kubernetes-Version für Ihren Cluster finden Sie unter [Aktualisieren eines AKS-Clusters][upgrade-cluster].

## <a name="register-the-node-image-upgrade-preview-feature"></a>Registrieren der Previewfunktion für Upgrades von Knotenimages

Um die Previewfunktion für Upgrades von Knotenimages während des Vorschauzeitraums verwenden zu können, müssen Sie die Funktion registrieren.

```azurecli
# Register the preview feature
az feature register --namespace "Microsoft.ContainerService" --name "NodeImageUpgradePreview"
```

Die Registrierung dauert einige Minuten. Verwenden Sie den folgenden Befehl, um zu überprüfen, ob die Funktion registriert ist:

```azurecli
# Verify the feature is registered:
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/NodeImageUpgradePreview')].{Name:name,State:properties.state}"
```

In der Vorschauphase benötigen Sie die CLI-Erweiterung *aks-preview*, um Upgrades für Knotenimages durchführen zu können. Verwenden Sie den Befehl [az extension add][az-extension-add], und suchen Sie dann mit dem Befehl [az extension update][az-extension-update] nach verfügbaren Updates:

```azurecli
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

Wenn der Status als registriert angezeigt wird, können Sie die Registrierung des `Microsoft.ContainerService`-Ressourcenanbieters mit dem Befehl [az provider register](/cli/azure/provider?view=azure-cli-latest#az-provider-register) aktualisieren:

```azurecli
az provider register --namespace Microsoft.ContainerService
```  

## <a name="upgrade-all-nodes-in-all-node-pools"></a>Aktualisieren aller Knoten in allen Knotenpools

Das Upgrade für Knotenimages wird mit `az aks upgrade` durchgeführt. Verwenden Sie folgenden Befehl, um das Upgrade für das Knotenimage durchzuführen:

```azurecli
az aks upgrade \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-image-only
```

Prüfen Sie während des Upgrades den Status der Knotenimages mit dem folgenden `kubectl`-Befehl, um die Bezeichnungen zu erhalten und die aktuellen Knotenimageinformationen herauszufiltern:

```azurecli
kubectl get nodes -o jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.metadata.labels.kubernetes\.azure\.com\/node-image-version}{"\n"}{end}'
```

Wenn das Upgrade beendet ist, verwenden Sie `az aks show`, um die aktualisierten Details zum Knotenpool abzurufen. Das aktuelle Knotenimage wird in der `nodeImageVersion`-Eigenschaft angezeigt.

```azurecli
az aks show \
    --resource-group myResourceGroup \
    --name myAKSCluster
```

## <a name="upgrade-a-specific-node-pool"></a>Aktualisieren eines bestimmten Knotenpools

Das Durchführen eines Upgrades für das Image in einem Knotenpool ähnelt dem Durchführen eines Upgrades für das Image in einem Cluster.

Um das Betriebssystemimage des Knotenpools zu aktualisieren, ohne ein Upgrade des Kubernetes-Clusters durchzuführen, verwenden Sie im folgenden Beispiel die Option `--node-image-only`:

```azurecli
az aks nodepool upgrade \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool \
    --node-image-only
```

Prüfen Sie während des Upgrades den Status der Knotenimages mit dem folgenden `kubectl`-Befehl, um die Bezeichnungen zu erhalten und die aktuellen Knotenimageinformationen herauszufiltern:

```azurecli
kubectl get nodes -o jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.metadata.labels.kubernetes\.azure\.com\/node-image-version}{"\n"}{end}'
```

Wenn das Upgrade beendet ist, verwenden Sie `az aks nodepool show`, um die aktualisierten Details zum Knotenpool abzurufen. Das aktuelle Knotenimage wird in der `nodeImageVersion`-Eigenschaft angezeigt.

```azurecli
az aks nodepool show \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool
```

## <a name="upgrade-node-images-with-node-surge"></a>Aktualisieren von Knotenimages mit Knotenanstiegen

Zur Beschleunigung des Upgradeprozesses für Knotenimages können Sie Ihre Knotenimages mit einem anpassbaren Knotenanstiegswert aktualisieren. Standardmäßig verwendet AKS einen zusätzlichen Knoten, um Upgrades zu konfigurieren.

Wenn Sie die Geschwindigkeit von Upgrades erhöhen möchten, verwenden Sie den Wert `--max-surge`, um die Anzahl der Knoten zu konfigurieren, die für Upgrades verwendet werden sollen, damit diese schneller abgeschlossen werden. Weitere Informationen zu den Kompromissen verschiedener `--max-surge`-Einstellungen finden Sie unter [Anpassen des Upgrades für Knotenanstiege][max-surge].

Der folgende Befehl legt den maximalen Anstiegswert für die Durchführung eines Upgrades des Knotenimages fest:

```azurecli
az aks nodepool upgrade \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool \
    --max-surge 33% \
    --node-image-only \
    --no-wait
```

Prüfen Sie während des Upgrades den Status der Knotenimages mit dem folgenden `kubectl`-Befehl, um die Bezeichnungen zu erhalten und die aktuellen Knotenimageinformationen herauszufiltern:

```azurecli
kubectl get nodes -o jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.metadata.labels.kubernetes\.azure\.com\/node-image-version}{"\n"}{end}'
```

Verwenden Sie `az aks nodepool show`, um die aktualisierten Knotenpooldetails zu erhalten. Das aktuelle Knotenimage wird in der `nodeImageVersion`-Eigenschaft angezeigt.

```azurecli
az aks nodepool show \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool
```

## <a name="next-steps"></a>Nächste Schritte

- Informationen zu den neuesten Knotenimages finden Sie in den [AKS-Versionshinweisen](https://github.com/Azure/AKS/releases).
- Erfahren Sie, wie Sie die Kubernetes-Version durch [Aktualisieren eines AKS-Clusters][upgrade-cluster] aktualisieren können.
- [Anwenden von Sicherheits- und Kernelupdates auf Linux-Knoten in Azure Kubernetes Service (AKS)][security-update]
- Weitere Informationen zu mehreren Knotenpools und zum Durchführen von Upgrades für Knotenpools erhalten Sie unter [Erstellen und Verwalten von mehreren Knotenpools][use-multiple-node-pools].

<!-- LINKS - internal -->
[upgrade-cluster]: upgrade-cluster.md
[security-update]: node-updates-kured.md
[use-multiple-node-pools]: use-multiple-node-pools.md
[max-surge]: upgrade-cluster.md#customize-node-surge-upgrade-preview
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
