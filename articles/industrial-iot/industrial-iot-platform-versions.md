---
title: Azure Industrial IoT-Plattformversionen
description: Dieser Artikel enthält eine Übersicht über die vorhandenen Versionen der Industrial IoT-Plattform und deren Unterstützung.
author: jehona-m
ms.author: jemorina
ms.service: industrial-iot
ms.topic: overview
ms.date: 03/08/2021
ms.openlocfilehash: 26d71a438638bbd681687a26c9ebdc1db8273ca7
ms.sourcegitcommit: 7d63ce88bfe8188b1ae70c3d006a29068d066287
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2021
ms.locfileid: "114467221"
---
# <a name="azure-industrial-iot-platform-v280-lts"></a>Azure Industrial IoT-Plattform v2.8.0 LTS

Wir freuen uns, die Veröffentlichung der LTS-Version 2.8.0 (Long-Term Support, langfristiger Support) bekannt zu geben. Wir entwickeln und veröffentlichen weiterhin Updates für unsere laufenden Projekte auf GitHub, bieten jedoch nun auch einen Branch an, der ab Juli 2021 nur kritische Fehlerbehebungen und Sicherheitsupdates enthalten wird. Kunden können sich auf einen längerfristigen Supportlebenszyklus für diese LTS-Builds verlassen. Dies ermöglicht die von unseren Kunden benötigte Stabilität und Sicherheit für die Planung über längere Zeiträume. Der LTS-Branch bietet Kunden die Garantie, dass sie von allen erforderlichen Sicherheitsfixes oder kritischen Fehlerbehebungen mit minimalen Auswirkungen auf ihre Bereitstellungen und Modulinteraktionen profitieren.  Gleichzeitig können Kunden auf die neuesten Updates im [Mainbranch](https://github.com/Azure/Industrial-IoT) zugreifen, um mit den neuesten Entwicklungen und der schnellsten Zykluszeit für Produktupdates Schritt zu halten. 

## <a name="version-history"></a>Versionsverlauf 

|Version      |type                   |Date         |Highlights                             |
|-------------|-----------------------|-------------|---------------------------------------|
|2.5.4        |Stable                 |März 2020   |IoT Hub-Schnittstelle mit direkter Methode, Steuerung über die Cloud ohne zusätzliche Microservices (eigenständiger Modus), OPC UA-Serverschnittstelle, verwendet den OPC-Stapel der OPC Foundation – [Versionshinweise](https://github.com/Azure/Industrial-IoT/releases/tag/2.5.4)|
|[2.7.206](https://github.com/Azure/Industrial-IoT/tree/release/2.7.206)      |Stable                 |Januar 2021 |Konfiguration über die REST-API (orchestrierter Modus), unterstützt das Telemetrieformat von Beispielen sowie PubSub – [Versionshinweise](https://github.com/Azure/Industrial-IoT/releases/tag/2.7.206)|
|[2.8.0](https://github.com/Azure/Industrial-IoT/tree/release/2.8)        |Langfristiger Support (Long-Term Support, LTS)|Juli 2021    |IoT Edge-Update auf 1.1 LTS, Protokollierung und Ablaufverfolgung im OPC-Stapel für bessere Diagnosen durch den OPC-Herausgeber, Sicherheitsfixes|

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Was ist Industrial IoT?](overview-what-is-industrial-iot.md)