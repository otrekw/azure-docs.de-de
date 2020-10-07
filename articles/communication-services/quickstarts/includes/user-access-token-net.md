---
title: include file
description: include file
services: azure-communication-services
author: matthewrobertson
manager: nimag
ms.service: azure-communication-services
ms.subservice: azure-communication-services
ms.date: 08/20/2020
ms.topic: include
ms.custom: include file
ms.author: marobert
ms.openlocfilehash: 5c9066f369183de3b4cfe19cc5635e8f1b4a94a2
ms.sourcegitcommit: ef69245ca06aa16775d4232b790b142b53a0c248
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/06/2020
ms.locfileid: "91779338"
---
## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). 
- Die aktuelle Version der [.NET Core-Clientbibliothek](https://dotnet.microsoft.com/download/dotnet-core) für Ihr Betriebssystem.
- Eine aktive Communication Services-Ressource und eine Verbindungszeichenfolge. [Erstellen Sie eine Communication Services-Ressource.](../create-communication-resource.md)

## <a name="setting-up"></a>Einrichten

### <a name="create-a-new-c-application"></a>Erstellen einer neuen C#-Anwendung

Verwenden Sie in einem Konsolenfenster (z. B. cmd, PowerShell oder Bash) den Befehl `dotnet new` zum Erstellen einer neuen Konsolen-App mit dem Namen `UserAccessTokensQuickstart`. Dieser Befehl erstellt ein einfaches „Hallo Welt“-C#-Projekt mit einer einzigen Quelldatei: **Program.cs**.

```console
dotnet new console -o UserAccessTokensQuickstart
```

Wechseln Sie zum neu erstellten App-Ordner, und verwenden Sie den Befehl `dotnet build`, um Ihre Anwendung zu kompilieren.

```console
cd UserAccessTokensQuickstart
dotnet build
```

### <a name="install-the-package"></a>Installieren des Pakets

Installieren Sie im Anwendungsverzeichnis mithilfe des Befehls `dotnet add package` das .NET-Paket der Azure Communication Services-Bibliothek „Administration“.

```console
dotnet add package Azure.Communication.Administration --version 1.0.0-beta.2
```

### <a name="set-up-the-app-framework"></a>Einrichten des App-Frameworks

Über das Projektverzeichnis:

1. Öffnen Sie die Datei **Program.cs** in einem Text-Editor.
1. Fügen Sie eine Anweisung vom Typ `using` hinzu, um den Namespace `Azure.Communication.Administration` einzuschließen.
1. Aktualisieren der `Main`-Methodendeklaration zur Unterstützung von asynchronem Code

Verwenden Sie zum Einstieg den folgenden Code:

```csharp
using System;
using Azure.Communication.Administration;

namespace UserAccessTokensQuickstart
{
    class Program
    {
        static async System.Threading.Tasks.Task Main(string[] args)
        {
            Console.WriteLine("Azure Communication Services - User Access Tokens Quickstart");

            // Quickstart code goes here
        }
    }
}
```

[!INCLUDE [User Access Tokens Object Model](user-access-tokens-object-model.md)]

## <a name="authenticate-the-client"></a>Authentifizieren des Clients

Initialisieren Sie einen Kommunikationsidentitätsclient (`CommunicationIdentityClient`) mit Ihrer Verbindungszeichenfolge. Im folgenden Code wird die Verbindungszeichenfolge für die Ressource aus einer Umgebungsvariablen namens `COMMUNICATION_SERVICES_CONNECTION_STRING` abgerufen. Informationen zur Verwaltung der Verbindungszeichenfolge Ihrer Ressource finden Sie [hier](../create-communication-resource.md#store-your-connection-string).

Fügen Sie der `Main`-Methode folgenden Code hinzu:

```csharp
// This code demonstrates how to fetch your connection string
// from an environment variable.
string ConnectionString = Environment.GetEnvironmentVariable("COMMUNICATION_SERVICES_CONNECTION_STRING");
var client = new CommunicationIdentityClient(ConnectionString);
```

## <a name="create-a-user"></a>Erstellen eines Benutzers

Von Azure Communication Services wird ein einfaches Identitätsverzeichnis gepflegt. Verwenden Sie die Methode `createUser`, um in dem Verzeichnis einen neuen Eintrag mit einer eindeutigen `Id` zu erstellen. Es empfiehlt sich, eine Zuordnung zwischen den Benutzern Ihrer Anwendung und den von Communication Services generierten Identitäten zu pflegen (etwa durch Speichern in der Datenbank Ihres Anwendungsservers).

```csharp
var userResponse = await client.CreateUserAsync();
var user = userResponse.Value;
Console.WriteLine($"\nCreated a user with ID: {user.Id}");
```

## <a name="issue-user-access-tokens"></a>Ausstellen von Benutzerzugriffstoken

Verwenden Sie die Methode `issueToken`, um ein Zugriffstoken für einen Communication Services-Benutzer auszustellen. Ohne Angabe des optionalen Parameters `user` wird ein neuer Benutzer erstellt und mit dem Token zurückgegeben.

```csharp
// Issue an access token with the "voip" scope for a new user
var tokenResponse = await client.IssueTokenAsync(user, scopes: new [] { CommunicationTokenScope.VoIP });
var token =  tokenResponse.Value.Token;
var expiresOn = tokenResponse.Value.ExpiresOn;
Console.WriteLine($"\nIssued a token with 'voip' scope that expires at {expiresOn}:");
Console.WriteLine(token);
```

Benutzerzugriffstoken sind kurzlebige Anmeldeinformationen, die neu ausgestellt werden müssen, um Dienstunterbrechungen für Ihre Benutzer zu vermeiden. Die Antworteigenschaft `expiresOn` gibt die Lebensdauer des Tokens an.

## <a name="revoke-user-access-tokens"></a>Widerrufen von Benutzerzugriffstoken

Manchmal müssen Benutzerzugriffstoken ggf. explizit widerrufen werden – etwa, wenn ein Benutzer das Kennwort ändert, mit dem er sich bei Ihrem Dienst authentifiziert. Diese Funktion wird über die Azure Communication Services-Clientbibliothek „Administration“ bereitgestellt.

```csharp  
await client.RevokeTokensAsync(user);
Console.WriteLine($"\nSuccessfully revoked all tokens for user with ID: {user.Id}");
```

## <a name="delete-a-user"></a>Löschen eines Benutzers

Wird eine Identität gelöscht, werden alle aktiven Token widerrufen, und für die Identitäten können keine weiteren Token mehr ausgestellt werden. Außerdem werden alle gespeicherten Inhalte entfernt, die dem Benutzer zugeordnet sind.

```csharp
await client.DeleteUserAsync(user);
Console.WriteLine($"\nDeleted the user with ID: {user.Id}");
```

## <a name="run-the-code"></a>Ausführen des Codes

Führen Sie die Anwendung mit dem Befehl `dotnet run` aus dem Anwendungsverzeichnis aus.

```console
dotnet run
```
