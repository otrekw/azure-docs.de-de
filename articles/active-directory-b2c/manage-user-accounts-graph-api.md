---
title: Verwalten von Benutzern mit der Microsoft Graph-API
titleSuffix: Azure AD B2C
description: Erfahren Sie mehr zum Verwalten von Benutzern in einem Azure AD B2C-Mandanten durch Aufrufen der Microsoft Graph-API und Automatisieren des Vorgangs mithilfe einer Anwendungsidentität.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 01/13/2021
ms.custom: project-no-code
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: ff3cd858de86d21637f4a7a9ab9d9a83c7022f5a
ms.sourcegitcommit: c136985b3733640892fee4d7c557d40665a660af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98178873"
---
# <a name="manage-azure-ad-b2c-user-accounts-with-microsoft-graph"></a>Verwalten von Azure AD B2C-Benutzerkonten mit Microsoft Graph

Microsoft Graph ermöglicht Ihnen das Verwalten von Benutzerkonten in Ihrem Azure AD B2C-Verzeichnis durch das Bereitstellen von Methoden zum Erstellen, Lesen, Aktualisieren und Löschen in der Microsoft Graph-API. Sie können einen vorhandenen Benutzerspeicher zu einem Azure AD B2C-Mandanten migrieren und andere Verwaltungsvorgänge für Benutzerkonten durch Aufrufen der Microsoft Graph-API ausführen.

In den folgenden Abschnitten werden die wichtigsten Aspekte der Azure AD B2C-Benutzerverwaltung mit der Microsoft Graph-API vorgestellt. Die hier dargestellten Vorgänge, Typen und Eigenschaften der Microsoft Graph-API stellen nur einen kleinen Teil dessen dar, was in der Referenzdokumentation zur Microsoft Graph-API enthalten ist.

## <a name="register-a-management-application"></a>Registrieren einer Managementanwendung

Bevor von Ihnen geschriebene Benutzerverwaltungsanwendungen oder -skripts mit den Ressourcen in Ihrem Azure AD B2C-Mandanten interagieren können, benötigen Sie eine Anwendungsregistrierung, über die die dafür erforderlichen Berechtigungen erteilt werden.

Befolgen Sie die Anleitungsschritte in diesem Artikel, um eine Anwendungsregistrierung zu erstellen, die von Ihrer Managementanwendung verwendet werden kann:

[Verwalten von Azure AD B2C mit Microsoft Graph](microsoft-graph-get-started.md)

## <a name="user-management-microsoft-graph-operations"></a>Microsoft Graph-Vorgänge für die Benutzerverwaltung

Die folgenden Benutzerverwaltungsvorgänge sind in der [Microsoft Graph-API](/graph/api/resources/user) verfügbar:

- [Abrufen einer Liste der Benutzer](/graph/api/user-list)
- [Erstellen eines Benutzers](/graph/api/user-post-users)
- [Abrufen eines Benutzers](/graph/api/user-get)
- [Aktualisieren eines Benutzers](/graph/api/user-update)
- [Löschen eines Benutzers](/graph/api/user-delete)


## <a name="code-sample-how-to-programmatically-manage-user-accounts"></a>Codebeispiel: Programmgesteuertes Verwalten von Benutzerkonten

Dieses Codebeispiel ist eine .NET Core-Konsolenanwendung, die das [Microsoft Graph SDK](/graph/sdks/sdks-overview) für die Interaktion mit der Microsoft Graph-API verwendet. Der Code veranschaulicht, wie die API aufgerufen wird, um Benutzer in einem Azure AD B2C-Mandanten programmgesteuert zu verwalten.
Sie können das [Beispielarchiv herunterladen](https://github.com/Azure-Samples/ms-identity-dotnetcore-b2c-account-management/archive/master.zip) (*.zip), das Repository auf GitHub [durchsuchen](https://github.com/Azure-Samples/ms-identity-dotnetcore-b2c-account-management) oder das Repository klonen:

```cmd
git clone https://github.com/Azure-Samples/ms-identity-dotnetcore-b2c-account-management.git
```

Nachdem Sie das Codebeispiel abgerufen haben, konfigurieren Sie es für Ihre Umgebung, und erstellen Sie dann das Projekt:

1. Öffnen Sie das Projekt in [Visual Studio](https://visualstudio.microsoft.com) oder [Visual Studio Code](https://code.visualstudio.com).
1. Öffnen Sie `src/appsettings.json`.
1. Ersetzen Sie im Abschnitt `appSettings` den Wert `your-b2c-tenant` durch den Namen Ihres Mandanten und `Application (client) ID` und `Client secret` durch die Werte für die Registrierung Ihrer Managementanwendung. (Weitere Informationen finden Sie im Abschnitt [Registrieren einer Managementanwendung](#register-a-management-application) dieses Artikels.)
1. Öffnen Sie in Ihrem lokalen Klon des Repositorys ein Konsolenfenster, wechseln Sie in das Verzeichnis `src`, und erstellen Sie dann das Projekt:
    ```console
    cd src
    dotnet build
    ```
1. Führen Sie die Anwendung mit Befehl `dotnet` aus:

    ```console
    dotnet bin/Debug/netcoreapp3.1/b2c-ms-graph.dll
    ```

Die Anwendung zeigt eine Liste der Befehle an, die Sie ausführen können. Beispielsweise können Sie alle Benutzer oder einen einzelnen Benutzer abrufen, einen Benutzer löschen, das Kennwort eines Benutzers aktualisieren und einen Massenimport ausführen.

### <a name="code-discussion"></a>Überlegungen zum Code

Der Beispielcode verwendet das [Microsoft Graph SDK](/graph/sdks/sdks-overview), das entwickelt wurde, um das Entwickeln hochwertiger, effizienter und robuster Anwendungen mit Zugriff auf Microsoft Graph zu vereinfachen.

Alle Anforderungen an die Microsoft Graph-API erfordern ein Zugriffstoken für die Authentifizierung. Die Lösung nutzt das NuGet-Paket [Microsoft.Graph.Auth](https://www.nuget.org/packages/Microsoft.Graph.Auth/), das einen auf einem Authentifizierungsszenario basierenden Wrapper der Microsoft Authentication Library (MSAL) für die Verwendung mit dem Microsoft Graph SDK bereitstellt.

Die `RunAsync`-Methode in der Datei _Program.cs_ führt Folgendes aus:

1. Sie liest Anwendungseinstellungen aus der Datei _appsettings.json_
1. Sie initialisiert den Authentifizierungsanbieter unter Verwendung des [Gewährungsflows für OAuth 2.0-Clientanmeldeinformationen](../active-directory/develop/v2-oauth2-client-creds-grant-flow.md). Mit dem Gewährungsflow für Clientanmeldeinformationen kann die App ein Zugriffstoken abrufen, um die Microsoft Graph-API aufzurufen.
1. Sie richtet den Microsoft Graph-Dienstclient mit dem Authentifizierungsanbieter ein:

    ```csharp
    // Read application settings from appsettings.json (tenant ID, app ID, client secret, etc.)
    AppSettings config = AppSettingsFile.ReadFromJsonFile();

    // Initialize the client credential auth provider
    IConfidentialClientApplication confidentialClientApplication = ConfidentialClientApplicationBuilder
        .Create(config.AppId)
        .WithTenantId(config.TenantId)
        .WithClientSecret(config.ClientSecret)
        .Build();
    ClientCredentialProvider authProvider = new ClientCredentialProvider(confidentialClientApplication);

    // Set up the Microsoft Graph service client with client credentials
    GraphServiceClient graphClient = new GraphServiceClient(authProvider);
    ```

Der initialisierte *GraphServiceClient* wird dann in _UserService.cs_ verwendet, um die Vorgänge zur Benutzerverwaltung auszuführen. Beispielsweise können Sie eine Liste der Benutzerkonten im Mandanten abrufen:

```csharp
public static async Task ListUsers(GraphServiceClient graphClient)
{
    Console.WriteLine("Getting list of users...");

    // Get all users (one page)
    var result = await graphClient.Users
        .Request()
        .Select(e => new
        {
            e.DisplayName,
            e.Id,
            e.Identities
        })
        .GetAsync();

    foreach (var user in result.CurrentPage)
    {
        Console.WriteLine(JsonConvert.SerializeObject(user));
    }
}
```

Das [Ausführen von API-Aufrufen mithilfe der Microsoft Graph-SDKs](/graph/sdks/create-requests) umfasst Informationen zum Lesen und Schreiben von Informationen aus Microsoft Graph. Verwenden Sie `$select`, um die zurückgegebenen Eigenschaften zu steuern, benutzerdefinierte Abfrageparameter bereitzustellen und die Abfrageparameter `$filter` und `$orderBy` zu verwenden.

## <a name="next-steps"></a>Nächste Schritte

Einen vollständigen Index der Microsoft Graph-API-Vorgänge, die für Azure AD B2C-Ressourcen unterstützt werden, finden Sie unter [Microsoft Graph Vorgänge, die für Azure AD B2C verfügbar sind](microsoft-graph-operations.md).

<!-- LINK -->

[graph-objectIdentity]: /graph/api/resources/objectidentity
[graph-user]: (https://docs.microsoft.com/graph/api/resources/user)
