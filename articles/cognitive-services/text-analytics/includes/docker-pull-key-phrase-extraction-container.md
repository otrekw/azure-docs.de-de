---
title: Docker-Pullvorgang für den Schlüsselbegriffserkennungs-Container
titleSuffix: Azure Cognitive Services
description: Docker-Pullbefehl für den Schlüsselbegriffserkennungs-Container
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 09/12/2019
ms.author: dapine
ms.openlocfilehash: 59313528e4beb343f9ac1103d3f9f2faae5dfc16
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "70966705"
---
#### <a name="docker-pull-for-the-key-phrase-extraction-container"></a>Docker-Pullvorgang für den Schlüsselbegriffserkennungs-Container

Verwenden Sie den Befehl [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/), um ein Containerimage aus Microsoft Container Registry herunterzuladen.

Eine vollständige Beschreibung der verfügbaren Tags für die Textanalysecontainer finden Sie in Docker Hub im [Container für die Schlüsselbegriffserkennung](https://go.microsoft.com/fwlink/?linkid=2018757).

```
docker pull mcr.microsoft.com/azure-cognitive-services/keyphrase:latest
```
