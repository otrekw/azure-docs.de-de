---
author: mikben
ms.service: azure-communication-services
ms.topic: include
ms.date: 03/10/2021
ms.author: mikben
ms.openlocfilehash: 2ecbd207c4b1946a69b01f43ec2bc77d29b1a8c9
ms.sourcegitcommit: 73fb48074c4c91c3511d5bcdffd6e40854fb46e5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2021
ms.locfileid: "106072899"
---
## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Eine bereitgestellte Communication Services-Ressource. [Erstellen Sie eine Communication Services-Ressource](../../create-communication-resource.md).
- Ein Benutzerzugriffstoken zum Aktivieren des Anrufclients. Weitere Informationen finden Sie unter [Erstellen und Verwalten von Zugriffstoken](../../access-tokens.md).
- Optional: Durchlaufen Sie den Schnellstart zum [Hinzufügen von Sprachanrufen zu Ihrer Anwendung](../getting-started-with-calling.md).

## <a name="install-the-sdk"></a>Installieren des SDK

> [!NOTE]
> Dieses Dokument verwendet das ACS Calling Web SDK.

Verwenden Sie den Befehl `npm install`, um das Azure Communication Services Calling SDK sowie allgemeine SDKs für JavaScript zu installieren.

```console
npm install @azure/communication-common --save
npm install @azure/communication-calling --save

```

## <a name="object-model"></a>Objektmodell

Die folgenden Klassen und Schnittstellen befassen sich mit einigen der wichtigsten Features des Azure Communication Services Calling SDK:

| Name                             | BESCHREIBUNG                                                                                                                                 |
| ---------------------------------| ------------------------------------------------------------------------------------------------------------------------------------------- |
| `CallClient`                      | Der Haupteinstiegspunkt des Calling SDK.                                                                       |
| `CallAgent`                        | Dient zum Starten und Verwalten von Anrufen.                                                                                            |
| `DeviceManager`                    | Dient zum Verwalten von Mediengeräten.                                                                                           |
| `AzureCommunicationTokenCredential` | Implementiert die `CommunicationTokenCredential`-Schnittstelle zum Instanziieren von `callAgent`. |

## <a name="initialize-a-callclient-instance-create-a-callagent-instance-and-access-devicemanager"></a>Initialisieren einer CallClient-Instanz, Erstellen einer CallAgent-Instanz und Zugreifen auf deviceManager

Erstellen Sie eine neue `CallClient`-Instanz. Sie können sie mit benutzerdefinierten Optionen wie eine Protokollierungsinstanz konfigurieren.

Wenn Sie eine `CallClient`-Instanz verwenden, können Sie eine `CallAgent`-Instanz erstellen, indem Sie die Methode `createCallAgent` für die `CallClient`-Instanz aufrufen. Dadurch wird ein `CallAgent`-Instanzobjekt asynchron zurückgegeben.

Die Methode `createCallAgent` verwendet `CommunicationTokenCredential` als Argument, welches ein [Benutzerzugriffstoken](../../access-tokens.md) akzeptiert.

Nachdem Sie eine `callAgent`-Instanz erstellt haben, können Sie mit der Methode `getDeviceManager` für die `CallClient`-Instanz auf `deviceManager` zugreifen.

```js
// Set the logger's log level
setLogLevel('verbose');
// Redirect logger output to wherever desired. By default it logs to console
AzureLogger.log = (...args) => { console.log(...args) };
const userToken = '<user token>';
callClient = new CallClient(options);
const tokenCredential = new AzureCommunicationTokenCredential(userToken);
const callAgent = await callClient.createCallAgent(tokenCredential, {displayName: 'optional ACS user name'});
const deviceManager = await callClient.getDeviceManager()
```

## <a name="place-a-call"></a>Tätigen eines Anrufs

Um einen Anruf zu erstellen und zu starten, verwenden Sie eine der APIs für `callAgent`, und geben Sie einen Benutzer an, den Sie über das Communication Services Identity SDK erstellt haben.

Erstellung und Start des Anrufs erfolgen synchron. Die Anrufinstanz ermöglicht Ihnen das Abonnieren von Anrufereignissen.

### <a name="place-a-1n-call-to-a-user-or-pstn"></a>Tätigen eines 1:n-Anrufs an einen Benutzer oder ein Telefonfestnetz

Um einen anderen Communication Services-Benutzer anzurufen, verwenden Sie die Methode `startCall` für `callAgent`, und übergeben Sie den `CommunicationUserIdentifier`, den Sie [mit der Communication Services-Verwaltungsbibliothek](../../access-tokens.md) erstellt haben.

```js
const userCallee = { communicationUserId: '<ACS_USER_ID>' }
const oneToOneCall = callAgent.startCall([userCallee]);
```

Um einen Anruf an ein PSTN (Public Switched Telephone Network) zu tätigen, verwenden Sie die Methode `startCall` für `callAgent`, und übergeben Sie den `PhoneNumberIdentifier` des Empfängers. Die Communication Services-Ressource muss so konfiguriert werden, dass Anrufe über das Telefonfestnetz möglich sind.

Wenn Sie eine Telefonfestnetznummer anrufen, geben Sie Ihre alternative Anrufer-ID an. Eine alternative Anrufer-ID bezieht sich auf eine Telefonnummer (basierend auf dem E.164-Standard), die den Anrufer in einem PSTN-Anruf identifiziert. Dies ist die Telefonnummer, die dem Anrufempfänger bei einem eingehenden Anruf angezeigt wird.

> [!NOTE]
> Anrufe über das Telefonfestnetz sind derzeit in der Phase „Private Vorschau“. Um auf diese Funktion zuzugreifen, [bewerben Sie sich für das Early Adopter-Programm](https://aka.ms/ACS-EarlyAdopter).

Verwenden Sie für einen 1:1-Anruf den folgenden Code:

```js
const pstnCalee = { phoneNumber: '<ACS_USER_ID>' }
const alternateCallerId = {alternateCallerId: '<Alternate caller Id>'};
const oneToOneCall = callAgent.startCall([pstnCallee], {alternateCallerId});
```

Verwenden Sie für einen 1:n-Anruf den folgenden Code:

```js
const userCallee = { communicationUserId: <ACS_USER_ID> }
const pstnCallee = { phoneNumber: <PHONE_NUMBER>};
const alternateCallerId = {alternateCallerId: '<Alternate caller Id>'};
const groupCall = callAgent.startCall([userCallee, pstnCallee], {alternateCallerId});

```

### <a name="place-a-11-call-with-video-camera"></a>Tätigen eines 1:1-Anrufs mit Videokamera

> [!IMPORTANT]
> Derzeit ist nicht mehr als ein ausgehender lokaler Videostream möglich.

Zum Tätigen eines Videoanrufs müssen Sie Ihre Kameras über die Methode `getCameras()` in `deviceManager` angeben.

Nachdem Sie eine Kamera ausgewählt haben, verwenden Sie sie, um eine `LocalVideoStream`-Instanz zu erstellen. Übergeben Sie sie in `videoOptions` als Element innerhalb des `localVideoStream`-Arrays an die `startCall`-Methode.

```js
const deviceManager = await callClient.getDeviceManager();
const cameras = await deviceManager.getCameras();
const camera = cameras[0]
localVideoStream = new LocalVideoStream(camera);
const placeCallOptions = {videoOptions: {localVideoStreams:[localVideoStream]}};
const call = callAgent.startCall(['acsUserId'], placeCallOptions);

```

Sobald die Anrufverbindung hergestellt ist, wird automatisch ein Videostream von der ausgewählten Kamera an den anderen Teilnehmer gesendet. Dies gilt auch für die `Call.Accept()`- und die `CallAgent.join()`-Videooptionen.

### <a name="join-a-group-call"></a>Teilnehmen an einem Gruppenanruf

> [!NOTE]
> Der `groupId`-Parameter wird als Systemmetadaten angesehen und kann von Microsoft für Vorgänge verwendet werden, die zum Ausführen des Systems erforderlich sind. Fügen Sie keine personenbezogenen Daten in den `groupId`-Wert ein. Microsoft behandelt diesen Parameter nicht wie personenbezogene Daten. Seine Inhalte sind möglicherweise für Microsoft-Mitarbeiter sichtbar oder werden langfristig gespeichert.
>
> Der `groupId`-Parameter erfordert, dass Daten im GUID-Format vorliegen. Es wird empfohlen, zufällig generierte GUIDs zu verwenden, die in Ihren Systemen nicht als personenbezogene Daten betrachtet werden.
>

Um einen neuen Gruppenanruf zu starten oder an einem aktuellen Gruppenanruf teilzunehmen, müssen Sie die Methode `join` aufrufen und ein Objekt mit einer `groupId`-Eigenschaft übergeben. Der `groupId`-Wert muss eine GUID sein.

```js

const context = { groupId: <GUID>}
const call = callAgent.join(context);

```

### <a name="join-a-teams-meeting"></a>Teilnehmen an einer Teams-Besprechung
> [!NOTE]
> Diese API wird als Vorschau für Entwickler bereitgestellt. Je nachdem, welches Feedback wir dazu erhalten, werden möglicherweise Änderungen vorgenommen. Verwenden Sie diese API nicht in einer Produktionsumgebung. Um diese API einzusetzen, verwenden Sie das Betarelease des ACS Calling Web SDK.

Verwenden Sie für die Teilnahme an einer Teams-Besprechung die Methode `join`, und übergeben Sie einen Besprechungslink oder die Besprechungsdaten.

Teilnehmen über einen Besprechungslink:

```js
const locator = { meetingLink: <meeting link>}
const call = callAgent.join(locator);
```

Teilnehmen über Besprechungsdaten:

```js
const locator = {
    threadId: <thread id>,
    organizerId: <organizer id>,
    tenantId: <tenant id>,
    messageId: <message id>
}
const call = callAgent.join(locator);
```

## <a name="receive-an-incoming-call"></a>Empfangen eines eingehenden Anrufs

Die `callAgent`-Instanz gibt das Ereignis `incomingCall` aus, wenn die angemeldete Identität einen eingehenden Anruf empfängt. Um auf dieses Ereignis zu lauschen, abonnieren Sie es mithilfe einer der folgenden Optionen:

```js
const incomingCallHander = async (args: { incomingCall: IncomingCall }) => {

    //Get incoming call ID
    var incomingCallId = incomingCall.id

    // Get information about caller
    var callerInfo = incomingCall.callerInfo

    // Accept the call
    var call = await incomingCall.accept();

    // Reject the call
    incomingCall.reject();

    // Subscribe to callEnded event and get the call end reason
     incomingCall.on('callEnded', args => {
        console.log(args.callEndReason);
    });

    // callEndReason is also a property of IncomingCall
    var callEndReason = incomingCall.callEndReason;
};
callAgentInstance.on('incomingCall', incomingCallHander);
```

Das `incomingCall`-Ereignis umfasst eine `incomingCall`-Instanz, die Sie akzeptieren oder ablehnen können.

## <a name="manage-calls"></a>Verwalten von Anrufen

Während eines Anrufs können Sie auf Anrufeigenschaften zugreifen und Video- und Audioeinstellungen verwalten.

### <a name="check-call-properties"></a>Überprüfen von Anrufeigenschaften

Abrufen der eindeutigen ID (Zeichenfolge) für einen Anruf:

   ```js
    const callId: string = call.id;
   ```

Erhalten Sie Informationen zu anderen Anrufteilnehmern, indem Sie sich die `remoteParticipants`-Sammlung für die call-Instanz ansehen:

   ```js
   const remoteParticipants = call.remoteParticipants;
   ```

Identifizieren des Anrufers eines eingehenden Anrufs:

   ```js
   const callerIdentity = call.callerInfo.identifier;
   ```

   `identifier` ist einer der `CommunicationIdentifier`-Typen.

Abrufen des Anrufzustands:

   ```js
   const callState = call.state;
   ```

   Gibt eine Zeichenfolge zurück, die den aktuellen Zustand eines Anrufs darstellt:

  - `None`: anfänglicher Anrufzustand.
  - `Connecting`: anfänglicher Übergangszustand, wenn ein Anruf getätigt oder angenommen wird.
  - `Ringing`: Weist bei einem ausgehenden Anruf darauf hin, dass für den Anruf Remoteteilnehmer ein Klingeln ertönt. Auf deren Seite lautet der Status `Incoming`.
  - `EarlyMedia`: Gibt einen Zustand an, in dem vor dem Verbinden des Anrufs eine Ansage wiedergegeben wird.
  - `Connected`: Gibt an, dass der Anruf verbunden wurde.
  - `LocalHold`: Gibt an, dass der Anruf von einem lokalen Teilnehmer gehalten wird. Zwischen dem lokalen Endpunkt und den Remoteteilnehmern werden keine Medien übertragen.
  - `RemoteHold`: Gibt an, dass der Anruf von einem Remoteteilnehmer gehalten wird. Zwischen dem lokalen Endpunkt und den Remoteteilnehmern werden keine Medien übertragen.
  - `Disconnecting`: Übergangszustand, bevor der Anruf in einen `Disconnected`-Zustand wechselt.
  - `Disconnected`: Abschließender Anrufzustand. Wenn die Netzwerkverbindung unterbrochen wird, ändert sich der Zustand nach zwei Minuten in `Disconnected`.

Ermitteln Sie, warum der Anruf beendet wurde, indem Sie die Eigenschaft `callEndReason` überprüfen:

   ```js
   const callEndReason = call.callEndReason;
   const callEndReasonCode = callEndReason.code // (number) code associated with the reason
   const callEndReasonSubCode = callEndReason.subCode // (number) subCode associated with the reason
   ```

Erfahren Sie, ob der aktuelle Anruf ein- oder ausgeht, indem Sie die Eigenschaft `direction` überprüfen. Er gibt `CallDirection` zurück.

  ```js
   const isIncoming = call.direction == 'Incoming';
   const isOutgoing = call.direction == 'Outgoing';
   ```

Stellen Sie fest, ob das aktuelle Mikrofon stummgeschaltet ist. Er gibt `Boolean` zurück.

   ```js
   const muted = call.isMuted;
   ```

Ermitteln Sie, ob der Bildschirmübertragungsstream von einem bestimmten Endpunkt aus gesendet wird, indem Sie die Eigenschaft `isScreenSharingOn` überprüfen. Er gibt `Boolean` zurück.

   ```js
   const isScreenSharingOn = call.isScreenSharingOn;
   ```

Untersuchen Sie aktive Videostreams, indem Sie sich die `localVideoStreams`-Sammlung ansehen. Sie gibt `LocalVideoStream`-Objekte zurück.

   ```js
   const localVideoStreams = call.localVideoStreams;
   ```

### <a name="check-a-callended-event"></a>Überprüfen eines callEnded-Ereignisses

Die `call`-Instanz gibt das Ereignis `callEnded` aus, wenn der Anruf endet. Um auf dieses Ereignis zu lauschen, abonnieren Sie es mithilfe des folgenden Codes:

```js
const callEndHander = async (args: { callEndReason: CallEndReason }) => {
    console.log(args.callEndReason)
};

call.on('callEnded', callEndHander);
```

### <a name="mute-and-unmute"></a>Stummschalten und Aufheben der Stummschaltung

Zum Stummschalten oder Aufheben der Stummschaltung des lokalen Endpunkts können Sie die asynchronen APIs `mute` und `unmute` verwenden:

```js

//mute local device
await call.mute();

//unmute local device
await call.unmute();

```

### <a name="start-and-stop-sending-local-video"></a>Starten und Beenden des Sendens von lokalem Video

Um ein Video zu starten, müssen Sie mit der Methode `getCameras` Kameras für das `deviceManager`-Objekt angeben. Erstellen Sie dann eine neue Instanz von `LocalVideoStream`, indem Sie die gewünschte Kamera als Argument an die Methode `startVideo` übergeben:

```js
const deviceManager = await callClient.getDeviceManager();
const cameras = await deviceManager.getCameras();
const camera = cameras[0]
const localVideoStream = new LocalVideoStream(camera);
await call.startVideo(localVideoStream);
```

Sobald Videodaten erfolgreich gesendet werden, wird eine `LocalVideoStream`-Instanz der `localVideoStreams`-Sammlung für eine Anrufinstanz hinzugefügt.

```js
call.localVideoStreams[0] === localVideoStream;
```

Um das lokale Video anzuhalten, übergeben Sie die `localVideoStream`-Instanz, die in der `localVideoStreams`-Sammlung verfügbar ist:

```js
await call.stopVideo(localVideoStream);
```

Sie können während des Sendens von Videodaten auf ein anderes Kameragerät umschalten, indem Sie `switchSource` für eine `localVideoStream`-Instanz aufrufen:

```js
const cameras = await callClient.getDeviceManager().getCameras();
const camera = cameras[1];
localVideoStream.switchSource(camera);
```

## <a name="manage-remote-participants"></a>Verwalten von Remoteteilnehmern

Alle Remoteteilnehmer werden durch den Typ `RemoteParticipant` dargestellt und sind über die `remoteParticipants`-Sammlung für eine Anrufinstanz verfügbar.

### <a name="list-the-participants-in-a-call"></a>Auflisten der Teilnehmer an einem Anruf

Die `remoteParticipants`-Sammlung gibt eine Liste der Remoteteilnehmer eines Anrufs zurück:

```js
call.remoteParticipants; // [remoteParticipant, remoteParticipant....]
```

### <a name="access-remote-participant-properties"></a>Zugreifen auf Eigenschaften von Remoteteilnehmern

Remoteteilnehmern ist ein Satz von Eigenschaften und Sammlungen zugeordnet:

- `CommunicationIdentifier`: Ruft den Bezeichner eines Remoteteilnehmers ab. „Identity“ ist einer der `CommunicationIdentifier`-Typen:

  ```js
  const identifier = remoteParticipant.identifier;
  ```

  Mögliche `CommunicationIdentifier`-Typen:

  - `{ communicationUserId: '<ACS_USER_ID'> }`: Objekt, das den ACS-Benutzer darstellt.
  - `{ phoneNumber: '<E.164>' }`: Objekt, das die Telefonnummer im E.164-Format darstellt.
  - `{ microsoftTeamsUserId: '<TEAMS_USER_ID>', isAnonymous?: boolean; cloud?: "public" | "dod" | "gcch" }`: Objekt, das den Teams-Benutzer darstellt.
  - `{ id: string }`: Objekt, das den Bezeichner darstellt, der keinem der anderen Bezeichnertypen entspricht.

- `state`: Ruft den Zustand eines Remoteteilnehmers ab.

  ```js
  const state = remoteParticipant.state;
  ```

  Mögliche Zustände:

  - `Idle`: Anfangszustand.
  - `Connecting`: Übergangszustand, während sich der Teilnehmer mit dem Anruf verbindet.
  - `Ringing`: Für den Teilnehmeranruf ertönt ein Klingeln.
  - `Connected`: Der Teilnehmer ist mit dem Anruf verbunden.
  - `Hold`: Der Teilnehmer wird gehalten.
  - `EarlyMedia`: Ansage, die vor dem Verbinden eines Teilnehmers mit dem Anruf wiedergegeben wird.
  - `Disconnected`: Abschließender Zustand. Der Teilnehmer wird vom Anruf getrennt. Wenn der Remoteteilnehmer seine Netzwerkkonnektivität verliert, ändert sich der Zustand nach zwei Minuten in `Disconnected`.

- `callEndReason`: Um zu erfahren, warum ein Teilnehmer den Anruf verlassen hat, prüfen Sie die Eigenschaft `callEndReason`:

  ```js
  const callEndReason = remoteParticipant.callEndReason;
  const callEndReasonCode = callEndReason.code // (number) code associated with the reason
  const callEndReasonSubCode = callEndReason.subCode // (number) subCode associated with the reason
  ```

- `isMuted`-Status: Um herauszufinden, ob ein Remoteteilnehmer stummgeschaltet wurde, überprüfen Sie die Eigenschaft `isMuted`. Er gibt `Boolean` zurück.

  ```js
  const isMuted = remoteParticipant.isMuted;
  ```

- `isSpeaking`-Status: Um herauszufinden, ob ein Remoteteilnehmer gerade spricht, überprüfen Sie die Eigenschaft `isSpeaking`. Er gibt `Boolean` zurück.

  ```js
  const isSpeaking = remoteParticipant.isSpeaking;
  ```

- `videoStreams`: Um alle Videostreams zu prüfen, die ein bestimmter Teilnehmer im Rahmen dieses Anrufs sendet, sehen Sie sich die `videoStreams`-Sammlung an. Sie enthält `RemoteVideoStream`-Objekte.

  ```js
  const videoStreams = remoteParticipant.videoStreams; // [RemoteVideoStream, ...]
  ```
- `displayName`: Um den Anzeigenamen für diesen Remoteteilnehmer zu erhalten, überprüfen Sie die zurückgegebene Zeichenfolge der Eigenschaft `displayName`. 

  ```js
  const displayName = remoteParticipant.displayName;
  ```

### <a name="add-a-participant-to-a-call"></a>Hinzufügen eines Teilnehmers zu einem Anruf

Wenn Sie einem Anruf einen Teilnehmer (einen Benutzer oder eine Telefonnummer) hinzufügen möchten, können Sie `addParticipant` verwenden. Geben Sie einen der `Identifier`-Typen an. Dieser gibt die `remoteParticipant`-Instanz zurück.

```js
const userIdentifier = { communicationUserId: <ACS_USER_ID> };
const pstnIdentifier = { phoneNumber: <PHONE_NUMBER>}
const remoteParticipant = call.addParticipant(userIdentifier);
const remoteParticipant = call.addParticipant(pstnIdentifier, {alternateCallerId: '<Alternate Caller ID>'});
```

### <a name="remove-a-participant-from-a-call"></a>Entfernen eines Teilnehmers aus einem Anruf

Um einen Teilnehmer (einen Benutzer oder eine Telefonnummer) aus einem Anruf zu entfernen, können Sie `removeParticipant` aufrufen. Sie müssen einen der `Identifier`-Typen übergeben. Dieser Vorgang wird asynchron aufgelöst, nachdem der Teilnehmer aus dem Anruf entfernt wurde. Der Teilnehmer wird auch aus der `remoteParticipants`-Sammlung entfernt.

```js
const userIdentifier = { communicationUserId: <ACS_USER_ID> };
const pstnIdentifier = { phoneNumber: <PHONE_NUMBER>}
await call.removeParticipant(userIdentifier);
await call.removeParticipant(pstnIdentifier);
```

## <a name="render-remote-participant-video-streams"></a>Rendern von Videostreams von Remoteteilnehmern

Um die Video- und Bildschirmübertragungs-Streams der Remoteteilnehmer aufzulisten, sehen Sie sich die `videoStreams`-Sammlungen an:

```js
const remoteVideoStream: RemoteVideoStream = call.remoteParticipants[0].videoStreams[0];
const streamType: MediaStreamType = remoteVideoStream.mediaStreamType;
```

Zum Rendern von `RemoteVideoStream` müssen Sie ein `isAvailableChanged`-Ereignis abonnieren. Wenn sich die Eigenschaft `isAvailable` in `true` ändert, sendet ein Remoteteilnehmer einen Stream. Erstellen Sie daraufhin eine neue Instanz von `VideoStreamRenderer` und anschließend mit der asynchronen Methode `createView` eine neue `VideoStreamRendererView`-Instanz.  Sie können dann `view.target` an ein beliebiges Benutzeroberflächenelement anfügen.

Immer wenn sich die Verfügbarkeit eines Remotestreams ändert, können Sie wählen, ob Sie den gesamten `VideoStreamRenderer`, eine bestimmte `VideoStreamRendererView` zerstören oder sie behalten möchten, was jedoch zur Anzeige leerer Videobilder führt.

```js
function subscribeToRemoteVideoStream(remoteVideoStream: RemoteVideoStream) {
    let videoStreamRenderer: VideoStreamRenderer = new VideoStreamRenderer(remoteVideoStream);
    const displayVideo = () => {
        const view = await videoStreamRenderer.createView();
        htmlElement.appendChild(view.target);
    }
    remoteVideoStream.on('isAvailableChanged', async () => {
        if (remoteVideoStream.isAvailable) {
            displayVideo();
        } else {
            videoStreamRenderer.dispose();
        }
    });
    if (remoteVideoStream.isAvailable) {
        displayVideo();
    }
}
```

### <a name="remote-video-stream-properties"></a>Eigenschaften von Remotevideostreams

Für Remotevideostreams gibt es die folgenden Eigenschaften:

- `id`: ID eines Remotevideostreams.

  ```js
  const id: number = remoteVideoStream.id;
  ```

- `mediaStreamType`: Dies kann `Video` oder `ScreenSharing` sein.

  ```js
  const type: MediaStreamType = remoteVideoStream.mediaStreamType;
  ```

- `isAvailable`: Gibt an, ob der Endpunkt des Remoteteilnehmers aktiv einen Stream sendet.

  ```js
  const type: boolean = remoteVideoStream.isAvailable;
  ```

### <a name="videostreamrenderer-methods-and-properties"></a>Methoden und Eigenschaften von VideoStreamRenderer

Erstellen Sie eine `VideoStreamRendererView`-Instanz, die in der Benutzeroberfläche der Anwendung angefügt werden kann, um den Remotevideostream zu rendern. Verwenden Sie die asynchrone Methode `createView()`. Sie wird aufgelöst, wenn der Stream zum Rendern bereit ist, und gibt ein Objekt mit der Eigenschaft `target` zurück. Dieses stellt ein `video`-Element dar, das an beliebiger Stelle in der DOM-Struktur angefügt werden kann.

  ```js
  videoStreamRenderer.createView()
  ```

Löscht `videoStreamRenderer` und alle zugeordneten `VideoStreamRendererView`-Instanzen:

  ```js
  videoStreamRenderer.dispose()
  ```

### <a name="videostreamrendererview-methods-and-properties"></a>Methoden und Eigenschaften von VideoStreamRendererView

Beim Erstellen von `VideoStreamRendererView` können Sie die Eigenschaften `scalingMode` und `isMirrored` angeben. `scalingMode` kann `Stretch`, `Crop` oder `Fit` sein. Bei Angabe von `isMirrored` wird der gerenderte Stream vertikal gespiegelt.

```js
const videoStreamRendererView: VideoStreamRendererView = await videoStreamRenderer.createView({ scalingMode, isMirrored });
```

Jede `VideoStreamRendererView`-Instanz verfügt über eine `target`-Eigenschaft, die die Renderingoberfläche darstellt. Fügen Sie diese Eigenschaft in der Benutzeroberfläche der Anwendung an:

```js
htmlElement.appendChild(view.target);
```

Sie können `scalingMode` aktualisieren, indem Sie die Methode `updateScalingMode` aufrufen:

```js
view.updateScalingMode('Crop')
```

## <a name="device-management"></a>Geräteverwaltung

In `deviceManager` können Sie lokale Geräte angeben, die Ihre Audio- und Videostreams in einem Anruf übertragen können. deviceManager unterstützt Sie auch beim Anfordern von Berechtigungen für den Zugriff auf das Mikrofon und die Kamera eines anderen Benutzers über die native Browser-API.

Sie können auf `deviceManager` zugreifen, indem Sie die Methode `callClient.getDeviceManager()` aufrufen:

```js
const deviceManager = await callClient.getDeviceManager();
```

### <a name="get-local-devices"></a>Abrufen lokaler Geräte

Für den Zugriff auf lokale Geräte können Sie Enumerationsmethoden für `deviceManager` verwenden.

```js
//  Get a list of available video devices for use.
const localCameras = await deviceManager.getCameras(); // [VideoDeviceInfo, VideoDeviceInfo...]

// Get a list of available microphone devices for use.
const localMicrophones = await deviceManager.getMicrophones(); // [AudioDeviceInfo, AudioDeviceInfo...]

// Get a list of available speaker devices for use.
const localSpeakers = await deviceManager.getSpeakers(); // [AudioDeviceInfo, AudioDeviceInfo...]
```

### <a name="set-the-default-microphone-and-speaker"></a>Festlegen des Standardmikrofons und -lautsprechers

In `deviceManager` können Sie ein Standardgerät festlegen, das zum Starten eines Anrufs verwendet wird. Wenn keine Clientstandardwerte festgelegt sind, verwendet Communication Services die Standardwerte des Betriebssystems.

```js
// Get the microphone device that is being used.
const defaultMicrophone = deviceManager.selectedMicrophone;

// Set the microphone device to use.
await deviceManager.selectMicrophone(localMicrophones[0]);

// Get the speaker device that is being used.
const defaultSpeaker = deviceManager.selectedSpeaker;

// Set the speaker device to use.
await deviceManager.selectSpeaker(localSpeakers[0]);
```

### <a name="local-camera-preview"></a>Vorschau auf lokaler Kamera

Sie können `deviceManager` und `VideoStreamRenderer` verwenden, um mit dem Rendern von Streams über Ihre lokale Kamera zu beginnen. Dieser Stream wird nicht an andere Teilnehmer gesendet. Es handelt sich um einen lokalen Vorschaufeed.

```js
const cameras = await deviceManager.getCameras();
const camera = cameras[0];
const localCameraStream = new LocalVideoStream(camera);
const videoStreamRenderer = new VideoStreamRenderer(localCameraStream);
const view = await videoStreamRenderer.createView();
htmlElement.appendChild(view.target);

```

### <a name="request-permission-to-camera-and-microphone"></a>Anfordern der Berechtigung für Kamera und Mikrofon

Fordern Sie einen Benutzer auf, Berechtigungen für Kamera und Mikrofon zu erteilen:

```js
const result = await deviceManager.askDevicePermission({audio: true, video: true});
```

Dies wird mit einem Objekt aufgelöst, das angibt, ob `audio`- und `video`-Berechtigungen erteilt wurden:

```js
console.log(result.audio);
console.log(result.video);
```

## <a name="record-calls"></a>Aufzeichnen von Anrufen
> [!NOTE]
> Diese API wird als Vorschau für Entwickler bereitgestellt. Je nachdem, welches Feedback wir dazu erhalten, werden möglicherweise Änderungen vorgenommen. Verwenden Sie diese API nicht in einer Produktionsumgebung. Um diese API einzusetzen, verwenden Sie das Betarelease des ACS Calling Web SDK.

Die Anrufaufzeichnung ist ein erweitertes Feature der zentralen `Call`-API. Sie müssen zunächst das API-Objekt der Aufzeichnungsfunktion abrufen:

```js
const callRecordingApi = call.api(Features.Recording);
```

Um anschließend herauszufinden, ob der Anruf aufgezeichnet wird, prüfen Sie die Eigenschaft `isRecordingActive` von `callRecordingApi`. Er gibt `Boolean` zurück.

```js
const isResordingActive = callRecordingApi.isRecordingActive;
```

Sie können auch Aufzeichnungsänderungen abonnieren:

```js
const isRecordingActiveChangedHandler = () => {
  console.log(callRecordingApi.isRecordingActive);
};

callRecordingApi.on('isRecordingActiveChanged', isRecordingActiveChangedHandler);

```

## <a name="transfer-calls"></a>Durchstellen von Anrufen
> [!NOTE]
> Diese API wird als Vorschau für Entwickler bereitgestellt. Je nachdem, welches Feedback wir dazu erhalten, werden möglicherweise Änderungen vorgenommen. Verwenden Sie diese API nicht in einer Produktionsumgebung. Um diese API einzusetzen, verwenden Sie das Betarelease des ACS Calling Web SDK.

Die Anrufdurchstellung ist ein erweitertes Feature der zentralen `Call`-API. Sie müssen zunächst das API-Objekt der Durchstellungsfunktion abrufen:

```js
const callTransferApi = call.api(Features.Transfer);
```

Am Durchstellen von Anrufen sind drei Parteien beteiligt:

- *Durchsteller*: die Person, die die Durchstellungsanforderung auslöst.
- *Durchgestellter*: die Person, deren Anruf durchgestellt wird.
- *Durchstellungsziel*: die Person, an die der Anruf durchgestellt wird.

Beim Durchstellen werden folgende Schritt durchgeführt:

1. Es besteht bereits eine Anrufverbindung zwischen dem *Durchsteller* und dem *Durchgestellten*. Der *Durchsteller* beschließt, den Anruf vom *Durchgestellten* an das *Durchstellungsziel* zu übertragen.
1. Der *Durchsteller* ruft die `transfer`-API auf.
1. Der *Durchgestellte* entscheidet bei einem `transferRequested`-Ereignis mithilfe von `accept` oder `reject`, ob er die Durchstellungsanforderung an das *Durchstellungsziel* annimmt oder ablehnt.
1. Das *Durchstellungsziel* empfängt nur dann einen eingehenden Anruf, wenn der *Durchgestellte* die Durchstellungsanforderung akzeptiert.

Zum Durchstellen eines aktuellen Anrufs können Sie die `transfer`-API verwenden. `transfer` verwendet die optionalen `transferCallOptions`, mit denen Sie ein Flag `disableForwardingAndUnanswered` festlegen können:

- `disableForwardingAndUnanswered = false`: Wenn das *Durchstellungsziel* den durchgestellten Anruf nicht annimmt, werden die Einstellungen für Weiterleitung und Nichtannahme des *Durchstellungsziels* befolgt.
- `disableForwardingAndUnanswered = true`: Wenn das *Durchstellungsziel* den durchgestellten Anruf nicht annimmt, wird der Durchstellungsversuch beendet.

```js
// transfer target can be an ACS user
const id = { communicationUserId: <ACS_USER_ID> };
```

```js
// call transfer API
const transfer = callTransferApi.transfer({targetParticipant: id});
```

Die `transferRequested`-API erlaubt Ihnen das Abonnieren der Ereignisse `transfer` und `transferStateChanged`. Ein `transferRequested`-Ereignis stammt aus einer `call`-Instanz. Ein `transferStateChanged`-Ereignis sowie `state` und `error` des Durchstellungsvorgangs stammen aus einer `transfer`-Instanz.

```js
// transfer state
const transferState = transfer.state; // None | Transferring | Transferred | Failed

// to check the transfer failure reason
const transferError = transfer.error; // transfer error code that describes the failure if a transfer request failed
```

Der *Durchgestellte* kann die vom *Durchsteller* ausgelöste Durchstellungsanforderung im Ereignis `transferRequested` über `accept()` oder `reject()` in `transferRequestedEventArgs` annehmen oder ablehnen. Sie können auf `targetParticipant`-Informationen und auf die Methoden `accept` und `reject` in `transferRequestedEventArgs` zugreifen.

```js
// Transferee to accept the transfer request
callTransferApi.on('transferRequested', args => {
  args.accept();
});

// Transferee to reject the transfer request
callTransferApi.on('transferRequested', args => {
  args.reject();
});
```

## <a name="learn-about-eventing-models"></a>Weitere Informationen zu Ereignismodellen

Untersuchen Sie aktuelle Werte, und abonnieren Sie Aktualisierungsereignisse, um zukünftige Werte zu erhalten.

### <a name="properties"></a>Eigenschaften

```js
// Inspect the current value
console.log(object.property);

// Subscribe to value updates
object.on('propertyChanged', () => {
    // Inspect new value
    console.log(object.property)
});

// Unsubscribe from updates:
object.off('propertyChanged', () => {});



// Example for inspecting a call state
console.log(call.state);
call.on('stateChanged', () => {
    console.log(call.state);
});
call.off('stateChanged', () => {});
```

### <a name="collections"></a>Auflistungen

```js
// Inspect the current collection
object.collection.forEach(v => {
    console.log(v);
});

// Subscribe to collection updates
object.on('collectionUpdated', e => {
    // Inspect new values added to the collection
    e.added.forEach(v => {
        console.log(v);
    });
    // Inspect values removed from the collection
    e.removed.forEach(v => {
        console.log(v);
    });
});

// Unsubscribe from updates:
object.off('collectionUpdated', () => {});

// Example for subscribing to remote participants and their video streams
call.remoteParticipants.forEach(p => {
    subscribeToRemoteParticipant(p);
})

call.on('remoteParticipantsUpdated', e => {
    e.added.forEach(p => { subscribeToRemoteParticipant(p) })
    e.removed.forEach(p => { unsubscribeFromRemoteParticipant(p) })
});

function subscribeToRemoteParticipant(p) {
    console.log(p.state);
    p.on('stateChanged', () => { console.log(p.state); });
    p.videoStreams.forEach(v => { subscribeToRemoteVideoStream(v) });
    p.on('videoStreamsUpdated', e => { e.added.forEach(v => { subscribeToRemoteVideoStream(v) }) })
}
```
