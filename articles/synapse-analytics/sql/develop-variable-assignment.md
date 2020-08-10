---
title: Zuweisen von Variablen mit Synapse SQL
description: In diesem Artikel finden Sie Tipps zur Zuweisung von T-SQL-Variablen mit Synapse SQL.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick
ms.openlocfilehash: c3bd04d037f7852bd7d9ea51719abf27f09093b0
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2020
ms.locfileid: "87495674"
---
# <a name="assign-variables-with-synapse-sql"></a>Zuweisen von Variablen mit Synapse SQL

In diesem Artikel finden Sie Tipps zur Zuweisung von T-SQL-Variablen mit Synapse SQL.

## <a name="set-variables-with-declare"></a>Festlegen von Variablen mit DECLARE

Variablen werden in Synapse SQL mit der `DECLARE`-Anweisung oder der `SET`-Anweisung festgelegt. Das Initialisieren von Variablen mit DECLARE ist eine der flexibelsten Möglichkeiten zum Festlegen eines Variablenwerts in Synapse SQL.

```sql
DECLARE @v  int = 0
;
```

Sie können mit DECLARE auch mehrere Variablen gleichzeitig festlegen. Folgende Vorgänge können nicht mithilfe von SELECT oder UPDATE ausgeführt werden:

```sql
DECLARE @v  INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Smith')
,       @v1 INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Jones')
;
```

Eine Variable kann nicht in der gleichen DECLARE-Anweisung initialisiert und verwendet werden. Zur Veranschaulichung: Das folgende Beispiel ist nicht zulässig, da *\@p1* in der gleichen DECLARE-Anweisung sowohl initialisiert als auch verwendet wird. Das folgende Beispiel zeigt einen Fehler.

```sql
DECLARE @p1 int = 0
,       @p2 int = (SELECT COUNT (*) FROM sys.types where is_user_defined = @p1 )
;
```

## <a name="set-values-with-set"></a>Festlegen von Werten mit SET

SET ist eine allgemeine Methode zum Festlegen einer einzelnen Variablen.

Die folgenden Anweisungen sind alle gültige Möglichkeiten zum Festlegen einer Variablen mit SET:

```sql
SET     @v = (Select max(database_id) from sys.databases);
SET     @v = 1;
SET     @v = @v+1;
SET     @v +=1;
```

Sie können mit SET nur jeweils eine einzige Variable festlegen. Allerdings sind zusammengesetzte Operatoren zulässig.

## <a name="limitations"></a>Einschränkungen

Sie können UPDATE nicht für die Zuweisung von Variablen verwenden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Tipps zur Entwicklung finden Sie im Artikel [Entwicklungsübersicht für Synapse SQL](develop-overview.md).
