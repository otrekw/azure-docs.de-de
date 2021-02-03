---
title: Verwenden von MapReduce und Curl mit Hadoop in HDInsight – Azure
description: Erfahren Sie, wie Sie MapReduce-Aufträge mit Apache Hadoop in HDInsight mithilfe von Curl remote ausführen.
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive
ms.date: 01/13/2020
ms.openlocfilehash: e90dc2c7220caf5bd72b7086adc275934652e150
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2021
ms.locfileid: "98939685"
---
# <a name="run-mapreduce-jobs-with-apache-hadoop-on-hdinsight-using-rest"></a>Ausführen von MapReduce-Aufträgen mit Apache Hadoop in HDInsight mithilfe von REST

Erfahren Sie, wie mithilfe der Apache Hive WebHCat-REST-API MapReduce-Aufträge auf einem Apache Hadoop-Cluster in HDInsight ausführen. Curl dient zum Veranschaulichen der Interaktion mit HDInsight über unformatierte HTTP-Anforderungen zum Ausführen von MapReduce-Aufträgen.

> [!NOTE]  
> Wenn Sie bereits mit der Verwendung von Linux-basierten Hadoop-Servern vertraut sind, nicht jedoch mit HDInsight, lesen Sie den Artikel [Informationen zu Linux-basiertem Apache Hadoop in HDInsight](../hdinsight-hadoop-linux-information.md).

## <a name="prerequisites"></a>Voraussetzungen

* Ein Apache Hadoop-Cluster in HDInsight. Siehe [Erstellen von Apache Hadoop-Clustern im Azure-Portal](../hdinsight-hadoop-create-linux-clusters-portal.md).

Optionen:
  * Windows PowerShell oder
  * [cURL](https://curl.haxx.se/) mit [jq](https://stedolan.github.io/jq/)

## <a name="run-a-mapreduce-job"></a>Ausführen eines MapReduce-Auftrags

> [!NOTE]  
> Wenn Sie Curl oder eine andere REST-Kommunikation mit WebHCat verwenden, müssen Sie die Anforderungen authentifizieren, indem Sie den Benutzernamen und das Kennwort des Administrators des HDInsight-Clusters bereitstellen. Sie müssen den Clusternamen als Teil des URI verwenden, der zum Senden der Anforderungen an den Server genutzt wird.
>
> Die REST-API wird durch [Standardauthentifizierung](https://en.wikipedia.org/wiki/Basic_access_authentication)gesichert. Sie sollten Anforderungen immer über HTTPS stellen, um sicherzustellen, dass Ihre Anmeldeinformationen sicher an den Server gesendet werden.

### <a name="curl"></a>Curl

1. Legen Sie zur Verbesserung der Benutzerfreundlichkeit die unten aufgeführten Variablen fest. Dieses Beispiel basiert auf einer Windows-Umgebung, die nach Bedarf für Ihre Umgebung überarbeitet werden kann.

    ```cmd
    set CLUSTERNAME=
    set PASSWORD=
    ```

1. Verwenden Sie den folgenden Befehl in einer Befehlszeile, um zu überprüfen, ob Sie die Verbindung zum HDInsight-Cluster herstellen können:

    ```bash
    curl -u admin:%PASSWORD% -G https://%CLUSTERNAME%.azurehdinsight.net/templeton/v1/status
    ```

    Folgende Parameter werden in diesem Befehl verwendet:

   * **-u**: Gibt den Benutzernamen und das Kennwort für die Authentifizierung der Anforderung an
   * **-G**: Gibt an, dass dieser Vorgang eine GET-Anforderung ist

   Der Anfang des URI (`https://CLUSTERNAME.azurehdinsight.net/templeton/v1`) ist für alle Anforderungen gleich.

    Sie erhalten eine Antwort, die in etwa dem folgenden JSON entspricht:

    ```json
    {"version":"v1","status":"ok"}
    ```

1. Verwenden Sie den folgenden Befehl, um einen MapReduce-Auftrag zu übermitteln. Ändern Sie nach Bedarf den Pfad zu **jq**.

    ```cmd
    curl -u admin:%PASSWORD% -d user.name=admin ^
    -d jar=/example/jars/hadoop-mapreduce-examples.jar ^
    -d class=wordcount -d arg=/example/data/gutenberg/davinci.txt -d arg=/example/data/output ^
    https://%CLUSTERNAME%.azurehdinsight.net/templeton/v1/mapreduce/jar | ^
    C:\HDI\jq-win64.exe .id
    ```

    Das Ende des URIs (/mapreduce/jar) weist WebHCat darauf hin, dass diese Anforderung einen MapReduce-Auftrag aus einer Klasse in einer JAR-Datei startet. Folgende Parameter werden in diesem Befehl verwendet:

   * **-d:** `-G` wird nicht verwendet, daher verwendet die Anforderung standardmäßig die POST-Methode. `-d` gibt die Datenwerte an, die mit der Anforderung gesendet werden.
     * **user.name**: Der Benutzer, der den Befehl ausführt
     * **jar**: Der Speicherort der JAR-Datei, die die auszuführende Klasse enthält
     * **class**: Die Klasse, die die MapReduce-Logik enthält
     * **arg**: Die Argumente, die an den MapReduce-Auftrag übergeben werden, in diesem Fall die Eingabetextdatei und das Verzeichnis für die Ausgabe

    Dieser Befehl sollte eine Auftrags-ID zurückgeben, mit der der Status des Auftrags überprüft werden kann: `job_1415651640909_0026`.

1. Verwenden Sie den folgenden Befehl, um den Status des Auftrags zu prüfen. Ersetzen Sie den Wert für `JOBID` durch den **tatsächlichen** Wert, der im vorherigen Schritt zurückgegeben wurde. Ändern Sie den Speicherort von **jq** nach Bedarf.

    ```cmd
    set JOBID=job_1415651640909_0026

    curl -G -u admin:%PASSWORD% -d user.name=admin https://%CLUSTERNAME%.azurehdinsight.net/templeton/v1/jobs/%JOBID% | ^
    C:\HDI\jq-win64.exe .status.state
    ```

### <a name="powershell"></a>PowerShell

1. Legen Sie zur Verbesserung der Benutzerfreundlichkeit die unten aufgeführten Variablen fest. Ersetzen Sie `CLUSTERNAME` durch den tatsächlichen Clusternamen. Führen Sie den Befehl aus, und geben Sie nach Aufforderung das Anmeldekennwort für den Cluster ein.

    ```powershell
    $clusterName="CLUSTERNAME"
    $creds = Get-Credential -UserName admin -Message "Enter the cluster login password"
    ```

1. Verwenden Sie den folgenden Befehl, um zu überprüfen, ob Sie die Verbindung mit dem HDInsight-Cluster herstellen können:

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clustername.azurehdinsight.net/templeton/v1/status" `
        -Credential $creds `
        -UseBasicParsing
    $resp.Content
    ```

    Sie erhalten eine Antwort, die in etwa dem folgenden JSON entspricht:

    ```json
    {"version":"v1","status":"ok"}
    ```

1. Verwenden Sie den folgenden Befehl, um einen MapReduce-Auftrag zu übermitteln.

    ```powershell
    $reqParams = @{}
    $reqParams."user.name" = "admin"
    $reqParams.jar = "/example/jars/hadoop-mapreduce-examples.jar"
    $reqParams.class = "wordcount"
    $reqParams.arg = @()
    $reqParams.arg += "/example/data/gutenberg/davinci.txt"
    $reqparams.arg += "/example/data/output"
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/templeton/v1/mapreduce/jar" `
       -Credential $creds `
       -Body $reqParams `
       -Method POST `
       -UseBasicParsing
    $jobID = (ConvertFrom-Json $resp.Content).id
    $jobID
    ```

    Das Ende des URIs (/mapreduce/jar) weist WebHCat darauf hin, dass diese Anforderung einen MapReduce-Auftrag aus einer Klasse in einer JAR-Datei startet. Folgende Parameter werden in diesem Befehl verwendet:

    * **user.name**: Der Benutzer, der den Befehl ausführt
    * **jar**: Der Speicherort der JAR-Datei, die die auszuführende Klasse enthält
    * **class**: Die Klasse, die die MapReduce-Logik enthält
    * **arg**: Die Argumente, die an den MapReduce-Auftrag übergeben werden, in diesem Fall die Eingabetextdatei und das Verzeichnis für die Ausgabe

   Dieser Befehl sollte eine Auftrags-ID zurückgeben, mit der der Status des Auftrags überprüft werden kann: `job_1415651640909_0026`.

1. Verwenden Sie den folgenden Befehl, um den Status des Auftrags zu prüfen:

    ```powershell
    $reqParams=@{"user.name"="admin"}
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/templeton/v1/jobs/$jobID" `
       -Credential $creds `
       -Body $reqParams `
       -UseBasicParsing

    # ConvertFrom-JSON can't handle duplicate names with different case
    # So change one to prevent the error
    $fixDup=$resp.Content.Replace("jobID","job_ID")
    (ConvertFrom-Json $fixDup).status.state
    ```

### <a name="both-methods"></a>Beide Methoden

1. Wenn der Auftrag abgeschlossen ist, wird der Status `SUCCEEDED` angezeigt.

1. Sobald der Status des Auftrags in `SUCCEEDED` wechselt, können Sie die Ergebnisse des Auftrags aus Azure Blob Storage abrufen. Der mit der Abfrage übergebene Parameter `statusdir` enthält den Speicherort der Ausgabedatei. In diesem Beispiel ist dies `/example/curl`. Diese Adresse speichert die Ausgabe des Auftrags unter `/example/curl` im Standardspeicher des Clusters.

Sie können diese Dateien mithilfe der [Azure-Befehlszeilenschnittstelle](/cli/azure/install-azure-cli) auflisten und herunterladen. Weitere Informationen zur Verwendung der Azure CLI für das Arbeiten mit Azure Blob Storage finden Sie unter [Schnellstart: Erstellen, Herunterladen und Auflisten von Blobs mit der Azure-Befehlszeilenschnittstelle](../../storage/blobs/storage-quickstart-blobs-cli.md).

## <a name="next-steps"></a>Nächste Schritte

Informationen zu anderen Möglichkeiten, wie Sie mit Hadoop in HDInsight arbeiten können:

* [Verwenden von MapReduce mit Apache Hadoop in HDInsight](hdinsight-use-mapreduce.md)
* [Verwenden von Apache Hive mit Apache Hadoop in HDInsight](hdinsight-use-hive.md)

Weitere Informationen zur REST-Schnittstelle, die in diesem Artikel verwendet wird, finden Sie in der [WebHCat-Referenz](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).
