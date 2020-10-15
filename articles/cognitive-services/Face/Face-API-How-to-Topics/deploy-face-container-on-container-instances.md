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
ms.date: 07/16/2020
ms.author: aahi
ms.openlocfilehash: 5fbe316580cfa2ca280a2587536df037146e8d02
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "86538121"
---
# <a name="deploy-the-face-container-to-azure-container-instances"></a>Bereitstellen eines Containers für die Gesichtserkennung in Azure Container Instances

> [!IMPORTANT]
> Das Limit für Benutzer von Gesichtserkennungscontainern wurde erreicht. Wir akzeptieren derzeit keine neuen Anträge für den Gesichtserkennungscontainer.

Erfahren Sie, wie Sie Container für die [Gesichtserkennung](../face-how-to-install-containers.md) von Cognitive Services für Azure [Container Instances](https://docs.microsoft.com/azure/container-instances/) bereitstellen. Dieses Verfahren veranschaulicht die Erstellung der Azure-Ressource für die Gesichtserkennung. Anschließend wird das Abrufen des dazugehörigen Containerimages erläutert. Abschließend erklären wir, wie Sie die Orchestrierung von Ressource und Image über einen Browser ausführen können. Die Verwendung von Containern kann die Aufmerksamkeit der Entwickler von der Verwaltung der Infrastruktur auf die Anwendungsentwicklung lenken.

[!INCLUDE [Prerequisites](../../containers/includes/container-preview-prerequisites.md)]

[!INCLUDE [Create a Cognitive Services Face resource](../includes/create-face-resource.md)]

[!INCLUDE [Create an Face container on Azure Container Instances](../../containers/includes/create-container-instances-resource-from-azure-cli.md)]

[!INCLUDE [API documentation](../../../../includes/cognitive-services-containers-api-documentation.md)]

[!INCLUDE [Containers Next Steps](../../containers/includes/containers-next-steps.md)]