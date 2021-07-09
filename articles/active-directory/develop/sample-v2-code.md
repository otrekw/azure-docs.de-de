---
title: Codebeispiele für die Microsoft Identity Platform
description: Dieser Artikel enthält einen Index der verfügbaren Microsoft Identity Platform-Codebeispiele, geordnet nach Szenario.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: sample
ms.workload: identity
ms.date: 11/04/2020
ms.author: marsma
ms.reviewer: jmprieur
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: a4beecec7a1b9dc25a19732e5d6687fba848d2fa
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110083060"
---
# <a name="microsoft-identity-platform-code-samples-v20-endpoint"></a>Microsoft Identity Platform – Codebesipiele (V2.0-Endpunkt)

Sie können mit Microsoft Identity Platform folgende Aktionen ausführen:

- Hinzufügen von Authentifizierungs- und Autorisierungsfeatures zu Ihren Webanwendungen und Web-APIs
- Anfordern eines Zugriffstokens für den Zugriff auf eine geschützte Web-API

In diesem Artikel werden Links und Beispiele für Microsoft Identity Platform bereitgestellt und kurz erläutert. Anhand dieser Beispiele wird die Vorgehensweise erläutert, und es werden Codeausschnitte bereitgestellt, die Sie in Ihren Anwendungen verwenden können. Auf der Seite mit den Codebeispielen finden Sie ausführliche Infothemen mit hilfreichen Angaben zu Anforderungen, Installation und Einrichtung. Außerdem enthält der Code Kommentare, die die wichtigsten Abschnitte besser verständlich machen.

Informationen zum grundlegenden Szenario für die einzelnen Beispieltypen finden Sie unter [Anwendungstypen für Microsoft Identity Platform](v2-app-types.md).

Sie können auch Beiträge zu den Beispielen auf GitHub schreiben. Wie das geht, erfahren Sie unter [Microsoft Azure Active Directory – Beispiele und Dokumentation](https://github.com/Azure-Samples?page=3&query=active-directory).

## <a name="single-page-applications"></a>Single-Page-Webanwendungen

Diese Beispiele zeigen, wie Sie eine Single-Page-Webanwendung schreiben, die mit Microsoft Identity Platform geschützt ist. Diese Beispiele verwenden eine der Arten von MSAL.js.

> [!div class="mx-tdCol2BreakAll"]
> | Sprache/<br/>Plattform | Codebeispiel | Beschreibung | Auth-Bibliotheken | Autorisierungsflow |  
> | ------- | -------- | --------------------------- | ------------- | -------------- |  
> |Angular|[GitHub-Repository](https://github.com/Azure-Samples/ms-identity-javascript-angular-spa)| &#8226; Meldet Benutzer bei AAD an. <br/>&#8226; Ruft Microsoft Graph auf. | MSAL Angular | Autorisierungscodeflow (mit PKCE) |
> | Angular | [GitHub-Repository](https://github.com/Azure-Samples/ms-identity-javascript-angular-tutorial) | &#8226; [Meldet Benutzer an](https://github.com/Azure-Samples/ms-identity-javascript-angular-tutorial/tree/main/1-Authentication/1-sign-in/README.md)<br/>&#8226; [Meldet Benutzer an (B2C)](https://github.com/Azure-Samples/ms-identity-javascript-angular-tutorial/tree/main/1-Authentication/2-sign-in-b2c/README.md) <br/> &#8226; [Ruft Microsoft Graph auf](https://github.com/Azure-Samples/ms-identity-javascript-angular-tutorial/tree/main/2-Authorization-I/1-call-graph/README.md)<br/>&#8226; [Ruft die .NET Core-Web-API auf](https://github.com/Azure-Samples/ms-identity-javascript-angular-tutorial/tree/main/3-Authorization-II/1-call-api)<br/>&#8226; [Ruft die .NET Core-Web-API auf (B2C)](https://github.com/Azure-Samples/ms-identity-javascript-angular-tutorial/tree/main/3-Authorization-II/2-call-api-b2c)<br/>&#8226; [Ruft Microsoft Graph über OBO auf](https://github.com/Azure-Samples/ms-identity-javascript-angular-tutorial/tree/main/7-AdvancedScenarios/1-call-api-obo/README.md)<br/>&#8226; [Ruft die .NET Core-Web-API mittels PoP auf](https://github.com/Azure-Samples/ms-identity-javascript-angular-tutorial/tree/main/7-AdvancedScenarios/2-call-api-pop/README.md)<br/>&#8226; [Verwendet App-Rollen für die Zugriffssteuerung](https://github.com/Azure-Samples/ms-identity-javascript-angular-tutorial/tree/main/5-AccessControl/1-call-api-roles/README.md)<br/>&#8226; [Verwendet Sicherheitsgruppen für die Zugriffssteuerung](https://github.com/Azure-Samples/ms-identity-javascript-angular-tutorial/tree/main/5-AccessControl/2-call-api-groups/README.md)<br/>&#8226; [Führt Bereitstellung in Azure Storage und App Service durch](https://github.com/Azure-Samples/ms-identity-javascript-angular-tutorial/tree/main/4-Deployment/README.md)| MSAL Angular | &#8226; Autorisierungscodeflow (mit PKCE)<br/>&#8226; „Im Auftrag von“-Flow (OBO, On-behalf-of)<br/>&#8226; Eigentumsnachweis (PoP, Proof of Possession)|
> | Blazor WebAssembly | [GitHub-Repository](https://github.com/Azure-Samples/ms-identity-blazor-wasm) | &#8226; Meldet Benutzer an<br/>&#8226; Ruft Microsoft Graph auf. | MSAL.js | Autorisierungscodeflow (mit PKCE) |
> | JavaScript | [GitHub-Repository](https://github.com/Azure-Samples/ms-identity-javascript-v2) | &#8226; Meldet Benutzer an<br/>&#8226; Ruft Microsoft Graph auf. | MSAL.js | Autorisierungscodeflow (mit PKCE) |
> | JavaScript | [GitHub-Repository](https://github.com/Azure-Samples/ms-identity-b2c-javascript-spa) | &#8226; Meldet Benutzer an (B2C)<br/>&#8226; Ruft die Node.js-Web-API auf |  MSAL.js | Autorisierungscodeflow (mit PKCE) |
> | JavaScript | [GitHub-Repository](https://github.com/Azure-Samples/ms-identity-javascript-tutorial) | &#8226; [Meldet Benutzer an](https://github.com/Azure-Samples/ms-identity-javascript-tutorial/tree/main/1-Authentication/1-sign-in/README.md)<br/>&#8226; [Meldet Benutzer an (B2C)](https://github.com/Azure-Samples/ms-identity-javascript-tutorial/tree/main/1-Authentication/2-sign-in-b2c/README.md) <br/> &#8226; [ Ruft Microsoft Graph auf](https://github.com/Azure-Samples/ms-identity-javascript-tutorial/tree/main/2-Authorization-I/1-call-graph/README.md)<br/>&#8226; [Ruft die Node.js-Web-API auf](https://github.com/Azure-Samples/ms-identity-javascript-tutorial/tree/main/3-Authorization-II/1-call-api/README.md)<br/>&#8226; [Ruft die Node.js-Web-API auf (B2C)](https://github.com/Azure-Samples/ms-identity-javascript-tutorial/tree/main/3-Authorization-II/2-call-api-b2c/README.md)<br/>&#8226; [Ruft Microsoft Graph über OBO auf](https://github.com/Azure-Samples/ms-identity-javascript-tutorial/tree/main/4-AdvancedGrants/1-call-api-graph/README.md)<br/>&#8226; [Ruft die Node.js-Web-API über OBO und bedingten Zugriff auf](https://github.com/Azure-Samples/ms-identity-javascript-tutorial/tree/main/4-AdvancedGrants/2-call-api-api-ca/README.md)<br/>&#8226; [Führt Bereitstellung in Azure Storage und App Service durch](https://github.com/Azure-Samples/ms-identity-javascript-tutorial/tree/main/5-Deployment/README.md)| MSAL.js | &#8226; Autorisierungscodeflow (mit PKCE)<br/>&#8226; „Im Auftrag von“-Flow (OBO, On-behalf-of)<br/>&#8226; Bedingter Zugriff (CA, Conditional Access) |
> | React | [GitHub-Repository](https://github.com/Azure-Samples/ms-identity-javascript-react-spa) | &#8226; Meldet Benutzer an<br/>&#8226; Ruft Microsoft Graph auf. | MSAL React | Autorisierungscodeflow (mit PKCE) |
> | React | [GitHub-Repository](https://github.com/Azure-Samples/ms-identity-javascript-react-tutorial) | &#8226; [Meldet Benutzer an](https://github.com/Azure-Samples/ms-identity-javascript-react-tutorial/tree/main/1-Authentication/1-sign-in/README.md)<br/>&#8226; [Meldet Benutzer an (B2C)](https://github.com/Azure-Samples/ms-identity-javascript-react-tutorial/tree/main/1-Authentication/2-sign-in-b2c/README.md) <br/> &#8226; [ Ruft Microsoft Graph auf](https://github.com/Azure-Samples/ms-identity-javascript-react-tutorial/tree/main/2-Authorization-I/1-call-graph/README.md)<br/>&#8226; [Ruft die Node.js-Web-API auf](https://github.com/Azure-Samples/ms-identity-javascript-react-tutorial/tree/main/3-Authorization-II/1-call-api)<br/>&#8226; [Ruft die Node.js-Web-API auf (B2C)](https://github.com/Azure-Samples/ms-identity-javascript-react-tutorial/tree/main/3-Authorization-II/2-call-api-b2c)<br/>&#8226; [Ruft Microsoft Graph über OBO auf](https://github.com/Azure-Samples/ms-identity-javascript-react-tutorial/tree/main/6-AdvancedScenarios/1-call-api-obo/README.md)<br/>&#8226; [Ruft die Node.js-Web-API per PoP auf](https://github.com/Azure-Samples/ms-identity-javascript-react-tutorial/tree/main/6-AdvancedScenarios/2-call-api-pop/README.md)<br/>&#8226; [Verwendet App-Rollen für die Zugriffssteuerung](https://github.com/Azure-Samples/ms-identity-javascript-react-tutorial/tree/main/5-AccessControl/1-call-api-roles/README.md)<br/>&#8226; [Verwendet Sicherheitsgruppen für die Zugriffssteuerung](https://github.com/Azure-Samples/ms-identity-javascript-react-tutorial/tree/main/5-AccessControl/2-call-api-groups/README.md)<br/>&#8226; [Führt Bereitstellung in Azure Storage und App Service durch](https://github.com/Azure-Samples/ms-identity-javascript-react-tutorial/tree/main/4-Deployment/1-deploy-storage/README.md)<br/>&#8226; [Führt Bereitstellung in Azure Static Web Apps durch](https://github.com/Azure-Samples/ms-identity-javascript-react-tutorial/tree/main/4-Deployment/2-deploy-static/README.md)| MSAL React | &#8226; Autorisierungscodeflow (mit PKCE)<br/>&#8226; „Im Auftrag von“-Flow (OBO, On-behalf-of)<br/>&#8226; Bedingter Zugriff (CA, Conditional Access)<br/>&#8226; Eigentumsnachweis (PoP, Proof of Possession) |

## <a name="web-applications"></a>Webanwendungen

Die folgenden Beispiele zeigen Webanwendungen, die Benutzer anmelden. Einige Beispiele zeigen außerdem, wie die Anwendung Microsoft Graph oder Ihre eigene Web-API mit der Identität des Benutzers aufruft.

| Plattform | Nur Benutzeranmeldung | Benutzeranmeldung und Aufruf von Microsoft Graph |
| -------- | ------------------- | --------------------------------- |
| ![Diese Abbildung zeigt das ASP.NET Core-Logo.](media/sample-v2-code/logo_NETcore.png)</p>ASP.NET Core | [Benutzertutorial für die ASP.NET Core-Web-App-Anmeldung](https://aka.ms/aspnetcore-webapp-sign-in) | Das gleiche Beispiel im Schritt [ASP.NET Core-Web-App-Aufrufe an Microsoft Graph](https://aka.ms/aspnetcore-webapp-call-msgraph)</p>Erweitertes Beispiel [Zugreifen auf den Tokencache des angemeldeten Benutzers über Hintergrund-Apps, APIs und Dienste](https://github.com/Azure-Samples/ms-identity-dotnet-advanced-token-cache) |
| ![Diese Abbildung zeigt das ASP.NET Framework-Logo.](media/sample-v2-code/logo_NETframework.png)</p>ASP.NET Core | Im [Playbook für Entwickler zur Migration von Anwendungen von AD FS zu Azure AD](https://github.com/Azure-Samples/ms-identity-dotnet-adfs-to-aad) erfahren Sie, wie Sie Ihre in Active Directory-Verbunddienste (AD FS) integrierten Anwendungen problemlos und sicher zu Azure Active Directory (Azure AD) migrieren können. | |
| ![Diese Abbildung zeigt das ASP.NET Framework-Logo.](media/sample-v2-code/logo_NETframework.png)</p> ASP.NET | [Schnellstart für ASP.NET](https://github.com/AzureAdQuickstarts/AppModelv2-WebApp-OpenIDConnect-DotNet) </p> [dotnet-webapp-openidconnect-v2](https://github.com/azure-samples/active-directory-dotnet-webapp-openidconnect-v2)  |  [dotnet-admin-restricted-scopes-v2](https://github.com/azure-samples/active-directory-dotnet-admin-restricted-scopes-v2) </p> [msgraph-training-aspnetmvcapp](https://github.com/microsoftgraph/msgraph-training-aspnetmvcapp) |
| ![Diese Abbildung zeigt das Java-Logo.](media/sample-v2-code/logo_java.png)  |[Java-Servlet-Tutorial: Kapitel 1.1](https://github.com/Azure-Samples/ms-identity-java-servlet-webapp-authentication/blob/main/README.md) Anmelden mit AAD| |
| ![Diese Abbildung zeigt das Java-Logo.](media/sample-v2-code/logo_java.png)  |[Java-Servlet-Tutorial: Kapitel 1.2](https://github.com/Azure-Samples/ms-identity-java-servlet-webapp-authentication/blob/main/README.md) Anmelden mit B2C |
| ![Diese Abbildung zeigt das Java-Logo.](media/sample-v2-code/logo_java.png)  | | [Java-Servlet-Tutorial: Kapitel 2.1](https://github.com/Azure-Samples/ms-identity-java-servlet-webapp-authentication/blob/main/README.md) Anmelden mit AAD und Aufrufen von Graph|
| ![Diese Abbildung zeigt das Java-Logo.](media/sample-v2-code/logo_java.png)  |[Java-Servlet-Tutorial: Kapitel 3.1](https://github.com/Azure-Samples/ms-identity-java-servlet-webapp-authentication/blob/main/README.md) Anmelden mit AAD und Steuern des Zugriffs mit Rollenanspruch| |
| ![Diese Abbildung zeigt das Java-Logo.](media/sample-v2-code/logo_java.png)  | | [Java-Servlet-Tutorial: Kapitel 3.2](https://github.com/Azure-Samples/ms-identity-java-servlet-webapp-authentication/blob/main/README.md) Anmelden mit AAD und Steuern des Zugriffs mit Gruppenanspruch|
| ![Diese Abbildung zeigt das Java-Logo.](media/sample-v2-code/logo_java.png) | |[Java-Servlet-Tutorial: Kapitel 4.1](https://github.com/Azure-Samples/ms-identity-java-servlet-webapp-authentication/blob/main/README.md) Bereitstellen in Azure App Service|
| ![Diese Abbildung zeigt das Java-Logo.](media/sample-v2-code/logo_java.png)  | | [ms-identity-java-webapp](https://github.com/Azure-Samples/ms-identity-java-webapp) |
| ![Diese Abbildung zeigt das Java-Logo.](media/sample-v2-code/logo_java.png)  | [ms-identity-b2c-java-servlet-webapp-authentication](https://github.com/Azure-Samples/ms-identity-b2c-java-servlet-webapp-authentication)|  |
| ![Diese Abbildung zeigt das Node.js-Logo.](media/sample-v2-code/logo_nodejs.png)</p>Node.js (MSAL Node) | [Tutorial zum Anmelden von Benutzern durch eine Express-Web-App](https://github.com/Azure-Samples/ms-identity-node) | |
| ![Diese Abbildung zeigt das Python-Logo.](media/sample-v2-code/logo_python.png)  | [Python Flask-Tutorial: Kapitel 1.1](https://github.com/Azure-Samples/ms-identity-python-flask-tutorial/blob/main/README.md) Anmelden mit AAD  |  |
| ![Diese Abbildung zeigt das Python-Logo.](media/sample-v2-code/logo_python.png)  | [Python Flask-Tutorial: Kapitel 1.2](https://github.com/Azure-Samples/ms-identity-python-flask-tutorial/blob/main/README.md) Anmelden mit B2C                    |  |
| ![Diese Abbildung zeigt das Python-Logo.](media/sample-v2-code/logo_python.png)  | | [Python Flask-Tutorial: Kapitel 2.1](https://github.com/Azure-Samples/ms-identity-python-flask-tutorial/blob/main/README.md) Anmelden mit AAD und Aufrufen von Graph |
| ![Diese Abbildung zeigt das Python-Logo.](media/sample-v2-code/logo_python.png)  | |[Python Flask-Tutorial: Kapitel 3.1](https://github.com/Azure-Samples/ms-identity-python-flask-tutorial/blob/main/README.md) Bereitstellen in Azure App Service  |
| ![Diese Abbildung zeigt das Python-Logo.](media/sample-v2-code/logo_python.png)  | [Python-Django-Tutorial: Kapitel 1.1](https://github.com/Azure-Samples/ms-identity-python-django-tutorial/blob/main/README.md) Anmelden mit AAD  | |
| ![Diese Abbildung zeigt das Python-Logo.](media/sample-v2-code/logo_python.png)  | [Python-Django-Tutorial: Kapitel 1.2](https://github.com/Azure-Samples/ms-identity-python-django-tutorial/blob/main/README.md) Anmelden mit B2C                    |  |
| ![Diese Abbildung zeigt das Python-Logo.](media/sample-v2-code/logo_python.png)  | | [Python-Django-Tutorial: Kapitel 2.1](https://github.com/Azure-Samples/ms-identity-python-django-tutorial/blob/main/README.md) Anmelden mit AAD und Aufrufen von Graph|
| ![Diese Abbildung zeigt das Python-Logo.](media/sample-v2-code/logo_python.png)  | | [Python-Django-Tutorial: Kapitel 3.1](https://github.com/Azure-Samples/ms-identity-python-django-tutorial/blob/main/README.md) Bereitstellen in Azure App Service                    |
| ![Diese Abbildung zeigt das Python-Logo.](media/sample-v2-code/logo_python.png)  | | [Python Flask-Web-App](https://github.com/Azure-Samples/ms-identity-python-webapp) |
| ![Diese Abbildung zeigt das Ruby-Logo.](media/sample-v2-code/logo_ruby.png) |                   | [msgraph-training-rubyrailsapp](https://github.com/microsoftgraph/msgraph-training-rubyrailsapp) |
| ![Diese Abbildung zeigt das Blazor-Logo.](media/sample-v2-code/logo-blazor.png)</p>Blazor Server | [Tutorial zum Anmelden von Benutzern durch eine Blazor Server-App](https://github.com/Azure-Samples/ms-identity-blazor-server/tree/main/WebApp-OIDC) | [Aufruf von Microsoft Graph durch die Blazor Server-App](https://github.com/Azure-Samples/ms-identity-blazor-server/tree/main/WebApp-graph-user/Call-MSGraph)</p>Chapterwise-Tutorial: [Blazor Server-App zum Anmelden von Benutzern und Aufrufen von APIs mit Azure Active Directory](https://github.com/Azure-Samples/ms-identity-blazor-server) |

## <a name="desktop-and-mobile-public-client-apps"></a>Öffentliche Desktop- und mobile Client-Apps

In den folgenden Beispielen werden öffentliche Clientanwendungen (Desktop-/mobile Anwendungen) veranschaulicht, die im Namen eines Benutzers auf die Microsoft Graph-API oder Ihre eigene Web-API zugreifen. Mit Ausnahme des Beispiels *Desktop (Konsole) mit WAM* verwenden alle diese Clientanwendungen die Microsoft-Authentifizierungsbibliothek (Microsoft Authentication Library, MSAL).

| Clientanwendung | Plattform | Flow/Zuweisung | Aufrufen von Microsoft Graph | Aufrufen einer ASP.NET Core-Web-API |
| ------------------ | -------- |  ----------| ---------- | ------------------------- |
| Desktop-Tutorial (.NET Core): Optionale Verwendung von:</p>– plattformübergreifendes Tokencache</p>– benutzerdefinierte Web-Benutzeroberfläche | ![Diese Abbildung zeigt das .NET/C#-Logo.](media/sample-v2-code/logo_NETcore.png) | [Autorisierungscode](msal-authentication-flows.md#authorization-code)| [ms-identity-dotnet-desktop-tutorial](https://github.com/azure-samples/ms-identity-dotnet-desktop-tutorial) | |
| Desktop (WPF)      | ![Diese Abbildung zeigt das .NET-Desktop/C#-Logo](media/sample-v2-code/logo_NET.png) | [Autorisierungscode](msal-authentication-flows.md#authorization-code)| [dotnet-desktop-msgraph-v2](https://github.com/azure-samples/active-directory-dotnet-desktop-msgraph-v2) | [dotnet-native-aspnetcore-v2](https://aka.ms/msidentity-aspnetcore-webapi) |
| Desktop (Konsole)   | ![Diese Abbildung zeigt das .NET/C#-Logo (Desktop)](media/sample-v2-code/logo_NET.png) | [Integrierte Windows-Authentifizierung](msal-authentication-flows.md#integrated-windows-authentication) | [dotnet-iwa-v2](https://github.com/azure-samples/active-directory-dotnet-iwa-v2) |  |
| Desktop (Konsole) | ![Diese Abbildung zeigt das .NET/C#-Logo (Desktop)](media/sample-v2-code/logo_NET.png) |  [Autorisierungscode](msal-authentication-flows.md#authorization-code) | [active-directory-dotnetcore-daemon-v2](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2/tree/master/1-Call-MSGraph) |[active-directory-dotnetcore-daemon-v2](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2/blob/master/2-Call-OwnApi/README.md)  |
| Desktop (Konsole) <br> Verwenden von Zertifikaten anstelle von Geheimnissen | ![Diese Abbildung zeigt das .NET/C#-Logo (Desktop)](media/sample-v2-code/logo_NET.png) |  [Autorisierungscode](msal-authentication-flows.md#authorization-code) | [active-directory-dotnetcore-daemon-v2](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2/tree/master/1-Call-MSGraph#variation-daemon-application-using-client-credentials-with-certificates) |[active-directory-dotnetcore-daemon-v2](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2/tree/master/2-Call-OwnApi#variation-daemon-application-using-client-credentials-with-certificates)  |
| Desktop (Konsole)   | ![Diese Abbildung zeigt das Java-Logo.](media/sample-v2-code/logo_java.png) | [Integrierte Windows-Authentifizierung](msal-authentication-flows.md#integrated-windows-authentication) |[ms-identity-java-desktop](https://github.com/Azure-Samples/ms-identity-java-desktop/) |  |
| Desktop (Konsole)   | ![Dies ist das .NET/C#-Logo (Desktop)](media/sample-v2-code/logo_NETcore.png) | [Benutzername/Kennwort](msal-authentication-flows.md#usernamepassword) |[dotnetcore-up-v2](https://github.com/azure-samples/active-directory-dotnetcore-console-up-v2) |  |
| Desktop (Konsole) mit WAM  | ![Dies ist das Logo für .NET/C# (Desktop)](media/sample-v2-code/logo_NETcore.png) | Interaktiv mit [Web Account Manager](/windows/uwp/security/web-account-manager) (WAM) |[dotnet-native-uwp-wam](https://github.com/azure-samples/active-directory-dotnet-native-uwp-wam) |  |
| Desktop (Konsole)   | ![Diese Abbildung zeigt das Java-Logo.](media/sample-v2-code/logo_java.png) | [Benutzername/Kennwort](msal-authentication-flows.md#usernamepassword) |[ms-identity-java-desktop](https://github.com/Azure-Samples/ms-identity-java-desktop/) |  |
| Desktop (Konsole)   | ![Diese Abbildung zeigt das Python-Logo.](media/sample-v2-code/logo_python.png) | [Benutzername/Kennwort](msal-authentication-flows.md#usernamepassword) |[ms-identity-python-desktop](https://github.com/Azure-Samples/ms-identity-python-desktop) |  |
| Desktop (Electron)   | ![Diese Abbildung zeigt das Node.js-Logo.](media/sample-v2-code/logo_nodejs.png)</p>Node.js (MSAL Node) | [Autorisierungscodeflow (PKCE)](msal-authentication-flows.md#authorization-code) |[ms-identity-javascript-nodejs-desktop](https://github.com/Azure-Samples/ms-identity-javascript-nodejs-desktop) |  |
| Mobil (Android, iOS, UWP)   | ![Diese Abbildung zeigt das .NET/C#-Logo (Xamarin).](media/sample-v2-code/logo_xamarin.png) | [Autorisierungscode](msal-authentication-flows.md#authorization-code) |[xamarin-native-v2](https://github.com/azure-samples/active-directory-xamarin-native-v2) |  |
| Mobil (iOS)       | ![Diese Abbildung zeigt das iOS-/Objective-C- oder Swift-Logo.](media/sample-v2-code/logo_iOS.png) | [Autorisierungscode](msal-authentication-flows.md#authorization-code) |[ios-swift-objc-native-v2](https://github.com/azure-samples/active-directory-ios-swift-native-v2) </p> [ios-native-nxoauth2-v2](https://github.com/azure-samples/active-directory-ios-native-nxoauth2-v2) |  |
| Desktop (macOS)       | macOS | [Autorisierungscode](msal-authentication-flows.md#authorization-code) |[macOS-swift-objc-native-v2](https://github.com/Azure-Samples/ms-identity-macOS-swift-objc) |  |
| Mobil (Android-Java)   | ![Diese Abbildung zeigt das Android-Logo.](media/sample-v2-code/logo_Android.png) | [Autorisierungscode](msal-authentication-flows.md#authorization-code) |  [android-Java](https://github.com/Azure-Samples/ms-identity-android-java) |  |
| Mobil (Android-Kotlin)   | ![Diese Abbildung zeigt das Android-Logo.](media/sample-v2-code/logo_Android.png) | [Autorisierungscode](msal-authentication-flows.md#authorization-code) |  [android-Kotlin](https://github.com/Azure-Samples/ms-identity-android-kotlin) |  |

## <a name="daemon-applications"></a>Daemonanwendungen

Die folgenden Beispiele zeigen eine Anwendung, die auf die Microsoft Graph-API mit ihrer eigenen Identität (ohne Benutzer) zugreift.

| Clientanwendung | Plattform | Ablauf/Erteilung | Aufrufen von Microsoft Graph |
| ------------------ | -------- | ---------- | -------------------- |
| Konsole | ![Diese Abbildung zeigt das .NET Core-Logo.](media/sample-v2-code/logo_NETcore.png)</p> ASP.NET  | [Clientanmeldeinformationen](msal-authentication-flows.md#client-credentials) | [dotnetcore-daemon-v2](https://github.com/azure-samples/active-directory-dotnetcore-daemon-v2) |
| Web-App | ![Screenshot mit dem ASP.NET-Logo](media/sample-v2-code/logo_NETframework.png)</p> ASP.NET  | [Clientanmeldeinformationen](msal-authentication-flows.md#client-credentials) | [dotnet-daemon-v2](https://github.com/azure-samples/active-directory-dotnet-daemon-v2) |
| Konsole | ![Diese Abbildung zeigt das Java-Logo.](media/sample-v2-code/logo_java.png) | [Clientanmeldeinformationen](msal-authentication-flows.md#client-credentials) | [ms-identity-java-daemon](https://github.com/Azure-Samples/ms-identity-java-daemon) |
| Konsole | ![Diese Abbildung zeigt das Node.js-Logo.](media/sample-v2-code/logo_nodejs.png)</p>Node.js (MSAL Node)| [Clientanmeldeinformationen](msal-authentication-flows.md#client-credentials) | [ms-identity-javascript-nodejs-console](https://github.com/Azure-Samples/ms-identity-javascript-nodejs-console) |
| Konsole | ![Diese Abbildung zeigt das Python-Logo.](media/sample-v2-code/logo_python.png) | [Clientanmeldeinformationen](msal-authentication-flows.md#client-credentials) | [ms-identity-python-daemon](https://github.com/Azure-Samples/ms-identity-python-daemon) |

## <a name="headless-applications"></a>Monitorlose Anwendungen

In den folgenden Beispielen wird eine öffentliche Clientanwendung veranschaulicht, die auf einem Gerät ohne Webbrowser ausgeführt wird. Diese App kann ein Befehlszeilentool sein oder unter Linux oder Mac ausgeführt werden, oder es kann sich um eine IoT-Anwendung handeln. In den Beispielen wird eine App verwendet, die im Namen eines Benutzers, der sich interaktiv an einem anderen Gerät anmeldet (z. B. einem Mobiltelefon), auf die Microsoft Graph-API zugreift. Diese Clientanwendung verwendet die Microsoft-Authentifizierungsbibliothek (Microsoft Authentication Library, MSAL).

| Clientanwendung | Plattform | Ablauf/Erteilung | Aufrufen von Microsoft Graph |
| ------------------ | -------- |  ----------| ---------- |
| Desktop (Konsole)   | ![Diese Abbildung zeigt das .NET/C#-Logo (Desktop).](media/sample-v2-code/logo_NETcore.png) | [Gerätecodeflow](msal-authentication-flows.md#device-code) |[dotnetcore-devicecodeflow-v2](https://github.com/azure-samples/active-directory-dotnetcore-devicecodeflow-v2) |
| Desktop (Konsole)   | ![Diese Abbildung zeigt das Java-Logo.](media/sample-v2-code/logo_java.png) | [Gerätecodeflow](msal-authentication-flows.md#device-code) |[ms-identity-java-devicecodeflow](https://github.com/Azure-Samples/ms-identity-java-devicecodeflow) |
| Desktop (Konsole)   | ![Diese Abbildung zeigt das Python-Logo.](media/sample-v2-code/logo_python.png) | [Gerätecodeflow](msal-authentication-flows.md#device-code) |[ms-identity-python-devicecodeflow](https://github.com/Azure-Samples/ms-identity-python-devicecodeflow) |

## <a name="multi-tenant-saas-applications"></a>SaaS-Anwendungen mit mehreren Mandanten

In den folgenden Beispielen wird gezeigt, wie Sie Ihre Anwendung so konfigurieren, dass Anmeldungen von einem beliebigen Azure AD-Mandanten (Azure Active Directory) akzeptiert werden. Wenn Sie Ihre Anwendung als *mehrinstanzenfähig* konfigurieren, bedeutet das, dass Sie eine **SaaS-Anwendung (Software-as-a-Service)** für viele Organisationen anbieten können. Die Benutzer können sich dann nach Erteilung der Einwilligung bei Ihrer Anwendung anmelden.

| Plattform | Beschreibung | Link |
| -------- | --------------------- | -------- |
| ![Diese Abbildung zeigt das Angular-Logo.](media/sample-v2-code/logo_angular.png) [Angular (MSAL Angular 2.0)](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular) | Mehrinstanzenfähige SPA ruft mehrinstanzenfähige benutzerdefinierte Web-API auf. |[ms-identity-javascript-angular-spa-aspnet-webapi-multitenant](https://github.com/Azure-Samples/ms-identity-javascript-angular-spa-aspnet-webapi-multitenant/tree/master/Chapter2) |
| ![Diese Abbildung zeigt das ASP.NET Core-Logo.](media/sample-v2-code/logo_NETcore.png) [.NET Core (MSAL.NET)](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) | ASP.NET Core-MVC-Webanwendung ruft Graph-API auf. |[active-directory-aspnetcore-webapp-openidconnect-v2](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/2-WebApp-graph-user/2-3-Multi-Tenant) |
| ![Diese Abbildung zeigt das ASP.NET Core-Logo.](media/sample-v2-code/logo_NETcore.png) [.NET Core (MSAL.NET)](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) | ASP.NET Core-MVC-Webanwendung ruft ASP.NET Core-Web-API auf. |[active-directory-aspnetcore-webapp-openidconnect-v2](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/4-WebApp-your-API/4-3-AnyOrg) |

## <a name="web-apis"></a>Web-APIs

Die folgenden Beispiele veranschaulichen, wie Sie eine Web-API mit Microsoft Identity Platform schützen und wie Sie eine nachgeschaltete API von der Web-API aus aufrufen.

| Plattform | Beispiel |
| -------- | ------------------- |
| ![Diese Abbildung zeigt das ASP.NET Core-Logo.](media/sample-v2-code/logo_NETcore.png)</p>ASP.NET Core | ASP.NET Core-Web-API (Dienst) von [dotnet-native-aspnetcore-v2](https://aka.ms/msidentity-aspnetcore-webapi-calls-msgraph)  |
| ![Diese Abbildung zeigt das ASP.NET-Logo.](media/sample-v2-code/logo_NET.png)</p>ASP.NET MVC | Web-API (Dienst) von [ms-identity-aspnet-webapi-onbehalfof](https://github.com/Azure-Samples/ms-identity-aspnet-webapi-onbehalfof) |
| ![Diese Abbildung zeigt das Java-Logo.](media/sample-v2-code/logo_java.png) | Web-API (Dienst) von [ms-identity-java-webapi](https://github.com/Azure-Samples/ms-identity-java-webapi) |
| ![Diese Abbildung zeigt das Node.js-Logo.](media/sample-v2-code/logo_nodejs.png)</p>Node.js (Passport.js)| Web-API (Dienst) von [active-directory-javascript-nodejs-webapi-v2](https://github.com/Azure-Samples/active-directory-javascript-nodejs-webapi-v2) |
| ![Diese Abbildung zeigt das Node.js-Logo.](media/sample-v2-code/logo_nodejs.png)</p>Node.js (Passport.js)| B2C-Web-API (Dienst) von [active-directory-b2c-javascript-nodejs-webapi](https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi) |

## <a name="azure-functions-as-web-apis"></a>Azure Functions als Web-APIs

Die folgenden Beispiele veranschaulichen, wie Sie mit „HttpTrigger“ eine Azure-Funktion schützen, mit Microsoft Identity Platform eine Web-API verfügbar machen und über die Web-API eine nachgeschaltete API aufrufen.

| Plattform | Beispiel |
| -------- | ------------------- |
| ![Diese Abbildung zeigt das ASP.NET Core-Logo.](media/sample-v2-code/logo_NETcore.png)</p>ASP.NET Core | ASP.NET Core-Web-API (Dienst)-Azure-Funktion von [dotnet-native-aspnetcore-v2](https://github.com/Azure-Samples/ms-identity-dotnet-webapi-azurefunctions)  |
| ![Diese Abbildung zeigt das Python-Logo.](media/sample-v2-code/logo_python.png)</p>Python | Web-API (Dienst) von [Python](https://github.com/Azure-Samples/ms-identity-python-webapi-azurefunctions) |
| ![Diese Abbildung zeigt das Node.js-Logo.](media/sample-v2-code/logo_nodejs.png)</p>Node.js (Passport.js)| Web-API (Dienst) von [Node.js und Passport-Azure-AD](https://github.com/Azure-Samples/ms-identity-nodejs-webapi-azurefunctions) |
| ![Diese Abbildung zeigt das Node.js-Logo.](media/sample-v2-code/logo_nodejs.png)</p>Node.js (Passport.js)| Web-API (Dienst) von [Node.js und Passport-Azure-AD mithilfe von OnBehalfOf](https://github.com/Azure-Samples/ms-identity-nodejs-webapi-onbehalfof-azurefunctions) |

## <a name="other-microsoft-graph-samples"></a>Weitere Microsoft Graph-Beispiele

[Beispiele](https://github.com/microsoftgraph/msgraph-community-samples/tree/master/samples#aspnet) und Tutorials, die verschiedene Verwendungsmuster für die Microsoft Graph-API (einschließlich Authentifizierung mit Azure AD) veranschaulichen, finden Sie unter [Beispiele und Tutorials der Microsoft Graph-Community](https://github.com/microsoftgraph/msgraph-community-samples).

## <a name="see-also"></a>Weitere Informationen

[Microsoft Graph-API – Konzepte und Referenz](/graph/use-the-api?context=graph%2fapi%2fbeta&view=graph-rest-beta&preserve-view=true)
