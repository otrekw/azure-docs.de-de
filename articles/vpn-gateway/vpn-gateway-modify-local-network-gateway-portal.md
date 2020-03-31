---
title: 'VPN Gateway: Ändern der Einstellungen für die Gateway-IP-Adresse: Azure-Portal'
description: In diesem Artikel wird erläutert, wie Sie die IP-Adresspräfixe für Ihr lokales Netzwerkgateway mithilfe des Azure-Portals ändern.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 06/19/2017
ms.author: cherylmc
ms.openlocfilehash: fdb98242cede36f818604a7a5d713f5f7c75daad
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "75864025"
---
# <a name="modify-local-network-gateway-settings-using-the-azure-portal"></a>Ändern der Einstellungen des lokalen Netzwerkgateways mithilfe des Azure-Portals

Manchmal ändern sich die Einstellungen für „AddressPrefix“ oder „GatewayIPAddress“ Ihres lokalen Netzwerkgateways. In diesem Artikel wird gezeigt, wie Sie die Einstellungen Ihres lokalen Netzwerkgateways ändern. Sie können diese Einstellungen auch über eine andere Option aus der folgenden Liste ändern:

Bevor Sie die Verbindung löschen, sollten Sie die Konfiguration für Ihre verbundenen Geräte herunterladen, um den definierten PSK zu erhalten. Auf diese Weise müssen Sie ihn nicht auf der anderen Seite definieren.

> [!div class="op_single_selector"]
> * [Azure portal](vpn-gateway-modify-local-network-gateway-portal.md)
> * [PowerShell](vpn-gateway-modify-local-network-gateway.md)
> * [Azure-Befehlszeilenschnittstelle](vpn-gateway-modify-local-network-gateway-cli.md)
>
>


## <a name="modify-ip-address-prefixes"></a><a name="ipaddprefix"></a>Ändern von IP-Adresspräfixen

Wenn Sie IP-Adresspräfixe ändern, hängen die Schritte davon ab, ob Ihr lokales Netzwerkgateway über eine Verbindung verfügt.

[!INCLUDE [modify prefix](../../includes/vpn-gateway-modify-ip-prefix-portal-include.md)]

## <a name="modify-the-gateway-ip-address"></a><a name="gwip"></a>Ändern der Gateway-IP-Adresse

Wenn für das VPN-Gerät, mit dem Sie eine Verbindung herstellen möchten, die öffentliche IP-Adresse geändert wurde, müssen Sie das Gateway des lokalen Netzwerks entsprechend anpassen. Wenn Sie die öffentliche IP-Adresse ändern, hängen die Schritte davon ab, ob Ihr lokales Netzwerkgateway über eine Verbindung verfügt.

[!INCLUDE [modify gateway IP](../../includes/vpn-gateway-modify-lng-gateway-ip-portal-include.md)]

## <a name="next-steps"></a>Nächste Schritte

Sie können die Gatewayverbindung überprüfen. Informationen finden Sie unter [Überprüfen einer Gatewayverbindung](vpn-gateway-verify-connection-resource-manager.md).
