---
title: Verwenden von Shaka Player mit Azure Media Services
description: Dieser Artikel erläutert die Verwendung von Shaka Player mit Azure Media Services.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 08/31/2020
ms.author: inhenkel
ms.custom: devx-track-js
ms.openlocfilehash: 9699e6ad3f004f94c83440dd39f13c6db887e224
ms.sourcegitcommit: 02bc06155692213ef031f049f5dcf4c418e9f509
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2021
ms.locfileid: "106281248"
---
# <a name="how-to-use-the-shaka-player-with-azure-media-services"></a>Verwenden von Shaka Player mit Azure Media Services

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

## <a name="overview"></a>Übersicht

Shaka Player ist eine Open-Source-JavaScript-Bibliothek für adaptive Medien. Der Player gibt adaptive Medienformate wie DASH und HLS in einem Browser wieder, und zwar ohne Plug-Ins oder Flash. Stattdessen verwendet Shaka Player die offenen Webstandards MSE (Media Source Extensions) und EME (Encrypted Media Extensions).

Es empfiehlt sich die Verwendung von [Mux.js](https://github.com/videojs/mux.js/), da Shaka Player sonst zwar das HLS-CMAF-Format unterstützen würde, aber nicht HLS TS.

Lesen Sie auch die offizielle [Dokumentation zu Shaka Player](https://shaka-player-demo.appspot.com/docs/api/tutorial-welcome.html).

## <a name="sample-code"></a>Beispielcode

Beispielcode für diesen Artikel steht unter [Azure-Samples/media-services-3rdparty-player-samples](https://github.com/Azure-Samples/media-services-3rdparty-player-samples) zur Verfügung.

## <a name="implementing-the-player"></a>Implementieren des Players

Befolgen Sie diese Anweisungen, wenn Sie eine eigene Instanz des Players implementieren müssen:

1. Erstellen Sie zum Hosten des Players eine `index.html`-Datei. Fügen Sie die folgenden Codezeilen hinzu (ersetzen Sie die Versionen ggf. durch neuere):

    ```html
    <html>
      <head>
        <script src="//cdn.jsdelivr.net/npm/shaka-player@3.0.1/dist/shaka-player.compiled.js"></script>
        <script src="//cdn.jsdelivr.net/npm/mux.js@5.6.3/dist/mux.js"></script>
        <script type="module" src="index.js"></script>
      </head>
      <body>
        <video id="video" controls></video>
      </body>
    </html>
    ```

1. Fügen Sie eine JavaScript-Datei mit folgendem Code hinzu:

    ```javascript
    // myScript.js
    shaka.polyfill.installAll();

    var video = document.getElementById('video');
    var player = new shaka.Player(video);
    window.player = player;

    var manifestUrl = 'https://amsplayeraccount-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/sample-vod.ism/manifest(format=m3u8-aapl)';
    player.load(manifestUrl);
    ```

1. Ersetzen Sie `manifestUrl` durch die HLS- oder DASH-URL des Streaminglocators Ihres Medienobjekts. Diese finden Sie im Azure-Portal auf der Seite des Streaminglocators.
    ![Streaminglocator-URLs](media/player-shaka-player-how-to/streaming-urls.png)

1. Führen Sie einen Server aus (z. B. mit `npm http-server`). Der Player sollte funktionieren.

## <a name="set-up-captions"></a>Einrichten von Untertiteln

### <a name="set-up-vod-captions"></a>Einrichten von VOD-Untertiteln

Führen Sie die folgenden Codezeilen aus, und ersetzen Sie `captionUrl` durch Ihr VTT-Verzeichnis (die VTT-Datei muss sich auf demselben Host befinden, um CORS-Fehler zu vermeiden), `lang` durch die zweibuchstabige Sprachkennung und `type` durch `caption` oder `subtitle`:

```javascript
player.configure('streaming.alwaysStreamText', true)
player.load(manifestUrl).then(function(){
        player.addTextTrack(captionUrl, lang, type, 'text/vtt');
        var tracks = player.getTextTracks();
        player.selectTextTrack(tracks[0]);
});
```

### <a name="set-up-live-stream-captions"></a>Einrichten von Untertiteln für Livestreams

Aktivieren Sie Untertitel in Livestreams, indem Sie die folgende Codezeile hinzufügen:

```javascript
player.setTextTrackVisibility(true)
```

## <a name="set-up-token-authentication"></a>Einrichten der Tokenauthentifizierung

Führen Sie die folgenden Codezeilen aus, und ersetzen Sie `token` durch Ihre Tokenzeichenfolge:

```javascript
player.getNetworkingEngine().registerRequestFilter(function (type, request) {
  if (type === shaka.net.NetworkingEngine.RequestType.LICENSE) {
    request.headers['Authorization'] = 'Bearer ' + token;
  }
});
```

## <a name="set-up-aes-128-encryption"></a>Einrichten der AES-128-Verschlüsselung

Shaka Player unterstützt die AES-128-Verschlüsselung derzeit nicht.

Verfolgen Sie den Status dieses Features in diesem [Issue](https://github.com/google/shaka-player/issues/850) in GitHub.

## <a name="set-up-drm-protection"></a>Einrichten von DRM-Schutz

Shaka Player verwendet Encrypted Media Extensions (EME), für die eine sichere URL verwendet werden muss. Zum Testen von mit DRM geschützten Inhalten muss also HTTPS verwendet werden. Wenn die Website HTTPS verwendet, gilt dies auch für das Manifest und jedes einzelne Segment. Dies liegt an den Anforderungen gemischter Inhalte.

Shaka verwaltet die URLs der Lizenzserver bevorzugt in dieser Reihenfolge:

1. Die für das Debuggen verwendete ClearKey-Konfiguration sollte alles andere überschreiben. (Die Anwendung kann dennoch weiterhin einen ClearKey-Lizenzserver angeben.)
2. Von der Anwendung konfigurierte Server, sofern vorhanden, sollten alle Angaben aus dem Manifest überschreiben.
3. Im Manifest angegebene Lizenzserver werden nur dann verwendet, wenn sonst nichts angegeben ist.

Um die Lizenzserver-URL für Widevine oder PlayReady anzugeben, können Sie folgenden Code verwenden:

```javascript
player.configure({
  drm: {
    servers: {
      "com.widevine.alpha": "YOUR WIDEVINE LICENSE URL",
      "com.microsoft.playready": "YOUR PLAYREADY LICENSE URL"
    }
  }
});

```

Alle FairPlay-Inhalte erfordern das Festlegen eines Serverzertifikats. Dieses wird in der Player-Konfiguration festgelegt:

```javascript
const req = await fetch("YOUR FAIRPLAY CERTIFICATE URL");
const cert = await req.arrayBuffer();
player.configure('drm.advanced.com\\.apple\\.fps\\.1_0.serverCertificate', new Uint8Array(cert));
```

Weitere Informationen finden Sie in der [Dokumentation des DRM-Schutzes für Shaka Player](https://shaka-player-demo.appspot.com/docs/api/tutorial-drm-config.html).

## <a name="next-steps"></a>Nächste Schritte

* [Übersicht über Azure Media Player](../azure-media-player/azure-media-player-overview.md)
* [Schnellstart: Verschlüsseln von Inhalten mithilfe des Portals](drm-encrypt-content-how-to.md)
