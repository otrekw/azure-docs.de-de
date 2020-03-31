---
title: 'Tutorial: Erstellen einer End-to-End-ETL-Pipeline zum Gewinnen von Erkenntnissen aus Vertriebsdaten in Azure HDInsight'
description: Hier erfahren Sie, wie Sie ETL-Pipelines mit Azure HDInsight erstellen, um mithilfe von bedarfsgesteuerten Spark-Clustern und Power BI Erkenntnisse aus Vertriebsdaten zu gewinnen.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: tutorial
ms.custom: hdinsightactive
ms.date: 03/24/2020
ms.openlocfilehash: a4df99c45b27ad662133010422cae2e30e36e584
ms.sourcegitcommit: 940e16ff194d5163f277f98d038833b1055a1a3e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/25/2020
ms.locfileid: "80247264"
---
# <a name="tutorial-create-an-end-to-end-data-pipeline-to-derive-sales-insights-in-azure-hdinsight"></a>Tutorial: Erstellen einer End-to-End-Datenpipeline zum Gewinnen von Erkenntnissen aus Vertriebsdaten in Azure HDInsight

In diesem Tutorial erstellen Sie eine End-to-End-Datenpipeline, die Extrahierungs-, Transformations- und Ladevorgänge (ETL) durchführt. Die Pipeline verwendet in Azure HDInsight ausgeführte [Apache Spark](./spark/apache-spark-overview.md)- und Apache Hive-Cluster zum Abfragen und Bearbeiten der Daten. Sie verwenden auch Technologien wie Azure Data Lake Storage Gen2 für die Datenspeicherung und Power BI für die Visualisierung.

Diese Datenpipeline kombiniert die Daten aus unterschiedlichen Speichern, entfernt unerwünschte Daten, fügt neue Daten an und lädt sie dann alle wieder in den Speicher, um geschäftliche Erkenntnisse zu visualisieren. Weitere Informationen zu ETL-Pipelines finden Sie unter [Bedarfsorientiertes Extrahieren, Transformieren und Laden (ETL)](./hadoop/apache-hadoop-etl-at-scale.md).

![ETL-Architektur](./media/hdinsight-sales-insights-etl/architecture.png)

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Befehlszeilenschnittstelle. Weitere Informationen finden Sie unter [Installieren der Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).

* Mitglied der [integrierten Azure-Rolle „Besitzer“](../role-based-access-control/built-in-roles.md)

* [Power BI Desktop](https://www.microsoft.com/download/details.aspx?id=45331), um am Ende dieses Tutorials geschäftliche Erkenntnisse zu visualisieren

## <a name="create-resources"></a>Erstellen von Ressourcen

### <a name="clone-the-repository-with-scripts-and-data"></a>Klonen des Repositorys mit Skripts und Daten

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Öffnen Sie Azure Cloud Shell über die obere Menüleiste. Wählen Sie Ihr Abonnement zum Erstellen einer Dateifreigabe aus, wenn Sie von Cloud Shell dazu aufgefordert werden.

   ![Öffnen von Azure Cloud Shell](./media/hdinsight-sales-insights-etl/hdinsight-sales-insights-etl-click-cloud-shell.png)

1. Wählen Sie im Dropdownmenü **Umgebung auswählen** die Option **Bash** aus.

1. Stellen Sie sicher, dass Sie Mitglied der Azure-Rolle [Besitzer](../role-based-access-control/built-in-roles.md) sind. Ersetzen Sie `user@contoso.com` durch Ihr Konto, und geben Sie dann den folgenden Befehl ein:

    ```azurecli
    az role assignment list \
    --assignee "user@contoso.com" \
    --role "Owner"
    ```

    Wird kein Datensatz zurückgegeben, sind Sie kein Mitglied und können dieses Tutorial nicht durchführen.

1. Listen Sie Ihre Abonnements durch Eingabe des folgenden Befehls auf:

    ```azurecli
    az account list --output table
    ```

    Notieren Sie sich die ID des Abonnements, das Sie für dieses Projekt verwenden möchten.

1. Legen Sie das Abonnement fest, das Sie für dieses Projekt verwenden möchten. Ersetzen Sie `SUBSCRIPTIONID` durch den tatsächlichen Wert, und geben Sie anschließend den folgenden Befehl ein:

    ```azurecli
    subscriptionID="SUBSCRIPTIONID"
    az account set --subscription $subscriptionID
    ```

1. Erstellen Sie eine neue Ressourcengruppe für das Projekt. Ersetzen Sie `RESOURCEGROUP` durch den gewünschten Namen, und geben Sie anschließend den folgenden Befehl ein:

    ```azurecli
    resourceGroup="RESOURCEGROUP"
    az group create --name $resourceGroup --location westus
    ```

1. Laden Sie die Daten und Skripts für dieses Tutorial aus dem [Repository für das HDInsight-ETL-Szenario mit Vertriebsdaten](https://github.com/Azure-Samples/hdinsight-sales-insights-etl) herunter.  Geben Sie den folgenden Befehl ein:

    ```bash
    git clone https://github.com/Azure-Samples/hdinsight-sales-insights-etl.git
    cd hdinsight-sales-insights-etl
    ```

1. Vergewissern Sie sich, ob `salesdata scripts templates` erstellt wurde. Verwenden Sie dazu den folgenden Befehl:

   ```bash
   ls
   ```

### <a name="deploy-azure-resources-needed-for-the-pipeline"></a>Bereitstellen der erforderlichen Azure-Ressourcen für die Pipeline

1. Fügen Sie Ausführungsberechtigungen für alle Skripts hinzu, indem Sie Folgendes eingeben:

    ```bash
    chmod +x scripts/*.sh
    ````

1. Führen Sie das Skript aus. Ersetzen Sie `RESOURCE_GROUP_NAME` und `LOCATION` durch die entsprechenden Werte, und geben Sie dann den folgenden Befehl ein:

    ```bash
    ./scripts/resources.sh RESOURCE_GROUP_NAME LOCATION
    ```

    Mit dem Befehl werden die folgenden Ressourcen bereitgestellt:

    * Ein Azure Blob Storage-Konto. Dieses Konto enthält die Vertriebsdaten des Unternehmens.
    * Ein Azure Data Lake Storage Gen2-Konto. Dieses Konto dient als Speicherkonto für beide HDInsight-Cluster. Weitere Informationen zu HDInsight und Data Lake Storage Gen2 finden Sie unter [Azure HDInsight-Integration in Data Lake Storage Gen2](https://azure.microsoft.com/blog/azure-hdinsight-integration-with-data-lake-storage-gen-2-preview-acl-and-security-update/).
    * Eine vom Benutzer zugewiesene verwaltete Identität. Dieses Konto gewährt den HDInsight-Clustern Zugriff auf das Data Lake Storage Gen2-Konto.
    * Einen Apache Spark-Cluster. Dieser Cluster wird zum Bereinigen und Transformieren der Rohdaten verwendet.
    * Einen [Interactive Query](./interactive-query/apache-interactive-query-get-started.md)-Cluster in Apache Hive. Mit diesem Cluster können die Vertriebsdaten abgefragt und mit Power BI visualisiert werden.
    * Ein virtuelles Azure-Netzwerk, das von NSG-Regeln (Netzwerksicherheitsgruppe) unterstützt wird. Dieses virtuelle Netzwerk ermöglicht die Kommunikation der Cluster und sichert zudem ihre Kommunikation.

Die Erstellung des Clusters dauert ungefähr 20 Minuten.

Das Skript `resources.sh` enthält die folgenden Befehle. Sie müssen diese Befehle nicht ausführen, wenn Sie das Skript bereits im vorherigen Schritt ausgeführt haben.

* `resourcestemplate.json`: Dieser Befehl erstellt mithilfe einer Azure Resource Manager-Vorlage (`az group deployment create`) die angegebenen Ressourcen mit der gewünschten Konfiguration.

    ```azurecli
    az group deployment create --name ResourcesDeployment \
        --resource-group $resourceGroup \
        --template-file resourcestemplate.json \
        --parameters "@resourceparameters.json"
    ```

* `az storage blob upload-batch`: Dieser Befehl lädt mit dem folgenden Befehl die CSV-Dateien mit den Vertriebsdaten in das neu erstellte Blobspeicherkonto hoch:

    ```azurecli
    az storage blob upload-batch -d rawdata \
        --account-name <BLOB STORAGE NAME> -s ./ --pattern *.csv
    ```

Das Standardkennwort für den SSH-Zugriff auf die Cluster ist `Thisisapassword1`. Wenn Sie das Kennwort ändern möchten, rufen Sie die `resourcesparameters.json` auf, und ändern Sie das Kennwort für die Parameter `sparksshPassword`, `sparkClusterLoginPassword`, `llapClusterLoginPassword` und `llapsshPassword`.

### <a name="verify-deployment-and-collect-resource-information"></a>Überprüfen der Bereitstellung und Sammeln von Ressourceninformationen

1. Um den Status der Bereitstellung zu überprüfen, navigieren Sie im Azure-Portal zur Ressourcengruppe. Wählen Sie unter **Einstellungen** die Option **Bereitstellungen** aus. Wählen Sie den Namen Ihrer Bereitstellung (`ResourcesDeployment`) aus. Hier sehen Sie die Ressourcen, die erfolgreich bereitgestellt wurden, und jene, deren Bereitstellung noch läuft.

1. Geben Sie zum Anzeigen der Clusternamen den folgenden Befehl ein:

    ```azurecli
    sparkCluster=$(az hdinsight list \
        --resource-group $resourceGroup \
        --query "[?contains(name,'spark')].{clusterName:name}" -o tsv)

    llapCluster=$(az hdinsight list \
        --resource-group $resourceGroup \
        --query "[?contains(name,'llap')].{clusterName:name}" -o tsv)

    echo $sparkCluster
    echo $llapCluster
    ```

1. Geben Sie den folgenden Befehl ein, um das Azure-Speicherkonto und den Zugriffsschlüssel anzuzeigen:

    ```azurecli
    blobStorageName=$(cat resourcesoutputs.json | jq -r '.properties.outputs.blobStorageName.value')

    blobKey=$(az storage account keys list \
        --account-name $blobStorageName \
        --resource-group $resourceGroup \
        --query [0].value -o tsv)

    echo $blobStorageName
    echo $blobKey
    ```

1. Geben Sie den folgenden Befehl ein, um das Data Lake Storage Gen2-Konto und den Zugriffsschlüssel anzuzeigen:

    ```azurecli
    ADLSGen2StorageName=$(cat resourcesoutputs.json | jq -r '.properties.outputs.adlsGen2StorageName.value')

    adlsKey=$(az storage account keys list \
        --account-name $ADLSGen2StorageName \
        --resource-group $resourceGroup \
        --query [0].value -o tsv)

    echo $ADLSGen2StorageName
    echo $adlsKey
    ```

### <a name="create-a-data-factory"></a>Erstellen einer Data Factory

Azure Data Factory (ADF) ist ein Tool, mit dem Sie Azure Pipelines automatisieren können. Es ist nicht die einzige Möglichkeit, um diese Aufgaben durchzuführen, eignet sich aber hervorragend zum Automatisieren der Prozesse. Weitere Informationen zu Azure Data Factory finden Sie in der [Dokumentation zu Azure Data Factory](https://azure.microsoft.com/services/data-factory/).

Diese Data Factory enthält eine Pipeline mit zwei Aktivitäten:

* Die erste Aktivität kopiert die Daten aus dem Azure Blob Storage-Konto in das Data Lake Storage Gen2-Speicherkonto, um die Datenerfassung zu simulieren.
* Die zweite Aktivität transformiert die Daten im Spark-Cluster. Das Skript transformiert die Daten, indem unerwünschte Spalten entfernt werden. Es fügt auch eine neue Spalte an, die den durch eine einzelne Transaktion generierten Umsatz berechnet.

Führen Sie zum Einrichten der Azure Data Factory-Pipeline den folgenden Befehl aus:

```bash
./scripts/adf.sh
```

Das Skript führt folgende Schritte aus:

1. Es erstellt einen Dienstprinzipal mit `Storage Blob Data Contributor`-Berechtigungen für das Data Lake Storage Gen2-Speicherkonto.
1. Es ruft ein Authentifizierungstoken zum Autorisieren von POST-Anforderungen an die [REST-API für das Data Lake Storage Gen2-Dateisystem](https://docs.microsoft.com/rest/api/storageservices/datalakestoragegen2/filesystem/create) ab.
1. Es trägt in den Dateien `sparktransform.py` und `query.hql` den tatsächlichen Namen Ihres Data Lake Storage Gen2-Speicherkontos ein.
1. Es ruft Speicherschlüssel für die Data Lake Storage Gen2- und Blob Storage-Konten ab.
1. Es erstellt eine weitere Ressourcenbereitstellung, um eine Azure Data Factory-Pipeline mit den zugehörigen verknüpften Diensten und Aktivitäten zu erstellen. Es übergibt die Speicherschlüssel als Parameter an die Vorlagendatei, sodass die verknüpften Dienste korrekt auf die Speicherkonten zugreifen können.

Die Data Factory-Pipeline wird mit dem folgenden Befehl bereitgestellt:

```azurecli-interactive
az group deployment create --name ADFDeployment \
    --resource-group $resourceGroup \
    --template-file adftemplate.json \
    --parameters "@adfparameters.json"
```

## <a name="run-the-data-pipeline"></a>Ausführen der Datenpipeline

### <a name="trigger-the-data-factory-activities"></a>Auslösen der Data Factory-Aktivitäten

Die erste Aktivität in der von Ihnen erstellten Data Factory-Pipeline verschiebt die Daten aus Blob Storage in Data Lake Storage Gen2. Die zweite Aktivität wendet die Spark-Transformationen auf die Daten an und speichert die transformierten CSV-Dateien an einem neuen Speicherort. Die Ausführung der gesamten Pipeline kann einige Minuten dauern.

Sie können die Pipelines wie folgt auslösen:

* Lösen Sie die Data Factory-Pipelines in PowerShell aus. Ersetzen Sie `DataFactoryName` durch den tatsächlichen Data Factory-Namen, und führen Sie dann die folgenden Befehle aus:

    ```powershell
    Invoke-AzDataFactoryV2Pipeline -DataFactory DataFactoryName -PipelineName "CopyPipeline_k8z"
    Invoke-AzDataFactoryV2Pipeline -DataFactory DataFactoryName -PipelineName "sparkTransformPipeline"
    ```

    oder

* Öffnen Sie die Data Factory, und wählen Sie **Erstellen und überwachen** aus. Lösen Sie die Kopierpipeline und dann die Spark-Pipeline über das Portal aus. Informationen zum Auslösen von Pipelines über das Portal finden Sie unter [Erstellen bedarfsgesteuerter Apache Hadoop-Cluster in HDInsight mit Azure Data Factory](hdinsight-hadoop-create-linux-clusters-adf.md#trigger-a-pipeline).

Zum Überprüfen, ob die Pipelines ausgeführt wurden, können Sie einen der beiden folgenden Schritte ausführen:

* Rufen Sie im Portal den Abschnitt **Überwachen** in Ihrer Data Factory auf.
* Rufen Sie im Azure Storage-Explorer Ihr Data Lake Storage Gen2-Speicherkonto auf. Wechseln Sie zum Dateisystem `files`, navigieren Sie zum Ordner `transformed`, und überprüfen Sie den Inhalt, um festzustellen, ob die Pipeline erfolgreich ausgeführt wurde.

Weitere Möglichkeiten zum Transformieren von Daten mithilfe von HDInsight finden Sie in [diesem Artikel zur Verwendung von Jupyter Notebook](/azure/hdinsight/spark/apache-spark-load-data-run-query).

### <a name="create-a-table-on-the-interactive-query-cluster-to-view-data-on-power-bi"></a>Erstellen einer Tabelle im Interactive Query-Cluster zum Anzeigen von Daten in Power BI

1. Kopieren Sie die Datei `query.hql` mit SCP in den LLAP-Cluster. Ersetzen Sie `LLAPCLUSTERNAME` durch den tatsächlichen Namen, und geben Sie anschließend den folgenden Befehl ein:

    ```bash
    scp scripts/query.hql sshuser@LLAPCLUSTERNAME-ssh.azurehdinsight.net:/home/sshuser/
    ```

2. Verwenden Sie SSH für den Zugriff auf den LLAP-Cluster. Ersetzen Sie `LLAPCLUSTERNAME` durch den tatsächlichen Namen, und geben Sie anschließend den folgenden Befehl ein: Wenn Sie die Datei `resourcesparameters.json` nicht geändert haben, lautet das Kennwort `Thisisapassword1`.

    ```bash
    ssh sshuser@LLAPCLUSTERNAME-ssh.azurehdinsight.net
    ```

3. Verwenden Sie den folgenden Befehl, um das Skript auszuführen:

    ```bash
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -f query.hql
    ```

Dieses Skript erstellt eine verwaltete Tabelle im Interactive Query-Cluster, auf die Sie über Power BI zugreifen können.

### <a name="create-a-power-bi-dashboard-from-sales-data"></a>Erstellen eines Power BI-Dashboards anhand von Vertriebsdaten

1. Öffnen Sie Power BI Desktop.
1. Wählen Sie **Daten abrufen** aus.
1. Suchen Sie nach **HDInsight-Interactive Query-Cluster**.
1. Fügen Sie dort den URI für Ihren Cluster ein. Sie sollte folgendes Format aufweisen: `https://LLAPCLUSTERNAME.azurehdinsight.net`.

   Geben Sie `default` als Datenbank ein.
1. Geben Sie den Benutzernamen und das Kennwort für den Zugriff auf den Cluster ein.

Nachdem die Daten geladen wurden, können Sie mit dem Dashboard experimentieren, das Sie erstellen möchten. Informationen zu den ersten Schritten mit Power BI-Dashboards finden Sie unter den folgenden Links:

* [Einführung in Dashboards für Power BI-Designer](https://docs.microsoft.com/power-bi/service-dashboards)
* [Tutorial: Erste Schritte mit dem Power BI-Dienst](https://docs.microsoft.com/power-bi/service-get-started)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Falls Sie diese Anwendung nicht weiterverwenden möchten, löschen Sie mit dem folgenden Befehl alle Ressourcen, damit sie Ihnen nicht in Rechnung gestellt werden.

```azurecli-interactive
az group delete -n $resourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Bedarfsorientiertes Extrahieren, Transformieren und Laden (ETL)](./hadoop/apache-hadoop-etl-at-scale.md)
