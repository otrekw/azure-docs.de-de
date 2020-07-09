---
title: Livetranskription
titleSuffix: Azure Media Services
description: Erfahren Sie mehr zur Azure Media Services-Livetranskription.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 11/19/2019
ms.author: inhenkel
ms.openlocfilehash: 9481b4ee2f225c7f76337d73b27630e4c67cc780
ms.sourcegitcommit: 1f48ad3c83467a6ffac4e23093ef288fea592eb5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2020
ms.locfileid: "84193610"
---
# <a name="live-transcription-preview"></a>Livetranskription (Preview)

Azure Media Service übermittelt Video, Audio und Text in unterschiedlichen Protokollen. Wenn Sie Ihren Livestream mittels MPEG-DASH oder HLS/CMAF veröffentlichen, stellt unser Dienst neben den Video- und Audiospuren auch den transkribierten Text im IMSC1.1-kompatiblen TTML-Format bereit. Die Übermittlung ist in MPEG-4 Part 30-Fragmente (ISO/IEC 14496-30) gepackt. Bei Verwendung der Zustellung über HLS/TS wird Text als VTT-Blöcke übermittelt.

In diesem Artikel wird beschrieben, wie Sie Livetranskription aktivieren, wenn Sie ein Liveereignis mit Azure Media Services v3 streamen. Bevor Sie fortfahren, stellen Sie sicher, dass Sie mit der Verwendung von Media Services v3-REST-APIs vertraut sind (weitere Informationen finden Sie in [diesem Tutorial](stream-files-tutorial-with-rest.md)). Sie sollten außerdem mit dem Konzept des [Livestreamings](live-streaming-overview.md) vertraut sein. Es wird empfohlen, das Tutorial [Livestreaming mit Media Services](stream-live-tutorial-with-api.md) durchzuarbeiten.

> [!NOTE]
> Derzeit ist die Livetranskription nur als Vorschaufunktion in der Region „USA, Westen 2“ verfügbar. Sie unterstützt die Transkription von gesprochener englischer Sprache in Text. Die API-Referenz zu diesem Feature finden Sie nachstehend. Da es sich in der Vorschauphase befindet, sind die Details nicht in der REST-Dokumentation verfügbar.

## <a name="creating-the-live-event"></a>Erstellen des Liveereignisses

Zum Erstellen des Liveereignisses senden Sie den PUT-Vorgang an die Version 2019-05-01-preview, z. B.:

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

Fragen Sie den Status des Liveereignisses ab, bis es in den Zustand „Wird ausgeführt“ wechselt, was darauf hinweist, dass Sie jetzt einen RTMP-Beitragsfeed senden können. Sie können jetzt dieselben Schritte wie in diesem Tutorial ausführen, z. B. das Überprüfen des Vorschaufeeds und das Erstellen von Liveausgaben.

## <a name="start-transcription-after-live-event-has-started"></a>Starten der Transkription nach dem Start des Liveereignisses

Die Livetranskription kann gestartet werden, nachdem ein Liveereignis begonnen hat. Um Livetranskriptionen zu aktivieren, patchen Sie das Liveereignis, um die transcriptions-Eigenschaft einzuschließen. Um Livetranskriptionen zu deaktivieren, wird die transcriptions-Eigenschaft aus dem Objekt des Liveereignisses entfernt.

> [!NOTE]
> Das mehrmalige Aktivieren oder Deaktivieren der Transkription während des Liveereignisses wird nicht unterstützt.

Dies ist der Beispielaufruf zum Aktivieren von Livetranskriptionen.

PATCH: ```https://management.azure.com/subscriptions/:subscriptionId/resourceGroups/:resourceGroupName/providers/Microsoft.Media/mediaServices/:accountName/liveEvents/:liveEventName?api-version=2019-05-01-preview```

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

## <a name="transcription-delivery-and-playback"></a>Übermittlung und Wiedergabe der Transkription

Lesen Sie den Artikel [Dynamische Paketerstellung – Übersicht](dynamic-packaging-overview.md#to-prepare-your-source-files-for-delivery), worin beschrieben wird, wie unser Dienst dynamische Paketerstellung verwendet, um Video, Audio und Text in unterschiedlichen Protokollen zu übermitteln. Wenn Sie Ihren Livestream mittels MPEG-DASH oder HLS/CMAF veröffentlichen, stellt unser Dienst neben den Video- und Audiospuren auch den transkribierten Text im IMSC1.1-kompatiblen TTML-Format bereit. Diese Übermittlung ist in MPEG-4 Part 30-Fragmente (ISO/IEC 14496-30) gepackt. Bei Übermittlung über HLS/TS wird Text in Form von VTT-Blöcken übermittelt. Sie können einen Webplayer wie [Azure Media Player](use-azure-media-player.md) verwenden, um den Stream wiederzugeben.  

> [!NOTE]
> Wenn Sie Azure Media Player verwenden, verwenden Sie die Version 2.3.3 oder höher.

## <a name="known-issues"></a>Bekannte Probleme

In der Vorschauphase liegen die folgenden bekannten Probleme bei der Livetranskription vor:

* Die Funktion ist nur in der Region „USA, Westen 2“ verfügbar.
* Apps müssen die Vorschau-APIs verwenden, die in der [Media Services v3 OpenAPI-Spezifikation](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/mediaservices/resource-manager/Microsoft.Media/preview/2019-05-01-preview/streamingservice.json) beschrieben sind.
* Die einzige unterstützte Sprache ist Englisch (en-us).
* Zum Inhaltsschutz wird nur die AES-Umschlagverschlüsselung unterstützt.

## <a name="next-steps"></a>Nächste Schritte

* [Media Services: Übersicht](media-services-overview.md)
