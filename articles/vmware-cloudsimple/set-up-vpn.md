---
title: 'Azure VMware Solutions (AVS): Konfigurieren eines VPN zwischen der lokalen Umgebung und der privaten AVS-Cloud'
description: Beschreibt, wie ein Site-to-Site- oder Point-to-Site-VPN-Gateway zwischen Ihrem lokalen Netzwerk und Ihrer privaten AVS-Cloud konfiguriert wird
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/14/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: fbd2b227c9292593a7652044ef4c013bf0cfaf8e
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77017002"
---
# <a name="configure-a-vpn-connection-to-your-avs-private-cloud"></a>Konfigurieren einer VPN-Verbindung mit Ihrer privaten AVS-Cloud

VPN-Gateways ermöglichen Ihnen, eine Remoteverbindung mit einem AVS-Netzwerk aus Ihrem lokalen Netzwerk und über einen Clientcomputer herzustellen. In diesem Artikel finden Sie Informationen zum Einrichten von VPN-Gateways über das AVS-Portal. Eine VPN-Verbindung zwischen Ihrem lokalen Netzwerk und Ihrem AVS-Netzwerk ermöglicht den Zugriff auf vCenter und Workloads in Ihrer privaten AVS-Cloud. AVS unterstützt Point-to-Site- und Site-to-Site-VPN-Gateways.

## <a name="vpn-gateway-types"></a>VPN-Gatewaytypen

* Eine **Point-to-Site-VPN**-Verbindung ist die einfachste Möglichkeit, auf Ihrem Computer eine Verbindung mit Ihrer privaten AVS-Cloud herzustellen. Verwenden Sie eine Point-to-Site-VPN-Verbindung, wenn Sie eine Remoteverbindung mit der privaten AVS-Cloud herstellen.
* Eine **Site-to-Site-VPN**-Verbindung ermöglicht Ihnen das Einrichten der Workloads Ihrer privaten AVS-Cloud, um auf lokale Dienste zuzugreifen. Sie können auch ein lokales Active Directory als Identitätsquelle für die Authentifizierung bei Ihrer vCenter-Instanz in der privaten AVS-Cloud verwenden. Derzeit wird der **richtlinienbasierte VPN**-Typ unterstützt.

In einer Region können Sie ein Site-to-Site-VPN-Gateway und ein Point-to-Site-VPN-Gateway erstellen.

## <a name="point-to-site-vpn"></a>Punkt-zu-Standort-VPN

Informationen zum Erstellen eines Point-to-Site-VPN-Gateways finden Sie unter [Erstellen eines Point-to-Site-VPN-Gateways](vpn-gateway.md#create-point-to-site-vpn-gateway).

### <a name="connect-to-avs-using-point-to-site-vpn"></a>Herstellen einer Verbindung mit AVS über ein Point-to-Site-VPN

Es ist ein VPN-Client erforderlich, um von Ihrem Computer aus eine Verbindung mit AVS herzustellen. Laden Sie den [OpenVPN-Client](https://openvpn.net/community-downloads/) für Windows oder [Viscosity](https://www.sparklabs.com/viscosity/download/) für Mac OS und OS X herunter.

1. Starten Sie das AVS-Portal, und wählen Sie **Network** (Netzwerk) aus.
2. Wählen Sie **VPN Gateway** aus.
3. Klicken Sie in der Liste der VPN-Gateways auf das Point-to-Site-VPN-Gateway.
4. Wählen Sie **Benutzer** aus.
5. Klicken Sie auf **Download my VPN configuration**.

    ![Herunterladen der VPN-Konfiguration](media/download-p2s-vpn-configuration.png)

6. Importieren Sie die Konfiguration in Ihren VPN-Client.

    * Anweisungen zum [Importieren der Konfiguration auf einem Windows-Client](https://openvpn.net/vpn-server-resources/connecting-to-access-server-with-windows/#openvpn-open-source-openvpn-gui-program)
    * Anweisungen zum [Importieren der Konfiguration unter macOS oder OS X](https://www.sparklabs.com/support/kb/article/getting-started-with-viscosity-mac/#creating-your-first-connection)

7. Stellen Sie eine Verbindung mit dem AVS-VPN-Gateway her.

Das Beispiel unten zeigt das Importieren einer Verbindung mit dem **Viscosity-Client**.

#### <a name="import-connection-on-viscosity-client"></a>Importieren der Verbindung mit dem Viscosity-Client

1. Extrahieren Sie den Inhalt der VPN-Konfiguration aus der heruntergeladenen ZIP-Datei.

2. Öffnen Sie Viscosity auf Ihrem Computer.

3. Klicken Sie auf das Symbol **+** ,und wählen Sie **Import Connection (Verbindung importieren)**  > **From File (Aus Datei)** aus.

    ![Importieren der VPN-Konfiguration aus einer Datei](media/import-p2s-vpn-config.png)

4. Wählen Sie die OpenVPN-Konfigurationsdatei (OVPN-Datei) für das Protokoll aus, das Sie verwenden möchten, und klicken Sie auf **Open** (Öffnen).

    ![VPN](media/import-p2s-vpn-config-choose-ovpn.png)

Die Verbindung wird jetzt im Menü „Viscosity“ angezeigt.

#### <a name="connect-to-the-vpn"></a>Stellen Sie eine VPN-Verbindung her.

Wählen Sie zum Herstellen einer Verbindung mit dem VPN mit dem Viscosity OpenVPN-Client die Verbindung im Menü aus. Das Menüsymbol wird aktualisiert, um anzugeben, dass die Verbindung hergestellt wurde.

![VPN](media/vis03.png)

### <a name="connecting-to-multiple-avs-private-clouds"></a>Herstellen von Verbindungen mit mehreren privaten AVS-Clouds

Eine Point-to-Site-VPN-Verbindung löst die DNS-Namen der ersten erstellten privaten AVS-Cloud auf. Wenn Sie auf andere private AVS-Clouds zugreifen möchten, müssen Sie den DNS-Server für Ihren VPN-Client aktualisieren.

1. Starten Sie das [AVS-Portal](access-cloudsimple-portal.md).

2. Navigieren Sie zu **Resources** > **AVS Private Clouds** (Ressourcen > Private AVS-Clouds), und wählen Sie die private AVS-Cloud aus, mit der Sie eine Verbindung herstellen möchten.

3. Kopieren Sie auf der Seite **Summary** (Zusammenfassung) der privaten AVS-Cloud die IP-Adresse des DNS-Servers für die private AVS-Cloud unter **Basic Info** (Grundlegende Informationen).

    ![DNS-Server in der privaten AVS-Cloud](media/private-cloud-dns-server.png)

4. Klicken Sie mit der rechten Maustaste auf das Symbol „Viscosity“ in der Systemleiste Ihres Computers, und wählen Sie **Preferences** (Einstellungen) aus.

    ![VPN](media/vis00.png)

5. Wählen Sie die AVS-VPN-Verbindung aus.

    ![VPN-Verbindung](media/viscosity-client.png)

6. Klicken Sie auf **Edit** (Bearbeiten), um die Verbindungseigenschaften zu ändern.

    ![Bearbeiten der VPN-Verbindung](media/viscosity-edit-connection.png)

7. Klicken Sie auf die Registerkarte **Networking** (Netzwerk), und geben Sie die IP-Adressen der DNS-Server der privaten AVS-Cloud getrennt durch Kommas oder Leerzeichen sowie die Domäne als ```AVS.io``` ein. Wählen Sie **Ignore DNS settings sent by VPN server** (Vom VPN-Server gesendete DNS-Einstellungen ignorieren) aus.

    ![VPN-Netzwerk](media/viscosity-edit-connection-networking.png)

> [!IMPORTANT]
> Zum Herstellen einer Verbindung mit Ihrer ersten privaten AVS-Cloud entfernen Sie diese Einstellungen und stellen eine Verbindung mit dem VPN-Server her.

## <a name="site-to-site-vpn"></a>Site-to-Site-VPN-Verbindung

Informationen zum Erstellen eines Site-to-Site-VPN-Gateways finden Sie unter [Erstellen eines Site-to-Site-VPN-Gateways](vpn-gateway.md#set-up-a-site-to-site-vpn-gateway). Die Site-to-Site-VPN-Verbindung zwischen Ihrem lokalen Netzwerk und Ihrer privaten AVS-Cloud bietet die folgenden Vorteile. 

* Zugriff auf die vCenter-Instanz Ihrer privaten AVS-Cloud von jeder Arbeitsstation in Ihrem lokalen Netzwerk aus
* Verwenden Ihres lokalen Active Directory als vCenter-Identitätsquelle.
* Einfaches Übertragen von VM-Vorlagen, ISO-Dateien und anderen Dateien aus Ihren lokalen Ressourcen in die vCenter-Instanz Ihrer privaten AVS-Cloud
* Zugreifen auf Workloads, die in Ihrer privaten AVS-Cloud ausgeführt werden, aus dem lokalen Netzwerk

Informationen zum Einrichten des lokalen VPN-Gateways im Hochverfügbarkeitsmodus finden Sie unter [Konfigurieren einer VPN-Verbindung mit Hochverfügbarkeit](high-availability-vpn-connection.md).

> [!IMPORTANT]
>    1. Legen Sie TCP MSS-Clamping für das VPN-Gerät auf 1200 fest. Wenn Ihre VPN-Geräte MSS-Clamping nicht unterstützen, können Sie stattdessen auch den MTU-Wert der Tunnelschnittstelle auf 1240 Bytes festlegen.
> 2. Nach dem Einrichten des Site-to-Site-VPN können Sie die DNS-Anforderungen für *.avs.io an die DNS-Server der privaten AVS-Cloud weiterleiten. Befolgen Sie die Anweisungen unter [Einrichten des lokalen DNS](on-premises-dns-setup.md).
