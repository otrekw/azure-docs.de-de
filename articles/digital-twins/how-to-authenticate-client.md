---
title: Authentifizieren einer Clientanwendung
titleSuffix: Azure Digital Twins
description: Hier erfahren Sie, wie eine Clientanwendung für den Dienst Azure Digital Twins authentifiziert wird.
author: baanders
ms.author: baanders
ms.date: 4/22/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: e52307c92d9371af6479f64841c6f269ed10e4b4
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85390821"
---
# <a name="authenticate-a-client-application-with-azure-digital-twins"></a>Authentifizieren einer Clientanwendung für Azure Digital Twins

Nachdem Sie eine [Azure Digital Twins-Instanz](how-to-set-up-instance.md) erstellt haben, können Sie eine Clientanwendung erstellen, die Ihnen die Interaktion mit der Instanz ermöglicht. Nachdem Sie ein Clientstartprojekt eingerichtet haben, finden Sie in diesem Artikel Informationen dazu, wie Sie diese Clientanwendung ordnungsgemäß für die Azure Digital Twins-Instanz authentifizieren.

Dazu sind zwei Schritte erforderlich:
1. Erstellen einer App-Registrierung
2. Schreiben von Authentifizierungscode in einer Clientanwendung

[!INCLUDE [Cloud Shell for Azure Digital Twins](../../includes/digital-twins-cloud-shell.md)]

## <a name="create-an-app-registration"></a>Erstellen einer App-Registrierung

Zur Authentifizierung bei Azure Digital Twins über eine Clientanwendung müssen Sie eine **App-Registrierung** in [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) einrichten.

Über diese App-Registrierung konfigurieren Sie die Zugriffsberechtigungen für die [Azure Digital Twins-APIs](how-to-use-apis-sdks.md). Ihre Client-App wird für die App-Registrierung authentifiziert. Daraufhin werden ihr die konfigurierten Zugriffsberechtigungen für die APIs zugewiesen.

Zum Erstellen einer App-Registrierung müssen Sie die Ressourcen-IDs für die Azure Digital Twins-APIs und die Baselineberechtigungen für die API angeben. Öffnen Sie in Ihrem Arbeitsverzeichnis eine neue Datei, und geben Sie den folgenden JSON-Codeausschnitt ein, um diese Details zu konfigurieren: 

```json
[{
    "resourceAppId": "0b07f429-9f4b-4714-9392-cc5e8e80c8b0",
    "resourceAccess": [
     {
       "id": "4589bd03-58cb-4e6c-b17f-b580e39652f8",
       "type": "Scope"
     }
    ]
}]
``` 

Speichern Sie diese Datei unter *manifest.json*.

> [!NOTE] 
> Es gibt einige Stellen, an denen die benutzerfreundliche, lesbare Zeichenfolge `https://digitaltwins.azure.net` für die App-ID der Azure Digital Twins-Ressource anstelle der GUID `0b07f429-9f4b-4714-9392-cc5e8e80c8b0` verwendet werden kann. So wird in vielen Beispielen in dieser Dokumentation die Authentifizierung bei der MSAL-Bibliothek beschrieben, für die die benutzerfreundliche Zeichenfolge verwendet werden kann. Bei diesem Schritt beim Erstellen der App-Registrierung ist jedoch wie oben dargestellt das GUID-Format der ID erforderlich. 

Klicken Sie im Cloud Shell-Fenster auf das Symbol „Dateien hochladen/herunterladen“, und wählen Sie „Hochladen“ aus.

:::image type="content" source="media/how-to-authenticate-client/upload-extension.png" alt-text="Cloud Shell-Fenster: Auswahl der Option „Hochladen“":::
Navigieren Sie zu der eben erstellten Datei *manifest.json*, und wählen Sie „Öffnen“ aus.

Führen Sie den folgenden Befehl aus, um eine App-Registrierung zu erstellen (und ersetzen Sie dabei die Platzhalter):

```azurecli
az ad app create --display-name <name-for-your-app> --native-app --required-resource-accesses manifest.json --reply-url http://localhost
```

Die Ausgabe dieses Befehls sieht in etwa wie folgt aus.

:::image type="content" source="media/how-to-authenticate-client/new-app-registration.png" alt-text="Registrierung einer neuen AAD-App":::

Nachdem Sie die App-Registrierung erstellt haben, folgen Sie [diesem Link](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps), um zur Seite mit der Übersicht über die Registrierung der AAD-App im Azure-Portal zu navigieren.

Wählen Sie in dieser Übersicht in der Liste die App-Registrierung aus, die Sie eben erstellt haben. Dadurch werden die Details in einer Seite wie dieser geöffnet:

:::image type="content" source="media/how-to-authenticate-client/get-authentication-ids.png" alt-text="Azure-Portal: Authentifizierungs-IDs":::

Notieren Sie sich die *Anwendungs-ID (Client)* und die *Verzeichnis-ID (Mandant)* , die auf **Ihrer** Seite angezeigt werden. Diese Werte verwenden Sie später zum Authentifizieren einer Client-App für die Azure Digital Twins-APIs.

> [!NOTE]
> Je nach Szenario müssen Sie weitere Änderungen an der App-Registrierung vornehmen. Im Folgenden sind einige allgemeine Anforderungen aufgeführt, die Sie möglicherweise erfüllen müssen:
> * Aktivieren des öffentlichen Clientzugriffs
> * Festlegen bestimmter Antwort-URLs für den Web- und Desktopzugriff
> * Zulassen von impliziten OAuth2-Authentifizierungsflows
> * Wenn Ihr Azure-Abonnement mit einem Microsoft-Konto wie Live, Xbox oder Hotmail erstellt wurde, müssen Sie zur Unterstützung von persönlichen Konten das Attribut *signInAudience* in der App-Registrierung festlegen.
> Hierzu verwenden Sie am besten das [Azure-Portal](https://portal.azure.com/). Weitere Informationen zu diesem Vorgang finden Sie unter [Registrieren einer Anwendung bei Microsoft Identity Platform](https://docs.microsoft.com/graph/auth-register-app-v2).

## <a name="write-client-app-authentication-code-net-c-sdk"></a>Schreiben von Authentifizierungscode für die Client-App: .NET (C#) SDK

In diesem Abschnitt wird der Code beschrieben, den Sie in Ihre Clientanwendung einbinden müssen, um den Authentifizierungsprozess mit dem .NET (C#) SDK abzuschließen.
Das Azure Digital Twins C# SDK ist Teil des Azure SDK für .NET. Sie finden es an der folgenden Stelle: [Azure IoT Digital Twin-Clientbibliothek für .NET](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/digitaltwins/Azure.DigitalTwins.Core).

### <a name="prerequisites"></a>Voraussetzungen

Wenn Sie noch kein Client-App-Startprojekt eingerichtet haben, erstellen Sie ein einfaches .NET-Projekt zur Verwendung bei diesem Tutorial.

Wenn Sie das .NET SDK verwenden möchten, müssen Sie die folgenden Pakete in Ihr Projekt einbinden:
* `Azure.DigitalTwins.Core` (Version `1.0.0-preview.2`)
* `Azure.Identity`

Je nachdem, welche Tools Sie verwenden, können Sie hierzu den Visual Studio-Paket-Manager oder das Befehlszeilentool `dotnet` verwenden. 

### <a name="authentication-and-client-creation-net"></a>Authentifizierung und Clienterstellung: .NET

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

Weitere Informationen finden Sie unter [Vorgehensweise: Einrichten einer Azure-Funktion zum Verarbeiten von Daten](how-to-create-azure-function.md). Hier finden Sie ein ausführlicheres Beispiel, anhand dessen einige wichtige Konfigurationsoptionen im Zusammenhang mit Funktionen erläutert werden.

Wenn Sie die Authentifizierung in einer Funktion verwenden möchten, müssen Sie zudem Folgendes beachten:
* [Aktivieren der verwalteten Identität](https://docs.microsoft.com/azure/app-service/overview-managed-identity?tabs=dotnet)
* [Umgebungsvariablen](https://docs.microsoft.com/sandbox/functions-recipes/environment-variables?tabs=csharp)
* Zuweisen von Berechtigungen zur Funktions-App, um dieser den Zugriff auf die Digital Twins-APIs zu ermöglichen. Weitere Informationen finden Sie unter [Vorgehensweise: Einrichten einer Azure-Funktion zum Verarbeiten von Daten](how-to-create-azure-function.md).

## <a name="authentication-in-an-autorest-generated-sdk"></a>Authentifizierung in einem mit AutoRest generierten SDK

Wenn Sie .NET nicht verwenden, können Sie eine SDK-Bibliothek in einer Sprache Ihrer Wahl erstellen. Informationen dazu finden Sie unter [Vorgehensweise: Erstellen von benutzerdefinierten SDKs für Azure Digital Twins mit AutoRest](how-to-create-custom-sdks.md).

In diesem Abschnitt wird die Authentifizierung in solch einem Fall erläutert.

### <a name="prerequisites"></a>Voraussetzungen

In diesem Beispiel wird ein mit AutoRest generiertes TypeScript SDK verwendet. Folglich ist auch Folgendes erforderlich:
* [msal-js](https://github.com/AzureAD/microsoft-authentication-library-for-js)
* [ms-rest-js](https://github.com/Azure/ms-rest-js)

### <a name="minimal-authentication-code-sample"></a>Beispiel für minimalen Authentifizierungscode

Zum Authentifizieren einer .NET-App mit Azure-Diensten können Sie den folgenden minimalen Code in Ihrer Client-App verwenden.

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

MSAL bietet viele weitere Optionen, die Sie verwenden können, um Funktionen wie Caching und andere Authentifizierungsflows zu implementieren. Weitere Informationen hierzu finden Sie unter [Übersicht über die Microsoft-Authentifizierungsbibliothek (MSAL)](../active-directory/develop/msal-overview.md).

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr zum Thema Sicherheit in Azure Digital Twins:
* [Konzepte: Sicherheit für Azure Digital Twins-Lösungen](concepts-security.md)

Oder informieren Sie sich über die Erstellung von Modellen in einer Instanz, wenn Sie die Authentifizierung eingerichtet haben:
* [Gewusst wie: Verwalten benutzerdefinierter Modelle](how-to-manage-model.md)