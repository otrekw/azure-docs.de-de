---
title: 'CREATE EXTERNAL STREAM (Transact-SQL): Azure SQL Edge (Vorschau)'
description: Erfahren Sie mehr über die CREATE EXTERNAL STREAM-Anweisung in Azure SQL Edge (Vorschau).
keywords: ''
services: sql-edge
ms.service: sql-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: 9e1f672a62ee7687fec9cea96ca03240c893ba95
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84233330"
---
# <a name="create-external-stream-transact-sql"></a>CREATE EXTERNAL STREAM (Transact-SQL)

Das EXTERNAL STREAM-Objekt dient einem doppelten Zweck als Eingabe und Ausgabe. Es kann als Eingabe verwendet werden, um Streamingdaten von Ereigniserfassungsdiensten wie Azure Event Hubs oder IoT Hubs abzufragen, oder als Ausgabe, um anzugeben, wo und wie Ergebnisse aus einer Streamingabfrage gespeichert werden sollen.

Ein EXTERNAL STREAM kann auch als Ausgabe und Eingabe für Dienste wie Event Hubs oder Blob Storage angegeben und erstellt werden. Dies ist für die Verkettung von Szenarien gedacht, in denen eine Streamingabfrage Ergebnisse an den externen Stream als Ausgabe sendet und eine andere Streamingabfrage aus dem gleichen externen Stream als Eingabe liest. 


## <a name="syntax"></a>Syntax

```sql
CREATE EXTERNAL STREAM {external_stream_name}  
(column definition [,column definitions ] * ) --Used for input - optional 
WITH  
(  
  DATA_SOURCE = <data_source_name>, 
  LOCATION = <location_name>, 
  EXTERNAL_FILE_FORMAT = <external_file_format_name>, --Used for input - optional 
   
  INPUT_OPTIONS =  
    ‘CONSUMER_GROUP=<consumer_group_name>; 
    ‘TIME_POLICY=<time_policy>; 
    LATE_EVENT_TOLERANCE=<late_event_tolerance_value>; 
    OUT_OF_ORDER_EVENT_TOLERANCE=<out_of_order_tolerance_value> 
     
    /* Edge options */ 
  , 
  OUTPUT_OPTIONS =  
    ‘REJECT_POLICY=<reject_policy>; 
    MINIMUM_ROWS=<row_value>; 
    MAXIMUM_TIME=<time_value_minutes>; 
    PARTITION_KEY_COLUMN=<partition_key_column_name>; 
    PROPERTY_COLUMNS=(); 
    SYSTEM_PROPERTY_COLUMNS=(); 
    PARTITION_KEY=<partition_key_name>; 
    ROW_KEY=<row_key_name>; 
    BATCH_SIZE=<batch_size_value>; 
    MAXIMUM_BATCH_COUNT=<batch_value>;  
    STAGING_AREA=<blob_data_source>’ 
     
    /* Edge options */ TAGS=<tag_column_value> 
); 
```


## <a name="arguments"></a>Argumente


- [EXTERNAL DATA SOURCE](/sql/t-sql/statements/create-external-data-source-transact-sql/)
- [EXTERNAL FILE FORMAT](/sql/t-sql/statements/create-external-file-format-transact-sql/)
- **LOCATION**: Gibt den Namen für die tatsächlichen Daten oder den Speicherort in der Datenquelle an. Bei einem Edge Hub oder einem Kafka-Streamobjekt gibt LOCATION den Namen des Edge Hub- oder Kafka-Themas an, aus dem gelesen bzw. in das geschrieben werden soll.
- **INPUT_OPTIONS**: Geben Sie Optionen als Schlüssel-Wert-Paare für Dienste wie Event Hubs und IoT Hubs an, die Eingaben für Streamingabfragen sind.
    - CONSUMER_GROUP: Event Hubs und IoT Hubs beschränken die Anzahl der Leser innerhalb einer Consumergruppe (auf 5). Wenn dieses Feld leer bleibt, wird die Consumergruppe „$Default“ verwendet.
      - Gilt für Event Hubs und IoT Hubs.
    - TIME_POLICY: Beschreibt, ob Ereignisse gelöscht oder die Ereigniszeit angepasst werden soll, wenn zu späte Ereignisse oder Ereignisse außerhalb der Reihenfolge ihren Toleranzwert übergeben.
      - Gilt für Event Hubs und IoT Hubs.
    - LATE_EVENT_TOLERANCE: Die maximal zulässige Zeitverzögerung. Die Verzögerung stellt den Unterschied zwischen dem Zeitstempel des Ereignisses und der Systemuhr dar.
      - Gilt für Event Hubs und IoT Hubs.
    - OUT_OF_ORDER_EVENT_TOLERANCE: Ereignisse können in nicht ordnungsgemäßer Reihenfolge eintreffen, nachdem Sie die Eingabe bis zur Streamingabfrage durchlaufen haben. Diese Ereignisse können so akzeptiert werden, wie sie sind, oder Sie können für einen bestimmten Zeitraum eine Pause festlegen, um sie neu zu ordnen.
      - Gilt für Event Hubs und IoT Hubs.
- **OUTPUT_OPTIONS**: Geben Sie Optionen als Schlüssel-Wert-Paare für unterstützte Dienste an, die Ausgaben für Streamingabfragen sind. 
  - REJECT_POLICY:  DROP | RETRY: Gibt die Richtlinien für die Fehlerbehandlung von Daten an, wenn Datenkonvertierungsfehler auftreten. 
    - Gilt für alle unterstützten Ausgaben. 
  - MINIMUM_ROWS:  
    Mindestens erforderliche Zeilen pro Batch, die in eine Ausgabe geschrieben werden. Für Parquet erstellt jeder Batch eine neue Datei. 
    - Gilt für alle unterstützten Ausgaben. 
  - MAXIMUM_TIME:  
    Maximale Wartezeit pro Batch. Nach Ablauf dieser Zeit wird der Batch auch dann in die Ausgabe geschrieben, wenn die Anforderung der Mindestanzahl von Zeilen nicht erfüllt ist. 
    - Gilt für alle unterstützten Ausgaben. 
  - PARTITION_KEY_COLUMN:  
    Die Spalte, die für den Partitionsschlüssel verwendet wird. 
    - Gilt für Event Hub- und Service Bus-Thema 
  - PROPERTY_COLUMNS:  
    Eine durch Trennzeichen getrennte Liste der Namen von Ausgabespalten, die (sofern angegeben) als benutzerdefinierte Eigenschaften an Nachrichten angefügt werden.  
    - Gilt für Event Hub- und Service Bus-Thema und -Warteschlange 
  - SYSTEM_PROPERTY_COLUMNS:  
    Eine JSON-formatierte Sammlung von Name-Wert-Paaren von Systemeigenschaftennamen und Ausgabespalten, die für Service Bus-Nachrichten aufgefüllt werden sollen. Beispiel: { "MessageId": "column1", "PartitionKey": "column2"} 
    - Gilt für Service Bus-Thema und -Warteschlange 
  - PARTITION_KEY:  
    Der Name der Ausgabespalte mit dem Partitionsschlüssel. Der Partitionsschlüssel ist ein eindeutiger Bezeichner für die Partition innerhalb einer Tabelle, die den ersten Teil des Primärschlüssels einer Entität bildet. Dabei handelt es sich um einen Zeichenfolgenwert, der bis zu 1 KB groß sein kann. 
    - Gilt für Table Storage und Azure Functions. 
  - ROW_KEY:  
    Der Name der Ausgabespalte, die den Zeilenschlüssel enthält. Der Zeilenschlüssel ist ein eindeutiger Bezeichner für eine Entität innerhalb einer Partition. Er bildet den zweiten Teil des Primärschlüssels einer Entität. Der Zeilenschlüssel ist ein Zeichenfolgenwert, der bis zu 1 KB groß sein kann. 
    - Gilt für Table Storage und Azure Functions. 
  - BATCH_SIZE:  
    Dieser Wert stellt die Anzahl der Transaktionen für den Tabellenspeicher dar. Dabei kann der Höchstwert bis zu 100 Datensätze erreichen. Für Azure Functions entspricht dies der Batchgröße in Bytes, die pro Aufruf an die Funktion gesendet werden. Der Standardwert ist 256 KB. 
    - Gilt für Table Storage und Azure Functions. 
  - MAXIMUM_BATCH_COUNT:  
    Die maximal zulässige Anzahl von Ereignissen, die pro Aufruf für Azure Functions an die Funktion gesendet werden können. Der Standardwert lautet 100. Für SQL-Datenbank stellt dieser Wert die maximale Anzahl von Datensätzen dar, die mit jeder Transaktion zum Masseneinfügen gesendet wird. Der Standardwert ist 10.000. 
    - Gilt für SQL-Datenbank und Azure Functions. 
  - STAGING_AREA: EXTERNAL DATA SOURCE- Objekt für Blob Storage. Der Stagingbereich für die Datenerfassung mit hohem Durchsatz in SQL Data Warehouse. 
    - Gilt für SQL Data Warehouse.


## <a name="examples"></a>Beispiele

### <a name="example-1---edgehub"></a>Beispiel 1: EdgeHub

Typ: Eingabe oder Ausgabe<br>
Parameter:
- Eingabe oder Ausgabe
  - Alias 
  - Ereignisserialisierungsformat 
  - Codieren 
- Nur Eingabe: 
  - Typ der Ereigniskomprimierung 

Syntax:

```sql
CREATE EXTERNAL DATA SOURCE MyEdgeHub 
WITH  
(      
  LOCATION = 'edgehub://'       
); 
 
CREATE EXTERNAL FILE FORMAT myFileFormat  
WITH (  
   FORMAT_TYPE = 'JSON', 
); 
 
CREATE EXTERNAL STREAM Stream_A  
WITH    
(   
   DATA_SOURCE = MyEdgeHub, 
   EXTERNAL_FILE_FORMAT = myFileFormat, 
   LOCATION = ‘<mytopicname>’, 
   OUTPUT_OPTIONS =   
     ‘REJECT_TYPE: Drop’ 
);
```


### <a name="example-2---azure-sql-database-azure-sql-edge-sql-server"></a>Beispiel 2: Azure SQL-Datenbank, Azure SQL Edge, SQL Server

Typ: Output<br>
Parameter:
- Ausgabealias  
- Datenbank (für SQL-Datenbank erforderlich) 
- Server (für SQL-Datenbank erforderlich) 
- Benutzername (für SQL-Datenbank erforderlich) 
- Kennwort (für SQL-Datenbank erforderlich) 
- Tabelle 
- Mergen aller Eingabepartitionen in einen einzelnen Schreibvorgang oder ein Inherit-Partitionsschema des vorherigen Abfrageschritts oder der Eingabe (erforderlich für SQL-Datenbank). 
- Max Batch Count 

Syntax:

```sql
CREATE DATABASE SCOPED CREDENTIAL SQLCredName 
WITH IDENTITY = '<user>’, 
SECRET = '<password>'; 
 
-- Azure SQL Database 
CREATE EXTERNAL DATA SOURCE MyTargetSQLTabl 
WITH 
(     
  LOCATION = ' <my_server_name>.database.windows.net’, 
  CREDENTIAL = SQLCredName 
); 
 
--SQL Server or Azure SQL Edge
CREATE EXTERNAL DATA SOURCE MyTargetSQLTabl 
WITH 
(     
  LOCATION = ' <sqlserver://<ipaddress>,<port>’, 
  CREDENTIAL = SQLCredName 
); 
 
--SQL Database/Edge 
CREATE EXTERNAL STREAM Stream_A 
WITH   
(  
    DATA_SOURCE = MyTargetSQLTable, 
    LOCATION = ‘<DatabaseName>.<SchemaName>.<TableName>’ 
   --Note: If table is container in the database, <TableName> should be sufficient 
   --Note: Do not need external file format in this case 
    EXTERNAL_FILE_FORMAT = myFileFormat,  
    OUTPUT_OPTIONS =  
      ‘REJECT_TYPE: Drop 
); 
```

### <a name="example-3---kafka"></a>Beispiel 3: Kafka

Typ: Eingabe<br>
Parameter:

- Kafka-Bootstrapserver 
- Kafka-Themaname 
- Anzahl Quellpartition 

Syntax:

```sql
CREATE EXTERNAL DATA SOURCE MyKafka_tweets 
WITH 
( 
  --The location maps to KafkaBootstrapServer 
  LOCATION = ' kafka://<kafkaserver>:<ipaddress>’, 
  CREDENTIAL = kafkaCredName 
 
); 
 
CREATE EXTERNAL FILE FORMAT myFileFormat 
WITH ( 
    FORMAT_TYPE = 'CSV', 
    DATA_COMPRESSION = 'GZIP', 
    ENCODING = ‘UTF-8’, 
    DELIMITER = ‘|’ 
); 
 
 
CREATE EXTERNAL STREAM Stream_A (user_id VARCHAR, tweet VARCHAR) 
WITH   
(  
    DATA_SOURCE = MyKafka_tweets, 
    LOCATION = ‘<KafkaTopicName>’, 
   --JSON: Format, CSV: Delimiter and Encoding, AVRO: None 
    EXTERNAL_FILE_FORMAT = myFileFormat,  
    INPUT_OPTIONS =  
      ‘PARTITIONS: 5’ 
); 
```

### <a name="example-4---blob-storage"></a>Beispiel 4: Blobspeicher

Typ: Eingabe oder Ausgabe<br>
Parameter:
- Eingabe oder Ausgabe:
  - Alias 
  - Speicherkonto 
  - Speicherkontoschlüssel 
  - Container 
  - Pfadmuster 
  - Datumsformat 
  - Zeitformat 
  - Ereignisserialisierungsformat 
  - Codieren 
- Nur Eingabe: 
  - Partitionen (Eingabe) 
  - Typ der Ereigniskomprimierung (Eingabe) 
- Nur Ausgabe: 
  - Mindestanzahl von Zeilen (Ausgabe) 
  - Maximaler Zeitraum (Ausgabe) 
  - Authentifizierungsmodus: (Ausgabe) 

Syntax:

```sql
CREATE DATABASE SCOPED CREDENTIAL StorageAcctCredName 
WITH IDENTITY = 'Shared Access Signature’, 
SECRET = 'AccountKey'; 
 
CREATE DATABASE SCOPED CREDENTIAL StorageAcctCredNameMSI 
WITH IDENTITY = 'Managed Identity’; 
 
CREATE EXTERNAL DATA SOURCE MyBlobStorage_tweets 
WITH 
(     
  LOCATION = 'sb://my-sb-namespace.servicebus.windows.net’, 
  CREDENTIAL = eventHubCredName 
); 
 
CREATE EXTERNAL FILE FORMAT myFileFormat 
WITH ( 
    FORMAT_TYPE = 'CSV', --Event serialization format 
    DATE_FORMAT = 'YYYY/MM/DD HH', --Both Date and Time format 
    ENCODING = ‘UTF-8’ 
); 
 
CREATE EXTERNAL STREAM Stream_A (user_id VARCHAR, tweet VARCHAR) 
WITH   
(  
    DATA_SOURCE = MyBlobStorage_tweets, 
    LOCATION = ‘<path_pattern>’, 
    EXTERNAL_FILE_FORMAT = myFileFormat,  
    INPUT_OPTIONS =  
      ‘PARTITIONS: 1’, 
  
    OUTPUT_OPTIONS =  
      ‘REJECT_TYPE: Drop, 
      PARTITION_KEY_COLUMN: , 
      PROPERTY_COLUMNS: (), 
      MINUMUM_ROWS: 100000, 
      MAXIMUM_TIME: 60’ 
); 
```

### <a name="example-5---event-hub"></a>Beispiel 5: Event Hub

Typ: Eingabe oder Ausgabe<br>
Parameter:
- Eingabe oder Ausgabe:
  - Alias 
  - Service Bus-Namespace 
  - Event Hub-Name 
  - Event Hub-Richtlinienname 
  - Event Hub-Richtlinienschlüssel 
  - Ereignisserialisierungsformat 
  - Codieren 
- Nur Eingabe: 
  - Event Hub-Consumergruppe 
  - Typ der Ereigniskomprimierung 
- Nur Ausgabe: 
  - Partitionsschlüsselspalte 
  - Eigenschaftenspalten 

Syntax:

```sql
CREATE DATABASE SCOPED CREDENTIAL eventHubCredName 
WITH IDENTITY = 'Shared Access Signature’, 
SECRET = '<policyName>'; 
 
CREATE EXTERNAL DATA SOURCE MyEventHub_tweets 
WITH 
(     
  LOCATION = 'sb://my-sb-namespace.servicebus.windows.net’, 
  CREDENTIAL = eventHubCredName 
); 
 
CREATE EXTERNAL FILE FORMAT myFileFormat 
WITH ( 
    FORMAT_TYPE = 'CSV', 
    DATA_COMPRESSION = 'GZIP', 
    ENCODING = ‘UTF-8’, 
    DELIMITER = ‘|’ 
); 
 
 
CREATE EXTERNAL STREAM Stream_A (user_id VARCHAR, tweet VARCHAR) 
WITH   
(  
    DATA_SOURCE = MyEventHub_tweets, 
    LOCATION = ‘<topicname>’, 
   --JSON: Format, CSV: Delimiter and Encoding, AVRO: None 
    EXTERNAL_FILE_FORMAT = myFileFormat,  
 
    INPUT_OPTIONS =  
      ‘CONSUMER_GROUP: FirstConsumerGroup’, 
          
    OUTPUT_OPTIONS =  
      ‘REJECT_TYPE: Drop, 
      PARTITION_KEY_COLUMN: , 
      PROPERTY_COLUMNS: ()’ 
);
```

### <a name="example-6---iot-hub"></a>Beispiel 6: IoT Hub

Typ: Eingabe<br>
Parameter:

- Eingabealias 
- IoT Hub 
- Endpunkt 
- Name der SAS-Richtlinie 
- Schlüssel für SAS-Richtlinie 
- Consumergruppe 
- Ereignisserialisierungsformat 
- Codieren 
- Typ der Ereigniskomprimierung 

Syntax:

```sql
CREATE DATABASE SCOPED CREDENTIAL IoTHubCredName 
WITH IDENTITY = 'Shared Access Signature’, 
SECRET = '<policyName>'; 
 
CREATE EXTERNAL DATA SOURCE MyIoTHub_tweets 
WITH 
(     
  LOCATION = ' iot://iot_hub_name.azure-devices.net’, 
  CREDENTIAL = IoTHubCredName 
);  

CREATE EXTERNAL FILE FORMAT myFileFormat 
WITH ( 
    FORMAT_TYPE = 'CSV', --Event serialization format 
    DATA_COMPRESSION = 'GZIP', 
    ENCODING = ‘UTF-8’ 
); 
 
CREATE EXTERNAL STREAM Stream_A (user_id VARCHAR, tweet VARCHAR) 
WITH   
(  
    DATA_SOURCE = MyIoTHub_tweets, 
    LOCATION = ‘<name>’, 
    EXTERNAL_FILE_FORMAT = myFileFormat,  
    INPUT_OPTIONS =  
      ‘ENDPOINT: Messaging, 
      CONSUMER_GROUP: ‘FirstConsumerGroup’ 
); 
```

### <a name="example-7---azure-synapse-analytics-formerly-sql-data-warehouse"></a>Beispiel 7: Azure Synapse Analytics (vormals SQL Data Warehouse)

Typ: Output<br>
Parameter:
- Ausgabealias 
- Datenbank 
- Server 
- Username 
- Kennwort 
- Tabelle 
- Stagingbereich (für COPY) 

Syntax:

```sql
CREATE DATABASE SCOPED CREDENTIAL SQLCredName 
WITH IDENTITY = '<user>’, 
SECRET = '<password>'; 
 
CREATE EXTERNAL DATA SOURCE MyTargetSQLTable 
WITH 
(     
  LOCATION = ' <my_server_name>.database.windows.net’, 
  CREDENTIAL = SQLCredName 
); 
 
CREATE EXTERNAL STREAM MySQLTableOutput 
WITH ( 
   DATA_SOURCE = MyTargetSQLTable, 
   LOCATION = ‘<TableName>’ 
   --Note: Do not need external file format in this case 
   OUTPUT_OPTIONS =  
     ‘REJECT_TYPE: Drop, 
     STAGING_AREA: staging_area_data_source’, 
); 
```


### <a name="example-8---table-storage"></a>Beispiel 8: Table Storage

Typ: Output<br>
Parameter:
- Ausgabealias 
- Speicherkonto 
- Speicherkontoschlüssel 
- Tabellenname 
- Partitionsschlüssel 
- Zeilenschlüssel 
- Batchgröße 

Syntax:

```sql
CREATE DATABASE SCOPED CREDENTIAL TableStorageCredName 
WITH IDENTITY = ‘Storage account Key’, 
SECRET = '<storage_account_key>'; 
 
CREATE EXTERNAL DATA SOURCE MyTargetTableStorage 
WITH 
(     
  LOCATION = 'abfss://<storage_account>.dfs.core.windows.net’, 
  CREDENTIAL = TableStorageCredName 
); 
 
CREATE EXTERNAL STREAM MyTargetTableStorageOutput 
WITH ( 
   DATA_SOURCE = MyTargetTableStorage, 
   LOCATION = ‘<TableName>’, 
   OUTPUT_OPTIONS =  
     ‘REJECT_TYPE: Drop, 
     PARTITION_KEY: <column_partition_key>, 
     ROW_KEY: <column_row_key>, 
     BATCH_SIZE: 100’ 
); 
```


### <a name="example-9---service-bus-topic-same-properties-as-queue"></a>Beispiel 9: Service Bus-Thema (gleiche Eigenschaften wie Warteschlange)

Typ: Output<br>
Parameter:
- Ausgabealias 
- Service Bus-Namespace 
- Themenname 
- Name der Themenrichtlinie 
- Schlüssel der Themenrichtlinie 
- Eigenschaftenspalten 
- Systemeigenschaftsspalten 
- Ereignisserialisierungsformat 
- Codieren 

Syntax:

```sql
CREATE DATABASE SCOPED CREDENTIAL serviceBusCredName 
WITH IDENTITY = 'Shared Access Signature’, 
SECRET = '<policyName>'; 
 
CREATE EXTERNAL DATA SOURCE MyServiceBus_tweets 
WITH 
(     
  LOCATION = 'sb://my-sb-namespace.servicebus.windows.net’, 
  CREDENTIAL = serviceBusCredName 
); 
 
CREATE EXTERNAL FILE FORMAT myFileFormat 
WITH ( 
    FORMAT_TYPE = 'CSV', --Event serialization format 
    DATA_COMPRESSION = 'GZIP', 
    ENCODING = ‘UTF-8’ 
); 
 
CREATE EXTERNAL STREAM MyServiceBusOutput 
WITH ( 
   DATA_SOURCE = MyServiceBus_tweets, 
   LOCATION = ‘<topic_name>’, 
   EXTERNAL_FILE_FORMAT = myFileFormat 
       OUTPUT_OPTIONS =  
     ‘REJECT_TYPE: Drop, 
     PARTITION_KEY_COLUMN: , 
     PROPERTY_COLUMNS: (), 
     SYSTEM_PROPERTY_COLUMNS: ()’ 
   --JSON: Format, CSV: Delimiter and Encoding, AVRO: None 
           
); 
```


### <a name="example-10---cosmos-db"></a>Beispiel 10: Cosmos DB

Typ: Output<br>
Parameter:
- Ausgabealias 
- Konto-ID 
- Kontoschlüssel 
- Datenbank 
- Containername 
- Dokument-ID 


Syntax:

```sql
CREATE DATABASE SCOPED CREDENTIAL cosmosDBCredName 
WITH IDENTITY = ‘Storage Account Key’, 
SECRET = '<accountKey>'; 
 
CREATE EXTERNAL DATA SOURCE MyCosmosDB_tweets 
WITH 
(     
  LOCATION = ' cosmosdb://accountid.documents.azure.com:443/ database’, 
  CREDENTIAL = cosmosDBCredName 
); 
 
CREATE EXTERNAL STREAM MyCosmosDBOutput 
WITH ( 
   DATA_SOURCE = MyCosmosDB_tweets, 
   LOCATION = ‘<container/documentID>’ 
   OUTPUT_OPTIONS =  
     ‘REJECT_TYPE: Drop', 
     --Note: Do not need external file format in this case 
          
);
```



### <a name="example-11---power-bi"></a>Beispiel 11: Power BI

Typ: Output<br>
Parameter:
- Ausgabealias 
- Datasetname 
- Tabellenname 


Syntax:

```sql
CREATE DATABASE SCOPED CREDENTIAL PBIDBCredName 
WITH IDENTITY = ‘Managed Identity’; 
 
CREATE EXTERNAL DATA SOURCE MyPbi_tweets 
WITH 
( 
  LOCATION = 'pbi://dataset/’, 
  CREDENTIAL = PBIDBCredName 
 
); 
 
CREATE EXTERNAL STREAM MyPbiOutput 
WITH ( 
   DATA_SOURCE = MyPbi_tweets, 
   LOCATION = 'tableName', 
   OUTPUT_OPTIONS =  
     ‘REJECT_TYPE: Drop' 
        
);
```

### <a name="example-12---azure-function"></a>Beispiel 12: Azure Functions

Typ: Output<br>
Parameter:
- Funktionen-App 
- Funktion 
- Schlüssel 
- Max Batch Size 
- Max Batch Count 

Syntax:

```sql
CREATE DATABASE SCOPED CREDENTIAL AzureFunctionCredName 
WITH IDENTITY = ‘Function Key’, 
SECRET = '<function_key>'; 
 
CREATE EXTERNAL DATA SOURCE MyTargetTableStorage 
WITH 
(     
  LOCATION = 'abfss://<storage_account>.dfs.core.windows.net’, 
  CREDENTIAL = TableStorageCredName 
); 
 
CREATE EXTERNAL STREAM MyTargetTableStorageOutput 
WITH ( 
   DATA_SOURCE = MyTargetTableStorage, 
   LOCATION = ‘<TableName>’, 
   OUTPUT_OPTIONS =  
     ‘REJECT_TYPE: 'Drop'      
     PARTITION_KEY: ‘<column_partition_key>, 
     ROW_KEY: <column_row_key>, 
     BATCH_SIZE: 100’ 
); 
```


## <a name="see-also"></a>Weitere Informationen

- [ALTER EXTERNAL STREAM (Transact-SQL)](alter-external-stream-transact-sql.md) 
- [DROP EXTERNAL STREAM (Transact-SQL)](drop-external-stream-transact-sql.md) 

