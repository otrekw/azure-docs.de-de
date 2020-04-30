---
title: Docker-Pullvorgang für den Sprachenerkennungscontainer
titleSuffix: Azure Cognitive Services
description: Befehl „docker pull“ für den Sprachenerkennungscontainer
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: 3444d6267c09056697abf8339cc688a606b6dc17
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "80877024"
---
#### <a name="docker-pull-for-the-language-detection-container"></a>Docker-Pullvorgang für den Sprachenerkennungscontainer

Verwenden Sie den Befehl [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/), um ein Containerimage aus Microsoft Container Registry herunterzuladen.

Eine vollständige Beschreibung der verfügbaren Tags für die Textanalysecontainer finden Sie im Docker Hub im Container für die [Sprachenerkennung](https://go.microsoft.com/fwlink/?linkid=2018759).

```
docker pull mcr.microsoft.com/azure-cognitive-services/language:latest
```
