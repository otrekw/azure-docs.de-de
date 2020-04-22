---
title: T-SQL-Funktionsunterschiede in Azure Synapse SQL
description: Liste der Transact-SQL-Funktionen, die in Synapse SQL verfügbar sind.
services: synapse analytics
author: jovanpop-msft
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: jovanpop
ms.reviewer: jrasnick
ms.openlocfilehash: b3e4d705bea7243966959038cf15373097c73ebc
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81421284"
---
# <a name="transact-sql-features-supported-in-azure-synapse-sql"></a>In Azure Synapse SQL unterstützte Transact-SQL-Funktionen

Azure Synapse SQL ist ein Big Data-Analysedienst, mit dem Sie Ihre Daten per T-SQL-Sprache abfragen und analysieren können. Sie können einen ANSI-kompatiblen Standarddialekt der SQL-Sprache verwenden, die in SQL Server und Azure SQL-Datenbank für die Datenanalyse verwendet wird. 

Die Transact-SQL-Sprache wird serverlos in Synapse SQL verwendet. Das bereitgestellte Modell kann auf verschiedene Objekte verweisen, und bei den unterstützten Funktionen gibt es gewisse Unterschiede. Auf dieser Seite finden Sie allgemeine Transact-SQL-Sprachunterschiede zwischen den Verbrauchsmodellen von Synapse SQL.

## <a name="database-objects"></a>Datenbankobjekte

Verbrauchsmodelle in Synapse SQL ermöglichen die Verwendung verschiedener Datenbankobjekte. Die folgende Tabelle gibt Aufschluss über die unterstützten Objekttypen:

|   | Bereitgestellt | Serverlos |
| --- | --- | --- |
| Tabellen | [Ja](/sql/t-sql/statements/create-table-azure-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | Nein. Im Rahmen des serverlosen Modells können nur externe Daten in [Azure Storage](#storage-options) abgefragt werden. |
| Sichten | [Ja](/sql/t-sql/statements/create-view-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest). Von Sichten können [Abfragesprachelemente](#query-language) verwendet werden, die im bereitgestellten Modell verfügbar sind. | [Ja](/sql/t-sql/statements/create-view-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest). Von Sichten können [Abfragesprachelemente](#query-language) verwendet werden, die im serverlosen Modell verfügbar sind. |
| Schemas | [Ja](/sql/t-sql/statements/create-schema-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | [Ja](/sql/t-sql/statements/create-schema-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |
| Temporäre Tabellen | [Ja](../sql-data-warehouse/sql-data-warehouse-tables-temporary.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) | Nein |
| Prozeduren | [Ja](/sql/t-sql/statements/create-procedure-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | Nein |
| Functions | [Ja](/sql/t-sql/statements/create-function-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | Nein |
| Trigger | Nein | Nein |
| Externe Tabellen | [Ja](/sql/t-sql/statements/create-external-table-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest). Weitere Informationen finden Sie in den unterstützten [Datenformaten](#data-formats). | [Ja](/sql/t-sql/statements/create-external-table-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest). Weitere Informationen finden Sie in den unterstützten [Datenformaten](#data-formats). |
| Zwischenspeichern von Abfragen | Ja, mehrere Varianten (SSD-basierte Zwischenspeicherung, In-Memory, Resultset-Zwischenspeicherung). Außerdem wird die materialisierte Sicht unterstützt. | Nein |
| Tabellenvariablen | [Nein](/sql/t-sql/data-types/table-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest). Verwenden Sie temporäre Tabellen. | Nein |

## <a name="query-language"></a>Abfragesprache

Die unterstützten Funktionen der in Synapse SQL verwendeten Abfragesprachen können sich abhängig vom Verbrauchsmodell unterscheiden. In der folgenden Tabelle sind die wichtigsten Unterschiede bei der Abfragesprache in Transact-SQL-Dialekten aufgeführt:

|   | Bereitgestellt | Serverlos |
| --- | --- | --- |
| SELECT-Anweisung | Ja. Die Transact-SQL-Abfrageklauseln [FOR XML/FOR JSON](/sql/t-sql/queries/select-for-clause-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [MATCH](/sql/t-sql/queries/match-sql-graph?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) und [PREDICT](/sql/t-sql/queries/predict-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) werden nicht unterstützt. | Ja. Die Transact-SQL-Abfrageklauseln [FOR XML](/sql/t-sql/queries/select-for-clause-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [MATCH](/sql/t-sql/queries/match-sql-graph?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [PREDICT](/sql/t-sql/queries/predict-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) und Abfragehinweise werden nicht unterstützt. [OFFSET/FETCH](/sql/t-sql/queries/select-order-by-clause-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest#using-offset-and-fetch-to-limit-the-rows-returned) und [PIVOT/UNPIVOT](/sql/t-sql/queries/from-using-pivot-and-unpivot?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) können verwendet werden, um nur Daten in temporären Tabellen (also keine externen Daten) abzufragen. |
| INSERT-Anweisung | Ja | Nein |
| UPDATE-Anweisung | Ja | Nein |
| DELETE-Anweisung | Ja | Nein |
| MERGE-Anweisung | Ja | Nein |
| Ladevorgänge für Daten | Ja. Das bevorzugte Hilfsprogramm ist zwar die [COPY](/sql/t-sql/statements/copy-into-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)-Anweisung, vom System werden jedoch auch Massenladen (BCP) sowie [CETAS](/sql/t-sql/statements/create-external-table-as-select-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) zum Laden von Daten unterstützt. | Nein |
| Datenexport | Ja. Unter Verwendung von [CETAS](/sql/t-sql/statements/create-external-table-as-select-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest). | Ja. Unter Verwendung von [CETAS](/sql/t-sql/statements/create-external-table-as-select-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest). |
| Typen | Ja. Alle Transact-SQL-Typen außer [cursor](/sql/t-sql/data-types/cursor-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [hierarchyid](/sql/t-sql/data-types/hierarchyid-data-type-method-reference?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [ntext, text und image](/sql/t-sql/data-types/ntext-text-and-image-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [rowversion](/sql/t-sql/data-types/rowversion-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [räumliche Typen](/sql/t-sql/spatial-geometry/spatial-types-geometry-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [sql\_variant](/sql/t-sql/data-types/sql-variant-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) und [xml](/sql/t-sql/xml/xml-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest). | Ja. Alle Transact-SQL-Typen außer [cursor](/sql/t-sql/data-types/cursor-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [hierarchyid](/sql/t-sql/data-types/hierarchyid-data-type-method-reference?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [ntext, text und image](/sql/t-sql/data-types/ntext-text-and-image-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [rowversion](/sql/t-sql/data-types/rowversion-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [räumliche Typen](/sql/t-sql/spatial-geometry/spatial-types-geometry-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [sql\_variant](/sql/t-sql/data-types/sql-variant-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [xml](/sql/t-sql/xml/xml-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) und Tabellentyp. |
| Datenbankübergreifende Abfragen | Nein | Ja, einschließlich [USE](/sql/t-sql/language-elements/use-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)-Anweisung. |
| Integrierte Funktionen (Analyse) | Ja. Alle Transact-SQL-Funktionen im Zusammenhang mit [Analyse](/sql/t-sql/functions/analytic-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), Konvertierung, [Datum und Uhrzeit](/sql/t-sql/functions/date-and-time-data-types-and-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), Logik und [Mathematik](/sql/t-sql/functions/mathematical-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) mit Ausnahme von [CHOOSE](/sql/t-sql/functions/logical-functions-choose-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [IIF](/sql/t-sql/functions/logical-functions-iif-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) und [PARSE](/sql/t-sql/functions/parse-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest). | Ja. Alle Transact-SQL-Funktionen im Zusammenhang mit [Analyse](/sql/t-sql/functions/analytic-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), Konvertierung, [Datum und Uhrzeit](/sql/t-sql/functions/date-and-time-data-types-and-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), Logik und [Mathematik](/sql/t-sql/functions/mathematical-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest). |
| Integrierte Funktionen (Text) | Ja. Alle Transact-SQL-Funktionen im Zusammenhang mit [Zeichenfolgen](/sql/t-sql/functions/string-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [JSON](/sql/t-sql/functions/json-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) und Sortierung mit Ausnahme von [STRING_ESCAPE](/sql/t-sql/functions/string-escape-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) und [TRANSLATE](/sql/t-sql/functions/translate-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest). | Ja. Alle Transact-SQL-Funktionen im Zusammenhang mit [Zeichenfolgen](/sql/t-sql/functions/string-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [JSON](/sql/t-sql/functions/json-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) und Sortierung. |
| Integrierte Tabellenwertfunktionen | Ja. [Transact-SQL-Rowsetfunktionen](/sql/t-sql/functions/functions?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest#rowset-functions) mit Ausnahme von [OPENXML](/sql/t-sql/functions/openxml-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [OPENDATASOURCE](/sql/t-sql/functions/opendatasource-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [OPENQUERY](/sql/t-sql/functions/openquery-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) und [OPENROWSET](/sql/t-sql/functions/openrowset-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest). | Ja. [Transact-SQL-Rowsetfunktionen](/sql/t-sql/functions/functions?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest#rowset-functions) mit Ausnahme von [OPENXML](/sql/t-sql/functions/openxml-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [OPENDATASOURCE](/sql/t-sql/functions/opendatasource-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) und [OPENQUERY](/sql/t-sql/functions/openquery-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest).  |
| Aggregate |  Integrierte Transact-SQL-Aggregate mit Ausnahme von [CHECKSUM_AGG](/sql/t-sql/functions/checksum-agg-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) und [GROUPING_ID](/sql/t-sql/functions/grouping-id-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest). | Integrierte Transact-SQL-Aggregate mit Ausnahme von [STRING\_AGG](/sql/t-sql/functions/string-agg-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest). |
| Operatoren | Ja. Alle [Transact-SQL-Operatoren](/sql/t-sql/language-elements/operators-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) mit Ausnahme von [!>](/sql/t-sql/language-elements/not-greater-than-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) und [!<](/sql/t-sql/language-elements/not-less-than-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest). | Ja. Alle [Transact-SQL-Operatoren](/sql/t-sql/language-elements/operators-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest).  |
| Ablaufsteuerung | Ja. Alle [Transact-SQL-Ablaufsteuerungsanweisungen](/sql/t-sql/language-elements/control-of-flow?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) mit Ausnahme von [CONTINUE](/sql/t-sql/language-elements/continue-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [GOTO](/sql/t-sql/language-elements/goto-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [RETURN](/sql/t-sql/language-elements/return-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [USE](/sql/t-sql/language-elements/use-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) und [WAITFOR](/sql/t-sql/language-elements/waitfor-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest). | Ja. Alle [Transact-SQL-Ablaufsteuerungsanweisungen](/sql/t-sql/language-elements/control-of-flow?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) mit Ausnahme von [RETURN](/sql/t-sql/language-elements/return-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) und SELECT-Abfrage in der Bedingung `WHILE (...)`. |
| DDL-Anweisungen (CREATE, ALTER, DROP) | Ja. Alle auf die unterstützten Objekttypen anwendbaren Transact-SQL-DDL-Anweisungen. | Ja. Alle auf die unterstützten Objekttypen anwendbaren Transact-SQL-DDL-Anweisungen. |

## <a name="security"></a>Sicherheit

Synapse SQL ermöglicht die Verwendung integrierter Sicherheitsfeatures, um Ihre Daten zu schützen und den Zugriff zu steuern. In der folgenden Tabelle werden allgemeine Unterschiede zwischen den Synapse-SQL-Verbrauchsmodellen gegenübergestellt:

|   | Bereitgestellt | Serverlos |
| --- | --- | --- |
| Anmeldungen | Nicht zutreffend (In Datenbanken werden nur enthaltene Benutzer unterstützt.) | Ja |
| Benutzer |  Nicht zutreffend (In Datenbanken werden nur enthaltene Benutzer unterstützt.) | Ja |
| [Eigenständige Benutzer](/sql/relational-databases/security/contained-database-users-making-your-database-portable?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | Ja. **Hinweis:** Es kann immer nur ein einzelner AAD-Benutzer uneingeschränkter Administrator sein. | Ja |
| Authentifizierung über Azure Active Directory (AAD) | Ja, AAD-Benutzer. | Ja, AAD-Anmeldungen und -Benutzer. |
| Passthrough-Authentifizierung für den Speicher | Ja | Ja |
| SAS-Tokenauthentifizierung für den Speicher | Nein | Ja, unter Verwendung von [DATABASE SCOPED CREDENTIAL](/sql/t-sql/statements/create-database-scoped-credential-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) in [EXTERNAL DATA SOURCE](/sql/t-sql/statements/create-external-data-source-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) oder [CREDENTIAL](/sql/t-sql/statements/create-credential-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) (auf Instanzebene). |
| Authentifizierung per Speicherzugriffsschlüssel | Ja, unter Verwendung von [DATABASE SCOPED CREDENTIAL](/sql/t-sql/statements/create-database-scoped-credential-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) in [EXTERNAL DATA SOURCE](/sql/t-sql/statements/create-external-data-source-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest). | Nein |
| Authentifizierung für den Speicher mittels verwalteter Identität | Ja, unter Verwendung von [Anmeldeinformationen für eine verwaltete Dienstidentität](../../sql-database/sql-database-vnet-service-endpoint-rule-overview.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest). | Ja, unter Verwendung von Anmeldeinformationen vom Typ `Managed Identity`. |
| Authentifizierung für den Speicher mittels Anwendungsidentität | [Ja](/sql/t-sql/statements/create-external-data-source-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | Nein |
| Berechtigungen: Objektebene | Ja, einschließlich der Möglichkeit zum Gewähren (GRANT), Verweigern (DENY) und Widerrufen (REVOKE) von Berechtigungen für Benutzer. | Ja, einschließlich der Möglichkeit zum Gewähren (GRANT), Verweigern (DENY) und Widerrufen (REVOKE) von Berechtigungen für Benutzer/Anmeldungen für die unterstützten Systemobjekte. |
| Berechtigungen: Schemaebene | Ja, einschließlich der Möglichkeit zum Gewähren (GRANT), Verweigern (DENY) und Widerrufen (REVOKE) von Berechtigungen für Benutzer/Anmeldungen für das Schema. | Ja, einschließlich der Möglichkeit zum Gewähren (GRANT), Verweigern (DENY) und Widerrufen (REVOKE) von Berechtigungen für Benutzer/Anmeldungen für das Schema. |
| Berechtigungen: [Datenbankebene](/sql/relational-databases/security/authentication-access/database-level-roles?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | Ja | Ja |
| Berechtigungen: [Serverebene](/sql/relational-databases/security/authentication-access/server-level-roles) | Nein | Ja. Systemadministrator und andere Serverrollen werden unterstützt. |
| Rollen/Gruppen | Ja, auf Datenbankebene. | Ja, auf Server- und Datenbankebene. |
| Sicherheits- und Identitätsfunktionen: | Einige Transact-SQL-Sicherheitsfunktionen und -operatoren: `CURRENT_USER`, `HAS_DBACCESS`, `IS_MEMBER`, `IS_ROLEMEMBER`, `SESSION_USER`, `SUSER_NAME`, `SUSER_SNAME`, `SYSTEM_USER`, `USER`, `USER_NAME`, `EXECUTE AS`, `OPEN/CLOSE MASTER KEY` | Einige Transact-SQL-Sicherheitsfunktionen und -operatoren: `CURRENT_USER`, `HAS_DBACCESS`, `HAS_PERMS_BY_NAME`, `IS_MEMBER', 'IS_ROLEMEMBER`, `IS_SRVROLEMEMBER`, `SESSION_USER`, `SUSER_NAME`, `SUSER_SNAME`, `SYSTEM_USER`, `USER`, `USER_NAME`, `EXECUTE AS` und `REVERT`. Sicherheitsfunktionen können nicht zum Abfragen externer Daten verwendet werden. (Speichern Sie das Ergebnis in einer Variablen, die in der Abfrage verwendet werden kann.)  |
| DATABASE SCOPED CREDENTIAL | Ja | Ja |

Von SQL-Pool- und SQL On-Demand-Instanzen wird die Transact-SQL-Standardsprache verwendet, um Daten abzufragen. Ausführliche Informationen zu den Unterschieden finden Sie in der [Transact-SQL-Sprachreferenz](/sql/t-sql/language-reference).

## <a name="tools"></a>Tools

Sie können verschiedene Tools verwenden, um eine Verbindung mit Synapse SQL herzustellen und Daten abzufragen.

|   | Bereitgestellt | Serverlos |
| --- | --- | --- |
| Synapse Studio | Ja, SQL-Skripts. | Ja, SQL-Skripts. |
| Power BI | Ja | [Ja](tutorial-connect-power-bi-desktop.md) |
| Azure Analysis Services | Ja | Ja |
| Azure Data Studio | Ja | Ja, ab Version 1.14. SQL-Skripts und -Notebooks werden unterstützt. |
| SQL Server Management Studio | Ja | Ja, ab Version 18.4. |

Die meisten Anwendungen verwenden die Transact-SQL-Standardsprache und können sowohl bereitgestellte als auch serverlose Verbrauchsmodelle von Synapse SQL abfragen.

## <a name="storage-options"></a>Speicheroptionen

Die zu analysierenden Daten können in verschiedenen Arten von Speicher gespeichert sein. In der folgenden Tabelle sind alle verfügbaren Speicheroptionen aufgeführt:

|   | Bereitgestellt | Serverlos |
| --- | --- | --- |
| Interner Speicher | Ja | Nein |
| Azure Data Lake v2 | Ja | Ja |
| Azure Blob Storage | Ja | Ja |

## <a name="data-formats"></a>Datenformate

Zu analysierende Daten können in verschiedenen Speicherformaten gespeichert sein. In der folgenden Tabelle sind alle für die Analyse verfügbaren Datenformate aufgeführt:

|   | Bereitgestellt | Serverlos |
| --- | --- | --- |
| Durch Trennzeichen getrennt | [Ja](/sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | [Ja](query-single-csv-file.md) |
| CSV | Ja. (Trennzeichen mit mehreren Zeichen werden nicht unterstützt.) | [Ja](query-single-csv-file.md) |
| Parquet | [Ja](/sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | [Ja](query-parquet-files.md), einschließlich Dateien mit [geschachtelten Typen](query-parquet-nested-types.md). |
| Hive ORC | [Ja](/sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | Nein |
| Hive RC | [Ja](/sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | Nein |
| JSON | Ja | [Ja](query-json-files.md) |
| [Delta Lake](https://delta.io/) | Nein | Nein |
| [CDM](https://docs.microsoft.com/common-data-model/) | Nein | Nein |

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu bewährten Methoden für SQL-Pool- und SQL On-Demand-Instanzen finden Sie in den folgenden Artikeln:

- [Bewährte Methoden für SQL-Pools in Azure Synapse Analytics](best-practices-sql-pool.md)
- [Bewährte Methoden für SQL On-Demand (Vorschauversion) in Azure Synapse Analytics](best-practices-sql-on-demand.md)