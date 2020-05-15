---
title: Schreiben von Plug-Ins für Azure Media Player
description: Erfahren Sie, wie Sie ein Plug-In für Azure Media Player mit JavaScript schreiben.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: how-to
ms.date: 04/20/2020
ms.openlocfilehash: 7902dfdf81d8e44921a5218d56effc90f433f02d
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2020
ms.locfileid: "82857404"
---
# <a name="writing-plugins-for-azure-media-player"></a>Schreiben von Plug-Ins für Azure Media Player #

Ein Plug-In besteht aus JavaScript-Code, der zur Erweiterung oder Verbesserung des Players geschrieben wurde. Sie können Plug-Ins schreiben, um Aussehen und Funktionalität von Azure Media Player zu ändern oder ihn sogar mit Schnittstellen zu anderen Diensten auszustatten. Dies können Sie in zwei einfachen Schritten erreichen:

## <a name="step-1"></a>Schritt 1 ##

Schreiben Sie Ihr JavaScript in einer Funktion, wie hier:

```javascript

    (function () {
        amp.plugin('yourPluginName', function (options) {
        var myPlayer = this;
           myPlayer.addEventListener(amp.eventName.ready, function () {
        console.log("player is ready!");
            });
        });
    }).call(this);
```

Sie können Ihren Code direkt auf Ihrer HTML-Seite innerhalb von `<script>`-Tags oder in einer externen JavaScript-Datei schreiben. Wenn Sie die letztere Möglichkeit wählen, achten Sie darauf, die JavaScript-Datei *nach* dem AMP-Script in den `<head>` Ihrer HTML-Seite aufzunehmen.

Beispiel:

```javascript
    <!--*****START OF Azure Media Player Scripts*****-->
    <script src="//amp.azure.net/libs/amp/latest/azuremediaplayer.min.js"></script>
    <link href="//amp.azure.net/libs/amp/latest/skins/amp-default/azuremediaplayer.min.css" rel="stylesheet">
    <!--*****END OF Azure Media Player Scripts*****-->
    <!--Add Plugins-->
    <script src="yourPluginName.js"></script>
```

## <a name="step-2"></a>Schritt 2 ##
Initialisieren Sie das Plug-In mit JavaScript auf eine von zwei Arten:

Methode 1:

```javascript
    var myOptions = {
        autoplay: true,
        controls: true,
        width: "640",
        height: "400",
        poster: "",
        plugins: {
            yourPluginName: {
                [your plugin options]: [example options]
           }
        }
    };     
    var myPlayer = amp([videotag id], myOptions);
```

Methode 2:

```javascript
    var video = amp([videotag id]);
    video.yourPluginName({[your plugins option]: [example option]});
```

Plug-In-Optionen sind nicht obligatorisch, ihre Einbeziehung ermöglicht es den Entwicklern aber, Ihr Plug-In zum Konfigurieren seines Verhaltens zu verwenden, ohne den Quellcode zu ändern.

Inspirationen und weitere Beispiele zum Erstellen von Plug-Ins finden Sie in unserem [Katalog](azure-media-player-plugin-gallery.md).

>[!NOTE]
> Der Code von Plug-Ins nimmt innerhalb der Lebensdauer der Playerdarstellung des Viewers dynamisch Änderungen am DOM vor, greift jedoch nicht dauerhaft in den Quellcode des Players ein. Hier kommt Ihnen das Wissen um die Entwicklertools Ihres Browsers zugute. Wenn Sie beispielsweise die Darstellung eines Elements im Player ändern möchten, können Sie sein HTML-Element anhand des Klassennamens suchen und dort dann Attribute ändern oder hinzufügen. Hier ist eine hervorragende Ressource zum [Ändern von HTML-Attributen.](http://www.w3schools.com/js/js_htmldom_html.asp)

### <a name="integrated-plugins"></a>Integrierte Plug-Ins ###

 Aktuell enthält AMP zwei integrierte Plug-Ins: [Time-Tip](http://sr-test.azurewebsites.net/Tests/Plugin%20Gallery/plugins/timetip/example.html) und [Hotkeys](http://sr-test.azurewebsites.net/Tests/Plugin%20Gallery/plugins/hotkeys/example.html). Diese Plug-Ins wurden ursprünglich als modulare Plug-Ins für den Player entwickelt, sind jetzt jedoch im Quellcode des Players enthalten.

### <a name="plugin-gallery"></a>Plug-In-Katalog ###

Der [Plug-In-Katalog](https://aka.ms/ampplugins) weist mehrere Plug-Ins auf, die bereits von der Community für Features wie Zeitachsenmarkierungen, Zoom, Analysen und mehr beigetragen wurden. Die Seite bietet Zugriff auf die Plug-Ins und Anweisungen zu ihrer Einrichtung sowie eine Demo, die die Plug-Ins in der Praxis zeigt. Wenn Sie ein tolles Plug-In geschaffen haben und denken, es sollte in unseren Katalog aufgenommen werden, können Sie es gerne einreichen, damit wir es prüfen können.

## <a name="next-steps"></a>Nächste Schritte ##

- [Schnellstart für Azure Media Player](azure-media-player-quickstart.md)
