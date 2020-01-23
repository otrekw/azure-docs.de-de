---
title: Abfragen von Daten aus einer Vorschauumgebung mit C# – Azure Time Series Insights | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Daten mithilfe einer in C# geschriebenen App aus einer Time Series Insights-Umgebung abfragen.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 12/05/2019
ms.custom: seodec18
ms.openlocfilehash: a1d32bf7ea296ed2c4ed9351fcefe400c03effa5
ms.sourcegitcommit: 12a26f6682bfd1e264268b5d866547358728cd9a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/10/2020
ms.locfileid: "75861437"
---
# <a name="query-data-from-the-azure-time-series-insights-preview-environment-using-c"></a>Abfragen von Daten aus der Azure Time Series Insights Preview-Umgebung mit C#

Dieses C#-Beispiel veranschaulicht, wie Daten aus der Azure Time Series Insights Preview-Umgebung abgefragt werden können.

Das Beispiel zeigt einige einfache Beispiele für die Verwendung der Abfrage-API:

1. Rufen Sie zur Vorbereitung das Zugriffstoken mithilfe der Azure Active Directory-API ab. Übergeben Sie dieses Token im `Authorization`-Header jeder Abfrage-API-Anforderung. Informationen zum Einrichten nicht interaktiver Anwendungen finden Sie unter [Authentifizierung und Autorisierung](time-series-insights-authentication-and-authorization.md). Stellen Sie außerdem sicher, dass alle Konstanten, die zu Anfang des Beispiels definiert wurden, ordnungsgemäß festgelegt sind.
1. Die Liste der Umgebungen, auf die der Benutzer Zugriff hat, wird abgerufen. Eine der Umgebungen wird als relevant übernommen, und für diese Umgebung werden weitere Daten abgefragt.
1. Als Beispiel für eine HTTPS-Anforderung werden Verfügbarkeitsdaten für die relevante Umgebung abgefragt.
1. Hier finden Sie ein Beispiel für die Unterstützung der automatischen SDK-Generierung von [Azure AutoRest](https://github.com/Azure/AutoRest).

> [!NOTE]
> Den Beispielcode und die Schritte zum Kompilieren und Ausführen dieses Codes sind unter [https://github.com/Azure-Samples/Azure-Time-Series-Insights](https://github.com/Azure-Samples/Azure-Time-Series-Insights/tree/master/csharp-tsi-preview-sample) verfügbar.

## <a name="c-example"></a>C#-Beispiel

[!code-csharp[csharpquery-example](~/samples-tsi/csharp-tsi-preview-sample/DataPlaneClientSampleApp/Program.cs)]

> [!NOTE]
> Das obige Codebeispiel kann ausgeführt werden, ohne die Standardumgebungswerte zu ändern.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum Abfragen finden Sie in der [Abfrage-API-Referenz](https://docs.microsoft.com/rest/api/time-series-insights/preview-query).

- Lesen Sie die Informationen, wie Sie [eine JavaScript-App mithilfe des Client-SDKs ](https://github.com/microsoft/tsiclient) mit Time Series Insights verbinden.
