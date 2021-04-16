---
title: 'Skalieren reservierter Einheiten für Medien: CLI'
description: In diesem Thema wird gezeigt, wie Sie die CLI zum Skalieren der Medienverarbeitung mit Azure Media Services verwenden.
services: media-services
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.topic: how-to
ms.date: 03/22/2021
ms.author: inhenkel
ms.openlocfilehash: c5fa3aa8397ea6e13500717f035c414af8de8e3d
ms.sourcegitcommit: 9f4510cb67e566d8dad9a7908fd8b58ade9da3b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/01/2021
ms.locfileid: "106121675"
---
# <a name="how-to-scale-media-reserved-units"></a>Skalieren reservierter Einheiten für Medien

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

In diesem Artikel erfahren Sie, wie Sie reservierte Einheiten für Medien für eine schnellere Codierung skalieren.

> [!WARNING]
> Dieser Befehl funktioniert nicht mehr für Media Services-Konten, die mit der API-Version 2020-05-01 oder höher erstellt werden. Für diese Konten sind keine reservierten Einheiten für Medien mehr erforderlich, da das System je nach Auslastung automatisch zentral hoch- und herunterskaliert wird. Wenn die Option zum Verwalten von reservierten Einheiten für Medien im Azure-Portal nicht angezeigt wird, bedeutet dies, dass Sie ein Konto verwenden, das mit einer API ab Version 2020-05-01 erstellt wurde.

## <a name="prerequisites"></a>Voraussetzungen

[Erstellen Sie ein Media Services-Konto.](./account-create-how-to.md)

Grundkenntnisse über [reservierte Einheiten für Medien](concept-media-reserved-units.md)

## <a name="scale-media-reserved-units-with-cli"></a>Skalieren von reservierten Einheiten für Medien mit der CLI

Führen Sie den Befehl `mru` aus.

Mit dem folgenden Befehl [az ams account mru](/cli/azure/ams/account/mru) werden reservierte Einheiten für Medien im Konto „amsaccount“ festgelegt, indem die Parameter **count** und **type** verwendet werden.

```azurecli
az ams account mru set -n amsaccount -g amsResourceGroup --count 10 --type S3
```

## <a name="billing"></a>Abrechnung

Die Abrechnung erfolgt nach der Anzahl der Minuten, in denen die reservierten Einheiten für Medien auf Ihrem Konto bereitgestellt werden. Dies erfolgt unabhängig davon, ob in Ihrem Konto Aufträge ausgeführt werden. Eine ausführliche Erläuterung finden Sie im Abschnitt mit den häufig gestellten Fragen auf der Seite [Media Services – Preise](https://azure.microsoft.com/pricing/details/media-services/).   

## <a name="next-step"></a>Nächster Schritt

[Analysieren von Videos](analyze-videos-tutorial.md)

## <a name="see-also"></a>Weitere Informationen

* [Kontingente und Grenzwerte](limits-quotas-constraints-reference.md)
