---
title: Vergleich der Voreinstellungen von Video Indexer und Azure Media Services v3
description: In diesem Artikel werden die Funktionen von Video Indexer und die Voreinstellungen von Azure Media Services v3 verglichen.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.subservice: video-indexer
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/24/2020
ms.author: juliako
ms.openlocfilehash: ceb105409ff218cd633eb36d793e8fc16c7d135c
ms.sourcegitcommit: edc7dc50c4f5550d9776a4c42167a872032a4151
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105961539"
---
# <a name="compare-azure-media-services-v3-presets-and-video-indexer"></a>Vergleich der Voreinstellungen von Azure Media Services v3 und Video Indexer 

In diesem Artikel werden die Funktionen von **Video Indexer-APIs** und **Media Services v3-APIs** verglichen. 

Derzeit gibt es eine Überschneidung bei den Features der [APIs von Video Indexer](https://api-portal.videoindexer.ai/) und der [APIs von Media Services v3](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2018-07-01/Encoding.json). In der folgenden Tabelle finden Sie die aktuelle Richtlinie zum Verständnis der Unterschiede und Gemeinsamkeiten. 

## <a name="compare"></a>Vergleichen

|Funktion|Video Indexer-APIs |Voreinstellungen für das Video- und Audioanalysetool<br/>in APIs von Azure Media Services v3|
|---|---|---|
|Medienerkenntnisse|[Erweitert](video-indexer-output-json-v2.md) |[Grundlagen](../latest/analyze-video-audio-files-concept.md)|
|Erfahrungen|Die vollständige Liste der unterstützten Features finden Sie hier: <br/> [Übersicht](video-indexer-overview.md)|Gibt nur Videoerkenntnisse zurück.|
|Abrechnung|[Media Services – Preise](https://azure.microsoft.com/pricing/details/media-services/#analytics)|[Media Services – Preise](https://azure.microsoft.com/pricing/details/media-services/#analytics)|
|Kompatibilität|Aktuelle Complianceupdates finden Sie in [Azure Compliance Offerings.pdf](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942/file/178110/23/Microsoft%20Azure%20Compliance%20Offerings.pdf), indem Sie nach „Video Indexer“ suchen, um zu prüfen, ob er mit einem bestimmten Zertifikat konform ist.|Aktuelle Complianceupdates finden Sie in [Azure Compliance Offerings.pdf](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942/file/178110/23/Microsoft%20Azure%20Compliance%20Offerings.pdf), indem Sie nach „Media Services“ suchen, um zu prüfen, ob der Dienst mit einem bestimmten Zertifikat konform ist.|
|Kostenlose Testversion|East US|Nicht verfügbar|
|Regionale Verfügbarkeit|Siehe [Verfügbarkeit von Cognitive Services nach Region](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services)|Siehe [Verfügbarkeit von Media Services nach Region](https://azure.microsoft.com/global-infrastructure/services/?products=media-services)|

## <a name="next-steps"></a>Nächste Schritte

[What is Video Indexer? (preview)](video-indexer-overview.md) (Was ist Video Indexer? (Vorschauversion))

[Media Services v3 – Übersicht](../latest/media-services-overview.md)
