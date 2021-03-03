---
title: 'Skalieren reservierter Einheiten für Medien: CLI'
description: In diesem Thema wird gezeigt, wie Sie die CLI zum Skalieren der Medienverarbeitung mit Azure Media Services verwenden.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 09/30/2020
ms.author: inhenkel
ms.openlocfilehash: 98e87cf9d1f46ddb8ee1d433bd0b0ba8806fac89
ms.sourcegitcommit: 97c48e630ec22edc12a0f8e4e592d1676323d7b0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/18/2021
ms.locfileid: "101091933"
---
# <a name="how-to-scale-media-reserved-units"></a>Skalieren reservierter Einheiten für Medien

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

In diesem Artikel erfahren Sie, wie Sie reservierte Einheiten für Medien für eine schnellere Codierung skalieren.

## <a name="prerequisites"></a>Voraussetzungen

[Erstellen Sie ein Media Services-Konto.](./create-account-howto.md)

Grundkenntnisse über [reservierte Einheiten für Medien](concept-media-reserved-units.md)

## <a name="scale-media-reserved-units-with-cli"></a>Skalieren von reservierten Einheiten für Medien mit der CLI

Führen Sie den Befehl `mru` aus.

Mit dem folgenden Befehl [az ams account mru](/cli/azure/ams/account/mru?view=azure-cli-latest) werden reservierte Einheiten für Medien im Konto „amsaccount“ festgelegt, indem die Parameter **count** und **type** verwendet werden.

```azurecli
az ams account mru set -n amsaccount -g amsResourceGroup --count 10 --type S3
```

## <a name="billing"></a>Abrechnung

Die Abrechnung erfolgt nach der Anzahl der Minuten, in denen die reservierten Einheiten für Medien auf Ihrem Konto bereitgestellt werden. Dies erfolgt unabhängig davon, ob in Ihrem Konto Aufträge ausgeführt werden. Eine ausführliche Erläuterung finden Sie im Abschnitt mit den häufig gestellten Fragen auf der Seite [Media Services – Preise](https://azure.microsoft.com/pricing/details/media-services/).   

## <a name="next-step"></a>Nächster Schritt

[Analysieren von Videos](analyze-videos-tutorial-with-api.md)

## <a name="see-also"></a>Weitere Informationen

* [Kontingente und Grenzwerte](limits-quotas-constraints.md)
