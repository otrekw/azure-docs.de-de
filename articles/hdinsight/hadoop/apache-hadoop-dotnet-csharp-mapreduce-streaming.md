---
title: Verwenden von C# mit MapReduce für Hadoop in HDInsight – Azure
description: Erfahren Sie, wie Sie C# verwenden, um MapReduce-Lösungen mit Apache Hadoop in Azure HDInsight zu erstellen.
author: hrasheed-msft
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: conceptual
ms.date: 10/17/2019
ms.author: hrasheed
ms.openlocfilehash: 1cdf029d296bd6ff11b6531cd47dc6a7fd3163c3
ms.sourcegitcommit: a10074461cf112a00fec7e14ba700435173cd3ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2019
ms.locfileid: "73930267"
---
# <a name="use-c-with-mapreduce-streaming-on-apache-hadoop-in-hdinsight"></a>Verwenden von C# mit MapReduce-Streaming auf Apache Hadoop in HDInsight

Erfahren Sie, wie Sie C# verwenden, um eine MapReduce-Lösung in HDInsight zu erstellen.

> [!IMPORTANT]
> Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Apache Hadoop-Komponenten in HDInsight](../hdinsight-component-versioning.md).

Mit Apache Hadoop-Streaming können Sie MapReduce-Aufträge über ein Skript oder eine ausführbare Datei ausführen. In diesem Beispiel wird .NET verwendet, um die Mapper- und Reducer-Prozesse für eine Lösung zum Zählen von Wörtern zu implementieren.

## <a name="net-on-hdinsight"></a>.NET in HDInsight

*Linux-basierte HDInsight*-Cluster nutzen [Mono (https://mono-project.com)](https://mono-project.com) für die Ausführung von .NET-Anwendungen. Mono-Version 4.2.1 ist in HDInsight Version 3.6 enthalten. Weitere Informationen über die in HDInsight enthaltene Version von Mono finden Sie unter [Verfügbare Apache Hadoop-Komponenten in verschiedenen Versionen von HDInsight](../hdinsight-component-versioning.md#apache-hadoop-components-available-with-different-hdinsight-versions). 

Weitere Informationen zur Kompatibilität von Mono mit .NET Framework-Versionen finden Sie unter [Mono compatibility](https://www.mono-project.com/docs/about-mono/compatibility/) (Kompatibilität von Mono).

## <a name="how-hadoop-streaming-works"></a>Funktionsweise von Hadoop-Datenströmen

Der in diesem Artikel verwendete grundlegende Prozess für das Streaming sieht wie folgt aus:

1. Hadoop übergibt Daten an den Mapper-Prozess (in diesem Beispiel *mapper.exe*) auf STDIN.
2. Der Mapper verarbeitet die Daten und gibt durch Tabstoppzeichen getrennte Schlüssel/Wert-Paare an STDOUT aus.
3. Die Ausgabe wird von Hadoop gelesen und dann an den Reducer-Prozess (in diesem Beispiel *reducer.exe*) auf STDIN übergeben.
4. Der Reducer-Prozess liest die durch Tabstoppzeichen getrennten Schlüssel/Wert-Paare, verarbeitet die Daten und gibt dann das Ergebnis als durch Tabstoppzeichen getrennte Schlüssel/Wert-Paare auf STDOUT aus.
5. Die Ausgabe wird von Hadoop gelesen und in das Ausgabeverzeichnis geschrieben.

Weitere Informationen zum Streaming finden Sie unter [Hadoop-Streaming](https://hadoop.apache.org/docs/r2.7.1/hadoop-streaming/HadoopStreaming.html).

## <a name="prerequisites"></a>Voraussetzungen

* Visual Studio.

* Gute Kenntnisse im Schreiben und Erstellen von C#-Code für .NET Framework 4.5.

* Eine Möglichkeit zum Hochladen von EXE-Dateien in den Cluster. In den Schritten in diesem Artikel werden die Data Lake-Tools für Visual Studio zum Hochladen der Dateien in primären Speicher für den Cluster verwendet.

* Azure PowerShell oder ein SSH-Client (Secure Shell).

* Hadoop für einen HDInsight-Cluster. Weitere Informationen zum Erstellen eines Clusters finden Sie unter [Erstellen eines HDInsight-Clusters](../hdinsight-hadoop-provision-linux-clusters.md).

## <a name="create-the-mapper"></a>Erstellen des Mappers

Erstellen Sie in Visual Studio eine neue .NET Framework-Konsolenanwendung mit dem Namen *mapper*. Verwenden Sie für die Anwendung den folgenden Code:

```csharp
using System;
using System.Text.RegularExpressions;

namespace mapper
{
    class Program
    {
        static void Main(string[] args)
        {
            string line;
            //Hadoop passes data to the mapper on STDIN
            while((line = Console.ReadLine()) != null)
            {
                // We only want words, so strip out punctuation, numbers, etc.
                var onlyText = Regex.Replace(line, @"\.|;|:|,|[0-9]|'", "");
                // Split at whitespace.
                var words = Regex.Matches(onlyText, @"[\w]+");
                // Loop over the words
                foreach(var word in words)
                {
                    //Emit tab-delimited key/value pairs.
                    //In this case, a word and a count of 1.
                    Console.WriteLine("{0}\t1",word);
                }
            }
        }
    }
}
```

Nachdem Sie die Anwendung erstellt haben, führen Sie den Buildvorgang durch, um die Datei */bin/Debug/mapper.exe* im Projektverzeichnis zu erzeugen.

## <a name="create-the-reducer"></a>Erstellen des Reducers

Erstellen Sie in Visual Studio eine neue .NET Framework-Konsolenanwendung mit dem Namen *reducer*. Verwenden Sie für die Anwendung den folgenden Code:

```csharp
using System;
using System.Collections.Generic;

namespace reducer
{
    class Program
    {
        static void Main(string[] args)
        {
            //Dictionary for holding a count of words
            Dictionary<string, int> words = new Dictionary<string, int>();

            string line;
            //Read from STDIN
            while ((line = Console.ReadLine()) != null)
            {
                // Data from Hadoop is tab-delimited key/value pairs
                var sArr = line.Split('\t');
                // Get the word
                string word = sArr[0];
                // Get the count
                int count = Convert.ToInt32(sArr[1]);

                //Do we already have a count for the word?
                if(words.ContainsKey(word))
                {
                    //If so, increment the count
                    words[word] += count;
                } else
                {
                    //Add the key to the collection
                    words.Add(word, count);
                }
            }
            //Finally, emit each word and count
            foreach (var word in words)
            {
                //Emit tab-delimited key/value pairs.
                //In this case, a word and a count of 1.
                Console.WriteLine("{0}\t{1}", word.Key, word.Value);
            }
        }
    }
}
```

Nachdem Sie die Anwendung erstellt haben, führen Sie den Buildvorgang durch, um die Datei */bin/Debug/reducer.exe* im Projektverzeichnis zu erzeugen.

## <a name="upload-to-storage"></a>Hochladen in den Speicher

Als nächstes müssen Sie die Anwendungen *mapper* und *reducer* in den HDInsight-Speicher hochladen.

1. Wählen Sie in Visual Studio **Ansicht** > **Server-Explorer** aus.

2. Erweitern Sie erst **Azure** und dann **HDInsight**.

3. Geben Sie bei Aufforderung die Anmeldeinformationen für Ihr Azure-Abonnement ein, und wählen Sie **Anmelden** aus.

4. Erweitern Sie den HDInsight-Cluster, in dem Sie diese Anwendung bereitstellen möchten. Ein Eintrag mit dem Text **(Standardspeicherkonto)** ist aufgeführt.

   ![Speicherkonto, HDInsight-Cluster, Server-Explorer, Visual Studio](./media/apache-hadoop-dotnet-csharp-mapreduce-streaming/hdinsight-storage-account.png)

   * Wenn der Eintrag **(Standardspeicherkonto)** erweitert werden kann, verwenden Sie ein **Azure Storage-Konto** als Standardspeicher für den Cluster. Um die Dateien im Standardspeicher für den Cluster anzuzeigen, erweitern Sie den Eintrag, und doppelklicken Sie dann auf **(Standardcontainer)** .

   * Wenn der Eintrag **(Standardspeicherkonto)** nicht erweitert werden kann, verwenden Sie **Azure Data Lake Storage** als Standardspeicher für den Cluster. Um die Dateien im Standardspeicher für den Cluster anzuzeigen, doppelklicken Sie auf den Eintrag **(Standardspeicherkonto)** .

5. Laden Sie die EXE-Dateien mithilfe einer der folgenden Methoden hoch:

    * Wenn Sie ein **Azure Storage-Konto** verwenden, wählen Sie das Symbol **Blob hochladen** aus. 

        ![HDInsight – Uploadsymbol für das Projekt „mapper“, Visual Studio](./media/apache-hadoop-dotnet-csharp-mapreduce-streaming/hdinsight-upload-icon.png)

        Wählen Sie im Dialogfeld **Neue Datei hochladen** unter **Dateiname** die Option **Durchsuchen** aus. Wechseln Sie im Dialogfeld **Blob hochladen** zum Ordner *bin\debug* dieses *mapper*-Projekts, und wählen Sie dann die Datei *mapper.exe* aus. Wählen Sie abschließend **Öffnen** und dann **OK** aus, um den Upload abzuschließen. 

    * Wenn Sie **Azure Data Lake Storage** verwenden, klicken Sie mit der rechten Maustaste auf einen leeren Bereich in der Dateiliste, und wählen Sie dann **Hochladen** aus. Wählen Sie abschließend die Datei *mapper.exe* und dann **Öffnen** aus.

    Sobald der Upload der Datei *mapper.exe* abgeschlossen ist, wiederholen Sie den Uploadvorgang für die Datei *reducer.exe*.

## <a name="run-a-job-using-an-ssh-session"></a>Ausführen eines Auftrags: Verwenden einer SSH-Sitzung

Im folgenden Verfahren wird beschrieben, wie Sie einen MapReduce-Auftrag über eine SSH-Sitzung ausführen:

1. Stellen Sie mithilfe von SSH eine Verbindung mit dem HDInsight-Cluster her. (Führen Sie zum Beispiel den Befehl `ssh sshuser@<clustername>-ssh.azurehdinsight.net` aus.) Weitere Informationen finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. Verwenden Sie zum Starten des MapReduce-Auftrags einen der folgenden Befehle:

   * Wenn der Standardspeicher **Azure Storage** ist:

        ```bash
        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar \
            -files wasb:///mapper.exe,wasb:///reducer.exe \
            -mapper mapper.exe \
            -reducer reducer.exe \
            -input /example/data/gutenberg/davinci.txt \
            -output /example/wordcountout
        ```

    * Wenn der Standardspeicher **Data Lake Storage Gen1**: ist:

        ```bash
        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar \
            -files adl:///mapper.exe,adl:///reducer.exe \
            -mapper mapper.exe \
            -reducer reducer.exe \
            -input /example/data/gutenberg/davinci.txt \
            -output /example/wordcountout
        ```

   * Wenn der Standardspeicher **Data Lake Storage Gen2**: ist:

        ```bash
        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar \
            -files abfs:///mapper.exe,abfs:///reducer.exe \
            -mapper mapper.exe \
            -reducer reducer.exe \
            -input /example/data/gutenberg/davinci.txt \
            -output /example/wordcountout
        ```

   In der folgenden Liste wird beschrieben, was die einzelnen Parameter und Optionen darstellen:

   * *hadoop-streaming.jar:* Gibt die JAR-Datei an, die die MapReduce-Streamingfunktionen enthält.
   * `-files`: Gibt die Dateien *mapper.exe* und *reducer.exe* für diesen Auftrag an. Die Protokolldeklaration `wasb:///`, `adl:///` oder `abfs:///` vor jeder Datei ist der Pfad zum Stamm des Standardspeichers für den Cluster.
   * `-mapper`: Gibt die Datei an, die den Mapper implementiert.
   * `-reducer`: Gibt die Datei an, die den Reducer implementiert.
   * `-input`: Gibt die Eingabedaten an.
   * `-output`: Gibt das Ausgabeverzeichnis an.

3. Wenn der MapReduce-Auftrag abgeschlossen ist, können Sie die Ergebnisse mit dem folgenden Befehl anzeigen:

   ```bash
   hdfs dfs -text /example/wordcountout/part-00000
   ```

   Der folgende Text ist ein Beispiel der von diesem Befehl zurückgegebenen Daten:

   ```output
   you     1128
   young   38
   younger 1
   youngest        1
   your    338
   yours   4
   yourself        34
   yourselves      3
   youth   17
   ```

## <a name="run-a-job-using-powershell"></a>Ausführen eines Auftrags: Verwenden von PowerShell

Verwenden Sie das folgende PowerShell-Skript zum Ausführen eines MapReduce-Auftrags und zum Herunterladen der Ergebnisse.

[!code-powershell[main](../../../powershell_scripts/hdinsight/use-csharp-mapreduce/use-csharp-mapreduce.ps1?range=5-87)]

Dieses Skript fordert Sie auf den Kontonamen und das Kennwort für die Clusteranmeldung sowie den HDInsight-Clusternamen einzugeben. Wenn der Auftrag abgeschlossen wurde, wird die Ausgabe in die Datei *output.txt* heruntergeladen. Der folgende Text ist ein Beispiel für die Daten in der Datei `output.txt`:

```output
you     1128
young   38
younger 1
youngest        1
your    338
yours   4
yourself        34
yourselves      3
youth   17
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Verwenden von MapReduce mit HDInsight finden Sie unter [Verwenden von MapReduce mit Apache Hadoop in HDInsight](hdinsight-use-mapreduce.md).

Informationen zur Verwendung von C# mit Hive und Pig finden Sie unter [Verwenden benutzerdefinierter C#-Funktionen mit Apache Hive- und Apache Pig-Streaming für Apache Hadoop in HDInsight](apache-hadoop-hive-pig-udf-dotnet-csharp.md).

Informationen zum Verwenden von C# mit Storm in HDInsight finden Sie unter [Entwickeln von C#-Topologien für Apache Storm mithilfe von Data Lake-Tools für Visual Studio](../storm/apache-storm-develop-csharp-visual-studio-topology.md).
