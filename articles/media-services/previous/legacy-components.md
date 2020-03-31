---
title: Legacy-Komponenten von Azure Media Services | Microsoft-Dokumentation
description: In diesem Thema werden die Legacy-Komponenten von Azure Media Services behandelt.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2020
ms.author: juliako
ms.openlocfilehash: 94a70a1234d902787f248890f0cb538a4ba9c2f9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "77921078"
---
# <a name="azure-media-services-legacy-components"></a>Legacy-Komponenten von Azure Media Services

Im Laufe der Zeit wurden die Media Service-Komponenten kontinuierlich verfeinert und verbessert. Infolgedessen wurden einige Legacy-Komponenten eingestellt. Die Anweisungen zum Migrieren Ihrer Anwendung von der Legacy-Komponente zu einer aktuellen Komponente finden Sie in den folgenden Artikeln.
 
## <a name="retirement-plans-of-legacy-components-and-migration-guidance"></a>Deaktivierungspläne für Legacy-Komponenten und Hinweise zur Migration

Wir kündigen die Einstellung der Medienprozessoren *Windows Azure Media Encoder* (WAME) und *Azure Media Encoder* (AME) an. Diese Prozessoren werden am 31. März 2020 eingestellt.

* [Migrieren von Windows Azure Media Encoder zu Media Encoder Standard](migrate-windows-azure-media-encoder.md)
* [Migrieren von Azure Media Encoder zu Media Encoder Standard](migrate-azure-media-encoder.md)

Außerdem wird die Deaktivierung der folgenden Media Analytics Medienprozessoren angekündigt: 
 
|Medienprozessorname|Deaktivierungstermin|Zusätzliche Hinweise|
|---|---|
|[Azure Media Indexer 2](media-services-process-content-with-indexer2.md)|1\. Januar 2020|Dieser Medienprozessor wird durch den [Azure Media Services Video Indexer](https://docs.microsoft.com/azure/media-services/video-indexer/) ersetzt. Weitere Informationen finden Sie unter [Migration von Azure Media Indexer 2 zu Azure Media Services Video Indexer](migrate-indexer-v1-v2.md).|
|[Azure Media Indexer](media-services-index-content.md)|1\. März 2023|Dieser Medienprozessor wird durch den [Azure Media Services Video Indexer](https://docs.microsoft.com/azure/media-services/video-indexer/) ersetzt. Weitere Informationen finden Sie unter [Migration von Azure Media Indexer zu Azure Media Services Video Indexer](migrate-indexer-v1-v2.md).|
|[Bewegungserkennung](media-services-motion-detection.md)|1\. Juni 2020|Derzeit keine Austauschpläne.|
|[Videozusammenfassung](media-services-video-summarization.md)|1\. Juni 2020|Derzeit keine Austauschpläne.|
|[Optische Zeichenerkennung in Video](media-services-video-optical-character-recognition.md)|1\. Juni 2020|Dieser Medienprozessor wird durch den [Azure Media Services Video Indexer](https://docs.microsoft.com/azure/media-services/video-indexer/) ersetzt. Erwägen Sie auch den Einsatz der [Azure Media Services v3-API](https://docs.microsoft.com/azure/media-services/latest/analyzing-video-audio-files-concept). <br/>Weitere Informationen finden Sie unter [Vergleich der Voreinstellungen von Azure Media Services v3 und Video Indexer](https://docs.microsoft.com/azure/media-services/video-indexer/compare-video-indexer-with-media-services-presets).|
|[Face Detector](media-services-face-and-emotion-detection.md)|1\. Juni 2020|Dieser Medienprozessor wird durch den [Azure Media Services Video Indexer](https://docs.microsoft.com/azure/media-services/video-indexer/) ersetzt. Erwägen Sie auch den Einsatz der [Azure Media Services v3-API](https://docs.microsoft.com/azure/media-services/latest/analyzing-video-audio-files-concept). <br/>Weitere Informationen finden Sie unter [Vergleich der Voreinstellungen von Azure Media Services v3 und Video Indexer](https://docs.microsoft.com/azure/media-services/video-indexer/compare-video-indexer-with-media-services-presets).|
|[Content Moderator](media-services-content-moderation.md)|1\. Juni 2020|Dieser Medienprozessor wird durch den [Azure Media Services Video Indexer](https://docs.microsoft.com/azure/media-services/video-indexer/) ersetzt. Erwägen Sie auch den Einsatz der [Azure Media Services v3-API](https://docs.microsoft.com/azure/media-services/latest/analyzing-video-audio-files-concept). <br/>Weitere Informationen finden Sie unter [Vergleich der Voreinstellungen von Azure Media Services v3 und Video Indexer](https://docs.microsoft.com/azure/media-services/video-indexer/compare-video-indexer-with-media-services-presets).|

## <a name="next-steps"></a>Nächste Schritte

[Hinweise zur Migration von Media Services v2 zu v3](../latest/migrate-from-v2-to-v3.md)
