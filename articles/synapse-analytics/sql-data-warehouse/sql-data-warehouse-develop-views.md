---
title: Verwenden von T-SQL-Sichten
description: Tipps für das Verwenden von T-SQL-Sichten und das Entwickeln von Lösungen im Synapse SQL-Pool.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 76442368fe4b3e498f622a8a3cd5b5b973f16bd6
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2020
ms.locfileid: "80633390"
---
# <a name="views-in-synapse-sql-pool"></a>Sichten im Synapse SQL-Pool

Sichten können auf verschiedene Weisen zur Verbesserung der Qualität Ihrer Lösung verwendet werden.

Der SQL-Pool unterstützt sowohl Standardsichten als auch materialisierte Sichten. Beides sind virtuelle Tabellen, die mit SELECT-Ausdrücken erstellt und Abfragen als logische Tabellen präsentiert werden.

Sichten kapseln die Komplexität von allgemeiner Datenberechnung und fügen eine Abstraktionsebene zur Berechnung von Änderungen hinzu, sodass Abfragen nicht erneut generiert werden müssen.

## <a name="standard-view"></a>Standardsicht

Eine Standardsicht berechnet die zugehörigen Daten jedes Mal, wenn sie verwendet wird. Auf dem Datenträger sind keine Daten gespeichert. Normalerweise verwenden Benutzer Standardsichten als Tool, das sie beim Organisieren logischer Objekte und Abfragen in einer Datenbank unterstützt.

Um eine Standardsicht verwenden zu können, muss eine Abfrage direkt darauf verweisen. Weitere Informationen finden Sie in der Dokumentation zu [CREATE VIEW](/sql/t-sql/statements/create-view-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest).

Sichten im SQL-Pool werden ausschließlich als Metadaten gespeichert. Daher sind die folgenden Optionen nicht verfügbar:

* Es gibt keine Schemabindungsoption.
* Basistabellen können nicht über die Sicht aktualisiert werden.
* Es können keine Sichten für temporäre Tabellen erstellt werden.
* EXPAND/NOEXPAND-Hinweise werden nicht unterstützt.
* Es sind keine indizierten Sichten im SQL-Pool verfügbar.

Standardsichten können genutzt werden, um leistungsoptimierte Verknüpfungen zwischen Tabellen zu erzwingen. Beispielsweise kann eine Sicht einen redundanten Verteilungsschlüssel als Teil des Verknüpfungskriteriums enthalten, um die Datenverschiebung zu minimieren.

Ein weiterer Vorteil einer Sicht kann beispielsweise sein, eine bestimmte Abfrage oder einen Verknüpfungshinweis zu erzwingen. Durch den Einsatz von Sichten auf diese Weise wird sichergestellt, dass Verknüpfungen immer optimal durchgeführt werden, und Benutzer müssen sich nicht mehr das richtige Konstrukt für ihre Verknüpfungen merken.

## <a name="materialized-view"></a>Materialisierte Sicht

Eine materialisierte Sicht berechnet vorab, speichert und verwaltet die zugehörigen Daten im SQL-Pool wie eine Tabelle. Jedes Mal, wenn eine materialisierte Sicht verwendet wird, muss nichts neu berechnet werden.

Während die Daten in Basistabellen geladen werden, führt SQL-Pool synchron eine Aktualisierung der materialisierten Sichten aus.  Der Abfrageoptimierer verwendet automatisch bereitgestellte materialisierte Sichten, um die Abfrageleistung zu verbessern, auch wenn in der Abfrage nicht auf die Sichten verwiesen wird.  

Abfragen, die am meisten von materialisierten Sichten profitieren, sind komplexe Abfragen (normalerweise Abfragen mit Joins und Aggregationen) für große Tabellen, die ein kleines Resultset ergeben.  

Ausführliche Informationen zur Syntax für materialisierte Sichten und andere Anforderungen finden Sie unter [CREATE MATERIALIZED VIEW AS SELECT](/sql/t-sql/statements/create-materialized-view-as-select-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest).  

Eine Anleitung zur Abfrageoptimierung finden Sie unter [Leistungsoptimierung mit materialisierten Sichten](performance-tuning-materialized-views.md).

## <a name="example"></a>Beispiel

Ein häufig verwendetes Anwendungsmuster ist das erneute Erstellen von Tabellen mit CREATE TABLE AS SELECT (CTAS), gefolgt von einem Muster zur Objektumbenennung beim Laden von Daten.  

Im folgenden Beispiel werden einer Datumsdimension neue Datumsdatensätze hinzugefügt. Beachten Sie, wie eine neue Tabelle (DimDate_New) zuerst erstellt und dann umbenannt wird, um die ursprüngliche Version der Tabelle zu ersetzen.

```sql
CREATE TABLE dbo.DimDate_New
WITH (DISTRIBUTION = ROUND_ROBIN
, CLUSTERED INDEX (DateKey ASC)
)
AS
SELECT *
FROM   dbo.DimDate  AS prod
UNION ALL
SELECT *
FROM   dbo.DimDate_stg AS stg;

RENAME OBJECT DimDate TO DimDate_Old;
RENAME OBJECT DimDate_New TO DimDate;

```

Dieser Ansatz kann aber auch dazu führen, dass Tabellen in der Sicht eines Benutzers ein- und ausgeblendet und Fehlermeldungen der Art „Tabelle nicht vorhanden“ ausgegeben werden.

Sichten können verwendet werden, um eine konsistente Darstellungsschicht für Benutzer bereitzustellen, während die zugrunde liegenden Objekte umbenannt werden. Wenn Zugriff auf Daten über Sichten bereitgestellt wird, sind Benutzer nicht auf die Sichtbarkeit der zugrunde liegenden Tabellen angewiesen.

Diese Ebene bietet eine konsistente Benutzererfahrung und ermöglicht gleichzeitig die Weiterentwicklung des Datenmodells durch die Data Warehouse-Designer. Dank der Möglichkeit, die zugrunde liegenden Tabellen weiterentwickeln zu können, können Designer die Leistung beim Laden von Daten mit CTAS maximieren.

## <a name="next-steps"></a>Nächste Schritte

Weitere Tipps zur Entwicklung finden Sie in der [Entwicklungsübersicht für den SQL-Pool](sql-data-warehouse-overview-develop.md).
