---
title: 'VPN Gateway: Ändern der Einstellungen für die Gateway-IP-Adresse: Azure-Portal'
description: In diesem Artikel wird erläutert, wie Sie die IP-Adresspräfixe für Ihr lokales Netzwerkgateway mithilfe des Azure-Portals ändern.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 10/16/2020
ms.author: cherylmc
ms.openlocfilehash: 4ff4f1238764d7bdab6e74d29254a6388ea76d78
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "92143104"
---
# <a name="modify-local-network-gateway-settings-using-the-azure-portal"></a>Ändern der Einstellungen des lokalen Netzwerkgateways mithilfe des Azure-Portals

Manchmal ändern sich die Einstellungen für „AddressPrefix“ oder „GatewayIPAddress“ Ihres lokalen Netzwerkgateways. In diesem Artikel wird gezeigt, wie Sie die Einstellungen Ihres lokalen Netzwerkgateways ändern. Sie können diese Einstellungen auch über eine andere Option aus der folgenden Liste ändern:

> [!div class="op_single_selector"]
> * [Azure portal](vpn-gateway-modify-local-network-gateway-portal.md)
> * [PowerShell](vpn-gateway-modify-local-network-gateway.md)
> * [Azure-Befehlszeilenschnittstelle](vpn-gateway-modify-local-network-gateway-cli.md)
>

## <a name="local-network-gateway-configuration"></a><a name="configure-lng"></a>Konfiguration des lokalen Netzwerkgateways

Der folgende Screenshot zeigt die Seite **Konfiguration** einer lokalen Netzwerkgateway-Ressource mit öffentlichem IP-Adressenendpunkt:

:::image type="content" source="./media/vpn-gateway-modify-local-network-gateway-portal/settings.png" alt-text="Einstellungen für IP-Adressen" lightbox="./media/vpn-gateway-modify-local-network-gateway-portal/settings-expand.png":::

Dies ist die Konfigurationsseite mit einem FQDN-Endpunkt:

:::image type="content" source="./media/vpn-gateway-modify-local-network-gateway-portal/name.png" alt-text="FQDN-Einstellungen":::

## <a name="to-modify-the-gateway-ip-address-or-fqdn"></a><a name="ip"></a>So ändern Sie die Gateway-IP-Adresse oder den FQDN

> [!NOTE]
> Sie können das Gateway eines lokalen Netzwerks zwischen FQDN-Endpunkt und IP-Adressenendpunkt nicht ändern. Sie müssen alle Verbindungen löschen, die mit diesem lokalen Netzwerkgateway verbunden sind, ein neues mit dem neuen Endpunkt (IP-Adresse oder FQDN) erstellen und dann die Verbindungen neu erstellen.
>

Wenn für das VPN-Gerät, mit dem Sie eine Verbindung herstellen möchten, die öffentliche IP-Adresse geändert wurde, ändern Sie das Gateway des lokalen Netzwerks mithilfe der folgenden Schritte:

1. Wählen Sie für die Ressource für das Gateway des lokalen Netzwerks im Abschnitt **Einstellungen** die Option **Konfiguration** aus.
2. Ändern Sie im Feld **IP-Adresse** die IP-Adresse.
3. Klicken Sie auf **Speichern**, um die Einstellungen zu speichern.

Wenn für das VPN-Gerät, mit dem Sie eine Verbindung herstellen möchten, der FQDN (Fully Qualified Domain Name, vollqualifizierter Domänenname) geändert wurde, ändern Sie das Gateway des lokalen Netzwerks mithilfe der folgenden Schritte:

1. Wählen Sie für die Ressource für das Gateway des lokalen Netzwerks im Abschnitt **Einstellungen** die Option **Konfiguration** aus.
2. Ändern Sie im Feld **FQDN** den Domänennamen.
3. Klicken Sie auf **Speichern**, um die Einstellungen zu speichern.

## <a name="to-modify-ip-address-prefixes"></a><a name="ipaddprefix"></a>So ändern Sie IP-Adresspräfixe

So fügen Sie weitere Adresspräfixe hinzu:

1. Wählen Sie für die Ressource für das Gateway des lokalen Netzwerks im Abschnitt **Einstellungen** die Option **Konfiguration** aus.
2. Fügen Sie den IP-Adressbereich im Feld *Zusätzlichen Adressbereich hinzufügen* hinzu.
3. Klicken Sie auf **Save** (Speichern), um Ihre Einstellungen zu speichern.

So entfernen Sie Adresspräfixe:

1. Wählen Sie für die Ressource für das Gateway des lokalen Netzwerks im Abschnitt **Einstellungen** die Option **Konfiguration** aus.
2. Wählen Sie in der Zeile mit dem Präfix, das Sie entfernen möchten, die Auslassungspunkte ( **...** ) aus.
3. Wählen Sie **Entfernen**.
4. Klicken Sie auf **Save** (Speichern), um Ihre Einstellungen zu speichern.

## <a name="to-modify-bgp-settings"></a><a name="bgp"></a>So ändern Sie BGP-Einstellungen

So können Sie BGP-Einstellungen hinzufügen oder aktualisieren

1. Wählen Sie für die Ressource für das Gateway des lokalen Netzwerks im Abschnitt **Einstellungen** die Option **Konfiguration** aus.
2. Wählen Sie **BGP-Einstellungen konfigurieren** aus, um die BGP-Konfigurationen für dieses lokale Netzwerkgateway anzuzeigen oder zu aktualisieren.
3. Hinzufügen oder Aktualisieren der autonomen Systemnummer bzw. IP-Adresse des BGP-Peers in den entsprechenden Feldern
4. Klicken Sie auf **Save** (Speichern), um Ihre Einstellungen zu speichern.

So entfernen Sie BGP-Einstellungen

1. Wählen Sie für die Ressource für das Gateway des lokalen Netzwerks im Abschnitt **Einstellungen** die Option **Konfiguration** aus.
2. Deaktivieren Sie **BGP-Einstellungen konfigurieren**, um die vorhandene ASN oder IP-Adresse des BGP-Peers zu entfernen.
3. Klicken Sie auf **Save** (Speichern), um Ihre Einstellungen zu speichern.

## <a name="next-steps"></a>Nächste Schritte

Sie können die Gatewayverbindung überprüfen. Informationen finden Sie unter [Überprüfen einer Gatewayverbindung](vpn-gateway-verify-connection-resource-manager.md).
