---
title: 'Regionen, in dem Video Indexer verfügbar ist: Azure'
titleSuffix: Azure Media Services
description: In diesem Artikel werden Azure-Regionen behandelt, in denen Azure Media Services Video Indexer verfügbar ist.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 05/15/2019
ms.author: anzaman
ms.openlocfilehash: b71890323f6aca92fb9f847252feed57d8db234f
ms.sourcegitcommit: 3abadafcff7f28a83a3462b7630ee3d1e3189a0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2020
ms.locfileid: "82594949"
---
# <a name="azure-regions-in-which-video-indexer-exists"></a>Azure-Regionen, in denen Video Indexer vorhanden ist

Video Indexer-APIs enthalten einen Parameter **location**, den Sie auf die Azure-Region festlegen sollten, an die der Aufruf weitergeleitet werden soll. Dabei muss es sich um eine [Azure-Region handeln, in der Video Indexer verfügbar ist](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services&regions=all).

## <a name="locations"></a>Standorte

Dem Parameter `location` muss der Codename der Azure-Region als Wert zugewiesen werden. Wenn Sie Video Indexer im Vorschaumodus verwenden, sollten Sie `"trial"` als Wert verwenden. `trial` ist der Standardwert für den Parameter `location`. Andernfalls können Sie die folgende Zeile in der [Azure CLI](/cli/azure) ausführen, um den Codenamen der Azure-Region abzurufen, in der sich Ihr Konto befindet und a die Ihr Aufruf weitergeleitet werden soll:

```azurecli-interactive
az account list-locations
```

Nach dem Ausführen der oben gezeigten Zeile erhalten Sie eine Liste mit allen Azure-Regionen. Navigieren Sie zu der Azure-Region, die den *displayName* aufweist, nach dem Sie suchen, und verwenden ihren Wert *name* für den Parameter **location**.

Verwenden Sie für die Azure-Region „USA, Westen 2“ (unten angezeigt) beispielsweise „westus2“ als Wert für den Parameter **location**.

```json
   {
      "displayName": "West US 2",
      "id": "/subscriptions/35c2594a-23da-4fce-b59c-f6fb9513eeeb/locations/westus2",
      "latitude": "47.233",
      "longitude": "-119.852",
      "name": "westus2",
      "subscriptionId": null
    }
```

## <a name="next-steps"></a>Nächste Schritte

- [Anpassen des Sprachmodells mit APIs](customize-language-model-with-api.md)
- [Anpassen des Markenmodells mit APIs](customize-brands-model-with-api.md)
- [Anpassen des Personenmodells mit APIs](customize-person-model-with-api.md)
