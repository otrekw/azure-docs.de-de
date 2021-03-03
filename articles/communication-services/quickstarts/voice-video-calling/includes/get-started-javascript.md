---
title: Schnellstart – Hinzufügen von VoIP-Telefonie zu einer Web-App mithilfe von Azure Communication Services
description: In diesem Tutorial erfahren Sie, wie Sie die Clientbibliothek für Telefonie von Azure Communication Services für JavaScript verwenden.
author: ddematheu
ms.author: nimag
ms.date: 08/11/2020
ms.topic: quickstart
ms.service: azure-communication-services
ms.openlocfilehash: d27a79e180a0219773a3094fb85f842773d75183
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101656615"
---
In diesem Schnellstart erfahren Sie, wie Sie einen Anruf mithilfe der Clientbibliothek für Telefonie von Azure Communication Services für JavaScript beginnen.
Dieses Dokument verweist auf Typen in Version 1.0.0-beta.5 der aufrufenden Bibliothek.

> [!NOTE]
> Dieses Dokument verwendet Version 1.0.0-beta.6 der aufrufenden Clientbibliothek.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Node.js](https://nodejs.org/), Active LTS- und Maintenance LTS-Versionen (8.11.1 und 10.14.1 empfohlen)
- Eine aktive Communication Services-Ressource. [Erstellen Sie eine Communication Services-Ressource](../../create-communication-resource.md).
- Ein Benutzerzugriffstoken, um den Anruf-Client zu instanziieren. Erfahren Sie mehr über das [Erstellen und Verwalten von Benutzerzugriffstoken](../../access-tokens.md).


[!INCLUDE [Calling with JavaScript](./get-started-javascript-setup.md)]

Der Code lautet wie folgt:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Communication Client - Calling Sample</title>
  </head>
  <body>
    <h4>Azure Communication Services</h4>
    <h1>Calling Quickstart</h1>
    <input 
      id="callee-id-input"
      type="text"
      placeholder="Who would you like to call?"
      style="margin-bottom:1em; width: 200px;"
    />
    <div>
      <button id="call-button" type="button" disabled="true">
        Start Call
      </button>
      &nbsp;
      <button id="hang-up-button" type="button" disabled="true">
        Hang Up
      </button>
    </div>
    <script src="./bundle.js"></script>
  </body>
</html>
```

Erstellen Sie im Stammverzeichnis Ihres Projekts eine Datei mit dem Namen **client.js**, die die Anwendungslogik für diese Schnellstartanleitung enthalten soll. Fügen Sie den folgenden Code hinzu, um den Client für Telefonie zu importieren und Verweise auf die DOM-Elemente abzurufen, damit wir unsere Geschäftslogik anfügen können. 

```javascript
import { CallClient, CallAgent } from "@azure/communication-calling";
import { AzureCommunicationTokenCredential } from '@azure/communication-common';

let call;
let callAgent;
const calleeInput = document.getElementById("callee-id-input");
const callButton = document.getElementById("call-button");
const hangUpButton = document.getElementById("hang-up-button");
```

## <a name="object-model"></a>Objektmodell

Die folgenden Klassen und Schnittstellen befassen sich mit einigen der wichtigsten Features der Azure Communication Services-Clientbibliothek „Calling“:

| Name                             | Beschreibung                                                                                                                                 |
| ---------------------------------| ------------------------------------------------------------------------------------------------------------------------------------------- |
| CallClient                       | CallClient ist der Haupteinstiegspunkt in die Clientbibliothek „Calling“.                                                                       |
| CallAgent                        | CallAgent dient zum Starten und Verwalten von Anrufen.                                                                                            |
| AzureCommunicationTokenCredential | Die AzureCommunicationTokenCredential-Klasse implementiert die CommunicationTokenCredential-Schnittstelle, die zum Instanziieren von CallAgent verwendet wird. |


## <a name="authenticate-the-client"></a>Authentifizieren des Clients

Sie müssen `<USER_ACCESS_TOKEN>` durch ein gültiges Benutzerzugriffstoken für Ihre Ressource ersetzen. Wenn Sie noch über kein Token verfügen, finden Sie in der Dokumentation zu [Benutzerzugriffstoken](../../access-tokens.md) weitere Informationen. Initialisieren Sie mithilfe von `CallClient` eine `CallAgent`-Instanz mit einem `CommunicationTokenCredential`-Objekt, das es uns ermöglicht, Anrufe zu tätigen und zu empfangen. Fügen Sie den folgenden Code zu **client.js** hinzu.

```javascript
async function init() {
    const callClient = new CallClient();
    const tokenCredential = new AzureCommunicationTokenCredential("<USER ACCESS TOKEN>");
    callAgent = await callClient.createCallAgent(tokenCredential);
    callButton.disabled = false;
}
init();
```

## <a name="start-a-call"></a>Beginnen eines Anrufs

Fügen Sie einen Ereignishandler hinzu, um einen Anruf zu initiieren, wenn auf `callButton` geklickt wird:

```javascript
callButton.addEventListener("click", () => {
    // start a call
    const userToCall = calleeInput.value;
    call = callAgent.startCall(
        [{ communicationUserId: userToCall }],
        {}
    );
    // toggle button states
    hangUpButton.disabled = false;
    callButton.disabled = true;
});
```

## <a name="end-a-call"></a>Beenden eines Anrufs

Fügen Sie einen Ereignislistener hinzu, um den aktuellen Anruf zu beenden, wenn auf `hangUpButton` geklickt wird:

```javascript
hangUpButton.addEventListener("click", () => {
  // end the current call
  call.hangUp({ forEveryone: true });

  // toggle button states
  hangUpButton.disabled = true;
  callButton.disabled = false;
});
```

Die Eigenschaft `forEveryone` beendet den Anruf für alle Anrufteilnehmer.

## <a name="run-the-code"></a>Ausführen des Codes

Verwenden Sie `webpack-dev-server`, um Ihre App zu erstellen und auszuführen. Führen Sie den folgenden Befehl aus, um den Anwendungshost auf einem lokalen Webserver zu bündeln:

```console
npx webpack-dev-server --entry ./client.js --output bundle.js --debug --devtool inline-source-map
```

Navigieren Sie in Ihrem Browser zu http://localhost:8080/. Daraufhin sollte Folgendes angezeigt werden:

:::image type="content" source="../media/javascript/calling-javascript-app.png" alt-text="Screenshot der fertigen JavaScript-Anwendung":::

Sie können einen ausgehenden VoIP-Anruf tätigen, indem Sie eine Benutzer-ID im Textfeld eingeben und auf die Schaltfläche **StartCall** (Anruf beginnen) klicken. Wenn Sie `8:echo123` anrufen, werden Sie mit einem Echobot verbunden. Dies eignet sich hervorragend für die ersten Schritte und zum Überprüfen, ob Ihre Audiogeräte funktionieren.
