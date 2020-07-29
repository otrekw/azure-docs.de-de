---
title: Verwenden einer statischen IP-Adresse für ausgehenden Datenverkehr
titleSuffix: Azure Kubernetes Service
description: Erfahren Sie, wie Sie eine statische öffentliche IP-Adresse für ausgehenden Datenverkehr in einem AKS-Cluster erstellen und verwenden
services: container-service
ms.topic: article
ms.date: 03/04/2019
ms.openlocfilehash: f7ea25c3348b96ec6d8818e8e1db4660b308dabc
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86517772"
---
# <a name="use-a-static-public-ip-address-for-egress-traffic-with-a-basic-sku-load-balancer-in-azure-kubernetes-service-aks"></a>Verwenden einer statischen öffentlichen IP-Adresse für ausgehenden Datenverkehr mit einem Lastenausgleich der SKU *Basic* in Azure Kubernetes Service (AKS)

Standardmäßig wird die ausgehende IP-Adresse eines AKS-Clusters zufällig zugewiesen. Diese Konfiguration ist nicht ideal, wenn Sie beispielsweise eine IP-Adresse für den Zugriff auf externe Dienste identifizieren müssen. Stattdessen müssen Sie möglicherweise eine statische IP-Adresse zuweisen, die einer Liste zugelassener IP-Adressen für den Dienstzugriff hinzugefügt wird.

In diesem Artikel wird Ihnen gezeigt, wie Sie eine statische öffentliche IP-Adresse für ausgehenden Datenverkehr in einem AKS-Cluster erstellen und verwenden.

## <a name="before-you-begin"></a>Voraussetzungen

In diesem Artikel wird davon ausgegangen, dass Sie Azure Load Balancer Basic verwenden.  Wir empfehlen die Verwendung von [Azure Load Balancer Standard](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview), und Sie können erweiterte Features zum [Steuern des ausgehenden AKS-Datenverkehrs](https://docs.microsoft.com/azure/aks/limit-egress-traffic) verwenden.

Es wird vorausgesetzt, dass Sie über ein AKS-Cluster verfügen. Wenn Sie einen AKS-Cluster benötigen, erhalten Sie weitere Informationen im AKS-Schnellstart. Verwenden Sie dafür entweder die [Azure CLI][aks-quickstart-cli] oder das [Azure-Portal][aks-quickstart-portal].

Außerdem muss mindestens die Version 2.0.59 der Azure CLI installiert und konfiguriert sein. Führen Sie  `az --version` aus, um die Version zu ermitteln. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie weitere Informationen unter  [Installieren der Azure CLI][install-azure-cli].

> [!IMPORTANT]
> In diesem Artikel wird der Lastenausgleich der SKU *Basic* mit einem einzelnen Knotenpool verwendet. Diese Konfiguration ist für mehrere Knotenpools nicht verfügbar, da der Lastenausgleich der SKU *Basic* mit mehreren Knotenpools nicht unterstützt wird. Unter [Verwenden einer öffentlichen Instanz von Load Balancer Standard in Azure Kubernetes Service (AKS)][slb] finden Sie ausführlichere Informationen zur Verwendung des Lastenausgleichs der SKU *Standard*.

## <a name="egress-traffic-overview"></a>Übersicht über ausgehenden Datenverkehr

Der ausgehende Datenverkehr aus einem AKS-Cluster unterliegt den [Azure Load Balancer-Konventionen][outbound-connections]. Bevor der erste Kubernetes-Dienst des Typs `LoadBalancer` erstellt wird, gehören die Agent-Knoten in einem AKS-Cluster nicht zum Azure Load Balancer-Pool. In dieser Konfiguration verfügen die Knoten über keine öffentliche IP-Adresse auf Instanzebene. Azure verschiebt den ausgehenden Datenfluss zu einer öffentlichen Quell-IP-Adresse, die nicht konfigurierbar oder deterministisch ist.

Wenn ein Kubernetes-Dienst des Typs `LoadBalancer` erstellt wurde, werden Knoten des Agent einem Azure Load Balancer-Pool hinzugefügt. Azure verschiebt den ausgehenden Datenfluss zu der ersten öffentlichen IP-Adresse, die vom Load Balancer konfiguriert wurde. Diese öffentliche IP-Adresse gilt nur für die Lebensdauer dieser Ressource. Wenn Sie den Kubernetes-Lastenausgleichsdienst löschen, werden auch der zugehörige Lastenausgleich und die zugehörige IP-Adresse gelöscht. Wenn Sie eine bestimmte IP-Adresse für einen Kubernetes-Dienst zuweisen oder beibehalten möchten, können Sie eine statische öffentliche IP-Adresse erstellen und verwenden.

## <a name="create-a-static-public-ip"></a>Erstellen einer statischen öffentlichen IP-Adresse

Rufen Sie den Namen der Ressourcengruppe mit dem Befehl [az aks show][az-aks-show] ab, und fügen Sie den Abfrageparameter `--query nodeResourceGroup` hinzu. Im folgenden Beispiel wird der Knoten „Ressourcengruppe“ für den AKS-Clusternamen *myAKSCluster* in der Ressourcengruppe *myResourceGroup* abgerufen:

```azurecli-interactive
$ az aks show --resource-group myResourceGroup --name myAKSCluster --query nodeResourceGroup -o tsv

MC_myResourceGroup_myAKSCluster_eastus
```

Erstellen Sie dann über den Befehl [az network public-ip create][az-network-public-ip-create] eine statische öffentliche IP-Adresse. Geben Sie den Knoten „Ressourcengruppe“ an, den Sie über den vorherigen Befehl erhalten haben, und fügen Sie anschließend einen Namen für die Ressource der IP-Adresse hinzu, z.B. *myAKSPublicIP*:

```azurecli-interactive
az network public-ip create \
    --resource-group MC_myResourceGroup_myAKSCluster_eastus \
    --name myAKSPublicIP \
    --allocation-method static
```

Die IP-Adresse wird ähnlich wie in der folgenden gekürzten Beispielausgabe angezeigt:

```json
{
  "publicIp": {
    "dnsSettings": null,
    "etag": "W/\"6b6fb15c-5281-4f64-b332-8f68f46e1358\"",
    "id": "/subscriptions/<SubscriptionID>/resourceGroups/MC_myResourceGroup_myAKSCluster_eastus/providers/Microsoft.Network/publicIPAddresses/myAKSPublicIP",
    "idleTimeoutInMinutes": 4,
    "ipAddress": "40.121.183.52",
    [..]
  }
```

Anschließend können Sie die öffentliche IP-Adresse über den Befehl [az network public-ip list][az-network-public-ip-list] abrufen. Geben Sie den Namen der node-Ressourcengruppe an, und fragen Sie anschließend wie im folgenden Beispiel gezeigt den Wert *ipAddress* ab:

```azurecli-interactive
$ az network public-ip list --resource-group MC_myResourceGroup_myAKSCluster_eastus --query [0].ipAddress --output tsv

40.121.183.52
```

## <a name="create-a-service-with-the-static-ip"></a>Erstellen eines Dienstes mit der statischen IP-Adresse

Fügen Sie die `loadBalancerIP`-Eigenschaft und den Wert der öffentlichen statischen IP-Adresse dem YAML-Manifest hinzu, um mit der statischen öffentlichen IP-Adresse einen Dienst zu erstellen. Erstellen Sie eine Datei namens „`egress-service.yaml`“, und fügen Sie den folgenden YAML-Code ein. Geben Sie Ihre eigene öffentliche IP-Adresse an, die Sie im vorherigen Schritt erstellt haben:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: azure-egress
spec:
  loadBalancerIP: 40.121.183.52
  type: LoadBalancer
  ports:
  - port: 80
```

Erstellen Sie den Dienst und die Bereitstellung mit dem Befehl `kubectl apply`.

```console
kubectl apply -f egress-service.yaml
```

Beim Erstellen dieses Dienstes wird eine neue Front-End-IP in Azure Load Balancer konfiguriert. Wenn keine anderen IP-Adressen konfiguriert wurden, wird ab sofort **sämtlicher ausgehender Datenverkehr** diese Adresse verwenden. Wenn für den Azure Load Balancer mehrere Adressen konfiguriert sind, sind diese öffentlichen IP-Adressen Kandidaten für ausgehende Datenflüsse, und eine davon wird nach dem Zufallsprinzip ausgewählt.

## <a name="verify-egress-address"></a>Überprüfen der Adresse für ausgehende Daten

Sie können einen DNS-Lookup-Dienst wie `checkip.dyndns.org` verwenden, um zu überprüfen, ob die statische öffentliche IP-Adresse verwendet wird.

Beginnen Sie zunächst mit dem Anfügen eines einfachen *Debian*-Pods:

```console
kubectl run -it --rm aks-ip --image=debian
```

Verwenden Sie `apt-get` zum Installieren von `curl` im Container, um innerhalb des Containers auf eine Website zuzugreifen.

```console
apt-get update && apt-get install curl -y
```

Verwenden Sie nun „curl“, um auf die Website *checkip.dyndns.org* zuzugreifen. Die ausgehende IP-Adresse wird ähnlich wie in der folgenden Beispielausgabe angezeigt. Diese IP-Adresse stimmt mit der statischen öffentlichen IP-Adresse überein, die für den Lastenausgleichsdienst erstellt und definiert wurde:

```console
$ curl -s checkip.dyndns.org

<html><head><title>Current IP Check</title></head><body>Current IP Address: 40.121.183.52</body></html>
```

## <a name="next-steps"></a>Nächste Schritte

Um der Verwaltung mehrerer öffentlicher IP-Adressen in Azure Load Balancer zu entgehen, können Sie stattdessen einen Eingangscontroller verwenden. Eingangscontroller bieten zusätzliche Vorteile wie SSL-/TLS-Terminierung, Unterstützung der URI-Neugenerierung und Upstream der SSL-/TLS-Verschlüsselung. Weitere Informationen finden Sie unter [Erstellen eines einfachen Eingangscontrollers in AKS][ingress-aks-cluster].

<!-- LINKS - internal -->
[az-network-public-ip-create]: /cli/azure/network/public-ip#az-network-public-ip-create
[az-network-public-ip-list]: /cli/azure/network/public-ip#az-network-public-ip-list
[az-aks-show]: /cli/azure/aks#az-aks-show
[azure-cli-install]: /cli/azure/install-azure-cli
[ingress-aks-cluster]: ./ingress-basic.md
[outbound-connections]: ../load-balancer/load-balancer-outbound-connections.md#scenarios
[public-ip-create]: /cli/azure/network/public-ip#az-network-public-ip-create
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[slb]: load-balancer-standard.md
