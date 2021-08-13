---
title: Datei einfügen
description: Datei einfügen
services: Communication Services
author: shahen
manager: anvalent
ms.service: Communication Services
ms.subservice: Communication Services
ms.date: 06/30/2021
ms.topic: include
ms.custom: include file
ms.author: shahen
ms.openlocfilehash: 0193f80952e7f9ea5878a99de0189eb0b5911b0d
ms.sourcegitcommit: 98308c4b775a049a4a035ccf60c8b163f86f04ca
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/30/2021
ms.locfileid: "114471433"
---
## <a name="setting-up"></a>Einrichten

### <a name="create-a-new-nodejs-application"></a>Erstellen einer neuen Node.js-Anwendung

Öffnen Sie Ihr Terminal- oder Befehlsfenster, erstellen Sie ein neues Verzeichnis für Ihre App, und navigieren Sie zu diesem Verzeichnis.

```console
mkdir relay-token-quickstart && cd relay-token-quickstart
```
Führen Sie `npm init -y` aus, um die Datei **package.json** mit den Standardeinstellungen zu erstellen.

```console
npm init -y
```

### <a name="install-the-package"></a>Installieren des Pakets

Verwenden Sie den Befehl `npm install`, um die Clientbibliothek für Identitäten von Azure Communication Services sowie die Network Traversal-Bibliothek für JavaScript zu installieren.

```console
npm install @azure/communication-identity --save
npm install @azure/communication-network-traversal --save
```

Mit der Option `--save` wird die Bibliothek als Abhängigkeit in Ihrer Datei **package.json** aufgelistet.

### <a name="set-up-the-app-framework"></a>Einrichten des App-Frameworks

Über das Projektverzeichnis:

1. Öffnen einer neuen Textdatei im Code-Editor
2. Fügen Sie die Direktiven vom Typ `require` am Anfang der Datei hinzu, um das Azure Communication Identity SDK und das Network Traversal SDK zu verwenden.
3. Erstellen der Struktur für das Programm, einschließlich einer einfachen Ausnahmebehandlung

   Verwenden Sie zum Einstieg den folgenden Code:

   ```javascript
   const { CommunicationIdentityClient } = require("@azure/communication-identity");
   const { CommunicationRelayClient } = require("@azure/communication-network-traversal");;

   const main = async () => {
     console.log("Azure Communication Services - Relay Token Quickstart")
  
     // Quickstart code goes here
   };

   main().catch((error) => {
     console.log("Encountered and error");
     console.log(error);
   })
   ```

4. Speichern Sie die neue Datei im Verzeichnis *user-tokens-quickstart* als **relay-token.js**.

### <a name="authenticate-the-client"></a>Authentifizieren des Clients

Instanziieren Sie einen Kommunikationsidentitätsclient (`CommunicationIdentityClient`) mit Ihrer Verbindungszeichenfolge. Im folgenden Code wird die Verbindungszeichenfolge für die Ressource aus einer Umgebungsvariablen namens `COMMUNICATION_SERVICES_CONNECTION_STRING` abgerufen. Informationen zur [Verwaltung der Verbindungszeichenfolge Ihrer Ressource](../create-communication-resource.md#store-your-connection-string).

Fügen Sie der `main`-Methode folgenden Code hinzu:

```javascript
// This code demonstrates how to fetch your connection string
// from an environment variable.
const connectionString = process.env['COMMUNICATION_SERVICES_CONNECTION_STRING'];

// Instantiate the identity client
const identityClient = new CommunicationIdentityClient(connectionString);
```

## <a name="create-an-identity"></a>Erstellen einer Identität

Von Azure Communication Services wird ein einfaches Identitätsverzeichnis gepflegt. Verwenden Sie die Methode `createUser`, um in dem Verzeichnis einen neuen Eintrag mit einer eindeutigen `Id` zu erstellen. Speichern Sie die empfangene Identität mit einer Zuordnung zu den Benutzern Ihrer Anwendung. Beispielsweise, indem Sie sie in der Datenbank des Anwendungsservers speichern. Die Identität ist später erforderlich, um Zugriffstoken auszustellen.

```javascript
let identityResponse = await identityClient.createUser();
console.log(`\nCreated an identity with ID: ${identityResponse.communicationUserId}`);
```

### <a name="exchange-the-user-access-token-for-a-relay-token"></a>Austauschen des Benutzerzugriffstokens gegen ein Relaytoken

Rufen Sie den Azure Communication-Tokendienst auf, um das Benutzerzugriffstoken gegen ein TURN-Diensttoken zu tauschen.

```javascript
    const relayClient = new CommunicationRelayClient(connectionString);
    console.log("Getting relay configuration");

    const config = await relayClient.getRelayConfiguration(identityResponse);
    console.log("RelayConfig", config);
```

### <a name="use-the-token-on-the-client-as-an-ice-candidate"></a>Verwenden des Tokens auf dem Client als ICE-Kandidat

Das Token kann jetzt deserialisiert und als ICE-Kandidat mit WebRTC im Clientbrowser hinzugefügt werden.

```javascript  
var configuration = { iceServers: iceServers };
var peerConnection = new RTCPeerConnection(configuration);
```

## <a name="run-the-code"></a>Ausführen des Codes

Navigieren Sie an einer Konsoleneingabeaufforderung zum Verzeichnis mit der Datei *issue-token.js*, und führen Sie anschließend den folgenden Befehl vom Typ `node` aus, um die App auszuführen:

```console
node ./relay-token.js
```
