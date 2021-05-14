---
title: 'Konfigurationen und GitOps: Azure Arc-fähiges Kubernetes'
services: azure-arc
ms.service: azure-arc
ms.date: 03/02/2021
ms.topic: conceptual
author: shashankbarsin
ms.author: shasb
description: Dieser Artikel bietet einen konzeptionellen Überblick über GitOps und die Konfigurationsfunktionen von Azure Arc-fähigem Kubernetes.
keywords: Kubernetes, Arc, Azure, Container, Konfiguration, GitOps
ms.openlocfilehash: 198bd45374d998ef0d9d0e612e974484d52e2f20
ms.sourcegitcommit: 56b0c7923d67f96da21653b4bb37d943c36a81d6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2021
ms.locfileid: "106443305"
---
# <a name="configurations-and-gitops-with-azure-arc-enabled-kubernetes"></a>Konfigurationen und GitOps: Azure Arc-fähiges Kubernetes

Bezogen auf Kubernetes ist GitOps die Vorgehensweise, den gewünschten Zustand von Kubernetes-Clusterkonfigurationen (Bereitstellungen, Namespaces usw.) in einem Git-Repository zu deklarieren. Auf diese Deklaration folgt eine abruf- und pullbasierte Bereitstellung dieser Clusterkonfigurationen mithilfe eines Operators. Das Git-Repository kann Folgendes enthalten:
* Manifeste im YAML-Format, die gültige Kubernetes-Ressourcen beschreiben, einschließlich Namespaces, ConfigMaps, Bereitstellungen, DaemonSets usw.
* Helm-Charts für die Bereitstellung von Anwendungen.

[Flux](https://docs.fluxcd.io/), ein beliebtes Open-Source-Tool im GitOps-Raum, kann im Kubernetes-Cluster bereitgestellt werden, um den Fluss von Konfigurationen aus einem Git-Repository in einen Kubernetes-Cluster zu vereinfachen. Flux unterstützt die Bereitstellung seines Operators sowohl im Cluster- als auch im Namespaceumfang. Ein mit Namespaceumfang bereitgestellter Flux-Operator kann nur Kubernetes-Objekte innerhalb dieses spezifischen Namespaces bereitstellen. Die Möglichkeit, zwischen Cluster- und Namespaceumfang auszuwählen, hilft Ihnen dabei, mehrinstanzenfähige Bereitstellungsmuster im selben Kubernetes-Cluster zu erzielen.

## <a name="configurations"></a>Configurations

[![Konfigurationsarchitektur](./media/conceptual-configurations.png)](./media/conceptual-configurations.png#lightbox)

Die Verbindung zwischen Ihrem Cluster und einem Git-Repository wird als Konfigurationsressource (`Microsoft.KubernetesConfiguration/sourceControlConfigurations`) auf der Kubernetes-Ressource mit Azure Arc-Aktivierung (von `Microsoft.Kubernetes/connectedClusters` dargestellt) in Azure Resource Manager erstellt. 

Die Eigenschaften der Konfigurationsressource werden verwendet, um den Flux-Operator auf dem Cluster mit den entsprechenden Parametern bereitzustellen, z. B. dem Git-Repository, aus dem Manifeste abgerufen werden sollen, sowie dem Abrufintervall, in dem Sie abzurufen sind. Die Daten der Konfigurationsressource werden im Ruhezustand verschlüsselt in einer Azure Cosmos DB-Datenbank gespeichert, um ihre Vertraulichkeit zu gewährleisten.

Der `config-agent`, der in Ihrem Cluster ausgeführt wird, ist für Folgendes verantwortlich:
* Nachverfolgen neuer oder aktualisierter Konfigurationsressourcen in der Kubernetes-Ressource mit Azure Arc-Aktivierung.
* Bereitstellen eines Flux-Operators, um das Git-Repository für jede Konfigurationsressource zu überwachen.
* Anwenden beliebiger Updates, die an einer Konfigurationsressource vorgenommen werden. 

Sie können mehrere Konfigurationsressourcen mit Namespaceumfang im selben Kubernetes-Cluster mit Azure Arc-Aktivierung erstellen, um die Mehrinstanzenfähigkeit zu erzielen.

> [!NOTE]
> * `config-agent` überwacht, ob neue oder aktualisierte Konfigurationsressourcen auf der Kubernetes-Ressource mit Azure Arc-Aktivierung verfügbar sind. Agents erfordern also Konnektivität, um den gewünschten Zustand auf den Cluster zu pullen. Wenn Agents keine Verbindung mit Azure herstellen können, kommt es zu einer Verzögerung bei der Weitergabe des gewünschten Zustands an den Cluster.
> * Vertrauliche Kundeneingaben wie private Schlüssel, Inhalte bekannter Hosts, der HTTPS-Benutzername und das Token/Kennwort werden nicht länger als 48 Stunden in Azure Arc-fähigen Kubernetes-Diensten gespeichert. Wenn Sie vertrauliche Eingaben für Konfigurationen verwenden, schalten Sie die Cluster so regelmäßig wie möglich online.

## <a name="apply-configurations-at-scale"></a>Anwenden von Konfigurationen im großen Stil

Da Azure Resource Manager Ihre Konfigurationen verwaltet, können Sie mithilfe von Azure Policy im Rahmen eines Abonnements oder einer Ressourcengruppe automatisch dieselbe Konfiguration für alle Kubernetes-Ressourcen mit Azure Arc-Aktivierung erstellen. 

Diese Erzwingung im großen Stil stellt sicher, dass eine allgemeine Baselineversion (mit Konfigurationen wie ClusterRoleBindings, RoleBindings und NetworkPolicy) auf eine gesamte Flotte oder das ganze Inventar von Kubernetes-Clustern mit Azure Arc-Aktivierung angewendet werden kann.

## <a name="next-steps"></a>Nächste Schritte

* Nutzen Sie den Schnellstart zum [Verbinden eines Kubernetes-Clusters mit Azure Arc](./quickstart-connect-cluster.md).
* [Erstellen Sie Konfigurationen](./tutorial-use-gitops-connected-cluster.md) in Ihrem Kubernetes-Cluster mit Azure Arc-Unterstützung.
* [Verwenden Sie Azure Policy zum Anwenden von Konfigurationen im großen Stil.](./use-azure-policy.md)