---
title: Unterstützung für Sortierungen
description: In Azure Synapse SQL unterstützte Sortierungstypen
author: filippopovic
ms.service: synapse-analytics
ms.topic: reference
ms.subservice: sql
ms.date: 04/15/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.openlocfilehash: 5e46cd744be609adff764edfe5a506b710e9d788
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91288068"
---
# <a name="database-collation-support-for-synapse-sql"></a>Unterstützung der Datenbanksortierung für Synapse SQL

Sortierungen geben das Gebietsschema, die Codepage, die Sortierreihenfolge und Regeln für die Groß-/Kleinschreibung von Zeichen für zeichenbasierte Datentypen an. Nach dem Auswählen erben alle Spalten und Ausdrücke, für die Sortierungsinformationen erforderlich sind, die gewählte Sortierung aus der Datenbankeinstellung. Die Standardvererbung kann durch explizites Angeben einer anderen Sortierung für einen zeichenbasierten Datentyp überschrieben werden.

Sie können die standardmäßige Datenbanksortierung im Azure-Portal ändern, wenn Sie eine neue SQL-Pool-Datenbank erstellen. Diese Funktion macht das Erstellen einer neuen Datenbank mit einer der 3.800 unterstützten Datenbanksortierungen noch einfacher.

Mit der CREATE DATABASE-Anweisung können Sie die standardmäßige Datenbanksortierung von Synapse SQL On-Demand zur Erstellungszeit angeben.

## <a name="change-collation"></a>Ändern der Sortierung
Aktualisieren Sie zum Ändern der Standardsortierung für die SQL-Pooldatenbank das Feld „Sortierung“ auf der Bereitstellungsoberfläche. Beispiel: Wenn Sie die Standardsortierung so ändern möchten, dass die Groß-/Kleinschreibung beachtet wird, ändern Sie den Namen „SQL_Latin1_General_CP1_CI_AS“ der Sortierung in „SQL_Latin1_General_CP1_CS_AS“. 

Um die Standardsortierung für eine SQL On-Demand-Datenbank zu ändern, können Sie die ALTER DATABASE-Anweisung verwenden.

## <a name="list-of-unsupported-collation-types"></a>Liste mit nicht unterstützten Sortierungstypen
*    Japanese_Bushu_Kakusu_140_BIN
*    Japanese_Bushu_Kakusu_140_BIN2
*    Japanese_Bushu_Kakusu_140_CI_AI_VSS
*    Japanese_Bushu_Kakusu_140_CI_AI_WS_VSS
*    Japanese_Bushu_Kakusu_140_CI_AI_KS_VSS
*    Japanese_Bushu_Kakusu_140_CI_AI_KS_WS_VSS
*    Japanese_Bushu_Kakusu_140_CI_AS_VSS
*    Japanese_Bushu_Kakusu_140_CI_AS_WS_VSS
*    Japanese_Bushu_Kakusu_140_CI_AS_KS_VSS
*    Japanese_Bushu_Kakusu_140_CI_AS_KS_WS_VSS
*    Japanese_Bushu_Kakusu_140_CS_AI_VSS
*    Japanese_Bushu_Kakusu_140_CS_AI_WS_VSS
*    Japanese_Bushu_Kakusu_140_CS_AI_KS_VSS
*    Japanese_Bushu_Kakusu_140_CS_AI_KS_WS_VSS
*    Japanese_Bushu_Kakusu_140_CS_AS_VSS
*    Japanese_Bushu_Kakusu_140_CS_AS_WS_VSS
*    Japanese_Bushu_Kakusu_140_CS_AS_KS_VSS
*    Japanese_Bushu_Kakusu_140_CS_AS_KS_WS_VSS
*    Japanese_Bushu_Kakusu_140_CI_AI
*    Japanese_Bushu_Kakusu_140_CI_AI_WS
*    Japanese_Bushu_Kakusu_140_CI_AI_KS
*    Japanese_Bushu_Kakusu_140_CI_AI_KS_WS
*    Japanese_Bushu_Kakusu_140_CI_AS
*    Japanese_Bushu_Kakusu_140_CI_AS_WS
*    Japanese_Bushu_Kakusu_140_CI_AS_KS
*    Japanese_Bushu_Kakusu_140_CI_AS_KS_WS
*    Japanese_Bushu_Kakusu_140_CS_AI
*    Japanese_Bushu_Kakusu_140_CS_AI_WS
*    Japanese_Bushu_Kakusu_140_CS_AI_KS
*    Japanese_Bushu_Kakusu_140_CS_AI_KS_WS
*    Japanese_Bushu_Kakusu_140_CS_AS
*    Japanese_Bushu_Kakusu_140_CS_AS_WS
*    Japanese_Bushu_Kakusu_140_CS_AS_KS
*    Japanese_Bushu_Kakusu_140_CS_AS_KS_WS
*    Japanese_XJIS_140_BIN
*    Japanese_XJIS_140_BIN2
*    Japanese_XJIS_140_CI_AI_VSS
*    Japanese_XJIS_140_CI_AI_WS_VSS
*    Japanese_XJIS_140_CI_AI_KS_VSS
*    Japanese_XJIS_140_CI_AI_KS_WS_VSS
*    Japanese_XJIS_140_CI_AS_VSS
*    Japanese_XJIS_140_CI_AS_WS_VSS
*    Japanese_XJIS_140_CI_AS_KS_VSS
*    Japanese_XJIS_140_CI_AS_KS_WS_VSS
*    Japanese_XJIS_140_CS_AI_VSS
*    Japanese_XJIS_140_CS_AI_WS_VSS
*    Japanese_XJIS_140_CS_AI_KS_VSS
*    Japanese_XJIS_140_CS_AI_KS_WS_VSS
*    Japanese_XJIS_140_CS_AS_VSS
*    Japanese_XJIS_140_CS_AS_WS_VSS
*    Japanese_XJIS_140_CS_AS_KS_VSS
*    Japanese_XJIS_140_CS_AS_KS_WS_VSS
*    Japanese_XJIS_140_CI_AI
*    Japanese_XJIS_140_CI_AI_WS
*    Japanese_XJIS_140_CI_AI_KS
*    Japanese_XJIS_140_CI_AI_KS_WS
*    Japanese_XJIS_140_CI_AS
*    Japanese_XJIS_140_CI_AS_WS
*    Japanese_XJIS_140_CI_AS_KS
*    Japanese_XJIS_140_CI_AS_KS_WS
*    Japanese_XJIS_140_CS_AI
*    Japanese_XJIS_140_CS_AI_WS
*    Japanese_XJIS_140_CS_AI_KS
*    Japanese_XJIS_140_CS_AI_KS_WS
*    Japanese_XJIS_140_CS_AS
*    Japanese_XJIS_140_CS_AS_WS
*    Japanese_XJIS_140_CS_AS_KS
*    Japanese_XJIS_140_CS_AS_KS_WS

Außerdem unterstützt der SQL-Pool die folgenden Sortierungstypen nicht:

*    SQL_EBCDIC1141_CP1_CS_AS
*    SQL_EBCDIC277_2_CP1_CS_AS
*    UTF-8

## <a name="check-the-current-collation"></a>Überprüfen der aktuellen Sortierung
Führen Sie den folgenden T-SQL-Codeausschnitt aus, um die aktuelle Sortierung für die Datenbank zu überprüfen:
```sql
SELECT DATABASEPROPERTYEX(DB_NAME(), 'Collation') AS Collation;
```
Wenn „Collation“ als Eigenschaftenparameter übergeben wird, gibt die Funktion „DatabasePropertyEx“ die aktuelle Sortierung für die angegebene Datenbank zurück. Weitere Informationen zur Funktion „DatabasePropertyEx“ finden Sie auf MSDN.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu bewährten Methoden für SQL-Pool- und SQL On-Demand-Instanzen finden Sie in den folgenden Artikeln:

- [Bewährte Methoden für SQL-Pools in Azure Synapse Analytics](best-practices-sql-pool.md)
- [Bewährte Methoden für SQL On-Demand (Vorschauversion) in Azure Synapse Analytics](best-practices-sql-on-demand.md)


