---
title: Verwenden einer statischen IP-Adresse mit dem Lastenausgleich von Azure Kubernetes Service (AKS)
description: Informationen zum Erstellen und Verwenden einer statischen IP-Adresse mit dem Lastenausgleich von Azure Kubernetes Service (AKS)
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 11/06/2019
ms.author: mlearned
ms.openlocfilehash: 8457f1c0c5b6107c4b44f6f00236a33f7c67452a
ms.sourcegitcommit: b77e97709663c0c9f84d95c1f0578fcfcb3b2a6c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/22/2019
ms.locfileid: "74325442"
---
# <a name="use-a-static-public-ip-address-with-the-azure-kubernetes-service-aks-load-balancer"></a>Verwenden einer statischen öffentlichen IP-Adresse mit dem Lastenausgleich von Azure Kubernetes Service (AKS)

Standardmäßig ist die öffentliche IP-Adresse, die einer Lastenausgleichsressource zugeordnet ist, die von einem AKS-Cluster erstellt wurde, nur gültig solange die Ressource existiert. Wenn Sie den Kubernetes-Dienst löschen, werden auch der zugehörige Lastenausgleich und die zugehörige IP-Adresse gelöscht. Wenn Sie eine bestimmte IP-Adresse für einen Kubernetes-Dienst zuweisen oder beibehalten möchten, können Sie eine statische öffentliche IP-Adresse erstellen und verwenden.

In diesem Artikel wird erläutert, wie Sie eine statische öffentliche IP-Adresse erstellen und Ihrem Kubernetes-Dienst zuweisen.

## <a name="before-you-begin"></a>Voraussetzungen

Es wird vorausgesetzt, dass Sie über ein AKS-Cluster verfügen. Wenn Sie einen AKS-Cluster benötigen, erhalten Sie weitere Informationen im AKS-Schnellstart. Verwenden Sie dafür entweder die [Azure CLI][aks-quickstart-cli] oder das [Azure-Portal][aks-quickstart-portal].

Außerdem muss mindestens die Version 2.0.59 der Azure CLI installiert und konfiguriert sein. Führen Sie  `az --version` aus, um die Version zu ermitteln. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie weitere Informationen unter  [Installieren der Azure CLI][install-azure-cli].

In diesem Artikel wird die Verwendung einer IP-Adresse der SKU *Standard* mit einem Lastenausgleich der SKU *Standard* behandelt. Weitere Informationen finden Sie unter [IP-Adresstypen und Zuordnungsmethoden in Azure][ip-sku].

## <a name="create-a-static-ip-address"></a>Erstellen einer statischen IP-Adresse

Erstellen Sie mithilfe des Befehls [az network public-ip create][az-network-public-ip-create] eine statische öffentliche IP-Adresse. Im folgenden Beispiel wird eine statische IP-Adressressource namens *myAKSPublicIP* in der Ressourcengruppe *myResourceGroup* erstellt:

```azurecli-interactive
az network public-ip create \
    --resource-group myResourceGroup \
    --name myAKSPublicIP \
    --sku Standard \
    --allocation-method static
```

> [!NOTE]
> Falls Sie in Ihrem AKS-Cluster einen Lastenausgleich der SKU *Basic* verwenden, geben Sie für den Parameter *sku* die Option *Basic* an, wenn Sie eine öffentliche IP-Adresse definieren. Mit dem Lastenausgleich der SKU *Basic* können nur IP-Adressen der SKU *Basic* verwendet werden. Analog dazu können mit einem Lastenausgleich der SKU *Standard* nur IP-Adressen der SKU *Standard* verwendet werden. 

Die IP-Adresse wird ähnlich wie in der folgenden gekürzten Beispielausgabe angezeigt:

```json
{
  "publicIp": {
    ...
    "ipAddress": "40.121.183.52",
    ...
  }
}
```

Anschließend können Sie die öffentliche IP-Adresse über den Befehl [az network public-ip list][az-network-public-ip-list] abrufen. Geben Sie den Namen der erstellten Knotenressourcengruppe und die öffentliche IP-Adresse an, und fragen Sie anschließend wie im folgenden Beispiel gezeigt den Wert von *ipAddress* ab:

```azurecli-interactive
$ az network public-ip show --resource-group myResourceGroup --name myAKSPublicIP --query ipAddress --output tsv

40.121.183.52
```

## <a name="create-a-service-using-the-static-ip-address"></a>Erstellen eines Diensts mithilfe der statischen IP-Adresse

Vergewissern Sie sich vor dem Erstellen eines Diensts, dass der vom AKS-Cluster verwendete Dienstprinzipal über delegierte Berechtigungen für die andere Ressourcengruppe verfügt. Beispiel:

```azurecli-interactive
az role assignment create \
    --assignee <SP Client ID> \
    --role "Contributor" \
    --scope /subscriptions/<subscription id>/resourceGroups/<resource group name>
```

Fügen Sie die Eigenschaft `loadBalancerIP` und den Wert der statischen öffentlichen IP-Adresse dem YAML-Manifest hinzu, um einen Dienst vom Typ *LoadBalancer* mit der statischen öffentlichen IP-Adresse zu erstellen. Erstellen Sie eine Datei namens „`load-balancer-service.yaml`“, und fügen Sie den folgenden YAML-Code ein. Geben Sie Ihre eigene öffentliche IP-Adresse an, die Sie im vorherigen Schritt erstellt haben: Im folgenden Beispiel wird auch die Anmerkung auf die Ressourcengruppe *myResourceGroup* festgelegt. Geben Sie Ihren eigenen Ressourcengruppennamen an.

```yaml
apiVersion: v1
kind: Service
metadata:
  annotations:
    service.beta.kubernetes.io/azure-load-balancer-resource-group: myResourceGroup
  name: azure-load-balancer
spec:
  loadBalancerIP: 40.121.183.52
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-load-balancer
```

Erstellen Sie den Dienst und die Bereitstellung mit dem Befehl `kubectl apply`.

```console
kubectl apply -f load-balancer-service.yaml
```

## <a name="troubleshoot"></a>Problembehandlung

Wenn es die in der *loadBalancerIP*-Eigenschaft des Dienstmanifests für Kubernetes angegebene statische IP-Adresse nicht gibt oder nicht in dem Knoten „Ressourcengruppe“ erstellt wurde und keine zusätzlichen Delegierungen konfiguriert sind, kann der Lastenausgleichsdienst nicht erstellt werden. Prüfen Sie zur Problembehandlung mithilfe des Befehls [kubectl describe][kubectl-describe] die Ereignisse, die zur Erstellung des Diensts geführt haben. Geben Sie den Namen des Diensts wie folgt genauso wie in dem YAML-Manifest an:

```console
kubectl describe service azure-load-balancer
```

Es werden Informationen zur Dienstressource für Kubernetes angezeigt. Die *Ereignisse*, die am Ende der folgenden Beispielausgabe angezeigt werden, deuten darauf hin, dass die vom *Benutzer angegebene IP-Adresse nicht gefunden wurde*. Überprüfen Sie dann, ob Sie tatsächlich eine statische öffentliche IP-Adresse im Knoten „Ressourcengruppe“ erstellt haben und ob die im Dienstmanifest für Kubernetes angegebene IP-Adresse stimmt.

```
Name:                     azure-load-balancer
Namespace:                default
Labels:                   <none>
Annotations:              <none>
Selector:                 app=azure-load-balancer
Type:                     LoadBalancer
IP:                       10.0.18.125
IP:                       40.121.183.52
Port:                     <unset>  80/TCP
TargetPort:               80/TCP
NodePort:                 <unset>  32582/TCP
Endpoints:                <none>
Session Affinity:         None
External Traffic Policy:  Cluster
Events:
  Type     Reason                      Age               From                Message
  ----     ------                      ----              ----                -------
  Normal   CreatingLoadBalancer        7s (x2 over 22s)  service-controller  Creating load balancer
  Warning  CreatingLoadBalancerFailed  6s (x2 over 12s)  service-controller  Error creating load balancer (will retry): Failed to create load balancer for service default/azure-load-balancer: user supplied IP Address 40.121.183.52 was not found
```

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie mehr Kontrolle über den Netzwerkdatenverkehr benötigen, der an Ihre Anwendungen gesendet wird, können Sie stattdessen einen [Eingangscontroller erstellen][aks-ingress-basic]. Sie können aber auch einen Eingangscontroller mit einer statischen öffentlichen IP-Adresse erstellen. Außerdem können Sie einen [Eingangscontroller mit einer statischen öffentlichen IP-Adresse erstellen][aks-static-ingress].

<!-- LINKS - External -->
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe

<!-- LINKS - Internal -->
[aks-faq-resource-group]: faq.md#why-are-two-resource-groups-created-with-aks
[az-network-public-ip-create]: /cli/azure/network/public-ip#az-network-public-ip-create
[az-network-public-ip-list]: /cli/azure/network/public-ip#az-network-public-ip-list
[az-aks-show]: /cli/azure/aks#az-aks-show
[aks-ingress-basic]: ingress-basic.md
[aks-static-ingress]: ingress-static-ip.md
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[ip-sku]: ../virtual-network/virtual-network-ip-addresses-overview-arm.md#sku
