---
title: 'VPN Gateway: Ändern der Einstellungen für die Gateway-IP-Adresse: Azure-Portal'
description: In diesem Artikel wird erläutert, wie Sie die IP-Adresspräfixe für Ihr lokales Netzwerkgateway mithilfe des Azure-Portals ändern.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: cherylmc
ms.openlocfilehash: 3a59f618536d44e838bf840264e70b0b2a43cced
ms.sourcegitcommit: ae8b23ab3488a2bbbf4c7ad49e285352f2d67a68
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2019
ms.locfileid: "74014910"
---
# <a name="modify-local-network-gateway-settings-using-the-azure-portal"></a>Ändern der Einstellungen des lokalen Netzwerkgateways mithilfe des Azure-Portals

Manchmal ändern sich die Einstellungen für „AddressPrefix“ oder „GatewayIPAddress“ Ihres lokalen Netzwerkgateways. In diesem Artikel wird gezeigt, wie Sie die Einstellungen Ihres lokalen Netzwerkgateways ändern. Sie können diese Einstellungen auch über eine andere Option aus der folgenden Liste ändern:

Bevor Sie die Verbindung löschen, sollten Sie die Konfiguration für Ihre verbundenen Geräte herunterladen, um den definierten PSK zu erhalten. Auf diese Weise müssen Sie ihn nicht auf der anderen Seite definieren.

> [!div class="op_single_selector"]
> * [Azure-Portal](vpn-gateway-modify-local-network-gateway-portal.md)
> * [PowerShell](vpn-gateway-modify-local-network-gateway.md)
> * [Azure-Befehlszeilenschnittstelle](vpn-gateway-modify-local-network-gateway-cli.md)
>
>


## <a name="ipaddprefix"></a>Ändern von IP-Adresspräfixen

Wenn Sie IP-Adresspräfixe ändern, hängen die Schritte davon ab, ob Ihr lokales Netzwerkgateway über eine Verbindung verfügt.

[!INCLUDE [modify prefix](../../includes/vpn-gateway-modify-ip-prefix-portal-include.md)]

## <a name="gwip"></a>Ändern der Gateway-IP-Adresse

Wenn für das VPN-Gerät, mit dem Sie eine Verbindung herstellen möchten, die öffentliche IP-Adresse geändert wurde, müssen Sie das Gateway des lokalen Netzwerks entsprechend anpassen. Wenn Sie die öffentliche IP-Adresse ändern, hängen die Schritte davon ab, ob Ihr lokales Netzwerkgateway über eine Verbindung verfügt.

[!INCLUDE [modify gateway IP](../../includes/vpn-gateway-modify-lng-gateway-ip-portal-include.md)]

## <a name="next-steps"></a>Nächste Schritte

Sie können die Gatewayverbindung überprüfen. Informationen finden Sie unter [Überprüfen einer Gatewayverbindung](vpn-gateway-verify-connection-resource-manager.md).
