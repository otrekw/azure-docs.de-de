---
title: Durchführen eines Upgrades für einen Azure Kubernetes Service-Cluster (AKS)
description: Erfahren Sie, wie Sie das Upgrade eines Azure Kubernetes Service-Clusters (AKS) vornehmen, um die neuesten Funktionen und Sicherheitsupdates zu erhalten.
services: container-service
ms.topic: article
ms.date: 05/28/2020
ms.openlocfilehash: da46c44dc9cc16dfa44aacb15b35b652c0c912a9
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87050614"
---
# <a name="upgrade-an-azure-kubernetes-service-aks-cluster"></a>Durchführen eines Upgrades für einen Azure Kubernetes Service-Cluster (AKS)

Im Verlauf des Lebenszyklus eines AKS-Clusters müssen Sie häufig ein Upgrade auf die neueste Kubernetes-Version vornehmen. Es ist wichtig, jeweils die aktuelle Kubernetes-Sicherheitsversion anzuwenden oder bei einem Upgrade die neuesten Features zu erhalten. In diesem Artikel wird veranschaulicht, wie Sie die Masterkomponenten oder einen einzelnen Standardknotenpool in einem AKS-Cluster aktualisieren.

Informationen zu AKS-Clustern, für die mehrere Knotenpools oder Windows Server-Knoten verwendet werden, finden Sie unter [Durchführen eines Upgrades für einen Knotenpool in AKS][nodepool-upgrade].

## <a name="before-you-begin"></a>Voraussetzungen

Der Artikel setzt voraus, dass Sie mindestens Version 2.0.65 der Azure-Befehlszeilenschnittstelle (Azure CLI) ausführen. Führen Sie `az --version` aus, um die Version zu ermitteln. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sie bei Bedarf unter [Installieren der Azure CLI][azure-cli-install].

> [!WARNING]
> Durch ein AKS-Clusterupgrade werden Ihre Knoten als nicht planbar markiert und entleert (cordon/drain). Wenn Sie nur über ein geringes Computekontingent verfügen, kann das Upgrade möglicherweise nicht durchgeführt werden. Weitere Informationen finden Sie unter [Anfordern einer Kontingenterhöhung](../azure-portal/supportability/resource-manager-core-quotas-request.md).

## <a name="check-for-available-aks-cluster-upgrades"></a>Suchen nach verfügbaren AKS-Clusterupgrades

Überprüfen Sie mithilfe des Befehls [az aks get-upgrades][az-aks-get-upgrades], welche Kubernetes-Versionen für Ihren Cluster verfügbar sind. Im folgenden Beispiel werden für den Cluster namens *myAKSCluster* in der Ressourcengruppe namens *myResourceGroup* verfügbare Upgrades gesucht:

```azurecli-interactive
az aks get-upgrades --resource-group myResourceGroup --name myAKSCluster --output table
```

> [!NOTE]
> Beim Upgrade eines unterstützten AKS-Clusters können Nebenversionen von Kubernetes nicht übersprungen werden. Beispielsweise sind Upgrades von *1.12.x* -> *1.13.x* oder *1.13.x* -> *1.14.x* zulässig, ein Upgrade von *1.12.x* -> *1.14.x* ist jedoch nicht möglich.
>
> Zum Durchführen eines Upgrades von *1.12.x* -> *1.14.x* müssen Sie zuerst ein Upgrade von *1.12.x* -> *1.13.x* und dann ein Upgrade von *1.13.x* -> *1.14.x* durchführen.
>
> Das Überspringen mehrerer Versionen ist nur möglich, wenn ein Upgrade von einer nicht unterstützten Version auf eine unterstützte Version erfolgt. Beispielsweise kann ein Upgrade von einer nicht unterstützten Version *1.10.x* auf eine unterstützte Version *1.15.x* durchgeführt werden.

Die Ausgabe im folgenden Beispiel zeigt, dass der Cluster auf die Versionen *1.13.9* und *1.13.10* aktualisiert werden kann:

```console
Name     ResourceGroup     MasterVersion    NodePoolVersion    Upgrades
-------  ----------------  ---------------  -----------------  ---------------
default  myResourceGroup   1.12.8           1.12.8             1.13.9, 1.13.10
```
Wenn kein Upgrade verfügbar ist, erhalten Sie Folgendes:
```console
ERROR: Table output unavailable. Use the --query option to specify an appropriate query. Use --debug for more info.
```

## <a name="customize-node-surge-upgrade-preview"></a>Anpassen des Upgrades für Knotenanstiege (Vorschauversion)

> [!Important]
> Knotenanstiege erfordern ein Abonnementkontingent für die angeforderte maximale Anstiegsanzahl für jeden Upgradevorgang. Beispielsweise umfasst ein Cluster mit 5 Knotenpools, von denen jeder über 4 Knoten verfügt, insgesamt 20 Knoten. Wenn für jeden Knotenpool ein maximaler Anstiegswert von 50 % gilt, sind zusätzliche Compute- und IP-Kontingente von 10 Knoten (2 Knoten * 5 Pools) erforderlich, um das Upgrade abzuschließen.
>
> Wenn Sie Azure CNI verwenden, stellen Sie sicher, dass im Subnetz IPs verfügbar sind und die [Anforderungen von Azure CNI an IPs](configure-azure-cni.md) erfüllt werden.

Standardmäßig konfiguriert AKS Upgrades für einen Anstieg mit einem zusätzlichen Knoten. Ein Standardwert von „1“ für die Einstellung des maximalen Anstiegs ermöglicht AKS das Minimieren von Workloadunterbrechungen, indem vor dem Absperren/Ausgleichen vorhandener Anwendungen ein zusätzlicher Knoten erstellt wird, um einen Knoten einer älterer Version zu ersetzen. Der maximale Anstiegswert kann pro Knotenpool angepasst werden, um einen Kompromiss zwischen Upgradegeschwindigkeit und Upgradeunterbrechung zu ermöglichen. Wenn Sie den maximalen Anstiegswert erhöhen, wird der Upgradevorgang schneller abgeschlossen, aber das Festlegen eines hohen Werts für den maximalen Anstieg kann zu Unterbrechungen während des Upgradevorgangs führen. 

Beispielsweise ermöglicht ein maximaler Anstiegswert von 100 % den schnellstmöglichen Upgradevorgang (wobei die Knotenanzahl verdoppelt wird), führt aber auch dazu, dass alle Knoten im Knotenpool gleichzeitig ausgeglichen werden. Möglicherweise möchten Sie (z. B. für Testumgebungen) einen höheren Wert verwenden. Für Produktionsknotenpools wird ein maximaler Anstiegswert von 33 % empfohlen.

Für den maximalen Anstieg akzeptiert AKS sowohl ganzzahlige Werte als auch einen prozentualen Wert. Eine ganze Zahl wie z. B. „5“ gibt für den Anstieg fünf zusätzliche Knoten an. Der Wert „50 %“ gibt einen Anstiegswert der Hälfte der aktuellen Knotenanzahl im Pool an. Die Prozentwerte für den maximalen Anstieg können mindestens 1 % und maximal 100 % lauten. Ein Prozentwert wird auf die nächste Knotenanzahl aufgerundet. Wenn der maximale Anstiegswert zum Zeitpunkt des Upgrades niedriger ist als die aktuelle Knotenanzahl, wird die aktuelle Knotenanzahl für den maximalen Anstiegswert verwendet.

Während eines Upgrades kann der maximale Anstiegswert mindestens „1“ betragen und maximal der Anzahl von Knoten in Ihrem Knotenpool entsprechen. Sie können größere Werte festlegen, aber die maximale Anzahl von Knoten, die für den maximalen Anstieg verwendet wird, ist nicht größer als die Anzahl der Knoten im Pool zum Zeitpunkt des Upgrades.

### <a name="set-up-the-preview-feature-for-customizing-node-surge-upgrade"></a>Einrichten der Vorschaufunktion zum Anpassen des Upgrades für Knotenanstiege

```azurecli-interactive
# register the preview feature
az feature register --namespace "Microsoft.ContainerService" --name "MaxSurgePreview"
```

Die Registrierung dauert einige Minuten. Verwenden Sie den folgenden Befehl, um zu überprüfen, ob die Funktion registriert ist:

```azurecli-interactive
# Verify the feature is registered:
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/MaxSurgePreview')].{Name:name,State:properties.state}"
```

In der Vorschauphase benötigen Sie die CLI-Erweiterung *aks-preview*, um den maximalen Anstieg verwenden zu können. Verwenden Sie den Befehl [az extension add][az-extension-add], und suchen Sie dann mit dem Befehl [az extension update][az-extension-update] nach verfügbaren Updates:

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

> [!Important]
> Die Einstellung des maximalen Anstiegs für einen Knotenpools ist permanent.  Diese Einstellung wird bei nachfolgenden Kubernetes-Upgrades oder bei Knotenversionsupgrades verwendet. Sie können den maximalen Anstiegswert für Ihre Knotenpools jederzeit ändern. Für Produktionsknotenpools wird ein maximaler Anstiegswert von 33 % empfohlen.

Verwenden Sie die folgenden Befehle, um Werte für den maximalen Anstieg für neue oder vorhandene Knotenpools festzulegen.

```azurecli-interactive
# Set max surge for a new node pool
az aks nodepool add -n mynodepool -g MyResourceGroup --cluster-name MyManagedCluster --max-surge 33%
```

```azurecli-interactive
# Update max surge for an existing node pool 
az aks nodepool update -n mynodepool -g MyResourceGroup --cluster-name MyManagedCluster --max-surge 5
```

## <a name="upgrade-an-aks-cluster"></a>Aktualisieren eines AKS-Clusters

Wenn Sie die Liste der verfügbaren Versionen für Ihren AKS-Cluster angezeigt haben, verwenden Sie den Befehl [az aks upgrade][az-aks-upgrade], um den Cluster zu aktualisieren. Während des Upgradeprozesses fügt AKS dem Cluster einen neuen Knoten hinzu, auf dem die angegebene Kubernetes-Version ausgeführt wird. Danach sorgt AKS dafür, dass einer der alten Knoten [als unplanbar markiert und entleert wird][kubernetes-drain] (cordon/drain), um die Beeinträchtigung für ausgeführte Anwendungen möglichst gering zu halten. Wenn für den neuen Knoten die Ausführung von Anwendungspods bestätigt wird, wird der alte Knoten gelöscht. Dieser Prozess wird wiederholt, bis alle Knoten im Cluster aktualisiert wurden.

```azurecli-interactive
az aks upgrade \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --kubernetes-version KUBERNETES_VERSION
```

Die Dauer des Clusterupgrades hängt von der Anzahl der vorhanden Knoten ab und kann einige Minuten in Anspruch nehmen.

> [!NOTE]
> Für den Abschluss von Clusterupgrades besteht eine zulässige Gesamtzeit. Diese Zeit wird als das Produkt aus `10 minutes * total number of nodes in the cluster` berechnet. Beispielsweise müssen Upgradevorgänge in einem Cluster mit 20 Knoten in 200 Minuten erfolgreich ausgeführt werden. Andernfalls löst AKS einen Fehler aus, um einen nicht behebbaren Clusterstatus zu vermeiden. Um bei einem Upgradefehler eine Wiederherstellung auszuführen, wiederholen Sie den Upgradevorgang, nachdem das Timeout aufgetreten ist.

Überprüfen Sie nun mit dem Befehl [az aks show][az-aks-show], ob das Upgrade erfolgreich war:

```azurecli-interactive
az aks show --resource-group myResourceGroup --name myAKSCluster --output table
```

Die Ausgabe im folgenden Beispiel zeigt, dass im Cluster nun Version *1.13.10* ausgeführt wird:

```json
Name          Location    ResourceGroup    KubernetesVersion    ProvisioningState    Fqdn
------------  ----------  ---------------  -------------------  -------------------  ---------------------------------------------------------------
myAKSCluster  eastus      myResourceGroup  1.13.10               Succeeded            myaksclust-myresourcegroup-19da35-90efab95.hcp.eastus.azmk8s.io
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wurde gezeigt, wie Sie einen vorhandenen AKS-Cluster aktualisieren. In verschiedenen Tutorials können Sie mehr über die Bereitstellung und Verwaltung von AKS-Clustern erfahren.

> [!div class="nextstepaction"]
> [AKS-Tutorials][aks-tutorial-prepare-app]

<!-- LINKS - external -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-get-upgrades]: /cli/azure/aks#az-aks-get-upgrades
[az-aks-upgrade]: /cli/azure/aks#az-aks-upgrade
[az-aks-show]: /cli/azure/aks#az-aks-show
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
