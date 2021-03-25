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
ms.date: 03/11/2021
ms.author: aahi
ms.openlocfilehash: 8b596a5e54c0b59c4c0b49aa5cdc4fd6477d46dc
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "104599340"
---
# <a name="supported-entity-categories-in-the-text-analytics-api-v3"></a>Unterstützte Entitätskategorien in der Textanalyse-API v3.

Ermitteln Sie anhand der Informationen in diesem Artikel die Entitätskategorien, die von der [Erkennung benannter Entitäten](how-tos/text-analytics-how-to-entity-linking.md) (Named Entity Recognition, NER) zurückgegeben werden können. Die NER (Erkennung benannter Entitäten) führt ein Vorhersagemodell aus, um benannte Entitäten aus einem Eingabedokument zu identifizieren und zu kategorisieren.

Darüber hinaus ist eine Vorschau von NER v3.1 verfügbar, die eine Funktion zum Erkennen von personenbezogenen Informationen (`PII`) und von Gesundheitsinformationen (`PHI`) beinhaltet. Klicken Sie außerdem auf die Registerkarte **Integrität**, um eine Liste der unterstützten Kategorien in Text Analytics for Health anzuzeigen. 

Im [Migrationsleitfaden](migration-guide.md?tabs=named-entity-recognition) finden Sie eine Liste der von Version 2.1 zurückgegebenen Typen.

## <a name="entity-categories"></a>Entitätskategorien

#### <a name="general"></a>[Allgemein](#tab/general)

[!INCLUDE [supported entity types - general](./includes/entity-types/general-entities.md)]

#### <a name="pii"></a>[PII](#tab/personal)

[!INCLUDE [supported entity types - personally identifying information](./includes/entity-types/personal-information-entities.md)]

#### <a name="health"></a>[Gesundheit](#tab/health)

[!INCLUDE [biomedical entity types](./includes/entity-types/health-entities.md)]

***

## <a name="next-steps"></a>Nächste Schritte

* [Verwenden der Erkennung benannter Entitäten in der Textanalyse](how-tos/text-analytics-how-to-entity-linking.md)
