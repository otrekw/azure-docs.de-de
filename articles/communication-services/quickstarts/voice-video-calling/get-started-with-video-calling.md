---
title: 'Schnellstart: Hinzufügen von Videoanrufen zu Ihrer App (JavaScript)'
titleSuffix: An Azure Communication Services quickstart
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie Ihrer App mithilfe von Azure Communication Services Videoanruffunktionen hinzufügen.
author: xumo-95
ms.author: mikben
ms.date: 07/24/2020
ms.topic: quickstart
ms.service: azure-communication-services
ms.openlocfilehash: 00721a5c7763663a17b1e67cf0ac3ae59fd9448a
ms.sourcegitcommit: dda0d51d3d0e34d07faf231033d744ca4f2bbf4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102204523"
---
# <a name="quickstart-add-11-video-calling-to-your-app-javascript"></a>Schnellstart: Hinzufügen der 1:1-Videoanruffunktion zu Ihrer App (JavaScript)

## <a name="prerequisites"></a>Voraussetzungen
- Rufen Sie ein Azure-Konto mit einem aktiven Abonnement ab. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Node.js](https://nodejs.org/en/): Active LTS- und Maintenance LTS-Versionen (8.11.1 und 10.14.1).
- Erstellen Sie eine aktive Communication Services-Ressource. [Erstellen Sie eine Communication Services-Ressource](https://docs.microsoft.com/azure/communication-services/quickstarts/create-communication-resource?tabs=windows&pivots=platform-azp).
- Erstellen Sie ein Benutzerzugriffstoken, um den Anrufclient zu instanziieren. Lernen Sie mehr über das [Erstellen und Verwalten von Benutzerzugriffstoken](https://docs.microsoft.com/azure/communication-services/quickstarts/access-tokens?pivots=programming-language-csharp).

## <a name="setting-up"></a>Einrichten
### <a name="create-a-new-nodejs-application"></a>Erstellen einer neuen Node.js-Anwendung
Öffnen Sie Ihr Terminal- oder Befehlsfenster, erstellen Sie ein neues Verzeichnis für Ihre App, und navigieren Sie zu diesem Verzeichnis.
```console
mkdir calling-quickstart && cd calling-quickstart
```
### <a name="install-the-package"></a>Installieren des Pakets
Verwenden Sie den Befehl `npm install`, um die Clientbibliothek für Telefonie von Azure Communication Services für JavaScript zu installieren.

In diesem Schnellstart wird die Clientbibliothek `1.0.0.beta-6` für die Azure Communication-Telefonie verwendet. 

```console
npm install @azure/communication-common --save
npm install @azure/communication-calling --save
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
import { CallClient, CallAgent, Renderer, LocalVideoStream } from "@azure/communication-calling";
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

Die folgenden Klassen und Schnittstellen befassen sich mit einigen der wichtigsten Features der Azure Communication Services-Clientbibliothek „Calling“:

| Name      | Beschreibung | 
| :---        |    :----   |
| CallClient  | CallClient ist der Haupteinstiegspunkt in die Clientbibliothek „Calling“.      |
| CallAgent  | CallAgent dient zum Starten und Verwalten von Anrufen.        |
| DeviceManager | DeviceManager wird zum Verwalten von Mediengeräten verwendet.    |
| AzureCommunicationTokenCredential | Die AzureCommunicationTokenCredential-Klasse implementiert die CommunicationTokenCredential-Schnittstelle, die zum Instanziieren von CallAgent verwendet wird.        |

## <a name="authenticate-the-client-and-access-devicemanager"></a>Authentifizieren des Clients und Zugreifen auf DeviceManager

Sie müssen <USER_ACCESS_TOKEN> durch ein gültiges Benutzerzugriffstoken für Ihre Ressource ersetzen. Wenn Sie noch über kein Token verfügen, finden Sie unter Benutzerzugriffstoken weitere Informationen. Initialisieren Sie mithilfe von CallClient eine CallAgent-Instanz mit einem CommunicationUserCredential-Objekt, das es uns ermöglicht, Anrufe zu tätigen und zu empfangen. Um auf DeviceManager zugreifen zu können, muss zunächst eine callAgent-Instanz erstellt werden. Anschließend können Sie die `getDeviceManager`-Methode für die `CallClient`-Instanz ausführen, um `DeviceManager` abzurufen.

Fügen Sie den folgenden Code zu `client.js` hinzu:

```JavaScript
async function init() {
    const callClient = new CallClient();
    const tokenCredential = new AzureCommunicationTokenCredential("<USER ACCESS TOKEN>");
    callAgent = await callClient.createCallAgent(tokenCredential, { displayName: 'optional ACS user name' });
    
    deviceManager = await callClient.getDeviceManager();
    callButton.disabled = false;
}
init();
```
## <a name="place-a-11-outgoing-video-call-to-a-user"></a>Tätigen eines ausgehenden 1:1-Videoanrufs an einen Benutzer

Fügen Sie einen Ereignislistener hinzu, um einen Anruf zu initiieren, wenn auf `callButton` geklickt wird:

Zunächst müssen Sie lokale Kameras mithilfe der deviceManager getCameraList-API auflisten. In dieser Schnellstartanleitung verwenden wir die erste Kamera in der Sammlung. Sobald die gewünschte Kamera ausgewählt ist, wird eine LocalVideoStream-Instanz erstellt und innerhalb von videoOptions als Element innerhalb des localVideoStream-Arrays an die call-Methode übermittelt. Sobald eine Verbindung hergestellt wird, wird automatisch ein Videostream an den anderen Teilnehmer gesendet. 

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
Sie müssen eine neue `Renderer`-Instanz erstellen und anschließend eine neue RendererView-Instanz mithilfe der `createView`-Methode erstellen, um einen `LocalVideoStream` zu rendern. Sie können dann `view.target` an ein beliebiges Benutzeroberflächenelement anfügen. 

```JavaScript
async function localVideoView() {
    rendererLocal = new Renderer(localVideoStream);
    const view = await rendererLocal.createView();
    document.getElementById("myVideo").appendChild(view.target);
}
```
Alle Remoteteilnehmer sind über die `remoteParticipants`-Sammlung für eine Anrufinstanz verfügbar. Sie müssen die Remoteteilnehmer des aktuellen Anrufs abonnieren und am Ereignis `remoteParticipantsUpdated` lauschen, um die hinzugefügten Remoteteilnehmer zu abonnieren.

```JavaScript
function subscribeToRemoteParticipantInCall(callInstance) {
    callInstance.remoteParticipants.forEach( p => {
        subscribeToRemoteParticipant(p);
    })
    callInstance.on('remoteParticipantsUpdated', e => {
        e.added.forEach( p => {
            subscribeToRemoteParticipant(p);
        })
    });   
}
```
Sie können die `remoteParticipants`-Sammlung des aktuellen Anrufs abonnieren und die `videoStreams`-Sammlungen untersuchen, um die Streams jedes Teilnehmers aufzulisten. Außerdem müssen Sie das remoteParticipantsUpdated-Ereignis abonnieren, um die hinzugefügten Remoteteilnehmer zu verarbeiten. 

```JavaScript
function subscribeToRemoteParticipant(remoteParticipant) {
    remoteParticipant.videoStreams.forEach(v => {
        handleVideoStream(v);
    });
    remoteParticipant.on('videoStreamsUpdated', e => {
        e.added.forEach(v => {
            handleVideoStream(v);
        })
    });
}
```
Zum Rendern eines `remoteVideoStream` müssen Sie ein `isAvailableChanged`-Ereignis abonnieren. Wenn sich die Eigenschaft `isAvailable` in `true` ändert, sendet ein Remoteteilnehmer einen Stream. Immer wenn sich die Verfügbarkeit eines Remotestreams ändert, können Sie wählen, ob Sie den gesamten `Renderer`, eine bestimmte `RendererView` zerstören oder sie behalten möchten, was jedoch zur Anzeige leerer Videobilder führt.
```JavaScript
function handleVideoStream(remoteVideoStream) {
    remoteVideoStream.on('availabilityChanged', async () => {
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
Wenn Sie einen `RemoteVideoStream` rendern möchten, müssen Sie eine neue Instanz von `Renderer` erstellen und anschließend eine neue `RendererView`-Instanz mithilfe der asynchronen `createView`-Methode erstellen. Sie können dann `view.target` an ein beliebiges Benutzeroberflächenelement anfügen. 

```JavaScript
async function remoteVideoView(remoteVideoStream) {
    rendererRemote = new Renderer(remoteVideoStream);
    const view = await rendererRemote.createView();
    document.getElementById("remoteVideo").appendChild(view.target);
}
```
## <a name="receive-an-incoming-call"></a>Empfangen eines eingehenden Anrufs
Sie müssen dem `incomingCall`-Ereignis von `callAgent` lauschen, um eingehende Anrufe zu verarbeiten. Sobald ein eingehender Anruf getätigt wird, müssen Sie lokale Kameras auflisten und eine `LocalVideoStream`-Instanz erstellen, um dem anderen Teilnehmer einen Videostream zu senden. Sie müssen auch `remoteParticipants` abonnieren, um remote Videostreams zu verarbeiten. Sie können den Anruf annehmen oder verweigern, indem Sie die `incomingCall`-Instanz verwenden. 

Platzieren Sie die Implementierung in `init()`, um eingehende Anrufe zu verarbeiten. 

```JavaScript
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
## <a name="subscribe-to-call-updates"></a>Abonnieren von Anrufupdates
Sie müssen das Ereignis abonnieren, wenn der Remoteteilnehmer den Anruf beendet, um Videorenderer und Umschaltflächenzustände zu verwerfen. 

Platzieren Sie die Implementierung in init(), um das `callsUpdated`-Ereignis zu abonnieren. 
 ```JavaScript 
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

:::image type="content" source="./media/javascript/1-on-1-video-calling.png" alt-text="1:1-Videoanruf":::

Sie können einen ausgehenden 1:1-Videoanruf tätigen, indem Sie eine Benutzer-ID im Textfeld eingeben und auf die Schaltfläche „Start Call“ (Anruf beginnen) klicken. 

## <a name="sample-code"></a>Beispielcode
Sie können die Beispiel-App von [GitHub](https://github.com/Azure-Samples/communication-services-javascript-quickstarts/tree/main/Add%201%20on%201%20video%20calling) herunterladen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Wenn Sie ein Communication Services-Abonnement bereinigen und entfernen möchten, können Sie die Ressource oder die Ressourcengruppe löschen. Wenn Sie die Ressourcengruppe löschen, werden auch alle anderen Ressourcen gelöscht, die ihr zugeordnet sind. Weitere Informationen zum Bereinigen von Ressourcen finden Sie [hier](https://docs.microsoft.com/azure/communication-services/quickstarts/create-communication-resource?tabs=windows&pivots=platform-azp#clean-up-resources).

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in den folgenden Artikeln:
- Sehen Sie sich das [Beispiel für Webanrufe](https://docs.microsoft.com/azure/communication-services/samples/web-calling-sample) an.
- Informieren Sie sich über die [Funktionen der Clientbibliothek für Telefonie](https://docs.microsoft.com/azure/communication-services/quickstarts/voice-video-calling/calling-client-samples?pivots=platform-web).
- Informieren Sie sich über die [Funktionsweise von Anrufen](https://docs.microsoft.com/azure/communication-services/concepts/voice-video-calling/about-call-types).
