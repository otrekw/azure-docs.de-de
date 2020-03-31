---
title: Aufrufen von gespeicherten Prozeduren aus der Kopieraktivität in Azure Data Factory
description: Erfahren Sie, wie eine gespeicherte Prozedur in Azure SQL-Datenbank oder SQL-Server aus einer Kopieraktivität von Azure Data Factory aufgerufen wird.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: d05c2b03a0c498144f37c9b6205053120a596b09
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "74924094"
---
# <a name="invoke-stored-procedure-from-copy-activity-in-azure-data-factory"></a>Aufrufen von gespeicherten Prozeduren aus der Kopieraktivität in Azure Data Factory
> [!NOTE]
> Dieser Artikel gilt für Version 1 von Data Factory. Wenn Sie die aktuelle Version des Data Factory-Diensts verwenden, finden Sie weitere Informationen unter [Transformieren von Daten mithilfe der Aktivität für gespeicherte Prozeduren in Data Factory](../transform-data-using-stored-procedure.md).


Beim Kopieren von Daten nach [SQL Server](data-factory-sqlserver-connector.md) oder [Azure SQL-Datenbank](data-factory-azure-sql-connector.md) können Sie die **SqlSink** in der Kopieraktivität für das Aufrufen einer gespeicherten Prozedur konfigurieren. Es kann sinnvoll sein, eventuelle zusätzliche Verarbeitungsschritte (Zusammenführen von Spalten, Nachschlagen von Werten, Einfügungen in mehrere Tabellen zugleich usw.), die vor dem Einfügen von Daten in die Zieltabelle erforderlich sind, von der gespeicherten Prozedur ausführen zu lassen. Diese Funktion nutzt [Tabellenwertparameter](https://msdn.microsoft.com/library/bb675163.aspx). 

Das folgende Beispiel zeigt den Aufruf einer gespeicherten Prozedur in einer SQL Server-Datenbank aus einer Data Factory-Pipeline (Kopieraktivität):  

## <a name="output-dataset-json"></a>JSON des Ausgabedatasets
Legen Sie im Ausgabedataset-JSON den **type** auf **SqlServerTable** fest. Legen Sie ihn für die Verwendung mit einer Azure SQL-Datenbank auf **AzureSqlTable** fest. Der Wert der Eigenschaft **tableName** muss mit dem Namen des ersten Parameters der gespeicherten Prozedur übereinstimmen.  

```json
{
  "name": "SqlOutput",
  "properties": {
    "type": "SqlServerTable",
    "linkedServiceName": "SqlLinkedService",
    "typeProperties": {
      "tableName": "Marketing"
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

## <a name="sqlsink-section-in-copy-activity-json"></a>JSON im Abschnitt „SqlSink“ in der Kopieraktivität
Definieren Sie den Abschnitt **SqlSink** im JSON der Kopieraktivität wie folgt. Um beim Einfügen von Daten in die Senken-/Zieldatenbank eine gespeicherte Prozedur aufzurufen, geben Sie Werte sowohl für die **SqlWriterStoredProcedureName**-Eigenschaft als auch für die **SqlWriterTableType**-Eigenschaft an. Beschreibungen dieser Eigenschaften finden Sie im [SqlSink-Abschnitt im SQL Server-Connector-Artikel](data-factory-sqlserver-connector.md#sqlsink).

```json
"sink":
{
    "type": "SqlSink",
    "SqlWriterTableType": "MarketingType",
    "SqlWriterStoredProcedureName": "spOverwriteMarketing", 
    "storedProcedureParameters":
            {
                "stringData": 
                {
                    "value": "str1"     
                }
            }
}
```

## <a name="stored-procedure-definition"></a>Definition der gespeicherten Prozedur 
Definieren Sie in Ihrer Datenbank die gespeicherte Prozedur mit dem gleichen Namen wie **SqlWriterStoredProcedureName**. Die gespeicherte Prozedur verarbeitet Eingabedaten aus dem Quelldatenspeicher und fügt Daten in eine Tabelle in der Zieldatenbank ein. Der Name des ersten Parameters der gespeicherten Prozedur muss mit dem im Dataset-JSON definierten „tableName“ übereinstimmen (Marketing).

```sql
CREATE PROCEDURE spOverwriteMarketing @Marketing [dbo].[MarketingType] READONLY, @stringData varchar(256)
AS
BEGIN
    DELETE FROM [dbo].[Marketing] where ProfileID = @stringData
    INSERT [dbo].[Marketing](ProfileID, State)
    SELECT * FROM @Marketing
END
```

## <a name="table-type-definition"></a>Tabellentypdefinition
Definieren Sie in Ihrer Datenbank den Tabellentyp mit dem gleichen Namen wie **SqlWriterTableType**. Das Schema des Tabellentyps muss mit dem Schema des Eingabedatasets übereinstimmen.

```sql
CREATE TYPE [dbo].[MarketingType] AS TABLE(
    [ProfileID] [varchar](256) NOT NULL,
    [State] [varchar](256) NOT NULL
)
```

## <a name="next-steps"></a>Nächste Schritte
Gehen Sie die folgenden Connector-Artikel durch, um vollständige JSON-Beispiele zu finden: 

- [Azure SQL-Datenbank](data-factory-azure-sql-connector.md)
- [SQL Server](data-factory-sqlserver-connector.md)
