---
title: Rotieren von Zertifikaten in Azure Kubernetes Service (AKS)
description: Erfahren Sie, wie Sie Ihre Zertifikate in einem Azure Kubernetes Service-Cluster (AKS) rotieren.
services: container-service
author: zr-msft
ms.topic: article
ms.date: 11/15/2019
ms.author: zarhoads
ms.openlocfilehash: 3c22f63b7085c7ab8d6b54e383528568dc9c12e7
ms.sourcegitcommit: 3c925b84b5144f3be0a9cd3256d0886df9fa9dc0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2020
ms.locfileid: "77917032"
---
# <a name="rotate-certificates-in-azure-kubernetes-service-aks"></a>Rotieren von Zertifikaten in Azure Kubernetes Service (AKS)

Azure Kubernetes Service (AKS) verwendet Zertifikate für die Authentifizierung bei vielen seiner Komponenten. Sie müssen diese Zertifikate in regelmäßigen Abständen aus Sicherheits- oder Richtliniengründen rotieren. Beispielsweise kann es eine Richtlinie bei Ihnen geben, dass Sie alle Ihre Zertifikate alle 90 Tage rotieren müssen.

In diesem Artikel wird beschrieben, wie Sie die Zertifikate in Ihrem AKS-Cluster rotieren.

## <a name="before-you-begin"></a>Voraussetzungen

Der Artikel setzt voraus, dass Sie mindestens Version 2.0.77 der Azure-Befehlszeilenschnittstelle (Azure CLI) ausführen. Führen Sie `az --version` aus, um die Version zu ermitteln. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sie bei Bedarf unter [Installieren der Azure CLI][azure-cli-install].

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
> AKS-Cluster, die vor März 2019 erstellt wurden, besitzen Zertifikate, die nach zwei Jahren ablaufen. Alle Cluster, die nach März 2019 erstellt wurden, oder alle Cluster, deren Zertifikate rotiert werden, besitzen Zertifikate, die nach 30 Jahren ablaufen. Um zu überprüfen, wann Ihr Cluster erstellt wurde, verwenden Sie `kubectl get nodes`, um das *Alter* Ihrer Knotenpools anzuzeigen.
> 
> Darüber hinaus können Sie das Ablaufdatum des Zertifikats Ihres Clusters überprüfen. Beispielsweise zeigt der folgende Befehl die Zertifikatdetails für den Cluster *myAKSCluster* an.
> ```console
> kubectl config view --raw -o jsonpath="{.clusters[?(@.name == 'myAKSCluster')].cluster.certificate-authority-data}" | base64 -d > my-cert.crt
> openssl x509 -in my-cert.crt -text
> ```

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

> [!NOTE]
> Wenn Sie über Dienste verfügen, die auf AKS basieren (z.B. [Azure Dev Spaces][dev-spaces]), müssen Sie möglicherweise auch [die Zertifikate für diese Dienste aktualisieren][dev-spaces-rotate].

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wurde gezeigt, wie Sie die Zertifikate, Zertifizierungsstellen und SAs Ihres Clusters automatisch rotieren. Weitere Informationen zu bewährte Methoden der AKS-Sicherheit finden Sie unter [Bewährte Methoden für Clustersicherheit und Upgrades in Azure Kubernetes Service (AKS)][aks-best-practices-security-upgrades].


[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[aks-best-practices-security-upgrades]: operator-best-practices-cluster-security.md
[dev-spaces]: https://docs.microsoft.com/azure/dev-spaces/
[dev-spaces-rotate]: ../dev-spaces/troubleshooting.md#error-using-dev-spaces-after-rotating-aks-certificates
