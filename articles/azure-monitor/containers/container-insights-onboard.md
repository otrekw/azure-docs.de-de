---
title: Aktivieren von Azure Monitor für Container | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie Azure Monitor für Container aktivieren und konfigurieren, damit Sie erfahren, wie gut die Leistung Ihrer Container ist und welche leistungsbezogenen Probleme erkannt wurden.
ms.topic: conceptual
ms.date: 06/30/2020
ms.openlocfilehash: 56f60b58cff351aa37e98cdba933c929aaaedab6
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/17/2021
ms.locfileid: "100601963"
---
# <a name="enable-azure-monitor-for-containers"></a>Aktivieren von Azure Monitor für Container

In diesem Artikel finden Sie eine Übersicht der verfügbaren Optionen für das Einrichten von Azure Monitor für Container zum Überwachen der Leistung von Workloads, die in Kubernetes-Umgebungen bereitgestellt und folgendermaßen gehostet werden:

- [Azure Kubernetes Service (AKS)](../../aks/index.yml)  
- [Azure Red Hat OpenShift](../../openshift/intro-openshift.md), Version 3.x und 4.x  
- [Red Hat OpenShift](https://docs.openshift.com/container-platform/4.3/welcome/index.html) Version 4.x  
- [Arc-aktivierter Kubernetes-Cluster](../../azure-arc/kubernetes/overview.md)

Sie können auch die Leistung von Workloads überwachen, die für selbstverwaltete Kubernetes-Cluster bereitgestellt werden, deren Hosting folgendermaßen erfolgt:
- In Azure unter Verwendung der [AKS-Engine](https://github.com/Azure/aks-engine).
- [Azure Stack](/azure-stack/user/azure-stack-kubernetes-aks-engine-overview) oder lokal mithilfe der AKS-Engine.

Sie können Azure Monitor für Container für eine neue Bereitstellung oder für mindestens eine vorhandene Bereitstellung von Kubernetes aktivieren, indem Sie eine der folgenden unterstützten Methoden verwenden:

- Das Azure-Portal
- Azure PowerShell
- Die Azure-CLI
- [Terraform und AKS](/azure/developer/terraform/create-k8s-cluster-with-tf-and-aks)

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie beginnen, stellen Sie sicher, dass die folgenden Anforderungen erfüllt werden:

> [!IMPORTANT]
> Der Log Analytics-Agent für containerisiertes Linux (ReplicaSet-Pod) führt API-Aufrufe an alle Windows-Knoten am sicheren Kubelet-Port (10250) im Cluster aus, um Metriken zur Knoten- und Containerleistung zu erfassen. Der sichere Kubelet-Port (10250) sollte im virtuellen Netzwerk des Clusters ein- und ausgehend geöffnet sein, damit Metriken zur Leistung der Windows-Knoten und -Container erfasst werden können.
>
> Wenn Sie über einen Kubernetes-Cluster mit Windows-Knoten verfügen, überprüfen und konfigurieren Sie die Netzwerksicherheitsgruppe und Netzwerkrichtlinien, um sicherzustellen, dass der sichere Kubelet-Port (10250) für ein- und ausgehenden Datenverkehr im virtuellen Netzwerk des Clusters geöffnet ist.


- Sie verfügen über einen Log Analytics-Arbeitsbereich.

   Azure Monitor für Container unterstützt einen Log Analytics-Arbeitsbereich in den unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/?regions=all&products=monitor) aufgeführten Regionen.

   Sie können einen Arbeitsbereich beim Aktivieren der Überwachung des neuen AKS-Clusters erstellen oder beim Onboarding einen Standardarbeitsbereich in der Standardressourcengruppe des AKS-Clusterabonnements erstellen lassen. 
   
   Wenn Sie sich dafür entscheiden, den Arbeitsbereich selbst zu erstellen, können Sie ihn wie folgt einrichten: 
   - [Azure Resource Manager](../samples/resource-manager-workspace.md)
   - [PowerShell](../scripts/powershell-sample-create-workspace.md?toc=%2fpowershell%2fmodule%2ftoc.json)
   - [Azure-Portal](../learn/quick-create-workspace.md) 
   
   Eine Liste der unterstützten Zuordnungspaare, die für den Standardarbeitsbereich verwendet werden, finden Sie unter [Von Azure Monitor für Container unterstützte Regionszuordnungen](container-insights-region-mapping.md).

- Sie müssen der Gruppe *Log Analytics-Mitwirkender* angehören, um die Containerüberwachung aktivieren zu können. Weitere Informationen zur Zugriffssteuerung auf einen Log Analytics-Arbeitsbereich finden Sie unter [Verwalten von Arbeitsbereichen](../platform/manage-access.md).

- Sie sind ein Mitglied der Gruppe [*Besitzer*](../../role-based-access-control/built-in-roles.md#owner) für die AKS-Clusterressource.

   [!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

- Sie müssen über die Rolle [*Log Analytics-Leser*](../platform/manage-access.md#manage-access-using-azure-permissions) im mit Azure Monitor für Container konfigurierten Log Analytics-Arbeitsbereich verfügen, um die Überwachungsdaten anzeigen zu können.

- Prometheus-Metriken werden standardmäßig nicht erfasst. Bevor Sie [den Agent konfigurieren](container-insights-prometheus-integration.md), um die Metriken zu erfassen, sollten Sie sich unbedingt mit der [Prometheus-Dokumentation](https://prometheus.io/) befassen, um zu verstehen, welche Daten erfasst werden können und welche Methoden unterstützt werden.

## <a name="supported-configurations"></a>Unterstützte Konfigurationen

Azure Monitor für Container unterstützt offiziell die folgenden Konfigurationen:

- Umgebungen: Azure Red Hat OpenShift, Kubernetes lokal und AKS-Engine in Azure und Azure Stack. Weitere Informationen finden Sie unter [AKS-Engine in Azure Stack](/azure-stack/user/azure-stack-kubernetes-aks-engine-overview).
- Die Versionen von Kubernetes und Unterstützungsrichtlinien sind identisch mit denen, die [in Azure Kubernetes Service (AKS) unterstützt werden](../../aks/supported-kubernetes-versions.md). 

## <a name="network-firewall-requirements"></a>Netzwerkfirewallanforderungen

In der folgenden Tabelle sind die Proxy- und Firewallkonfigurationsinformationen aufgelistet, die für den Container-Agent zur Kommunikation mit Azure Monitor für Container benötigt werden. Der gesamte Netzwerkdatenverkehr vom Agent ist ausgehender Datenverkehr an Azure Monitor.

|Agent-Ressource|Port |
|--------------|------|
| `*.ods.opinsights.azure.com` | 443 |
| `*.oms.opinsights.azure.com` | 443 |
| `dc.services.visualstudio.com` | 443 |
| `*.monitoring.azure.com` | 443 |
| `login.microsoftonline.com` | 443 |

In der folgenden Tabelle sind die Proxy- und Firewallkonfigurationsinformationen für Azure China 21ViaNet aufgelistet:

|Agent-Ressource|Port |BESCHREIBUNG | 
|--------------|------|-------------|
| `*.ods.opinsights.azure.cn` | 443 | Datenerfassung |
| `*.oms.opinsights.azure.cn` | 443 | OMS-Onboarding |
| `dc.services.visualstudio.com` | 443 | Für Agent-Telemetrie, die Application Insights in der öffentlichen Azure-Cloud verwendet. |

In der folgenden Tabelle sind die Proxy- und Firewallkonfigurationsinformationen für Azure US Government aufgelistet:

|Agent-Ressource|Port |BESCHREIBUNG | 
|--------------|------|-------------|
| `*.ods.opinsights.azure.us` | 443 | Datenerfassung |
| `*.oms.opinsights.azure.us` | 443 | OMS-Onboarding |
| `dc.services.visualstudio.com` | 443 | Für Agent-Telemetrie, die Application Insights in der öffentlichen Azure-Cloud verwendet. |

## <a name="components"></a>Komponenten

Ihre Möglichkeit zum Überwachen der Leistung hängt von einem containerbasierten Log Analytics-Agent für Linux ab, der speziell für Azure Monitor für Container entwickelt wurde. Dieser spezialisierte Agent sammelt Leistungs- und Ereignisdaten von allen Knoten im Cluster, und der Agent wird während der Bereitstellung automatisch bereitgestellt und mit dem angegebenen Log Analytics-Arbeitsbereich registriert. 

Dabei wird die Agent-Version „microsoft/oms:ciprod04202018“ oder eine höhere Version bereitgestellt (dargestellt im Format *mmttjjjj*).

>[!NOTE]
>Mit der allgemeinen Verfügbarkeit von Windows Server-Unterstützung für AKS verfügt ein AKS-Cluster mit Windows Server-Knoten über einen Vorschau-Agent, der als Daemonset-Pod auf jedem einzelnen Windows Server-Knoten installiert ist, um Protokolle zu erfassen und an Log Analytics weiterzuleiten. Ein Linux-Knoten, der als Teil der Standardbereitstellung automatisch im Cluster bereitgestellt wird, erfasst die Daten der Leistungsmetriken und leitet diese für alle Windows-Knoten im Cluster an Azure Monitor weiter.

Wenn eine neue Version des Agents veröffentlicht wird, wird er in Ihren Managed Kubernetes-Clustern, die unter Azure Kubernetes Service (AKS) gehostet werden, automatisch aktualisiert. Informationen dazu, welche Versionen veröffentlicht werden, finden Sie unter [Agent-Releaseankündigungen](https://github.com/microsoft/docker-provider/tree/ci_feature_prod).

> [!NOTE]
> Wenn Sie bereits einen AKS-Cluster bereitgestellt haben, können Sie die Überwachung mithilfe der Azure CLI oder einer bereitgestellten Azure Resource Manager-Vorlage aktivieren (siehe weiter unten in diesem Artikel). Sie können mit `kubectl` kein Upgrade für den Agent durchführen oder diesen löschen, bereitstellen oder erneut bereitstellen.
>
> Die Vorlage muss in derselben Ressourcengruppe wie der Cluster bereitgestellt werden.

Verwenden Sie eine der Methoden verwenden, die in der folgenden Tabelle beschrieben werden, um Azure Monitor für Container zu aktivieren:

| Bereitstellungszustand | Methode | BESCHREIBUNG |
|------------------|--------|-------------|
| Neuer Kubernetes-Cluster | [Erstellen eines AKS-Clusters mithilfe der Azure CLI](../../aks/kubernetes-walkthrough.md#create-aks-cluster)| Sie können die Überwachung eines neuen AKS-Clusters aktivieren, den Sie mit der Azure CLI erstellen. |
| | [Erstellen eines AKS-Clusters mithilfe von Terraform](container-insights-enable-new-cluster.md#enable-using-terraform)| Sie können die Überwachung für einen neuen AKS-Cluster aktivieren, den Sie mithilfe des Open-Source-Tools Terraform erstellen. |
| | [Erstellen eines OpenShift-Clusters mithilfe einer Azure Resource Manager-Vorlage](container-insights-azure-redhat-setup.md#enable-for-a-new-cluster-using-an-azure-resource-manager-template) | Sie können die Überwachung für einen neuen OpenShift-Cluster, den Sie erstellen, mit einer vorkonfigurierten Azure Resource Manager-Vorlage aktivieren. |
| | [Erstellen eines OpenShift-Clusters mithilfe der Azure CLI](/cli/azure/openshift#az-openshift-create) | Sie können Überwachung aktivieren, wenn Sie einen neuen OpenShift-Cluster mithilfe der Azure CLI bereitstellen. |
| Vorhandener Kubernetes-Cluster | [Aktivieren der Überwachung eines AKS-Clusters mithilfe der Azure CLI](container-insights-enable-existing-clusters.md#enable-using-azure-cli) | Sie können die Überwachung für einen AKS-Cluster aktivieren, der bereits bereitgestellt ist, indem Sie die Azure CLI verwenden. |
| |[Aktivieren für AKS-Cluster mithilfe von Terraform](container-insights-enable-existing-clusters.md#enable-using-terraform) | Sie können die Überwachung eines bereits bereitgestellten AKS-Clusters mithilfe des Open-Source-Tools Terraform aktivieren. |
| | [Aktivieren für AKS-Cluster aus Azure Monitor](container-insights-enable-existing-clusters.md#enable-from-azure-monitor-in-the-portal)| Sie können Überwachung für mindestens einen bereits bereitgestellten AKS-Cluster über die Multi-Cluster-Seite in Azure Monitor aktivieren. |
| | [Aktivieren aus einem AKS-Cluster](container-insights-enable-existing-clusters.md#enable-directly-from-aks-cluster-in-the-portal)| Sie können die Überwachung direkt aus einem AKS-Cluster im Azure-Portal aktivieren. |
| | [Aktivieren für AKS-Cluster mithilfe einer Azure Resource Manager-Vorlage](container-insights-enable-existing-clusters.md#enable-using-an-azure-resource-manager-template)| Sie können die Überwachung für einen AKS-Cluster mit einer vorkonfigurierten Azure Resource Manager-Vorlage aktivieren. |
| | [Aktivieren für Kubernetes-Hybridcluster](container-insights-hybrid-setup.md) | Sie können die Überwachung für die AKS-Engine aktivieren, die unter Azure Stack gehostet wird, oder für einen lokal gehosteten Kubernetes-Cluster. |
| | [Aktivierung für Kubernetes-Cluster mit Arc-Aktivierung](container-insights-enable-arc-enabled-clusters.md). | Sie können Überwachung für Ihre Kubernetes-Cluster aktivieren, die außerhalb von Azure gehostet und mit Azure Arc aktiviert werden. |
| | [Aktivieren für OpenShift-Cluster mithilfe einer Azure Resource Manager-Vorlage](container-insights-azure-redhat-setup.md#enable-using-an-azure-resource-manager-template) | Sie können Überwachung für einen vorhandenen OpenShift-Cluster mit einer vorkonfigurierten Azure Resource Manager-Vorlage aktivieren. |
| | [Aktivieren für OpenShift-Cluster aus Azure Monitor](container-insights-azure-redhat-setup.md#from-the-azure-portal) | Sie können Überwachung für mindestens einen bereits bereitgestellten OpenShift-Cluster über die Multi-Cluster-Seite in Azure Monitor aktivieren. |

## <a name="next-steps"></a>Nächste Schritte

Da Sie Überwachung nun aktiviert haben, können Sie mit der Analyse der Leistung Ihrer Kubernetes-Cluster beginnen, die in Azure Kubernetes Service (AKS), Azure Stack oder in einer anderen Umgebung gehostet werden. Informationen zur Verwendung von Azure Monitor für Container finden Sie unter [Anzeigen der Leistung von Kubernetes-Clustern](container-insights-analyze.md).
