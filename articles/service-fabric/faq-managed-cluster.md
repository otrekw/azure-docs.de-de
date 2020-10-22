---
title: Häufige Fragen zu verwalteten Service Fabric-Clustern
description: Häufig gestellte Fragen zu verwalteten Service Fabric-Clustern, z. B. in Bezug auf Funktionen, Anwendungsfälle und gängige Szenarien.
ms.topic: troubleshooting
ms.author: pepogors
author: peterpogorski
ms.date: 09/28/2020
ms.custom: references_regions
ms.openlocfilehash: 4dc41d2c13c834657534971041440bb744cfca38
ms.sourcegitcommit: ce8eecb3e966c08ae368fafb69eaeb00e76da57e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2020
ms.locfileid: "92319825"
---
# <a name="service-fabric-managed-clusters-frequently-asked-questions"></a>Häufig gestellte Fragen zu verwalteten Service Fabric-Clustern

Es gibt viele häufig gestellte Fragen dazu, was mit verwalteten Service Fabric-Clustern (Vorschauversion) möglich ist und wie sie verwendet werden können. In diesem Dokument werden viele dieser häufig gestellten Fragen und die zugehörigen Antworten behandelt.

## <a name="general"></a>Allgemein

### <a name="what-are-service-fabric-managed-clusters"></a>Was sind verwaltete Service Fabric-Cluster?

Bei verwalteten Service Fabric-Clustern handelt es sich um eine Weiterentwicklung des Service Fabric-Clusterressourcenmodells, das Ihnen die Bereitstellung und Verwaltung von Clustern vereinfachen soll. Für einen verwalteten Service Fabric-Cluster wird das Azure Resource Manager-Kapselungsmodell verwendet, damit ein Benutzer nur eine Clusterressource definieren und bereitstellen muss – verglichen mit den vielen unabhängigen Ressourcen, die derzeit bereitgestellt werden müssen (VM-Skalierungsgruppe, Load Balancer, IP-Adresse und mehr).

### <a name="what-regions-are-supported-in-the-preview"></a>Welche Regionen werden für die Vorschauversion unterstützt?

Zu den unterstützten Regionen für die Vorschauversion der verwalteten Service Fabric-Cluster zählen `centraluseuap`, `eastus2euap`, `eastasia`, `northeurope`, `westcentralus` und `eastus2`.

### <a name="can-i-do-an-in-place-migration-of-my-existing-service-fabric-cluster-to-a-managed-cluster-resource"></a>Kann ich eine direkte Migration meines vorhandenen Service Fabric-Clusters zu einer Ressource vom Typ „Verwalteter Cluster“ durchführen?

Derzeit müssen Sie eine neue Service Fabric-Clusterressource erstellen, um den neuen Ressourcentyp „Verwalteter Service Fabric-Cluster“ zu verwenden.

### <a name="is-there-an-additional-cost-for-service-fabric-managed-clusters"></a>Fallen für verwaltete Service Fabric-Cluster zusätzliche Kosten an?

Nein. Für einen verwalteten Service Fabric-Cluster fallen keine zusätzlichen Kosten an, sondern nur die Kosten für die zugrunde liegenden Compute-, Speicher- und Netzwerkressourcen, die für den Cluster benötigt werden.

### <a name="is-there-a-new-sla-introduced-by-the-service-fabric-managed-cluster-resource"></a>Wird für die Ressource vom Typ „Verwalteter Service Fabric-Cluster“ eine neue SLA eingeführt?

Die SLA ändert sich gegenüber dem aktuellen Service Fabric-Ressourcenmodell nicht.

### <a name="what-is-the-difference-between-a-basic-and-standard-sku-cluster"></a>Worin besteht der Unterschied zwischen einem Cluster mit der SKU „Basic“ und der SKU „Standard“?

Ein Cluster mit der SKU „Basic“ bedeutet, dass die meisten Konfigurationen vom Service Fabric-Ressourcenanbieter bereitgestellt werden. Cluster mit der SKU „Basic“ sind für die Verwendung mit Test- und Präproduktionsumgebungen vorgesehen. Ein Cluster mit der SKU „Standard“ kann von einem Benutzer so konfiguriert werden, dass seine spezifischen Anforderungen erfüllt werden. Weitere Informationen finden Sie unter [SKUs für verwaltete Service Fabric-Cluster](./overview-managed-cluster.md#service-fabric-managed-cluster-skus).

## <a name="cluster-deployment-and-management"></a>Clusterbereitstellung und -verwaltung

### <a name="i-run-custom-script-extensions-on-my-virtual-machine-scale-set-can-i-continue-to-do-that-with-a-managed-service-fabric-resource"></a>Ich führe benutzerdefinierte Skripterweiterungen in meiner VM-Skalierungsgruppe aus. Ist dies auch mit einer verwalteten Service Fabric-Ressource weiter möglich?

Ja. Sie können weiterhin VM-Erweiterungen für einen Knotentyp angeben. Weitere Informationen finden Sie im Beispiel zur Knotentyperweiterung.

### <a name="i-want-to-have-an-internal-only-load-balancer-is-that-possible"></a>Ich möchte nur einen rein internen Lastenausgleich nutzen. Ist das möglich?

Derzeit ist es nicht möglich, einen rein internen Lastenausgleich zu nutzen. Wir empfehlen Ihnen, die Netzwerksicherheitsgruppen-Regeln zu sperren, um unerwünschten eingehenden bzw. ausgehenden Datenverkehr zu blockieren.

### <a name="can-i-autoscale-my-cluster"></a>Kann ich für meinen Cluster die Autoskalierung nutzen? 
Die Autoskalierung ist in der Vorschauversion derzeit nicht verfügbar.

### <a name="can-i-deploy-my-cluster-across-availability-zones"></a>Kann ich meinen Cluster über Verfügbarkeitszonen hinweg bereitstellen? 
Übergreifende Cluster für Verfügbarkeitszonen sind in der Vorschauversion derzeit nicht verfügbar.

### <a name="can-i-select-between-automatic-and-manual-upgrades-for-my-cluster-runtime"></a>Kann ich für meine Clusterruntime zwischen automatischen und manuellen Upgrades wählen? 
In der Vorschauversion werden alle Runtimeupgrades automatisch durchgeführt.

## <a name="applications"></a>Anwendungen

### <a name="is-there-a-local-development-experience-for-service-fabric-managed-clusters"></a>Ist eine lokale Entwicklungsumgebung für verwaltete Service Fabric-Cluster vorhanden?

Die lokale Entwicklungsumgebung bleibt gegenüber den vorhandenen Service Fabric-Clustern unverändert. Weitere Informationen zur lokalen Entwicklungsumgebung finden Sie unter [Schnellstart: Bereitstellen einer .NET Reliable Services-Anwendung in Service Fabric](./service-fabric-quickstart-dotnet.md).

### <a name="can-i-deploy-my-applications-as-an-azure-resource-manager-resource"></a>Kann ich meine Anwendungen als Azure Resource Manager-Ressource bereitstellen?

In der Vorschauversion können Sie keine Anwendungen als Azure Resource Manager-Ressource bereitstellen. Anwendungen müssen bereitgestellt werden, indem Sie entweder über PowerShell oder die CLI eine direkte Verbindung mit dem Cluster herstellen. Diese Funktion wird vor Beginn der allgemeinen Verfügbarkeit von verwalteten Service Fabric-Clustern hinzugefügt.