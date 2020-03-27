---
title: Migrieren von Azure-Medienressourcen von Azure Deutschland zu Azure weltweit
description: Dieser Artikel enthält Informationen zum Migrieren von Azure-Medienressourcen von Azure Deutschland zu Azure weltweit.
author: gitralf
services: germany
cloud: Azure Germany
ms.author: ralfwi
ms.service: germany
ms.date: 12/12/2019
ms.topic: article
ms.custom: bfmigrate
ms.openlocfilehash: acfcb5a40a4f7deb10d8a28a9e8766eae7086a54
ms.sourcegitcommit: fe6c9a35e75da8a0ec8cea979f9dec81ce308c0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/26/2020
ms.locfileid: "75436655"
---
# <a name="migrate-media-resources-to-global-azure"></a>Migrieren von Medienressourcen zu Azure weltweit

> [!IMPORTANT]
> Seit [August 2018](https://news.microsoft.com/europe/2018/08/31/microsoft-to-deliver-cloud-services-from-new-datacentres-in-germany-in-2019-to-meet-evolving-customer-needs/) haben wir keine neuen Kunden akzeptiert und keine neuen Features und Dienste an den ursprünglichen Microsoft Cloud Deutschland-Standorten bereitgestellt.
>
> Aufgrund der Weiterentwicklung der Kundenbedürfnisse haben wir vor Kurzem zwei neue Rechenzentrumsregionen in Deutschland [gestartet](https://azure.microsoft.com/blog/microsoft-azure-available-from-new-cloud-regions-in-germany/), die Datenresidenz für Kundendaten, umfassende Konnektivität mit dem globalen Cloudnetzwerk von Microsoft sowie wettbewerbsfähige Preise bieten. 
>
> Profitieren Sie von der Vielfalt der Funktionen, Sicherheit auf Unternehmensniveau und den umfangreichen Features, die in unseren neuen deutschen Rechenzentrumsregionen zur Verfügung stehen, und [migrieren](germany-migration-main.md) Sie noch heute.

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
- [Sicherheit](./germany-migration-security.md)
- [Verwaltungstools](./germany-migration-management-tools.md)
