---
title: Vorteile der Multimasterunterstützung von Azure Cosmos DB
description: Erfahren Sie etwas über die Vorteile der Multimasterunterstützung in Azure Cosmos DB durch einen Vergleich der Latenz und der SLA-Anforderungen bei einem einzelnen und bei mehreren Schreibstandorten.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: mjbrown
ms.openlocfilehash: b21b6ba82ba1ada0103501b8beeca270df86abd9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "74872008"
---
# <a name="understand-multi-master-benefits-in-azure-cosmos-db"></a>Vorteile der Multimasterunterstützung in Azure Cosmos DB

Die Betreiber von Cosmos DB-Konten sollten eine geeignete globale Verteilungskonfiguration auswählen, um die Latenz, Verfügbarkeit und RTO-Anforderungen für ihre Anwendungen sicherzustellen. Azure Cosmos-Konten, die mit mehreren Schreibstandorten konfiguriert sind, bieten erhebliche Vorteile gegenüber Konten mit einem einzigen Schreibstandort, z.B. eine Schreibverfügbarkeits-SLA von 99,999 %, eine Schreiblatenz-SLA mit weniger als 10 ms im 99. Quantil und RTO = 0 bei einer regionalen Katastrophe.

## <a name="comparison-of-features"></a>Vergleich der Features

|Anwendungsanforderung|Mehrere Schreibstandorte|Einzelner Schreibstandort|Hinweis|
|---|---|---|---|
|Schreiblatenz-SLA < 10ms in Q99|**Ja**|Nein|Konten mit einem einzelnen Schreibstandort verursachen zusätzliche regionsübergreifende Netzwerklatenz für jeden Schreibvorgang.|
|Leselatenz-SLA < 10ms in Q99|**Ja**|Ja| |
|Schreib-SLA mit 99,999 %|**Ja**|Nein|Konten mit einem einzelnen Schreibstandort garantieren eine SLA von 99,99 %|
|RTO = 0|**Ja**|Nein|Keine Ausfallzeiten für Schreibvorgänge bei regionalen Katastrophen. Konten mit einem einzelnen Schreibstandort weisen einen RTO-Wert von 15 Minuten auf.|

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie EnableMultipleWriteLocations in Ihrem Azure Cosmos-Konto trotzdem deaktivieren möchten, [öffnen Sie ein Supportticket](https://azure.microsoft.com/support/create-ticket/).
