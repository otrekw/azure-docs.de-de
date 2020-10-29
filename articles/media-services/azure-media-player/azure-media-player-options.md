---
title: Azure Media Player-Optionen
description: Der Einbindungscode für Azure Media Player ist einfach ein HTML5-Videotag, daher können Sie zum Festlegen vieler Optionen die Tag-Standardattribute verwenden.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: reference
ms.date: 04/20/2020
ms.custom: devx-track-js
ms.openlocfilehash: 67f1f2fe3f6ac1061f1edcd22532bffaf0a1e815
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2020
ms.locfileid: "92366869"
---
# <a name="options"></a>Tastatur #

## <a name="setting-options"></a>Festlegen von Optionen ##

Der Einbindungscode für Azure Media Player ist einfach ein HTML5-Videotag, daher können Sie zum Festlegen vieler Optionen die Tag-Standardattribute verwenden.

`<video controls autoplay ...>`

Alternativ können Sie das data-setup-Attribut verwenden, um Optionen im [JSON](http://json.org/example.html)-Format anzugeben. Dies ist auch die Vorgehensweise zum Festlegen von Optionen, die nicht standardmäßig im Videotag verfügbar sind.

`<video data-setup='{ "controls": true, "autoplay": false }'...>`

Schließlich können Sie, wenn Sie nicht das data-setup-Attribut zum Auslösen der Playereinrichtung verwenden, ein Objekt übergeben, das die Playeroptionen als zweites Argument in der JavaScript-Setupfunktion enthält.

`amp("vid1", { "controls": true, "autoplay": false });`

> [!NOTE]
> Optionen im Konstruktor werden nur bei der erstmaligen Initialisierung vor dem Festlegen der Quelle festgelegt.  Wenn Sie die Optionen für das gleiche initialisierte Azure Media Player-Element ändern möchten, müssen Sie die Optionen aktualisieren, bevor Sie die Quelle ändern. Die Optionen können in JavaScript mithilfe von `myPlayer.options({/*updated options*/});` aktualisiert werden. Beachten Sie, dass nur geänderte Optionen betroffen sind, alle anderen zuvor festgelegten Optionen werden beibehalten.

## <a name="individual-options"></a>Einzelne Optionen ##

> [!NOTE]
>Videotagattribute können nur wahr oder falsch sein (boolesch). Sie nehmen ein Attribut (ohne Gleichheitszeichen) einfach auf, um es zu aktivieren, oder schließen es aus, um es zu deaktivieren. Um beispielsweise die Steuerelemente zu aktivieren: FALSCH `<video controls="true" ...>` RICHTIG `<video controls ...>` Das größte Problem, dem Benutzer begegnen, tritt bei dem Versuch auf, diese Werte mithilfe von „false“ als falsch festzulegen (z. B. controls="false"), womit das Gegenteil des beabsichtigten Verhaltens erreicht und der Wert auf „true“ festgelegt wird, da das Attribut enthalten ist.

### <a name="controls"></a>controls ###

Die controls-Option legt fest, ob der Player Steuerelemente aufweist, mit denen der Benutzer interagieren kann. Ohne Steuerelemente besteht die einzige Möglichkeit, die Videowiedergabe zu starten, im Attribut für die automatische Wiedergabe oder der API.

`<video controls ...>` oder `{ "controls": true }`

### <a name="autoplay"></a>autoplay ###

Wenn „autoplay“ wahr ist, beginnt die Wiedergabe des Videos, sobald die Seite geladen ist (ohne Benutzerinteraktion).

> [!NOTE]
> Diese Option wird von mobilen Geräten wie Windows Phone, Apple iOS und Android nicht unterstützt. Mobile Geräte blockieren die Funktionalität für automatische Wiedergabe, um zu verhindern, dass die monatlichen Datentarife des Benutzers (die häufig teuer sind) übermäßig genutzt werden. In diesem Fall ist ein Tippen/Klick des Benutzers erforderlich, um das Video zu starten.

`<video autoplay ...>` oder `{ "autoplay": true }`

### <a name="poster"></a>poster ###
Das Poster-Attribut legt das Bild fest, das vor dem Beginn der Videowiedergabe angezeigt wird. Dies ist oftmals ein Einzelbild aus dem Video oder ein benutzerdefinierter Titelbildschirm. Sobald der Benutzer auf „Wiedergabe“ klickt, wird das Bild ausgeblendet.

`<video poster="myPoster.jpg" ...>` oder `{ "poster": "myPoster.jpg" }`

### <a name="width"></a>width ###

Das width-Attribut legt die Anzeigebreite des Videos fest.

`<video width="640" ...>` oder `{ "width": 640 }`

### <a name="height"></a>height ###

Mit dem height-Attribut wird die Anzeigehöhe des Videos festgelegt.

`<video height="480" ...>` oder `{ "height": 480 }`

### <a name="plugins"></a>plugins ###

Das plugins-JSON-Attribut bestimmt, welche Plug-Ins mit der betreffenden Instanz von AMP geladen werden. Hier können Sie alle Optionen festlegen, über die das betreffende Plug-In verfügt.

   `<video... data-setup='{plugins: { "contentTitle": {"name": "Azure Medi Services Overview"}}}'...>`

Weitere Informationen zur Entwicklung und Verwendung von Plug-Ins finden Sie unter [Schreiben von Plug-Ins](azure-media-player-writing-plugins.md)

### <a name="other-options"></a>Weitere Optionen ###

Weitere Optionen können für das `<video>`-Tag mithilfe des `data-setup`-Parameters festgelegt werden, der ein JSON-Argument akzeptiert.
`<video ... data-setup='{"nativeControlsForTouch": false}'>`

#### <a name="nativecontrolsfortouch"></a>nativeControlsForTouch ####

Dies ist explizit auf „false“ festgelegt. Durch die Einstellung auf „false“ kann das Azure Media Player-Design plattformübergreifend gleich dargestellt werden.  Ferner ist, dem Namen zum Trotz, Touchbedienung nach wie vor möglich.

### <a name="fluid"></a>fluid ###

Durch Festlegen dieser Option auf „true“ nimmt das Videoelement die gesamte Breite des übergeordneten Containers ein. Seine Höhe wird für ein Video mit dem Standardseitenverhältnis von 16:9 angepasst.

`<video ... data-setup='{"fluid": true}'>`

Die Option `fluid` setzt explizite Werte für `width` und `height` außer Kraft. Diese Option steht nur in Azure Media Player-Version `2.0.0` und höher zur Verfügung.

### <a name="playbackspeed"></a>playbackSpeed ###

Die `playbackSpeed`-Option steuert das playbackSpeed-Steuerelement und eine Reihe von Einstellungen der Wiedergabegeschwindigkeit, die dem Benutzer zur Verfügung stehen. `playbackSpeed` erwartet ein Objekt. Um die Steuerung der Wiedergabegeschwindigkeit in der Steuerleiste zu aktivieren, muss die Eigenschaft `enabled` des Objekts auf „true“ festgelegt werden. Ein Beispiel zum Aktivieren der Wiedergabegeschwindigkeit im Markup:

`<video ... data-setup='{"playbackSpeed": {"enabled": true}}'>`


Weitere Eigenschaften der `playbackSpeed`-Einstellung stehen über das [PlaybackSpeedOptions](/javascript/api/azuremediaplayer/playbackspeedoptions)-Objekt zur Verfügung.

Beispiel zum Festlegen von Optionen für die Wiedergabegeschwindigkeit in JavaScript:

```javascript
    var myPlayer = amp('vid1', {
        fluid: true,
        playbackSpeed: {
            enabled: true,
            initialSpeed: 1.0,
            speedLevels: [
                { name: "x4.0", value: 4.0 },
                { name: "x3.0", value: 3.0 },
                { name: "x2.0", value: 2.0 },
                { name: "x1.75", value: 1.75 },
                { name: "x1.5", value: 1.5 },
                { name: "x1.25", value: 1.25 },
                { name: "normal", value: 1.0 },
                { name: "x0.75", value: 0.75 },
                { name: "x0.5", value: 0.5 },
            ]
        }
    });
```

Diese Option steht nur in Azure Media Player-Version 2.0.0 und höher zur Verfügung.

### <a name="staledatatimelimitinsec"></a>staleDataTimeLimitInSec ###

Die Option `staleDataTimeLimitInSec` ist eine Optimierung, mit der Sie konfigurieren können, wie viele Sekunden veraltete Daten in den MediaSource-Puffern aufbewahrt werden sollen. Diese Einstellung ist standardmäßig deaktiviert.

### <a name="cea708captionssettings"></a>cea708CaptionsSettings ###

Wenn diese Einstellung aktiviert und auf „true“ festgelegt wird, können CEA-Liveuntertitel in Livestreams und Livearchiven angezeigt werden. Das label-Attribut ist nicht erforderlich, wenn es nicht enthalten ist, greift der Player auf eine Standardbezeichnung zurück.

```javascript
     cea708CaptionsSettings: {
                enabled: true,
                srclang: 'en',
                label: 'Live CC'
            }
```

Diese Option steht nur in Azure Media Player-Version 2.1.1 und höher zur Verfügung.

## <a name="next-steps"></a>Nächste Schritte ##

- [Schnellstart für Azure Media Player](azure-media-player-quickstart.md)
