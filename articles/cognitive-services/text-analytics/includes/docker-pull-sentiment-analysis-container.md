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
ms.openlocfilehash: a3db0e2ffdd4a75f02634ca2227c3c41416d4f65
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83588383"
---
#### <a name="docker-pull-for-the-sentiment-analysis-v3-container"></a>Docker-Pullvorgang für den Container der Standpunktanalyse v3

Der Container für die Standpunktanalyse v3 ist in mehreren Sprachen verfügbar. Verwenden Sie den folgenden Befehl, um den Container für Englisch herunterzuladen. 

```
docker pull mcr.microsoft.com/azure-cognitive-services/sentiment:3.0-en
```

Wenn Sie den Container für eine andere Sprache herunterladen möchten, ersetzen Sie `en` durch einen der folgenden Sprachcodes: 

| Textanalysecontainer | Sprachcode |
|--|--|
| Englisch | `en` |
| Spanisch | `es` |
| Französisch | `fr` |
| Italienisch | `it` |
| Deutsch | `de` |
| Chinesisch (vereinfacht) | `zh` |
| Chinesisch (traditionell) | `zht` |
| Japanisch | `ja` |
| Portugiesisch | `pt` |
| Niederländisch | `nl` |

Eine vollständige Beschreibung der verfügbaren Tags für die Textanalysecontainer finden Sie bei [Docker Hub](https://go.microsoft.com/fwlink/?linkid=2018654).