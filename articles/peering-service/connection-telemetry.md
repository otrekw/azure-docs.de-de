---
title: Azure Peering Service-Verbindungstelemetrie
description: Erfahren Sie etwas über die Verbindungstelemetrie für Microsoft Azure Peering Service.
services: peering-service
author: derekolo
ms.service: peering-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 05/18/2020
ms.author: derekol
ms.openlocfilehash: 0e54b20cc6aefa69226c24f557ae02166dc4b4b6
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "84871300"
---
# <a name="peering-service-connection-telemetry"></a>Peering Service-Verbindungstelemetrie

Die Verbindungstelemetrie bietet Einblicke, die für die Konnektivität zwischen dem Standort des Kunden und dem Microsoft-Netzwerk erfasst werden. Kunden können Telemetrie für die Azure Peering Service-Verbindung abrufen, indem sie die Verbindung im Azure-Portal registrieren. Dieses Feature bietet Präfixsicherheit und Erkenntnisse zur Netzwerkleistung.


Die Verbindungstelemetrie setzt sich aus den folgenden Bereichen zusammen:

### <a name="latency-measurement"></a>Latenzmessung

 Die Latenz wird vom Client zum Microsoft Edge-PoP für die registrierten Präfixe in Peering Service gemessen.

### <a name="route-prefix-monitoring-and-protection"></a>Überwachung und Schutz des Routenpräfix

Routingpfade werden auf verdächtige Aktivitäten überwacht, die dann in Ereignisprotokollen aufgezeichnet werden. Beispielsweise werden Ereignisprotokolle für einige dieser Faktoren erstellt:

- Präfixhijacking
- Präfixwiderruf
- Routenleaks

## <a name="next-steps"></a>Nächste Schritte

- Informationen zur Peering Service-Verbindung finden Sie unter [Peering Service-Verbindung](connection.md).
- Informationen zum Onboarding einer Peering Service-Verbindung finden Sie unter [Onboarding eines Peering Service-Modells](onboarding-model.md).
- Informationen zum Messen der Telemetrie finden Sie unter [Messen der Verbindungstelemetrie](measure-connection-telemetry.md).
