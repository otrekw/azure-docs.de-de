---
title: Azure On-Demand Media Encoder – Übersicht | Microsoft-Dokumentation
description: Azure Media Services bietet verschiedene Optionen für die Codierung von Medien in der Cloud. Dieser Artikel bietet eine Übersicht über Azure On-Demand-Media Encoder.
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
ms.date: 06/25/2019
ms.author: juliako
ms.openlocfilehash: d6e64ed7476b3f9fd5427c2f3d26855bc4d5f97d
ms.sourcegitcommit: 77afc94755db65a3ec107640069067172f55da67
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2021
ms.locfileid: "98695762"
---
# <a name="overview-of-azure-on-demand-media-encoders"></a>Übersicht der Azure On-Demand Media Encoder

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)]

> [!NOTE]
> Media Services v2 werden derzeit keine neuen Features oder Funktionen hinzugefügt. <br/>Sehen Sie sich die neuste Version – [Media Services v3](../latest/index.yml) – an. Lesen Sie außerdem die [Hinweise zur Migration von v2 zu v3](../latest/migrate-v-2-v-3-migration-introduction.md).

Azure Media Services bietet verschiedene Optionen für die Codierung von Medien in der Cloud.

Bei Verwendung von Media Services ist es wichtig, den Unterschied zwischen Codecs und Dateiformaten zu kennen.
Bei Codecs handelt es sich um die Software, die die Algorithmen für die Komprimierung/Dekomprimierung implementiert. Dateiformate dagegen sind die Container, die das komprimierte Video enthalten.

Media Services bietet dynamische Paketerstellung zum Übermitteln Ihrer MP4-Dateien mit adaptiver Bitrate oder Smooth Streaming-codierten Inhalte in Streamingformaten, die von Media Services unterstützt werden (MPEG-DASH, HLS, Smooth Streaming), ohne dass Sie diese Streamingformate erneut packen müssen.

Beim Erstellen Ihres Media Services-Kontos wird dem Konto ein **Standard**-Streamingendpunkt mit dem Status **Beendet** hinzugefügt. Um mit dem Streamen der Inhalte zu beginnen und die dynamische Paketerstellung und dynamische Verschlüsselung zu nutzen, muss der Streamingendpunkt, von dem Sie Inhalte streamen möchten, den Status **Wird ausgeführt** aufweisen. Die Abrechnung für Streamingendpunkte erfolgt immer dann, wenn sich der Endpunkt im Status **Wird ausgeführt** befindet.

Media Services unterstützt die folgenden On-Demand-Encoder:

* [Media Encoder Standard](media-services-encode-asset.md#media-encoder-standard)

Dieser Artikel enthält eine kurze Übersicht über On-Demand-Media Encoder und stellt Links zu Artikeln bereit, die detailliertere Informationen bieten.

Standardmäßig kann jedes Media Services-Konto je eine aktive Codierungsaufgabe gleichzeitig aufweisen. Sie können Einheiten für die Codierung reservieren, mit denen Sie mehrere Codierungsaufgaben gleichzeitig ausführen kennen – jeweils eine für jede reservierte Einheit für die Codierung, die Sie erwerben. Weitere Informationen finden Sie unter [Skalieren der Codierung](media-services-scale-media-processing-overview.md).

## <a name="media-encoder-standard"></a>Media Encoder Standard

### <a name="how-to-use"></a>Verwendung
[Gewusst wie: Codieren mit Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md)

### <a name="formats"></a>Formate
[Formate und Codecs](media-services-media-encoder-standard-formats.md)

### <a name="presets"></a>Voreinstellungen
Media Encoder Standard wird mithilfe von Encoder-Voreinstellungen konfiguriert, die [hier](./media-services-mes-presets-overview.md)beschrieben sind.

### <a name="input-and-output-metadata"></a>Eingabe- und Ausgabemetadaten
Die Eingabemetadaten für den Encoder werden [hier](media-services-input-metadata-schema.md)beschrieben.

Die Ausgabemetadaten für den Encoder werden [hier](media-services-output-metadata-schema.md)beschrieben.

### <a name="generate-thumbnails"></a>Generieren von Miniaturansichten
Informationen finden Sie unter [Generieren von Miniaturansichten mithilfe von Media Encoder Standard](media-services-advanced-encoding-with-mes.md).

### <a name="trim-videos-clipping"></a>Kürzen von Videos (Clipping)
Informationen finden Sie unter [Kürzen von Videos mithilfe von Media Encoder Standard](media-services-advanced-encoding-with-mes.md#trim_video).

### <a name="create-overlays"></a>Erstellen von Überlagerungen
Informationen finden Sie unter [Erstellen von Überlagerungen mithilfe von Media Encoder Standard](media-services-advanced-encoding-with-mes.md#overlay).

### <a name="see-also"></a>Weitere Informationen
[Media Services-Blog](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/)

### <a name="known-issues"></a>Bekannte Probleme
Wenn Ihr Eingabevideo keine Untertitel enthält, enthält das Ausgabemedienobjekt trotzdem eine leere TTML-Datei.

## <a name="media-services-learning-paths"></a>Media Services-Lernpfade
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a>Verwandte Artikel
* [Ausführen von Aufgaben für die erweiterte Codierung durch Anpassen der Media Encoder Standard-Voreinstellungen](media-services-custom-mes-presets-with-dotnet.md)
* [Kontingente und Einschränkungen](media-services-quotas-and-limitations.md)

<!--Reference links in article-->
[1]: https://azure.microsoft.com/pricing/details/media-services/