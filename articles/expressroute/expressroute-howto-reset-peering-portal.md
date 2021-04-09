---
title: 'Azure ExpressRoute: Zurücksetzen des Verbindungspeerings mithilfe des Azure-Portals'
description: Erfahren Sie, wie Peerings einer Azure ExpressRoute-Verbindung mithilfe des Azure-Portals deaktiviert und aktiviert werden.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: how-to
ms.date: 11/30/2020
ms.author: duau
ms.openlocfilehash: 432ecedbbb8965926499380eb1165fdf43018426
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "97680279"
---
# <a name="reset-expressroute-circuit-peerings-by-using-the-azure-portal"></a>Zurücksetzen des ExpressRoute-Verbindungspeerings im Azure-Portal

In diesem Artikel wird beschrieben, wie das Peering einer Azure ExpressRoute-Verbindung im Azure-Portal deaktiviert und aktiviert wird. Wenn Sie ein Peering deaktivieren, wird die BGP-Sitzung (Border Gateway Protocol) der primären und sekundären Verbindung Ihrer ExpressRoute-Verbindung heruntergefahren. Wenn Sie das Peering aktivieren, wird die BGP-Sitzung jeweils auf der primären und sekundären Verbindung Ihrer ExpressRoute-Verbindung wiederhergestellt.

> [!Note]
> Wenn Sie die Peerings zum ersten Mal auf Ihrer ExpressRoute-Verbindung konfigurieren, werden diese standardmäßig aktiviert.

Das Zurücksetzen des ExpressRoute-Peerings kann in folgenden Szenarien hilfreich sein:

* Sie Testen Ihren Entwurf und die Implementierung der Notfallwiederherstellung. Sie verfügen beispielsweise über zwei ExpressRoute-Verbindungen. Sie können die Peerings einer Verbindung deaktivieren und erzwingen, dass der Netzwerkdatenverkehr die andere Verbindung verwendet.

* Sie möchten die bidirektionale Weiterleitungserkennung (Bidirectional Forwarding Detection, BFD) für das private Azure-Peering oder Microsoft-Peering aktivieren. Wenn Ihre ExpressRoute-Verbindung vor dem 1. August 2018 für das private Azure-Peering oder vor dem 10. Januar 2020 für das Microsoft-Peering erstellt wurde, war BFD standardmäßig nicht aktiviert. Setzen Sie das Peering zurück, um BFD zu aktivieren.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Navigieren Sie in einem Browser zum [Azure-Portal](https://portal.azure.com), und melden Sie sich mit Ihrem Azure-Konto an.

## <a name="reset-a-peering"></a>Zurücksetzen eines Peerings

Sie können das Microsoft-Peering und das private Azure-Peering unabhängig voneinander für eine ExpressRoute-Verbindung zurücksetzen.

1. Wählen Sie die Verbindung aus, die Sie ändern möchten.

    :::image type="content" source="./media/expressroute-howto-reset-peering-portal/expressroute-circuit-list.png" alt-text="Screenshot, der die Auswahl einer Verbindung in der Liste der ExpressRoute-Verbindungen darstellt":::

1. Wählen Sie die Peeringkonfiguration aus, die Sie zurücksetzen möchten.

    :::image type="content" source="./media/expressroute-howto-reset-peering-portal/expressroute-circuit.png" alt-text="Screenshot, der die Peeringauswahl in der Übersicht der ExpressRoute-Verbindungen darstellt":::

1. Deaktivieren Sie das Kontrollkästchen **Peering aktivieren**, und wählen Sie **Speichern** aus, um die Peeringkonfiguration zu deaktivieren.

    :::image type="content" source="./media/expressroute-howto-reset-peering-portal/disable-peering.png" alt-text="Screenshot, der das Deaktivieren des Kontrollkästchens „Peering aktivieren“ zeigt":::

1. Aktivieren Sie das Kontrollkästchen **Peering aktivieren**, und wählen Sie **Speichern** aus, um die Peeringkonfiguration wieder zu aktivieren.

## <a name="next-steps"></a>Nächste Schritte

Informationen zur Problembehandlung bei ExpressRoute-Verbindungen finden Sie in den folgenden Artikeln:

* [Überprüfen der ExpressRoute-Konnektivität](expressroute-troubleshooting-expressroute-overview.md)
* [Beheben von Problemen bei der Netzwerkleistung](expressroute-troubleshooting-network-performance.md)
