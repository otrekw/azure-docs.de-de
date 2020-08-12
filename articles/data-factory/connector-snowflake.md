---
title: Kopieren von Daten aus und in Snowflake
description: Erfahren Sie, wie Sie Daten mithilfe von Azure Data Factory aus und in Snowflake kopieren.
services: data-factory
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 07/30/2020
ms.openlocfilehash: 48248b07b64278d5c8d4f297bf83df813aa486fe
ms.sourcegitcommit: 3d56d25d9cf9d3d42600db3e9364a5730e80fa4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/03/2020
ms.locfileid: "87529499"
---
# <a name="copy-data-from-and-to-snowflake-by-using-azure-data-factory"></a>Kopieren von Daten aus und in Snowflake mithilfe von Azure Data Factory

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

In diesem Artikel wird beschrieben, wie Sie die Kopieraktivität in Azure Data Factory verwenden, um Daten aus und in Snowflake zu kopieren. Weitere Informationen zu Data Factory finden Sie im [Einführungsartikel](introduction.md).

## <a name="supported-capabilities"></a>Unterstützte Funktionen

Dieser Snowflake-Connector wird für die folgenden Aktivitäten unterstützt:

- [Kopieraktivität](copy-activity-overview.md) mit einer Tabelle zur [unterstützten Quellen/Senken-Matrix](copy-activity-overview.md)
- [Lookup-Aktivität](control-flow-lookup-activity.md)

Für die Kopieraktivität unterstützt dieser Snowflake-Connector die folgenden Funktionen:

- Kopieren von Daten aus Snowflake unter Verwendung des Snowflake-Befehls [COPY into [Speicherort]](https://docs.snowflake.com/en/sql-reference/sql/copy-into-location.html), um optimale Leistung zu erzielen.
- Kopieren von Daten in Snowflake unter Verwendung des Snowflake-Befehls [COPY into [Tabelle]](https://docs.snowflake.com/en/sql-reference/sql/copy-into-table.html), um optimale Leistung zu erzielen. Snowflake in Azure wird unterstützt.

## <a name="get-started"></a>Erste Schritte

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Die folgenden Abschnitte enthalten Details zu Eigenschaften, die zum Definieren von Data Factory-Entitäten speziell für einen Snowflake-Connector verwendet werden.

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts

Die folgenden Eigenschaften werden für den mit Snowflake verknüpften Dienst unterstützt.

| Eigenschaft         | BESCHREIBUNG                                                  | Erforderlich |
| :--------------- | :----------------------------------------------------------- | :------- |
| type             | Die „type“-Eigenschaft muss auf **Snowflake** festgelegt werden.              | Ja      |
| connectionString | Gibt die erforderlichen Informationen für Verbindungen mit der Snowflake-Instanz an. Sie können das Kennwort oder die gesamte Verbindungszeichenfolge in Azure Key Vault speichern. Ausführlichere Informationen finden Sie in den Beispielen unter der Tabelle sowie im Artikel [Speichern von Anmeldeinformationen in Azure Key Vault](store-credentials-in-key-vault.md).<br><br>Einige typische Einstellungen:<br>- **Kontoname:** Der [vollständige Kontoname](https://docs.snowflake.net/manuals/user-guide/connecting.html#your-snowflake-account-name) Ihres Snowflake-Kontos (einschließlich zusätzlicher Segmente zur Identifizierung von Region und Cloudplattform), z. B. „xy12345.east-us-2.azure“.<br/>- **Benutzername:** Der Anmeldename des Benutzers für die Verbindung.<br>- **Kennwort:** Das Kennwort für den Benutzer.<br>- **Datenbank:** Die Standarddatenbank, die nach dem Herstellen der Verbindung verwendet werden soll. Dabei sollte es sich um eine vorhandene Datenbank handeln, für die die angegebene Rolle über die erforderlichen Berechtigungen verfügt.<br>- **Warehouse:** Das virtuelle Warehouse, das nach dem Herstellen der Verbindung verwendet werden soll. Dabei sollte es sich um ein vorhandenes Warehouse handeln, für das die angegebene Rolle über die erforderlichen Berechtigungen verfügt.<br>- **Rolle:** Die Standardrolle aus der Zugriffssteuerung, die in der Snowflake-Sitzung verwendet werden soll. Die angegebene Rolle sollte vorhanden und dem angegebenen Benutzer bereits zugewiesen sein. Die Standardrolle ist PUBLIC. | Ja      |
| connectVia       | Die [Integration Runtime](concepts-integration-runtime.md), die zum Herstellen einer Verbindung mit dem Datenspeicher verwendet wird. Sie können die Azure Integration Runtime oder eine selbstgehostete Integration Runtime verwenden (wenn sich der Datenspeicher in einem privaten Netzwerk befindet). Wenn kein Wert angegeben ist, wird die standardmäßige Azure Integration Runtime verwendet. | Nein       |

**Beispiel:**

```json
{
    "name": "SnowflakeLinkedService",
    "properties": {
        "type": "Snowflake",
        "typeProperties": {
            "connectionString": "jdbc:snowflake://<accountname>.snowflakecomputing.com/?user=<username>&password=<password>&db=<database>&warehouse=<warehouse>&role=<myRole>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Kennwort in Azure Key Vault:**

```json
{
    "name": "SnowflakeLinkedService",
    "properties": {
        "type": "Snowflake",
        "typeProperties": {
            "connectionString": "jdbc:snowflake://<accountname>.snowflakecomputing.com/?user=<username>&db=<database>&warehouse=<warehouse>&role=<myRole>",
            "password": {
                "type": "AzureKeyVaultSecret",
                "store": { 
                    "referenceName": "<Azure Key Vault linked service name>",
                    "type": "LinkedServiceReference"
                }, 
                "secretName": "<secretName>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Dataset-Eigenschaften

Eine vollständige Liste mit den Abschnitten und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel zu [Datasets](concepts-datasets-linked-services.md). 

Folgende Eigenschaften werden für das Snowflake-Dataset unterstützt.

| Eigenschaft  | BESCHREIBUNG                                                  | Erforderlich                    |
| :-------- | :----------------------------------------------------------- | :-------------------------- |
| type      | Die „type“-Eigenschaft des Datasets muss auf **SnowflakeTable** festgelegt werden. | Ja                         |
| schema | Name des Schemas. |Quelle: Nein, Senke: Ja  |
| table | Name der Tabelle/Ansicht. |Quelle: Nein, Senke: Ja  |

**Beispiel:**

```json
{
    "name": "SnowflakeDataset",
    "properties": {
        "type": "SnowflakeTable",
        "typeProperties": {
            "schema": "<Schema name for your Snowflake database>",
            "table": "<Table name for your Snowflake database>"
        },
        "schema": [ < physical schema, optional, retrievable during authoring > ],
        "linkedServiceName": {
            "referenceName": "<name of linked service>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschaften der Kopieraktivität

Eine vollständige Liste mit den Abschnitten und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Pipelines](concepts-pipelines-activities.md). Dieser Abschnitt enthält eine Liste der Eigenschaften, die von der Snowflake-Quelle und -Senke unterstützt werden.

### <a name="snowflake-as-the-source"></a>Snowflake als Quelle

Der Snowflake-Connector verwendet den Snowflake-Befehl [COPY into [Speicherort]](https://docs.snowflake.com/en/sql-reference/sql/copy-into-location.html), um optimale Leistung zu erzielen.

Wenn der Senkendatenspeicher und das Format vom Snowflake-Befehl „COPY“ nativ unterstützt werden, können Sie mit der Kopieraktivität Kopiervorgänge direkt aus Snowflake in die Senke durchführen. Details finden Sie unter [Direktes Kopieren aus Snowflake](#direct-copy-from-snowflake). Verwenden Sie andernfalls das integrierte [gestaffelte Kopieren aus Snowflake](#staged-copy-from-snowflake).

Beim Kopieren von Daten aus Snowflake werden die folgenden Eigenschaften im Abschnitt **source** der Kopieraktivität unterstützt.

| Eigenschaft                     | BESCHREIBUNG                                                  | Erforderlich |
| :--------------------------- | :----------------------------------------------------------- | :------- |
| type                         | Die „type“-Eigenschaft der Quelle der Kopieraktivität muss auf **SnowflakeSource** festgelegt werden. | Ja      |
| Abfrage          | Gibt die SQL-Abfrage an, mit der Daten aus Snowflake gelesen werden.<br>Die Ausführung der gespeicherten Prozedur wird nicht unterstützt. | Nein       |
| exportSettings | Erweiterte Einstellungen, die zum Abrufen von Daten aus Snowflake verwendet werden. Sie können die vom Befehl „COPY into“ unterstützten Einstellungen konfigurieren, die Data Factory beim Aufrufen der Anweisung durchläuft. | Nein       |
| ***Unter `exportSettings`:*** |  |  |
| type | Der Typ des Exportbefehls, festgelegt auf **SnowflakeExportCopyCommand**. | Ja |
| additionalCopyOptions | Zusätzliche Kopieroptionen, die als Wörterbuch mit Schlüssel-Wert-Paaren bereitgestellt werden. Beispiele: MAX_FILE_SIZE, OVERWRITE. Weitere Informationen finden Sie unter [Snowflake Copy Options](https://docs.snowflake.com/en/sql-reference/sql/copy-into-location.html#copy-options-copyoptions) (Snowflake-Kopieroptionen). | Nein |
| additionalFormatOptions | Zusätzliche Dateiformatoptionen, die im Befehl „COPY“ als Wörterbuch mit Schlüssel-Wert-Paaren bereitgestellt werden. Beispiele: DATE_FORMAT, TIME_FORMAT, TIMESTAMP_FORMAT. Weitere Informationen finden Sie unter [Snowflake Format Type Options](https://docs.snowflake.com/en/sql-reference/sql/copy-into-location.html#format-type-options-formattypeoptions) (Snowflake-Formattypoptionen). | Nein |

#### <a name="direct-copy-from-snowflake"></a>Direktes Kopieren aus Snowflake

Wenn der Senkendatenspeicher und das Format die in diesem Abschnitt beschriebenen Kriterien erfüllen, können Sie mit der Kopieraktivität Kopiervorgänge direkt aus Snowflake in die Senke durchführen. Data Factory überprüft die Einstellungen und gibt bei der Ausführung der Kopieraktivität einen Fehler aus, wenn die folgenden Kriterien nicht erfüllt werden:

- Der **verknüpfte Senkendienst** ist [**Azure Blob Storage**](connector-azure-blob-storage.md) mit **SAS**-Authentifizierung.

- Das **Senkendatenformat** lautet **Parquet**, **Durch Trennzeichen getrennter Text** oder **JSON** mit den folgenden Konfigurationen:

    - Beim Format **Parquet** wird einer der Komprimierungscodecs **None**, **Snappy** oder **Lzo** verwendet.
    - Beim Format **Durch Trennzeichen getrennter Text**:
        - `rowDelimiter` ist **\r\n** oder ein beliebiges einzelnes Zeichen.
        - `compression` kann auf **keine Komprimierung** oder auf **gzip**, **bzip2** oder **deflate** festgelegt sein.
        - `encodingName` wird als Standardwert übernommen oder ist auf **utf-8** festgelegt.
        - `quoteChar` ist als **doppeltes Anführungszeichen**, **einfaches Anführungszeichen** oder als **leere Zeichenfolge** (kein Anführungszeichen) festgelegt.
    - Für das **JSON**-Format unterstützt direktes Kopieren nur den Fall, dass die Snowflake-Quelltabelle oder das Abfrageergebnis nur eine einzelne Spalte hat und der Datentyp dieser Spalte **VARIANT**, **OBJECT** oder **ARRAY** ist.
        - `compression` kann auf **keine Komprimierung** oder auf **gzip**, **bzip2** oder **deflate** festgelegt sein.
        - `encodingName` wird als Standardwert übernommen oder ist auf **utf-8** festgelegt.
        - `filePattern` in der Senke der Kopieraktivität wird auf dem Standardwert belassen oder auf **setOfObjects** festgelegt.

- In der Quelle der Kopieraktivität ist `additionalColumns` nicht angegeben.
- Die Spaltenzuordnung ist nicht angegeben.

**Beispiel:**

```json
"activities":[
    {
        "name": "CopyFromSnowflake",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Snowflake input dataset name>",
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
                "type": "SnowflakeSource",
                "sqlReaderQuery": "SELECT * FROM MyTable",
                "exportSettings": {
                    "type": "SnowflakeExportCopyCommand",
                    "additionalCopyOptions": {
                        "MAX_FILE_SIZE": "64000000",
                        "OVERWRITE": true
                    },
                    "additionalFormatOptions": {
                        "DATE_FORMAT": "'MM/DD/YYYY'"
                    }
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

#### <a name="staged-copy-from-snowflake"></a>Gestaffeltes Kopieren aus Snowflake

Wenn der Senkendatenspeicher oder das Format nicht nativ mit dem Snowflake-Befehl „COPY“ kompatibel ist (wie im letzten Abschnitt beschrieben), aktivieren Sie mithilfe einer Azure Blob Storage-Zwischeninstanz das integrierte gestaffelte Kopieren. Das gestaffelte Kopieren bietet auch einen höheren Durchsatz. Data Factory exportiert Daten aus Snowflake in den Stagingspeicher, kopiert dann die Daten in die Senke und bereinigt schließlich die temporären Daten aus dem Stagingspeicher. Ausführliche Informationen zum Kopieren von Daten mithilfe von Staging finden Sie unter [Gestaffeltes Kopieren](copy-activity-performance-features.md#staged-copy).

Um diese Funktion verwenden zu können, erstellen Sie einen [mit Azure Blob Storage verknüpften Dienst](connector-azure-blob-storage.md#linked-service-properties), der auf das Azure Storage-Konto als Stagingzwischenspeicher verweist. Geben Sie dann die Eigenschaften `enableStaging` und `stagingSettings` in der Kopieraktivität an.

> [!NOTE]
> Der mit Azure Blob Storage verknüpfte Stagingdienst muss die SAS-Authentifizierung (Shared Access Signature) verwenden, die für den Snowflake-Befehl „COPY“ erforderlich ist. 

**Beispiel:**

```json
"activities":[
    {
        "name": "CopyFromSnowflake",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Snowflake input dataset name>",
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
                "type": "SnowflakeSource",
                "sqlReaderQuery": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            },
            "enableStaging": true,
            "stagingSettings": {
                "linkedServiceName": {
                    "referenceName": "MyStagingBlob",
                    "type": "LinkedServiceReference"
                },
                "path": "mystagingpath"
            }
        }
    }
]
```

### <a name="snowflake-as-sink"></a>Snowflake als Senke

Der Snowflake-Connector verwendet den Snowflake-Befehl [COPY into [Tabelle]](https://docs.snowflake.com/en/sql-reference/sql/copy-into-table.html), um optimale Leistung zu erzielen. Er unterstützt das Schreiben von Daten in Snowflake in Azure.

Wenn der Quelldatenspeicher und das Format vom Snowflake-Befehl „COPY“ nativ unterstützt werden, können Sie mit der Kopieraktivität Kopiervorgänge direkt aus der Quelle in Snowflake durchführen. Details finden Sie unter [Direktes Kopieren in Snowflake](#direct-copy-to-snowflake). Verwenden Sie andernfalls das integrierte [gestaffelte Kopieren in Snowflake](#staged-copy-to-snowflake).

Beim Kopieren von Daten in Snowflake werden die folgenden Eigenschaften im Abschnitt **sink** der Kopieraktivität unterstützt.

| Eigenschaft          | BESCHREIBUNG                                                  | Erforderlich                                      |
| :---------------- | :----------------------------------------------------------- | :-------------------------------------------- |
| type              | Die „type“-Eigenschaft der Senke der Kopieraktivität, festgelegt auf **SnowflakeSink**. | Ja                                           |
| preCopyScript     | Geben Sie eine SQL-Abfrage an, die bei jeder Ausführung von der Kopieraktivität ausgeführt werden soll, bevor Daten in Snowflake geschrieben werden. Sie können diese Eigenschaft nutzen, um vorab geladene Daten zu bereinigen. | Nein                                            |
| importSettings | Erweiterte Einstellungen, die zum Schreiben von Daten in Snowflake verwendet werden. Sie können die vom Befehl „COPY into“ unterstützten Einstellungen konfigurieren, die Data Factory beim Aufrufen der Anweisung durchläuft. | Nein |
| ***Unter `importSettings`:*** |                                                              |  |
| type | Der Typ des Importbefehls, festgelegt auf **SnowflakeImportCopyCommand**. | Ja |
| additionalCopyOptions | Zusätzliche Kopieroptionen, die als Wörterbuch mit Schlüssel-Wert-Paaren bereitgestellt werden. Beispiele: ON_ERROR, FORCE, LOAD_UNCERTAIN_FILES. Weitere Informationen finden Sie unter [Snowflake Copy Options](https://docs.snowflake.com/en/sql-reference/sql/copy-into-table.html#copy-options-copyoptions) (Snowflake-Kopieroptionen). | Nein |
| additionalFormatOptions | Zusätzliche Dateiformatoptionen, die im Befehl „COPY“ als Wörterbuch mit Schlüssel-Wert-Paaren bereitgestellt werden. Beispiele: DATE_FORMAT, TIME_FORMAT, TIMESTAMP_FORMAT. Weitere Informationen finden Sie unter [Snowflake Format Type Options](https://docs.snowflake.com/en/sql-reference/sql/copy-into-table.html#format-type-options-formattypeoptions) (Snowflake-Formattypoptionen). | Nein |

#### <a name="direct-copy-to-snowflake"></a>Direktes Kopieren in Snowflake

Wenn der Quelldatenspeicher und das Format die in diesem Abschnitt beschriebenen Kriterien erfüllen, können Sie mit der Kopieraktivität Kopiervorgänge direkt aus der Quelle in Snowflake durchführen. Azure Data Factory überprüft die Einstellungen und gibt bei der Ausführung der Kopieraktivität einen Fehler aus, wenn die folgenden Kriterien nicht erfüllt werden:

- Der **verknüpfte Quelldienst** ist [**Azure Blob Storage**](connector-azure-blob-storage.md) mit **SAS**-Authentifizierung.

- Das **Quelldatenformat** lautet **Parquet**, **Durch Trennzeichen getrennter Text** oder **JSON** mit den folgenden Konfigurationen:

    - Beim Format **Parquet** wird einer der Komprimierungscodecs **None** oder **Snappy** verwendet.

    - Beim Format **Durch Trennzeichen getrennter Text**:
        - `rowDelimiter` ist **\r\n** oder ein beliebiges einzelnes Zeichen. Wenn das Zeilentrennzeichen nicht „\r\n“ ist, muss `firstRowAsHeader` auf **false** festgelegt sein. `skipLineCount` ist nicht angegeben.
        - `compression` kann auf **keine Komprimierung** oder auf **gzip**, **bzip2** oder **deflate** festgelegt sein.
        - `encodingName` wird als Standardwert übernommen oder ist auf „UTF-8“, „UTF-16“, „UTF-16BE“, „UTF-32“, „UTF-32BE“, „BIG5“, „EUC-JP“, „EUC-KR“, „GB18030“, „ISO-2022-JP“, „ISO-2022-KR“, „ISO-8859-1“, „ISO-8859-2“, „ISO-8859-5“, „ISO-8859-6“, „ISO-8859-7“, „ISO-8859-8“, „ISO-8859-9“, „WINDOWS-1250“, „WINDOWS-1251“, „WINDOWS-1252“, „WINDOWS-1253“, „WINDOWS-1254“ oder „WINDOWS-1255“ festgelegt.
        - `quoteChar` ist als **doppeltes Anführungszeichen**, **einfaches Anführungszeichen** oder als **leere Zeichenfolge** (kein Anführungszeichen) festgelegt.
    - Für das **JSON**-Format unterstützt direktes Kopieren nur den Fall, dass die Snowflake-Senkentabelle nur eine einzelne Spalte hat und der Datentyp dieser Spalte **VARIANT**, **OBJECT** oder **ARRAY** ist.
        - `compression` kann auf **keine Komprimierung** oder auf **gzip**, **bzip2** oder **deflate** festgelegt sein.
        - `encodingName` wird als Standardwert übernommen oder ist auf **utf-8** festgelegt.
        - Die Spaltenzuordnung ist nicht angegeben.

- In der Quelle der Kopieraktivität: 

   -  `additionalColumns` ist nicht angegeben.
   - Wenn es sich bei der Quelle um einen Ordner handelt, wird `recursive` auf TRUE festgelegt.
   - `prefix`, `modifiedDateTimeStart` und `modifiedDateTimeEnd` sind nicht angegeben.

**Beispiel:**

```json
"activities":[
    {
        "name": "CopyToSnowflake",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Snowflake output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SnowflakeSink",
                "importSettings": {
                    "type": "SnowflakeImportCopyCommand",
                    "copyOptions": {
                        "FORCE": "TRUE",
                        "ON_ERROR": "SKIP_FILE",
                    },
                    "fileFormatOptions": {
                        "DATE_FORMAT": "YYYY-MM-DD",
                    }
                }
            }
        }
    }
]
```

#### <a name="staged-copy-to-snowflake"></a>Gestaffeltes Kopieren in Snowflake

Wenn der Senkendatenspeicher oder das Format nicht nativ mit dem Snowflake-Befehl „COPY“ kompatibel ist (wie im letzten Abschnitt beschrieben), aktivieren Sie mithilfe einer Azure Blob Storage-Zwischeninstanz das integrierte gestaffelte Kopieren. Das gestaffelte Kopieren bietet auch einen höheren Durchsatz. Data Factory konvertiert die Daten automatisch, damit das Datenformat dem von Snowflake entspricht. Dann wird der Befehl „COPY“ aufgerufen, um die Daten in Snowflake zu laden. Abschließend werden Sie die temporären Daten in Blob Storage bereinigt. Ausführliche Informationen zum Kopieren von Daten mithilfe von Staging finden Sie unter [Gestaffeltes Kopieren](copy-activity-performance-features.md#staged-copy).

Um diese Funktion verwenden zu können, erstellen Sie einen [mit Azure Blob Storage verknüpften Dienst](connector-azure-blob-storage.md#linked-service-properties), der auf das Azure Storage-Konto als Stagingzwischenspeicher verweist. Geben Sie dann die Eigenschaften `enableStaging` und `stagingSettings` in der Kopieraktivität an.

> [!NOTE]
> Der mit Azure Blob Storage verknüpfte Stagingdienst muss die SAS-Authentifizierung (Shared Access Signature) verwenden, die für den Snowflake-Befehl „COPY“ erforderlich ist.

**Beispiel:**

```json
"activities":[
    {
        "name": "CopyToSnowflake",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Snowflake output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SnowflakeSink"
            },
            "enableStaging": true,
            "stagingSettings": {
                "linkedServiceName": {
                    "referenceName": "MyStagingBlob",
                    "type": "LinkedServiceReference"
                },
                "path": "mystagingpath"
            }
        }
    }
]
```


## <a name="lookup-activity-properties"></a>Eigenschaften der Lookup-Aktivität

Weitere Informationen zu den Eigenschaften finden Sie unter [Lookup-Aktivität in Azure Data Factory](control-flow-lookup-activity.md).

## <a name="next-steps"></a>Nächste Schritte

Eine Liste der Datenspeicher, die als Quellen und Senken für die Kopieraktivität in Data Factory unterstützt werden, finden Sie unter [Unterstützte Datenspeicher und Formate](copy-activity-overview.md#supported-data-stores-and-formats).
