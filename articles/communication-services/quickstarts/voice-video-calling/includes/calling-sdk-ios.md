---
author: mikben
ms.service: azure-communication-services
ms.topic: include
ms.date: 9/1/2020
ms.author: mikben
ms.openlocfilehash: fa7fd73a7d8019919a89dd9e9522b7389dc9c18f
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90931641"
---
## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). 
- Eine bereitgestellte Communication Services-Ressource. [Erstellen Sie eine Communication Services-Ressource](../../create-communication-resource.md).
- Ein `User Access Token`, um den Anrufclient zu aktivieren. Weitere Informationen zum [Abrufen eines `User Access Token`](../../access-tokens.md)
- Optional: Gehen Sie den Schnellstart [Erste Schritte beim Hinzufügen von Anruffunktionen zu einer Anwendung](../getting-started-with-calling.md) durch.

## <a name="setting-up"></a>Einrichten

### <a name="creating-the-xcode-project"></a>Erstellen des Xcode-Projekts

Erstellen Sie in Xcode ein neues iOS-Projekt, und wählen Sie die Vorlage **Single View App** aus. In diesem Schnellstart wird das [SwiftUI-Framework](https://developer.apple.com/xcode/swiftui/) verwendet, weshalb Sie **Language** auf **Swift** und **User Interface** auf **SwiftUI** festlegen müssen. Während dieses Schnellstarts werden keine Komponenten- oder Benutzeroberflächentests erstellt. Sie können daher **Include Unit Tests** und auch **Include UI Tests** deaktivieren.

:::image type="content" source="../media/ios/xcode-new-ios-project.png" alt-text="Screenshot des Fensters „Create new project“ in Xcode":::

### <a name="install-the-package"></a>Installieren des Pakets

Fügen Sie Ihrem Clientprojekt die Azure Communication Services-Clientbibliothek „Calling“ und ihre Abhängigkeiten (AzureCore.framework und AzureCommunication.framework) hinzu.

> [!NOTE]
> Mit der Veröffentlichung des AzureCommunicationCalling SDK steht Ihnen das Bash-Skript `BuildAzurePackages.sh` zur Verfügung. Das Skript zur Ausführung von `sh ./BuildAzurePackages.sh` gibt Ihnen den Pfad zu den generierten Frameworkpaketen an, die im nächsten Schritt in die Beispiel-App importiert werden müssen. Beachten Sie, dass Sie Xcode-Befehlszeilentools einrichten müssen, falls noch nicht vor der Ausführung des Skripts geschehen: Starten Sie Xcode, und wählen Sie „Preferences-> Locations“ aus. Wählen Sie Ihre Xcode-Version für die Befehlszeilentools aus.

1. Laden Sie die Azure Communication Services-Clientbibliothek „Calling“ für iOS herunter.
2. Klicken Sie in Xcode auf Ihre Projektdatei, und wählen Sie das Buildziel aus, um den Editor für Projekteinstellungen zu öffnen.
3. Scrollen Sie unter der Registerkarte **General** zum Abschnitt **Frameworks, Libraries, and Embedded Content**, und klicken Sie auf das **+** -Symbol.
4. Wählen Sie links unten im Dialogfeld **Add Files** aus. Navigieren Sie zum Verzeichnis **AzureCommunicationCalling.framework** des dekomprimierten Clientbibliothekspakets.
    1. Wiederholen Sie den letzten Schritt zum Hinzufügen von **AzureCore.framework** und **AzureCommunication.framework**.
5. Öffnen Sie im Editor für Projekteinstellungen die Registerkarte **Build Settings**, und scrollen Sie zum Abschnitt **Search Paths**. Fügen Sie dem Verzeichnis mit **AzureCommunicationCalling.framework** den neuen Eintrag **Framework Search Paths** hinzu.
    1. Fügen Sie einen weiteren Eintrag des Typs „Framework Search Paths“ hinzu, der auf den Ordner mit den Abhängigkeiten zeigt.

:::image type="content" source="../media/ios/xcode-framework-search-paths.png" alt-text="Screenshot der Aktualisierung der Frameworksuchpfade in Xcode":::

### <a name="request-access-to-the-microphone"></a>Anfordern des Zugriffs auf das Mikrofon

Um auf das Mikrofon des Geräts zugreifen zu können, müssen Sie die Liste der Informationseigenschaften Ihrer App mit `NSMicrophoneUsageDescription` aktualisieren. Sie legen den zugehörigen Wert auf einen `string` fest, der in den Dialogfeld aufgenommen wird, das das System verwendet, um vom Benutzer den Zugriff anzufordern.

Klicken Sie mit der rechten Maustaste auf den Eintrag `Info.plist` der Projektstruktur, und wählen Sie anschließend **Open As** > **Source Code** aus. Fügen Sie die folgenden Zeilen im Abschnitt `<dict>` der obersten Ebene hinzu, und speichern dann Sie die Datei.

```xml
<key>NSMicrophoneUsageDescription</key>
<string>Need microphone access for VOIP calling.</string>
```

### <a name="set-up-the-app-framework"></a>Einrichten des App-Frameworks

Öffnen Sie die Datei **ContentView.swift** Ihres Projekts, und fügen Sie am Anfang der Datei eine `import`-Deklaration hinzu, um die `AzureCommunicationCalling library` zu importieren. Importieren Sie außerdem `AVFoundation`, was wir für die Berechtigungsanforderung für Audio im Code benötigen.

```swift
import AzureCommunicationCalling
import AVFoundation
```

## <a name="object-model"></a>Objektmodell

Die folgenden Klassen und Schnittstellen befassen sich mit einigen der wichtigsten Features der Azure Communication Services-Clientbibliothek „Calling“ für iOS.


| Name                                  | Beschreibung                                                  |
| ------------------------------------- | ------------------------------------------------------------ |
| ACSCallClient | ACSCallClient ist der Haupteinstiegspunkt in die Clientbibliothek „Calling“.|
| ACSCallAgent | ACSCallAgent dient zum Starten und Verwalten von Anrufen. |
| CommunicationUserCredential | CommunicationUserCredential dient als tokengestützte Anmeldeinformationen zum Instanziieren von CallAgent.| 
| CommunicationIndentifier | CommunicationIndentifier wird zur Darstellung der Identität des Benutzers verwendet, die eine der folgenden sein: CommunicationUser/PhoneNumber/CallingApplication. |

> [!NOTE]
> Bei der Implementierung von Ereignisdelegaten muss die Anwendung einen eindeutigen Verweis auf die Objekte enthalten, die Ereignisabonnements erfordern. Wenn beispielsweise ein `ACSRemoteParticipant`-Objekt beim Aufrufen der `call.addParticipant`-Methode zurückgegeben wird und die Anwendung den Delegaten auf das Lauschen auf `ACSRemoteParticipantDelegate` festlegt, muss die Anwendung einen eindeutigen Verweis auf das `ACSRemoteParticipant`-Objekt enthalten. Andernfalls löst der Delegat, wenn dieses Objekt erfasst wird, eine schwerwiegende Ausnahme aus, sobald das aufrufende SDK versucht, das Objekt aufzurufen.

## <a name="initialize-the-acscallagent"></a>Initialisieren von ACSCallAgent

Um eine `ACSCallAgent`-Instanz anhand von `ACSCallClient` zu erzeugen, müssen Sie die `callClient.createCallAgent`-Methode verwenden, die ein `ACSCallAgent`-Objekt asynchron zurückgibt, nachdem es initialisiert wurde.

Um einen Anrufclient zu erstellen, müssen Sie ein `CommunicationUserCredential`-Objekt übergeben.

```swift

import AzureCommunication

let tokenString = "token_string"
var userCredential: CommunicationUserCredential?
do {
    userCredential = try CommunicationUserCredential(
        initialToken: tokenString, refreshProactively: true,
        tokenRefresher: self.fetchTokenSync
    )
} catch {
    print("Failed to create CommunicationCredential object")
}

// tokenProvider needs to be implemented by contoso which fetches new token
public func fetchTokenSync(then onCompletion: TokenRefreshOnCompletion) {
    let newToken = self.tokenProvider!.fetchNewToken()
    onCompletion(newToken, nil)
}
```

Übergeben des zuvor erstellten CommunicationUserCredential-Objekts an ACSCallClient

```swift

callClient = ACSCallClient()
callClient?.createCallAgent(userCredential!,
    withCompletionHandler: { (callAgent, error) in
        if error != nil {
            print("Create agent succeeded")
            self.callAgent = callAgent
        } else {
            print("Create agent failed")
        }
})

```

## <a name="place-an-outgoing-call"></a>Tätigen eines ausgehenden Anrufs

Um einen Anruf zu erstellen und zu starten, müssen Sie eine der APIs für `ACSCallAgent` aufrufen und die Communication Services-Identität eines Benutzers angeben, den Sie über die Communication Services-Clientbibliothek „Administration“ bereitgestellt haben.

Erstellung und Start des Anrufs erfolgen synchron. Sie erhalten eine Anrufinstanz, die es Ihnen ermöglicht, alle Ereignisse im Zusammenhang mit dem Anruf zu abonnieren.

### <a name="place-a-11-call-to-a-user-or-a-1n-call-with-users-and-pstn"></a>Tätigen eines 1:1-Anrufs eines Benutzers oder eines 1:n-Anrufs mit Benutzern und Festnetznummern

```swift

let callees = [CommunicationUser(identifier: 'acsUserId')]
let oneToOneCall = self.CallingApp.callAgent.call(participants: callees, options: ACSStartCallOptions())

```

### <a name="place-a-1n-call-with-users-and-pstn"></a>1:n-Anruf mit Benutzern und Festnetznummern
Um den Anruf im Telefonfestnetz zu tätigen, müssen Sie die Telefonnummer angeben, die Sie mit Communication Services bezogen haben.
```swift

let pstnCallee = PhoneNumber('+1999999999')
let callee = CommunicationUser(identifier: 'acsUserId')
let groupCall = self.CallingApp.callAgent.call(participants: [pstnCallee, callee], options: ACSStartCallOptions())

```

### <a name="place-a-11-call-with-with-video"></a>Tätigen eines 1:1-Anrufs mit Video
Informationen zum Abrufen der Geräte-Manager-Instanz finden Sie [hier](#device-management).

```swift

let camera = self.deviceManager!.getCameraList()![0]
let localVideoStream = ACSLocalVideoStream(camera)
let videoOptions = ACSVideoOptions(localVideoStream)

let startCallOptions = ACSStartCallOptions()
startCallOptions?.videoOptions = videoOptions

let callee = CommunicationUser(identifier: 'acsUserId')
let call = self.callAgent?.call([callee], options: startCallOptions)

```

### <a name="join-a-group-call"></a>Teilnehmen an einem Gruppenanruf
Um an einem Anruf teilzunehmen, müssen Sie eine der APIs für *CallAgent* aufrufen.

```swift

let groupCallContext = ACSGroupCallContext()
groupCallContext?.groupId = UUID(uuidString: "uuid_string")!
let call = self.callAgent?.join(with: groupCallContext, joinCallOptions: ACSJoinCallOptions())

```

## <a name="push-notification"></a>Pushbenachrichtigung

Eine mobile Pushbenachrichtigung ist die Popupbenachrichtigung, die Sie auf dem Mobilgerät erhalten. Bei Anruffunktionen konzentrieren wir uns auf VoIP-Pushbenachrichtigungen (Voice over Internet Protocol). Wir bieten Ihnen die Möglichkeit, sich für Pushbenachrichtigungen zu registrieren, diese zu bearbeiten und ihre Registrierung aufzuheben.

### <a name="prerequisite"></a>Voraussetzungen

- Schritt 1: Xcode -> Signing & Capabilities -> Add Capability -> Push Notifications
- Schritt 2: Xcode -> Signing & Capabilities -> Add Capability -> Background Modes
- Schritt 3: Background Modes -> „Voice over IP“ und „Remote notifications“ auswählen

:::image type="content" source="../media/ios/xcode-push-notification.png" alt-text="Screenshot, der das Hinzufügen von Funktionen in Xcode zeigt" lightbox="../media/ios/xcode-push-notification.png":::

#### <a name="register-for-push-notifications"></a>Registrieren für Pushbenachrichtigungen

Um sich für Pushbenachrichtigungen zu registrieren, rufen Sie registerPushNotification() für eine *CallAgent*-Instanz mit einem Geräteregistrierungstoken auf.

Die Registrierung für die Pushbenachrichtigung muss nach erfolgreicher Initialisierung aufgerufen werden. Sobald das `callAgent`-Objekt zerstört wurde, wird `logout` aufgerufen, wodurch die Registrierung von Pushbenachrichtigungen automatisch aufgehoben wird.


```swift

let deviceToken: Data = pushRegistry?.pushToken(for: PKPushType.voIP)
callAgent.registerPushNotifications(deviceToken,
                withCompletionHandler: { (error) in
    if(error == nil) {
        print("Successfully registered to push notification.")
    } else {
        print("Failed to register push notification.")
    }
})

```

#### <a name="push-notification-handling"></a>Behandlung von Pushbenachrichtigungen
Um Pushbenachrichtigungen für eingehende Anrufe zu empfangen, rufen Sie *handlePushNotification()* für eine *CallAgent*-Instanz mit Wörterbuchnutzdaten auf.

```swift

let dictionaryPayload = pushPayload?.dictionaryPayload
callAgent.handlePushNotification(dictionaryPayload, withCompletionHandler: { (error) in
    if (error != nil) {
        print("Handling of push notification failed")
    } else {
        print("Handling of push notification was successful")
    }
})

```
#### <a name="unregister-push-notification"></a>Aufheben der Registrierung der Pushbenachrichtigung

Anwendungen können die Registrierung der Pushbenachrichtigung jederzeit aufheben. Rufen Sie einfach die `unRegisterPushNotification`-Methode für *CallAgent* auf.
> [!NOTE]
> Die Registrierung von Anwendungen für Pushbenachrichtigungen wird bei der Abmeldung nicht automatisch aufgehoben.

```swift

callAgent.unRegisterPushNotifications(completionHandler: { (error) in
    if (error != nil) {
        print("Unregister of push notification failed, please try again")
    } else {
        print("Unregister of push notification was successful")
    }
})

```

## <a name="mid-call-operations"></a>Vorgänge im Verlauf des Anrufs

Sie können während eines Anrufs verschiedene Vorgänge ausführen, um Einstellungen für Video und Audio zu verwalten.

### <a name="mute-and-unmute"></a>Stummschalten und Aufheben der Stummschaltung

Zum Stummschalten oder Aufheben der Stummschaltung des lokalen Endpunkts können Sie die asynchronen APIs `mute` und `unmute` verwenden:

```swift
call.mute(completionHandler: { (error) in
    if error == nil {
        print("Successfully muted")
    } else {
        print("Failed to mute")
    }
})

```

[Asynchron] Lokale Stummschaltung

```swift
call.unmute(completionHandler:{ (error) in
    if error == nil {
        print("Successfully un-muted")
    } else {
        print("Failed to unmute")
    }
})
```

### <a name="start-and-stop-sending-local-video"></a>Starten und Beenden des Sendens von lokalem Video

Um damit zu beginnen, lokale Videos an andere Anrufteilnehmer zu senden, verwenden Sie die `startVideo`-API, und übergeben Sie `localVideoStream` mit `camera`.

```swift

let firstCamera: ACSVideoDeviceInfo? = self.deviceManager?.getCameraList()![0]
let localVideoStream = ACSLocalVideoStream(firstCamera)

call.startVideo(localVideoStream) { (error) in
    if (error == nil) {
        print("Local video started successfully")
    }
    else {
        print("Local video failed to start")
    }
}

```

Sobald Sie mit dem Senden von Video beginnen, wird die `ACSLocalVideoStream`-Instanz der `localVideoStreams`-Sammlung für eine Anrufinstanz hinzugefügt:

```swift

call.localVideoStreams[0]

```

[Asynchron] Um lokales Video zu beenden, übergeben Sie den `localVideoStream`, der vom Aufruf von `call.startVideo` zurückgegeben wurde:

```swift

call.stopVideo(localVideoStream,{ (error) in
    if (error == nil) {
        print("Local video stopped successfully")
    }
    else {
        print("Local video failed to stop")
    }
}

```

## <a name="remote-participants-management"></a>Verwaltung von Remoteteilnehmern

Alle Remoteteilnehmer werden durch den Typ `ACSRemoteParticipant` dargestellt und sind über die `remoteParticipants`-Sammlung für eine Anrufinstanz verfügbar:

### <a name="list-participants-in-a-call"></a>Auflisten der Teilnehmer an einem Anruf

```swift

call.remoteParticipants

```

### <a name="remote-participant-properties"></a>Eigenschaften von Remoteteilnehmern

```swift

// [ACSRemoteParticipantDelegate] delegate - an object you provide to receive events from this ACSRemoteParticipant instance
var remoteParticipantDelegate = remoteParticipant.delegate

// [CommunicationIdentifier] identity - same as the one used to provision token for another user
var identity = remoteParticipant.identity

// ACSParticipantStateIdle = 0, ACSParticipantStateEarlyMedia = 1, ACSParticipantStateConnecting = 2, ACSParticipantStateConnected = 3, ACSParticipantStateOnHold = 4, ACSParticipantStateInLobby = 5, ACSParticipantStateDisconnected = 6
var state = remoteParticipant.state

// [ACSError] callEndReason - reason why participant left the call, contains code/subcode/message
var callEndReason = remoteParticipant.callEndReason

// [Bool] isMuted - indicating if participant is muted
var isMuted = remoteParticipant.isMuted

// [Bool] isSpeaking - indicating if participant is currently speaking
var isSpeaking = remoteParticipant.isSpeaking

// ACSRemoteVideoStream[] - collection of video streams this participants has
var videoStreams = remoteParticipant.videoStreams // [ACSRemoteVideoStream, ACSRemoteVideoStream, ...]

```

### <a name="add-a-participant-to-a-call"></a>Hinzufügen eines Teilnehmers zu einem Anruf

Um einen Teilnehmer einem Anruf hinzuzufügen (entweder als Benutzer oder Telefonnummer), können Sie `addParticipant` aufrufen. Dadurch wird die Instanz eines Remoteteilnehmers synchron zurückgegeben.

```swift

let remoteParticipantAdded: ACSRemoteParticipant = call.addParticipant(CommunicationUser(identifier: "userId"))

```

### <a name="remove-a-participant-from-a-call"></a>Entfernen eines Teilnehmers aus einem Anruf
Um einen Teilnehmer aus einem Anruf zu entfernen (entweder als Benutzer oder Telefonnummer), können Sie die API `removeParticipant` aufrufen. Dieser Vorgang wird asynchron aufgelöst.

```swift

call!.remove(remoteParticipantAdded) { (error) in
    if (error == nil) {
        print("Successfully removed participant")
    } else {
        print("Failed to remove participant")
    }
}

```

## <a name="render-remote-participant-video-streams"></a>Rendern von Videostreams von Remoteteilnehmern

Remoteeilnehmer können während eines Anrufs eine Video- oder Bildschirmübertragung veranlassen.

### <a name="handle-remote-participant-videoscreen-sharing-streams"></a>Verarbeiten von Video-/Bildschirmübertragungs-Streams für Remoteteilnehmer

Prüfen Sie die `videoStreams`-Sammlungen, um die Streams von Remoteteilnehmern aufzulisten:

```swift

var remoteParticipantVideoStream = call.remoteParticipants[0].videoStreams[0]

```

### <a name="remote-video-stream-properties"></a>Eigenschaften von Remotevideostreams

```swift

var type: ACSMediaStreamType = remoteParticipantVideoStream.type // 'ACSMediaStreamTypeVideo'

var isAvailable: Bool = remoteParticipantVideoStream.isAvailable // indicates if remote stream is available

var id: Int = remoteParticipantVideoStream.id // id of remoteParticipantStream

```

### <a name="render-remote-participant-stream"></a>Rendern von Streams von Remoteteilnehmern

So beginnen Sie mit dem Rendern von Streams von Remoteteilnehmern

```swift

let renderer: ACSRenderer? = ACSRenderer(remoteVideoStream: remoteParticipantVideoStream)
let targetRemoteParticipantView: ACSRendererView? = renderer?.createView(ACSRenderingOptions(ACSScalingMode.crop))
// To update the scaling mode later
targetRemoteParticipantView.update(ACSScalingMode.fit)

```

### <a name="remote-video-renderer-methods-and-properties"></a>Methoden und Eigenschaften für den Renderer für Remotevideo

```swift
// [Bool] isRendering - indicating if stream is being rendered
remoteVideoRenderer.isRendering()
```

## <a name="device-management"></a>Geräteverwaltung

`DeviceManager` ermöglicht Ihnen das Aufzählen lokaler Geräte, die in einem Anruf zur Übertragung von Audio-/Videostreams verwendet werden können. Mithilfe des Geräte-Managers können Sie auch beim Benutzer die Berechtigung für den Zugriff auf Mikrofon/Kamera anfordern. Sie können im `callClient`-Objekt auf `deviceManager` zugreifen:

```swift

self.callClient!.getDeviceManager(
    completionHandler: { (deviceManager, error) in
        if (error == nil) {
            print("Got device manager instance")
            self.deviceManager = deviceManager
        } else {
            print("Failed to get device manager instance")
        }
    })
```

### <a name="enumerate-local-devices"></a>Aufzählen lokaler Geräte

Für den Zugriff auf lokale Geräte können Sie Enumerationsmethoden für den Geräte-Manager verwenden. Enumeration ist ein synchroner Vorgang.

```swift
// enumerate local cameras
var localCameras = deviceManager.getCameraList() // [ACSVideoDeviceInfo, ACSVideoDeviceInfo...]
// enumerate local cameras
var localMicrophones = deviceManager.getMicrophoneList() // [ACSAudioDeviceInfo, ACSAudioDeviceInfo...]
// enumerate local cameras
var localSpeakers = deviceManager.getSpeakerList() // [ACSAudioDeviceInfo, ACSAudioDeviceInfo...]
``` 

### <a name="set-default-microphonespeaker"></a>Festlegen des Standardmikrofons/-lautsprechers

Mit dem Geräte-Manager können Sie ein Standardgerät festlegen, das beim Starten eines Anrufs verwendet wird. Wenn keine Stapelstandardwerte festgelegt sind, nutzt Communication Services die Standardeinstellungen des Betriebssystems.

```swift
// get first microphone
var firstMicrophone = self.deviceManager!.getMicrophoneList()![0]
// [Synchronous] set microphone
deviceManager.setMicrophone(ACSAudioDeviceInfo())
// get first speaker
var firstSpeaker = self.deviceManager!.getSpeakerList()![0]
// [Synchronous] set speaker
deviceManager.setSpeakers(ACSAudioDeviceInfo())
```

### <a name="local-camera-preview"></a>Vorschau auf lokaler Kamera

Sie können mit `ACSRenderer` mit dem Rendern eines Streams von Ihrer lokalen Kamera beginnen. Dieser Stream wird nicht an andere Teilnehmer gesendet. Es handelt sich um einen lokalen Vorschaufeed. Dies ist ein asynchroner Vorgang.

```swift

let camera: ACSVideoDeviceInfo = self.deviceManager!.getCameraList()![0]
let localVideoStream: ACSLocalVideoStream = ACSLocalVideoStream(camera)
let renderer: ACSRenderer = ACSRenderer(localVideoStream: localVideoStream)
self.view = renderer!.createView(ACSRenderingOptions())

```

### <a name="local-camera-preview-properties"></a>Eigenschaften der Vorschau auf lokale Kamera

Der Renderer verfügt über eine Reihe von Eigenschaften und Methoden, mit denen Sie das Rendering steuern können:

```swift

// Constructor can take in ACSLocalVideoStream or ACSRemoteVideoStream
let localRenderer = ACSRenderer(localVideoStream:localVideoStream)
let remoteRenderer = ACSRenderer(remoteVideoStream:remoteVideoStream)

// [ACSStreamSize] size of the rendering view
localRenderer.size

// [ACSRendererDelegate] an object you provide to receive events from this ACSRenderer instance
localRenderer.delegate

// [Synchronous] create view with rendering options
localRenderer.createView(options:ACSRenderingOptions())
// [Synchronous] dispose rendering view
localRenderer.dispose()

```

## <a name="eventing-model"></a>Ereignismodell

Sie können die meisten Objekte und Sammlungen abonnieren, um bei Änderungen von Werten benachrichtigt zu werden.

### <a name="properties"></a>Eigenschaften
So abonnieren Sie `property changed`-Ereignisse

```swift
call.delegate = self
// Get the property of the call state by doing get on the call's state member
public func onCallStateChanged(_ call: ACSCall!,
                               _ args: ACSPropertyChangedEventArgs!)
{
    print("Callback from SDK when the call state changes, current state: " + call.state.rawValue)
}

 // to unsubscribe
 self.call.delegate = nil

```

### <a name="collections"></a>Auflistungen
So abonnieren Sie `collection updated`-Ereignisse

```swift
call.delegate = self
// Collection contains the streams that were added or removed only
public func onLocalVideoStreamsChanged(_ call: ACSCall!,
                                       _ args: ACSLocalVideoStreamsUpdatedEventArgs!)
{
    print(args.addedStreams.count)
    print(args.removedStreams.count)
}
// to unsubscribe
self.call.delegate = nil
```
