---
title: 'ALTER EXTERNAL STREAM (Transact-SQL): Azure SQL Edge (Vorschau)'
description: Erfahren Sie mehr über die ALTER EXTERNAL STREAM-Anweisung in Azure SQL Edge (Vorschau).
keywords: ''
services: sql-database-edge
ms.service: sql-database-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: 0183972b5eb92d3f081b857940609bffc183b331
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83595409"
---
# <a name="alter-external-stream-transact-sql"></a>ALTER EXTERNAL STREAM (Transact-SQL)

Ändert die Definition eines externen Streams. Das Ändern eines externen Streams, der von einem Streamingauftrag im Status *Wird ausgeführt* ausgeführt wird, ist unzulässig. 



## <a name="syntax"></a>Syntax

```sql
  ALTER EXTERNAL STREAM external_stream_name 
  SET 
    [DATA_SOURCE] = <data_source_name> 
    , LOCATION = <location_name> 
    , EXTERNAL_FILE_FORMAT = <external_file_format_name> 
    , INPUT_OPTIONS = <input_options> 
    , OUTPUT_OPTIONS = <output_options> 
```

## <a name="arguments"></a>Argumente

Ausführliche Informationen zu den Befehlsargumenten für ALTER EXTERN STREAM finden Sie unter [CREATE EXTERNAL STREAM](create-external-stream-transact-sql.md) (Transact-SQL).

## <a name="return-code-values"></a>Rückgabecodewerte

ALTER EXTERNAL STREAM gibt bei Erfolg 0 zurück. Ein Rückgabewert ungleich NULL weist auf einen Fehler hin.


## <a name="see-also"></a>Weitere Informationen

- [CREATE EXTERNAL STREAM (Transact-SQL)](create-external-stream-transact-sql.md) 
- [DROP EXTERNAL STREAM (Transact-SQL)](drop-external-stream-transact-sql.md) 
