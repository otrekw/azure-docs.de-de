---
title: Metadaten und Datenherkunft aus dem Apache Atlas Spark-Connector
description: In diesem Artikel wird die Datenherkunftsextraktion aus Spark mithilfe des Atlas Spark-Connectors beschrieben.
author: chanuengg
ms.author: csugunan
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 04/28/2021
ms.openlocfilehash: aebe227c8f2a278131d38143c43c38ee4687e701
ms.sourcegitcommit: e1d5abd7b8ded7ff649a7e9a2c1a7b70fdc72440
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2021
ms.locfileid: "110579051"
---
# <a name="how-to-use-apache-atlas-connector-to-collect-spark-lineage"></a>Verwenden des Apache Atlas-Connectors zum Erfassen der Spark-Datenherkunft

Der Apache Atlas Spark-Connector ist ein Hook zum Nachverfolgen von Spark SQL/DataFrame-Datenverschiebungen und zum Pushen von Metadatenänderungen an den Purview Atlas-Endpunkt. 

## <a name="supported-scenarios"></a>Unterstützte Szenarios

Dieser Connector unterstützt die folgende Nachverfolgung:
1.  SQL-DDLs wie „CREATE/ALTER DATABASE“, „CREATE/ALTER TABLE“
2.  SQL-DMLs wie „CREATE TABLE HelloWorld AS SELECT“, „INSERT INTO...“, „LOAD DATA [LOCAL] INPATH“, „INSERT OVERWRITE [LOCAL] DIRECTORY“ usw.
3.  DataFrame-Verschiebungen mit Eingaben und Ausgaben

Von diesem Connector wird der Abfragelistener dazu verwendet, die Abfrage abzurufen und die Auswirkungen zu untersuchen. Dabei erfolgt eine Korrelation mit anderen Systemen wie Hive und HDFS, damit der Lebenszyklus von Daten in Atlas nachverfolgt werden kann.
Da Purview die Atlas-API und den nativen Atlas-Hook unterstützt, kann der Connector nach der Konfiguration mit Spark die Datenherkunft an Purview melden. Der Connector kann pro Auftrag oder als Standardeinstellung des Clusters konfiguriert werden. 

## <a name="configuration-requirement"></a>Konfigurationsanforderung

Die Connectors erfordern eine Version von Spark 2.4.0 oder höher. Die Version 3 von Spark wird nicht unterstützt. Spark unterstützt drei Arten von Listenern, die festgelegt werden müssen:  

| Listener |    Ab Spark-Version|
| ------------------- | ------------------- | 
| spark.extraListeners | 1.3.0 |
| spark.sql.queryExecutionListeners | 2.3.0 |
| spark.sql.streaming.streamingQueryListeners | 2.4.0 |

Wenn die Spark-Clusterversion niedriger ist als 2.4.0, werden die Stream-Abfragedatenherkunft und der Großteil der Abfragedatenherkunft nicht erfasst.

### <a name="step-1-prepare-spark-atlas-connector-package"></a>Schritt 1: Vorbereiten des Spark Atlas-Connectorpakets
Die folgenden Schritte werden anhand von DataBricks als Beispiel dokumentiert:

1.  Generieren des Pakets
    1. Pullen Sie Code aus GitHub: https://github.com/hortonworks-spark/spark-atlas-connector
    2. [Für Windows] Kommentieren Sie das **maven-enforcer-plugin** in spark-atlas-connector\pom.xml aus, um die Abhängigkeit von Unix zu entfernen.

    ```web
    <requireOS>
        <family>unix</family>
    </requireOS>
    ```
    
    c. Führen Sie den Befehl **mvn package -DskipTests** im zu erstellenden Projektstamm aus. 
    
    d. Rufen Sie die JAR-Datei aus *~\spark-atlas-connector-assembly\target\spark-atlas-connector-assembly-0.1.0-SNAPSHOT.jar* ab.
    
    e. Legen Sie das Paket an einem Ort ab, auf den der Spark-Cluster zugreifen kann. Für den DataBricks-Cluster kann das Paket in den dbfs-Ordner hochgeladen werden, z. B. /FileStore/jars.

2. Vorbereiten der Connectorkonfiguration
    1. Rufen Sie den Kafka-Endpunkt und die entsprechenden Anmeldeinformationen im Azure-Portal des Purview-Kontos ab.
        1. Legen Sie für Ihr Konto die Berechtigung *„Datenkurator für Purview“* fest. 

        :::image type="content" source="./media/how-to-lineage-spark-atlas-connector/assign-purview-data-curator-role.png" alt-text="Screenshot: Rollenzuweisung für Datenkurator" lightbox="./media/how-to-lineage-spark-atlas-connector/assign-purview-data-curator-role.png":::

        1. Endpunkt: Abrufen der *primären Verbindungszeichenfolge des Atlas Kafka-Endpunkts*. Endpunktteil
        1. Anmeldeinformationen: Gesamte *primäre Verbindungszeichenfolge des Atlas-Kafka-Endpunkts*
        
        :::image type="content" source="./media/how-to-lineage-spark-atlas-connector/atlas-kafka-endpoint.png" alt-text="Screenshot: Atlas-Kafka-Endpunkt" lightbox="./media/how-to-lineage-spark-atlas-connector/atlas-kafka-endpoint.png":::        
        
    1. Bereiten Sie die Datei *atlas-application.properties* vor, und ersetzen Sie den *atlas.kafka.bootstrap.servers*-Wert und den Kennwortwert in *atlas.kafka.sasl.jaas.config*.

    ```script
    atlas.client.type=kafka
    atlas.kafka.sasl.mechanism=PLAIN
    atlas.kafka.security.protocol=SASL_SSL
    atlas.kafka.bootstrap.servers= atlas-46c097e6-899a-44aa-9a30-6ccd0b2a2a91.servicebus.windows.net:9093
    atlas.kafka.sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="<connection string got from your Purview account>";
    ```
    
    c.  Vergewissern Sie sich, dass sich die Atlas-Konfigurationsdatei im Treiberklassenpfad befindet, der in [Schritt 1 des obigen Abschnitts „Generieren des Pakets“](../purview/how-to-lineage-spark-atlas-connector.md#step-1-prepare-spark-atlas-connector-package) generiert wurde. Senden Sie diese Konfigurationsdatei im Clustermodus an das Remotelaufwerk *--files atlas-application.properties*.


### <a name="step-2-prepare-your-purview-account"></a>Schritt 2: Vorbereiten Ihres Purview-Kontos
Nachdem die Atlas Spark-Modelldefinition erfolgreich erstellt wurde, führen Sie die folgenden Schritte aus.
1. Abrufen der Spark-Typdefinition von GitHub: https://github.com/apache/atlas/blob/release-2.1.0-rc3/addons/models/1000-Hadoop/1100-spark_model.json

2. Zuweisen der Rolle
    1. Navigieren Sie zu Ihrem Purview-Konto, und wählen Sie Zugriffssteuerung (IAM) aus. 
    1. Fügen Sie Benutzer hinzu, und weisen Sie dem Dienstprinzipal die Rolle *Datenquellenadministrator für Purview* zu.
3. Abrufen des Authentifizierungstokens
    1. Öffnen Sie „Postman“ oder ähnliche Tools. 
    1. Verwenden Sie den Dienstprinzipal, der im vorherigen Schritt verwendet wurde, um das Bearertoken abzurufen:
        * Endpunkt: https://login.windows.net/microsoft.com/oauth2/token
        * grant_type: client_credentials
        * client_id: {service principal ID}
        * client_secret: {service principal key}
        * resource: `https://purview.azure.net`

        :::image type="content" source="./media/how-to-lineage-spark-atlas-connector/postman-examples.png" alt-text="Screenshot: Postman-Beispiel" lightbox="./media/how-to-lineage-spark-atlas-connector/postman-examples.png":::      

4. Bereitstellen der Spark Atlas-Modelldefinition im Purview-Konto
    1.  Rufen Sie den Atlas-Endpunkt des Purview-Kontos im Abschnitt „Eigenschaften“ des Azure-Portals ab.
    1. Stellen Sie die Spark-Typdefinition im Purview-Konto bereit.
       * Bereitstellen: {{endpoint}}/api/atlas/v2/types/typedefs
       * Verwenden Sie das generierte Zugriffstoken. 
       * Text: Wählen Sie raw aus, und kopieren Sie alle Inhalte aus GitHub: https://github.com/apache/atlas/blob/release-2.1.0-rc3/addons/models/1000-Hadoop/1100-spark_model.json.

:::image type="content" source="./media/how-to-lineage-spark-atlas-connector/postman-example-type-definition.png" alt-text="Screenshot des Postman-Beispiels für die Typdefinition" lightbox="./media/how-to-lineage-spark-atlas-connector/postman-example-type-definition.png":::

### <a name="step-3-prepare-spark-job"></a>Schritt 3: Vorbereiten eines Spark-Auftrags
1. Schreiben Sie Ihren Spark-Auftrag wie gewohnt.
2. Fügen Sie Connectoreinstellungen im Quellcode des Spark-Auftrags hinzu. Legen Sie den Systemeigenschaftswert *"atlas.conf"* im Code wie unten angegeben fest, um sicherzustellen, dass die Datei *atlas-application.properties* gefunden werden kann.

    **System.setProperty("atlas.conf", "/dbfs/FileStore/jars/")**

3. Erstellen Sie den Quellcode des Spark-Auftrags, um eine JAR-Datei zu generieren. 
4. Legen Sie die JAR-Datei der Spark-Anwendung an einem Speicherort ab, auf den Ihr Cluster zugreifen kann. Legen Sie die JAR-Datei beispielsweise unter *"/dbfs/FileStore/jars/"DataBricks* ab. 

### <a name="step-4-prepare-to-run-job"></a>Schritt 4. Vorbereiten der Ausführung des Auftrags
 
1. Die folgenden Anweisungen gelten für die einzelnen Auftragseinstellungen: Verwenden Sie zum Erfassen der Datenherkunft spezifischer Aufträge spark-submit, um einen Auftrag mit dem zugehörigen Parameter zu starten. 

    Im Auftragsparametersatz:
* Pfad der JAR-Datei des Connectors 
* Drei Listener: extraListeners, queryExecutionListeners, streamingQueryListeners als Connector 

| Listener | Details |
| ------------------- | ------------------- | 
| spark.extraListeners  | com.hortonworks.spark.atlas.SparkAtlasEventTracker|
| spark.sql.queryExecutionListeners | com.hortonworks.spark.atlas.SparkAtlasEventTracker
| spark.sql.streaming.streamingQueryListeners | com.hortonworks.spark.atlas.SparkAtlasStreamingQueryEventTracker |

* Pfad der JAR-Datei Ihrer Spark-Auftragsanwendung

Einrichten eines Databricks-Auftrags: Der wichtigste Teil besteht darin, dass Sie spark-submit verwenden, um einen Auftrag mit ordnungsgemäß eingerichteten Listenern auszuführen. Legen Sie die Listenerinformationen im Aufgabenparameter fest.   

Im Folgenden finden Sie einen Beispielparameter für den Spark-Auftrag.

```script
["--jars","dbfs:/FileStore/jars/spark-atlas-connector-assembly-0.1.0-SNAPSHOT.jar ","--conf","spark.extraListeners=com.hortonworks.spark.atlas.SparkAtlasEventTracker","--conf","spark.sql.queryExecutionListeners=com.hortonworks.spark.atlas.SparkAtlasEventTracker","--conf","spark.sql.streaming.streamingQueryListeners=com.hortonworks.spark.atlas.SparkAtlasStreamingQueryEventTracker","--class","com.microsoft.SparkAtlasTest","dbfs:/FileStore/jars/08cde51d_34d8_4913_a930_46f376606d7f-sparkatlas_1_6_SNAPSHOT-17452.jar"]
```

Im Folgenden finden Sie ein Beispiel für spark-submit über die Befehlszeile:

```script
spark-submit --class com.microsoft.SparkAtlasTest --master yarn --deploy-mode --files /data/atlas-application.properties --jars /data/ spark-atlas-connector-assembly-0.1.0-SNAPSHOT.jar 
--conf spark.extraListeners=com.hortonworks.spark.atlas.SparkAtlasEventTracker 
--conf spark.sql.queryExecutionListeners=com.hortonworks.spark.atlas.SparkAtlasEventTracker 
--conf spark.sql.streaming.streamingQueryListeners=com.hortonworks.spark.atlas.SparkAtlasEventTracker
/data/worked/sparkApplication.jar
```

2. Die folgenden Anweisungen gelten für die Clustereinstellung: Die Connector-JAR-Datei und die Einstellung des Listeners sind in Spark-Clustern anzugeben: *conf/spark-defaults.conf.* . Mit spark-submit werden die Optionen in *conf/spark-defaults.conf* gelesen und an Ihre Anwendung übergeben. 
 
### <a name="step-5-run-and-check-lineage-in-purview-account"></a>Schritt 5: Ausführen und Überprüfen der Datenherkunft im Purview-Konto
Starten Sie den Spark-Auftrag, und überprüfen Sie die Informationen zur Datenherkunft in Ihrem Purview-Konto. 

:::image type="content" source="./media/how-to-lineage-spark-atlas-connector/purview-with-spark-lineage.png" alt-text="Screenshot: Purview mit Spark-Datenherkunft" lightbox="./media/how-to-lineage-spark-atlas-connector/purview-with-spark-lineage.png":::

## <a name="known-limitations-with-the-connector-for-spark-lineage"></a>Bekannte Einschränkungen beim Connector für die Spark-Datenherkunft
1. Unterstützt die SQL/DataFrame-API (d. h. RDD wird nicht unterstützt). Von diesem Connector wird der Abfragelistener dazu verwendet, die Abfrage abzurufen und die Auswirkungen zu untersuchen.
    
2. Alle „Eingaben“ und „Ausgaben“ aus mehreren Abfragen werden zu einer einzigen „spark_process“-Entität kombiniert.
    
    „spark_process“ wird einer „applicationId“ in Spark zugeordnet. Dadurch können Administratoren alle Änderungen nachverfolgen, die im Rahmen einer Anwendung aufgetreten sind. Dadurch wird aber auch bewirkt, dass das Datenherkunfts-/Beziehungsdiagramm in „spark_process“ kompliziert und weniger aussagekräftig ist.
3. Nur ein Teil der Eingaben wird in der Streamingabfrage nachverfolgt.

* Die Kafka-Quelle unterstützt das Abonnieren mit „Pattern“, und dieser Connector führt nicht alle vorhandenen übereinstimmenden Themen, geschweige denn alle möglichen Themen auf. 
 
* Der „ausgeführte Plan“ stellt bestehende Themen mit (Micro-)Batchlesevorgängen und -prozessen bereit. Daher werden nur Eingaben, die Teil vom (Micro-)Batch sind, als „Eingaben“ der Entität „spark_process“ einbezogen.
    
4. Dieser Connector unterstützt keine Datenherkunft auf Spaltenebene.

5. Tabellen, die gelöscht werden (Spark-Modelle), werden nicht nachverfolgt.

    Das Ereignis „Drop Table“ von Spark stellt nur db und „table name“ bereit, was NICHT ausreicht, um den eindeutigen Schlüssel zum Erkennen der Tabelle zu erstellen.

    Der Connector hängt vom Lesen des Spark-Katalogs ab, damit Tabelleninformationen abgerufen werden können. Spark hat die Tabelle bereits gelöscht, wenn von diesem Connector festgestellt wird, dass die Tabelle gelöscht ist, sodass „Drop Table“ nicht funktioniert.


## <a name="next-steps"></a>Nächste Schritte

- [Erfahren Sie mehr über Data Lineage in Azure Purview](catalog-lineage-user-guide.md)
- [Verknüpfen Sie Azure Data Factory, um die automatisierte Lineage zu pushen](how-to-link-azure-data-factory.md)
