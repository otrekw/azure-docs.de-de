---
title: Installieren vorhandener Anwendungen mit Helm in AKS
description: Erfahren Sie, wie Sie das Helm-Paketerstellungstool verwenden, um Container in einem AKS-Cluster (Azure Kubernetes Service) bereitzustellen.
services: container-service
author: zr-msft
ms.topic: article
ms.date: 06/24/2020
ms.author: zarhoads
ms.openlocfilehash: d05d0166724e586fa79e58e2e74fb583b45d0cc6
ms.sourcegitcommit: b33c9ad17598d7e4d66fe11d511daa78b4b8b330
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/25/2020
ms.locfileid: "88852874"
---
# <a name="install-existing-applications-with-helm-in-azure-kubernetes-service-aks"></a>Installieren vorhandener Anwendungen mit Helm in Azure Kubernetes Service (AKS)

[Helm][helm] ist ein Open Source-Verpackungstool, das Ihnen dabei hilft, Kubernetes-Anwendungen zu installieren und ihren Lebenszyklus zu verwalten. Ähnlich wie Linux-Paket-Manager (z.B. *APT* und *Yum*) wird Helm zur Verwaltung von Kubernetes-Diagrammen verwendet, bei denen es sich um Pakete aus vorkonfigurierten Kubernetes-Ressourcen handelt.

Dieser Artikel veranschaulicht die Konfiguration und Verwendung von Helm in einem Kubernetes-Cluster in AKS.

## <a name="before-you-begin"></a>Voraussetzungen

Es wird vorausgesetzt, dass Sie über ein AKS-Cluster verfügen. Wenn Sie einen AKS-Cluster benötigen, erhalten Sie weitere Informationen im AKS-Schnellstart. Verwenden Sie dafür entweder die [Azure CLI][aks-quickstart-cli] oder das [Azure-Portal][aks-quickstart-portal].

Außerdem muss die Helm CLI installiert sein, d. h. der Client, der auf Ihrem Entwicklungssystem ausgeführt wird. Sie ermöglicht es Ihnen, Anwendungen mit Helm zu starten, zu beenden und zu verwalten. Wenn Sie Azure Cloud Shell verwenden, ist die Helm-CLI bereits installiert. Installationsanweisungen für Ihre lokale Plattform finden Sie unter [Installieren von Helm][helm-install].

> [!IMPORTANT]
> Helm ist für die Ausführung auf Linux-Knoten vorgesehen. Wenn sich in Ihrem Cluster Windows Server-Knoten befinden, müssen Sie sicherstellen, dass Helm-Pods nur für die Ausführung auf Linux-Knoten geplant sind. Sie müssen auch sicherstellen, dass alle von Ihnen installierten Helm-Diagramme auch für die Ausführung auf den richtigen Knoten geplant sind. Die Befehle in diesem Artikel verwenden [node-selectors][k8s-node-selector], um sicherzustellen, dass die Pods für die richtigen Knoten geplant sind, aber nicht alle Helm-Charts können einen Knotenselektor verfügbar machen. Sie können auch andere Optionen für Ihren Cluster verwenden, z. B. [Taints][taints].

## <a name="verify-your-version-of-helm"></a>Überprüfen Ihrer Version von Helm

Überprüfen Sie mit dem Befehl `helm version`, ob Helm 3 installiert ist:

```console
helm version
```

Das folgende Beispiel zeigt, dass Helm, Version 3.0.0 installiert ist:

```console
$ helm version

version.BuildInfo{Version:"v3.0.0", GitCommit:"e29ce2a54e96cd02ccfce88bee4f58bb6e2a28b6", GitTreeState:"clean", GoVersion:"go1.13.4"}
```

## <a name="install-an-application-with-helm-v3"></a>Installieren einer Anwendung mit Helm v3

### <a name="add-helm-repositories"></a>Hinzufügen von Helm-Repositorys

Verwenden Sie den Befehl [helm repo][helm-repo-add], um die offiziellen stabilen Helm-Repositorys für Charts sowie *ingress-nginx* hinzuzufügen.

```console
helm repo add stable https://kubernetes-charts.storage.googleapis.com/
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
```

### <a name="find-helm-charts"></a>Suchen von Helm-Diagrammen

Helm-Diagramme dienen zum Bereitstellen von Anwendungen in einem Kubernetes-Cluster. Verwenden Sie zum Suchen nach zuvor erstellten Helm-Charts den Befehl [helm search][helm-search]:

```console
helm search repo stable
```

Die folgende verkürzte Beispielausgabe veranschaulicht einige der verfügbaren Helm-Diagramme:

```console
$ helm search repo stable

NAME                                    CHART VERSION   APP VERSION                     DESCRIPTION                                       
stable/acs-engine-autoscaler            2.2.2           2.1.1                           DEPRECATED Scales worker nodes within agent pools 
stable/aerospike                        0.3.1           v4.5.0.5                        A Helm chart for Aerospike in Kubernetes          
stable/airflow                          4.10.0          1.10.4                          Airflow is a platform to programmatically autho...
stable/ambassador                       4.4.7           0.85.0                          A Helm chart for Datawire Ambassador              
stable/anchore-engine                   1.3.7           0.5.2                           Anchore container analysis and policy evaluatio...
stable/apm-server                       2.1.5           7.0.0                           The server receives data from the Elastic APM a...
stable/ark                              4.2.2           0.10.2                          DEPRECATED A Helm chart for ark                   
stable/artifactory                      7.3.1           6.1.0                           DEPRECATED Universal Repository Manager support...
stable/artifactory-ha                   0.4.1           6.2.0                           DEPRECATED Universal Repository Manager support...
stable/atlantis                         3.8.4           v0.8.2                          A Helm chart for Atlantis https://www.runatlant...
stable/auditbeat                        1.1.0           6.7.0                           A lightweight shipper to audit the activities o...
stable/aws-cluster-autoscaler           0.3.3                                           Scales worker nodes within autoscaling groups.    
stable/aws-iam-authenticator            0.1.1           1.0                             A Helm chart for aws-iam-authenticator            
stable/bitcoind                         0.2.2           0.17.1                          Bitcoin is an innovative payment network and a ...
stable/bookstack                        1.1.2           0.27.4-1                        BookStack is a simple, self-hosted, easy-to-use...
stable/buildkite                        0.2.4           3                               DEPRECATED Agent for Buildkite                    
stable/burrow                           1.5.2           0.29.0                          Burrow is a permissionable smart contract machine 
stable/centrifugo                       3.1.0           2.1.0                           Centrifugo is a real-time messaging server.       
stable/cerebro                          1.3.1           0.8.5                           A Helm chart for Cerebro - a web admin tool tha...
stable/cert-manager                     v0.6.7          v0.6.2                          A Helm chart for cert-manager                     
stable/chaoskube                        3.1.2           0.14.0                          Chaoskube periodically kills random pods in you...
stable/chartmuseum                      2.4.0           0.8.2                           Host your own Helm Chart Repository               
stable/chronograf                       1.1.0           1.7.12                          Open-source web application written in Go and R...
stable/clamav                           1.0.4           1.6                             An Open-Source antivirus engine for detecting t...
stable/cloudserver                      1.0.3           8.1.5                           An open-source Node.js implementation of the Am...
stable/cluster-autoscaler               6.2.0           1.14.6                          Scales worker nodes within autoscaling groups.    
stable/cluster-overprovisioner          0.2.6           1.0                             Installs the a deployment that overprovisions t...
stable/cockroachdb                      2.1.16          19.1.5                          CockroachDB is a scalable, survivable, strongly...
stable/collabora-code                   1.0.5           4.0.3.1                         A Helm chart for Collabora Office - CODE-Edition  
stable/concourse                        8.2.7           5.6.0                           Concourse is a simple and scalable CI system.     
stable/consul                           3.9.2           1.5.3                           Highly available and distributed service discov...
stable/contour                          0.1.0           v0.15.0                         Contour Ingress controller for Kubernetes         
stable/coredns                          1.7.4           1.6.4                           CoreDNS is a DNS server that chains plugins and...
stable/cosbench                         1.0.1           0.0.6                           A benchmark tool for cloud object storage services
stable/coscale                          1.0.0           3.16.0                          CoScale Agent                                     
stable/couchbase-operator               1.0.1           1.2.1                           A Helm chart to deploy the Couchbase Autonomous...
stable/couchdb                          2.3.0           2.3.1                           DEPRECATED A database featuring seamless multi-...
stable/dask                             3.1.0           1.1.5                           Distributed computation in Python with task sch...
stable/dask-distributed                 2.0.2                                           DEPRECATED: Distributed computation in Python     
stable/datadog                          1.38.3          6.14.0                          DataDog Agent 
...
```

Verwenden Sie zum Aktualisieren der Liste der Diagramme den Befehl [helm repo update][helm-repo-update].

```console
helm repo update
```

Das folgende Beispiel stellt das erfolgreiche Update eines Repositorys dar:

```console
$ helm repo update

Hang tight while we grab the latest from your chart repositories...
...Successfully got an update from the "ingress-nginx" chart repository
...Successfully got an update from the "stable" chart repository
Update Complete. ⎈ Happy Helming!⎈
```

### <a name="run-helm-charts"></a>Ausführen von Helm-Diagrammen

Verwenden Sie zum Installieren von Diagrammen mit Helm den Befehl [helm install][helm-install-command], und geben Sie einen Releasenamen und den Namen des zu installierenden Diagramms an. Um die Installation eines Helm-Diagramms in Aktion zu sehen, installieren wir eine grundlegende nginx-Bereitstellung mithilfe eines Helm-Diagramms.

```console
helm install my-nginx-ingress ingress-nginx/ingress-nginx \
    --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux \
    --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux
```

Die folgende verkürzte Beispielausgabe zeigt den Bereitstellungsstatus der Kubernetes-Ressource an, die vom Helm-Diagramm erstellt wurde:

```console
$ helm install my-nginx-ingress ingress-nginx/ingress-nginx \
>     --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux \
>     --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux

NAME: my-nginx-ingress
LAST DEPLOYED: Fri Nov 22 10:08:06 2019
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
The nginx-ingress controller has been installed.
It may take a few minutes for the LoadBalancer IP to be available.
You can watch the status by running 'kubectl --namespace default get services -o wide -w my-nginx-ingress-ingress-nginx-controller'
...
```

Verwenden Sie den Befehl `kubectl get services`, um den *EXTERNAL-IP*-Wert Ihres Dienstanbieters abzurufen.

```console
kubectl --namespace default get services -o wide -w my-nginx-ingress-ingress-nginx-controller
```

Der folgende Befehl zeigt z. B. den Wert von *EXTERNAL-IP* für den Dienst *my-nginx-ingress-ingress-nginx-controller* an:

```console
$ kubectl --namespace default get services -o wide -w my-nginx-ingress-ingress-nginx-controller

NAME                                        TYPE           CLUSTER-IP   EXTERNAL-IP      PORT(S)                      AGE   SELECTOR
my-nginx-ingress-ingress-nginx-controller   LoadBalancer   10.0.2.237   <EXTERNAL-IP>    80:31380/TCP,443:32239/TCP   72s   app.kubernetes.io/component=controller,app.kubernetes.io/instance=my-nginx-ingress,app.kubernetes.io/name=ingress-nginx
```

### <a name="list-releases"></a>Releases auflisten

Verwenden Sie zum Anzeigen einer Liste der im Cluster installierten Releases den Befehl `helm list`.

```console
helm list
```

Im folgenden Beispiel wird das *my-nginx-ingress*-Release angezeigt, das im vorherigen Schritt bereitgestellt wurde:

```console
$ helm list

NAME                NAMESPACE   REVISION    UPDATED                                 STATUS      CHART                   APP VERSION
my-nginx-ingress    default     1           2019-11-22 10:08:06.048477 -0600 CST    deployed    nginx-ingress-1.25.0    0.26.1 
```

### <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie ein Helm-Diagramm bereitstellen, werden eine Reihe von Kubernetes-Ressourcen erstellt. Zu diesen Ressourcen gehören Pods, Bereitstellungen und Dienste. Um diese Ressourcen zu bereinigen, verwenden Sie den Befehl [helm uninstall][helm-cleanup]. Geben Sie dabei den Namen Ihres Releases an, das Sie mit dem vorherigen Befehl `helm list` ermittelt haben.

```console
helm uninstall my-nginx-ingress
```

Das folgende Beispiel zeigt, dass das Release mit dem Namen *my-nginx-ingress* deinstalliert wurde:

```console
$ helm uninstall my-nginx-ingress

release "my-nginx-ingress" uninstalled
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Verwalten von Kubernetes-Anwendungsbereitstellungen mit Helm finden Sie in der Helm-Dokumentation.

> [!div class="nextstepaction"]
> [Helm-Dokumentation][helm-documentation]

<!-- LINKS - external -->
[helm]: https://github.com/kubernetes/helm/
[helm-cleanup]: https://helm.sh/docs/intro/using_helm/#helm-uninstall-uninstalling-a-release
[helm-documentation]: https://helm.sh/docs/
[helm-install]: https://helm.sh/docs/intro/install/
[helm-install-command]: https://helm.sh/docs/intro/using_helm/#helm-install-installing-a-package
[helm-repo-add]: https://helm.sh/docs/intro/quickstart/#initialize-a-helm-chart-repository
[helm-search]: https://helm.sh/docs/intro/using_helm/#helm-search-finding-charts
[helm-repo-update]: https://helm.sh/docs/intro/using_helm/#helm-repo-working-with-repositories
            
<!-- LINKS - internal -->
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[taints]: operator-best-practices-advanced-scheduler.md
