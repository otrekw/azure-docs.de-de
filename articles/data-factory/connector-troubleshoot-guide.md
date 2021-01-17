---
title: Problembehandlung für Azure Data Factory-Connectors
description: Es wird beschrieben, wie Sie in Azure Data Factory Connectorprobleme beheben.
services: data-factory
author: linda33wj
ms.service: data-factory
ms.topic: troubleshooting
ms.date: 01/07/2021
ms.author: jingwang
ms.reviewer: craigg
ms.custom: has-adal-ref
ms.openlocfilehash: 68547b8fb673cd54b7c21963ede122553bbbc390
ms.sourcegitcommit: 9514d24118135b6f753d8fc312f4b702a2957780
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/07/2021
ms.locfileid: "97967122"
---
# <a name="troubleshoot-azure-data-factory-connectors"></a>Problembehandlung für Azure Data Factory-Connectors

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

In diesem Artikel werden die gängigen Problembehandlungsmethoden für Connectors in Azure Data Factory beschrieben.
  
## <a name="azure-blob-storage"></a>Azure Blob Storage

### <a name="error-code-azurebloboperationfailed"></a>Fehlercode: AzureBlobOperationFailed

- **Meldung**: `Blob operation Failed. ContainerName: %containerName;, path: %path;.`

- **Ursache:** Beim Speichervorgang für Blobs ist ein Problem aufgetreten.

- **Empfehlung**:  Überprüfen Sie den Fehler genau. Weitere Informationen finden Sie in diesem Blob-Hilfedokument: https://docs.microsoft.com/rest/api/storageservices/blob-service-error-codes. Wenden Sie sich an das Storage-Team, wenn Sie weitere Hilfe benötigen.


### <a name="invalid-property-during-copy-activity"></a>Ungültige Eigenschaft während der Copy-Aktivität

- **Meldung**: `Copy activity <Activity Name> has an invalid "source" property. The source type is not compatible with the dataset <Dataset Name> and its linked service <Linked Service Name>. Please verify your input against.`

- **Ursache:** Der im Dataset definierte Typ stimmt nicht mit dem in der Copy-Aktivität definierten Quell-/Senkentyp überein.

- **Lösung:** Bearbeiten Sie die JSON-Definition für das Dataset oder die Pipeline, um die Typen zu vereinheitlichen, und führen Sie die Bereitstellung erneut aus.


## <a name="azure-cosmos-db"></a>Azure Cosmos DB

### <a name="error-message-request-size-is-too-large"></a>Fehlermeldung: Anforderung ist zu groß.

- **Symptome:** Sie kopieren Daten mit der Standardgröße für Batchvorgänge in Azure Cosmos DB und erhalten einen Fehler der Art *„**Anforderung ist zu groß**“* .

- **Ursache:** In Cosmos DB ist die Größe einer einzelnen Anforderung auf 2 MB begrenzt. Die Formel lautet: Anforderungsgröße = Einzeldokumentgröße * Schreibbatchgröße. Wenn Ihr Dokument zu groß ist, ist beim Standardverhalten auch die Anforderung zu groß. Sie können die Batchgröße für Schreibvorgänge optimieren.

- **Lösung:** Reduzieren Sie in der Senke der Kopieraktivität den Wert für die „Schreibbatchgröße“ (Standardwert ist „10.000“).

### <a name="error-message-unique-index-constraint-violation"></a>Fehlermeldung: Einschränkungsverletzung für eindeutigen Index

- **Symptome:** Beim Kopieren von Daten in Cosmos DB tritt der folgende Fehler auf:

    ```
    Message=Partition range id 0 | Failed to import mini-batch. 
    Exception was Message: {"Errors":["Encountered exception while executing function. Exception = Error: {\"Errors\":[\"Unique index constraint violation.\"]}... 
    ```

- **Ursache:** Es gibt zwei mögliche Ursachen:

    - Wenn Sie **Einfügen** als Schreibverhalten verwenden, bedeutet dieser Fehler, dass Ihre Quelldaten Zeilen/Objekte mit der gleichen ID enthalten.
    - Wenn Sie **Upsert** als Schreibverhalten verwenden und einen anderen eindeutigen Schlüssel für den Container festlegen, bedeutet dieser Fehler, dass Ihre Quelldaten über Zeilen/Objekte mit unterschiedlichen IDs verfügen, aber für den definierten eindeutigen Schlüssel den gleichen Wert aufweisen.

- **Lösung:** 

    - Legen Sie für Ursache 1 die Option **Upsert** als Schreibverhalten fest.
    - Stellen Sie für Ursache 2 sicher, dass jedes Dokument über einen anderen Wert für den definierten eindeutigen Schlüssel verfügt.

### <a name="error-message-request-rate-is-large"></a>Fehlermeldung: Anforderungsrate ist hoch

- **Symptome:** Beim Kopieren von Daten in Cosmos DB tritt der folgende Fehler auf:

    ```
    Type=Microsoft.Azure.Documents.DocumentClientException,
    Message=Message: {"Errors":["Request rate is large"]}
    ```

- **Ursache:** Die verwendeten Anforderungseinheiten sind größer als die verfügbare Anforderungseinheit in Cosmos DB. Informationen zur Berechnung von Anforderungseinheiten in Cosmos DB finden Sie [hier](../cosmos-db/request-units.md#request-unit-considerations).

- **Lösung:** Hier sind zwei Lösungen beschrieben:

    - **Legen Sie in Cosmos DB für die Anforderungseinheit für Container einen höheren Wert fest**, um die Leistung der Kopieraktivität zu verbessern, auch wenn hierdurch der Kostenaufwand in Cosmos DB erhöht wird. 
    - Reduzieren Sie **writeBatchSize** auf einen niedrigeren Wert (z. B. 1.000), und legen Sie auch **parallelCopies** auf einen niedrigeren Wert fest (z. B. 1). Hierdurch verschlechtert sich die Leistung für die Ausführung des Kopiervorgangs, aber in Cosmos DB fallen keine höheren Kosten an.

### <a name="column-missing-in-column-mapping"></a>Fehlende Spalte in Spaltenzuordnung

- **Symptome:** Beim Importieren des Schemas für Cosmos DB für die Spaltenzuordnung fehlen einige Spalten. 

- **Ursache:** ADF leitet das Schema aus den ersten zehn Cosmos DB-Dokumenten ab. Falls einige Spalten/Eigenschaften in diesen Dokumenten nicht über einen Wert verfügen, werden sie von ADF nicht erkannt und somit auch nicht angezeigt.

- **Lösung:** Sie können die Abfrage wie unten angegeben optimieren, um zu erzwingen, dass die Spalte im Resultset mit einem leeren Wert angezeigt wird (Annahme: Spalte „impossible“ fehlt in den ersten zehn Dokumenten). Alternativ können Sie die Spalte auch manuell für die Zuordnung hinzufügen.

    ```sql
    select c.company, c.category, c.comments, (c.impossible??'') as impossible from c
    ```

### <a name="error-message-the-guidrepresentation-for-the-reader-is-csharplegacy"></a>Fehlermeldung: GuidRepresentation für den Reader ist CSharpLegacy

- **Symptome:** Beim Kopieren von Daten aus Cosmos DB MongoAPI/MongoDB mit UUID-Feld tritt der folgende Fehler auf:

    ```
    Failed to read data via MongoDB client.,
    Source=Microsoft.DataTransfer.Runtime.MongoDbV2Connector,Type=System.FormatException,
    Message=The GuidRepresentation for the reader is CSharpLegacy which requires the binary sub type to be UuidLegacy not UuidStandard.,Source=MongoDB.Bson,’“,
    ```

- **Ursache:** Es gibt zwei Möglichkeiten, UUID in BSON darzustellen: UuidStandard und UuidLegacy. Standardmäßig wird UuidLegacy zum Lesen von Daten verwendet. Ein Fehler tritt auf, wenn für Ihre UUID-Daten in MongoDB „UuidStandard“ festgelegt ist.

- **Lösung:** Fügen Sie der MongoDB-Verbindungszeichenfolge die Option „**uuidRepresentation=standard**“ hinzu. Weitere Informationen finden Sie im Artikel zur [MongoDB-Verbindungszeichenfolge](connector-mongodb.md#linked-service-properties).
            
## <a name="azure-cosmos-db-sql-api"></a>Azure Cosmos DB (SQL-API)

### <a name="error-code--cosmosdbsqlapioperationfailed"></a>Fehlercode:  CosmosDbSqlApiOperationFailed

- **Meldung**: `CosmosDbSqlApi operation Failed. ErrorMessage: %msg;.`

- **Ursache:** Beim CosmosDbSqlApi-Vorgang ist ein Problem aufgetreten.

- **Empfehlung**:  Überprüfen Sie den Fehler genau. Weitere Informationen finden Sie im [Hilfedokument für Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/troubleshoot-dot-net-sdk). Wenden Sie sich an das Cosmos DB-Team, wenn Sie weitere Hilfe benötigen.


## <a name="azure-data-lake-storage-gen1"></a>Azure Data Lake Storage Gen1

### <a name="error-message-the-underlying-connection-was-closed-could-not-establish-trust-relationship-for-the-ssltls-secure-channel"></a>Fehlermeldung: Die zugrunde liegende Verbindung wurde geschlossen: Es konnte keine Vertrauensstellung für den sicheren SSL/TLS-Kanal eingerichtet werden.

- **Symptome:** Bei der Kopieraktivität tritt der folgende Fehler auf: 

    ```
    Message: ErrorCode = `UserErrorFailedFileOperation`, Error Message = `The underlying connection was closed: Could not establish trust relationship for the SSL/TLS secure channel`.
    ```

- **Ursache:** Bei der Zertifikatüberprüfung ist während des TLS-Handshakes ein Fehler aufgetreten.

- **Lösung:** Problemumgehung: Verwenden Sie das gestaffelte Kopieren, um die TLS-Validierung für ADLS Gen1 zu überspringen. Sie müssen dieses Problem reproduzieren und die Netmon-Ablaufverfolgung erfassen. Anschließend prüfen Sie zusammen mit dem Netzwerkteam die Konfiguration des lokalen Netzwerks.

    ![Problembehandlung bei ADLS Gen1](./media/connector-troubleshoot-guide/adls-troubleshoot.png)


### <a name="error-message-the-remote-server-returned-an-error-403-forbidden"></a>Fehlermeldung: The remote server returned an error: (403) Unzulässig

- **Symptome:** Für die Kopieraktivität tritt der folgende Fehler auf: 

    ```
    Message: The remote server returned an error: (403) Forbidden.. 
    Response details: {"RemoteException":{"exception":"AccessControlException""message":"CREATE failed with error 0x83090aa2 (Forbidden. ACL verification failed. Either the resource does not exist or the user is not authorized to perform the requested operation.)....
    ```

- **Ursache:** Eine mögliche Ursache ist, dass der von Ihnen verwendete Dienstprinzipal oder die verwaltete Identität nicht über die Berechtigung zum Zugreifen auf den jeweiligen Ordner bzw. die Datei verfügt.

- **Lösung:** Gewähren Sie die entsprechenden Berechtigungen für alle Ordner und Unterordner, die Sie kopieren müssen. Weitere Informationen finden Sie in [diesem Dokument](connector-azure-data-lake-store.md#linked-service-properties).

### <a name="error-message-failed-to-get-access-token-by-using-service-principal-adal-error-service_unavailable"></a>Fehlermeldung: Fehler beim Abrufen des Zugriffstokens mit dem Dienstprinzipal. ADAL-Fehler: service_unavailable

- **Symptome:** Für die Kopieraktivität tritt der folgende Fehler auf:

    ```
    Failed to get access token by using service principal. 
    ADAL Error: service_unavailable, The remote server returned an error: (503) Server Unavailable.
    ```

- **Ursache:** Wenn der Tokenserver des Diensts von Azure Active Directory nicht verfügbar ist – also die Auslastung für die Verarbeitung von Anforderungen zu hoch ist –, wird der HTTP-Fehler 503 zurückgegeben. 

- **Lösung:** Führen Sie die Kopieraktivität nach einigen Minuten erneut aus.


## <a name="azure-data-lake-storage-gen2"></a>Azure Data Lake Storage Gen2

### <a name="error-code-adlsgen2operationfailed"></a>Fehlercode: ADLSGen2OperationFailed

- **Meldung**: `ADLS Gen2 operation failed for: %adlsGen2Message;.%exceptionData;.`

- **Ursache:** ADLS Gen2 gibt den Fehler aus, der anzeigt, dass der Vorgang fehlgeschlagen ist.

- **Empfehlung**:  Überprüfen Sie die von ADLS Gen2 ausgegebene detaillierte Fehlermeldung. Versuchen Sie es nochmal, wenn dies durch einen vorübergehenden Fehler verursacht wird. Wenn Sie weitere Hilfe benötigen, wenden Sie sich an den Azure Storage-Support, und geben Sie die Anforderungs-ID in der Fehlermeldung an.

- **Ursache:** Wenn die Fehlermeldung „Forbidden“ (Verboten) enthält, verfügt der von Ihnen verwendete Dienstprinzipal oder die verwaltete Identität möglicherweise nicht über die erforderliche Berechtigung, um auf ADLS Gen2 zuzugreifen.

- **Empfehlung**:  Weitere Informationen finden Sie in diesem Hilfedokument: https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage#service-principal-authentication.

- **Ursache:** Wenn die Fehlermeldung „InternalServerError“ enthält, wird der Fehler von ADLS Gen2 zurückgegeben.

- **Empfehlung**:  Dies wird möglicherweise durch einen vorübergehenden Fehler verursacht. Versuchen Sie es daher nochmal. Wenden Sie sich an den Azure Storage-Support, und geben Sie die Anforderungs-ID in der Fehlermeldung an, wenn das Problem weiterhin besteht.

### <a name="request-to-adls-gen2-account-met-timeout-error"></a>Timeoutfehler bei der Anforderung an das ADLS Gen2-Konto

- **Meldung**: Fehlercode: `UserErrorFailedBlobFSOperation`, Fehlermeldung: `BlobFS operation failed for: A task was canceled`.

- **Ursache:** Das Problem wird durch einen Timeoutfehler der ADLS Gen2-Senke verursacht, der meist auf dem Computer mit der selbstgehosteten Integration Runtime auftritt.

- **Empfehlung**: 

    - Platzieren Sie den Computer mit der selbstgehosteten IR und das ADLS Gen2-Zielkonto wenn möglich in derselben Region. Dadurch vermeiden Sie zufällige Timeoutfehler und erzielen eine bessere Leistung.

    - Überprüfen Sie, ob spezielle Netzwerkeinstellungen wie ExpressRoute vorliegen, und stellen Sie sicher, dass das Netzwerk über genügend Bandbreite verfügt. Es wird empfohlen, die Einstellung für die Anzahl paralleler Aufträge für die selbstgehostete IR zu verringern, wenn die Gesamtbandbreite niedrig ist. So können Sie Konflikte bei Netzwerkressourcen vermeiden, wenn Sie mehrere Aufträge gleichzeitig ausführen.

    - Verwenden Sie für nicht binäre Kopiervorgänge eine kleinere Blockgröße, um derartige Timeoutfehler bei kleinen oder mittleren Dateigrößen zu vermeiden. Weitere Informationen finden Sie unter [Blob Storage: PUT-Block](https://docs.microsoft.com/rest/api/storageservices/put-block).

       Um eine benutzerdefinierte Blockgröße anzugeben, bearbeiten Sie die Eigenschaft im JSON-Editor:
        ```
        "sink": {
            "type": "DelimitedTextSink",
            "storeSettings": {
                "type": "AzureBlobFSWriteSettings",
                "blockSizeInMB": 8
            }
        }
        ```

                  
## <a name="azure-file-storage"></a>Azure File Storage

### <a name="error-code--azurefileoperationfailed"></a>Fehlercode:  AzureFileOperationFailed

- **Meldung**: `Azure File operation Failed. Path: %path;. ErrorMessage: %msg;.`

- **Ursache:** Beim Azure File Storage-Vorgang ist ein Problem aufgetreten.

- **Empfehlung**:  Überprüfen Sie den Fehler genau. Weitere Informationen finden Sie in diesem Azure File-Hilfedokument: https://docs.microsoft.com/rest/api/storageservices/file-service-error-codes. Wenden Sie sich an das Storage-Team, wenn Sie weitere Hilfe benötigen.


## <a name="azure-synapse-analyticsazure-sql-databasesql-server"></a>Azure Synapse Analytics/Azure SQL-Datenbank/SQL Server

### <a name="error-code--sqlfailedtoconnect"></a>Fehlercode:  SqlFailedToConnect

- **Meldung**: `Cannot connect to SQL Database: '%server;', Database: '%database;', User: '%user;'. Check the linked service configuration is correct, and make sure the SQL Database firewall allows the integration runtime to access.`

- **Ursache:** Azure SQL: Wenn die Fehlermeldung „SqlErrorNumber=47073“ enthält, bedeutet dies, dass der öffentliche Netzwerkzugriff in der Konnektivitätseinstellung verweigert wird.

- **Empfehlung**: Legen Sie in der Azure SQL-Firewall die Option „Öffentlichen Netzwerkzugriff verweigern“ auf „Nein“ fest. Weitere Informationen dazu finden Sie https://docs.microsoft.com/azure/azure-sql/database/connectivity-settings#deny-public-network-access.

- **Ursache:** Azure SQL: Wenn die Fehlermeldung einen SQL-Fehlercode wie „SqlErrorNumber=[errorcode]“ enthält, finden Sie weitere Informationen im Leitfaden zur Problembehandlung bei Azure SQL.

- **Empfehlung**: Weitere Informationen finden Sie unter https://docs.microsoft.com/azure/azure-sql/database/troubleshoot-common-errors-issues.

- **Ursache:** Überprüfen Sie, ob Port 1433 in der Zulassungsliste der Firewall enthalten ist.

- **Empfehlung**: Folgen Sie diesem Referenzdokument: https://docs.microsoft.com/sql/sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access#ports-used-by-.

- **Ursache:** Wenn die Fehlermeldung „SqlException“ enthält, löst SQL-Datenbank den Fehler aus, der angibt, dass ein bestimmter Vorgang fehlgeschlagen ist.

- **Empfehlung**:  Weitere Informationen zu SQL-Fehlercodes finden Sie im Referenzdokument unter https://docs.microsoft.com/sql/relational-databases/errors-events/database-engine-events-and-errors. Wenn Sie weitere Hilfe benötigen, wenden Sie sich an den Azure SQL-Support.

- **Ursache:** Wenn dies ein vorübergehendes Problem ist (z. B. eine instabile Netzwerkverbindung), fügen Sie zur Behebung in der Aktivitätsrichtlinie einen Wiederholungsversuch hinzu.

- **Empfehlung**:  Folgen Sie diesem Referenzdokument: https://docs.microsoft.com/azure/data-factory/concepts-pipelines-activities#activity-policy.

- **Ursache:** Wenn die Fehlermeldung „Der Client mit der IP-Adresse [...] hat keine Zugriffsberechtigung für den Server“ enthält und Sie versuchen, eine Verbindung mit Azure SQL-Datenbank herzustellen, wird dies normalerweise durch ein Problem mit der Azure SQL-Datenbank-Firewall verursacht.

- **Empfehlung**:  Aktivieren Sie in der Azure SQL Server-Firewallkonfiguration die Option „Allow Azure services and resources to access this server“ (Anderen Azure-Diensten und -Ressourcen den Zugriff auf diesen Server gestatten). Das Referenzdokument finden Sie unter https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure.


### <a name="error-code--sqloperationfailed"></a>Fehlercode:  SqlOperationFailed

- **Meldung**: `A database operation failed. Please search error to get more details.`

- **Ursache:** Wenn die Fehlermeldung „SqlException“ enthält, löst SQL-Datenbank den Fehler aus, der angibt, dass ein bestimmter Vorgang fehlgeschlagen ist.

- **Empfehlung**:  Wenn der SQL-Fehler nicht eindeutig ist, versuchen Sie, die Datenbank auf den aktuellen Kompatibilitätsgrad „150“ zu ändern. Es können SQL-Fehler der neuesten Version ausgelöst werden. Weitere Informationen finden Sie im [Detaildokument](/sql/t-sql/statements/alter-database-transact-sql-compatibility-level#backwardCompat).

    Weitere Informationen zur Problembehandlung bei SQL-Problemen finden Sie unter „SQL-Fehlercode“ in diesem Referenzdokument: https://docs.microsoft.com/sql/relational-databases/errors-events/database-engine-events-and-errors. Wenn Sie weitere Hilfe benötigen, wenden Sie sich an den Azure SQL-Support.

- **Ursache:** Wenn die Fehlermeldung „PdwManagedToNativeInteropException“ enthält, wird sie normalerweise durch einen Konflikt zwischen Quell- und Senkspaltengrößen verursacht.

- **Empfehlung**:  Überprüfen Sie die Größe der Quell- und Senkenspalten. Wenn Sie weitere Hilfe benötigen, wenden Sie sich an den Azure SQL-Support.

- **Ursache:** Wenn die Fehlermeldung „InvalidOperationException“ enthält, wird sie normalerweise durch ungültige Eingabedaten verursacht.

- **Empfehlung**:  Wenn Sie ermitteln möchten, in welcher Zeile das Problem auftritt, aktivieren Sie die Fehlertoleranzfunktion für die Kopieraktivität, die problematische Zeilen zur weiteren Untersuchung in den Speicher umleiten kann. Das Referenzdokument finden Sie unter https://docs.microsoft.com/azure/data-factory/copy-activity-fault-tolerance.


### <a name="error-code--sqlunauthorizedaccess"></a>Fehlercode:  SqlUnauthorizedAccess

- **Meldung**: `Cannot connect to '%connectorName;'. Detail Message: '%message;'`

- **Ursache:** Die Anmeldeinformationen sind falsch, oder das Anmeldekonto kann auf SQL-Datenbank nicht zugreifen.

- **Empfehlung**:  Überprüfen Sie, ob das Anmeldekonto über die erforderliche Berechtigung für den Zugriff auf die SQL-Datenbank verfügt.


### <a name="error-code--sqlopenconnectiontimeout"></a>Fehlercode:  SqlOpenConnectionTimeout

- **Meldung**: `Open connection to database timeout after '%timeoutValue;' seconds.`

- **Ursache:** Dies könnte ein vorübergehender SQL-Datenbank-Fehler sein.

- **Empfehlung**:  Versuchen Sie nochmal, die Verbindungszeichenfolge für den verknüpften Dienst mit einem größeren Verbindungszeitwert zu aktualisieren.


### <a name="error-code--sqlautocreatetabletypemapfailed"></a>Fehlercode:  SqlAutoCreateTableTypeMapFailed

- **Meldung**: `Type '%dataType;' in source side cannot be mapped to a type that supported by sink side(column name:'%columnName;') in autocreate table.`

- **Ursache:** Die automatische Erstellung der Tabelle kann die Quellanforderungen nicht erfüllen.

- **Empfehlung**:  Aktualisieren Sie den Spaltentyp in „mappings“ (Zuordnungen), oder erstellen Sie manuell die Senkentabelle auf dem Zielserver.


### <a name="error-code--sqldatatypenotsupported"></a>Fehlercode:  SqlDataTypeNotSupported

- **Meldung**: `A database operation failed. Check the SQL errors.`

- **Ursache:** Wenn das Problem in der SQL-Quelle auftritt und der Fehler im Zusammenhang mit dem SqlDateTime-Überlauf steht, liegt der Datenwert über dem logischen Eingabebereich (01.01.1753 12:00:00 Uhr bis 31.12.9999 23:59:59 Uhr).

- **Empfehlung**:  Wandeln Sie den Typ in der SQL-Quellabfrage in eine Zeichenfolge um, oder ändern Sie in der Spaltenzuordnung der Kopieraktivität den Spaltentyp in „String“.

- **Ursache:** Wenn das Problem in der SQL-Senkentabelle auftritt und der Fehler im Zusammenhang mit dem SqlDateTime-Überlauf steht, liegt der Datenwert über dem zulässigen Bereich in der Senkentabelle.

- **Empfehlung**:  Aktualisieren Sie den entsprechenden Spaltentyp in der Senkentabelle auf den Typ „datetime2“.


### <a name="error-code--sqlinvaliddbstoredprocedure"></a>Fehlercode:  SqlInvalidDbStoredProcedure

- **Meldung**: `The specified Stored Procedure is not valid. It could be caused by that the stored procedure doesn't return any data. Invalid Stored Procedure script: '%scriptName;'.`

- **Ursache:** Die angegebene gespeicherte Prozedur ist ungültig. Dies kann daran liegen, dass die gespeicherte Prozedur keine Daten zurückgibt.

- **Empfehlung**:  Überprüfen Sie die gespeicherte Prozedur mithilfe von SQL-Tools. Stellen Sie sicher, dass die gespeicherte Prozedur Daten zurückgeben kann.


### <a name="error-code--sqlinvaliddbquerystring"></a>Fehlercode:  SqlInvalidDbQueryString

- **Meldung**: `The specified SQL Query is not valid. It could be caused by that the query doesn't return any data. Invalid query: '%query;'`

- **Ursache:** Die angegebene SQL-Abfrage ist ungültig. Dies kann daran liegen, dass die Abfrage keine Daten zurückgibt.

- **Empfehlung**:  Überprüfen Sie die SQL-Abfrage mithilfe von SQL-Tools. Stellen Sie sicher, dass die Abfrage Daten zurückgeben kann.


### <a name="error-code--sqlinvalidcolumnname"></a>Fehlercode:  SqlInvalidColumnName

- **Meldung**: `Column '%column;' does not exist in the table '%tableName;', ServerName: '%serverName;', DatabaseName: '%dbName;'.`

- **Ursache:** Die Spalte wurde nicht gefunden. Die Konfiguration ist möglicherweise falsch.

- **Empfehlung**:  Überprüfen Sie die Spalte in der Abfrage – „Struktur“ im Dataset und „Zuordnungen“ in der Aktivität.


### <a name="error-code--sqlbatchwritetimeout"></a>Fehlercode:  SqlBatchWriteTimeout

- **Meldung**: `Timeouts in SQL write operation.`

- **Ursache:** Dies könnte ein vorübergehender SQL-Datenbank-Fehler sein.

- **Empfehlung**:  Wiederholen Sie den Vorgang. Wenn nochmal ein Problem auftritt, wenden Sie sich an den Azure SQL-Support.


### <a name="error-code--sqlbatchwritetransactionfailed"></a>Fehlercode:  SqlBatchWriteTransactionFailed

- **Meldung**: `SQL transaction commits failed`

- **Ursache:** Wenn die Ausnahmedetails ständig ein Transaktionstimeout melden, ist die Netzwerklatenz zwischen Integration Runtime und Datenbank höher als der Standardschwellenwert von 30 Sekunden.

- **Empfehlung**:  Aktualisieren Sie die Verbindungszeichenfolge für den verknüpften SQL-Dienst mit einem Wert für „Verbindungstimeout“ gleich mindestens 120, und führen Sie die Aktivität erneut aus.

- **Ursache:** Wenn die Ausnahmedetails für „sqlconnection“ zeitweilig „broken“ (unterbrochen) melden, könnte ein vorübergehender Netzwerkfehler oder ein Problem mit der SQL-Datenbank vorliegen.

- **Empfehlung**:  Wiederholen Sie die Aktivität, und überprüfen Sie die Metriken der SQL-Datenbank.


### <a name="error-code--sqlbulkcopyinvalidcolumnlength"></a>Fehlercode:  SqlBulkCopyInvalidColumnLength

- **Meldung**: `SQL Bulk Copy failed due to receive an invalid column length from the bcp client.`

- **Ursache:** Beim SQL-Massenkopiervorgang ist ein Fehler aufgetreten, weil eine ungültige Spaltenlänge vom BCP-Client empfangen wurde.

- **Empfehlung**:  Wenn Sie ermitteln möchten, in welcher Zeile das Problem auftritt, aktivieren Sie die Fehlertoleranzfunktion für die Kopieraktivität, die problematische Zeilen zur weiteren Untersuchung in den Speicher umleiten kann. Das Referenzdokument finden Sie unter https://docs.microsoft.com/azure/data-factory/copy-activity-fault-tolerance.


### <a name="error-code--sqlconnectionisclosed"></a>Fehlercode:  SqlConnectionIsClosed

- **Meldung**: `The connection is closed by SQL Database.`

- **Ursache:** Die SQL-Verbindung wird durch SQL-Datenbank getrennt, wenn viele Vorgänge gleichzeitig durchgeführt werden und der Server die Verbindung trennt.

- **Empfehlung**:  Der Remoteserver hat die SQL-Verbindung getrennt. Wiederholen. Wenn nochmal ein Problem auftritt, wenden Sie sich an den Azure SQL-Support.


### <a name="error-message-conversion-failed-when-converting-from-a-character-string-to-uniqueidentifier"></a>Fehlermeldung: Fehler beim Konvertieren einer Zeichenfolge in „uniqueidentifier“.

- **Symptome:** Wenn Sie Daten aus der Tabellendatenquelle (z. B. SQL Server) in Azure Synapse Analytics kopieren, indem Sie das gestaffelte Kopieren und PolyBase verwenden, tritt der folgende Fehler auf:

    ```
    ErrorCode=FailedDbOperation,Type=Microsoft.DataTransfer.Common.Shared.HybridDeliveryException,
    Message=Error happened when loading data into Azure Synapse Analytics.,
    Source=Microsoft.DataTransfer.ClientLibrary,Type=System.Data.SqlClient.SqlException,
    Message=Conversion failed when converting from a character string to uniqueidentifier...
    ```

- **Ursache:** Mit Azure Synapse Analytics PolyBase kann eine leere Zeichenfolge nicht in eine GUID konvertiert werden.

- **Lösung:** Legen Sie in der Senke der Kopieraktivität unter den PolyBase-Einstellungen die Option „**use type default**“ auf „false“ fest.


### <a name="error-message-expected-data-type-decimalxx-offending-value"></a>Fehlermeldung: Erwarteter Datentyp: DECIMAL(x,x), Auslösender Wert

- **Symptome:** Wenn Sie Daten aus der Tabellendatenquelle (z. B. SQL Server) in Azure Synapse Analytics kopieren, indem Sie das gestaffelte Kopieren und PolyBase verwenden, tritt der folgende Fehler auf:

    ```
    ErrorCode=FailedDbOperation,Type=Microsoft.DataTransfer.Common.Shared.HybridDeliveryException,
    Message=Error happened when loading data into Azure Synapse Analytics.,
    Source=Microsoft.DataTransfer.ClientLibrary,Type=System.Data.SqlClient.SqlException,
    Message=Query aborted-- the maximum reject threshold (0 rows) was reached while reading from an external source: 1 rows rejected out of total 415 rows processed. (/file_name.txt) 
    Column ordinal: 18, Expected data type: DECIMAL(x,x), Offending value:..
    ```

- **Ursache:** Azure Synapse Analytics PolyBase kann in eine Dezimalspalte keine leere Zeichenfolge (NULL-Wert) einfügen.

- **Lösung:** Legen Sie in der Senke der Kopieraktivität unter den PolyBase-Einstellungen die Option „**use type default**“ auf „false“ fest.


### <a name="error-message-java-exception-message-hdfsbridgecreaterecordreader"></a>Fehlermeldung: Java-Ausnahmemeldung: HdfsBridge::CreateRecordReader

- **Symptome:** Sie kopieren Daten mit PolyBase in Azure Synapse Analytics und erhalten den folgenden Fehler:

    ```
    Message=110802;An internal DMS error occurred that caused this operation to fail. 
    Details: Exception: Microsoft.SqlServer.DataWarehouse.DataMovement.Common.ExternalAccess.HdfsAccessException, 
    Message: Java exception raised on call to HdfsBridge_CreateRecordReader. 
    Java exception message:HdfsBridge::CreateRecordReader - Unexpected error encountered creating the record reader.: Error [HdfsBridge::CreateRecordReader - Unexpected error encountered creating the record reader.] occurred while accessing external file.....
    ```

- **Ursache:** Die mögliche Ursache ist, dass das Schema (gesamte Spaltenbreite) zu groß ist (größer als 1 MB). Überprüfen Sie das Schema der Azure Synapse Analytics-Zieltabelle, indem Sie die Größe aller Spalten hinzufügen:

    - Int -> 4 Byte
    - Bigint -> 8 Byte
    - Varchar(n),char(n),binary(n), varbinary(n) -> N Byte
    - Nvarchar(n), nchar(n) -> N*2 Byte
    - Date -> 6 Byte
    - Datetime/(2), smalldatetime -> 16 Byte
    - Datetimeoffset -> 20 Byte
    - Decimal -> 19 Byte
    - Float -> 8 Byte
    - Money -> 8 Byte
    - Smallmoney -> 4 Byte
    - Real -> 4 Byte
    - Smallint -> 2 Byte
    - Time -> 12 Byte
    - Tinyint -> 1 Byte

- **Lösung:** 
    - Reduzieren Sie die Spaltenbreite auf weniger als 1 MB.
    - Sie können auch das Masseneinfügen verwenden, indem Sie PolyBase deaktivieren.


### <a name="error-message-the-condition-specified-using-http-conditional-headers-is-not-met"></a>Fehlermeldung: Die mit bedingtem HTTP-Header angegebene Bedingung ist nicht erfüllt.

- **Symptome:** Sie verwenden die SQL-Abfrage zum Abrufen von Daten per Pullvorgang aus Azure Synapse Analytics und erhalten den folgenden Fehler:

    ```
    ...StorageException: The condition specified using HTTP conditional header(s) is not met...
    ```

- **Ursache:** Für Azure Synapse Analytics ist beim Abfragen der externen Tabelle in Azure Storage ein Fehler aufgetreten.

- **Lösung:** Führen Sie die gleiche Abfrage in SSMS aus, und überprüfen Sie, ob das gleiche Ergebnis angezeigt wird. Wenn ja: Erstellen Sie ein Supportticket für Azure Synapse Analytics, und geben Sie für die weitere Problembehandlung den Namen Ihres Azure Synapse Analytics-Servers und der Datenbank an.


### <a name="performance-tier-is-low-and-leads-to-copy-failure"></a>Die Leistungsstufe ist niedrig und führt zu einem Kopierfehler

- **Symptome:** Folgende Fehlermeldung wurde beim Kopieren von Daten in Azure SQL-Datenbank angezeigt: `Database operation failed. Error message from database execution : ExecuteNonQuery requires an open and available Connection. The connection's current state is closed.`

- **Ursache:** Azure SQL-Datenbank S1 wird gerade verwendet. Dabei können in einem solchen Fall die E/A-Grenzwerte erreicht werden.

- **Lösung:** Führen Sie ein Upgrade der Azure SQL-Datenbank-Leistungsstufe durch, um das Problem zu beheben. 


### <a name="sql-table-cannot-be-found"></a>SQL-Tabelle kann nicht gefunden werden 

- **Symptome:** Fehler beim Kopieren von Daten aus einem Hybridsystem in eine lokale SQL Server-Tabelle: `Cannot find the object "dbo.Contoso" because it does not exist or you do not have permissions.`

- **Ursache:** Das aktuelle SQL-Konto verfügt nicht über ausreichende Berechtigungen zum Ausführen von Anforderungen, die von .NET SqlBulkCopy.WriteToServer ausgegeben wurden.

- **Lösung:** Wechseln Sie zu einem SQL-Konto mit höheren Berechtigungen.


### <a name="error-message-string-or-binary-data-would-be-truncated"></a>Fehlermeldung: String- oder binary-Daten würden abgeschnitten

- **Symptome:** Fehler beim Kopieren von Daten in eine lokale/Azure SQL Server-Tabelle: 

- **Ursache:** Die Definition des Cx Sql-Tabellenschemas weist mindestens eine Spalte mit einer geringeren Länge als erwartet auf.

- **Lösung:** Führen Sie die folgenden Schritte aus, um das Problem zu beheben:

    1. Wenden Sie eine [Fehlertoleranz](https://docs.microsoft.com/azure/data-factory/copy-activity-fault-tolerance) für die SQL-Senke (insbesondere „redirectIncompatibleRowSettings“) an, um die Zeilen mit dem Problem zu korrigieren.

        > [!NOTE]
        > Beachten Sie, dass eine Fehlertoleranz zusätzliche Ausführungszeit mit sich bringen kann, was möglicherweise zu höheren Kosten führt.

    2. Überprüfen Sie die Spaltenlänge der umgeleiteten Daten im SQL-Tabellenschema genau, um zu ermitteln, welche Spalten aktualisiert werden müssen.

    3. Aktualisieren Sie das Tabellenschema entsprechend.


## <a name="azure-table-storage"></a>Azure Table Storage

### <a name="error-code--azuretableduplicatecolumnsfromsource"></a>Fehlercode:  AzureTableDuplicateColumnsFromSource

- **Meldung**: `Duplicate columns with same name '%name;' are detected from source. This is NOT supported by Azure Table Storage sink`

- **Ursache:** Dies könnte häufig bei SQL-Abfragen mit Join oder unstrukturierten CSV-Dateien der Fall sein.

- **Empfehlung**: Überprüfen Sie die Quellspalten, und korrigieren Sie diese entsprechend.


## <a name="db2"></a>DB2

### <a name="error-code--db2driverrunfailed"></a>Fehlercode:  DB2DriverRunFailed

- **Meldung**: `Error thrown from driver. Sql code: '%code;'`

- **Ursache:** Wenn die Fehlermeldung „SQLSTATE=51002 SQLCODE =-805“ enthält, lesen Sie den Tipp in diesem Dokument: https://docs.microsoft.com/azure/data-factory/connector-db2#linked-service-properties

- **Empfehlung**:  Versuchen Sie, „NULLID“ in der Eigenschaft „packageCollection“ festzulegen.


## <a name="delimited-text-format"></a>Textformat mit Trennzeichen

### <a name="error-code--delimitedtextcolumnnamenotallownull"></a>Fehlercode:  DelimitedTextColumnNameNotAllowNull

- **Meldung**: `The name of column index %index; is empty. Make sure column name is properly specified in the header row.`

- **Ursache:** Wenn in der Aktivität der Wert „firstRowAsHeader“ festgelegt wurde, wird die erste Zeile als Spaltenname verwendet. Dieser Fehler bedeutet, dass die erste Zeile einen leeren Wert enthält. Beispiel: „ColumnA, ColumnB“.

- **Empfehlung**:  Überprüfen Sie die erste Zeile, und korrigieren Sie den Wert, wenn ein leerer Wert vorhanden ist.


### <a name="error-code--delimitedtextmorecolumnsthandefined"></a>Fehlercode:  DelimitedTextMoreColumnsThanDefined

- **Meldung**: `Error found when processing '%function;' source '%name;' with row number %rowCount;: found more columns than expected column count: %expectedColumnCount;.`

- **Ursache:** Die Spaltenanzahl der problematischen Zeile ist größer als die Spaltenanzahl der ersten Zeile. Dies kann durch ein Datenproblem oder falsche Einstellungen für Spaltentrennzeichen/Anführungszeichen verursacht werden.

- **Empfehlung**:  Rufen Sie die Zeilenanzahl in der Fehlermeldung ab, überprüfen Sie die Spalte in der Zeile, und korrigieren Sie die Daten.

- **Ursache:** Wenn die erwartete Spaltenanzahl in der Fehlermeldung „1“ ist, haben Sie möglicherweise fehlerhafte Komprimierungs- oder Formateinstellungen angegeben. Daher hat ADF die Dateien falsch analysiert.

- **Empfehlung**:  Überprüfen Sie die Formateinstellungen, um sicherzustellen, dass sie mit Ihrer/Ihren Quelldatei(en) übereinstimmen.

- **Ursache:** Wenn Ihre Quelle ein Ordner ist, weisen die Dateien im angegebenen Ordner möglicherweise unterschiedliche Schemas auf.

- **Empfehlung**:  Stellen Sie sicher, dass das Schema der Dateien im angegebenen Ordner identisch ist.


## <a name="dynamics-365common-data-servicedynamics-crm"></a>Dynamics 365/Common Data Service/Dynamics CRM

### <a name="error-code--dynamicscreateserviceclienterror"></a>Fehlercode:  DynamicsCreateServiceClientError

- **Meldung**: `This is a transient issue on dynamics server side. Try to rerun the pipeline.`

- **Ursache:** Dies ist ein vorübergehendes Problem bei dem Dynamics-Server.

- **Empfehlung**:  Führen Sie die Pipeline erneut aus. Wenn ein Fehler auftritt, versuchen Sie, die Parallelität zu verringern. Wenn weiterhin Fehler auftreten, wenden Sie sich an den Dynamics-Support.


### <a name="columns-are-missing-when-previewingimporting-schema"></a>Beim Anzeigen einer Vorschau oder dem Importieren des Schemas fehlen Spalten

- **Symptome:** Beim Importieren des Schemas oder dem Anzeigen einer Vorschau der Daten fehlen einige Spalten. Fehlermeldung: `The valid structure information (column name and type) are required for Dynamics source.`

- **Ursache:** Dieses Problem ist wahrscheinlich entwurfsbedingt: ADF kann keine Spalten anzeigen, die in den ersten 10 Datensätzen keinen Wert enthalten. Stellen Sie sicher, dass die hinzugefügten Spalten das richtige Format aufweisen. 

- **Empfehlung**: Fügen Sie die Spalten auf der Registerkarte „Zuordnung“ manuell hinzu.


### <a name="error-code--dynamicsmissingtargetformultitargetlookupfield"></a>Fehlercode:  DynamicsMissingTargetForMultiTargetLookupField

- **Meldung**: `Cannot find the target column for multi-target lookup field: '%fieldName;'.`

- **Ursache:** Die Zielspalte ist in der Quelle oder der Spaltenzuordnung nicht vorhanden.

- **Empfehlung**:  1. Stellen Sie sicher, dass die Quelle die Zielspalte enthält. 2. Fügen Sie die Zielspalte in der Spaltenzuordnung hinzu. Stellen Sie sicher, dass die Senkenspalte das Muster „{fieldName}@EntityReference“ aufweist.


### <a name="error-code--dynamicsinvalidtargetformultitargetlookupfield"></a>Fehlercode:  DynamicsInvalidTargetForMultiTargetLookupField

- **Meldung**: `The provided target: '%targetName;' is not a valid target of field: '%fieldName;'. Valid targets are: '%validTargetNames;"`

- **Ursache:** Ein falscher Entitätsname wird als Zielentität eines Nachschlagefelds mit mehreren Zielen bereitgestellt.

- **Empfehlung**:  Geben Sie einen gültigen Entitätsnamen für das Nachschlagefeld mit mehreren Zielen an.


### <a name="error-code--dynamicsinvalidtypeformultitargetlookupfield"></a>Fehlercode:  DynamicsInvalidTypeForMultiTargetLookupField

- **Meldung**: `The provided target type is not a valid string. Field: '%fieldName;'.`

- **Ursache:** Der Wert in der Zielspalte ist keine Zeichenfolge.

- **Empfehlung**:  Geben Sie eine gültige Zeichenfolge in der Zielspalte für das Nachschlagen mit mehreren Zielen an.


### <a name="error-code--dynamicsfailedtorequetserver"></a>Fehlercode:  DynamicsFailedToRequetServer

- **Meldung**: `The dynamics server or the network is experiencing issues. Check network connectivity or check dynamics server log for more details.`

- **Ursache:** Der Dynamics-Server ist instabil, es kann nicht darauf zugegriffen werden oder im Netzwerk treten Probleme auf.

- **Empfehlung**:  Überprüfen Sie die Netzwerkkonnektivität, oder suchen Sie im Dynamics-Serverprotokoll nach weiteren Details. Wenden Sie sich an den Dynamics-Support, um weitere Unterstützung zu erhalten.
    

## <a name="ftp"></a>FTP

### <a name="error-code--ftpfailedtoconnecttoftpserver"></a>Fehlercode:  FtpFailedToConnectToFtpServer

- **Meldung**: `Failed to connect to FTP server. Please make sure the provided server informantion is correct, and try again.`

- **Ursache:** Möglicherweise wird ein falscher Typ von verknüpftem Dienst für den FTP-Server verwendet. Beispielsweise wird mit einem verknüpften SFTP-Dienst versucht, eine Verbindung mit einem FTP-Server herzustellen.

- **Empfehlung**:  Überprüfen Sie den Port des Zielservers. Standardmäßig verwendet FTP den Port 21.


## <a name="http"></a>Http

### <a name="error-code--httpfilefailedtoread"></a>Fehlercode:  HttpFileFailedToRead

- **Meldung**: `Failed to read data from http server. Check the error from http server：%message;`

- **Ursache:** Dieser Fehler tritt auf, wenn Azure Data Factory mit dem HTTP-Server kommuniziert, beim HTTP-Anforderungsvorgang jedoch ein Fehler auftritt.

- **Empfehlung**:  Überprüfen Sie den HTTP-Statuscode bzw. die HTTP-Statusnachricht in der Fehlermeldung, und beheben Sie das Remoteserverproblem.


## <a name="oracle"></a>Oracle

### <a name="error-code-argumentoutofrangeexception"></a>Fehlercode: ArgumentOutOfRangeException

- **Meldung**: `Hour, Minute, and Second parameters describe an un-representable DateTime.`

- **Ursache:** In ADF werden DateTime-Werte im Bereich von 0001-01-01 00:00:00 bis 9999-12-31 23:59:59 unterstützt. Oracle unterstützt jedoch einen größeren Bereich von DateTime-Werten (z. B. Jahreszahlen vor unserer Zeitrechnung oder Minuten-/Sekundenangaben > 59). Dies kann in ADF zu einem Fehler führen.

- **Empfehlung**: 

    Führen Sie `select dump(<column name>)` aus, um zu überprüfen, ob sich der Wert in Oracle im Bereich von ADF befindet. 

    Wenn Sie die Bytesequenz im Ergebnis erfahren möchten, überprüfen Sie https://stackoverflow.com/questions/13568193/how-are-dates-stored-in-oracle.


## <a name="orc-format"></a>ORC-Format

### <a name="error-code--orcjavainvocationexception"></a>Fehlercode:  OrcJavaInvocationException

- **Meldung**: `An error occurred when invoking java, message: %javaException;.`

- **Ursache:** Wenn die Fehlermeldung „java.lang.OutOfMemory“, „Java heap space“ und „doubleCapacity“ enthält, liegt normalerweise ein Speicherverwaltungsproblem in der alten Version von Integration Runtime vor.

- **Empfehlung**:  Wenn Sie die selbstgehostete Integration Runtime verwenden, empfiehlt es sich, ein Upgrade auf die neueste Version durchzuführen.

- **Ursache:** Wenn die Fehlermeldung „java.lang.OutOfMemory“ enthält, verfügt die Integration Runtime nicht über genügend Ressourcen zum Verarbeiten der Datei(en).

- **Empfehlung**:  Begrenzen Sie die gleichzeitigen Ausführungen auf die Integration Runtime. Skalieren Sie für „Selbstgehostete Integration Runtime“ hoch auf einen leistungsfähigen Computer mit einem Arbeitsspeicher von mindestens 8 GB.

- **Ursache:** Wenn die Fehlermeldung „NullPointerReference“ enthält, ist möglicherweise ein vorübergehender Fehler aufgetreten.

- **Empfehlung**:  Wiederholen Sie den Vorgang. Wenn das Problem weiterhin besteht, wenden Sie sich an den Support.

- **Ursache:** Wenn die Fehlermeldung „BufferOverflowException“ enthält, ist möglicherweise ein vorübergehender Fehler aufgetreten.

- **Empfehlung**:  Wiederholen Sie den Vorgang. Wenn das Problem weiterhin besteht, wenden Sie sich an den Support.

- **Ursache:** Wenn die Fehlermeldung „java.lang.ClassCastException:org.apache.hadoop.hive.serde2.io.HiveCharWritable cannot be cast to org.apache.hadoop.io.Text“ enthält, handelt es sich um ein Typkonvertierungsproblem in Java Runtime. Normalerweise wird dies dadurch verursacht, das Quelldaten in Java Runtime nicht gut verarbeitet werden können.

- **Empfehlung**:  Dies ist ein Datenproblem. Versuchen Sie, „string“ anstelle von „char/varchar“ in den Daten im ORC-Format zu verwenden.

### <a name="error-code--orcdatetimeexceedlimit"></a>Fehlercode:  OrcDateTimeExceedLimit

- **Meldung**: `The Ticks value '%ticks;' for the datetime column must be between valid datetime ticks range -621355968000000000 and 2534022144000000000.`

- **Ursache:** Wenn der DateTime-Wert „0001-01-01 00:00:00“ lautet, kann dies durch den Unterschied zwischen julianischem Kalender und gregorianischem Kalender verursacht werden. https://en.wikipedia.org/wiki/Proleptic_Gregorian_calendar#Difference_between_Julian_and_proleptic_Gregorian_calendar_dates.

- **Empfehlung**:  Überprüfen Sie den Taktwert, und vermeiden Sie den DateTime-Wert „0001-01-01 00:00:00“.


## <a name="parquet-format"></a>Parquet-Format

### <a name="error-code--parquetjavainvocationexception"></a>Fehlercode:  ParquetJavaInvocationException

- **Meldung**: `An error occurred when invoking java, message: %javaException;.`

- **Ursache:** Wenn die Fehlermeldung „java.lang.OutOfMemory“, „Java heap space“ und „doubleCapacity“ enthält, liegt normalerweise ein Speicherverwaltungsproblem in der alten Version von Integration Runtime vor.

- **Empfehlung**:  Wenn Sie selbstgehostete Integration Runtime verwenden und die Version älter als 3.20.7159.1 ist, wird empfohlen, ein Upgrade auf die neueste Version durchzuführen.

- **Ursache:** Wenn die Fehlermeldung „java.lang.OutOfMemory“ enthält, verfügt die Integration Runtime nicht über genügend Ressourcen zum Verarbeiten der Datei(en).

- **Empfehlung**:  Begrenzen Sie die gleichzeitigen Ausführungen auf die Integration Runtime. Skalieren Sie für „Selbstgehostete Integration Runtime“ hoch auf einen leistungsfähigen Computer mit einem Arbeitsspeicher von mindestens 8 GB.

- **Ursache:** Wenn die Fehlermeldung „NullPointerReference“ enthält, ist möglicherweise ein vorübergehender Fehler aufgetreten.

- **Empfehlung**:  Wiederholen Sie den Vorgang. Wenn das Problem weiterhin besteht, wenden Sie sich an den Support.


### <a name="error-code--parquetinvalidfile"></a>Fehlercode:  ParquetInvalidFile

- **Meldung**: `File is not a valid Parquet file.`

- **Ursache:** Problem bei der Parquet-Datei.

- **Empfehlung**:  Überprüfen Sie, ob die Eingabe eine gültige Parquet-Datei ist.


### <a name="error-code--parquetnotsupportedtype"></a>Fehlercode:  ParquetNotSupportedType

- **Meldung**: `Unsupported Parquet type. PrimitiveType: %primitiveType; OriginalType: %originalType;.`

- **Ursache:** Das Parquet-Format wird in Azure Data Factory nicht unterstützt.

- **Empfehlung**:  Überprüfen Sie die Quelldaten. Weitere Informationen finden Sie in diesem Dokument: https://docs.microsoft.com/azure/data-factory/supported-file-formats-and-compression-codecs.


### <a name="error-code--parquetmisseddecimalprecisionscale"></a>Fehlercode:  ParquetMissedDecimalPrecisionScale

- **Meldung**: `Decimal Precision or Scale information is not found in schema for column: %column;.`

- **Ursache:** Sie versuchen, die Zahlengenauigkeit und Dezimalstellen zu analysieren, doch diese Informationen werden nicht geliefert.

- **Empfehlung**:  „Quelle“ gibt nicht die richtige „Genauigkeit“ und das richtige „Dezimalzeichen“ zurück. Überprüfen Sie die Genauigkeit und das Dezimalzeichen in der Problemspalte.


### <a name="error-code--parquetinvaliddecimalprecisionscale"></a>Fehlercode:  ParquetInvalidDecimalPrecisionScale

- **Meldung**: `Invalid Decimal Precision or Scale. Precision: %precision; Scale: %scale;.`

- **Ursache:** Das Schema ist ungültig.

- **Empfehlung**:  Überprüfen Sie die Genauigkeit und das Dezimalzeichen in der Problemspalte.


### <a name="error-code--parquetcolumnnotfound"></a>Fehlercode:  ParquetColumnNotFound

- **Meldung**: `Column %column; does not exist in Parquet file.`

- **Ursache:** Das Quellschema stimmt mit dem Senkenschema nicht überein.

- **Empfehlung**:  Überprüfen Sie in „activity“ (Aktivität) den Wert für „mappings“ (Zuordnungen). Stellen Sie sicher, dass die Quellspalte der rechten Senkenspalte zugeordnet werden kann.


### <a name="error-code--parquetinvaliddataformat"></a>Fehlercode:  ParquetInvalidDataFormat

- **Meldung**: `Incorrect format of %srcValue; for converting to %dstType;.`

- **Ursache:** Die Daten können nicht in den in „mappings.source“ angegebenen Typ konvertiert werden.

- **Empfehlung**:  Überprüfen Sie die Quelldaten, oder geben Sie den richtigen Datentyp für diese Spalte in der Spaltenzuordnung der Kopieraktivität an. Weitere Informationen finden Sie in diesem Dokument: https://docs.microsoft.com/azure/data-factory/supported-file-formats-and-compression-codecs.


### <a name="error-code--parquetdatacountnotmatchcolumncount"></a>Fehlercode:  ParquetDataCountNotMatchColumnCount

- **Meldung**: `The data count in a row '%sourceColumnCount;' does not match the column count '%sinkColumnCount;' in given schema.`

- **Ursache:** Die Anzahl der Quellspalten und die Anzahl der Senkenspalten stimmen nicht überein.

- **Empfehlung**:  Überprüfen Sie, ob die Anzahl der Quellspalten mit der Anzahl der Senkenspalten in „Mapping“ übereinstimmt.


### <a name="error-code--parquetdatatypenotmatchcolumntype"></a>Fehlercode:  ParquetDataTypeNotMatchColumnType

- **Meldung**: Der Datentyp „%srcType;“ entspricht nicht dem angegebenen Spaltentyp „%dstType;“ in Spalte „%columnIndex;“.

- **Ursache:** Daten aus der Quelle können nicht in den Typ konvertiert werden, der in der Senke definiert wurde.

- **Empfehlung**:  Geben Sie in „mapping.sink“ einen richtigen Typ an.


### <a name="error-code--parquetbridgeinvaliddata"></a>Fehlercode:  ParquetBridgeInvalidData

- **Meldung**: `%message;`

- **Ursache:** Datenwert über Begrenzung

- **Empfehlung**:  Wiederholen Sie den Vorgang. Wenn das Problem weiterhin auftritt, wenden Sie sich an uns.


### <a name="error-code--parquetunsupportedinterpretation"></a>Fehlercode:  ParquetUnsupportedInterpretation

- **Meldung**: `The given interpretation '%interpretation;' of Parquet format is not supported.`

- **Ursache:** Nicht unterstütztes Szenario

- **Empfehlung**:  „ParquetInterpretFor“ sollte nicht „sparkSql“ sein.


### <a name="error-code--parquetunsupportfilelevelcompressionoption"></a>Fehlercode:  ParquetUnsupportFileLevelCompressionOption

- **Meldung**: `File level compression is not supported for Parquet.`

- **Ursache:** Nicht unterstütztes Szenario

- **Empfehlung**:  Entfernen Sie „CompressionType“ in der Nutzlast.


### <a name="error-code--usererrorjniexception"></a>Fehlercode:  UserErrorJniException

- **Meldung**: `Cannot create JVM: JNI return code [-6][JNI call failed: Invalid arguments.]`

- **Ursache:** JVM kann nicht erstellt werden, da einige ungültige (globale) Argumente festgelegt wurden.

- **Empfehlung**:  Melden Sie sich bei dem Computer an, auf dem die **einzelnen Knoten** Ihrer selbstgehosteten IR gehostet werden. Überprüfen Sie, ob die Systemvariable ordnungsgemäß wie folgt festgelegt ist: `_JAVA_OPTIONS "-Xms256m -Xmx16g" with memory bigger than 8 G`. Starten Sie alle IR-Knoten neu, und führen Sie dann die Pipeline erneut aus.


### <a name="arithmetic-overflow"></a>Arithmetischer Überlauf

- **Symptome:** Beim Kopieren von Parquet-Dateien ist eine Fehlermeldung aufgetreten: `Message = Arithmetic Overflow., Source = Microsoft.DataTransfer.Common`

- **Ursache:** Derzeit werden beim Kopieren von Dateien aus Oracle in das Parquet-Format nur Dezimalzahlen mit einer Genauigkeit <= 38 unterstützt, deren ganzzahliger Teil eine Länge <= 20 aufweist. 

- **Lösung:** Zur Umgehung des Problems können Sie Spalten mit einem solchen Problem in VARCHAR2 konvertieren.


### <a name="no-enum-constant"></a>Keine Enumerationskonstante

- **Symptome:** Beim Kopieren von Daten in das Parquet-Format ist eine Fehlermeldung aufgetreten: `java.lang.IllegalArgumentException:field ended by &apos;;&apos;` oder `java.lang.IllegalArgumentException:No enum constant org.apache.parquet.schema.OriginalType.test`.

- **Ursache**: 

    Das Problem kann durch Leerzeichen oder nicht unterstützte Zeichen (z. B. ,;{}()\n\t=) im Spaltennamen verursacht werden, da dieses Format von Parquet nicht unterstützt wird. 

    Beispielsweise wird bei einem Spaltennamen wie *contoso(test)* der Typ in Klammern als [Code](https://github.com/apache/parquet-mr/blob/master/parquet-column/src/main/java/org/apache/parquet/schema/MessageTypeParser.java) `Tokenizer st = new Tokenizer(schemaString, " ;{}()\n\t");` analysiert. Der Fehler wird ausgelöst, da kein Typ „test“ vorhanden ist.

    Sie können die unterstützten Typen [hier](https://github.com/apache/parquet-mr/blob/master/parquet-column/src/main/java/org/apache/parquet/schema/OriginalType.java) überprüfen.

- **Lösung:** 

    - Überprüfen Sie, ob im Spaltennamen der Senke Leerzeichen enthalten sind.

    - Überprüfen Sie, ob als Spaltenname die erste Spalte mit Leerzeichen verwendet wird.

    - Überprüfen Sie, ob der Typ „OriginalType“ unterstützt wird. Vermeiden Sie diese Sonderzeichen: `,;{}()\n\t=`. 


## <a name="rest"></a>REST

### <a name="error-code--restsinkcallfailed"></a>Fehlercode:  RestSinkCallFailed

- **Meldung**: `Rest Endpoint responded with Failure from server. Check the error from server:%message;`

- **Ursache:** Dieser Fehler tritt auf, wenn Azure Data Factory mit dem REST-Endpunkt über das HTTP-Protokoll kommuniziert und beim Anforderungsvorgang ein Fehler auftritt.

- **Empfehlung**:  Überprüfen Sie den HTTP-Statuscode bzw. die HTTP-Statusnachricht in der Fehlermeldung, und beheben Sie das Remoteserverproblem.

### <a name="unexpected-network-response-from-rest-connector"></a>Unerwartete Netzwerkantwort vom REST-Connector

- **Symptome:** Es kann vorkommen, dass der Endpunkt eine unerwartete Antwort (400/401/403/500) vom REST-Connector erhält.

- **Ursache:** Für den REST-Quellenconnector werden beim Erstellen einer HTTP-Anforderung die URL und die HTTP-Methode (mit Header und Text) des verknüpften Diensts, des Datasets oder der Kopierquelle als Parameter verwendet. Der wahrscheinlichste Grund für das Problem sind einige Fehler in mindestens einem der angegebenen Parameter.

- **Lösung:** 
    - Verwenden Sie „curl“ im Befehlsfenster, um zu überprüfen, ob der Parameter die Ursache ist (die Header **Accept** und **User-Agent** sollten immer vorhanden sein):
        ```
        curl -i -X <HTTP method> -H <HTTP header1> -H <HTTP header2> -H "Accept: application/json" -H "User-Agent: azure-data-factory/2.0" -d '<HTTP body>' <URL>
        ```
      Wenn für diesen Befehl die gleiche unerwartete Antwort zurückgegeben wird, sollten Sie die obigen Parameter mit „curl“ korrigieren, bis die erwartete Antwort zurückgegeben wird. 

      Sie können auch „curl--help“ verwenden, um Informationen zu den erweiterten Optionen des Befehls anzuzeigen.

    - Falls nur vom ADF-REST-Connector unerwartete Antworten zurückgegeben werden, sollten Sie sich für die weitere Problembehandlung an den Microsoft-Support wenden.
    
    - Beachten Sie hierbei, dass „curl“ ggf. nicht geeignet ist, um ein Problem bei der Überprüfung des SSL-Zertifikats zu reproduzieren. In einigen Szenarien war die Ausführung des Befehls „curl“ erfolgreich, ohne dass ein Problem bei der Überprüfung des SSL-Zertifikats aufgetreten ist. Wenn dieselbe URL dann im Browser ausgeführt wird, wird für den Client aber kein SSL-Zertifikat zurückgegeben, mit dem eine Vertrauensstellung mit dem Server erzielt werden kann.

      Für den obigen Fall empfehlen wir die Verwendung von Tools wie **Postman** und **Fiddler**.


## <a name="sftp"></a>SFTP

#### <a name="error-code--sftpoperationfail"></a>Fehlercode:  SftpOperationFail

- **Meldung**: `Failed to '%operation;'. Check detailed error from SFTP.`

- **Ursache:** Beim SFTP-Vorgang ist ein Problem aufgetreten.

- **Empfehlung**:  Überprüfen Sie den detaillierten Fehler von SFTP.


### <a name="error-code--sftprenameoperationfail"></a>Fehlercode:  SftpRenameOperationFail

- **Meldung**: `Failed to rename the temp file. Your SFTP server doesn't support renaming temp file, please set "useTempFileRename" as false in copy sink to disable uploading to temp file.`

- **Ursache:** Der SFTP-Server unterstützt kein Umbenennen einer temporären Datei.

- **Empfehlung**:  Legen Sie „useTempFileRename“ in der Kopiersenke auf „false“ fest, um das Hochladen in eine temporäre Datei zu deaktivieren.


### <a name="error-code--sftpinvalidsftpcredential"></a>Fehlercode:  SftpInvalidSftpCredential

- **Meldung**: `Invalid Sftp credential provided for '%type;' authentication type.`

- **Ursache:** Der Inhalt des privaten Schlüssels wird aus AKV/SDK abgerufen, aber nicht ordnungsgemäß codiert.

- **Empfehlung**:  

    Der Inhalt des privaten Schlüssels stammt aus AKV, und die ursprüngliche Schlüsseldatei funktioniert, wenn der Kunde sie direkt in den verknüpften SFTP-Dienst hochlädt:

    Unter https://docs.microsoft.com/azure/data-factory/connector-sftp#using-ssh-public-key-authentication ist beschrieben, dass der Inhalt von privateKey ein Base64-codierter privater SSH-Schlüssel ist.

    Codieren Sie **den gesamten Inhalt der ursprünglichen Datei mit dem privaten Schlüssel** mit Base64, und speichern Sie die codierte Zeichenfolge in AKV. Die ursprüngliche Datei mit dem privaten Schlüssel funktioniert bei einem verknüpften SFTP-Dienst, wenn Sie auf „Aus Datei hochladen“ klicken.

    Im Folgenden finden Sie einige Beispiele für das Generieren der Zeichenfolge:

    - In C#:
    ```
    byte[] keyContentBytes = File.ReadAllBytes(Private Key Path);
    string keyContent = Convert.ToBase64String(keyContentBytes, Base64FormattingOptions.None);
    ```

    - In Python:
    ```
    import base64
    rfd = open(r'{Private Key Path}', 'rb')
    keyContent = rfd.read()
    rfd.close()
    print base64.b64encode(Key Content)
    ```

    - Mit einem Base64-Konvertierungstools von Drittanbietern:

        Es werden Tools wie https://www.base64encode.org/ empfohlen.

- **Ursache:** Es wurde das falsche Format für den Schlüsselinhalt ausgewählt.

- **Empfehlung**:  

    Private SSH-Schlüssel im PKCS#8-Format (beginnt mit „-----BEGIN ENCRYPTED PRIVATE KEY-----“) werden derzeit nicht für den Zugriff auf SFTP-Server in ADF unterstützt. 

    Führen Sie die folgenden Befehle aus, um den Schlüssel in ein herkömmliches SSH-Schlüsselformat zu konvertieren (beginnt mit „-----BEGIN RSA PRIVATE KEY-----“):

    ```
    openssl pkcs8 -in pkcs8_format_key_file -out traditional_format_key_file
    chmod 600 traditional_format_key_file
    ssh-keygen -f traditional_format_key_file -p
    ```

- **Ursache:** Anmeldeinformationen oder Inhalt des privaten Schlüssels ungültig.

- **Empfehlung**:  Überprüfen Sie mit Tools wie WinSCP, ob die Schlüsseldatei und das Kennwort richtig sind.

### <a name="sftp-copy-activity-failed"></a>Fehler bei der Copy-Aktivität bei SFTP

- **Symptome:** Fehlercode: UserErrorInvalidColumnMappingColumnNotFound. Fehlermeldung: `Column &apos;AccMngr&apos; specified in column mapping cannot be found in source data.`

- **Ursache:** Die Quelle enthält keine Spalte mit dem Namen „AccMngr“.

- **Lösung:** Überprüfen Sie, ob Ihr Dataset so konfiguriert wurde, dass die für die Bestätigung im Zieldataset zuordnete Spalte „AccMngr“ lautet.


### <a name="error-code--sftpfailedtoconnecttosftpserver"></a>Fehlercode:  SftpFailedToConnectToSftpServer

- **Meldung**: `Failed to connect to Sftp server '%server;'.`

- **Ursache:** Wenn die Fehlermeldung „Socket read operation has timed out after 30000 milliseconds“ enthält, ist eine mögliche Ursache, dass ein falscher Typ von verknüpftem Dienst für den SFTP-Server verwendet wird. Beispielsweise wird mit einem verknüpften FTP-Dienst versucht, eine Verbindung mit einem SFTP-Server herzustellen.

- **Empfehlung**:  Überprüfen Sie den Port des Zielservers. Standardmäßig verwendet SFTP den Port 22.

- **Ursache:** Wenn die Fehlermeldung „Server response does not contain SSH protocol identification“ enthält, ist eine mögliche Ursache, dass der SFTP-Server die Verbindung gedrosselt hat. ADF erstellt mehrere Verbindungen, um parallele Downloads vom SFTP-Server durchführen zu können. Manchmal wird dabei die SFTP-Serverdrosselung ausgelöst. In der Praxis geben unterschiedliche Server bei einer Drosselung andere Fehler zurück.

- **Empfehlung**:  

    Geben Sie als maximale Anzahl gleichzeitiger Verbindungen für das SFTP-Datasets „1“ an, und führen Sie den Kopiervorgang erneut aus. Bei einem Erfolg können Sie sicher sein, dass die Drosselung die Ursache ist.

    Wenn Sie den niedrigen Durchsatz steigern möchten, wenden Sie sich an den SFTP-Administrator, damit dieser die Anzahl gleichzeitiger Verbindungen erhöht, oder fügen Sie der Positivliste folgende IP-Adresse hinzu:

    - Wenn Sie eine verwaltete IR verwenden, fügen Sie die [IP-Adressbereiche der Azure-Rechenzentren](https://www.microsoft.com/download/details.aspx?id=41653) hinzu.
      Sie können auch eine selbstgehostete IR installieren, wenn Sie der Positivliste auf dem SFTP-Server keine umfangreiche Liste von IP-Adressbereichen hinzufügen möchten.

    - Wenn Sie eine selbstgehostete IR verwenden, fügen Sie der Positivliste die IP-Adresse des Computers hinzu, auf dem die selbstgehostete IR installiert ist.


## <a name="sharepoint-online-list"></a>SharePoint Online-Liste

### <a name="error-code--sharepointonlineauthfailed"></a>Fehlercode:  SharePointOnlineAuthFailed

- **Meldung**: `The access token generated failed, status code: %code;, error message: %message;.`

- **Ursache:** Die Dienstprinzipal-ID und der Schlüssel sind möglicherweise nicht richtig festgelegt.

- **Empfehlung**:  Überprüfen Sie, ob die registrierte Anwendung (Dienstprinzipal-ID) und der Schlüssel richtig festgelegt sind.


## <a name="xml-format"></a>XML-Format

### <a name="error-code--xmlsinknotsupported"></a>Fehlercode:  XmlSinkNotSupported

- **Meldung**: `Write data in xml format is not supported yet, please choose a different format!`

- **Ursache:** Es wurde ein XML-Datasets als Senkendataset in der Kopieraktivität verwendet.

- **Empfehlung**:  Verwenden Sie ein Dataset in einem anderen Format als die Kopiersenke.


### <a name="error-code--xmlattributecolumnnameconflict"></a>Fehlercode:  XmlAttributeColumnNameConflict

- **Meldung**: `Column names %attrNames;' for attributes of element '%element;' conflict with that for corresponding child elements, and the attribute prefix used is '%prefix;'.`

- **Ursache:** Es wurde ein Attributpräfix verwendet, das den Konflikt verursacht hat.

- **Empfehlung**:  Legen Sie einen anderen Wert für die Eigenschaft „attributePrefix“ fest.


### <a name="error-code--xmlvaluecolumnnameconflict"></a>Fehlercode:  XmlValueColumnNameConflict

- **Meldung**: `Column name for the value of element '%element;' is '%columnName;' and it conflicts with the child element having the same name.`

- **Ursache:** Es wurde einer der untergeordneten Elementnamen als Spaltennamen für den Elementwert verwendet.

- **Empfehlung**:  Legen Sie einen anderen Wert für die Eigenschaft „valueColumn“ fest.


### <a name="error-code--xmlinvalid"></a>Fehlercode:  XmlInvalid

- **Meldung**: `Input XML file '%file;' is invalid with parsing error '%error;'.`

- **Ursache:** Die XML-Eingabedatei ist nicht richtig formatiert.

- **Empfehlung**:  Korrigieren Sie die XML-Datei, sodass sie richtig formatiert ist.


## <a name="general-copy-activity-error"></a>Fehler bei allgemeiner Kopieraktivität

### <a name="error-code--jrenotfound"></a>Fehlercode:  JreNotFound

- **Meldung**: `Java Runtime Environment cannot be found on the Self-hosted Integration Runtime machine. It is required for parsing or writing to Parquet/ORC files. Make sure Java Runtime Environment has been installed on the Self-hosted Integration Runtime machine.`

- **Ursache:** Die selbstgehostete Integration Runtime kann Java Runtime nicht finden. Java Runtime ist zum Lesen einer bestimmten Quelle erforderlich.

- **Empfehlung**:  Überprüfen Sie Ihre Integration Runtime-Umgebung, dieses Referenzdokument: https://docs.microsoft.com/azure/data-factory/format-parquet#using-self-hosted-integration-runtime


### <a name="error-code--wildcardpathsinknotsupported"></a>Fehlercode:  WildcardPathSinkNotSupported

- **Meldung**: `Wildcard in path is not supported in sink dataset. Fix the path: '%setting;'.`

- **Ursache:** Das Senkendataset unterstützt keine Platzhalter.

- **Empfehlung**:  Überprüfen Sie das Senkendataset, und korrigieren Sie den Pfad ohne den Platzhalterwert.


### <a name="fips-issue"></a>FIPS-Problem

- **Symptome:** Bei der Kopieraktivität auf einem Computer mit der selbstgehosteten Integration Runtime und aktiviertem FIPS tritt die Fehlermeldung `This implementation is not part of the Windows Platform FIPS validated cryptographic algorithms.` auf. Dies kann beim Kopieren von Daten mit Connectors wie Azure Blob, SFTP usw. auftreten.

- **Ursache:** FIPS (Federal Information Processing Standards) definiert eine bestimmte Gruppe von Kryptografiealgorithmen, die verwendet werden können. Wenn der FIPS-Modus auf dem Computer aktiviert ist, werden einige Kryptografieklassen, von denen die Kopieraktivität abhängig ist, in einigen Szenarien blockiert.

- **Lösung:** Informationen zum aktuellen Stand des FIPS-Modus unter Windows finden Sie in [diesem Artikel](https://techcommunity.microsoft.com/t5/microsoft-security-baselines/why-we-8217-re-not-recommending-8220-fips-mode-8221-anymore/ba-p/701037). Prüfen Sie außerdem, ob Sie FIPS auf dem Computer mit der selbstgehosteten Integration Runtime deaktivieren können.

    Wenn Sie jedoch nur Azure Data Factory die Umgehung von FIPS und erfolgreiche Aktivitätsausführungen ermöglichen möchten, können Sie die folgenden Schritte ausführen:

    1. Öffnen Sie den Ordner, in dem die selbstgehostete Integration Runtime installiert ist (normalerweise unter `C:\Program Files\Microsoft Integration Runtime\<IR version>\Shared`).

    2. Öffnen Sie „diawp.exe.config“, und fügen Sie dem Abschnitt `<runtime>` wie folgt `<enforceFIPSPolicy enabled="false"/>` hinzu.

        ![Deaktivieren von FIPS](./media/connector-troubleshoot-guide/disable-fips-policy.png)

    3. Starten Sie den Computer mit der selbstgehosteten Integration Runtime neu.


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Problembehandlung finden Sie in diesen Ressourcen:

*  [Data Factory-Blog](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Data Factory-Funktionsanfragen](https://feedback.azure.com/forums/270578-data-factory)
*  [Azure-Videos](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [Frageseite von Microsoft Q&A (Fragen und Antworten)](/answers/topics/azure-data-factory.html)
*  [Stack Overflow-Forum für Data Factory](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Twitter-Informationen über Data Factory](https://twitter.com/hashtag/DataFactory)
