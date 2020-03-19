---
title: Azure Monitor-Integration für Azure Red Hat OpenShift 4.3
description: Erfahren Sie, wie Sie Azure Monitor in Ihrem Microsoft Azure Red Hat OpenShift-Cluster aktivieren.
author: sakthi-vetrivel
ms.author: suvetriv
ms.service: container-service
ms.topic: conceptual
ms.date: 03/06/2020
ms.openlocfilehash: a784fc070400995c56d16a3bc264d589bcb1f64e
ms.sourcegitcommit: 72c2da0def8aa7ebe0691612a89bb70cd0c5a436
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "79082845"
---
# <a name="azure-monitor-integration-for-azure-red-hat-openshift-43"></a>Azure Monitor-Integration für Azure Red Hat OpenShift 4.3

> [!IMPORTANT] 
> Beachten Sie, dass Azure Red Hat OpenShift 4.3 derzeit nur in der privaten Vorschau in der Region „USA, Osten“ verfügbar ist. Die Annahme der privaten Vorschau ist nur auf Einladung möglich. Stellen Sie sicher, dass Sie Ihr Abonnement registrieren, bevor Sie versuchen, dieses Feature zu aktivieren: [Registrierung für die private Vorschau von Azure Red Hat OpenShift](https://aka.ms/aro-preview-register)

> [!NOTE]
> Vorschaufunktionen stehen gemäß dem Self-Service-Prinzip zur Verfügung, werden wie besehen und wie verfügbar bereitgestellt und sind von den Vereinbarungen zum Service Level (SLA) und der eingeschränkten Garantie ausgeschlossen. Daher sind die Funktionen nicht für den Einsatz in der Produktion vorgesehen.

In diesem Artikel wird beschrieben, wie Sie die private Vorschau von Azure Monitor für Container für OpenShift 4.3-Cluster aktivieren, die lokal oder in einer beliebigen Cloudumgebung gehostet werden. Die gleichen Anweisungen gelten auch für das Aktivieren der Überwachung für Azure Red Hat OpenShift (ARO) 4.3-Cluster.  

## <a name="prerequisites"></a>Voraussetzungen

- [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
- [Helm 3](https://helm.sh/docs/intro/install/)
- Zugriff auf „kubeconfig“ des Kubernetes-Clusters
- Zugriff auf ein Azure-Abonnement
- Zugriff auf den OpenShift 4.3-Cluster zum Installieren des Helm-Charts von Azure Monitor für Container
- Mindestens RBAC-Rollenberechtigung „Mitwirkender“ für das Azure-Abonnement  
- Der Überwachungs-Agent benötigt die folgenden ausgehenden Ports und Domänen zum Senden der Überwachungsdaten an das Azure Monitor-Back-End (falls durch Proxy/Firewall blockiert):
  - *.ods.opinsights.azure.com 443
  - *.oms.opinsights.azure.com 443
  - *.blob.core.windows.net 443
  - dc.services.visualstudio.com 443

## <a name="onboarding"></a>Onboarding

> [!TIP]
> Da das Skript Bash 4-Features verwendet, stellen Sie sicher, dass Ihr Bash auf dem neuesten Stand ist. Sie können Ihre aktuelle Version mit `bash --version` überprüfen.

### <a name="download-the-onboarding-script"></a>Herunterladen des Onboardingskripts

```bash
curl -LO  https://raw.githubusercontent.com/microsoft/OMS-docker/ci_feature/docs/openshiftV4/onboarding_azuremonitor_for_containers.sh
```

Führen Sie das folgende Skript mit den Werten des Kubernetes-Clusters für „azureSubscriptionId“, „Workspace Region“, „clusterName“ und „Context“ aus.

```bash
bash onboarding_azuremonitor_for_containers.sh <azureSubscriptionId> <azureRegionforLogAnalyticsWorkspace> <clusterName> <kubeconfigContextNameOftheCluster>
```

Beispiel:

```bash
 bash onboarding_azuremonitor_for_containers.sh 27ac26cf-a9f0-4908-b300-9a4e9a0fb205 eastus myocp42 admin 
```

## <a name="configure-agent-data-collection"></a>Konfigurieren der Datensammlung des Agents

Standardmäßig sammelt der Überwachungs-Agent die {stdout; stderr}-Containerprotokolle aller Container, die in allen Namespaces außer „kube-system“ ausgeführt werden.  Wenn Sie die Containerprotokollsammlung für einen bestimmten Namespace oder Namespaces konfigurieren möchten, finden Sie Informationen dazu unter [Konfigurieren des Container Insights-Agents](../azure-monitor/insights/container-insights-agent-config.md). Hier können Sie den Überwachungs-Agent mit den gewünschten Einstellungen für die Datensammlung mithilfe der ConfigMap konfigurieren.

## <a name="configure-scraping-of-prometheus-metrics"></a>Konfigurieren der Erfassung von Prometheus-Metriken

Azure Monitor für Container erfasst die Prometheus-Metriken für das Azure Monitor-Back-End. Anweisungen zum Konfigurieren der Prometheus-Erfassung finden Sie unter [Konfigurieren von Prometheus in Container Insights](../azure-monitor/insights/container-insights-prometheus-integration.md).

Nach dem erfolgreichen Onboarding navigieren Sie zu [Hybridüberwachung](https://aka.ms/azmon-containers-hybrid), und wählen Sie für „Umgebung“ die Option **Alle** aus, um den neu integrierten OpenShift v4-Cluster anzuzeigen.

## <a name="disable-monitoring"></a>Deaktivieren der Überwachung

Wenn Sie die Überwachung deaktivieren möchten, können Sie das Helm-Chart von Azure Monitor für Container löschen. Dazu verwenden Sie den folgenden Befehl, um die Erfassung von Überwachungsdaten für das Azure Monitor für Container-Back-End zu beenden.

``` bash
helm del azmon-containers-release-1
```

## <a name="update-monitoring"></a>Aktualisieren der Überwachung

Führen Sie zum Aktualisieren auf das neueste Helm-Chart das Onboardingskript wie im Abschnitt [Onboarding](#onboarding) beschrieben mit demselben Parameter aus.

## <a name="after-successful-onboarding"></a>Nach erfolgreichem Onboarding

Navigieren Sie zu [Hybridüberwachung](https://aka.ms/azmon-containers-hybrid). Hier wird der neu aktivierte OpenShift/ARO v4-Cluster mit Integritätsstatus auf der Registerkarte **Überwachte Cluster** angezeigt. Sie können tiefere Einblicke erhalten (z. B. Metriken, Bestand und Protokolle), indem Sie auf die Spalte **Cluster** klicken.

## <a name="supported-features"></a>Unterstützte Features

Weitere Informationen zu den unterstützten Features und Funktionen finden Sie in der [Übersicht über Container Insights](../azure-monitor/insights/container-insights-overview.md).

Bei Feedback und Fragen kontaktieren Sie uns über askcoin@microsoft.com.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Überwachung finden Sie unter folgenden Themen:
- [Übersicht über Container Insights](../azure-monitor/insights/container-insights-overview.md)

- [Übersicht über Protokollabfragen](../azure-monitor/log-query/log-query-overview.md)
