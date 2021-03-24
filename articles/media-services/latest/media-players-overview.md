---
title: Übersicht über Mediaplayer für Media Services
description: Welche Mediaplayer kann ich mit Azure Media Services verwenden? Bisher Azure Media Player, Shaka und Video.js
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: overview
ms.date: 3/08/2021
ms.openlocfilehash: 1a1d415b374818d9a51c87e78e7ac422fa374bc5
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102510404"
---
# <a name="media-players-for-media-services"></a>Mediaplayer für Media Services

Sie können mehrere Mediaplayer mit Media Services verwenden.

## <a name="azure-media-player"></a>Azure Media Player

Azure Media Player ist ein Videoplayer für eine Vielzahl von Browsern und Geräten. Der Azure Media Player nutzt Industriestandards wie HTML5, MSE (Media Source Extensions) und EME (Encrypted Media Extensions) für ein funktionsreiches adaptives Streaming. Wenn diese Standards auf einem Gerät oder in einem Browser nicht verfügbar sind, verwendet Azure Media Player Flash und Silverlight als Fallback-Technologien. Unabhängig von der verwendeten Wiedergabetechnologie verfügen Entwickler über eine einheitliche JavaScript-Schnittstelle für den Zugriff auf APIs. Von Azure Media Services bereitgestellte Inhalte können über eine breite Palette von Geräten und Browsern ohne jeglichen Zusatzaufwand wiedergegeben werden.

Informationen finden Sie in der [Azure Media Player-Dokumentation](https://docs.microsoft.com/azure/media-services/azure-media-player/azure-media-player-overview).

## <a name="shaka"></a>Shaka

Shaka Player ist eine Open-Source-JavaScript-Bibliothek für adaptive Medien. Der Player gibt adaptive Medienformate wie DASH und HLS in einem Browser wieder, und zwar ohne Plug-Ins oder Flash. Stattdessen verwendet Shaka Player die offenen Webstandards MSE (Media Source Extensions) und EME (Encrypted Media Extensions).

Informationen finden Sie unter [Verwenden von Shaka Player mit Azure Media Services](how-to-shaka-player.md).

## <a name="videojs"></a>Video.js

Video.js ist ein Videoplayer, der adaptive Medienformate (etwa DASH und HLS) in einem Browser wiedergibt. Video.js verwendet die offenen Webstandards MSE (Media Source Extensions) und EME (Encrypted Media Extensions). Er unterstützt die Videowiedergabe auf Desktops und mobilen Geräten. Die offizielle Dokumentation finden Sie unter https://docs.videojs.com/.

Informationen finden Sie unter [Verwenden des Video.js-Players mit Azure Media Services](how-to-video-js-player.md).


## <a name="next-steps"></a>Nächste Schritte ##

- [Schnellstart für Azure Media Player](../azure-media-player/azure-media-player-quickstart.md)
