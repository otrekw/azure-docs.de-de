---
title: 'sys.sp_cleanup_data_retention (Transact-SQL): Azure SQL Edge'
description: Erfahren Sie mehr über das Verwenden von sys.sp_cleanup_data_retention (Transact-SQL) in Azure SQL Edge.
keywords: sys.sp_cleanup_data_retention (Transact-SQL), SQL Edge
services: sql-edge
ms.service: sql-edge
ms.topic: reference
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 09/22/2020
ms.openlocfilehash: 9c0a6700a476d4f7f875af5373e3c99bc4e25af2
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "90930596"
---
# <a name="syssp_cleanup_data_retention-transact-sql"></a>sys.sp_cleanup_data_retention (Transact-SQL)

**Anwendungsbereich:** Azure SQL Edge

Führt die Bereinigung veralteter Datensätze in Tabellen aus, für die Datenaufbewahrungsrichtlinien aktiviert sind. Weitere Informationen finden Sie unter [Datenaufbewahrung](data-retention-overview.md). 

## <a name="syntax"></a>Syntax 

```sql
sys.sp_cleanup_data_retention   
    { [@schema_name = ] 'schema_name' },  
    { [@table_name = ] 'table_name' },   
    [ [@rowcount =] rowcount OUTPUT ]    

```

## <a name="arguments"></a>Argumente  
`[ @schema_name = ] schema_name`    
 Dies ist der Name des besitzenden Schemas für die Tabelle, in der die Bereinigung ausgeführt werden muss. *schema_name* ist ein erforderlicher Parameter und hat den Typ **sysname**.
  
`[ @table_name = ] 'table_name'`    
 Dies ist der Name der Tabelle, in der der Bereinigungsvorgang ausgeführt werden muss. *table_name* ist ein erforderlicher Parameter und hat den Typ **sysname**.

## <a name="output-parameter"></a>Ausgabeparameter  

`[ @rowcount = ] rowcount OUTPUT`   
 „rowcount“ ist ein optionaler Ausgabeparameter (output), der die Anzahl der Datensätze enthält, die in der Tabelle bereinigt wurden. *rowcount* hat den Typ „int“.
  
## <a name="permissions"></a>Berechtigungen    
 Erfordert db_owner-Berechtigungen.

## <a name="next-steps"></a>Nächste Schritte
- [Übersicht über Datenaufbewahrungsrichtlinie](data-retention-overview.md)
- [Verwalten von historischen Daten mit einer Aufbewahrungsrichtlinie](data-retention-cleanup.md) 
- [Aktivieren und Deaktivieren von Datenaufbewahrung](data-retention-enable-disable.md)
