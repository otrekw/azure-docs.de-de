---
title: Durchführen des Onboardings für Azure Arc mit Azure Monitor für Container (Vorschauversion)
services: azure-arc
ms.service: azure-arc
ms.date: 05/19/2020
ms.topic: article
author: mlearned
ms.author: mlearned
description: Durchführen des Onboardings für Azure Arc mit Azure Monitor für Container
keywords: Kubernetes, Arc, Azure, K8s, Container
ms.openlocfilehash: 3e1ea96a9241211b25a4e5b356723290fa647412
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2020
ms.locfileid: "83680751"
---
# <a name="onboard-azure-monitor-for-containers-with-arc-preview"></a>Durchführen des Onboardings für Azure Monitor für Container mit Arc (Vorschauversion)

Hier erfahren Sie, wie Sie das Onboarding von [Azure Monitor-fähigen Containern](https://docs.microsoft.com/azure/azure-monitor/insights/container-insights-overview) in Kubernetes-Cluster mit Azure Arc-Aktivierung durchführen.

## <a name="before-you-begin"></a>Voraussetzungen

* [Kubernetes-Versionen](https://docs.microsoft.com/azure/aks/supported-kubernetes-versions)
* Linux-Distributionen für die Clusterknoten (Master und Worker): Ubuntu (18.04 LTS und 16.04 LTS)
* Mindestens RBAC-Rollenberechtigung „Mitwirkender“ für das Azure-Abonnement des Kubernetes-Clusters mit Azure Arc-Aktivierung
* Vollqualifizierte Azure-Ressourcen-ID des Kubernetes-Clusters mit Azure Arc-Aktivierung
* Kubeconfig-Kontext des Kubernetes-Clusters
* Für den Überwachungs-Agent muss cAdvisor auf dem Kubelet am sicheren Port 10250 oder am unsicheren Port 10255 auf allen Knoten ausgeführt werden, um die Leistungsmetriken pullen zu können.   
* Es empfiehlt sich, den Kubelet-cAdvisor-Port auf den sicheren Port 10250 festzulegen.
* Der Überwachungs-Agent benötigt die folgenden ausgehenden Ports und Domänen zum Senden der Überwachungsdaten an das Azure Monitor-Back-End (falls durch Proxy/Firewall blockiert):
    -  *.ods.opinsights.azure.com 443
    -  *.oms.opinsights.azure.com 443
    -  *.blob.core.windows.net 443
    -  dc.services.visualstudio.com 443

## <a name="onboarding"></a>Onboarding

### <a name="using-helm-chart"></a>Verwenden eines HELM-Charts

### <a name="option-1-using-powershell--script"></a>Option 1: Verwenden eines PowerShell-Skripts

1. Herunterladen des Onboardingskripts

    ```console
    curl -LO https://raw.githubusercontent.com/microsoft/OMS-docker/ci_feature/docs/haiku/onboarding_azuremonitor_for_containers.ps1
     ```

2. Installieren Sie [PowerShell Core](https://docs.microsoft.com/PowerShell/scripting/install/installing-PowerShell?view=PowerShell-6) auf Ihrem Entwicklercomputer, um das PowerShell-Onboardingskript auszuführen.

3. Anmelden an Azure

    ```console
    az login --use-device-code
    ```

4. Führen Sie das folgende Skript mit der Azure Arc-K8s-Clusterressourcen-ID Ihres Clusters und dem Kontext des Kubernetes-Clusters aus.

    ```console
    .\onboarding_azuremonitor_for_containers.ps1 -azureArcClusterResourceId <resourcedIdOfAzureArcCluster> -kubeContext <kube-context>

    For Example ..
    .\onboarding_azuremonitor_for_containers.ps1 -azureArcClusterResourceId /subscriptions/57ac26cf-a9f0-4908-b300-9a4e9a0fb205/resourceGroups/AzureArcTest/providers/Microsoft.Kubernetes/connectedClusters/AzureArcTest1 -kubeContext MyK8sTestCluster
     ```

### <a name="option-2-using-bash-script"></a>Option 2: Verwenden eines Bash-Skripts

> **Tipp:** Da das Skript Bash 4-Features verwendet, stellen Sie sicher, dass Ihr Bash auf dem neuesten Stand ist. Sie können Ihre aktuelle Version mit `bash --version` überprüfen.

1. Herunterladen des Onboardingskripts

    ```console
    curl -LO https://raw.githubusercontent.com/microsoft/OMS-docker/ci_feature/docs/haiku/onboarding_azuremonitor_for_containers.sh
     ```

2. Führen Sie das folgende Skript mit der Azure Arc-K8s-Clusterressourcen-ID Ihres Clusters und dem Kontext des Kubernetes-Clusters aus.

    ```console
    bash onboarding_azuremonitor_for_containers.sh <resourcedIdOfAzureArcCluster>  <kube-context>

    For Example:
    bash onboarding_azuremonitor_for_containers.sh /subscriptions/57ac26cf-a9f0-4908-b300-9a4e9a0fb205/resourceGroups/AzureArcTest/providers/Microsoft.Kubernetes/connectedClusters/AzureArcTest1 MyK8sTestCluster

     ```

## <a name="configure-agent-data-collection"></a>Konfigurieren der Datensammlung des Agents

Der Agent erfasst standardmäßig keine stdout- und stderr-Protokolle von Containern im Namespace „kube-system“.
Informationen zum Konfigurieren des Agents mit den gewünschten Datenerfassungseinstellungen finden Sie unter https://docs.microsoft.com/azure/azure-monitor/insights/container-insights-agent-config.

## <a name="configure-scraping-of-prometheus-metrics"></a>Konfigurieren der Erfassung von Prometheus-Metriken

Azure Monitor für Container erfasst die Prometheus-Metriken für das Azure Monitor-Back-End.
Eine Konfigurationsanleitung für die Prometheus-Erfassung finden Sie unter https://docs.microsoft.com/azure/azure-monitor/insights/container-insights-prometheus-integration.

## <a name="user-interface"></a>Benutzeroberfläche

Navigieren Sie zu https://aka.ms/azmon-containers-azurearc, um den Cluster anzuzeigen, für den das Onboarding durchgeführt wurde.

## <a name="disable-monitoring"></a>Deaktivieren der Überwachung

Wenn Sie die Überwachung deaktivieren möchten, können Sie ganz einfach das Helm-Chart von Azure Monitor für Container löschen, um die Erfassung von Überwachungsdaten im Back-End von Azure Monitor für Container zu beenden.

    ```console
    helm del azmon-containers-release-1
    ```

## <a name="next-steps"></a>Nächste Schritte

* [Verwenden von Azure Policy zum Steuern der Clusterkonfiguration](./use-azure-policy.md)

