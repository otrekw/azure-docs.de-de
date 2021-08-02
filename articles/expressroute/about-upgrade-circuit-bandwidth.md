---
title: Informationen zum Upgrade der Leitungsbandbreite | Azure ExpressRoute
description: In diesem Artikel lernen Sie die Best Practices für ein Upgrade der Leitungsbandbreite für ExpressRoute kennen.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: conceptual
ms.date: 06/08/2021
ms.author: duau
ms.openlocfilehash: 75570beb243fca802ecebebf34edf02b3d8ca9a5
ms.sourcegitcommit: 190658142b592db528c631a672fdde4692872fd8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2021
ms.locfileid: "112006489"
---
# <a name="about-upgrading-expressroute-circuit-bandwidth"></a>Informationen zum Upgrade der Leitungsbandbreite für ExpressRoute

Bei ExpressRoute handelt es sich um eine dedizierte, private Verbindung mit dem globalen Netzwerk von Microsoft. Diese Konnektivität wird durch ein ExpressRoute-Partnernetzwerk oder durch Direktverbindung mit MSEE-Geräten (Microsoft Enterprise Edge) ermöglicht. Sobald die physische Konnektivität konfiguriert und getestet wurde, können Sie die Layer-2- und Layer-3-Konnektivität aktivieren, indem Sie eine ExpressRoute-Leitung erstellen und das Peering konfigurieren.

## <a name="upgrade-circuit-bandwidth"></a><a name="upgrade"></a>Upgrade der Leitungsbandbreite

Für ein Upgrade der Leitungsbandbreite benötigt der ExpressRoute Direct- oder ExpressRoute-Partner [ausreichend verfügbare Bandbreite](#considerations), damit das Upgrade erfolgreich durchgeführt werden kann.

Wenn die erforderliche Kapazität vorhanden ist, können Sie ein Upgrade der Leitung mithilfe der folgenden Methoden durchführen:

* [Azure portal](expressroute-howto-circuit-portal-resource-manager.md#modify)
* [PowerShell](expressroute-howto-circuit-arm.md#modify)
* [Azure-Befehlszeilenschnittstelle](howto-circuit-cli.md#modify)

## <a name="capacity-considerations"></a><a name="considerations"></a>Überlegungen zur Kapazität

### <a name="insufficient-capacity-for-physical-connection"></a>Unzureichende Kapazität für physische Verbindungen

Eine ExpressRoute-Leitung wird über eine physische Verbindung zwischen Microsoft und einem ExpressRoute-Partner erstellt. Die physische Verbindung verfügt über eine feste Kapazität. Wenn Sie die Leitungsgröße nicht erhöhen können, bedeutet dies, dass die zugrunde liegende physische Verbindung für Ihre vorhandene Leitung keine Kapazität für das Upgrade hat. Sie müssen eine neue Leitung erstellen, wenn Sie die Leitungsgröße ändern möchten.

Nachdem Sie die neue ExpressRoute-Leitung erfolgreich erstellt haben, müssen Sie Ihre vorhandenen virtuellen Netzwerke mit dieser Leitung verknüpfen. Anschließend können Sie die Konnektivität der neuen ExpressRoute-Leitung testen und überprüfen, bevor Sie die Bereitstellung der alten Leitung aufheben. Dies sind die empfohlenen Migrationsschritte, um Ausfallzeiten und Unterbrechungen Ihrer Produktionsworkload zu minimieren.

### <a name="insufficient-expressroute-partner-bandwidth"></a><a name="bandwidth"></a>Unzureichende Bandbreite beim ExpressRoute-Partner

Wenn Sie aufgrund eines Kapazitätsfehlers keine neue ExpressRoute-Verbindung erstellen können, bedeutet das, dass dieser ExpressRoute-Partner nicht über die Kapazität zum Herstellen einer Verbindung mit Microsoft an diesem Peeringstandort verfügt. Wenden Sie sich an Ihren ExpressRoute-Partner, um mehr Kapazität anzufordern.

Sobald die neue Kapazität bereitgestellt wurde, können Sie die Schritte im Abschnitt [Upgrade der Leitungsbandbreite](#upgrade) ausführen, um eine neue Leitung zu erstellen, die Konnektivität zu konfigurieren und die alte Leitung zu löschen.


### <a name="insufficient-expressroute-direct-bandwidth"></a><a name="bandwidth"></a>Unzureichende Bandbreite beim ExpressRoute Direct-Partner

Wenn ExpressRoute Direct nicht über genügend Kapazität verfügt, haben Sie zwei Möglichkeiten. Sie können entweder Leitungen löschen, die der ExpressRoute Direct-Ressource zugewiesen sind, aber nicht mehr benötigt werden, oder eine neue ExpressRoute Direct-Ressource erstellen. Anleitungen zum Verwalten der ExpressRoute Direct-Ressource finden Sie unter [Erstellen von ExpressRoute Direct mit dem Portal](how-to-expressroute-direct-portal.md).

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen und Ändern einer Verbindung](expressroute-howto-circuit-portal-resource-manager.md)
* [Erstellen und Ändern einer Peeringkonfiguration](expressroute-howto-routing-portal-resource-manager.md)
* [Verknüpfen eines virtuellen Netzwerks mit einer ExpressRoute-Verbindung](expressroute-howto-linkvnet-portal-resource-manager.md)
