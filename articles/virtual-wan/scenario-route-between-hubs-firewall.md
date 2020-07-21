---
title: 'Szenario: Azure Firewall: Routing zwischen Hubs'
titleSuffix: Azure Virtual WAN
description: 'Szenarien für das Routing: Routing zwischen mehreren Hubs mit Azure Firewall'
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 06/29/2020
ms.author: cherylmc
ms.openlocfilehash: 1057dc28682c90374b2f8e8e0297f3d769c08bd6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85567623"
---
# <a name="scenario-azure-firewall---interhub-preview"></a>Szenario: Azure Firewall: Zwischen Hubs (Vorschau)

Wenn Sie mit Virtual WAN-Routing für virtuelle Hubs arbeiten, stehen Ihnen eine ganze Reihe von Szenarien zur Verfügung. In diesem Szenario besteht das Ziel im Routing zwischen mehreren Hubs, auf denen Azure Firewall vorhanden ist. Weitere Informationen zum Routing für virtuelle Hubs finden Sie unter [Informationen zum Routing virtueller Hubs](about-virtual-hub-routing.md).

## <a name="scenario-architecture"></a><a name="architecture"></a>Szenarioarchitektur

In diesem Szenario gehen wir von der folgenden Konfiguration aus:

* Sie verfügen über mehrere Hubs, auf denen jeweils Azure Firewall installiert ist.
* Sie verwenden einen sicheren virtuellen Hub.
* Die entsprechenden Richtlinien für privaten (VNET), Internet- und Branchdatenverkehr wurden eingerichtet.
* Datenverkehr vom VNET in das Internet (V2I) oder in einen Branch (V2B) sowie vom Branch in das VNET (B2V) durchläuft in jedem Hub Azure Firewall.

Zusätzliche Überlegungen:

* Bei einem Szenario zwischen Hubs mit Azure Firewall wird davon ausgegangen, dass die Spoke-VNETs nicht separaten Routingtabellen zugeordnet sind. (Beispiel: Zuordnung von **VNET 1** zu **Routingtabelle A** und **VNET 2** zu **Routingtabelle B**.) Alle VNETs sind derselben Routingtabelle zugeordnet, in der sich die Routen für Azure Firewall befinden.
* Die Sicherung über Azure Firewall ist derzeit auf **Branch <-> VNET** und ausschließlich **Internet**-Datenverkehr beschränkt. Der Flow zwischen Branches über Azure Firewall wird derzeit nicht unterstützt.

**Abbildung 1**

:::image type="content" source="./media/routing-scenarios/between-hubs-firewall/architecture.png" alt-text="Architektur":::

## <a name="scenario-workflow"></a><a name="workflow"></a>Szenarioworkflow

Führen Sie zur Konfiguration dieses Szenarios die folgenden Schritte aus:

### <a name="step-1"></a><a name="step-1"></a>Schritt 1

Sofern Sie die Verbindungen bereits über Azure Firewall Manager abgesichert haben, besteht der erste Schritt darin, sicherzustellen, dass für alle Branch- und VNET-Verbindungen der Verteilungsmodus **Keine** festgelegt ist. Dies ist erforderlich, um sicherzustellen, dass Datenverkehr über Azure Firewall gesendet wird.

1. Wählen Sie den Hub aus, und bearbeiten Sie die Routingtabelle **Keine**.
1. Aktualisieren Sie **Verteilung**, sodass alle Branches und alle VNETs ausgewählt werden.

### <a name="step-2"></a><a name="step-2"></a>Schritt 2

Richten Sie eine benutzerdefinierte Routingtabelle pro Hub ein.

1. Erstellen Sie für **Hub 1** die Routingtabelle **RT_Hub1**.

    * Wenn Sie Azure Firewall Manager verwendet haben, wird automatisch eine Route für 0.0.0.0/0 mit dem nächsten Hop **AZFW1** in der Standardroutingtabelle des Hubs erstellt. Diese Einstellung wird in Schritt 3 geändert. Erstellen Sie für **RT_Hub1** explizit einen Eintrag für 0.0.0.0/0 mit dem nächsten Hop **AZFW1**. Diese Einstellung aktiviert V2V, B2V und V2I über AZFW1.
    * Da **Hub 1** die Branches und das VNET von **Hub 2** über **AZFW2** (anstelle von AZFW1) erreichen können soll, müssen Sie zwei weitere aggregierte Routen für die Branches (10.5.0.0/16 -> AZFW2) und VNETs (10.2.0.0/16 -> AZFW2) von **Hub 2** hinzufügen.

1. Erstellen Sie für **Hub 2** die Routingtabelle **RT_Hub2**.

    * Wenn Sie Azure Firewall Manager verwendet haben, wird automatisch eine Route für 0.0.0.0/0 mit dem nächsten Hop **AZFW2** in der Standardroutingtabelle des Hubs erstellt. Diese Einstellung wird in Schritt 3 geändert. Erstellen Sie für **RT_Hub2** explizit einen Eintrag für 0.0.0.0/0 mit dem nächsten Hop **AZFW2**. Diese Einstellung aktiviert V2V, B2V und V2I über **AZFW2**.
    * Da der Datenverkehr zwischen den Hubs durch **AZFW2** von Hub 2 geschützt werden soll, müssen Sie nicht explizit einen Schritt wie unter dem zweiten Aufzählungszeichen im vorherigen Schritt für Hub 1 hinzufügen.

### <a name="step-3"></a><a name="step-3"></a>Schritt 3

Ändern Sie die **Standardroutingtabelle** in jedem Hub, um eine statische Route für die Flows **vom Branch zum VNET** (B2V) und **vom Branch zum Internet** (B2I) über Azure Firewall hinzuzufügen. Datenverkehr zwischen Branches wird derzeit über Azure Firewall nicht unterstützt.

1. Für die **Standardroutingtabelle von Hub 1** gilt:

    * **Von einem Branch zu den Branches von Hub 2 über AZFW2:** 10.5.0.0/16 -> AZFW2 Mit dieser Konfiguration wird eine Route für die Firewall von Hub 2 eingerichtet.
    * **Von einem Branch zu den VNETs von Hub 2 über AZFW2:** 10.2.0.0/16 -> AZFW2 Mit dieser Konfiguration wird eine Route für die Firewall von Hub 2 eingerichtet.
    * **Von einem Branch zu einem Branch (lokal)/von einem Branch zu einem VNET (lokal)/von einem Branch zum Internet:** 0.0.0.0/0 -> AZFW1

2. Für die **Standardroutingtabelle von Hub 2** gilt:

    * Von einem Branch zu einem Branch (lokal und remote)/von einem Branch zu einem VNET (lokal und remote)/von einem Branch zum Internet: 0.0.0.0/0 -> AZFW2

Dies führt zu Änderungen der Routingkonfiguration, wie in der folgenden Abbildung dargestellt.

**Abbildung 2**

:::image type="content" source="./media/routing-scenarios/between-hubs-firewall/workflow.png" alt-text="workflow":::

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu Virtual WAN finden Sie unter [Häufig gestellte Fragen](virtual-wan-faq.md).
* Weitere Informationen zum Routing für virtuelle Hubs finden Sie unter [Informationen zum Routing virtueller Hubs](about-virtual-hub-routing.md).
* Weitere Informationen zum Konfigurieren des Routings für virtuelle Hubs finden Sie unter [Konfigurieren des Routings für virtuelle Hubs](how-to-virtual-hub-routing.md).
