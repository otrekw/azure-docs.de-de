---
title: Verwenden des Player-Widgets des Azure Video Analyzers
description: In diesem Referenzartikel wird erläutert, wie Sie Ihrer Anwendung ein Video Analyzer-Player-Widget hinzufügen.
ms.service: azure-video-analyzer
ms.topic: reference
ms.date: 05/11/2021
ms.openlocfilehash: 12565f2ca1a86d3a9e57fef652abab9564cb7b5e
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110386098"
---
# <a name="using-the-azure-video-analyzer-player-widget"></a>Verwenden des Player-Widgets des Azure Video Analyzers

In diesem Tutorial erfahren Sie, wie Sie das Azure Video Analyzer Player-Widget in Ihrer Anwendung verwenden.  Bei diesem Code handelt es sich um einen leicht einzubettenden Widget, das es Ihren Endbenutzern ermöglicht, Videos abzuspielen und durch Teile einer segmentierten Videodatei zu navigieren.  Erstellen Sie für diesen Zweck eine statische HTML-Seite mit eingebettetem Widget und allen Teilen, damit es funktioniert.

In diesem Lernprogramm führen Sie folgende Schritte aus:

> [!div class="checklist"]
> * Erstellen Sie ein Token
> * Listen Sie Videos auf
> * Erhalten Sie die Basis-URL zur Wiedergabe einer [Videoanwendungsressource](./terminology.md#video)
> * Erstellen Sie eine Seite mit dem Player
> * Übergeben Sie den Streaming-Endpunkt plus Token an den Player

## <a name="suggested-pre-reading"></a>Empfohlene Lektüre zur Vorbereitung

- [Webkomponenten](https://developer.mozilla.org/en-US/docs/Web/Web_Components)
- [TypeScript](https://www.typescriptlang.org)

## <a name="prerequisites"></a>Voraussetzungen

Dies sind die Voraussetzungen für dieses Tutorial:
* Ein Azure-Konto mit einem aktiven Abonnement. Sie können ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), falls Sie noch keins besitzen 
* [Visual Studio Code](https://code.visualstudio.com/) oder einen anderen Editor für die HTML-Datei.
.
* Entweder [kontinuierliche Videoaufzeichnung und -Wiedergabe](./use-continuous-video-recording.md) oder [Bewegungserkennung und Videoaufzeichnung auf Edge-Geräten](./detect-motion-record-video-clips-cloud.md)

## <a name="create-a-token"></a>Erstellen Sie ein Token

In diesem Abschnitt erstellen wir ein JWT-Token, das wir später im Dokument verwenden.  Wir verwenden eine Beispielanwendung, die das JWT-Token erstellt und Ihnen alle Felder zur Verfügung stellt, die zum Erstellen der Zugriffsrichtlinie erforderlich sind.

> [!NOTE] 
> Wenn Sie mit dem Generieren eines JWT-Tokens auf der Grundlage eines RSA- oder ECC-Zertifikats vertraut sind, können Sie diesen Abschnitt überspringen.

1. Laden Sie die JWTTokenIssuer-Anwendung [hier](https://github.com/Azure-Samples/video-analyzer-iot-edge-csharp/tree/main/src/jwt-token-issuer/readme.md) herunter.

   > [!NOTE] 
   > Weitere Informationen zum Konfigurieren ihrer Zielgruppenwerte finden Sie in diesem [Artikel](./access-policies.md)

2. Starten Sie Visual Studio Code, und öffnen Sie den Ordner, der die *.sln-Datei enthält.
3. Navigieren Sie im Explorer-Bereich zur Datei „program.cs“
4. Passen Sie Zeile 77 an: Ändern Sie die Zielgruppe in Ihren Video Analyzer-Endpunkt plus /videos/*, sodass sie wie folgt aussieht:

   ```
   https://{Azure Video Analyzer Account ID}.api.{Azure Long Region Code}.videoanalyzer.azure.net/videos/*
   ```
5. Passen Sie Zeile 78 an: Ändern Sie den Aussteller in den Ausstellerwert Ihres Zertifikats.  Beispiel: https://contoso.com

   > [!NOTE] 
   > Den Video Analyzer-Endpunkt finden Sie im Übersicht-Abschnitt der Video Analyzer-Ressource in Azure. Klicken Sie auf den Link „JSON-View" 

   > [!div class="mx-imgBorder"]
   > :::image type="content" source="./media/player-widget/endpoint.png" alt-text="Playe-Widget – Endpunkt":::
6. Speichern Sie die Datei .
7. Drücken Sie `F5`, um die JWTTokenIssuer-Anwendung auszuführen.

Hiermit wird die Anwendung erstellt und ausgeführt.  Nach dem Erstellen wird sie ausgeführt, indem ein Zertifikat über openssl erstellt wird.  Sie können auch die JWTTokenIssuer.exe Datei ausführen, die sich im Debug-Ordner befindet.  Der Vorteil der Ausführung der Anwendung besteht darin, dass Sie Eingabeoptionen wie folgt angeben können:

- .JwtTokenIssuer [--audience=<audience>] [--issuer=<issuer>] [--expiration=<expiration>] [--certificatePath=<filepath> --certificatePassword=<password>]

JWTTokenIssuer erstellt das JWT-Token und die folgenden erforderlichen Komponenten:

- `kty`; `alg`; `kid`; `n`; `e`

Stellen Sie sicher, dass Sie diese Werte zur späteren Verwendung kopieren.

## <a name="create-an-access-policy"></a>Erstellen einer Zugriffsrichtlinie

Die Zugriffsrichtlinien bestimmen die Berechtigungen und die Dauer des Zugriffs auf einen bestimmten Video Analyzer-Videostream.  Für dieses Tutorial konfigurieren wir eine Zugriffsrichtlinie für den Video Analyzer im Azure-Portal.  

1. Melden Sie sich beim Azure-Portal an, und navigieren Sie zu Ihrer Ressourcengruppe, in der sich Ihr Video Analyzer-Konto befindet.
1. Wählen Sie die Azure Video Analyzer-Ressource aus.
1. Wählen Sie unter Video Analyzer die Option Zugriffsrichtlinien aus

   > [!div class="mx-imgBorder"]
   > :::image type="content" source="./media/player-widget/portal-access-policies.png" alt-text="Player-Widget - Portal-Zugriffsrichtlinien":::    
1. Klicken Sie auf neu, und geben Sie Folgendes ein:

   > [!NOTE] 
   > Diese Werte entstammen der JWTTokenIssuer-Anwendung, die im vorherigen Schritt erstellt wurde.

   - Name der Zugriffsrichtlinie – beliebiger Name

   - Aussteller – muss mit dem JWT-Token-Aussteller übereinstimmen 

   - Zielgruppe - Zielgruppe für das JWT-Token - ${System.Runtime.BaseResourceUrlPattern} ist die Standardeinstellung.  Weitere Informationen zur Zielgruppe und ${System.Runtime.BaseResourceUrlPattern} finden Sie in diesem [Artikel](./access-policies.md)

   - Schlüsseltyp – kty – RSA 

   - Algorithmus - die unterstützten Werte sind RS256, RS384, RS512

   - Schlüssel-ID – Kind – von Ihrem Zertifikat erstellt

   - N-Wert - Für RSA ist der N-Wert der Modulus

   - E-Wert - Für RSA ist der E-Wert der öffentliche Exponent

   > [!div class="mx-imgBorder"]
   > :::image type="content" source="./media/player-widget/access-policies-portal.png" alt-text="Player-Widget - Portal-Zugriffsrichtlinien":::     
1. klicken `save`.

## <a name="list-video-analyzer-video-resources"></a>Videoressourcen des Video Analyzers auflisten

Als Nächstes müssen wir eine Liste von Videoressourcen erstellen.  Dies erfolgt über einen REST-Aufruf an den Kontoendpunkt, den wir oben verwendeten, und der anhand des von uns erstellten Tokens authentifiziert wird.

Es sind viele Möglichkeiten vorhanden, eine GET-Anforderung an eine REST-API zu senden, dafür verwenden wir jedoch eine JavaScript-Funktion.  Der folgende Code verwendet [xmlHttpRequest](https://www.w3schools.com/xml/ajax_xmlhttprequest_create.asp) in Verbindung mit Werten, die wir in den `clientApiEndpointUrl` und `token`-Feldern auf der Seite speichern, um eine synchrone Anforderung zu `GET` senden.  Anschließend wird die resultierende Liste der Videos verwendet und im `videoList` Textbereich gespeichert, den wir auf der Seite eingerichtet haben.

```javascript
function getVideos()
{
    var xhttp = new XMLHttpRequest();
    var getUrl = document.getElementById("clientApiEndpointUrl").value + "/videos?api-version=2021-05-01-preview";
    xhttp.open("GET", getUrl, false);
    xhttp.setRequestHeader("Authorization", "Bearer " + document.getElementById("token").value);
    xhttp.send();
    document.getElementById("videoList").value = xhttp.responseText.toString();
}
```

## <a name="add-the-video-analyzer-player-component"></a>Fügen Sie die Video Analyzer Player-Komponente hinzu

Da wir nun über eine Client-API-Endpunkt-URL, ein Token und einen Videonamen verfügen, können wir den Player zur Seite hinzufügen.

1. Schließen Sie das Player-Modul selbst ein, indem Sie das Paket direkt zu Ihrer Seite hinzufügen.  Sie können entweder die NPM-Paketrichtung in Ihre Anwendung einbinden oder zur Laufzeit dynamisch einbetten lassen, wie hier zu sehen:
   ```html
   <script async type="module" src="https://unpkg.com/@azure/video-analyzer-widgets"></script>
   ```
1. Fügen Sie ein AVA-Player-Element zum Dokument hinzu:
   ```html
   <ava-player width="720px" id="avaPlayer"></ava-player>
   ```
1. Rufen Sie einen Link zum Video Analyzer-Player-Widget ab, das sich auf der Seite befindet:
   ```javascript
   const avaPlayer = document.getElementById("avaPlayer");
   ```
1. Um den Player mit den Werten zu konfigurieren, über die Sie verfügen, müssen Sie die Werte wie hier angezeigt als Objekt einrichten:
   ```javascript
   avaPlayer.configure( {
      token: document.getElementById("token").value,
      clientApiEndpointUrl: document.getElementById("clientApiEndpointUrl").value,
      videoName: document.getElementById("videoName").value
   } );
   ```
1. Laden sie das Video in den Player, um zu beginnen
   ```javascript
   avaPlayer.load();
   ```

## <a name="put-it-all-together"></a>Korrektes Zusammenfügen

Wenn wir die oben genannten Webelemente kombinieren, erhalten wir die folgende statische HTML-Seite, die es uns ermöglicht, einen Kontoendpunkt und ein Token zum Anzeigen eines Videos zu verwenden.

```html
<html>
<head>
<title>Video Analyzer Player Widget Demo</title>
</head>
<script async type="module" src="https://unpkg.com/@azure/video-analyzer-widgets"></script>
<body>
<script>
    function getVideos()
    {
        var xhttp = new XMLHttpRequest();
        var getUrl = document.getElementById("clientApiEndpointUrl").value + "/videos?api-version=2021-05-01-preview";
        xhttp.open("GET", getUrl, false);
        xhttp.setRequestHeader("Authorization", "Bearer " + document.getElementById("token").value);
        xhttp.send();
        document.getElementById("videoList").value = xhttp.responseText.toString();
    }
    function playVideo() {
        const avaPlayer = document.getElementById("avaPlayer");
        avaPlayer.configure( {
            token: document.getElementById("token").value,
            clientApiEndpointUrl: document.getElementById("clientApiEndpointUrl").value,
            videoName: document.getElementById("videoName").value
        } );
        avaPlayer.load();
    }
</script>
Client API endpoint URL: <input type="text" id="clientApiEndpointUrl" /><br><br>
Token: <input type="text" id="token" /><br><br>
<button type="submit" onclick="getVideos()">Get Videos</button><br><br>
<textarea rows="20" cols="100" id="videoList"></textarea><br><br>
Video name: <input type="text" id="videoName" /><br><br>
<button type="submit" onclick="playVideo()">Play Video</button><br><br>
<ava-player width="720px" id="avaPlayer"></ava-player>
</body>
</html>
```

## <a name="host-the-page"></a>Hosten der Seite

Sie können diese Seite lokal testen, Sie möchten jedoch möglicherweise eine gehostete Version testen.  Falls Sie über keine schnelle Möglichkeit zum Hosten einer Seite verfügen, finden Sie hier Anweisungen zur Verwendung [statischer Websites](../../storage/blobs/storage-blob-static-website.md) mit Storage.  Wie folgt, finden Sie eine verkürzte Version [dieser ausführlicheren Anweisungen,](../../storage/blobs/storage-blob-static-website-how-to.md) die für die in diesem Tutorial verwendeten Dateien aktualisiert wurden.

1. Erstellen eines Speicherkontos
1. Klicken Sie unter `Data management`, auf der linken Seite auf `Static website`
1. `Enable` die statische Website im Speicherkonto
1. Für `Index document name`, geben Sie `index.html` ein
1. Für `Error document path`, geben Sie `404.html` ein
1. Wählen Sie oben `Save` aus
1. Beachten Sie `Primary endpoint` dass angezeigt wird - dies wird Ihre Website sein
1. Klicken Sie auf `$web` oben`Primary endpoint`
1. Laden Sie ihre `Upload` statische HTML-Seite mithilfe der oberen Schaltfläche als `index.html` hoch

## <a name="play-a-video"></a>Spielen Sie ein Video ab

Nachdem Sie nun die Seite gehostet haben, navigieren Sie dorthin, und Sie sollten die Schritte ausführen können.

1. Fügen Sie `Client API endpoint URL` und `Token` ein
1. Wählen Sie `Get videos`.
1. Wählen Sie in der Videoliste einen Videonamen aus, und füllen Sie ihn im `Video name` Eingabefeld aus
1. Wählen Sie `Play video`.

## <a name="additional-details"></a>Zusätzliche Details

### <a name="refreshing-the-access-token"></a>Aktualisieren des Zugriffstokens

Der Player verwendet das Zugriffstoken, das wir zuvor erstellt haben, um ein Wiedergabe-Autorisierungstoken abzurufen.  Token laufen in regelmäßigen Abständen ab, daher müssen sie aktualisiert werden.  Es gibt zwei Möglichkeiten, das Zugriffstoken für den Player zu aktualisieren, nachdem Sie ein neues generiert haben.

* Aktiv die Widget-Methode `setAccessToken` aufrufen
    ```typescript
    avaPlayer.setAccessToken('<NEW-ACCESS-TOKEN>');
    ```
* Handeln auf das `TOKEN_EXPIRED`-Ereignis durch Abhören dieses Ereignisses
    ```typescript
    avaPlayer.addEventListener(PlayerEvents.TOKEN_EXPIRED, () => {
        avaPlayer.setAccessToken('<YOUR-NEW-TOKEN>');
    });
    ```

Das `TOKEN_EXPIRED`-Ereignis tritt 5 Sekunden vor Ablauf des Tokens auf.  Wenn Sie einen Ereignishörer einstellen, empfiehlt es sich, dies vor dem Aufrufen der `load`-Funktion im Player-Widget vorzunehmen.

### <a name="configuration-details"></a>Konfigurationsdetails

Wir haben eine einfache Konfiguration für den obengenannten Player vorgenommen, es unterstützt jedoch eine größere Auswahl an Optionen für Konfigurationswerte.  Die unterstützten Werte sind nachfolgend aufgeführt:

| Name   | Typ             | Beschreibung                         |
| ------ | ---------------- | ----------------------------------- |
| token  | Zeichenfolge | Ihr JWT-Token für das Widget |
| Videoname | Zeichenfolge | Die Bezeichnung der Video-Ressource  |
| clientApiEndpointUrl | Zeichenfolge | Die Endpunkt-URL für die Client-API |

### <a name="alternate-ways-to-load-the-code-into-your-application"></a>Alternative Möglichkeiten zum Laden des Codes in Ihre Anwendung

Das Paket, das zum Herunterladen des Codes in Ihre Anwendung verwendet wird, ist ein NPM-Paket [hier](https://www.npmjs.com/package/video-analyzer-widgets).  Während im obigen Beispiel die neueste Version zur Laufzeit direkt aus dem Repositorium geladen wurde, können Sie das Paket auch herunterladen und lokal installieren:

```bash
npm install @azure/video-analyzer/widgets
```

Sie können sie auch in Ihren Anwendungscode importieren, mithilfe von:

```typescript
import { Player } from '@video-analyzer/widgets';
```

Wenn Sie diese Methode zum Importieren verwenden, müssen Sie das Player-Objekt programmgesteuert erstellen, nachdem der Import abgeschlossen ist.  Im obigen Beispiel haben Sie das Modul mithilfe des `ava-player`-HTML-Tags zur Seite hinzugefügt.  Um ein Player-Objekt mithilfe von Code zu erstellen, können Sie in die folgenden Schritte ausführen, entweder in JavaScript:

```javascript
const avaPlayer = new ava.widgets.player();
```

oder in Typescript:

```typescript
const avaPlayer = new Player();
```

Anschließend müssen Sie es dem HTML-Code hinzufügen:

```javascript
document.firstElementChild.appendChild(avaPlayer);
```

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum [API-Widget](https://github.com/Azure/video-analyzer/widgets)
