---
title: Verwenden der CLI zum Skalieren reservierter Einheiten für Medien – Azure | Microsoft-Dokumentation
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
ms.openlocfilehash: bcbe5fe71e5a4d4d39a29d4a6828c104f6891c0d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91617144"
---
# <a name="how-to-scale-media-reserved-units"></a>Skalieren reservierter Einheiten für Medien

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

In diesem Artikel erfahren Sie, wie Sie reservierte Einheiten für Medien für eine schnellere Codierung skalieren.

## <a name="prerequisites"></a>Voraussetzungen

[Erstellen Sie ein Media Services-Konto.](./create-account-howto.md)

Grundkenntnisse über [reservierte Einheiten für Medien](concept-media-reserved-units.md)

[!INCLUDE [media-services-cli-instructions](../../../includes/media-services-cli-instructions.md)]

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
