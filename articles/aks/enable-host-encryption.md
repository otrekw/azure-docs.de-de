---
title: Aktivieren der hostbasierten Verschlüsselung in Azure Kubernetes Service (AKS)
description: Erfahren Sie, wie Sie eine hostbasierte Verschlüsselung in einem Azure Kubernetes Service-Cluster (AKS) konfigurieren.
services: container-service
ms.topic: article
ms.date: 03/03/2021
ms.custom: devx-track-azurepowershell, devx-track-azurecli
ms.openlocfilehash: 3d5009c164ab09d3977bb15d85b166a31c1f1a0b
ms.sourcegitcommit: 1b19b8d303b3abe4d4d08bfde0fee441159771e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/11/2021
ms.locfileid: "109754315"
---
# <a name="host-based-encryption-on-azure-kubernetes-service-aks"></a>Hostbasierte Verschlüsselung in Azure Kubernetes Service (AKS)

Mit hostbasierter Verschlüsselung werden die auf dem VM-Host der VMs Ihres AKS-Agent-Knotens gespeicherten Daten ruhend verschlüsselt und verschlüsselt an den Speicherdienst übermittelt. Dies bedeutet, dass temporäre Datenträger im Ruhezustand mit von der Plattform verwalteten Schlüsseln verschlüsselt werden. Der Cache von Betriebssystem- und sonstigen Datenträgern wird im Ruhezustand entweder mit von der Plattform verwalteten Schlüsseln oder vom Kunden verwalteten Schlüsseln verschlüsselt, je nachdem, welcher Verschlüsselungstyp auf diesen Datenträgern festgelegt wurde. Standardmäßig werden bei der Verwendung von AKS Betriebssystem- und sonstige Datenträger im Ruhezustand mit von der Plattform verwalteten Schlüsseln verschlüsselt. Dies bedeutet, dass die Caches für diese Datenträger standardmäßig auch im Ruhezustand mit von der Plattform verwalteten Schlüsseln verschlüsselt werden.  Sie können Ihre eigenen verwalteten Schlüssel wie in [Bring Your Own Key (BYOK) mit Azure-Datenträgern in Azure Kubernetes Service (AKS)](azure-disk-customer-managed-keys.md) beschrieben angeben. Der Cache für diese Datenträger wird dann auch mit dem Schlüssel verschlüsselt, den Sie in diesem Schritt angeben.


## <a name="before-you-begin"></a>Voraussetzungen

Diese Funktion kann nur bei der Erstellung des Clusters oder bei der Erstellung eines Knotenpools festgelegt werden.

> [!NOTE]
> Die hostbasierte Verschlüsselung ist in [Azure-Regionen][supported-regions] verfügbar, die die serverseitige Verschlüsselung von verwalteten Azure-Datenträgern unterstützen, und nur mit bestimmten [unterstützten VM-Größen][supported-sizes].

### <a name="prerequisites"></a>Voraussetzungen

- Azure CLI, Version 2.23.0 oder höher

### <a name="limitations"></a>Einschränkungen

- Kann nur auf neuen Knoten aktiviert werden.
- Kann nur in [Azure-Regionen][supported-regions] aktiviert werden, die die serverseitige Verschlüsselung von verwalteten Azure-Datenträgern unterstützen, und nur mit bestimmten [unterstützten VM-Größen][supported-sizes].
- Erfordert einen AKS-Cluster und einen Knotenpool, der auf VM-Skalierungsgruppen (Virtual Machine Scale Sets, VMSS) als *VM set type* basiert.

## <a name="use-host-based-encryption-on-new-clusters"></a>Verwenden der hostbasierten Verschlüsselung in neuen Clustern

Konfigurieren Sie die Cluster-Agent-Knoten für die Verwendung der hostbasierten Verschlüsselung, wenn der Cluster erstellt wird. 

```azurecli-interactive
az aks create --name myAKSCluster --resource-group myResourceGroup -s Standard_DS2_v2 -l westus2 --enable-encryption-at-host
```

Wenn Sie Cluster ohne hostbasierte Verschlüsselung erstellen möchten, lassen Sie den Parameter `--enable-encryption-at-host` weg.

## <a name="use-host-based-encryption-on-existing-clusters"></a>Verwenden der hostbasierten Verschlüsselung auf vorhandenen Clustern

Sie können die hostbasierte Verschlüsselung auf vorhandenen Clustern aktivieren, indem Sie dem Cluster einen neuen Knotenpool hinzufügen. Konfigurieren Sie einen neuen Knotenpool für die Verwendung der hostbasierten Verschlüsselung mit dem Parameter `--enable-encryption-at-host`.

```azurecli
az aks nodepool add --name hostencrypt --cluster-name myAKSCluster --resource-group myResourceGroup -s Standard_DS2_v2 -l westus2 --enable-encryption-at-host
```

Wenn Sie neue Knotenpools ohne das Feature der hostbasierten Verschlüsselung erstellen möchten, lassen Sie den Parameter `--enable-encryption-at-host` weg.

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über [Best Practices für Clustersicherheit und Upgrades in Azure Kubernetes Service (AKS)][best-practices-security] und erfahren Sie mehr über [Serverseitige Verschlüsselung von verwalteten Azure-Datenträgern](../virtual-machines/disk-encryption.md#encryption-at-host---end-to-end-encryption-for-your-vm-data).


<!-- LINKS - external -->

<!-- LINKS - internal -->
[az-extension-add]: /cli/azure/extension#az_extension_add
[az-extension-update]: /cli/azure/extension#az_extension_update
[best-practices-security]: ./operator-best-practices-cluster-security.md
[supported-regions]: ../virtual-machines/disk-encryption.md#supported-regions
[supported-sizes]: ../virtual-machines/disk-encryption.md#supported-vm-sizes
[azure-cli-install]: /cli/azure/install-azure-cli
[az-feature-register]: /cli/azure/feature#az_feature_register
[az-feature-list]: /cli/azure/feature#az_feature_list
[az-provider-register]: /cli/azure/provider#az_provider_register
