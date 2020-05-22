---
title: 'sys.external_streams (Transact-SQL): Azure SQL Edge (Vorschau)'
description: Informationen zur Verwendung von sys.external_streams in Azure SQL Edge (Vorschau)
keywords: sys.external_streams, SQL Edge
services: sql-database-edge
ms.service: sql-database-edge
ms.topic: reference
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2019
ms.openlocfilehash: d47139b0b2a20ecfcf92bfc55a8b47c8e3cf7dab
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83594469"
---
# <a name="sysexternal_streams-transact-sql"></a>sys.external_streams (Transact-SQL)

Gibt eine Zeile für jedes externe Streamobjekt zurück, das innerhalb des Gültigkeitsbereichs der Datenbank erstellt wurde.

|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|
|**name**|**sysname**|Der Name des Datenstroms. Ist in der Datenbank eindeutig.|
|**object_id**|**int**|Die Objekt-ID des Streamobjekts. Ist in der Datenbank eindeutig.|
|**principal_id**|**int**|Die ID des Prinzipals, der Besitzer dieser Assembly ist.|
|**schema_id**|**int**| ID des Schemas, das das Objekt enthält.|
|**parent_object_id**|**id**| Die Objekt-ID des übergeordneten Objekts für diesen Stream. In der aktuellen Implementierung ist dieser Wert immer NULL.|
|**type**|**char(2)**|Objekttyp. Für Streamobjekte ist der Typ immer „ES“.|
|**type_desc**|**nvarchar(60)**| Beschreibung des Objekttyps. Für Streamobjekte ist der Typ immer „EXTERNAL_STREAM“.|
|**create_date**|**datetime**| Datum, an dem das Objekt erstellt wurde.|
|**modify_date**|**datetime**| Das Datum, an dem das Objekt zuletzt mithilfe einer ALTER-Anweisung geändert wurde.|
|**is_ms_shipped**|**bit**| Ein von einer internen Komponente erstelltes Objekt.|  
|**is_published**|**bit**|Objekt wurde veröffentlicht.|  
|**is_schema_published**|**bit**|Nur das Schema des Objekts wird veröffentlicht.|
|**max_column_id_used**|**bit**| Diese Spalte wird für interne Zwecke verwendet und in Zukunft entfernt werden.|  
|**uses_ansi_nulls**|**bit**| Beim Erstellen des Streamobjekts war die Datenbankoption SET ANSI_NULLS auf ON festgelegt.|
|**data_source_id**|**int**| Die Objekt-ID für die externe Datenquelle, die durch das Streamobjekt dargestellt wird. |  
|**file_format_id**|**int**| Die Objekt-ID für das externe Dateiformat, das vom Streamobjekt verwendet wird. Das externe Dateiformat ist erforderlich, um das tatsächliche Layout der Daten anzugeben, auf die von einem externen Stream verwiesen wird.| 
|**location**|**varchar(max)**| Das Ziel für das externe Streamobjekt. Weitere Informationen finden Sie unter [Erstellen eines externen Streams](overview.md). |
|**input_option**|**varchar(max)**| Die Eingabeoptionen, die während der Erstellung des externen Streams verwendet werden. Weitere Informationen finden Sie unter [Erstellen eines externen Streams](overview.md). |
|**output_option**|**varchar(max)**| Die Ausgabeoptionen, die während der Erstellung des externen Streams verwendet werden. Weitere Informationen finden Sie unter [Erstellen eines externen Streams](overview.md). | 

## <a name="permissions"></a>Berechtigungen

Die Sichtbarkeit der Metadaten in Katalogsichten ist auf sicherungsfähige Elemente eingeschränkt, bei denen der Benutzer entweder der Besitzer ist oder für die dem Benutzer eine Berechtigung erteilt wurde. Weitere Informationen finden Sie unter [Metadata Visibility Configuration](/sql/relational-databases/security/metadata-visibility-configuration/).

## <a name="see-also"></a>Weitere Informationen

- [Katalogsichten (Transact-SQL)](/sql/relational-databases/system-catalog-views/catalog-views-transact-sql/)
- [Systemsichten (Transact-SQL)](/sql/t-sql/language-reference/)
