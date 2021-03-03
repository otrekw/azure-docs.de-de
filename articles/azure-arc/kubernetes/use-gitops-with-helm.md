---
title: Bereitstellen von Helm-Charts mithilfe von GitOps in Arc-fähigen Kubernetes-Clustern
services: azure-arc
ms.service: azure-arc
ms.date: 03/02/2021
ms.topic: article
author: mlearned
ms.author: mlearned
description: Verwenden von GitOps mit Helm für Azure Arc-fähige Clusterkonfiguration
keywords: GitOps, Kubernetes, K8s, Azure, Helm, Arc, AKS, Azure Kubernetes Service, Container
ms.openlocfilehash: 0176d614a6bf44e2bf20cc7935521a6387a3b574
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101687282"
---
# <a name="deploy-helm-charts-using-gitops-on-an-arc-enabled-kubernetes-cluster"></a>Bereitstellen von Helm-Charts mithilfe von GitOps in einem Arc-fähigen Kubernetes-Cluster

Helm ist ein Open Source-Verpackungstool, das Ihnen dabei hilft, Kubernetes-Anwendungen zu installieren und ihren Lebenszyklus zu verwalten. Ähnlich wie Linux-Paket-Manager (z. B. APT und Yum) wird Helm zur Verwaltung von Kubernetes-Charts verwendet, bei denen es sich um Pakete aus vorkonfigurierten Kubernetes-Ressourcen handelt.

In diesem Artikel wird die Konfiguration und Verwendung von Helm mit Azure Arc-fähigem Kubernetes veranschaulicht.

## <a name="before-you-begin"></a>Voraussetzungen

Überprüfen Sie, ob Sie über einen verbundenen, Azure Arc-fähigen Kubernetes-Cluster verfügen. Wenn Sie einen verbundenen Cluster benötigen, sehen Sie sich den [Schnellstart zum Verbinden eines Azure Arc-fähigen Kubernetes-Clusters](./quickstart-connect-cluster.md) an.

## <a name="overview-of-using-gitops-and-helm-with-azure-arc-enabled-kubernetes"></a>Übersicht über die Verwendung von GitOps und Helm mit Azure Arc-fähigem Kubernetes

 Der Helm-Operator stellt eine Erweiterung für Flux bereit, die Helm-Chartreleases automatisiert. Ein Helm-Chart-Release wird von einer benutzerdefinierten Kubernetes-Ressource namens HelmRelease beschrieben. Flux synchronisiert diese Ressourcen zwischen Git und dem Cluster, während der Helm-Operator sicherstellt, dass Helm-Charts wie in den Ressourcen festgelegt veröffentlicht werden.

 Das [Beispielrepository](https://github.com/Azure/arc-helm-demo) in diesem Artikel ist wie folgt strukturiert:

```console
├── charts
│   └── azure-arc-sample
│       ├── Chart.yaml
│       ├── templates
│       │   ├── NOTES.txt
│       │   ├── deployment.yaml
│       │   └── service.yaml
│       └── values.yaml
└── releases
    └── app.yaml
```

Im Git-Repository sind zwei Verzeichnisse enthalten. Eines enthält ein Helm-Chart, und das andere enthält die Konfiguration für die Releases. Im Verzeichnis `releases` enthält die Datei `app.yaml` die HelmRelease-Konfigurationsdatei, wie im Folgenden gezeigt:

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

| Feld | BESCHREIBUNG |
| ------------- | ------------- | 
| `metadata.name` | Pflichtfeld. Muss den Kubernetes-Namenskonventionen entsprechen. |
| `metadata.namespace` | Optionales Feld. Bestimmt, wo das Release erstellt wird. |
| `spec.releaseName` | Optionales Feld. Wenn keine Angabe erfolgt, lautet der Releasename `$namespace-$name`. |
| `spec.chart.path` | Das Verzeichnis mit dem Chart (relativ zum Stammverzeichnis des Repositorys). |
| `spec.values` | Benutzerdefinierte Varianten von Standardparameterwerten aus dem Chart selbst. |

Die in der HelmRelease-Datei `spec.values` angegebenen Optionen überschreiben die Optionen, die in der Datei `values.yaml` der Chart-Quelle angegeben sind.

Weitere Informationen zu HelmRelease finden Sie in der offiziellen [Dokumentation für Helm-Operatoren](https://docs.fluxcd.io/projects/helm-operator/en/stable/).

## <a name="create-a-configuration"></a>Erstellen einer Konfiguration

Mit der Azure CLI-Erweiterung für `k8s-configuration` verknüpfen Sie den verbundenen Cluster mit dem Git-Beispielrepository. Geben Sie dieser Konfiguration den Namen `azure-arc-sample`, und stellen Sie den Flux-Operator im `arc-k8s-demo`-Namespace bereit.

```console
az k8s-configuration create --name azure-arc-sample --cluster-name AzureArcTest1 --resource-group AzureArcTest --operator-instance-name flux --operator-namespace arc-k8s-demo --operator-params='--git-readonly --git-path=releases' --enable-helm-operator --helm-operator-chart-version='1.2.0' --helm-operator-params='--set helm.versions=v3' --repository-url https://github.com/Azure/arc-helm-demo.git --scope namespace --cluster-type connectedClusters
```

### <a name="configuration-parameters"></a>Konfigurationsparameter

[Erfahren Sie mehr über die zusätzlichen Parameter](./tutorial-use-gitops-connected-cluster.md#additional-parameters), mit denen Sie die Erstellung der Konfiguration anpassen können.

## <a name="validate-the-configuration"></a>Überprüfen der Konfiguration

Vergewissern Sie sich mithilfe der Azure CLI, dass die Konfiguration erfolgreich erstellt wurde.

```console
az k8s-configuration show --name azure-arc-sample --cluster-name AzureArcTest1 --resource-group AzureArcTest --cluster-type connectedClusters
```

Die Konfigurationsressource wird mit Compliancestatus, Nachrichten und Debuginformationen aktualisiert.

```output
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
    "chartVersion": "1.2.0"
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
