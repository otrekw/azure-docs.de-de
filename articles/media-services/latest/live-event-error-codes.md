---
title: Azure Media Services-Fehlercodes für Liveereignisse | Microsoft-Dokumentation
description: In diesem Artikel werden Fehlercodes für Liveereignisse aufgelistet.
author: Juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/21/2020
ms.author: juliako
ms.openlocfilehash: bfbef771d33ad4d63ec8eaef83331e497d476071
ms.sourcegitcommit: 99ac4a0150898ce9d3c6905cbd8b3a5537dd097e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/25/2020
ms.locfileid: "77599687"
---
# <a name="media-services-live-event-error-codes"></a>Azure Media Services-Fehlercodes für Liveereignisse

In der folgenden Tabelle sind die Fehlercodes für [Liveereignisse](live-events-outputs-concept.md) aufgelistet:

|Fehler|BESCHREIBUNG|
|---|---| 
|MPE_INGEST_FRAMERATE_EXCEEDED|Dieser Fehler tritt auf, wenn der eingehende Encoder Datenströme mit mehr als 30 fps zur Codierung von Liveereignissen/Kanälen sendet.|
|MPE_INGEST_VIDEO_RESOLUTION_NOT_SUPPORTED|Dieser Fehler tritt auf, wenn der eingehende Encoder Datenströme sendet, die die folgenden Auflösungen überschreiten: 1920 x 1088 für die Codierung von Liveereignissen/Kanälen und 4096 x 2160 für Pass-Through-Liveereignisse/-Kanäle.|

## <a name="see-also"></a>Weitere Informationen

[Fehlercodes für den Streamingendpunkt (Ursprung)](streaming-endpoint-error-codes.md)

## <a name="next-steps"></a>Nächste Schritte

[Tutorial: Livestreaming mit Media Services](stream-live-tutorial-with-api.md)
