---
title: 'sys.external_job_streams (Transact-SQL): Azure SQL Edge (Vorschau)'
description: Informationen zur Verwendung von sys.external_job_streams in Azure SQL Edge (Vorschau)
keywords: sys.external_job_streams, SQL Edge
services: sql-database-edge
ms.service: sql-database-edge
ms.topic: reference
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2019
ms.openlocfilehash: 7f26c87c0de09618b2d24413f7ff692fd764b4cf
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83594479"
---
# <a name="sysexternal_job_streams-transact-sql"></a>sys.external_job_streams (Transact-SQL)

Gibt jeweils eine Zeile für das externe Eingabe- oder Ausgabestreamobjekt zurück, das einem externen Streamingauftrag zugeordnet ist.

|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|
|**job_id**|**int**| Die Objekt-ID des Streamingauftragsobjekts. Diese Spalte wird der object_id-Spalte von sys.external_streaming_jobs zugeordnet.|
|**stream_id**|**int**| Die Objekt-ID des Streamobjekts. Diese Spalte wird der object_id-Spalte von sys.external_streams zugeordnet. |
|**is_input**|**bit**| 1, wenn das Streamobjekt als Eingabe für den Streamingauftrag verwendet wird, andernfalls 0.|
|**is_output**|**bit**| 1, wenn das Streamobjekt als Ausgabe für den Streamingauftrag verwendet wird, andernfalls 0.|

## <a name="example"></a>Beispiel

Diese Katalogsicht wird in Verbindung mit `sys.external_streams`- und `sys.external_streaming_jobs`-Katalogsichten verwendet. Unten sehen Sie eine Beispielabfrage.

```sql
Select
    sj.Name as Job_Name,
    sj.Create_date as Job_Create_date,
    sj.modify_date as Job_Modify_date,
    sj.statement as Stream_Job_Query,
    Input_Stream_Name =
        Case js.is_input
            when 1 then s.Name
            else null
        END,
    output_Stream_Name =
        case js.is_output
            when 1 then s.Name
            else null
        END,
    s.location as Stream_Location
from sys.external_job_streams js
inner join sys.external_streams s on s.object_id = js.stream_id
inner join sys.external_streaming_jobs sj on sj.object_id = js.job_id
```

## <a name="permissions"></a>Berechtigungen

Die Sichtbarkeit der Metadaten in Katalogsichten ist auf sicherungsfähige Elemente eingeschränkt, bei denen der Benutzer entweder der Besitzer ist oder für die dem Benutzer eine Berechtigung erteilt wurde. Weitere Informationen finden Sie unter [Metadata Visibility Configuration](/sql/relational-databases/security/metadata-visibility-configuration/).

## <a name="see-also"></a>Weitere Informationen

- [Katalogsichten (Transact-SQL)](/sql/relational-databases/system-catalog-views/catalog-views-transact-sql/)
- [Systemsichten (Transact-SQL)](/sql/t-sql/language-reference/)
