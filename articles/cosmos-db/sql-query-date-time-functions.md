---
title: Datums- und Uhrzeitfunktionen in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktionen für Datum und Uhrzeit in Azure Cosmos DB, um DateTime- und Zeitstempeloperationen durchzuführen.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/18/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 1ea838224f9d91b000100d5fa9308289619fd963
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88605186"
---
# <a name="date-and-time-functions-azure-cosmos-db"></a>Datums- und Uhrzeitfunktionen (Azure Cosmos DB)

Mit den Datums- und Uhrzeitfunktionen können Sie DateTime- und Zeitstempelvorgänge in Azure Cosmos DB ausführen.

## <a name="functions-to-obtain-the-date-and-time"></a>Funktionen zum Abrufen von Datum und Uhrzeit

Mit den folgenden skalaren Funktionen können das aktuelle UTC-Datum und die aktuelle UTC-Zeit in drei Formaten abgerufen werden: als Zeichenfolge, die dem ISO 8601-Format entspricht, als numerischer Zeitstempel, dessen Wert der Anzahl der seit der UNIX-Epoche verstrichenen Millisekunden entspricht, oder als Anzahl der Takte, die der Anzahl der 100-Nanosekunden-Takte seit der UNIX-Epoche entspricht:

* [GetCurrentDateTime](sql-query-getcurrentdatetime.md)
* [GetCurrentTimestamp](sql-query-getcurrenttimestamp.md)
* [GetCurrentTicks](sql-query-getcurrentticks.md)

## <a name="functions-to-work-with-datetime-values"></a>Funktionen für die Arbeit mit DateTime-Werten

Mithilfe der folgenden Funktionen können DateTime-Werte, Zeitstempelwerte und Taktwerte ganz einfach bearbeitet werden:

* [DateTimeAdd](sql-query-datetimeadd.md)
* [DateTimeDiff](sql-query-datetimediff.md)
* [DateTimeFromParts](sql-query-datetimefromparts.md)
* [DateTimePart](sql-query-datetimepart.md)
* [DateTimeToTicks](sql-query-datetimetoticks.md)
* [DateTimeToTimestamp](sql-query-datetimetotimestamp.md)
* [TicksToDateTime](sql-query-tickstodatetime.md)
* [TimestampToDateTime](sql-query-timestamptodatetime.md)

## <a name="next-steps"></a>Nächste Schritte

- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
- [Benutzerdefinierte Funktionen](sql-query-udfs.md)
- [Aggregate](sql-query-aggregates.md)
