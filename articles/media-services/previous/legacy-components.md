---
title: Legacy-Komponenten von Azure Media Services | Microsoft-Dokumentation
description: In diesem Thema werden die Legacy-Komponenten von Azure Media Services behandelt.
services: media-services
documentationcenter: ''
author: myoungerman
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2020
ms.author: v-myoung
ms.openlocfilehash: d5a10ccc887124b2bc5595d66baad833a32275cc
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90600890"
---
# <a name="azure-media-services-legacy-components"></a>Legacy-Komponenten von Azure Media Services

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)]

Im Laufe der Zeit verbessern wir Media Service-Komponenten und nehmen Legacy-Komponenten außer Betrieb. Dieser Artikel unterstützt Sie bei der Migration Ihrer Anwendung von einer Legacy- zu einer aktuellen Komponente.
 
## <a name="retirement-plans-of-legacy-components-and-migration-guidance"></a>Deaktivierungspläne für Legacy-Komponenten und Hinweise zur Migration

Die Medienprozessoren *Windows Azure Media Encoder* (WAME) und *Azure Media Encoder* (AME) sind veraltet.

* [Migrieren von Windows Azure Media Encoder zu Media Encoder Standard](migrate-windows-azure-media-encoder.md)
* [Migrieren von Azure Media Encoder zu Media Encoder Standard](migrate-azure-media-encoder.md)

Die folgenden Media Analytics-Medienprozessoren sind entweder veraltet oder gelten bald als veraltet:  
 
|Medienprozessorname|Deaktivierungstermin|Zusätzliche Hinweise|
|---|---|
|Azure Media Indexer 2|1\. Januar 2020|Dieser Medienprozessor wird durch den [Azure Media Services Video Indexer](../video-indexer/index.yml) ersetzt. Weitere Informationen finden Sie unter [Migration von Azure Media Indexer 2 zu Azure Media Services Video Indexer](migrate-indexer-v1-v2.md).|
|[Azure Media Indexer](media-services-index-content.md)|1\. März 2023|Dieser Medienprozessor wird vom [Azure Media Services Video Indexer](../video-indexer/index.yml) ersetzt. Weitere Informationen finden Sie unter [Migrieren von Azure Media Indexer zu Azure Media Services Video Indexer](migrate-indexer-v1-v2.md).|
|Bewegungserkennung|1\. Juni 2020|Derzeit keine Austauschpläne.|
|Videozusammenfassung](media-services-video-summarization.md|1\. Juni 2020|Derzeit keine Austauschpläne.|
|Optische Zeichenerkennung in Video|1\. Juni 2020|Dieser Medienprozessor wird durch den [Azure Media Services Video Indexer](../video-indexer/index.yml) ersetzt. Erwägen Sie auch den Einsatz der [Azure Media Services v3-API](../latest/analyzing-video-audio-files-concept.md). <br/>Weitere Informationen finden Sie unter [Vergleichen der Voreinstellungen von Azure Media Services v3 und Video Indexer](../video-indexer/compare-video-indexer-with-media-services-presets.md).|
|Face Detector|1\. Juni 2020|Dieser Medienprozessor wird durch den [Azure Media Services Video Indexer](../video-indexer/index.yml) ersetzt. Erwägen Sie auch den Einsatz der [Azure Media Services v3-API](../latest/analyzing-video-audio-files-concept.md). <br/>Weitere Informationen finden Sie unter [Vergleichen der Voreinstellungen von Azure Media Services v3 und Video Indexer](../video-indexer/compare-video-indexer-with-media-services-presets.md).|
|Content Moderator|1\. Juni 2020|Dieser Medienprozessor wird durch den [Azure Media Services Video Indexer](../video-indexer/index.yml) ersetzt. Erwägen Sie auch den Einsatz der [Azure Media Services v3-API](../latest/analyzing-video-audio-files-concept.md). <br/>Weitere Informationen finden Sie unter [Vergleichen der Voreinstellungen von Azure Media Services v3 und Video Indexer](../video-indexer/compare-video-indexer-with-media-services-presets.md).|

## <a name="next-steps"></a>Nächste Schritte

[Hinweise zur Migration von Media Services v2 zu v3](../latest/migrate-from-v2-to-v3.md)
