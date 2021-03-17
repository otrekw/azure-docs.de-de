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
ms.openlocfilehash: dedea2a622cb0eece92bb8b57871c76daa05fb68
ms.sourcegitcommit: 4bda786435578ec7d6d94c72ca8642ce47ac628a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2021
ms.locfileid: "103495408"
---
## <a name="prerequisites"></a>Voraussetzungen
Führen Sie die folgenden Schritte aus, bevor Sie beginnen:

- Erstellen Sie ein Azure-Konto mit einem aktiven Abonnement. Details finden Sie auf der [Seite zum Erstellen eines kostenloses Azure-Kontos](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). 
- Installieren Sie [Xcode](https://developer.apple.com/xcode/) und [CocoaPods](https://cocoapods.org/). Wir verwenden Xcode, um eine iOS-Anwendung für die Schnellstartanleitung zu erstellen, und CocoaPods, um Abhängigkeiten zu installieren.
- Erstellen Sie eine Azure Communication Services-Ressource. Ausführlichere Informationen hierzu finden Sie unter [Erstellen einer Azure Communication Services-Ressource](../../create-communication-resource.md). Für diese Schnellstartanleitung müssen Sie den **Endpunkt** Ihrer Ressource aufzeichnen.
- Erstellen Sie **zwei** ACS-Benutzer, und stellen Sie ein [Benutzerzugriffstoken](../../access-tokens.md) für sie aus. Legen Sie den Bereich auf **Chat** fest, und **notieren Sie sich die Tokenzeichenfolge und die userId-Zeichenfolge**. In dieser Schnellstartanleitung wird ein Thread mit einem einzelnen Teilnehmer erstellt und anschließend ein zweiter Teilnehmer hinzugefügt.

## <a name="setting-up"></a>Einrichten

### <a name="create-a-new-ios-application"></a>Erstellen einer neuen iOS-Anwendung

Starten Sie Xcode, und wählen Sie `Create a new Xcode project` (Neues Xcode-Projekt erstellen) aus.

Wählen Sie im nächsten Fenster `iOS` als Plattform und `App` für die Vorlage aus.

Geben Sie beim Auswählen von Optionen `ChatQuickstart` als Projektnamen ein. Wählen Sie `Storyboard` als Schnittstelle, `UIKit App Delegate` als Lebenszyklus und `Swift` als Sprache aus.

Klicken Sie auf „Weiter“, und wählen Sie das Verzeichnis aus, in dem Sie das Projekt erstellen möchten.

### <a name="install-the-libraries"></a>Installieren der Bibliotheken

Wir verwenden CocoaPods, um die erforderlichen Communication Services-Abhängigkeiten zu installieren.

Navigieren Sie in der Befehlszeile zum Stammverzeichnis des iOS-Projekts `ChatQuickstart`.

Erstellen Sie eine Podfile-Datei: `pod init`

Öffnen Sie die Podfile-Datei, und fügen Sie dem Ziel `ChatQuickstart` die folgenden Abhängigkeiten hinzu:
```
pod 'AzureCommunication', '~> 1.0.0-beta.9'
pod 'AzureCommunicationChat', '~> 1.0.0-beta.9'
```

Installieren Sie die Abhängigkeiten. Dadurch wird auch ein Xcode-Arbeitsbereich erstellt: `pod install`

**Öffnen Sie das Projekt nach dem Ausführen der Podinstallation erneut in Xcode, indem Sie den neu erstellten Arbeitsbereich `.xcworkspace` auswählen.**

### <a name="setup-the-placeholders"></a>Einrichten der Platzhalter

Öffnen Sie den Arbeitsbereich `ChatQuickstart.xcworkspace` in Xcode und anschließend `ViewController.swift`.

In dieser Schnellstartanleitung fügen wir unseren Code zu `viewController` hinzu und sehen uns die Ausgabe in der Xcode-Konsole an. Das Erstellen einer Benutzeroberfläche in iOS wird in dieser Schnellstartanleitung nicht behandelt. 

Importieren Sie im oberen Bereich von `viewController.swift` die Bibliotheken `AzureCommunication` und `AzureCommunicatonChat`:

```
import AzureCommunication
import AzureCommunicationChat
```

Kopieren Sie den folgenden Code in die Methode `viewDidLoad()` von `ViewController`:

```
override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.

        let semaphore = DispatchSemaphore(value: 0)
        DispatchQueue.global(qos: .background).async {
            do {
                // <CREATE A CHAT CLIENT>
                
                // <CREATE A CHAT THREAD>
                
                // <CREATE A CHAT THREAD CLIENT>
                
                // <SEND A MESSAGE>
                
                // <ADD A USER>
                
                // <LIST USERS>
                
                // <REMOVE A USER>
            } catch {
                print("Quickstart failed: \(error.localizedDescription)")
            }
        }
    }
```

Zu Demonstrationszwecken wird Code mithilfe eines Semaphors synchronisiert. In den folgenden Schritten werden die Platzhalter durch Beispielcode ersetzt. Dabei verwenden wir die Chatbibliothek von Azure Communication Services.


### <a name="create-a-chat-client"></a>Erstellen eines Chatclients

Ersetzen Sie den Kommentar `<CREATE A CHAT CLIENT>` durch folgenden Code:

```
let endpoint = "<ACS_RESOURCE_ENDPOINT>"
    let credential =
    try CommunicationTokenCredential(
        token: "<ACCESS_TOKEN>"
    )
    let options = AzureCommunicationChatClientOptions()

    let chatClient = try ChatClient(
        endpoint: endpoint,
        credential: credential,
        withOptions: options
    )
```

Ersetzen `<ACS_RESOURCE_ENDPOINT>` Sie durch den Endpunkt Ihrer ACS-Ressource.
Ersetzen Sie `<ACCESS_TOKEN>` durch ein gültiges ACS-Zugriffstoken.

Die Erstellung einer Dienstebene zum Verwalten von Token für Ihre Chatanwendung wird in dieser Schnellstartanleitung nicht behandelt, sie wird jedoch empfohlen. Ausführlichere Informationen finden Sie in der Dokumentation zur [Chatarchitektur](../../../concepts/chat/concepts.md).

Informieren Sie sich über [Benutzerzugriffstoken](../../access-tokens.md).

## <a name="object-model"></a>Objektmodell 
Die folgenden Klassen und Schnittstellen werden für einige der wichtigsten Features der JavaScript-Clientbibliothek für Chats von Azure Communication Services verwendet.

| Name                                   | Beschreibung                                                                                                                                                                           |
| -------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ChatClient | Diese Klasse wird für die Chatfunktionalität benötigt. Sie instanziieren sie mit Ihren Abonnementinformationen und verwenden sie zum Erstellen, Abrufen und Löschen von Threads. |
| ChatThreadClient | Diese Klasse wird für die Chatthreadfunktionalität benötigt. Sie rufen eine Instanz über den ChatClient ab und verwenden sie zum Senden/Empfangen/Aktualisieren/Löschen von Nachrichten, Hinzufügen/Entfernen/Abrufen von Benutzern, Senden von Eingabebenachrichtigungen und Lesebestätigungen und Abonnieren von Chatereignissen. |

## <a name="start-a-chat-thread"></a>Starten eines Chatthreads

Nun verwenden wir unseren Chatclient (`ChatClient`), um einen neuen Thread mit zunächst einem einzelnen Benutzer zu erstellen.

Ersetzen Sie den Kommentar `<CREATE A CHAT THREAD>` durch folgenden Code:

```
let request = CreateThreadRequest(
    topic: "Quickstart",
    participants: [
        Participant(
            id: CommunicationUserIdentifier("<USER_ID>"),
            displayName: "Jack"
        )
    ]
)

var threadId: String?
chatClient.create(thread: request) { result, _ in
    switch result {
    case let .success(result):
        threadId = result.thread?.id

    case .failure:
        fatalError("Failed to create thread.")
    }
    semaphore.signal()
}
semaphore.wait()
```

Ersetzen Sie `<USER_ID>` durch eine gültige Communication Services-Benutzer-ID.

Wir verwenden hier einen Semaphor, um auf den Fertigstellungshandler zu warten, bevor der Vorgang fortgesetzt wird. Die Thread-ID (`threadId`) aus der Antwort an den Fertigstellungshandler wird in späteren Schritten verwendet.

## <a name="get-a-chat-thread-client"></a>Abrufen eines Clients für den Chatthread

Nachdem wir nun einen Chatthread erstellt haben, rufen wir als Nächstes einen Chatthreadclient (`ChatThreadClient`) ab, um Vorgänge innerhalb des Threads ausführen zu können.

Ersetzen Sie den Kommentar `<CREATE A CHAT THREAD CLIENT>` durch folgenden Code:

```
let chatThreadClient = try chatClient.createClient(forThread: threadId!)
```

## <a name="send-a-message-to-a-chat-thread"></a>Senden einer Nachricht an einen Chatthread

Ersetzen Sie den Kommentar `<SEND A MESSAGE>` durch folgenden Code:

```
let message = SendChatMessageRequest(
    content: "Hello!",
    senderDisplayName: "Jack"
)

chatThreadClient.send(message: message) { result, _ in
    switch result {
    case let .success(result):
        print("Message sent, message id: \(result.id)")
    case .failure:
        print("Failed to send message")
    }
    semaphore.signal()
}
semaphore.wait()
```

Als Erstes erstellen wir die Anforderung zum Senden von Chatnachrichten (`SendChatMessageRequest`). Sie enthält den Inhalt und den Anzeigenamen des Absenders (und optional auch den Zeitpunkt der Verlaufsfreigabe). Die an den Fertigstellungshandler zurückgegebene Antwort enthält die ID der gesendeten Nachricht.

## <a name="add-a-user-as-a-participant-to-the-chat-thread"></a>Hinzufügen eines Benutzers als Teilnehmer des Chatthreads

Ersetzen Sie den Kommentar `<ADD A USER>` durch folgenden Code:

```
let user = Participant(
    id: CommunicationUserIdentifier("<USER_ID>"),
    displayName: "Jane"
)

chatThreadClient.add(participants: [user]) { result, _ in
    switch result {
    case let .success(result):
        (result.errors != nil) ? print("Added participant") : print("Error adding participant")
    case .failure:
        print("Failed to list participants")
    }
    semaphore.signal()
}
semaphore.wait()
```

Ersetzen Sie `<USER_ID>` durch die ACS-Benutzer-ID des hinzuzufügenden Benutzers.

Wenn Sie einem Thread einen Teilnehmer hinzufügen, enthält die zurückgegebene Antwort für den Abschluss ggf. Fehler. Diese Fehler bedeuten, dass bestimmte Teilnehmer nicht hinzugefügt wurden.

## <a name="list-users-in-a-thread"></a>Auflisten der Benutzer in einem Thread

Ersetzen Sie den `<LIST USERS>`-Kommentar durch folgenden Code:

```
chatThreadClient.listParticipants { result, _ in
    switch result {
    case let .success(participants):
        var iterator = participants.syncIterator
        while let participant = iterator.next() {
            let user = participant.id as! CommunicationUserIdentifier
            print(user.identifier)
        }
    case .failure:
        print("Failed to list participants")
    }
    semaphore.signal()
}
semaphore.wait()
```


## <a name="remove-user-from-a-chat-thread"></a>Entfernen eines Benutzers aus einem Chatthread

Ersetzen Sie den `<REMOVE A USER>`-Kommentar durch folgenden Code:

```
chatThreadClient
    .remove(
        participant: CommunicationUserIdentifier("<USER_ID>")
    ) { result, _ in
        switch result {
        case .success:
            print("Removed user from the thread.")
        case .failure:
            print("Failed to remove user from the thread.")
        }
    }
```

Ersetzen Sie `<USER ID>` durch die Communication Services-Benutzer-ID des Teilnehmers, der entfernt wird.

## <a name="run-the-code"></a>Ausführen des Codes

Klicken Sie in Xcode auf die Ausführungsschaltfläche, um das Projekt zu erstellen und auszuführen. In der Konsole können Sie sich die Ausgabe des Codes sowie die Protokollierungsausgabe des Chatclients ansehen.


