---
title: Migrieren von Azure Media Encoder zu Media Encoder Standard | Microsoft-Dokumentation
description: In diesem Thema wird erläutert, wie Sie von Azure Media Encoder zum Media Encoder Standard-Medienprozessor migrieren.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/10/2021
ms.author: inhenkel
ms.custom: devx-track-csharp
ms.openlocfilehash: 4f528cea9b475c0158524ad9b46623a78df5761d
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "103016201"
---
# <a name="migrate-from-azure-media-encoder-to-media-encoder-standard"></a>Migrieren von Azure Media Encoder zu Media Encoder Standard

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)]

In diesem Artikel werden die Schritte für die Migration vom älteren AME-Medienprozessor (Azure Media Encoder), der eingestellt wird, zum Media Encoder Standard-Medienprozessor erläutert. Die Daten zur Einstellung finden Sie unter dem Thema [Legacykomponenten](legacy-components.md).

Beim Codieren von Dateien mit AME haben Kunden in der Regel eine benannte Voreinstellungszeichenfolge wie `H264 Adaptive Bitrate MP4 Set 1080p` verwendet. Für die Migration muss Ihr Code aktualisiert werden, um den **Media Encoder Standard**-Medienprozessor anstelle von AME und eine der entsprechenden [Systemvoreinstellungen](media-services-mes-presets-overview.md) wie `H264 Multiple Bitrate 1080p` zu verwenden. 

## <a name="migrating-to-media-encoder-standard"></a>Migrieren zu Media Encoder Standard

Hier folgt ein typisches C#-Codebeispiel, das den Legacy-Medienprozessor verwendet. 

```csharp
// Declare a new job. 
IJob job = _context.Jobs.Create("AME Job"); 
// Get a media processor reference, and pass to it the name of the  
// processor to use for the specific task. 
IMediaProcessor processor = GetLatestMediaProcessorByName("Azure Media Encoder"); 

// Create a task with the encoding details, using a string preset. 
// In this case " H264 Adaptive Bitrate MP4 Set 1080p" preset is used. 
ITask task = job.Tasks.AddNew("My encoding task", 
    processor, 
    " H264 Adaptive Bitrate MP4 Set 1080p", 
    TaskOptions.None); 
```

Hier ist die aktualisierte Version, die Media Encoder Standard verwendet.

```csharp
// Declare a new job. 
IJob job = _context.Jobs.Create("Media Encoder Standard Job"); 
// Get a media processor reference, and pass to it the name of the  
// processor to use for the specific task. 
IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard"); 

// Create a task with the encoding details, using a string preset. 
// In this case " H264 Multiple Bitrate 1080p" preset is used. 
ITask task = job.Tasks.AddNew("My encoding task", 
    processor, 
    "H264 Multiple Bitrate 1080p", 
    TaskOptions.None); 
```

### <a name="advanced-scenarios"></a>Erweiterte Szenarien 

Wenn Sie eine eigene Codierungsvoreinstellung für AME mit dessen Schema erstellt haben, gibt es ein [äquivalentes Schema für Media Encoder Standard](media-services-mes-schema.md). Wenn Sie Fragen zur Zuordnung der älteren Einstellungen zum neuen Encoder haben, können Sie uns über mailto:amshelp@microsoft.com erreichen.  
## <a name="known-differences"></a>Bekannte Unterschiede 

Media Encoder Standard ist robuster, zuverlässiger, hat eine höhere Leistung und liefert eine bessere Qualität als der herkömmliche AME-Encoder. Berücksichtigen Sie zudem Folgendes: 

* Media Encoder Standard erzeugt Ausgabedateien mit einer anderen Namenskonvention als AME.
* Media Encoder Standard erzeugt Artefakte wie Dateien, die die [Eingabedateimetadaten](media-services-input-metadata-schema.md) und die [Metadaten der Ausgabedatei(en)](media-services-output-metadata-schema.md) enthalten.

## <a name="next-steps"></a>Nächste Schritte

* [Legacy-Komponenten](legacy-components.md)
* [Seite mit der Preisübersicht](https://azure.microsoft.com/pricing/details/media-services/#encoding)
