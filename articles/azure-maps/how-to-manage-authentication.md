---
title: Verwalten der Authentifizierung | Microsoft Azure Maps
description: Sie können über das Azure-Portal die Authentifizierung in Microsoft Azure Maps verwalten.
author: walsehgal
ms.author: v-musehg
ms.date: 01/16/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 1f7f128898089292a8ccd92686af5d68fe328f3c
ms.sourcegitcommit: 984c5b53851be35c7c3148dcd4dfd2a93cebe49f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2020
ms.locfileid: "76766056"
---
# <a name="manage-authentication-in-azure-maps"></a>Verwalten der Authentifizierung in Azure Maps

Nachdem Sie ein Azure Maps-Konto erstellt haben, werden eine Client-ID und Schlüssel erstellt, um die Authentifizierung über Azure Active Directory (Azure AD) und Shared Key zu unterstützen.

## <a name="view-authentication-details"></a>Anzeigen von Authentifizierungsdetails

Nach dem Erstellen des Azure Maps-Kontos werden der primäre und sekundäre Schlüssel generiert. Verwenden Sie den primären Schlüssel als Abonnementschlüssel (manchmal werden diese Namen austauschbar verwendet). Der sekundäre Schlüssel kann in Szenarien wie Änderungen beim Schlüsselrollover verwendet werden. In jedem Fall ist ein Schlüssel erforderlich, um Azure Maps aufzurufen. Dieser Prozess wird als [Authentifizierung mit gemeinsam verwendetem Schlüssel](https://docs.microsoft.com/azure/azure-maps/azure-maps-authentication#shared-key-authentication) bezeichnet. Weitere Informationen zur Authentifizierung mit gemeinsam verwendetem Schlüssel und zur Azure AD-Authentifizierung finden Sie unter [Authentifizierung bei Azure Maps](https://aka.ms/amauth).

Sie können Ihre Authentifizierungsdetails für das Azure-Portal anzeigen. Wechseln Sie zu Ihrem Konto, und klicken Sie im Menü **Einstellungen** auf **Authentifizierung**.

![Authentifizierungsdetails](./media/how-to-manage-authentication/how-to-view-auth.png)


## <a name="set-up-azure-ad-app-registration"></a>Einrichten der Azure AD-App-Registrierung

Nachdem Sie ein Azure Maps-Konto erstellt haben, müssen Sie eine Verknüpfung zwischen Ihrem Azure AD-Mandanten und der Azure Maps-Ressource erstellen.

1. Wählen Sie **Azure Active Directory** im Menü des Portals aus. Geben Sie einen Namen für die Registrierung ein. Klicken Sie auf **App-Registrierungen** und dann auf **Neue Registrierung**. Geben Sie im Feld **Umleitungs-URI** die Startseite der Web-App an. Beispiel: https://localhost/. Wenn Sie bereits über eine registrierte App verfügen, fahren Sie mit Schritt 2 fort.

    ![App-Registrierung](./media/how-to-manage-authentication/app-registration.png)

    ![App-Registrierungsdetails](./media/how-to-manage-authentication/app-create.png)

2. Navigieren Sie unter **App-Registrierungen** zur Anwendung, und wählen Sie **API-Berechtigungen** aus, um Azure Maps die delegierten API-Berechtigungen zuzuweisen. Wählen Sie **Berechtigung hinzufügen** aus. Suchen Sie **Azure Maps** unter **API auswählen**, und wählen Sie die Option aus.

    ![App-API-Berechtigungen](./media/how-to-manage-authentication/app-permissions.png)

3. Aktivieren Sie unter **Berechtigungen auswählen** das Kontrollkästchen für **Benutzeridentitätswechsel**, und klicken Sie dann unten auf die Schaltfläche **Auswählen**.

    ![Auswählen von App-API-Berechtigungen](./media/how-to-manage-authentication/select-app-permissions.png)

4. Führen Sie je nach Ihrer Authentifizierungsmethode Schritt a oder b aus.

    1. Wenn Ihre Anwendung die Benutzertokenauthentifizierung mit dem Azure Maps Web SDK nutzt, aktivieren Sie `oauth2AllowImplicitFlow`, indem Sie die Option im Abschnitt „Manifest“ Ihrer App-Registrierung auf „true“ festlegen.
    
       ![App-Manifest](./media/how-to-manage-authentication/app-manifest.png)

    2. Wenn Ihre Anwendung Server/Anwendung-Authentifizierung verwendet, wechseln Sie innerhalb der App-Registrierung zum Blatt **Zertifikate und Geheimnisse**, und erstellen Sie entweder ein Kennwort, oder laden Sie ein öffentliches Schlüsselzertifikat in die App-Registrierung hoch. Wenn Sie ein Kennwort erstellen, speichern Sie es zur späteren Verwendung sicher. Dieses Kennwort benötigen Sie zum Abrufen von Token von Azure AD.

       ![App-Schlüssel](./media/how-to-manage-authentication/app-keys.png)


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
