---
title: Docker-Pullvorgang für den Standpunktanalysecontainer
titleSuffix: Azure Cognitive Services
description: Befehl „docker pull“ für den Standpunktanalysecontainer
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: 32a550e120331a8255281d51725d2d5fc8ca1e05
ms.sourcegitcommit: 49b2069d9bcee4ee7dd77b9f1791588fe2a23937
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2021
ms.locfileid: "107564529"
---
#### <a name="docker-pull-for-the-sentiment-analysis-v3-container"></a>Docker-Pullvorgang für den Container der Standpunktanalyse v3

Der Container für die Standpunktanalyse v3 ist in mehreren Sprachen verfügbar. Verwenden Sie den folgenden Befehl, um den Container für Englisch herunterzuladen. 

```
docker pull mcr.microsoft.com/azure-cognitive-services/textanalytics/sentiment:3.0-en
```

Wenn Sie den Container für eine andere Sprache herunterladen möchten, ersetzen Sie `en` durch einen der folgenden Sprachcodes: 

| Textanalysecontainer | Sprachcode |
|--|--|
| Chinesisch (vereinfacht)    |   `zh-hans`   |
| Chinesisch (traditionell)   |   `zh-hant`   |
| Niederländisch                 |     `nl`      |
| Englisch               |     `en`      |
| Französisch                |     `fr`      |
| Deutsch                |     `de`      |
| Hindi                 |    `hi`       |
| Italienisch               |     `it`      |
| Japanisch              |     `ja`      |
| Koreanisch                |     `ko`      |
| Norwegisch (Bokmål)   |     `no`      |
| Portugiesisch (Brasilien)   |    `pt-BR`    |
| Portugiesisch (Portugal) |    `pt-PT`    |
| Spanisch               |     `es`      |
| Türkisch               |     `tr`      |

Eine vollständige Beschreibung der verfügbaren Tags für die Textanalysecontainer finden Sie bei [Docker Hub](https://go.microsoft.com/fwlink/?linkid=2018654).
