---
title: Angeben der Modellversion in Textanalyse v3
titleSuffix: Azure Cognitive Services
description: Es wird beschrieben, wie Sie das Textanalyse-API-Modell angeben, das für Ihre Daten verwendet wird.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: article
ms.date: 04/21/2020
ms.author: aahi
ms.openlocfilehash: 9431ff862dd987a1a806087053014e7c880bf801
ms.sourcegitcommit: f0b206a6c6d51af096a4dc6887553d3de908abf3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84143259"
---
# <a name="model-versioning-in-the-text-analytics-api"></a>Versionsverwaltung der Modelle in der Textanalyse-API

Mit Version 3 der Textanalyse-API können Sie die für Ihre Daten verwendete Modellversion auswählen. Verwenden Sie den optionalen Parameter `model-version`, um die Version des Modells in Ihren API-Anforderungen auszuwählen. Beispiel: `<resource-url>/text/analytics/v3.0/sentiment?model-version=2020-04-01`. Wenn dieser Parameter nicht angegeben wird, wird für die API standardmäßig die letzte stabile Version verwendet. 

## <a name="available-versions"></a>Verfügbare Versionen

Ermitteln Sie anhand der Tabelle unten, welche Modellversionen von den einzelnen Endpunkten unterstützt werden.


| Endpunkt                        | Unterstützte Versionen                       | Aktuelle Version |
|---------------------------------|------------------------------------------|----------------|
| `/sentiment`                    | `2019-10-01`, `2020-04-01`               | `2020-04-01`   |
| `/languages`                    | `2019-10-01`                             | `2019-10-01`   |
| `/entities/linking`             | `2019-10-01`, `2020-02-01`               | `2020-02-01`   |
| `/entities/recognition/general` | `2019-10-01`, `2020-02-01`, `2020-04-01` | `2020-04-01`   |
| `/entities/recognition/pii`     | `2019-10-01`, `2020-02-01`, `2020-04-01` | `2020-04-01`   |
| `/keyphrases`                   | `2019-10-01`                             | `2019-10-01`   |


Sie finden Details zu den Updates für diese Modelle im Artikel zu den [Neuigkeiten ](../whats-new.md).

## <a name="next-steps"></a>Nächste Schritte

* [Übersicht über die Textanalyse](../overview.md)
* [Standpunktanalyse](../how-tos/text-analytics-how-to-sentiment-analysis.md)
* [Entitätserkennung](../how-tos/text-analytics-how-to-entity-linking.md)