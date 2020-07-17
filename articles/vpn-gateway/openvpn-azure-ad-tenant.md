---
title: 'VPN Gateway: Azure AD-Mandant für P2S-VPN-Verbindungen: Azure AD-Authentifizierung'
description: Sie können P2S VPN verwenden, um eine Verbindung mit Azure AD-Authentifizierung zu Ihrem VNet herzustellen.
services: vpn-gateway
author: anzaman
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 04/17/2020
ms.author: alzam
ms.openlocfilehash: 2dda6cb84fc881b4ca628ff1cecdec7c00555e8b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85414305"
---
# <a name="create-an-azure-active-directory-tenant-for-p2s-openvpn-protocol-connections"></a>Erstellen eines Azure Active Directory-Mandanten für Verbindungen mit dem P2S OpenVPN-Protokoll

Beim Herstellen einer Verbindung mit Ihrem VNet können Sie die zertifikatbasierte Authentifizierung oder die RADIUS-Authentifizierung verwenden. Wenn Sie das Open VPN-Protokoll verwenden, können Sie jedoch auch die Azure Active Directory-Authentifizierung verwenden. Dieser Artikel unterstützt Sie beim Einrichten eines Azure AD-Mandanten für die P2S Open VPN-Authentifizierung.

> [!NOTE]
> Die Azure AD-Authentifizierung wird nur für Verbindungen mit dem OpenVPN®-Protokoll unterstützt.
>


## <a name="1-verify-azure-ad-tenant"></a><a name="tenant"></a>1. Überprüfen des Azure AD-Mandanten

Stellen Sie sicher, dass Sie über einen Azure AD-Mandanten verfügen. Wenn Sie über keinen Azure AD Mandanten verfügen, können Sie mithilfe der Schritte im Artikel [Erstellen eines neuen Mandanten](../active-directory/fundamentals/active-directory-access-create-new-tenant.md) einen neuen Mandanten erstellen:

* Organisationsname
* Name der Anfangsdomäne

Beispiel:

   ![Neuer Azure AD-Mandant](./media/openvpn-create-azure-ad-tenant/newtenant.png)

## <a name="2-create-azure-ad-tenant-users"></a><a name="users"></a>2. Erstellen von Azure AD-Mandantenbenutzern

Ihr Azure AD Mandant benötigt die folgenden Konten: ein globales Administratorkonto und ein Masterbenutzerkonto. Das Masterbenutzerkonto wird als Masterkonto für die Einbettung verwendet (Dienstkonto). Wenn Sie ein Azure AD-Mandantenbenutzerkonto erstellen, passen Sie die Verzeichnisrolle für den Typ des Benutzers an, den Sie erstellen möchten.

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
   https://login.microsoftonline.us/common/oauth2/authorize?client_id=51bb15d4-3a4f-4ebf-9dca-40096fe32426&response_type=code&redirect_uri=https://portal.azure.us&nonce=1234&prompt=admin_consent
    ````

    Microsoft Cloud Deutschland

    ```
    https://login-us.microsoftonline.de/common/oauth2/authorize?client_id=538ee9e6-310a-468d-afef-ea97365856a9&response_type=code&redirect_uri=https://portal.microsoftazure.de&nonce=1234&prompt=admin_consent
    ````

    Azure China 21Vianet

    ```
    https://login.chinacloudapi.cn/common/oauth2/authorize?client_id=49f817b6-84ae-4cc0-928c-73f27289b3aa&response_type=code&redirect_uri=https://portal.azure.cn&nonce=1234&prompt=admin_consent
    ```

5. Wählen Sie das Konto **Globaler Administrator** aus, wenn Sie dazu aufgefordert werden.

    ![Verzeichnis-ID](./media/openvpn-create-azure-ad-tenant/pick.png)

6. Wählen Sie **Akzeptieren** aus, wenn Sie dazu aufgefordert werden.

    ![Akzeptieren](./media/openvpn-create-azure-ad-tenant/accept.jpg)

7. Unter ihrem Azure AD wird in **Unternehmensanwendungen** **Azure-VPN** aufgeführt.

    ![Azure-VPN](./media/openvpn-create-azure-ad-tenant/azurevpn.png)
    
8. Wenn Sie noch keine funktionsfähige Point-to-Site-Umgebung haben, folgen Sie der Anweisung zu deren Erstellung. Informationen zum Erstellen und Konfigurieren eines Point-to-Site-VPN-Gateways finden Sie unter [Erstellen eines Point-to-Site-VPN](vpn-gateway-howto-point-to-site-resource-manager-portal.md). 

    > [!IMPORTANT]
    > Die Basic-SKU wird für OpenVPN nicht unterstützt.

9. Aktivieren Sie Azure AD-Authentifizierung auf dem VPN-Gateway, indem Sie zu **Punkt-zu-Standort-Konfiguration** navigieren und **OpenVPN (SSL)** als **Tunneltyp** auswählen. Wählen Sie **Azure Active Directory** als **Authentifizierungstyp** aus, und geben Sie dann die entsprechenden Informationen im Abschnitt **Azure Active Directory** ein.

    ![Azure-VPN](./media/openvpn-create-azure-ad-tenant/azure-ad-auth-portal.png)


   > [!NOTE]
   > Achten Sie darauf, den nachgestellten Schrägstrich am Ende des `AadIssuerUri`-Werts anzugeben. Andernfalls kann es sein, dass die Verbindung fehlschlägt.

10. Erstellen Sie das Profil, indem Sie es durch Klicken auf den Link **VPN-Client herunterladen** herunterladen.

11. Extrahieren Sie die heruntergeladene ZIP-Datei.

12. Navigieren Sie zum entzippten Ordner „AzureVPN“.

13. Notieren Sie sich den Speicherort der Datei „azurevpnconfig.xml“. Die Datei „azurevpnconfig.xml“ enthält die Einstellung für die VPN-Verbindung und kann direkt in die Azure VPN Client-Anwendung importiert werden. Sie können diese Datei auch per e-Mail oder anderweitig an alle Benutzer verteilen, die eine Verbindung herstellen müssen. Der Benutzer benötigt gültige Azure AD-Anmeldeinformationen, um eine Verbindung herstellen zu können.

## <a name="next-steps"></a>Nächste Schritte

Um eine Verbindung mit Ihrem virtuellen Netzwerk herzustellen, müssen Sie ein VPN-Clientprofil erstellen und konfigurieren. Entsprechende Informationen finden Sie unter [Konfigurieren eines VPN-Clients für P2S-VPN-Verbindungen](openvpn-azure-ad-client.md).
