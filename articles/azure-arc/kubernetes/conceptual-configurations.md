---
title: 'Konfigurationen und GitOps: Azure Arc-fähiges Kubernetes'
services: azure-arc
ms.service: azure-arc
ms.date: 02/15/2021
ms.topic: conceptual
author: shashankbarsin
ms.author: shasb
description: Dieser Artikel bietet einen konzeptionellen Überblick über GitOps und die Konfigurationsfunktionen von Azure Arc-fähigem Kubernetes.
keywords: Kubernetes, Arc, Azure, Container, Konfiguration, GitOps
ms.openlocfilehash: 780c3c5c578c8a9b12eb7dda711070790477ac5f
ms.sourcegitcommit: de98cb7b98eaab1b92aa6a378436d9d513494404
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/17/2021
ms.locfileid: "100561175"
---
# <a name="configurations-and-gitops-with-azure-arc-enabled-kubernetes"></a>Konfigurationen und GitOps: Azure Arc-fähiges Kubernetes

Bezogen auf Kubernetes ist GitOps die Vorgehensweise, den gewünschten Zustand von Kubernetes-Clusterkonfigurationen (Bereitstellungen, Namespaces usw.) in einem Git-Repository zu deklarieren. Auf diese Deklaration folgt eine abruf- und pullbasierte Bereitstellung dieser Clusterkonfigurationen mithilfe eines Operators. Das Git-Repository kann Folgendes enthalten:
* Manifeste im YAML-Format, die gültige Kubernetes-Ressourcen beschreiben, einschließlich Namespaces, ConfigMaps, Bereitstellungen, DaemonSets usw.
* Helm-Charts für die Bereitstellung von Anwendungen.

[Flux](https://docs.fluxcd.io/), ein beliebtes Open-Source-Tool im GitOps-Raum, kann im Kubernetes-Cluster bereitgestellt werden, um den Fluss von Konfigurationen aus einem Git-Repository in einen Kubernetes-Cluster zu vereinfachen. Flux unterstützt die Bereitstellung seines Operators sowohl im Cluster- als auch im Namespaceumfang. Ein mit Namespaceumfang bereitgestellter Flux-Operator kann nur Kubernetes-Objekte innerhalb dieses spezifischen Namespaces bereitstellen. Die Möglichkeit, zwischen Cluster- und Namespaceumfang auszuwählen, hilft Ihnen dabei, mehrinstanzenfähige Bereitstellungsmuster im selben Kubernetes-Cluster zu erzielen.

## <a name="configurations"></a>Configurations

[![Konfigurationsarchitektur](./media/conceptual-configurations.png)](./media/conceptual-configurations.png#lightbox)

Die Verbindung zwischen Ihrem Cluster und einem Git-Repository wird als `Microsoft.KubernetesConfiguration/sourceControlConfigurations`-Erweiterungsressource auf der Azure Arc-fähigen Kubernetes-Ressource (von `Microsoft.Kubernetes/connectedClusters` dargestellt) in Azure Resource Manager erstellt. 

Die `sourceControlConfiguration`-Ressourceneigenschaften werden verwendet, um den Flux-Operator auf dem Cluster mit den entsprechenden Parametern bereitzustellen, z. B. dem Git-Repository, aus dem Manifeste abgerufen werden sollen, sowie dem Abrufintervall, in dem Sie abzurufen sind. Die `sourceControlConfiguration`-Daten werden im Ruhezustand verschlüsselt in einer Azure Cosmos DB-Datenbank gespeichert, um ihre Vertraulichkeit zu gewährleisten.

Der `config-agent`, der in Ihrem Cluster ausgeführt wird, ist für Folgendes verantwortlich:
* Nachverfolgen neuer oder aktualisierter `sourceControlConfiguration`-Erweiterungsressourcen in der Azure Arc-fähigen Kubernetes-Ressource.
* Bereitstellen eines Flux-Operators, um das Git-Repository für jede `sourceControlConfiguration` zu überwachen.
* Anwenden aller Aktualisierungen, die an einer `sourceControlConfiguration` vorgenommenen wurden. 

Sie können mehrere `sourceControlConfiguration`-Ressourcen mit Namespaceumfang im selben Azure Arc-fähigen Kubernetes-Cluster erstellen, um die Mehrinstanzenfähigkeit zu erzielen.

> [!NOTE]
> * Da der `config-agent` auf die Verfügbarkeit neuer oder aktualisierter `sourceControlConfiguration`-Erweiterungsressourcen auf der Azure Arc-fähigen Kubernetes-Ressource überwacht, benötigen die Agents eine Konnektivität, damit der gewünschte Zustand in den Cluster abgerufen werden kann. Immer, wenn Agents nicht in der Lage sind, eine Verbindung mit Azure herzustellen, werden die für die `sourceControlConfiguration`-Ressource in Azure Resource Manager deklarierten Eigenschaften des gewünschten Zustands nicht auf den Cluster angewendet.
> * Vertrauliche Kundeneingaben wie private Schlüssel, Inhalte bekannter Hosts, der HTTPS-Benutzername und das Token/Kennwort werden nicht länger als 48 Stunden in Azure Arc-fähigen Kubernetes-Diensten gespeichert. Wenn Sie vertrauliche Eingaben für Konfigurationen verwenden, empfiehlt es sich, die Cluster so regelmäßig wie möglich online zu schalten.

## <a name="apply-configurations-at-scale"></a>Anwenden von Konfigurationen im großen Stil

Da Azure Resource Manager Ihre Konfigurationen verwaltet, können Sie Azure Policy verwenden, um die Erstellung derselben Konfiguration auf allen Azure Arc-fähigen Kubernetes-Ressourcen innerhalb des Umfangs eines Abonnements oder einer Ressourcengruppe zu automatisieren. 

Diese Erzwingung im großen Stil stellt sicher, dass eine allgemeine Baselineversion (mit Konfigurationen wie ClusterRoleBindings, RoleBindings und NetworkPolicy) auf die gesamte Flotte oder das ganze Inventar von Azure Arc-fähigen Kubernetes-Clustern angewendet werden kann.

## <a name="next-steps"></a>Nächste Schritte

* [Verbinden eines Clusters mit Azure Arc](./connect-cluster.md)
* [Erstellen von Konfigurationen auf Ihrem Arc-fähigen Kubernetes-Cluster](./use-gitops-connected-cluster.md)
* [Verwenden von Azure Policy zum Anwenden von Konfigurationen im großen Stil](./use-azure-policy.md)