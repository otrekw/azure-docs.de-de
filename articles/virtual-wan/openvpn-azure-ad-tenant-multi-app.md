---
title: 'VPN Gateway: Azure AD-Mandant für verschiedene Benutzergruppen: Azure AD-Authentifizierung'
description: Sie können P2S VPN verwenden, um eine Verbindung mit Azure AD-Authentifizierung zu Ihrem VNet herzustellen.
services: virtual-wan
author: anzaman
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 1/10/2020
ms.author: alzam
ms.openlocfilehash: b390b5f8b00f61994db820a3af7bce26a3e0a30d
ms.sourcegitcommit: 014e916305e0225512f040543366711e466a9495
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/14/2020
ms.locfileid: "75934887"
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

## <a name="enable-authentication"></a>3. Registrieren von Azure VPN Client in Azure AD-Mandant

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

1. Klicken Sie unter Ihrem Azure Active Directory auf die Option **App-Registrierungen** und dann auf **+ Neue Registrierung**.

    ![Azure-VPN](./media/openvpn-azure-ad-tenant-multi-app/app1.png)

2. Geben Sie auf dem Blatt **Registrieren einer Anwendung** den **Namen** ein, wählen Sie den gewünschten **unterstützten Kontotypen** aus, und klicken Sie auf **Registrieren**.

    ![Azure-VPN](./media/openvpn-azure-ad-tenant-multi-app/app2.png)

3. Nachdem die neue Anwendung registriert wurde, klicken Sie unter dem Blatt „Anwendung“ auf **Eine API verfügbar machen**.

4. Klicken Sie auf **+ Bereich hinzufügen**.
5. Behalten Sie den Standard **Anwendungs-ID-URI** bei, und klicken Sie auf **Speichern und fortfahren**.

    ![Azure-VPN](./media/openvpn-azure-ad-tenant-multi-app/app3.png)
6. Füllen Sie die Pflichtfelder aus, und stellen Sie sicher, dass **Zustand** auf **Aktiviert** gesetzt ist. Klicken Sie auf **Bereich hinzufügen**.

    ![Azure-VPN](./media/openvpn-azure-ad-tenant-multi-app/app4.png)
7. Klicken Sie auf **Eine API verfügbar machen** und dann auf **+ Eine Clientanwendung hinzufügen**.  Geben Sie für **Client-ID** abhängig von der Cloud die folgenden Werte ein:
    -   Geben Sie **41b23e61-6c1e-4545-b367-cd054e0ed4b4** für Azure **Public** ein.
    -   Geben Sie **51bb15d4-3a4f-4ebf-9dca-40096fe32426** für Azure **Government** ein.
    -   Geben Sie **538ee9e6-310a-468d-afef-ea97365856a9** für Azure **Germany** ein.
    -   Geben Sie **49f817b6-84ae-4cc0-928c-73f27289b3aa** für Azure **China 21Vianet** ein.


8. Klicken Sie auf **Anwendung hinzufügen**.

    ![Azure-VPN](./media/openvpn-azure-ad-tenant-multi-app/app5.png)
9. Kopieren Sie auf der Seite **Übersicht** die **Anwendungs-ID (Client)** . Sie benötigen sie zum Konfigurieren Ihres/Ihrer VPN-Gateway(s).

    ![Azure-VPN](./media/openvpn-azure-ad-tenant-multi-app/app6.png)

10. Wiederholen Sie die Schritte in diesem Abschnitt (4), um so viele Anwendungen zu erstellen, wie für Ihre Sicherheitsanforderung erforderlich sind. Jede Anwendung wird einem VPN-Gateway zugeordnet und kann über eine jeweils andere Benutzergruppe verfügen. Einem Gateway kann nur eine Anwendung zugeordnet werden.

## <a name="enable-authentication"></a>5. Zuweisen von Benutzern zu Ihren Anwendungen

1. Wählen Sie unter Azure AD **Unternehmensanwendungen** und dann die neu registrierte Anwendung aus, und klicken Sie auf **Eigenschaften**. Stellen Sie sicher, dass **Benutzerzuweisung erforderlich?** auf **Ja** festgelegt ist. Klicken Sie unten auf der Seite auf **Speichern**.

    ![Azure-VPN](./media/openvpn-azure-ad-tenant-multi-app/user2.png)

2. Klicken Sie auf der Anwendungsseite auf **Benutzer und Gruppen** und dann auf **Benutzer hinzufügen**.

    ![Azure-VPN](./media/openvpn-azure-ad-tenant-multi-app/user3.png)
3. Klicken Sie unter **Zuweisung hinzufügen** auf die Option **Benutzer und Gruppen**. Wählen Sie die Benutzer aus, die auf diese VPN-Anwendung zugreifen können sollen. Klicken Sie auf **Auswählen**.

    ![Azure-VPN](./media/openvpn-azure-ad-tenant-multi-app/user4.png)

## <a name="site"></a>6. Erstellen einer neuen P2S-Konfiguration

Eine P2S-Konfiguration definiert die Parameter für das Herstellen der Verbindung mit Remoteclients.

1. Legen Sie die folgenden Variablen fest, und ersetzen Sie dabei die Werte nach Bedarf für Ihre Umgebung.

   ```powershell
   $aadAudience = "00000000-abcd-abcd-abcd-999999999999"
   $aadIssuer = "https://sts.windows.net/00000000-abcd-abcd-abcd-999999999999/"
   $aadTenant = "https://login.microsoftonline.com/00000000-abcd-abcd-abcd-999999999999"    
   ```

2. Führen Sie die folgenden Befehle aus, um die Konfiguration zu erstellen:

   ```powershell
   $aadConfig = New-AzVpnServerConfiguration -ResourceGroupName <ResourceGroup> -Name newAADConfig -VpnProtocol OpenVPN -VpnAuthenticationType AAD -AadTenant $aadTenant -AadIssuer $aadIssuer -AadAudience $aadAudience -Location westcentralus
> [!NOTE]
> Do not use the Azure VPN client's application ID in the commands above as it will grant all users access to the VPN gateway. Use the ID of the application(s) you registered.

## <a name="hub"></a>7. Edit hub assignment

1. Navigate to the **Hubs** blade under the virtual WAN.
2. Select the hub that you want to associate the vpn server configuration to and click the ellipsis (...).

   ![new site](media/virtual-wan-point-to-site-azure-ad/p2s4.jpg)
3. Click **Edit virtual hub**.
4. Check the **Include point-to-site gateway** check box and pick the **Gateway scale unit** that you want.

   ![new site](media/virtual-wan-point-to-site-azure-ad/p2s2.jpg)
5. Enter the **Address pool** from which the VPN clients will be assigned IP addresses.
6. Click **Confirm**.
7. The operation will can take up to 30 minutes to complete.

## <a name="device"></a>8. Download VPN profile

Use the VPN profile to configure your clients.

1. On the page for your virtual WAN, click **User VPN configurations**.
2. At the top of the  page, click **Download user VPN config**.
3. Once the file has finished creating, you can click the link to download it.
4. Use the profile file to configure the VPN clients.

4. Extract the downloaded zip file.

5. Browse to the unzipped “AzureVPN” folder.

6. Make a note of the location of the “azurevpnconfig.xml” file. The azurevpnconfig.xml contains the setting for the VPN connection and can be imported directly into the Azure VPN Client application. You can also distribute this file to all the users that need to connect via e-mail or other means. The user will need valid Azure AD credentials to connect successfully.
## Configure user VPN clients

To connect, you need to download the Azure VPN Client (Preview) and import the VPN client profile that was downloaded in the previous steps on every computer that wants to connect to the VNet.

> [!NOTE]
> Azure AD authentication is supported only for OpenVPN® protocol connections.
>

#### To download the Azure VPN client

Use this [link](https://www.microsoft.com/p/azure-vpn-client-preview/9np355qt2sqb?rtc=1&activetab=pivot:overviewtab) to download the Azure VPN Client (Preview).

#### <a name="import"></a>To import a client profile

1. On the page, select **Import**.

    ![import](./media/virtual-wan-point-to-site-azure-ad/import/import1.jpg)

2. Browse to the profile xml file and select it. With the file selected, select **Open**.

    ![import](./media/virtual-wan-point-to-site-azure-ad/import/import2.jpg)

3. Specify the name of the profile and select **Save**.

    ![import](./media/virtual-wan-point-to-site-azure-ad/import/import3.jpg)

4. Select **Connect** to connect to the VPN.

    ![import](./media/virtual-wan-point-to-site-azure-ad/import/import4.jpg)

5. Once connected, the icon will turn green and say **Connected**.

    ![import](./media/virtual-wan-point-to-site-azure-ad/import/import5.jpg)

#### <a name="delete"></a>To delete a client profile

1. Select the ellipsis (...) next to the client profile that you want to delete. Then, select **Remove**.

    ![delete](./media/virtual-wan-point-to-site-azure-ad/delete/delete1.jpg)

2. Select **Remove** to delete.

    ![delete](./media/virtual-wan-point-to-site-azure-ad/delete/delete2.jpg)

#### <a name="diagnose"></a>Diagnose connection issues

1. To diagnose connection issues, you can use the **Diagnose** tool. Select the ellipsis (...) next to the VPN connection that you want to diagnose to reveal the menu. Then select **Diagnose**.

    ![diagnose](./media/virtual-wan-point-to-site-azure-ad/diagnose/diagnose1.jpg)

2. On the **Connection Properties** page, select **Run Diagnosis**.

    ![diagnose](./media/virtual-wan-point-to-site-azure-ad/diagnose/diagnose2.jpg)

3. Sign in with your credentials.

    ![diagnose](./media/virtual-wan-point-to-site-azure-ad/diagnose/diagnose3.jpg)

4. View the diagnosis results.

    ![diagnose](./media/virtual-wan-point-to-site-azure-ad/diagnose/diagnose4.jpg)

## <a name="viewwan"></a>View your virtual WAN

1. Navigate to the virtual WAN.
2. On the Overview page, each point on the map represents a hub. Hover over any point to view the hub health summary.
3. In the Hubs and connections section, you can view hub status, site, region, VPN connection status, and bytes in and out.

## <a name="viewhealth"></a>View your resource health

1. Navigate to your WAN.
2. On your WAN page, in the **SUPPORT + Troubleshooting** section, click **Health** and view your resource.


## <a name="cleanup"></a>Clean up resources

When you no longer need these resources, you can use [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) to remove the resource group and all of the resources it contains. Replace "myResourceGroup" with the name of your resource group and run the following PowerShell command:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Virtual WAN finden Sie auf der Seite mit der [Übersicht über Virtual WAN](virtual-wan-about.md).