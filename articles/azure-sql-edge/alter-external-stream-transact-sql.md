---
title: 'ALTER EXTERNAL STREAM (Transact-SQL): Azure SQL Edge (Vorschau)'
description: Erfahren Sie mehr über die ALTER EXTERNAL STREAM-Anweisung in Azure SQL Edge (Vorschau).
keywords: ''
services: sql-edge
ms.service: sql-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: 2559c4b4b875403b7c70671e27cb6222a3f1103a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84235192"
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
