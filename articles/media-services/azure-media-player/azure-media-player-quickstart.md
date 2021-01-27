---
title: Schnellstart für Azure Media Player
description: Hier werden die grundlegenden Schritte zum Einrichten von Azure Media Player erläutert.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: quickstart
ms.date: 04/20/2020
ms.openlocfilehash: 1634731381be6ebbb8750746f88b8ff9e761025d
ms.sourcegitcommit: aaa65bd769eb2e234e42cfb07d7d459a2cc273ab
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/27/2021
ms.locfileid: "98879189"
---
# <a name="azure-media-player-quickstart"></a>Schnellstart für Azure Media Player
Die Einrichtung von Azure Media Player ist einfach. Es dauert nur wenige Minuten, bis die einfache Wiedergabe von Medieninhalten über Ihr Azure Media Services-Konto möglich ist. In diesem Abschnitt werden die grundlegenden Schritte beschrieben, ohne zu sehr ins Detail zu gehen. Die nachfolgenden Abschnitte enthalten dann genauere Informationen zur Einrichtung und Konfiguration von Azure Media Player.  Fügen Sie dem `<head>`-Element Ihres Dokuments einfach Folgendes hinzu:

```html
    <link href="//amp.azure.net/libs/amp/latest/skins/amp-default/azuremediaplayer.min.css" rel="stylesheet">
    <script src= "//amp.azure.net/libs/amp/latest/azuremediaplayer.min.js"></script>
```

> [!IMPORTANT]
> Es wird **NICHT** empfohlen, die Version vom Typ `latest` in der Produktion zu nutzen, da sie bedarfsabhängigen Änderungen unterliegt. Ersetzen Sie `latest` durch eine andere Version von Azure Media Player. Ersetzen Sie beispielsweise `latest` durch `1.0.0`. Azure Media Player-Versionen können von [hier](azure-media-player-changelog.md) aus abgefragt werden.

## <a name="use-the-video-element"></a>Verwenden des Videoelemente

Verwenden Sie als Nächstes einfach das `<video>`-Element wie gewohnt, aber mit einem zusätzlichen `data-setup`-Attribut, das alle Optionen enthält. Diese Optionen können beliebige Azure Media Services-Optionen in einem gültigen JSON-Objekt enthalten.

```html
    <video id="vid1" class="azuremediaplayer amp-default-skin" autoplay controls width="640" height="400" poster="poster.jpg" data-setup='{"nativeControlsForTouch": false}'>
        <source src="http://amssamples.streaming.mediaservices.windows.net/91492735-c523-432b-ba01-faba6c2206a2/AzureMediaServicesPromo.ism/manifest" type="application/vnd.ms-sstr+xml" />
        <p class="amp-no-js">
            To view this video please enable JavaScript, and consider upgrading to a web browser that supports HTML5 video
        </p>
    </video>
```

Wenn Sie nicht die automatische Einrichtung verwenden möchten, können Sie das Attribut `data-setup` weglassen und manuell ein Videoelement initialisieren.

```html
    var myPlayer = amp('vid1', { /* Options */
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
            }
          }
    );
    myPlayer.src([{
        src: "http://amssamples.streaming.mediaservices.windows.net/91492735-c523-432b-ba01-faba6c2206a2/AzureMediaServicesPromo.ism/manifest",
        type: "application/vnd.ms-sstr+xml"
    }]);
```

## <a name="next-steps"></a>Nächste Schritte ##

- [Vollständiges Setup von Azure Media Player](./azure-media-player-full-setup.md)