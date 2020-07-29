---
title: 'Azure AD-Mandant für Benutzer-VPN-Verbindungen: Azure AD-Authentifizierung'
description: Sie können über das Benutzer-VPN von Azure Virtual WAN (Point-to-Site) mithilfe der Azure AD-Authentifizierung eine Verbindung mit Ihrem VNET herstellen.
titleSuffix: Azure Virtual WAN
services: virtual-wan
author: kumudD
ms.service: virtual-wan
ms.topic: how-to
ms.date: 03/19/2020
ms.author: alzam
ms.openlocfilehash: e88437dc03772348ebbe0d179afc7fd4ddd24bd9
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86507555"
---
# <a name="prepare-azure-active-directory-tenant-for-user-vpn-openvpn-protocol-connections"></a>Vorbereiten eines Azure Active Directory-Mandanten für Benutzer-VPN-Verbindungen über das OpenVPN-Protokoll

Beim Herstellen einer Verbindung mit Ihrem virtuellen Hub über das IKEv2-Protokoll können Sie die zertifikatbasierte Authentifizierung oder die RADIUS-Authentifizierung nutzen. Bei Verwendung des OpenVPN-Protokolls können Sie auch die Azure Active Directory-Authentifizierung nutzen. In diesem Artikel wird beschrieben, wie Sie einen Azure AD-Mandanten für eine Virtual WAN-Benutzer-VPN-Verbindung (Point-to-Site) mit OpenVPN-Authentifizierung einrichten.

> [!NOTE]
> Die Azure AD-Authentifizierung wird nur für Verbindungen mit dem OpenVPN&reg;-Protokoll unterstützt.
>

## <a name="1-create-the-azure-ad-tenant"></a><a name="tenant"></a>1. Erstellen des Azure AD-Mandanten

Stellen Sie sicher, dass Sie über einen Azure AD-Mandanten verfügen. Wenn Sie über keinen Azure AD Mandanten verfügen, können Sie mithilfe der Schritte im Artikel [Erstellen eines neuen Mandanten](../active-directory/fundamentals/active-directory-access-create-new-tenant.md) einen neuen Mandanten erstellen:

* Organization name (Name der Organisation)
* Name der Anfangsdomäne

Beispiel:

   ![Neuer Azure AD-Mandant](./media/openvpn-create-azure-ad-tenant/newtenant.png)

## <a name="2-create-azure-ad-tenant-users"></a><a name="users"></a>2. Erstellen von Azure AD-Mandantenbenutzern

Erstellen Sie als Nächstes auf dem neu erstellten Azure AD-Mandanten zwei Benutzerkonten: ein Konto eines globalen Administrators und ein Benutzerkonto. Das Benutzerkonto kann verwendet werden, um die OpenVPN-Authentifizierung zu testen, und das Konto für den globalen Administrator wird genutzt, um die Einwilligung für die Registrierung der Azure-VPN-App zu erteilen. Nachdem Sie ein Azure AD-Benutzerkonto erstellt haben, weisen Sie dem Benutzer eine **Verzeichnisrolle** zu, um Administratorberechtigungen zu delegieren.

Verwenden Sie die Schritte in [diesem Artikel](../active-directory/fundamentals/add-users-azure-active-directory.md), um die beiden Benutzer für Ihren Azure AD-Mandanten zu erstellen. Achten Sie darauf, dass Sie die **Verzeichnisrolle** für eines der erstellten Konten in **Globaler Administrator** ändern.

## <a name="3-grant-consent-to-the-azure-vpn-app-registration"></a><a name="enable-authentication"></a>3. Erteilen der Einwilligung für die Registrierung der Azure-VPN-App

1. Melden Sie sich beim Azure-Portal als Benutzer an, dem die Rolle als **globaler Administrator** zugewiesen ist.

2. Erteilen Sie als Nächstes die Administratoreinwilligung für Ihre Organisation. Dies ermöglicht der Azure-VPN-Anwendung die Anmeldung und das Lesen von Benutzerprofilen. Kopieren Sie die URL für den jeweiligen Bereitstellungsspeicherort und fügen Sie sie in die Adressleiste Ihres Browsers ein:

    Öffentlich

    ```
    https://login.microsoftonline.com/common/oauth2/authorize?client_id=41b23e61-6c1e-4545-b367-cd054e0ed4b4&response_type=code&redirect_uri=https://portal.azure.com&nonce=1234&prompt=admin_consent
    ````

    Azure Government

    ```
    https://login-us.microsoftonline.com/common/oauth2/authorize?client_id=51bb15d4-3a4f-4ebf-9dca-40096fe32426&response_type=code&redirect_uri=https://portal.azure.us&nonce=1234&prompt=admin_consent
    ````

    Microsoft Cloud Deutschland

    ```
    https://login-us.microsoftonline.de/common/oauth2/authorize?client_id=538ee9e6-310a-468d-afef-ea97365856a9&response_type=code&redirect_uri=https://portal.microsoftazure.de&nonce=1234&prompt=admin_consent
    ````

    Azure China 21Vianet

    ```
    https://https://login.chinacloudapi.cn/common/oauth2/authorize?client_id=49f817b6-84ae-4cc0-928c-73f27289b3aa&response_type=code&redirect_uri=https://portal.azure.cn&nonce=1234&prompt=admin_consent
    ```

3. Wählen Sie das Konto vom Typ **Globaler Administrator** aus, wenn die Aufforderung angezeigt wird.

    ![Verzeichnis-ID](./media/openvpn-create-azure-ad-tenant/pick.png)

4. Wählen Sie **Akzeptieren** aus, wenn Sie dazu aufgefordert werden.

    ![Akzeptieren](./media/openvpn-create-azure-ad-tenant/accept.jpg)

5. In Azure AD sollte unter **Unternehmensanwendungen** jetzt der Eintrag **Azure-VPN** aufgeführt sein.

    ![Azure-VPN](./media/openvpn-create-azure-ad-tenant/azurevpn.png)

## <a name="next-steps"></a>Nächste Schritte

Zum Herstellen einer Verbindung mit Ihren virtuellen Netzwerken per Azure AD-Authentifizierung müssen Sie eine Benutzer-VPN-Konfiguration erstellen und einem virtuellen Hub zuordnen. Informationen finden Sie unter [Konfigurieren der Azure AD-Authentifizierung für eine Point-to-Site-Verbindung mit Azure](virtual-wan-point-to-site-azure-ad.md).
