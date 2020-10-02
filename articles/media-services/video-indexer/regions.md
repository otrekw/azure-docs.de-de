---
title: 'Regionen, in dem Video Indexer verfügbar ist: Azure'
titleSuffix: Azure Media Services
description: In diesem Artikel werden Azure-Regionen behandelt, in denen Azure Media Services Video Indexer verfügbar ist.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 09/14/2020
ms.author: juliako
ms.openlocfilehash: 6ebdb22f50efbefc695f9752c6e6fc333571828c
ms.sourcegitcommit: 03662d76a816e98cfc85462cbe9705f6890ed638
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/15/2020
ms.locfileid: "90530935"
---
# <a name="azure-regions-in-which-video-indexer-exists"></a>Azure-Regionen, in denen Video Indexer vorhanden ist

Video Indexer-APIs enthalten einen Parameter **location**, den Sie auf die Azure-Region festlegen sollten, an die der Aufruf weitergeleitet werden soll. Dabei muss es sich um eine [Azure-Region handeln, in der Video Indexer verfügbar ist](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services&regions=all).

## <a name="locations"></a>Standorte

Dem Parameter `location` muss der Codename der Azure-Region als Wert zugewiesen werden. Wenn Sie Video Indexer im Vorschaumodus verwenden, sollten Sie `"trial"` als Wert verwenden. `trial` ist der Standardwert für den Parameter `location`. Andernfalls können Sie das Azure-Portal verwenden oder einen [Azure CLI-Befehl](/cli/azure) ausführen, um den Codenamen der Azure-Region abzurufen, in der sich Ihr Konto befindet und an die Ihr Aufruf weitergeleitet werden soll.

### <a name="azure-portal"></a>Azure-Portal

1. Melden Sie sich bei der [Video Indexer](https://www.videoindexer.ai/)-Website an.
1. Klicken Sie rechts oben auf der Seite auf **Benutzerkonten**.
1. Suchen Sie in der oberen rechten Ecke nach dem Standort Ihres Kontos.  

    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/location/location1.png" alt-text="Location":::
    
###  <a name="cli-command"></a>CLI-Befehl

```azurecli-interactive
az account list-locations
```

Nach dem Ausführen der oben gezeigten Zeile erhalten Sie eine Liste mit allen Azure-Regionen. Navigieren Sie zu der Azure-Region, die den *displayName* aufweist, nach dem Sie suchen, und verwenden ihren Wert *name* für den Parameter **location**.

Verwenden Sie für die Azure-Region „USA, Westen 2“ (unten angezeigt) beispielsweise „westus2“ als Wert für den Parameter **location**.

```json
   {
      "displayName": "West US 2",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/locations/westus2",
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
