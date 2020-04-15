---
title: 'Tutorial: Laden von Daten aus Azure Data Lake Storage'
description: Laden Sie mit externen PolyBase-Tabellen Daten aus Azure Data Lake Storage für Synapse SQL.
services: synapse-analytics
author: kevinvngo
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/08/2020
ms.author: kevin
ms.reviewer: igorstan
ms.custom: azure-synapse
ms.openlocfilehash: aeaa95090dc40c6e161f0c336c43032a50e5aad0
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2020
ms.locfileid: "80983567"
---
# <a name="load-data-from-azure-data-lake-storage-for-sql-analytics"></a>Laden von Daten aus Azure Data Lake Storage für SQL Analytics

In dieser Anleitung erfahren Sie, wie Sie mithilfe externer PolyBase-Tabellen Daten aus Azure Data Lake Storage laden. Obwohl Sie Ad-hoc-Abfragen für Daten ausführen können, die in Data Lake Storage gespeichert sind, sollten Sie die Daten zur Leistungsoptimierung importieren.

> [!NOTE]  
> Eine Alternative zum Laden stellt die [COPY-Anweisung](/sql/t-sql/statements/copy-into-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) dar, die sich derzeit in der Public Preview befindet.  Die COPY-Anweisung bietet die höchste Flexibilität. Wenn Sie Feedback zur COPY-Anweisung geben möchten, senden Sie eine E-Mail an die folgende Verteilerliste: sqldwcopypreview@service.microsoft.com.
>
> [!div class="checklist"]
>
> * Erstellen Sie die erforderlichen Datenbankobjekte zum Laden von Daten aus Data Lake Storage.
> * Stellen Sie eine Verbindung mit einem Data Lake Storage-Verzeichnis her.
> * Laden Sie Daten in das Data Warehouse.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="before-you-begin"></a>Voraussetzungen

Bevor Sie mit diesem Tutorial beginnen, laden Sie die neueste Version von [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) (SSMS) herunter, und installieren Sie sie.

Für dieses Tutorial benötigen Sie Folgendes:

* Einen SQL-Pool. Lesen Sie dazu [Erstellen eines SQL-Pools und Abfragen von Daten](create-data-warehouse-portal.md).
* Ein Data Lake Storage-Konto. Siehe [Erste Schritte mit Azure Data Lake Storage](../../data-lake-store/data-lake-store-get-started-portal.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json). Für dieses Speicherkonto müssen Sie zum Laden eine der folgenden Anmeldeinformationen konfigurieren oder angeben: Einen Speicherkontoschlüssel, einen Azure Active Directory-Anwendungsbenutzer oder einen AAD-Benutzer, der über die entsprechende RBAC-Rolle für das Speicherkonto verfügt.

## <a name="create-a-credential"></a>Erstellen von Anmeldeinformationen

Sie können diesen Abschnitt überspringen und mit „Erstellen der externen Datenquelle“ fortfahren, wenn Sie sich mit AAD-Pass-Through authentifizieren. Es ist nicht erforderlich, Anmeldeinformationen für den Geltungsbereich der Datenbank zu erstellen oder anzugeben, wenn Sie AAD-Pass-Through verwenden. Sie sollten aber sicherstellen, dass Ihr AAD-Benutzer über die entsprechende RBAC-Rolle (Storage-Blobdatenleser, Mitwirkender oder Besitzer) für das Speicherkonto verfügt. Weitere Einzelheiten finden Sie [hier](https://techcommunity.microsoft.com/t5/Azure-SQL-Data-Warehouse/How-to-use-PolyBase-by-authenticating-via-AAD-pass-through/ba-p/862260).

Für den Zugriff auf Ihr Data Lake Storage-Konto müssen Sie einen Datenbankhauptschlüssel erstellen, um die geheimen Anmeldeinformationen zu verschlüsseln. Anschließend erstellen Sie datenbankbezogene Anmeldeinformationen für das Speichern Ihres Geheimnisses. Bei der Authentifizierung mittels Dienstprinzipalen (Azure Active Directory-Anwendungsbenutzer) werden die in AAD eingerichteten Dienstprinzipal-Anmeldeinformationen in den datenbankweit gültigen Anmeldeinformationen gespeichert. Sie können den Speicherkontoschlüssel für Gen2 auch in den datenbankweit gültigen Anmeldeinformationen speichern.

Um mithilfe von Dienstprinzipalen eine Verbindung mit Data Lake Storage herzustellen, müssen Sie **zuerst** eine Azure Active Directory-Anwendung sowie einen Zugriffsschlüssel erstellen und der Anwendung Zugriff auf das Data Lake Storage-Konto gewähren. Anweisungen hierzu finden Sie unter [Dienst-zu-Dienst-Authentifizierung mit Azure Data Lake Storage Gen1 unter Verwendung von Azure Active Directory](../../data-lake-store/data-lake-store-service-to-service-authenticate-using-active-directory.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json).

Melden Sie sich bei Ihrem SQL-Pool mit einem Benutzer an, der über Berechtigungen für die Steuerungsebene (CONTROL) verfügt, und führen Sie die folgenden SQL-Anweisungen für Ihre Datenbank aus:

```sql
-- A: Create a Database Master Key.
-- Only necessary if one does not already exist.
-- Required to encrypt the credential secret in the next step.
-- For more information on Master Key: https://docs.microsoft.com/sql/t-sql/statements/create-master-key-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest

CREATE MASTER KEY;


-- B (for service principal authentication): Create a database scoped credential
-- IDENTITY: Pass the client id and OAuth 2.0 Token Endpoint taken from your Azure Active Directory Application
-- SECRET: Provide your AAD Application Service Principal key.
-- For more information on Create Database Scoped Credential: https://docs.microsoft.com/sql/t-sql/statements/create-database-scoped-credential-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest

CREATE DATABASE SCOPED CREDENTIAL ADLSCredential
WITH
    -- Always use the OAuth 2.0 authorization endpoint (v1)
    IDENTITY = '<client_id>@<OAuth_2.0_Token_EndPoint>',
    SECRET = '<key>'
;

-- B (for Gen2 storage key authentication): Create a database scoped credential
-- IDENTITY: Provide any string, it is not used for authentication to Azure storage.
-- SECRET: Provide your Azure storage account key.

CREATE DATABASE SCOPED CREDENTIAL ADLSCredential
WITH
    IDENTITY = 'user',
    SECRET = '<azure_storage_account_key>'
;

-- It should look something like this when authenticating using service principal:
CREATE DATABASE SCOPED CREDENTIAL ADLSCredential
WITH
    IDENTITY = '536540b4-4239-45fe-b9a3-629f97591c0c@https://login.microsoftonline.com/42f988bf-85f1-41af-91ab-2d2cd011da47/oauth2/token',
    SECRET = 'BjdIlmtKp4Fpyh9hIvr8HJlUida/seM5kQ3EpLAmeDI='
;
```

## <a name="create-the-external-data-source"></a>Erstellen der externen Datenquelle

Verwenden Sie den Befehl [CREATE EXTERNAL DATA SOURCE](/sql/t-sql/statements/create-external-data-source-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest), um den Speicherort der Daten zu speichern. Wenn Sie sich mit AAD-Pass-Through authentifizieren, ist der CREDENTIAL-Parameter nicht erforderlich. Wenn Sie sich mit der verwalteten Identität für Dienstendpunkte authentifizieren, folgen Sie den Anleitungen in dieser [Dokumentation](https://docs.microsoft.com/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview?toc=/azure/sql-data-warehouse/toc.json#azure-sql-data-warehouse-polybase), um die externe Datenquelle einzurichten. 

```sql
-- C (for Gen1): Create an external data source
-- TYPE: HADOOP - PolyBase uses Hadoop APIs to access data in Azure Data Lake Storage.
-- LOCATION: Provide Data Lake Storage Gen1 account name and URI
-- CREDENTIAL: Provide the credential created in the previous step.

CREATE EXTERNAL DATA SOURCE AzureDataLakeStorage
WITH (
    TYPE = HADOOP,
    LOCATION = 'adl://<datalakestoregen1accountname>.azuredatalakestore.net',
    CREDENTIAL = ADLSCredential
);

-- C (for Gen2): Create an external data source
-- TYPE: HADOOP - PolyBase uses Hadoop APIs to access data in Azure Data Lake Storage.
-- LOCATION: Provide Data Lake Storage Gen2 account name and URI
-- CREDENTIAL: Provide the credential created in the previous step.

CREATE EXTERNAL DATA SOURCE AzureDataLakeStorage
WITH (
    TYPE = HADOOP,
    LOCATION='abfs[s]://<container>@<AzureDataLake account_name>.dfs.core.windows.net', -- Please note the abfss endpoint for when your account has secure transfer enabled
    CREDENTIAL = ADLSCredential
);
```

## <a name="configure-data-format"></a>Konfigurieren des Datenformats

Zum Importieren der Daten aus Data Lake Storage müssen Sie das externe Dateiformat angeben. Dieses Objekt definiert, wie die Dateien in Data Lake Storage geschrieben werden.
Eine vollständige Liste finden Sie in unserer T-SQL-Dokumentation [CREATE EXTERNAL FILE FORMAT](/sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest).

```sql
-- D: Create an external file format
-- FIELD_TERMINATOR: Marks the end of each field (column) in a delimited text file
-- STRING_DELIMITER: Specifies the field terminator for data of type string in the text-delimited file.
-- DATE_FORMAT: Specifies a custom format for all date and time data that might appear in a delimited text file.
-- Use_Type_Default: Store missing values as default for datatype.

CREATE EXTERNAL FILE FORMAT TextFileFormat
WITH
(   FORMAT_TYPE = DELIMITEDTEXT
,    FORMAT_OPTIONS    (   FIELD_TERMINATOR = '|'
                    ,    STRING_DELIMITER = ''
                    ,    DATE_FORMAT         = 'yyyy-MM-dd HH:mm:ss.fff'
                    ,    USE_TYPE_DEFAULT = FALSE
                    )
);
```

## <a name="create-the-external-tables"></a>Erstellen von externen Tabellen

Nun, da Sie die Datenquelle und das Dateiformat festgelegt haben, können Sie die externen Tabellen erstellen. Externe Tabellen ermöglichen die Interaktion mit externen Daten. Der location-Parameter kann eine Datei oder ein Verzeichnis angeben. Wenn er ein Verzeichnis angibt, werden alle Dateien im Verzeichnis geladen.

```sql
-- D: Create an External Table
-- LOCATION: Folder under the Data Lake Storage root folder.
-- DATA_SOURCE: Specifies which Data Source Object to use.
-- FILE_FORMAT: Specifies which File Format Object to use
-- REJECT_TYPE: Specifies how you want to deal with rejected rows. Either Value or percentage of the total
-- REJECT_VALUE: Sets the Reject value based on the reject type.

-- DimProduct
CREATE EXTERNAL TABLE [dbo].[DimProduct_external] (
    [ProductKey] [int] NOT NULL,
    [ProductLabel] [nvarchar](255) NULL,
    [ProductName] [nvarchar](500) NULL
)
WITH
(
    LOCATION='/DimProduct/'
,   DATA_SOURCE = AzureDataLakeStorage
,   FILE_FORMAT = TextFileFormat
,   REJECT_TYPE = VALUE
,   REJECT_VALUE = 0
)
;

```

## <a name="external-table-considerations"></a>Überlegungen zu externen Tabellen

Eine externe Tabelle lässt sich sehr einfach erstellen, es gibt jedoch einige Feinheiten zu beachten.

Externe Tabellen sind stark typisiert. Das bedeutet, dass jede Zeile der erfassten Daten der Tabellenschemadefinition entsprechen muss.
Zeilen, die der Schemadefinition nicht entsprechen, werden nicht geladen.

Mithilfe des Optionen „REJECT_TYPE“ und „REJECT_VALUE“ können Sie definieren, wie viele Zeilen oder wie viel Prozent der Daten in der endgültigen Tabelle vorhanden sein müssen. Wird während des Ladevorgangs der Ablehnungswert erreicht, ist der Vorgang nicht erfolgreich. Die Ablehnung von Zeilen ist in den meisten Fällen auf einen Konflikt mit der Schemadefinition zurückzuführen. Wenn also beispielsweise eine Spalte fälschlicherweise mit einem int-Schema versehen wird, obwohl es sich bei den Daten in der Datei um eine Zeichenfolge handelt, können die Zeilen nicht geladen werden.

Data Lake Storage Gen1 steuert den Zugriff auf die Daten durch die rollenbasierte Zugriffssteuerung (Role Based Access Control, RBAC). Dies bedeutet, dass der Dienstprinzipal mit Leseberechtigungen für die im Speicherortparameter definierten Verzeichnisse sowie die untergeordneten Ordner der endgültigen Verzeichnisse und Dateien ausgestattet sein muss. Dadurch kann PolyBase diese Daten authentifizieren und laden.

## <a name="load-the-data"></a>Laden der Daten

Verwenden Sie die Anweisung [CREATE TABLE AS SELECT (Transact-SQL)](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest), um Daten aus Data Lake Storage zu laden.

CTAS erstellt eine neue Tabelle und füllt sie mit den Ergebnissen einer SELECT-Anweisung. CTAS definiert die neue Tabelle, sodass sie die gleichen Spalten und Datentypen wie die Ergebnisse der SELECT-Anweisung aufweist. Wenn Sie alle Spalten einer externen Tabelle auswählen, ist die neue Tabelle ein Replikat der Spalten und Datentypen aus der externen Tabelle.

In diesem Beispiel erstellen wir auf der Grundlage der externen Tabelle „DimProduct_external“ eine verteilte Hashtabelle namens „DimProduct“.

```sql

CREATE TABLE [dbo].[DimProduct]
WITH (DISTRIBUTION = HASH([ProductKey]  ) )
AS
SELECT * FROM [dbo].[DimProduct_external]
OPTION (LABEL = 'CTAS : Load [dbo].[DimProduct]');
```

## <a name="optimize-columnstore-compression"></a>Optimieren der Columnstore-Komprimierung

Tabellen werden standardmäßig als gruppierter Columnstore-Index definiert. Nach Abschluss eines Ladevorgangs sind einige der Datenzeilen möglicherweise nicht in den Columnstore-Index komprimiert.  Es gibt zahlreiche Gründe, warum dies geschieht. Weitere Informationen finden Sie unter [Verwalten von Columnstore-Indizes](sql-data-warehouse-tables-index.md).

Um die Abfrageleistung und die Columnstore-Komprimierung nach dem Ladevorgang zu optimieren, stellen Sie die Tabelle wieder her, um den Columstore-Index zu zwingen alle Zeilen zu komprimieren.

```sql

ALTER INDEX ALL ON [dbo].[DimProduct] REBUILD;

```

## <a name="optimize-statistics"></a>Optimieren von Statistiken

Es empfiehlt sich, Statistiken für einzelne Spalten sofort nach dem Ladevorgang zu erstellen. Es gibt einige Optionen für Statistiken. Beim Erstellen einer Einspaltenstatistik auf jeder Spalten vergeht möglicherweise viel Zeit bis alle Statistiken wiederhergestellt werden. Wenn Sie wissen, dass bestimmte Spalten nicht in den Abfrageprädikaten erhalten sein werden, können Sie die Erstellung von Statistiken für diese Spalten überspringen.

Wenn Sie Einzelspaltenstatistiken für jede Spalte jeder Tabelle erstellen möchten, können Sie das Codebeispiel für die gespeicherte Prozedur `prc_sqldw_create_stats` im Artikel [Statistiken](sql-data-warehouse-tables-statistics.md) verwenden.

Das folgende Beispiel ist ein guter Ausgangspunkt zum Erstellen von Statistiken. Es werden Statistiken für einzelne Spalten für jede Spalte in der Dimensionstabelle erstellt sowie für jede verknüpfte Spalte in der Faktentabelle. Sie können später immer Statistiken für einzelne oder mehrere Spalten auf anderen Faktentabellenspalten hinzufügen.

## <a name="achievement-unlocked"></a>Der Weg ist frei!

Sie haben Daten erfolgreich in Ihr Data Warehouse geladen. Großartig!

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie externe Tabellen erstellt, um die Struktur für die in Data Lake Storage Gen1 gespeicherten Daten zu definieren, und dann die PolyBase-Anweisung CREATE TABLE AS SELECT verwendet, um Daten in Ihre Instanz von Data Warehouse zu laden.

Sie haben folgende Schritte ausgeführt:
> [!div class="checklist"]
>
> * Erstellen der erforderlichen Datenbankobjekte zum Laden aus Data Lake Storage
> * Herstellen einer Verbindung mit einem Data Lake Storage-Verzeichnis
> * Laden von Daten in das Data Warehouse.
>

Das Laden von Daten ist der erste Schritt bei der Entwicklung einer Data Warehouse-Lösung mit Azure Synapse Analytics. Sehen Sie sich unsere Entwicklungsressourcen an.

> [!div class="nextstepaction"]
> [Erfahren Sie, wie Tabellen für Data Warehousing entwickelt werden.](sql-data-warehouse-tables-overview.md)
