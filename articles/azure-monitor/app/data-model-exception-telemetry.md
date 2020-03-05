---
title: Azure Application Insights-Datenmodell für Ausnahmentelemetrie
description: Application Insights-Datenmodell für Ausnahmentelemetrie
ms.topic: conceptual
ms.date: 04/25/2017
ms.reviewer: sergkanz
ms.openlocfilehash: fa9bc4efd9549cbcb1d50439989c5dea79d9d60c
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2020
ms.locfileid: "77671935"
---
# <a name="exception-telemetry-application-insights-data-model"></a>Telemetriedaten für Ausnahmen: Application Insights-Datenmodell

In [Application Insights](../../azure-monitor/app/app-insights-overview.md) stellt eine Instanz einer Ausnahme eine behandelte oder nicht behandelte Ausnahme dar, die während der Ausführung der überwachten Anwendung aufgetreten ist.

## <a name="problem-id"></a>Problem-ID

Bezeichner der Stelle, an der die Ausnahme im Code ausgelöst wurde. Dient zum Gruppieren von Ausnahmen. In der Regel eine Kombination von Ausnahmetyp und einer Funktion in der Aufrufliste.

Max. Länge: 1.024 Zeichen

## <a name="severity-level"></a>Schweregrad

Schweregrad der Ablaufverfolgung. Möglicher Wert: `Verbose`, `Information`, `Warning`, `Error`, `Critical`.

## <a name="exception-details"></a>Ausnahmedetails

(Erweiterung folgt)

## <a name="custom-properties"></a>Benutzerdefinierte Eigenschaften

[!INCLUDE [application-insights-data-model-properties](../../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Benutzerdefinierte Messungen

[!INCLUDE [application-insights-data-model-measurements](../../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a>Nächste Schritte

- Lesen Sie die Informationen zu den Application Insights-Typen und zum Datenmodell unter [Datenmodell](data-model.md).
- Erfahren Sie mehr zum [Diagnostizieren von Ausnahmen in Ihren Web-Apps mit Application Insights](../../azure-monitor/app/asp-net-exceptions.md).
- Lesen Sie die Informationen zu den von Application Insights unterstützten [Plattformen](../../azure-monitor/app/platforms.md).
