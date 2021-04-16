---
title: Azure Media Player-Demos
description: Diese Seite enthält eine Liste mit Links zu Demos für Azure Media Player.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: article
ms.date: 04/24/2020
ms.openlocfilehash: 584748b23f526e6f03b543b8298927e3f202f743
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "82138913"
---
# <a name="azure-media-player-demos"></a>Azure Media Player-Demos

Nachfolgend finden Sie eine Liste mit Links zu Demos für Azure Media Player. Sie können sämtliche [Azure Media Player-Beispiele](https://github.com/Azure-Samples/azure-media-player-samples) auf GitHub herunterladen.

## <a name="demo-listing"></a>Demoliste

| Name des Beispiels | Programmgesteuert über JavaScript | Statisch per HTML5-Videoelement | BESCHREIBUNG |
| ------------|----------------------------|-------------------------------------|--------------|
| Basic |
| Festlegen der Quelle | [Dynamisch](https://amp.azure.net/libs/amp/latest/samples/dynamic_setsource.html) | [Statisch](https://amp.azure.net/libs/amp/latest/samples/videotag_setsource.html) |Wiedergabe von ungeschützten Inhalten.|
| Features |
| VoD – Einfügen von Werbeeinblendungen – VAST | [Dynamisch](https://amp.azure.net/libs/amp/latest/samples/dynamic_vast_ads_vod.html) | – | Einfügen von Pre-, Mid- und Post-Roll-VAST-Werbeeinblendungen in ein VoD-Objekt. |
| Wiedergabegeschwindigkeit | [Dynamisch](https://amp.azure.net/libs/amp/latest/samples/dynamic_playback_speed.html)| – | Ermöglicht es Benutzern, die Geschwindigkeit des Videos festzulegen, das sie sich ansehen. |
| AMP-Flush-Skin | [Dynamisch](https://amp.azure.net/libs/amp/latest/samples/dynamic_flush_skin.html) | [Statisch](https://amp.azure.net/libs/amp/latest/samples/videotag_flush_skin.html) | Aktiviert die neue AMP-Skin. **Hinweis:** Die AMP-Flush-Skin wird erst ab AMP-Version 2.1.0 unterstützt |
| Untertitel | [Dynamisch](https://amp.azure.net/libs/amp/latest/samples/dynamic_webvtt.html) | [Statisch](https://amp.azure.net/libs/amp/latest/samples/videotag_webvtt.html) | Wiedergabe mit WebVTT-Untertiteln.
| CEA 708-Liveuntertitel | [Dynamisch](https://amp.azure.net/libs/amp/latest/samples/dynamic_live_captions.html) | – | Wiedergabe mit CEA 708-Liveuntertiteln (eingehend). Die Untertitel werden linksbündig eingeblendet. |
| Streaming mit progressivem Fallback | [Dynamisch](https://amp.azure.net/libs/amp/latest/samples/dynamic_progressiveFallback.html) | [Statisch](https://amp.azure.net/libs/amp/latest/samples/videotag_progressiveFallback.html) | Grundlegende Einrichtung der adaptiven Wiedergabe mit Fallback auf die progressive Wiedergabe, wenn eine Plattform kein Streaming unterstützt. |
| MP4-Video progressiv | [Dynamisch](https://amp.azure.net/libs/amp/latest/samples/dynamic_progressiveVideo.html) | [Statisch](https://amp.azure.net/libs/amp/latest/samples/videotag_progressiveVideo.html) | Progressive MP4-Videowiedergabe. |
| MP3-Audio progressiv | [Dynamisch](https://amp.azure.net/libs/amp/latest/samples/dynamic_progressiveAudio.html) | [Statisch](https://amp.azure.net/libs/amp/latest/samples/videotag_progressiveAudio.html) | Progressive MP3-Audiowiedergabe. |
| DD+ | [Dynamisch](https://amp.azure.net/libs/amp/latest/samples/dynamic_dolbyDigitalPlus.html) | – | Wiedergabe von Inhalten mit DD+-Audiodaten. |
| Tastatur |
| Heuristikprofil | [Dynamisch](https://amp.azure.net/libs/amp/latest/samples/dynamic_heuristicsProfile.html) | [Statisch](https://amp.azure.net/libs/amp/latest/samples/videotag_heuristicsProfile.html) | Ändern des Heuristikprofils |
| Lokalisierung | [Dynamisch](https://amp.azure.net/libs/amp/latest/samples/dynamic_localization.html) | [Statisch](https://amp.azure.net/libs/amp/latest/samples/videotag_localization.html) |
Festlegen der Lokalisierung |
| Audiotitelmenü | [Dynamisch](https://amp.azure.net/libs/amp/latest/samples/dynamic_multiAudio.html) | [Statisch](https://amp.azure.net/libs/amp/latest/samples/videotag_multiAudio.html) |
Optionen für die Anzeige des Audiotitelmenüs in der Standardskin. |
| Tastenkürzel | [Dynamisch](https://amp.azure.net/libs/amp/latest/samples/dynamic_hotKeys.html) | [Statisch](https://amp.azure.net/libs/amp/latest/samples/videotag_hotKeys.html) | Dieses Beispiel zeigt, wie Sie konfigurieren, welche Tastenkürzel im Player aktiviert sind |
| Ereignisse, Protokollierung und Diagnose |
| Registrieren von Ereignissen | [Dynamisch](https://amp.azure.net/libs/amp/latest/samples/dynamic_registerEvents.html) | – | Wiedergabe mit Ereignislistenern. |
| Protokollierung | [Dynamisch](https://amp.azure.net/libs/amp/latest/samples/dynamic_logging.html) | [Statisch](https://amp.azure.net/libs/amp/latest/samples/videotag_logging.html) | Aktivieren der ausführlichen Protokollierung in der Konsole. |
| Diagnose | [Dynamisch](https://amp.azure.net/libs/amp/latest/samples/dynamic_diagnostics.html) | – | Abrufen von Diagnosedaten. Dieses Beispiel funktioniert nur bei bestimmten Technologien. |
| AES |
| AES ohne Token | [Dynamisch](https://amp.azure.net/libs/amp/latest/samples/dynamic_aes_notoken.html) | [Statisch](https://amp.azure.net/libs/amp/latest/samples/videotag_aes_notoken.html) | Wiedergabe von AES-Inhalten ohne Token. |
| AES mit Token | [Dynamisch](https://amp.azure.net/libs/amp/latest/samples/dynamic_aes_token.html) | [Statisch](https://amp.azure.net/libs/amp/latest/samples/videotag_aes_token.html) | Wiedergabe von AES-Inhalten mit Token. |
| AES – HLS-Proxysimulation | [Dynamisch](https://amp.azure.net/libs/amp/latest/samples/dynamic_aes_token_withHLSProxy.html) | [Statisch](https://amp.azure.net/libs/amp/latest/samples/videotag_aes_token_withHLSProxy.html) | Wiedergabe von AES-Inhalten mit Token. Dabei wird ein Proxy für HLS angezeigt, damit bei iOS-Geräten Token verwendet werden können. |
| AES mit Token – Erzwingen von Flash | [Dynamisch](https://amp.azure.net/libs/amp/latest/samples/dynamic_aes_token_forceFlash.html) | [Statisch](https://amp.azure.net/libs/amp/latest/samples/videotag_aes_token_forceFlash.html) | Wiedergabe von AES-Inhalten mit Token und Erzwingung der flashSS-Technologie. |
| DRM |
| Multi-DRM mit PlayReady, Widevine und FairPlay | [Dynamisch](https://amp.azure.net/libs/amp/latest/samples/dynamic_multiDRM_PlayReadyWidevineFairPlay_notoken.html) | [Statisch](https://amp.azure.net/libs/amp/latest/samples/videotag_multiDRM_PlayReadyWidevineFairPlay_notoken.html) | Wiedergabe von DRM-Inhalten ohne Token, mit PlayReady-, Widevine- und FairPlay-Headern. |
| PlayReady ohne Token | [Dynamisch](https://amp.azure.net/libs/amp/latest/samples/dynamic_playready_notoken.html) | [Statisch](https://amp.azure.net/libs/amp/latest/samples/videotag_playready_notoken.html) | Wiedergabe von PlayReady-Inhalten ohne Token. |
| PlayReady ohne Token, Erzwingen von Silverlight | [Dynamisch](https://amp.azure.net/libs/amp/latest/samples/dynamic_playready_notoken_forceSilverlight.html) | [Statisch](https://amp.azure.net/libs/amp/latest/samples/videotag_playready_notoken_forceSilverlight.html) | Wiedergabe von PlayReady-Inhalten ohne Token, Erzwingen der silverlightSS-Technologie. |
| PlayReady mit Token | [Dynamisch](https://amp.azure.net/libs/amp/latest/samples/dynamic_playready_token.html) | [Statisch](https://amp.azure.net/libs/amp/latest/samples/videotag_playready_token.html) | Wiedergabe von PlayReady-Inhalten mit Token. |
| PlayReady mit Token, Erzwingen von Silverlight | [Dynamisch](https://amp.azure.net/libs/amp/latest/samples/dynamic_playready_token_forceSilverlight.html) | [Statisch](https://amp.azure.net/libs/amp/latest/samples/videotag_playready_token_forceSilverlight.html) | Wiedergabe von PlayReady-Inhalten mit Token, Erzwingen der silverlightSS-Technologie. |
| Protokoll und Technologie |
| Ändern der Technologiereihenfolge | [Dynamisch](https://amp.azure.net/libs/amp/latest/samples/dynamic_techOrder.html) | [Statisch](https://amp.azure.net/libs/amp/latest/samples/videotag_techOrder.html) | Ändern der Technologiereihenfolge |
| Erzwingen von MPEG-DASH nur in UrlRewriter | [Dynamisch](https://amp.azure.net/libs/amp/latest/samples/dynamic_forceDash.html) | [Statisch](https://amp.azure.net/libs/amp/latest/samples/videotag_forceDash.html) | Wiedergabe von ungeschützten Inhalten, wobei ausschließlich das DASH-Protokoll verwendet wird. |
| Ausschließen von MPEG-DASH in UrlRewriter | [Dynamisch](https://amp.azure.net/libs/amp/latest/samples/dynamic_forceNoDash.html) | [Statisch](https://amp.azure.net/libs/amp/latest/samples/videotag_forceNoDash.html) | Wiedergabe von ungeschützten Inhalten, wobei ausschließlich die Protokolle Smooth und HLS verwendet werden. |
| Mehrere Übermittlungsrichtlinien | [Dynamisch](https://amp.azure.net/libs/amp/latest/samples/dynamic_multipleDeliveryPolicy.html) | [Statisch](https://amp.azure.net/libs/amp/latest/samples/videotag_multipleDeliveryPolicy.html) | Festlegen der Quelle mit Inhalten, die mehrere Übermittlungsrichtlinien aufweisen, aus Azure Media Services |
| Programmgesteuerte Auswahl |
| Auswahl eines Texttitels | [Dynamisch](https://amp.azure.net/libs/amp/latest/samples/dynamic_selectTextTrack.html) | – | Auswahl eines WebVTT-Titels aus der Titelliste. |
| Auswahl der Bitrate | [Dynamisch](https://amp.azure.net/libs/amp/latest/samples/dynamic_selectBitrate.html) | – | Auswahl einer Bitrate aus der Liste. Dieses Beispiel funktioniert nur bei bestimmten Technologien. |
| Auswahl des Audiodatenstroms | [Dynamisch](https://amp.azure.net/libs/amp/latest/samples/dynamic_selectAudioStream.html) | – | Auswahl eines Audiodatenstroms aus der Liste der verfügbaren Audiodatenströme. Dieses Beispiel funktioniert nur bei bestimmten Technologien. |

## <a name="next-steps"></a>Nächste Schritte

<!---Some context for the following links goes here--->
- [Schnellstart für Azure Media Player](azure-media-player-quickstart.md)