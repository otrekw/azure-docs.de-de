---
title: Konfigurieren der Authentifizierung in einer Beispielwebanwendung, die eine Web-API mithilfe von Azure Active Directory B2C aufruft
description: Hier finden Sie Informationen zur Verwendung von Azure Active Directory B2C für die Anmeldung und Registrierung von Benutzern in einer ASP.NET-Webanwendung, die eine Web-API aufruft.
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
ms.openlocfilehash: fab89113665b8b9534118fc443474349f4e9c3b6
ms.sourcegitcommit: 8651d19fca8c5f709cbb22bfcbe2fd4a1c8e429f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/14/2021
ms.locfileid: "112072994"
---
# <a name="configure-authentication-in-a-sample-web-application-that-calls-a-web-api-using-azure-active-directory-b2c"></a>Konfigurieren der Authentifizierung in einer Beispielwebanwendung, die eine Web-API mithilfe von Azure Active Directory B2C aufruft

In diesem Artikel wird anhand einer ASP.NET-Beispielwebanwendung, die eine Web-API aufruft, veranschaulicht, wie Sie Ihren Webanwendungen die Authentifizierung von Azure Active Directory B2C (Azure AD B2C) hinzufügen.

> [!IMPORTANT]
> Die ASP.NET-Beispielwebanwendung, auf die in diesem Artikel verwiesen wird, wird verwendet, um eine Web-API mit einem Bearertoken aufzurufen. Informationen zu einer Webanwendung, die keine Web-API aufruft, finden Sie unter [Konfigurieren der Authentifizierung in einer Beispielwebanwendung mit Azure Active Directory B2C](configure-authentication-sample-web-app.md).  

## <a name="overview"></a>Übersicht

OpenID Connect (OIDC) ist ein Authentifizierungsprotokoll auf Grundlage von OAuth 2.0, mit dem Benutzer sicher bei einer Anwendung angemeldet werden können. Für dieses Web-App-Beispiel wird [Microsoft Identity Web](https://www.nuget.org/packages/Microsoft.Identity.Web) verwendet. Microsoft Identity Web ist eine Reihe von ASP.NET Core-Bibliotheken, die das Hinzufügen von Authentifizierungs- und Autorisierungsunterstützung zu Web-Apps vereinfacht, die eine sichere Web-API aufrufen können. 

Der Anmeldeflow umfasst die folgenden Schritte:

1. Der Benutzer navigiert zur Webanwendung und wählt **Anmelden** aus.
1. Die App initiiert eine Authentifizierungsanforderung und leitet den Benutzer zu Azure AD B2C um.
1. Der Benutzer [registriert sich oder meldet sich an](add-sign-up-and-sign-in-policy.md), [setzt das Kennwort zurück](add-password-reset-policy.md) oder meldet sich mit einem [Social Media-Konto](add-identity-provider.md) an.
1. Nach erfolgreicher Anmeldung gibt Azure AD B2C einen Autorisierungscode an die App zurück.
1. Die App führt die folgenden Aktionen aus:
    1. Sie tauscht den Autorisierungscode gegen ein ID-Token, ein Zugriffstoken und ein Aktualisierungstoken.
    1. Sie liest die ID-Tokenansprüche und behält ein Anwendungsautorisierungscookie bei.
    1. Sie speichert das Aktualisierungstoken zur späteren Verwendung in einem In-Memory-Cache.

### <a name="app-registration-overview"></a>Übersicht über die App-Registrierung

Damit sich Ihre App bei Azure AD B2C anmelden und eine Web-API aufrufen kann, registrieren Sie im Azure AD B2C-Verzeichnis zwei Anwendungen.  

- Durch die **Webanwendungsregistrierung** kann sich Ihre App bei Azure AD B2C anmelden. Bei der App-Registrierung geben Sie den *Umleitungs-URI* an. Der Umleitungs-URI ist der Endpunkt, an den der Benutzer von Azure AD B2C umgeleitet wird, nachdem die Authentifizierung mit Azure AD B2C abgeschlossen ist. Beim App-Registrierungsprozess wird eine *Anwendungs-ID* generiert, die auch als *Client-ID* bezeichnet wird und Ihre App eindeutig identifiziert. Außerdem erstellen Sie einen *geheimen Clientschlüssel*, der von Ihrer Anwendung zum sicheren Abrufen der Token verwendet wird.

- Durch die **Web-API**-Registrierung kann Ihre App eine sichere Web-API aufrufen. Die Registrierung umfasst die Web-API-*Bereiche*. Die Bereiche bieten eine Möglichkeit, Berechtigungen für geschützte Ressourcen wie Ihre Web-API zu verwalten. Sie erteilen der Webanwendung Berechtigungen für die Bereiche der Web-API. Wenn ein Zugriffstoken angefordert wird, gibt Ihre App im Bereichsparameter der Anforderung die gewünschten Berechtigungen an.  

In den folgenden Diagrammen werden die App-Registrierung und die Anwendungsarchitektur veranschaulicht.

![Web-App mit Web-API-Aufrufregistrierungen und Token](./media/configure-authentication-sample-web-app-with-api/web-app-with-api-architecture.png) 

### <a name="call-to-a-web-api"></a>Aufrufen einer Web-API

[!INCLUDE [active-directory-b2c-app-integration-call-api](../../includes/active-directory-b2c-app-integration-call-api.md)]

### <a name="sign-out"></a>Abmeldung

[!INCLUDE [active-directory-b2c-app-integration-sign-out-flow](../../includes/active-directory-b2c-app-integration-sign-out-flow.md)]

## <a name="prerequisites"></a>Voraussetzungen

Ein Computer, auf dem eine der folgenden Optionen ausgeführt wird: 

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

## <a name="step-2-register-web-applications"></a>Schritt 2: Registrieren von Webanwendungen

In diesem Schritt erstellen Sie die Web-App und die Web-API-Anwendungsregistrierung und geben die Bereiche Ihrer Web-API an.

### <a name="21-register-the-web-api-app"></a>2.1 Registrieren der Web-API-App

[!INCLUDE [active-directory-b2c-app-integration-register-api](../../includes/active-directory-b2c-app-integration-register-api.md)]

### <a name="22-configure-web-api-app-scopes"></a>2.2 Konfigurieren von Web-API-App-Bereichen

[!INCLUDE [active-directory-b2c-app-integration-api-scopes](../../includes/active-directory-b2c-app-integration-api-scopes.md)]


### <a name="23-register-the-web-app"></a>2.3 Registrieren der Web-App

Führen Sie die folgenden Schritte aus, um die Web-App-Registrierung zu erstellen:

1. Wählen Sie **App-Registrierungen** aus, und wählen Sie dann **Registrierung einer neuen Anwendung** aus.
1. Geben Sie unter **Name** einen Namen für die Anwendung ein. Beispiel: *webapp1*.
1. Wählen Sie unter **Unterstützte Kontotypen** die Option **Konten in einem beliebigen Identitätsanbieter oder Organisationsverzeichnis (zum Authentifizieren von Benutzern mit Benutzerflows)** aus. 
1. Wählen Sie unter **Umleitungs-URI** die Option **Web** aus, und geben Sie `https://localhost:5000/signin-oidc` in das URL-Textfeld ein.
1. Aktivieren Sie unter **Berechtigungen** das Kontrollkästchen **Administratoreinwilligung für openid- und offline_access-Berechtigungen erteilen**.
1. Wählen Sie **Registrieren**.
1. Wählen Sie nach Abschluss der App-Registrierung **Übersicht** aus.
1. Notieren Sie sich die **Anwendungs-ID (Client)** , die Sie später beim Konfigurieren der Webanwendung verwenden.

    ![Abrufen Ihrer Webanwendungs-ID](./media/configure-authentication-sample-web-app-with-api/get-azure-ad-b2c-app-id.png)  

### <a name="24-create-a-web-app-client-secret"></a>2.4 Erstellen eines geheimen Clientschlüssels für die Web-App

[!INCLUDE [active-directory-b2c-app-integration-client-secret](../../includes/active-directory-b2c-app-integration-client-secret.md)]


### <a name="25-grant-the-web-app-permissions-for-the-web-api"></a>2.5 Erteilen von Berechtigungen für die Web-App zur Verwendung der Web-API

[!INCLUDE [active-directory-b2c-app-integration-grant-permissions](../../includes/active-directory-b2c-app-integration-grant-permissions.md)]

## <a name="step-3-get-the-web-app-sample"></a>Schritt 3: Herunterladen des Web-App-Beispiels

[Laden Sie die ZIP-Datei herunter](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/archive/refs/heads/master.zip), oder klonen Sie die Beispielwebanwendung von GitHub. 

```bash
git clone https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2
```

Extrahieren Sie die Beispieldatei in einem Ordner, dessen Pfad insgesamt maximal 259 Zeichen lang ist.

## <a name="step-4-configure-the-sample-web-api"></a>Schritt 4: Konfigurieren der Beispiel-Web-API

Öffnen Sie im Beispielordner unter dem Ordner `4-WebApp-your-API/4-2-B2C/TodoListService` das Projekt **TodoListService.csproj** mit Visual Studio oder Visual Studio Code. 

Öffnen Sie im Projektstammordner die Datei `appsettings.json`. Diese Datei enthält Informationen zu Ihrem Azure AD B2C-Identitätsanbieter. Die Web-API-App verwendet diese Informationen, um das Zugriffstoken zu überprüfen, das die Web-App als Bearertoken übergibt. Aktualisieren Sie die folgenden Eigenschaften der App-Einstellungen:

|`Section`  |Key  |Wert  |
|---------|---------|---------|
|AzureAdB2C|Instanz| Der erste Teil Ihres Azure AD B2C-[Mandantennamens](tenant-management.md#get-your-tenant-name). Beispielsweise `https://contoso.b2clogin.com`.|
|AzureAdB2C|Domain| Der vollständige [Mandantenname](tenant-management.md#get-your-tenant-name) Ihres Azure AD B2C-Mandanten. Beispielsweise `contoso.onmicrosoft.com`.|
|AzureAdB2C|ClientId| Die Web-API-Anwendungs-ID aus [Schritt 2.1](#21-register-the-web-api-app).|
|AzureAdB2C|SignUpSignInPolicyId|Die Benutzerflows oder die benutzerdefinierte Richtlinie, die Sie in [Schritt 1](#step-1-configure-your-user-flow) erstellt haben.|

Die endgültige Konfigurationsdatei sollte wie die folgende JSON aussehen:

```json
{
  "AzureAdB2C": {
    "Instance": "https://contoso.b2clogin.com",
    "Domain": "contoso.onmicrosoft.com",
    "ClientId": "<web-api-app-application-id>",
    "SignedOutCallbackPath": "/signout/<your-sign-up-in-policy>",
    "SignUpSignInPolicyId": "<your-sign-up-in-policy>"
  },
  // More setting here
}
```

### <a name="41-set-the-permission-policy"></a>4.1 Festlegen der Berechtigungsrichtlinie

Die Web-API überprüft, ob sich der Benutzer mit dem Bearertoken authentifiziert hat, und das Bearertoken verfügt über die konfigurierten akzeptierten Bereiche. Wenn das Bearertoken über keinen dieser akzeptierten Bereiche verfügt, gibt die Web-API den HTTP-Statuscode 403 (Verboten) zurück und schreibt in den Antworttext eine Meldung, die angibt, welche Bereiche im Token erwartet werden. 

Öffnen Sie zum Konfigurieren der akzeptierten Bereiche die `Controller/TodoListController.cs`-Klasse, und legen Sie den Bereichsnamen fest. Der Bereichsname ohne den vollständigen URI:

```csharp
[RequiredScope("tasks.read")]
```

### <a name="42-run-the-sample-web-api-app"></a>4.2 Ausführen der Beispiel-Web-API-App

Führen Sie zum Ausführen der Web-API die folgenden Schritte aus, um das Aufrufen des Web-API-Beispiels durch eine Web-App zuzulassen:

1. Stellen Sie auf Anforderung Abhängigkeiten wieder her.
1. Erstellen Sie das Projekt, und führen Sie es aus.
1. Nachdem das Projekt erstellt wurde, startet Visual Studio oder Visual Studio Code die Web-API in den Browsern mit der folgenden Adresse: https://localhost:44332.

## <a name="step-5-configure-the-sample-web-app"></a>Schritt 5: Konfigurieren der Beispiel-Web-App

Öffnen Sie im Beispielordner unter dem Ordner `4-WebApp-your-API/4-2-B2C/Client` das Projekt **TodoListClient.csproj** mit Visual Studio oder Visual Studio Code. 

Öffnen Sie im Projektstammordner die Datei `appsettings.json`. Diese Datei enthält Informationen zu Ihrem Azure AD B2C-Identitätsanbieter. Die Web-App verwendet diese Informationen, um eine Vertrauensstellung mit Azure AD B2C herzustellen, den Benutzer anzumelden und abzumelden sowie Token zu erhalten und zu überprüfen. Aktualisieren Sie die folgenden Eigenschaften der App-Einstellungen:

|`Section`  |Key  |Wert  |
|---------|---------|---------|
|AzureAdB2C|Instanz| Der erste Teil Ihres Azure AD B2C-[Mandantennamens](tenant-management.md#get-your-tenant-name). Beispielsweise `https://contoso.b2clogin.com`.|
|AzureAdB2C|Domain| Der vollständige [Mandantenname](tenant-management.md#get-your-tenant-name) Ihres Azure AD B2C-Mandanten. Beispielsweise `contoso.onmicrosoft.com`.|
|AzureAdB2C|ClientId| Die Webanwendungs-ID aus [Schritt 2.1](#21-register-the-web-api-app).|
|AzureAdB2C | ClientSecret | Das Geheimnis der Webanwendung aus [Schritt 2.4](#24-create-a-web-app-client-secret). | 
|AzureAdB2C|SignUpSignInPolicyId|Die Benutzerflows oder die benutzerdefinierte Richtlinie, die Sie in [Schritt 1](#step-1-configure-your-user-flow) erstellt haben.|
| Aufgabenliste | TodoListScope | Die Bereiche aus [Schritt 2.5](#25-grant-the-web-app-permissions-for-the-web-api).|
| Aufgabenliste | TodoListBaseAddress | Der Basis-URI Ihrer Web-API, z. B. `https://localhost:44332`|

Die endgültige Konfigurationsdatei sollte wie die folgende JSON aussehen:

```JSon
{
  "AzureAdB2C": {
    "Instance": "https://contoso.b2clogin.com",
    "Domain": "contoso.onmicrosoft.com",
    "ClientId": "<web-app-application-id>",
    "ClientSecret": "<web-app-application-secret>",  
    "SignedOutCallbackPath": "/signout/<your-sign-up-in-policy>",
    "SignUpSignInPolicyId": "<your-sign-up-in-policy>"
  },
  "TodoList": {
    "TodoListScope": "https://contoso.onmicrosoft.com/api/demo.read",
    "TodoListBaseAddress": "https://localhost:44332"
  }
}
```


## <a name="step-6-run-the-sample-web-app"></a>Schritt 6: Ausführen der Beispiel-Web-App

1. Erstellen Sie das Projekt, und führen Sie es aus.
1. Navigieren Sie zu https://localhost:5000. 
1. Schließen Sie den Registrierungs- oder Anmeldevorgang ab.

Nach erfolgreicher Authentifizierung wird Ihr Anzeigename in der Navigationsleiste angezeigt. Wählen Sie **TodoList** aus, um die Ansprüche anzuzeigen, die das Azure AD B2C-Token an Ihre App zurückgibt.

![Ansprüche des Web-App-Token](./media/configure-authentication-sample-web-app-with-api/web-api-to-do-list.png)


## <a name="deploy-your-application"></a>Bereitstellen der Anwendung

In einer Produktionsanwendung handelt es sich bei dem Umleitungs-URI der App-Registrierung in der Regel um einen öffentlich zugänglichen Endpunkt, an dem Ihre App ausgeführt wird, wie beispielsweise `https://contoso.com/signin-oidc`. 

Sie können Umleitungs-URIs in Ihren registrierten Anwendungen jederzeit hinzufügen und ändern. Für Umleitungs-URIs gelten die folgenden Einschränkungen:

* Die Antwort-URL muss mit dem Schema `https` beginnen.
* Bei der Antwort-URL muss die Groß-/Kleinschreibung beachtet werden. Die Groß-/Kleinschreibung muss der Groß-/Kleinschreibung des URL-Pfads Ihrer ausgeführten Anwendung entsprechen. 

### <a name="token-cache-for-a-web-app"></a>Tokencache für eine Web-App

Das Web-App-Beispiel verwendet In-Memory-Tokencacheserialisierung. Diese Implementierung eignet sich hervorragend für Beispiele. Sie eignet sich auch für Produktionsanwendungen, falls es Ihnen nichts ausmacht, wenn der Tokencache beim Neustart der Web-App verloren geht. 

Für Produktionsumgebungen wird empfohlen, einen verteilten Speichercache zu verwenden, z. B. Redis Cache, NCache oder einen SQL Server-Cache. Ausführliche Informationen zu den Implementierungen des verteilten Speichercaches finden Sie unter [Tokencache für eine Web-App](../active-directory/develop/msal-net-token-cache-serialization.md#token-cache-for-a-web-app-confidential-client-application).


## <a name="next-steps"></a>Nächste Schritte

* [Weitere Informationen zum Codebeispiel](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-5-B2C#about-the-code)
* [Aktivieren der Authentifizierung in Ihrer eigenen Webanwendung mit Azure Active Directory B2C](enable-authentication-web-application.md)
