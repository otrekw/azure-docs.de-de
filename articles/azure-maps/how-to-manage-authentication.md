---
title: Verwalten der Authentifizierung | Microsoft Azure Maps
description: Sie können über das Azure-Portal die Authentifizierung in Microsoft Azure Maps verwalten.
author: walsehgal
ms.author: v-musehg
ms.date: 01/29/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: f856aebe5e3acaca142e460d18ec8c6498b18787
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/04/2020
ms.locfileid: "76989219"
---
# <a name="manage-authentication-in-azure-maps"></a>Verwalten der Authentifizierung in Azure Maps

Nachdem Sie ein Azure Maps-Konto erstellt haben, werden eine Client-ID und Schlüssel erstellt, um die Authentifizierung über Azure Active Directory (Azure AD) und Shared Key zu unterstützen.

## <a name="view-authentication-details"></a>Anzeigen von Authentifizierungsdetails

Nach dem Erstellen des Azure Maps-Kontos werden der primäre und sekundäre Schlüssel generiert. Es wird empfohlen, den Primärschlüssel als Abonnementschlüssel zu verwenden, wenn Sie Azure Maps über die [Authentifizierung mit gemeinsam verwendetem Schlüssel](https://docs.microsoft.com/azure/azure-maps/azure-maps-authentication#shared-key-authentication) aufrufen. Der sekundäre Schlüssel kann in Szenarien wie Änderungen beim Schlüsselrollover verwendet werden. Weitere Informationen finden Sie unter [Authentifizierung mit Azure Maps](https://aka.ms/amauth).

Sie können Ihre Authentifizierungsdetails für das Azure-Portal anzeigen. Wechseln Sie zu Ihrem Konto, und klicken Sie im Menü **Einstellungen** auf **Authentifizierung**.

![Authentifizierungsdetails](./media/how-to-manage-authentication/how-to-view-auth.png)


## <a name="configure-azure-ad-app-registration"></a>Konfigurieren der Azure AD-App-Registrierung

1. Wählen Sie **Azure Active Directory** in der Liste der Azure-Dienste im Azure-Portal aus.  Wählen Sie **App-Registrierungen** aus, und klicken Sie dann auf **Neue Registrierung**.  Stellen Sie als Nächstes Geben Sie einen **Namen** ein, wählen Sie **Supportkontotyp** aus, und klicken Sie auf **Registrieren**.  Wenn Sie bereits über eine registrierte App verfügen, fahren Sie mit Schritt 2 fort. 

    ![App-Registrierung](./media/how-to-manage-authentication/app-registration.png)

    ![App-Registrierungsdetails](./media/how-to-manage-authentication/app-create.png)

2. Navigieren Sie unter **App-Registrierungen** zu der Anwendung, um Azure Maps die delegierten API-Berechtigungen zuzuweisen. Wählen Sie anschließend **API-Berechtigungen** und dann **Berechtigung hinzufügen** aus. Suchen Sie **Azure Maps** unter **Von meiner Organisation verwendete APIs**, und wählen Sie die Option aus.

    ![App-API-Berechtigungen](./media/how-to-manage-authentication/app-permissions.png)

3. Aktivieren Sie **Auf Azure Maps zugreifen**, und klicken Sie dann auf **Berechtigungen hinzufügen**.

    ![Auswählen von App-API-Berechtigungen](./media/how-to-manage-authentication/select-app-permissions.png)

4. Führen Sie je nach Ihrer Authentifizierungsmethode Schritt a oder b aus. 

    1. Wenn Ihre Anwendung die Benutzertokenauthentifizierung mit dem Azure Maps Web SDK verwendet, aktivieren Sie `oauth2AllowImplicitFlow`. Um `oauth2AllowImplicitFlow` zu aktivieren, legen Sie es im Abschnitt „manifest“ Ihrer App-Registrierung auf TRUE fest. 
    
       ![App-Manifest](./media/how-to-manage-authentication/app-manifest.png)

    2. Wenn Ihre Anwendung die Server/Anwendung-Authentifizierung verwendet, wechseln Sie auf der Seite „App-Registrierung“ zum Blatt **Zertifikate und Geheimnisse**, und erstellen Sie entweder ein Kennwort, indem Sie auf **Neuer geheimer Clientschlüssel** klicken, oder laden Sie ein öffentliches Schlüsselzertifikat in die App-Registrierung hoch. Wenn Sie ein Kennwort erstellen, kopieren Sie nach dem Klicken auf **Speichern** das Kennwort zur späteren Verwendung, und bewahren Sie es sicher auf. Dieses Kennwort benötigen Sie zum Abrufen von Token von Azure AD.

       ![App-Schlüssel](./media/how-to-manage-authentication/app-keys.png)

       ![Hinzufügen eines Schlüssels](./media/how-to-manage-authentication/add-key.png)


## <a name="grant-role-based-access-control-rbac-to-azure-maps"></a>Gewähren der rollenbasierten Zugriffssteuerung (RBAC) in Azure Maps

Nachdem Sie Ihrem Azure AD-Mandanten ein Azure Maps-Konto zugeordnet haben, können Sie die Zugriffssteuerung gewähren. Sie gewähren Zugriffssteuerung, indem Sie einen Benutzer, eine Gruppe oder eine Anwendung zu einer oder mehreren Azure Maps-Zugriffssteuerungsrollen zuweisen.

1. Navigieren Sie zu Ihrem **Azure Maps-Konto**. Wählen Sie **Zugriffssteuerung (IAM)** und dann **Rollenzuweisung** aus.

    ![Gewähren von RBAC](./media/how-to-manage-authentication/how-to-grant-rbac.png)

2. Wählen Sie im Fenster **Rollenzuweisung** unter **Rolle** die Rolle **Azure Maps Date Reader (Preview)** aus. Wählen Sie unter **Zugriff zuweisen zu** die Option **Azure AD-Benutzer, -Gruppe oder -Dienstprinzipal** aus. Wählen Sie den Benutzer oder die Anwendung aus. Wählen Sie **Speichern** aus.

    ![Rollenzuweisung hinzufügen](./media/how-to-manage-authentication/add-role-assignment.png)

## <a name="view-available-azure-maps-rbac-roles"></a>Anzeigen verfügbarer Azure Maps-RBAC-Rollen

Öffnen Sie die **Zugriffssteuerung (IAM)** , klicken Sie auf **Rollen**, und suchen Sie dann nach Rollen, die mit **Azure Maps** beginnen, um Rollen für die rollenbasierte Zugriffssteuerung für Azure Maps anzuzeigen. Hierbei handelt es sich um die Rollen, denen Sie den Zugriff gewähren können.

![Anzeigen verfügbarer Rollen](./media/how-to-manage-authentication/how-to-view-avail-roles.png)


## <a name="view-azure-maps-rbac"></a>Anzeigen der rollenbasierten Zugriffssteuerung von Azure Maps

Mit der rollenbasierten Zugriffssteuerung kann die Zugriffssteuerung genau abgestimmt werden.

Öffnen Sie die **Zugriffssteuerung (IAM)** , klicken Sie auf **Rollenzuweisungen**, und filtern Sie dann nach **Azure Maps**, um Benutzer und Apps anzuzeigen, denen die rollenbasierte Zugriffssteuerung für Azure Maps gewährt wurde.

![Benutzer und Apps mit gewährter rollenbasierter Zugriffssteuerung anzeigen](./media/how-to-manage-authentication/how-to-view-amrbac.png)


## <a name="request-tokens-for-azure-maps"></a>Anfordern von Token für Azure Maps

Nachdem Sie Ihre App registriert und mit Azure Maps verknüpft haben, können Sie Zugriffstoken anfordern.

* Wenn Ihre Anwendung die Benutzertokenauthentifizierung mit dem Azure Maps Web SDK nutzt, müssen Sie Ihre HTML-Seite mit der Azure Maps-Client-ID und der Azure AD-App-ID konfigurieren.

* Wenn Ihre Anwendung die Server-/Anwendungsauthentifizierung nutzt, müssen Sie ein Token vom Azure AD-Tokenendpunkt `https://login.microsoftonline.com` mit der Azure AD-Ressourcen-ID `https://atlas.microsoft.com/`, der Azure Maps-Client-ID, der Azure AD-App-ID und dem Azure AD-App-Registrierungskennwort bzw. -zertifikat anfordern.

| Azure-Umgebung   | Azure AD-Tokenendpunkt | Azure-Ressourcen-ID |
| --------------------|-------------------------|-------------------|
| Azure – Öffentlich        | https://login.microsoftonline.com | https://atlas.microsoft.com/ |
| Azure Government    | https://login.microsoftonline.us  | https://atlas.microsoft.com/ | 

Weitere Informationen zum Anfordern von Zugriffstoken aus Azure AD für Benutzer und Dienstprinzipale finden Sie unter [Was ist Authentifizierung?](https://docs.microsoft.com/azure/active-directory/develop/authentication-scenarios).


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure AD-Authentifizierung und Azure Maps-Web-SDK finden Sie unter [Verwenden des Azure Maps-Kartensteuerelements](https://docs.microsoft.com/azure/azure-maps/how-to-use-map-control).

Erfahren Sie, wie Sie die API-Nutzungsmetriken für Ihr Azure Maps-Konto anzeigen:
> [!div class="nextstepaction"] 
> [Anzeigen von Nutzungsmetriken](how-to-view-api-usage.md)

Eine Liste mit Beispielen zur Integration von Azure Active Directory (AAD) in Azure Maps finden Sie hier:

> [!div class="nextstepaction"]
> [Beispiele für die Azure AD-Authentifizierung](https://github.com/Azure-Samples/Azure-Maps-AzureAD-Samples)
