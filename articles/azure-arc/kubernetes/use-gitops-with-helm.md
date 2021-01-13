---
title: Bereitstellen von Helm-Charts mithilfe von GitOps in Arc-fähigen Kubernetes-Clustern (Vorschau)
services: azure-arc
ms.service: azure-arc
ms.date: 05/19/2020
ms.topic: article
author: mlearned
ms.author: mlearned
description: Verwenden von GitOps mit Helm für Azure Arc-fähige Clusterkonfiguration (Vorschauversion)
keywords: GitOps, Kubernetes, K8s, Azure, Helm, Arc, AKS, Azure Kubernetes Service, Container
ms.openlocfilehash: eea81d458ac6631c4a023134b3198e4cdb04526e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91541610"
---
# <a name="deploy-helm-charts-using-gitops-on-arc-enabled-kubernetes-cluster-preview"></a>Bereitstellen von Helm-Charts mithilfe von GitOps in Arc-fähigen Kubernetes-Clustern (Vorschau)

Helm ist ein Open Source-Verpackungstool, das Ihnen dabei hilft, Kubernetes-Anwendungen zu installieren und ihren Lebenszyklus zu verwalten. Ähnlich wie der Linux-Paket-Manager (z. B. APT und Yum) wird Helm zur Verwaltung von Kubernetes-Diagrammen verwendet, bei denen es sich um Pakete aus vorkonfigurierten Kubernetes-Ressourcen handelt.

In diesem Artikel wird die Konfiguration und Verwendung von Helm mit Azure Arc-fähigem Kubernetes veranschaulicht.

## <a name="before-you-begin"></a>Voraussetzungen

In diesem Artikel wird davon ausgegangen, dass Sie über einen verbundenen, Azure Arc-fähigen Kubernetes-Cluster verfügen. Wenn Sie einen verbundenen Cluster benötigen, finden Sie weitere Informationen im [Schnellstart zum Verbinden eines Clusters](./connect-cluster.md).

## <a name="overview-of-using-gitops-and-helm-with-azure-arc-enabled-kubernetes"></a>Übersicht über die Verwendung von GitOps und Helm mit Azure Arc-fähigem Kubernetes

 Der Helm-Operator stellt eine Erweiterung für Flux bereit, die Helm-Chartreleases automatisiert. Ein Chartrelease wird von einer benutzerdefinierten Kubernetes-Ressource namens HelmRelease beschrieben. Flux synchronisiert diese Ressourcen zwischen Git und dem Cluster. Der Helm-Operator stellt sicher, dass Helm-Charts wie in den Ressourcen festgelegt veröffentlicht werden.

 Die [Beispielrepository](https://github.com/Azure/arc-helm-demo) in diesem Dokument ist wie folgt strukturiert:

```console
├── charts
│   └── azure-arc-sample
│       ├── Chart.yaml
│       ├── templates
│       │   ├── NOTES.txt
│       │   ├── deployment.yaml
│       │   └── service.yaml
│       └── values.yaml
└── releases
    └── app.yaml
```

Im Git-Repository sind zwei Verzeichnisse enthalten. Eines enthält einen Helm-Chart, und das andere enthält die Konfigurationsdatei für die Releases. Im Verzeichnis `releases` enthält die Datei `app.yaml` die HelmRelease-Konfigurationsdatei, wie im Folgenden gezeigt:

```yaml
apiVersion: helm.fluxcd.io/v1
kind: HelmRelease
metadata:
  name: azure-arc-sample
  namespace: arc-k8s-demo
spec:
  releaseName: arc-k8s-demo
  chart:
    git: https://github.com/Azure/arc-helm-demo
    ref: master
    path: charts/azure-arc-sample
  values:
    serviceName: arc-k8s-demo
```

Die Konfigurationsdatei für das Helm-Release enthält die folgenden Felder:

- `metadata.name` ist erforderlich und muss den Kubernetes-Benennungskonventionen entsprechen.
- `metadata.namespace` ist optional und bestimmt, wo das Release erstellt wird.
- `spec.releaseName` ist optional, und der Releasename entspricht „$namespace-$name“, wenn kein Name angegeben wird.
- `spec.chart.path` ist das Verzeichnis, das das Chart enthält, das im Zusammenhang mit dem Repositorystamm steht.
- `spec.values` beschreibt benutzerdefinierte Varianten von Standardparameterwerten aus dem Chart selbst.

Die in der Datei „HelmRelease“ unter „spec.values“ angegebenen Optionen überschreiben die Optionen, die in der Datei „values.yaml“ der Chart-Quelle angegeben sind.

Weitere Informationen über die Datei „HelmRelease“ finden Sie in der offiziellen [Dokumentation zum Helm-Operator](https://docs.fluxcd.io/projects/helm-operator/en/stable/).

## <a name="create-a-configuration"></a>Erstellen einer Konfiguration

Mit der Azure CLI-Erweiterung für `k8sconfiguration` verknüpfen Sie den verbundenen Cluster mit dem Git-Beispielrepository. Geben Sie dieser Konfiguration den Namen `azure-arc-sample`, und stellen Sie den Flux-Operator im `arc-k8s-demo`-Namespace bereit.

```console
az k8sconfiguration create --name azure-arc-sample --cluster-name AzureArcTest1 --resource-group AzureArcTest --operator-instance-name flux --operator-namespace arc-k8s-demo --operator-params='--git-readonly --git-path=releases' --enable-helm-operator --helm-operator-version='0.6.0' --helm-operator-params='--set helm.versions=v3' --repository-url https://github.com/Azure/arc-helm-demo.git --scope namespace --cluster-type connectedClusters
```

### <a name="configuration-parameters"></a>Konfigurationsparameter

[Erfahren Sie mehr die zusätzliche Parameter](./use-gitops-connected-cluster.md#additional-parameters), um die Erstellung der Konfiguration anzupassen.

## <a name="validate-the-configuration"></a>Überprüfen der Konfiguration

Überprüfen Sie mithilfe der Azure CLI, ob die `sourceControlConfiguration`-Ressource erfolgreich erstellt wurde.

```console
az k8sconfiguration show --name azure-arc-sample --cluster-name AzureArcTest1 --resource-group AzureArcTest --cluster-type connectedClusters
```

Die `sourceControlConfiguration`-Ressource wird mit Compliancestatus, Nachrichten und Debuginformationen aktualisiert.

**Ausgabe:**

```console
Command group 'k8sconfiguration' is in preview. It may be changed/removed in a future release.
{
  "complianceStatus": {
    "complianceState": "Installed",
    "lastConfigApplied": "2019-12-05T05:34:41.481000",
    "message": "{\"OperatorMessage\":null,\"ClusterState\":null}",
    "messageLevel": "3"
  },
  "enableHelmOperator": "True",
  "helmOperatorProperties": {
    "chartValues": "--set helm.versions=v3",
    "chartVersion": "0.6.0"
  },
  "id": "/subscriptions/57ac26cf-a9f0-4908-b300-9a4e9a0fb205/resourceGroups/AzureArcTest/providers/Microsoft.Kubernetes/connectedClusters/AzureArcTest1/providers/Microsoft.KubernetesConfiguration/sourceControlConfigurations/azure-arc-sample",
  "name": "azure-arc-sample",
  "operatorInstanceName": "flux",
  "operatorNamespace": "arc-k8s-demo",
  "operatorParams": "--git-readonly --git-path=releases",
  "operatorScope": "namespace",
  "operatorType": "Flux",
  "provisioningState": "Succeeded",
  "repositoryPublicKey": "",
  "repositoryUrl": "https://github.com/Azure/arc-helm-demo.git",
  "resourceGroup": "AzureArcTest",
  "type": "Microsoft.KubernetesConfiguration/sourceControlConfigurations"
}
```

## <a name="validate-application"></a>Überprüfen der Anwendung

Führen Sie den folgenden Befehl aus, und navigieren Sie in Ihrem Browser zu `localhost:8080`, um zu überprüfen, ob die Anwendung ausgeführt wird.

```console
kubectl port-forward -n arc-k8s-demo svc/arc-k8s-demo 8080:8080
```

## <a name="next-steps"></a>Nächste Schritte

- [Verwenden von Azure Policy zum Steuern der Clusterkonfiguration](./use-azure-policy.md)
