---
title: Problembehandlung bei Zuordnungsdatenflüssen
description: Hier erfahren Sie, wie Sie in Azure Data Factory Datenflussprobleme beheben können.
ms.author: makromer
author: kromerm
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: troubleshooting
ms.date: 03/25/2021
ms.openlocfilehash: 72ab685b58f7d940fe4d682cacba6212fe80ced8
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105933082"
---
# <a name="troubleshoot-mapping-data-flows-in-azure-data-factory"></a>Problembehandlung bei Zuordnungsdatenflüssen in Azure Data Factory

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

In diesem Artikel werden die gängigen Problembehandlungsmethoden für Zuordnungsdatenflüsse in Azure Data Factory beschrieben.

## <a name="common-error-codes-and-messages"></a>Allgemeine Fehlercodes und Meldungen 

### <a name="error-code-df-executor-sourceinvalidpayload"></a>Fehlercode: DF-Executor-SourceInvalidPayload
- **Meldung**: Data preview, debug, and pipeline data flow execution failed because container does not exist (Fehler beim Anzeigen von Daten in der Vorschau, beim Debuggen und bei der Datenflussausführung in der Pipeline, weil der Container nicht vorhanden ist)
- **Ursache**: Ein Dataset enthält einen Container, der im Speicher nicht vorhanden ist.
- **Empfehlung**: Stellen Sie sicher, dass der Container, auf den Sie in Ihrem Dataset verwiesen haben, vorhanden ist und darauf zugegriffen werden kann.

### <a name="error-code-df-executor-systeminvalidjson"></a>Fehlercode: DF-Executor-SystemInvalidJson

- **Meldung**: JSON parsing error, unsupported encoding or multiline (JSON-Analysefehler: nicht unterstützte Codierung oder mehrzeiliger Text)
- **Ursache**: Mögliche Probleme bei der JSON-Datei: nicht unterstützte Codierung, beschädigte Bytes oder Verwendung der JSON-Quelle als einzelnes Dokument in vielen geschachtelten Zeilen.
- **Empfehlung**: Überprüfen Sie, ob die Codierung der JSON-Datei unterstützt wird. Erweitern Sie in der Quelltransformation, die ein JSON-Dataset verwendet, die Option **JSON-Einstellungen**, und aktivieren Sie **Einzelnes Dokument**.
 
### <a name="error-code-df-executor-broadcasttimeout"></a>Fehlercode: DF-Executor-BroadcastTimeout

- **Meldung**: Broadcast join timeout error, make sure broadcast stream produces data within 60 secs in debug runs and 300 secs in job runs (Übertragungsjoin-Timeoutfehler: Stellen Sie sicher, dass der Übertragungsdatenstrom in Debugausführungen Daten innerhalb von 60 Sekunden und in Auftragsausführungen innerhalb von 300 Sekunden Daten erzeugt.)
- **Ursache**: Für Übertragungen gilt bei Debugausführungen ein Standardtimeout von 60 Sekunden und bei Auftragsausführungen ein Standardtimeout von 300 Sekunden. Der für die Übertragung ausgewählte Datenstrom ist zu groß, um innerhalb dieser Zeitspanne Daten zu erzeugen.
- **Empfehlung**: Überprüfen Sie die Registerkarte **Optimieren** bei Ihren Datenflusstransformationen auf „join“, „exists“ und „lookup“. Die Standardoption für die Übertragung ist **Auto**. Wenn **Auto** festgelegt ist oder Sie die linke oder rechte Seite unter **Fixed** für die Übertragung manuell festlegen, können Sie entweder eine größere Azure Integration Runtime (IR)-Konfiguration festlegen oder die Übertragung deaktivieren. Für die optimale Leistung bei Datenflüssen empfehlen wir, Spark zu erlauben, mithilfe von **Auto** zu übertragen und eine arbeitsspeicheroptimierte Azure IR zu verwenden. 
 
  Wenn Sie den Datenfluss in einer Debugtestausführung über eine Debugpipelineausführung ausführen, könnte diese Bedingung häufiger auftreten. Der Grund: Azure Data Factory drosselt das Übertragungstimeout auf 60 Sekunden, um ein schnelleres Debuggen zu ermöglichen. Sie können das Timeout auf das 300-Sekunden-Timeout einer ausgelösten Ausführung verlängern. Zu diesem Zweck können Sie die Option **Debug** > **Aktivitätsruntime verwenden** auswählen, um die in Ihrer Aktivität „Datenflusspipeline ausführen“ definierte Azure IR zu verwenden.

- **Meldung:** Broadcastjoin-Timeoutfehler: Sie können die Option „Broadcast“ in der join-/exists-/lookup-Transformation auf „Aus“ festlegen, um dieses Problem zu vermeiden. Wenn Sie die Broadcastjoinoption nutzen möchten, um die Leistung zu verbessern, stellen Sie sicher, dass der Broadcast-Datenstrom Daten in Debugausführungen innerhalb von 60 Sekunden und in Auftragsausführungen von 300 Sekunden erstellen kann.
- **Ursache**: Für Übertragungen gilt bei Debugausführungen ein Standardtimeout von 60 Sekunden und bei Auftragsausführungen ein Standardtimeout von 300 Sekunden. Der bei einem Broadcastjoin für die Übertragung ausgewählte Datenstrom ist zu groß, um innerhalb dieser Zeitspanne Daten zu erzeugen. Wenn kein Broadcastjoin verwendet wird, kann der Standardbroadcast durch den Datenfluss denselben Grenzwert erreichen.
- **Empfehlung**: Deaktivieren Sie die Broadcastoption, oder vermeiden Sie die Übertragung großer Datenströme, deren Verarbeitung länger als 60 Sekunden dauern kann. Wählen Sie einen kleineren Datenstrom für die Übertragung. Große Azure SQL Data Warehouse-Tabellen und -Quelldateien sind normalerweise keine gute Wahl. Sollte kein Broadcastjoin vorhanden sein, verwenden Sie einen größeren Cluster, wenn dieser Fehler auftritt.

### <a name="error-code-df-executor-conversion"></a>Fehlercode: DF-Executor-Conversion

- **Meldung**: Converting to a date or time failed due to an invalid character (Fehler beim Umwandeln eines Datums oder einer Uhrzeit aufgrund eines ungültigen Zeichens)
- **Ursache**: Die Daten haben nicht das erwartete Format.
- **Empfehlung**: Verwenden Sie den richtigen Datentyp.

### <a name="error-code-df-executor-invalidcolumn"></a>Fehlercode: DF-Executor-InvalidColumn
- **Meldung**: Column name needs to be specified in the query, set an alias if using a SQL function (Spaltenname muss in der Abfrage angegeben werden. Legen Sie einen Alias fest, wenn Sie eine SQL-Funktion verwenden.)
- **Ursache**: Es wurde kein Spaltenname angegeben.
- **Empfehlung**: Legen Sie einen Alias fest, wenn Sie eine SQL-Funktion wie „min()“ oder „max()“ verwenden.

### <a name="error-code-df-executor-drivererror"></a>Fehlercode: DF-Executor-DriverError
- **Meldung**: INT96 ist ein älterer Zeitstempeltyp, der vom ADF-Datenfluss nicht unterstützt wird. Sie sollten den Spaltentyp auf die aktuellen Typen aktualisieren.
- **Ursache**: Treiberfehler.
- **Empfehlung**: INT96 ist ein Legacy-Zeitstempeltyp, der von Azure Data Factory-Datenfluss nicht unterstützt wird. Sie sollten den Spaltentyp auf den neuesten Typ aktualisieren.

### <a name="error-code-df-executor-blockcountexceedslimiterror"></a>Fehlercode: DF-Executor-BlockCountExceedsLimitError
- **Meldung**: Die Anzahl der Blöcke ohne Commit darf den maximalen Grenzwert von 100.000 Blöcken nicht überschreiten. Überprüfen Sie die Blobkonfiguration.
- **Ursache**: Die maximale Anzahl von Blöcken ohne Commit in einem Blob ist 100.000.
- **Empfehlung**: Weitere Informationen zu diesem Problem erhalten Sie vom Microsoft-Produktteam.

### <a name="error-code-df-executor-partitiondirectoryerror"></a>Fehlercode: DF-Executor-PartitionDirectoryError
- **Meldung**: Der angegebene Quellpfad weist entweder mehrere partitionierte Verzeichnisse (z. B. <Source Path>/<Partitionsstammverzeichnis 1>/a=10/b=20, <Source Path>/<Partitionsstammverzeichnis 2>/c=10/d=30) oder ein partitioniertes Verzeichnis mit einer anderen Datei oder einem nicht partitionierten Verzeichnis auf (z. B. <Source Path>/<Partitionsstammverzeichnis 1>/a=10/b=20, <Source Path>/Verzeichnis 2/Datei1). Entfernen Sie das Partitionsstammverzeichnis aus dem Quellpfad, und lesen Sie es durch eine separate Quelltransformation.
- **Ursache**: Der Quellpfad enthält entweder mehrere partitionierte Verzeichnisse oder ein partitioniertes Verzeichnis mit einer anderen Datei oder einem nicht partitionierten Verzeichnis.
- **Empfehlung**: Entfernen Sie das partitionierte Stammverzeichnis aus dem Quellpfad, und lesen Sie es durch eine separate Quelltransformation.

### <a name="error-code-df-executor-invalidtype"></a>Fehlercode: DF-Executor-InvalidType
- **Meldung**: Stellen Sie sicher, dass der Parametertyp mit dem Typ des übergebenen Werts übereinstimmt. Das Übergeben von float-Parametern aus Pipelines wird derzeit nicht unterstützt.
- **Ursache**: Der Datentyp für den deklarierten Typ ist mit dem tatsächlichen Parameterwert nicht kompatibel.
- **Empfehlung**: Vergewissern Sie sich, dass die an den Datenfluss übergebenen Parameterwerte dem deklarierten Typ entsprechen.

### <a name="error-code-df-executor-parseerror"></a>Fehlercode: DF-Executor-ParseError
- **Meldung**: Der Ausdruck kann nicht analysiert werden.
- **Ursache**: Ein Ausdruck hat aufgrund von falscher Formatierung Analysefehler generiert.
- **Empfehlung**: Überprüfen Sie die Formatierung im Ausdruck.

### <a name="error-code-df-executor-systemimplicitcartesian"></a>Fehlercode: DF-Executor-SystemImplicitCartesian
- **Meldung**: Implicit cartesian product for INNER join is not supported, use CROSS JOIN instead. (Implizites kartesisches Produkt wird für INNER JOIN nicht unterstützt. Verwenden Sie stattdessen CROSS JOIN.) Die im Join verwendeten Spalten müssen einen eindeutigen Schlüssel für Zeilen erstellen.
- **Ursache**: Implizite kartesische Produkte für INNER JOINs zwischen logischen Plänen werden nicht unterstützt. Wenn Sie Spalten im Join verwenden, erstellen Sie einen eindeutigen Schlüssel.
- **Empfehlung**: Verwenden Sie bei nicht gleichheitsbasierten Joins CROSS JOIN.

### <a name="error-code-getcommand-outputasync-failed"></a>Fehlercode: GetCommand OutputAsync failed (Fehler bei GetCommand OutputAsync)
- **Meldung**: During Data Flow debug and data preview: GetCommand OutputAsync failed (Beim Debuggen des Datenflusses und Anzeigen von Daten in der Vorschau: Fehler bei GetCommand OutputAsync mit ...)
- **Ursache:** Dies ist ein Fehler beim Back-End-Dienst. 
- **Empfehlung**: Wiederholen Sie den Vorgang, und starten Sie Ihre Debugsitzung neu. Wenn das Problem durch Wiederholung und Neustart nicht behoben werden kann, wenden Sie sich an den Kundensupport. 

### <a name="error-code-df-executor-outofmemoryerror"></a>Fehlercode: DF-Executor-OutOfMemoryError
 
- **Meldung**: Nicht genügend Arbeitsspeicher während der Ausführung im Cluster verfügbar. Wiederholen Sie den Vorgang mit einer Integration Runtime mit einer größeren Anzahl von Kernen und/oder einem arbeitsspeicheroptimierten Computetyp.
- **Ursache**: Der Cluster verfügt nicht über genügend Arbeitsspeicher.
- **Empfehlung**: Debugcluster sind für die Entwicklung gedacht. Nutzen Sie die Datenstichprobenentnahme, den entsprechenden Computetyp und die richtige Größe zur Ausführung der Payload. Leistungstipps finden Sie in der [Mapping Data Flow Performance Guide](concepts-data-flow-performance.md) (Anleitung zur Leistung von Zuordnungsdatenflüssen).

### <a name="error-code-df-executor-illegalargument"></a>Fehlercode: DF-Executor-illegalArgument

- **Meldung**: Stellen Sie sicher, dass in Ihrem verknüpften Dienst der richtige Zugriffsschlüssel verwendet wird.
- **Ursache**: Der Kontoname oder Zugriffsschlüssel ist falsch.
- **Empfehlung**: Stellen Sie sicher, dass in Ihrem verknüpften Dienst der richtige Kontoname oder Zugriffsschlüssel angegeben ist. 

### <a name="error-code-df-executor-columnunavailable"></a>Fehlercode: DF-Executor-ColumnUnavailable
- **Meldung:** Der im Ausdruck verwendete Spaltenname ist nicht verfügbar oder ungültig.
- **Ursache**: In einem Ausdruck wird ein ungültiger oder nicht verfügbarer Spaltenname verwendet.
- **Empfehlung**: Überprüfen Sie die in Ausdrücken verwendeten Spaltennamen.

 ### <a name="error-code-df-executor-outofdiskspaceerror"></a>Fehlercode: DF-Executor-OutOfDiskSpaceError
- **Meldung:** Interner Serverfehler
- **Ursache**: Der Cluster verfügt nicht über genügend Speicherplatz.
- **Empfehlung**: Führen Sie die Pipeline erneut aus. Wenn das Problem dadurch nicht behoben wird, wenden Sie sich an den Kundensupport.


 ### <a name="error-code-df-executor-storeisnotdefined"></a>Fehlercode: DF-Executor-StoreIsNotDefined
- **Meldung:** Die Speicherkonfiguration ist nicht definiert. Dieser Fehler wird möglicherweise durch eine ungültige Parameterzuweisung in der Pipeline verursacht.
- **Ursache**: Unbestimmt.
- **Empfehlung**: Überprüfen Sie die Zuweisung von Parameterwerten in der Pipeline. Ein Parameterausdruck enthält möglicherweise ungültige Zeichen.


### <a name="error-code-4502"></a>Fehlercode: 4502
- **Meldung:** There are substantial concurrent MappingDataflow executions that are causing failures due to throttling under Integration Runtime. (Es gibt viele gleichzeitige MappingDataflow-Ausführungen, die aufgrund von Drosselung unter Integration Runtime zu Fehlern führen).
- **Ursache**: Auf der Integration Runtime wird eine große Anzahl von Datenflussaktivitäten gleichzeitig ausgeführt. Weitere Informationen finden Sie unter [Data Factory-Grenzwerte](../azure-resource-manager/management/azure-subscription-service-limits.md#data-factory-limits).
- **Empfehlung**: Wenn Sie weitere Datenflussaktivitäten parallel ausführen möchten, verteilen Sie sie auf mehrere Integration Runtime-Instanzen.


### <a name="error-code-invalidtemplate"></a>Fehlercode: InvalidTemplate
- **Meldung:** The pipeline expression cannot be evaluated. (Der Pipelineausdruck kann nicht ausgewertet werden.)
- **Ursache**: Der in der Datenflussaktivität übergebene Pipelineausdruck wird aufgrund eines Syntaxfehlers nicht richtig verarbeitet.
- **Empfehlung**: Überprüfen Sie Ihre Aktivität in der Aktivitätsüberwachung zur Überprüfung des Ausdrucks.

### <a name="error-code-2011"></a>Fehlercode: 2011
- **Meldung:** The activity was running on Azure Integration Runtime and failed to decrypt the credential of data store or compute connected via a Self-hosted Integration Runtime. (Die Aktivität wurde in Azure Integration Runtime ausgeführt und konnte die Anmeldeinformationen für den Datenspeicher oder Compute, der/das über eine selbstgehostete Integration Runtime verbunden war, nicht entschlüsseln.) Bitte überprüfen Sie die Konfiguration der verknüpften Dienste, die dieser Aktivität zugeordnet sind, und stellen Sie sicher, dass Sie den richtigen Integration Runtime-Typ verwenden.
- **Ursache**: Der Datenfluss unterstützt keine verknüpften Dienste für selbstgehostete Integration Runtime-Instanzen.
- **Empfehlung**: Konfigurieren Sie den Datenfluss zur Ausführung in einer Integration Runtime mit „Verwaltetes virtuelles Netzwerk“.

### <a name="error-code-df-xml-invalidvalidationmode"></a>Fehlercode: DF-Xml-InvalidValidationMode
- **Meldung:** Es wurde ein ungültiger XML-Überprüfungsmodus angegeben.
- **Empfehlung:** Überprüfen Sie den Parameterwert, und geben Sie den richtigen Überprüfungsmodus an.

### <a name="error-code-df-xml-invaliddatafield"></a>Fehlercode: DF-Xml-InvalidDataField
- **Meldung:** Das Feld für beschädigte Datensätze muss den Typ „String“ haben und Nullwerte zulassen.
- **Empfehlung:** Stellen Sie sicher, dass die Spalte `\"_corrupt_record\"` im Quellprojekt einen Zeichenfolgendatentyp aufweist.

### <a name="error-code-df-xml-malformedfile"></a>Fehlercode: DF-Xml-MalformedFile
- **Meldung:** Falsch formatierter XML-Code in „FailFastMode“.
- **Empfehlung:** Aktualisieren Sie den Inhalt der XML-Datei in das richtige Format.

### <a name="error-code-df-xml-invaliddatatype"></a>Fehlercode: DF-Xml-InvalidDataType
- **Meldung:** Das XML-Element enthält Unterelemente oder Attribute und kann nicht konvertiert werden.

### <a name="error-code-df-xml-invalidreferenceresource"></a>Fehlercode: DF-Xml-InvalidReferenceResource
- **Meldung:** Die Verweisressource in der XML-Datendatei kann nicht aufgelöst werden.
- **Empfehlung:** Überprüfen Sie die Verweisressource in der XML-Datendatei.

### <a name="error-code-df-xml-invalidschema"></a>Fehlercode: DF-Xml-InvalidSchema
- **Meldung:** Fehler bei der Schemaüberprüfung.

### <a name="error-code-df-xml-unsupportedexternalreferenceresource"></a>Fehlercode: DF-Xml-UnsupportedExternalReferenceResource
- **Meldung:** Die externe Verweisressource in der XML-Datendatei wird nicht unterstützt.
- **Empfehlung:** Aktualisieren Sie den Inhalt der XML-Datei, wenn die externe Verweisressource aktuell nicht unterstützt wird.

### <a name="error-code-df-gen2-invalidaccountconfiguration"></a>Fehlercode: DF-GEN2-InvalidAccountConfiguration
- **Meldung:** Entweder der Kontoschlüssel oder tenant/spnId/spnCredential/spnCredentialType oder miServiceUri/miServiceToken muss angegeben werden.
- **Empfehlung:** Konfigurieren Sie das richtige Konto im zugehörigen verknüpften GEN2-Dienst.

### <a name="error-code-df-gen2-invalidauthconfiguration"></a>Fehlercode: DF-GEN2-InvalidAuthConfiguration
- **Meldung:** Es kann nur eine der drei Authentifizierungsmethoden (Key, ServicePrincipal und MI) angegeben werden. 
- **Empfehlung:** Wählen Sie den richtigen Authentifizierungstyp im zugehörigen verknüpften GEN2-Dienst aus.

### <a name="error-code-df-gen2-invalidserviceprincipalcredentialtype"></a>Fehlercode: DF-GEN2-InvalidServicePrincipalCredentialType
- **Meldung:** ServicePrincipalCredentialType ist ungültig.

### <a name="error-code-df-gen2-invaliddatatype"></a>Fehlercode: DF-GEN2-InvalidDataType
- **Meldung:** Cloudtyp ist ungültig.

### <a name="error-code-df-blob-invalidaccountconfiguration"></a>Fehlercode: DF-Blob-InvalidAccountConfiguration
- **Meldung:** Entweder der Kontoschlüssel oder sas_token muss angegeben werden.

### <a name="error-code-df-blob-invalidauthconfiguration"></a>Fehlercode: DF-Blob-InvalidAuthConfiguration
- **Meldung:** Es kann nur eine der beiden Authentifizierungsmethoden (Key, SAS) angegeben werden.

### <a name="error-code-df-blob-invaliddatatype"></a>Fehlercode: DF-Blob-InvalidDataType
- **Meldung:** Cloudtyp ist ungültig.

### <a name="error-code-df-cosmos-partitionkeymissed"></a>Fehlercode: DF-Cosmos-PartitionKeyMissed
- **Meldung:** Der Partitionsschlüsselpfad muss für Aktualisierungs- und Löschvorgänge angegeben werden.
- **Empfehlung:** Verwenden Sie den angegebenen Partitionsschlüssel in den Einstellungen der Cosmos-Senke.

### <a name="error-code-df-cosmos-invalidpartitionkey"></a>Fehlercode: DF-Cosmos-InvalidPartitionKey
- **Meldung:** Der Partitionsschlüsselpfad darf für Aktualisierungs- und Löschvorgänge nicht leer sein.
- **Empfehlung:** Verwenden Sie den angegebenen Partitionsschlüssel in den Einstellungen der Cosmos-Senke.

### <a name="error-code-df-cosmos-idpropertymissed"></a>Fehlercode: DF-Cosmos-IdPropertyMissed
- **Meldung:** Die id-Eigenschaft muss für Lösch- und Aktualisierungsvorgänge zugeordnet werden.
- **Empfehlung:** Stellen Sie sicher, dass die Eingabedaten eine `id`-Spalte in den Einstellungen der Cosmos-Senke enthalten. Wenn das nicht der Fall ist, verwenden Sie eine **Auswahl- oder Ableitungstransformation**, um diese Spalte vor der Senke zu generieren.

### <a name="error-code-df-cosmos-invalidpartitionkeycontent"></a>Fehlercode: DF-Cosmos-InvalidPartitionKeyContent
- **Meldung:** Der Partitionsschlüssel muss mit „/“ beginnen.
- **Empfehlung:** Legen Sie den Partitionsschlüssel in den Einstellungen der Cosmos-Senke so fest, dass er mit `/` beginnt, z. B. `/movieId`.

### <a name="error-code-df-cosmos-invalidpartitionkey"></a>Fehlercode: DF-Cosmos-InvalidPartitionKey
- **Meldung:** partitionKey ist in der Senke für Lösch- und Aktualisierungsvorgänge nicht zugeordnet.
- **Empfehlung:** Verwenden Sie in den Einstellungen der Cosmos-Senke den Partitionsschlüssel, der dem Ihres Containers entspricht.

### <a name="error-code-df-cosmos-invalidconnectionmode"></a>Fehlercode: DF-Cosmos-InvalidConnectionMode
- **Meldung:** Ungültiger connectionMode-Wert.
- **Empfehlung:** Vergewissern Sie sich, dass der unterstützte Modus in den Cosmos-Einstellungen **Gateway** und **DirectHttps** lautet.

### <a name="error-code-df-cosmos-invalidaccountconfiguration"></a>Fehlercode: DF-Cosmos-InvalidAccountConfiguration
- **Meldung:** Es muss entweder accountName oder accountEndpoint angegeben werden.

### <a name="error-code-df-github-writenotsupported"></a>Fehlercode: DF-Github-WriteNotSupported
- **Meldung:** Im GitHub-Speicher sind Schreibvorgänge nicht zulässig.

### <a name="error-code-df-pgsql-invalidcredential"></a>Fehlercode: DF-PGSQL-InvalidCredential
- **Meldung:** Benutzer/Kennwort muss angegeben werden.
- **Empfehlung:** Stellen Sie sicher, dass die richtigen Anmeldeinformationen im zugehörigen verknüpften postgresql-Dienst festgelegt sind.

### <a name="error-code-df-snowflake-invalidstageconfiguration"></a>Fehlercode: DF-Snowflake-InvalidStageConfiguration
- **Meldung:** Nur der Typ „Blob Storage“ kann als Stage in Lese-/Schreibvorgängen in Snowflake verwendet werden.

### <a name="error-code-df-snowflake-invalidstageconfiguration"></a>Fehlercode: DF-Snowflake-InvalidStageConfiguration
- **Meldung:** Die Snowflake-Phaseneigenschaften müssen mit der Azure Blob- und SAS-Authentifizierung angegeben werden.

### <a name="error-code-df-snowflake-invaliddatatype"></a>Fehlercode: DF-Snowflake-InvalidDataType
- **Meldung:** Der Spark-Typ wird in Snowflake nicht unterstützt.
- **Empfehlung:** Verwenden Sie die **Ableitungstransformation**, um die zugehörige Spalte der Eingabedaten vor der Snowflake-Senke in den Typ „String“ zu ändern. 

### <a name="error-code-df-hive-invalidblobstagingconfiguration"></a>Fehlercode: DF-Hive-InvalidBlobStagingConfiguration
- **Meldung:** Die Stagingeigenschaften des Blobspeichers müssen angegeben werden.

### <a name="error-code-df-hive-invalidgen2stagingconfiguration"></a>Fehlercode: DF-Hive-InvalidGen2StagingConfiguration
- **Meldung:** Im ADLS Gen2-Stagingspeicher werden nur Anmeldeinformationen für den Dienstprinzipalschlüssel unterstützt.
- **Empfehlung:** Vergewissern Sie sich, dass die Anmeldeinformationen für den Dienstprinzipalschlüssel im verknüpften ADLS Gen2-Dienst verwendet werden, der als Stagingspeicher verwendet wird.

### <a name="error-code-df-hive-invalidgen2stagingconfiguration"></a>Fehlercode: DF-Hive-InvalidGen2StagingConfiguration
- **Meldung:** Die Stagingeigenschaften des ADLS Gen2-Speichers müssen angegeben werden. Entweder der Schlüssel oder tenant/spnId/spnKey oder miServiceUri/miServiceToken ist erforderlich.
- **Empfehlung:** Wenden Sie die richtigen Anmeldeinformationen an, die für das Staging in der Struktur im zugehörigen verknüpften ADLS Gen2-Dienst verwendet werden. 

### <a name="error-code-df-hive-invaliddatatype"></a>Fehlercode: DF-Hive-InvalidDataType
- **Meldung:** Nicht unterstützte Spalte(n).
- **Empfehlung:** Aktualisieren Sie die Spalte der Eingabedaten so, dass sie dem in der Struktur unterstützten Datentyp entspricht.

### <a name="error-code-df-hive-invalidstoragetype"></a>Fehlercode: DF-Hive-InvalidStorageType
- **Meldung:** Als Speichertyp kann entweder „Blob“ oder „Gen2“ festgelegt werden.

### <a name="error-code-df-delimited-invalidconfiguration"></a>Fehlercode: DF-Delimited-InvalidConfiguration
- **Meldung:** Es müssen entweder leere Zeilen oder benutzerdefinierte Header angegeben werden.
- **Empfehlung:** Geben Sie in den CSV-Einstellungen leere Zeilen oder benutzerdefinierte Header an.

### <a name="error-code-df-delimited-columndelimitermissed"></a>Fehlercode: DF-Delimited-ColumnDelimiterMissed
- **Meldung:** Das Spaltentrennzeichen ist für die Analyse erforderlich.
- **Empfehlung:** Vergewissern Sie sich, dass das Spaltentrennzeichen in den CSV-Einstellungen festgelegt ist.

### <a name="error-code-df-mssql-invalidcredential"></a>Fehlercode: DF-MSSQL-InvalidCredential
- **Meldung:** Entweder user/pwd oder tenant/spnId/spnKey oder miServiceUri/miServiceToken muss angegeben werden.
- **Empfehlung:** Wenden Sie die richtigen Anmeldeinformationen im zugehörigen verknüpften MSSQL-Dienst an.

### <a name="error-code-df-mssql-invaliddatatype"></a>Fehlercode: DF-MSSQL-InvalidDataType
- **Meldung:** Nicht unterstützte(s) Feld(er).
- **Empfehlung:** Ändern Sie die Spalte der Eingabedaten so, dass sie dem in MSSQL unterstützten Datentyp entspricht.

### <a name="error-code-df-mssql-invalidauthconfiguration"></a>Fehlercode: DF-MSSQL-InvalidAuthConfiguration
- **Meldung:** Es kann nur eine der drei Authentifizierungsmethoden (Key, ServicePrincipal und MI) angegeben werden.
- **Empfehlung:** Sie können nur eine der drei Authentifizierungsmethoden (Key, ServicePrincipal und MI) im zugehörigen verknüpften MSSQL-Dienst angeben.

### <a name="error-code-df-mssql-invalidcloudtype"></a>Fehlercode: DF-MSSQL-InvalidCloudType
- **Meldung:** Cloudtyp ist ungültig.
- **Empfehlung:** Überprüfen Sie den Cloudtyp im zugehörigen verknüpften MSSQL-Dienst.

### <a name="error-code-df-sqldw-invalidblobstagingconfiguration"></a>Fehlercode: DF-SQLDW-InvalidBlobStagingConfiguration
- **Meldung:** Die Stagingeigenschaften des Blobspeichers müssen angegeben werden.

### <a name="error-code-df-sqldw-invalidstoragetype"></a>Fehlercode: DF-SQLDW-InvalidStorageType
- **Meldung:** Als Speichertyp kann entweder „Blob“ oder „Gen2“ festgelegt werden.

### <a name="error-code-df-sqldw-invalidgen2stagingconfiguration"></a>Fehlercode: DF-SQLDW-InvalidGen2StagingConfiguration
- **Meldung:** Im ADLS Gen2-Stagingspeicher werden nur Anmeldeinformationen für den Dienstprinzipalschlüssel unterstützt.

### <a name="error-code-df-sqldw-invalidconfiguration"></a>Fehlercode: DF-SQLDW-InvalidConfiguration
- **Meldung:** Die Stagingeigenschaften des ADLS Gen2-Speichers müssen angegeben werden. Entweder der Schlüssel oder tenant/spnId/spnCredential/spnCredentialType oder miServiceUri/miServiceToken ist erforderlich.

### <a name="error-code-df-delta-invalidconfiguration"></a>Fehlercode: DF-DELTA-InvalidConfiguration
- **Meldung:** Zeitstempel und Version können nicht gleichzeitig festgelegt werden.

### <a name="error-code-df-delta-keycolumnmissed"></a>Fehlercode: DF-DELTA-KeyColumnMissed
- **Meldung:** Für nicht einfügbare Vorgänge müssen Schlüsselspalten angegeben werden.

### <a name="error-code-df-delta-invalidtableoperationsettings"></a>Fehlercode: DF-DELTA-InvalidTableOperationSettings
- **Meldung:** Die Optionen zum Neuerstellen und Entfernen können nicht gleichzeitig angegeben werden.

### <a name="error-code-df-excel-worksheetconfigmissed"></a>Fehlercode: DF-Excel-WorksheetConfigMissed
- **Meldung:** Der Name oder Index der Excel-Tabelle ist erforderlich.
- **Empfehlung:** Überprüfen Sie den Parameterwert, und geben Sie den Namen oder Index der Tabelle zum Lesen der Excel-Daten an.

### <a name="error-code-df-excel-invalidworksheetconfiguration"></a>Fehlercode: DF-Excel-InvalidWorksheetConfiguration
- **Meldung:** Der Name und der Index der Excel-Tabelle können nicht gleichzeitig vorhanden sein.
- **Empfehlung:** Überprüfen Sie den Parameterwert, und geben Sie den Namen oder Index der Tabelle zum Lesen der Excel-Daten an.

### <a name="error-code-df-excel-invalidrange"></a>Fehlercode: DF-Excel-InvalidRange
- **Meldung:** Es wurde ein ungültiger Bereich angegeben.
- **Empfehlung:** Überprüfen Sie den Parameterwert, und geben Sie den gültigen Bereich mit dem folgenden Verweis an: [Excel-Format in den Azure Data Factory-Dataseteigenschaften](./format-excel.md#dataset-properties).

### <a name="error-code-df-excel-worksheetnotexist"></a>Fehlercode: DF-Excel-WorksheetNotExist
- **Meldung:** Das Excel-Arbeitsblatt ist nicht vorhanden.
- **Empfehlung:** Überprüfen Sie den Parameterwert, und geben Sie den gültigen Namen oder Index der Tabelle zum Lesen der Excel-Daten an.

### <a name="error-code-df-excel-differentschemanotsupport"></a>Fehlercode: DF-Excel-DifferentSchemaNotSupport
- **Meldung:** Das Lesen von Excel-Dateien mit einem anderen Schema wird derzeit nicht unterstützt.

### <a name="error-code-df-excel-invaliddatatype"></a>Fehlercode: DF-Excel-InvalidDataType
- **Meldung:** Der Datentyp wird nicht unterstützt.

### <a name="error-code-df-excel-invalidfile"></a>Fehlercode: DF-Excel-InvalidFile
- **Meldung:** Es wurde eine ungültige Excel-Datei angegeben. Es werden nur XLSX- und XLS-Dateien unterstützt.


## <a name="miscellaneous-troubleshooting-tips"></a>Verschiedene Tipps zur Problembehandlung
- **Problem**: Unerwartete Ausnahme und Fehler bei der Ausführung.
    - **Meldung**: Während der Ausführung der Datenflussaktivität: Unerwartete Ausnahme, Fehler bei der Ausführung.
    - **Ursache:** Dies ist ein Fehler beim Back-End-Dienst. Wiederholen Sie den Vorgang, und starten Sie Ihre Debugsitzung neu.
    - **Empfehlung**: Wenn das Problem durch Wiederholung und Neustart nicht behoben werden kann, wenden Sie sich an den Kundensupport.

-  **Problem**: Während der Debugdatenvorschau gibt es keine Ausgabedaten für den Join.
    - **Meldung**: There are a high number of null values or missing values which may be caused by having too few rows sampled. Try updating the debug row limit and refreshing the data. (Es ist eine hohe Anzahl von NULL-Werten oder fehlenden Werten vorhanden. Möglicherweise wurden von zu wenigen Zeilen Stichproben genommen. Aktualisieren Sie das Debugzeilenlimit, und aktualisieren Sie die Daten.)
    - **Ursache**: Die Joinbedingung stimmte entweder mit irgendwelchen Zeilen nicht überein oder führte während der Datenvorschau zu einer großen Anzahl von NULL-Werten.
    - **Empfehlung**: Erhöhen Sie in **Debugeinstellungen** die maximale Anzahl der Quellzeilen. Sie müssen eine Azure IR mit einem Datenflusscluster auswählen, der groß genug ist, um mehr Daten verarbeiten zu können.
    
- **Problem:** Überprüfungsfehler auf Quelle mit mehrzeiligen CSV-Dateien. 
    - **Meldung**: Möglicherweise wird eine der folgenden Fehlermeldungen angezeigt:
        - Die letzte Spalte ist NULL oder fehlt.
        - Fehler bei der Schemaüberprüfung auf der Quelle.
        - Der Schemaimport kann auf der Benutzeroberfläche nicht richtig angezeigt werden, und der Name der letzten Spalte enthält ein Neue-Zeile-Zeichen.
    - **Ursache**: Im Zuordnungsdatenfluss funktionieren mehrzeilige CSV-Quelldateien zurzeit nicht, wenn „\r\n“ als Zeilentrennzeichen verwendet wird. Manchmal können zusätzliche Zeilen bei Wagenrückläufen Fehler verursachen. 
    - **Empfehlung**: Generieren Sie die Datei auf der Quelle mit „\n“ (statt „\r\n“) als Zeilentrennzeichen. Oder konvertieren Sie die CSV-Datei mithilfe der Copy-Aktivität so, dass „\n“ als Zeilentrennzeichen verwendet wird.

## <a name="general-troubleshooting-guidance"></a>Allgemeine Anleitungen zur Problembehandlung
1. Überprüfen Sie den Status der Datasetverbindungen. Wechseln Sie in jeder einzelnen Quell-und Senkentransformation zum verknüpften Dienst für jedes verwendete Dataset, und testen Sie die Verbindungen.
2. Überprüfen Sie im Datenfluss-Designer den Status Ihrer Datei- und Tabellenverbindungen. Wählen Sie im Debugmodus in Ihren Quelltransformationen **Datenvorschau** aus, um sicherzustellen, dass Sie auf Ihre Daten zugreifen können.
3. Wenn in der Datenvorschau keine Probleme erkennbar sind, wechseln Sie zum Pipeline-Designer, und fügen Sie Ihren Datenfluss in eine Pipelineaktivität ein. Debuggen Sie die Pipeline in einem End-to-End-Test.

### <a name="improvement-on-csvcdm-format-in-data-flow"></a>Verbesserung im CSV/CDM-Format im Datenfluss 

Wenn Sie die Formatierung **Delimited Text oder CDM für das Mapping des Datenflusses in Azure Data Factory V2** verwenden, kann es aufgrund der Verbesserung für Delimited Text/CDM im Datenfluss ab dem **1. Mai 2021** zu Verhaltensänderungen in Ihren bestehenden Pipelines kommen. 

Die folgenden Probleme können vor der Verbesserung auftreten, die Probleme wurden jedoch nach der Verbesserung behoben. Lesen Sie den folgenden Inhalt, um zu bestimmen, ob sich diese Verbesserung auf Sie auswirkt. 

#### <a name="scenario-1-encounter-the-unexpected-row-delimiter-issue"></a>Szenario 1: Problem mit dem unerwarteten Zeilentrennzeichen

 Sie sind betroffen, wenn Sie sich in den folgenden Umständen befinden:
 - Verwenden Sie den durch Trennzeichen getrennten Text, bei dem die mehrzeilige Einstellung auf True oder CDM als Quelle eingestellt ist.
 - Die erste Zeile enthält über 128 Zeichen. 
 - Das Zeilentrennzeichen in Datendateien ist nicht `\n`.

 Vor der Verbesserung kann das Standardzeilen Trennzeichen `\n` unerwartet verwendet werden, um durch Trennzeichen getrennte Textdateien zu analysieren, denn wenn die mehrzeilige Einstellung auf True festgelegt ist, wird die Zeilentrennzeichen-Einstellung ungültig, und das Zeilentrennzeichen wird automatisch basierend auf den ersten 128 Zeichen erkannt. Wenn Sie das tatsächliche Zeilentrennzeichen nicht ermitteln können, greift es auf `\n` zurück.  

 Nach der Verbesserung sollte eines der drei Zeilentrennzeichen: `\r`, `\n`, `\r\n` bearbeitet werden.
 
 Das folgende Beispiel zeigt Ihnen eine Pipeline-Verhaltensänderung nach der Verbesserung an:

 **Beispiel**:<br/>
   Für die folgende Spalte:<br/>
    `C1, C2, {long first row}, C128\r\n `<br/>
    `V1, V2, {values………………….}, V128\r\n `<br/>
 
   Vor der Verbesserung wird `\r` im Spaltenwert beibehalten. Das Ergebnis der geparsten Spalte ist:<br/>
   `C1 C2 {long first row} C128`**`\r`**<br/>
   `V1 V2 {values………………….} V128`**`\r`**<br/> 

   Nach der Verbesserung sollte das Ergebnis der geparsten Spalte sein:<br/>
   `C1 C2 {long first row} C128`<br/>
   `V1 V2 {values………………….} V128`<br/>
  
#### <a name="scenario-2-encounter-an-issue-of-incorrectly-reading-column-values-containing-rn"></a>Szenario 2: Auftreten des Problems des fehlerhaften Lesens von Spaltenwerten mit '\r\n'

 Sie sind betroffen, wenn Sie sich in den folgenden Umständen befinden:
 - Verwendung von Begrenztem Text mit der Mehrzeilen-Einstellung auf True oder CDM als Quelle. 
 - Das Zeilentrennzeichen ist `\r\n`.

 Vor der Verbesserung wurde beim Lesen des Spaltenwerts das darin enthaltene `\r\n` möglicherweise fälschlicherweise durch `\n`ersetzt. 

 Nach der Verbesserung, wird das `\r\n` im Spaltenwert nicht durch `\n`ersetzt.

 Das folgende Beispiel zeigt Ihnen eine Pipeline-Verhaltensänderung nach der Verbesserung an:
 
 **Beispiel**:<br/>
  
 Für die folgende Spalte：<br/>
  **`"A\r\n"`**`, B, C\r\n`<br/>

 Vor der Verbesserung ist das Ergebnis der geparsten Spalte:<br/>
  **`A\n`**` B C`<br/>

 Nach der Verbesserung sollte das Ergebnis der geparsten Spalte sein:<br/>
  **`A\r\n`**` B C`<br/>  

#### <a name="scenario-3-encounter-an-issue-of-incorrectly-writing-column-values-containing-n"></a>Szenario 3: Auftreten des Problems des fehlerhaften Lesens von Spaltenwerten mit '\r\n'

 Sie sind betroffen, wenn Sie sich in den folgenden Umständen befinden:
 - Verwenden des durch Trennzeichen getrennten Texts als Senke.
 - Der Spaltenwert enthält `\n`.
 - Das Zeilentrennzeichen ist auf `\r\n` festgelegt.
 
 Vor der Verbesserung wurde beim Lesen des Spaltenwerts das darin enthaltene `\n` möglicherweise fälschlicherweise durch `\r\n`ersetzt. 

 Nach der Verbesserung, wird das `\n` im Spaltenwert nicht durch `\r\n`ersetzt.
 
 Das folgende Beispiel zeigt Ihnen eine Pipeline-Verhaltensänderung nach der Verbesserung an:

 **Beispiel**:<br/>

 Für die folgende Spalte:<br/>
 **`A\n`**` B C`<br/>

 Vor der Verbesserung ist die CSV-Senke wie folgt:<br/>
  **`"A\r\n"`**`, B, C\r\n` <br/>

 Nach der Verbesserung sollte die CSV-Senke wie folgt sein:<br/>
  **`"A\n"`**`, B, C\r\n`<br/>

#### <a name="scenario-4-encounter-an-issue-of-incorrectly-reading-empty-string-as-null"></a>Szenario 4: Problem beim nicht ordnungsgemäßem Lesen einer leeren Zeichenfolge als NULL
 
 Sie sind betroffen, wenn Sie sich in den folgenden Umständen befinden:
 - Verwenden des durch Trennzeichen getrennten Texts als Senke. 
 - Der NULL-Wert ist auf einen nicht leeren Wert festgelegt. 
 - Der Spaltenwert ist eine leere Zeichenfolge und ist nicht in Anführungszeichen eingeschlossen. 
 
 Vor der Verbesserung wird der Spaltenwert der leeren Zeichenfolge ohne Anführungszeichen als Null gelesen. 

 Nach der Verbesserung wird eine leere Zeichenfolge nicht als NULL-Wert analysiert. 
 
 Das folgende Beispiel zeigt Ihnen eine Pipeline-Verhaltensänderung nach der Verbesserung an:

 **Beispiel**:<br/>

 Für die folgende Spalte:<br/>
  `A, ,B, `<br/>

 Vor der Verbesserung ist das Ergebnis der geparsten Spalte:<br/>
  `A null B null`<br/>

 Nach der Verbesserung sollte das Ergebnis der geparsten Spalte sein:<br/>
  `A "" (empty string) B "" (empty string)`<br/>


## <a name="next-steps"></a>Nächste Schritte

Weitere Hilfe zur Problembehandlung finden Sie in diesen Ressourcen:

*  [Data Factory-Blog](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Data Factory-Funktionsanfragen](https://feedback.azure.com/forums/270578-data-factory)
*  [Azure-Videos](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [Stack Overflow-Forum für Data Factory](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Twitter-Informationen über Data Factory](https://twitter.com/hashtag/DataFactory)
