---
title: Beispiel zum Ausführen eines Containers mit dem „docker run“-Befehl
titleSuffix: Azure Cognitive Services
description: Befehl „docker run“ für Standpunktanalyse-Container
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/29/2020
ms.author: aahi
ms.openlocfilehash: 253c391b222c757a367bd5cd39939052cc697b00
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83588386"
---
Zum Ausführen des Containers für *Standpunktanalyse v3* führen Sie den folgenden `docker run`-Befehl aus:

```bash
docker run --rm -it -p 5000:5000 --memory 8g --cpus 1 \
mcr.microsoft.com/azure-cognitive-services/sentiment \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Dieser Befehl:

* Führt einen *Standpunktanalyse-Container* aus dem Containerimage aus
* Zuweisen von einem CPU-Kern und 8 GB Arbeitsspeicher
* Verfügbarmachen des TCP-Ports 5000 und Zuweisen einer Pseudo-TTY-Verbindung für den Container
* Entfernt den Container automatisch, nachdem er beendet wurde. Das Containerimage ist auf dem Hostcomputer weiterhin verfügbar.