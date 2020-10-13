---
title: Übersicht über die Microsoft Identity Web-Authentifizierungsbibliothek
titleSuffix: Microsoft identity platform
description: Erfahren Sie mehr über Microsoft Identity Web, eine Authentifizierungs- und Autorisierungsbibliothek für ASP.NET Core-Anwendungen, die in Azure Active Directory, Azure AD B2C, Microsoft Graph und andere Web-APIs integriert werden können.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 10/09/2020
ms.author: jmprieur
ms.reviewer: marsma
ms.custom: devx-track-csharp, aaddev
ms.openlocfilehash: 4d915cee962546e355ee9d53c683581730bbbf36
ms.sourcegitcommit: ef69245ca06aa16775d4232b790b142b53a0c248
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/06/2020
ms.locfileid: "91778831"
---
# <a name="microsoft-identity-web-authentication-library"></a>Microsoft Identity Web-Authentifizierungsbibliothek

Microsoft Identity Web ist ein Satz von ASP.NET Core-Bibliotheken, der das Hinzufügen von Authentifizierung und Autorisierung zu Web-Apps und Web-APIs vereinfacht, die in die Microsoft Identity-Plattform integriert sind. Er bietet eine praktische API-Schicht mit einer einzelnen Benutzeroberfläche, in der ASP.NET Core, seine Authentifizierungsmiddleware und die [Microsoft-Authentifizierungsbibliothek (Microsoft Authentication Library, MSAL) für .NET](https://github.com/azuread/microsoft-authentication-library-for-dotnet) zusammengefasst werden.

## <a name="supported-application-scenarios"></a>Unterstützte Anwendungsszenarien

Wenn Sie ASP.NET Core-Web-Apps oder Web-APIs entwickeln und Azure Active Directory (Azure AD) oder Azure AD B2C für die Identitäts- und Zugriffsverwaltung (Identity and Access Management, IAM) verwenden möchten, empfehlen wir die Verwendung von Microsoft Identity Web für alle folgenden Szenarien:

- [Web-App, die Benutzer anmeldet](scenario-web-app-sign-user-overview.md)
- [Web-App, die einen Benutzer anmeldet und eine Web-API im Namen des Benutzers aufruft](scenario-web-app-call-api-overview.md)
- [Geschützte Web-API, auf die nur authentifizierte Benutzer zugreifen können](scenario-protected-web-api-overview.md)
- [Geschützte Web-API, die im Namen des angemeldeten Benutzers eine andere (Downstream-)Web-API aufruft](scenario-web-api-call-api-overview.md)

## <a name="get-the-library"></a>Abrufen der Bibliothek

Sie können Microsoft Identity Web von [NuGet](#nuget), [.NET Core-Projektvorlagen](#project-templates) und [GitHub](#github) erhalten.

#### <a name="nuget"></a>NuGet

Microsoft Identity Web ist auf NuGet als Satz von Paketen verfügbar, die modulare Funktionen basierend auf den Anforderungen Ihrer App bereitstellen. Verwenden Sie den Befehl `dotnet add` der .NET-CLI oder den **NuGet-Paket-Manager** von Visual Studio, um die für Ihr Projekt geeigneten Pakete zu installieren:

- [Microsoft.Identity.Web](https://www.nuget.org/packages/Microsoft.Identity.Web) – das Hauptpaket. Erforderlich für alle Apps, die Microsoft Identity Web verwenden.
- [Microsoft.Identity.Web.UI](https://www.nuget.org/packages/Microsoft.Identity.Web.UI) – optional. Fügt eine Benutzeroberfläche für die Anmeldung und Abmeldung von Benutzern und einen zugeordneten Controller für Web-Apps hinzu.
- [Microsoft.Identity.Web.MicrosoftGraph](https://www.nuget.org/packages/Microsoft.Identity.Web.MicrosoftGraph) – optional. Bietet eine vereinfachte Interaktion mit der Microsoft Graph-API.

#### <a name="project-templates"></a>Projektvorlagen

Microsoft Identity-Web-Projektvorlagen sind in .NET 5.0 enthalten und können für ASP.NET Core 3.1-Projekte heruntergeladen werden.

Wenn Sie ASP.NET Core 3.1 verwenden, installieren Sie die Vorlagen mit der .NET-CLI:

```dotnetcli
dotnet new --install Microsoft.Identity.Web.ProjectTemplates::1.0.0
```

Das folgende Diagramm zeigt eine allgemeine Übersicht über die unterstützten App-Typen und ihre relevanten Argumente:

:::image type="content" source="media/microsoft-identity-web-overview/diagram-microsoft-identity-web-templates.png" lightbox="media/microsoft-identity-web-overview/diagram-microsoft-identity-web-templates.png" alt-text="Diagramm der verfügbaren Dotnet-CLI-Projektvorlagen für Microsoft Identity Web&quot;:::
<br /><sup><b>*</b></sup> `MultiOrg` wird nicht mit `webapi2` unterstützt, kann jedoch in *appsettings.json* aktiviert werden, indem der Mandant auf `common` oder `organizations` festgelegt wird.
<br /><sup><b>**</b></sup> `--calls-graph` wird für Azure AD B2C nicht unterstützt.

Dieser .NET-CLI-Beispielbefehl aus unserem [Blazor Server-Tutorial](tutorial-blazor-server.md) generiert ein neues Blazor Server Projekt, das die richtigen Pakete und den richtigen Startcode enthält (Platzhalterwerte werden angezeigt):

```dotnetcli
dotnet new blazorserver2 --auth SingleOrg --calls-graph --client-id &quot;00000000-0000-0000-0000-000000000000&quot; --tenant-id &quot;11111111-1111-1111-1111-111111111111" --output my-blazor-app
```

#### <a name="github"></a>GitHub

Microsoft Identity Web ist ein Open-Source-Projekt, das auf GitHub gehostet wird: [AzureAD/microsoft-identity-web](https://github.com/AzureAD/microsoft-identity-web?azure-portal=true)

Das [Repository-Wiki](https://github.com/AzureAD/microsoft-identity-web/wiki) enthält zusätzliche Dokumentation. Wenn Sie Hilfe benötigen oder einen Fehler entdecken, können Sie ein [Problem melden](https://github.com/AzureAD/microsoft-identity-web/issues).

## <a name="features"></a>Features

Microsoft Identity Web enthält mehrere Features, die nicht bereitgestellt werden, wenn Sie die standardmäßigen ASP.NET 3.1-Projektvorlagen verwenden.

| Funktion                                                                                  | ASP.NET Core 3.1                                                     | Microsoft Identity-Web                                                                                  |
|------------------------------------------------------------------------------------------|----------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|
| [Anmelden von Benutzern](scenario-web-app-sign-user-app-configuration.md) in Web-Apps             | <li>Geschäfts-, Schul- oder Unikonten<li>Identitäten in sozialen Netzwerken (mit Azure AD B2C) | <li>Geschäfts-, Schul- oder Unikonten<li>Persönliche Microsoft-Konten<li>Identitäten in sozialen Netzwerken (mit Azure AD B2C)     |
| [Schützen von Web-APIs](scenario-protected-web-api-app-configuration.md#microsoftidentityweb) | <li>Geschäfts-, Schul- oder Unikonten<li>Identitäten in sozialen Netzwerken (mit Azure AD B2C) | <li>Geschäfts-, Schul- oder Unikonten<li>Persönliche Microsoft-Konten<li>Identitäten in sozialen Netzwerken (mit Azure AD B2C)     |
| Ausstellerüberprüfung in mehrinstanzenfähigen Apps                                                   | Nein                                                                   | Ja, für [alle Clouds](authentication-national-cloud.md) und [Azure AD B2C](/azure/active-directory-b2c) |
| Web-App/API [ruft Microsoft Graph auf] [Szenario-API-Aufruf-Graph]                             | Nein                                                                   | Ja                                                                                                     |
| Web-App/API [ruft Web-API auf] [Szenario-API-Aufruf-API]                                       | Nein                                                                   | Ja                                                                                                     |
| Unterstützt Zertifikatanmeldeinformationen                                                         | Nein                                                                   | Ja, einschließlich Azure Key Vault                                                                          |
| Unterstützung von inkrementeller Zustimmung und bedingtem Zugriff in Web-Apps                           | Nein                                                                   | Ja, in MVC, Razor Pages und Blazor                                                                    |
| Token-Verschlüsselungszertifikate in Web-APIs                                                | Nein                                                                   | Ja                                                                                                     |
| [Bereiche/Überprüfung der App-Rolle][Szenario-API-Validierung] in Web-APIs                        | Nein                                                                   | Ja                                                                                                     |
| `WWW-Authenticate`-Headergenerierung in Web-APIs                                         | Nein                                                                   | Ja                                                                                                     |

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie Microsoft Identity Web in Action sehen möchten, testen Sie das Blazor Server-Tutorial:

[Tutorial: Erstellen einer Blazor Server-App, die Microsoft Identity Platform für die Authentifizierung verwendet](tutorial-blazor-server.md)

Das Microsoft Identity Web-Wiki auf GitHub enthält eine umfassende Referenzdokumentation für verschiedene Aspekte der Bibliothek. Beispielsweise können Sie Zertifikatverwendung, die inkrementelle Zustimmung und den Verweis auf den bedingten Zugriff hier finden:

- [Using certificates with Microsoft.Identity.Web](https://github.com/AzureAD/microsoft-identity-web/wiki/Using-certificates?azure-portal=true) (Verwenden von Zertifikaten mit Microsoft.Identity.Web; GitHub)
- [Managing incremental consent and conditional access](https://github.com/AzureAD/microsoft-identity-web/wiki/Managing-incremental-consent-and-conditional-access?azure-portal=true) (Verwalten von inkrementeller Zustimmung und bedingtem Zugriff; GitHub)

<!-- LINKS -->
<!--  [miw-certs]: microsoft-identity-web-certificates.md  -->
<!--  [miw-certs-decrypt]: microsoft-identity-web-certificates.md#decryption-certificates  -->
<!--  [miw-inc-consent-ca-header]: microsoft-identity-web-consent-conditional-access.md#handling-incremental-consent-or-conditional-access-in-web-apis  -->
<!--  [miw-inc-consent-ca]: microsoft-identity-web-consent-conditional-access.md  -->
[Szenario-API-Aufruf-API]: Szenario-Web-API-Aufruf-API-Aufruf-API.md#Option-1-Aufruf-Microsoft-Graph-mit-dem-SDK [Szenario-API-Aufruf-Graph]: Szenario-Web-API-Aufruf-API-Aufruf-API.md#Option-1-Aufruf-Microsoft-Graph-mit-dem-SDK [Szenario-API-Validierung]: Szenario-geschützt-Web-API-Überprüfung-Bereich-App-Rollen.md
