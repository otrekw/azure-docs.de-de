---
title: Entwickeln unter Azure Kubernetes Service (AKS) mit Helm
description: Verwenden Sie Helm mit AKS und Azure Container Registry, um Anwendungscontainer in einem Cluster zu packen und auszuführen.
services: container-service
author: zr-msft
ms.topic: article
ms.date: 03/15/2021
ms.author: zarhoads
ms.openlocfilehash: 4f5232920853908aa5ad714313ead201494caa0d
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "103493075"
---
# <a name="quickstart-develop-on-azure-kubernetes-service-aks-with-helm"></a>Schnellstart: Entwickeln unter Azure Kubernetes Service (AKS) mit Helm

[Helm][helm] ist ein Open Source-Verpackungstool, das Ihnen dabei hilft, Kubernetes-Anwendungen zu installieren und ihren Lebenszyklus zu verwalten. Ähnlich wie Linux-Paket-Manager (z. B. *APT* und *Yum*) wird Helm zur Verwaltung von Kubernetes-Charts verwendet, bei denen es sich um Pakete aus vorkonfigurierten Kubernetes-Ressourcen handelt.

In diesem Schnellstart verwenden Sie Helm, um eine Anwendung in AKS zu packen und auszuführen. Weitere Informationen zum Installieren einer vorhandenen Anwendung mit Helm finden Sie in der Schrittanleitung [Installieren vorhandener Anwendungen mit Helm in AKS][helm-existing].

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Falls Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen.
* Die [Azure CLI](/cli/azure/install-azure-cli) muss installiert sein.
* [Helm v3 muss installiert sein][helm-install].

## <a name="create-an-azure-container-registry"></a>Erstellen einer Azure-Containerregistrierung
Damit Sie Helm zum Ausführen Ihrer Anwendung in Ihrem AKS-Cluster verwenden können, müssen Sie Ihre Containerimages in einer Azure Container Registry-Instanz (ACR) speichern. Geben Sie Ihren eigenen, in Azure eindeutigen Registrierungsnamen an, der 5–50 alphanumerische Zeichen enthalten darf. Die *Basic*-SKU ist ein kostenoptimierter Einstiegspunkt für Entwicklungszwecke, der ein ausgewogenes Verhältnis von Speicher und Durchsatz bietet.

Im nachfolgenden Beispiel wird [az acr create][az-acr-create] verwendet, um mithilfe der *Basic*-SKU eine ACR-Instanz mit dem Namen *MyHelmACR* in der Ressourcengruppe *MyResourceGroup* zu erstellen.

```azurecli
az group create --name MyResourceGroup --location eastus
az acr create --resource-group MyResourceGroup --name MyHelmACR --sku Basic
```

Die Ausgabe ähnelt folgendem Beispiel. Notieren Sie sich den Wert von *loginServer* für Ihre ACR-Instanz, da Sie diesen in einem späteren Schritt benötigen. Im nachfolgenden Beispiel ist *myhelmacr.azurecr.io* der *loginServer*-Wert für *MyHelmACR*.

```console
{
  "adminUserEnabled": false,
  "creationDate": "2019-06-11T13:35:17.998425+00:00",
  "id": "/subscriptions/<ID>/resourceGroups/MyResourceGroup/providers/Microsoft.ContainerRegistry/registries/MyHelmACR",
  "location": "eastus",
  "loginServer": "myhelmacr.azurecr.io",
  "name": "MyHelmACR",
  "networkRuleSet": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "MyResourceGroup",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "status": null,
  "storageAccount": null,
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

## <a name="create-an-aks-cluster"></a>Erstellen eines AKS-Clusters

Ihr neuer AKS-Cluster benötigt Zugriff auf Ihre ACR-Instanz, um die Containerimages pullen und ausführen zu können. Verwenden Sie den folgenden Befehl, um folgende Aktionen auszuführen:
* Erstellen Sie einen AKS-Cluster namens *MyAKS*, und fügen Sie *MyHelmACR* an.
* Gewähren Sie dem *MyAKS*-Cluster Zugriff auf Ihre *MyHelmACR*-ACR.


```azurecli
az aks create -g MyResourceGroup -n MyAKS --location eastus  --attach-acr MyHelmACR --generate-ssh-keys
```

## <a name="connect-to-your-aks-cluster"></a>Herstellen einer Verbindung mit dem AKS-Cluster

Verwenden Sie zum lokalen Verbinden eines Kubernetes-Clusters den Kubernetes-Befehlszeilenclient [kubectl][kubectl]. `kubectl` ist bei Verwendung von Azure Cloud Shell bereits installiert. 

1. Verwenden Sie für die lokale Installation von `kubectl` den Befehl `az aks install-cli`:

    ```azurecli
    az aks install-cli
    ```

2. Mit dem Befehl `az aks get-credentials` können Sie `kubectl` für die Verbindungsherstellung mit Ihrem Kubernetes-Cluster konfigurieren. Mit dem Befehl im folgenden Beispiel werden Anmeldeinformationen für den AKS-Cluster namens *MyAKS* in der Ressourcengruppe *MyResourceGroup* abgerufen:  

    ```azurecli
    az aks get-credentials --resource-group MyResourceGroup --name MyAKS
    ```

## <a name="download-the-sample-application"></a>Herunterladen der Beispielanwendung

In diesem Schnellstart wird [eine Node.js-Beispielanwendung aus dem Azure Dev Spaces-Beispielrepository][example-nodejs] verwendet. Klonen Sie die Anwendung von GitHub, und navigieren Sie zum Verzeichnis `dev-spaces/samples/nodejs/getting-started/webfrontend`.

```console
git clone https://github.com/Azure/dev-spaces
cd dev-spaces/samples/nodejs/getting-started/webfrontend
```

## <a name="create-a-dockerfile"></a>Erstellen eines Dockerfile

Verwenden Sie zum Erstellen einer neuen *Dockerfile*-Datei die folgenden Befehle:

```dockerfile
FROM node:latest

WORKDIR /webfrontend

COPY package.json ./

RUN npm install

COPY . .

EXPOSE 80
CMD ["node","server.js"]
```

## <a name="build-and-push-the-sample-application-to-the-acr"></a>Erstellen und Pushen der Beispielanwendung in die ACR

Verwenden Sie den Befehl [az acr build][az-acr-build], um ein Image mithilfe des vorherigen Dockerfile zu erstellen und in die Registrierung zu pushen. Der Punkt (`.`) am Ende des Befehls legt den Speicherort des Dockerfile fest (in diesem Fall: das aktuelle Verzeichnis).

```azurecli
az acr build --image webfrontend:v1 \
  --registry MyHelmACR \
  --file Dockerfile .
```

## <a name="create-your-helm-chart"></a>Erstellen Ihres Helm-Diagramms

Generieren Sie Ihr Helm-Diagramm mit dem Befehl `helm create`.

```console
helm create webfrontend
```

Aktualisieren Sie *webfrontend/values.yaml*:
* Ersetzen Sie den Anmeldeserver („loginServer“) durch den Ihrer Registrierung, den Sie in einem früheren Schritt notiert haben, z. B. *myhelmacr.azurecr.io*.
* Ändern Sie `image.repository` in `<loginServer>/webfrontend`.
* Ändern Sie `service.type` in `LoadBalancer`.

Beispiel:

```yml
# Default values for webfrontend.
# This is a YAML-formatted file.
# Declare variables to be passed into your templates.

replicaCount: 1

image:
  repository: myhelmacr.azurecr.io/webfrontend
  pullPolicy: IfNotPresent
...
service:
  type: LoadBalancer
  port: 80
...
```

Aktualisieren Sie in *webfrontend/Chart.yaml* `appVersion` auf `v1`. Beispiel:

```yml
apiVersion: v2
name: webfrontend
...
# This is the version number of the application being deployed. This version number should be
# incremented each time you make changes to the application.
appVersion: v1
```

## <a name="run-your-helm-chart"></a>Ausführen Ihres Helm-Diagramms

Verwenden Sie den Befehl `helm install`, um Ihre Anwendung mithilfe Ihres Helm-Charts zu installieren.

```console
helm install webfrontend webfrontend/
```

Es kann einige Minuten dauern, bis der Dienst eine öffentliche IP-Adresse zurückgibt. Verwenden Sie zum Überwachen des Fortschritts den Befehl `kubectl get service` mit dem Argument `--watch`:

```console
$ kubectl get service --watch

NAME                TYPE          CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
webfrontend         LoadBalancer  10.0.141.72   <pending>     80:32150/TCP   2m
...
webfrontend         LoadBalancer  10.0.141.72   <EXTERNAL-IP> 80:32150/TCP   7m
```

Navigieren Sie in einem Browser mithilfe der `<EXTERNAL-IP>` zum Lastenausgleichsmodul Ihrer Anwendung, um die Beispielanwendung anzuzeigen.

## <a name="delete-the-cluster"></a>Löschen des Clusters

Entfernen Sie mit dem Befehl [az group delete][az-group-delete] die Ressourcengruppe, den AKS-Cluster, die Containerregistrierung, die darin gespeicherten Containerimages sowie alle weiteren zugehörigen Ressourcen.

```azurecli-interactive
az group delete --name MyResourceGroup --yes --no-wait
```

> [!NOTE]
> Wenn Sie den Cluster löschen, wird der vom AKS-Cluster verwendete Azure Active Directory-Dienstprinzipal nicht entfernt. Schritte zum Entfernen des Dienstprinzipals finden Sie unter den [Überlegungen zum AKS-Dienstprinzipal und dessen Löschung][sp-delete].
> 
> Wenn Sie eine verwaltete Identität verwendet haben, wird die Identität von der Plattform verwaltet und muss nicht entfernt werden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Verwendung von Helm finden Sie in der Helm-Dokumentation.

> [!div class="nextstepaction"]
> [Helm-Dokumentation][helm-documentation]

[az-acr-create]: /cli/azure/acr#az-acr-create
[az-acr-build]: /cli/azure/acr#az-acr-build
[az-group-delete]: /cli/azure/group#az-group-delete
[az aks get-credentials]: /cli/azure/aks#az-aks-get-credentials
[az aks install-cli]: /cli/azure/aks#az-aks-install-cli
[example-nodejs]: https://github.com/Azure/dev-spaces/tree/master/samples/nodejs/getting-started/webfrontend
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[helm]: https://helm.sh/
[helm-documentation]: https://helm.sh/docs/
[helm-existing]: kubernetes-helm.md
[helm-install]: https://helm.sh/docs/intro/install/
[sp-delete]: kubernetes-service-principal.md#additional-considerations
