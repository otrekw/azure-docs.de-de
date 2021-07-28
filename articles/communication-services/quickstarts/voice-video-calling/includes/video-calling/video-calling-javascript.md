---
author: mikben
ms.service: azure-communication-services
ms.topic: include
ms.date: 03/10/2021
ms.author: mikben
ms.openlocfilehash: 2bbff7c68b76b82bb834528124271eff69b8ad87
ms.sourcegitcommit: 67cdbe905eb67e969d7d0e211d87bc174b9b8dc0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111896520"
---
Steigen Sie in Azure Communication Services ein, indem Sie das Communication Services-SDK „Calling“ nutzen, um Ihrer App 1:1-Videoanrufe hinzuzufügen. Hier erfahren Sie, wie Sie einen Videoanruf mithilfe des Calling SDK von Azure Communication Services für JavaScript beginnen und beantworten.


> [!NOTE]
> Den fertigen Code für diesen Schnellstart finden Sie auf [GitHub](https://github.com/Azure-Samples/communication-services-javascript-quickstarts/tree/main/add-1-on-1-video-calling).


## <a name="prerequisites"></a>Voraussetzungen
- Rufen Sie ein Azure-Konto mit einem aktiven Abonnement ab. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Node.js](https://nodejs.org/en/): Active LTS- und Maintenance LTS-Versionen (8.11.1 und 10.14.1).
- Erstellen Sie eine aktive Communication Services-Ressource. [Erstellen Sie eine Communication Services-Ressource](../../../create-communication-resource.md?pivots=platform-azp&tabs=windows).
- Erstellen Sie ein Benutzerzugriffstoken, um den Anrufclient zu instanziieren. Lernen Sie mehr über das [Erstellen und Verwalten von Benutzerzugriffstoken](../../../access-tokens.md?pivots=programming-language-csharp).

## <a name="setting-up"></a>Einrichten
### <a name="create-a-new-nodejs-application"></a>Erstellen einer neuen Node.js-Anwendung
Öffnen Sie Ihr Terminal- oder Befehlsfenster, erstellen Sie ein neues Verzeichnis für Ihre App, und navigieren Sie zu diesem Verzeichnis.
```console
mkdir calling-quickstart && cd calling-quickstart
```
### <a name="install-the-package"></a>Installieren des Pakets
Verwenden Sie den Befehl `npm install`, um das Azure Communication Services Calling SDK für JavaScript zu installieren.

> [!IMPORTANT]
> In dieser Schnellstartanleitung wird Version `1.1.0-beta.1` des Azure Communication Services Calling SDK verwendet. 


```console
npm install @azure/communication-common --save
npm install @azure/communication-calling@1.1.0-beta.1 --save
```
### <a name="set-up-the-app-framework"></a>Einrichten des App-Frameworks

In dieser Schnellstartanleitung wird Webpack verwendet, um die Anwendungsressourcen zu bündeln. Führen Sie den folgenden Befehl aus, um die npm-Pakete `webpack`, `webpack-cli` und `webpack-dev-server` zu installieren und diese als Entwicklungsabhängigkeiten in `package.json` aufzulisten:

```console
npm install webpack@4.42.0 webpack-cli@3.3.11 webpack-dev-server@3.10.3 --save-dev
```
Erstellen Sie im Stammverzeichnis Ihres Projekts die Datei `index.html`. Diese Datei wird zum Konfigurieren eines grundlegenden Layouts verwendet, das es dem Benutzer ermöglicht, einen 1:1-Videoanruf zu tätigen.

Der Code lautet wie folgt:
```html
<!DOCTYPE html>
<html>
<head>
    <title>Communication Client - 1:1 Video Calling Sample</title>
</head>

<body>
    <h4>Azure Communication Services</h4>
    <h1>1:1 Video Calling Quickstart</h1>
    <input 
    id="callee-id-input"
    type="text"
    placeholder="Who would you like to call?"
    style="margin-bottom:1em; width: 200px;"
    />

    <div>
    <button id="call-button" type="button" disabled="true">
        start call
    </button>
        &nbsp;
    <button id="hang-up-button" type="button" disabled="true">
        hang up
    </button>
        &nbsp;
    <button id="start-Video" type="button" disabled="true">
        start video
    </button>
        &nbsp;
    <button id="stop-Video" type="button" disabled="true">
        stop video
    </button>     
    </div>

    <div>Local Video</div>
    <div style="height:200px; width:300px; background-color:black; position:relative;">
      <div id="myVideo" style="background-color: black; position:absolute; top:50%; transform: translateY(-50%);">
      </div>     
    </div>
    <div>Remote Video</div>
    <div style="height:200px; width:300px; background-color:black; position:relative;"> 
      <div id="remoteVideo" style="background-color: black; position:absolute; top:50%; transform: translateY(-50%);">
      </div>
    </div>

    <script src="./bundle.js"></script>
</body>
</html>
```

Erstellen Sie im Stammverzeichnis Ihres Projekts eine Datei mit dem Namen `client.js`, die die Anwendungslogik für diese Schnellstartanleitung enthalten soll. Fügen Sie den folgenden Code hinzu, um den anrufenden Client zu importieren und Verweise auf die DOM-Elemente abzurufen.

```JavaScript
import { CallClient, CallAgent, VideoStreamRenderer, LocalVideoStream } from "@azure/communication-calling";
import { AzureCommunicationTokenCredential } from '@azure/communication-common';

let call;
let callAgent;
const calleeInput = document.getElementById("callee-id-input");
const callButton = document.getElementById("call-button");
const hangUpButton = document.getElementById("hang-up-button");
const stopVideoButton = document.getElementById("stop-Video");
const startVideoButton = document.getElementById("start-Video");

let placeCallOptions;
let deviceManager;
let localVideoStream;
let rendererLocal;
let rendererRemote;
```
## <a name="object-model"></a>Objektmodell

Die folgenden Klassen und Schnittstellen befassen sich mit einigen der wichtigsten Features des Azure Communication Services Calling SDK:

| Name      | Beschreibung | 
| :---        |    :----   |
| CallClient  | „CallClient“ ist der Haupteinstiegspunkt des Calling SDK.      |
| CallAgent  | CallAgent dient zum Starten und Verwalten von Anrufen.        |
| DeviceManager | DeviceManager wird zum Verwalten von Mediengeräten verwendet.    |
| AzureCommunicationTokenCredential | Die AzureCommunicationTokenCredential-Klasse implementiert die CommunicationTokenCredential-Schnittstelle, die zum Instanziieren von CallAgent verwendet wird.        |

## <a name="authenticate-the-client-and-access-devicemanager"></a>Authentifizieren des Clients und Zugreifen auf DeviceManager

Sie müssen <[USER_ACCESS_TOKEN](https://github.com/Azure-Samples/communication-services-javascript-quickstarts/tree/main/access-tokens-quickstart)> durch ein gültiges Benutzerzugriffstoken für Ihre Ressource ersetzen. Wenn Sie noch über kein Token verfügen, finden Sie unter Benutzerzugriffstoken weitere Informationen. Initialisieren Sie mithilfe von `CallClient` eine `CallAgent`-Instanz mit einem `CommunicationUserCredential`-Objekt, das es uns ermöglicht, Anrufe zu tätigen und zu empfangen. Um auf `DeviceManager` zugreifen zu können, muss zunächst eine callAgent-Instanz erstellt werden. Anschließend können Sie die `getDeviceManager`-Methode für die `CallClient`-Instanz ausführen, um `DeviceManager` abzurufen.

Fügen Sie den folgenden Code zu `client.js` hinzu:

```JavaScript
async function init() {
    const callClient = new CallClient();
    const tokenCredential = new AzureCommunicationTokenCredential("<USER ACCESS TOKEN>");
    callAgent = await callClient.createCallAgent(tokenCredential, { displayName: 'optional ACS user name' });
    
    // Receive an incoming call
    // To handle incoming calls you need to listen to the `incomingCall` event of `callAgent`. Once there is an incoming call, you need to enumerate local cameras and construct 
    // a `LocalVideoStream` instance to send a video stream to the other participant. You also need to subscribe to `remoteParticipants` to handle remote video streams. You can 
    // accept or reject the call through the `incomingCall` instance. 
    callAgent.on('incomingCall', async e => {
        const videoDevices = await deviceManager.getCameras();
        const videoDeviceInfo = videoDevices[0];
        localVideoStream = new LocalVideoStream(videoDeviceInfo);
        localVideoView();

        stopVideoButton.disabled = false;
        callButton.disabled = true;
        hangUpButton.disabled = false;

        const addedCall = await e.incomingCall.accept({videoOptions: {localVideoStreams:[localVideoStream]}});
        call = addedCall;

        subscribeToRemoteParticipantInCall(addedCall);  
    });
    
    // Subscribe to call updates
    // You need to subscribe to the event when the remote participant ends the call to dispose of video renderers and toggle button states. 
    callAgent.on('callsUpdated', e => {
        e.removed.forEach(removedCall => {
            // dispose of video renders
            rendererLocal.dispose();
            rendererRemote.dispose();
            // toggle button states
            hangUpButton.disabled = true;
            callButton.disabled = false;
            stopVideoButton.disabled = true;
        })
    })

    deviceManager = await callClient.getDeviceManager();
    callButton.disabled = false;
}

init();
```
## <a name="place-a-11-outgoing-video-call-to-a-user"></a>Tätigen eines ausgehenden 1:1-Videoanrufs an einen Benutzer

Fügen Sie einen Ereignislistener hinzu, um einen Anruf zu initiieren, wenn auf `callButton` geklickt wird:

Zunächst müssen Sie lokale Kameras mithilfe der deviceManager-API `getCameraList` auflisten. In dieser Schnellstartanleitung verwenden wir die erste Kamera in der Sammlung. Sobald die gewünschte Kamera ausgewählt ist, wird eine LocalVideoStream-Instanz erstellt und innerhalb von `videoOptions` als Element innerhalb des localVideoStream-Arrays an die call-Methode übermittelt. Sobald eine Verbindung hergestellt wird, wird automatisch ein Videostream an den anderen Teilnehmer gesendet. 

```JavaScript
callButton.addEventListener("click", async () => {
    const videoDevices = await deviceManager.getCameras();
    const videoDeviceInfo = videoDevices[0];
    localVideoStream = new LocalVideoStream(videoDeviceInfo);
    placeCallOptions = {videoOptions: {localVideoStreams:[localVideoStream]}};

    localVideoView();
    stopVideoButton.disabled = false;
    startVideoButton.disabled = true;

    const userToCall = calleeInput.value;
    call = callAgent.startCall(
        [{ communicationUserId: userToCall }],
        placeCallOptions
    );

    subscribeToRemoteParticipantInCall(call);

    hangUpButton.disabled = false;
    callButton.disabled = true;
});
```  
Wenn Sie einen `LocalVideoStream` rendern möchten, müssen Sie eine neue Instanz von `VideoStreamRenderer` erstellen und anschließend eine neue `VideoStreamRendererView`-Instanz mithilfe der asynchronen `createView`-Methode erstellen. Sie können dann `view.target` an ein beliebiges Benutzeroberflächenelement anfügen. 

```JavaScript
async function localVideoView() {
    rendererLocal = new VideoStreamRenderer(localVideoStream);
    const view = await rendererLocal.createView();
    document.getElementById("myVideo").appendChild(view.target);
}
```
Alle Remoteteilnehmer sind über die `remoteParticipants`-Sammlung für eine Anrufinstanz verfügbar. Sie müssen auf das Ereignis `remoteParticipantsUpdated` lauschen, um benachrichtigt zu werden, wenn dem Anruf ein neuer Remoteteilnehmer hinzugefügt wird. Außerdem müssen Sie die Sammlung `remoteParticipants` durchlaufen, um die einzelnen Teilnehmer und damit ihre Videostreams abonnieren zu können. 

```JavaScript
function subscribeToRemoteParticipantInCall(callInstance) {
    callInstance.on('remoteParticipantsUpdated', e => {
        e.added.forEach( p => {
            subscribeToParticipantVideoStreams(p);
        })
    }); 
    callInstance.remoteParticipants.forEach( p => {
        subscribeToParticipantVideoStreams(p);
    })
}
```
Sie müssen das Ereignis `videoStreamsUpdated` abonnieren, um die hinzugefügten Videostreams von Remoteteilnehmern zu verarbeiten. Sie können die `videoStreams`-Sammlungen untersuchen, um die Streams der einzelnen Teilnehmer aufzulisten, während Sie die `remoteParticipants`-Sammlung des aktuellen Anrufs durchgehen.

```JavaScript
function subscribeToParticipantVideoStreams(remoteParticipant) {
    remoteParticipant.on('videoStreamsUpdated', e => {
        e.added.forEach(v => {
            handleVideoStream(v);
        })
    });
    remoteParticipant.videoStreams.forEach(v => {
        handleVideoStream(v);
    });
}
```
Zum Rendern eines `remoteVideoStream` müssen Sie ein `isAvailableChanged`-Ereignis abonnieren. Wenn sich die Eigenschaft `isAvailable` in `true` ändert, sendet ein Remoteteilnehmer einen Stream. Immer wenn sich die Verfügbarkeit eines Remotestreams ändert, können Sie wählen, ob Sie den gesamten `Renderer`, eine bestimmte `RendererView` zerstören oder sie behalten möchten, was jedoch zur Anzeige leerer Videobilder führt.
```JavaScript
function handleVideoStream(remoteVideoStream) {
    remoteVideoStream.on('isAvailableChanged', async () => {
        if (remoteVideoStream.isAvailable) {
            remoteVideoView(remoteVideoStream);
        } else {
            rendererRemote.dispose();
        }
    });
    if (remoteVideoStream.isAvailable) {
        remoteVideoView(remoteVideoStream);
    }
}
```
Wenn Sie einen `RemoteVideoStream` rendern möchten, müssen Sie eine neue Instanz von `VideoStreamRenderer` erstellen und anschließend eine neue `VideoStreamRendererView`-Instanz mithilfe der asynchronen `createView`-Methode erstellen. Sie können dann `view.target` an ein beliebiges Benutzeroberflächenelement anfügen. 

```JavaScript
async function remoteVideoView(remoteVideoStream) {
    rendererRemote = new VideoStreamRenderer(remoteVideoStream);
    const view = await rendererRemote.createView();
    document.getElementById("remoteVideo").appendChild(view.target);
}
```

## <a name="end-the-current-call"></a>Beenden des aktuellen Anrufs
Fügen Sie einen Ereignislistener hinzu, um den aktuellen Anruf zu beenden, wenn auf `hangUpButton` geklickt wird:
```JavaScript
hangUpButton.addEventListener("click", async () => {
    // dispose of the renderers
    rendererLocal.dispose();
    rendererRemote.dispose();
    // end the current call
    await call.hangUp();
    // toggle button states
    hangUpButton.disabled = true;
    callButton.disabled = false;
    stopVideoButton.disabled = true;
});
```

## <a name="start-and-end-video-during-the-call"></a>Starten und Beenden des Videos während des Anrufs
Sie können das Video während des aktuellen Anrufs beenden, indem Sie der Schaltfläche „Video beenden“ einen Ereignislistener hinzufügen, um den Renderer von `localVideoStream` zu verwerfen. 
 ```JavaScript       
stopVideoButton.addEventListener("click", async () => {
    await call.stopVideo(localVideoStream);
    rendererLocal.dispose();
    startVideoButton.disabled = false;
    stopVideoButton.disabled = true;
});
```
Sie können der Schaltfläche „Video starten“ einen Ereignislistener hinzufügen, um das Video erneut zu aktivieren, wenn es während des Anrufs beendet wurde. 
```JavaScript
startVideoButton.addEventListener("click", async () => {
    await call.startVideo(localVideoStream);
    localVideoView();
    stopVideoButton.disabled = false;
    startVideoButton.disabled = true;
});
```
## <a name="run-the-code"></a>Ausführen des Codes
Verwenden Sie `webpack-dev-server`, um Ihre App zu erstellen und auszuführen. Führen Sie den folgenden Befehl aus, um den Anwendungshost auf einem lokalen Webserver zu bündeln:

```console
npx webpack-dev-server --entry ./client.js --output bundle.js --debug --devtool inline-source-map
```
Navigieren Sie in Ihrem Browser zu http://localhost:8080/. Daraufhin sollte Folgendes angezeigt werden:

:::image type="content" source="../../media/javascript/1-on-1-video-calling.png" alt-text="1:1-Videoanruf":::

Sie können einen ausgehenden 1:1-Videoanruf tätigen, indem Sie eine Benutzer-ID im Textfeld eingeben und auf die Schaltfläche „Start Call“ (Anruf beginnen) klicken. 

## <a name="sample-code"></a>Beispielcode
Sie können die Beispiel-App von [GitHub](https://github.com/Azure-Samples/communication-services-javascript-quickstarts/tree/main/add-1-on-1-video-calling) herunterladen.
