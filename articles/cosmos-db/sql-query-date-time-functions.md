---
title: Datums- und Uhrzeitfunktionen in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktionen für Datum und Uhrzeit in Azure Cosmos DB, um DateTime- und Zeitstempeloperationen durchzuführen.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/09/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: e3666f58b12855c19dd9b8ecf5519ab772c49743
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2020
ms.locfileid: "86246936"
---
# <a name="date-and-time-functions-azure-cosmos-db"></a>Datums- und Uhrzeitfunktionen (Azure Cosmos DB)

Mit den Datums- und Uhrzeitfunktionen können Sie DateTime- und Zeitstempelvorgänge in Azure Cosmos DB ausführen.

## <a name="functions-to-obtain-the-date-and-time"></a>Funktionen zum Abrufen von Datum und Uhrzeit

Mit den folgenden Skalarfunktionen können Sie das aktuelle UTC-Datum und die aktuelle UTC-Uhrzeit in zwei Formaten abrufen: als Zeichenfolge im ISO 8601-Format oder als numerischer Zeitstempel, dessen Wert der Unix-Epochenzeit in Millisekunden entspricht.

* [GetCurrentDateTime](sql-query-getcurrentdatetime.md)
* [GetCurrentTimestamp](sql-query-getcurrenttimestamp.md)

## <a name="functions-to-work-with-datetime-values"></a>Funktionen für die Arbeit mit DateTime-Werten

Mithilfe der folgenden Funktionen können DateTime-Werte ganz einfach bearbeitet werden:

* [DateTimeAdd](sql-query-datetimeadd.md)
* [DateTimeDiff](sql-query-datetimediff.md)
* [DateTimeFromParts](sql-query-datetimefromparts.md)

## <a name="next-steps"></a>Nächste Schritte

- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
- [Benutzerdefinierte Funktionen](sql-query-udfs.md)
- [Aggregate](sql-query-aggregates.md)
