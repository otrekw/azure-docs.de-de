---
title: Docker-Pullbefehl für den „Textanalyse für Gesundheit“-Container
titleSuffix: Azure Cognitive Services
description: Docker-Pullbefehl für „Textanalyse für Gesundheit“-Container
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 07/07/2020
ms.author: aahi
ms.openlocfilehash: 4c33952ed56399b94a7db1088cb60919a4f88137
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86108929"
---
Füllen Sie das [Formular zum Anfordern von Cognitive Services-Containern](https://aka.ms/cognitivegate) aus, und übermitteln Sie es, um Zugriff auf den Container anzufordern.

[!INCLUDE [Request access to the container registry](../../../../includes/cognitive-services-containers-request-access-only.md)]

Stellen Sie mit dem Docker-Anmeldebefehl mit Anmeldeinformationen, die in ihrer Onboarding-E-Mail bereitgestellt werden, eine Verbindung mit unserer privaten Containerregistrierung für Cognitive Services-Container her.

```bash
docker login containerpreview.azurecr.io -u <username> -p <password>
```

Laden Sie mit dem Befehl [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/) dieses Containerimage aus unserer privaten Containerregistrierung herunter.

```
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-healthcare:latest
```
