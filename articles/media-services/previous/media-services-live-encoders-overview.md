---
title: Konfigurieren von lokalen Encodern beim Erstellen von Datenströmen mit Mehrfachbitraten mithilfe von Azure Media Services | Microsoft-Dokumentation
description: Dieses Thema zählt lokale Liveencoder auf, mit denen Sie Ihre Liveereignisse aufzeichnen und zur weiteren Verarbeitung einen Single-Bitrate-Livedatenstrom an AMS-Kanäle (für die die Livecodierung aktiviert ist) senden können. Das Thema enthält Links zu Tutorials, in denen die Konfiguration der aufgelisteten Encoder erläutert wird.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.assetid: 0ec6f046-0841-4673-9057-883bdbc30d5c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/10/2021
ms.author: inhenkel
ms.openlocfilehash: d83c81e1a8a7d8d689e18cc8ba46fbb137959481
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "103014858"
---
# <a name="how-to-configure-on-premises-encoders-when-using-azure-media-services-to-create-multi-bitrate-streams"></a>Vorgehensweise: Konfigurieren von lokalen Encodern beim Erstellen von Multi-Bitrate-Datenströmen mit Azure Media Services

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)]

Dieses Thema zählt lokale Liveencoder auf, mit denen Sie Ihre Liveereignisse aufzeichnen und zur weiteren Verarbeitung einen Single-Bitrate-Livedatenstrom an AMS-Kanäle (für die die Livecodierung aktiviert ist) senden können. Das Thema enthält auch Links zu Tutorials, in denen die Konfiguration der aufgelisteten Encoder erläutert wird.

> [!NOTE]
> Überprüfen Sie beim Streamen per RTMP in den Firewall- und/oder Proxyeinstellungen, ob die ausgehenden TCP-Ports 1935 und 1936 geöffnet sind.

## <a name="haivision-kb-encoder"></a>Haivision KB-Encoder
Informationen zum Konfigurieren des [Haivision KB-Encoders](https://www.haivision.com/products/kb-series/) zum Senden eines Single-Bitrate-Livedatenstroms an einen AMS-Kanal finden Sie unter [Konfigurieren des Haivision KB-Encoders](media-services-configure-kb-live-encoder.md).

## <a name="telestream-wirecast"></a>Telestream Wirecast
Informationen zum Konfigurieren des [Telestream Wirecast](https://www.telestream.net/wirecast/overview.htm)-Encoders zum Senden eines Single-Bitrate-Livedatenstroms an einen AMS-Kanal finden Sie unter [Konfigurieren von Wirecast](media-services-configure-wirecast-live-encoder.md).

## <a name="elemental-live"></a>Elemental Live
Weitere Informationen finden Sie unter [Elemental Live](https://www.elemental.com/products/aws-elemental-appliances-software/#elemental-live).

## <a name="media-services-learning-paths"></a>Media Services-Lernpfade
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Nächste Schritte

[Livestreaming mit Azure Media Services zum Erstellen von Multi-Bitrate-Datenströmen](media-services-manage-live-encoder-enabled-channels.md).

