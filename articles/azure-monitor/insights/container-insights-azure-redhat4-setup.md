---
title: Konfigurieren von Azure Red Hat OpenShift, Version 4.x mit Azure Monitor für Container | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie die Überwachung eines in Azure Red Hat OpenShift, Version 4 und höher gehosteten Kubernetes-Clusters mit Azure Monitor konfigurieren können.
ms.topic: conceptual
ms.date: 04/22/2020
ms.openlocfilehash: 4b827524845874dabaabe535163d99c408f77a60
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82195749"
---
# <a name="configure-azure-red-hat-openshift-v4x-with-azure-monitor-for-containers"></a>Konfigurieren von Azure Red Hat OpenShift, Version 4.x mit Azure Monitor für Container

Azure Monitor für Container bietet umfassende Überwachungsfunktionen für Azure Kubernetes Service- (AKS) und AKS-Engine-Cluster. In diesem Artikel wird beschrieben, wie Sie die Überwachung von auf [Azure Red Hat OpenShift,](../../openshift/intro-openshift.md) Version 4.x gehosteten Kubernetes-Clustern aktivieren, um eine ähnliche Überwachung zu erreichen.

>[!NOTE]
>Die Unterstützung für Azure Red Hat OpenShift ist zurzeit als Feature in der öffentlichen Vorschauversion verfügbar.
>

Azure Monitor für Container kann für mindestens eine vorhandene Bereitstellung von Azure Red Hat OpenShift, Version 4.x mit den folgenden unterstützten Methoden aktiviert werden:

- Für einen vorhandenen Cluster, der das bereitgestellte Bash-Skript verwendet und in der [Azure CLI](https://docs.microsoft.com/cli/azure/openshift?view=azure-cli-latest#az-openshift-create)ausgeführt wird.

## <a name="supported-and-unsupported-features"></a>Unterstützte und nicht unterstützte Funktionen

Azure Monitor für Container unterstützt wie im Artikel [Übersicht](container-insights-overview.md) beschrieben die Überwachung von Azure Red Hat OpenShift, Version 4.x mit Ausnahme der folgenden Features:

- Livedaten (Vorschauversion)
- [Sammeln von Metriken](container-insights-update-metrics.md) von Clusterknoten und Pods sowie das Speichern in der Azure Monitor-Metrikdatenbank

## <a name="prerequisites"></a>Voraussetzungen

- Azure CLI, Version 2.0.72 oder höher

- CLI-Tool [Helm 3](https://helm.sh/docs/intro/install/)

- [Bash, Version 4](https://www.gnu.org/software/bash/)

- Befehlszeilentool [Kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

- Sie müssen in Ihrem Azure-Abonnement mindestens über die Azure-Rolle *Mitwirkender* und im Log Analytics-Arbeitsbereich über die mit Azure Monitor für Container konfigurierte Rolle [*Log Analytics-Mitwirkender*](../platform/manage-access.md#manage-access-using-azure-permissions) verfügen, um auf die Funktionen in Azure Monitor für Container zugreifen zu können.

- Sie müssen über die Berechtigungen der Rolle [*Log Analytics-Leser*](../platform/manage-access.md#manage-access-using-azure-permissions) mit dem mit Azure Monitor für Container konfigurierten Log Analytics-Arbeitsbereich verfügen, um die Überwachungsdaten anzuzeigen.

## <a name="enable-for-an-existing-cluster"></a>Aktivieren für einen vorhandenen Cluster

Führen Sie die folgenden Schritte aus, um die Überwachung eines in Azure bereitgestellten Clusters für Azure Red Hat OpenShift, Version 4 oder höher mithilfe des bereitgestellten Bash-Skripts zu aktivieren.

1. Anmelden bei Azure

    ```azurecli
    az login
    ```

2. Laden Sie das Skript herunter, und speichern Sie es in einem lokalen Ordner, der Ihren Cluster über die folgenden Befehle mit dem Überwachungs-Add-on konfiguriert:

    `curl -LO https://raw.githubusercontent.com/microsoft/OMS-docker/ci_feature/docs/aroV4/onboarding_azuremonitor_for_containers.sh.`

3. Führen Sie den Befehl `kubectl config current-context` aus, und kopieren Sie den Wert, um den **kube-context** Ihres Clusters nach erfolgreicher `oc login` zu identifizieren.

### <a name="integrate-with-an-existing-workspace"></a>Integrieren in einen vorhandenen Arbeitsbereich

Der folgende Schritt ermöglicht die Überwachung Ihres Clusters mithilfe des zuvor heruntergeladenen Bash-Skripts. Wenn Sie die Integration in einen vorhandenen Log Analytics-Arbeitsbereich vornehmen möchten, führen Sie die folgenden Schritte aus. Dabei ermitteln Sie zuerst die vollständige Ressourcen-ID Ihres Log Analytics-Arbeitsbereichs, die für den `workspaceResourceId`-Parameter erforderlich ist, und führen dann den Befehl zum Aktivieren des Überwachungs-Add-Ons für den angegebenen Arbeitsbereich aus. Wenn Sie nicht über einen Arbeitsbereich verfügen, gehen Sie direkt zu Schritt 5, und lassen Sie das Skript einen neuen Arbeitsbereich für Sie erstellen.

1. Listen Sie mit dem folgenden Befehl alle Abonnements auf, auf die Sie Zugriff haben:

    ```azurecli
    az account list --all -o table
    ```

    Die Ausgabe ähnelt der folgenden:

    ```azurecli
    Name                                  CloudName    SubscriptionId                        State    IsDefault
    ------------------------------------  -----------  ------------------------------------  -------  -----------
    Microsoft Azure                       AzureCloud   68627f8c-91fO-4905-z48q-b032a81f8vy0  Enabled  True
    ```

    Kopieren Sie den Wert für **SubscriptionId**.

2. Wechseln Sie mit dem folgenden Befehl zu dem Abonnement, das den Log Analytics-Arbeitsbereich hostet:

    ```azurecli
    az account set -s <subscriptionId of the workspace>
    ```

3. Im folgenden Beispiel wird die Liste mit den Arbeitsbereichen Ihrer Abonnements im JSON-Standardformat angezeigt.

    ```
    az resource list --resource-type Microsoft.OperationalInsights/workspaces -o json
    ```

    Suchen Sie in der Ausgabe nach dem Namen des Arbeitsbereichs, und kopieren Sie dann die vollständige Ressourcen-ID dieses Log Analytics-Arbeitsbereichs unter dem Feld **ID**.

4. Führen Sie den folgenden Befehl aus, um die Überwachung zu aktivieren, und ersetzen Sie dabei den Wert für den `workspaceResourceId`-Parameter: 

    `bash onboarding_azuremonitor_for_containers.sh <kube-context> <azureAroV4ResourceId> <LogAnayticsWorkspaceResourceId>`

    Beispiel:

    `bash onboarding_azuremonitor_for_containers.sh MyK8sTestCluster /subscriptions/57ac26cf-a9f0-4908-b300-9a4e9a0fb205/resourceGroups/test-aro-v4-rg/providers/Microsoft.RedHatOpenShift/OpenShiftClusters/test-aro-v4  /subscriptions/57ac26cf-a9f0-4908-b300-9a4e9a0fb205/resourcegroups/test-la-workspace-rg/providers/microsoft.operationalinsights/workspaces/test-la-workspace`

Nach dem Aktivieren der Überwachung kann es ca. 15 Minuten dauern, bis Integritätsmetriken für den Cluster angezeigt werden.

### <a name="integrate-with-default-workspace"></a>Integrieren in den Standardarbeitsbereich

Der folgende Schritt ermöglicht die Überwachung Ihres Clusters für Azure Red Hat OpenShift, Version 4.x mithilfe des heruntergeladenen Bash-Skripts. In diesem Beispiel müssen Sie keinen Arbeitsbereich erstellen oder einen vorhandenen angeben. Dieser Befehl vereinfacht den Prozess durch Erstellen eines Standardarbeitsbereichs in der Standardressourcengruppe des Clusterabonnements, wenn noch keiner in der Region vorhanden ist. Das Format des Standardarbeitsbereichs ähnelt *DefaultWorkspace-\<GUID>-\<Region>* .  

    `bash onboarding_azuremonitor_for_containers.sh <kube-context> <azureAroV4ResourceId>`

    For example:

    `bash onboarding_azuremonitor_for_containers.sh MyK8sTestCluster /subscriptions/57ac26cf-a9f0-4908-b300-9a4e9a0fb205/resourceGroups/test-aro-v4-rg/providers/Microsoft.RedHatOpenShift/OpenShiftClusters/test-aro-v4`

Nach dem Aktivieren der Überwachung kann es ca. 15 Minuten dauern, bis Integritätsmetriken für den Cluster angezeigt werden.

### <a name="from-the-azure-portal"></a>Über das Azure-Portal

In der Ansicht für mehrere Cluster in Azure Monitor für Container werden Ihre Azure Red Hat OpenShift-Cluster hervorgehoben, für die die Überwachung auf der Registerkarte **Nicht überwachte Cluster** nicht aktiviert ist. Durch die Option **Aktivieren** neben Ihrem Cluster wird kein Onboarding für die Überwachung über das Portal durchgeführt. Sie werden zu diesem Artikel weitergeleitet, um die Überwachung manuell zu aktivieren. Befolgen Sie hierzu die weiter oben in diesem Artikel beschriebenen Schritte.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Klicken Sie im Azure-Portalmenü oder auf der Homepage auf **Azure Monitor**. Wählen Sie im Abschnitt **Insights** die Option **Container** aus.

3. Wählen Sie auf der Seite **Überwachung – Container** die Option **Nicht überwachte Cluster** aus.

4. Suchen Sie in der Liste der nicht überwachten Cluster den Cluster, und klicken Sie auf **Aktivieren**. Sie können die Ergebnisse in der Liste ermitteln, indem Sie in der Spalte **CLUSTERTYP** nach dem Wert **ARO** suchen. Nachdem Sie auf **Aktivieren** geklickt haben, werden Sie zu diesem Artikel weitergeleitet.

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über die [Verwendung von Azure Monitor für Container](container-insights-analyze.md), um bei aktivierter Überwachung die Integrität und Ressourcennutzung Ihres Clusters für RedHat OpenShift, Version 4.x und der darauf ausgeführten Workloads zu erfassen.

- Standardmäßig sammelt der Container-Agent die Containerprotokolle stdout und stderr aller Container, die in allen Namespaces mit Ausnahme von kube-system ausgeführt werden. Wenn Sie die Containerprotokollsammlung bestimmter Namespaces konfigurieren möchten, finden Sie weitere Informationen unter [Agent-Konfiguration für Container Insights](container-insights-agent-config.md). In diesem Artikel wird beschrieben, wie Sie die gewünschten Einstellungen für die Datensammlung für Ihre ConfigMap-Konfigurationsdatei konfigurieren.

- Weitere Informationen zum Auslesen und Analysieren von Prometheus-Metriken aus Ihrem Cluster finden Sie unter [Konfigurieren des Abrufs von Prometheus-Metriken](container-insights-prometheus-integration.md).

- Informationen zum Beenden der Überwachung Ihres Clusters mit Azure Monitor für Container finden Sie unter [Beenden der Überwachung von Azure Red Hat OpenShift-Clustern](container-insights-optout-openshift.md).
