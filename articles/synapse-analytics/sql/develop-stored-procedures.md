---
title: Verwenden von gespeicherten Prozeduren
description: Tipps zum Implementieren von gespeicherten Prozeduren im Synapse SQL-Pool (Data Warehouse) für die Entwicklung von Lösungen
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql
ms.date: 04/15/2020
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: 294652a42d3b6a2468f024ce7ebdbdfc3615f9e1
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2020
ms.locfileid: "89647871"
---
# <a name="use-stored-procedures-in-sql-pool"></a>Verwenden gespeicherter Prozeduren im SQL-Pool

Tipps zum Implementieren von gespeicherten Prozeduren im Synapse SQL-Pool (Data Warehouse) für die Entwicklung von Lösungen

## <a name="what-to-expect"></a>Ausblick

Der SQL-Pool unterstützt viele der T-SQL-Features, die in SQL Server verwendet werden. Darüber hinaus sind bestimmte Features für das horizontale Hochskalieren vorhanden, mit denen Sie die Leistung Ihrer Lösung verbessern können.

In Bezug auf die Verwaltung der Skalierung und Leistung des SQL-Pools sind auch einige Features und Funktionen vorhanden, die Unterschiede beim Verhalten aufweisen. Es gibt auch Features, die nicht unterstützt werden.

## <a name="stored-procedures-in-sql-pool"></a>Gespeicherte Prozeduren im SQL-Pool

Gespeicherte Prozeduren sind eine hervorragende Möglichkeit zum Einschließen (Kapseln) von SQL-Code. Hierbei wird Code im Data Warehouse in der Nähe Ihrer Daten gespeichert. Gespeicherte Prozeduren unterstützen Entwickler beim Modularisieren ihrer Lösungen, indem Code in besser verwaltbare Einheiten eingeschlossen wird. Dies ermöglicht eine bessere Wiederverwendbarkeit des Codes. Für jede gespeicherte Prozedur können außerdem Parameter verwendet werden, um sie flexibler zu machen.

Der SQL-Pool bietet eine vereinfachte und optimierte Implementierung von gespeicherten Prozeduren. Der größte Unterschied im Vergleich zu SQL Server ist, dass die gespeicherte Prozedur kein vorab kompilierter Code ist. In Data Warehouses ist die Kompilierzeit im Vergleich zu der benötigten Zeit, um Abfragen für große Datenmengen auszuführen, relativ gering. Es ist wichtiger sicherzustellen, dass der Code der gespeicherten Prozedur für große Abfragen ordnungsgemäß optimiert ist. Das Ziel besteht darin, Stunden, Minuten und Sekunden zu sparen, keine Millisekunden. Es ist daher hilfreicher, gespeicherte Prozeduren als Container für SQL-Logik zu betrachten.

Wenn der SQL-Pool Ihre gespeicherte Prozedur ausführt, werden die SQL-Anweisungen zur Laufzeit analysiert, übersetzt und optimiert. Während dieses Vorgangs wird jede Anweisung in verteilte Abfragen konvertiert. Der SQL-Code, der für die Daten ausgeführt wird, unterscheidet sich von der übermittelten Abfrage.

## <a name="nesting-stored-procedures"></a>Schachteln von gespeicherten Prozeduren

Wenn gespeicherte Prozeduren andere gespeicherte Prozeduren aufrufen oder dynamischen SQL-Code ausführen, wird die innere gespeicherte Prozedur bzw. der Codeaufruf als „geschachtelt“ bezeichnet.

Der SQL-Pool unterstützt maximal acht Schachtelungsebenen. Dies ist ein Unterschied zu SQL Server. In SQL Server sind 32 Schachtelungsebenen zulässig.

Der Aufruf der obersten gespeicherten Prozedur entspricht Schachtelungsebene 1.

```sql
EXEC prc_nesting
```

Wenn die gespeicherte Prozedur auch einen weiteren EXEC-Aufruf durchführt, wird die Schachtelungsebene auf 2 erhöht.

```sql
CREATE PROCEDURE prc_nesting
AS
EXEC prc_nesting_2  -- This call is nest level 2
GO
EXEC prc_nesting
```

Wenn die zweite Prozedur dann dynamischen SQL-Code ausführt, wird die Schachtelungsebene auf 3 erhöht.

```sql
CREATE PROCEDURE prc_nesting_2
AS
EXEC sp_executesql N'SELECT ''another nest level'''  -- This call is nest level 2
GO
EXEC prc_nesting
```

> [!NOTE]
> Der SQL-Pool unterstützt derzeit kein [@@NESTLEVEL](/sql/t-sql/functions/nestlevel-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest). Sie müssen die Schachtelungsebene nachverfolgen. Es ist unwahrscheinlich, dass Sie die Grenze von acht Schachtelungsebenen überschreiten. Sollte dies dennoch der Fall sein, müssen Sie Ihren Code so überarbeiten, damit dieser innerhalb des Grenzwerts bleibt.

## <a name="insertexecute"></a>INSERT..EXECUTE

Der SQL-Pool lässt nicht zu, dass Sie das Resultset einer gespeicherten Prozedur mit einer INSERT-Anweisung verwenden. Es gibt aber eine andere Möglichkeit. Ein Beispiel finden Sie im Artikel unter [Temporäre Tabellen](develop-tables-temporary.md).

## <a name="limitations"></a>Einschränkungen

Es gibt einige Aspekte von gespeicherten Transact-SQL-Prozeduren, die im SQL-Pool nicht implementiert sind.

Sie lauten wie folgt:

* Temporäre gespeicherte Prozeduren
* Nummerierte gespeicherte Prozeduren
* Erweiterte gespeicherte Prozeduren
* Gespeicherte CLR-Prozeduren
* Verschlüsselungsoption
* Replikationsoption
* Tabellenwertparameter
* Schreibgeschützte Parameter
* Standardparameter
* Ausführungskontexte
* return-Anweisung

## <a name="next-steps"></a>Nächste Schritte

Weitere Hinweise zur Entwicklung finden Sie in der [Entwicklungsübersicht](develop-overview.md).
