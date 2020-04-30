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
ms.openlocfilehash: e502b886c8031174180d40f1c5b7e373a974ee8e
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "80877020"
---
#### <a name="docker-pull-for-the-sentiment-analysis-container"></a>Docker-Pullvorgang für den Standpunktanalysecontainer

Verwenden Sie den Befehl [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/), um ein Containerimage aus Microsoft Container Registry herunterzuladen.

Eine vollständige Beschreibung der verfügbaren Tags für die Textanalysecontainer finden Sie im Docker Hub im Container für die [Standpunktanalyse](https://go.microsoft.com/fwlink/?linkid=2018654).

```
docker pull mcr.microsoft.com/azure-cognitive-services/sentiment:latest
```
