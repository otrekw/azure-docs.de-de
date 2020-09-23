---
title: Verwenden von Abfragebezeichnungen in Synapse SQL
description: In diesem Artikel finden Sie wichtige Tipps zur Verwendung von Abfragebezeichnungen in Synapse SQL.
services: synapse-analytics
author: filippopovic
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql
ms.date: 04/15/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.custom: ''
ms.openlocfilehash: 596b18332fc490f72ad185699fc59bf4521eca69
ms.sourcegitcommit: 3fc3457b5a6d5773323237f6a06ccfb6955bfb2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2020
ms.locfileid: "90032179"
---
# <a name="use-query-labels-in-synapse-sql"></a>Verwenden von Abfragebezeichnungen in Synapse SQL
In diesem Artikel finden Sie wichtige Tipps zur Verwendung von Abfragebezeichnungen in Synapse SQL.

> [!NOTE]
> SQL On-Demand (Vorschauversion) unterstützt keine Bezeichnungsabfragen.

## <a name="what-are-query-labels"></a>Was sind Abfragebezeichnungen?
Für den SQL-Pool wird das Konzept der „Abfragebezeichnungen“ unterstützt. Bevor wir darauf näher eingehen, sehen wir uns ein Beispiel an:

```sql
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query Label')
;
```

Mit dieser letzten Zeile wird die Abfrage mit der Zeichenfolge „My Query Label“ versehen. Dieses Tag ist besonders hilfreich, da die Bezeichnung über die DMVs abgefragt werden kann. Abfragen für Bezeichnungen stellen einen Mechanismus dar, um fehlerhafte Abfragen zu suchen und den Fortschritt über eine ELT-Ausführung zu ermitteln.

Gute Namenskonventionen sind sehr hilfreich. Beispielsweise ist eine Bezeichnung, die mit „PROJEKT“, „PROZEDUR“, „ANWEISUNG“ oder „KOMMENTAR“ beginnt, hilfreich, um die Abfrage im Code der Quellcodeverwaltung eindeutig zu identifizieren.

Die folgende Abfrage verwendet zum Suchen nach Bezeichnungen eine dynamische Verwaltungssicht:

```sql
SELECT  *
FROM    sys.dm_pdw_exec_requests r
WHERE   r.[label] = 'My Query Label'
;
```

> [!NOTE]
> Es ist wichtig, dass Sie das Wort „label“ bei Abfragen in eckige Klammern oder doppelte Anführungszeichen setzen. Das Wort „label“ ist ein reserviertes Wort und verursacht einen Fehler, wenn es nicht begrenzt wird. 
> 
> 

## <a name="next-steps"></a>Nächste Schritte
Weitere Hinweise zur Entwicklung finden Sie in der [Entwicklungsübersicht](develop-overview.md).


