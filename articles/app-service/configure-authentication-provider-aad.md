---
title: Konfigurieren von Azure AD-Authentifizierung
description: Erfahren Sie, wie Sie die Azure Active Directory-Authentifizierung als Identitätsanbieter für Ihre App Services-App konfigurieren.
ms.assetid: 6ec6a46c-bce4-47aa-b8a3-e133baef22eb
ms.topic: article
ms.date: 09/03/2019
ms.custom: fasttrack-edit
ms.openlocfilehash: 717336e0ddfe99c96afda4861f4de1239ee949bf
ms.sourcegitcommit: 3c925b84b5144f3be0a9cd3256d0886df9fa9dc0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2020
ms.locfileid: "77913207"
---
# <a name="configure-your-app-service-app-to-use-azure-ad-login"></a>Konfigurieren Ihrer App Service-App zur Verwendung der Azure AD-Anmeldung

[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

In diesem Artikel wird gezeigt, wie Sie Azure App Service so konfigurieren, dass Azure Active Directory (Azure AD) als Authentifizierungsanbieter verwendet wird.

Befolgen Sie diese bewährten Methoden, wenn Sie Ihre App und die Authentifizierung einrichten:

- Erteilen Sie jeder App Service-App eigene Berechtigungen und Einwilligung.
- Konfigurieren Sie jede App Service-App mit eigener Registrierung.
- Vermeiden Sie das Teilen von Berechtigungen zwischen Umgebungen, indem Sie separate App-Registrierungen für gesonderte Bereitstellungsslots verwenden. Wenn Sie neuen Code testen, kann diese Vorgehensweise dabei helfen, Probleme zu verhindern, die sich auf die Produktions-App auswirken können.

## <a name="express"> </a>Konfigurieren mit Expresseinstellungen

> [!NOTE]
> Die Option **Express** steht für Government Clouds nicht zur Verfügung. 

1. Suchen Sie im [Azure portal] die Option **App Services**, wählen Sie sie aus, und wählen Sie anschließend Ihre App aus.
2. Wählen Sie im linken Navigationsbereich **Authentifizierung/Autorisierung** > **Ein** aus.
3. Wählen Sie **Azure Active Directory** > **Express** aus.

   Wenn Sie stattdessen eine vorhandene App-Registrierung auswählen möchten:

   1. Wählen Sie **Vorhandene AD-App auswählen** aus, und klicken Sie dann auf **Azure AD-App**.
   2. Wählen Sie eine vorhandene App-Registrierung aus, und klicken Sie auf **OK**.

3. Klicken Sie auf **OK**, um die App Service-App in Azure Active Directory zu registrieren. Eine neue App-Registrierung wird erstellt.
   
    ![Express-Einstellungen in Azure Active Directory.](./media/configure-authentication-provider-aad/express-settings.png)
   
4. (Optional:) Standardmäßig erfolgt die Authentifizierung über App Service, wobei jedoch der autorisierte Zugriff auf Ihre Websiteinhalte und APIs nicht eingeschränkt wird. Sie müssen die Benutzer in Ihrem App-Code autorisieren. Um den Zugriff auf Ihre App ausschließlich auf Benutzer zu beschränken, die von Azure Active Directory authentifiziert werden, legen Sie **Die auszuführende Aktion, wenn die Anforderung nicht authentifiziert ist** auf **Mit Azure Active Directory anmelden** fest. Wenn Sie diese Funktion festlegen, erfordert Ihre App, dass alle Anforderungen authentifiziert werden. Sie leitet außerdem alle nicht authentifizierten Anforderungen zur Authentifizierung an Azure Active Directory um.

    > [!CAUTION]
    > Das Einschränken des Zugriffs auf diese Weise gilt für alle Aufrufe Ihrer App, was für Apps, die eine öffentlich verfügbare Startseite haben, eventuell nicht wünschenswert ist, wie bei vielen Single-Page-Anwendungen. Bei solchen Anwendungen ist möglicherweise die Einstellung **Anonyme Anforderungen zulassen (keine Aktion)** vorzuziehen, wobei die App selbst die Anmeldung manuell startet. Weitere Informationen finden Sie unter [Authentifizierungsflow](overview-authentication-authorization.md#authentication-flow).
5. Wählen Sie **Speichern** aus.

## <a name="advanced"> </a>Konfigurieren mit erweiterten Einstellungen

Sie können App-Einstellungen manuell konfigurieren, wenn Sie eine App-Registrierung von einem anderen Azure AD-Mandanten verwenden möchten. Gehen Sie zum Abschließen dieser benutzerdefinierten Konfiguration wie folgt vor:

1. Erstellen einer Registrierung in Azure AD.
2. Bereitstellen einige Registrierungsdetails für den App Service.

### <a name="register"> </a>Erstellen einer App-Registrierung in Azure AD für Ihre App Service-App

Sie benötigen die folgenden Informationen, wenn Sie Ihre App Service-App konfigurieren:

- Client-ID
- Mandanten-ID
- Geheimer Clientschlüssel (optional)
- Anwendungs-ID-URI

Führen Sie die folgenden Schritte aus:

1. Melden Sie sich beim [Azure portal] an, suchen Sie die Option **App Services**, wählen Sie sie aus, und wählen Sie anschließend Ihre App aus. Notieren Sie sich die **URL** Ihrer App. Sie verwenden diese, um die Registrierung Ihrer Azure Active Directory-App zu konfigurieren.
1. Wählen Sie **Azure Active Directory** > **App-Registrierungen** > **Neue Registrierung** aus.
1. Geben Sie auf der Seite **Anwendung registrieren** einen **Namen** für Ihre App-Registrierung ein.
1. Wählen Sie unter **Umleitungs-URIs** die Option **Web** aus, und geben Sie `<app-url>/.auth/login/aad/callback` ein. Beispiel: `https://contoso.azurewebsites.net/.auth/login/aad/callback`. 
1. Klicken Sie auf **Erstellen**.
1. Nachdem die App-Registrierung erstellt wurde, kopieren Sie die **Anwendungs-ID (Client)** und die **Verzeichnis-ID (Mandant)** , damit Sie diese später verwenden können.
1. Wählen Sie **Branding** aus. Geben Sie in **URL der Startseite** die URL Ihrer App Service-App ein, und wählen Sie **Speichern** aus.
1. Wählen Sie **Eine API verfügbar machen** > **Festlegen** aus. Fügen Sie die URL Ihrer App Service-App ein, und wählen Sie **Speichern** aus.

   > [!NOTE]
   > Dieser Wert ist der **Anwendungs-ID-URI** der App-Registrierung. Wenn Ihre Web-App Zugriff auf eine API in der Cloud erfordert, benötigen Sie den **Anwendungs-ID-URI** der Web-App, wenn Sie die App Service-Cloudressource konfigurieren. Sie können dies beispielsweise verwenden, wenn der Clouddienst explizit Zugriff auf die Web-App gewähren soll.

1. Wählen Sie **Bereich hinzufügen**.
   1. Geben Sie in **Bereichsname** den Namen *user_impersonation* ein.
   1. Geben Sie in die Textfelder den Namen und die Beschreibung für den Einwilligungsbereich ein, die Benutzern auf der Einwilligungsseite angezeigt werden sollen. Geben Sie z. B. *Zugriff auf meine App* ein. 
   1. Wählen Sie **Bereich hinzufügen** aus.
1. (Optional) Um einen geheimen Clientschlüssel zu erstellen, wählen Sie **Zertifikate und Geheimnisse** > **Neuer geheimer Clientschlüssel** > **Hinzufügen** aus. Kopieren Sie den Wert des geheimen Clientschlüssels, der auf der Seite angezeigt wird. Er wird nicht noch einmal angezeigt.
1. (Optional) Wenn Sie mehrere **Antwort-URLs** hinzufügen möchten, wählen Sie **Authentifizierung** aus.

### <a name="secrets"> </a>Aktivieren von Azure Active Directory in Ihrer App Service-App

1. Suchen Sie im [Azure portal] die Option **App Services**, wählen Sie sie aus, und wählen Sie anschließend Ihre App aus. 
1. Wählen Sie im linken Bereich unter **Einstellungen** die Optionen **Authentifizierung/Autorisierung** > **Ein** aus.
1. (Optional) Standardmäßig lässt die App Service Authentifizierung nicht authentifizierten Zugriff auf Ihre App zu. Um Benutzerauthentifizierung zu erzwingen, legen Sie **Die auszuführende Aktion, wenn die Anforderung nicht authentifiziert ist** auf **Mit Azure Active Directory anmelden** fest.
1. Wählen Sie unter **Authentifizierungsanbieter** die Option **Azure Active Directory** aus.
1. Wählen Sie in **Verwaltungsmodus** die Option **Erweitert** aus, und konfigurieren Sie die App Service-Authentifizierung gemäß der folgenden Tabelle:

    |Feld|BESCHREIBUNG|
    |-|-|
    |Client-ID| Verwenden Sie die **Anwendungs-ID (Client)** der App-Registrierung. |
    |Zertifikataussteller-ID| Verwenden Sie `https://login.microsoftonline.com/<tenant-id>`, und ersetzen Sie *\<tenant-id* durch die **Verzeichnis-ID (Mandant)** der App-Registrierung. |
    |Geheimer Clientschlüssel (optional)| Verwenden Sie den geheimen Clientschlüssel, den Sie in der App-Registrierung generiert haben.|
    |Zulässige Tokenzielgruppen| Wenn dies eine Cloud- oder Server-App ist und Sie Authentifizierungstoken von einer Web-App zulassen möchten, fügen Sie hier den **Anwendungs-ID-URI** der Web-App hinzu. Die konfigurierte **Client-ID** wird *immer* implizit als zulässige Zielgruppe angesehen. |

2. Wählen Sie **OK** und anschließend **Speichern** aus.

Sie können nun Azure Active Directory für die Authentifizierung in Ihrer App Service-App verwenden.

## <a name="configure-a-native-client-application"></a>Konfigurieren einer nativen Clientanwendung

Sie können native Clients registrieren, um die Authentifizierung mittel Clientbibliothek zuzulassen, etwa der **Active Directory-Authentifizierungsbibliothek**.

1. Wählen Sie im [Azure portal] nacheinander **Active Directory** > **App-Registrierungen** > **Neue Registrierung** aus.
1. Geben Sie auf der Seite **Anwendung registrieren** einen **Namen** für Ihre App-Registrierung ein.
1. Wählen Sie unter **Umleitungs-URI** die Option **Öffentlicher Client (Mobilgerät und Desktop)** aus, und geben Sie die URL `<app-url>/.auth/login/aad/callback` ein. Beispiel: `https://contoso.azurewebsites.net/.auth/login/aad/callback`.

    > [!NOTE]
    > Für eine Windows-Anwendung verwenden Sie stattdessen die [Paket-SID](../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#package-sid) als URI.
1. Klicken Sie auf **Erstellen**.
1. Sobald die App-Registrierung erstellt wurde, kopieren Sie den Wert von **Anwendungs-ID (Client)** .
1. Wählen Sie über die Option **API-Berechtigungen** > **Berechtigung hinzufügen** > **Meine APIs** aus.
1. Wählen Sie die App-Registrierung aus, die Sie zuvor für Ihre App Service-App erstellt haben. Wenn die App-Registrierung nicht angezeigt wird, stellen Sie sicher, dass Sie den Bereich **user_impersonation** in [Erstellen einer App-Registrierung in Azure AD für Ihre App Service-App](#register) hinzugefügt haben.
1. Wählen Sie **user_impersonation** aus, und wählen Sie dann **Berechtigungen hinzufügen** aus.

Sie haben nun eine native Clientanwendung konfiguriert, die auf Ihre App Service-App zugreifen kann.

## <a name="related-content"> </a>Nächste Schritte

[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- URLs. -->

[Azure portal]: https://portal.azure.com/
