---
title: Ausführen eines Containers für die Anomalieerkennung in Azure Container Instances
titleSuffix: Azure Cognitive Services
description: Stellen Sie den Container für die Anomalieerkennung in einer Azure Container Instances-Instanz bereit, und testen Sie diesen in einem Webbrowser.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: conceptual
ms.date: 01/23/2020
ms.author: dapine
ms.openlocfilehash: 2fba0a0d64502a30b6dfbc9f4f109bca65cca8b9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "76716347"
---
# <a name="deploy-an-anomaly-detector-container-to-azure-container-instances"></a>Bereitstellen eines Containers für die Anomalieerkennung in Azure Container Instances

Erfahren Sie, wie Sie Container für die [Anomalieerkennung](../anomaly-detector-container-howto.md) von Cognitive Services für Azure [Container Instances](https://docs.microsoft.com/azure/container-instances/) bereitstellen. Dieses Verfahren veranschaulicht die Erstellung der Ressource für die Anomalieerkennung. Anschließend wird das Abrufen des dazugehörigen Containerimages erläutert. Abschließend erklären wir, wie Sie die Orchestrierung von Ressource und Image über einen Browser ausführen können. Die Verwendung von Containern kann die Aufmerksamkeit der Entwickler von der Verwaltung der Infrastruktur auf die Anwendungsentwicklung lenken.

[!INCLUDE [Prerequisites](../../containers/includes/container-preview-prerequisites.md)]

## <a name="request-access-to-the-private-container-registry"></a>Anfordern des Zugriffs auf die private Containerregistrierung

Sie müssen zuerst das [Formular zum Anfordern von Containern für die Anomalieerkennung](https://aka.ms/adcontainer) ausfüllen und übermitteln, um Zugriff auf den Container anzufordern.

[!INCLUDE [Request access](../../../../includes/cognitive-services-containers-request-access-only.md)]

[!INCLUDE [Create a Cognitive Services Anomaly Detector resource](../includes/create-anomaly-detector-resource.md)]

[!INCLUDE [Create an Anomaly Detector container on Azure Container Instances](../../containers/includes/create-container-instances-resource-from-azure-cli.md)]

[!INCLUDE [API documentation](../../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Abrufen des Containerimages per Pull und Ausführen des Containers finden Sie unter [Konfigurieren von Containern für die Anomalieerkennung](../anomaly-detector-container-configuration.md).
* Konfigurationseinstellungen finden Sie unter [Konfigurieren von Containern](../anomaly-detector-container-configuration.md).
* [Erfahren Sie mehr über den Anomalieerkennungs-API-Dienst.](https://go.microsoft.com/fwlink/?linkid=2080698&clcid=0x409)
