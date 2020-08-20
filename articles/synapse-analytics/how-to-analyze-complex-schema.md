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
ms.openlocfilehash: fdf3dc56575a45ad0c9e716054184ba2691133ba
ms.sourcegitcommit: 2ff0d073607bc746ffc638a84bb026d1705e543e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2020
ms.locfileid: "87831701"
---
# <a name="analyze-complex-data-types-in-azure-synapse-analytics"></a>Analysieren komplexer Datentypen in Azure Synapse Analytics

Dieser Artikel bezieht sich auf Parquet-Dateien und Container in [Synapse Link für Azure Cosmos DB](.\synapse-link\how-to-connect-synapse-link-cosmos-db.md). Es wird erläutert, wie Benutzer Spark oder SQL verwenden können, um Daten mit komplexen Schemas wie Arrays oder geschachtelten Strukturen zu lesen oder zu transformieren. Das folgende Beispiel wird mit einem einzigen Dokument durchgeführt, kann aber mit Spark oder SQL problemlos auf Milliarden von Dokumenten skaliert werden. Der in diesem Artikel enthaltene Code verwendet PySpark (Python).

## <a name="use-case"></a>Anwendungsfall

Komplexe Datentypen sind immer häufiger anzutreffen und stellen eine Herausforderung für Data Engineers dar, da das Analysieren geschachtelter Schemas und Arrays in der Regel zeitaufwändige und komplexe SQL-Abfragen beinhaltet. Darüber hinaus kann es schwierig sein, den Datentyp geschachtelter Spalten umzubenennen oder umzuwandeln. Außerdem treten beim Arbeiten mit tief geschachtelten Objekten Leistungsprobleme auf.

Data Engineers müssen wissen, wie komplexe Datentypen effizient verarbeitet und für alle leicht zugänglich gemacht werden können.

Im folgenden Beispiel wird Synapse Spark zum Lesen und Transformieren von Objekten in eine vereinfachte Struktur über Datenrahmen verwendet. Synapse SQL (serverlos) wird verwendet, um solche Objekte direkt abzufragen und die entsprechenden Ergebnisse als reguläre Tabelle zurückzugeben.

## <a name="what-are-arrays-and-nested-structures"></a>Was sind Arrays und geschachtelte Strukturen?

Das folgende Objekt stammt aus [App Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview). Dieses Objekt enthält geschachtelte Strukturen und Arrays, die geschachtelte Strukturen enthalten.

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
Beim Ausgeben des Schemas für den Datenrahmen des Objekts (mit dem Namen **df**) über den Befehl `df.printschema` wird die folgende Darstellung angezeigt:

* Die geschachtelte Struktur ist gelb hervorgehoben.
* Ein Array mit zwei Elementen ist grün hervorgehoben.

[![Schema Ursprung](./media/how-to-complex-schema/schema-origin.png)](./media/how-to-complex-schema/schema-origin.png#lightbox)

**_rid**, **_ts** und **_etag** wurden dem System beim Erfassen des Dokuments im Transaktionsspeicher von Azure Cosmos DB hinzugefügt.

Der obige Datenrahmen umfasst nur 5 Spalten und 1 Zeile. Nach der Transformation verfügt der erstellte Datenrahmen über 13 Spalten und 2 Zeilen in einem Tabellenformat.

## <a name="flatten-nested-structures-and-explode-arrays-with-apache-spark"></a>Vereinfachen von geschachtelten Strukturen und Auflösen von Arrays mit Apache Spark

Mit Synapse Spark ist das Transformieren von geschachtelten Strukturen in Spalten und Arrayelementen in mehrere Zeilen ganz einfach. Die folgenden Schritte können für die Implementierung verwendet werden.

[![Schritte für Transformationen in Spark](./media/how-to-complex-schema/spark-transform-steps.png)](./media/how-to-complex-schema/spark-transform-steps.png#lightbox)

**Schritt 1:** Sie definieren eine Funktion zum Vereinfachen des geschachtelten Schemas. Diese Funktion kann ohne Änderung übernommen werden. Erstellen Sie eine Zelle in einem [PySpark-Notebook](quickstart-apache-spark-notebook.md) mit der folgenden Funktion:

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

**Schritt 2:** Verwenden Sie die Funktion, um das geschachtelte Schema des Datenrahmens (**df**) in einen neuen Datenrahmen `df_flat` zu vereinfachen:

```python
from pyspark.sql.types import StringType, StructField, StructType
df_flat = flatten_df(df)
display(df_flat.limit(10))
```

Die Anzeigefunktion sollte 10 Spalten und 1 Zeile zurückgeben. Das Array und seine geschachtelten Elemente sind immer noch vorhanden.

**Schritt 3:** Transformieren Sie das Array `context_custom_dimensions` im Datenrahmen `df_flat` in einen neuen Datenrahmen `df_flat_explode`. Im folgenden Code wird auch definiert, welche Spalte ausgewählt werden soll:

```python
from pyspark.sql.functions import explode
from pyspark.sql.functions import flatten
from pyspark.sql.functions import arrays_zip
df_flat_explode = df_flat.select("_rid","_ts","id","_etag",explode(df_flat.context_custom_dimensions),"context_session_isFirst","context_session_id","context_data_eventTime","context_data_samplingRate","context_data_isSynthetic")\
.select("_rid","_ts","id","_etag","col.*","context_session_isFirst","context_session_id","context_data_eventTime","context_data_samplingRate","context_data_isSynthetic")
display(df_flat_explode.limit(10))

```

Die Anzeigefunktion sollte 10 Spalten und 2 Zeilen zurückgeben. Der nächste Schritt ist das Vereinfachen von geschachtelten Schemas mit der in Schritt 1 definierten Funktion.

**Schritt 4:** Verwenden Sie die Funktion, um das geschachtelte Schema des Datenrahmens `df_flat_explode` in einen neuen Datenrahmen `df_flat_explode_flat` zu vereinfachen:
```python
df_flat_explode_flat = flatten_df(df_flat_explode)
display(df_flat_explode_flat.limit(10))
```

Die Anzeigefunktion sollte 13 Spalten und 2 Zeilen zurückgeben.

Die Funktion `printSchema` des Datenrahmens `df_flat_explode_flat` gibt das folgende Ergebnis zurück:

[![Schema final](./media/how-to-complex-schema/schema-final.png)](./media/how-to-complex-schema/schema-final.png#lightbox)

## <a name="read-arrays-and-nested-structures-directly-with-sql-serverless"></a>Direktes Lesen von Arrays und geschachtelten Strukturen mit SQL Serverless

Das Abfragen und Erstellen von Ansichten und Tabellen für solche Objekte ist mit SQL (serverlos) möglich.

Zuerst sollten die Benutzer, je nachdem, wie die Daten gespeichert wurden, die folgende Taxonomie verwenden. Alles in GROSSBUCHSTABEN ist spezifisch für Ihren Anwendungsfall:

| BULK              | FORMAT |
| -------------------- | --- |
| 'https://ACCOUNTNAME.dfs.core.windows.net/FILESYSTEM/PATH/FINENAME.parquet ' |'Parquet' (ADLSg2)|
| N'endpoint=https://ACCOUNTNAME.documents-staging.windows-ppe.net:443/;account=ACCOUNTNAME;database=DATABASENAME;collection=COLLECTIONNAME;region=REGIONTOQUERY', SECRET='YOURSECRET' |'CosmosDB' (Synapse Link)|


> [!NOTE]
> SQL (serverlos) unterstützt verknüpfte Dienste für Synapse Link für Azure Cosmos und AAD-Passthrough. Diese Funktion befindet sich derzeit in der geschlossenen Vorschau für Synapse Link.

Ersetzen Sie jedes Feld wie folgt:
* 'YOUR BULK ABOVE' = die Verbindungszeichenfolge der Datenquelle, mit der Sie eine Verbindung herstellen.
* 'YOUR TYPE ABOVE' = das Format, das Sie zum Herstellen einer Verbindung mit der Quelle verwenden.

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

Es gibt zwei verschiedene Arten von Vorgängen:

Der erste Vorgangstyp ist in der folgenden Codezeile angegeben, die die Spalte mit dem Namen `contextdataeventTime` definiert, die auf das geschachtelte Element verweist: Context.Data.eventTime. 
```sql
contextdataeventTime varchar(50) '$.context.data.eventTime'
```

In dieser Zeile wird die Spalte contextdataeventTime definiert, die auf das Schachtelungselement verweist: Context>Data>eventTime.

Der zweite Vorgangstyp verwendet `cross apply`, um neue Zeilen für jedes Element im Array zu erstellen und anschließend alle geschachtelten Objekte ähnlich wie im ersten Punkt zu definieren: 
```sql
cross apply openjson (contextcustomdimensions) 
with ( ProfileType varchar(50) '$.customerInfo.ProfileType', 
```

Wenn das Array 5 Elemente mit 4 geschachtelten Strukturen enthält, gibt SQL Serverless 5 Zeilen und 4 Spalten zurück. SQL Serverless kann direkte Abfragen ausführen, das Array in 2 Zeilen zuordnen und alle geschachtelten Strukturen in Spalten anzeigen.

## <a name="next-steps"></a>Nächste Schritte

* [Erfahren Sie, wie Sie Synapse Link für Azure Cosmos DB mit Spark abfragen.](./synapse-link/how-to-query-analytical-store-spark.md)
* [Abfragen von geschachtelten Parquet-Typen](./sql/query-parquet-nested-types.md) 
