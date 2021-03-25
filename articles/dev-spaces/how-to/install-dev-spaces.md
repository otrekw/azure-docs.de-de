---
title: Installieren von Azure Dev Spaces unter AKS und Installieren der clientseitigen Tools
services: azure-dev-spaces
ms.date: 07/24/2019
ms.topic: conceptual
description: Erfahren Sie, wie Sie Azure Dev Spaces in einem AKS-Cluster aktivieren und die clientseitigen Tools installieren.
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, Container, Helm, Service Mesh, Service Mesh-Routing, kubectl, k8s
ms.openlocfilehash: 177496a53d204306b2b655b8736ce063dedf0f61
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102202245"
---
# <a name="enable-azure-dev-spaces-on-an-aks-cluster-and-install-the-client-side-tools"></a>Aktivieren von Azure Dev Spaces in einem AKS-Cluster und Installieren der clientseitigen Tools

[!INCLUDE [Azure Dev Spaces deprecation](../../../includes/dev-spaces-deprecation.md)]

In diesem Artikel werden verschiedene Möglichkeiten zum Aktivieren von Azure Dev Spaces in einem AKS-Cluster sowie zum Installieren der clientseitigen Tools beschrieben.

## <a name="enable-azure-dev-spaces-using-the-cli"></a>Aktivieren von Azure Dev Spaces mit der CLI

Bevor Sie Dev Spaces mit der CLI aktivieren können, benötigen Sie Folgendes:
* Ein Azure-Abonnement. Falls Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Konto][az-portal-create-account] erstellen.
* [Eine Installation der Azure CLI][install-cli].
* [Einen AKS-Cluster][create-aks-cli] in einer [unterstützten Region][supported-regions].

Verwenden Sie den Befehl `use-dev-spaces`, um Dev Spaces in Ihrem AKS-Cluster zu aktivieren, und befolgen Sie die angezeigten Anweisungen.

```azurecli
az aks use-dev-spaces -g myResourceGroup -n myAKSCluster
```

Mit dem obigen Befehl wird Dev Spaces im Cluster *myAKSCluster* in der Gruppe *myResourceGroup* aktiviert und der Entwicklerbereich *default* erstellt.

```console
'An Azure Dev Spaces Controller' will be created that targets resource 'myAKSCluster' in resource group 'myResourceGroup'. Continue? (y/N): y

Creating and selecting Azure Dev Spaces Controller 'myAKSCluster' in resource group 'myResourceGroup' that targets resource 'myAKSCluster' in resource group 'myResourceGroup'...2m 24s

Select a dev space or Kubernetes namespace to use as a dev space.
 [1] default
Type a number or a new name: 1

Kubernetes namespace 'default' will be configured as a dev space. This will enable Azure Dev Spaces instrumentation for new workloads in the namespace. Continue? (Y/n): Y

Configuring and selecting dev space 'default'...3s

Managed Kubernetes cluster 'myAKSCluster' in resource group 'myResourceGroup' is ready for development in dev space 'default'. Type `azds prep` to prepare a source directory for use with Azure Dev Spaces and `azds up` to run.
```

Mit dem Befehl `use-dev-spaces` wird auch die Azure Dev Spaces CLI installiert.

## <a name="install-the-client-side-tools"></a>Installieren der clientseitigen Tools

Sie können die clientseitigen Azure Dev Spaces-Tools verwenden, um mit Dev Spaces über einen AKS-Cluster auf Ihrem lokalen Computer zu interagieren. Es gibt mehrere Möglichkeiten, die clientseitigen Tools zu installieren:

* Installieren Sie in [Visual Studio Code][vscode] die [Azure Dev Spaces-Erweiterung][vscode-extension].
* Installieren Sie in [Visual Studio 2019][visual-studio] die Workload „Azure-Entwicklung“.
* Laden Sie die [Windows][cli-win]-, [Mac][cli-mac]- oder [Linux][cli-linux]-CLI herunter, und installieren Sie sie.

## <a name="remove-azure-dev-spaces-using-the-cli"></a>Entfernen von Azure Dev Spaces mithilfe der CLI

Verwenden Sie den Befehl `azds remove`, um Azure Dev Spaces aus dem AKS-Cluster zu entfernen.

```azurecli
azds remove -g MyResourceGroup -n MyAKS
```

Die folgende Beispielausgabe zeigt das Entfernen von Azure Dev Spaces aus dem Cluster *MyAKS*.

```azurecli
$ azds remove -g MyResourceGroup -n MyAKS
Azure Dev Spaces Controller 'MyAKS' in resource group 'MyResourceGroup' that targets resource 'MyAKS' in resource group 'MyResourceGroup' will be deleted. This will remove Azure Dev Spaces instrumentation from the target resource for new workloads. Continue? (y/N): y

Deleting Azure Dev Spaces Controller 'MyAKS' in resource group 'MyResourceGroup' that targets resource 'MyAks' in resource group 'MyResourceGroup' (takes a few minutes)...
```

Alle Namespaces, die Sie mit Azure Dev Spaces erstellt haben, bleiben zusammen mit ihren Workloads erhalten, neue Workloads in diesen Namespaces werden jedoch nicht mit Azure Dev Spaces instrumentiert. Wenn Sie darüber hinaus vorhandene Pods neu starten, die mit Azure Dev Spaces instrumentiert werden, werden möglicherweise Fehler angezeigt. Diese Pods müssen ohne Azure Dev Spaces-Tools neu bereitgestellt werden. Wenn Sie Azure Dev Spaces vollständig aus Ihrem Cluster entfernen möchten, löschen Sie alle Pods in sämtlichen Namespaces, in denen Azure Dev Spaces aktiviert war.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Funktionsweise von Azure Dev Spaces:

> [!div class="nextstepaction"]
> [Funktionsweise von Azure Dev Spaces](../how-dev-spaces-works.md)

[create-aks-cli]: ../../aks/kubernetes-walkthrough.md#create-a-resource-group
[install-cli]: /cli/azure/install-azure-cli
[supported-regions]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service
[az-portal]: https://portal.azure.com
[az-portal-create-account]: https://azure.microsoft.com/free
[cli-linux]: https://aka.ms/get-azds-linux
[cli-mac]: https://aka.ms/get-azds-mac
[cli-win]: https://aka.ms/get-azds-windows
[visual-studio]: https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs
[visual-studio-k8s-tools]: https://aka.ms/get-vsk8stools
[vscode]: https://code.visualstudio.com/download
[vscode-extension]: https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds
