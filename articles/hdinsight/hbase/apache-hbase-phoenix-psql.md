---
title: Massenladen in Apache Phoenix mit psql – Azure HDInsight
description: Verwenden des psql-Tools, um das Massenladen von Daten in Apache Phoenix-Tabellen in Azure HDInsight durchzuführen
author: ashishthaps
ms.author: ashishth
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 12/17/2019
ms.openlocfilehash: 845c4a62aee04a8acdc645ba4c41f1f5496537c3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "75552609"
---
# <a name="bulk-load-data-into-apache-phoenix-using-psql"></a>Massenladen von Daten in Apache Phoenix mithilfe von psql

[Apache Phoenix](https://phoenix.apache.org/) ist eine relationale Open-Source-Datenbank mit hochgradig parallelisierter Verarbeitung, die auf [Apache HBase](../hbase/apache-hbase-overview.md) basiert. Bei Phoenix werden SQL-ähnliche Abfragen über HBase bereitgestellt. Phoenix nutzt JDBC-Treiber, um Benutzern das Erstellen, Löschen und Ändern von SQL-Tabellen, -Indizes, -Sichten und -Sequenzen sowie das Durchführen des Upsert-Vorgangs für Zeilen einzeln oder als Massenvorgang zu ermöglichen. Für Phoenix wird anstelle von MapReduce die native NoSQL-Kompilierung verwendet, um Abfragen zu kompilieren und so basierend auf HBase Anwendungen mit geringer Wartezeit zu erstellen. Phoenix fügt Coprozessoren hinzu, um das Ausführen von per Client bereitgestelltem Code im Adressraum des Servers zu unterstützen und den Code auszuführen, der den Daten beigefügt ist. Auf diese Weise kann die Datenübertragung zwischen Client und Server reduziert werden.  Erstellen Sie zuerst Tabellen, und laden Sie anschließend Daten in diese Tabellen, um Phoenix in HDInsight zu verwenden.

## <a name="bulk-loading-with-apache-phoenix"></a>Massenladen mit Apache Phoenix

Es gibt mehrere Möglichkeiten, wie Daten in HBase gelangen, z.B. die Nutzung von Client-APIs, ein MapReduce-Auftrag mit TableOutputFormat oder das manuelle Eingeben der Daten per HBase-Shell. In Phoenix gibt es zwei Methoden zum Laden von CSV-Daten in Phoenix-Tabellen: ein Clientladetool mit dem Namen `psql` und ein MapReduce-basiertes Tool für das Massenladen.

Das Tool `psql` ist ein Singlethread-Tool und am besten zum Laden von Daten im MB- oder GB-Bereich geeignet. Alle CSV-Dateien, die geladen werden sollen, müssen über die Dateierweiterung „.csv“ verfügen.  Sie können in der Befehlszeile von `psql` auch SQL-Skriptdateien mit der Dateierweiterung „.sql“ angeben.

Das Massenladen mit MapReduce wird für deutlich größere Datenvolumen verwendet (normalerweise in der Produktion), da bei MapReduce mehrere Threads genutzt werden.

Stellen Sie vor Beginn des Datenladevorgangs sicher, dass Phoenix aktiviert ist und die Einstellungen für den Abfragetimeout wie erwartet festgelegt wurden.  Greifen Sie auf das [Apache Ambari](https://ambari.apache.org/)-Dashboard Ihres HDInsight-Clusters zu, und wählen Sie „HBase“ und dann die Konfigurationsregisterkarte aus.  Scrollen Sie nach unten, um zu überprüfen, ob Apache Phoenix wie hier dargestellt auf `enabled` festgelegt ist:

![Apache Phoenix – Einstellungen für HDInsight-Cluster](./media/apache-hbase-phoenix-psql/apache-ambari-phoenix.png)

### <a name="use-psql-to-bulk-load-tables"></a>Verwenden von `psql` für das Massenladen von Tabellen

1. Erstellen Sie eine Datei mit dem Namen `createCustomersTable.sql`, und kopieren Sie den folgenden Code in die Datei. Speichern und schließen Sie die Datei anschließend.

    ```sql
    CREATE TABLE Customers (
        ID varchar NOT NULL PRIMARY KEY,
        Name varchar,
        Income decimal,
        Age INTEGER,
        Country varchar);
    ```

1. Erstellen Sie eine Datei mit dem Namen `listCustomers.sql`, und kopieren Sie den folgenden Code in die Datei. Speichern und schließen Sie die Datei anschließend.

    ```sql
    SELECT * from Customers;
    ```

1. Erstellen Sie eine Datei mit dem Namen `customers.csv`, und kopieren Sie den folgenden Code in die Datei. Speichern und schließen Sie die Datei anschließend.

    ```txt
    1,Samantha,260000.0,18,US
    2,Sam,10000.5,56,US
    3,Anton,550150.0,42,Norway
    ```

1. Erstellen Sie eine Datei mit dem Namen `customers2.csv`, und kopieren Sie den folgenden Code in die Datei. Speichern und schließen Sie die Datei anschließend.

    ```txt
    4,Nicolle,180000.0,22,US
    5,Kate,210000.5,24,Canada
    6,Ben,45000.0,32,Poland
    ```

1. Öffnen Sie eine Eingabeaufforderung, und ändern Sie das Verzeichnis in den Speicherort der neu erstellten Dateien. Ersetzen Sie unten CLUSTERNAME durch den tatsächlichen Namen Ihres HBase-Clusters. Führen Sie dann den Code aus, um die Dateien auf den Hauptknoten des Clusters hochzuladen:

    ```cmd
    scp customers.csv customers2.csv createCustomersTable.sql listCustomers.sql sshuser@CLUSTERNAME-ssh.azurehdinsight.net:/tmp
    ```

1. Verwenden Sie einen [ssh-Befehl](../hdinsight-hadoop-linux-use-ssh-unix.md) zum Herstellen der Verbindung mit dem Cluster. Bearbeiten Sie den folgenden Befehl, indem Sie CLUSTERNAME durch den Namen Ihres Clusters ersetzen, und geben Sie den Befehl dann ein:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. Ändern Sie in Ihrer SSH-Sitzung das Verzeichnis in den Speicherort des **psql**-Tools. Führen Sie den folgenden Befehl aus:

    ```bash
    cd /usr/hdp/current/phoenix-client/bin
    ```

1. Führen Sie ein Massenladen der Daten aus. Mit dem folgenden Code wird zuerst die Tabelle **Customers** erstellt, und dann werden die Daten hochgeladen.

    ```bash
    python psql.py /tmp/createCustomersTable.sql /tmp/customers.csv
    ```

    Nachdem der `psql`-Vorgang abgeschlossen wurde, sollte eine Meldung ähnlich der folgenden angezeigt werden:

    ```output
    csv columns from database.
    CSV Upsert complete. 3 rows upserted
    Time: 0.081 sec(s)
    ```

1. Sie können `psql` weiterhin verwenden, um den Inhalt der Tabelle „Customers“ anzuzeigen. Führen Sie den folgenden Code aus:

    ```bash
    python psql.py /tmp/listCustomers.sql
    ```

    Alternativ können Sie [HBase-Shell](./query-hbase-with-hbase-shell.md) oder [Apache Zeppelin](./apache-hbase-phoenix-zeppelin.md) verwenden, um die Daten abzufragen.

1. Laden Sie weitere Dateien hoch. Da nun die Tabelle bereits vorhanden ist, gibt der Befehl die Tabelle an. Führen Sie den folgenden Befehl aus:

    ```bash
    python psql.py -t CUSTOMERS /tmp/customers2.csv
    ```

## <a name="use-mapreduce-to-bulk-load-tables"></a>Verwenden von MapReduce für das Massenladen von Tabellen

Verwenden Sie das MapReduce-Ladetool, um für die Ladevorgänge im Cluster einen höheren Durchsatz zu erzielen. Mit diesem Ladeprogramm werden zunächst alle Daten in HFiles konvertiert, und anschließend werden die erstellten HFiles für HBase bereitgestellt.

1. In diesem Abschnitt wird die SSH-Sitzung mit den zuvor erstellten Objekten fortgesetzt. Erstellen Sie bei Bedarf die Tabelle **Customers** und die Datei **customers.csv** anhand der oben beschriebenen Schritte. Stellen Sie ggf. die SSH-Verbindung erneut her.

1. Kürzen Sie den Inhalt der Tabelle **Customers**. Führen Sie in Ihrer geöffneten SSH-Sitzung die folgenden Befehle aus:

    ```bash
    hbase shell
    truncate 'CUSTOMERS'
    exit
    ```

1. Kopieren Sie die Datei `customers.csv` von Ihrem Hauptknoten in Azure Storage.

    ```bash
    hdfs dfs -put /tmp/customers.csv wasbs:///tmp/customers.csv
    ```

1. Wechseln Sie in das Ausführungsverzeichnis für den MapReduce-Befehl für das Massenladen:

    ```bash
    cd /usr/hdp/current/phoenix-client
    ```

1. Starten Sie das MapReduce-CSV-Ladeprogramm, indem Sie den Befehl `hadoop` mit dem JAR-Archiv des Phoenix-Clients verwenden:

    ```bash
    HADOOP_CLASSPATH=/usr/hdp/current/hbase-client/lib/hbase-protocol.jar:/etc/hbase/conf hadoop jar phoenix-client.jar org.apache.phoenix.mapreduce.CsvBulkLoadTool --table Customers --input /tmp/customers.csv
    ```

    Nachdem der Upload abgeschlossen ist, sollte eine Meldung ähnlich der folgenden angezeigt werden:

    ```output
    19/12/18 18:30:57 INFO client.ConnectionManager$HConnectionImplementation: Closing master protocol: MasterService
    19/12/18 18:30:57 INFO client.ConnectionManager$HConnectionImplementation: Closing zookeeper sessionid=0x26f15dcceff02c3
    19/12/18 18:30:57 INFO zookeeper.ZooKeeper: Session: 0x26f15dcceff02c3 closed
    19/12/18 18:30:57 INFO zookeeper.ClientCnxn: EventThread shut down
    19/12/18 18:30:57 INFO mapreduce.AbstractBulkLoadTool: Incremental load complete for table=CUSTOMERS
    19/12/18 18:30:57 INFO mapreduce.AbstractBulkLoadTool: Removing output directory /tmp/50254426-aba6-400e-88eb-8086d3dddb6
    ```

1. Um MapReduce mit Azure Data Lake Storage zu verwenden, suchen Sie das Stammverzeichnis von Data Lake Storage. Es handelt sich dabei um den `hbase.rootdir`-Wert in `hbase-site.xml`. Im folgenden Befehl lautet das Data Lake Storage-Stammverzeichnis `adl://hdinsightconf1.azuredatalakestore.net:443/hbase1`. Geben Sie in diesem Befehl die Data Lake Storage-Eingabe- und -Ausgabeordner als Parameter an:

    ```bash
    cd /usr/hdp/current/phoenix-client

    $HADOOP_CLASSPATH=$(hbase mapredcp):/etc/hbase/conf hadoop jar /usr/hdp/2.4.2.0-258/phoenix/phoenix-4.4.0.2.4.2.0-258-client.jar

    org.apache.phoenix.mapreduce.CsvBulkLoadTool --table Customers --input adl://hdinsightconf1.azuredatalakestore.net:443/hbase1/data/hbase/temp/input/customers.csv –zookeeper ZookeeperQuorum:2181:/hbase-unsecure --output  adl://hdinsightconf1.azuredatalakestore.net:443/hbase1/data/hbase/output1
    ```

1. Zum Abfragen und Anzeigen der Daten können Sie **psql** wie zuvor beschrieben verwenden. Sie können auch [HBase-Shell](./query-hbase-with-hbase-shell.md) oder [Apache Zeppelin](./apache-hbase-phoenix-zeppelin.md) verwenden.

## <a name="recommendations"></a>Empfehlungen

* Verwenden Sie für die Eingabe- und Ausgabeordner das gleiche Speichermedium, entweder Azure Storage (WASB) oder Azure Data Lake Storage (ADL). Um Daten aus Azure Storage an Data Lake Storage zu übertragen, können Sie den Befehl `distcp` verwenden:

    ```bash
    hadoop distcp wasb://@.blob.core.windows.net/example/data/gutenberg adl://.azuredatalakestore.net:443/myfolder
    ```

* Verwenden Sie größere Workerknoten. Bei den Zuordnungsprozessen des MapReduce-Vorgangs für das Massenkopieren werden große Mengen von temporären Ausgaben erzeugt, die den verfügbaren Speicherplatz (nicht DFS) belegen. Verwenden Sie beim Massenladen von großen Datenmengen eine höhere Zahl von Workerknoten und außerdem größere Workerknoten. Die Anzahl von Workerknoten, die Sie für Ihren Cluster zuordnen, hat eine direkte Auswirkung auf die Verarbeitungsgeschwindigkeit.

* Unterteilen Sie Eingabedateien in Blöcke mit einer Größe von ca. 10 GB. Da das Massenladen ein speicherintensiver Vorgang ist, führt das Aufteilen der Eingabedateien in mehrere Blöcke zu einer besseren Leistung.

* Vermeiden Sie Regionsserver-Hotspots. Falls sich Ihr Zeilenschlüssel monoton erhöht, kommt es durch sequenzielle HBase-Schreibvorgänge ggf. zu Regionsserver-Hotspotting. Durch das so genannte *Salting* des Zeilenschlüssels wird die Anzahl von sequenziellen Schreibvorgängen reduziert. In Phoenix besteht die Möglichkeit, den Zeilenschlüssel auf transparente Weise mit einem Salting-Byte für eine bestimmte Tabelle zu versehen, wie unten angegeben.

## <a name="next-steps"></a>Nächste Schritte

* [Bulk Data Loading with Apache Phoenix](https://phoenix.apache.org/bulk_dataload.html) (Massenladen von Daten mit Apache Phoenix)
* [Verwenden von Apache Phoenix mit Linux-basierten Apache HBase-Clustern in HDInsight](../hbase/apache-hbase-query-with-phoenix.md)
* [Salted Tables](https://phoenix.apache.org/salted.html) (Tabellen mit Salting)
* [Apache Phoenix Grammar](https://phoenix.apache.org/language/index.html)
