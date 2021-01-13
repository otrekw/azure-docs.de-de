---
title: include file
description: include file
services: azure-communication-services
author: mikben
manager: mikben
ms.service: azure-communication-services
ms.subservice: azure-communication-services
ms.date: 9/1/2020
ms.topic: include
ms.custom: include file
ms.author: mikben
ms.openlocfilehash: d0754ea2d7e8f8f59ec475be8e27fcffd058c11f
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91376543"
---
## <a name="prerequisites"></a>Voraussetzungen
Führen Sie die folgenden Schritte aus, bevor Sie beginnen:

- Erstellen Sie ein Azure-Konto mit einem aktiven Abonnement. Details finden Sie auf der [Seite zum Erstellen eines kostenloses Azure-Kontos](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). 
- Installieren Sie die Versionen Active LTS und Maintenance LTS (8.11.1 und 10.14.1 empfohlen) von [Node.js](https://nodejs.org/en/download/).
- Erstellen Sie eine Azure Communication Services-Ressource. Ausführlichere Informationen hierzu finden Sie unter [Erstellen einer Azure Communication Services-Ressource](../../create-communication-resource.md). Für diese Schnellstartanleitung müssen Sie den **Endpunkt** Ihrer Ressource aufzeichnen.
- Ein [Benutzerzugriffstoken](../../access-tokens.md). Achten Sie darauf, dass Sie den Bereich auf „Chat“ festlegen, und notieren Sie sich die Tokenzeichenfolge und die userId-Zeichenfolge.

## <a name="setting-up"></a>Einrichten

### <a name="create-a-new-web-application"></a>Erstellen einer neuen Webanwendung

Öffnen Sie zunächst Ihr Terminal- bzw. Befehlsfenster, erstellen Sie ein neues Verzeichnis für Ihre App, und greifen Sie auf das Verzeichnis zu.

```console
mkdir chat-quickstart && cd chat-quickstart
```
   
Führen Sie `npm init -y` aus, um die Datei **package.json** mit den Standardeinstellungen zu erstellen.

```console
npm init -y
```

Verwenden Sie einen Text-Editor, um im Stammverzeichnis des Projekts die Datei **start-chat.js** zu erstellen. In den folgenden Abschnitten fügen Sie der Datei den gesamten Quellcode für diese Schnellstartanleitung hinzu.

### <a name="install-the-packages"></a>Installieren der Pakete

Verwenden Sie den Befehl `npm install`, um die unten angegebenen Communication Services-Clientbibliotheken für JavaScript zu installieren.

```console
npm install @azure/communication-common --save

npm install @azure/communication-administration --save

npm install @azure/communication-signaling --save

npm install @azure/communication-chat --save

```

Mit der Option `--save` wird die Bibliothek als Abhängigkeit in Ihrer Datei **package.json** aufgelistet.

### <a name="set-up-the-app-framework"></a>Einrichten des App-Frameworks

In dieser Schnellstartanleitung wird Webpack verwendet, um die Anwendungsressourcen zu bündeln. Führen Sie den folgenden Befehl aus, um die npm-Pakete „webpack“, „webpack-cli“ und „webpack-dev-server“ zu installieren und als Entwicklungsabhängigkeiten in Ihrer Datei **package.json** aufzulisten:

```console
npm install webpack webpack-cli webpack-dev-server --save-dev
```

Erstellen Sie im Stammverzeichnis Ihres Projekts die Datei **index.html**. Wir verwenden diese Datei als Vorlage zum Hinzufügen der Chatfunktion mit der JavaScript-Clientbibliothek für Chats von Azure Communication Services.

Hier folgt der Code:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Communication Client - Chat Sample</title>
  </head>
  <body>
    <h4>Azure Communication Services</h4>
    <h1>Chat Quickstart</h1>
    <script src="./bundle.js"></script>
  </body>
</html>
```
Erstellen Sie im Stammverzeichnis Ihres Projekts eine Datei mit dem Namen **client.js**, die die Anwendungslogik für diese Schnellstartanleitung enthalten soll. 

### <a name="create-a-chat-client"></a>Erstellen eines Chatclients

Zum Erstellen eines Chatclients in Ihrer Web-App verwenden Sie den Communication Services-Endpunkt und das Zugriffstoken, das im Rahmen der Schritte zur Erfüllung der Voraussetzungen generiert wurde. Mit Benutzerzugriffstoken können Sie Clientanwendungen erstellen, die gegenüber Azure Communication Services direkt authentifiziert werden. Nachdem Sie diese Token auf Ihrem Server generiert haben, übergeben Sie sie zurück an ein Clientgerät. Sie müssen die `AzureCommunicationUserCredential`-Klasse aus `Common client library` verwenden, um das Token an Ihren Chatclient zu übergeben.

Erstellen Sie eine **client.js**-Datei im Stammverzeichnis Ihres Projekts. Wir verwenden diese Datei zum Hinzufügen der Chatfunktion mithilfe der JavaScript-Clientbibliothek für Chats von Azure Communication Services.

```JavaScript

import { ChatClient } from '@azure/communication-chat';
import { AzureCommunicationUserCredential } from '@azure/communication-common';

// Your unique Azure Communication service endpoint
let endpointUrl = 'https://<RESOURCE_NAME>.communication.azure.com';
let userAccessToken = '<USER_ACCESS_TOKEN>';

let chatClient = new ChatClient(endpointUrl, new AzureCommunicationUserCredential(userAccessToken));
console.log('Azure Communication Chat client created!');
```
Ersetzen Sie **ENDPOINT** durch den Endpunkt, den Sie weiter oben gemäß der Anleitung in [Erstellen einer Azure Communication Services-Ressource](../../create-communication-resource.md) erstellt haben.
Ersetzen Sie **USER_ACCESS_TOKEN** durch ein Token, das basierend auf der Anleitung unter [Benutzerzugriffstoken](../../access-tokens.md) ausgestellt wurde.
Fügen Sie diesen Code der Datei **client.js** hinzu.


### <a name="run-the-code"></a>Ausführen des Codes
Verwenden Sie `webpack-dev-server`, um Ihre App zu erstellen und auszuführen. Führen Sie den folgenden Befehl aus, um den Anwendungshost auf einem lokalen Webserver zu bündeln:
```console
npx webpack-dev-server --entry ./client.js --output bundle.js --debug --devtool inline-source-map
```
Navigieren Sie in Ihrem Browser zu http://localhost:8080/.
In Ihrem Browser in der Konsole mit den Entwicklertools sollte Folgendes angezeigt werden:

```console
Azure Communication Chat client created!
```

## <a name="object-model"></a>Objektmodell 
Die folgenden Klassen und Schnittstellen werden für einige der wichtigsten Features der JavaScript-Clientbibliothek für Chats von Azure Communication Services verwendet.

| Name                                   | Beschreibung                                                                                                                                                                           |
| -------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ChatClient | Diese Klasse wird für die Chatfunktionalität benötigt. Sie instanziieren sie mit Ihren Abonnementinformationen und verwenden sie zum Erstellen, Abrufen und Löschen von Threads. |
| ChatThreadClient | Diese Klasse wird für die Chatthreadfunktionalität benötigt. Sie rufen eine Instanz über den ChatClient ab und verwenden sie zum Senden/Empfangen/Aktualisieren/Löschen von Nachrichten, Hinzufügen/Entfernen/Abrufen von Benutzern, Senden von Eingabebenachrichtigungen und Lesebestätigungen und Abonnieren von Chatereignissen. |


## <a name="start-a-chat-thread"></a>Starten eines Chatthreads

Verwenden Sie die `createThread`-Methode, um einen Chatthread zu erstellen.

`createThreadRequest` wird zum Beschreiben der Threadanforderung verwendet:

- Verwenden Sie `topic`, um für diesen Chat ein Thema anzugeben. Nach der Erstellung des Chatthreads kann das Thema mit der Funktion `UpdateThread` aktualisiert werden. 
- Verwenden Sie `members`, um die Mitglieder aufzulisten, die dem Chatthread hinzugefügt werden sollen.

Nach Abschluss des Vorgangs gibt die `createChatThread`-Methode `threadId` zurück. Diese ID wird zum Durchführen von Vorgängen im neu erstellten Chatthread verwendet, z. B. zum Hinzufügen von Mitgliedern zum Chatthread, Senden von Nachrichten, Löschen von Nachrichten usw.

```Javascript
async function createChatThread() {
   let createThreadRequest = {
       topic: 'Preparation for London conference',
       members: [{
                   user: { communicationUserId: '<USER_ID_FOR_JACK>' },
                   displayName: 'Jack'
               }, {
                   user: { communicationUserId: '<USER_ID_FOR_GEETA>' },
                   displayName: 'Geeta'
               }]
   };
   let chatThreadClient= await chatClient.createChatThread(createThreadRequest);
   let threadId = chatThreadClient.threadId;
   return threadId;
}

createChatThread().then(async threadId => {
   console.log(`Thread created:${threadId}`);
   // PLACEHOLDERS
   // <CREATE CHAT THREAD CLIENT>
   // <RECEIVE A CHAT MESSAGE FROM A CHAT THREAD>
   // <SEND MESSAGE TO A CHAT THREAD>
   // <LIST MESSAGES IN A CHAT THREAD>
   // <ADD NEW MEMBER TO THREAD>
   // <LIST MEMBERS IN A THREAD>
   // <REMOVE MEMBER FROM THREAD>
});
```

Ersetzen Sie **USER_ID_FOR_JACK** und **USER_ID_FOR_GEETA** durch die Benutzer-IDs aus dem vorherigen Schritt (Erstellen von Benutzern und Ausstellen von [Benutzerzugriffstoken](../../access-tokens.md)).

Wenn Sie Ihre Registerkarte im Browser aktualisieren, sollte in der Konsole Folgendes angezeigt werden:
```console
Thread created: <threadId>
```

## <a name="get-a-chat-thread-client"></a>Abrufen eines Clients für den Chatthread

Die `getChatThreadClient`-Methode gibt einen `chatThreadClient` für einen bereits vorhandenen Thread zurück. Sie kann zum Durchführen von Vorgängen im erstellten Thread verwendet werden: Hinzufügen von Mitgliedern, Senden einer Nachricht usw. „threadId“ ist die eindeutige ID des vorhandenen Chatthreads.

```JavaScript

let chatThreadClient = await chatClient.getChatThreadClient(threadId);
console.log(`Chat Thread client for threadId:${chatThreadClient.threadId}`);

```
Fügen Sie diesen Code anstelle des Kommentars `<CREATE CHAT THREAD CLIENT>` in **client.js** hinzu, aktualisieren Sie Ihre Browserregisterkarte, und sehen Sie in Ihrer Konsole nach. Folgendes sollte angezeigt werden:
```console
Chat Thread client for threadId: <threadId>
```

## <a name="send-a-message-to-a-chat-thread"></a>Senden einer Nachricht an einen Chatthread

Verwenden Sie die `sendMessage`-Methode zum Senden einer Chatnachricht an den Thread, den Sie gerade erstellt haben und der anhand der „threadId“ identifiziert wird.

Mit `sendMessageRequest` werden die erforderlichen Felder der Anforderung für die Chatnachricht beschrieben:

- Verwenden Sie `content`, um den Inhalt der Chatnachricht anzugeben.

Mit `sendMessageOptions` werden die optionalen Felder der Anforderung für die Chatnachricht beschrieben:

- Verwenden Sie `priority`, um die Prioritätsstufe für die Chatnachricht anzugeben, z. B. „Normal“ oder „High“ (Hoch). Diese Eigenschaft kann verwendet werden, um für den empfangenden Benutzer in Ihrer App einen Hinweis in der Benutzeroberfläche anzuzeigen, auf die Nachricht aufmerksam zu machen oder benutzerdefinierte Geschäftslogik auszuführen.   
- Verwenden Sie `senderDisplayName`, um den Anzeigenamen des Absenders anzugeben.

Die Antwort `sendChatMessageResult` enthält eine „ID“, bei der es sich um die eindeutige ID der Nachricht handelt.

```JavaScript

let sendMessageRequest =
{
    content: 'Hello Geeta! Can you share the deck for the conference?'
};
let sendMessageOptions =
{
    priority: 'Normal',
    senderDisplayName : 'Jack'
};
let sendChatMessageResult = await chatThreadClient.sendMessage(sendMessageRequest, sendMessageOptions);
let messageId = sendChatMessageResult.id;
console.log(`Message sent!, message id:${messageId}`);

```
Fügen Sie diesen Code anstelle des Kommentars `<SEND MESSAGE TO A CHAT THREAD>` in **client.js** hinzu, aktualisieren Sie Ihre Browserregisterkarte, und sehen Sie in Ihrer Konsole nach.
```console
Message sent!, message id:<number>
```

## <a name="receive-chat-messages-from-a-chat-thread"></a>Empfangen von Chatnachrichten aus einem Chatthread

Bei der Echtzeitsignalisierung können Sie eine Funktion abonnieren, mit der auf neue eingehende Nachrichten gelauscht wird und die aktuellen Nachrichten im Arbeitsspeicher entsprechend aktualisiert werden. Azure Communication Services unterstützt eine [Liste mit Ereignissen, die Sie abonnieren können](../../../concepts/chat/concepts.md#real-time-signaling).

```JavaScript
// open notifications channel
await chatClient.startRealtimeNotifications();
// subscribe to new notification
chatClient.on("chatMessageReceived", (e) => {
    console.log("Notification chatMessageReceived!");
    // your code here
});

```
Fügen Sie diesen Code anstelle des Kommentars `<RECEIVE A CHAT MESSAGE FROM A CHAT THREAD>` in **client.js** hinzu.
Wenn Sie die Registerkarte in Ihrem Browser aktualisieren, sollte in der Konsole die Meldung `Notification chatMessageReceived` angezeigt werden.

Alternativ können Sie Chatnachrichten auch abrufen, indem Sie die `listMessages`-Methode jeweils nach dem angegebenen Intervallzeitraum abfragen. 

```JavaScript

let pagedAsyncIterableIterator = await chatThreadClient.listMessages();
let nextMessage = await pagedAsyncIterableIterator.next();
 while (!nextMessage.done) {
     let chatMessage = nextMessage.value;
     console.log(`Message :${chatMessage.content}`);
     // your code here
     nextMessage = await pagedAsyncIterableIterator.next();
 }

```
Fügen Sie diesen Code anstelle des Kommentars `<LIST MESSAGES IN A CHAT THREAD>` in **client.js** hinzu.
Aktualisieren Sie Ihre Registerkarte. In der Konsole sollte eine Liste mit den Nachrichten angezeigt werden, die in diesem Chatthread gesendet wurden.


`listMessages` gibt die aktuelle Version der Nachricht zurück, einschließlich aller Bearbeitungen oder Löschungen, die für die Nachricht mit `updateMessage` und `deleteMessage` durchgeführt wurden.
Für gelöschte Nachrichten wird von `chatMessage.deletedOn` ein datetime-Wert zurückgegeben, mit dem der Löschzeitpunkt der Nachricht angegeben wird. Für bearbeitete Nachrichten gibt `chatMessage.editedOn` einen datetime-Wert zurück, mit dem der Bearbeitungszeitpunkt der Nachricht angegeben wird. Auf den ursprünglichen Zeitpunkt der Nachrichtenerstellung kann mit `chatMessage.createdOn` zugegriffen werden. Dieses Element kann zum Sortieren der Nachrichten genutzt werden.

Mit `listMessages` werden unterschiedliche Nachrichtentypen zurückgegeben, die mit `chatMessage.type` identifiziert werden können. Diese Typen lauten:

- `Text`: Reguläre Chatnachricht, die von einem Threadmitglied gesendet wurde.

- `ThreadActivity/TopicUpdate`: Systemnachricht mit dem Hinweis, dass das Thema aktualisiert wurde.

- `ThreadActivity/AddMember`: Systemnachricht mit dem Hinweis, dass dem Chatthread mindestens ein Mitglied hinzugefügt wurde.

- `ThreadActivity/RemoveMember`: Systemnachricht mit dem Hinweis, dass ein Mitglied aus dem Chatthread entfernt wurde.

Weitere Details finden Sie unter [Nachrichtentypen](../../../concepts/chat/concepts.md#message-types).

## <a name="add-a-user-as-member-to-the-chat-thread"></a>Hinzufügen eines Benutzers als Mitglied des Chatthreads

Nach der Erstellung eines Chatthreads können Sie dafür Benutzer hinzufügen und entfernen. Indem Sie Benutzer hinzufügen, gewähren Sie ihnen Zugriff zum Senden von Nachrichten an den Chatthread und zum Hinzufügen/Entfernen anderer Mitglieder. Stellen Sie vor dem Aufrufen der `addMembers`-Methode sicher, dass Sie ein neues Zugriffstoken und eine Identität für den Benutzer beschafft haben. Der Benutzer benötigt dieses Zugriffstoken, um den Chatclient initialisieren zu können.

Mit `addMembersRequest` wird das Anforderungsobjekt beschrieben, und darin wird `members` verwendet, um die hinzuzufügenden Mitglieder für den Chatthread aufzulisten.
- `user` (erforderlich) ist der Kommunikationsbenutzer, der dem Chatthread hinzugefügt werden soll.
- `displayName` (optional) ist der Anzeigename für das Threadmitglied.
- `shareHistoryTime` (optional) ist der Zeitpunkt, ab dem der Chatverlauf für das Mitglied freigegeben wird. Sie können den Verlauf seit dem Beginn des Chatthreads freigeben, indem Sie diese Eigenschaft auf das Datum der Threaderstellung (oder früher) festlegen. Geben Sie das aktuelle Datum an, um den Verlauf vor dem Hinzufügezeitpunkt des Mitglieds nicht freizugeben. Geben Sie ein Datum Ihrer Wahl an, um einen Teil des Verlaufs freizugeben.

```JavaScript

let addMembersRequest =
{
    members: [
        {
            user: { communicationUserId: '<NEW_MEMBER_USER_ID>' },
            displayName: 'Jane'
        }
    ]
};

await chatThreadClient.addMembers(addMembersRequest);

```
Ersetzen Sie **NEW_MEMBER_USER_ID** durch eine [neue Benutzer-ID](../../access-tokens.md). Fügen Sie diesen Code anstelle des Kommentars `<ADD NEW MEMBER TO THREAD>` in der Datei **client.js** hinzu.

## <a name="list-users-in-a-chat-thread"></a>Auflisten von Benutzern in einem Chatthread
```JavaScript
async function listThreadMembers() {
   let pagedAsyncIterableIterator = await chatThreadClient.listMembers();
   let next = await pagedAsyncIterableIterator.next();
   while (!next.done) {
      let user = next.value;
      console.log(`User :${user.displayName}`);
      next = await pagedAsyncIterableIterator.next();
   }
}
await listThreadMembers();
```
Fügen Sie diesen Code anstelle des Kommentars `<LIST MEMBERS IN A THREAD>` in **client.js** hinzu, aktualisieren Sie Ihre Browserregisterkarte, und sehen Sie in Ihrer Konsole nach. Es sollten Informationen zu den Benutzern eines Threads angezeigt werden.

## <a name="remove-user-from-a-chat-thread"></a>Entfernen eines Benutzers aus einem Chatthread

Sie können Mitglieder einem Chatthread nicht nur hinzufügen, sondern diese auch daraus entfernen. Zum Entfernen müssen Sie die IDs der von Ihnen hinzugefügten Mitglieder nachverfolgen.

Verwenden Sie die `removeMember`-Methode. Hierbei ist `member` der Kommunikationsbenutzer, der aus dem Thread entfernt werden soll.

```JavaScript

await chatThreadClient.removeMember({ communicationUserId: <MEMBER_ID> });
await listThreadMembers();
```
Ersetzen Sie **MEMBER_ID** durch eine Benutzer-ID aus dem vorherigen Schritt (<NEW_MEMBER_USER_ID>).
Fügen Sie diesen Code anstelle des Kommentars `<REMOVE MEMBER FROM THREAD>` in **client.js** hinzu.
