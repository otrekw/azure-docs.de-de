---
title: Übersicht über Mediaplayer für Media Services
description: Welche Mediaplayer kann ich mit Azure Media Services verwenden? Bisher Azure Media Player, Shaka und Video.js
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: overview
ms.date: 3/08/2021
ms.openlocfilehash: 43d0a8ee82a84a57be7c8ded9e7f5afc172bd9d6
ms.sourcegitcommit: 02bc06155692213ef031f049f5dcf4c418e9f509
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2021
ms.locfileid: "106281261"
---
# <a name="media-players-for-media-services"></a>Mediaplayer für Media Services

Sie können mehrere Mediaplayer mit Media Services verwenden.

## <a name="azure-media-player"></a>Azure Media Player

Azure Media Player ist ein Videoplayer für eine Vielzahl von Browsern und Geräten. Der Azure Media Player nutzt Industriestandards wie HTML5, MSE (Media Source Extensions) und EME (Encrypted Media Extensions) für ein funktionsreiches adaptives Streaming. Wenn diese Standards auf einem Gerät oder in einem Browser nicht verfügbar sind, verwendet Azure Media Player Flash und Silverlight als Fallback-Technologien. Unabhängig von der verwendeten Wiedergabetechnologie verfügen Entwickler über eine einheitliche JavaScript-Schnittstelle für den Zugriff auf APIs. Von Azure Media Services bereitgestellte Inhalte können über eine breite Palette von Geräten und Browsern ohne jeglichen Zusatzaufwand wiedergegeben werden.

Informationen finden Sie in der [Azure Media Player-Dokumentation](../azure-media-player/azure-media-player-overview.md).

## <a name="shaka"></a>Shaka

Shaka Player ist eine Open-Source-JavaScript-Bibliothek für adaptive Medien. Der Player gibt adaptive Medienformate wie DASH und HLS in einem Browser wieder, und zwar ohne Plug-Ins oder Flash. Stattdessen verwendet Shaka Player die offenen Webstandards MSE (Media Source Extensions) und EME (Encrypted Media Extensions).

Informationen finden Sie unter [Verwenden von Shaka Player mit Azure Media Services](player-shaka-player-how-to.md).

## <a name="videojs"></a>Video.js

Video.js ist ein Videoplayer, der adaptive Medienformate (etwa DASH und HLS) in einem Browser wiedergibt. Video.js verwendet die offenen Webstandards MSE (Media Source Extensions) und EME (Encrypted Media Extensions). Er unterstützt die Videowiedergabe auf Desktops und mobilen Geräten. Die offizielle Dokumentation finden Sie unter https://docs.videojs.com/.

Informationen finden Sie unter [Verwenden des Video.js-Players mit Azure Media Services](player-how-to-video-js-player.md).


## <a name="next-steps"></a>Nächste Schritte ##

- [Schnellstart für Azure Media Player](../azure-media-player/azure-media-player-quickstart.md)