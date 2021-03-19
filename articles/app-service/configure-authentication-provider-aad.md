---
title: Konfigurieren von Azure AD-Authentifizierung
description: Erfahren Sie, wie Sie die Azure Active Directory-Authentifizierung als Identitätsanbieter für Ihre App Services- oder Azure Functions-App konfigurieren.
ms.assetid: 6ec6a46c-bce4-47aa-b8a3-e133baef22eb
ms.topic: article
ms.date: 04/14/2020
ms.custom: seodec18, fasttrack-edit, has-adal-ref
ms.openlocfilehash: 2805500e4a4c98ad7b8360393e7d69ad9fb704a3
ms.sourcegitcommit: 7edadd4bf8f354abca0b253b3af98836212edd93
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2021
ms.locfileid: "102563335"
---
# <a name="configure-your-app-service-or-azure-functions-app-to-use-azure-ad-login"></a>Konfigurieren Ihrer App Service- oder Azure Functions-App zur Verwendung der Azure AD-Anmeldung

[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

In diesem Artikel erfahren Sie, wie Sie die Authentifizierung für Azure App Service oder Azure Functions so konfigurieren, dass Ihre App Benutzer mit Azure Active Directory (Azure AD) als Authentifizierungsanbieter anmeldet.

## <a name="configure-with-express-settings"></a><a name="express"> </a>Konfigurieren mit Expresseinstellungen

Die Option **Express** ist so konzipiert, dass die Authentifizierung ganz einfach und mit nur wenigen Klicks aktiviert werden kann.

Über die Express-Einstellungen wird automatisch eine Anwendungsregistrierung erstellt, bei der der Azure Active Directory V1-Endpunkt verwendet wird. Wenn Sie [Azure Active Directory v2.0](../active-directory/develop/v2-overview.md) verwenden möchten (einschließlich [MSAL](../active-directory/develop/msal-overview.md)), befolgen Sie die [erweiterten Konfigurationsanweisungen](#advanced).

> [!NOTE]
> Die Option **Express** steht für Government Clouds nicht zur Verfügung.

Führen Sie die folgenden Schritte aus, um die Authentifizierung mithilfe der Option **Express** zu aktivieren:

1. Suchen Sie im [Azure portal] die Option **App Services**, wählen Sie sie aus, und wählen Sie anschließend Ihre App aus.
2. Wählen Sie im linken Navigationsbereich **Authentifizierung/Autorisierung** > **Ein** aus.
3. Wählen Sie **Azure Active Directory** > **Express** aus.

   Wenn Sie stattdessen eine vorhandene App-Registrierung auswählen möchten:

   1. Wählen Sie **Vorhandene AD-App auswählen** aus, und klicken Sie dann auf **Azure AD-App**.
   2. Wählen Sie eine vorhandene App-Registrierung aus, und klicken Sie auf **OK**.

4. Klicken Sie auf **OK**, um die App Service-App in Azure Active Directory zu registrieren. Eine neue App-Registrierung wird erstellt.

    ![Express-Einstellungen in Azure Active Directory.](./media/configure-authentication-provider-aad/express-settings.png)

5. (Optional:) Standardmäßig erfolgt die Authentifizierung über App Service, wobei jedoch der autorisierte Zugriff auf Ihre Websiteinhalte und APIs nicht eingeschränkt wird. Sie müssen die Benutzer in Ihrem App-Code autorisieren. Um den Zugriff auf Ihre App ausschließlich auf Benutzer zu beschränken, die von Azure Active Directory authentifiziert werden, legen Sie **Die auszuführende Aktion, wenn die Anforderung nicht authentifiziert ist** auf **Mit Azure Active Directory anmelden** fest. Wenn Sie diese Funktion festlegen, erfordert Ihre App, dass alle Anforderungen authentifiziert werden. Sie leitet außerdem alle nicht authentifizierten Anforderungen zur Authentifizierung an Azure Active Directory um.

    > [!CAUTION]
    > Das Einschränken des Zugriffs auf diese Weise gilt für alle Aufrufe Ihrer App, was für Apps, die eine öffentlich verfügbare Startseite haben, eventuell nicht wünschenswert ist, wie bei vielen Single-Page-Anwendungen. Bei solchen Anwendungen ist möglicherweise die Einstellung **Anonyme Anforderungen zulassen (keine Aktion)** vorzuziehen, wobei die App selbst die Anmeldung manuell startet. Weitere Informationen finden Sie unter [Authentifizierungsflow](overview-authentication-authorization.md#authentication-flow).
6. Wählen Sie **Speichern** aus.

Ein Beispiel für die Konfiguration der Azure AD-Anmeldung für eine Web-App, die auf Azure Storage und Microsoft Graph zugreift, finden Sie in [diesem Tutorial](scenario-secure-app-authentication-app-service.md).

## <a name="configure-with-advanced-settings"></a><a name="advanced"> </a>Konfigurieren mit erweiterten Einstellungen

Damit Azure AD als Authentifizierungsanbieter für Ihre App fungieren kann, müssen Sie Ihre App bei diesem Anbieter registrieren. Die Option „Express“ erledigt dies automatisch für Sie. Mit der Option „Erweitert“ können Sie Ihre App manuell registrieren, die Registrierung anpassen und die Registrierungsdetails manuell in App Service eingeben. Dies ist beispielsweise hilfreich, wenn Sie eine App-Registrierung von einem anderen Azure AD-Mandanten als dem verwenden möchten, auf dem sich Ihre App Service-App befindet.

### <a name="create-an-app-registration-in-azure-ad-for-your-app-service-app"></a><a name="register"> </a>Erstellen einer App-Registrierung in Azure AD für Ihre App Service-App

Erstellen Sie zuerst Ihre App-Registrierung. Halten Sie dabei die folgenden Informationen fest, die Sie später beim Konfigurieren der Authentifizierung in der App Service-App benötigen:

- Client-ID
- Mandanten-ID
- Geheimer Clientschlüssel (optional)
- Anwendungs-ID-URI

Gehen Sie wie folgt vor, um die App zu registrieren:

1. Melden Sie sich beim [Azure portal] an, suchen Sie die Option **App Services**, wählen Sie sie aus, und wählen Sie anschließend Ihre App aus. Notieren Sie sich die **URL** Ihrer App. Sie verwenden diese, um die Registrierung Ihrer Azure Active Directory-App zu konfigurieren.
1. Klicken Sie im Portalmenü auf die Option **Azure Active Directory**, navigieren Sie zur Registerkarte **App-Registrierungen** und klicken Sie auf **Neue Registrierung**.
1. Geben Sie auf der Seite **Anwendung registrieren** einen **Namen** für Ihre App-Registrierung ein.
1. Wählen Sie unter **Umleitungs-URIs** die Option **Web** aus, und geben Sie `<app-url>/.auth/login/aad/callback` ein. Beispiel: `https://contoso.azurewebsites.net/.auth/login/aad/callback`.
1. Wählen Sie **Registrieren**.
1. Nachdem die App-Registrierung erstellt wurde, kopieren Sie die **Anwendungs-ID (Client)** und die **Verzeichnis-ID (Mandant)** , damit Sie diese später verwenden können.
1. Wählen Sie **Authentifizierung** aus. Aktivieren Sie unter **Implizite Genehmigung** die Option **ID-Token**, um OpenID Connect-Benutzeranmeldungen von App Service zuzulassen.
1. (Optional) Wählen Sie **Branding** aus. Geben Sie in **URL der Startseite** die URL Ihrer App Service-App ein, und wählen Sie **Speichern** aus.
1. Wählen Sie **Eine API verfügbar machen** > **Festlegen** aus. Fügen Sie für eine Einzelmandanten-App die URL Ihrer App Service-App ein, und wählen Sie **Speichern** aus, und fügen Sie für eine mehrinstanzenfähige App die URL ein, die auf einer von Mandanten verifizierten Domäne basiert, und wählen Sie dann **Speichern** aus.

   > [!NOTE]
   > Dieser Wert ist der **Anwendungs-ID-URI** der App-Registrierung. Wenn Ihre Web-App Zugriff auf eine API in der Cloud erfordert, benötigen Sie den **Anwendungs-ID-URI** der Web-App, wenn Sie die App Service-Cloudressource konfigurieren. Sie können dies beispielsweise verwenden, wenn der Clouddienst explizit Zugriff auf die Web-App gewähren soll.

1. Wählen Sie **Bereich hinzufügen**.
   1. Geben Sie in **Bereichsname** den Namen *user_impersonation* ein.
   1. Geben Sie in die Textfelder den Namen und die Beschreibung für den Einwilligungsbereich ein, die Benutzern auf der Einwilligungsseite angezeigt werden sollen. Geben Sie z. B. *Zugriff auf meine App* ein.
   1. Wählen Sie **Bereich hinzufügen** aus.
1. (Optional) Um einen geheimen Clientschlüssel zu erstellen, wählen Sie **Zertifikate und Geheimnisse** > **Neuer geheimer Clientschlüssel** > **Hinzufügen** aus. Kopieren Sie den Wert des geheimen Clientschlüssels, der auf der Seite angezeigt wird. Er wird nicht noch einmal angezeigt.
1. (Optional) Wenn Sie mehrere **Antwort-URLs** hinzufügen möchten, wählen Sie **Authentifizierung** aus.

### <a name="enable-azure-active-directory-in-your-app-service-app"></a><a name="secrets"> </a>Aktivieren von Azure Active Directory in Ihrer App Service-App

1. Suchen Sie im [Azure portal] die Option **App Services**, wählen Sie sie aus, und wählen Sie anschließend Ihre App aus.
1. Wählen Sie im linken Bereich unter **Einstellungen** die Optionen **Authentifizierung/Autorisierung** > **Ein** aus.
1. (Optional) Standardmäßig lässt die App Service Authentifizierung nicht authentifizierten Zugriff auf Ihre App zu. Um Benutzerauthentifizierung zu erzwingen, legen Sie **Die auszuführende Aktion, wenn die Anforderung nicht authentifiziert ist** auf **Mit Azure Active Directory anmelden** fest.
1. Wählen Sie unter **Authentifizierungsanbieter** die Option **Azure Active Directory** aus.
1. Wählen Sie in **Verwaltungsmodus** die Option **Erweitert** aus, und konfigurieren Sie die App Service-Authentifizierung gemäß der folgenden Tabelle:

    |Feld|BESCHREIBUNG|
    |-|-|
    |Client-ID| Verwenden Sie die **Anwendungs-ID (Client)** der App-Registrierung. |
    |Aussteller-URL| Verwenden Sie `<authentication-endpoint>/<tenant-id>/v2.0`, und ersetzen Sie *\<authentication-endpoint>* durch den [Authentifizierungsendpunkt für Ihre Cloudumgebung](../active-directory/develop/authentication-national-cloud.md#azure-ad-authentication-endpoints) (z. B. „https://login.microsoftonline.com“ für globales Azure), und ersetzen Sie *\<tenant-id>* durch die **Verzeichnis (Mandanten)-ID**, in der die App-Registrierung erstellt wurde. Dieser Wert wird verwendet, um Benutzer an den richtigen Azure AD-Mandanten umzuleiten sowie um die entsprechenden Metadaten herunterzuladen, um die entsprechenden Tokensignaturschlüssel und den Anspruchswert des Tokenausstellers zu ermitteln. Lassen Sie für Anwendungen, die Azure AD v1 verwenden, und für Azure Functions-Apps die Zeichenfolge `/v2.0` in der URL aus.|
    |Geheimer Clientschlüssel (optional)| Verwenden Sie den geheimen Clientschlüssel, den Sie in der App-Registrierung generiert haben.|
    |Zulässige Tokenzielgruppen| Wenn dies eine Cloud- oder Server-App ist und Sie Authentifizierungstoken von einer Web-App zulassen möchten, fügen Sie hier den **Anwendungs-ID-URI** der Web-App hinzu. Die konfigurierte **Client-ID** wird *immer* implizit als zulässige Zielgruppe angesehen. |

2. Wählen Sie **OK** und anschließend **Speichern** aus.

Sie können nun Azure Active Directory für die Authentifizierung in Ihrer App Service-App verwenden.

## <a name="configure-client-apps-to-access-your-app-service"></a>Konfigurieren von Client-Apps für den Zugriff auf Ihre App Service-App

Im letzten Abschnitt haben Sie Ihre App Service- oder Azure Functions-App registriert, um Benutzer zu authentifizieren. In diesem Abschnitt wird erläutert, wie Sie native Client- oder Daemon-Apps registrieren können, damit diese im Namen von Benutzern oder im eigenen Namen Zugriff auf APIs beantragen können, die von Ihrer App Service-App zur Verfügung gestellt werden. Sie müssen die in diesem Abschnitt beschriebenen Schritte nicht ausführen, wenn Sie nur Benutzer authentifizieren möchten.

### <a name="native-client-application"></a>Native Clientanwendung

Sie können native Clients registrieren, um im Namen eines angemeldeten Benutzers Zugriff auf die APIs Ihrer App Servic-App zu beantragen.

1. Wählen Sie im [Azure portal] nacheinander **Active Directory** > **App-Registrierungen** > **Neue Registrierung** aus.
1. Geben Sie auf der Seite **Anwendung registrieren** einen **Namen** für Ihre App-Registrierung ein.
1. Wählen Sie unter **Umleitungs-URI** die Option **Öffentlicher Client (Mobilgerät und Desktop)** aus, und geben Sie die URL `<app-url>/.auth/login/aad/callback` ein. Beispiel: `https://contoso.azurewebsites.net/.auth/login/aad/callback`.

    > [!NOTE]
    > Für eine Microsoft Store-Anwendung verwenden Sie stattdessen die [Paket-SID](/previous-versions/azure/app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library#package-sid) als URI.
1. Klicken Sie auf **Erstellen**.
1. Sobald die App-Registrierung erstellt wurde, kopieren Sie den Wert von **Anwendungs-ID (Client)** .
1. Wählen Sie über die Option **API-Berechtigungen** > **Berechtigung hinzufügen** > **Meine APIs** aus.
1. Wählen Sie die App-Registrierung aus, die Sie zuvor für Ihre App Service-App erstellt haben. Wenn die App-Registrierung nicht angezeigt wird, stellen Sie sicher, dass Sie den Bereich **user_impersonation** in [Erstellen einer App-Registrierung in Azure AD für Ihre App Service-App](#register) hinzugefügt haben.
1. Wählen Sie unter **Delegierte Berechtigungen** den Eintrag **user_impersonation** aus, und wählen Sie dann **Berechtigungen hinzufügen** aus.

Sie haben nun eine native Clientanwendung konfiguriert, die im Namen eines Benutzers Zugriff auf Ihre App Service-App beantragen kann.

### <a name="daemon-client-application-service-to-service-calls"></a>Daemon-Clientanwendung (Dienst-zu-Dienst-Aufrufe)

Ihre Anwendung kann ein Token abrufen, um eine Web-API, die in Ihrer App Service- oder Funktions-App gehostet wird, in deren eigenen Namen (nicht im Namen eines Benutzers) aufzurufen. Dieses Szenario ist nützlich für nicht interaktive Daemon-Anwendungen, die Aufgaben ohne angemeldeten Benutzer ausführen. Es verwendet die Standardzuweisung für [Clientanmeldeinformationen](../active-directory/azuread-dev/v1-oauth2-client-creds-grant-flow.md) von OAuth 2.0.

1. Wählen Sie im [Azure portal] nacheinander **Active Directory** > **App-Registrierungen** > **Neue Registrierung** aus.
1. Geben Sie auf der Seite **Anwendung registrieren** einen **Namen** für Ihre Daemon-App-Registrierung ein.
1. Für eine Daemon-Anwendung benötigen Sie keinen Umleitungs-URI, sodass Sie diesen Wert leer lassen können.
1. Klicken Sie auf **Erstellen**.
1. Sobald die App-Registrierung erstellt wurde, kopieren Sie den Wert von **Anwendungs-ID (Client)** .
1. Wählen Sie **Zertifikate und Geheimnisse** > **Neuer geheimer Clientschlüssel** > **Hinzufügen** aus. Kopieren Sie den Wert des geheimen Clientschlüssels, der auf der Seite angezeigt wird. Er wird nicht noch einmal angezeigt.

Sie können jetzt [ein Zugriffstoken mithilfe der Client-ID und des geheimen Clientschlüssels anfordern](../active-directory/azuread-dev/v1-oauth2-client-creds-grant-flow.md#first-case-access-token-request-with-a-shared-secret), indem Sie den Parameter `resource` auf den **Anwendungs-ID-URI** der Ziel-App festlegen. Das resultierende Zugriffstoken kann dann der Ziel-App mithilfe des standardmäßigen [OAuth 2.0-Autorisierungsheaders](../active-directory/azuread-dev/v1-oauth2-client-creds-grant-flow.md#use-the-access-token-to-access-the-secured-resource) angezeigt werden, woraufhin die App Service-Authentifizierung/-Autorisierung das Token überprüft und wie üblich verwendet, um jetzt anzuzeigen, dass der Aufrufer (in diesem Fall eine Anwendung, kein Benutzer) authentifiziert ist.

Im Moment ermöglicht dies _jeder_ Clientanwendung in Ihrem Azure AD-Mandanten, ein Zugriffstoken anzufordern und sich bei der Ziel-App zu authentifizieren. Wenn Sie außerdem bei der _Autorisierung_ erzwingen möchten, dass nur bestimmte Clientanwendungen zugelassen werden, müssen Sie einige zusätzliche Konfigurationen vornehmen.

1. [Definieren Sie eine App-Rolle](../active-directory/develop/howto-add-app-roles-in-azure-ad-apps.md) im Manifest der App-Registrierung, die die App Service- oder Funktions-App darstellt, die Sie schützen möchten.
1. Wählen Sie in der App-Registrierung, die den Client darstellt, der autorisiert werden muss, **API-Berechtigungen** > **Berechtigung hinzufügen** > **Meine APIs** aus.
1. Wählen Sie die App-Registrierung aus, die Sie zuvor erstellt haben. Wenn die App-Registrierung nicht angezeigt wird, stellen Sie sicher, dass Sie [eine App-Rolle hinzugefügt haben](../active-directory/develop/howto-add-app-roles-in-azure-ad-apps.md).
1. Wählen Sie unter **Anwendungsberechtigungen** die zuvor erstellte App-Rolle aus, und wählen Sie dann **Berechtigungen hinzufügen** aus.
1. Stellen Sie sicher, dass Sie auf **Administratoreinwilligung erteilen**, um die Clientanwendung zum Anfordern der Berechtigung zu autorisieren.
1. Ähnlich wie im vorherigen Szenario (vor dem Hinzufügen jeglicher Rollen) können Sie jetzt [ein Zugriffstoken anfordern](../active-directory/azuread-dev/v1-oauth2-client-creds-grant-flow.md#first-case-access-token-request-with-a-shared-secret) für dieselbe Ziel-`resource`, woraufhin das Zugriffstoken einen `roles`-Anspruch aufnimmt, der die App-Rollen enthält, die für die Clientanwendung autorisiert wurden.
1. Im Code der App Service- oder Funktions-Ziel-App können Sie jetzt überprüfen, ob die erwarteten Rollen im Token vorhanden sind (dies wird nicht von der App Service-Authentifizierung/-Autorisierung durchgeführt). Weitere Informationen finden Sie unter [Zugriff auf Benutzeransprüche](app-service-authentication-how-to.md#access-user-claims).

Sie haben nun eine Daemon-Clientanwendung konfiguriert, die unter Verwendung der eigenen Identität auf Ihre App Service-App zugreifen kann.

## <a name="best-practices"></a>Bewährte Methoden

Unabhängig von der Konfiguration, die Sie zum Einrichten der Authentifizierung verwenden, werden Ihnen die folgenden bewährten Methoden dabei helfen, Ihren Mandanten und Ihre Anwendungen besser zu schützen:

- Erteilen Sie jeder App Service-App eigene Berechtigungen und Einwilligung.
- Konfigurieren Sie jede App Service-App mit eigener Registrierung.
- Vermeiden Sie das Teilen von Berechtigungen zwischen Umgebungen, indem Sie separate App-Registrierungen für gesonderte Bereitstellungsslots verwenden. Wenn Sie neuen Code testen, kann diese Vorgehensweise dabei helfen, Probleme zu verhindern, die sich auf die Produktions-App auswirken können.

## <a name="next-steps"></a><a name="related-content"> </a>Nächste Schritte

[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]
* [Tutorial: Hinzufügen der Authentifizierung zu Ihrer Web-App in Azure App Service](scenario-secure-app-authentication-app-service.md)
* [Tutorial: Umfassendes Authentifizieren und Autorisieren von Benutzern in Azure App Service](tutorial-auth-aad.md)
<!-- URLs. -->

[Azure portal]: https://portal.azure.com/
