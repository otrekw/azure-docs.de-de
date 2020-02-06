---
title: 'VPN Gateway: Azure AD-Mandant für verschiedene Benutzergruppen: Azure AD-Authentifizierung'
description: Sie können P2S VPN verwenden, um eine Verbindung mit Azure AD-Authentifizierung zu Ihrem VNet herzustellen.
services: virtual-wan
author: anzaman
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 1/10/2020
ms.author: alzam
ms.openlocfilehash: 90244b9dcf30c2ef01d4e57c9d8e35fa1d71f434
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/04/2020
ms.locfileid: "76985644"
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
   ```

> [!NOTE]
> Verwenden Sie in den oben aufgeführten Befehlen nicht die Anwendungs-ID des Azure-VPN-Clients, da dadurch allen Benutzern Zugriff auf das VPN-Gateway gewährt wird. Verwenden Sie die ID der Anwendung(en), die Sie registriert haben.

## <a name="hub"></a>7. Bearbeiten der Hub-Zuweisung

1. Navigieren Sie unter dem virtuellen WAN zum Blatt **Hubs**.
2. Wählen Sie den Hub aus, dem Sie die VPN-Serverkonfiguration zuordnen möchten, und klicken Sie auf die Auslassungspunkte (...).

   ![Neue Site](media/virtual-wan-point-to-site-azure-ad/p2s4.jpg)
3. Klicken Sie auf **Virtuellen Hub bearbeiten**.
4. Aktivieren Sie das Kontrollkästchen **Point-to-Site-Gateway einschließen**, und wählen Sie die gewünschte **Gatewayskalierungseinheit** aus.

   ![Neue Site](media/virtual-wan-point-to-site-azure-ad/p2s2.jpg)
5. Geben Sie den **Adresspool** ein, aus dem den VPN-Clients IP-Adressen zugewiesen werden.
6. Klicken Sie auf **Confirm** (Bestätigen).
7. Der Vorgang kann bis zu 30 Minuten dauern.

## <a name="device"></a>8. Herunterladen des VPN-Profils

Verwenden Sie das VPN-Profil, um Ihre Clients zu konfigurieren.

1. Klicken Sie auf der Seite für Ihr virtuelles WAN auf **Benutzer-VPN-Konfigurationen**.
2. Klicken Sie oben auf der Seite auf **Benutzer-VPN-Konfiguration herunterladen**.
3. Nachdem die Erstellung der Datei abgeschlossen wurde, können Sie auf den Link klicken, um sie herunterzuladen.
4. Verwenden Sie die Profildatei, um die VPN-Clients zu konfigurieren.

4. Extrahieren Sie die heruntergeladene ZIP-Datei.

5. Navigieren Sie zum entzippten Ordner „AzureVPN“.

6. Notieren Sie sich den Speicherort der Datei „azurevpnconfig.xml“. Die Datei „azurevpnconfig.xml“ enthält die Einstellung für die VPN-Verbindung und kann direkt in die Azure VPN Client-Anwendung importiert werden. Sie können diese Datei auch per e-Mail oder anderweitig an alle Benutzer verteilen, die eine Verbindung herstellen müssen. Der Benutzer benötigt gültige Azure AD-Anmeldeinformationen, um eine Verbindung herstellen zu können.
## <a name="configure-user-vpn-clients"></a>Konfigurieren von Benutzer-VPN-Clients

Für die Verbindungsherstellung muss auf jedem Computer, von dem eine Verbindung mit dem VNET hergestellt werden soll, Azure VPN Client (Vorschauversion) heruntergeladen und das in den vorherigen Schritten heruntergeladene VPN-Clientprofil importiert werden.

> [!NOTE]
> Die Azure AD-Authentifizierung wird nur für Verbindungen mit dem OpenVPN®-Protokoll unterstützt.
>

#### <a name="to-download-the-azure-vpn-client"></a>So laden Sie Azure VPN Client herunter

Verwenden Sie [diesen Link](https://www.microsoft.com/p/azure-vpn-client-preview/9np355qt2sqb?rtc=1&activetab=pivot:overviewtab), um Azure VPN Client (Vorschauversion) herunterzuladen.

#### <a name="import"></a>So importieren Sie ein Clientprofil

1. Wählen Sie auf der Seite **Importieren**aus.

    ![Import](./media/virtual-wan-point-to-site-azure-ad/import/import1.jpg)

2. Navigieren Sie zur XML-Datei für das Profil und wählen Sie sie aus. Wählen Sie bei ausgewählter Datei **Öffnen** aus.

    ![Import](./media/virtual-wan-point-to-site-azure-ad/import/import2.jpg)

3. Geben Sie den Namen des Profils an und wählen Sie **Speichern** aus.

    ![Import](./media/virtual-wan-point-to-site-azure-ad/import/import3.jpg)

4. Wählen Sie **Verbinden** aus, um die Verbindung zum VPN herzustellen.

    ![Import](./media/virtual-wan-point-to-site-azure-ad/import/import4.jpg)

5. Nachdem die Verbindung hergestellt wurde, wird das Symbol grün und **Verbunden** angezeigt.

    ![Import](./media/virtual-wan-point-to-site-azure-ad/import/import5.jpg)

#### <a name="delete"></a>So löschen Sie ein Clientprofil

1. Wählen Sie neben dem Clientprofil, das Sie löschen möchten, die Auslassungspunkte (...) aus. Wählen Sie dann **Entfernen** aus.

    ![delete](./media/virtual-wan-point-to-site-azure-ad/delete/delete1.jpg)

2. Wählen Sie **Entfernen** aus, um den Löschvorgang durchzuführen.

    ![delete](./media/virtual-wan-point-to-site-azure-ad/delete/delete2.jpg)

#### <a name="diagnose"></a>Diagnose von Verbindungsproblemen

1. Zum Diagnostizieren von Verbindungsproblemen können Sie das **Diagnose**-Tool verwenden. Wählen Sie neben der VPN-Verbindung, die Sie diagnostizieren möchten, die Auslassungspunkte (...) aus, um das Menü einzublenden. Wählen Sie dann **Diagnose** aus.

    ![Diagnose](./media/virtual-wan-point-to-site-azure-ad/diagnose/diagnose1.jpg)

2. Wählen Sie auf der Seite **Verbindungseigenschaften** die Option **Diagnose ausführen** aus.

    ![Diagnose](./media/virtual-wan-point-to-site-azure-ad/diagnose/diagnose2.jpg)

3. Melden Sie sich mit Ihren Anmeldeinformationen an.

    ![Diagnose](./media/virtual-wan-point-to-site-azure-ad/diagnose/diagnose3.jpg)

4. Zeigen Sie die Diagnoseergebnisse an.

    ![Diagnose](./media/virtual-wan-point-to-site-azure-ad/diagnose/diagnose4.jpg)

## <a name="viewwan"></a>Anzeigen Ihrer Virtual WAN-Instanz

1. Navigieren Sie zum virtuellen WAN.
2. Auf der Seite „Übersicht“ steht jeder Punkt auf der Karte für einen Hub. Bewegen Sie den Mauszeiger jeweils auf einen dieser Punkte, um die Zusammenfassung zur Integrität des Hubs anzuzeigen.
3. Im Abschnitt mit den Hubs und Verbindungen können Sie den Hubstatus, die Site, die Region, den VPN-Verbindungsstatus und die Anzahl von ein- und ausgehenden Byte anzeigen.

## <a name="viewhealth"></a>Anzeigen der Ressourcenintegrität

1. Navigieren Sie zu Ihrem WAN.
2. Klicken Sie auf der Seite für Ihr WAN im Abschnitt **Support + Problembehandlung** auf **Integrität**, und zeigen Sie Ihre Ressource an.


## <a name="cleanup"></a>Bereinigen von Ressourcen

Wenn Sie diese Ressourcen nicht mehr benötigen, können Sie den Befehl [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) verwenden, um die Ressourcengruppe und alle darin enthaltenen Ressourcen zu entfernen. Ersetzen Sie „myResourceGroup“ durch den Namen Ihrer Ressourcengruppe, und führen Sie den folgenden PowerShell-Befehl aus:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Virtual WAN finden Sie auf der Seite mit der [Übersicht über Virtual WAN](virtual-wan-about.md).
