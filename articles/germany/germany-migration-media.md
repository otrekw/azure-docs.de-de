---
title: Migrieren von Azure-Medienressourcen von Azure Deutschland zu Azure weltweit
description: Dieser Artikel enthält Informationen zum Migrieren von Azure-Medienressourcen von Azure Deutschland zu Azure weltweit.
ms.topic: article
ms.date: 10/16/2020
author: gitralf
ms.author: ralfwi
ms.service: germany
ms.custom: bfmigrate
ms.openlocfilehash: 61e13d629f9c2abe84af5cc881da0820eabef365
ms.sourcegitcommit: 7dacbf3b9ae0652931762bd5c8192a1a3989e701
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2020
ms.locfileid: "92123554"
---
# <a name="migrate-media-resources-to-global-azure"></a>Migrieren von Medienressourcen zu Azure weltweit

[!INCLUDE [closureinfo](../../includes/germany-closure-info.md)]

Dieser Artikel enthält Informationen dazu, wie Sie Azure-Medienressourcen von Azure Deutschland zu Azure weltweit migrieren können.

## <a name="media-services"></a>Media Services

In Azure Media Services konfigurieren Sie Ihr eigenes Speicherkonto und alle Medienobjekte. Erstellen Sie zunächst ein neues Media Services-Konto in Azure weltweit. Laden Sie dann die entsprechenden Medienartefakte erneut, und führen Sie das Codieren und das Streamen unter dem neuen Media Services-Konto aus.

Weitere Informationen finden Sie unter:

- Frischen Sie Ihre Kenntnisse auf, indem Sie die [Tutorials zu Media Services](https://docs.microsoft.com/azure/media-services/previous/) durcharbeiten.
- Lesen Sie die [Übersicht über Media Services](../media-services/previous/media-services-overview.md).
- Erfahren Sie, wie Sie [ein Media Services-Konto erstellen](../media-services/previous/media-services-portal-create-account.md).

## <a name="media-player"></a>Media Player

Sie können mehrere Endpunkte in Azure Media Player auswählen. Sie können Ihre Inhalte von Azure Deutschland- oder Azure-weltweit-Endpunkten streamen.

Weitere Informationen finden Sie unter [Azure Media Player](https://ampdemo.azureedge.net/azuremediaplayer.html).

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über Tools, Techniken und Empfehlungen zum Migrieren von Ressourcen in den folgenden Dienstkategorien:

- [Compute](./germany-migration-compute.md)
- [Netzwerk](./germany-migration-networking.md)
- [Storage](./germany-migration-storage.md)
- [Web](./germany-migration-web.md)
- [Datenbanken](./germany-migration-databases.md)
- [Analyse](./germany-migration-analytics.md)
- [IoT](./germany-migration-iot.md)
- [Integration](./germany-migration-integration.md)
- [Identität](./germany-migration-identity.md)
- [Security](./germany-migration-security.md)
- [Verwaltungstools](./germany-migration-management-tools.md)
