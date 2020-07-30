---
title: Unterstützte Kategorien für die Erkennung benannter Entitäten
titleSuffix: Azure Cognitive Services
description: Hier erfahren Sie mehr über die unterstützten Entitätskategorien in der Textanalyse-API.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: article
ms.date: 07/28/2020
ms.author: aahi
ms.openlocfilehash: f66ed9a7d26a0c2d7def7d17820379b8e3460460
ms.sourcegitcommit: f353fe5acd9698aa31631f38dd32790d889b4dbb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2020
ms.locfileid: "87373067"
---
# <a name="supported-entity-categories-in-the-text-analytics-api-v3"></a>Unterstützte Entitätskategorien in der Textanalyse-API v3.

Ermitteln Sie anhand der Informationen in diesem Artikel die Entitätskategorien, die von der [Erkennung benannter Entitäten](how-tos/text-analytics-how-to-entity-linking.md) (Named Entity Recognition, NER) zurückgegeben werden können. Darüber hinaus ist eine Vorschau von NER v3.1 verfügbar, die eine Funktion zum Erkennen von personenbezogenen Informationen (`PII`) und von Gesundheitsinformationen (`PHI`) beinhaltet. Klicken Sie außerdem auf die Registerkarte **Integrität**, um eine Liste der unterstützten Kategorien in Text Analytics for Health anzuzeigen.

## <a name="entity-categories"></a>Entitätskategorien

#### <a name="general"></a>[Allgemein](#tab/general)

[!INCLUDE [supported entity types - general](./includes/entity-types/general-entities.md)]

#### <a name="personal"></a>[Persönlich](#tab/personal)

[!INCLUDE [supported entity types - personally identifying information](./includes/entity-types/personal-information-entities.md)]

#### <a name="health"></a>[Gesundheit](#tab/health)

[!INCLUDE [biomedical entity types](./includes/entity-types/health-entities.md)]

***

## <a name="next-steps"></a>Nächste Schritte

* [Verwenden der Erkennung benannter Entitäten in der Textanalyse](how-tos/text-analytics-how-to-entity-linking.md)
