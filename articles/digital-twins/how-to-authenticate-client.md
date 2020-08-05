---
title: Schreiben von App-Authentifizierungscode
titleSuffix: Azure Digital Twins
description: Hier erfahren Sie, wie Authentifizierungscode in einer Clientanwendung geschrieben wird.
author: baanders
ms.author: baanders
ms.date: 4/22/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 4d235280ae4a600994eb93ec08c7a13630f9682f
ms.sourcegitcommit: 0e8a4671aa3f5a9a54231fea48bcfb432a1e528c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2020
ms.locfileid: "87131581"
---
# <a name="write-client-app-authentication-code"></a>Schreiben von Authentifizierungscode für die Client-App

Nachdem Sie eine [Azure Digital Twins-Instanz und -Authentifizierung eingerichtet](how-to-set-up-instance-scripted.md) haben, können Sie eine Clientanwendung erstellen, die Ihnen die Interaktion mit der Instanz ermöglicht. Nachdem Sie ein Clientstartprojekt eingerichtet haben, finden Sie in diesem Artikel Informationen dazu, **wie Sie in dieser Client-App Code schreiben, um die App für die Azure Digital Twins-Instanz zu authentifizieren**.

In diesem Artikel werden zwei Konzepte für Beispielcode beschrieben. Verwenden Sie den für die verwendete Sprache geeigneten Code:
* Im ersten Abschnitt des Beispielcodes wird das Azure Digital Twins .NET SDK (C#) verwendet. Das SDK ist Teil des Azure SDK für .NET und befindet sich hier: [*Azure IoT Digital Twin-Clientbibliothek für .NET*](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/digitaltwins/Azure.DigitalTwins.Core).
* Der zweite Abschnitt des Beispielcodes ist für Benutzer bestimmt, die anstelle des .NET SDK mit AutoRest generierte SDKs in anderen Sprachen verwenden. Weitere Informationen zu dieser Strategie finden Sie unter [*Vorgehensweise: Erstellen von benutzerdefinierten SDKs für Azure Digital Twins mit AutoRest*](how-to-create-custom-sdks.md).

Weitere Informationen zu den APIs und SDKs für Azure Digital Twins finden Sie unter [*Vorgehensweise: Verwenden der Azure Digital Twins-APIs und SDKs*](how-to-use-apis-sdks.md).

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie zunächst die Schritte zur Einrichtung unter [*Vorgehensweise: Einrichten einer Instanz und der Authentifizierung*](how-to-set-up-instance-scripted.md) aus. Dadurch wird sichergestellt, dass Sie über eine Instanz von Azure Digital Twins verfügen, Ihr Benutzer über Zugriffsberechtigungen verfügt und Sie Berechtigungen für Client Anwendungen eingerichtet haben. Nachdem Sie diese Einrichtung abgeschlossen haben, können Sie Client-App-Code schreiben.

Als Nächstes benötigen Sie ein Client-App-Projekt, in dem Sie den Code schreiben. Wenn Sie nicht bereits ein Client-App-Projekt eingerichtet haben, erstellen Sie ein einfaches Projekt in der Sprache Ihrer Wahl zur Verwendung in diesem Tutorial.

## <a name="authentication-and-client-creation-net-c-sdk"></a>Authentifizierung und Clienterstellung: .NET SDK (C#)

Binden Sie zunächst die folgenden Pakete in Ihr Projekt ein, sodass Sie das .NET SDK und die Authentifizierungstools für diese Vorgehensweise verwenden können:
* `Azure.DigitalTwins.Core` (Version `1.0.0-preview.2`)
* `Azure.Identity`

Je nachdem, welche Tools Sie verwenden, können Sie die Pakete mit dem Visual Studio-Paket-Manager oder mit dem Befehlszeilentool `dotnet` einbinden. 

Verwenden Sie für die Authentifizierung beim .NET SDK eine der Methoden zum Abrufen von Anmeldeinformationen, die in der [Azure.Identity](https://docs.microsoft.com/dotnet/api/azure.identity?view=azure-dotnet)-Bibliothek definiert sind.

Die folgenden beiden Methoden werden häufig verwendet: 
* [InteractiveBrowserCredential](https://docs.microsoft.com/dotnet/api/azure.identity.interactivebrowsercredential?view=azure-dotnet). Diese Methode ist für interaktive Anwendungen gedacht und startet einen Webbrowser für die Authentifizierung.
* [ManagedIdentityCredential](https://docs.microsoft.com/dotnet/api/azure.identity.managedidentitycredential?view=azure-dotnet). Diese Methode eignet sich für Fälle, in denen [verwaltete Identitäten (MSI)](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) benötigt werden, beispielsweise bei er Verwendung von Azure Functions. 

Zudem werden die folgenden using-Anweisungen benötigt:

```csharp
using Azure.Identity;
using Azure.DigitalTwins.Core;
```

Fügen Sie den folgenden Code hinzu, um die interaktiven Browseranmeldeinformationen zum Erstellen eines authentifizierten SDK-Clients zu verwenden:

```csharp
// Your client / app registration ID
private static string clientId = "<your-client-ID>"; 
// Your tenant / directory ID
private static string tenantId = "<your-tenant-ID>";
// The URL of your instance, starting with the protocol (https://)
private static string adtInstanceUrl = "<your-Azure-Digital-Twins-instance-URL>";

//...

DigitalTwinsClient client;
try
{
    var credential = new InteractiveBrowserCredential(tenantId, clientId);
    client = new DigitalTwinsClient(new Uri(adtInstanceUrl), credential);
} catch(Exception e)
{
    Console.WriteLine($"Authentication or client creation error: {e.Message}");
    Environment.Exit(0);
}
```

>[!NOTE]
> Die Client-ID, die Mandanten-ID und die URL der Instanz können zwar wie oben dargestellt direkt in den Code eingefügt werden, es empfiehlt sich jedoch, den Code die Werte stattdessen aus einer Konfigurationsdatei oder Umgebungsvariablen abrufen zu lassen.

Dann können Sie die Anmeldeinformationen für die verwaltete Identität wie folgt in einer Azure-Funktion verwenden:

```csharp
ManagedIdentityCredential cred = new ManagedIdentityCredential(adtAppId);
DigitalTwinsClientOptions opts = 
    new DigitalTwinsClientOptions { Transport = new HttpClientTransport(httpClient) });
client = new DigitalTwinsClient(new Uri(adtInstanceUrl), cred, opts);
```

Weitere Informationen finden Sie unter [*Vorgehensweise: Einrichten einer Azure-Funktion zum Verarbeiten von Daten*](how-to-create-azure-function.md). Hier finden Sie ein ausführlicheres Beispiel, anhand dessen einige wichtige Konfigurationsoptionen im Zusammenhang mit Funktionen erläutert werden.

Wenn Sie die Authentifizierung in einer Funktion verwenden möchten, müssen Sie zudem Folgendes beachten:
* [Aktivieren der verwalteten Identität](https://docs.microsoft.com/azure/app-service/overview-managed-identity?tabs=dotnet)
* Verwenden von [Umgebungsvariablen](https://docs.microsoft.com/sandbox/functions-recipes/environment-variables?tabs=csharp) nach Bedarf
* Zuweisen von Berechtigungen zur Funktions-App, um dieser den Zugriff auf die Digital Twins-APIs zu ermöglichen. Weitere Informationen zu Azure Functions-Prozessen finden Sie unter [*Vorgehensweise: von Azure Functions-Apps für die Verarbeitung von Daten*](how-to-create-azure-function.md).

## <a name="authentication-with-an-autorest-generated-sdk"></a>Authentifizierung mit einem mit AutoRest generierten SDK

Wenn Sie .NET nicht verwenden, können Sie eine SDK-Bibliothek in einer Sprache Ihrer Wahl erstellen. Informationen dazu finden Sie unter [*Vorgehensweise: Erstellen von benutzerdefinierten SDKs für Azure Digital Twins mit AutoRest*](how-to-create-custom-sdks.md).

In diesem Abschnitt wird die Authentifizierung in solch einem Fall erläutert.

### <a name="prerequisites"></a>Voraussetzungen

Zunächst müssen Sie die Schritte zum Erstellen eines benutzerdefinierten SDK mit AutoRest durchführen. Verwenden Sie hierzu die Anleitung unter [*Vorgehensweise: Erstellen von benutzerdefinierten SDKs für Azure Digital Twins mit AutoRest*](how-to-create-custom-sdks.md) aus.

In diesem Beispiel wird ein mit AutoRest generiertes TypeScript SDK verwendet. Folglich ist auch Folgendes erforderlich:
* [msal-js](https://github.com/AzureAD/microsoft-authentication-library-for-js)
* [ms-rest-js](https://github.com/Azure/ms-rest-js)

### <a name="minimal-authentication-code-sample"></a>Beispiel für minimalen Authentifizierungscode

Zum Authentifizieren einer App mit Azure-Diensten können Sie den folgenden minimalen Code in Ihrer Client-App verwenden.

Sie benötigen die bereits erwähnte *Anwendungs-ID (Client)* und *Verzeichnis-ID (Mandant)* und die URL Ihrer Azure Digital Twins-Instanz.

> [!TIP]
> Die URL der Azure Digital Twins-Instanz wird durch Hinzufügen von *https://* an den Anfang des *Hostnamens* der Azure Digital Twins-Instanz erstellt. Um den *Hostnamen* mit allen Eigenschaften der Instanz anzuzeigen, können Sie `az dt show --dt-name <your-Azure-Digital-Twins-instance>` ausführen. Sie können den Befehl `az account show --query tenantId` verwenden, um die *Verzeichnis-ID (Mandant)* anzuzeigen. 

```javascript
import * as Msal from "msal";
import { TokenCredentials } from "@azure/ms-rest-js";
// Autorest-generated SDK
import { AzureDigitalTwinsAPI } from './azureDigitalTwinsAPI';

// Client / app registration ID
var ClientId = "<your-client-ID>";
// Azure tenant / directory ID
var TenantId = "<your-tenant-ID>";
// URL of the Azure Digital Twins instance
var AdtInstanceUrl = "<your-instance-URL>"; 

var AdtAppId = "https://digitaltwins.azure.net";

let client = null;

export async function login() {

    const msalConfig = {
        auth: {
            clientId: ClientId,
            redirectUri: "http://localhost:3000",
            authority: "https://login.microsoftonline.com/"+TenantId
        }
    };

    const msalInstance = new Msal.UserAgentApplication(msalConfig);

    msalInstance.handleRedirectCallback((error, response) => {
        // handle redirect response or error
    });

    var loginRequest = {
        scopes: [AdtAppId + "/.default"] 
    };

    try {
        await msalInstance.loginPopup(loginRequest)
        var accessToken;
        // if the user is already logged in you can acquire a token
        if (msalInstance.getAccount()) {
            var tokenRequest = {
                scopes: [AdtAppId + "/.default"]
            };
            try {
                const response = await msalInstance.acquireTokenSilent(tokenRequest);
                accessToken = response.accessToken;
            } catch (err) {
                if (err.name === "InteractionRequiredAuthError") {
                    const response = await msalInstance.acquireTokenPopup(tokenRequest)
                    accessToken = response.accessToken;
                }
            }
        }
        if (accessToken!=null)
        {
            var tokenCredentials = new TokenCredentials(accessToken);
                
            // Add token and server URL to service instance
            const clientConfig = {
                baseUri: AdtInstanceUrl
            };
            client = new AzureDigitalTwinsAPI(tokenCredentials, clientConfig);
            appDataStore.client = client;
        }
    } catch (err) {
        ...
    }
}
```

Auch hier gilt: Die Client-ID, die Mandanten-ID und die URL der Instanz können zwar der Einfachheit halber direkt in den Code eingefügt werden, es empfiehlt sich jedoch, den Code die Werte stattdessen aus einer Konfigurationsdatei oder Umgebungsvariablen abrufen zu lassen.

MSAL bietet viele weitere Optionen, die Sie verwenden können, um Funktionen wie Caching und andere Authentifizierungsflows zu implementieren. Weitere Informationen hierzu finden Sie unter [*Übersicht über die Microsoft-Authentifizierungsbibliothek (MSAL)* ](../active-directory/develop/msal-overview.md).

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr zum Thema Sicherheit in Azure Digital Twins:
* [*Konzepte: Sicherheit für Azure Digital Twins-Lösungen*](concepts-security.md)

Oder informieren Sie sich über die Erstellung von Modellen in einer Instanz, wenn Sie die Authentifizierung eingerichtet haben:
* [*Verwenden Verwalten von Azure Digital Twins-Modellen*](how-to-manage-model.md)