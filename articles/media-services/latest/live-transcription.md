---
title: Azure Media Services-Livetranskription | Microsoft-Dokumentation
description: In diesem Artikel wird erläutert, worum es sich bei der Azure Media Services-Livetranskription handelt.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 11/19/2019
ms.author: juliako
ms.openlocfilehash: a85f9f8b9d98f77cf673778f031d8f47f132fbe1
ms.sourcegitcommit: b77e97709663c0c9f84d95c1f0578fcfcb3b2a6c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/22/2019
ms.locfileid: "74327341"
---
# <a name="live-transcription-preview"></a>Livetranskription (Preview)

Azure Media Service bietet Video, Audio und jetzt Text in unterschiedlichen Protokollen. Wenn Sie Ihren Livestream mittels MPEG-DASH oder HLS/CMAF veröffentlichen, stellt unser Dienst neben den Video- und Audiospuren auch den transkribierten Text im IMSC1.1-kompatiblen TTML-Format bereit, gepackt in MPEG-4 Part 30-Fragmenten (ISO/IEC 14496-30) bereit. Bei Verwendung der Zustellung über HLS/TS wird Text als VTT-Blöcke übermittelt. 

In diesem Artikel wird beschrieben, wie Sie Livetranskription aktivieren, wenn Sie ein Liveereignis mit Azure Media Services v3 streamen. Bevor Sie fortfahren, stellen Sie sicher, dass Sie mit der Verwendung von Media Services v3-REST-APIs vertraut sind (Weitere Informationen finden Sie in [diesem Tutorial](stream-files-tutorial-with-rest.md).). Sie sollten außerdem mit dem Konzept des [Livestreamings](live-streaming-overview.md) vertraut sein. Es wird empfohlen, das Tutorial [Livestreaming mit Media Services](stream-live-tutorial-with-api.md) durchzuarbeiten. 

> [!NOTE]
> Derzeit ist die Livetranskription nur als Vorschaufunktion in der Region „USA, Westen 2“ verfügbar. Sie unterstützt die Transkription von gesprochener englischer Sprache in Text. Die API-Referenz für diese Funktion finden Sie in diesem Dokument – da sie sich in der Vorschauphase befindet, sind die Details nicht in der REST-Dokumentation verfügbar. 

## <a name="creating-the-live-event"></a>Erstellen des Liveereignisses 

Zum Erstellen des Liveereignisses senden Sie den PUT-Vorgang an die Version 2019-05-01, z. B.: 

```
PUT https://management.azure.com/subscriptions/:subscriptionId/resourceGroups/:resourceGroupName/providers/Microsoft.Media/mediaServices/:accountName/liveEvents/:liveEventName?api-version=2019-05-01-preview&autoStart=true 
```

Der Vorgang weist den folgenden Funktionskörper auf (wobei ein Pass-Through-Liveereignis mit RTMP als Erfassungsprotokoll erstellt wird). Beachten Sie das Hinzufügen einer transcriptions-Eigenschaft. Der einzige zulässige Wert für die Sprache ist „en-US“. 

```
{ 
  "properties": { 
    "description": "Demonstrate how to enable live transcriptions", 
    "input": { 
      "streamingProtocol": "RTMP", 
      "accessControl": { 
        "ip": { 
          "allow": [ 
            { 
              "name": "Allow All", 
              "address": "0.0.0.0", 
              "subnetPrefixLength": 0 
            } 
          ] 
        } 
      } 
    }, 
    "preview": { 
      "accessControl": { 
        "ip": { 
          "allow": [ 
            { 
              "name": "Allow All", 
              "address": "0.0.0.0", 
              "subnetPrefixLength": 0 
            } 
          ] 
        } 
      } 
    }, 
    "encoding": { 
      "encodingType": "None" 
    }, 
    "transcriptions": [ 
      { 
        "language": "en-US" 
      } 
    ], 
    "vanityUrl": false, 
    "streamOptions": [ 
      "Default" 
    ] 
  }, 
  "location": "West US 2" 
} 
```

Sie sollten den Status des Liveereignisses abfragen, bis es in den Zustand „Wird ausgeführt“ wechselt, was darauf hinweist, dass Sie jetzt einen RTMP-Beitragsfeed senden können. Sie können jetzt dieselben Schritte wie in diesem Tutorial ausführen, z. B. das Überprüfen des Vorschaufeeds und das Erstellen von Liveausgaben. 

## <a name="delivery-and-playback"></a>Übermittlung und Wiedergabe 

Lesen Sie den Artikel [Dynamische Paketerstellung – Übersicht](dynamic-packaging-overview.md#to-prepare-your-source-files-for-delivery), worin beschrieben wird, wie unser Dienst dynamische Paketerstellung verwendet, um Video, Audio und jetzt Text in unterschiedlichen Protokollen zu übermitteln. Wenn Sie Ihren Livestream mittels MPEG-DASH oder HLS/CMAF veröffentlichen, stellt unser Dienst neben den Video- und Audiospuren auch den transkribierten Text im IMSC1.1-kompatiblen TTML-Format bereit, gepackt in MPEG-4 Part 30-Fragmenten (ISO/IEC 14496-30) bereit. Bei Verwendung der Zustellung über HLS/TS wird Text als VTT-Blöcke übermittelt. Sie können einen Webplayer wie [Azure Media Player](use-azure-media-player.md) verwenden, um den Stream wiederzugeben.  

> [!NOTE]
>  Wenn Sie Azure Media Player verwenden, verwenden Sie die Version 2.3.3 oder höher.

## <a name="known-issues"></a>Bekannte Probleme 

In der Vorschauphase liegen die folgenden bekannten Probleme bei der Livetranskription vor 

* Die Funktion ist nur in der Region „USA, Westen 2“ verfügbar.
* Anwendungen müssen die Vorschau-APIs verwenden, die in der [Media Services v3 OpenAPI-Spezifikation](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/mediaservices/resource-manager/Microsoft.Media/preview/2019-05-01-preview/streamingservice.json) beschrieben sind.
* Die einzige unterstützte Sprache ist Englisch.
* Hinsichtlich des Inhaltsschutzes wird nur AES-Umschlagverschlüsselung unterstützt.

## <a name="next-steps"></a>Nächste Schritte

[Media Services: Übersicht](media-services-overview.md)
