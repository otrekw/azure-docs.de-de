---
title: Beispiel zum Ausführen eines Containers mit dem „docker run“-Befehl
titleSuffix: Azure Cognitive Services
description: Docker-Ausführungsbefehl für den Schlüsselbegriffserkennungs-Container
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 04/29/2020
ms.author: aahi
ms.openlocfilehash: 97efad40b28b1736658ffcbe468fb448edb1dacc
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "91779320"
---
Zum Ausführen des Containers für die *Schlüsselbegriffserkennung* führen Sie den folgenden `docker run`-Befehl aus. Ersetzen Sie die folgenden Platzhalter durch Ihre eigenen Werte:

| Platzhalter | Wert | Format oder Beispiel |
|-------------|-------|---|
| **{API_KEY}** | Der Schlüssel für Ihre Textanalyseressource. Sie finden ihn im Azure-Portal auf der Seite **Key and endpoint** (Schlüssel und Endpunkt) Ihrer Ressource. |`xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`|
| **{ENDPOINT_URI}** | Der Endpunkt für den Zugriff auf die Textanalyse-API. Sie finden ihn im Azure-Portal auf der Seite **Key and endpoint** (Schlüssel und Endpunkt) Ihrer Ressource. | `https://<your-custom-subdomain>.cognitiveservices.azure.com` |


```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
mcr.microsoft.com/azure-cognitive-services/textanalytics/keyphrase \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Dieser Befehl:

* Führt einen *Schlüsselbegriffserkennungs*-Container auf der Grundlage des Containerimages aus.
* Weist einen einzelnen CPU-Kern und 4 GB Arbeitsspeicher zu
* Verfügbarmachen des TCP-Ports 5000 und Zuweisen einer Pseudo-TTY-Verbindung für den Container
* Entfernt den Container automatisch, nachdem er beendet wurde. Das Containerimage ist auf dem Hostcomputer weiterhin verfügbar.