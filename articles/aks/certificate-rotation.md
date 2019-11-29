---
title: Rotieren von Zertifikaten in Azure Kubernetes Service (AKS)
description: Erfahren Sie, wie Sie Ihre Zertifikate in einem Azure Kubernetes Service-Cluster (AKS) rotieren.
services: container-service
author: zr-msft
ms.service: container-service
ms.topic: article
ms.date: 11/15/2019
ms.author: zarhoads
ms.openlocfilehash: 00d8546cb20d12c5f1a94bdcababa04a77c73133
ms.sourcegitcommit: 2d3740e2670ff193f3e031c1e22dcd9e072d3ad9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2019
ms.locfileid: "74134218"
---
# <a name="rotate-certificates-in-azure-kubernetes-service-aks"></a>Rotieren von Zertifikaten in Azure Kubernetes Service (AKS)

Azure Kubernetes Service (AKS) verwendet Zertifikate für die Authentifizierung bei vielen seiner Komponenten. Sie müssen diese Zertifikate in regelmäßigen Abständen aus Sicherheits- oder Richtliniengründen rotieren. Beispielsweise kann es eine Richtlinie bei Ihnen geben, dass Sie alle Ihre Zertifikate alle 90 Tage rotieren müssen.

In diesem Artikel wird beschrieben, wie Sie die Zertifikate in Ihrem AKS-Cluster rotieren.

## <a name="before-you-begin"></a>Voraussetzungen

Der Artikel setzt voraus, dass Sie mindestens Version 2.0.76 der Azure-Befehlszeilenschnittstelle (Azure CLI) ausführen. Führen Sie `az --version` aus, um die Version zu finden. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sei bei Bedarf unter [Installieren der Azure CLI][azure-cli-install].


### <a name="install-aks-preview-cli-extension"></a>Installieren der CLI-Erweiterung „aks-preview“

Um diese Funktion zu verwenden, benötigen Sie Version 0.4.21 oder höher der CLI-Erweiterung *aks-preview*. Installieren Sie die Azure CLI-Erweiterung *aks-preview* mit dem Befehl [az extension add][az-extension-add], und suchen Sie dann mit dem Befehl [az extension update][az-extension-update] nach verfügbaren Updates:

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

## <a name="aks-certificates-certificate-authorities-and-service-accounts"></a>AKS-Zertifikate, Zertifizierungsstellen und Dienstkonten

AKS generiert und verwendet die folgenden Zertifikate, Zertifizierungsstellen und Dienstkonten:

* Der AKS-API-Server erstellt eine Zertifizierungsstelle (CA), die als Clusterzertifizierungsstelle („Cluster CA“) bezeichnet wird.
* Der API-Server verfügt über eine Clusterzertifizierungsstelle, die Zertifikate für die unidirektionale Kommunikation vom API-Server zu kubelets signiert.
* Jedes kubelet erstellt außerdem eine Zertifikatsignierungsanforderung (Certificate Signing Request, CSR), die von der Clusterzertifizierungsstelle signiert wird, für die Kommunikation vom kubelet zum API-Server.
* Der etcd-Schlüsselwertspeicher verfügt über ein Zertifikat, das von der Clusterzertifizierungsstelle für die Kommunikation von etcd zum API-Server signiert wurde.
* Der etcd-Schlüsselwertspeicher erstellt eine Zertifizierungsstelle, die Zertifikate signiert, um die Datenreplikation zwischen etcd-Replikaten im AKS-Cluster zu authentifizieren und zu autorisieren.
* Der API-Aggregator verwendet die Clusterzertifizierungsstelle zum Ausstellen von Zertifikaten für die Kommunikation mit anderen APIs, z. B. Open Service Broker für Azure. Der API-Aggregator kann auch über eine eigene Zertifizierungsstelle zum Ausstellen dieser Zertifikate verfügen, aber zurzeit verwendet er die Clusterzertifizierungsstelle.
* Jeder Knoten verwendet ein Dienstkontotoken (SA-Token), das von der Clusterzertifizierungsstelle signiert wurde.
* Der `kubectl`-Client verfügt über ein Zertifikat für die Kommunikation mit dem AKS-Cluster.

> [!NOTE]
> AKS-Cluster, die vor März 2019 erstellt wurden, besitzen Zertifikate, die nach zwei Jahren ablaufen. Alle Cluster, die nach März 2019 erstellt wurden, oder alle Cluster, deren Zertifikate rotiert werden, besitzen Zertifikate, die nach 30 Jahren ablaufen.

## <a name="rotate-your-cluster-certificates"></a>Rotieren Ihrer Clusterzertifikate

> [!WARNING]
> Das Rotieren Ihrer Zertifikate mithilfe von `az aks rotate-certs` kann zu einer Ausfallzeit von bis zu 30 Minuten für Ihren AKS-Cluster führen.

Verwenden Sie [az aks get-credentials][az-aks-get-credentials], um sich bei Ihrem AKS-Cluster anzumelden. Mit diesem Befehl wird auch das `kubectl`-Clientzertifikat auf Ihren lokalen Computer heruntergeladen und konfiguriert.

```console
az aks get-credentials -g $RESOURCE_GROUP_NAME -n $CLUSTER_NAME
```

Verwenden Sie `az aks rotate-certs`, um alle Zertifikate, Zertifizierungsstellen und SAs auf Ihrem Cluster zu rotieren.

```console
az aks rotate-certs -g $RESOURCE_GROUP_NAME -n $CLUSTER_NAME
```

> [!IMPORTANT]
> Der `az aks rotate-certs`-Vorgang kann bis zu 30 Minuten dauern. Wenn der Befehl vor dem Abschluss fehlschlägt, überprüfen Sie mithilfe von `az aks show`, ob der Status des Clusters *Zertifikat wird rotiert* lautet. Wenn sich der Cluster in einem fehlerhaften Zustand befindet, führen Sie `az aks rotate-certs` noch mal aus, um die Zertifikate erneut zu rotieren.

Vergewissern Sie sich, dass die alten Zertifikate nicht mehr gültig sind, indem Sie einen `kubectl`-Befehl ausführen. Da Sie die von `kubectl` verwendeten Zertifikate nicht aktualisiert haben, wird eine Fehlermeldung angezeigt.  Beispiel:

```console
$ kubectl get no
Unable to connect to the server: x509: certificate signed by unknown authority (possibly because of "crypto/rsa: verification error" while trying to verify candidate authority certificate "ca")
```

Aktualisieren Sie das von `kubectl` verwendete Zertifikat, indem Sie `az aks get-credentials` ausführen.

```console
az aks get-credentials -g $RESOURCE_GROUP_NAME -n $CLUSTER_NAME --overwrite-existing
```

Überprüfen Sie, ob die Zertifikate aktualisiert wurden, indem Sie einen `kubectl` Befehl ausführen, der nun erfolgreich sein wird. Beispiel:

```console
kubectl get no
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wurde gezeigt, wie Sie die Zertifikate, Zertifizierungsstellen und SAs Ihres Clusters automatisch rotieren. Weitere Informationen zu bewährte Methoden der AKS-Sicherheit finden Sie unter [Bewährte Methoden für Clustersicherheit und Upgrades in Azure Kubernetes Service (AKS)][aks-best-practices-security-upgrades].


[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[aks-best-practices-security-upgrades]: operator-best-practices-cluster-security.md
