---
title: Importieren und Exportieren von Daten zwischen Spark-Pools (Vorschauversion) und SQL-Pools
description: Dieser Artikel enthält Informationen dazu, wie Sie den benutzerdefinierten Connector zum Verschieben von Daten zwischen SQL-Pools und Spark-Pools (Vorschauversion) verwenden.
services: synapse-analytics
author: euangMS
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: prgomata
ms.reviewer: euang
ms.openlocfilehash: f92c05476c9e85690fdeacade5463a43d0a4af42
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81420424"
---
# <a name="introduction"></a>Einführung

Der Spark SQL-Analyse-Connector ist für die effiziente Übertragung von Daten zwischen Spark-Pools (Vorschauversion) und SQL-Pools in Azure Synapse konzipiert. Der Spark SQL-Analyse-Connector kann nur in SQL-Pools, aber nicht in SQL On-Demand verwendet werden.

## <a name="design"></a>Entwurf

Die Übertragung von Daten zwischen Spark-Pools und SQL-Pools kann mithilfe von JDBC erfolgen. Bei zwei verteilten Systemen wie Spark und SQL-Pools stellt JDBC jedoch tendenziell einen Engpass bei der seriellen Datenübertragung dar.

Der SQL-Analyse-Connector für Spark-Pools ist eine Datenquellenimplementierung für Apache Spark. Er verwendet Azure Data Lake Storage Gen 2 und PolyBase in SQL-Pools, um Daten effizient zwischen dem Spark-Cluster und der SQL-Analyseinstanz zu übertragen.

![Connectorarchitektur](./media/synapse-spark-sqlpool-import-export/arch1.png)

## <a name="authentication-in-azure-synapse-analytics"></a>Authentifizierung in Azure Synapse Analytics

Die Authentifizierung zwischen Systemen erfolgt in Azure Synapse Analytics nahtlos. Es gibt einen Tokendienst, der eine Verbindung mit Azure Active Directory herstellt, um Sicherheitstoken für den Zugriff auf das Speicherkonto oder den Data Warehouse-Server zu erhalten. Aus diesem Grund ist es nicht erforderlich, Anmeldeinformationen zu erstellen oder in der Connector-API anzugeben, solange AAD-Authentifizierung im Speicherkonto und auf dem Data Warehouse-Server konfiguriert ist. Andernfalls kann die SQL-Authentifizierung angegeben werden. Ausführlichere Informationen finden Sie im Abschnitt [Verbrauch](#usage).

## <a name="constraints"></a>Einschränkungen

- Dieser Connector kann nur in Scala verwendet werden.

## <a name="prerequisites"></a>Voraussetzungen

- Sie müssen über die Rolle **db_exporter** in der Datenbank/dem SQL-Pool verfügen, die bzw. den Sie als Quelle oder Ziel für Datenübertragungen verwenden möchten.

Stellen Sie zum Erstellen von Benutzern eine Verbindung mit der Datenbank her, und halten Sie sich an die folgenden Beispiele:

```Sql
CREATE USER Mary FROM LOGIN Mary;
CREATE USER [mike@contoso.com] FROM EXTERNAL PROVIDER;
```

So weisen Sie eine Rolle zu:

```Sql
EXEC sp_addrolemember 'db_exporter', 'Mary';
```

## <a name="usage"></a>Verwendung

Die import-Anweisungen müssen nicht angegeben werden. Sie werden für die Notebookumgebung vorab importiert.

### <a name="transferring-data-to-or-from-a-sql-pool-in-the-logical-server-dw-instance-attached-with-the-workspace"></a>Übertragen von Daten in einen oder aus einem SQL-Pool auf dem logischen Server (DW-Instanz), der an den Arbeitsbereich angefügt wurde

> [!NOTE]
> **Importe sind in der Notebookumgebung nicht erforderlich.**

```Scala
 import com.microsoft.spark.sqlanalytics.utils.Constants
 import org.apache.spark.sql.SqlAnalyticsConnector._
```

#### <a name="read-api"></a>Lese-API

```Scala
val df = spark.read.sqlanalytics("[DBName].[Schema].[TableName]")
```

Die obige API kann sowohl für interne (verwaltete) als auch für externe Tabellen im SQL-Pool verwendet werden.

#### <a name="write-api"></a>API zum Schreiben

```Scala
df.write.sqlanalytics("[DBName].[Schema].[TableName]", [TableType])
```

Für „TableType“ kann „Constants.INTERNAL“ oder „Constants.EXTERNAL“ angegeben werden.

```Scala
df.write.sqlanalytics("[DBName].[Schema].[TableName]", Constants.INTERNAL)
df.write.sqlanalytics("[DBName].[Schema].[TableName]", Constants.EXTERNAL)
```

Die Authentifizierung bei Storage und SQL Server ist abgeschlossen.

### <a name="if-you-are-transferring-data-to-or-from-a-sql-pool-or-database-in-a-logical-server-outside-the-workspace"></a>Übertragen von Daten in einen oder aus einem SQL-Pool bzw. in eine oder aus einer Datenbank auf einem logischen Server außerhalb des Arbeitsbereichs

> [!NOTE]
> Importe sind in der Notebookumgebung nicht erforderlich.

```Scala
 import com.microsoft.spark.sqlanalytics.utils.Constants
 import org.apache.spark.sql.SqlAnalyticsConnector._
```

#### <a name="read-api"></a>Lese-API

```Scala
val df = spark.read.
option(Constants.SERVER, "samplews.database.windows.net").
sqlanalytics("<DBName>.<Schema>.<TableName>")
```

#### <a name="write-api"></a>API zum Schreiben

```Scala
df.write.
option(Constants.SERVER, "[samplews].[database.windows.net]").
sqlanalytics("[DBName].[Schema].[TableName]", [TableType])
```

### <a name="using-sql-auth-instead-of-aad"></a>Verwenden der SQL-Authentifizierung anstelle von AAD

#### <a name="read-api"></a>Lese-API

Der Connector unterstützt derzeit keine tokenbasierte Authentifizierung für einen SQL-Pool, der sich außerhalb des Arbeitsbereichs befindet. Sie müssen die SQL-Authentifizierung verwenden.

```Scala
val df = spark.read.
option(Constants.SERVER, "samplews.database.windows.net").
option(Constants.USER, [SQLServer Login UserName]).
option(Constants.PASSWORD, [SQLServer Login Password]).
sqlanalytics("<DBName>.<Schema>.<TableName>")
```

#### <a name="write-api"></a>API zum Schreiben

```Scala
df.write.
option(Constants.SERVER, "[samplews].[database.windows.net]").
option(Constants.USER, [SQLServer Login UserName]).
option(Constants.PASSWORD, [SQLServer Login Password]).
sqlanalytics("[DBName].[Schema].[TableName]", [TableType])
```

### <a name="using-the-pyspark-connector"></a>Verwenden des PySpark-Connectors

> [!NOTE]
> In diesem Beispiel wird nur die Notebookumgebung berücksichtigt.

Angenommen, Sie verfügen über den Datenrahmen „pyspark_df“, den Sie in das DW schreiben möchten.

Erstellen Sie mithilfe des Datenrahmens eine temporäre Tabelle in PySpark:

```Python
pyspark_df.createOrReplaceTempView("pysparkdftemptable")
```

Führen Sie mithilfe von Magics eine Scala-Zelle im PySpark-Notebook aus.

```Scala
%%spark
val scala_df = spark.sqlContext.sql ("select * from pysparkdftemptable")

pysparkdftemptable.write.sqlanalytics("sqlpool.dbo.PySparkTable", Constants.INTERNAL)
```
Lesen Sie analog dazu im Leseszenario die Daten mithilfe von Scala, schreiben Sie sie in eine temporäre Tabelle, und verwenden Sie Spark SQL in PySpark, um die temporäre Tabelle in einem Datenrahmen abzufragen.

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen eines SQL-Pools]([Create a new Apache Spark pool for an Azure Synapse Analytics workspace](../../synapse-analytics/quickstart-create-apache-spark-pool.md))
- [Erstellen eines neuen Apache Spark-Pools für einen Azure Synapse Analytics-Arbeitsbereich](../../synapse-analytics/quickstart-create-apache-spark-pool.md) 