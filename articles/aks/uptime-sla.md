---
title: Azure Kubernetes Service (AKS) mit Betriebszeit-SLA
description: Erfahren Sie mehr über das optionale Betriebszeit-SLA-Angebot für den Azure Kubernetes Service (AKS)-API-Server.
services: container-service
ms.topic: conceptual
ms.date: 05/19/2020
ms.openlocfilehash: e0e1399f69640dddfd618ac99637023390f28a92
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2020
ms.locfileid: "83683222"
---
# <a name="azure-kubernetes-service-aks-uptime-sla"></a>Azure Kubernetes Service (AKS): Betriebszeit-SLA

Betriebszeit-SLA ist ein optionales Feature, das eine finanziell abgesicherte, höhere SLA für einen Cluster ermöglicht. Betriebszeit-SLA garantiert eine Verfügbarkeit von 99,95 % des Kubernetes-API-Serverendpunkts für Cluster, die [Verfügbarkeitszonen][availability-zones] verwenden, und eine Verfügbarkeit von 99,9 % für Cluster, die keine Verfügbarkeitszonen verwenden. AKS verwendet Masterknotenreplikate über Update- und Fehlerdomänen hinweg, um sicherzustellen, dass die SLA-Anforderungen erfüllt werden.

Kunden, die eine SLA benötigen, um Complianceanforderungen zu erfüllen, oder die eine Erweiterung einer SLA auf ihre Endbenutzer erfordern, sollten dieses Feature aktivieren. Kunden mit kritischen Workloads, die von einer höheren Betriebszeit-SLA profitieren, haben möglicherweise ebenfalls Vorteile. Die Verwendung der Betriebszeit-SLA mit Verfügbarkeitszonen ermöglicht eine höhere Verfügbarkeit für die Betriebszeit des Kubernetes-API-Servers.  

Kunden können nach wie vor unbegrenzt kostenlose Cluster mit einem Servicelevelziel (SLO) von 99,5 % erstellen und sich je nach Bedarf für die bevorzugte SLO- oder SLA-Betriebszeit entscheiden.

> [!Important]
> Für Cluster mit ausgehender Sperrung finden Sie unter [Einschränken des ausgehenden Datenverkehrs](limit-egress-traffic.md) Informationen zum Öffnen entsprechender Ports.

## <a name="sla-terms-and-conditions"></a>SLA-Geschäftsbedingungen

Die Betriebszeit-SLA ist ein kostenpflichtiges Feature und wird pro Cluster aktiviert. Die Preise für die Betriebszeit-SLA werden durch die Anzahl der separaten Cluster und nicht durch die Größe der einzelnen Cluster bestimmt. Weitere Informationen finden Sie unter [Preisdetails zur Betriebszeit-SLA](https://azure.microsoft.com/pricing/details/kubernetes-service/).

## <a name="region-availability"></a>Regionale Verfügbarkeit

Betriebszeit-SLA ist in den folgenden Regionen verfügbar:

* Australien (Osten)
* Kanada, Mitte
* East US
* USA (Ost) 2
* USA Süd Mitte
* Südostasien
* USA, Westen 2

## <a name="before-you-begin"></a>Voraussetzungen

* Die Azure CLI mit Version 2.7.0 oder höher

## <a name="creating-a-cluster-with-uptime-sla"></a>Erstellen eines Clusters mit Betriebszeit-SLA

Um einen neuen Cluster mit Betriebszeit-SLA zu erstellen, verwenden Sie die Azure CLI.

Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen *myResourceGroup* am Standort *eastus*.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```
Erstellen Sie mithilfe des Befehls [az aks create][az-aks-create] einen AKS-Cluster. Im folgenden Beispiel wird ein Cluster mit dem Namen *myAKSCluster* mit einem Knoten erstellt. Azure Monitor für Container wird auch mit dem Parameter *--enable-addons monitoring* aktiviert.  Dieser Vorgang dauert einige Minuten.

```azurecli-interactive
az aks create --resource-group myResourceGroup --name myAKSCluster --uptime-sla --node-count 1 --enable-addons monitoring --generate-ssh-keys
```
Nach wenigen Minuten ist die Ausführung des Befehls abgeschlossen, und es werden Informationen zum Cluster im JSON-Format zurückgegeben. Der folgende JSON-Codeausschnitt zeigt den kostenpflichtigen Tarif für die SKU an, wodurch angezeigt wird, dass Betriebszeit-SLA für Ihren Cluster aktiviert ist.

```output
  },
  "sku": {
    "name": "Basic",
    "tier": "Paid"
  },
```

## <a name="limitations"></a>Einschränkungen

* Kann derzeit nicht als bestehender Cluster konvertiert werden, um die Betriebszeit-SLA zu aktivieren.
* Derzeit gibt es keine Möglichkeit, Betriebszeit-SLA aus einem AKS-Cluster zu entfernen, nachdem die Erstellung bei aktivierter Betriebszeit-SLA erfolgt ist.  
* Private Cluster werden zurzeit nicht unterstützt.

## <a name="next-steps"></a>Nächste Schritte

Verwenden Sie [Verfügbarkeitszonen][availability-zones], um die Hochverfügbarkeit Ihrer AKS-Clusterworkloads zu erhöhen.
Konfigurieren Sie den Cluster so, dass der [ausgehende Datenverkehr eingeschränkt](limit-egress-traffic.md) wird.

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
[region-availability]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service

<!-- LINKS - Internal -->
[vm-skus]: ../virtual-machines/linux/sizes.md
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool
[faq]: ./faq.md
[availability-zones]: ./availability-zones.md
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[limit-egress-traffic]: ./limit-egress-traffic.md
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
