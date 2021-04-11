---
title: Vollständiges Setup von Azure Media Player
description: Erfahren Sie, wie Azure Media Player eingerichtet wird.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: how-to
ms.date: 04/05/2021
ms.custom: devx-track-js
ms.openlocfilehash: d7c103a7eba2e5da7ac040b50fcc0009a0b7237b
ms.sourcegitcommit: 56b0c7923d67f96da21653b4bb37d943c36a81d6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2021
ms.locfileid: "106449867"
---
# <a name="azure-media-player-full-setup"></a>Vollständiges Setup von Azure Media Player #

Die Einrichtung von Azure Media Player ist einfach. Es dauert nur einige Augenblicke, bis die einfache Wiedergabe von Medieninhalten direkt über Ihr Azure Media Services-Konto möglich ist. Außerdem stehen [Beispiele](https://github.com/Azure-Samples/azure-media-player-samples) im Beispielverzeichnis des Release zur Verfügung.

## <a name="step-1-include-the-javascript-and-css-files-in-the-head-of-your-page"></a>Schritt 1: Fügen Sie die JavaScript- und CSS-Dateien in den Header Ihrer Seite ein. ##

Mithilfe von Azure Media Player können Sie auf die Skripts der gehosteten CDN-Version zugreifen. Es wird in letzter Zeit häufig empfohlen, JavaScript vor dem Endtag des Hauptteils `<body>` anstelle von `<head>` einzufügen, Azure Media Player enthält jedoch eine 'HTML5 Shiv', die im Header platziert werden muss, damit ältere IE-Versionen das Videotag als gültiges Element anerkennen.

> [!NOTE]
> Wenn Sie bereits eine HTML5-Shiv wie [Modernizr](https://modernizr.com/) verwenden, können Sie das JavaScript von Azure Media Player an jeder beliebigen Position einfügen. Vergewissern Sie sich aber, dass Ihre Version von Modernizr die Shiv für Video enthält.

### <a name="cdn-version"></a>CDN-Version ###

```html
    <link href="//amp.azure.net/libs/amp/latest/skins/amp-default/azuremediaplayer.min.css" rel="stylesheet">
    <script src= "//amp.azure.net/libs/amp/latest/azuremediaplayer.min.js"></script>
```

> [!IMPORTANT]
> Es wird **NICHT** empfohlen, die Version vom Typ `latest` in der Produktion zu nutzen, da sie bedarfsabhängigen Änderungen unterliegt. Ersetzen Sie `latest` durch eine Version von Azure Media Player. Ersetzen Sie beispielsweise `latest` durch `2.1.1`. Azure Media Player-Versionen können [hier](https://amp.azure.net/libs/amp/latest/docs/changelog.html) abgefragt werden.

> [!NOTE]
> Seit Version `1.2.0` ist es nicht mehr erforderlich, den Speicherort in die Angaben zum Fallback einzuschließen (der Speicherort wird automatisch aus dem relativen Pfad der Datei „azuremediaplayer.min.js“ ermittelt). Sie können den Speicherort der Angaben zum Fallback ändern, indem Sie im `<head>` das folgende Skript nach den oben aufgeführten Skripts hinzufügen.

> [!NOTE]
> Aufgrund der Beschaffenheit von Flash- und Silverlight-Plugins sollten die SWF- und XAP-Dateien auf einer Domäne ohne vertrauliche Informationen oder Daten gehostet werden – die im Azure CDN gehostete Version nimmt Ihnen diese Mühe automatisch ab.

```javascript
    <script>
      amp.options.flashSS.swf = "//amp.azure.net/libs/amp/latest/techs/StrobeMediaPlayback.2.0.swf"
      amp.options.silverlightSS.xap = "//amp.azure.net/libs/amp/latest/techs/SmoothStreamingPlayer.xap"
    </script>
```

## <a name="step-2-add-an-html5-video-tag-to-your-page"></a>Schritt 2: Hinzufügen eines HTML5-Videotags zu Ihrer Seite ##

Mit Azure Media Player können Sie ein HTML5-Videotag zum Einbetten eines Videos verwenden. Azure Media Player liest dann das Tag und stellt seine Funktion in allen Browsern sicher, nicht nur Browsern mit Unterstützung für HTML5-Video. Jenseits vom grundlegenden Markup benötigt Azure Media Player aber noch einige zusätzliche Informationen.

1. Das `<data-setup>`-Attribut auf dem `<video>` weist Azure Media Player an, das Video automatisch einzurichten, wenn die Seite bereit ist, und alle Informationen (im JSON-Format) aus dem Attribut zu lesen.
1. Das `id`-Attribut: Sollte für jedes Video auf der gleichen Seite verwendet werden und muss für jedes Video eindeutig sein.
1. Das `class`-Attribut enthält zwei Klassen:
    - `azuremediaplayer` wendet Formatvorlagen an, die für die Funktionalität der Benutzeroberfläche von Azure Media Player erforderlich sind.
    - `amp-default-skin` wendet den Standardskin auf die HTML5-Steuerelemente an.
1. `<source>` enthält zwei erforderliche Attribute.
    - Das `src`-Attribut kann eine * *.ism/manifest*-Datei aus Azure Media Services enthalten, Azure Media Player fügt dem Player automatisch die URLs für DASH, SMOOTH und HLS hinzu
    - Das `type`-Attribut gibt den erforderlichen MIME-Typ des Streams an. Der *".ism/manifest"* zugeordnete MIME-Typ ist *"application/vnd.ms-sstr+xml"*
1. Das *optionale* `<data-setup>`-Attribut der `<source>` teilt Azure Media Player mit, ob es besondere Übermittlungsrichtlinien für den Stream von Azure Media Services gibt, einschließlich – aber ohne Anspruch auf Vollständigkeit – Verschlüsselungstyp (AES oder PlayReady, Widevine oder FairPlay) und Token.

Schließen Sie Attribute, Einstellungen, Quellen und Titel ein oder aus, genau wie bei HTML5-Video.

```html
    <video id="vid1" class="azuremediaplayer amp-default-skin" autoplay controls width="640" height="400" poster="poster.jpg" data-setup='{"techOrder": ["azureHtml5JS", "flashSS", "html5FairPlayHLS","silverlightSS", "html5"], "nativeControlsForTouch": false}'>
        <source src="http://amssamples.streaming.mediaservices.windows.net/91492735-c523-432b-ba01-faba6c2206a2/AzureMediaServicesPromo.ism/manifest" type="application/vnd.ms-sstr+xml" />
        <p class="amp-no-js">
            To view this video please enable JavaScript, and consider upgrading to a web browser that supports HTML5 video
        </p>
    </video>
```

Standardmäßig befindet sich die große Wiedergabeschaltfläche in der oberen linken Ecke, sodass die interessanten Teile des Posters nicht überdeckt werden. Wenn Sie es vorziehen, die große Wiedergabeschaltfläche zu zentrieren, können Sie Ihrem `<video>`-Element ein zusätzliches `amp-big-play-centered` `class` hinzufügen.

### <a name="alternative-setup-for-dynamically-loaded-html"></a>Alternatives Setup für dynamisch geladenen HTML ###

Wenn Ihre Webseite oder Anwendung das Videotag dynamisch lädt (ajax, appendChild usw.), sodass dieses möglicherweise beim Laden der Seite noch gar nicht vorhanden ist, sollten Sie den Player manuell einrichten, statt sich auf das data-setup-Attribut zu verlassen. Entfernen Sie dazu zunächst das data-setup-Attribut aus dem Tag, damit es bei der Initialisierung des Players nicht zu Verwirrung kommt. Führen Sie anschließend einige Zeit nach dem Laden des Azure Media Player-JavaScripts und nach dem Laden des Videotags in das DOM das folgende JavaScript aus.

```javascript
    var myPlayer = amp('vid1', { /* Options */
            techOrder: ["azureHtml5JS", "flashSS", "html5FairPlayHLS","silverlightSS", "html5"],
            "nativeControlsForTouch": false,
            autoplay: false,
            controls: true,
            width: "640",
            height: "400",
            poster: ""
        }, function() {
              console.log('Good to go!');
               // add an event listener
              this.addEventListener('ended', function() {
                console.log('Finished!');
            });
          }
    );
    myPlayer.src([{
        src: "http://samplescdn.origin.mediaservices.windows.net/e0e820ec-f6a2-4ea2-afe3-1eed4e06ab2c/AzureMediaServices_Overview.ism/manifest",
        type: "application/vnd.ms-sstr+xml"
    }]);
```

Das erste Argument in der `amp`-Funktion ist die ID Ihres Videotags. Ersetzen Sie sie durch ihre eigene.

Das zweite Argument ist ein Optionsobjekt. Es ermöglicht Ihnen das Festlegen zusätzlicher Optionen wie beim data-setup-Attribut.

Das dritte Argument ist ein Bereitschaftsrückruf. Diese Funktion wird nach der Initialisierung von Azure Media Player aufgerufen. Im Bereitschaftsrückruf bezeichnet das 'this'-Objekt die Playerinstanz.

Statt eine Element-ID zu verwenden, können Sie auch einen Verweis auf das Element übergeben.

```javascript

    amp(document.getElementById('example_video_1'), {/*Options*/}, function() {
        //This is functionally the same as the previous example.
    });
    myPlayer.src([{ src: "//example/path/to/myVideo.ism/manifest", type: "application/vnd.ms-sstr+xml"]);
```

## <a name="next-steps"></a>Nächste Schritte ##

- [Schnellstart für Azure Media Player](azure-media-player-quickstart.md)
