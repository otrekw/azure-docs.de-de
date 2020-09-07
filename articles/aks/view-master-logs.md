---
title: Anzeigen von Azure Kubernetes Service-Controllerprotokollen (AKS)
description: Erfahren Sie, wie die Protokolle für den Kubernetes-Masterknoten in Azure Kubernetes Service (AKS) aktiviert und angezeigt werden.
services: container-service
ms.topic: article
ms.date: 01/03/2019
ms.openlocfilehash: 721ef4f60d263602b01b5957bfb9bc3b5682a2df
ms.sourcegitcommit: 8a7b82de18d8cba5c2cec078bc921da783a4710e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2020
ms.locfileid: "89048277"
---
# <a name="enable-and-review-kubernetes-master-node-logs-in-azure-kubernetes-service-aks"></a>Aktivieren und Überprüfen der Kubernetes-Masterknotenprotokolle in Azure Kubernetes Service (AKS)

Mit Azure Kubernetes Service (AKS) werden die Masterkomponenten wie *kube-apiserver* und *kube-controller-manager* als verwalteter Dienst bereitgestellt. Sie erstellen und verwalten die Knoten, die *kubelet* und die Containerruntime ausführen, und stellen Ihre Anwendungen über den Managed Kubernetes-API-Server bereit. Zur Behandlung von Problemen in Ihrer Anwendung und den Diensten müssen Sie möglicherweise die Protokolle anzeigen, die von diesen Masterkomponenten generiert wurden. In diesem Artikel wird veranschaulicht, wie Sie mit Azure Monitor-Protokollen die Protokolle der Kubernetes-Masterkomponenten aktivieren und abfragen.

## <a name="before-you-begin"></a>Voraussetzungen

Dieser Artikel setzt einen vorhandenen AKS-Cluster voraus, der in Ihrem Azure-Konto ausgeführt wird. Sollten Sie noch nicht über einen AKS-Cluster verfügen, erstellen Sie einen AKS-Cluster über die [Azure-Befehlszeilenschnittstelle][cli-quickstart] oder über das [Azure-Portal][portal-quickstart]. Azure Monitor-Protokolle funktionieren mit für RBAC und nicht für RBAC aktivierten AKS-Clustern.

## <a name="enable-resource-logs"></a>Aktivieren von Ressourcenprotokollen

Um Daten aus mehreren Quellen zu sammeln und zu überprüfen, bieten Azure Monitor-Protokolle eine Abfragesprache und ein Analysemodul, das Einblicke in Ihre Umgebung bereitstellt. Ein Arbeitsbereich wird verwendet, um die Daten zu sortieren und zu analysieren. Zudem können Sie andere Azure-Dienste wie Application Insights und Security Center integrieren. Um für die Analyse der Protokolle eine andere Plattform zu verwenden, können Sie stattdessen Ressourcenprotokolle an ein Azure-Speicherkonto oder einen Event Hub senden. Weitere Informationen finden Sie unter [Was sind Azure Monitor-Protokolle?][log-analytics-overview].

Azure Monitor-Protokolle werden im Azure-Portal aktiviert und verwaltet. Öffnen Sie zum Aktivieren der Protokollsammlung für die Kubernetes-Masterkomponenten in Ihrem AKS-Cluster das Azure-Portal in einem Webbrowser, und führen Sie die folgenden Schritte aus:

1. Wählen Sie die Ressourcengruppe für Ihren AKS-Cluster aus, z.B. *myResourceGroup*. Wählen Sie nicht die Ressourcengruppe aus, die Ihre einzelnen AKS-Clusterressourcen enthält, z.B. *MC_myResourceGroup_myAKSCluster_eastus*.
1. Wählen Sie auf der linken Seite **Diagnoseeinstellungen** aus.
1. Wählen Sie Ihren AKS-Cluster aus, z.B. *myAKSCluster*, wählen Sie dann **Diagnoseeinstellungen hinzufügen** aus.
1. Geben Sie einen Namen (etwa *myAKSClusterLogs*) ein, und wählen Sie dann die Option **An Log Analytics senden** aus.
1. Wählen Sie einen vorhandenen Arbeitsbereich aus, oder erstellen Sie einen neuen. Wenn Sie einen Arbeitsbereich erstellen, geben Sie einen Arbeitsbereichsnamen, eine Ressourcengruppe und einen Speicherort an.
1. Wählen Sie in der Liste der verfügbaren Protokolle die Protokolle aus, die Sie aktivieren möchten. Aktivieren Sie für dieses Beispiel die *kube-audit*-Protokolle. Zu den üblichen Protokollen gehören *kube-apiserver*, *kube-controller-manager* und *kube-scheduler*. Sie können zurückkehren und die gesammelten Protokolle ändern, nachdem Log Analytics-Arbeitsbereiche aktiviert wurden.
1. Wenn Sie fertig sind, wählen Sie **Speichern** aus, um die Sammlung der ausgewählten Protokolle zu aktivieren.

## <a name="schedule-a-test-pod-on-the-aks-cluster"></a>Planen eines Testpods im AKS-Cluster

Um einige Protokolle zu generieren, erstellen Sie einen neuen Pod in Ihrem AKS-Cluster. Das folgende Beispiel-YAML-Manifest kann verwendet werden, um eine einfache NGINX-Instanz zu erstellen. Erstellen Sie eine Datei mit dem Namen `nginx.yaml` in einem Editor Ihrer Wahl, und fügen Sie den folgenden Inhalt ein:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: mypod
    image: nginx:1.15.5
    resources:
      requests:
        cpu: 100m
        memory: 128Mi
      limits:
        cpu: 250m
        memory: 256Mi
    ports:
    - containerPort: 80
```

Erstellen Sie den Pod mit dem Befehl [kubectl create][kubectl-create], und geben Sie Ihre YAML-Datei an, wie im folgenden Beispiel gezeigt:

```
$ kubectl create -f nginx.yaml

pod/nginx created
```

## <a name="view-collected-logs"></a>Anzeigen der gesammelten Protokolle

Es kann einige Minuten dauern, bis die Diagnoseprotokolle aktiviert und angezeigt werden. Navigieren Sie im Azure-Portal zu Ihrem AKS-Cluster, und wählen Sie **Protokolle** auf der linken Seite aus. Schließen Sie das Fenster *Beispielabfragen*, wenn es angezeigt wird.


Wählen Sie auf der linken Seite **Protokolle** aus. Geben Sie zum Anzeigen der *cube-audit*-Protokolle die folgende Abfrage in das Textfeld ein:

```
AzureDiagnostics
| where Category == "kube-audit"
| project log_s
```

Wahrscheinlich werden viele Protokolle zurückgegeben. Um die Abfrage so einzuschränken, dass die Protokolle zu dem im vorherigen Schritt erstellten NGINX-Pod angezeigt werden, fügen Sie eine zusätzliche *where*-Anweisung hinzu, um nach *nginx* zu suchen, wie in der folgenden Beispielabfrage dargestellt:

```
AzureDiagnostics
| where Category == "kube-audit"
| where log_s contains "nginx"
| project log_s
```

Um weitere Protokolle anzuzeigen, können Sie die Abfrage für den *Category*-Namen in *kube-controller-manager* oder *kube-scheduler* ändern, je nachdem, welche weiteren Protokolle Sie aktivieren. Zusätzliche *where*-Anweisungen können dann verwendet werden, um die gesuchten Ereignisse zu verfeinern.

Weitere Informationen zum Abfragen und Filtern Ihrer Protokolldaten finden Sie unter [Anzeigen oder Analysieren der mit der Log Analytics-Protokollsuche gesammelten Daten][analyze-log-analytics].

## <a name="log-event-schema"></a>Protokollereignisschema

Als Unterstützung beim Analysieren der Protokolldaten wird in der folgenden Tabelle das für die einzelnen Ereignisse verwendete Schema erläutert:

| Feldname               | BESCHREIBUNG |
|--------------------------|-------------|
| *Ressourcen-ID*             | Azure-Ressource, die das Protokoll erstellt hat |
| *time*                   | Zeitstempel des Hochladens des Protokolls |
| *category*               | Name des Containers/der Komponente, der bzw. die das Protokoll generiert |
| *operationName*          | Immer *Microsoft.ContainerService/managedClusters/diagnosticLogs/Read* |
| *properties.log*         | Vollständiger Text des Protokolls von der Komponente |
| *properties.stream*      | *stderr* oder *stdout* |
| *properties.pod*         | Podname, von dem das Protokoll stammt |
| *properties.containerID* | ID des Docker-Containers, aus dem dieses Protokoll stammt |

## <a name="log-roles"></a>Protokollrollen

| Role                     | BESCHREIBUNG |
|--------------------------|-------------|
| *aksService*             | Der Anzeigename im Überwachungsprotokoll für den Vorgang auf der Steuerungsebene (aus „hcpService“). |
| *masterclient*           | Der Anzeigename im Überwachungsprotokoll für „MasterClientCertificate“ (das Zertifikat, das von „az aks get-credentials“ zurückgegeben wird). |
| *nodeclient*             | Der Anzeigename für „ClientCertificate“ (wird von Agent-Knoten verwendet). |

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie gelernt, wie die Protokolle für die Kubernetes-Masterkomponenten in Ihrem AKS-Cluster aktiviert und überprüft werden. Für eine zusätzliche Überwachung und Problembehandlung können Sie auch [die Kubelet-Protokolle anzeigen][kubelet-logs] und [SSH-Knotenzugriff aktivieren][aks-ssh].

<!-- LINKS - external -->
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create

<!-- LINKS - internal -->
[cli-quickstart]: kubernetes-walkthrough.md
[portal-quickstart]: kubernetes-walkthrough-portal.md
[log-analytics-overview]: ../azure-monitor/log-query/log-query-overview.md
[analyze-log-analytics]: ../azure-monitor/log-query/get-started-portal.md
[kubelet-logs]: kubelet-logs.md
[aks-ssh]: ssh.md
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
