---
title: 'Azure Application Insights-Telemetriedatenmodell: Ereignistelemetrie | Microsoft-Dokumentation'
description: Application Insights-Datenmodell für Ereignistelemetrie
ms.topic: conceptual
ms.date: 04/25/2017
ms.reviewer: sergkanz
ms.openlocfilehash: 69685afa14352a22b58bccbea342038e4273696e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87320611"
---
# <a name="event-telemetry-application-insights-data-model"></a>Ereignistelemetrie: Application Insights-Datenmodell

Sie können Ereignistelemetrieelemente erstellen (in [Application Insights](./app-insights-overview.md)), um ein Ereignis darzustellen, das in Ihrer Anwendung aufgetreten ist. In der Regel handelt es sich um eine Benutzerinteraktion wie ein Klicken auf eine Schaltfläche oder das Zahlen eines Warenkorbs. Es kann sich auch um ein Ereignis im Lebenszyklus einer Anwendung handeln, wie z.B. eine Initialisierung oder Konfigurationsaktualisierung. 

Semantisch können Ereignisse nun mit Anforderungen in Korrelation stehen. Bei ordnungsgemäßer Verwendung ist Ereignistelemetrie jedoch wichtiger als Anforderungen oder Ablaufverfolgungen. Ereignisse stellen Geschäftstelemetrie dar, für die eine getrennte, weniger aggressive [Stichprobennahme](./api-filtering-sampling.md) erfolgen muss.

## <a name="name"></a>Name

Ereignisname. Um eine ordnungsgemäße Gruppierung und nützliche Metriken zu ermöglichen, schränken Sie Ihre Anwendung so ein, dass sie eine geringe Anzahl von separaten Ereignisnamen generiert. Verwenden Sie z. B. nicht für jede generierte Instanz eines Ereignisses einen separaten Namen.

Maximale Länge: 512 Zeichen

## <a name="custom-properties"></a>Benutzerdefinierte Eigenschaften

[!INCLUDE [application-insights-data-model-properties](../../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Benutzerdefinierte Messungen

[!INCLUDE [application-insights-data-model-measurements](../../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a>Nächste Schritte

- Lesen Sie die Informationen zu den Application Insights-Typen und zum Datenmodell unter [Datenmodell](data-model.md).
- [Schreiben benutzerdefinierter Ereignistelemetriedaten](./api-custom-events-metrics.md#trackevent)
- Lesen Sie die Informationen zu den von Application Insights unterstützten [Plattformen](./platforms.md).

