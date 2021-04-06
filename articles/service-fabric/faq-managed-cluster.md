---
title: Häufige Fragen zu verwalteten Service Fabric-Clustern
description: Häufig gestellte Fragen zu verwalteten Service Fabric-Clustern, z. B. in Bezug auf Funktionen, Anwendungsfälle und gängige Szenarien.
ms.topic: troubleshooting
ms.author: pepogors
author: peterpogorski
ms.date: 02/15/2021
ms.custom: references_regions
ms.openlocfilehash: aa77896ba88d0ffd0a6f94a84603b5f4a1803357
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "100633086"
---
# <a name="service-fabric-managed-clusters-frequently-asked-questions"></a>Häufig gestellte Fragen zu verwalteten Service Fabric-Clustern

Hier finden Sie einige häufig gestellte Fragen (FAQs) und Antworten für verwaltete Service Fabric-Cluster (Vorschau).

## <a name="general"></a>Allgemein

### <a name="what-are-service-fabric-managed-clusters"></a>Was sind verwaltete Service Fabric-Cluster?

Bei verwalteten Service Fabric-Clustern handelt es sich um eine Weiterentwicklung des Service Fabric-Clusterressourcenmodells, das Ihnen die Bereitstellung und Verwaltung von Clustern vereinfachen soll. Für einen verwalteten Service Fabric-Cluster wird das Azure Resource Manager-Kapselungsmodell verwendet, damit ein Benutzer nur eine Clusterressource definieren und bereitstellen muss – verglichen mit den vielen unabhängigen Ressourcen, die derzeit bereitgestellt werden müssen (VM-Skalierungsgruppe, Load Balancer, IP-Adresse und mehr).

### <a name="what-regions-are-supported-in-the-preview"></a>Welche Regionen werden für die Vorschauversion unterstützt?

Zu den unterstützten Regionen für die Vorschauversion der verwalteten Service Fabric-Cluster zählen `centraluseuap`, `eastus2euap`, `eastasia`, `northeurope`, `westcentralus` und `eastus2`.

### <a name="can-i-do-an-in-place-migration-of-my-existing-service-fabric-cluster-to-a-managed-cluster-resource"></a>Kann ich eine direkte Migration meines vorhandenen Service Fabric-Clusters zu einer Ressource vom Typ „Verwalteter Cluster“ durchführen?

Nein. Derzeit müssen Sie eine neue Service Fabric-Clusterressource erstellen, um den neuen Ressourcentyp „Verwalteter Service Fabric-Cluster“ zu verwenden.

### <a name="is-there-an-additional-cost-for-service-fabric-managed-clusters"></a>Fallen für verwaltete Service Fabric-Cluster zusätzliche Kosten an?

Nein. Für einen verwalteten Service Fabric-Cluster fallen keine zusätzlichen Kosten an, sondern nur die Kosten für die zugrunde liegenden Compute-, Speicher- und Netzwerkressourcen, die für den Cluster benötigt werden.

### <a name="is-there-a-new-sla-introduced-by-the-service-fabric-managed-cluster-resource"></a>Wird für die Ressource vom Typ „Verwalteter Service Fabric-Cluster“ eine neue SLA eingeführt?

Die SLA ändert sich gegenüber dem aktuellen Service Fabric-Ressourcenmodell nicht.

### <a name="what-is-the-difference-between-a-basic-and-standard-sku-cluster"></a>Worin besteht der Unterschied zwischen einem Cluster mit der SKU „Basic“ und der SKU „Standard“?

Ein Cluster mit der SKU „Basic“ bedeutet, dass die meisten Konfigurationen vom Service Fabric-Ressourcenanbieter bereitgestellt werden. Cluster mit der SKU „Basic“ sind für die Verwendung mit Test- und Präproduktionsumgebungen vorgesehen. Ein Cluster mit der SKU „Standard“ kann von einem Benutzer so konfiguriert werden, dass seine spezifischen Anforderungen erfüllt werden. Weitere Informationen finden Sie unter [SKUs für verwaltete Service Fabric-Cluster](./overview-managed-cluster.md#service-fabric-managed-cluster-skus).

## <a name="cluster-deployment-and-management"></a>Clusterbereitstellung und -verwaltung

### <a name="i-run-custom-script-extensions-on-my-virtual-machine-scale-set-can-i-continue-to-do-that-with-a-managed-service-fabric-resource"></a>Ich führe benutzerdefinierte Skripterweiterungen in meiner VM-Skalierungsgruppe aus. Ist dies auch mit einer verwalteten Service Fabric-Ressource weiter möglich?

Ja. Sie können VM-Erweiterungen für verwaltete Clusterknotentypen angeben. Weitere Informationen finden Sie unter [Hinzufügen einer VM-Skalierungsgruppenerweiterung zu einem verwalteten Service Fabric-Clusterknotentyp (Vorschau)](how-to-managed-cluster-vmss-extension.md).

### <a name="i-want-to-have-an-internal-only-load-balancer-is-that-possible"></a>Ich möchte nur einen rein internen Lastenausgleich nutzen. Ist das möglich?

Nein. Derzeit ist es nicht möglich, einen rein internen Lastenausgleich zu nutzen. Wir empfehlen Ihnen, die Netzwerksicherheitsgruppen-Regeln (NSG) zu sperren, um unerwünschten eingehenden bzw. ausgehenden Datenverkehr zu blockieren.

### <a name="can-i-autoscale-my-cluster"></a>Kann ich für meinen Cluster die Autoskalierung nutzen?

Die Autoskalierung ist in der Vorschauversion derzeit nicht verfügbar.

### <a name="can-i-deploy-my-cluster-across-availability-zones"></a>Kann ich meinen Cluster über Verfügbarkeitszonen hinweg bereitstellen?

Übergreifende Cluster für Verfügbarkeitszonen sind in der Vorschauversion derzeit nicht verfügbar.

### <a name="can-i-select-between-automatic-and-manual-upgrades-for-my-cluster-runtime"></a>Kann ich für meine Clusterruntime zwischen automatischen und manuellen Upgrades wählen?

In der Vorschauversion werden alle Runtimeupgrades automatisch durchgeführt.

## <a name="applications"></a>Anwendungen

### <a name="is-there-a-local-development-experience-for-service-fabric-managed-clusters"></a>Ist eine lokale Entwicklungsumgebung für verwaltete Service Fabric-Cluster vorhanden?

Die lokale Entwicklungsumgebung bleibt gegenüber den vorhandenen Service Fabric-Clustern unverändert. Weitere Informationen zur lokalen Entwicklungsumgebung finden Sie unter [Einrichten Ihrer Entwicklungsumgebung](./service-fabric-get-started.md).

### <a name="can-i-deploy-my-applications-as-an-azure-resource-manager-resource"></a>Kann ich meine Anwendungen als Azure Resource Manager-Ressource bereitstellen?

Ja. Die Bereitstellung von Anwendungen als Azure Resource Manager-Ressource wird jetzt unterstützt (zusätzlich zur Bereitstellung mithilfe von PowerShell und der CLI). Informationen zu den ersten Schritten finden Sie unter [Bereitstellen einer Anwendung in einem verwalteten Service Fabric-Cluster (Vorschau) mithilfe einer ARM-Vorlage](how-to-managed-cluster-app-deployment-template.md).
