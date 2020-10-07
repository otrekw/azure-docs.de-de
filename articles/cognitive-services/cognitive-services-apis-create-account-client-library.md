---
title: Erstellen einer Cognitive Services-Ressource per Clientbibliothek für die Azure-Verwaltung
titleSuffix: Azure Cognitive Services
description: Es wird beschrieben, wie Sie Azure Cognitive Services-Ressourcen mit der Clientbibliothek für die Azure-Verwaltung erstellen und verwalten.
services: cognitive-services
keywords: Cognitive Services, kognitive Intelligenz, kognitive Lösungen, KI-Dienste
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.topic: quickstart
ms.date: 09/14/2020
ms.author: pafarley
zone_pivot_groups: programming-languages-set-ten
ms.openlocfilehash: e8628d051db7f5066a81171567f6f7e54fb0ab97
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91262449"
---
# <a name="quickstart-create-a-cognitive-services-resource-using-the-azure-management-client-library"></a>Schnellstart: Erstellen einer Azure Cognitive Services-Ressource mit der Clientbibliothek für die Azure-Verwaltung

Verwenden Sie diese Schnellstartanleitung, um Azure Cognitive Services-Ressourcen mit der Clientbibliothek für die Azure-Verwaltung zu erstellen und zu verwalten.

Azure Cognitive Services sind cloudbasierte Dienste mit REST-APIs und Clientbibliothek-SDKs, die Entwickler bei der Erstellung intelligenter Anwendungen unterstützen, ohne dass direkte KI- oder Data Science-Fähigkeiten oder -Kenntnisse erforderlich sind. Mit Cognitive Services können Entwickler ganz einfach kognitive Features in ihre Anwendungen integrieren – mit kognitiven Lösungen, die sehen, hören, sprechen, verstehen und sogar schlussfolgern können.

Einzelne KI-Dienste werden durch [Azure-Ressourcen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal) repräsentiert, die Sie unter Ihrem Azure-Abonnement erstellen. Nach dem Erstellen einer Ressource können Sie die generierten Schlüssel und den Endpunkt zum Authentifizieren Ihrer Anwendungen verwenden.

::: zone pivot="programming-language-csharp"

[!INCLUDE [C# SDK quickstart](includes/quickstarts/management-csharp.md)]

::: zone-end

::: zone pivot="programming-language-java"

[!INCLUDE [Java SDK quickstart](includes/quickstarts/management-java.md)]

::: zone-end

::: zone pivot="programming-language-javascript"

[!INCLUDE [NodeJS SDK quickstart](includes/quickstarts/management-node.md)]

::: zone-end

::: zone pivot="programming-language-python"

[!INCLUDE [Python SDK quickstart](includes/quickstarts/management-python.md)]

::: zone-end
