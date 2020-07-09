---
title: Verwenden des Spark-Connectors mit Microsoft Azure SQL und SQL Server
description: Erfahren Sie, wie Sie den Spark-Connector mit Microsoft Azure SQL und SQL Server verwenden.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: sqldbrb=2
ms.devlang: ''
ms.topic: conceptual
author: denzilribeiro
ms.author: denzilr
ms.reviewer: carlrab
ms.date: 09/25/2018
ms.openlocfilehash: b2e042f2c3a7c6e1528ff96fb4fb96f392274855
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2020
ms.locfileid: "84025681"
---
# <a name="accelerate-real-time-big-data-analytics-using-the-spark-connector"></a>Beschleunigung von Big-Data-Echtzeitanalysen mit dem Spark-Connector 
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

Der Spark-Connector ermöglicht die Verwendung von Datenbanken in Azure SQL-Datenbank, Azure Managed Instance und SQL Server als Eingabedatenquelle oder Ausgabedatensenke für Spark-Aufträge. So können transaktionale Echtzeitdaten in der Big Data-Analyse genutzt und Ergebnisse für Ad-Hoc-Abfragen oder Berichterstellung dauerhaft gespeichert werden. Im Gegensatz zum integrierten JDBC-Connector bietet dieser Connector die Möglichkeit zur Masseneinfügung von Daten in Microsoft Azure SQL- und SQL Server-Datenbanken. Dies führt zu einer erheblichen Leistungssteigerung: Daten können gegenüber einer zeilenweisen Einfügung 10- bis 20-Mal schneller eingefügt werden. Der Spark-Connector unterstützt AAD-Authentifizierung für das Herstellen einer Verbindung mit Azure SQL-Datenbanken. Dies ermöglicht es Ihnen, unter Verwendung Ihres AAD-Kontos aus Azure Databricks eine sichere Verbindung mit Azure SQL-Datenbank herzustellen. Mit dem integrierten JDBC-Connector werden ähnliche Schnittstellen bereitgestellt. Die Migration Ihrer vorhandenen Spark-Aufträge zu diesem neuen Connector ist sehr einfach durchzuführen.

## <a name="download-and-build-spark-connector"></a>Herunterladen und Erstellen des Spark-Connectors

Laden Sie zunächst den Spark-Connector aus dem GitHub-Repository [azure-sqldb-spark](https://github.com/Azure/azure-sqldb-spark) herunter.

### <a name="official-supported-versions"></a>Offiziell unterstützte Versionen

| Komponente                             | Version                  |
| :-----------------------------------  | :----------------------- |
| Apache Spark                          | 2.0.2 oder höher           |
| Scala                                 | 2.10 oder höher            |
| Microsoft JDBC-Treiber für SQL Server  | 6.2 oder höher             |
| Microsoft SQL Server                  | SQL Server 2008 oder höher |
| Azure SQL-Datenbank                    | Unterstützt                |
| Verwaltete Azure SQL-Instanz            | Unterstützt                |

Der Spark-Connector verwendet den Microsoft JDBC-Treiber für SQL Server zum Verschieben von Daten zwischen Spark-Workerknoten und SQL-Datenbanken:

Der Datenfluss ist wie folgt:

1. Der Spark-Masterknoten stellt eine Verbindung mit einer Azure SQL- oder SQL Server-Datenbank her und lädt Daten aus einer bestimmten Tabelle oder unter Verwendung einer bestimmten SQL-Abfrage.
2. Der Spark-Masterknoten verteilt die Daten zur Transformation auf die Workerknoten.
3. Der Workerknoten stellt eine Verbindung mit einer Azure SQL- oder SQL Server-Datenbank her und schreibt Daten in die Datenbank. Benutzer könne auswählen, ob sie die Daten Zeile für Zeile oder in einem Massenvorgang einfügen möchten.

Das folgende Diagramm veranschaulicht den Datenfluss.

   ![Architektur](./media/spark-connector/architecture.png)

### <a name="build-the-spark-connector"></a>Erstellen des Spark-Connectors

Derzeit wird für das Connectorprojekt Maven verwendet. Sie können Folgendes ausführen, um den Connector ohne Abhängigkeiten zu erstellen:

- „mvn clean package“
- Download der aktuellen JAR-Versionen aus dem release-Ordner
- Integrieren der SQL-Datenbank-Spark-JAR-Datei

## <a name="connect-and-read-data-using-the-spark-connector"></a>Herstellen einer Verbindung und Lesen von Daten mit dem Spark-Connector

Sie können in Spark-Aufträgen eine Verbindung mit einer Azure SQL- oder SQL Server-Datenbank herstellen und Daten lesen oder schreiben. Alternativ können Sie eine DML- oder DDL-Abfrage in einer Datenbank in Azure SQL oder SQL Server ausführen.

### <a name="read-data-from-azure-sql-and-sql-server"></a>Lesen von Daten aus Azure SQL und SQL Server

```scala
import com.microsoft.azure.sqldb.spark.config.Config
import com.microsoft.azure.sqldb.spark.connect._

val config = Config(Map(
  "url"            -> "mysqlserver.database.windows.net",
  "databaseName"   -> "MyDatabase",
  "dbTable"        -> "dbo.Clients",
  "user"           -> "username",
  "password"       -> "*********",
  "connectTimeout" -> "5", //seconds
  "queryTimeout"   -> "5"  //seconds
))

val collection = sqlContext.read.sqlDB(config)
collection.show()
```

### <a name="read-data-from-azure-sql-and-sql-server-with-specified-sql-query"></a>Lesen von Daten aus Azure SQL und SQL Server unter Angabe einer SQL-Abfrage

```scala
import com.microsoft.azure.sqldb.spark.config.Config
import com.microsoft.azure.sqldb.spark.connect._

val config = Config(Map(
  "url"          -> "mysqlserver.database.windows.net",
  "databaseName" -> "MyDatabase",
  "queryCustom"  -> "SELECT TOP 100 * FROM dbo.Clients WHERE PostalCode = 98074" //Sql query
  "user"         -> "username",
  "password"     -> "*********",
))

//Read all data in table dbo.Clients
val collection = sqlContext.read.sqlDB(config)
collection.show()
```

### <a name="write-data-to-azure-sql-and-sql-server"></a>Schreiben von Daten in Azure SQL und SQL Server

```scala
import com.microsoft.azure.sqldb.spark.config.Config
import com.microsoft.azure.sqldb.spark.connect._

// Aquire a DataFrame collection (val collection)

val config = Config(Map(
  "url"          -> "mysqlserver.database.windows.net",
  "databaseName" -> "MyDatabase",
  "dbTable"      -> "dbo.Clients",
  "user"         -> "username",
  "password"     -> "*********"
))

import org.apache.spark.sql.SaveMode
collection.write.mode(SaveMode.Append).sqlDB(config)
```

### <a name="run-dml-or-ddl-query-in-azure-sql-and-sql-server"></a>Ausführen einer DML- oder DDL-Abfrage in Azure SQL und SQL Server

```scala
import com.microsoft.azure.sqldb.spark.config.Config
import com.microsoft.azure.sqldb.spark.query._
val query = """
              |UPDATE Customers
              |SET ContactName = 'Alfred Schmidt', City = 'Frankfurt'
              |WHERE CustomerID = 1;
            """.stripMargin

val config = Config(Map(
  "url"          -> "mysqlserver.database.windows.net",
  "databaseName" -> "MyDatabase",
  "user"         -> "username",
  "password"     -> "*********",
  "queryCustom"  -> query
))

sqlContext.sqlDBQuery(config)
```

## <a name="connect-from-spark-to-azure-sql-using-aad-authentication"></a>Herstellen einer Verbindung zwischen Spark und Azure SQL mithilfe von AAD-Authentifizierung

Sie können unter Verwendung der Azure Active Directory-Authentifizierung (AAD) eine Verbindung mit Azure SQL herstellen. Verwenden Sie AAD-Authentifizierung zur zentralen Verwaltung von Identitäten von Datenbankbenutzern und als Alternative zur SQL Server-Authentifizierung.

### <a name="connecting-using-activedirectorypassword-authentication-mode"></a>Verbindungsherstellung im Authentifizierungsmodus „ActiveDirectoryPassword“

#### <a name="setup-requirement"></a>Anforderungen für die Einrichtung

Wenn Sie den Authentifizierungsmodus „ActiveDirectoryPassword“ verwenden, müssen Sie [azure-activedirectory-library-for-java](https://github.com/AzureAD/azure-activedirectory-library-for-java) und zugehörige Abhängigkeiten herunterladen und in den Java-Buildpfad einschließen.

```scala
import com.microsoft.azure.sqldb.spark.config.Config
import com.microsoft.azure.sqldb.spark.connect._

val config = Config(Map(
  "url"            -> "mysqlserver.database.windows.net",
  "databaseName"   -> "MyDatabase",
  "user"           -> "username",
  "password"       -> "*********",
  "authentication" -> "ActiveDirectoryPassword",
  "encrypt"        -> "true"
))

val collection = sqlContext.read.sqlDB(config)
collection.show()
```

### <a name="connecting-using-access-token"></a>Verbindungsherstellung per Zugriffstoken

#### <a name="setup-requirement"></a>Anforderungen für die Einrichtung

Wenn Sie den auf einem Zugriffstoken basierenden Authentifizierungsmodus verwenden, müssen Sie [azure-activedirectory-library-for-java](https://github.com/AzureAD/azure-activedirectory-library-for-java) und zugehörige Abhängigkeiten herunterladen und in den Java-Buildpfad einschließen.

Unter [Verwenden der Azure Active Directory-Authentifizierung](authentication-aad-overview.md) finden Sie Informationen dazu, wie Sie ein Zugriffstoken für Ihre Datenbank in Azure SQL-Datenbank oder Azure SQL Managed Instance abrufen.

```scala
import com.microsoft.azure.sqldb.spark.config.Config
import com.microsoft.azure.sqldb.spark.connect._

val config = Config(Map(
  "url"                   -> "mysqlserver.database.windows.net",
  "databaseName"          -> "MyDatabase",
  "accessToken"           -> "access_token",
  "hostNameInCertificate" -> "*.database.windows.net",
  "encrypt"               -> "true"
))

val collection = sqlContext.read.sqlDB(config)
collection.show()
```

## <a name="write-data-to-azure-sql-and-sql-server-using-bulk-insert"></a>Schreiben von Daten in Azure SQL und SQL Server per Masseneinfügung

Der herkömmliche JDBC-Connector schreibt Daten zeilenweise in Azure SQL und SQL Server. Mit dem Spark-Connector können Sie Daten per Masseneinfügung in Azure SQL und SQL Server schreiben. Dies führt zu einer erheblich verbesserten Schreibleistung beim Laden großer Datasets oder beim Laden von Daten in Tabellen, in denen ein ColumStore-Index verwenden wird.

```scala
import com.microsoft.azure.sqldb.spark.bulkcopy.BulkCopyMetadata
import com.microsoft.azure.sqldb.spark.config.Config
import com.microsoft.azure.sqldb.spark.connect._

/**
  Add column Metadata.
  If not specified, metadata is automatically added
  from the destination table, which may suffer performance.
*/
var bulkCopyMetadata = new BulkCopyMetadata
bulkCopyMetadata.addColumnMetadata(1, "Title", java.sql.Types.NVARCHAR, 128, 0)
bulkCopyMetadata.addColumnMetadata(2, "FirstName", java.sql.Types.NVARCHAR, 50, 0)
bulkCopyMetadata.addColumnMetadata(3, "LastName", java.sql.Types.NVARCHAR, 50, 0)

val bulkCopyConfig = Config(Map(
  "url"               -> "mysqlserver.database.windows.net",
  "databaseName"      -> "MyDatabase",
  "user"              -> "username",
  "password"          -> "*********",
  "dbTable"           -> "dbo.Clients",
  "bulkCopyBatchSize" -> "2500",
  "bulkCopyTableLock" -> "true",
  "bulkCopyTimeout"   -> "600"
))

df.bulkCopyToSqlDB(bulkCopyConfig, bulkCopyMetadata)
//df.bulkCopyToSqlDB(bulkCopyConfig) if no metadata is specified.
```

## <a name="next-steps"></a>Nächste Schritte

Laden Sie – sofern noch nicht geschehen – den Spark-Connector aus dem GitHub-Repository [azure-sqldb-spark](https://github.com/Azure/azure-sqldb-spark) herunter, und sehen Sie sich die zusätzlichen Ressourcen im Repository an:

- [Azure Databricks-Beispielnotebooks](https://github.com/Azure/azure-sqldb-spark/tree/master/samples/notebooks)
- [Beispielskripts (Scala)](https://github.com/Azure/azure-sqldb-spark/tree/master/samples/scripts)

Es kann auch nützlich sein, den [Leitfaden zu Apache Spark SQL, DataFrames und Datasets Guide](https://spark.apache.org/docs/latest/sql-programming-guide.html) (in englischer Sprache) und die [Azure Databricks-Dokumentation](https://docs.microsoft.com/azure/azure-databricks/) zu lesen.
