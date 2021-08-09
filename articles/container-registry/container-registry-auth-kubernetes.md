---
title: Authentifizieren per Azure-Containerregistrierung mithilfe eines Kubernetes-Pullgeheimnis
description: Erfahren Sie, wie Sie durch Erstellen eines Geheimnisses für Pullvorgänge unter Verwendung eines Dienstprinzipals einen Kubernetes-Cluster mit Zugriff auf Images in Ihrer Azure-Containerregistrierung bereitstellen.
ms.topic: article
author: karolz-ms
ms.author: karolz
ms.reviewer: danlep
ms.date: 06/02/2021
ms.openlocfilehash: 149035de0fc84c75cdcaa73c91d6cd5379c53498
ms.sourcegitcommit: 070122ad3aba7c602bf004fbcf1c70419b48f29e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2021
ms.locfileid: "111439618"
---
# <a name="pull-images-from-an-azure-container-registry-to-a-kubernetes-cluster-using-a-pull-secret"></a>Pullen von Images aus einer Azure-Containerregistrierung in einen Kubernetes-Cluster mithilfe eines Pullgeheimnisses

Sie können eine Azure-Containerregistrierung als Quelle von Containerimages mit einem beliebigen Kubernetes-Cluster verwenden, einschließlich „lokaler“ Kubernetes-Cluster wie [minikube](https://minikube.sigs.k8s.io/) und [kind](https://kind.sigs.k8s.io/). Dieser Artikel beschreibt, wie Sie ein Kubernetes-Pullgeheimnis mit Anmeldeinformationen für eine Azure-Containerregistrierung erstellen. Mit dem Geheimnis pullen Sie dann Images aus einer Azure-Containerregistrierung in einer Pod-Bereitstellung.

In diesem Beispiel wird ein Pullgeheimnis mithilfe Azure Active Directory [Dienstprinzipal-Anmeldeinformationen](container-registry-auth-service-principal.md) erstellt. Sie können ein Pullgeheimnis auch mit anderen Azure Container Registry-Anmeldeinformationen konfigurieren, z. B. einem [repositoryspezifischen Zugriffstoken.](container-registry-repository-scoped-permissions.md)

> [!NOTE]
> Pullgeheimnisse werden zwar häufig verwendet, verursachen aber zusätzlichen Verwaltungsaufwand. Wenn Sie [Azure Kubernetes Service](../aks/intro-kubernetes.md) verwenden, empfehlen wir [andere Optionen](authenticate-kubernetes-options.md), z. B. die Verwendung der verwalteten Identität oder des Dienstprinzipals des Clusters, um das Image ohne zusätzliche `imagePullSecrets`-Einstellung auf jedem Pod sicher zu pullen.

## <a name="prerequisites"></a>Voraussetzungen

In diesem Artikel wird davon ausgegangen, dass Sie bereits eine private Azure-Containerregistrierung erstellt haben. Außerdem benötigen Sie einen Kubernetes-Cluster, auf den über das Befehlszeilentool `kubectl` zugegriffen werden kann.

[!INCLUDE [container-registry-service-principal](../../includes/container-registry-service-principal.md)]

Wenn Sie das Kennwort des Dienstprinzipals nicht gespeichert oder es vergessen haben, können Sie es mit dem Befehl [az ad sp credential reset][az-ad-sp-credential-reset] zurücksetzen:

```azurecli
az ad sp credential reset  --name http://<service-principal-name> --query password --output tsv
```

Mit diesem Befehl wird ein neues gültiges Kennwort für den Dienstprinzipal zurückgegeben.

## <a name="create-an-image-pull-secret"></a>Erstellen eines Geheimnisses für Imagepullvorgänge

In Kubernetes wird ein *Geheimnis für Imagepullvorgänge* verwendet, um Informationen zu speichern, die für die Authentifizierung bei Ihrer Registrierung benötigt werden. Zum Erstellen des Geheimnisses für Pullvorgänge für eine Azure-Containerregistrierung geben Sie die ID und das Kennwort des Dienstprinzipals sowie die Registrierungs-URL an. 

Erstellen Sie mit dem folgenden `kubectl`-Befehl ein Geheimnis für Imagepullvorgänge:

```console
kubectl create secret docker-registry <secret-name> \
    --namespace <namespace> \
    --docker-server=<container-registry-name>.azurecr.io \
    --docker-username=<service-principal-ID> \
    --docker-password=<service-principal-password>
```
Dabei gilt:

| Wert | BESCHREIBUNG |
| :--- | :--- |
| `secret-name` | Name des Geheimnisses für Imagepullvorgänge, z. B. *acr-secret* |
| `namespace` | Kubernetes-Namespace, in dem das Geheimnis eingefügt werden soll. <br/> Nur erforderlich, wenn das Geheimnis in einem anderen Namespace als dem Standardnamespace eingefügt werden soll. |
| `container-registry-name` | Name Ihrer Azure-Containerregistrierung, z. B. *myregistry*<br/><br/>`--docker-server` ist der vollqualifizierte Name des Registrierungsanmeldeservers.  |
| `service-principal-ID` | ID des Dienstprinzipals, der in Kubernetes für den Zugriff auf die Registrierung verwendet wird |
| `service-principal-password` | Kennwort des Dienstprinzipals |

## <a name="use-the-image-pull-secret"></a>Verwenden des Geheimnisses für Imagepullvorgänge

Nach dem Erstellen des Geheimnisses für Imagepullvorgänge können Sie es verwenden, um Kubernetes-Pods und Kubernetes-Bereitstellungen zu erstellen. Geben Sie den Namen des Geheimnisses unter `imagePullSecrets` in der Bereitstellungsdatei an. Beispiel:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-awesome-app-pod
  namespace: awesomeapps
spec:
  containers:
    - name: main-app-container
      image: myregistry.azurecr.io/my-awesome-app:v1
      imagePullPolicy: IfNotPresent
  imagePullSecrets:
    - name: acr-secret
```

In diesem Beispiel ist `my-awesome-app:v1` der Name des aus der Azure-Containerregistrierung zu pullenden Images und `acr-secret` der Name des für den Zugriff auf die Registrierung erstellten Geheimnisses für Pullvorgänge. Wenn Sie den Pod bereitstellen, pullt Kubernetes das Image automatisch aus der Registrierung, wenn es nicht bereits im Cluster vorhanden ist.


## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum Arbeiten mit Dienstprinzipalen und Azure Container Registry finden Sie unter [Azure Container Registry-Authentifizierung mit Dienstprinzipalen](container-registry-auth-service-principal.md).
* Weitere Informationen zu Geheimnissen für Imagepullvorgänge finden Sie in der [Kubernetes-Dokumentation](https://kubernetes.io/docs/concepts/containers/images/#specifying-imagepullsecrets-on-a-pod).


<!-- IMAGES -->

<!-- LINKS - External -->
[acr-scripts-cli]: https://github.com/Azure/azure-docs-cli-python-samples/tree/master/container-registry
[acr-scripts-psh]: https://github.com/Azure/azure-docs-powershell-samples/tree/master/container-registry

<!-- LINKS - Internal -->
[az-ad-sp-credential-reset]: /cli/azure/ad/sp/credential#az_ad_sp_credential_reset