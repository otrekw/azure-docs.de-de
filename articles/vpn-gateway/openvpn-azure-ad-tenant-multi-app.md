---
title: 'VPN Gateway: Azure AD-Mandant für verschiedene Benutzergruppen: Azure AD-Authentifizierung'
description: Sie können P2S VPN verwenden, um eine Verbindung mit Azure AD-Authentifizierung zu Ihrem VNet herzustellen.
services: vpn-gateway
author: anzaman
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 12/20/2019
ms.author: alzam
ms.openlocfilehash: 73a47b20010be0b4eadec790dd8fb254f912ff2a
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75474120"
---
# <a name="create-an-azure-active-directory-tenant-for-p2s-openvpn-protocol-connections"></a>Erstellen eines Azure Active Directory-Mandanten für Verbindungen mit dem P2S OpenVPN-Protokoll

Beim Herstellen einer Verbindung mit Ihrem VNet können Sie die zertifikatbasierte Authentifizierung oder die RADIUS-Authentifizierung verwenden. Wenn Sie das Open VPN-Protokoll verwenden, können Sie jedoch auch die Azure Active Directory-Authentifizierung verwenden. Wenn Sie möchten, dass sich verschiedene Benutzergruppen mit verschiedenen VPN-Gateways verbinden können, können Sie mehrere Anwendungen im AD registrieren und sie mit verschiedenen VPN-Gateways verknüpfen. Dieser Artikel unterstützt Sie bei der Einrichtung eines Azure AD-Mandanten für die P2S-OpenVPN-Authentifizierung und bei der Erstellung und Registrierung mehrerer Anwendungen in Azure AD, um verschiedenen Benutzern und Gruppen unterschiedliche Zugriffsmöglichkeiten zu geben.

> [!NOTE]
> Die Azure AD-Authentifizierung wird nur für Verbindungen mit dem OpenVPN®-Protokoll unterstützt.
>

## <a name="tenant"></a>1. Erstellen des Azure AD-Mandanten

Erstellen Sie mit den im Artikel [Erstellen eines neuen Mandanten](../active-directory/fundamentals/active-directory-access-create-new-tenant.md) beschriebenen Schritten einen Azure AD-Mandanten:

* Organisationsname
* Name der Anfangsdomäne

Beispiel:

   ![Neuer Azure AD-Mandant](./media/openvpn-azure-ad-tenant-multi-app/newtenant.png)

## <a name="users"></a>2. Erstellen von Azure AD-Mandantenbenutzern

Erstellen Sie als Nächstes zwei Benutzerkonten. Erstellen Sie ein globales Administratorkonto und ein Masterbenutzerkonto. Das Masterbenutzerkonto wird als Masterkonto für die Einbettung verwendet (Dienstkonto). Wenn Sie ein Azure AD-Mandantenbenutzerkonto erstellen, passen Sie die Verzeichnisrolle für den Typ des Benutzers an, den Sie erstellen möchten.

Verwenden Sie die Schritte in [diesem Artikel](../active-directory/fundamentals/add-users-azure-active-directory.md), um mindestens zwei Benutzer für Ihren Azure AD-Mandanten zu erstellen. Achten Sie darauf, dass Sie die **Verzeichnisrolle** ändern, um die Kontotypen zu erstellen:

* Globaler Administrator
* Benutzer

## <a name="enable-authentication"></a>3. Registrieren von Azure VPN Client in Azure AD-Mandanten

1. Suchen Sie die Verzeichnis-ID des Verzeichnisses, das Sie für die Authentifizierung verwenden möchten. Sie wird im Abschnitt „Eigenschaften“ der Active Directory-Seite aufgeführt.

    ![Verzeichnis-ID](./media/openvpn-azure-ad-tenant-multi-app/directory-id.png)

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

    ![Verzeichnis-ID](./media/openvpn-azure-ad-tenant-multi-app/pick.png)

6. Wählen Sie **Akzeptieren** aus, wenn Sie dazu aufgefordert werden.

    ![Akzeptieren](./media/openvpn-azure-ad-tenant-multi-app/accept.jpg)

7. Unter ihrem Azure AD wird in **Unternehmensanwendungen** **Azure-VPN** aufgeführt.

    ![Azure-VPN](./media/openvpn-azure-ad-tenant-multi-app/azurevpn.png)

## <a name="enable-authentication"></a>4. Registrieren zusätzlicher Anwendungen für verschiedene Benutzer/Gruppen

1. Klicken Sie unter Ihrem Azure Active Directory auf die die Option **App-Registrierungen** und dann auf **+ Neue Registrierung**.

    ![Azure-VPN](./media/openvpn-azure-ad-tenant-multi-app/app1.png)

2. Geben Sie auf dem Blatt **Registrieren einer Anwendung** den **Namen** ein, wählen Sie den gewünschten **unterstützten Kontotypen** aus, und klicken Sie auf **Registrieren.**

    ![Azure-VPN](./media/openvpn-azure-ad-tenant-multi-app/app2.png)

3. Nachdem die neue Anwendung registriert wurde, klicken Sie unter dem Blatt „Anwendung“ auf **Eine API verfügbar machen**.

4. Klicken Sie auf **+ Bereich hinzufügen**.
5. Behalten Sie den Standard **Anwendungs-ID-URI** bei, und klicken Sie auf **Speichern und fortfahren**.

    ![Azure-VPN](./media/openvpn-azure-ad-tenant-multi-app/app3.png)
6. Füllen Sie die Pflichtfelder aus, und stellen Sie sicher, dass **Zustand** auf **Aktiviert** gesetzt ist. Klicken Sie auf **Bereich hinzufügen**.

    ![Azure-VPN](./media/openvpn-azure-ad-tenant-multi-app/app4.png)
7. Klicken Sie auf **Eine API verfügbar machen** und dann auf **+ Eine Clientanwendung hinzufügen**.  Geben Sie für **Client-ID-** abhängig von der Cloud die folgenden Werte ein:
    -   Geben Sie **41b23e61-6c1e-4545-b367-cd054e0ed4b4** für Azure **Public** ein.
    -   Geben Sie **51bb15d4-3a4f-4ebf-9dca-40096fe32426** für Azure **Government** ein.
    -   Geben Sie **538ee9e6-310a-468d-afef-ea97365856a9** für Azure **Germany** ein.
    -   Geben Sie **49f817b6-84ae-4cc0-928c-73f27289b3aa** für Azure **China 21Vianet** ein.


8. Klicken Sie auf **Anwendung hinzufügen**.

    ![Azure-VPN](./media/openvpn-azure-ad-tenant-multi-app/app5.png)
9. Kopieren Sie auf der Seite **Übersicht** die **Anwendungs-ID (Client)** . Sie benötigen sie zum Konfigurieren Ihres/Ihrer VPN-Gateway(s).

    ![Azure-VPN](./media/openvpn-azure-ad-tenant-multi-app/app6.png)

10. Wiederholen Sie die Schritte in diesem Abschnitt (4), um so viele Anwendungen zu erstellen, wie für Ihre Sicherheitsanforderung erforderlich sind. Jede Anwendung wird einem VPN-Gateway zugeordnet und kann über eine jeweils andere Benutzergruppe verfügen. Einem Gateway kann nur eine Anwendung zugeordnet werden.

## <a name="enable-authentication"></a>5. Zuweisen von Benutzern zu Ihrer Anwendungen

1. Wählen Sie unter Azure AD **Unternehmensanwendungen** und dann die neu registrierte Anwendung aus, und klicken Sie auf **Eigenschaften**. Stellen Sie sicher, dass **Benutzerzuweisung erforderlich?** auf **Ja** festgelegt ist. Klicken Sie unten auf der Seite auf **Speichern**.

    ![Azure-VPN](./media/openvpn-azure-ad-tenant-multi-app/user2.png)

2. Klicken Sie auf der Anwendungsseite auf **Benutzer und Gruppen** und dann auf **Benutzer hinzufügen**.

    ![Azure-VPN](./media/openvpn-azure-ad-tenant-multi-app/user3.png)
3. Klicken Sie unter **Zuweisung hinzufügen** auf die Option **Benutzer und Gruppen**. Wählen Sie die Benutzer aus, die auf diese VPN-Anwendung zugreifen können sollen. Klicken Sie auf **Auswählen**.

    ![Azure-VPN](./media/openvpn-azure-ad-tenant-multi-app/user4.png)

## <a name="enable-authentication"></a>6. Aktivieren der Azure AD-Authentifizierung im Aktivieren der Azure AD-Authentifizierung im VPN-Gateway

1. Aktivieren Sie die Azure AD-Authentifizierung auf dem VPN-Gateway, indem Sie die folgenden Befehle ausführen (Sie müssen den Befehl der jeweiligen Umgebung entsprechend ändern):

    ```azurepowershell-interactive
    $gw = Get-AzVirtualNetworkGateway -Name <name of VPN gateway> -ResourceGroupName <Resource group>
    Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -VpnClientRootCertificates @()
    Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -AadTenantUri "https://login.microsoftonline.com/<your Directory ID>" -AadAudienceId "application ID from previous section" -AadIssuerUri "https://sts.windows.net/<your Directory ID>/" -VpnClientAddressPool 192.168.0.0/24
    ```
> [!NOTE]
> Verwenden Sie in den oben aufgeführten Befehlen nicht die Anwendungs-ID des Azure-VPN-Clients, da dadurch allen Benutzern Zugriff auf das VPN-Gateway gewährt wird. Verwenden Sie die ID der Anwendung(en), die Sie registriert haben.

2. Erstellen Sie das Profil und laden Sie es herunter, indem Sie die folgenden Befehle ausführen. Ändern Sie die Werte von -ResourcGroupName und -Name den verwendeten Namen entsprechend.

    ```azurepowershell-interactive
    $profile = New-AzVpnClientConfiguration -Name <name of VPN gateway> -ResourceGroupName <Resource group> -AuthenticationMethod "EapTls"
    $PROFILE.VpnProfileSASUrl
    ```

3. Nach der Ausführung der Befehle wird ein ähnliches Ergebnis wie das folgende angezeigt. Kopieren Sie die Ergebnis-URL in Ihren Browser, um die ZIP-Datei für das Profil herunterzuladen.

    ![Azure-VPN](./media/openvpn-azure-ad-tenant-multi-app/profile.png)

4. Extrahieren Sie die heruntergeladene ZIP-Datei.

5. Navigieren Sie zum entzippten Ordner „AzureVPN“.

6. Notieren Sie sich den Speicherort der Datei „azurevpnconfig.xml“. Die Datei „azurevpnconfig.xml“ enthält die Einstellung für die VPN-Verbindung und kann direkt in die Azure VPN Client-Anwendung importiert werden. Sie können diese Datei auch per e-Mail oder anderweitig an alle Benutzer verteilen, die eine Verbindung herstellen müssen. Der Benutzer benötigt gültige Azure AD-Anmeldeinformationen, um eine Verbindung herstellen zu können.

## <a name="next-steps"></a>Nächste Schritte

Um eine Verbindung mit Ihrem virtuellen Netzwerk herzustellen, müssen Sie ein VPN-Clientprofil erstellen und konfigurieren. Entsprechende Informationen finden Sie unter [Konfigurieren eines VPN-Clients für P2S-VPN-Verbindungen](openvpn-azure-ad-client.md).
