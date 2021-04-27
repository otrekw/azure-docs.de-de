---
title: Planen und Bereitstellen von Kubernetes mit Azure Arc-Unterstützung
services: azure-arc
ms.service: azure-arc
ms.date: 04/12/2021
ms.topic: conceptual
author: shashankbarsin
ms.author: shasb
description: Durchführen des Onboardings für eine große Anzahl von Clustern in Kubernetes mit Azure Arc-Unterstützung für die Konfigurationsverwaltung
ms.openlocfilehash: 4b34cee08db508728f01d262ae4b1ee4ed4754e1
ms.sourcegitcommit: b4fbb7a6a0aa93656e8dd29979786069eca567dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2021
ms.locfileid: "107315194"
---
# <a name="plan-and-deploy-azure-arc-enabled-kubernetes"></a>Planen und Bereitstellen von Kubernetes mit Azure Arc-Unterstützung

Die Bereitstellung eines IT-Infrastrukturdiensts oder einer Geschäftsanwendung ist für jedes Unternehmen eine Herausforderung. Damit keine unangenehmen Überraschungen oder unerwartete Kosten auftreten, müssen Sie gründlich planen, sodass Sie bestmöglich vorbereitet sind. Ein solcher Plan sollte Entwurfs- und Bereitstellungskriterien identifizieren, die erfüllt sein müssen, um die Aufgaben auszuführen.

Damit die Bereitstellung problemlos fortgesetzt werden kann, sollte Ihr Plan folgende Punkte deutlich hervorheben:

* Rollen und Zuständigkeiten.
* Bestand aller Kubernetes-Cluster
* Erfüllen der Netzwerkanforderungen
* Die Fertigkeiten und Schulungen, die erforderlich sind, um eine erfolgreiche Bereitstellung und laufende Verwaltung zu ermöglichen.
* Akzeptanzkriterien und wie Sie den Erfolg verfolgen.
* Tools oder Methoden, die zur Automatisierung der Bereitstellungen verwendet werden sollen.
* Identifizierte Risiken und vorbeugende Maßnahmen, um Verzögerungen und Unterbrechungen zu vermeiden
* Vermeiden von Störungen während der Bereitstellung.
* Was ist der Eskalationspfad, wenn ein erhebliches Problem auftritt?

Dieser Artikel soll sicherstellen, dass Sie für eine erfolgreiche Bereitstellung von Kubernetes mit Azure Arc-Unterstützung in mehreren Produktionsclustern in Ihrer Umgebung vorbereitet sind.

## <a name="prerequisites"></a>Voraussetzungen

* Einen bestehenden Kubernetes-Cluster Wenn Sie über keinen Cluster verfügen, können Sie mit einer der folgenden Optionen einen erstellen:
    - [Kubernetes in Docker (KIND)](https://kind.sigs.k8s.io/)
    - Erstellen eines Kubernetes-Clusters mithilfe von Docker für [Mac](https://docs.docker.com/docker-for-mac/#kubernetes) oder [Windows](https://docs.docker.com/docker-for-windows/#kubernetes)
    - Selbst verwalteter Kubernetes-Cluster mithilfe der [Cluster-API](https://cluster-api.sigs.k8s.io/user/quick-start.html)

* Ihre Computer sind von Ihrem lokalen Netzwerk oder einer anderen Cloudumgebung aus entweder direkt oder über einen Proxyserver mit Ressourcen in Azure verbunden. Weitere Informationen finden Sie unter [Erfüllen von Netzwerkanforderungen](quickstart-connect-cluster.md#meet-network-requirements).

* Eine `kubeconfig`-Datei, die auf den Cluster verweist, den Sie mit Azure Arc verbinden möchten
* Lese- und Schreibberechtigungen für den Benutzer oder Dienstprinzipal, der den Kubernetes-Ressourcentyp `Microsoft.Kubernetes/connectedClusters` mit Azure Arc-Unterstützung erstellt

## <a name="pilot"></a>Pilotphase

Vor der Bereitstellung in allen Produktionsclustern sollten Sie diesen Bereitstellungsprozess zunächst auswerten, bevor Sie ihn in Ihrer Umgebung umfassend anwenden. Identifizieren Sie für eine Pilotphase eine repräsentative Stichprobe von Clustern, von denen nicht die Fähigkeit Ihres Unternehmens abhängt, den Geschäftsbetrieb aufrechtzuerhalten. Sie sollten sicher sein, dass Sie genügend Zeit für die Durchführung der Pilotphase und die Bewertung ihrer Auswirkungen haben: Wir empfehlen ca. 30 Tage.

Richten Sie einen formalen Plan ein, der den Umfang und die Details der Pilotphase beschreibt. Der folgende Beispielplan soll Sie bei den ersten Schritten unterstützen:

* **Ziele:** Beschreibt die geschäftlichen und technischen Anstöße, die zu der Entscheidung geführt haben, dass eine Pilotphase notwendig ist.
* **Auswahlkriterien:** Gibt die Kriterien zur Auswahl der Aspekte der Lösung an, die eine Pilotphase zeigen sollte.
* **Bereich:** Umfasst Lösungskomponenten, den erwarteten Zeitplan, die Dauer der Pilotphase und die Anzahl der zielorientierten Cluster.
* **Erfolgskriterien und Metriken:** Definiert die Erfolgskriterien der Pilotphase und bestimmte Measures, die zum Ermitteln des Erfolgsniveaus verwendet werden.
* **Trainingsplan:** Beschreibt den Plan für die Schulung von System Engineers, Administratoren usw., die im Rahmen des Pilotversuchs neu bei Azure und Azure-Diensten sind.
* **Übergangsplan:** Beschreibt die Strategie und die Kriterien, die für den Übergang von der Pilotphase zur Produktion verwendet werden.
* **Rollback:** Beschreibt die Prozedur für das Rollback einer Pilotphase in den Zustand vor der Bereitstellung.
* **Risiken:** Listet alle identifizierten Risiken für die Durchführung der Pilotphase und in Verbindung mit der Produktionsbereitstellung auf.

## <a name="phase-1-build-a-foundation"></a>Phase 1: Erstellen eines Fundaments

In dieser Phase führen System Engineers oder Administratoren die wichtigsten Aktivitäten aus, z. B. das Erstellen von Ressourcengruppen, Tags und Rollenzuweisungen, damit die Kubernetes-Ressourcen mit Azure Arc-Unterstützung erstellt und betrieben werden können.

|Aufgabe |Detail |Duration |
|-----|-------|---------|
| [Erstellen einer Ressourcengruppe](../../azure-resource-manager/management/manage-resource-groups-portal.md#create-resource-groups) | Eine dedizierte Ressourcengruppe, um nur Kubernetes-Ressourcen mit Azure Arc-Unterstützung einzubeziehen und die Verwaltung und Überwachung dieser Ressourcen zu zentralisieren | Eine Stunde |
| Anwenden von [Tags](../../azure-resource-manager/management/tag-resources.md) zur Unterstützung der Organisation von Computern. | Bewerten und entwickeln Sie eine IT-orientierte [Taggingstrategie](/azure/cloud-adoption-framework/decision-guides/resource-tagging/). Dies kann dazu beitragen, die Komplexität der Verwaltung Ihrer Kubernetes-Ressourcen mit Azure Arc-Unterstützung zu reduzieren und die Entscheidungsfindung für die Verwaltung zu vereinfachen. | Ein Tag |
| Identifizieren von [Konfigurationen](tutorial-use-gitops-connected-cluster.md) für GitOps | Identifizieren der Anwendungs- oder Baselinekonfigurationen wie `PodSecurityPolicy` oder `NetworkPolicy`, die Sie in Ihren Clustern bereitstellen möchten | Ein Tag |
| [Entwickeln eines Azure Policy](../../governance/policy/overview.md)-Governanceplans | Bestimmen Sie, wie Sie die Governance von Kubernetes-Clustern mit Azure Arc-Unterstützung im Abonnement- oder Ressourcengruppenbereich mit Azure Policy implementieren. | Ein Tag |
| Konfigurieren der [rollenbasierten Zugriffssteuerung](../../role-based-access-control/overview.md) (Role Based Access Control, RBAC) | Entwickeln eines Zugriffsplans, um zu ermitteln, wer über Lese-/Schreibberechtigungen oder alle Berechtigungen für Ihre Cluster verfügt | Ein Tag |

## <a name="phase-2-deploy-azure-arc-enabled-kubernetes"></a>Phase 2: Bereitstellen von Kubernetes mit Azure Arc-Unterstützung

In dieser Phase verbinden Sie Ihre Kubernetes-Cluster mit Azure:

|Aufgabe |Detail |Duration |
|-----|-------|---------|
| [Herstellen einer Verbindung zwischen Ihrem ersten Kubernetes-Cluster und Azure Arc](quickstart-connect-cluster.md) | Im Rahmen der Verbindungsherstellung Ihres ersten Clusters mit Azure Arc richten Sie Ihre Onboardingumgebung mit allen erforderlichen Tools wie Azure CLI, Helm und der Erweiterung `connectedk8s` für die Azure CLI ein. | 15 Minuten |
| [Erstellen eines Dienstprinzipals](create-onboarding-service-principal.md) | Erstellen Sie einen Dienstprinzipal, um Kubernetes-Cluster nicht interaktiv mithilfe der Azure CLI oder mithilfe von PowerShell zu verbinden. | Eine Stunde |


## <a name="phase-3-manage-and-operate"></a>Phase 3: Verwaltung und Betrieb

In dieser Phase stellen wir Anwendungen und Baselinekonfigurationen in Ihren Kubernetes-Clustern bereit.

|Aufgabe |Detail |Duration |
|-----|-------|---------|
|[Erstellen von Konfigurationen](tutorial-use-gitops-connected-cluster.md) in Ihren Clustern | Erstellen Sie Konfigurationen für die Bereitstellung Ihrer Anwendungen in Ihrer Kubernetes-Ressource mit Azure Arc-Unterstützung. | 15 Minuten |
|[Verwenden von Azure Policy](use-azure-policy.md) für die Erzwingung von Konfigurationen im großen Stil | Erstellen Sie Richtlinienzuweisungen, um die Bereitstellung von Baselinekonfigurationen in allen Ihren Clustern unter einem Abonnement- oder Ressourcengruppenbereich zu automatisieren. | 15 Minuten |
| [Upgrade von Azure Arc-Agents](agent-upgrade.md) | Wenn Sie das automatische Upgrade von Agents in Ihren Clustern deaktiviert haben, aktualisieren Sie Ihre Agents manuell auf die neueste Version, um sicherzustellen, dass Sie über die aktuellen Sicherheitskorrekturen und Fehlerbehebungen verfügen. | 15 Minuten |

## <a name="next-steps"></a>Nächste Schritte

* Nutzen Sie den Schnellstart zum [Verbinden eines Kubernetes-Clusters mit Azure Arc](./quickstart-connect-cluster.md).
* [Erstellen Sie Konfigurationen](./tutorial-use-gitops-connected-cluster.md) in Ihrem Kubernetes-Cluster mit Azure Arc-Unterstützung.
* [Verwenden Sie Azure Policy zum Anwenden von Konfigurationen im großen Stil.](./use-azure-policy.md)