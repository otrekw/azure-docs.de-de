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
ms.openlocfilehash: 549ecf940c53267db6b3caa576c4c24db414337a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90906089"
---
#### <a name="docker-pull-for-the-sentiment-analysis-v3-container"></a>Docker-Pullvorgang für den Container der Standpunktanalyse v3

Der Container für die Standpunktanalyse v3 ist in mehreren Sprachen verfügbar. Verwenden Sie den folgenden Befehl, um den Container für Englisch herunterzuladen. 

```
docker pull mcr.microsoft.com/azure-cognitive-services/textanalytics/sentiment:3.0-en
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