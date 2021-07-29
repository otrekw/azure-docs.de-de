---
title: Konfigurieren der Authentifizierung in einer Beispielwebanwendung mit Azure Active Directory B2C
description: Hier finden Sie Informationen zum Verwenden von Azure Active Directory B2C zum Anmelden und Registrieren von Benutzern in einer ASP.NET-Webanwendung.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 06/11/2021
ms.author: mimart
ms.subservice: B2C
ms.custom: b2c-support
ms.openlocfilehash: 23b66bef9de9fc83884f882eee3ad21aba695b48
ms.sourcegitcommit: 8651d19fca8c5f709cbb22bfcbe2fd4a1c8e429f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/14/2021
ms.locfileid: "112071327"
---
# <a name="configure-authentication-in-a-sample-web-application-using-azure-active-directory-b2c"></a>Konfigurieren der Authentifizierung in einer Beispielwebanwendung mit Azure Active Directory B2C

In diesem Artikel wird anhand einer ASP.NET-Beispielwebanwendung veranschaulicht, wie Sie Ihren Webanwendungen die Authentifizierung von Azure Active Directory B2C (Azure AD B2C) hinzufügen.

> [!IMPORTANT]
> Die in diesem Artikel referenzierte ASP.NET-Beispielwebanwendung kann nicht zum Aufrufen einer REST-API verwendet werden, weil sie ein ID-Token, aber kein Zugriffstoken zurückgibt. Ein Beispiel für eine Webanwendung, die eine REST-API aufrufen kann, finden Sie unter [Schützen einer mit ASP.NET Core erstellten Web-API mithilfe von Azure AD B2C](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/4-WebApp-your-API/4-2-B2C).  

## <a name="overview"></a>Übersicht

OpenID Connect (OIDC) ist ein Authentifizierungsprotokoll auf Grundlage von OAuth 2.0, mit dem Benutzer sicher bei einer Anwendung angemeldet werden können. In diesem Web-App-Beispiel wird [Microsoft Identity Web](https://www.nuget.org/packages/Microsoft.Identity.Web) verwendet. Bei Microsoft Identity Web handelt es sich um eine Reihe von ASP.NET Core-Bibliotheken, die das Hinzufügen von Authentifizierungs- und Autorisierungsunterstützung zu Web-Apps vereinfachen. 

Der Anmeldeflow umfasst die folgenden Schritte:

1. Der Benutzer navigiert zur Web-App und wählt **Anmelden** aus. 
1. Die App initiiert eine Authentifizierungsanforderung und leitet den Benutzer zu Azure AD B2C um.
1. Der Benutzer [registriert sich oder meldet sich an](add-sign-up-and-sign-in-policy.md), [setzt das Kennwort zurück](add-password-reset-policy.md) oder meldet sich mit einem [Social Media-Konto](add-identity-provider.md) an.
1. Nach erfolgreicher Anmeldung gibt Azure AD B2C ein ID-Token an die App zurück.
1. Die App überprüft das ID-Token, liest die Ansprüche und gibt eine sichere Seite an den Benutzer zurück.

Wenn das ID-Token abgelaufen ist oder die App-Sitzung ungültig wird, initiiert die App eine neue Authentifizierungsanforderung und leitet den Benutzer zu Azure AD B2C um. Wenn die Azure AD B2C-[SSO-Sitzung](session-behavior.md) aktiv ist, gibt Azure AD B2C ein Zugriffstoken aus, ohne den Benutzer zur erneuten Anmeldung aufzufordern. Wenn die Azure AD B2C-Sitzung abläuft oder ungültig wird, dann wird der Benutzer zur erneuten Anmeldung aufgefordert.

### <a name="sign-out"></a>Abmeldung

[!INCLUDE [active-directory-b2c-app-integration-sign-out-flow](../../includes/active-directory-b2c-app-integration-sign-out-flow.md)] 

## <a name="prerequisites"></a>Voraussetzungen

Ein Computer, auf dem eine der folgenden Komponenten ausgeführt wird: 

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* [Visual Studio 2019 Version 16.8 oder höher](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) mit der Workload **ASP.NET und Webentwicklung**
* [.NET 5.0 SDK](https://dotnet.microsoft.com/download/dotnet)

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [Visual Studio Code](https://code.visualstudio.com/download)
* [C# für Visual Studio Code (neueste Version)](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)
* [.NET 5.0 SDK](https://dotnet.microsoft.com/download/dotnet)

---

## <a name="step-1-configure-your-user-flow"></a>Schritt 1: Konfigurieren Ihres Benutzerflows

[!INCLUDE [active-directory-b2c-app-integration-add-user-flow](../../includes/active-directory-b2c-app-integration-add-user-flow.md)]

## <a name="step-2-register-a-web-application"></a>Schritt 2: Registrieren einer Webanwendung

Damit sich Ihre Anwendung mit Azure AD B2C anmelden kann, müssen Sie Ihre App im Azure AD B2C-Verzeichnis registrieren. Durch das Registrieren Ihrer App wird eine Vertrauensstellung zwischen der App und Azure AD B2C eingerichtet.  

Bei der App-Registrierung geben Sie den **Umleitungs-URI** an. Der Umleitungs-URI ist der Endpunkt, an den der Benutzer von Azure AD B2C umgeleitet wird, nachdem er sich mit Azure AD B2C authentifiziert hat. Beim App-Registrierungsprozess wird eine **Anwendungs-ID** generiert, die auch als **Client-ID** bezeichnet wird und Ihre App eindeutig identifiziert. Nachdem Ihre App registriert wurde, verwendet Azure AD B2C sowohl die Anwendungs-ID als auch den Umleitungs-URI, um Authentifizierungsanforderungen zu erstellen. 

### <a name="register-the-app"></a>Registrieren der App

Führen Sie die folgenden Schritte aus, um die App-Registrierung zu erstellen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie auf der Symbolleiste des Portals das Symbol **Verzeichnis und Abonnement** aus, und wählen Sie dann das Verzeichnis aus, das Ihren Azure AD B2C-Mandanten enthält.
1. Suchen Sie im Azure-Portal nach **Azure AD B2C**, und wählen Sie diese Option dann aus.
1. Wählen Sie **App-Registrierungen** aus, und wählen Sie dann **Registrierung einer neuen Anwendung** aus.
1. Geben Sie unter **Name** einen Namen für die Anwendung ein. Beispiel: *webapp1*.
1. Wählen Sie unter **Unterstützte Kontotypen** die Option **Konten in einem beliebigen Identitätsanbieter oder Organisationsverzeichnis (zum Authentifizieren von Benutzern mit Benutzerflows)** aus. 
1. Wählen Sie unter **Umleitungs-URI** die Option **Web** aus, und geben Sie `https://localhost:5001/signin-oidc` in das URL-Textfeld ein.
1. Aktivieren Sie unter **Berechtigungen** das Kontrollkästchen **Administratoreinwilligung für OpenID- und Offlinezugriffsberechtigungen erteilen**.
1. Wählen Sie **Registrieren**.
1. Wählen Sie **Übersicht**.
1. Notieren Sie sich die **Anwendungs-ID (Client)** , die Sie später beim Konfigurieren der Webanwendung verwenden.

    ![Abrufen Ihrer Anwendungs-ID](./media/configure-authentication-sample-web-app/get-azure-ad-b2c-app-id.png)  


### <a name="enable-id-tokens"></a>Aktivieren von ID-Token

Aktivieren Sie für Web-Apps, die ein ID-Token direkt von Azure AD B2C anfordern, in der App-Registrierung den Flow zur impliziten Genehmigung.

1. Wählen Sie im linken Menü unter **Verwalten** die Option **Authentifizierung** aus.
1. Aktivieren Sie unter **Implizite Genehmigung** das Kontrollkästchen **ID-Token**.
1. Wählen Sie **Speichern** aus.

## <a name="step-3-get-the-web-app-sample"></a>Schritt 3: Herunterladen des Web-App-Beispiels

[Laden Sie die ZIP-Datei herunter](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/archive/refs/heads/master.zip), oder klonen Sie die Beispielwebanwendung von GitHub. 

```bash
git clone https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2
```

Extrahieren Sie die Beispieldatei in einen Ordner, dessen Pfad insgesamt maximal 259 Zeichen umfasst.

## <a name="step-4-configure-the-sample-application"></a>Schritt 4: Konfigurieren der Beispielanwendung

Öffnen Sie im Beispielordner unter dem Ordner `1-WebApp-OIDC/1-5-B2C/` das Projekt **WebApp-OpenIDConnect-DotNet.csproj** mit Visual Studio oder Visual Studio Code. 

Öffnen Sie im Projektstammordner die Datei `appsettings.json`. Diese Datei enthält Informationen zu Ihrem Azure AD B2C-Identitätsanbieter. Aktualisieren Sie die folgenden Eigenschaften der App-Einstellungen: 

|`Section`  |Key  |Wert  |
|---------|---------|---------|
|AzureAdB2C|Instanz| Der erste Teil Ihres Azure AD B2C-[Mandantennamens](tenant-management.md#get-your-tenant-name). Beispielsweise `https://contoso.b2clogin.com`.|
|AzureAdB2C|Domain| Der vollständige [Mandantenname](tenant-management.md#get-your-tenant-name) Ihres Azure AD B2C-Mandanten. Beispielsweise `contoso.onmicrosoft.com`.|
|AzureAdB2C|ClientId| Die Web-API-Anwendungs-ID aus [Schritt 2](#step-2-register-a-web-application).|
|AzureAdB2C|SignUpSignInPolicyId|Die Benutzerflows oder die benutzerdefinierte Richtlinie, die Sie in [Schritt 1](#step-1-configure-your-user-flow) erstellt haben.|

Die endgültige Konfigurationsdatei sollte wie das folgende JSON-Beispiel aussehen:

```JSon
"AzureAdB2C": {
  "Instance": "https://contoso.b2clogin.com",
  "Domain": "contoso.onmicrosoft.com",
  "ClientId": "<web-app-application-id>",
  "SignedOutCallbackPath": "/signout/<your-sign-up-in-policy>",
  "SignUpSignInPolicyId": "<your-sign-up-in-policy>"
}
```

## <a name="step-5-run-the-sample-application"></a>Schritt 5: Ausführen der Beispielanwendung

1. Erstellen Sie das Projekt, und führen Sie es aus.
1. Navigieren Sie zu https://localhost:5001. 
1. Wählen Sie **Anmelden/Registrieren** aus.

    ![Schaltfläche für die Anmeldung oder Registrierung auswählen](./media/configure-authentication-sample-web-app/web-app-sign-in.png)

1. Schließen Sie den Registrierungs- oder Anmeldevorgang ab.

Nach erfolgreicher Authentifizierung wird Ihr Anzeigename auf der Navigationsleiste angezeigt. Wenn Sie die Ansprüche anzeigen möchten, die das Azure AD B2C-Token an Ihre App zurückgibt, wählen Sie **Ansprüche** aus.

![Ansprüche des Web-App-Tokens](./media/configure-authentication-sample-web-app/web-app-token-claims.png)

## <a name="deploy-your-application"></a>Bereitstellen der Anwendung 

In einer Produktionsanwendung handelt es sich bei dem Umleitungs-URI der App-Registrierung in der Regel um einen öffentlich zugänglichen Endpunkt, an dem Ihre App ausgeführt wird, wie beispielsweise `https://contoso.com/signin-oidc`. 

Sie können Umleitungs-URIs in Ihren registrierten Anwendungen jederzeit hinzufügen und ändern. Für Umleitungs-URIs gelten die folgenden Einschränkungen:

* Die Antwort-URL muss mit dem Schema `https` beginnen.
* Bei der Antwort-URL muss die Groß-/Kleinschreibung beachtet werden. Die Groß-/Kleinschreibung muss der Groß-/Kleinschreibung des URL-Pfads Ihrer ausgeführten Anwendung entsprechen. 

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über das [Codebeispiel](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-5-B2C#about-the-code).
* [Aktivieren der Authentifizierung in Ihrer eigenen Webanwendung mit Azure Active Directory B2C](enable-authentication-web-application.md)