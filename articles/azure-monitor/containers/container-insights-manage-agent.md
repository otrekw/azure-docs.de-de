---
title: Verwalten des Container Insights-Agents | Microsoft-Dokumentation
description: In diesem Artikel wird die Verwaltung der häufigsten Wartungsaufgaben mit dem containerbasierten Log Analytics-Agent beschrieben, der von Container Insights verwendet wird.
ms.topic: conceptual
ms.date: 07/21/2020
ms.openlocfilehash: 2a0c32ef797a953eca794e16fe0ace5e967f339f
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101713795"
---
# <a name="how-to-manage-the-container-insights-agent"></a>Verwalten des Container Insights-Agents

Für Container Insights wird eine Containerversion des Log Analytics-Agents für Linux verwendet. Nach der ersten Bereitstellung gibt es Routine- oder optionale Aufgaben, die Sie während des Lebenszyklus ausführen müssen. In diesem Artikel wird beschrieben, wie Sie das Upgrade des Agent manuell durchführen und die Sammlung von Umgebungsvariablen aus einem bestimmten Container deaktivieren können. 

## <a name="how-to-upgrade-the-container-insights-agent"></a>Durchführen eines Upgrades für den Container Insights-Agent

Für Container Insights wird eine Containerversion des Log Analytics-Agents für Linux verwendet. Wenn eine neue Version des Agents veröffentlicht wird, wird automatisch ein Upgrade des Agents in Ihren Managed Kubernetes-Clustern ausgeführt, die in Azure Kubernetes Service (AKS) und Azure Red Hat OpenShift Version 3.x gehostet werden. Bei einem [Kubernetes-Hybridcluster](container-insights-hybrid-setup.md) und Azure Red Hat OpenShift Version 4.x wird der Agent nicht verwaltet, und Sie müssen ein manuelles Upgrade des Agents durchführen.

In diesem Artikel wird außerdem der Prozess für das manuelle Upgrade des Agents beschrieben, falls während des Upgrades ein Fehler bei einem in AKS oder Azure Red Hat OpenShift Version 3.x gehosteten Cluster auftritt. Informationen zu den freigegebenen Versionen finden Sie unter [Agent-Versionsankündigungen](https://github.com/microsoft/docker-provider/tree/ci_feature_prod).

### <a name="upgrade-agent-on-aks-cluster"></a>Upgrade des Agents im AKS-Cluster

Der Prozess für das Upgrade des Agents in AKS-Clustern besteht aus zwei einfachen Schritten. Der erste Schritt besteht darin, die Überwachung mithilfe der Azure CLI in Container Insights zu deaktivieren. Führen Sie die Schritte aus, die im Artikel [Deaktivieren der Überwachung](container-insights-optout.md?#azure-cli) beschrieben sind. Indem wir die Azure CLI verwenden, können wir den Agent von den Knoten im Cluster entfernen, ohne dass sich Auswirkungen auf die Lösung und die entsprechenden Daten ergeben, die im Arbeitsbereich gespeichert sind. 

>[!NOTE]
>Während der Durchführung dieser Wartungsaktivität leiten die Knoten im Cluster keine gesammelten Daten weiter, und die Leistungsansichten zeigen keine Daten für den Zeitraum an, in dem Sie den Agent entfernen und die neue Version installieren. 
>

Führen Sie zum Installieren der neuen Agent-Version die Schritte aus, die im Artikel zum [Aktivieren der Überwachung mit der Azure CLI](container-insights-enable-new-cluster.md#enable-using-azure-cli) beschrieben sind, um diesen Vorgang abzuschließen.  

Nach dem erneuten Aktivieren der Überwachung kann es ca. 15 Minuten dauern, bis aktualisierte Integritätsmetriken für den Cluster angezeigt werden. Sie haben folgende Möglichkeiten, um zu überprüfen, ob das Agent-Upgrade erfolgreich war:

* Führen Sie den Befehl `kubectl get pod <omsagent-pod-name> -n kube-system -o=jsonpath='{.spec.containers[0].image}'` aus. Achten Sie im zurückgegebenen Status auf den Wert für „omsagent“ unter **Image** im Abschnitt *Container* der Ausgabe.
* Wählen Sie auf der Registerkarte **Knoten** den Clusterknoten aus, und sehen Sie sich rechts im Bereich **Eigenschaften** den Wert unter **Agent-Imagetag** an.

Die Version des angezeigten Agents sollte der aktuellen Version entsprechen, die auf der Seite [Releaseverlauf](https://github.com/microsoft/docker-provider/tree/ci_feature_prod) aufgeführt ist.

### <a name="upgrade-agent-on-hybrid-kubernetes-cluster"></a>Upgrade des Agents im Kubernetes-Hybridcluster

Führen Sie die folgenden Schritte aus, um ein Upgrade des Agents in einem Kubernetes-Cluster in Folgendem durchzuführen:

* Selbstverwaltete Kubernetes-Cluster, die in Azure mit der AKS-Engine gehostet werden
* Selbstverwaltete Kubernetes-Cluster, die in Azure Stack oder lokal mit der AKS-Engine gehostet werden
* Red Hat OpenShift Version 4.x

Wenn sich der Log Analytics-Arbeitsbereich in kommerziellem Azure befindet, führen Sie den folgenden Befehl aus:

```console
$ helm upgrade --name myrelease-1 \
--set omsagent.secret.wsid=<your_workspace_id>,omsagent.secret.key=<your_workspace_key>,omsagent.env.clusterName=<my_prod_cluster> incubator/azuremonitor-containers
```

Wenn sich der Log Analytics-Arbeitsbereich in Azure China 21Vianet befindet, führen Sie den folgenden Befehl aus:

```console
$ helm upgrade --name myrelease-1 \
--set omsagent.domain=opinsights.azure.cn,omsagent.secret.wsid=<your_workspace_id>,omsagent.secret.key=<your_workspace_key>,omsagent.env.clusterName=<your_cluster_name> incubator/azuremonitor-containers
```

Wenn sich der Log Analytics-Arbeitsbereich in einer Azure-Region von „US Government“ befindet, führen Sie den folgenden Befehl aus:

```console
$ helm upgrade --name myrelease-1 \
--set omsagent.domain=opinsights.azure.us,omsagent.secret.wsid=<your_workspace_id>,omsagent.secret.key=<your_workspace_key>,omsagent.env.clusterName=<your_cluster_name> incubator/azuremonitor-containers
```

### <a name="upgrade-agent-on-azure-red-hat-openshift-v4"></a>Upgrade des Agents in Azure Red Hat OpenShift v4

Führen Sie die folgenden Schritte aus, um ein Upgrade des Agents in einem Kubernetes-Cluster in Azure Red Hat OpenShift Version 4.x durchzuführen. 

>[!NOTE]
>Azure Red Hat OpenShift Version 4.x unterstützt nur die Ausführung in der kommerziellen Azure-Cloud.
>

```console
curl -o upgrade-monitoring.sh -L https://aka.ms/upgrade-monitoring-bash-script
export azureAroV4ClusterResourceId="/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.RedHatOpenShift/OpenShiftClusters/<clusterName>"
bash upgrade-monitoring.sh --resource-id $ azureAroV4ClusterResourceId
```

Ausführliche Informationen zur Verwendung eines Dienstprinzipals mit diesem Befehl finden Sie unter **Verwenden eines Dienstprinzipals** im Artikel [Aktivieren der Überwachung eines Kubernetes-Clusters mit Azure Arc-Aktivierung](container-insights-enable-arc-enabled-clusters.md#enable-using-bash-script).

### <a name="upgrade-agent-on-azure-arc-enabled-kubernetes"></a>Upgrade des Agents in Kubernetes mit Azure Arc-Aktivierung

Führen Sie den folgenden Befehl aus, um den Agent in einem Kubernetes-Cluster mit Azure Arc-Aktivierung zu aktualisieren.

```console
curl -o upgrade-monitoring.sh -L https://aka.ms/upgrade-monitoring-bash-script
export azureArcClusterResourceId="/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.Kubernetes/connectedClusters/<clusterName>"
bash upgrade-monitoring.sh --resource-id $azureArcClusterResourceId
```

Ausführliche Informationen zur Verwendung eines Dienstprinzipals mit diesem Befehl finden Sie unter **Verwenden eines Dienstprinzipals** im Artikel [Aktivieren der Überwachung eines Kubernetes-Clusters mit Azure Arc-Aktivierung](container-insights-enable-arc-enabled-clusters.md#enable-using-bash-script).


## <a name="how-to-disable-environment-variable-collection-on-a-container"></a>Deaktivieren der Sammlung von Umgebungsvariablen für einen Container

Container Insights sammelt Umgebungsvariablen von den in einem Pod ausgeführten Containern und stellt sie im Eigenschaftenbereich des ausgewählten Containers in der Ansicht **Container** dar. Sie können dieses Verhalten steuern, indem Sie die Sammlung für einen bestimmten Container entweder während der Bereitstellung des Kubernetes-Clusters oder danach deaktivieren, indem Sie die Umgebungsvariable *AZMON_COLLECT_ENV* festlegen. Dieses Feature ist ab der Agent-Version ciprod11292018 verfügbar.  

Um die Sammlung von Umgebungsvariablen für einen neuen oder vorhandenen Container zu deaktivieren, legen Sie für die Variable **AZMON_COLLECT_ENV** den Wert **False** in Ihrer yaml-Konfigurationsdatei für die Kubernetes-Bereitstellung fest. 

```yaml
- name: AZMON_COLLECT_ENV  
  value: "False"  
```

Führen Sie den folgenden Befehl aus, um die Änderung auf Kubernetes-Cluster (außer Azure Red Hat OpenShift) anzuwenden: `kubectl apply -f  <path to yaml file>`. Führen Sie den folgenden Befehl aus, um ConfigMap zu bearbeiten und diese Änderung für Azure Red Hat OpenShift-Cluster anzuwenden:

```bash
oc edit configmaps container-azm-ms-agentconfig -n openshift-azure-logging
```

Dadurch wird der standardmäßige Text-Editor geöffnet. Speichern Sie nach dem Festlegen der Variablen die Datei im Editor.

Wenn Sie überprüfen möchten, ob die Konfigurationsänderung wirksam ist, wählen Sie einen Container in der Ansicht **Container** in Container Insights aus, und erweitern Sie dann im Eigenschaftenbereich die Option **Umgebungsvariablen**.  Der Abschnitt sollte nur die zuvor erstellte Variable anzeigen: **AZMON_COLLECT_ENV=FALSE**. Für alle anderen Container sollte der Abschnitt „Umgebungsvariablen“ alle erkannten Umgebungsvariablen auflisten.

Um die Erkennung der Umgebungsvariablen wieder zu aktivieren, wenden Sie denselben vorherigen Prozess an, und ändern Sie den Wert von **False** in **True**. Anschließend wiederholen Sie den Befehl `kubectl`, um den Container zu aktualisieren.  

```yaml
- name: AZMON_COLLECT_ENV  
  value: "True"  
```  

## <a name="next-steps"></a>Nächste Schritte

Falls beim Aktualisieren des Agents Probleme auftreten, hilft Ihnen der [Leitfaden zur Problembehandlung](container-insights-troubleshoot.md) weiter.
