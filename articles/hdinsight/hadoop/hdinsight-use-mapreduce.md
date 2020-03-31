---
title: MapReduce mit Apache Hadoop in HDInsight
description: Erfahren Sie, wie Apache MapReduce-Aufträge in Apache Hadoop in HDInsight-Clustern ausgeführt werden.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 12/06/2019
ms.openlocfilehash: 238f37cc1e8cbc8d3260693181d6e54ac5f592f7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "75435723"
---
# <a name="use-mapreduce-in-apache-hadoop-on-hdinsight"></a>Verwenden von MapReduce mit Apache Hadoop in HDInsight

Erfahren Sie, wie MapReduce-Aufträge in HDInsight-Clustern ausgeführt werden.

## <a name="example-data"></a>Beispieldaten

HDInsight enthält verschiedene Beispieldatasets, die in den Verzeichnissen `/example/data` und `/HdiSamples` gespeichert sind. Diese Verzeichnisse befinden sich im Standardspeicher für den Cluster. In diesem Artikel wird die Datei `/example/data/gutenberg/davinci.txt` verwendet. Diese Datei enthält die Notizbücher von Leonardo da Vinci.

## <a name="example-mapreduce"></a>MapReduce-Beispiel

Eine MapReduce-Beispielanwendung zum Zählen von Wörtern ist in Ihrem HDInsight-Cluster enthalten. Dieses Beispiel befindet sich unter `/example/jars/hadoop-mapreduce-examples.jar` im Standardspeicher für Ihren Cluster.

Der folgende Java-Code ist die Quelle der in der Datei `hadoop-mapreduce-examples.jar` enthaltenen MapReduce-Anwendung:

```java
package org.apache.hadoop.examples;

import java.io.IOException;
import java.util.StringTokenizer;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.Mapper;
import org.apache.hadoop.mapreduce.Reducer;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
import org.apache.hadoop.util.GenericOptionsParser;

public class WordCount {

    public static class TokenizerMapper
        extends Mapper<Object, Text, Text, IntWritable>{

    private final static IntWritable one = new IntWritable(1);
    private Text word = new Text();

    public void map(Object key, Text value, Context context
                    ) throws IOException, InterruptedException {
        StringTokenizer itr = new StringTokenizer(value.toString());
        while (itr.hasMoreTokens()) {
        word.set(itr.nextToken());
        context.write(word, one);
        }
    }
    }

    public static class IntSumReducer
        extends Reducer<Text,IntWritable,Text,IntWritable> {
    private IntWritable result = new IntWritable();

    public void reduce(Text key, Iterable<IntWritable> values,
                        Context context
                        ) throws IOException, InterruptedException {
        int sum = 0;
        for (IntWritable val : values) {
        sum += val.get();
        }
        result.set(sum);
        context.write(key, result);
    }
    }

    public static void main(String[] args) throws Exception {
    Configuration conf = new Configuration();
    String[] otherArgs = new GenericOptionsParser(conf, args).getRemainingArgs();
    if (otherArgs.length != 2) {
        System.err.println("Usage: wordcount <in> <out>");
        System.exit(2);
    }
    Job job = new Job(conf, "word count");
    job.setJarByClass(WordCount.class);
    job.setMapperClass(TokenizerMapper.class);
    job.setCombinerClass(IntSumReducer.class);
    job.setReducerClass(IntSumReducer.class);
    job.setOutputKeyClass(Text.class);
    job.setOutputValueClass(IntWritable.class);
    FileInputFormat.addInputPath(job, new Path(otherArgs[0]));
    FileOutputFormat.setOutputPath(job, new Path(otherArgs[1]));
    System.exit(job.waitForCompletion(true) ? 0 : 1);
    }
}
```

Anweisungen zum Schreiben eigener MapReduce-Anwendungen finden Sie unter [Entwickeln von Java MapReduce-Anwendungen für HDInsight](apache-hadoop-develop-deploy-java-mapreduce-linux.md).

## <a name="run-the-mapreduce"></a>Ausführen von MapReduce

HDInsight kann HiveQL-Aufträge mithilfe verschiedener Methoden ausführen. Die folgende Tabelle hilft Ihnen bei der Entscheidung, welche Methode für Sie geeignet ist. Folgen Sie anschließend dem Link für eine exemplarische Vorgehensweise.

| **Option** | **Zweck** |  ...von diesem **Clusterbetriebssystem** |
|:--- |:--- |:--- |:--- |
| [SSH](apache-hadoop-use-mapreduce-ssh.md) |Verwenden des Hadoop-Befehls über **SSH** |Linux, Unix, Mac OS X oder Windows |
| [Curl](apache-hadoop-use-mapreduce-curl.md) |Remoteübermittlung des Auftrags mit **REST** |Linux, Unix, Mac OS X oder Windows |
| [Windows PowerShell](apache-hadoop-use-mapreduce-powershell.md) |Remoteübermittlung des Auftrags mit **Windows PowerShell**  |Windows |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Arbeiten mit Daten in HDInsight finden Sie in den folgenden Artikeln:

* [Entwickeln von Java MapReduce-Programmen für HDInsight](apache-hadoop-develop-deploy-java-mapreduce-linux.md)

* [Verwenden von Apache Hive mit HDInsight](./hdinsight-use-hive.md)
