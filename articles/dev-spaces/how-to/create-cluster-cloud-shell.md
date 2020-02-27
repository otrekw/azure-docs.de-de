---
title: Erstellen eines Kubernetes-Clusters mit aktiviertem Azure Dev Spaces – Azure Cloud Shell
services: azure-dev-spaces
ms.date: 10/04/2018
ms.topic: conceptual
description: Hier erfahren Sie, wie Sie schnell und ohne Installation zusätzlicher Komponenten direkt über Ihren Browser einen Kubernetes-Cluster mit Aktivierung für Azure Dev Spaces erstellen.
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, Container, Helm, Service Mesh, Service Mesh-Routing, kubectl, k8s
ms.openlocfilehash: 5e2e5cfd22eeedd3554737458caeca0b891b62fe
ms.sourcegitcommit: 0cc25b792ad6ec7a056ac3470f377edad804997a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/25/2020
ms.locfileid: "77605306"
---
# <a name="create-a-kubernetes-cluster-with-azure-dev-spaces-enabled-with-azure-cloud-shell"></a>Erstellen eines Kubernetes-Clusters mit aktiviertem Azure Dev Spaces mit Azure Cloud Shell

Sie können mit [Azure Cloud Shell](/azure/cloud-shell) einen Azure Kubernetes Service-Cluster erstellen. Verwenden Sie dazu die Schaltfläche **Ausprobieren** auf dieser Seite. Wenn Sie nicht angemeldet sind, folgen Sie den Anweisungen zum Anmelden mit einem Azure-Konto, und geben Sie dann die Befehle an der angezeigten Azure Cloud Shell-Eingabeaufforderung ein.

## <a name="create-the-cluster"></a>Erstellen Sie den Cluster.

Erstellen Sie zunächst die Ressourcengruppe in einer [Region, die Azure Dev Spaces unterstützt][supported-regions].

```azurecli-interactive
az group create --name MyResourceGroup --location <region>
```

Erstellen Sie mit dem folgenden Befehl einen Kubernetes-Cluster:

```azurecli-interactive
az aks create -g MyResourceGroup -n MyAKS --location <region> --generate-ssh-keys
```

Die Erstellung des Clusters dauert einige Minuten.  Nach Abschluss des Vorgangs wird die Ausgabe im JSON-Format angezeigt. Suchen Sie nach `provisioningState`, und vergewissern Sie sich, dass `Succeeded` dafür angezeigt wird.

## <a name="next-steps"></a>Nächste Schritte

Links zu vollständigen Tutorials finden Sie unter [Azure Dev Spaces](/azure/dev-spaces/).

> [!IMPORTANT]
> In vielen Azure Dev Spaces-Schnellstartanleitungen und -Tutorials wird die Azure Dev Spaces-Befehlszeilenschnittstelle zum Ausführen von Vorgängen verwendet. Die Azure Dev Spaces-Befehlszeilenschnittstelle kann nicht in Azure Cloud Shell installiert werden.


[supported-regions]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service