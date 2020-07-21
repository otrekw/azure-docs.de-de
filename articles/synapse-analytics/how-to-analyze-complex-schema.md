---
title: Analysieren von Schemas mit Arrays und geschachtelten Strukturen
description: Erfahren Sie, wie Sie Arrays und geschachtelte Strukturen mit Apache Spark und SQL analysieren.
services: synapse-analytics
author: ArnoMicrosoft
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: ''
ms.date: 06/15/2020
ms.author: acomet
ms.reviewer: jrasnick
ms.openlocfilehash: b02c3627cea5e441739c77d1882505c6b82489bc
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84907952"
---
# <a name="analyze-complex-data-types-in-synapse"></a>Analysieren komplexer Datentypen in Synapse

Dieser Artikel bezieht sich auf Parquet-Dateien und Container in **Azure Synapse Link für Azure Cosmos DB**. Es wird erläutert, wie Benutzer Spark oder SQL verwenden können, um Daten mit komplexen Schemas wie Arrays oder geschachtelten Strukturen zu lesen oder zu transformieren. Das folgende Beispiel bezieht sich auf ein einzelnes Dokument, kann aber mit Spark oder SQL problemlos auf Milliarden von Dokumenten skaliert werden. Im folgenden Code wird PySpark (Python) verwendet.

## <a name="use-case"></a>Anwendungsfall

Mit modernen Datentypen werden häufig komplexe Datentypen verarbeitet, die eine Herausforderung für Data Engineers darstellen. Das Analysieren von geschachtelten Schemas und Arrays bringt einige Herausforderungen mit sich:
* Das Schreiben von SQL-Abfragen ist komplex.
* Das Umbenennen/Umwandeln des Datentyps von geschachtelten Spalten ist schwierig.
* Bei tief geschachtelten Objekten kommt es zu Leistungsproblemen.

Data Engineers müssen wissen, wie diese Datentypen effizient verarbeitet und für alle leicht zugänglich gemacht werden können.

Im folgenden Beispiel wird Synapse Spark zum Lesen und Transformieren von Objekten durch Datenrahmen in eine flache Struktur verwendet. Synapse SQL Serverless wird verwendet, um solche Objekte direkt abzufragen und die entsprechenden Ergebnisse als reguläre Tabelle zurückzugeben.

## <a name="what-are-arrays-and-nested-structures"></a>Was sind Arrays und geschachtelte Strukturen?

Das folgende Objekt stammt aus App Insights. Dieses Objekt enthält geschachtelte Strukturen, aber auch Arrays, die ebenfalls geschachtelte Strukturen enthalten.

```json
{
    "id": "66532691-ab20-11ea-8b1d-936b3ec64e54",
    "context": {
        "data": {
            "eventTime": "2020-06-10T13:43:34.553Z",
            "samplingRate": "100.0",
            "isSynthetic": "false"
        },
        "session": {
            "isFirst": "false",
            "id": "38619c14-7a23-4687-8268-95862c5326b1"
        },
        "custom": {
            "dimensions": [
                {
                    "customerInfo": {
                        "ProfileType": "ExpertUser",
                        "RoomName": "",
                        "CustomerName": "diamond",
                        "UserName": "XXXX@yahoo.com"
                    }
                },
                {
                    "customerInfo": {
                        "ProfileType": "Novice",
                        "RoomName": "",
                        "CustomerName": "topaz",
                        "UserName": "XXXX@outlook.com"
                    }
                }
            ]
        }
    }
}
```

### <a name="schema-example-of-arrays-and-nested-structures"></a>Schemabeispiel für Arrays und geschachtelte Strukturen
Beim Ausgeben des Schemas des Datenrahmens für dieses Objekt (mit dem Namen **df**) über den Befehl **df.printschema** wird die folgende Darstellung angezeigt:

* Die geschachtelte Struktur ist gelb hervorgehoben.
* Ein Array mit zwei Elementen ist grün hervorgehoben.

[![Schema Ursprung](./media/how-to-complex-schema/schema-origin.png)](./media/how-to-complex-schema/schema-origin.png#lightbox)

_rid, _ts und _etag wurden dem System beim Erfassen des Dokuments im Transaktionsspeicher von Azure Cosmos DB hinzugefügt.

Der obige Datenrahmen umfasst nur 5 Spalten und 1 Zeile. Nach der Transformation verfügt der erstellte Datenrahmen über 13 Spalten und 2 Zeilen in einem Tabellenformat.

## <a name="flatten-nested-structures-and-explode-arrays-with-apache-spark"></a>Vereinfachen von geschachtelten Strukturen und Auflösen von Arrays mit Apache Spark

Mit Synapse Spark ist das Transformieren von geschachtelten Strukturen in Spalten und Arrayelementen in mehrere Zeilen ganz einfach. Die folgenden Schritte können von jedem für die eigene Implementierung verwendet werden.

[![Schritte für Transformationen in Spark](./media/how-to-complex-schema/spark-transfo-steps.png)](./media/how-to-complex-schema/spark-transfo-steps.png#lightbox)

**Schritt 1:** Sie definieren eine Funktion zum Vereinfachen des geschachtelten Schemas. Diese Funktion kann ohne Änderung übernommen werden. Erstellen Sie eine Zelle in einem PySpark-Notebook mit der folgenden Funktion:

```python
from pyspark.sql.functions import col

def flatten_df(nested_df):
    stack = [((), nested_df)]
    columns = []

    while len(stack) > 0:
        parents, df = stack.pop()

        flat_cols = [
            col(".".join(parents + (c[0],))).alias("_".join(parents + (c[0],)))
            for c in df.dtypes
            if c[1][:6] != "struct"
        ]

        nested_cols = [
            c[0]
            for c in df.dtypes
            if c[1][:6] == "struct"
        ]

        columns.extend(flat_cols)

        for nested_col in nested_cols:
            projected_df = df.select(nested_col + ".*")
            stack.append((parents + (nested_col,), projected_df))

    return nested_df.select(columns)
```

**Schritt 2**: Verwenden Sie die Funktion, um das geschachtelte Schema des Datenrahmens **df** in einen neuen Datenrahmen **df_flat** zu vereinfachen:

```python
from pyspark.sql.types import StringType, StructField, StructType
df_flat = flatten_df(df)
display(df_flat.limit(10))
```

Die Anzeigefunktion sollte 10 Spalten und 1 Zeile zurückgeben. Das Array und seine geschachtelten Elemente sind immer noch vorhanden.

**Schritt 3**: Nun transformieren Sie das Array **context_custom_dimensions** im Datenrahmen **df_flat** in einen neuen Datenrahmen **df_flat_explode**. Im folgenden Code wird auch definiert, welche Spalte ausgewählt wird:

```python
from pyspark.sql.functions import explode
from pyspark.sql.functions import flatten
from pyspark.sql.functions import arrays_zip
df_flat_explode = df_flat.select("_rid","_ts","id","_etag",explode(df_flat.context_custom_dimensions),"context_session_isFirst","context_session_id","context_data_eventTime","context_data_samplingRate","context_data_isSynthetic")\
.select("_rid","_ts","id","_etag","col.*","context_session_isFirst","context_session_id","context_data_eventTime","context_data_samplingRate","context_data_isSynthetic")
display(df_flat_explode.limit(10))

```

Die Anzeigefunktion sollte das folgende Ergebnis zurückgeben: 10 Spalten und 2 Zeilen. Der nächste Schritt ist das Vereinfachen von geschachtelten Schemas mit der in Schritt 1 definierten Funktion.

**Schritt 4**: Verwenden Sie die Funktion, um das geschachtelte Schema des Datenrahmens **df_flat_explode** in einen neuen Datenrahmen **df_flat_explode_flat** zu vereinfachen:
```python
df_flat_explode_flat = flatten_df(df_flat_explode)
display(df_flat_explode_flat.limit(10))
```

Die Anzeigefunktion sollte 13 Spalten und 2 Zeilen zurückgeben.

Die Funktion printSchema für den Datenrahmen df_flat_explode_flat gibt das folgende Ergebnis zurück:

[![Schema final](./media/how-to-complex-schema/schema-final.png)](./media/how-to-complex-schema/schema-final.png#lightbox)

## <a name="read-arrays-and-nested-structures-directly-with-sql-serverless"></a>Direktes Lesen von Arrays und geschachtelten Strukturen mit SQL Serverless

Das Abfragen und Erstellen von Ansichten und Tabellen für solche Objekte ist mit SQL Serverless möglich.

Zuerst sollten die Benutzer, je nachdem, wie Daten gespeichert wurden, die folgende Taxonomie verwenden. Alles in GROẞBUCHSTABEN ist spezifisch für Ihren Anwendungsfall:

| BULK              | FORMAT |
| -------------------- | --- |
| 'https://ACCOUNTNAME.dfs.core.windows.net/FILESYSTEM/PATH/FINENAME.parquet ' |'Parquet' (ADLSg2)|
| N'endpoint=https://ACCOUNTNAME.documents-staging.windows-ppe.net:443/;account= KONTONAME;database=DATENBANKNAME;collection=SAMMLUNGSNAME;region=ABZUFRAGENDE REGION, SECRET='IHR GEHEIMNIS' |'CosmosDB' (Synapse Link)|



**SQL Serverless** unterstützt verknüpfte Dienste für Azure Synapse Link für Azure Cosmos DB und AAD-Passthrough. Diese Funktion befindet sich derzeit in der geschlossenen Vorschau für Synapse Link.

Ersetzen Sie unten
* 'YOUR BULK ABOVE' durch die Verbindungszeichenfolge der Datenquelle, mit der Sie eine Verbindung herstellen.
* 'YOUR TYPE ABOVE' durch das Format, das Sie zum Herstellen einer Verbindung mit der Quelle verwenden.

```sql
select *
FROM
openrowset(
BULK 'YOUR BULK ABOVE',
            FORMAT='YOUR TYPE ABOVE'
)
with (id varchar(50),
        contextdataeventTime varchar(50) '$.context.data.eventTime',
        contextdatasamplingRate varchar(50) '$.context.data.samplingRate',
        contextdataisSynthetic varchar(50) '$.context.data.isSynthetic',
        contextsessionisFirst varchar(50) '$.context.session.isFirst',
        contextsessionid varchar(50) '$.context.session.id',
        contextcustomdimensions varchar(max) '$.context.custom.dimensions'
) as q 
cross apply openjson (contextcustomdimensions) 
with ( ProfileType varchar(50) '$.customerInfo.ProfileType',
            RoomName varchar(50) '$.customerInfo.RoomName',
            CustomerName varchar(50) '$.customerInfo.CustomerName',
            UserName varchar(50) '$.customerInfo.UserName'
    )
```

Zwei verschiedene Arten von Vorgängen werden ausgeführt:
* Mit der folgenden Codezeile wird die Spalte contextdataeventTime definiert, die auf das geschachtelte Element verweist: Context.Data.eventTime.
```sql
contextdataeventTime varchar(50) '$.context.data.eventTime'
```

In dieser Zeile wird die Spalte contextdataeventTime definiert, die auf das Schachtelungselement verweist: Context>Data>eventTime.

* **cross apply** wird verwendet, um neue Zeilen für jedes Element im Array zu erstellen und anschließend alle geschachtelten Objekte ähnlich wie im ersten Punkt zu definieren: 
```sql
cross apply openjson (contextcustomdimensions) 
with ( ProfileType varchar(50) '$.customerInfo.ProfileType', 
```

Wenn das Array 5 Elemente mit 4 geschachtelten Strukturen enthält, gibt SQL Serverless 5 Zeilen und 4 Spalten zurück.

SQL Serverless kann direkte Abfragen ausführen, das Array in 2 Zeilen zuordnen und alle geschachtelten Strukturen in Spalten anzeigen.

## <a name="next-steps"></a>Nächste Schritte

* [Erfahren Sie, wie Sie Azure Synapse Link für Azure Cosmos DB mit Spark abfragen.](./synapse-link/how-to-query-analytical-store-spark.md)
* [Abfragen von geschachtelten Parquet-Typen](./sql/query-parquet-nested-types.md) 