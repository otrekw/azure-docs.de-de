---
title: Erstellen eines T-SQL-Streamingauftrags in Azure SQL Edge (Vorschau)
description: Erfahren Sie mehr über das Erstellen von Stream Analytics-Aufträgen in Azure SQL Edge (Vorschau).
keywords: ''
services: sql-database-edge
ms.service: sql-database-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: 5e0043ebba1a317dcc6798d6be74aac051d97012
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83595389"
---
# <a name="create-stream-analytics-job-in-azure-sql-edge-preview"></a>Erstellen eines Stream Analytics-Auftrags in Azure SQL Edge (Vorschau) 

In diesem Artikel wird das Erstellen eines T-SQL-Streamingauftrags in Azure SQL Edge (Vorschau) erläutert. Zum Erstellen eines Streamingauftrags in SQL Edge sind die folgenden Schritte erforderlich:

1. Erstellen der externen Streameingabe- und -ausgabeobjekte
2. Definieren der Streamingauftragsabfrage im Rahmen der Erstellung des Streamingauftrags

## <a name="configure-an-external-stream-input-and-output-object"></a>Konfigurieren eines externen Streameingabe- und -ausgabeobjekts

T-SQL-Streaming verwendet die Funktionen der externen Datenquelle von SQL Server, um die Datenquellen zu definieren, die den externen Streameingaben und -ausgaben des Streamingauftrags zugeordnet sind. Die folgenden T-SQL-Befehle sind erforderlich, um ein externes Streameingabe- oder -ausgabeobjekt zu erstellen.

[CREATE EXTERNAL FILE FORMAT (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/create-external-file-format-transact-sql)

[CREATE EXTERNAL DATA SOURCE (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/create-external-data-source-transact-sql)

[CREATE EXTERNAL STREAM (Transact-SQL)](#example-create-an-external-stream-object-sql-database)

Wenn SQL Edge (oder SQL Server, Azure SQL) als Ausgabestream verwendet wird, ist außerdem der T-SQL-Befehl [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/create-database-scoped-credential-transact-sql) erforderlich, um die Anmeldeinformationen für den Zugriff auf die SQL-Datenbank zu definieren.

### <a name="supported-input-and-output-stream-data-sources"></a>Unterstützte Datenquellen für Eingabe- und Ausgabestreams

Azure SQL Edge unterstützt derzeit nur die folgenden Datenquellen als Streameingaben und -ausgaben.

| Datenquellentyp | Eingabe | Output | BESCHREIBUNG |
|------------------|-------|--------|------------------|
| Azure IoT Edge Hub | J | J | Datenquelle zum Lesen/Schreiben von Streamingdaten in einen Azure IoT Edge Hub. Weitere Informationen zu Azure IoT Edge Hub finden Sie unter [IoT Edge Hub](https://docs.microsoft.com/azure/iot-edge/iot-edge-runtime#iot-edge-hub).|
| SQL-Datenbank | N | J | Datenquellenverbindung, um Streamingdaten in SQL-Datenbank zu schreiben. Die SQL-Datenbank kann eine lokale SQL Edge-Datenbank oder eine SQL Server-Remotedaten oder Azure SQL-Datenbank sein.|
| Azure Blob Storage | N | J | Datenquelle zum Schreiben von Daten in ein Blob in einem Azure-Speicherkonto. |
| Kafka | J | N | Datenquelle zum Lesen von Streamingdaten aus einem Kafka-Thema. Dieser Adapter ist zurzeit nur für die Intel-/AMD-Version von Azure SQL Edge verfügbar und ist nicht für die ARM64-Version von SQL Edge verfügbar.|

### <a name="example-create-an-external-stream-inputoutput-object-for-azure-iot-edge-hub"></a>Beispiel: Erstellen eines externen Streameingabe-/-ausgabeobjekts für Azure IoT Edge Hub

Im folgenden Beispiel wird ein externes Streamobjekt für den Edge Hub erstellt. Zum Erstellen einer externen Streameingabe-/-ausgabedatenquelle für Azure IoT Edge Hub müssen Sie zunächst ein externes Dateiformat für SQL erstellen, um das Layout der Daten zu verstehen, die gelesen/geschrieben werden.

1. Erstellen Sie ein externes Dateiformat mit dem Formattyp JSON.

    ```sql
    Create External file format InputFileFormat
    WITH (  
       format_type = JSON,
    )
    go
    ```

2. Erstellen Sie eine externe Datenquelle für den IoT Edge Hub. Das folgende T-SQL-Skript erstellt eine Datenquellenverbindung mit einem Edge Hub, der auf demselben Docker-Host wie SQL Edge ausgeführt wird.

    ```sql
    CREATE EXTERNAL DATA SOURCE EdgeHubInput WITH (
    LOCATION = 'edgehub://'
    )
    go
    ```

3. Erstellen Sie das externe Streamobjekt für den IoT Edge Hub. Das T-SQL-Skript unten erstellt ein Streamobjekt für den Edge Hub. Im Fall eines Edge Hub-Streamobjekts ist der LOCATION-Parameter der Name des Edge Hub-Themas/-Kanals, aus dem gelesen oder in das geschrieben wird.

    ```sql
    CREATE EXTERNAL STREAM MyTempSensors WITH (
    DATA_SOURCE = EdgeHubInput,
    FILE_FORMAT = InputFileFormat,
    LOCATION = N'TemperatureSensors',
    INPUT_OPTIONS = N'',
    OUTPUT_OPTIONS = N''
    )
    go
    ```

### <a name="example-create-an-external-stream-object-sql-database"></a>Beispiel: Erstellen einer externen Streamobjekt-SQL-Datenbank

Im folgenden Beispiel wird ein externes Streamobjekt für die lokale SQL Edge-Datenbank erstellt. 

1. Erstellen Sie einen Hauptschlüssel in der Datenbank. Dies ist erforderlich, um das Geheimnis für die Anmeldeinformationen zu verschlüsseln.

    ```sql
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<<Strong_Password_For_Master_Key_Encryption>>';
    ```

2. Erstellen Sie datenbankweit gültige Anmeldeinformationen für den Zugriff auf die SQL Server-Quelle. Das folgende Beispiel erstellt Anmeldeinformationen für die externe Datenquelle mit IDENTITY = 'username' und SECRET = 'password'.

    ```sql
    CREATE DATABASE SCOPED CREDENTIAL SQLCredential
    WITH IDENTITY = '<SQL_Login>', SECRET = '<SQL_Login_PASSWORD>'
    go
    ```

3. Erstellen Sie mit CREATE EXTERNAL DATA SOURCE eine externe Datenquelle. Im folgenden Beispiel:

    * Wird eine externe Datenquelle mit dem Namen LocalSQLOutput erstellt.
    * Wird die externe Datenquellen (LOCATION = '<vendor>://<server>[:<port>]') identifiziert. Im Beispiel verweist sie auf eine lokale Instanz von SQL Edge.
    * Schließlich werden im Beispiel die zuvor erstellten Anmeldeinformationen verwendet.

    ```sql
    CREATE EXTERNAL DATA SOURCE LocalSQLOutput WITH (
    LOCATION = 'sqlserver://tcp:.,1433'
    ,CREDENTIAL = SQLCredential
    )
    go
    ```

4. Erstellen Sie das externe Streamobjekt. Im folgenden Beispiel wird ein externes Streamobjekt erstellt, das auf eine Tabelle *dbo.TemperatureMeasurements* in der Datenbank *MySQLDatabase* verweist.

    ```sql
    CREATE EXTERNAL STREAM TemperatureMeasurements WITH (
    DATA_SOURCE = LocalSQLOutput,
    LOCATION = N'MySQLDatabase.dbo.TemperatureMeasurements',
    INPUT_OPTIONS = N'',
    OUTPUT_OPTIONS = N''
    )
    ```

## <a name="create-the-streaming-job-and-the-streaming-queries"></a>Erstellen des Streamingauftrags und der Streamingabfragen

Verwenden Sie die gespeicherte Systemprozedur **sys.sp_create_streaming_job**, um die Streamingabfragen zu definieren und den Streamingauftrag zu erstellen. Die gespeicherte Prozedur **sp_create_streaming_job** nimmt zwei Parameter an.

- job_name: Der Name des Streamingauftrags. Die Namen von Streamingaufträgen sind in der gesamten Instanz eindeutig.
- satement: Auf der [Stream Analytics-Abfragesprache](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference?) basierende Streamingabfrageanweisungen.

Im folgenden Beispiel wird ein einfacher Streamingauftrag mit einer Streamingabfrage erstellt. Diese Abfrage liest die Eingaben aus dem Edge Hub und schreibt in *dbo.TemperatureMeasurements* in der Datenbank.

```sql
EXEC sys.sp_create_streaming_job @name=N'StreamingJob1',
@statement= N'Select * INTO TemperatureMeasurements from MyEdgeHubInput'
```

Im folgenden Beispiel wird ein komplexerer Streamingauftrag mit mehreren verschiedenen Abfragen erstellt, einschließlich einer Abfrage, die die integrierte AnomalyDetection_ChangePoint-Funktion verwendet, um Anomalien in den Temperaturdaten zu identifizieren.

```sql
EXEC sys.sp_create_streaming_job @name=N'StreamingJob2', @statement=
N' Select * INTO TemperatureMeasurements1 from MyEdgeHubInput1

Select * Into TemperatureMeasurements2 from MyEdgeHubInput2

Select * Into TemperatureMeasurements3 from MyEdgeHubInput3

SELECT
Timestamp as [Time],
[Temperature] As [Temperature],
GetRecordPropertyValue(AnomalyDetection_ChangePoint(Temperature, 80, 1200) OVER(LIMIT DURATION(minute, 20)), ''Score'') as ChangePointScore,
GetRecordPropertyValue(AnomalyDetection_ChangePoint(Temperature, 80, 1200) OVER(LIMIT DURATION(minute, 20)), ''IsAnomaly'') as IsChangePointAnomaly,
INTO TemperatureAnomalies FROM MyEdgeHubInput2;
'
go
```

## <a name="start-stop-drop-and-monitor-streaming-jobs"></a>Starten, Beenden, Löschen und Überwachen von Streamingaufträgen

Um einen Streamingauftrag in SQL Edge zu starten, führen Sie die gespeicherte Prozedur **sys.sp_start_streaming_job** aus. Die gespeicherte Prozedur erfordert, dass der Streamingauftrag als Eingabe gestartet wird.

```sql
exec sys.sp_start_streaming_job @name=N'StreamingJob1'
go
```

Um einen Streamingauftrag in SQL Edge zu beenden, führen Sie die gespeicherte Prozedur **sys.sp_stop_streaming_job** aus. Die gespeicherte Prozedur erfordert, dass der Streamingauftrag als Eingabe beendet wird.

```sql
exec sys.sp_stop_streaming_job @name=N'StreamingJob1'
go
```

Um einen Streamingauftrag in SQL Edge zu entfernen (oder löschen), führen Sie die gespeicherte Prozedur **sys.sp_drop_streaming_job** aus. Die gespeicherte Prozedur erfordert, dass der Streamingauftrag als Eingabe gelöscht wird.

```sql
exec sys.sp_drop_streaming_job @name=N'StreamingJob1'
go
```

Um den aktuellen Status eines Streamingauftrags in SQL Edge abzurufen, führen Sie die gespeicherte Prozedur **sys.sp_get_streaming_job** aus. Die gespeicherte Prozedur erfordert, dass der Streamingauftrag als Eingabe gelöscht wird, und gibt den Namen und den aktuellen Status des Streamingauftrags aus.

```sql
exec sys.sp_get_streaming_job @name=N'StreamingJob1'
        WITH RESULT SETS
(
       (
       name nvarchar(256),
       status nvarchar(256)
       )
)
```

Der Streamingauftrag kann einen der folgenden Zustände aufweisen:

| Status | BESCHREIBUNG |
|--------| ------------|
| Erstellt | Der Streamingauftrag wurde erstellt, aber noch nicht gestartet. |
| Wird gestartet | Der Streamingauftrag befindet sich in der Startphase. |
| Idle | Der Streamingauftrag wird ausgeführt, es sind jedoch keine Eingaben für die Verarbeitung vorhanden. |
| Verarbeitung | Der Streamingauftrag wird ausgeführt und verarbeitet Eingaben. Dieser Status zeigt einen fehlerfreien Status für den Streamingauftrag an. |
| Heruntergestuft | Der Streamingauftrag wird ausgeführt, es sind jedoch bei der Eingabeverarbeitung einige nicht schwerwiegende Fehler bei der Serialisierung/Deserialisierung der Eingabe/Ausgabe aufgetreten. Der Eingabeauftrag wird weiterhin ausgeführt, löscht jedoch Eingaben, bei denen Fehler auftreten. |
| Beendet | Der Streamingauftrag wurde beendet. |
| Fehler | Der Streamingauftrag ist fehlgeschlagen. Dies ist im Allgemeinen ein Hinweis auf einen schwerwiegenden Fehler während der Verarbeitung. |

## <a name="next-steps"></a>Nächste Schritte

- [Anzeigen von Metadaten für Streamingaufträge in Azure SQL Edge (Vorschau)](streaming-catalog-views.md) 
- [Erstellen eines externen Streams](create-external-stream-transact-sql.md)
