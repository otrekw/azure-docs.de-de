---
title: Featureliste für Azure Media Player
description: Eine Featurereferenz für Azure Media Player.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: reference
ms.date: 04/05/2021
ms.openlocfilehash: 4b0666b439c284fd402b3f6e04bbaed14d6aa358
ms.sourcegitcommit: 56b0c7923d67f96da21653b4bb37d943c36a81d6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2021
ms.locfileid: "106448762"
---
# <a name="feature-list"></a>Funktionsliste #
Hier finden Sie eine Liste der getesteten und der nicht unterstützten Features:

| Funktion | GETESTET | TEILWEISE GETESTET | NICHT GETESTET | NICHT UNTERSTÜTZT | HINWEISE |
| ------- | ------ | ---------------- | -------- | ----------- | ----- |
| **Wiedergabe**                                |        |                  |          |             |                                                                                                                      |
| Grundlegende On-Demand-Wiedergabe                | X      |                  |          |             | Unterstützt nur Streams aus Azure Media Services.                                                                      |
| Grundlegende Livewiedergabe                     | X      |                  |          |             | Unterstützt nur Streams aus Azure Media Services.                                                                      |
| AES                                     | X      |                  |          |             | Unterstützt den Schlüsselbereitstellungsdienst von Azure Media Services.                                                                   |
| Multi-DRM                               |        | X                |          |             |                                                                                                                      |
| PlayReady                               | X      |                  |          |             | Unterstützt den Schlüsselbereitstellungsdienst von Azure Media Services.                                                                   |
| Widevine                                |        | X                |          |             | Unterstützt im Manifest angegebene Widevine PSSH-Felder.                                                                    |
| FairPlay                                |        | X                |          |             | Unterstützt den Schlüsselbereitstellungsdienst von Azure Media Services.                                                                   |
| **Technologien**                                   |        |                  |          |             |                                                                                                                      |
| MSE/EME (AzureHtml5JS)                  | X      |                  |          |             |                                                                                                                      |
| Flash-Fallback (FlashSS)                | X      |                  |          |             | In dieser Technologie sind nicht alle Features verfügbar.                                                                         |
| Silverlight Fallback SilverlightSS      | X      |                  |          |             | In dieser Technologie sind nicht alle Features verfügbar.                                                                         |
| Natives HLS-Pass-Through (Html5)         |        | X                |          |             | Aufgrund von Plattformbeschränkungen sind in dieser Technologie nicht alle Features verfügbar.                                            |
| **Funktionen**                                |        |                  |          |             |                                                                                                                      |
| API-Unterstützung                             | X      |                  |          |             | Informationen finden Sie in der Liste der bekannten Probleme.                                                                                                |
| Standardbenutzeroberfläche                                | X      |                  |          |                                                                                                                                    |
| Initialisierung über JavaScript       | X      |                  |          |             |                                                                                                                      |
| Initialisierung über Videotag        |        | X                |          |             |                                                                                                                      |
| Segmentadressierung – zeitbasiert         | X      |                  |          |             |                                                                                                                      |
| Segmentadressierung – indexbasiert        |        |                  |          | X           |                                                                                                                      |
| Segmentadressierung – Byte-basiert         |        |                  |          | X           |                                                                                                                      |
| Azure Media Services-URL-Rewriter       |        | X                |          |             |                                                                                                                      |
| Barrierefreiheit – Beschriftungen und Untertitel  | X      |                 |          |             |  WebVTT (bedarfsgesteuert), CEA 708 (bedarfsgesteuert und live) und IMSC1 (bedarfsgesteuert und live)                                                       |
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
| Unterstützung von Werbeeinblendungen                             |        | X                |          |             | AMP unterstützt das Einfügen von linearen Werbeeinblendungen von VAST-konformen Werbeservern vor, während und nach der Wiedergabe für VOD in der AzureHtml5JS-Technologie. |
| Analytics                               |        | X                |          |             | AMP bietet die Möglichkeit, auf Analyse- und Diagnoseereignisse zu lauschen, um diese an ein Analytics-Back-End Ihrer Wahl zu senden.  Aufgrund von Plattformbeschränkungen sind nicht alle Ereignisse und Eigenschaften für alle Technologien verfügbar.                                                                            |
| Benutzerdefinierte Skins                            |        |                  | X        |             | Legen Sie die Einstellungssteuerelemente in AMP auf „false“ fest, und verwenden Sie eigene HTML- und CSS-Einstellungen, um dieses Szenario zu erzielen.           |
| Scrubbing der Suchleiste                      |        |                  |          | X           |                                                                                                                      |
| Trickwiedergabe                              |        |                  |          | X           |                                                                                                                      |
| Nur Audio                              | X      |                  |          |           | In AzureHtml5JS unterstützt. Progressive MP3-Wiedergabe kann mit der HTML5-Technologie funktionieren, wenn die Plattform dies unterstützt.                                                                                                        |
| Nur Video                              | X      |                  |          |           | In AzureHtml5JS unterstützt.                                                                                                        |
| Präsentation mit mehreren Zeiträumen               |        |                  |          | X                                                                                                                                  |
| Mehrere Kamerawinkel                  |        |                  |          | X           |                                                                                                                      |
| Wiedergabegeschwindigkeit                          |        | X                |          |             | Die Wiedergabegeschwindigkeit wird in den meisten Szenarien unterstützt. Ausgenommen sind mobile Versionen aufgrund eines partiellen Fehlers in Chrome.                 |

## <a name="next-steps"></a>Nächste Schritte ##
- [Schnellstart für Azure Media Player](azure-media-player-quickstart.md)