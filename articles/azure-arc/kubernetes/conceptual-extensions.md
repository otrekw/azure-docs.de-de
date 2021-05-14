---
title: 'Clustererweiterungen: Kubernetes mit Azure Arc-Unterstützung'
services: azure-arc
ms.service: azure-arc
ms.date: 04/05/2021
ms.topic: conceptual
author: shashankbarsin
ms.author: shasb
description: Dieser Artikel bietet eine konzeptionelle Übersicht über die Clustererweiterungenfunktionalität von Kubernetes mit Azure Arc-Unterstützung.
ms.openlocfilehash: 4b9a3991b51ec45e7a64acd546c031d4241c97af
ms.sourcegitcommit: 56b0c7923d67f96da21653b4bb37d943c36a81d6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2021
ms.locfileid: "106450878"
---
# <a name="cluster-extensions-on-azure-arc-enabled-kubernetes"></a>Clustererweiterungen in Kubernetes mit Azure Arc-Unterstützung

[Helm-Diagramme](https://helm.sh/) helfen Ihnen, Kubernetes-Anwendungen zu verwalten, indem sie die Bausteine bereitstellen, die zum Definieren, Installieren und Aktualisieren der komplexesten Kubernetes-Anwendungen erforderlich sind. Das Clustererweiterungenfeature ergänzt die Helm-Verpackungskomponenten. Auf der Basis von Azure Resource Manager werden Clustererweiterungen wie Azure Monitor und Azure Defender für Kubernetes installiert und ihre Lebenszyklen verwaltet. Das Clustererweiterungenfeature bietet die folgenden zusätzlichen Vorteile zu den Komponenten, die bereits nativ in Helm-Diagrammen verfügbar sind:

- Eine Inventur aller Cluster und Erweiterungen, die auf diesen Clustern installiert sind.
- Automatisieren der Bereitstellung von Clustererweiterungen im gewünschten Umfang mit Azure Policy.
- Abonnieren von Release Trains für jede Erweiterung.
- Einrichten des automatischen Upgrades für Erweiterungen.
- Ereignisse zum Erstellen von Erweiterungsinstanzen und Verwaltung des Lebenszyklus (Aktualisieren und Löschen) werden unterstützt.

[!INCLUDE [preview features note](./includes/preview/preview-callout.md)]

## <a name="architecture"></a>Aufbau

[ ![Architektur von Clustererweiterungen](./media/conceptual-extensions.png) ](./media/conceptual-extensions.png#lightbox)

Die Clustererweiterungsinstanz wird als Azure Resource Manager-Erweiterungsressource (`Microsoft.KubernetesConfiguration/extensions`) auf der Kubernetes-Ressource mit Azure Arc-Unterstützung (von `Microsoft.Kubernetes/connectedClusters` dargestellt) in Azure Resource Manager erstellt. Mithilfe der Darstellung in Azure Resource Manager können Sie eine Richtlinie erstellen, die ermittelt, welche Kubernetes-Ressourcen mit Azure Arc-Unterstützung eine spezifische Clustererweiterung haben oder nicht. Nachdem Sie ermittelt haben, welchen Clustern Clustererweiterungen mit den gewünschten Eigenschaftswerten fehlen, können Sie diese nicht konformen Ressourcen mit Azure Policy korrigieren.

Die Ausführung von `config-agent` in Ihrem Cluster verfolgt neue oder aktualisierte Erweiterungsressourcen in der Kubernetes-Ressource mit Azure Arc-Unterstützung nach. Die Ausführung von `extensions-manager` in Ihrem Cluster pullt das Helm-Diagramm aus der Azure Container Registry oder Microsoft Container Registry und installiert es im Cluster. 

Die im Cluster ausgeführten Komponenten `config-agent` und `extensions-manager` sind für Versionsupdates und das Löschen von Erweiterungsinstanzen zuständig.

> [!NOTE]
> * `config-agent` überwacht, ob neue oder aktualisierte Erweiterungsressourcen auf der Kubernetes-Ressource mit Azure Arc-Unterstützung verfügbar sind. Damit der gewünschte Zustand auf den Cluster gepullt werden kann, benötigen Agents also Konnektivität. Wenn Agents keine Verbindung mit Azure herstellen können, wird die Weitergabe des gewünschten Zustands an den Cluster verzögert.
> * Geschützte Konfigurationseinstellungen für eine Erweiterung werden bis zu 48 Stunden in den Kubernetes-Diensten mit Azure Arc-Unterstützung gespeichert. Wenn der Cluster während der 48 Stunden nach der Erstellung der Erweiterungsressource in Azure getrennt bleibt, geht die Erweiterung vom Status `Pending` in den Status `Failed` über. Sie sollten die Cluster so regelmäßig wie möglich online schalten.

## <a name="next-steps"></a>Nächste Schritte

* Nutzen Sie den Schnellstart zum [Verbinden eines Kubernetes-Clusters mit Azure Arc](./quickstart-connect-cluster.md).
* Stellen Sie [Clustererweiterungen](./extensions.md) in Ihrem Kubernetes-Cluster mit Azure Arc-Unterstützung bereit.