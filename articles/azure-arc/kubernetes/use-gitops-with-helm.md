---
title: Verwenden von GitOps mit Helm für Azure Arc-fähige Clusterkonfiguration (Vorschauversion)
services: azure-arc
ms.service: azure-arc
ms.date: 05/19/2020
ms.topic: article
author: mlearned
ms.author: mlearned
description: Verwenden von GitOps mit Helm für Azure Arc-fähige Clusterkonfiguration (Vorschauversion)
keywords: GitOps, Kubernetes, K8s, Azure, Helm, Arc, AKS, Azure Kubernetes Service, Container
ms.openlocfilehash: 9cd1169c7a622da0e4be3900f94dc31fc99e762d
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83662538"
---
# <a name="use-gitops-with-helm-for-an-azure-arc-enabled-cluster-configuration-preview"></a>Verwenden von GitOps mit Helm für Azure Arc-fähige Clusterkonfiguration (Vorschauversion)

Helm ist ein Open Source-Verpackungstool, das Ihnen dabei hilft, Kubernetes-Anwendungen zu installieren und ihren Lebenszyklus zu verwalten. Ähnlich wie der Linux-Paket-Manager (z. B. APT und Yum) wird Helm zur Verwaltung von Kubernetes-Diagrammen verwendet, bei denen es sich um Pakete aus vorkonfigurierten Kubernetes-Ressourcen handelt.

In diesem Artikel wird die Konfiguration und Verwendung von Helm mit Azure Arc-fähigem Kubernetes veranschaulicht.

## <a name="before-you-begin"></a>Voraussetzungen

In diesem Artikel wird davon ausgegangen, dass Sie über einen verbundenen, Azure Arc-fähigen Kubernetes-Cluster verfügen. Wenn Sie einen verbundenen Cluster benötigen, finden Sie weitere Informationen im [Schnellstart zum Verbinden eines Clusters](./connect-cluster.md).

Zunächst legen Sie die Umgebungsvariablen fest, die Sie im Verlauf dieses Tutorials verwenden. Sie benötigen den Ressourcengruppennamen und den Clusternamen für Ihren verbundenen Cluster.

```bash
export RESOURCE_GROUP=<Resource_Group_Name>
export CLUSTER_NAME=<ClusterName>
```

## <a name="verify-your-cluster-is-enabled-with-arc"></a>Überprüfen der Arc-Fähigkeit Ihres Clusters

```bash
az connectedk8s list -g $RESOURCE_GROUP -o table
```

Ausgabe:
```bash
Name           Location    ResourceGroup
-------------  ----------  ---------------
arc-helm-demo  eastus      k8s-clusters
```

## <a name="overview-of-using-helm-with-azure-arc-enabled-kubernetes"></a>Übersicht über die Verwendung von Helm mit Azure Arc-fähigem Kubernetes

 Der Helm-Operator stellt eine Erweiterung für Flux bereit, die Helm-Chartreleases automatisiert. Ein Chartrelease wird von einer benutzerdefinierten Kubernetes-Ressource namens HelmRelease beschrieben. Flux synchronisiert diese Ressourcen zwischen Git und dem Cluster. Der Helm-Operator stellt sicher, dass Helm-Charts wie in den Ressourcen festgelegt veröffentlicht werden.

 Das folgende Beispiel enthält die Struktur des Git-Repositorys, das in diesem Tutorial verwendet wird:

```bash
├── charts
│   └── azure-vote
│       ├── Chart.yaml
│       ├── templates
│       │   ├── NOTES.txt
│       │   ├── deployment.yaml
│       │   └── service.yaml
│       └── values.yaml
└── releases
    └── prod
        └── azure-vote-app.yaml
```

Im Git-Repository sind zwei Verzeichnisse enthalten. Eines enthält einen Helm-Chart, und das andere enthält die Konfigurationsdatei für die Releases. Im Verzeichnis `releases/prod` enthält `azure-vote-app.yaml` wie im Folgenden gezeigt die Konfigurationsdatei „HelmRelease“:

```bash
 apiVersion: helm.fluxcd.io/v1
 kind: HelmRelease
 metadata:
   name: azure-vote-app
   namespace: prod
   annotations:
 spec:
   releaseName: azure-vote-app
   chart:
     git: https://github.com/Azure/arc-helm-demo
     path: charts/azure-vote
     ref: master
   values:
     image:
       repository: dstrebel/azurevote
       tag: v1
     replicaCount: 3
```

Die Konfigurationsdatei für das Helm-Release enthält die folgenden Felder:

- `metadata.name` ist erforderlich und muss den Kubernetes-Benennungskonventionen entsprechen.
- `metadata.namespace` ist optional und bestimmt, wo das Release erstellt wird.
- `spec.releaseName` ist optional, und der Releasename entspricht „$namespace-$name“, wenn kein Name angegeben wird.
- `spec.chart.path` ist das Verzeichnis, das das Chart enthält, das im Zusammenhang mit dem Repositorystamm steht.
- `spec.values` beschreibt benutzerdefinierte Varianten von Standardparameterwerten aus dem Chart selbst.

Die in der Datei „HelmRelease“ unter „spec.values“ angegebenen Optionen überschreiben die Optionen, die in der Datei „values.yaml“ der Chart-Quelle angegeben sind.

Weitere Informationen über die Datei „HelmRelease“ finden Sie in der offiziellen [Dokumentation zum Helm-Operator](https://docs.fluxcd.io/projects/helm-operator/en/1.0.0-rc9/references/helmrelease-custom-resource.html).

## <a name="create-a-configuration"></a>Erstellen einer Konfiguration

Mit der Azure CLI-Erweiterung für `k8sconfiguration` verknüpfen Sie den verbundenen Cluster mit dem Git-Beispielrepository. Geben Sie dieser Konfiguration den Namen `azure-voting-app`, und stellen Sie den Flux-Operator im `prod`-Namespace bereit.

```bash
az k8sconfiguration create --name azure-voting-app --resource-group  $RESOURCE_GROUP --cluster-name $CLUSTER_NAME --operator-instance-name azure-voting-app --operator-namespace prod --enable-helm-operator --helm-operator-version='0.6.0' --helm-operator-params='--set helm.versions=v3' --repository-url https://github.com/Azure/arc-helm-demo.git --operator-params='--git-readonly --git-path=releases/prod' --scope namespace --cluster-type connectedClusters
```

### <a name="configuration-parameters"></a>Konfigurationsparameter

[Erfahren Sie mehr die zusätzliche Parameter](./use-gitops-connected-cluster.md#additional-parameters), um die Erstellung der Konfiguration anzupassen.


## <a name="validate-the-configuration"></a>Überprüfen der Konfiguration

Überprüfen Sie mithilfe der Azure CLI, ob die `sourceControlConfiguration`-Ressource erfolgreich erstellt wurde.

```console
az k8sconfiguration show --resource-group $RESOURCE_GROUP --name azure-voting-app --cluster-name $CLUSTER_NAME --cluster-type connectedClusters
```

Beachten Sie, dass die `sourceControlConfiguration`-Ressource mit einem Compliancestatus, Nachrichten und Debuginformationen aktualisiert wird.

**Ausgabe:**

```console
Command group 'k8sconfiguration' is in preview. It may be changed/removed in a future release.
{
  "complianceStatus": {
    "complianceState": "Compliant",
    "lastConfigApplied": "2019-12-05T05:34:41.481000",
    "message": "...",
    "messageLevel": "3"
  },
  "id": "/subscriptions/57ac26cf-a9f0-4908-b300-9a4e9a0fb205/resourceGroups/AzureArcTest/providers/Microsoft.Kubernetes/connectedClusters/AzureArcTest1/providers/Microsoft.KubernetesConfiguration/sourceControlConfigurations/cluster-config",
  "name": "azure-vote-app",
  "operatorInstanceName": "cluster-config",
  "operatorNamespace": "prod",
  "operatorParams": "--git-readonly --git-path=releases/prod",
  "operatorScope": "namespace",
  "operatorType": "Flux",
  "provisioningState": "Succeeded",
  "repositoryPublicKey": "...",
  "repositoryUrl": "git://github.com/Azure/arc-helm-demo.git",
  "resourceGroup": "AzureArcTest",
  "type": "Microsoft.KubernetesConfiguration/sourceControlConfigurations"
}
```

## <a name="validate-application"></a>Überprüfen der Anwendung

Jetzt überprüfen Sie, ob die Anwendung betriebsbereit ist, indem Sie die Dienst-IP abrufen.

```bash
kubectl get svc/azure-vote-front -n prod
```

**Ausgabe:**

```bash
NAME               TYPE           CLUSTER-IP    EXTERNAL-IP      PORT(S)        AGE
azure-vote-front   LoadBalancer   10.0.14.161   52.186.160.216   80:30372/TCP   4d22h
```

Suchen Sie die externe IP-Adresse in der obigen Ausgabe, und öffnen Sie sie in einem Browser.

## <a name="next-steps"></a>Nächste Schritte

- [Verwenden von Azure Policy zum Steuern der Clusterkonfiguration](./use-azure-policy.md)
