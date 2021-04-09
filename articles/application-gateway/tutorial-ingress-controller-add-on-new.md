---
title: 'Tutorial: Aktivieren des Eingangsdatencontroller-Add-Ons für einen neuen AKS-Cluster mit einer neuen Azure Application Gateway-Instanz'
description: In diesem Tutorial erfahren Sie, wie Sie das Eingangsdatencontroller-Add-On für Ihren neuen AKS-Cluster mit einer neuen Application Gateway-Instanz aktivieren.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: tutorial
ms.date: 03/02/2021
ms.author: caya
ms.openlocfilehash: c37168c5165f5402dd4f57c8557bc2b7b3603533
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "101720187"
---
# <a name="tutorial-enable-the-ingress-controller-add-on-for-a-new-aks-cluster-with-a-new-application-gateway-instance"></a>Tutorial: Aktivieren des Eingangsdatencontroller-Add-Ons für einen neuen AKS-Cluster mit einer neuen Application Gateway-Instanz

Mit der Azure CLI können Sie das Add-On [Application Gateway-Eingangsdatencontroller (Application Gateway Ingress Controller, AGIC)](ingress-controller-overview.md) für einen neuen Cluster für [Azure Kubernetes Services (AKS)](https://azure.microsoft.com/services/kubernetes-service/) aktivieren.

In diesem Tutorial erstellen Sie einen AKS-Cluster mit aktiviertem AGIC-Add-On. Beim Erstellen des Clusters wird automatisch eine Azure Application Gateway-Instanz erstellt, die verwendet werden soll. Anschließend stellen Sie eine Beispielanwendung bereit, die das Add-On verwendet, um die Anwendung über Application Gateway verfügbar zu machen. 

Das Add-On ermöglicht eine viel schnellere Bereitstellung von AGIC für Ihren AKS-Cluster als das [bisherige Verfahren mit Helm](ingress-controller-overview.md#difference-between-helm-deployment-and-aks-add-on). Es bietet außerdem eine umfassende Verwaltungserfahrung.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen Sie eine Ressourcengruppe. 
> * Erstellen eines neuen AKS-Clusters mit aktiviertem AGIC-Add-On.
> * Bereitstellen einer Beispielanwendung mithilfe von AGIC für eingehenden Datenverkehr auf dem AKS-Cluster.
> * Überprüfen der Erreichbarkeit der Anwendung über Application Gateway.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../includes/azure-cli-prepare-your-environment.md)]

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

In Azure ordnen Sie verwandte Ressourcen einer Ressourcengruppe zu. Erstellen Sie mit [az group create](/cli/azure/group#az-group-create) eine Ressourcengruppe. Im folgenden Beispiel wird eine Ressourcengruppe namens *myResourceGroup* im Standort (der Region) *canadacentral* erstellt: 

```azurecli-interactive
az group create --name myResourceGroup --location canadacentral
```

## <a name="deploy-an-aks-cluster-with-the-add-on-enabled"></a>Bereitstellen eines AKS-Clusters mit aktiviertem Add-On

Sie stellen nun einen neuen AKS-Clusters mit aktiviertem AGIC-Add-On bereit. Wenn Sie keine vorhandene Application Gateway-Instanz bereitstellen, die in diesem Prozess verwendet werden soll, erstellen wir automatisch eine neue Application Gateway-Instanz ein und richten diese ein, um Datenverkehr an den AKS-Cluster zu verarbeiten.  

> [!NOTE]
> Das AGIC-Add-On (Application Gateway Ingress Controller) unterstützt *nur* Application Gateway v2-SKUs (Standard und WAF) und *keine* Application Gateway v1-SKUs. Wenn Sie eine neue Application Gateway-Instanz über das AGIC-Add-On bereitstellen, können Sie nur eine Application Gateway Standard_v2-SKU bereitstellen. Wenn Sie das Add-On für eine Application Gateway WAF_v2-SKU aktivieren möchten, verwenden Sie eine der folgenden Methoden:
>
> - Aktivieren Sie WAF für Application Gateway über das Portal. 
> - Erstellen Sie zuerst die WAF_v2-Instanz von Application Gateway, und befolgen Sie dann die Anweisungen zum [Aktivieren des AGIC-Add-Ons mit einem vorhandenen AKS-Cluster und vorhandener Application Gateway Instanz](tutorial-ingress-controller-add-on-existing.md). 

Im folgenden Beispiel stellen Sie einen neuen AKS-Cluster namens *myCluster* mithilfe von [Azure CNI](../aks/concepts-network.md#azure-cni-advanced-networking) und [verwalteten Identitäten](../aks/use-managed-identity.md) bereit. Das AGIC-Add-On wird in der Ressourcengruppe aktiviert, die Sie erstellt haben, *myResourceGroup*. 

Wenn Sie einen neuen AKS-Cluster mit aktiviertem AGIC-Add-On ohne Angabe einer vorhandenen Application Gateway-Instanz bereitstellen, bedeutet dies, dass automatisch eine Standard_v2-SKU-Instanz von Application Gateway erstellt wird. Sie müssen also auch den Namen und den Subnetzadressraum der Application Gateway-Instanz angeben. Der Name der Application Gateway-Instanz ist dann *myApplicationGateway* und der verwendete Subnetzadressraum „10.2.0.0/16“.

```azurecli-interactive
az aks create -n myCluster -g myResourceGroup --network-plugin azure --enable-managed-identity -a ingress-appgw --appgw-name myApplicationGateway --appgw-subnet-cidr "10.2.0.0/16" --generate-ssh-keys
```

Um zusätzliche Parameter für den Befehl `az aks create` zu konfigurieren, lesen Sie [diese Referenzen](/cli/azure/aks#az-aks-create). 

> [!NOTE]
> Der von Ihnen erstellte AKS-Cluster wird in der Ressourcengruppe aktiviert, die Sie erstellt haben, *myResourceGroup*. Allerdings befindet sich die automatisch erstellte Application Gateway-Instanz in der Knotenressourcengruppe, in der sich die Agentpools befinden. Die Knotenressourcengruppe hat standardmäßig den Namen *MC_resource-group-name_cluster-name_location*, der aber geändert werden kann. 

## <a name="deploy-a-sample-application-by-using-agic"></a>Bereitstellen einer Beispielanwendung mithilfe von AGIC

Nun stellen Sie eine Beispielanwendung für den von Ihnen erstellten AKS-Cluster bereit. Die Anwendung verwendet das AGIC-Add-On für den eingehenden Datenverkehr, und verbindet die Application Gateway-Instanz mit dem AKS-Cluster. 

Zuerst rufen Sie Anmeldeinformationen für den AKS-Cluster ab, indem Sie den Befehl `az aks get-credentials` ausführen: 

```azurecli-interactive
az aks get-credentials -n myCluster -g myResourceGroup
```

Nachdem Sie nun über die Anmeldeinformationen verfügen, führen Sie den folgenden Befehl aus, um eine Beispielanwendung einzurichten, die AGIC für beim Cluster eingehenden Datenverkehr verwendet. AGIC aktualisiert die Application Gateway-Instanz, die Sie zuvor eingerichtet haben, mit den entsprechenden Routingregeln für die neue Beispielanwendung, die Sie bereitgestellt haben.  

```azurecli-interactive
kubectl apply -f https://raw.githubusercontent.com/Azure/application-gateway-kubernetes-ingress/master/docs/examples/aspnetapp.yaml 
```

## <a name="check-that-the-application-is-reachable"></a>Überprüfen der Erreichbarkeit der Anwendung

Nachdem die Application Gateway-Instanz für den Datenverkehr zum AKS-Cluster eingerichtet wurde, überprüfen Sie, ob Ihre Anwendung erreichbar ist. Rufen Sie zunächst die IP-Adresse des eingehenden Datenverkehrs ab: 

```azurecli-interactive
kubectl get ingress
```

Überprüfen Sie mit einer der folgenden Methoden, ob die von Ihnen erstellte Beispielanwendung ausgeführt wird:

- Besuchen Sie die IP-Adresse der Application Gateway-Instanz, die Sie durch Ausführen des vorherigen Befehls erhalten haben.
- Verwenden Sie `curl`. 

Es kann ein paar Minuten dauern, bis Application Gateway das Update abgerufen hat. Wenn der Zustand von Application Gateway im Portal immer noch **Wird aktualisiert** lautet, warten Sie, bis der Vorgang abgeschlossen ist, bevor Sie versuchen, die IP-Adresse zu erreichen. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie sie nicht mehr benötigen, entfernen Sie die Ressourcengruppe, die Application Gateway-Instanz und alle dazugehörigen Ressourcen:

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Informationen zum Deaktivieren des AGIC-Add-Ons](./ingress-controller-disable-addon.md)
