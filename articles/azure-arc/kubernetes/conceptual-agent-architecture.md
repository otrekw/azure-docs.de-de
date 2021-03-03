---
title: Architektur eines Azure Arc-fähigen Kubernetes-Agents
services: azure-arc
ms.service: azure-arc
ms.date: 02/15/2021
ms.topic: conceptual
author: shashankbarsin
ms.author: shasb
description: Dieser Artikel enthält eine Übersicht über die Architekturen Azure Arc-fähiger Kubernetes-Agents.
keywords: Kubernetes, Arc, Azure, Container
ms.openlocfilehash: e1f51f066598b59501b30704cb1475dd5332160e
ms.sourcegitcommit: de98cb7b98eaab1b92aa6a378436d9d513494404
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/17/2021
ms.locfileid: "100561171"
---
# <a name="azure-arc-enabled-kubernetes-agent-architecture"></a>Architektur eines Azure Arc-fähigen Kubernetes-Agents

[Kubernetes](https://kubernetes.io/) kann verwendet werden, um containerisierte Workloads in Hybridumgebungen und Umgebungen mit mehreren Clouds auf konsistente Weise bereitzustellen. Azure Arc-fähiges Kubernetes kann als zentralisierte Steuerungsebene verwendet werden, um Richtlinien, Governance und Sicherheit in diesen heterogenen Umgebungen konsistent zu verwalten. Dieser Artikel enthält:

* Eine Übersicht der Architekturen für das Verbinden eines Clusters mit Azure Arc.
* Das Konnektivitätsmuster, das Agents einhalten.
* Eine Beschreibung der zwischen der Clusterumgebung und Azure ausgetauschten Daten.

## <a name="deploy-agents-to-your-cluster"></a>Bereitstellen von Agents in Ihrem Cluster

Die meisten lokalen Rechenzentren erzwingen strikte Netzwerkregeln, die eingehende Kommunikation an der Firewall verhindern, die an der Netzwerkgrenze verwendet wird. Azure Arc-fähiges Kubernetes arbeitet mit diesen Einschränkungen, indem nur selektive Ausgangsendpunkte für die ausgehende Kommunikation aktiviert werden und keine eingehenden Ports in der Firewall erforderlich sind. Azure Arc-fähige Kubernetes-Agents initiieren die ausgehenden Verbindungen.

![Übersicht über die Architektur](./media/architectural-overview.png)

Verbinden Sie einen Cluster mit Azure Arc, indem Sie die folgenden Schritte ausführen:

1. Erstellen Sie einen Kubernetes-Cluster auf der von Ihnen gewünschten Infrastruktur (VMware vSphere, Amazon Web Services, Google Cloud Platform usw.). 

    > [!NOTE]
    > Kunden müssen den Lebenszyklus des Kubernetes-Clusters selbst erstellen und verwalten, da Azure Arc-fähiges Kubernetes derzeit nur das Anfügen vorhandener Kubernetes-Cluster an Azure Arc unterstützt.  

1. Initiieren Sie die Azure Arc-Registrierung für Ihren Cluster mithilfe der Azure CLI.
    * Azure CLI verwendet Helm, um das Agent-Helm-Chart im Cluster bereitzustellen.
    * Die Clusterknoten initiieren eine ausgehende Kommunikation mit der [Microsoft Container Registry](https://github.com/microsoft/containerregistry) und rufen die Images ab, die zum Erstellen der folgenden Agents im `azure-arc`-Namespace erforderlich sind:

        | Agent | BESCHREIBUNG |
        | ----- | ----------- |
        | `deployment.apps/clusteridentityoperator` | Azure Arc-fähiges Kubernetes unterstützt derzeit nur [systemseitig zugewiesene Identitäten](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview). „clusteridentityoperator“ führt die erste ausgehende Kommunikation aus, die erforderlich ist, um das Zertifikat der verwalteten Dienstidentität (MSI) abzurufen, das von anderen Agents für die Kommunikation mit Azure verwendet wird. |
        | `deployment.apps/config-agent` | Überwacht den verbundenen Cluster auf Quellcodeverwaltungsressourcen, die auf den Cluster angewendet werden, und aktualisiert den Compliancezustand. |
        | `deployment.apps/controller-manager` | Ein Operator von Operatoren, der Interaktionen zwischen Azure Arc-Komponenten orchestriert. |    
        | `deployment.apps/metrics-agent` | Erfasst Metriken anderer Arc-Agents, um sicherzustellen, dass diese Agents eine optimale Leistung aufweisen. |
        | `deployment.apps/cluster-metadata-operator` | Erfasst Clustermetadaten: Clusterversion, Knotenanzahl und Version des Azure Arc-Agents. |
        | `deployment.apps/resource-sync-agent` | Synchronisiert die oben erwähnten Clustermetadaten mit Azure. |
        | `deployment.apps/flux-logs-agent` | Sammelt Protokolle von den bei der Konfiguration der Quellcodeverwaltung bereitgestellten Flux-Operatoren. |
    
1. Stellen Sie sicher, dass Ihr Cluster mit Azure Arc verbunden ist, sobald alle Azure Arc-fähigen Kubernetes-Agent-Pods den Zustand `Running` haben. Folgendes sollte angezeigt werden:
    * Eine Azure Arc-fähige Kubernetes-Ressource in [Azure Resource Manager](../../azure-resource-manager/management/overview.md). Diese Ressource wird in Azure als Projektion des kundenseitig verwalteten Kubernetes-Clusters nachverfolgt, nicht der eigentliche Kubernetes-Cluster selbst.
    * Clustermetadaten wie Kubernetes-Version, Agent-Version und Anzahl der Knoten werden in der Azure Arc-fähigen Kubernetes-Ressource als Metadaten angezeigt.

## <a name="data-exchange-between-cluster-environment-and-azure"></a>Datenaustausch zwischen Clusterumgebung und Azure

| Datentyp | Szenario | Kommunikationsmodus |
| --------- | -------- | ------------------ |
| Kubernetes-Clusterversion | Clustermetadaten | Agent-Pushvorgänge zu Azure |
| Anzahl der Knoten im Cluster | Clustermetadaten | Agent-Pushvorgänge an Azure |
| Agent-Version | Clustermetadaten | Agent-Pushvorgänge an Azure |
| Kubernetes-Verteilungstyp | Clustermetadaten | Azure CLI-Pushvorgänge an Azure |
| Infrastrukturtyp (AWS/GCP/vSphere/...) | Clustermetadaten | Azure CLI-Pushvorgänge an Azure |
| vCPU-Anzahl der Knoten im Cluster | Abrechnung | Azure CLI-Pushvorgänge an Azure |
| Agent-Takt | Resource Health | Agent-Pushvorgänge an Azure |
| Ressourcenverbrauch (Arbeitsspeicher/CPU) nach Agents | Diagnose und Unterstützbarkeit | Agent-Pushvorgänge an Azure |
| Protokolle aller Agent-Container | Diagnose und Unterstützbarkeit | Agent-Pushvorgänge an Azure |
| Upgrade-Verfügbarkeit für Agent | Agent-Upgrade | Agent-Pullvorgänge von Azure |
| Gewünschter Zustand der Konfiguration: Git-Repository-URL, Flux-Operatorparameter, privater Schlüssel, Inhalt bekannter Hosts, HTTPS-Benutzername, Token/Kennwort | Konfiguration | Agent-Pullvorgänge von Azure |
| Status der Flux-Operatorinstallation | Konfiguration | Agent-Pushvorgänge an Azure |
| Azure Policy-Zuweisungen, die Gatekeeper-Erzwingung innerhalb des Clusters benötigen. | Azure Policy | Agent-Pullvorgänge von Azure |
| Überwachungs- und Compliancestatus von Richtlinienerzwingungen im Cluster | Azure Policy | Agent-Pushvorgänge an Azure |
| Metriken und Protokolle von Kundenworkloads | Azure Monitor | Agent-Pushvorgänge an Log Analytics-Arbeitsbereichsressource im Mandanten und Abonnement des Kunden. |

## <a name="connectivity-status"></a>Konnektivitätsstatus

| Status | BESCHREIBUNG |
| ------ | ----------- |
| Verbindung | Azure Arc-fähige Kubernetes-Ressource wurde in Azure Resource Manager erstellt, aber der Dienst hat noch keine Agent-Takt empfangen. |
| Verbunden | Azure Arc-fähiger Kubernetes-Dienst hat irgendwann während der vorangegangenen 15 Minuten einen Agent-Takt empfangen. |
| Offline | Azure Arc-fähig Kubernetes-Ressource war zuvor verbunden, aber der Dienst hat 15 Minuten lang keinen Agent-Takt mehr empfangen. |
| Abgelaufen | Das Zertifikat der verwalteten Dienstidentität (MSI) besitzt ein Ablauffenster von 90 Tagen nach seiner Ausgabe. Nach Ablauf dieses Zertifikats wird die Ressource als `Expired` betrachtet, und alle Features wie Konfiguration, Überwachung und Richtlinie stellen in diesem Cluster die Funktion ein. Weitere Informationen zum Behandeln abgelaufener Azure Arc-fähiger Kubernetes-Ressourcen finden Sie [hier](./faq.md#how-to-address-expired-azure-arc-enabled-kubernetes-resources). |

## <a name="understand-connectivity-modes"></a>Grundlegendes zu Konnektivitätsmodi

| Konnektivitätsmodus | BESCHREIBUNG |
| ----------------- | ----------- |
| Vollständig verbunden | Agents können jederzeit Azure erreichen. Die Erfahrung ist in diesem Fall ideal, da es nur zu geringen Verzögerungen bei der Weitergabe von Konfigurationen (für GitOps), der Erzwingung von Richtlinien (in Azure Policy und Gatekeeper) und der Erfassung von Metriken und Protokollen von Workloads (in Azure Monitor) kommt. |
| Halb verbunden | Das von `clusteridentityoperator` abgerufene MSI-Zertifikat ist maximal für 90 Tage gültig, bevor es abläuft. Sobald das Zertifikat abläuft, stellt die Azure Arc-fähige Kubernetes-Ressource den Betrieb ein. Löschen Sie die Azure Arc-fähige Kubernetes-Ressource und die Agents, und erstellen Sie sie neu, damit alle Arc-Features auf dem Cluster funktionieren. Während der 90 Tage wird Benutzern empfohlen, den Cluster mindestens einmal alle 30 Tage zu verbinden. |
| Getrennt | Kubernetes-Cluster in nicht verbundenen Umgebungen ohne Zugriff auf Azure werden zurzeit von Azure Arc-fähigem Kubernetes nicht unterstützt. Wenn diese Funktion für Sie von Interesse ist, reichen Sie eine Idee im [UserVoice-Forum von Azure Arc](https://feedback.azure.com/forums/925690-azure-arc) ein, oder stimmen Sie dort für eine Idee ab.

## <a name="next-steps"></a>Nächste Schritte

* [Verbinden eines Clusters mit Azure Arc](./connect-cluster.md)
* [Konzeptionelle Übersicht von Konfigurationen](./conceptual-configurations.md)