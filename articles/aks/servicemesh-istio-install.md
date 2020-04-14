---
title: Installieren von Istio in Azure Kubernetes Service (AKS)
description: Erfahren Sie, wie Sie Istio installieren und zum Erstellen eines Service Mesh in einem AKS-Cluster (Azure Kubernetes Service) verwenden.
author: paulbouwer
ms.topic: article
ms.date: 02/19/2020
ms.author: pabouwer
zone_pivot_groups: client-operating-system
ms.openlocfilehash: d1d02cb42a86023e5c341daab678c39f22f75dda
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2020
ms.locfileid: "80877693"
---
# <a name="install-and-use-istio-in-azure-kubernetes-service-aks"></a>Installieren und Verwenden von Istio in Azure Kubernetes Service (AKS)

[Istio][istio-github] ist ein Open-Source-Dienstmesh, das eine Reihe wichtiger Funktionen für Microservices in einem Kubernetes-Cluster bereitstellt. Zu diesen Funktionen gehören: Verwaltung des Datenverkehrs, Dienstidentität und -sicherheit, Richtlinienerzwingung und Telemetrie. Weitere Informationen zu Istio finden Sie in der offiziellen Dokumentation [What is Istio?][istio-docs-concepts] (Was ist Istio?).

In diesem Artikel wird gezeigt, wie Sie Istio installieren. Die Istio-Clientbinärdatei `istioctl` wird auf Ihrem Clientcomputer installiert, und anschließend werden die Istio-Komponenten in einem Kubernetes-Cluster in AKS installiert.

> [!NOTE]
> Die folgenden Anweisungen beziehen sich auf die Istio-Version `1.4.0`.
>
> Der Istio-Versionen `1.4.x` wurden vom Istio-Team mit den Kubernetes-Versionen `1.13`, `1.14` und `1.15` getestet. Weitere Istio-Versionen finden Sie unter [GitHub – Istio-Releases][istio-github-releases]. Informationen zu den Releases finden Sie unter [Istio-Neuigkeiten][istio-release-notes], und unterstützte Kubernetes-Versionen finden Sie in den [allgemeinen FAQ für Istio][istio-faq].

In diesem Artikel werden folgende Vorgehensweisen behandelt:

> [!div class="checklist"]
> * Herunterladen und Installieren der Istio-Clientbinärdatei „istioctl“
> * Installieren von Istio in AKS
> * Überprüfen der Istio-Installation
> * Zugreifen auf die Add-Ons
> * Deinstallieren von Istio aus AKS

## <a name="before-you-begin"></a>Voraussetzungen

Bei den in diesem Artikel beschriebenen Schritten wird vorausgesetzt, dass Sie einen AKS-Cluster (Kubernetes `1.13` und höher, RBAC aktiviert) erstellt und eine `kubectl`-Verbindung mit dem Cluster hergestellt haben. Wenn Sie Hilfe bei einem dieser Elemente benötigen, beachten Sie das Dokument [AKS-Schnellstart][aks-quickstart].

Stellen Sie sicher, dass Sie die Dokumentation zur [Istio-Leistung und -Skalierbarkeit](https://istio.io/docs/concepts/performance-and-scalability/) gelesen und die zusätzlichen Ressourcenanforderungen für die Ausführung von Istio in Ihrem AKS-Cluster verstanden haben. Die Anforderungen an Core und Arbeitsspeicher variieren je nach Workload. Wählen Sie eine geeignete Anzahl von Knoten und eine geeignete VM-Größe für Ihr Setup aus.

Im vorliegenden Artikel wird die Istio-Installationsanleitung in mehrere Einzelschritte untergliedert. Das Endergebnis hat dieselbe Struktur wie beim Befolgen der offiziellen [Anleitung][istio-install-istioctl] für die Installation von Istio.

::: zone pivot="client-operating-system-linux"

[!INCLUDE [Linux - download and install client binary](includes/servicemesh/istio/install-client-binary-linux.md)]

::: zone-end

::: zone pivot="client-operating-system-macos"

[!INCLUDE [MacOS - download and install client binary](includes/servicemesh/istio/install-client-binary-macos.md)]

::: zone-end

::: zone pivot="client-operating-system-windows"

[!INCLUDE [Windows - download and install client binary](includes/servicemesh/istio/install-client-binary-windows.md)]

::: zone-end

## <a name="install-the-istio-components-on-aks"></a>Installieren der Istio-Komponenten in AKS

Wir installieren [Grafana][grafana] und [Kiali][kiali] als Teil unserer Istio-Installation. Grafana stellt Analyse- und Überwachungsdashboards bereit, und Kiali bietet ein Übersichtsdashboard für das Dienstmesh. Bei diesem Setup erfordert jede dieser Komponenten Anmeldeinformationen, die als ein [Geheimnis][kubernetes-secrets] bereitgestellt werden müssen.

Bevor Sie die Istio-Komponenten installieren können, müssen Sie die Geheimnisse für Grafana und Kiali erstellen. Diese Geheimnisse müssen im Namespace `istio-system` installiert werden, der von Istio verwendet wird. Daher müssen wir den Namespace ebenfalls erstellen. Wir müssen die Option `--save-config` zum Erstellen des Namespaces über `kubectl create` verwenden, damit das Istio-Installationsprogramm in Zukunft `kubectl apply` für dieses Objekt ausführen kann.

```console
kubectl create namespace istio-system --save-config
```

::: zone pivot="client-operating-system-linux"

[!INCLUDE [Bash - create secrets for Grafana and Kiali](includes/servicemesh/istio/install-create-secrets-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-macos"

[!INCLUDE [Bash check for CRDs](includes/servicemesh/istio/install-create-secrets-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-windows"

[!INCLUDE [PowerShell check for CRDs](includes/servicemesh/istio/install-create-secrets-powershell.md)]

::: zone-end

### <a name="install-istio-components"></a>Installieren der Istio-Komponenten

Nachdem Sie in Ihrem AKS-Cluster erfolgreich die Geheimnisse für Grafana und Kiali erstellt haben, können Sie die Istio-Komponenten installieren. 

Der [Helm][helm]-Installationsansatz für Istio wird in Zukunft als veraltet betrachtet. Der neue Installationsansatz für Istio nutzt die `istioctl`-Clientbinärdatei, die [Istio-Konfigurationsprofile][istio-configuration-profiles] und die neue [Istio-Steuerungsebenenspezifikation und -API][istio-control-plane]. Dieser neue Ansatz wird für die Installation von Istio verwendet.

> [!NOTE]
> Istio muss aktuell für die Ausführung auf Linux-Knoten geplant werden. Wenn sich in Ihrem Cluster Windows Server-Knoten befinden, müssen Sie sicherstellen, dass Istio-Pods nur für die Ausführung auf Linux-Knoten geplant sind. Wir verwenden [Knotenselektoren][kubernetes-node-selectors], um sicherzustellen, dass Pods für die richtigen Knoten geplant werden.

> [!CAUTION]
> Die Istio-Features [SDS (Secret Discovery Service)][istio-feature-sds] und [Istio CNI][istio-feature-cni] befinden sich derzeit in der [Alpha-Phase][istio-feature-stages] und müssen daher mit Bedacht verwendet werden. 
>
> Beachten Sie, dass die Kubernetes-Funktion [Service Account Token Volume Projection][kubernetes-feature-sa-projected-volume] (Dienstkontotoken-Volumenprojektion; eine Voraussetzung für SDS) in allen Kubernetes 1.13 und höheren Versionen in AKS jetzt **aktiviert** ist.

Erstellen Sie eine Datei namens `istio.aks.yaml` mit dem folgenden Inhalt. Diese Datei enthält die Details der [Istio-Steuerungsebenenspezifikation][istio-control-plane] zum Konfigurieren von Istio.

```yaml
apiVersion: install.istio.io/v1alpha2
kind: IstioControlPlane
spec:
  # Use the default profile as the base
  # More details at: https://istio.io/docs/setup/additional-setup/config-profiles/
  profile: default
  values:
    global:
      # Ensure that the Istio pods are only scheduled to run on Linux nodes
      defaultNodeSelector:
        beta.kubernetes.io/os: linux
      # Enable mutual TLS for the control plane
      controlPlaneSecurityEnabled: true
      mtls:
        # Require all service to service communication to have mtls
        enabled: false
    grafana:
      # Enable Grafana deployment for analytics and monitoring dashboards
      enabled: true
      security:
        # Enable authentication for Grafana
        enabled: true
    kiali:
      # Enable the Kiali deployment for a service mesh observability dashboard
      enabled: true
    tracing:
      # Enable the Jaeger deployment for tracing
      enabled: true
```

Installieren Sie Istio mithilfe des Befehls `istioctl apply` und der Istio-Steuerungsebenen-Spezifikationsdatei `istio.aks.yaml` wie folgt:

```console
istioctl manifest apply -f istio.aks.yaml --logtostderr --set installPackagePath=./install/kubernetes/operator/charts
```

Das Installationsprogramm stellt eine Reihe von [CRDs][kubernetes-crd] bereit und verwaltet dann die Abhängigkeiten, um alle relevanten Objekte zu installieren, die für diese Konfiguration von Istio definiert sind. Das angezeigte Ergebnis sollte ähnlich wie der folgende Ausgabecodeausschnitt aussehen.

```console
Applying manifests for these components:
- Tracing
- EgressGateway
- NodeAgent
- Grafana
- Policy
- Citadel
- CertManager
- IngressGateway
- Injector
- Prometheus
- PrometheusOperator
- Kiali
- Telemetry
- Galley
- Cni
- Pilot
- Base
- CoreDNS
NodeAgent is waiting on a prerequisite...
Telemetry is waiting on a prerequisite...
Galley is waiting on a prerequisite...
Cni is waiting on a prerequisite...
Grafana is waiting on a prerequisite...
Policy is waiting on a prerequisite...
Citadel is waiting on a prerequisite...
EgressGateway is waiting on a prerequisite...
Tracing is waiting on a prerequisite...
Kiali is waiting on a prerequisite...
PrometheusOperator is waiting on a prerequisite...
IngressGateway is waiting on a prerequisite...
Prometheus is waiting on a prerequisite...
CertManager is waiting on a prerequisite...
Injector is waiting on a prerequisite...
Pilot is waiting on a prerequisite...
Applying manifest for component Base
Waiting for CRDs to be applied.
CRDs applied.
Finished applying manifest for component Base
Prerequisite for Tracing has completed, proceeding with install.
Prerequisite for Injector has completed, proceeding with install.
Prerequisite for Telemetry has completed, proceeding with install.
Prerequisite for Policy has completed, proceeding with install.
Prerequisite for PrometheusOperator has completed, proceeding with install.
Prerequisite for NodeAgent has completed, proceeding with install.
Prerequisite for IngressGateway has completed, proceeding with install.
Prerequisite for Kiali has completed, proceeding with install.
Prerequisite for EgressGateway has completed, proceeding with install.
Prerequisite for Galley has completed, proceeding with install.
Prerequisite for Grafana has completed, proceeding with install.
Prerequisite for Cni has completed, proceeding with install.
Prerequisite for Citadel has completed, proceeding with install.
Applying manifest for component Tracing
Prerequisite for Prometheus has completed, proceeding with install.
Prerequisite for Pilot has completed, proceeding with install.
Prerequisite for CertManager has completed, proceeding with install.
Applying manifest for component Kiali
Applying manifest for component Prometheus
Applying manifest for component IngressGateway
Applying manifest for component Policy
Applying manifest for component Telemetry
Applying manifest for component Citadel
Applying manifest for component Galley
Applying manifest for component Pilot
Applying manifest for component Injector
Applying manifest for component Grafana
Finished applying manifest for component Kiali
Finished applying manifest for component Tracing
Finished applying manifest for component Prometheus
Finished applying manifest for component Citadel
Finished applying manifest for component Policy
Finished applying manifest for component IngressGateway
Finished applying manifest for component Injector
Finished applying manifest for component Galley
Finished applying manifest for component Pilot
Finished applying manifest for component Grafana
Finished applying manifest for component Telemetry

Component IngressGateway installed successfully:
================================================

serviceaccount/istio-ingressgateway-service-account created
deployment.apps/istio-ingressgateway created
gateway.networking.istio.io/ingressgateway created
sidecar.networking.istio.io/default created
poddisruptionbudget.policy/ingressgateway created
horizontalpodautoscaler.autoscaling/istio-ingressgateway created
service/istio-ingressgateway created

...
```

An diesem Punkt haben Sie Istio in Ihrem AKS-Cluster bereitgestellt. Um eine erfolgreiche Bereitstellung von Istio sicherzustellen, fahren Sie mit dem nächsten Abschnitt [Überprüfen der Istio-Installation](#validate-the-istio-installation) fort.

## <a name="validate-the-istio-installation"></a>Überprüfen der Istio-Installation

Vergewissern Sie sich zunächst, dass die erwarteten Dienste erstellt wurden. Verwenden Sie den Befehl [Kubectl get svc][kubectl-get], um die ausgeführten Dienste anzuzeigen. Fragen Sie den `istio-system`-Namespace ab, in dem die Istio- und Add-On-Komponenten vom Helm-Diagramm `istio` installiert wurden:

```console
kubectl get svc --namespace istio-system --output wide
```

Die folgende Beispielausgabe zeigt die Dienste, die jetzt ausgeführt werden sollten:

- `istio-*`-Dienste
- Add-On-Ablaufverfolgungsdienste `jaeger-*`, `tracing` und `zipkin`
- Add-On-Metrikdienst `prometheus`
- Add-On-Dienst `grafana` für Analyse- und -Überwachungsdashboards
- Add-On-Dashboarddienst `kiali` für das Dienstmesh

Wenn das `istio-ingressgateway` für die externe IP-Adresse `<pending>` anzeigt, warten Sie einige Minuten, bis vom Azure-Netzwerk eine IP-Adresse zugewiesen wurde.

```console
NAME                     TYPE           CLUSTER-IP     EXTERNAL-IP      PORT(S)                                                                                                                      AGE   SELECTOR
grafana                  ClusterIP      10.0.116.147   <none>           3000/TCP                                                                                                                     92s   app=grafana
istio-citadel            ClusterIP      10.0.248.152   <none>           8060/TCP,15014/TCP                                                                                                           94s   app=citadel
istio-galley             ClusterIP      10.0.50.100    <none>           443/TCP,15014/TCP,9901/TCP,15019/TCP                                                                                         93s   istio=galley
istio-ingressgateway     LoadBalancer   10.0.36.213    20.188.221.111   15020:30369/TCP,80:31368/TCP,443:30045/TCP,15029:32011/TCP,15030:31212/TCP,15031:32411/TCP,15032:30009/TCP,15443:30010/TCP   93s   app=istio-ingressgateway
istio-pilot              ClusterIP      10.0.23.222    <none>           15010/TCP,15011/TCP,8080/TCP,15014/TCP                                                                                       93s   istio=pilot
istio-policy             ClusterIP      10.0.59.250    <none>           9091/TCP,15004/TCP,15014/TCP                                                                                                 93s   istio-mixer-type=policy,istio=mixer
istio-sidecar-injector   ClusterIP      10.0.123.219   <none>           443/TCP                                                                                                                      93s   istio=sidecar-injector
istio-telemetry          ClusterIP      10.0.216.9     <none>           9091/TCP,15004/TCP,15014/TCP,42422/TCP                                                                                       89s   istio-mixer-type=telemetry,istio=mixer
jaeger-agent             ClusterIP      None           <none>           5775/UDP,6831/UDP,6832/UDP                                                                                                   96s   app=jaeger
jaeger-collector         ClusterIP      10.0.221.24    <none>           14267/TCP,14268/TCP,14250/TCP                                                                                                95s   app=jaeger
jaeger-query             ClusterIP      10.0.46.154    <none>           16686/TCP                                                                                                                    95s   app=jaeger
kiali                    ClusterIP      10.0.174.97    <none>           20001/TCP                                                                                                                    94s   app=kiali
prometheus               ClusterIP      10.0.245.226   <none>           9090/TCP                                                                                                                     94s   app=prometheus
tracing                  ClusterIP      10.0.249.95    <none>           9411/TCP                                                                                                                     95s   app=jaeger
zipkin                   ClusterIP      10.0.154.89    <none>           9411/TCP                                                                                                                     94s   app=jaeger
```

Vergewissern Sie sich als Nächstes, dass die erforderlichen Pods erstellt wurden. Verwenden Sie den Befehl [kubectl get pods][kubectl-get], und fragen Sie den `istio-system`-Namespace erneut ab:

```console
kubectl get pods --namespace istio-system
```

Die folgende Beispielausgabe zeigt die Pods, die ausgeführt werden:

- `istio-*`-Pods
- Add-On-Metrikpod `prometheus-*`
- Add-On-Pod `grafana-*` für Analyse- und -Überwachungsdashboards
- Add-On-Pod `kiali` für Dienstmeshdashboard

```console
NAME                                          READY   STATUS    RESTARTS   AGE
grafana-6bc97ff99-k9sk4                       1/1     Running   0          92s
istio-citadel-6b5c754454-tb8nf                1/1     Running   0          94s
istio-galley-7d6d78d7c5-zshsd                 2/2     Running   0          94s
istio-ingressgateway-85869c5cc7-x5d76         1/1     Running   0          95s
istio-pilot-787d6995b5-n5vrj                  2/2     Running   0          94s
istio-policy-6cf4fbc8dc-sdsg5                 2/2     Running   2          94s
istio-sidecar-injector-5d5b978668-wrz2s       1/1     Running   0          94s
istio-telemetry-5498db684-6kdnw               2/2     Running   1          94s
istio-tracing-78548677bc-74tx6                1/1     Running   0          96s
kiali-59b7fd7f68-92zrh                        1/1     Running   0          95s
prometheus-7c7cf9dbd6-rjxcv                   1/1     Running   0          94s
```

Alle Pods müssen den Status `Running` haben. Wenn Ihre Pods nicht diesen Status haben, warten Sie ein oder zwei Minuten, bis dies der Fall ist. Wenn einer der Pods ein Problem meldet, verwenden Sie den Befehl [kubectl describe pod][kubectl-describe], um die Ausgabe und den Status des betreffenden Pods zu überprüfen.

## <a name="accessing-the-add-ons"></a>Zugreifen auf die Add-Ons

Einige der Add-Ons, die bei der obigen Einrichtung durch Istio installiert wurden, bieten zusätzliche Funktionen. Die Webanwendungen für die Add-Ons werden **nicht** öffentlich über eine externe IP-Adresse verfügbar gemacht. 

Verwenden Sie den Befehl `istioctl dashboard`, um auf diese Add-On-Benutzeroberflächen zuzugreifen. Dieser Befehl nutzt [kubectl port-forward][kubectl-port-forward] und einen willkürlichen Port, um eine sichere Verbindung zwischen Ihrem Clientcomputer und dem relevanten Pod in Ihrem AKS-Cluster herzustellen. Anschließend wird automatisch die Add-On-Webanwendung in Ihrem Standardbrowser geöffnet.

Sie haben zuvor in diesem Artikel für Grafana und Kiali eine zusätzliche Sicherheitsebene hinzugefügt, indem Sie Anmeldeinformationen angegeben haben.

### <a name="grafana"></a>Grafana

Die Analyse- und Überwachungsdashboards für Istio werden von [Grafana][grafana] bereitgestellt. Sie müssen die Anmeldeinformationen verwenden, die Sie zuvor nach Aufforderung mithilfe des Grafana-Geheimnisses erstellten haben. Gehen Sie wie folgt vor, um das Grafana-Dashboard auf sichere Weise zu öffnen:

```console
istioctl dashboard grafana
```

### <a name="prometheus"></a>Prometheus

Die Metriken für Istio werden von [Prometheus][prometheus] bereitgestellt. Gehen Sie wie folgt vor, um das Prometheus-Dashboard auf sichere Weise zu öffnen:

```console
istioctl dashboard prometheus
```

### <a name="jaeger"></a>Jaeger

Die Ablaufverfolgung in Istio wird über [Jaeger][jaeger] bereitgestellt. Gehen Sie wie folgt vor, um das Jaeger-Dashboard auf sichere Weise zu öffnen:

```console
istioctl dashboard jaeger
```

### <a name="kiali"></a>Kiali

Ein Service Mesh-Telemetriedashboard wird von [Kiali][kiali] bereitgestellt. Sie müssen die Anmeldeinformationen verwenden, die Sie zuvor nach Aufforderung mithilfe des Kiali-Geheimnisses erstellten haben. Gehen Sie wie folgt vor, um das Kiali-Dashboard auf sichere Weise zu öffnen:

```console
istioctl dashboard kiali
```

### <a name="envoy"></a>Envoy

Für die [Envoy][envoy]-Proxys steht eine einfache Oberfläche zur Verfügung. Sie enthält Konfigurationsinformationen und Metriken für einen Envoy-Proxy, der in einem bestimmten Pod ausgeführt wird. Gehen Sie wie folgt vor, um die Envoy-Oberfläche auf sichere Weise zu öffnen:

```console
istioctl dashboard envoy <pod-name>.<namespace>
```

## <a name="uninstall-istio-from-aks"></a>Deinstallieren von Istio aus AKS

> [!WARNING]
> Das Löschen von Istio in einem laufenden System kann zu Problemen beim Datenverkehr zwischen Ihren Diensten führen. Stellen Sie sicher, dass Sie das System so weit vorbereitet haben, dass es auch ohne Istio ordnungsgemäß fortgesetzt werden kann.

### <a name="remove-istio-components-and-namespace"></a>Entfernen von Istio-Komponenten und -Namespace

Um Istio aus dem AKS-Cluster zu entfernen, verwenden Sie den Befehl `istioctl manifest generate` mit der Istio-Steuerungsebenen-Spezifikationsdatei `istio.aks.yaml`. Dadurch wird das bereitgestellte Manifest generiert, das wir an `kubectl delete` weiterleiten, um alle installierten Komponenten und den Namespace `istio-system` zu entfernen.

```console
istioctl manifest generate -f istio.aks.yaml -o istio-components-aks --logtostderr --set installPackagePath=./install/kubernetes/operator/charts 

kubectl delete -f istio-components-aks -R
```

### <a name="remove-istio-crds-and-secrets"></a>Entfernen von Istio-CRDs und Geheimnissen

Durch die obigen Befehle werden alle Istio-Komponenten und der Namespace gelöscht, die generierten Istio-Geheimnisse bleiben jedoch erhalten. 

::: zone pivot="client-operating-system-linux"

[!INCLUDE [Bash - remove Istio secrets](includes/servicemesh/istio/uninstall-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-macos"

[!INCLUDE [Bash - remove Istio secrets](includes/servicemesh/istio/uninstall-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-windows"

[!INCLUDE [PowerShell - remove Istio secrets](includes/servicemesh/istio/uninstall-powershell.md)]

::: zone-end

## <a name="next-steps"></a>Nächste Schritte

In der folgenden Dokumentation wird beschrieben, wie Sie Istio verwenden können, um ein intelligentes Routing für den Rollout eines Canaryrelease bereitzustellen:

> [!div class="nextstepaction"]
> [AKS Istio intelligent routing scenario (Szenario für intelligentes Routing mit AKS Istio)][istio-scenario-routing]

Um mehr über Installations- und Konfigurationsoptionen für Istio zu erfahren, beachten Sie die folgenden offiziellen Istio-Leitfäden:

- [Istio: Installationsleitfäden][istio-installation-guides]

Weitere Szenarien finden Sie hier:

- [Istio-Anwendungsbeispiel: BookInfo][istio-bookinfo-example]

Um zu erfahren, wie Sie Ihre AKS-Anwendung mit Application Insights und Istio überwachen können, lesen Sie die folgende Dokumentation zu Azure Monitor:

- [Anwendungsüberwachung ohne Instrumentierung für gehostete Kubernetes-Anwendungen][app-insights]

<!-- LINKS - external -->
[istio]: https://istio.io
[helm]: https://helm.sh

[istio-faq]: https://istio.io/faq/general/
[istio-docs-concepts]: https://istio.io/docs/concepts/what-is-istio/
[istio-github]: https://github.com/istio/istio
[istio-github-releases]: https://github.com/istio/istio/releases
[istio-release-notes]: https://istio.io/news/
[istio-installation-guides]: https://istio.io/docs/setup/install/
[istio-install-download]: https://istio.io/docs/setup/kubernetes/download-release/
[istio-install-istioctl]: https://istio.io/docs/setup/install/istioctl/
[istio-configuration-profiles]: https://istio.io/docs/setup/additional-setup/config-profiles/
[istio-control-plane]: https://istio.io/docs/reference/config/istio.operator.v1alpha1/
[istio-bookinfo-example]: https://istio.io/docs/examples/bookinfo/

[istio-feature-stages]: https://istio.io/about/feature-stages/
[istio-feature-sds]: https://istio.io/docs/tasks/traffic-management/ingress/secure-ingress-sds/
[istio-feature-cni]: https://istio.io/docs/setup/additional-setup/cni/

[install-wsl]: https://docs.microsoft.com/windows/wsl/install-win10

[kubernetes-feature-sa-projected-volume]: https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/#service-account-token-volume-projection
[kubernetes-crd]: https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/#customresourcedefinitions
[kubernetes-secrets]: https://kubernetes.io/docs/concepts/configuration/secret/
[kubernetes-node-selectors]: https://docs.microsoft.com/azure/aks/concepts-clusters-workloads#node-selectors
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe
[kubectl-port-forward]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#port-forward

[grafana]: https://grafana.com/
[prometheus]: https://prometheus.io/
[jaeger]: https://www.jaegertracing.io/
[kiali]: https://www.kiali.io/
[envoy]: https://www.envoyproxy.io/

[app-insights]: https://docs.microsoft.com/azure/azure-monitor/app/kubernetes

<!-- LINKS - internal -->
[aks-quickstart]: ./kubernetes-walkthrough.md
[istio-scenario-routing]: ./servicemesh-istio-scenario-routing.md
