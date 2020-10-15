---
title: 'VPN Gateway: Ändern der Einstellungen für die Gateway-IP-Adresse: Azure-Portal'
description: In diesem Artikel wird erläutert, wie Sie die IP-Adresspräfixe für Ihr lokales Netzwerkgateway mithilfe des Azure-Portals ändern.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 09/16/2020
ms.author: cherylmc
ms.openlocfilehash: af3513c4a4f3b3187e85c65de51ad2e6e2d7279c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90983162"
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

## <a name="local-network-gateway-configuration"></a><a name="configure-lng"></a>Konfiguration des lokalen Netzwerkgateways

Der folgende Screenshot zeigt die Seite **Konfiguration** einer lokalen Netzwerkgateway-Ressource mit öffentlichem IP-Adressenendpunkt:

:::image type="content" source="./media/vpn-gateway-modify-local-network-gateway-portal/ip-address.png" alt-text="Konfigurieren des lokalen Netzwerkgateways: IP-Adresse":::

Dies ist die gleiche Konfigurationsseite mit einem FQDN-Endpunkt:

:::image type="content" source="./media/vpn-gateway-modify-local-network-gateway-portal/fqdn.png" alt-text="Konfigurieren des lokalen Netzwerkgateways: IP-Adresse":::

## <a name="modify-the-gateway-ip-address"></a><a name="ip"></a>Ändern der Gateway-IP-Adresse

Wenn für das VPN-Gerät, mit dem Sie eine Verbindung herstellen möchten, die öffentliche IP-Adresse geändert wurde, müssen Sie das Gateway des lokalen Netzwerks entsprechend anpassen.

1. Klicken Sie für die Ressource für das lokale Netzwerkgateway unter **Einstellungen** auf **Konfiguration**.
2. Ändern Sie im Feld **IP-Adresse** die IP-Adresse.
3. Klicken Sie auf **Speichern** , um die Einstellungen zu speichern.

## <a name="modify-the-gateway-fqdn"></a><a name="fqdn"></a>Ändern des FQDN des Gateways

Wenn für das VPN-Gerät, mit dem Sie eine Verbindung herstellen möchten, der FQDN (Fully Qualified Domain Name, vollqualifizierter Domänenname) geändert wurde, müssen Sie das Gateway des lokalen Netzwerks entsprechend anpassen.

1. Klicken Sie für die Ressource für das lokale Netzwerkgateway unter **Einstellungen** auf **Konfiguration**.
2. Ändern Sie im Feld **FQDN** den Domänennamen.
3. Klicken Sie auf **Speichern** , um die Einstellungen zu speichern.

> ![HINWEIS] Sie können ein lokales Netzwerkgateway zwischen FQDN-Endpunkt und IP-Adressenendpunkt nicht ändern. Sie müssen alle Verbindungen löschen, die mit diesem lokalen Netzwerkgateway verbunden sind, ein neues mit dem neuen Endpunkt (IP-Adresse oder FQDN) erstellen und dann die Verbindungen neu erstellen.

## <a name="modify-ip-address-prefixes"></a><a name="ipaddprefix"></a>Ändern von IP-Adresspräfixen

### <a name="to-add-additional-address-prefixes"></a>So fügen Sie weitere Adresspräfixe hinzu:

1. Klicken Sie für die Ressource für das lokale Netzwerkgateway unter **Einstellungen** auf **Konfiguration**.
2. Fügen Sie den IP-Adressbereich im Feld *Zusätzlichen Adressbereich hinzufügen* hinzu.
3. Klicken Sie auf **Save**, um Ihre Einstellungen zu speichern.

### <a name="to-remove-address-prefixes"></a>So entfernen Sie Adresspräfixe:

1. Klicken Sie für die Ressource für das lokale Netzwerkgateway unter **Einstellungen** auf **Konfiguration**.
2. Klicken Sie in der Zeile mit dem Präfix, das Sie entfernen möchten, auf **...**
3. Klicken Sie auf **Entfernen**.
4. Klicken Sie auf **Save**, um Ihre Einstellungen zu speichern.

## <a name="modify-bgp-settings"></a><a name="bgp"></a>Ändern von BGP-Einstellungen

### <a name="to-add-or-update-bgp-settings"></a>So können Sie BGP-Einstellungen hinzufügen oder aktualisieren

1. Klicken Sie für die Ressource für das lokale Netzwerkgateway unter **Einstellungen** auf **Konfiguration**.
2. Wählen Sie **BGP-Einstellungen konfigurieren** aus, um die BGP-Konfigurationen für dieses lokale Netzwerkgateway anzuzeigen oder zu aktualisieren.
3. Hinzufügen oder Aktualisieren der autonomen Systemnummer bzw. IP-Adresse des BGP-Peers in den entsprechenden Feldern
4. Klicken Sie auf **Save**, um Ihre Einstellungen zu speichern.

### <a name="to-remove-bgp-settings"></a>So entfernen Sie BGP-Einstellungen

1. Klicken Sie für die Ressource für das lokale Netzwerkgateway unter **Einstellungen** auf **Konfiguration**.
2. Deaktivieren Sie **BGP-Einstellungen konfigurieren**, um die vorhandene ASN oder IP-Adresse des BGP-Peers zu entfernen.
3. Klicken Sie auf **Save**, um Ihre Einstellungen zu speichern.

## <a name="next-steps"></a>Nächste Schritte

Sie können die Gatewayverbindung überprüfen. Informationen finden Sie unter [Überprüfen einer Gatewayverbindung](vpn-gateway-verify-connection-resource-manager.md).
