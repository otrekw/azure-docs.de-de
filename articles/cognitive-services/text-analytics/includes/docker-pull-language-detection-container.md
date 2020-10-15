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
ms.openlocfilehash: 95bcb4b424010f63ac1ee4eb02f9e4793647051a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90906123"
---
#### <a name="docker-pull-for-the-language-detection-container"></a>Docker-Pullvorgang für den Sprachenerkennungscontainer

Verwenden Sie den Befehl [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/), um ein Containerimage aus Microsoft Container Registry herunterzuladen.

Eine vollständige Beschreibung der verfügbaren Tags für die Textanalysecontainer finden Sie im Docker Hub im Container für die [Sprachenerkennung](https://go.microsoft.com/fwlink/?linkid=2018759).

```
docker pull mcr.microsoft.com/azure-cognitive-services/textanalytics/language:latest
```
