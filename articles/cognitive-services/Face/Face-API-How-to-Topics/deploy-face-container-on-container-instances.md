---
title: Ausführen des Containers für die Gesichtserkennung in Azure Container Instances
titleSuffix: Azure Cognitive Services
description: Stellen Sie den Container für die Gesichtserkennung in einer Azure Container Instance bereit, und testen Sie diesen in einem Webbrowser.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: 4df5949e24e3fa59d37379c058a777c93dda2c44
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "80878362"
---
# <a name="deploy-the-face-container-to-azure-container-instances"></a>Bereitstellen eines Containers für die Gesichtserkennung in Azure Container Instances

Erfahren Sie, wie Sie Container für die [Gesichtserkennung](../face-how-to-install-containers.md) von Cognitive Services für Azure [Container Instances](https://docs.microsoft.com/azure/container-instances/) bereitstellen. Dieses Verfahren veranschaulicht die Erstellung der Azure-Ressource für die Gesichtserkennung. Anschließend wird das Abrufen des dazugehörigen Containerimages erläutert. Abschließend erklären wir, wie Sie die Orchestrierung von Ressource und Image über einen Browser ausführen können. Die Verwendung von Containern kann die Aufmerksamkeit der Entwickler von der Verwaltung der Infrastruktur auf die Anwendungsentwicklung lenken.

[!INCLUDE [Prerequisites](../../containers/includes/container-preview-prerequisites.md)]

## <a name="request-access-to-the-private-container-registry"></a>Anfordern des Zugriffs auf die private Containerregistrierung

[!INCLUDE [Request access to private container registry](../../../../includes/cognitive-services-containers-request-access.md)]

[!INCLUDE [Create a Cognitive Services Face resource](../includes/create-face-resource.md)]

[!INCLUDE [Create an Face container on Azure Container Instances](../../containers/includes/create-container-instances-resource-from-azure-cli.md)]

[!INCLUDE [API documentation](../../../../includes/cognitive-services-containers-api-documentation.md)]

[!INCLUDE [Containers Next Steps](../../containers/includes/containers-next-steps.md)]