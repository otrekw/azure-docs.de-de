---
title: Vorgehensweise zum Zusammenfügen von zwei oder mehr Videodateien mit .NET | Microsoft-Dokumentation
description: In diesem Artikel wird gezeigt, wie Sie zwei oder mehr Videodateien zusammenfügen.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.topic: how-to
ms.date: 03/24/2021
ms.author: inhenkel
ms.custom: devx-track-csharp
ms.openlocfilehash: 5eacc366961d3101a7eaf8877a34ef2d462ea76b
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105111401"
---
# <a name="how-to-stitch-two-or-more-video-files-with-net"></a>Vorgehensweise zum Zusammenfügen von zwei oder mehr Videodateien mit .NET

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

## <a name="stitch-two-or-more-video-files"></a>Zusammenfügen von zwei oder mehr Videodateien

Das folgende Beispiel veranschaulicht, wie Sie eine Voreinstellung generieren, um zwei oder mehr Videodateien zusammenzufügen. Das häufigste Szenario hierfür ist das Hinzufügen eines Vor- oder Nachspanns zum Hauptvideo.

> [!NOTE]
> Die Videodateien, die gemeinsam bearbeitet werden, sollten über die gleichen Eigenschaften verfügen (Videoauflösung, Bildfrequenz, Anzahl von Audiospuren usw.). Sie sollten darauf achten, Videos mit verschiedenen Frameraten oder einer unterschiedlichen Anzahl von Audiospuren nicht gemeinsam zu verwenden.

## <a name="prerequisites"></a>Voraussetzungen

Klonen Sie die [Media Services .NET-Beispiele](https://github.com/Azure-Samples/media-services-v3-dotnet/), oder laden Sie sie herunter.  Der Code, auf den unten verwiesen wird, befindet sich im Ordner [EncodingWithMESCustomStitchTwoAssets](https://github.com/Azure-Samples/media-services-v3-dotnet/blob/main/VideoEncoding/EncodingWithMESCustomStitchTwoAssets/Program.cs).

### <a name="net-code"></a>.NET code

[!code-csharp[Main](../../../media-services-v3-dotnet/VideoEncoding/EncodingWithMESCustomStitchTwoAssets/Program.cs)]
