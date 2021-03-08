---
title: LiveEvent-Einstellungen für niedrige Latenz in Azure Media Services
description: Dieses Thema bietet eine Übersicht über die LiveEvent-Einstellungen für niedrige Latenz und zeigt, wie Sie eine niedrige Latenz festlegen.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: conceptual
ms.date: 08/31/2020
ms.author: inhenkel
ms.custom: devx-track-csharp
ms.openlocfilehash: f9f4476e932df9a5f4c093968b2e7c4840e7ff39
ms.sourcegitcommit: dac05f662ac353c1c7c5294399fca2a99b4f89c8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102122900"
---
# <a name="live-event-low-latency-settings"></a>LiveEvent-Einstellungen für niedrige Latenz

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Dieser Artikel zeigt, wie Sie eine niedrige Latenz für ein [Liveereignis](/rest/api/media/liveevents) festlegen. Außerdem werden typische Ergebnisse beschrieben, die bei der Verwendung der Einstellungen für geringe Latenz und verschiedenen Playern angezeigt werden. Die Ergebnisse variieren basierend auf dem CDN und der Netzwerklatenz.

Um die neue Funktion **LowLatency** zu nutzen, legen Sie **StreamOptionsFlag** für **LiveEvent** auf **LowLatency** fest. Legen Sie beim Erstellen von [LiveOutput](/rest/api/media/liveoutputs) für die HLS-Wiedergabe [LiveOutput.Hls.fragmentsPerTsSegment](/rest/api/media/liveoutputs/create#hls) auf „1“ fest. Sobald der Stream bereit ist, können Sie im [Azure Media Player](https://ampdemo.azureedge.net/) (AMP-Demoseite) die Option für die Wiedergabe auf „Low Latency Heuristics Profile“ (Heuristische Profile mit geringer Latenz) festlegen.

> [!NOTE]
> Derzeit dient das LowLatency HeuristicProfile in Azure Media Player der Wiedergabe von Streams im MPEG-DASH-Protokoll mit dem CSF- oder CMAF-Format (z.B. `format=mdp-time-csf` oder `format=mdp-time-cmaf`). 

Das folgende .NET-Beispiel zeigt, wie Sie **LowLatency** für **LiveEvent** festlegen:

[!code-csharp[Main](../../../media-services-v3-dotnet/Live/LiveEventWithDVR/Program.cs#NewLiveEvent)]

Ein vollständiges Beispiel finden Sie unter [Liveereignis mit DVR](https://github.com/Azure-Samples/media-services-v3-dotnet/blob/main/Live/LiveEventWithDVR/Program.cs).

## <a name="live-events-latency"></a>Latenz von Liveereignissen

Die folgenden Tabellen zeigen übliche Latenzergebnisse (wenn das Flag „LowLatency“ aktiviert ist) in Media Services, gemessen ab dem Zeitpunkt, zu dem der Beitragsfeed den Dienst erreicht, bis zum Zeitpunkt, zu dem ein Benutzer die Wiedergabe im Player betrachtet. Um die geringe Latenz optimal zu nutzen, sollten Sie Ihre Encodereinstellungen auf 1 Sekunde GOP-Länge („Group Of Pictures“) abstimmen. Bei Verwendung einer höheren GOP-Länge minimieren Sie den Bandbreitenverbrauch und verringern die Bitrate bei gleicher Framerate. Dies ist besonders vorteilhaft bei Videos mit wenig Bewegung.

### <a name="pass-through"></a>Pass-Through 

||Niedrige Latenz aktiviert (2 Sek. GOP)|Niedrige Latenz aktiviert (1 Sek. GOP)|
|---|---|---|
|**DASH in AMP**|10 Sek.|8 Sek.|
|**HLS in nativem iOS-Player**|14 Sek.|10 Sek.|

### <a name="live-encoding"></a>Live Encoding

||Niedrige Latenz aktiviert (2 Sek. GOP)|Niedrige Latenz aktiviert (1 Sek. GOP)|
|---|---|---|
|**DASH in AMP**|14 Sek.|10 Sek.|
|**HLS in nativem iOS-Player**|18 Sek.|13 Sek.|

> [!NOTE]
> Die End-to-End-Latenz hängt vom lokalen Netzwerk ab und kann von einer CDN-Cachingebene beeinträchtigt werden. Testen Sie Ihre genauen Konfigurationen.

## <a name="next-steps"></a>Nächste Schritte

- [Übersicht über das Livestreaming](live-streaming-overview.md)
- [Tutorial zu Livestreaming](stream-live-tutorial-with-api.md)
