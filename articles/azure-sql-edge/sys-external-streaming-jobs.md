---
title: 'sys.external_streaming_jobs (Transact-SQL): Azure SQL Edge (Vorschau)'
description: Informationen zur Verwendung von sys.external_streaming_jobs in Azure SQL Edge (Vorschau)
keywords: sys.external_streaming_jobs, SQL Edge
services: sql-edge
ms.service: sql-edge
ms.topic: reference
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2019
ms.openlocfilehash: c4da73e3197df894a0726556b4e92141818a520e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84233073"
---
# <a name="sysexternal_streaming_jobs-transact-sql"></a>sys.external_streaming_jobs (Transact-SQL)

Gibt eine Zeile für jeden externen Streamingauftrag zurück, der innerhalb des Gültigkeitsbereichs der Datenbank erstellt wurde.

|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|
|**name**|**sysname**|Der Name des Datenstroms. Ist in der Datenbank eindeutig.|
|**object_id**|**int**|Die Objekt-ID des Streamobjekts. Ist in der Datenbank eindeutig.|
|**principal_id**|**int**|Die ID des Prinzipals, der Besitzer dieser Assembly ist.|
|**schema_id**|**int**| ID des Schemas, das das Objekt enthält.|
|**parent_object_id**|**id**| Die Objekt-ID des übergeordneten Objekts für diesen Stream. In der aktuellen Implementierung ist dieser Wert immer NULL.|
|**type**|**char(2)**|Objekttyp. Für Streamobjekte ist der Typ immer „EJ“.|
|**type_desc**|**nvarchar(60)**| Beschreibung des Objekttyps. Für Streamobjekte ist der Typ immer „EXTERNAL_STREAMING_JOB“.|
|**create_date**|**datetime**| Datum, an dem das Objekt erstellt wurde.|
|**modify_date**|**datetime**| In der aktuellen Implementierung ist dieser Wert identisch mit dem create_date für das Streamobjekt. |
|**is_ms_shipped**|**bit**| Ein von einer internen Komponente erstelltes Objekt.|  
|**is_published**|**bit**| Objekt wurde veröffentlicht.|  
|**is_schema_published**|**bit**|Nur das Schema des Objekts wird veröffentlicht.|
|**uses_ansi_nulls**|**bit**| Beim Erstellen des Streamobjekts war die Datenbankoption SET ANSI_NULLS auf ON festgelegt.|
|**statement**|**varchar(max)**| Der Stream Analytics-Abfragetext für den Streamingauftrag. Weitere Informationen finden Sie unter [sp_create_streaming_job](overview.md) |
|**status**|**int**| Den aktuellen Status des Streamingauftrags. Mögliche Werte: <br /><br /> **Erstellt** = 0. Der Streamingauftrag wurde erstellt, aber noch nicht gestartet. <br /><br /> **Wird gestartet** = 1. Der Streamingauftrag befindet sich in der Startphase. <br /><br /> **Fehler** = 6. Der Streamingauftrag ist fehlgeschlagen. Dies ist im Allgemeinen ein Hinweis auf einen schwerwiegenden Fehler während der Verarbeitung. <br /><br /> **Beendet** = 4. Der Streamingauftrag wurde beendet. <br /><br /> **Im Leerlauf** = 7. Der Streamingauftrag wird ausgeführt, es sind jedoch keine Eingaben für die Verarbeitung vorhanden. <br /><br /> **Wird verarbeitet** = 8. Der Streamingauftrag wird ausgeführt und verarbeitet Eingaben. Dieser Zustand zeigt einen fehlerfreien Status für den Streamingauftrag an. <br /><br /> **Beeinträchtigt** = 9. Der Streamingauftrag wird ausgeführt, es sind jedoch bei der Eingabeverarbeitung einige nicht schwerwiegende Fehler bei der Serialisierung/Deserialisierung der Eingabe/Ausgabe aufgetreten. Der Eingabeauftrag wird weiterhin ausgeführt, löscht jedoch Eingaben, bei denen Fehler auftreten.|

## <a name="permissions"></a>Berechtigungen

Die Sichtbarkeit der Metadaten in Katalogsichten ist auf sicherungsfähige Elemente eingeschränkt, bei denen der Benutzer entweder der Besitzer ist oder für die dem Benutzer eine Berechtigung erteilt wurde. Weitere Informationen finden Sie unter [Metadata Visibility Configuration](/sql/relational-databases/security/metadata-visibility-configuration/).

## <a name="see-also"></a>Weitere Informationen

- [T-SQL Streaming: Katalogsichten](overview.md)
- [Katalogsichten (Transact-SQL)](/sql/relational-databases/system-catalog-views/catalog-views-transact-sql/)
- [Systemsichten (Transact-SQL)](/sql/t-sql/language-reference/)

