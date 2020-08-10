---
title: Featureliste für Azure Media Player
description: Eine Featurereferenz für Azure Media Player.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: reference
ms.date: 04/20/2020
ms.openlocfilehash: 41e090b9e1d4c091bd3972afd296c5751e6b8c58
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87082714"
---
# <a name="feature-list"></a>Funktionsliste #
Hier finden Sie eine Liste der getesteten und der nicht unterstützten Features:

| Funktion | GETESTET | TEILWEISE GETESTET | NICHT GETESTET | NICHT UNTERSTÜTZT | HINWEISE |
| ------- | ------ | ---------------- | -------- | ----------- | ----- |
| Wiedergabe                                |        |                  |          |             |                                                                                                                      |
| Grundlegende On-Demand-Wiedergabe                | X      |                  |          |             | Unterstützt nur Streams aus Azure Media Services.                                                                      |
| Grundlegende Livewiedergabe                     | X      |                  |          |             | Unterstützt nur Streams aus Azure Media Services.                                                                      |
| AES                                     | X      |                  |          |             | Unterstützt den Schlüsselbereitstellungsdienst von Azure Media Services.                                                                   |
| Multi-DRM                               |        | X                |          |             |                                                                                                                      |
| PlayReady                               | X      |                  |          |             | Unterstützt den Schlüsselbereitstellungsdienst von Azure Media Services.                                                                   |
| Widevine                                |        | X                |          |             | Unterstützt im Manifest angegebene Widevine PSSH-Felder.                                                                    |
| FairPlay                                |        | X                |          |             | Unterstützt den Schlüsselbereitstellungsdienst von Azure Media Services.                                                                   |
| Technologien                                   |        |                  |          |             |                                                                                                                      |
| MSE/EME (AzureHtml5JS)                  | X      |                  |          |             |                                                                                                                      |
| Flash-Fallback (FlashSS)                | X      |                  |          |             | In dieser Technologie sind nicht alle Features verfügbar.                                                                         |
| Silverlight Fallback SilverlightSS      | X      |                  |          |             | In dieser Technologie sind nicht alle Features verfügbar.                                                                         |
| Natives HLS-Pass-Through (Html5)         |        | X                |          |             | Aufgrund von Plattformbeschränkungen sind in dieser Technologie nicht alle Features verfügbar.                                            |
| Features                                |        |                  |          |             |                                                                                                                      |
| API-Unterstützung                             | X      |                  |          |             | Informationen finden Sie in der Liste der bekannten Probleme.                                                                                                |
| Standardbenutzeroberfläche                                | X      |                  |          |                                                                                                                                    |
| Initialisierung über JavaScript       | X      |                  |          |             |                                                                                                                      |
| Initialisierung über Videotag        |        | X                |          |             |                                                                                                                      |
| Segmentadressierung – zeitbasiert         | X      |                  |          |             |                                                                                                                      |
| Segmentadressierung – indexbasiert        |        |                  |          | X           |                                                                                                                      |
| Segmentadressierung – Byte-basiert         |        |                  |          | X           |                                                                                                                      |
| Azure Media Services-URL-Rewriter       |        | X                |          |             |                                                                                                                      |
| Barrierefreiheit – Beschriftungen und Untertitel  |        | X                |          |             |  WebVTT für On-Demand-Funktionen unterstützt, Live-CEA 708 teilweise getestet.                                                       |
| Barrierefreiheit – Tastenkürzel                 | X      |                  |          |             |                                                                                                                      |
| Barrierefreiheit – hoher Kontrast           |        | X                |          |             |                                                                                                                      |
| Barrierefreiheit – Fokus über TAB-Taste               |        | X                |          |             |                                                                                                                      |
| Fehlermeldungen                         |        | X                |          |             | Fehlermeldungen sind technologieübergreifend inkonsistent.                                                                         |
| Ereignisauslösung                        | X      |                  |          |             |                                                                                                                      |
| Diagnose                             |        | X                |          |             | Diagnoseinformationen sind nur in der AzureHtml5JS-Technologie und in der SilverlightSS-Technologie teilweise verfügbar. |
| Anpassbare Technologiereihenfolge                 |        | X                |          |             |                                                                                                                      |
| Heuristik – grundlegende Features                      | X      |                  |          |             |                                                                                                                      |
| Heuristik – Anpassung              |        |                  | X        |             | Anpassungen sind nur bei der AzureHtml5JS-Technologie verfügbar.                                                          |
| Unterbrechungen                         | X      |                  |          |             |                                                                                                                      |
| Bitrate auswählen                          | X      |                  |          |             | Diese API ist nur in den Technologien AzureHtml5JS und FlashSS verfügbar.                                                    |
| Multiaudiostream                      |        | X                |          |             | Der programmgesteuerte Wechsel von Audiostreams ist nur in den Technologien AzureHtml5JS und FlashSS verfügbar, über die Benutzeroberflächenauswahl in AzureHtml5JS, FlashSS und nativem Html5 (in Safari).  Auf den meisten Plattformen sind zum Wechseln von Audiostreams dieselben privaten Codecdaten erforderlich (gleicher Codec und Kanal, gleiche Samplingrate usw.). |
| Lokalisierung der Benutzeroberfläche                         |        | X                |          |             |                                                                                                                      |
| Wiedergabe mehrerer Instanzen                 |        |                  |          | X           | Dieses Szenario funktioniert möglicherweise für einige Technologien, ist jedoch derzeit nicht unterstützt und nicht getestet. Die Funktionalität kann möglicherweise mit iFrames erzielt werden. |
| Unterstützung von Werbeeinblendungen                             |        | x                |          |             | AMP unterstützt das Einfügen von linearen Werbeeinblendungen von VAST-konformen Werbeservern vor, während und nach der Wiedergabe für VOD in der AzureHtml5JS-Technologie. |
| Analytics                               |        | X                |          |             | AMP bietet die Möglichkeit, auf Analyse- und Diagnoseereignisse zu lauschen, um diese an ein Analytics-Back-End Ihrer Wahl zu senden.  Aufgrund von Plattformbeschränkungen sind nicht alle Ereignisse und Eigenschaften für alle Technologien verfügbar.                                                                            |
| Benutzerdefinierte Skins                            |        |                  | X        |             | Legen Sie die Einstellungssteuerelemente in AMP auf „false“ fest, und verwenden Sie eigene HTML- und CSS-Einstellungen.           |
| Scrubbing der Suchleiste                      |        |                  |          | X           |                                                                                                                      |
| Trickwiedergabe                              |        |                  |          | X           |                                                                                                                      |
| Nur Audio                              |        |                  |          | X           | Funktioniert möglicherweise in einigen Technologien für adaptives Streaming, wird aber derzeit nicht unterstützt und funktioniert nicht in AzureHtml5JS. Progressive MP3-Wiedergabe kann mit der HTML5-Technologie funktionieren, wenn die Plattform dies unterstützt.                                                                                                        |
| Nur Video                              |        |                  |          | X           | Funktioniert möglicherweise in einigen Technologien für adaptives Streaming, wird aber derzeit nicht unterstützt und funktioniert nicht in AzureHtml5JS.      |
| Präsentation mit mehreren Zeiträumen               |        |                  |          | X                                                                                                                                  |
| Mehrere Kamerawinkel                  |        |                  |          | X           |                                                                                                                      |
| Wiedergabegeschwindigkeit                          |        | X                |          |             | Die Wiedergabegeschwindigkeit wird in den meisten Szenarien unterstützt. Ausgenommen sind mobile Versionen aufgrund eines partiellen Fehlers in Chrome.                 |

## <a name="next-steps"></a>Nächste Schritte ##
- [Schnellstart für Azure Media Player](azure-media-player-quickstart.md)