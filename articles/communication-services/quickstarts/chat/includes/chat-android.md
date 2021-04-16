---
title: include file
description: include file
services: azure-communication-services
author: mikben
manager: mikben
ms.service: azure-communication-services
ms.subservice: azure-communication-services
ms.date: 03/10/2021
ms.topic: include
ms.custom: include file
ms.author: mikben
ms.openlocfilehash: b2c5237f3f7e949edbfb5486a3a17cc6e0a008a4
ms.sourcegitcommit: d23602c57d797fb89a470288fcf94c63546b1314
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/01/2021
ms.locfileid: "106178353"
---
[!INCLUDE [Public Preview Notice](../../../includes/public-preview-include-chat.md)]

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie die folgenden Schritte aus, bevor Sie beginnen:

- Erstellen Sie ein Azure-Konto mit einem aktiven Abonnement. Details finden Sie auf der [Seite zum Erstellen eines kostenloses Azure-Kontos](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Installieren Sie [Android Studio](https://developer.android.com/studio). Damit erstellen Sie in diesem Schnellstart eine Android-Anwendung zum Installieren von Abhängigkeiten.
- Erstellen Sie eine Azure Communication Services-Ressource. Ausführlichere Informationen hierzu finden Sie unter [Erstellen einer Azure Communication Services-Ressource](../../create-communication-resource.md). Für diese Schnellstartanleitung müssen Sie den **Endpunkt** Ihrer Ressource aufzeichnen.
- Erstellen Sie **zwei** Communication Services-Benutzer und [Benutzerzugriffstoken](../../access-tokens.md) dafür. Stellen Sie sicher, dass Sie als Bereich **Chat** festlegen, und **notieren Sie sich die Tokenzeichenfolge und die userId-Zeichenfolge**. In diesem Schnellstart erstellen Sie einen Thread mit einem Teilnehmer und fügen anschließend einen zweiten Teilnehmer hinzu.

## <a name="setting-up"></a>Einrichten

### <a name="create-a-new-android-application"></a>Erstellen einer neuen Android-Anwendung

1. Öffnen Sie Android Studio, und klicken Sie auf `Create a new project` (Neues Projekt erstellen). 
2. Wählen Sie im nächsten Fenster die Projektvorlage `Empty Activity` (Leere Aktivität) aus.
3. Geben Sie bei der Auswahl der Optionen `ChatQuickstart` als Projektnamen ein.
4. Klicken Sie auf „Weiter“, und wählen Sie das Verzeichnis aus, in dem Sie das Projekt erstellen möchten.

### <a name="install-the-libraries"></a>Installieren der Bibliotheken

Für die Installation der erforderlichen Communication Services-Abhängigkeiten wird in diesem Schnellstart Gradle verwendet. Navigieren Sie in der Befehlszeile zum Stammverzeichnis des Projekts `ChatQuickstart`. Öffnen Sie die Datei „build.gradle“ der App, und fügen Sie die folgenden Abhängigkeiten für das `ChatQuickstart`-Ziel hinzu:

```
implementation 'com.azure.android:azure-communication-common:1.0.0-beta.8'
implementation 'com.azure.android:azure-communication-chat:1.0.0-beta.8'
```

#### <a name="exclude-meta-files-in-packaging-options-in-root-buildgradle"></a>Ausschließen von Metadatendateien in den Paketoptionen der Stammdatei „build.gradle“
```
android {
   ...
    packagingOptions {
        exclude 'META-INF/DEPENDENCIES'
        exclude 'META-INF/LICENSE'
        exclude 'META-INF/license'
        exclude 'META-INF/NOTICE'
        exclude 'META-INF/notice'
        exclude 'META-INF/ASL2.0'
        exclude("META-INF/*.md")
        exclude("META-INF/*.txt")
        exclude("META-INF/*.kotlin_module")
    }
}
```

#### <a name="alternative-to-install-libraries-through-maven"></a>(Alternative) So installieren Sie Bibliotheken über Maven
Wenn Sie die Bibliothek über das [Maven](https://maven.apache.org/)-Buildsystem in Ihr Projekt importieren möchten, fügen Sie sie in der Datei `pom.xml` Ihrer App im Abschnitt `dependencies` hinzu, und geben Sie dabei die Artefakt-ID und die Version an, die Sie verwenden möchten:

```xml
<dependency>
  <groupId>com.azure.android</groupId>
  <artifactId>azure-communication-chat</artifactId>
  <version>1.0.0-beta.8</version>
</dependency>
```


### <a name="setup-the-placeholders"></a>Einrichten der Platzhalter

Öffnen Sie die Datei `MainActivity.java`, und bearbeiten Sie sie. In diesem Schnellstart fügen Sie den Code zu `MainActivity` hinzu und sehen sich die Ausgabe in der Konsole an. Das Erstellen einer Benutzeroberfläche wird nicht behandelt. Importieren Sie oben in der Datei die Bibliotheken `Communication common` und `Communication chat`:

```
import com.azure.android.communication.chat.*;
import com.azure.android.communication.common.*;
```

Kopieren Sie den folgenden Code in die Datei `MainActivity`:

```java
    private String secondUserId = "<second_user_id>";
    private String threadId = "<thread_id>";
    private String chatMessageId = "<chat_message_id>";
    private final String sdkVersion = "1.0.0-beta.8";
    private static final String APPLICATION_ID = "Chat Quickstart App";
    private static final String SDK_NAME = "azure-communication-com.azure.android.communication.chat";
    private static final String TAG = "--------------Chat Quickstart App-------------";

    private void log(String msg) {
        Log.i(TAG, msg);
        Toast.makeText(this, msg, Toast.LENGTH_LONG).show();
    }
    
   @Override
    protected void onStart() {
        super.onStart();
        try {
            // <CREATE A CHAT CLIENT>

            // <CREATE A CHAT THREAD>

            // <CREATE A CHAT THREAD CLIENT>

            // <SEND A MESSAGE>

            // <ADD A USER>

            // <LIST USERS>

            // <REMOVE A USER>
            
            // <<SEND A TYPING NOTIFICATION>>
            
            // <<SEND A READ RECEIPT>>
               
            // <<LIST READ RECEIPTS>>
        } catch (Exception e){
            System.out.println("Quickstart failed: " + e.getMessage());
        }
    }
```

In den folgenden Schritten werden die Platzhalter durch Beispielcode ersetzt. Dabei verwenden wir die Chatbibliothek von Azure Communication Services.


### <a name="create-a-chat-client"></a>Erstellen eines Chatclients

Ersetzen Sie den Kommentar `<CREATE A CHAT CLIENT>` durch den folgenden Code (fügen Sie die Importanweisungen oben in der Datei ein):

```java
import com.azure.android.communication.chat.ChatAsyncClient;
import com.azure.android.communication.chat.ChatClientBuilder;
import com.azure.android.core.credential.AccessToken;
import com.azure.android.core.http.HttpHeader;
import com.azure.android.core.http.okhttp.OkHttpAsyncClientProvider;
import com.azure.android.core.http.policy.BearerTokenAuthenticationPolicy;
import com.azure.android.core.http.policy.UserAgentPolicy;

final String endpoint = "https://<resource>.communication.azure.com";
final String userAccessToken = "<user_access_token>";

ChatAsyncClient chatAsyncClient = new ChatClientBuilder()
    .endpoint(endpoint)
    .credentialPolicy(new BearerTokenAuthenticationPolicy((request, callback) ->
        callback.onSuccess(new AccessToken(userAccessToken, OffsetDateTime.now().plusDays(1)))))
    .addPolicy(new UserAgentPolicy(APPLICATION_ID, SDK_NAME, sdkVersion))
    .httpClient(new OkHttpAsyncClientProvider().createInstance())
    .buildAsyncClient();

```

1. Verwenden Sie `ChatClientBuilder`, um eine Instanz von `ChatAsyncClient` zu konfigurieren und zu erstellen.
2. Ersetzen Sie `<resource>` durch Ihre Communication Services-Ressource.
3. Ersetzen Sie `<user_access_token>` durch ein gültiges Communication Services-Zugriffstoken.

## <a name="object-model"></a>Objektmodell
Die folgenden Klassen und Schnittstellen werden für einige der wichtigsten Features des JavaScript-SDKs für Chats von Azure Communication Services verwendet.

| name                                   | BESCHREIBUNG                                                                                                                                                                           |
| -------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ChatClient/ChatAsyncClient | Diese Klasse wird für die Chatfunktionalität benötigt. Sie instanziieren sie mit Ihren Abonnementinformationen und verwenden sie zum Erstellen, Abrufen und Löschen von Threads. |
| ChatThreadClient/ChatThreadAsyncClient | Diese Klasse wird für die Chatthreadfunktionalität benötigt. Sie rufen eine Instanz über den ChatClient ab und verwenden sie zum Senden/Empfangen/Aktualisieren/Löschen von Nachrichten, Hinzufügen/Entfernen/Abrufen von Benutzern, Senden von Eingabebenachrichtigungen und Lesebestätigungen und Abonnieren von Chatereignissen. |

## <a name="start-a-chat-thread"></a>Starten eines Chatthreads

Mit `ChatAsyncClient` erstellen Sie einen neuen Thread mit zunächst einem Benutzer.

Ersetzen Sie den Kommentar `<CREATE A CHAT THREAD>` durch folgenden Code:

```java
// A list of ChatParticipant to start the thread with.
List<ChatParticipant> participants = new ArrayList<>();
// The communication user ID you created before, required.
String id = "<user_id>";
// The display name for the thread participant.
String displayName = "initial participant";
participants.add(new ChatParticipant()
    .setCommunicationIdentifier(new CommunicationUserIdentifier(id))
    .setDisplayName(displayName));

// The topic for the thread.
final String topic = "General";
// Optional, set a repeat request ID.
final String repeatabilityRequestID = "";
// Options to pass to the create method.
CreateChatThreadOptions createChatThreadOptions = new CreateChatThreadOptions()
    .setTopic(topic)
    .setParticipants(participants)
    .setIdempotencyToken(repeatabilityRequestID);

CreateChatThreadResult createChatThreadResult =
    chatAsyncClient.createChatThread(createChatThreadOptions).get();
ChatThreadProperties chatThreadProperties = createChatThreadResult.getChatThreadProperties();
threadId = chatThreadProperties.getId();

```

Ersetzen Sie `<user_id>` durch eine gültige Communication Services-Benutzer-ID. Die `threadId` aus der vom Fertigstellungshandler zurückgegebenen Antwort wird in späteren Schritten noch verwendet. Ersetzen Sie daher `<thread_id>` in der Klasse durch die bei dieser Anforderung erhaltene `threadId`, und führen Sie die App erneut aus.

## <a name="get-a-chat-thread-client"></a>Abrufen eines Clients für den Chatthread

Nachdem Sie nun einen Chatthread erstellt haben, rufen Sie als Nächstes einen Chatthreadclient (`ChatThreadAsyncClient`) ab, um in diesem Thread Vorgänge ausführen zu können. Ersetzen Sie den Kommentar `<CREATE A CHAT THREAD CLIENT>` durch folgenden Code:

```
ChatThreadAsyncClient chatThreadAsyncClient = new ChatThreadClientBuilder()
    .endpoint(endpoint)
    .credentialPolicy(new BearerTokenAuthenticationPolicy((request, callback) ->
        callback.onSuccess(new AccessToken(userAccessToken, OffsetDateTime.now().plusDays(1)))))
    .addPolicy(new UserAgentPolicy(APPLICATION_ID, SDK_NAME, sdkVersion))
    .httpClient(new OkHttpAsyncClientProvider().createInstance())
    .chatThreadId(threadId)
    .buildAsyncClient();

```

## <a name="send-a-message-to-a-chat-thread"></a>Senden einer Nachricht an einen Chatthread

Stellen Sie sicher, dass Sie `<thread_id>` durch eine gültige Thread-ID ersetzt haben. Nun senden Sie eine Nachricht an diesen Thread.

Ersetzen Sie den Kommentar `<SEND A MESSAGE>` durch folgenden Code:

```java
// The chat message content, required.
final String content = "Test message 1";
// The display name of the sender, if null (i.e. not specified), an empty name will be set.
final String senderDisplayName = "An important person";
SendChatMessageOptions chatMessageOptions = new SendChatMessageOptions()
    .setType(ChatMessageType.TEXT)
    .setContent(content)
    .setSenderDisplayName(senderDisplayName);

// A string is the response returned from sending a message, it is an id, which is the unique ID of the message.
chatMessageId = chatThreadAsyncClient.sendMessage(chatMessageOptions).get().getId();

```

Nach dem Erhalt von `chatMessageId` können Sie `<chat_message_id>` durch die `chatMessageId` zur Verwendung durch Methoden zu einem späteren Zeitpunkt in diesem Schnellstart ersetzen und die App erneut ausführen.

## <a name="add-a-user-as-a-participant-to-the-chat-thread"></a>Hinzufügen eines Benutzers als Teilnehmer des Chatthreads

Ersetzen Sie den Kommentar `<ADD A USER>` durch folgenden Code:

```java
// The display name for the thread participant.
displayName = "a new participant";
ChatParticipant participant = new ChatParticipant()
    .setCommunicationIdentifier(new CommunicationUserIdentifier(secondUserId))
    .setDisplayName(secondUserDisplayName);
        
chatThreadAsyncClient.addParticipant(participant);

```

Ersetzen Sie `<second_user_id>` in der Klasse durch die Communication Services-Benutzer-ID des Benutzers, der hinzugefügt werden soll. 

## <a name="list-users-in-a-thread"></a>Auflisten der Benutzer in einem Thread

Ersetzen Sie den `<LIST USERS>`-Kommentar durch folgenden Code:

```java
// The maximum number of participants to be returned per page, optional.
final int maxPageSize = 10;

// Skips participants up to a specified position in response.
final int skip = 0;

// Options to pass to the list method.
ListParticipantsOptions listParticipantsOptions = new ListParticipantsOptions()
    .setMaxPageSize(maxPageSize)
    .setSkip(skip);

PagedResponse<ChatParticipant> firstPageWithResponse =
    chatThreadAsyncClient.getParticipantsFirstPageWithResponse(listParticipantsOptions, Context.NONE).get();

for (ChatParticipant participant : firstPageWithResponse.getValue()) {
    // You code to handle participant
}

listParticipantsNextPage(firstPageWithResponse.getContinuationToken(), 2);

```

Fügen Sie die folgende Hilfsmethode in die Klasse ein:

```java
void listParticipantsNextPage(String continuationToken, int pageNumber) {
if (continuationToken != null) {
    PagedResponse<ChatParticipant> nextPageWithResponse =
        chatThreadAsyncClient.getParticipantsNextPageWithResponse(continuationToken, Context.NONE).get();
        for (ChatParticipant participant : nextPageWithResponse.getValue()) {
            // You code to handle participant
        }
            
        listParticipantsNextPage(nextPageWithResponse.getContinuationToken(), ++pageNumber);
    }
}

```


## <a name="remove-user-from-a-chat-thread"></a>Entfernen eines Benutzers aus einem Chatthread

Stellen Sie sicher, dass Sie `<second_user_id>` durch eine gültige Benutzer-ID ersetzen. Nun entfernen Sie den zweiten Benutzer aus dem Thread.

Ersetzen Sie den `<REMOVE A USER>`-Kommentar durch folgenden Code:

```java
// Using the unique ID of the participant.
chatThreadAsyncClient.removeParticipant(new CommunicationUserIdentifier(secondUserId)).get();

```

## <a name="send-a-typing-notification"></a>Senden einer Tippbenachrichtigung

Ersetzen Sie den `<SEND A TYPING NOTIFICATION>`-Kommentar durch folgenden Code:

```java
chatThreadAsyncClient.sendTypingNotification().get();
```

## <a name="send-a-read-receipt"></a>Senden einer Lesebestätigung

Stellen Sie sicher, dass Sie `<chat_message_id>` durch eine gültige Chatnachrichten-ID ersetzen. Nun senden Sie die Lesebestätigung für diese Nachricht.

Ersetzen Sie den `<SEND A READ RECEIPT>`-Kommentar durch folgenden Code:

```java
chatThreadAsyncClient.sendReadReceipt(chatMessageId).get();
```

## <a name="list-read-receipts"></a>Auflisten von Lesebestätigungen

Ersetzen Sie den `<READ RECEIPTS>`-Kommentar durch folgenden Code:

```java
// The maximum number of participants to be returned per page, optional.
maxPageSize = 10;
// Skips participants up to a specified position in response.
skip = 0;
// Options to pass to the list method.
ListReadReceiptOptions listReadReceiptOptions = new ListReadReceiptOptions()
    .setMaxPageSize(maxPageSize)
    .setSkip(skip);

PagedResponse<ChatMessageReadReceipt> firstPageWithResponse =
    chatThreadAsyncClient.getReadReceiptsFirstPageWithResponse(listReadReceiptOptions, Context.NONE).get();

for (ChatMessageReadReceipt readReceipt : firstPageWithResponse.getValue()) {
    // You code to handle readReceipt
}

listReadReceiptsNextPage(firstPageWithResponse.getContinuationToken(), 2);

```

Fügen Sie die folgende Hilfsmethode in die Klasse ein:
```java
void listReadReceiptsNextPage(String continuationToken, int pageNumber) {
    if (continuationToken != null) {
        PagedResponse<ChatMessageReadReceipt> nextPageWithResponse =
            chatThreadAsyncClient.getReadReceiptsNextPageWithResponse(continuationToken, Context.NONE).get();

        for (ChatMessageReadReceipt readReceipt : nextPageWithResponse.getValue()) {
            // You code to handle readReceipt
        }

        listParticipantsNextPage(nextPageWithResponse.getContinuationToken(), ++pageNumber);
    }
}

```


## <a name="run-the-code"></a>Ausführen des Codes

Klicken Sie in Android Studio auf die Schaltfläche „Run“ (Ausführen), um das Projekt zu erstellen und auszuführen. In der Konsole können Sie sich die Ausgabe des Codes sowie die Protokollierungsausgabe des Chatclients ansehen.
