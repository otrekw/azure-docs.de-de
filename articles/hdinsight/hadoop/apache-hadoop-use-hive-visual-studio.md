---
title: Apache Hive und Data Lake-Tools für Visual Studio – Azure HDInsight
description: Erfahren Sie, wie Sie die Data Lake Tools für Visual Studio verwenden, um Apache Hive-Abfragen mit Apache Hadoop in Azure HDInsight auszuführen.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 11/27/2019
ms.openlocfilehash: 27ab13481525819eb1435f4c9ac256a21acd21fb
ms.sourcegitcommit: 48b7a50fc2d19c7382916cb2f591507b1c784ee5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/02/2019
ms.locfileid: "74687803"
---
# <a name="run-apache-hive-queries-using-the-data-lake-tools-for-visual-studio"></a>Ausführen von Apache Hive-Abfragen mit Data Lake-Tools für Visual Studio

Erfahren Sie, wie Sie die Data Lake Tools für Visual Studio für Abfragen in Apache Hive verwenden. Mithilfe der Data Lake Tools können Sie Hive-Abfragen an Apache Hadoop in Azure HDInsight ganz einfach erstellen, übermitteln und überwachen.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Apache Hadoop-Cluster in HDInsight. Informationen zum Erstellen dieses Elements finden Sie unter [Erstellen eines Apache Hadoop-Clusters in Azure HDInsight mit einer Resource Manager-Vorlage](./apache-hadoop-linux-tutorial-get-started.md).

* [Visual Studio](https://visualstudio.microsoft.com/vs/) Für die Schritte in diesem Artikel wird Visual Studio 2019 verwendet.

* HDInsight-Tools für Visual Studio oder Azure Data Lake-Tools für Visual Studio. Informationen zum Installieren und Konfigurieren der Tools finden Sie unter [Installieren von Data Lake-Tools für Visual Studio](apache-hadoop-visual-studio-tools-get-started.md#install-data-lake-tools-for-visual-studio).

## <a name="run-apache-hive-queries-using-the-visual-studio"></a>Ausführen von Apache Hive-Abfragen mit Visual Studio

Zum Erstellen und Ausführen von Hive-Abfragen stehen Ihnen zwei Möglichkeiten zur Auswahl:

* Erstellen von Ad-hoc-Abfragen.
* Erstellen einer Hive-Anwendung.

### <a name="create-an-ad-hoc-hive-query"></a>Erstellen einer Ad-hoc-Hive-Abfrage.

Ad-hoc-Abfragen können entweder im Modus **Batch** oder **Interaktiv** ausgeführt werden.

1. Öffnen Sie **Visual Studio**, und wählen Sie **Ohne Code fortfahren** aus.

2. Klicken Sie im **Server-Explorer** mit der rechten Maustaste auf **Azure**, klicken Sie auf **Verbindung mit Microsoft Azure-Abonnement herstellen**, und melden Sie sich an.

3. Erweitern Sie **HDInsight**, klicken Sie mit der rechten Maustaste auf den Cluster, in dem Sie die Abfrage ausführen möchten, und wählen Sie dann **Write a Hive Query** (Hive-Abfrage schreiben) aus.

4. Geben Sie die folgende Hive-Abfrage ein:

    ```hql
    SELECT * FROM hivesampletable;
    ```

5. Wählen Sie **Execute**(Ausführen). Der Ausführungsmodus ist standardmäßig **Interaktiv**.

    ![Ausführen einer interaktiver Hive-Abfrage, Visual Studio](./media/apache-hadoop-use-hive-visual-studio/vs-execute-hive-query.png)

6. Um dieselbe Abfrage im Modus **Batch** auszuführen, wechseln Sie in der Dropdownliste von **Interaktiv** zu **Batch**. Die Schaltfläche für die Ausführung wechselt von **Ausführen** zu **Übermitteln**.

    ![Übermitteln einer Hive-Batchabfrage, Visual Studio](./media/apache-hadoop-use-hive-visual-studio/visual-studio-batch-query.png)

    Der Hive-Editor unterstützt IntelliSense. Data Lake Tools für Visual Studio unterstützt das Laden von Remotemetadaten, wenn Sie Ihr Hive-Skript bearbeiten. Wenn Sie beispielsweise `SELECT * FROM` eingeben, listet IntelliSense alle vorgeschlagenen Tabellennamen auf. Wird ein Tabellenname angegeben, listet IntelliSense die Spaltennamen auf. Die Tools unterstützen die meisten Hive-DML-Anweisungen, Unterabfragen und integrierte UDFs. IntelliSense schlägt nur die Metadaten des Clusters vor, der auf der HDInsight-Symbolleiste ausgewählt ist.

7. Wählen Sie auf der Symbolleiste für Abfragen (der Bereich unterhalb der Registerkarte „Abfrage“ und über dem Abfragetext) entweder **Übermitteln** oder den Pulldownmenü-Pfeil neben **Übermitteln** und dann aus der Pulldownliste **Erweitert** aus. Wenn Sie die zweite Option auswählen,

8. Konfigurieren Sie den **Auftragsnamen**, **Argumente**, **zusätzliche Konfigurationen** und das **Statusverzeichnis** im Dialogfeld **Skript übermitteln**, wenn Sie die Option für erweitertes Senden auswählen. Klicken Sie dann auf **Senden**.

    ![Dialogfeld „Skript übermitteln“, HDInsight Hadoop Hive-Abfrage](./media/apache-hadoop-use-hive-visual-studio/vs-tools-submit-jobs-advanced.png)

### <a name="create-a-hive-application"></a>Erstellen einer Hive-Anwendung

Führen Sie die folgenden Schritte aus, um eine Hive-Abfrage durch Erstellen einer Hive-Anwendung auszuführen:

1. Öffnen Sie **Visual Studio**.

2. Wählen Sie im Fenster **Start** die Option **Neues Projekt erstellen** aus.

3. Geben Sie im Fenster **Neues Projekt erstellen** im Feld **Nach Vorlagen suchen** die Zeichenfolge *Hive* ein. Wählen Sie dann **Hive Application** (Hive-Anwendung) und anschließend **Weiter** aus.

4. Geben Sie im Fenster **Neues Projekt konfigurieren** einen **Projektnamen** ein, wählen Sie den **Speicherort** für das neue Projekt aus, oder erstellen Sie ihn, und wählen Sie dann **Erstellen** aus.

5. Öffnen Sie die Datei **Script.hql** , die mit diesem Projekt erstellt wird, und fügen Sie dann die folgenden HiveQL-Anweisungen ein:

    ```hql
    set hive.execution.engine=tez;
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION '/example/data/';
    SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND  INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;
    ```

    Diese Anweisungen führen die folgenden Aktionen aus:

    * `DROP TABLE`: Löscht die Tabelle, falls vorhanden.

    * `CREATE EXTERNAL TABLE`: Erstellt eine neue „externe“ Tabelle in Hive. Externe Tabellen speichern nur die Tabellendefinition in Hive. (Die Daten verbleiben an ihrem ursprünglichen Speicherort.)

        > [!NOTE]  
        > Externe Tabellen sollten Sie verwenden, wenn Sie erwarten, dass die zugrunde liegenden Daten aus einer externen Quelle aktualisiert werden, wie z.B. einem MapReduce-Auftrag oder einem Azure-Dienst.
        >
        > Durch das Löschen einer externen Tabelle werden **nicht** die Daten, sondern nur die Tabellendefinitionen gelöscht.

    * `ROW FORMAT`: Teilt Hive mit, wie die Daten formatiert werden. In diesem Fall werden die Felder in den einzelnen Protokollen durch Leerzeichen getrennt.

    * `STORED AS TEXTFILE LOCATION`: Teilt Hive mit, dass die Daten im Verzeichnis *example/data* als Text gespeichert sind.

    * `SELECT`: Wählt die Anzahl aller Zeilen aus, bei denen die Spalte `t4` den Wert `[ERROR]` enthält. Mit dieser Anweisung wird der Wert `3` zurückgegeben, da dieser Wert in drei Zeilen enthalten ist.

    * `INPUT__FILE__NAME LIKE '%.log'`: Teilt Hive mit, dass nur Daten aus Dateien mit der Erweiterung „.log“ zurückgeben werden sollen. Diese Klausel schränkt die Suche auf die Datei *sample.log* ein, die die Daten enthält.

6. Wählen Sie auf der Symbolleiste der Abfragedatei (die der Symbolleiste der Ad-hoc-Abfrage ähnelt) den HDInsight-Cluster aus, den Sie für diese Abfrage verwenden möchten. Ändern Sie dann **Interaktiv** in **Batch** (falls erforderlich), und wählen Sie **Übermitteln** aus, um die Anweisungen als Hive-Auftrag auszuführen.

   Die **Hive-Auftragszusammenfassung** wird mit Informationen zum aktiven Auftrag angezeigt. Verwenden Sie den Link **Aktualisieren**, um die Auftragsinformationen zu aktualisieren, bis der **Auftragsstatus** zu **Abgeschlossen** wechselt.

   ![Abgeschlossene Hive-Auftragszusammenfassung, Hive-Anwendung, Visual Studio](./media/apache-hadoop-use-hive-visual-studio/hdinsight-job-summary.png)

7. Wählen Sie **Auftragsausgabe** aus, um die Ausgabe dieses Auftrags anzuzeigen. `[ERROR] 3` wird angezeigt – dies ist der Wert, der von dieser Abfrage zurückgegeben wird.

### <a name="additional-example"></a>Zusätzliches Beispiel

Das folgende Beispiel basiert auf der `log4jLogs`-Tabelle, die in der vorherigen Prozedur, [Erstellen einer Hive-Anwendungs](#create-a-hive-application) erstellt wurde.

1. Klicken Sie in **Server-Explorer** mit der rechten Maustaste auf Ihren Cluster, und wählen Sie **Hive-Abfrage schreiben** aus.

2. Geben Sie die folgende Hive-Abfrage ein:

    ```hql
    set hive.execution.engine=tez;
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';
    ```

    Diese Anweisungen führen die folgenden Aktionen aus:

    * `CREATE TABLE IF NOT EXISTS`: Erstellt eine Tabelle, sofern diese noch nicht vorhanden ist. Da das `EXTERNAL`-Schlüsselwort nicht verwendet wird, erstellt diese Anweisung eine interne Tabelle. Interne Tabellen werden im Hive-Data Warehouse gespeichert und von Hive verwaltet.

        > [!NOTE]  
        > Anders als bei `EXTERNAL` Tabellen werden beim Löschen von internen Tabellen auch die zugrunde liegenden Daten gelöscht.

    * `STORED AS ORC`: Speichert die Daten im ORC-Format (*Optimized Row Columnar*). ORC ist ein stark optimiertes und effizientes Format zum Speichern von Hive-Daten.

    * `INSERT OVERWRITE ... SELECT`: Wählt die Zeilen aus der `log4jLogs` Tabelle, die `[ERROR]` enthalten, und fügt dann die Daten in die `errorLogs` Tabelle ein.

3. Ändern Sie **Interaktiv** falls erforderlich in **Batch**, und wählen Sie dann **Übermitteln** aus.

4. Um zu überprüfen, ob der Auftrag die Tabelle erstellt hat, wechseln Sie zum **Server-Explorer**, und erweitern Sie **Azure** > **HDInsight**. Erweitern Sie den HDInsight-Cluster, und erweitern Sie dann **Hive-Datenbanken** > **Standard**. Die Tabellen **errorLogs** und **log4jLogs** werden aufgelistet.

## <a name="next-steps"></a>Nächste Schritte

Wie Sie sehen, können Sie mit den HDInsight-Tools für Visual Studio mühelos mit Hive-Abfragen in HDInsight arbeiten.

* Allgemeinere Informationen zu Hive in HDInsight finden Sie unter [Was sind Apache Hive und HiveQL in Azure HDInsight?](hdinsight-use-hive.md).

* Informationen zu anderen Methoden zur Verwendung von Hadoop in HDInsight finden Sie unter [Verwenden von MapReduce mit Apache Hadoop in HDInsight](hdinsight-use-mapreduce.md).

* Weitere Informationen zu den HDInsight-Tools für Visual Studio finden Sie unter [Herstellen einer Verbindung mit Azure HDInsight und Ausführen von Apache Hive-Abfragen mithilfe von Data Lake Tools für Visual Studio](apache-hadoop-visual-studio-tools-get-started.md).
