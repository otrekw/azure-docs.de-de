---
title: Clouds und Regionen, in denen Azure Media Services v3 verfügbar ist
description: In diesem Artikel werden Azure-Clouds und -Regionen behandelt, in denen Azure Media Services v3 verfügbar ist.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 08/31/2020
ms.author: inhenkel
ms.custom: references_regions
ms.openlocfilehash: e9da87f3b5f7cb513b4b5d51c93a59d1703d401d
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/01/2020
ms.locfileid: "89258655"
---
# <a name="clouds-and-regions-in-which-azure-media-services-v3-exists"></a>Clouds und Regionen, in denen Azure Media Services v3 verfügbar ist

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Azure Media Services v3 ist über ein Azure Resource Manager-Manifest im globalen Azure, in Azure Government, Azure Deutschland und Azure China 21Vianet verfügbar. Allerdings sind nicht alle Media Services-Features in allen Azure-Clouds verfügbar. In diesem Dokument wird die Verfügbarkeit der Hauptkomponenten von Media Services v3 beschrieben.

## <a name="feature-availability-in-azure-clouds"></a>Featureverfügbarkeit in Azure-Clouds

| Funktion|Globale Azure-Regionen | Azure Government|Azure Deutschland|Azure China 21Vianet|
| --- | --- | --- | --- | --- |
| [Azure EventGrid](reacting-to-media-services-events.md) | Verfügbar | Nicht verfügbar | Nicht verfügbar | Nicht verfügbar |
| [VideoAnalyzerPreset](analyzing-video-audio-files-concept.md) |  Verfügbar | Nicht verfügbar | Nicht verfügbar | Nicht verfügbar |
| [AudioAnalyzerPreset](analyzing-video-audio-files-concept.md) |  Verfügbar | Nicht verfügbar | Nicht verfügbar | Nicht verfügbar |
| [StandardEncoderPreset](encoding-concept.md) | Verfügbar | Verfügbar | Verfügbar | Verfügbar |
| [LiveEvents](live-streaming-overview.md) | Verfügbar | Verfügbar | Verfügbar | Verfügbar |
| [StreamingEndpoints](streaming-endpoint-concept.md) | Verfügbar | Verfügbar | Verfügbar | Verfügbar |

## <a name="feature-availability-in-preview"></a>Featureverfügbarkeit in der Vorschauversion

[LiveTranscription](live-transcription.md) ist in den folgenden Regionen verfügbar:

- Asien, Südosten
- Europa, Westen
- Nordeuropa
- East US
- USA (Mitte)
- USA Süd Mitte
- USA, Westen 2
- Brasilien Süd

## <a name="regionsgeographieslocations"></a>Regionen/Geografien/Standorte

[Regionen, in denen der Azure Media Services-Dienst bereitgestellt wird](https://azure.microsoft.com/global-infrastructure/services/?products=media-services)

### <a name="region-code-name"></a>Name des Regionscodes

Wenn Sie den Parameter **location** festlegen müssen, müssen Sie den Regionscodenamen als **location**-Wert angeben. Sie können die folgende Zeile in der [Azure CLI](/cli/azure/?view=azure-cli-latest) ausführen, um den Codenamen der Region abzurufen, in der sich Ihr Konto befindet und an die Ihr Aufruf weitergeleitet werden soll:

```azurecli-interactive
az account list-locations
```

Nach dem Ausführen der oben gezeigten Zeile erhalten Sie eine Liste mit allen Azure-Regionen. Navigieren Sie zu der Azure-Region, die den *displayName* aufweist, nach dem Sie suchen, und verwenden ihren Wert *name* für den Parameter **location**.

Verwenden Sie für die Azure-Region „USA, Westen 2“ (unten angezeigt) beispielsweise „westus2“ als Wert für den Parameter **location**.

```json
   {
      "displayName": "West US 2",
      "id": "/subscriptions/00000000-23da-4fce-b59c-f6fb9513eeeb/locations/westus2",
      "latitude": "47.233",
      "longitude": "-119.852",
      "name": "westus2",
      "subscriptionId": null
    }
```

## <a name="endpoints"></a>Endpunkte  

Die folgenden Endpunkte sind wichtig für Verbindungen mit Media Services-Konten aus verschiedenen nationalen Azure-Clouds.

### <a name="global-azure"></a>Globale Azure-Umgebung

| Dienst | Endpunkt |
| ------- | -------- |
| Azure Resource Manager |  `https://management.azure.com/` |
| Authentifizierung | `https://login.microsoftonline.com/` |
| Zielgruppe | `https://management.core.windows.net/` |

### <a name="azure-government"></a>Azure Government

| Dienst | Endpunkt |
| ------- | -------- |
| Azure Resource Manager |  `https://management.usgovcloudapi.net/` |
| Authentifizierung | `https://login.microsoftonline.us/` |
| Zielgruppe | `https://management.core.usgovcloudapi.net/` |

### <a name="azure-germany"></a>Azure Deutschland

| Dienst | Endpunkt |
| ------- | -------- |
| Azure Resource Manager | `https://management.cloudapi.de/` |
| Authentifizierung | `https://login.microsoftonline.de/` |
| Zielgruppe | `https://management.core.cloudapi.de/`|

### <a name="azure-china-21vianet"></a>Azure China 21Vianet

| Dienst | Endpunkt |
| ------- | -------- |
| Azure Resource Manager | `https://management.chinacloudapi.cn/` |
| Authentifizierung | `https://login.chinacloudapi.cn/` |
| Zielgruppe |  `https://management.core.chinacloudapi.cn/` |

## <a name="see-also"></a>Weitere Informationen

* [Azure-Regionen](https://azure.microsoft.com/global-infrastructure/regions/)
* [Azure-Geografien](https://azure.microsoft.com/global-infrastructure/geographies/)
* [Azure-Standorte](https://azure.microsoft.com/global-infrastructure/locations/)

## <a name="next-steps"></a>Nächste Schritte

[Media Services v3 – Übersicht](media-services-overview.md)
