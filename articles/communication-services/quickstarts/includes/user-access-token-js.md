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
ms.openlocfilehash: 22cfe369561eab1ca334c7ff2450162dfae3e761
ms.sourcegitcommit: 03713bf705301e7f567010714beb236e7c8cee6f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2020
ms.locfileid: "92347171"
---
## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Node.js](https://nodejs.org/): Active LTS- und Maintenance LTS-Versionen (8.11.1 und 10.14.1 empfohlen).
- Eine aktive Communication Services-Ressource und eine Verbindungszeichenfolge. [Erstellen Sie eine Communication Services-Ressource.](../create-communication-resource.md)

## <a name="setting-up"></a>Einrichten

### <a name="create-a-new-nodejs-application"></a>Erstellen einer neuen Node.js-Anwendung

Öffnen Sie Ihr Terminal- oder Befehlsfenster, erstellen Sie ein neues Verzeichnis für Ihre App, und navigieren Sie zu diesem Verzeichnis.

```console
mkdir user-tokens-quickstart && cd user-tokens-quickstart
```

Führen Sie `npm init -y` aus, um die Datei **package.json** mit den Standardeinstellungen zu erstellen.

```console
npm init -y
```

### <a name="install-the-package"></a>Installieren des Pakets

Verwenden Sie den Befehl `npm install`, um die Azure Communication Services-Clientbibliothek „Administration“ für JavaScript zu installieren.

```console

npm install @azure/communication-administration --save

```

Durch die Option `--save` wird die Bibliothek als Abhängigkeit in der Datei **package.json** aufgeführt.

## <a name="set-up-the-app-framework"></a>Einrichten des App-Frameworks

Über das Projektverzeichnis:

1. Öffnen einer neuen Textdatei im Code-Editor
1. Fügen Sie einen Aufruf vom Typ `require` hinzu, um den Kommunikationsidentitätsclient (`CommunicationIdentityClient`) zu laden.
1. Erstellen der Struktur für das Programm, einschließlich einer einfachen Ausnahmebehandlung

Verwenden Sie zum Einstieg den folgenden Code:

```javascript
const { CommunicationIdentityClient } = require('@azure/communication-administration');

const main = async () => {
  console.log("Azure Communication Services - User Access Tokens Quickstart")

  // Quickstart code goes here
};

main().catch((error) => {
  console.log("Encountered and error");
  console.log(error);
})
```

1. Speichern Sie die neue Datei im Verzeichnis *user-tokens-quickstart* als **issue-token.js**.

[!INCLUDE [User Access Tokens Object Model](user-access-tokens-object-model.md)]

## <a name="authenticate-the-client"></a>Authentifizieren des Clients

Instanziieren Sie einen Kommunikationsidentitätsclient (`CommunicationIdentityClient`) mit Ihrer Verbindungszeichenfolge. Im folgenden Code wird die Verbindungszeichenfolge für die Ressource aus einer Umgebungsvariablen namens `COMMUNICATION_SERVICES_CONNECTION_STRING` abgerufen. Informationen zur Verwaltung der Verbindungszeichenfolge Ihrer Ressource finden Sie [hier](../create-communication-resource.md#store-your-connection-string).

Fügen Sie der `main`-Methode folgenden Code hinzu:

```javascript
// This code demonstrates how to fetch your connection string
// from an environment variable.
const connectionString = process.env['COMMUNICATION_SERVICES_CONNECTION_STRING'];

// Instantiate the user token client
const identityClient = new CommunicationIdentityClient(connectionString);
```

## <a name="create-a-user"></a>Erstellen eines Benutzers

Von Azure Communication Services wird ein einfaches Identitätsverzeichnis gepflegt. Verwenden Sie die Methode `createUser`, um in dem Verzeichnis einen neuen Eintrag mit einer eindeutigen `Id` zu erstellen. Es empfiehlt sich, eine Zuordnung zwischen den Benutzern Ihrer Anwendung und den von Communication Services generierten Identitäten zu pflegen (etwa durch Speichern in der Datenbank Ihres Anwendungsservers).

```javascript
let userResponse = await identityClient.createUser();
console.log(`\nCreated a user with ID: ${userResponse.communicationUserId}`);
```

## <a name="issue-user-access-tokens"></a>Ausstellen von Benutzerzugriffstoken

Verwenden Sie die Methode `issueToken`, um ein Zugriffstoken für einen Communication Services-Benutzer auszustellen. Ohne Angabe des optionalen Parameters `user` wird ein neuer Benutzer erstellt und mit dem Token zurückgegeben.

```javascript
// Issue an access token with the "voip" scope for a new user
let tokenResponse = await identityClient.issueToken(userResponse, ["voip"]);
const { token, expiresOn } = tokenResponse;
console.log(`\nIssued a token with 'voip' scope that expires at ${expiresOn}:`);
console.log(token);
```

Benutzerzugriffstoken sind kurzlebige Anmeldeinformationen, die neu ausgestellt werden müssen, um Dienstunterbrechungen für Ihre Benutzer zu vermeiden. Die Antworteigenschaft `expiresOn` gibt die Lebensdauer des Tokens an.

## <a name="revoke-user-access-tokens"></a>Widerrufen von Benutzerzugriffstoken

Manchmal müssen Benutzerzugriffstoken ggf. explizit widerrufen werden – etwa, wenn ein Benutzer das Kennwort ändert, mit dem er sich bei Ihrem Dienst authentifiziert. Verwenden Sie die Methode `revokeTokens`, um alle Zugriffstoken eines Benutzers ungültig zu machen.

```javascript  
await identityClient.revokeTokens(userResponse);
console.log(`\nSuccessfully revoked all tokens for user with Id: ${userResponse.communicationUserId}`);
```

## <a name="refresh-user-access-tokens"></a>Aktualisieren von Benutzerzugriffstoken

Verwenden Sie zum Aktualisieren eines Tokens das `CommunicationUser`-Objekt für die erneute Ausstellung:

```javascript  
let userResponse = new CommunicationUser(existingUserId);
let tokenResponse = await identityClient.issueToken(userResponse, ["voip"]);
```

## <a name="delete-a-user"></a>Löschen eines Benutzers

Wird ein Benutzer gelöscht, werden alle aktiven Token widerrufen, und für die Identitäten können keine weiteren Token mehr ausgestellt werden. Außerdem werden alle gespeicherten Inhalte entfernt, die dem Benutzer zugeordnet sind.

```javascript
await identityClient.deleteUser(userResponse);
console.log(`\nDeleted the user with Id: ${userResponse.communicationUserId}`);
```

## <a name="run-the-code"></a>Ausführen des Codes

Navigieren Sie an einer Konsoleneingabeaufforderung zum Verzeichnis mit der Datei *issue-token.js* , und führen Sie anschließend den folgenden Befehl vom Typ `node` aus, um die App auszuführen:

```console
node ./issue-token.js
```
