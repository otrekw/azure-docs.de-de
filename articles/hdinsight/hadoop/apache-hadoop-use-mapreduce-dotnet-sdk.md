---
title: Übermitteln von MapReduce-Aufträgen mit dem HDInsight .NET SDK – Azure
description: Erfahren Sie, wie Sie MapReduce-Jobs mit HDInsight .NET-SDK an Azure HDInsight Apache Hadoop übermitteln können.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 01/15/2020
ms.openlocfilehash: e50510f2420d69be37af584a2648a794e1561ee3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "76157048"
---
# <a name="run-mapreduce-jobs-using-hdinsight-net-sdk"></a>Ausführen von MapReduce-Jobs mit HDInsight .NET SDK

[!INCLUDE [mapreduce-selector](../../../includes/hdinsight-selector-use-mapreduce.md)]

Erfahren Sie, wie Sie MapReduce-Jobs mit HDInsight .NET SDK übermitteln können. HDInsight-Cluster weisen eine JAR-Datei mit einigen MapReduce-Beispielen auf. Die JAR-Datei ist `/example/jars/hadoop-mapreduce-examples.jar`.  Eines der Beispiele ist **Wordcount**. Sie entwickeln ein C#-Konsolenanwendungsprojekt zur Übermittlung eines Wordcount-Auftrags.  Vom Auftrag werden die Datei `/example/data/gutenberg/davinci.txt` gelesen und die Ergebnisse in `/example/data/davinciwordcount` ausgegeben.  Wenn Sie die Anwendung erneut ausführen möchten, müssen Sie den Ausgabeordner bereinigen.

> [!NOTE]  
> Die Schritte in diesem Artikel müssen auf einem Windows-Client ausgeführt werden. Informationen zur Verwendung eines Linux-, OS X- oder Unix-Clients für Hive erhalten Sie, indem Sie die Registerkartenauswahl am Anfang des Artikels nutzen.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Apache Hadoop-Cluster in HDInsight. Siehe [Erstellen von Apache Hadoop-Clustern im Azure-Portal](../hdinsight-hadoop-create-linux-clusters-portal.md).

* [Visual Studio](https://visualstudio.microsoft.com/vs/community/).

## <a name="submit-mapreduce-jobs-using-hdinsight-net-sdk"></a>Übermitteln von MapReduce-Jobs mit HDInsight .NET SDK

Das HDInsight .NET SDK enthält .NET-Clientbibliotheken, die das Arbeiten mit HDInsight-Clustern in .NET vereinfachen.

1. Starten Sie Visual Studio, und erstellen Sie eine C#-Konsolenanwendung.

1. Navigieren Sie zu **Extras** > **NuGet-Paket-Manager** > **Paket-Manager-Konsole**, und geben Sie den folgenden Befehl ein:

    ```   
    Install-Package Microsoft.Azure.Management.HDInsight.Job
    ```

1. Kopieren Sie den folgenden Code in **Program.cs**. Bearbeiten Sie dann den Code, indem Sie die Werte für `existingClusterName`, `existingClusterPassword`, `defaultStorageAccountName`, `defaultStorageAccountKey` und `defaultStorageContainerName` festlegen.

    ```csharp
    using System.Collections.Generic;
    using System.IO;
    using System.Text;
    using System.Threading;
    using Microsoft.Azure.Management.HDInsight.Job;
    using Microsoft.Azure.Management.HDInsight.Job.Models;
    using Hyak.Common;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    
    namespace SubmitHDInsightJobDotNet
    {
        class Program
        {
            private static HDInsightJobManagementClient _hdiJobManagementClient;
    
            private const string existingClusterName = "<Your HDInsight Cluster Name>";
            private const string existingClusterPassword = "<Cluster User Password>";
            private const string defaultStorageAccountName = "<Default Storage Account Name>"; 
            private const string defaultStorageAccountKey = "<Default Storage Account Key>";
            private const string defaultStorageContainerName = "<Default Blob Container Name>";
    
            private const string existingClusterUsername = "admin";
            private const string existingClusterUri = existingClusterName + ".azurehdinsight.net";
            private const string sourceFile = "/example/data/gutenberg/davinci.txt";
            private const string outputFolder = "/example/data/davinciwordcount";
    
            static void Main(string[] args)
            {
                System.Console.WriteLine("The application is running ...");
    
                var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = existingClusterUsername, Password = existingClusterPassword };
                _hdiJobManagementClient = new HDInsightJobManagementClient(existingClusterUri, clusterCredentials);
    
                SubmitMRJob();
    
                System.Console.WriteLine("Press ENTER to continue ...");
                System.Console.ReadLine();
            }
    
            private static void SubmitMRJob()
            {
                List<string> args = new List<string> { { "/example/data/gutenberg/davinci.txt" }, { "/example/data/davinciwordcount" } };
    
                var paras = new MapReduceJobSubmissionParameters
                {
                    JarFile = @"/example/jars/hadoop-mapreduce-examples.jar",
                    JarClass = "wordcount",
                    Arguments = args
                };
    
                System.Console.WriteLine("Submitting the MR job to the cluster...");
                var jobResponse = _hdiJobManagementClient.JobManagement.SubmitMapReduceJob(paras);
                var jobId = jobResponse.JobSubmissionJsonResponse.Id;
                System.Console.WriteLine("Response status code is " + jobResponse.StatusCode);
                System.Console.WriteLine("JobId is " + jobId);
    
                System.Console.WriteLine("Waiting for the job completion ...");
    
                // Wait for job completion
                var jobDetail = _hdiJobManagementClient.JobManagement.GetJob(jobId).JobDetail;
                while (!jobDetail.Status.JobComplete)
                {
                    Thread.Sleep(1000);
                    jobDetail = _hdiJobManagementClient.JobManagement.GetJob(jobId).JobDetail;
                }
    
                // Get job output
                System.Console.WriteLine("Job output is: ");
                var storageAccess = new AzureStorageAccess(defaultStorageAccountName, defaultStorageAccountKey,
                    defaultStorageContainerName);
    
                if (jobDetail.ExitValue == 0)
                {
                    // Create the storage account object
                    CloudStorageAccount storageAccount = CloudStorageAccount.Parse("DefaultEndpointsProtocol=https;AccountName=" +
                        defaultStorageAccountName +
                        ";AccountKey=" + defaultStorageAccountKey);
    
                    // Create the blob client.
                    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    
                    // Retrieve reference to a previously created container.
                    CloudBlobContainer container = blobClient.GetContainerReference(defaultStorageContainerName);
    
                    CloudBlockBlob blockBlob = container.GetBlockBlobReference(outputFolder.Substring(1) + "/part-r-00000");
    
                    using (var stream = blockBlob.OpenRead())
                    {
                        using (StreamReader reader = new StreamReader(stream))
                        {
                            while (!reader.EndOfStream)
                            {
                                System.Console.WriteLine(reader.ReadLine());
                            }
                        }
                    }
                }
                else
                {
                    // fetch stderr output in case of failure
                    var output = _hdiJobManagementClient.JobManagement.GetJobErrorLogs(jobId, storageAccess);
    
                    using (var reader = new StreamReader(output, Encoding.UTF8))
                    {
                        string value = reader.ReadToEnd();
                        System.Console.WriteLine(value);
                    }
    
                }
            }
        }
    }

    ```

1. Drücken Sie **F5**, um die Anwendung auszuführen.

Um den Auftrag erneut auszuführen, müssen Sie den Ordnernamen der Auftragsausgabe ändern. Dieser lautet im Beispiel `/example/data/davinciwordcount`.

Wenn der Auftrag erfolgreich abgeschlossen wurde, gibt die Anwendung den Inhalt der Ausgabedatei `part-r-00000` aus.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie mehrere Möglichkeiten zum Erstellen von HDInsight-Clustern kennengelernt. Weitere Informationen erhalten Sie in den folgenden Artikeln:

* Informationen zum Übermitteln eines Hive-Auftrags finden Sie unter [Ausführen von Apache Hive-Abfragen per HDInsight .NET-SDK](apache-hadoop-use-hive-dotnet-sdk.md).
* Informationen zum Erstellen von HDInsight Clustern finden Sie unter [Erstellen von Linux-basierten Apache Hadoop-Clustern in HDInsight](../hdinsight-hadoop-provision-linux-clusters.md).
* Informationen zum Verwalten von HDInsight Clustern finden Sie unter [Verwalten von Apache Hadoop-Clustern in HDInsight](../hdinsight-administer-use-portal-linux.md).
* Grundlegendes zu HDInsight .NET SDK finden Sie unter [HDInsight .NET SDK-Referenz](https://docs.microsoft.com/dotnet/api/overview/azure/hdinsight).
* Weitere Informationen zur nicht interaktiven Authentifizierung bei Azure finden Sie unter [Erstellen von .NET HDInsight-Anwendungen für die nicht interaktive Authentifizierung](../hdinsight-create-non-interactive-authentication-dotnet-applications.md).