---
title: Schützen des Datenverkehrs zwischen einem Anwendungsgateway und Back-End-Pools
titleSuffix: Azure Virtual WAN
description: 'Szenarien für das Routing: Schützen von Datenverkehr, der über ein in einem Spoke-VNet bereitgestelltes Anwendungsgateway gesendet wird, das mit einem geschützten virtuellen WAN-Hub verbunden ist.'
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 04/12/2021
ms.author: cherylmc
ms.openlocfilehash: d9cb1251b90cf1c928f8286072bcd91e5ddf767e
ms.sourcegitcommit: b4fbb7a6a0aa93656e8dd29979786069eca567dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2021
ms.locfileid: "107315247"
---
# <a name="scenario-secure-traffic-between-application-gateway-and-backend-pools"></a>Szenario: Schützen des Datenverkehrs zwischen einem Anwendungsgateway und Back-End-Pools

Wenn Sie mit Virtual WAN-Routing für virtuelle Hubs arbeiten, steht Ihnen eine ganze Reihe von Szenarios zur Verfügung. In diesem Szenario geht der Datenverkehr eines Benutzers über ein in einem virtuellen Spoke-Netzwerk (VNet) bereitgestelltes Anwendungsgateway in Azure ein, das mit einem geschützten virtuellen WAN-Hub (virtueller WAN-Hub mit einer Azure Firewall-Instanz) verbunden ist. Ziel ist es, den Datenverkehr zwischen dem Anwendungsgateway und den Back-End-Pools mithilfe der Azure Firewall-Instanz im geschützten virtuellen Hub zu überprüfen.

In diesem Szenario gibt es zwei spezifische Entwurfsmuster, je nachdem, ob sich das Anwendungsgateway und die Back-End-Pools im gleichen VNet oder in unterschiedlichen VNets befinden.

* **Szenario 1:** Das Anwendungsgateway und die Back-End-Pools befinden sich im gleichen virtuellen Netzwerk, das mittels Peering mit einem virtuellen WAN-Hub verbunden ist (separate Subnetze).
* **Szenario 2:** Das Anwendungsgateway und die Back-End-Pools befinden sich in unterschiedlichen virtuellen Netzwerken, die mittels Peering mit einem virtuellen WAN-Hub verbunden sind.

## <a name="scenario-1---same-vnet"></a><a name="scenario-1"></a>Szenario 1: Gleiches VNet

In diesem Szenario befinden sich das Anwendungsgateway und die Back-End-Pools im gleichen virtuellen Netzwerk, das mittels Peering mit einem virtuellen WAN-Hub verbunden ist (separate Subnetze).

:::image type="content" source="./media/routing-scenarios/secured-application-gateway/same-vnet.png" alt-text="Diagramm für Szenario 1" lightbox="./media/routing-scenarios/secured-application-gateway/same-vnet.png":::

### <a name="workflow"></a>Workflow

Derzeit werden Routen, die virtuellen Spoke-Netzwerken von der Virtual WAN-Routingtabelle angekündigt werden, auf das gesamte virtuelle Netzwerk angewendet und nicht auf die Subnetze des Spoke-VNet. Daher sind benutzerdefinierte Routen erforderlich, um dieses Szenario zu ermöglichen. Informationen zu benutzerdefinierten Routen (User-Defined Routes, UDRs) finden Sie unter [Benutzerdefinierte virtuelle Netzwerkrouten](../virtual-network/virtual-networks-udr-overview.md#user-defined).


1. Wählen Sie in Azure Firewall Manager für das virtuelle Spoke-Netzwerk, das das Anwendungsgateway und die Back-End-Pools enthält, die Optionen **Enable Secure Internet Traffic** (Sicheren Internetdatenverkehr aktivieren) und **Enable Secure Private Traffic** (Sicheren privaten Datenverkehr aktivieren) aus.
1. Konfigurieren Sie benutzerdefinierte Routen im Subnetz des Anwendungsgateways.

   * Um sicherzustellen, dass das Anwendungsgateway Datenverkehr direkt an das Internet senden kann, geben Sie die folgende benutzerdefinierte Route an:

     * **Adresspräfix:** 0.0.0.0.0/0
     * **Nächster Hop:** Internet

   * Um sicherzustellen, dass das Anwendungsgateway Datenverkehr über die Azure Firewall-Instanz im virtuellen WAN-Hub an den Back-End-Pool senden kann, geben Sie die folgende benutzerdefinierte Route an:

      * **Adresspräfix:** Subnetz des Back-End-Pools (10.2.0.0/24)
      * **Nächster Hop:** Private Azure Firewall-IP-Adresse

1. Konfigurieren Sie eine benutzerdefinierte Route im Subnetz des Back-End-Pools.

   * **Adresspräfix:** Subnetz des Anwendungsgateways
   * **Nächster Hop:** Private Azure Firewall-IP-Adresse

## <a name="scenario-2---different-vnets"></a><a name="scenario-2"></a>Szenario 2: Verschiedene VNets

In diesem Szenario befinden sich das Anwendungsgateway und die Back-End-Pools in unterschiedlichen virtuellen Netzwerken, die mittels Peering mit einem virtuellen WAN-Hub verbunden sind.

:::image type="content" source="./media/routing-scenarios/secured-application-gateway/different-vnets.png" alt-text="Diagramm für Szenario 2" lightbox="./media/routing-scenarios/secured-application-gateway/different-vnets.png":::

### <a name="workflow"></a>Workflow

Derzeit werden Routen, die virtuellen Spoke-Netzwerken von der Virtual WAN-Routingtabelle angekündigt werden, auf das gesamte virtuelle Netzwerk angewendet und nicht auf die Subnetze des Spoke-VNet. Daher sind benutzerdefinierte Routen erforderlich, um dieses Szenario zu ermöglichen. Informationen zu benutzerdefinierten Routen (User-Defined Routes, UDRs) finden Sie unter [Benutzerdefinierte virtuelle Netzwerkrouten](../virtual-network/virtual-networks-udr-overview.md#user-defined).

1. Wählen Sie in **Azure Firewall Manager** die Optionen **Enable Secure Internet Traffic** (Sicheren Internetdatenverkehr aktivieren) und **Enable Secure Private Traffic** (Sicheren privaten Datenverkehr aktivieren) für beide virtuellen Spoke-Netzwerke aus.

1. Konfigurieren Sie benutzerdefinierte Routen im Subnetz des Anwendungsgateways. Um sicherzustellen, dass das Anwendungsgateway Datenverkehr direkt an das Internet senden kann, geben Sie die folgende benutzerdefinierte Route an:

   * **Adresspräfix:** 0.0.0.0.0/0
   * **Nächster Hop:** Internet

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum Routing für virtuelle Hubs finden Sie unter [Informationen zum Routing virtueller Hubs](about-virtual-hub-routing.md).
* Weitere Informationen zu benutzerdefinierten Routen finden Sie unter [Benutzerdefinierte virtuelle Netzwerkrouten](../virtual-network/virtual-networks-udr-overview.md#user-defined).
* Informationen zu geschützten virtuellen WAN-Hubs finden Sie unter [Geschützte virtuelle Hubs](../firewall-manager/secured-virtual-hub.md).
