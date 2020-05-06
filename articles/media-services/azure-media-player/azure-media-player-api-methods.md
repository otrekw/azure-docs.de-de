---
title: Azure Media Player API-Methoden
description: Die Azure Media Player-API ermöglicht Ihnen die Interaktion mit dem Video mithilfe von JavaScript, unabhängig davon, ob der Browser das Video über HTML5-Video, Flash, Silverlight oder beliebige andere unterstützte Wiedergabetechnologien wiedergibt.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: reference
ms.date: 04/20/2020
ms.openlocfilehash: d1ad0cbe30ce20e61a31f0534a47fe031f6f257b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "81725907"
---
# <a name="api"></a>API #

Die Azure Media Player-API ermöglicht Ihnen die Interaktion mit dem Video mithilfe von JavaScript, unabhängig davon, ob der Browser das Video über HTML5-Video, Flash, Silverlight oder beliebige andere unterstützte Wiedergabetechnologien wiedergibt.

## <a name="referencing-the-player"></a>Verweisen auf den Player ##

Zum Verwenden der API-Funktionen benötigen Sie Zugriff auf das Playerobjekt. Das ist glücklicherweise einfach. Sie müssen sich lediglich vergewissern, dass Ihr Videotag über eine ID verfügt. Das Beispiel für den Einbindungscode verwendet die ID `vid1`. Wenn Sie mehrere Videos auf einer Seite platzieren, achten Sie darauf, dass jedes Videotag eine eindeutige ID aufweist.

`var myPlayer = amp('vid1');`

> [!NOTE]
> Wenn der Player noch nicht mithilfe des data-setup-Attributs oder einer anderen Methode initialisiert wurde, erfolgt hierdurch auch zugleich die Initialisierung des Players.

## <a name="wait-until-the-player-is-ready"></a>Warten Sie, bis der Player bereit ist. ##

Die Zeitspanne, die Azure Media Player zum Einrichten von Video und API benötigt, hängt von der verwendeten Wiedergabetechnologie ab. HTML5 lässt sich oftmals viel schneller laden als Flash oder Silverlight. Aus diesem Grund sollte die Funktion ‚ready‘ des Players genutzt werden, um jeglichen Code auszulösen, für den die API des Players erforderlich ist.

```javacript
    amp("vid_1").ready(function(){
      var myPlayer = this;

      // EXAMPLE: Start playing the video.
      myPlayer.play();
    });
```

oder

```javacript
    var myPlayer = amp("vid_1", myOptions, function(){
        //this is the ready function and will only execute after the player is loaded
    });
```

## <a name="api-methods"></a>API-Methoden ##

Jetzt, mit dem Zugriff auf einen Player in Bereitschaft, können Sie das Video steuern, Werte abrufen oder auf Video-Ereignisse reagieren. Die Funktionsnamen der Azure Media Player-API orientieren sich an der [HTML5-Media-API](http://www.whatwg.org/specs/web-apps/current-work/multipage/the-video-element.html). Der Hauptunterschied besteht in der Verwendung von Getter/Setter-Funktionen für Videoeigenschaften.

```javacript
    // setting a property on a bare HTML5 video element
    myVideoElement.currentTime = 120;

    // setting a property with Azure Media Player
    myPlayer.currentTime(120);
```

## <a name="registering-for-events"></a>Registrieren von Ereignissen ##
Ereignisse sollten direkt nach der erstmaligen Initialisierung des Players registriert werden, um sicherzustellen, dass der Anwendung alle Ereignisse ordnungsgemäß gemeldet werden. Dies sollte außerhalb des Ready-Ereignisses erfolgen.

```javacript
    var myPlayer = amp("vid_1", myOptions, function(){
        //this is the ready function and will only execute after the player is loaded
    });
    myPlayer.addEventListener(amp.eventName.error, _ampEventHandler);
    //add other event listeners
```

## <a name="next-steps"></a>Nächste Schritte ##

<!---Some context for the following links goes here--->
- [Schnellstart für Azure Media Player](azure-media-player-quickstart.md)