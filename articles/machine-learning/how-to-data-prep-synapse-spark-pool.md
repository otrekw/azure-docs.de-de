---
title: Data Wrangling mit Apache Spark-Pools (Vorschau)
titleSuffix: Azure Machine Learning
description: Hier erfahren Sie, wie Sie Apache Spark-Pools für Data Wrangling mit Azure Synapse Analytics und Azure Machine Learning anfügen und starten.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.author: nibaccam
author: nibaccam
ms.reviewer: nibaccam
ms.date: 03/02/2021
ms.custom: devx-track-python, data4ml, synapse-azureml
ms.openlocfilehash: ec0ceb496d2e6d1b15819aa6b2353e54a5303354
ms.sourcegitcommit: 5ce88326f2b02fda54dad05df94cf0b440da284b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2021
ms.locfileid: "107889770"
---
# <a name="attach-apache-spark-pools-powered-by-azure-synapse-analytics-for-data-wrangling-preview"></a>Anfügen von Apache Spark-Pools (unterstützt von Azure Synapse Analytics) für Data Wrangling (Vorschau)

In diesem Artikel erfahren Sie, wie Sie einen Apache Spark-Pool, der von [Azure Synapse Analytics](../synapse-analytics/overview-what-is.md) unterstützt wird, Ihrem Azure Machine Learning-Arbeitsbereich anfügen, damit Sie ihn starten und Data Wrangling in großem Umfang durchführen können. 

Dieser Artikel enthält Anleitungen zum interaktiven Ausführen von Data Wrangling-Aufgaben innerhalb einer dedizierten Synapse-Sitzung in einer Jupyter Notebook-Instanz mit dem [Azure Machine Learning SDK für Python](/python/api/overview/azure/ml/). Wenn Sie die Verwendung von Azure Machine Learning-Pipelines bevorzugen, finden Sie weitere Informationen unter [Verwenden von Apache Spark (unterstützt von Azure Synapse Analytics) in Ihrer Machine Learning-Pipeline (Vorschau)](how-to-use-synapsesparkstep.md).

Eine Anleitung zur Verwendung von Azure Synapse Analytics mit einem Synapse-Arbeitsbereich finden Sie in der Reihe [Erste Schritte mit Azure Synapse Analytics](../synapse-analytics/get-started.md).

>[!IMPORTANT]
> Die Integration von Azure Machine Learning und Azure Synapse Analytics befindet sich in der Vorschauphase. Die in diesem Artikel vorgestellten Funktionen verwenden das Paket `azureml-synapse`, das [experimentelle](/python/api/overview/azure/ml/#stable-vs-experimental) Previewfunktionen enthält, die sich jederzeit ändern können.

## <a name="azure-machine-learning-and-azure-synapse-analytics-integration-preview"></a>Integration von Azure Machine Learning und Azure Synapse Analytics (Vorschau)

Die Integration von Azure Synapse Analytics in Azure Machine Learning (Vorschau) ermöglicht Ihnen das Anfügen eines von Azure Synapse unterstützten Apache Spark-Pools für die interaktive Datenuntersuchung und -aufbereitung. Dank dieser Integration können Sie eine dedizierte Computeressource für das Data Wrangling im großen Stil innerhalb des gleichen Python-Notebooks nutzen, das Sie auch zum Trainieren Ihrer Machine Learning-Modelle verwenden.

## <a name="prerequisites"></a>Voraussetzungen

* Das [Azure Machine Learning Python SDK muss installiert sein](/python/api/overview/azure/ml/install). 

* [Erstellen Sie einen Azure Machine Learning-Arbeitsbereich](how-to-manage-workspace.md?tabs=python).

* [Erstellen eines Azure Synapse Analytics-Arbeitsbereichs im Azure-Portal](../synapse-analytics/quickstart-create-workspace.md)

* [Erstellen eines Apache Spark-Pools über das Azure-Portal, Webtools oder Synapse Studio](../synapse-analytics/quickstart-create-apache-spark-pool-portal.md)

* [Konfigurieren Sie Ihre Entwicklungsumgebung](how-to-configure-environment.md) für die Installation des Azure Machine Learning SDK, oder verwenden Sie eine [Azure Machine Learning-Computeinstanz](concept-compute-instance.md#create) mit bereits installiertem SDK. 

* Installieren Sie das Paket `azureml-synapse` (Vorschau) mit dem folgenden Code:

  ```python
  pip install azureml-synapse
  ```

* [Verknüpfen des Azure Machine Learning-Arbeitsbereichs und des Azure Synapse Analytics-Arbeitsbereichs](how-to-link-synapse-ml-workspaces.md)

## <a name="get-an-existing-linked-service"></a>Abrufen eines vorhandenen verknüpften Diensts
Damit Sie eine dedizierte Computeressource für Data Wrangling anfügen können, benötigen Sie einen ML-Arbeitsbereich, der mit einem Azure Synapse Analytics-Arbeitsbereich verknüpft ist. Dies wird als verknüpfter Dienst bezeichnet. 

Zum Abrufen und Verwenden eines vorhandenen verknüpften Diensts sind Berechtigungen vom Typ **Benutzer oder Mitwirkender** für den Azure Synapse Analytics-Arbeitsbereich erforderlich.

Zeigen Sie alle verknüpften Dienste an, die Ihrem Machine Learning-Arbeitsbereich zugeordnet sind. 

```python
from azureml.core import LinkedService

LinkedService.list(ws)
```

In diesem Beispiel wird ein vorhandener verknüpfter Dienst (`synapselink1`) aus dem Arbeitsbereich `ws` mit der Methode [`get()`](/python/api/azureml-core/azureml.core.linkedservice#get-workspace--name-) abgerufen.
```python
from azureml.core import LinkedService

linked_service = LinkedService.get(ws, 'synapselink1')
```
 
## <a name="attach-synapse-spark-pool-as-a-compute"></a>Anfügen eines Synapse Spark-Pools als Computeressource

Wenn Sie den verknüpften Dienst abgerufen haben, fügen Sie einen Synapse Apache Spark-Pool als dedizierte Computeressource für Ihre Data Wrangling-Aufgaben an. 

Sie können Apache Spark-Pools über folgende Komponenten anfügen:
* Azure Machine Learning Studio
* [Azure Resource Manager-Vorlagen](https://github.com/Azure/azure-quickstart-templates/blob/master/101-machine-learning-linkedservice-create/azuredeploy.json)
* Python-SDK für Azure Machine Learning 

### <a name="attach-a-pool-via-the-studio"></a>Anfügen eines Pools über Studio
Folgen Sie diesen Schritten: 

1. Melden Sie sich bei [Azure Machine Learning Studio](https://ml.azure.com/) an.
1. Wählen Sie im linken Bereich im Abschnitt **Verwalten** die Option **Verknüpfte Dienste** aus.
1. Wählen Sie Ihren Synapse-Arbeitsbereich aus.
1. Wählen Sie oben links **Angefügte Spark-Pools** aus. 
1. Wählen Sie **Anfügen** aus. 
1. Wählen Sie Ihren Apache Spark-Pool in der Liste aus, und geben Sie einen Namen an.  
    1. Diese Liste gibt Aufschluss über die verfügbaren Synapse Spark-Pools, die an Ihre Computeressource angefügt werden können. 
    1. Informationen zum Erstellen eines neuen Synapse Spark-Pools finden Sie unter [Erstellen eines Apache Spark-Pools mit Synapse Studio](../synapse-analytics/quickstart-create-apache-spark-pool-portal.md).
1. Wählen Sie **Attach selected** (Ausgewählte anfügen) aus. 

### <a name="attach-a-pool-with-the-python-sdk"></a>Anfügen eines Pools mit dem Python SDK

Sie können einen Apache Spark-Pool auch mithilfe des **Python SDK** anfügen. 

Mit dem unten genannten Code werden die folgenden Schritte ausgeführt: 
1. Konfiguration von [`SynapseCompute`](/python/api/azureml-core/azureml.core.compute.synapsecompute) mit

   1. dem [`LinkedService`](/python/api/azureml-core/azureml.core.linkedservice), `linked_service`, den Sie im vorherigen Schritt erstellt oder abgerufen haben. 
   1. dem Typ des Computeziels, das Sie anfügen möchten: `SynapseSpark`
   1. dem Namen des Apache Spark-Pools. Dieser muss einem vorhandenen Apache Spark-Pool entsprechen, der sich in Ihrem Azure Synapse Analytics-Arbeitsbereich befindet.
   
1. Erstellen eines Machine Learning-[`ComputeTarget`](/python/api/azureml-core/azureml.core.computetarget) durch Übergabe 
   1. des Machine Learning-Arbeitsbereichs, den Sie verwenden möchten: `ws`
   1. des Namens, mit dem Sie im Azure Machine Learning-Arbeitsbereich auf die Computeressource verweisen möchten 
   1. der Anfügekonfiguration (attach_configuration), die Sie beim Konfigurieren der Synapse-Computeressource angegeben haben
       1. Der Aufruf von „ComputeTarget.attach()“ ist asynchron, daher wird das Beispiel blockiert, bis der Aufruf abgeschlossen ist.

```python
from azureml.core.compute import SynapseCompute, ComputeTarget

attach_config = SynapseCompute.attach_configuration(linked_service, #Linked synapse workspace alias
                                                    type='SynapseSpark', #Type of assets to attach
                                                    pool_name=synapse_spark_pool_name) #Name of Synapse spark pool 

synapse_compute = ComputeTarget.attach(workspace= ws,                
                                       name= synapse_compute_name, 
                                       attach_configuration= attach_config
                                      )

synapse_compute.wait_for_completion()
```

Überprüfen Sie, ob der Apache Spark-Pool angefügt wurde.

```python
ws.compute_targets['Synapse Spark pool alias']
```

## <a name="launch-synapse-spark-pool-for-data-wrangling-tasks"></a>Starten des Synapse Spark-Pools für Data Wrangling-Aufgaben

Geben Sie für die Datenaufbereitung mit dem Apache Spark-Pool zunächst den Namen des Apache Spark-Pools an:

> [!IMPORTANT]
> Wenn Sie den Apache Spark-Pool weiterhin verwenden möchten, müssen Sie angeben, welche Computeressource in den Data Wrangling-Aufgaben verwendet werden soll. Verwenden Sie `%synapse` für einzelne Codezeilen und `%%synapse` für mehrere Zeilen. 

```python
%synapse start -c SynapseSparkPoolAlias
```

Nach dem Start der Sitzung können Sie die Metadaten der Sitzung überprüfen.

```python
%synapse meta
```

Sie können eine [Azure Machine Learning-Umgebung](concept-environments.md) angeben, die während der Apache Spark-Sitzung verwendet werden soll. Nur in der Umgebung angegebene Conda-Abhängigkeiten werden wirksam. Ein Docker-Image wird nicht unterstützt.

>[!WARNING]
>  In Conda-Umgebungsabhängigkeiten angegebene Python-Abhängigkeiten werden in Apache Spark-Pools nicht unterstützt. Derzeit werden nur feste Python-Versionen unterstützt. Überprüfen Sie Ihre Python-Version, indem Sie `sys.version_info` in Ihr Skript einschließen.

Der folgende Code erstellt die-Umgebung `myenv`, in der vor Beginn der Sitzung Version 1.20.0 von `azureml-core` und Version 1.17.0 von `numpy` installiert werden. Anschließend können Sie diese Umgebung in die `start`-Anweisung der Apache Spark-Sitzung einschließen.

```python

from azureml.core import Workspace, Environment

# creates environment with numpy and azureml-core dependencies
ws = Workspace.from_config()
env = Environment(name="myenv")
env.python.conda_dependencies.add_pip_package("azureml-core==1.20.0")
env.python.conda_dependencies.add_conda_package("numpy==1.17.0")
env.register(workspace=ws)
```

Geben Sie für die Datenaufbereitung mit dem Apache Spark-Pool und der benutzerdefinierten Umgebung zunächst den Namen des Apache Spark-Pools und die Umgebung an, die während der Apache Spark-Sitzung verwendet werden soll. Darüber hinaus können Sie Ihre Abonnement-ID, die Ressourcengruppe für den Machine Learning-Arbeitsbereich und den Namen des Machine Learning-Arbeitsbereichs angeben.

```python
%synapse start -c SynapseSparkPoolAlias -e myenv -s AzureMLworkspaceSubscriptionID -r AzureMLworkspaceResourceGroupName -w AzureMLworkspaceName
```
## <a name="load-data-from-storage"></a>Laden von Daten aus dem Speicher

Lesen Sie nach dem Starten der Apache Spark-Sitzung die Daten ein, die Sie aufbereiten möchten. Das Laden von Daten wird für Azure Blob Storage and Azure Data Lake Storage Gen1 und Gen2 unterstützt.

Es gibt zwei Möglichkeiten, Daten aus diesen Speicherdiensten zu laden: 

* Direktes Laden von Daten aus dem Speicher mithilfe des HDFS-Pfads (Hadoop Distributed Files System)

* Einlesen von Daten aus einem vorhandenen [Azure Machine Learning-Dataset](how-to-create-register-datasets.md)

Für den Zugriff auf diese Speicherdienste benötigen Sie Berechtigungen vom Typ **Storage-Blobdatenleser**. Wenn Sie Daten in diese Speicherdienste zurückschreiben möchten, benötigen Sie Berechtigungen vom Typ **Mitwirkender an Storage-Blobdaten**. Weitere Informationen zu Speicherberechtigungen und -rollen finden Sie [hier](../storage/common/storage-auth-aad-rbac-portal.md#azure-roles-for-blobs-and-queues).

### <a name="load-data-with-hadoop-distributed-files-system-hdfs-path"></a>Laden von Daten mithilfe des HDFS-Pfads (Hadoop Distributed Files System)

Zum Laden und Einlesen von Daten aus Speicher mit dem entsprechenden HDFS-Pfad müssen Sie Ihre Anmeldeinformationen für die Datenzugriffsauthentifizierung bereithalten. Diese Anmeldeinformationen unterscheiden sich je nach Speichertyp.  

Der folgende Code veranschaulicht das Einlesen von Daten aus **Azure Blob Storage** in einen Spark-Datenrahmen mit Ihrem SAS-Token (Shared Access Signature) oder Zugriffsschlüssel: 

```python
%%synapse

# setup access key or SAS token
sc._jsc.hadoopConfiguration().set("fs.azure.account.key.<storage account name>.blob.core.windows.net", "<access key>")
sc._jsc.hadoopConfiguration().set("fs.azure.sas.<container name>.<storage account name>.blob.core.windows.net", "<sas token>")

# read from blob 
df = spark.read.option("header", "true").csv("wasbs://demo@dprepdata.blob.core.windows.net/Titanic.csv")
```

Der folgende Code veranschaulicht das Einlesen von Daten aus **Azure Data Lake Storage Generation 1 (ADLS Gen 1)** mit den Anmeldeinformationen Ihres Dienstprinzipals: 

```python
%%synapse

# setup service principal which has access of the data
sc._jsc.hadoopConfiguration().set("fs.adl.account.<storage account name>.oauth2.access.token.provider.type","ClientCredential")

sc._jsc.hadoopConfiguration().set("fs.adl.account.<storage account name>.oauth2.client.id", "<client id>")

sc._jsc.hadoopConfiguration().set("fs.adl.account.<storage account name>.oauth2.credential", "<client secret>")

sc._jsc.hadoopConfiguration().set("fs.adl.account.<storage account name>.oauth2.refresh.url",
"https://login.microsoftonline.com/<tenant id>/oauth2/token")

df = spark.read.csv("adl://<storage account name>.azuredatalakestore.net/<path>")

```

Der folgende Code veranschaulicht das Einlesen von Daten aus **Azure Data Lake Storage Generation 2 (ADLS Gen 2)** mit den Anmeldeinformationen Ihres Dienstprinzipals: 

```python
%%synapse

# setup service principal which has access of the data
sc._jsc.hadoopConfiguration().set("fs.azure.account.auth.type.<storage account name>.dfs.core.windows.net","OAuth")
sc._jsc.hadoopConfiguration().set("fs.azure.account.oauth.provider.type.<storage account name>.dfs.core.windows.net", "org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider")
sc._jsc.hadoopConfiguration().set("fs.azure.account.oauth2.client.id.<storage account name>.dfs.core.windows.net", "<client id>")
sc._jsc.hadoopConfiguration().set("fs.azure.account.oauth2.client.secret.<storage account name>.dfs.core.windows.net", "<client secret>")
sc._jsc.hadoopConfiguration().set("fs.azure.account.oauth2.client.endpoint.<storage account name>.dfs.core.windows.net",
"https://login.microsoftonline.com/<tenant id>/oauth2/token")

df = spark.read.csv("abfss://<container name>@<storage account>.dfs.core.windows.net/<path>")

```

### <a name="read-in-data-from-registered-datasets"></a>Einlesen von Daten aus registrierten Datasets

Sie können auch ein vorhandenes registriertes Dataset in Ihrem Arbeitsbereich abrufen und die Datenaufbereitung durchführen, indem Sie es in einen Spark-Datenrahmen konvertieren.

Im folgenden Beispiel wird der Arbeitsbereich authentifiziert sowie das registrierte Tabellendataset (TabularDataset) `blob_dset` abgerufen, das auf Dateien im Blobspeicher verweist, und in einen Spark-Datenrahmen konvertiert. Beim Konvertieren Ihrer Datasets in einen Spark-Datenrahmen können Sie die `pyspark`-Datenuntersuchung und -Vorbereitungsbibliotheken nutzen.  

``` python
%%synapse

from azureml.core import Workspace, Dataset

subscription_id = "<enter your subscription ID>"
resource_group = "<enter your resource group>"
workspace_name = "<enter your workspace name>"

ws = Workspace(workspace_name = workspace_name,
               subscription_id = subscription_id,
               resource_group = resource_group)

dset = Dataset.get_by_name(ws, "blob_dset")
spark_df = dset.to_spark_dataframe()
```

## <a name="perform-data-wrangling-tasks"></a>Ausführen von Data Wrangling-Aufgaben

Nach dem Abrufen und Untersuchen Ihrer Daten können Sie Data Wrangling-Aufgaben ausführen.

Der folgende Code erweitert das HDFS-Beispiel im vorherigen Abschnitt, filtert die Daten im Spark-Datenrahmen `df` basierend auf der Spalte **Survivor** und gruppiert diese Liste nach **Age**.

```python
%%synapse

from pyspark.sql.functions import col, desc

df.filter(col('Survived') == 1).groupBy('Age').count().orderBy(desc('count')).show(10)

df.show()

```

## <a name="save-data-to-storage-and-stop-spark-session"></a>Speichern von Daten im Speicher und Beenden der Spark-Sitzung

Nachdem Sie die Datenuntersuchung und -aufbereitung abgeschlossen haben, speichern Sie Ihre aufbereiteten Daten zur späteren Verwendung in Ihrem Speicherkonto in Azure.

Im folgenden Beispiel werden die aufbereiteten Daten in Azure Blob Storage zurückgeschrieben und die ursprüngliche Datei `Titanic.csv` im Verzeichnis `training_data` überschrieben. Für das Zurückschreiben in den Speicher benötigen Sie Berechtigungen vom Typ **Mitwirkender an Storage-Blobdaten**. Weitere Informationen zu Speicherberechtigungen und -rollen finden Sie [hier](../storage/common/storage-auth-aad-rbac-portal.md#azure-roles-for-blobs-and-queues).

```python
%% synapse

df.write.format("csv").mode("overwrite").save("wasbs://demo@dprepdata.blob.core.windows.net/training_data/Titanic.csv")
```

Wenn die Datenaufbereitung abgeschlossen ist und die aufbereiteten Daten im Speicher gespeichert wurden, beenden Sie die Verwendung Ihres Apache Spark-Pools mit dem folgenden Befehl:

```python
%synapse stop
```

## <a name="create-dataset-to-represent-prepared-data"></a>Erstellen eines Datasets zur Darstellung aufbereiteter Daten

Wenn Sie die aufbereiteten Daten für das Modelltraining nutzen möchten, stellen Sie mit einem [Azure Machine Learning-Datenspeicher](how-to-access-data.md) eine Verbindung mit dem Speicher her, und geben Sie an, welche Dateien mit einem [Azure Machine Learning-Dataset](how-to-create-register-datasets.md) verwendet werden sollen.

Für das folgende Codebeispiel gilt Folgendes:

* Es wird davon ausgegangen, dass Sie bereits einen Datenspeicher erstellt haben, der eine Verbindung mit dem Speicherdienst herstellt, in dem Sie die aufbereiteten Daten gespeichert haben.  
* Der vorhandene Datenspeicher `mydatastore` wird mit der Methode „get()“ aus dem Arbeitsbereich `ws` abgerufen.
* Das [FileDataset](how-to-create-register-datasets.md#filedataset)-Element `train_ds` wird erstellt, das auf die aufbereiteten Datendateien im Verzeichnis `training_data` in `mydatastore` verweist.  
* Die Variable `input1` wird erstellt, die später zum Verfügbarmachen des Datasets `train_ds` für ein Computeziel verwendet werden kann.

```python
from azureml.core import Datastore, Dataset

datastore = Datastore.get(ws, datastore_name='mydatastore')

datastore_paths = [(datastore, '/training_data/')]
train_ds = Dataset.File.from_files(path=datastore_paths, validate=True)
input1 = train_ds.as_mount()

```
## <a name="use-a-scriptrunconfig-to-submit-an-experiment-run-to-a-synapse-spark-pool"></a>Verwenden eines `ScriptRunConfig` zum Übermitteln einer Experimentausführung an einen Synapse Spark-Pool

Sie können auch [den Synapse Spark-Cluster nutzen, den Sie zuvor als Computeziel angefügt haben](#attach-a-pool-with-the-python-sdk), um eine Experimentausführung mit einem [ScriptRunConfig](/python/api/azureml-core/azureml.core.scriptrunconfig)-Objekt zu übermitteln.

```Python
from azureml.core import RunConfiguration
from azureml.core import ScriptRunConfig 
from azureml.core import Experiment

run_config = RunConfiguration(framework="pyspark")
run_config.target = synapse_compute_name

run_config.spark.configuration["spark.driver.memory"] = "1g" 
run_config.spark.configuration["spark.driver.cores"] = 2 
run_config.spark.configuration["spark.executor.memory"] = "1g" 
run_config.spark.configuration["spark.executor.cores"] = 1 
run_config.spark.configuration["spark.executor.instances"] = 1 

run_config.environment.python.conda_dependencies = conda_dep

script_run_config = ScriptRunConfig(source_directory = './code',
                                    script= 'dataprep.py',
                                    arguments = ["--tabular_input", input1, 
                                                 "--file_input", input2,
                                                 "--output_dir", output],
                                    run_config = run_config)
```

Nachdem Ihr `ScriptRunConfig`-Objekt eingerichtet wurde, können Sie die Ausführung übermitteln.

```python
from azureml.core import Experiment 

exp = Experiment(workspace=ws, name="synapse-spark") 
run = exp.submit(config=script_run_config) 
run
```
Weitere Details, z. B. das in diesem Beispiel verwendete `dataprep.py`-Skript, finden Sie im [Beispielnotebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/azure-synapse/spark_session_on_synapse_spark_pool.ipynb).

## <a name="example-notebooks"></a>Beispielnotebooks

In diesem [Beispielnotebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/azure-synapse/spark_session_on_synapse_spark_pool.ipynb) finden Sie weitere Konzepte und Demonstrationen der Azure Synapse Analytics- und Azure Machine Learning-Integrationsfunktionen.

## <a name="next-steps"></a>Nächste Schritte

* [Trainieren eines Modells](how-to-set-up-training-targets.md).
* [Trainieren mit Azure Machine Learning-Datasets](how-to-train-with-datasets.md)
