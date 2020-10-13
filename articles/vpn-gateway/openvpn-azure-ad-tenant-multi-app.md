---
title: 'VPN Gateway: Azure AD-Mandant für verschiedene Benutzergruppen: Azure AD-Authentifizierung'
description: Sie können P2S VPN verwenden, um eine Verbindung mit Azure AD-Authentifizierung zu Ihrem VNet herzustellen.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 10/07/2020
ms.author: cherylmc
ms.openlocfilehash: 1305ca603aef63dafcc7b055d55e3f0fe281f4fc
ms.sourcegitcommit: d2222681e14700bdd65baef97de223fa91c22c55
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/07/2020
ms.locfileid: "91819676"
---
# <a name="create-an-active-directory-ad-tenant-for-p2s-openvpn-protocol-connections"></a>Erstellen eines Active Directory-Mandanten (AD) für Verbindungen mit dem P2S OpenVPN-Protokoll

Beim Herstellen einer Verbindung mit Ihrem VNet können Sie die zertifikatbasierte Authentifizierung oder die RADIUS-Authentifizierung verwenden. Wenn Sie das Open VPN-Protokoll verwenden, können Sie jedoch auch die Azure Active Directory-Authentifizierung verwenden. Wenn Sie möchten, dass sich verschiedene Benutzergruppen mit verschiedenen VPN-Gateways verbinden können, können Sie mehrere Anwendungen im AD registrieren und sie mit verschiedenen VPN-Gateways verknüpfen. Dieser Artikel unterstützt Sie bei der Einrichtung eines Azure AD-Mandanten für die P2S-OpenVPN-Authentifizierung und bei der Erstellung und Registrierung mehrerer Anwendungen in Azure AD, um verschiedenen Benutzern und Gruppen unterschiedliche Zugriffsmöglichkeiten zu geben.

[!INCLUDE [Windows 10 and OpenVPN note](../../includes/vpn-gateway-openvpn-auth-include.md)]

[!INCLUDE [create](../../includes/openvpn-azure-ad-tenant-multi-app.md)]

## <a name="6-enable-authentication-on-the-gateway"></a><a name="enable-authentication"></a>6. Aktivieren der Authentifizierung im Gateway

In diesem Schritt aktivieren Sie die Azure AD-Authentifizierung im VPN-Gateway.

1. Aktivieren Sie die Azure AD-Authentifizierung im VPN-Gateway, indem Sie zu **Punkt-zu-Standort-Konfiguration** navigieren und **OpenVPN (SSL)** als **Tunneltyp** auswählen. Wählen Sie **Azure Active Directory** als **Authentifizierungstyp** aus, und geben Sie dann die entsprechenden Informationen im Abschnitt **Azure Active Directory** ein.

    ![Azure-Portal-Ansicht](./media/openvpn-azure-ad-tenant-multi-app/azure-ad-auth-portal.png)

    > [!NOTE]
    > Verwenden Sie nicht die Anwendungs-ID des Azure-VPN-Clients: Sie erteilt allen Benutzern Zugriff auf das VPN-Gateway. Verwenden Sie die ID der Anwendung(en), die Sie registriert haben.

2. Erstellen Sie das Profil, indem Sie es durch Klicken auf den Link **VPN-Client herunterladen** herunterladen.

3. Extrahieren Sie die heruntergeladene ZIP-Datei.

4. Navigieren Sie zum entzippten Ordner „AzureVPN“.

5. Notieren Sie sich den Speicherort der Datei „azurevpnconfig.xml“. Die Datei „azurevpnconfig.xml“ enthält die Einstellung für die VPN-Verbindung und kann direkt in die Azure VPN Client-Anwendung importiert werden. Sie können diese Datei auch per e-Mail oder anderweitig an alle Benutzer verteilen, die eine Verbindung herstellen müssen. Der Benutzer benötigt gültige Azure AD-Anmeldeinformationen, um eine Verbindung herstellen zu können.

## <a name="next-steps"></a>Nächste Schritte

Um eine Verbindung mit Ihrem virtuellen Netzwerk herzustellen, müssen Sie ein VPN-Clientprofil erstellen und konfigurieren. Entsprechende Informationen finden Sie unter [Konfigurieren eines VPN-Clients für P2S-VPN-Verbindungen](openvpn-azure-ad-client.md).
