---
title: 'Verwenden von Spark zum Lesen und Schreiben von HBase-Daten: Azure HDInsight'
description: Verwenden Sie den Spark HBase-Connector, um Daten aus einem Spark-Cluster zu lesen und in einen HBase-Cluster zu schreiben.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive,seoapr2020
ms.date: 04/20/2020
ms.openlocfilehash: 3ddb8734a3d15a6cd5f4a43ee069d6364f7523ed
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86087485"
---
# <a name="use-apache-spark-to-read-and-write-apache-hbase-data"></a>Verwenden von Apache Spark zum Lesen und Schreiben von Apache HBase-Daten

Apache HBase wird üblicherweise über die Low-Level-API (scan-, get- und put-Abfragen) oder mit einer SQL-Syntax unter Verwendung von Apache Phoenix abgefragt. Apache bietet außerdem den Apache Spark HBase-Connector. Der Connector ist eine praktische und leistungsfähige Alternative zum Abfragen und Ändern von Daten, die von HBase gespeichert wurden.

## <a name="prerequisites"></a>Voraussetzungen

* Zwei separate HDInsight-Cluster, die im selben [virtuellen Netzwerk](./hdinsight-plan-virtual-network-deployment.md) bereitgestellt wurden: einen HBase-Cluster und einen Spark-Cluster mit Spark 2.1 (HDInsight 3.6) als Mindestversion. Weitere Informationen finden Sie unter [Erstellen von Linux-basierten Clustern in HDInsight mithilfe des Azure-Portals](hdinsight-hadoop-create-linux-clusters-portal.md).

* Das URI-Schema für Ihren primären Clusterspeicher. Dieses Schema ist „wasb://“ für Azure Blob Storage, „`abfs://`“ für Azure Data Lake Storage Gen2 oder „adl://“ für Azure Data Lake Storage Gen1. Wenn die sichere Übertragung für Blob Storage aktiviert ist, lautet der URI `wasbs://`.  Siehe auch [Vorschreiben einer sicheren Übertragung in Azure Storage](../storage/common/storage-require-secure-transfer.md).

## <a name="overall-process"></a>Übersicht über den Prozess

Der allgemeine Prozess zum Aktivieren Ihres Spark-Clusters für die Abfrage Ihres HDInsight-Clusters sieht wie folgt aus:

1. Bereiten Sie einige Beispieldaten in HBase vor.
2. Rufen Sie die Datei „hbase-site.xml“ aus dem Konfigurationsordner Ihres HBase-Clusters ab (/etc/hbase/conf).
3. Speichern Sie eine Kopie von „hbase-site.xml“ in Ihrem Spark 2-Konfigurationsordner (/etc/spark2/conf).
4. Führen Sie `spark-shell` aus, und verweisen Sie in der Option `packages` mit den Maven-Koordinaten auf den Spark HBase-Connector.
5. Definieren Sie einen Katalog, der das Spark-Schema zu HBase zuordnet.
6. Interagieren Sie entweder über die RDD- oder die Dataframe-APIs mit den HBase-Daten.

## <a name="prepare-sample-data-in-apache-hbase"></a>Vorbereiten von Beispieldaten in Apache HBase

In diesem Schritt erstellen Sie eine Tabelle in Apache HBase und füllen sie auf. Diese Tabelle können Sie dann mit Spark abfragen.

1. Verwenden Sie zum Herstellen der Verbindung mit Ihrem HBase-Cluster `ssh`. Bearbeiten Sie den unten angegebenen Befehl, indem Sie `HBASECLUSTER` durch den Namen Ihres HBase-Clusters ersetzen, und geben Sie den Befehl dann ein:

    ```cmd
    ssh sshuser@HBASECLUSTER-ssh.azurehdinsight.net
    ```

2. Verwenden Sie den Befehl `hbase shell`, um die interaktive HBase-Shell zu starten. Geben Sie den folgenden Befehl in Ihrer SSH-Verbindung ein:

    ```bash
    hbase shell
    ```

3. Verwenden Sie den Befehl `create`, um eine HBase-Tabelle mit zwei Spaltenfamilien zu erstellen. Geben Sie den folgenden Befehl ein:

    ```hbase
    create 'Contacts', 'Personal', 'Office'
    ```

4. Verwenden Sie den Befehl `put`, um Werte in einer angegebenen Spalte einer angegebenen Zeile in einer bestimmten Tabelle einzufügen. Geben Sie den folgenden Befehl ein:

    ```hbase
    put 'Contacts', '1000', 'Personal:Name', 'John Dole'
    put 'Contacts', '1000', 'Personal:Phone', '1-425-000-0001'
    put 'Contacts', '1000', 'Office:Phone', '1-425-000-0002'
    put 'Contacts', '1000', 'Office:Address', '1111 San Gabriel Dr.'
    put 'Contacts', '8396', 'Personal:Name', 'Calvin Raji'
    put 'Contacts', '8396', 'Personal:Phone', '230-555-0191'
    put 'Contacts', '8396', 'Office:Phone', '230-555-0191'
    put 'Contacts', '8396', 'Office:Address', '5415 San Gabriel Dr.'
    ```

5. Verwenden Sie den Befehl `exit`, um die interaktive HBase-Shell zu beenden. Geben Sie den folgenden Befehl ein:

    ```hbase
    exit
    ```

## <a name="copy-hbase-sitexml-to-spark-cluster"></a>Kopieren von „hbase-site.xml“ in den Spark-Cluster

Kopieren Sie die Datei „hbase-site.xml“ aus dem lokalen Speicher in das Stammverzeichnis des Standardspeichers Ihres Spark-Clusters.  Bearbeiten Sie den Befehl, um ihn an Ihre Konfiguration anzupassen.  Geben Sie anschließend in Ihrer geöffneten SSH-Sitzung diesen Befehl für den HBase-Cluster ein:

| Syntaxwert | Neuer Wert|
|---|---|
|[URI-Schema](hdinsight-hadoop-linux-information.md#URI-and-scheme) | Nehmen Sie Änderungen zur Anpassung an Ihren Speicher vor.  Die unten angegebene Syntax gilt für Blobspeicher mit aktivierter sicherer Übertragung.|
|`SPARK_STORAGE_CONTAINER`|Fügen Sie den Namen Ihres Standardspeichercontainers ein, der für den Spark-Cluster verwendet wird.|
|`SPARK_STORAGE_ACCOUNT`|Fügen Sie den Namen Ihres Standardspeicherkontos ein, der für den Spark-Cluster verwendet wird.|

```bash
hdfs dfs -copyFromLocal /etc/hbase/conf/hbase-site.xml wasbs://SPARK_STORAGE_CONTAINER@SPARK_STORAGE_ACCOUNT.blob.core.windows.net/
```

Trennen Sie dann die SSH-Verbindung mit Ihrem HBase-Cluster.

```bash
exit
```

## <a name="put-hbase-sitexml-on-your-spark-cluster"></a>Ablegen von „hbase-site.xml“ in Ihrem Spark-Cluster

1. Stellen Sie über SSH eine Verbindung mit dem Hauptknoten Ihres Spark-Clusters her. Bearbeiten Sie den unten angegebenen Befehl, indem Sie `SPARKCLUSTER` durch den Namen Ihres Spark-Clusters ersetzen, und geben Sie dann den folgenden Befehl ein:

    ```cmd
    ssh sshuser@SPARKCLUSTER-ssh.azurehdinsight.net
    ```

2. Geben Sie den folgenden Befehl ein, um `hbase-site.xml` aus dem Standardspeicher Ihres Spark-Clusters in den Spark 2-Konfigurationsordner im lokalen Speicher des Clusters zu kopieren:

    ```bash
    sudo hdfs dfs -copyToLocal /hbase-site.xml /etc/spark2/conf
    ```

## <a name="run-spark-shell-referencing-the-spark-hbase-connector"></a>Ausführen der Spark-Shell und Verweisen auf den Spark HBase-Connector

Nachdem Sie den vorherigen Schritt abgeschlossen haben, sollten Sie die Spark-Shell ausführen können, die auf die entsprechende Version des Spark HBase-Connectors verweist. Die neueste geeignete Kernversion des Spark HBase-Connectors für Ihr Clusterszenario finden Sie unter [SHC Core Repository](https://repo.hortonworks.com/content/groups/public/com/hortonworks/shc/shc-core/).

In der folgenden Tabelle werden beispielsweise zwei Versionen und die entsprechenden Befehle aufgelistet, die vom HDInsight-Team derzeit verwendet werden. Sie können die gleichen Versionen für Ihre Cluster verwenden, wenn die Versionen von HBase und Spark den in der Tabelle aufgeführten entsprechen. 


1. Geben Sie in Ihrer geöffneten SSH-Sitzung für den Spark-Cluster den folgenden Befehl ein, um eine Spark-Shell zu starten:

    |Spark-Version| HDI HBase-Version  | SHC-Version    |  Get-Help  |
    | :-----------:| :----------: | :-----------: |:----------- |
    |      2.1    | HDI 3.6 (HBase 1.1) | 1.1.0.3.1.2.2-1    | `spark-shell --packages com.hortonworks:shc-core:1.1.1-2.1-s_2.11 --repositories https://repo.hortonworks.com/content/groups/public/` |
    |      2.4    | HDI 4.0 (HBase 2.0) | 1.1.1-2.1-s_2.11  | `spark-shell --packages com.hortonworks.shc:shc-core:1.1.0.3.1.2.2-1 --repositories http://repo.hortonworks.com/content/groups/public/` |

2. Lassen Sie diese Instanz der Spark-Shell geöffnet, und fahren Sie mit dem [Definieren von Katalog und Abfrage](#define-a-catalog-and-query) fort. Wenn Sie die JAR-Daten nicht finden, die Ihren Versionen im SHC Core-Respository entsprechen, lesen Sie weiter. 

Sie können die JAR-Dateien direkt über die GitHub-Verzweigung [spark-hbase-connector](https://github.com/hortonworks-spark/shc) erstellen. Wenn Sie beispielsweise Spark 2.3 und HBase 1.1 verwenden, führen Sie die folgenden Schritte aus:

1. Klonen Sie das Repository:

    ```bash
    git clone https://github.com/hortonworks-spark/shc
    ```
    
2. Navigieren Sie zu „branch-2.3“:

    ```bash
    git checkout branch-2.3
    ```

3. Nehmen Sie die Erstellung auf der Grundlage des Branch vor (Erstellung einer JAR-Datei):

    ```bash
    mvn clean package -DskipTests
    ```
    
3. Führen Sie den folgenden Befehl aus. (Ändern Sie unbedingt den JAR-Namen, der der von Ihnen erstellten JAR-Datei entspricht.)

    ```bash
    spark-shell --jars <path to your jar>,/usr/hdp/current/hbase-client/lib/htrace-core-3.1.0-incubating.jar,/usr/hdp/current/hbase-client/lib/hbase-client.jar,/usr/hdp/current/hbase-client/lib/hbase-common.jar,/usr/hdp/current/hbase-client/lib/hbase-server.jar,/usr/hdp/current/hbase-client/lib/hbase-protocol.jar,/usr/hdp/current/hbase-client/lib/htrace-core-3.1.0-incubating.jar
    ```
    
4. Lassen Sie diese Instanz der Spark-Shell geöffnet, und fahren Sie mit dem nächsten Abschnitt fort. 



## <a name="define-a-catalog-and-query"></a>Definieren von Katalog und Abfrage

In diesem Schritt definieren Sie ein Katalogobjekt, das das Apache Spark-Schema Apache HBase zuordnet.  

1. Geben Sie in Ihrer geöffneten Spark-Shell die folgenden `import`-Anweisungen ein:

    ```scala
    import org.apache.spark.sql.{SQLContext, _}
    import org.apache.spark.sql.execution.datasources.hbase._
    import org.apache.spark.{SparkConf, SparkContext}
    import spark.sqlContext.implicits._
    ```  

1. Geben Sie den folgenden Befehl ein, um einen Katalog für die Tabelle „Kontakte“ zu definieren, die Sie in HBase erstellt haben:

    ```scala
    def catalog = s"""{
        |"table":{"namespace":"default", "name":"Contacts"},
        |"rowkey":"key",
        |"columns":{
        |"rowkey":{"cf":"rowkey", "col":"key", "type":"string"},
        |"officeAddress":{"cf":"Office", "col":"Address", "type":"string"},
        |"officePhone":{"cf":"Office", "col":"Phone", "type":"string"},
        |"personalName":{"cf":"Personal", "col":"Name", "type":"string"},
        |"personalPhone":{"cf":"Personal", "col":"Phone", "type":"string"}
        |}
    |}""".stripMargin
    ```

    Der Code:  

    1. definiert ein Katalogschema für die HBase-Tabelle namens `Contacts`.  
    1. identifiziert das rowkey-Element als `key` und ordnet die in Spark verwendeten Spaltennamen der Spaltenfamilie, dem Spaltennamen und dem Spaltentyp zu, die in HBase verwendet werden.  
    1. definiert den rowkey im Detail als benannte Spalte (`rowkey`), die über eine spezifische `cf`-Spaltenfamilie `rowkey` verfügt.  

1. Geben Sie den folgenden Befehl ein, um eine Methode zu definieren, die einen Datenrahmen um Ihre `Contacts`-Tabelle in HBase bereitstellt:

    ```scala
    def withCatalog(cat: String): DataFrame = {
        spark.sqlContext
        .read
        .options(Map(HBaseTableCatalog.tableCatalog->cat))
        .format("org.apache.spark.sql.execution.datasources.hbase")
        .load()
     }
    ```

1. Erstellen Sie eine Instanz des Datenrahmens:

    ```scala
    val df = withCatalog(catalog)
    ```  

1. Fragen Sie den Datenrahmen ab:

    ```scala
    df.show()
    ```

    Es sollten zwei Zeilen mit Daten angezeigt werden:

    ```output
    +------+--------------------+--------------+-------------+--------------+
    |rowkey|       officeAddress|   officePhone| personalName| personalPhone|
    +------+--------------------+--------------+-------------+--------------+
    |  1000|1111 San Gabriel Dr.|1-425-000-0002|    John Dole|1-425-000-0001|
    |  8396|5415 San Gabriel Dr.|  230-555-0191|  Calvin Raji|  230-555-0191|
    +------+--------------------+--------------+-------------+--------------+
    ```

1. Registrieren Sie eine temporäre Tabelle, sodass Sie die HBase-Tabelle mithilfe von Spark SQL abfragen können:

    ```scala
    df.createTempView("contacts")
    ```

1. Erstellen Sie eine SQL-Abfrage für die `contacts`-Tabelle:

    ```scala
    spark.sqlContext.sql("select personalName, officeAddress from contacts").show
    ```

    Die Ergebnisse sollten wie folgt aussehen:

    ```output
    +-------------+--------------------+
    | personalName|       officeAddress|
    +-------------+--------------------+
    |    John Dole|1111 San Gabriel Dr.|
    |  Calvin Raji|5415 San Gabriel Dr.|
    +-------------+--------------------+
    ```

## <a name="insert-new-data"></a>Einfügen neuer Daten

1. Um einen neuen Kontaktdatensatz einzufügen, definieren Sie eine `ContactRecord`-Klasse:

    ```scala
    case class ContactRecord(
        rowkey: String,
        officeAddress: String,
        officePhone: String,
        personalName: String,
        personalPhone: String
        )
    ```

1. Erstellen Sie eine Instanz von `ContactRecord`, und fügen Sie sie in ein Array ein:

    ```scala
    val newContact = ContactRecord("16891", "40 Ellis St.", "674-555-0110", "John Jackson","230-555-0194")

    var newData = new Array[ContactRecord](1)
    newData(0) = newContact
    ```

1. Speichern Sie das Array mit den neuen Daten in HBase:

    ```scala
    sc.parallelize(newData).toDF.write.options(Map(HBaseTableCatalog.tableCatalog -> catalog, HBaseTableCatalog.newTable -> "5")).format("org.apache.spark.sql.execution.datasources.hbase").save()
    ```

1. Untersuchen Sie die Ergebnisse:

    ```scala  
    df.show()
    ```

    Es sollte in etwa folgende Ausgabe angezeigt werden:

    ```output
    +------+--------------------+--------------+------------+--------------+
    |rowkey|       officeAddress|   officePhone|personalName| personalPhone|
    +------+--------------------+--------------+------------+--------------+
    |  1000|1111 San Gabriel Dr.|1-425-000-0002|   John Dole|1-425-000-0001|
    | 16891|        40 Ellis St.|  674-555-0110|John Jackson|  230-555-0194|
    |  8396|5415 San Gabriel Dr.|  230-555-0191| Calvin Raji|  230-555-0191|
    +------+--------------------+--------------+------------+--------------+
    ```

1. Geben Sie den folgenden Befehl ein, um die Spark-Shell zu schließen:

    ```scala
    :q
    ```

## <a name="next-steps"></a>Nächste Schritte

* [Apache Spark HBase-Connector](https://github.com/hortonworks-spark/shc)
