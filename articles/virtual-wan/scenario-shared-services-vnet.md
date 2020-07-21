---
title: 'Szenario: Routen zu VNETs für gemeinsame Dienste'
titleSuffix: Azure Virtual WAN
description: 'Routingszenarios: Einrichten von Routen für den Zugriff auf ein VNET für gemeinsame Dienste mit einer Workload, auf die alle VNETs und Branches zugreifen können.'
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 06/29/2020
ms.author: cherylmc
ms.openlocfilehash: 64bb4c85399f811c0ab7ff84b297b64734efc491
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85567902"
---
# <a name="scenario-route-to-shared-services-vnets"></a>Szenario: Routen zu VNETs für gemeinsame Dienste

Wenn Sie mit Virtual WAN-Routing für virtuelle Hubs arbeiten, steht Ihnen eine ganze Reihe von Szenarios zur Verfügung. In diesem Szenario richten Sie Routen für den Zugriff auf ein VNET für **gemeinsame Dienste** mit einer Workload ein, auf die alle VNETs und Branches (VPN/ER/P2S) zugreifen können. Weitere Informationen zum Routing virtueller Hubs finden Sie unter [Informationen zum Routing virtueller Hubs](about-virtual-hub-routing.md).

## <a name="scenario-workflow"></a><a name="workflow"></a>Szenarioworkflow

Führen Sie zur Konfiguration dieses Szenarios die folgenden Schritte aus:

1. Identifizieren Sie das VNET für **gemeinsame Dienste**.
2. Erstellen Sie eine benutzerdefinierte Routingtabelle. Im diesem Beispiel wird die Routingtabelle als **RT_SHARED** bezeichnet. Eine Anleitung zum Erstellen einer Routingtabelle finden Sie unter [Konfigurieren des Routings für virtuelle Hubs](how-to-virtual-hub-routing.md). Verwenden Sie als Richtlinie die folgenden Werte:

   * **Zuordnung**
     * Wählen Sie für **VNETs *mit Ausnahme* des VNETs für gemeinsame Dienste** die VNETs aus, die isoliert werden sollen. Dadurch können alle ausgewählten VNETs (mit Ausnahme des VNETs für gemeinsame Dienste) basierend auf den Routen der Tabelle „RT_SHARED“ das Ziel erreichen.

   * **Weitergabe**
      * Geben Sie für **Branches** zusätzlich zu weiteren Routingtabellen, die Sie möglicherweise bereits ausgewählt haben, Routen an diese Routingtabelle weiter. Durch diesen Schritt erlernt die Routingtabelle „RT_SHARED“ die Routen aller Branchverbindungen (VPN/ER/Benutzer-VPN).
      * Wählen Sie für **VNETs** das **VNET für gemeinsame Dienste** aus. Durch diesen Schritt erlernt die Routingtabelle „RT_SHARED“ die Routen der Verbindung des VNETs für gemeinsame Dienste.

     Dies führt zu folgenden Änderungen der Routingkonfiguration:

   :::image type="content" source="./media/routing-scenarios/shared-service-vnet/shared-services.png" alt-text="VNET für gemeinsame Dienste":::

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu Virtual WAN finden Sie unter [Häufig gestellte Fragen](virtual-wan-faq.md).
* Weitere Informationen zum Routing virtueller Hubs finden Sie unter [Informationen zum Routing virtueller Hubs](about-virtual-hub-routing.md).
