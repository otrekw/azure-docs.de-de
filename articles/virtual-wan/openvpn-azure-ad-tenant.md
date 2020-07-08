---
title: 'Azure AD-Mandant für Benutzer-VPN-Verbindungen: Azure AD-Authentifizierung'
description: Sie können über das Benutzer-VPN von Azure Virtual WAN (Point-to-Site) mithilfe der Azure AD-Authentifizierung eine Verbindung mit Ihrem VNET herstellen.
titleSuffix: Azure Virtual WAN
services: virtual-wan
author: anzaman
ms.service: virtual-wan
ms.topic: how-to
ms.date: 03/19/2020
ms.author: alzam
ms.openlocfilehash: 76c65d194d03dd1b7ff4cc2f3b45d84ff7909968
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84753355"
---
# <a name="create-an-azure-active-directory-tenant-for-user-vpn-openvpn-protocol-connections"></a>Erstellen eines Azure Active Directory-Mandanten für Benutzer-VPN-Verbindungen über das OpenVPN-Protokoll

Beim Herstellen einer Verbindung mit Ihrem VNet können Sie die zertifikatbasierte Authentifizierung oder die RADIUS-Authentifizierung verwenden. Wenn Sie das Open VPN-Protokoll verwenden, können Sie jedoch auch die Azure Active Directory-Authentifizierung verwenden. In diesem Artikel erfahren Sie, wie Sie einen Azure AD-Mandanten für eine Virtual WAN-Benutzer-VPN-Verbindung mit OpenVPN-Authentifizierung einrichten.

> [!NOTE]
> Die Azure AD-Authentifizierung wird nur für Verbindungen mit dem OpenVPN&reg;-Protokoll unterstützt.
>

## <a name="1-create-the-azure-ad-tenant"></a><a name="tenant"></a>1. Erstellen des Azure AD-Mandanten

Erstellen Sie mit den im Artikel [Erstellen eines neuen Mandanten](../active-directory/fundamentals/active-directory-access-create-new-tenant.md) beschriebenen Schritten einen Azure AD-Mandanten:

* Organisationsname
* Name der Anfangsdomäne

Beispiel:

   ![Neuer Azure AD-Mandant](./media/openvpn-create-azure-ad-tenant/newtenant.png)

## <a name="2-create-azure-ad-tenant-users"></a><a name="users"></a>2. Erstellen von Azure AD-Mandantenbenutzern

Erstellen Sie als Nächstes zwei Benutzerkonten. Erstellen Sie ein globales Administratorkonto und ein Masterbenutzerkonto. Das Masterbenutzerkonto wird als Masterkonto für die Einbettung verwendet (Dienstkonto). Wenn Sie ein Azure AD-Mandantenbenutzerkonto erstellen, passen Sie die Verzeichnisrolle für den Typ des Benutzers an, den Sie erstellen möchten.

Verwenden Sie die Schritte in [diesem Artikel](../active-directory/fundamentals/add-users-azure-active-directory.md), um mindestens zwei Benutzer für Ihren Azure AD-Mandanten zu erstellen. Achten Sie darauf, dass Sie die **Verzeichnisrolle** ändern, um die Kontotypen zu erstellen:

* Globaler Administrator
* Benutzer

## <a name="3-enable-azure-ad-authentication-on-the-vpn-gateway"></a><a name="enable-authentication"></a>3. Aktivieren der Azure AD-Authentifizierung im Aktivieren der Azure AD-Authentifizierung im VPN-Gateway

1. Suchen Sie die Verzeichnis-ID des Verzeichnisses, das Sie für die Authentifizierung verwenden möchten. Sie wird im Abschnitt „Eigenschaften“ der Active Directory-Seite aufgeführt.

    ![Verzeichnis-ID](./media/openvpn-create-azure-ad-tenant/directory-id.png)

2. Kopieren Sie die Verzeichnis-ID.

3. Melden Sie sich beim Azure-Portal als Benutzer an, dem die Rolle als **globaler Administrator** zugewiesen ist.

4. Geben Sie als nächstes Administratoreinwilligung. Kopieren Sie die URL für den jeweiligen Bereitstellungsspeicherort und fügen Sie sie in die Adressleiste Ihres Browsers ein:

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

5. Wählen Sie das Konto **Globaler Administrator** aus, wenn Sie dazu aufgefordert werden.

    ![Verzeichnis-ID](./media/openvpn-create-azure-ad-tenant/pick.png)

6. Wählen Sie **Akzeptieren** aus, wenn Sie dazu aufgefordert werden.

    ![Akzeptieren](./media/openvpn-create-azure-ad-tenant/accept.jpg)

7. Unter ihrem Azure AD wird in **Unternehmensanwendungen** **Azure-VPN** aufgeführt.

    ![Azure-VPN](./media/openvpn-create-azure-ad-tenant/azurevpn.png)

8. Führen Sie die Schritte in [Konfigurieren der Azure AD-Authentifizierung für eine Point-to-Site-Verbindung zu Azure](virtual-wan-point-to-site-azure-ad.md), um die Azure AD-Authentifizierung für das Benutzer-VPN zu konfigurieren und einem virtuellen Hub zuzuweisen.

## <a name="next-steps"></a>Nächste Schritte

Um eine Verbindung mit Ihrem virtuellen Netzwerk herzustellen, müssen Sie ein VPN-Clientprofil erstellen und konfigurieren und einem virtuellen Hub zuordnen. Informationen finden Sie unter [Konfigurieren der Azure AD-Authentifizierung für eine Point-to-Site-Verbindung mit Azure](virtual-wan-point-to-site-azure-ad.md).
