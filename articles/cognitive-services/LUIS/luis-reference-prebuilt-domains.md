---
title: 'Vordefinierter Domänenverweis: LUIS'
titleSuffix: Azure Cognitive Services
description: Referenz für die vordefinierten Domänen, die vordefinierte Sammlungen von Absichten und Entitäten aus LUIS (Language Understanding Intelligent Services) darstellen.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 09/27/2019
ms.author: diberry
ms.openlocfilehash: d41ec3abfa2e562d5a6e11dd9ed0fcbcffead5c6
ms.sourcegitcommit: 9ce0350a74a3d32f4a9459b414616ca1401b415a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2020
ms.locfileid: "88191979"
---
# <a name="prebuilt-domain-reference-for-your-luis-app"></a>Vordefinierter Domänenverweis für die LUIS-App
Diese Referenz enthält Informationen über die [vordefinierten Domänen](luis-how-to-use-prebuilt-domains.md), bei denen es sich um vordefinierte Sammlungen der von LUIS gebotenen Absichten und Entitäten handelt.

[Benutzerdefinierte Domänen](luis-how-to-start-new-app.md) weisen im Gegensatz dazu anfänglich keine Absichten und Modelle auf. Sie können einem benutzerdefinierten Modell beliebige Absichten und Entitäten aus einer vordefinierten Domäne hinzufügen.

## <a name="prebuilt-domains-per-language"></a>Vordefinierte Domänen pro Sprache

In der folgenden Tabelle sind die derzeit unterstützten Domänen zusammengefasst. Die Unterstützung für Englisch ist in der Regel vollständiger als für andere Sprachen.

| Entitätstyp       | EN-US      | ZH-CN   | DE    | BV     | ES    | IT      | PT-BR |  JP  |      KO |        NL |    TR |
|:-----------------:|:-------:|:-------:|:-----:|:------:|:-----:|:-------:| :-------:| :-------:| :-------:| :-------:|  :-------:|
| Kalender  | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
|Kommunikation  | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| Email     | ✓    | ✓       | ✓   | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| HomeAutomation          | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| Notizen     | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| Orte   | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| RestaurantReservation  | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| ToDo     | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| Versorgungsunternehmen      | ✓    | ✓        | ✓    | ✓      | ✓     | ✓       | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| Weather        | ✓    | ✓        | ✓    | ✓      | ✓     | ✓       | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| Web    | ✓    | ✓        | ✓    | ✓      | ✓     | ✓       | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |

Vordefinierte Domänen werden in den folgenden Sprachen **nicht unterstützt**:

* Französisch (Kanada)
* Hindi
* Spanisch (Mexiko)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur [Entität vom Typ „Simple“](reference-entity-simple.md).
