---
title: Informationen zum Upgrade der Leitungsbandbreite | Azure ExpressRoute
description: In diesem Artikel lernen Sie die Best Practices für ein Upgrade der Leitungsbandbreite für ExpressRoute kennen.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 07/07/2020
ms.author: cherylmc
ms.openlocfilehash: a8f5aaa7b2a054aa31198779414387cebf0f0fbd
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86537036"
---
# <a name="about-upgrading-expressroute-circuit-bandwidth"></a>Informationen zum Upgrade der Leitungsbandbreite für ExpressRoute

ExpressRoute ermöglicht eine dedizierte, private Konnektivität mit dem globalen Netzwerk von Microsoft. Diese Konnektivität wird durch ein ExpressRoute-Partnernetzwerk oder durch Direktverbindung mit MSEE-Geräten (Microsoft Enterprise Edge) ermöglicht. Sobald die physische Konnektivität konfiguriert und getestet wurde, können Sie die Layer-2- und Layer-3-Konnektivität aktivieren, indem Sie eine ExpressRoute-Leitung erstellen und das Peering konfigurieren.

## <a name="upgrade-circuit-bandwidth"></a><a name="upgrade"></a>Upgrade der Leitungsbandbreite

Für ein Upgrade der Leitungsbandbreite benötigt der ExpressRoute Direct- oder ExpressRoute-Partner [ausreichend verfügbare Bandbreite](#considerations), damit das Upgrade erfolgreich durchgeführt werden kann.

Wenn die erforderliche Kapazität vorhanden ist, können Sie ein Upgrade der Leitung mithilfe der folgenden Methoden durchführen:

* [Azure portal](expressroute-howto-circuit-portal-resource-manager.md#modify)
* [PowerShell](expressroute-howto-circuit-arm.md#modify)
* [Azure-Befehlszeilenschnittstelle](howto-circuit-cli.md#modify)

## <a name="capacity-considerations"></a><a name="considerations"></a>Überlegungen zur Kapazität

### <a name="insufficient-expressroute-partner-bandwidth"></a><a name="bandwidth"></a>Unzureichende Bandbreite beim ExpressRoute-Partner

Wenn der ExpressRoute-Partner nicht über ausreichend Kapazität verfügt, müssen Sie eine neue Leitung erstellen, die für die gewünschte Bandbreite konfiguriert ist. Um die Konnektivität aufrechtzuerhalten, löschen Sie die alte Leitung erst dann, wenn die neue Leitung vollständig bereitgestellt, das Peering konfiguriert und (bei privatem Peering) das Verbindungsobjekt für das virtuelle ExpressRoute-Netzwerkgateway bereitgestellt wurde.

Wenn Ihr ExpressRoute-Partner nicht über ausreichend Kapazität verfügt, müssen Sie am gewünschten Peeringstandort zusätzliche Kapazität anfordern. Sobald die neue Kapazität bereitgestellt wurde, können Sie die Schritte in den Artikeln im Abschnitt [Upgrade der Leitungsbandbreite](#upgrade) ausführen, um eine neue Leitung zu erstellen, die Konnektivität zu konfigurieren und die alte Leitung zu löschen.


### <a name="insufficient-expressroute-direct-bandwidth"></a><a name="bandwidth"></a>Unzureichende Bandbreite beim ExpressRoute Direct-Partner

Wenn der ExpressRoute Direct-Partner nicht über ausreichend Kapazität verfügt, können Sie entweder Leitungen löschen, die der ExpressRoute Direct-Ressource zugewiesen sind, aber nicht mehr benötigt werden, oder eine neue ExpressRoute Direct-Ressource erstellen. Anleitungen zum Verwalten der ExpressRoute Direct-Ressource finden Sie unter [Konfigurieren von ExpressRoute Direct](how-to-expressroute-direct-portal.md).

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen und Ändern einer Verbindung](expressroute-howto-circuit-portal-resource-manager.md)
* [Erstellen und Ändern einer Peeringkonfiguration](expressroute-howto-routing-portal-resource-manager.md)
* [Verknüpfen eines virtuellen Netzwerks mit einer ExpressRoute-Verbindung](expressroute-howto-linkvnet-portal-resource-manager.md)
