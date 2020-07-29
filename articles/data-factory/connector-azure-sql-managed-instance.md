---
title: Kopieren von Daten in eine verwaltete Azure SQL-Instanz und aus der Instanz
description: Erfahren Sie, wie Sie Daten mit Azure Data Factory in eine bzw. aus einer verwalteten Azure SQL-Instanz verschieben.
services: data-factory
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.custom: seo-lt-2019
ms.date: 07/15/2020
ms.openlocfilehash: d67a050ccd590e220c51e02b827013ace7707ee2
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86523246"
---
# <a name="copy-data-to-and-from-azure-sql-managed-instance-by-using-azure-data-factory"></a>Kopieren von Daten in eine bzw. aus einer verwalteten Azure SQL-Instanz mit Azure Data Factory

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

In diesem Artikel wird beschrieben, wie Sie die Kopieraktivität in Azure Data Factory verwenden, um Daten in eine bzw. aus einer verwalteten Azure SQL-Instanz zu kopieren. Er baut auf dem Artikel zur [Übersicht über die Kopieraktivität](copy-activity-overview.md) auf, der eine allgemeine Übersicht über die Kopieraktivität enthält.

## <a name="supported-capabilities"></a>Unterstützte Funktionen

Dieser Connector für verwaltete SQL-Instanzen wird für die folgenden Aktivitäten unterstützt:

- [Kopieraktivität](copy-activity-overview.md) mit [unterstützter Quellen/Senken-Matrix](copy-activity-overview.md)
- [Lookup-Aktivität](control-flow-lookup-activity.md)
- [GetMetadata-Aktivität](control-flow-get-metadata-activity.md)

Sie können Daten aus einer verwalteten SQL-Instanz in beliebige unterstützte Senkendatenspeicher kopieren. Zudem können Sie Daten aus jedem unterstützten Quelldatenspeicher in eine verwaltete SQL-Instanz kopieren. Eine Liste der Datenspeicher, die als Quellen oder Senken für die Kopieraktivität unterstützt werden, finden Sie in der Tabelle [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).

Dieser Connector für verwaltete SQL-Instanzen unterstützt insbesondere Folgendes:

- Kopieren von Daten mit SQL-Authentifizierung und Azure Active Directory-Anwendungstokenauthentifizierung (Azure AD) mit einem Dienstprinzipal oder verwalteten Identitäten für Azure-Ressourcen
- Als Quelle das Abrufen von Daten mithilfe einer SQL-Abfrage oder gespeicherten Prozedur
- Als Senke das automatische Erstellen einer Zieltabelle (sofern nicht vorhanden) basierend auf dem Quellschema. Außerdem das Anfügen von Daten an eine Tabelle oder das Aufrufen einer gespeicherten Prozedur mit benutzerdefinierter Logik während des Kopiervorgangs.

>[!NOTE]
> [Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine?view=azuresqldb-mi-current) der verwalteten SQL-Instanz wird derzeit von diesem Connector nicht unterstützt. Um dies zu umgehen, können Sie einen [generischen ODBC-Connector](connector-odbc.md) und einen SQL Server-ODBC-Treiber über eine selbstgehostete Integration Runtime verwenden. Weitere Informationen finden Sie im Abschnitt [Verwenden von Always Encrypted](#using-always-encrypted). 

## <a name="prerequisites"></a>Voraussetzungen

Für den Zugriff auf den [öffentlichen Endpunkt](../azure-sql/managed-instance/public-endpoint-overview.md) der verwalteten SQL-Instanz können Sie eine über Azure Data Factory verwaltete Azure Integration Runtime verwenden. Stellen Sie sicher, dass Sie den öffentlichen Endpunkt aktivieren und außerdem Datenverkehr über den öffentlichen Endpunkt für die Netzwerksicherheitsgruppe zulassen, damit Azure Data Factory eine Verbindung mit Ihrer Datenbank herstellen kann. Weitere Informationen finden Sie in [dieser Anleitung](../azure-sql/managed-instance/public-endpoint-configure.md).

Richten Sie für den Zugriff auf den privaten Endpunkt der verwalteten SQL-Instanz eine [selbstgehostete Integration Runtime](create-self-hosted-integration-runtime.md) ein, die Zugriff auf die Datenbank hat. Wenn Sie die selbstgehostete Integration Runtime im gleichen virtuellen Netzwerk wie Ihre verwaltete Instanz bereitstellen, stellen Sie sicher, dass sich der Computer mit der Integration Runtime in einem anderen Subnetz als die verwaltete Instanz befindet. Wenn Sie die selbstgehostete Integration Runtime in einem anderen virtuellen Netzwerk bereitstellen als die verwaltete Instanz, empfiehlt sich ein Peering virtueller Netzwerke oder eine Verbindung zwischen den virtuellen Netzwerken. Weitere Informationen finden Sie unter [Herstellen einer Verbindung zwischen einer Anwendung und einer verwalteten SQL-Instanz](../azure-sql/managed-instance/connect-application-instance.md).

## <a name="get-started"></a>Erste Schritte

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Die folgenden Abschnitte enthalten Details zu Eigenschaften, die zum Definieren von Azure Data Factory-Entitäten verwendet werden, die speziell für den Connector für die verwaltete SQL-Instanz verfügbar sind.

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts

Folgende Eigenschaften werden für den mit der verwalteten SQL-Instanz verknüpften Dienst unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft muss auf **AzureSqlMI** festgelegt werden. | Ja |
| connectionString |Diese Eigenschaft gibt die **connectionString**-Informationen an, die zum Herstellen einer Verbindung mit der verwalteten Instanz über die SQL-Authentifizierung benötigt werden. Weitere Informationen finden Sie in den folgenden Beispielen. <br/>Der Standardport ist 1433. Wenn Sie die verwaltete SQL-Instanz mit einem öffentlichen Endpunkt verwenden, geben Sie explizit Port 3342 an.<br> Sie können auch ein Kennwort in Azure Key Vault speichern. Bei Verwendung der SQL-Authentifizierung pullen Sie die `password`-Konfiguration aus der Verbindungszeichenfolge. Weitere Informationen finden Sie im JSON-Beispiel unter der Tabelle sowie unter [Speichern von Anmeldeinformationen in Azure Key Vault](store-credentials-in-key-vault.md). |Ja |
| servicePrincipalId | Geben Sie die Client-ID der Anwendung an. | Ja, bei Azure AD-Authentifizierung mit einem Dienstprinzipal |
| servicePrincipalKey | Geben Sie den Schlüssel der Anwendung an. Markieren Sie dieses Feld als **SecureString**, um es sicher in Azure Data Factory zu speichern, oder [verweisen Sie auf ein in Azure Key Vault gespeichertes Geheimnis](store-credentials-in-key-vault.md). | Ja, bei Azure AD-Authentifizierung mit einem Dienstprinzipal |
| tenant | Geben Sie die Mandanteninformationen, wie Domänenname oder Mandanten-ID, für Ihre Anwendung an. Diese können Sie abrufen, indem Sie im Azure-Portal mit der Maus auf den Bereich oben rechts zeigen. | Ja, bei Azure AD-Authentifizierung mit einem Dienstprinzipal |
| connectVia | Diese [Integration Runtime](concepts-integration-runtime.md) wird zum Herstellen einer Verbindung mit dem Datenspeicher verwendet. Sie können eine selbstgehostete Integration Runtime oder eine Azure Integration Runtime verwenden, wenn die verwaltete Instanz über einen öffentlichen Endpunkt verfügt und den Zugriff von Azure Data Factory darauf zulässt. Wenn kein Wert angegeben ist, wird die standardmäßige Azure Integration Runtime verwendet. |Ja |

Weitere Voraussetzungen und JSON-Beispiele für die verschiedenen Authentifizierungstypen finden Sie in den folgenden Abschnitten:

- [SQL-Authentifizierung](#sql-authentication)
- [Azure AD-Anwendungstokenauthentifizierung: Dienstprinzipal](#service-principal-authentication)
- [Azure AD-Anwendungstokenauthentifizierung: Verwaltete Identitäten für Azure-Ressourcen](#managed-identity)

### <a name="sql-authentication"></a>SQL-Authentifizierung

**Beispiel 1: Verwenden der SQL-Authentifizierung**

```json
{
    "name": "AzureSqlMILinkedService",
    "properties": {
        "type": "AzureSqlMI",
        "typeProperties": {
            "connectionString": "Data Source=<hostname,port>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;Password=<password>;"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Beispiel 2: Verwenden der SQL-Authentifizierung mit einem Kennwort in Azure Key Vault**

```json
{
    "name": "AzureSqlMILinkedService",
    "properties": {
        "type": "AzureSqlMI",
        "typeProperties": {
            "connectionString": "Data Source=<hostname,port>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;",
            "password": { 
                "type": "AzureKeyVaultSecret", 
                "store": { 
                    "referenceName": "<Azure Key Vault linked service name>", 
                    "type": "LinkedServiceReference" 
                }, 
                "secretName": "<secretName>" 
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="service-principal-authentication"></a>Dienstprinzipalauthentifizierung

Um die Azure AD-Anwendungstokenauthentifizierung basierend auf dem Dienstprinzipal zu verwenden, gehen Sie folgendermaßen vor:

1. Führen Sie die unter [Bereitstellen eines Azure Active Directory-Administrators für Ihre verwaltete SQL-Datenbank-Instanz](../azure-sql/database/authentication-aad-configure.md#provision-azure-ad-admin-sql-managed-instance) beschriebenen Schritte aus.

2. [Erstellen Sie eine Azure Active Directory-Anwendung](../active-directory/develop/howto-create-service-principal-portal.md#register-an-application-with-azure-ad-and-create-a-service-principal) im Azure-Portal. Notieren Sie sich den Anwendungsnamen und die folgenden Werte zum Definieren des verknüpften Diensts:

    - Anwendungs-ID
    - Anwendungsschlüssel
    - Mandanten-ID

3. [Erstellen Sie Anmeldungen](https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current) für die verwaltete Azure Data Factory-Identität. Stellen Sie in SQL Server Management Studio (SSMS) über ein SQL Server-Konto (ein **SysAdmin**-Konto) eine Verbindung mit Ihrer verwalteten Instanz her. Führen Sie in der **Masterdatenbank** den folgenden T-SQL-Befehl aus:

    ```sql
    CREATE LOGIN [your application name] FROM EXTERNAL PROVIDER
    ```

4. [Erstellen Sie für eine eigenständige Datenbank Benutzer](../azure-sql/database/authentication-aad-configure.md#create-contained-users-mapped-to-azure-ad-identities) für die verwaltete Azure Data Factory-Identität. Stellen Sie eine Verbindung mit der Datenbank her, aus der bzw. in die Sie Daten kopieren möchten, und führen Sie den folgenden T-SQL-Befehl aus: 
  
    ```sql
    CREATE USER [your application name] FROM EXTERNAL PROVIDER
    ```

5. Gewähren Sie der verwalteten Data Factory-Identität die notwendigen Berechtigungen, und gehen Sie dabei so vor wie für SQL-Benutzer und andere Benutzer. Führen Sie den folgenden Code aus. Weitere Optionen finden Sie in [diesem Dokument](https://docs.microsoft.com/sql/t-sql/statements/alter-role-transact-sql?view=azuresqldb-mi-current).

    ```sql
    ALTER ROLE [role name e.g. db_owner] ADD MEMBER [your application name]
    ```

6. Konfigurieren Sie einen mit der verwalteten SQL-Instanz verknüpften Dienst in Azure Data Factory.

**Beispiel: Verwenden der Dienstprinzipalauthentifizierung**

```json
{
    "name": "AzureSqlDbLinkedService",
    "properties": {
        "type": "AzureSqlMI",
        "typeProperties": {
            "connectionString": "Data Source=<hostname,port>;Initial Catalog=<databasename>;",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="managed-identities-for-azure-resources-authentication"></a><a name="managed-identity"></a>Verwaltete Identitäten für Azure-Ressourcenauthentifizierung

Eine Data Factory kann einer [verwalteten Identität für Azure-Ressourcen](data-factory-service-identity.md) zugeordnet werden, die diese spezielle Data Factory darstellt. Sie können diese verwaltete Identität für die Authentifizierung einer verwalteten SQL-Instanz verwenden. Die angegebene Factory kann mithilfe dieser Identität auf Daten zugreifen und Daten aus der oder in die Datenbank kopieren.

Führen Sie die folgenden Schritte aus, um die Authentifizierung mit einer verwalteten Identität zu verwenden.

1. Führen Sie die unter [Bereitstellen eines Azure Active Directory-Administrators für Ihre verwaltete SQL-Datenbank-Instanz](../azure-sql/database/authentication-aad-configure.md#provision-azure-ad-admin-sql-managed-instance) beschriebenen Schritte aus.

2. [Erstellen Sie Anmeldungen](https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current) für die verwaltete Azure Data Factory-Identität. Stellen Sie in SQL Server Management Studio (SSMS) über ein SQL Server-Konto (ein **SysAdmin**-Konto) eine Verbindung mit Ihrer verwalteten Instanz her. Führen Sie in der **Masterdatenbank** den folgenden T-SQL-Befehl aus:

    ```sql
    CREATE LOGIN [your Data Factory name] FROM EXTERNAL PROVIDER
    ```

3. [Erstellen Sie für eine eigenständige Datenbank Benutzer](../azure-sql/database/authentication-aad-configure.md#create-contained-users-mapped-to-azure-ad-identities) für die verwaltete Azure Data Factory-Identität. Stellen Sie eine Verbindung mit der Datenbank her, aus der bzw. in die Sie Daten kopieren möchten, und führen Sie den folgenden T-SQL-Befehl aus: 
  
    ```sql
    CREATE USER [your Data Factory name] FROM EXTERNAL PROVIDER
    ```

4. Gewähren Sie der verwalteten Data Factory-Identität die notwendigen Berechtigungen, und gehen Sie dabei so vor wie für SQL-Benutzer und andere Benutzer. Führen Sie den folgenden Code aus. Weitere Optionen finden Sie in [diesem Dokument](https://docs.microsoft.com/sql/t-sql/statements/alter-role-transact-sql?view=azuresqldb-mi-current).

    ```sql
    ALTER ROLE [role name e.g. db_owner] ADD MEMBER [your Data Factory name]
    ```

5. Konfigurieren Sie einen mit der verwalteten SQL-Instanz verknüpften Dienst in Azure Data Factory.

**Beispiel: Verwenden der Authentifizierung der verwalteten Identität**

```json
{
    "name": "AzureSqlDbLinkedService",
    "properties": {
        "type": "AzureSqlMI",
        "typeProperties": {
            "connectionString": "Data Source=<hostname,port>;Initial Catalog=<databasename>;"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Dataset-Eigenschaften

Eine vollständige Liste mit den Abschnitten und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel zu Datasets. Dieser Abschnitt enthält eine Liste mit Eigenschaften, die vom Dataset der verwalteten SQL-Instanz unterstützt werden.

Zum Kopieren von Daten in die bzw. aus der verwaltete(n) SQL-Instanz werden die folgenden Eigenschaften unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft des Datasets muss auf **AzureSqlMITable** festgelegt werden. | Ja |
| schema | Name des Schemas. |Quelle: Nein, Senke: Ja  |
| table | Name der Tabelle/Ansicht. |Quelle: Nein, Senke: Ja  |
| tableName | Name der Tabelle/Ansicht mit Schema. Diese Eigenschaft wird aus Gründen der Abwärtskompatibilität weiterhin unterstützt. Verwenden Sie für eine neue Workload `schema` und `table`. | Quelle: Nein, Senke: Ja |

**Beispiel**

```json
{
    "name": "AzureSqlMIDataset",
    "properties":
    {
        "type": "AzureSqlMITable",
        "linkedServiceName": {
            "referenceName": "<SQL Managed Instance linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, retrievable during authoring > ],
        "typeProperties": {
            "schema": "<schema_name>",
            "table": "<table_name>"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschaften der Kopieraktivität

Eine vollständige Liste mit den Abschnitten und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Pipelines](concepts-pipelines-activities.md). Dieser Abschnitt enthält eine Liste mit Eigenschaften, die von der Quelle und der Senke der verwalteten SQL-Instanz unterstützt werden.

### <a name="sql-managed-instance-as-a-source"></a>Verwaltete SQL-Instanz als Quelle

Zum Kopieren von Daten aus der verwalteten SQL-Instanz werden im Abschnitt für die Kopieraktivität der Quelle die folgenden Eigenschaften unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft der Quelle der Kopieraktivität muss auf **SqlMISource** festgelegt werden. | Ja |
| sqlReaderQuery |Diese Eigenschaft verwendet die benutzerdefinierte SQL-Abfrage zum Lesen von Daten. z. B. `select * from MyTable`. |Nein |
| sqlReaderStoredProcedureName |Diese Eigenschaft ist der Name der gespeicherten Prozedur, die Daten aus der Quelltabelle liest. Die letzte SQL-Anweisung muss eine SELECT-Anweisung in der gespeicherten Prozedur sein. |Nein |
| storedProcedureParameters |Diese Parameter werden für die gespeicherte Prozedur verwendet.<br/>Zulässige Werte sind Namen oder Name-Wert-Paare. Die Namen und die Groß-/Kleinschreibung der Parameter müssen denen der Parameter der gespeicherten Prozedur entsprechen. |Nein |
| isolationLevel | Gibt das Sperrverhalten für Transaktionen für die SQL-Quelle an. Zulässige Werte sind: **ReadCommitted** (Standard), **ReadUncommitted**, **RepeatableRead**, **Serializable**, **Snapshot**. Weitere Informationen finden Sie in [dieser Dokumentation](https://docs.microsoft.com/dotnet/api/system.data.isolationlevel). | Nein |

**Beachten Sie folgende Punkte:**

- Wenn **sqlReaderQuery** für **SqlMISource** angegeben ist, führt die Kopieraktivität diese Abfrage für die Quelle der verwalteten SQL-Instanz aus, um die Daten abzurufen. Sie können auch eine gespeicherte Prozedur angeben, indem Sie **sqlReaderStoredProcedureName** und **storedProcedureParameters** angeben, sofern die gespeicherten Prozeduren Parameter verwenden.
- Ohne Angabe von **sqlReaderQuery** oder der Eigenschaft **sqlReaderStoredProcedureName** werden die im Abschnitt „structure“ des JSON-Codes des Datasets definierten Spalten zum Erstellen einer Abfrage verwendet. Die Abfrage `select column1, column2 from mytable` wird für die verwaltete SQL-Instanz ausgeführt. Falls die Datasetdefinition keinen Abschnitt „structure“ enthält, werden alle Spalten der Tabelle ausgewählt.

**Beispiel: Verwenden einer SQL-Abfrage**

```json
"activities":[
    {
        "name": "CopyFromAzureSqlMI",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SQL Managed Instance input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "SqlMISource",
                "sqlReaderQuery": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

**Beispiel: Verwenden einer gespeicherten Prozedur**

```json
"activities":[
    {
        "name": "CopyFromAzureSqlMI",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SQL Managed Instance input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "SqlMISource",
                "sqlReaderStoredProcedureName": "CopyTestSrcStoredProcedureWithParameters",
                "storedProcedureParameters": {
                    "stringData": { "value": "str3" },
                    "identifier": { "value": "$$Text.Format('{0:yyyy}', <datetime parameter>)", "type": "Int"}
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

**Definition der gespeicherten Prozedur**

```sql
CREATE PROCEDURE CopyTestSrcStoredProcedureWithParameters
(
    @stringData varchar(20),
    @identifier int
)
AS
SET NOCOUNT ON;
BEGIN
    select *
    from dbo.UnitTestSrcTable
    where dbo.UnitTestSrcTable.stringData != stringData
    and dbo.UnitTestSrcTable.identifier != identifier
END
GO
```

### <a name="sql-managed-instance-as-a-sink"></a>Verwaltete SQL-Instanz als Senke

> [!TIP]
> Weitere Informationen zum unterstützten Schreibverhalten, zu unterstützten Konfigurationen und zu bewährten Methoden finden Sie unter [Bewährte Methode zum Laden von Daten in eine verwaltete SQL-Instanz](#best-practice-for-loading-data-into-sql-managed-instance).

Zum Kopieren von Daten in die verwaltete SQL-Instanz werden im Abschnitt für die Kopieraktivität der Senke die folgenden Eigenschaften unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft der Senke der Kopieraktivität muss auf **SqlMISink** festgelegt werden. | Ja |
| preCopyScript |Diese Eigenschaft gibt eine SQL-Abfrage für die Kopieraktivität an, die vor dem Schreiben von Daten in die verwaltete SQL-Instanz ausgeführt werden soll. Sie wird pro Ausführung der Kopieraktivität nur einmal aufgerufen. Sie können diese Eigenschaft nutzen, um die vorab geladenen Daten zu bereinigen. |Nein |
| tableOption | Gibt an, ob die [Senkentabelle auf Basis des Quellschemas automatisch erstellt werden soll](copy-activity-overview.md#auto-create-sink-tables), wenn sie nicht vorhanden ist. Die automatische Tabellenerstellung wird nicht unterstützt, wenn die Senke die gespeicherte Prozedur angibt oder das gestaffelte Kopieren in der Kopieraktivität konfiguriert ist. Zulässige Werte: `none` (Standard), `autoCreate`. |Nein |
| sqlWriterStoredProcedureName | Der Name der gespeicherten Prozedur, die definiert, wie Quelldaten auf eine Zieltabelle angewandt werden. <br/>Diese gespeicherte Prozedur wird *pro Batch aufgerufen*. Für nur einmalig ausgeführte Vorgänge, die nicht mit Quelldaten in Zusammenhang stehen (etwa Löschen/Kürzen), verwenden Sie die `preCopyScript`-Eigenschaft.<br>Ein Beispiel finden Sie unter [Aufrufen einer gespeicherten Prozedur aus einer SQL-Senke](#invoke-a-stored-procedure-from-a-sql-sink). | Nein |
| storedProcedureTableTypeParameterName |Der Parametername des Tabellentyps, der in der gespeicherten Prozedur angegeben ist.  |Nein |
| sqlWriterTableType |Der Tabellentypname, der in der gespeicherten Prozedur verwendet werden soll. Die Kopieraktivität macht die verschobenen Daten in einer temporären Tabelle mit diesem Tabellentyp verfügbar. Der gespeicherte Prozedurcode kann dann die kopierten Daten mit vorhandenen Daten zusammenführen. |Nein |
| storedProcedureParameters |Parameter für die gespeicherte Prozedur.<br/>Zulässige Werte: Name-Wert-Paare. Die Namen und die Groß-/Kleinschreibung von Parametern müssen denen der Parameter der gespeicherten Prozedur entsprechen. | Nein |
| writeBatchSize |Anzahl der Zeilen, die *pro Batch* in die SQL-Tabelle eingefügt werden sollen.<br/>Zulässige Werte sind Integer-Werte für die Anzahl der Zeilen. Standardmäßig bestimmt Azure Data Factory die geeignete Batchgröße dynamisch anhand der Zeilengröße.  |Nein |
| writeBatchTimeout |Diese Eigenschaft gibt die Wartezeit für den Abschluss der Batcheinfügung an, bevor ein Timeout auftritt.<br/>Zulässige Werte werden für den Zeitraum verwendet. Ein Beispiel ist „00:30:00“. Das sind 30 Minuten. |Nein |

**Beispiel 1: Anfügen von Daten**

```json
"activities":[
    {
        "name": "CopyToAzureSqlMI",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<SQL Managed Instance output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SqlMISink",
                "tableOption": "autoCreate",
                "writeBatchSize": 100000
            }
        }
    }
]
```

**Beispiel 2: Aufrufen einer gespeicherten Prozedur während des Kopiervorgangs**

Weitere Informationen finden Sie unter [Aufrufen einer gespeicherten Prozedur aus einer SQL-MI-Senke](#invoke-a-stored-procedure-from-a-sql-sink).

```json
"activities":[
    {
        "name": "CopyToAzureSqlMI",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<SQL Managed Instance output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SqlMISink",
                "sqlWriterStoredProcedureName": "CopyTestStoredProcedureWithParameters",
                "storedProcedureTableTypeParameterName": "MyTable",
                "sqlWriterTableType": "MyTableType",
                "storedProcedureParameters": {
                    "identifier": { "value": "1", "type": "Int" },
                    "stringData": { "value": "str1" }
                }
            }
        }
    }
]
```

## <a name="best-practice-for-loading-data-into-sql-managed-instance"></a>Bewährte Methode zum Laden von Daten in eine verwaltete SQL-Instanz

Beim Kopieren von Daten in eine verwaltete SQL-Instanz ist möglicherweise ein anderes Schreibverhalten erforderlich:

- [Anfügen:](#append-data) Meine Quelldaten enthalten nur neue Datensätze.
- [Upsert:](#upsert-data) Meine Quelldaten umfassen sowohl Einfügungen als auch Aktualisierungen.
- [Überschreiben:](#overwrite-the-entire-table) Ich möchte die gesamte Dimensionstabelle jedes Mal neu laden.
- [Schreiben von Daten mit benutzerdefinierter Logik:](#write-data-with-custom-logic) Ich benötige eine zusätzliche Verarbeitung vor dem endgültigen Einfügen in die Zieltabelle. 

Informationen zur Konfiguration in Azure Data Factory und zu bewährten Methoden finden Sie in den entsprechenden Abschnitten.

### <a name="append-data"></a>Anfügen von Daten

Das Anfügen von Daten ist das Standardverhalten dieses Senkenconnectors für die verwaltete SQL-Instanz. Azure Data Factory führt eine Masseneinfügung aus, um Daten effizient in Ihre Tabelle zu schreiben. Sie können die Quelle und Senke in der Kopieraktivität entsprechend konfigurieren.

### <a name="upsert-data"></a>Durchführen von Upsert für Daten

**Option 1:** Wenn Sie große Datenmengen kopieren möchten, können Sie mithilfe der Kopieraktivität für alle Datensätze einen Massenladevorgang in eine Stagingtabelle ausführen. Führen Sie anschließend eine Aktivität einer gespeicherten Prozedur aus, um eine [MERGE](https://docs.microsoft.com/sql/t-sql/statements/merge-transact-sql?view=azuresqldb-mi-current)- oder INSERT/UPDATE-Anweisung in einem Schritt anzuwenden. 

Das Laden von Daten in eine temporäre Datenbanktabelle wird derzeit nicht nativ von der Kopieraktivität unterstützt. Ein erweitertes Verfahren für die Einrichtung mit mehreren Aktivitäten finden Sie unter [Optimize SQL Database Bulk Upsert scenarios](https://github.com/scoriani/azuresqlbulkupsert) (Optimieren von Szenarien mit upsert-Massenvorgängen in einer SQL-Datenbank). Das nachfolgende Beispiel veranschaulicht die Verwendung einer permanenten Tabelle als Stagingtabelle.

Sie können beispielsweise in Azure Data Factory eine Pipeline mit einer **Kopieraktivität** erstellen, die mit einer **Aktivität der gespeicherten Prozedur** verkettet ist. Erstere kopiert Daten aus Ihrem Quellspeicher in eine Stagingtabelle der verwalteten Azure SQL-Instanz (z. B. **UpsertStagingTable** als Tabellenname im Dataset). Die Aktivität der gespeicherten Prozedur ruft dann eine gespeicherte Prozedur auf, um die Quelldaten aus der Stagingtabelle mit der Zieltabelle zusammenzuführen und die Stagingtabelle zu bereinigen.

![Upsert](./media/connector-azure-sql-database/azure-sql-database-upsert.png)

Definieren Sie in Ihrer Datenbank eine gespeicherte Prozedur mit MERGE-Logik (wie im folgenden Beispiel), auf die über die vorherige Aktivität der gespeicherten Prozedur gezeigt wird. Angenommen, das Ziel ist die Tabelle **Marketing** mit den drei Spalten: **ProfileID**, **State** und **Category**. Führen Sie den Upsert-Vorgang basierend auf der Spalte **ProfileID** aus.

```sql
CREATE PROCEDURE [dbo].[spMergeData]
AS
BEGIN
    MERGE TargetTable AS target
    USING UpsertStagingTable AS source
    ON (target.[ProfileID] = source.[ProfileID])
    WHEN MATCHED THEN
        UPDATE SET State = source.State
    WHEN NOT matched THEN
        INSERT ([ProfileID], [State], [Category])
      VALUES (source.ProfileID, source.State, source.Category);
    
    TRUNCATE TABLE UpsertStagingTable
END
```

**Option 2:** Sie können [eine gespeicherte Prozedur in der Kopieraktivität aufrufen](#invoke-a-stored-procedure-from-a-sql-sink). Dabei wird jeder Batch in der Quelltabelle (entsprechend der `writeBatchSize`-Eigenschaft) ausgeführt, statt Masseneinfügen als Standardansatz in der Kopieraktivität zu verwenden.

### <a name="overwrite-the-entire-table"></a>Überschreiben der gesamten Tabelle

Sie können die **preCopyScript**-Eigenschaft in einer Kopieraktivitätssenke konfigurieren. In diesem Fall führt Azure Data Factory für jede ausgeführte Kopieraktivität zuerst das Skript aus. Dann wird der Kopiervorgang ausgeführt, um die Daten einzufügen. Beispiel: Um die gesamte Tabelle mit den neuesten Daten zu überschreiben, geben Sie ein Skript an, um zunächst alle Datensätze zu löschen, bevor die neuen Daten durch Massenladen aus der Quelle eingefügt werden.

### <a name="write-data-with-custom-logic"></a>Schreiben von Daten mit benutzerdefinierter Logik

Die Schritte zum Schreiben von Daten mit benutzerdefinierter Logik ähneln den im Abschnitt [Durchführen von Upsert für Daten](#upsert-data) beschriebenen Schritten. Wenn vor dem Einfügen von Quelldaten in die Zieltabelle weitere Verarbeitungsschritte erforderlich sind, können Sie die Daten in eine Stagingtabelle laden und dann Aktivitäten einer gespeicherten Prozedur oder eine gespeicherte Prozedur in der Kopieraktivität der Senke aufrufen, um die Daten anzuwenden.

## <a name="invoke-a-stored-procedure-from-a-sql-sink"></a><a name="invoke-a-stored-procedure-from-a-sql-sink"></a> Aufrufen der gespeicherten Prozedur von der SQL-Senke

Beim Kopieren von Daten in eine verwaltete SQL-Instanz können Sie auch eine vom Benutzer angegebene gespeicherte Prozedur mit zusätzlichen Parametern für jeden Batch der Quelltabelle konfigurieren und aufrufen. Das Feature der gespeicherten Prozedur nutzt [Tabellenwertparameter](https://msdn.microsoft.com/library/bb675163.aspx).

Sie können eine gespeicherte Prozedur nutzen, wenn integrierte Kopiermechanismen nicht den Zweck erfüllen. Ein Beispiel hierfür ist ein Szenario, in dem Sie vor dem endgültigen Einfügen von Quelldaten in die Zieltabelle eine zusätzliche Verarbeitung anwenden möchten. Beispiele für eine zusätzliche Verarbeitung sind das Zusammenführen von Spalten, das Suchen nach zusätzlichen Werten und das Einfügen in mehr als eine Tabelle.

Das folgende Beispiel zeigt, wie Sie eine gespeicherte Prozedur verwenden, um einen einfachen Upsert-Vorgang in eine Tabelle in der SQL Server-Datenbank auszuführen. Im Beispiel wird angenommen, dass Eingabedaten vorhanden sind und die Senkentabelle **Marketing** drei Spalten enthält: **ProfileID**, **State** und **Category**. Führen Sie den Upsert-Vorgang basierend auf der Spalte **ProfileID** aus, und wenden Sie ihn nur auf die Kategorie „ProductA“ an.

1. Definieren Sie in Ihrer Datenbank den Tabellentyp mit dem gleichen Namen wie **sqlWriterTableType**. Das Schema des Tabellentyps muss mit dem Schema übereinstimmen, das von den Eingabedaten zurückgegeben wird.

    ```sql
    CREATE TYPE [dbo].[MarketingType] AS TABLE(
        [ProfileID] [varchar](256) NOT NULL,
        [State] [varchar](256) NOT NULL,
        [Category] [varchar](256) NOT NULL
    )
    ```

2. Definieren Sie die gespeicherte Prozedur in Ihrer Datenbank mit demselben Namen wie **SqlWriterStoredProcedureName**. Sie verarbeitet die Eingabedaten aus der angegebenen Quelle und führt sie mit der Ausgabetabelle zusammen. Der Parametername des Tabellentyps in der gespeicherten Prozedur entspricht dem im Dataset definierten **tableName**.

    ```sql
    CREATE PROCEDURE spOverwriteMarketing @Marketing [dbo].[MarketingType] READONLY, @category varchar(256)
    AS
    BEGIN
    MERGE [dbo].[Marketing] AS target
    USING @Marketing AS source
    ON (target.ProfileID = source.ProfileID and target.Category = @category)
    WHEN MATCHED THEN
        UPDATE SET State = source.State
    WHEN NOT MATCHED THEN
        INSERT (ProfileID, State, Category)
        VALUES (source.ProfileID, source.State, source.Category);
    END
    ```

3. Definieren Sie in Azure Data Factory den Abschnitt **SQL-MI-Senke** in der Copy-Aktivität wie folgt:

    ```json
    "sink": {
        "type": "SqlMISink",
        "sqlWriterStoredProcedureName": "spOverwriteMarketing",
        "storedProcedureTableTypeParameterName": "Marketing",
        "sqlWriterTableType": "MarketingType",
        "storedProcedureParameters": {
            "category": {
                "value": "ProductA"
            }
        }
    }
    ```

## <a name="data-type-mapping-for-sql-managed-instance"></a>Datentypzuordnung für eine verwaltete SQL-Instanz

Beim Kopieren von Daten in eine bzw. aus einer verwalteten SQL-Instanz werden die folgenden Zuordnungen von Datentypen der verwalteten SQL-Instanz zu Azure Data Factory-Zwischendatentypen verwendet. Informationen dazu, wie die Kopieraktivität das Quellschema und den Datentyp zur Senke zuordnet, finden Sie unter [Schema- und Datentypzuordnungen](copy-activity-schema-and-type-mapping.md).

| Datentyp der verwalteten SQL-Instanz | Azure Data Factory-Zwischendatentyp |
|:--- |:--- |
| BIGINT |Int64 |
| BINARY |Byte[] |
| bit |Boolean |
| char |String, Char[] |
| date |Datetime |
| Datetime |Datetime |
| datetime2 |Datetime |
| Datetimeoffset |DateTimeOffset |
| Decimal |Decimal |
| FILESTREAM attribute (varbinary(max)) |Byte[] |
| Float |Double |
| image |Byte[] |
| INT |Int32 |
| money |Decimal |
| NCHAR |String, Char[] |
| ntext |String, Char[] |
| NUMERIC |Decimal |
| NVARCHAR |String, Char[] |
| real |Single |
| rowversion |Byte[] |
| smalldatetime |Datetime |
| SMALLINT |Int16 |
| SMALLMONEY |Decimal |
| sql_variant |Object |
| text |String, Char[] |
| time |TimeSpan |
| timestamp |Byte[] |
| TINYINT |Int16 |
| UNIQUEIDENTIFIER |Guid |
| varbinary |Byte[] |
| varchar |String, Char[] |
| Xml |Xml |

>[!NOTE]
> Für Datentypen, die dem Zwischendatentyp „Decimal“ zugeordnet sind, unterstützt die Kopieraktivität derzeit eine Genauigkeit von bis zu 28. Wenn Ihre Daten eine höhere Genauigkeit als 28 erfordern, erwägen Sie, sie per SQL-Abfrage in eine Zeichenfolge zu konvertieren.

## <a name="lookup-activity-properties"></a>Eigenschaften der Lookup-Aktivität

Ausführliche Informationen zu den Eigenschaften finden Sie unter [Lookup-Aktivität](control-flow-lookup-activity.md).

## <a name="getmetadata-activity-properties"></a>Eigenschaften der GetMetadata-Aktivität

Ausführliche Informationen zu den Eigenschaften finden Sie unter [GetMetadata-Aktivität](control-flow-get-metadata-activity.md). 

## <a name="using-always-encrypted"></a>Verwenden von Always Encrypted

Verwenden Sie zum Kopieren von Daten mit [Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine?view=azuresqldb-mi-current) in eine bzw. aus einer verwalteten SQL-Instanz den [generischen ODBC-Connector](connector-odbc.md) und den SQL Server-ODBC-Treiber über die selbstgehostete Integration Runtime. Always Encrypted wird von diesem Connector für verwaltete Azure SQL-Instanzen derzeit nicht unterstützt. 

Dies gilt insbesondere in folgenden Fällen:

1. Richten Sie eine selbstgehostete Integration Runtime ein, sofern noch nicht vorhanden. Im Artikel [Selbstgehostete Integration Runtime](create-self-hosted-integration-runtime.md) finden Sie Details.

2. Laden Sie [hier](https://docs.microsoft.com/sql/connect/odbc/download-odbc-driver-for-sql-server?view=azuresqldb-mi-current) den 64-Bit-ODBC-Treiber für SQL Server herunter, und installieren Sie ihn auf dem Computer mit der Integration Runtime. Weitere Informationen zur Funktionsweise dieses Treibers finden Sie unter [Verwendung von Always Encrypted mit dem ODBC-Treiber für SQL Server](https://docs.microsoft.com/sql/connect/odbc/using-always-encrypted-with-the-odbc-driver?view=azuresqldb-mi-current#using-the-azure-key-vault-provider).

3. Erstellen Sie einen verknüpften Dienst mit dem ODBC-Typ für die Verbindung mit Ihrer SQL-Datenbank. Nachfolgend sind einige Beispiele gezeigt:

    - Zur Verwendung der **SQL-Authentifizierung**: Geben Sie die ODBC-Verbindungszeichenfolge wie unten gezeigt an, und wählen Sie die **Standardauthentifizierung** zum Festlegen von Benutzername und Kennwort aus.

        ```
        Driver={ODBC Driver 17 for SQL Server};Server=<serverName>;Database=<databaseName>;ColumnEncryption=Enabled;KeyStoreAuthentication=KeyVaultClientSecret;KeyStorePrincipalId=<servicePrincipalKey>;KeyStoreSecret=<servicePrincipalKey>
        ```

    - Zur Verwendung der **Authentifizierung der verwalteten Identität über Data Factory**: 

        1. Stellen Sie sicher, dass diese [Voraussetzungen](#managed-identity) erfüllt sind, um einen Datenbankbenutzer für die verwaltete Identität zu erstellen, und weisen Sie in Ihrer Datenbank die entsprechende Rolle zu.
        2. Geben Sie im verknüpften Dienst die ODBC-Verbindungszeichenfolge wie unten gezeigt an, und wählen Sie die **anonyme Authentifizierung** aus, da die Verbindungszeichenfolge selbst `Authentication=ActiveDirectoryMsi` angibt.

        ```
        Driver={ODBC Driver 17 for SQL Server};Server=<serverName>;Database=<databaseName>;ColumnEncryption=Enabled;KeyStoreAuthentication=KeyVaultClientSecret;KeyStorePrincipalId=<servicePrincipalKey>;KeyStoreSecret=<servicePrincipalKey>; Authentication=ActiveDirectoryMsi;
        ```

4. Erstellen Sie ein Dataset und eine Kopieraktivität mit dem ODBC-Typ. Weitere Informationen finden Sie im Artikel zum [ODBC-Connector](connector-odbc.md).

## <a name="next-steps"></a>Nächste Schritte
Eine Liste der Datenspeicher, die als Quellen und Senken für die Kopieraktivität in Azure Data Factory unterstützt werden, finden Sie unter [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).
