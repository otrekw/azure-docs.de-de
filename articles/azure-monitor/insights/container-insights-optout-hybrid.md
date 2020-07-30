---
title: Beenden der Überwachung Ihres Kubernetes-Hybridclusters | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie die Überwachung Ihres Kubernetes-Hybridclusters mit Azure Monitor für Container beenden können.
ms.topic: conceptual
ms.date: 06/16/2020
ms.openlocfilehash: 8369c82b83cfbaa7128383c6203aaf584916cae9
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87091197"
---
# <a name="how-to-stop-monitoring-your-hybrid-cluster"></a>Beenden der Überwachung Ihres Hybridclusters

Die Überwachung Ihres Kubernetes-Clusters kann nach der Aktivierung wieder beendet werden, wenn Sie den Cluster nicht mehr mit Azure Monitor für Container überwachen möchten. Dieser Artikel erläutert die Vorgehensweise für eine der folgenden Umgebungen:

- AKS-Engine in Azure und Azure Stack
- OpenShift-Version 4 und höher
- Kubernetes mit Azure Arc-Aktivierung (Vorschauversion)

## <a name="how-to-stop-monitoring-using-helm"></a>Beenden der Überwachung mithilfe von Helm

Die folgenden Schritte gelten für folgende Umgebungen:

- AKS-Engine in Azure und Azure Stack
- OpenShift-Version 4 und höher

1. Führen Sie den folgenden Helm-Befehl aus, um zunächst das auf Ihrem Cluster installierte Azure Monitor für Container-Helm-Chartrelease zu identifizieren.

    ```
    helm list
    ```

    Die Ausgabe ähnelt der folgenden:

    ```
    NAME                            NAMESPACE       REVISION        UPDATED                                 STATUS          CHART                           APP VERSION
    azmon-containers-release-1      default         3               2020-04-21 15:27:24.1201959 -0700 PDT   deployed        azuremonitor-containers-2.7.0   7.0.0-1
    ```

    *azmon-containers-release-1* stellt das Helm-Chartrelease für Azure Monitor für Container dar.

2. Führen Sie den folgenden Helm-Befehl aus, um das Chartrelease zu löschen.

    `helm delete <releaseName>`

    Beispiel:

    `helm delete azmon-containers-release-1`

    Dadurch wird das Release aus dem Cluster entfernt. Sie können dies überprüfen, indem Sie den `helm list`-Befehl ausführen:

    ```
    NAME                            NAMESPACE       REVISION        UPDATED                                 STATUS          CHART                           APP VERSION
    ```

Die Änderung der Konfiguration kann einige Minuten dauern. Da Helm Ihre Releases auch nach dem Löschen nachverfolgt, können Sie den Verlauf eines Clusters überwachen und ein Release mit `helm rollback` sogar wiederherstellen.

## <a name="how-to-stop-monitoring-on-arc-enabled-kubernetes"></a>Vorgehensweise zum Beenden der Überwachung von auf Arc aktiviertem Kubernetes

### <a name="using-powershell"></a>PowerShell

1. Laden Sie das Skript herunter, und speichern Sie es in einem lokalen Ordner, der Ihren Cluster über die folgenden Befehle mit dem Überwachungs-Add-on konfiguriert:

    ```powershell
    wget https://aka.ms/disable-monitoring-powershell-script -OutFile disable-monitoring.ps1
    ```

2. Konfigurieren Sie die `$azureArcClusterResourceId`-Variable, indem Sie die entsprechenden Werte für `subscriptionId`, `resourceGroupName` und `clusterName` festlegen, die die Ressourcen-ID Ihrer auf Azure Arc aktivierten Kubernetes-Clusterressource darstellen.

    ```powershell
    $azureArcClusterResourceId = "/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.Kubernetes/connectedClusters/<clusterName>"
    ```

3. Konfigurieren Sie die `$kubeContext`-Variable mit dem **kube-context** Ihres Clusters mit dem Befehl `kubectl config get-contexts`. Wenn Sie den aktuellen Kontext verwenden möchten, legen Sie den Wert auf `""` fest.

    ```powershell
    $kubeContext = "<kubeContext name of your k8s cluster>"
    ```

4. Führen Sie den folgenden Befehl aus, um die Überwachung des Clusters zu beenden.

    ```powershell
    .\disable-monitoring.ps1 -clusterResourceId $azureArcClusterResourceId -kubeContext $kubeContext
    ```

### <a name="using-bash"></a>Verwenden von Bash

1. Laden Sie das Skript herunter, und speichern Sie es in einem lokalen Ordner, der Ihren Cluster über die folgenden Befehle mit dem Überwachungs-Add-on konfiguriert:

    ```bash
    curl -o disable-monitoring.sh -L https://aka.ms/disable-monitoring-bash-script
    ```

2. Konfigurieren Sie die `azureArcClusterResourceId`-Variable, indem Sie die entsprechenden Werte für `subscriptionId`, `resourceGroupName` und `clusterName` festlegen, die die Ressourcen-ID Ihrer auf Azure Arc aktivierten Kubernetes-Clusterressource darstellen.

    ```bash
    export azureArcClusterResourceId="/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.Kubernetes/connectedClusters/<clusterName>"
    ```

3. Konfigurieren Sie die `kubeContext`-Variable mit dem **kube-context** Ihres Clusters mit dem Befehl `kubectl config get-contexts`.

    ```bash
    export kubeContext="<kubeContext name of your k8s cluster>"
    ```

4. Um die Überwachung Ihres Clusters zu beenden, werden je nach Bereitstellungsszenario verschiedene Befehle bereitgestellt.

    Führen Sie den folgenden Befehl aus, um die Überwachung des Clusters mit dem aktuellen Kontext zu beenden.

    ```bash
    bash disable-monitoring.sh --resource-id $azureArcClusterResourceId
    ```

    Führen Sie den folgenden Befehl aus, um die Überwachung des Clusters durch Angabe eines Kontexts zu beenden.

    ```bash
    bash disable-monitoring.sh --resource-id $azureArcClusterResourceId --kube-context $kubeContext
    ```

## <a name="next-steps"></a>Nächste Schritte

Wenn der Log Analytics-Arbeitsbereich nur als Unterstützung für die Clusterüberwachung erstellt wurde und nicht mehr benötigt wird, müssen Sie ihn manuell löschen. Wie ein Arbeitsbereich gelöscht wird, erfahren Sie unter [Löschen eines Log Analytics-Arbeitsbereichs](../platform/delete-workspace.md).
