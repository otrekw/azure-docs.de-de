---
title: Konfigurieren von Kubernetes-Clustern mit Azure Arc-Aktivierung mit Azure Monitor für Container | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie mit Azure Monitor die Überwachung für Container auf Kubernetes-Clustern mit Azure Arc-Aktivierung konfigurieren können.
ms.topic: conceptual
ms.date: 06/23/2020
ms.openlocfilehash: 54a8fea6ddb46dc00fff29ad83a2a348d9218380
ms.sourcegitcommit: 07166a1ff8bd23f5e1c49d4fd12badbca5ebd19c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/15/2020
ms.locfileid: "90090617"
---
# <a name="enable-monitoring-of-azure-arc-enabled-kubernetes-cluster"></a>Aktivieren der Überwachung eines Kubernetes-Clusters mit Azure Arc-Aktivierung

Azure Monitor für Container bietet umfassende Überwachungsfunktionen für Azure Kubernetes Service- (AKS) und AKS-Engine-Cluster. In diesem Artikel wird beschrieben, wie Sie die Überwachung Ihrer Kubernetes-Cluster mit Azure Arc-Aktivierung, die außerhalb von Azure gehostet werden, aktivieren und eine ähnliche Überwachung erreichen können.

Azure Monitor für Container kann für mindestens eine vorhandene Bereitstellung von Kubernetes entweder mit einem PowerShell- oder einem Bash-Skript aktiviert werden.

## <a name="supported-configurations"></a>Unterstützte Konfigurationen

Azure Monitor für Container unterstützt wie im Artikel [Übersicht](container-insights-overview.md) beschrieben die Überwachung von Kubernetes mit Azure Arc-Aktivierung (Vorschau) mit Ausnahme der folgenden Features:

- Livedaten (Vorschauversion)

- [Sammeln von Metriken](container-insights-update-metrics.md) von Clusterknoten und Pods sowie das Speichern in der Azure Monitor-Metrikdatenbank

Folgendes wird mit Azure Monitor für Container offiziell unterstützt:

- Die Versionen von Kubernetes und die Supportrichtlinie entsprechen den unterstützten Versionen von [AKS](../../aks/supported-kubernetes-versions.md).

- Folgende Containerruntimes werden unterstützt: Docker-, Moby- und CRI-kompatible Runtimes wie CRI-O und ContainerD.

- Unterstützte Linux-Betriebssystemreleases für Master- und Workerknoten: Ubuntu (18.04 LTS und 16.04 LTS)

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie zunächst sicher, dass Sie über Folgendes verfügen:

- Einen Log Analytics-Arbeitsbereich

    Azure Monitor für Container unterstützt einen Log Analytics-Arbeitsbereich in den unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/?regions=all&products=monitor) aufgeführten Regionen in Azure. Ihren eigenen Arbeitsbereich können Sie über [Azure Resource Manager](../platform/template-workspace-configuration.md), über [PowerShell](../scripts/powershell-sample-create-workspace.md?toc=%2fpowershell%2fmodule%2ftoc.json) oder im [Azure-Portal](../learn/quick-create-workspace.md) erstellen.

- Sie müssen in Ihrem Azure-Abonnement mindestens über die Azure-Rolle *Mitwirkender* und im Log Analytics-Arbeitsbereich über die mit Azure Monitor für Container konfigurierte Rolle [*Log Analytics-Mitwirkender*](../platform/manage-access.md#manage-access-using-azure-permissions) verfügen, um auf die Funktionen in Azure Monitor für Container zugreifen zu können.

- Sie sind ein Mitglied der Rolle [Mitwirkender](../../role-based-access-control/built-in-roles.md#contributor) für die Azure Arc-Clusterressource.

- Sie müssen über die Berechtigungen der Rolle [*Log Analytics-Leser*](../platform/manage-access.md#manage-access-using-azure-permissions) mit dem mit Azure Monitor für Container konfigurierten Log Analytics-Arbeitsbereich verfügen, um die Überwachungsdaten anzuzeigen.

- [HELM-Client](https://helm.sh/docs/using_helm/) zum Integrieren des Azure Monitor für Containercharts für den angegebenen Kubernetes-Cluster

- Für die Kommunikation zwischen der Containerversion des Log Analytics-Agents für Linux und Azure Monitor sind die folgenden Proxy- und Firewallkonfigurationsinformationen erforderlich:

    |Agent-Ressource|Ports |
    |------|---------|
    |`*.ods.opinsights.azure.com` |Port 443 |
    |`*.oms.opinsights.azure.com` |Port 443 |
    |`*.dc.services.visualstudio.com` |Port 443 |

- Für den Container-Agent muss `cAdvisor secure port: 10250` oder `unsecure port :10255` von Kubelet auf allen Knoten im Cluster geöffnet werden, damit Leistungsmetriken erfasst werden können. Es wird empfohlen, `secure port: 10250` in cAdvisor von Kubelet zu konfigurieren, wenn dieser noch nicht konfiguriert ist.

- Für den Container-Agent müssen die folgenden Umgebungsvariablen im Container angegeben werden, damit die Kommunikation mit dem Kubernetes-API-Dienst innerhalb des Clusters zum Erfassen von Bestandsdaten möglich ist: `KUBERNETES_SERVICE_HOST` und `KUBERNETES_PORT_443_TCP_PORT`.

    >[!IMPORTANT]
    >Die für die Überwachung von Kubernetes-Clustern mit Azure Arc-Aktivierung unterstützte Agent-Mindestversion ist ciprod04162020 oder höher.

- [PowerShell Core](/powershell/scripting/install/installing-powershell?view=powershell-6) ist erforderlich, wenn Sie die Überwachung mit der PowerShell-Skriptmethode aktivieren.

- [Bash Version 4](https://www.gnu.org/software/bash/) ist erforderlich, wenn Sie die Überwachung mit der Bash-Skriptmethode aktivieren.

## <a name="identify-workspace-resource-id"></a>Identifizieren der Arbeitsbereichsressourcen-ID

Wenn Sie die Überwachung Ihres Clusters mithilfe eines zuvor heruntergeladenen PowerShell- oder Bash-Skripts aktivieren und die Integration in einen vorhandenen Log Analytics-Arbeitsbereich vornehmen möchten, führen Sie die folgenden Schritte aus, um die vollständige Ressourcen-ID Ihres Log Analytics-Arbeitsbereichs zu ermitteln. Dies ist für den Parameter `workspaceResourceId` erforderlich, wenn Sie den Befehl ausführen, um das Überwachungs-Add-On für den angegebenen Arbeitsbereich zu aktivieren. Wenn Sie über keinen Arbeitsbereich verfügen, den Sie angeben können, überspringen Sie das Einfügen des Parameters `workspaceResourceId`, und lassen Sie das Skript einen neuen Arbeitsbereich für Sie erstellen.

1. Listen Sie mit dem folgenden Befehl alle Abonnements auf, auf die Sie Zugriff haben:

    ```azurecli
    az account list --all -o table
    ```

    Die Ausgabe ähnelt der folgenden:

    ```azurecli
    Name                                  CloudName    SubscriptionId                        State    IsDefault
    ------------------------------------  -----------  ------------------------------------  -------  -----------
    Microsoft Azure                       AzureCloud   0fb60ef2-03cc-4290-b595-e71108e8f4ce  Enabled  True
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

## <a name="enable-monitoring-using-powershell"></a>Aktivieren der Überwachung mithilfe von PowerShell

1. Laden Sie das Skript herunter, und speichern Sie es in einem lokalen Ordner, der Ihren Cluster über die folgenden Befehle mit dem Überwachungs-Add-on konfiguriert:

    ```powershell
    wget https://aka.ms/enable-monitoring-powershell-script -outfile enable-monitoring.ps1
    ```

2. Konfigurieren Sie die `$azureArcClusterResourceId`-Variable, indem Sie die entsprechenden Werte für `subscriptionId`, `resourceGroupName` und `clusterName` festlegen, die die Ressourcen-ID Ihrer Kubernetes-Clusterressource mit Azure Arc-Aktivierung darstellen.

    ```powershell
    $azureArcClusterResourceId = "/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.Kubernetes/connectedClusters/<clusterName>"
    ```

3. Konfigurieren Sie die `$kubeContext`-Variable mit dem **kube-context** Ihres Clusters mit dem Befehl `kubectl config get-contexts`. Wenn Sie den aktuellen Kontext verwenden möchten, legen Sie den Wert auf `""` fest.

    ```powershell
    $kubeContext = "<kubeContext name of your k8s cluster>"
    ```

4. Wenn Sie einen vorhandenen Azure Monitor Log Analytics-Arbeitsbereich verwenden möchten, konfigurieren Sie die Variable `$logAnalyticsWorkspaceResourceId` mit dem entsprechenden Wert, der die Ressourcen-ID des Arbeitsbereichs darstellt. Legen Sie andernfalls die Variable auf `""` fest, und das Skript erstellt einen Standardarbeitsbereich in der Standardressourcengruppe des Clusterabonnements, wenn noch keiner in der Region vorhanden ist. Das Format des Standardarbeitsbereichs ähnelt *DefaultWorkspace-\<SubscriptionID>-\<Region>* .

    ```powershell
    $logAnalyticsWorkspaceResourceId = "/subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/microsoft.operationalinsights/workspaces/<workspaceName>"
    ```

5. Wenn Ihr Kubernetes-Cluster mit Arc-Aktivierung über einen Proxyserver kommuniziert, konfigurieren Sie die Variable `$proxyEndpoint` mit der URL des Proxyservers. Wenn der Cluster nicht über einen Proxyserver kommuniziert, können Sie den Wert auf `""` festlegen.  Weitere Informationen finden Sie unter [Konfigurieren des Proxyendpunkts](#configure-proxy-endpoint) weiter unten in diesem Artikel.

6. Führen Sie den folgenden Befehl aus, um die Überwachung zu aktivieren.

    ```powershell
    .\enable-monitoring.ps1 -clusterResourceId $azureArcClusterResourceId -kubeContext $kubeContext -workspaceResourceId $logAnalyticsWorkspaceResourceId -proxyEndpoint $proxyEndpoint
    ```

Nach dem Aktivieren der Überwachung kann es ca. 15 Minuten dauern, bis Integritätsmetriken für den Cluster angezeigt werden.

## <a name="enable-using-bash-script"></a>Aktivieren mithilfe eines Bash-Skripts

Führen Sie die folgenden Schritte durch, um die Überwachung mithilfe des bereitgestellten Bash-Skripts zu aktivieren.

1. Laden Sie das Skript herunter, und speichern Sie es in einem lokalen Ordner, der Ihren Cluster über die folgenden Befehle mit dem Überwachungs-Add-on konfiguriert:

    ```bash
    curl -o enable-monitoring.sh -L https://aka.ms/enable-monitoring-bash-script
    ```

2. Konfigurieren Sie die `azureArcClusterResourceId`-Variable, indem Sie die entsprechenden Werte für `subscriptionId`, `resourceGroupName` und `clusterName` festlegen, die die Ressourcen-ID Ihrer Kubernetes-Clusterressource mit Azure Arc-Aktivierung darstellen.

    ```bash
    export azureArcClusterResourceId="/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.Kubernetes/connectedClusters/<clusterName>"
    ```

3. Konfigurieren Sie die `kubeContext`-Variable mit dem **kube-context** Ihres Clusters mit dem Befehl `kubectl config get-contexts`. Wenn Sie den aktuellen Kontext verwenden möchten, legen Sie den Wert auf `""` fest.

    ```bash
    export kubeContext="<kubeContext name of your k8s cluster>"
    ```

4. Wenn Sie einen vorhandenen Azure Monitor Log Analytics-Arbeitsbereich verwenden möchten, konfigurieren Sie die Variable `logAnalyticsWorkspaceResourceId` mit dem entsprechenden Wert, der die Ressourcen-ID des Arbeitsbereichs darstellt. Legen Sie andernfalls die Variable auf `""` fest, und das Skript erstellt einen Standardarbeitsbereich in der Standardressourcengruppe des Clusterabonnements, wenn noch keiner in der Region vorhanden ist. Das Format des Standardarbeitsbereichs ähnelt *DefaultWorkspace-\<SubscriptionID>-\<Region>* .

    ```bash
    export logAnalyticsWorkspaceResourceId=“/subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/microsoft.operationalinsights/workspaces/<workspaceName>”
    ```

5. Wenn Ihr Kubernetes-Cluster mit Arc-Aktivierung über einen Proxyserver kommuniziert, konfigurieren Sie die Variable `proxyEndpoint` mit der URL des Proxyservers. Wenn der Cluster nicht über einen Proxyserver kommuniziert, können Sie den Wert auf `""` festlegen. Weitere Informationen finden Sie unter [Konfigurieren des Proxyendpunkts](#configure-proxy-endpoint) weiter unten in diesem Artikel.

6. Um die Überwachung Ihres Clusters zu aktivieren, werden je nach Bereitstellungsszenario verschiedene Befehle bereitgestellt.

    Führen Sie den folgenden Befehl aus, um die Überwachung mit Standardoptionen zu aktivieren, wie z. B. das Verwenden des aktuellen „kube-context“ oder das Erstellen eines standardmäßigen Log Analytics-Arbeitsbereichs ohne Angabe eines Proxyservers:

    ```bash
    bash enable-monitoring.sh --resource-id $azureArcClusterResourceId
    ```

    Führen Sie den folgenden Befehl aus, um einen standardmäßigen Log Analytics-Arbeitsbereich ohne Angabe eines Proxyservers zu erstellen:

    ```bash
   bash enable-monitoring.sh --resource-id $azureArcClusterResourceId --kube-context $kubeContext
    ```

    Führen Sie den folgenden Befehl aus, um einen vorhandenen Log Analytics-Arbeitsbereich ohne Angabe eines Proxyservers zu verwenden:

    ```bash
    bash enable-monitoring.sh --resource-id $azureArcClusterResourceId --kube-context $kubeContext  --workspace-id $logAnalyticsWorkspaceResourceId
    ```

    Führen Sie den folgenden Befehl aus, um einen vorhandenen Log Analytics-Arbeitsbereich zu verwenden und einen Proxyserver anzugeben:

    ```bash
    bash enable-monitoring.sh --resource-id $azureArcClusterResourceId --kube-context $kubeContext  --workspace-id $logAnalyticsWorkspaceResourceId --proxy $proxyEndpoint
    ```

Nach dem Aktivieren der Überwachung kann es ca. 15 Minuten dauern, bis Integritätsmetriken für den Cluster angezeigt werden.

## <a name="configure-proxy-endpoint"></a>Konfigurieren des Proxyendpunkts

Beim containerisierten Agent für Azure Monitor für Container können Sie einen Proxyendpunkt so konfigurieren, dass er über Ihren Proxyserver kommunizieren kann. Die Kommunikation zwischen dem containerisierten Agent und Azure Monitor kann über einen HTTP- oder HTTPS-Proxyserver erfolgen. Außerdem werden die anonyme und die Standardauthentifizierung (mit Benutzername und Kennwort) unterstützt.

Der Wert für die Proxykonfiguration weist die folgende Syntax auf: `[protocol://][user:password@]proxyhost[:port]`.

> [!NOTE]
>Wenn für den Proxyserver keine Authentifizierung erforderlich ist, müssen Sie dennoch einen Pseudobenutzernamen und ein Pseudokennwort angeben. Dies kann ein beliebiger Benutzername oder ein beliebiges Kennwort sein.

|Eigenschaft| BESCHREIBUNG |
|--------|-------------|
|Protocol | HTTP oder HTTPS |
|user | Optionaler Benutzername für die Proxyauthentifizierung |
|password | Optionales Kennwort für die Proxyauthentifizierung |
|proxyhost | Adresse oder FQDN des Proxyservers |
|port | Optionale Portnummer für den Proxyserver |

Beispiel: `http://user01:password@proxy01.contoso.com:3128`

Wenn Sie das Protokoll als **http** angeben, werden die HTTP-Anforderungen mit einer sicheren SSL/TLS-Verbindung erstellt. Der Proxyserver muss SSL/TLS-Protokolle unterstützen.

### <a name="configure-using-powershell"></a>Konfigurieren mithilfe von PowerShell

Geben Sie den Benutzernamen und das Kennwort, die IP-Adresse oder den FQDN und die Portnummer für den Proxyserver an. Beispiel:

```powershell
$proxyEndpoint = https://<user>:<password>@<proxyhost>:<port>
```

### <a name="configure-using-bash"></a>Konfigurieren mithilfe von Bash

Geben Sie den Benutzernamen und das Kennwort, die IP-Adresse oder den FQDN und die Portnummer für den Proxyserver an. Beispiel:

```bash
export proxyEndpoint=https://<user>:<password>@<proxyhost>:<port>
```

## <a name="next-steps"></a>Nächste Schritte

- Nach erfolgter Aktivierung der Überwachung zum Erfassen der Integrität und Ressourcennutzung Ihres Kubernetes-Clusters mit Arc-Aktivierung und der darin ausgeführten Workloads finden Sie hier [Informationen zur Verwendung](container-insights-analyze.md) von Azure Monitor für Container.

- Standardmäßig sammelt der Container-Agent die Containerprotokolle stdout und stderr aller Container, die in allen Namespaces mit Ausnahme von kube-system ausgeführt werden. Wenn Sie die Containerprotokollsammlung bestimmter Namespaces konfigurieren möchten, finden Sie weitere Informationen unter [Agent-Konfiguration für Container Insights](container-insights-agent-config.md). In diesem Artikel wird beschrieben, wie Sie die gewünschten Einstellungen für die Datensammlung für Ihre ConfigMap-Konfigurationsdatei konfigurieren.

- Weitere Informationen zum Auslesen und Analysieren von Prometheus-Metriken aus Ihrem Cluster finden Sie unter [Konfigurieren des Abrufs von Prometheus-Metriken](container-insights-prometheus-integration.md).

- Informationen zum Beenden der Überwachung Ihres Kubernetes-Clusters mit Arc-Aktivierung mit Azure Monitor für Container finden Sie unter [Vorgehensweise zum Beenden der Überwachung von auf Arc aktiviertem Kubernetes](container-insights-optout-hybrid.md#how-to-stop-monitoring-on-arc-enabled-kubernetes).
