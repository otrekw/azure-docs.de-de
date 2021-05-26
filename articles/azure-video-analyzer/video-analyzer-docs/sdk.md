---
title: Azure Video Analyzer SDKs und Ressourcen
description: Hier erfahren Sie mehr über die Azure Video Analyzer SDKs.
author: bennage
ms.author: christb
ms.topic: reference
ms.date: 05/14/2021
ms.openlocfilehash: 480d3fbadbac6dcf7ec56f92e45e7c2e65653195
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110386077"
---
# <a name="azure-video-analyzer-sdks"></a>Azure Video Analyzer SDKs

Azure Video Analyzer umfasst zwei Gruppen von SDKs. Die Verwaltungs-SDKs werden für die Verwaltung der Azure-Ressource und die Client-SDKs für die Interaktion mit Edgemodulen verwendet.

## <a name="management-sdks"></a>Verwaltungs-SDKs

Mit den Verwaltungs-SDKs können Sie mit den Ressourcen interagieren, die von Azure Resource Manager zur Verfügung gestellt werden. Sie können beispielsweise ein Video Analyzer-Konto erstellen, Bereitstellungstoken für Edgemodule generieren und Zugriffsrichtlinien für Videos verwalten. Die SDKs bauen auf einer zugrunde liegenden [REST-API] auf.

Die folgenden Plattformen werden unterstützt:

- [.NET](https://aka.ms/ava/sdk/mgt/net)
- [Java](https://aka.ms/ava/sdk/mgt/java)
- [Python](https://aka.ms/ava/sdk/mgt/python)

## <a name="client-sdks"></a>Client-SDKs

Die Client-SDKs ermöglichen Ihnen, mit den [direkten Methoden][docs-direct-methods] eines Video Analyzer-Edgemoduls zu interagieren. Diese SDKs sind für die Verwendung mit den [Azure IoT Hub SDKs][docs-iot-hub-sdks] konzipiert. Sie unterstützen das Erstellen von Objekten, die die direkten Methoden darstellen, die dann mithilfe der IoT Hub SDKs an das Edgemodul gesendet werden können.

Die folgenden Plattformen werden unterstützt:

- [.NET](https://aka.ms/ava/sdk/client/net)
- [Python](https://aka.ms/ava/sdk/client/python)
- [Java](https://aka.ms/ava/sdk/client/java)

<!-- links -->
[docs-direct-methods]: direct-methods.md
[docs-iot-hub-sdks]: /azure/iot-hub/iot-hub-devguide-sdks

[REST-API]: https://aka.ms/ava/api/rest
