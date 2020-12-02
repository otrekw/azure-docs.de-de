---
title: 'Ausführen von Azure Container Instances: Textanalyse'
titleSuffix: Azure Cognitive Services
description: Stellen Sie die Textanalysecontainer in Azure Container Instances bereit, und testen Sie sie in einem Webbrowser.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 07/07/2020
ms.author: aahi
ms.openlocfilehash: be43d04672dcefe368eb4052b4d1a929e25327ab
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/25/2020
ms.locfileid: "96009919"
---
# <a name="deploy-a-text-analytics-container-to-azure-container-instances"></a>Bereitstellen eines Textanalysecontainers in Azure Container Instances

Hier erfahren Sie, wie Sie die Cognitive Services-Container für die [Textanalyse][install-and-run-containers] in [Azure Container Instances][container-instances] bereitstellen. Dieses Verfahren veranschaulicht die Erstellung einer Textanalyseressource, die Erstellung eines zugehörigen Standpunktanalysebilds und die Möglichkeit, diese Orchestrierung der beiden über einen Browser durchzuführen. Die Verwendung von Containern kann die Aufmerksamkeit der Entwickler von der Verwaltung der Infrastruktur auf die Anwendungsentwicklung lenken.

## <a name="prerequisites"></a>Voraussetzungen

* Verwenden Sie ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/cognitive-services) erstellen, bevor Sie beginnen.

[!INCLUDE [Create a Cognitive Services Text Analytics resource](../includes/create-text-analytics-resource.md)]

[!INCLUDE [Create a Text Analytics Containers on Azure Container Instances](../../containers/includes/create-container-instances-resource.md)]

#### <a name="key-phrase-extraction"></a>[Schlüsselbegriffserkennung](#tab/keyphrase)

[!INCLUDE [Verify the Key Phrase Extraction container instance](../includes/verify-key-phrase-extraction-container.md)]

#### <a name="language-detection"></a>[Sprachenerkennung](#tab/language)

[!INCLUDE [Verify the Language Detection container instance](../includes/verify-language-detection-container.md)]

#### <a name="sentiment-analysis"></a>[Standpunktanalyse](#tab/sentiment)

[!INCLUDE [Verify the Sentiment Analysis container instance](../includes/verify-sentiment-analysis-container.md)]

#### <a name="text-analytics-for-health"></a>[Text Analytics for Health](#tab/health)

[!INCLUDE [Verify the health container instance](../includes/verify-health-container.md)]

***

## <a name="next-steps"></a>Nächste Schritte 

* Verwenden weiterer [Cognitive Services-Container](../../cognitive-services-container-support.md)
* Verwenden des [Text Analytics Connected Service](../index.yml)

[install-and-run-containers]: ./text-analytics-how-to-install-containers.md
[container-instances]: https://docs.microsoft.com/azure/container-instances