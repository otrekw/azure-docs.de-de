---
title: Verwenden eines Lastenausgleichs mit Standard-SKU
titleSuffix: Azure Kubernetes Service
description: Hier erfahren Sie, wie Sie einen Lastenausgleich mit einer Standard-SKU verwenden, um Ihre Dienste mit Azure Kubernetes Service (AKS) verfügbar zu machen.
services: container-service
author: zr-msft
ms.topic: article
ms.date: 09/27/2019
ms.author: zarhoads
ms.openlocfilehash: 3be60888d3d12d37650ad2cffc1911fb3b5e6682
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/05/2020
ms.locfileid: "82790676"
---
# <a name="use-a-standard-sku-load-balancer-in-azure-kubernetes-service-aks"></a>Verwenden eines Lastenausgleichs mit einer Standard-SKU in Azure Kubernetes Service (AKS)

Sie können eine Azure Load Balancer-Instanz verwenden, um über Kubernetes-Dienste Zugriff auf Anwendungen vom Typ `LoadBalancer` in Azure Kubernetes Service (AKS) zu ermöglichen. Ein in AKS ausgeführter Lastenausgleich kann als interner oder externer Lastenausgleich verwendet werden. Bei einem internen Lastenausgleich können nur Anwendungen, die im gleichen virtuellen Netzwerk ausgeführt werden wie der AKS-Cluster, auf einen Kubernetes-Dienst zugreifen. Ein externer Lastenausgleich erhält mindestens eine öffentliche IP-Adresse für eingehenden Datenverkehr, um externe Zugriffe auf einen Kubernetes-Dienst über die öffentlichen IP-Adressen zu ermöglichen.

Azure Load Balancer ist in zwei SKUs verfügbar: *Basic* und *Standard*. Beim Erstellen eines AKS-Clusters wird normalerweise die SKU vom Typ *Standard* verwendet. Bei Verwendung eines Lastenausgleich mit einer *Standard*-SKU stehen zusätzliche Features und Funktionen zur Verfügung (etwa ein größerer Back-End-Pool und Verfügbarkeitszonen). Wichtig: Machen Sie sich mit den Unterschieden zwischen *Standard* und *Basic* vertraut, bevor Sie sich für einen Lastenausgleich entscheiden. Nach Erstellung eines AKS-Clusters kann die Lastenausgleichs-SKU für diesen Cluster nicht mehr geändert werden. Weitere Informationen zu den SKUs *Basic* und *Standard* finden Sie unter [Vergleich der Load Balancer-SKUs][azure-lb-comparison].

Für diesen Artikel werden Grundkenntnisse im Zusammenhang mit Kubernetes und Azure Load Balancer vorausgesetzt. Weitere Informationen finden Sie unter [Grundlegende Kubernetes-Konzepte für Azure Kubernetes Service (AKS)][kubernetes-concepts] und [Was versteht man unter Azure Load Balancer?][azure-lb].

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die Befehlszeilenschnittstelle (CLI) lokal installieren und verwenden möchten, müssen Sie für diesen Artikel mindestens Version 2.0.81 der Azure-Befehlszeilenschnittstelle ausführen. Führen Sie `az --version` aus, um die Version zu ermitteln. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sie bei Bedarf unter [Installieren der Azure CLI][install-azure-cli].

## <a name="before-you-begin"></a>Voraussetzungen

In diesem Artikel wird davon ausgegangen, dass Sie über einen AKS-Cluster mit der Azure Load Balancer-SKU *Standard* verfügen. Wenn Sie einen AKS-Cluster benötigen, erhalten Sie weitere Informationen im AKS-Schnellstart. Verwenden Sie dafür entweder die [Azure CLI][aks-quickstart-cli] oder das [Azure-Portal][aks-quickstart-portal].

Der AKS-Clusterdienstprinzipal benötigt auch die Berechtigung zum Verwalten von Netzwerkressourcen, wenn Sie ein bestehendes Subnetz oder eine vorhandene Ressourcengruppe verwenden. Im Allgemeinen weisen Sie die Rolle *Netzwerkmitwirkender* Ihrem Dienstprinzipal für die delegierten Ressourcen zu. Anstelle eines Dienstprinzipals können Sie für Berechtigungen auch die vom System zugewiesene verwaltete Identität verwenden. Weitere Informationen finden Sie unter [Verwenden verwalteter Identitäten](use-managed-identity.md). Weitere Informationen zu Berechtigungen finden Sie unter [Delegieren des Zugriffs auf andere Azure-Ressourcen][aks-sp].

### <a name="moving-from-a-basic-sku-load-balancer-to-standard-sku"></a>Wechseln von einem Lastenausgleich mit einer Basic-SKU zu einer Standard-SKU

Wenn Sie bereits über einen Cluster mit einem Lastenausgleich mit einer Basic-SKU verfügen, sind bei der Migration zur Verwendung eines Clusters mit einem Lastenausgleich mit einer Standard-SKU wichtige Verhaltensunterschiede zu beachten.

Beispielsweise ist das Erstellen von Blau/Grün-Bereitstellungen zum Migrieren von Clustern eine gängige Vorgehensweise, da der `load-balancer-sku`-Typ eines Clusters nur zum Zeitpunkt der Clustererstellung definiert werden kann. Allerdings verwendet ein Lastenausgleich mit einer *Basic-SKU* IP-Adressen der *Basic-SKU*, die nicht mit einem Lastenausgleich mit einer *Standard-SKU* kompatibel sind, da diese IP-Adressen der *Standard-SKU* benötigen. Beim Migrieren von Clustern zum Upgraden von Load Balancer-SKUs ist eine neue IP-Adresse mit einer kompatiblen IP-Adressen-SKU erforderlich.

Weitere Informationen zum Migrieren von Clustern finden Sie in der [Dokumentation mit Überlegungen zur Migration](aks-migration.md), die eine Liste wichtiger Themen enthält, die bei der Migration zu berücksichtigen sind. Die folgenden Einschränkungen sind ebenfalls wichtige Verhaltensunterschiede, die bei der Verwendung eines Lastenausgleichs mit einer Standard-SKU in AKS zu beachten sind.

### <a name="limitations"></a>Einschränkungen

Wenn Sie AKS-Cluster erstellen und verwalten, die einen Lastenausgleich mit der SKU *Standard* unterstützen, gelten folgende Einschränkungen:

* Mindestens eine öffentliche IP-Adresse oder ein IP-Präfix ist erforderlich, um ausgehenden Datenverkehr aus dem AKS-Cluster zuzulassen. Darüber hinaus wird die öffentliche IP-Adresse oder das IP-Präfix benötigt, um die Konnektivität zwischen der Steuerungsebene und den Agent-Knoten sowie die Kompatibilität mit früheren Versionen von AKS zu gewährleisten. Sie haben die folgenden Optionen zum Angeben von öffentlichen IP-Adressen oder IP- Präfixen mit einem *Standard*-SKU-Lastenausgleichsmodul:
    * Geben Sie Ihre eigenen öffentlichen IP-Adressen an.
    * Geben Sie Ihre eigenen öffentlichen IP-Präfixe an.
    * Geben Sie eine Zahl bis 100 an, damit der AKS-Cluster so viele öffentliche *Standard*-SKU-IP-Adressen in derselben Ressourcengruppe erstellen kann, in der sich der AKS-Cluster befindet, in der Regel mit *MC_* am Anfang benannt. AKS weist die öffentliche IP-Adresse dem Lastenausgleich mit der SKU *Standard* zu. Standardmäßig wird automatisch eine öffentliche IP-Adresse in derselben Ressourcengruppe wie der AKS-Cluster erstellt, wenn weder eine öffentliche IP-Adresse noch ein öffentliches IP-Präfix oder eine Anzahl von IP-Adressen angegeben wird. Sie müssen auch öffentliche Adressen zulassen und dürfen keine Azure-Richtlinie erstellen, die die Erstellung von IP-Adressen unterbindet.
* Wenn Sie für einen Lastenausgleich die *Standard*-SKU verwenden, benötigen Sie die Kubernetes-Version *1.13 oder höher*.
* Das Definieren der Lastenausgleichs-SKU kann nur durchgeführt werden, wenn Sie einen AKS-Cluster erstellen. Nach der Erstellung eines AKS-Clusters kann die Lastenausgleichs-SKU nicht mehr geändert werden.
* Pro Cluster kann immer nur eine Art von Lastenausgleichs-SKU („Basic“ oder „Standard“) verwendet werden.
* Ein Lastenausgleich mit einer *Standard*-SKU unterstützt nur IP-Adressen der *Standard*-SKU.

## <a name="use-the-standard-sku-load-balancer"></a>Verwenden eines Lastenausgleichs mit *Standard*-SKU

Wenn Sie einen AKS-Cluster erstellen, wird standardmäßig ein Lastenausgleich mit *Standard*-SKU verwendet, wenn Sie Dienste in diesem Cluster ausführen. So wird beispielsweise im [Schnellstart mit der Azure-Befehlszeilenschnittstelle][aks-quickstart-cli] eine Beispielanwendung bereitgestellt, die einen Lastenausgleich mit *Standard*-SKU verwendet.

> [!IMPORTANT]
> Öffentliche IP-Adressen können durch Anpassen einer benutzerdefinierten Route (User-Defined Route, UDR) vermieden werden. Wenn Sie den ausgehenden Typ eines AKS-Clusters als UDR angeben, können die IP-Bereitstellung und die Back-End-Pool-Einrichtung für den per AKS erstellten Azure-Lastenausgleich übersprungen werden. Informationen finden Sie im Artikel zum [Festlegen des `outboundType`-Elements eines Clusters auf „userDefinedRouting“](egress-outboundtype.md).

## <a name="configure-the-load-balancer-to-be-internal"></a>Konfigurieren des Load Balancers als internen Lastenausgleich

Sie können den Lastenausgleich auch als internen Lastenausgleich konfigurieren und keine öffentliche IP-Adresse verfügbar machen. Wenn Sie den Lastenausgleich als intern konfigurieren möchten, müssen Sie dem Dienst *LoadBalancer* die Anmerkung `service.beta.kubernetes.io/azure-load-balancer-internal: "true"` hinzufügen. Ein exemplarisches YAML-Manifest sowie weitere Details zum internen Lastenausgleich finden Sie [hier][internal-lb-yaml].

## <a name="scale-the-number-of-managed-public-ips"></a>Skalieren der Anzahl der verwalteten öffentlichen IP-Adressen

Wenn Sie einen *Standard*-SKU-Lastenausgleich mit verwalteten ausgehenden öffentlichen IP-Adressen verwenden, die standardmäßig erstellt werden, können Sie die Anzahl verwalteter ausgehender öffentlicher IP-Adressen mit dem *load-balancer-managed-ip-count*-Parameter skalieren.

Zum Aktualisieren eines vorhandenen Clusters führen Sie den unten angegebenen Befehl aus. Dieser Parameter kann auch zum Zeitpunkt der Clustererstellung festgelegt werden, um mehrere verwaltete ausgehende öffentliche IP-Adressen zu erhalten.

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --load-balancer-managed-outbound-ip-count 2
```

Im obigen Beispiel wird die Anzahl der verwalteten ausgehenden öffentlichen IP-Adressen für den *myAKSCluster*-Cluster in *myResourceGroup* auf *2* festgelegt. 

Sie können auch den Parameter *load-balancer-managed-ip-count* verwenden, um die anfängliche Anzahl von verwalteten ausgehenden öffentlichen IP-Adressen beim Erstellen des Clusters festzulegen. Dazu fügen Sie den Parameter `--load-balancer-managed-outbound-ip-count` an und legen ihn auf den gewünschten Wert fest. Die Standardanzahl von verwalteten ausgehenden öffentlichen IP-Adressen beträgt 1.

## <a name="provide-your-own-public-ips-or-prefixes-for-egress"></a>Angeben Ihrer eigenen öffentlichen IP-Adressen oder Präfixe für ausgehenden Datenverkehr

Wenn Sie einen *Standard*-SKU-Lastenausgleich verwenden, erstellt der AKS-Cluster automatisch eine öffentliche IP-Adresse in derselben Ressourcengruppe, die für den AKS-Cluster erstellt wurde, und weist die öffentliche IP-Adresse dem *Standard*-SKU-Lastenausgleich zu. Alternativ können Sie Ihre eigene öffentliche IP-Adresse zum Zeitpunkt der Clustererstellung zuweisen, oder Sie können die Lastenausgleichseigenschaften eines vorhandenen Clusters aktualisieren.

Durch Einbinden mehrerer IP-Adressen oder IP-Präfixe können Sie mehrere unterstützende Dienste definieren, wenn Sie die IP-Adresse hinter einem einzelnen Lastenausgleichsobjekt definieren. Der Endpunkt für ausgehenden Datenverkehr bestimmter Knoten hängt von dem Dienst ab, dem sie zugeordnet sind.

> [!IMPORTANT]
> Sie müssen die öffentlichen IP-Adressen der *Standard*-SKU für ausgehenden Datenverkehr mit Ihrem *Standard*-SKU-Lastenausgleich verwenden. Sie können die SKU Ihrer öffentlichen IP-Adressen mit dem Befehl [az network public-ip show][az-network-public-ip-show] überprüfen:
>
> ```azurecli-interactive
> az network public-ip show --resource-group myResourceGroup --name myPublicIP --query sku.name -o tsv
> ```

Verwenden Sie den Befehl [az network public-ip show][az-network-public-ip-show], um die IDs Ihrer öffentlichen IP-Adressen aufzulisten.

```azurecli-interactive
az network public-ip show --resource-group myResourceGroup --name myPublicIP --query id -o tsv
```

Der obige Befehl zeigt die ID für die öffentliche IP-Adresse *myPublicIP* in der Ressourcengruppe *myResourceGroup* an.

Verwenden Sie den Befehl *az aks update* mit dem Parameter *load-balancer-outbound-ips*, um Ihren Cluster mit ihren öffentlichen IP-Adressen zu aktualisieren.

Im folgenden Beispiel wird der Parameter *load-balancer-outbound-ips* mit den IDs aus dem vorherigen Befehl verwendet.

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --load-balancer-outbound-ips <publicIpId1>,<publicIpId2>
```

Sie können auch öffentliche IP-Präfixe für ausgehenden Datenverkehr mit Ihrem *Standard*-SKU-Lastenausgleich verwenden. Im folgenden Beispiel wird der Befehl [az network public-ip prefix show][az-network-public-ip-prefix-show] verwendet, um die IDs Ihrer öffentlichen IP-Präfixe aufzulisten:

```azurecli-interactive
az network public-ip prefix show --resource-group myResourceGroup --name myPublicIPPrefix --query id -o tsv
```

Der obige Befehl zeigt die ID für das öffentliche IP-Präfix *myPublicIPPrefix* in der Ressourcengruppe *myResourceGroup* an.

Im folgenden Beispiel wird der Parameter *load-balancer-outbound-ip-prefixes* mit den IDs aus dem vorherigen Befehl verwendet.

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --load-balancer-outbound-ip-prefixes <publicIpPrefixId1>,<publicIpPrefixId2>
```

> [!IMPORTANT]
> Die öffentlichen IP-Adressen und IP-Präfixe müssen sich in derselben Region wie Ihr AKS-Cluster befinden und Teil desselben Abonnements sein. 

### <a name="define-your-own-public-ip-or-prefixes-at-cluster-create-time"></a>Definieren eigener öffentlicher IP-Adressen oder IP-Präfixe zum Zeitpunkt der Clustererstellung

Möglicherweise möchten Sie zum Zeitpunkt der Clustererstellung Ihre eigenen IP-Adressen oder IP-Präfixe für den ausgehenden Datenverkehr einbinden, um Szenarien wie das Aufnehmen von Endpunkten für ausgehenden Datenverkehr in die Whitelists zu unterstützen. Fügen Sie die oben gezeigten Parameter im Schritt zur Clustererstellung an, um Ihre eigenen öffentlichen IP-Adressen und IP-Präfixe zu Beginn des Lebenszyklus eines Clusters zu definieren.

Verwenden Sie den Befehl *az aks create* mit dem Parameter *load-balancer-outbound-ips*, um einen neuen Cluster mit Ihren öffentlichen IP-Adressen zu Beginn zu erstellen.

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --vm-set-type VirtualMachineScaleSets \
    --node-count 1 \
    --load-balancer-sku standard \
    --generate-ssh-keys \
    --load-balancer-outbound-ips <publicIpId1>,<publicIpId2>
```

Verwenden Sie den Befehl *az aks create* mit dem Parameter *load-balancer-outbound-ip-prefixes*, um einen neuen Cluster mit Ihren öffentlichen IP-Präfixen zu Beginn zu erstellen.

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --vm-set-type VirtualMachineScaleSets \
    --node-count 1 \
    --load-balancer-sku standard \
    --generate-ssh-keys \
    --load-balancer-outbound-ip-prefixes <publicIpPrefixId1>,<publicIpPrefixId2>
```

## <a name="configure-outbound-ports-and-idle-timeout"></a>Konfigurieren von ausgehenden Ports und Leerlauftimeout

> [!WARNING]
> Der folgende Abschnitt ist für erweiterte Szenarien mit größeren Netzwerken oder für die Behandlung von Problemen mit der SNAT-Auslastung bei Standardkonfigurationen konzipiert. Sie müssen über genaue Bestandsinformationen zu den verfügbaren Kontingenten für VMs und IP-Adressen verfügen, bevor Sie den Standardwert für *AllocatedOutboundPorts* oder *IdleTimeoutInMinutes* ändern, um die Fehlerfreiheit der Cluster sicherzustellen.
> 
> Durch die Änderung der Werte für *AllocatedOutboundPorts* und *IdleTimeoutInMinutes* kann sich das Verhalten der Ausgangsregel für Ihren Lastenausgleich erheblich ändern. Informieren Sie sich über die [Load Balancer-Ausgangsregeln][azure-lb-outbound-rules-overview] (auch in [diesem Artikel][azure-lb-outbound-rules]) und [ausgehende Verbindungen in Azure][azure-lb-outbound-connections], bevor Sie diese Werte aktualisieren, um die Auswirkungen Ihrer Änderungen richtig zu verstehen.

Zugeordnete ausgehende Ports und die zugehörigen Leerlauftimeouts werden für [SNAT][azure-lb-outbound-connections] verwendet. Standardmäßig wird vom Load Balancer mit der SKU *Standard* die [automatische Zuweisung für die Anzahl von ausgehenden Ports basierend auf der Back-End-Poolgröße][azure-lb-outbound-preallocatedports] und ein Leerlauftimeout von 30 Minuten für jeden Port verwendet. Zeigen Sie die Ausgangsregel für den Load Balancer mit dem Befehl [az network lb outbound-rule list][az-network-lb-outbound-rule-list] an, um diese Werte zu erhalten:

```azurecli-interactive
NODE_RG=$(az aks show --resource-group myResourceGroup --name myAKSCluster --query nodeResourceGroup -o tsv)
az network lb outbound-rule list --resource-group $NODE_RG --lb-name kubernetes -o table
```

Mit den vorherigen Befehlen wird die Ausgangsregel für den Load Balancer aufgelistet, z. B.:

```console
AllocatedOutboundPorts    EnableTcpReset    IdleTimeoutInMinutes    Name             Protocol    ProvisioningState    ResourceGroup
------------------------  ----------------  ----------------------  ---------------  ----------  -------------------  -------------
0                         True              30                      aksOutboundRule  All         Succeeded            MC_myResourceGroup_myAKSCluster_eastus  
```

In der Beispielausgabe wird der Standardwert für *AllocatedOutboundPorts* und *IdleTimeoutInMinutes* angezeigt. Mit dem Wert „0“ für *AllocatedOutboundPorts* wird die Anzahl von ausgehenden Ports festgelegt, indem die automatische Zuweisung für die Anzahl von ausgehenden Ports basierend auf der Größe des Back-End-Pools verwendet wird. Wenn der Cluster beispielsweise über 50 oder weniger Knoten verfügt, werden für jeden Knoten 1.024 Ports zugeordnet.

Erwägen Sie, die Einstellung von *allocatedOutboundPorts* oder *IdleTimeoutInMinutes* zu ändern, wenn Sie basierend auf der obigen Standardkonfiguration das Auftreten einer SNAT-Auslastung erwarten. Mit jeder zusätzlichen IP-Adresse können 64.000 weitere Ports für die Zuordnung genutzt werden, aber von Azure Load Balancer Standard wird nicht automatisch die Anzahl von Ports pro Knoten erhöht, wenn mehr IP-Adressen hinzugefügt werden. Sie können diese Werte ändern, indem Sie die Parameter *load-balancer-outbound-ports* und *load-balancer-idle-timeout* festlegen. Beispiel:

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --load-balancer-outbound-ports 0 \
    --load-balancer-idle-timeout 30
```

> [!IMPORTANT]
> Sie müssen [Ihr erforderliches Kontingent berechnen][calculate-required-quota], bevor Sie *allocatedOutboundPorts* anpassen, um Konnektivitäts- oder Skalierungsprobleme zu vermeiden. Der Wert, den Sie für *allocatedOutboundPorts* angeben, muss auch ein Vielfaches von „8“ sein.

Sie können beim Erstellen eines Clusters zusätzlich die Parameter *load-balancer-outbound-ports* und *load-balancer-idle-timeout* verwenden, aber Sie müssen auch *load-balancer-managed-outbound-ip-count*, *load-balancer-outbound-ips* oder *load-balancer-outbound-ip-prefixes* angeben.  Beispiel:

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --vm-set-type VirtualMachineScaleSets \
    --node-count 1 \
    --load-balancer-sku standard \
    --generate-ssh-keys \
    --load-balancer-managed-outbound-ip-count 2 \
    --load-balancer-outbound-ports 0 \
    --load-balancer-idle-timeout 30
```

Wenn Sie die Standardwerte der Parameter *load-balancer-outbound-ports* und *load-balancer-idle-timeout* ändern, wirkt sich dies auf das Verhalten des Load Balancer-Profils aus. Dies hat Auswirkungen auf den gesamten Cluster.

### <a name="required-quota-for-customizing-allocatedoutboundports"></a>Erforderliches Kontingent für die Anpassung von „allocatedOutboundPorts“
Sie müssen basierend auf der Anzahl Ihrer Knoten-VMs und den gewünschten zugeordneten ausgehenden Ports über ausreichend IP-Kapazität in ausgehender Richtung verfügen. Verwenden Sie die folgende Formel, um sicherzustellen, dass genügend IP-Kapazität in ausgehender Richtung vorhanden ist: 
 
*outboundIPs* \* 64.000 \> *nodeVMs* \* *desiredAllocatedOutboundPorts*.
 
Wenn Sie beispielsweise drei *nodeVMs* und 50.000 *desiredAllocatedOutboundPorts* verwenden, benötigen Sie mindestens drei *outboundIPs*. Wir empfehlen Ihnen, über Ihren Bedarf hinaus weitere IP-Kapazität in ausgehender Richtung bereitzustellen. Darüber hinaus müssen Sie die automatische Clusterskalierung und die Möglichkeit von Knotenpoolupgrades berücksichtigen, wenn Sie die IP-Kapazität in ausgehender Richtung berechnen. Überprüfen Sie für die automatische Clusterskalierung die aktuelle und die maximale Knotenanzahl, und verwenden Sie den höheren Wert. Berücksichtigen Sie für Upgrades eine zusätzliche Knoten-VM für jeden Knotenpool, für den Upgrades zulässig sind.
 
Wenn Sie *IdleTimeoutInMinutes* auf einen anderen Wert als den Standardwert von 30 Minuten festlegen, sollten Sie berücksichtigen, wie lange Sie für Ihre Workloads eine ausgehende Verbindung benötigen. Berücksichtigen Sie auch, dass der Standardtimeoutwert für einen Load Balancer mit der SKU *Standard*, der außerhalb von AKS verwendet wird, 4 Minuten beträgt. Ein *IdleTimeoutInMinutes*-Wert, der Ihre spezifische AKS-Workload genauer widerspiegelt, kann zu einer Verringerung der SNAT-Auslastung beitragen. Hierzu kann es kommen, wenn nicht mehr verwendete Verbindungen vorhanden sind.

## <a name="restrict-access-to-specific-ip-ranges"></a>Beschränken des Zugriffs auf bestimmte IP-Adressbereiche

Die Netzwerksicherheitsgruppe (NSG), die dem virtuellen Netzwerk für den Lastenausgleich zugeordnet ist, verfügt standardmäßig über eine Regel, die den gesamten eingehenden externen Datenverkehr zulässt. Sie können diese Regel ändern, um nur bestimmte IP-Adressbereiche für eingehenden Datenverkehr zuzulassen. Im folgenden Manifest wird *loadBalancerSourceRanges* verwendet, um einen neuen IP-Adressbereich für eingehenden externen Datenverkehr anzugeben:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-vote-front
  loadBalancerSourceRanges:
  - MY_EXTERNAL_IP_RANGE
```

Im obigen Beispiel wird die Regel so angepasst, dass nur eingehender externer Datenverkehr aus dem Bereich *MY_EXTERNAL_IP_RANGE* zugelassen wird. Weitere Informationen zur Verwendung dieser Methode zum Einschränken des Zugriffs auf den Lastenausgleichsdienst finden Sie in der [Kubernetes-Dokumentation][kubernetes-cloud-provider-firewall].

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Kubernetes-Diensten finden Sie in der entsprechenden [Dokumentation][kubernetes-services].

<!-- LINKS - External -->
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubernetes-cloud-provider-firewall]: https://kubernetes.io/docs/tasks/access-application-cluster/configure-cloud-provider-firewall/#restrict-access-for-loadbalancer-service
[kubectl-delete]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#delete
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubernetes-services]: https://kubernetes.io/docs/concepts/services-networking/service/
[aks-engine]: https://github.com/Azure/aks-engine

<!-- LINKS - Internal -->
[advanced-networking]: configure-azure-cni.md
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[aks-sp]: kubernetes-service-principal.md#delegate-access-to-other-azure-resources
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-aks-install-cli]: /cli/azure/aks?view=azure-cli-latest#az-aks-install-cli
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-group-create]: /cli/azure/group#az-group-create
[az-provider-register]: /cli/azure/provider#az-provider-register
[az-network-lb-outbound-rule-list]: /cli/azure/network/lb/outbound-rule?view=azure-cli-latest#az-network-lb-outbound-rule-list
[az-network-public-ip-show]: /cli/azure/network/public-ip?view=azure-cli-latest#az-network-public-ip-show
[az-network-public-ip-prefix-show]: /cli/azure/network/public-ip/prefix?view=azure-cli-latest#az-network-public-ip-prefix-show
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[azure-lb]: ../load-balancer/load-balancer-overview.md
[azure-lb-comparison]: ../load-balancer/skus.md
[azure-lb-outbound-rules]: ../load-balancer/load-balancer-outbound-rules-overview.md#snatports
[azure-lb-outbound-connections]: ../load-balancer/load-balancer-outbound-connections.md#snat
[azure-lb-outbound-preallocatedports]: ../load-balancer/load-balancer-outbound-connections.md#preallocatedports
[azure-lb-outbound-rules-overview]: ../load-balancer/load-balancer-outbound-rules-overview.md
[install-azure-cli]: /cli/azure/install-azure-cli
[internal-lb-yaml]: internal-lb.md#create-an-internal-load-balancer
[kubernetes-concepts]: concepts-clusters-workloads.md
[use-kubenet]: configure-kubenet.md
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[calculate-required-quota]: #required-quota-for-customizing-allocatedoutboundports
