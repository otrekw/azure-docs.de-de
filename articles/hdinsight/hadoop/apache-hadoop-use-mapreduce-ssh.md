---
title: MapReduce und SSH-Verbindung mit Apache Hadoop in HDInsight – Azure HDInsight
description: Erfahren Sie, wie MapReduce-Aufträge mithilfe von SSH mit Apache Hadoop in HDInsight ausgeführt werden.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive
ms.date: 01/10/2020
ms.openlocfilehash: 2736d0cfe514252e36ba6d7e0d71efe09da15aae
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "86076299"
---
# <a name="use-mapreduce-with-apache-hadoop-on-hdinsight-with-ssh"></a>Verwenden von MapReduce mit Apache Hadoop in HDInsight über SSH

[!INCLUDE [mapreduce-selector](../../../includes/hdinsight-selector-use-mapreduce.md)]

In diesem Artikel wird erläutert, wie MapReduce-Jobs aus einer Secure Shell-Verbindung (SSH) an HDInsight übermittelt werden können.

> [!NOTE]
> Falls Sie bereits mit der Verwendung von Linux-basierten Apache Hadoop-Servern vertraut sind, aber noch keine Erfahrung mit HDInsight haben, finden Sie weitere Informationen unter [Tipps zu Linux-basiertem HDInsight](../hdinsight-hadoop-linux-information.md).

## <a name="prerequisites"></a>Voraussetzungen

Ein Apache Hadoop-Cluster in HDInsight. Siehe [Erstellen von Apache Hadoop-Clustern im Azure-Portal](../hdinsight-hadoop-create-linux-clusters-portal.md).

## <a name="use-hadoop-commands"></a>Verwenden von Hadoop-Befehlen

1. Verwenden Sie einen [ssh-Befehl](../hdinsight-hadoop-linux-use-ssh-unix.md) zum Herstellen der Verbindung mit dem Cluster. Bearbeiten Sie den folgenden Befehl, indem Sie CLUSTERNAME durch den Namen Ihres Clusters ersetzen, und geben Sie den Befehl dann ein:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. Nachdem die Verbindung mit dem HDInsight-Cluster hergestellt wurde, verwenden Sie den folgenden Befehl, um einen MapReduce-Auftrag zu starten:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/WordCountOutput
    ```

    Dieser Befehl ruft die `wordcount`-Klasse auf, die sich in der Datei `hadoop-mapreduce-examples.jar` befindet. Der Befehl verwendet das Dokument `/example/data/gutenberg/davinci.txt` als Eingabe, und die Ausgabe wird in `/example/data/WordCountOutput` gespeichert.

    > [!NOTE]
    > Weitere Informationen zu diesem MapReduce-Auftrag und die Beispieldaten finden Sie unter [Verwenden von MapReduce mit Apache Hadoop in HDInsight](hdinsight-use-mapreduce.md).

    Der Auftrag gibt während der Verarbeitung Details aus und gibt ähnliche Informationen wie den folgenden Text zurück, wenn der Auftrag abgeschlossen ist:

    ```output
    File Input Format Counters
    Bytes Read=1395666
    File Output Format Counters
    Bytes Written=337623
    ```

1. Wenn der Auftrag abgeschlossen ist, verwenden Sie den folgenden Befehl zum Auflisten der Ausgabedateien:

    ```bash
    hdfs dfs -ls /example/data/WordCountOutput
    ```

    Dieser Befehl zeigt die beiden Dateien, `_SUCCESS` und `part-r-00000` an. Die Datei `part-r-00000` enthält die Ausgabe für diesen Auftrag.

    > [!NOTE]  
    > Einige MapReduce-Aufträge teilen die Ergebnisse möglicherweise auf mehrere **part-r-#####** -Dateien auf. Verwenden Sie in diesem Fall das Suffix "#####", um die Reihenfolge der Dateien anzugeben.

1. Verwenden Sie den folgenden Befehl, um die Ausgabe anzuzeigen:

    ```bash
    hdfs dfs -cat /example/data/WordCountOutput/part-r-00000
    ```

    Dieser Befehl zeigt eine Liste mit in der Datei **wasbs://example/data/gutenberg/davinci.txt** enthaltenen Wörtern und die Häufigkeit ihres jeweiligen Vorkommens an. Im folgenden Text finden Sie ein Beispiel für die in der Datei enthaltenen Daten:

    ```output
    wreathed        3
    wreathing       1
    wreaths         1
    wrecked         3
    wrenching       1
    wretched        6
    wriggling       1
    ```

## <a name="next-steps"></a>Nächste Schritte

Wie Sie sehen können, bieten Hadoop-Befehle eine einfache Möglichkeit zum Ausführen von MapReduce-Aufträgen in einem HDInsight-Cluster und dem anschließenden Anzeigen der Auftragsausgabe. Informationen zu anderen Möglichkeiten, wie Sie mit Hadoop in HDInsight arbeiten können:

* [Verwenden von MapReduce mit Hadoop in HDInsight](hdinsight-use-mapreduce.md)
* [Verwenden von Apache Hive mit Apache Hadoop in HDInsight](hdinsight-use-hive.md)
