---
title: Datenaufbereitung mit Apache Spark-Pools (Vorschau)
titleSuffix: Azure Machine Learning
description: Hier erfahren Sie, wie Sie Apache Spark-Pools für die Datenaufbereitung mit Azure Synapse Analytics und Azure Machine Learning anfügen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: nibaccam
author: nibaccam
ms.reviewer: nibaccam
ms.date: 03/02/2021
ms.custom: how-to, devx-track-python, data4ml
ms.openlocfilehash: 22945cdaff2696a15d5b119bd0f32fd0a179ebf7
ms.sourcegitcommit: dda0d51d3d0e34d07faf231033d744ca4f2bbf4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102202092"
---
# <a name="attach-apache-spark-pools-powered-by-azure-synapse-analytics-for-data-preparation-preview"></a>Anfügen von Apache Spark-Pools (unterstützt von Azure Synapse Analytics) für die Datenaufbereitung (Vorschau)

In diesem Artikel erfahren Sie, wie Sie einen von [Azure Synapse Analytics](/synapse-analytics/overview-what-is.md) unterstützten Apache Spark-Pool für die Datenaufbereitung anfügen und starten. 

>[!IMPORTANT]
> Die Integration von Azure Machine Learning und Azure Synapse Analytics befindet sich in der Vorschauphase. Die in diesem Artikel vorgestellten Funktionen verwenden das Paket `azureml-synapse`, das [experimentelle](/python/api/overview/azure/ml/?preserve-view=true&view=azure-ml-py#stable-vs-experimental) Previewfunktionen enthält, die sich jederzeit ändern können.

## <a name="azure-machine-learning-and-azure-synapse-analytics-integration-preview"></a>Integration von Azure Machine Learning und Azure Synapse Analytics (Vorschau)

Die Integration von Azure Synapse Analytics in Azure Machine Learning (Vorschau) ermöglicht Ihnen das Anfügen eines von Azure Synapse unterstützten Apache Spark-Pools für die interaktive Datenuntersuchung und -aufbereitung. Dank dieser Integration können Sie eine dedizierte Computeressource für die Datenaufbereitung im großen Stil innerhalb des gleichen Python-Notebooks nutzen, das Sie auch zum Trainieren Ihrer Machine Learning-Modelle verwenden.

## <a name="prerequisites"></a>Voraussetzungen

* [Erstellen Sie einen Azure Machine Learning-Arbeitsbereich](how-to-manage-workspace.md?tabs=python).

* [Erstellen eines Azure Synapse Analytics-Arbeitsbereichs im Azure-Portal](../synapse-analytics/quickstart-create-workspace.md)

* [Erstellen eines Apache Spark-Pools über das Azure-Portal, Webtools oder Synapse Studio](../synapse-analytics/quickstart-create-apache-spark-pool-portal.md)

* [Installieren des Azure Machine Learning Python SDK](/python/api/overview/azure/ml/install?preserve-view=true&view=azure-ml-py), das das Paket `azureml-synapse` (Vorschau) enthält. 
    * Sie können das Paket auch selbst installieren, es ist jedoch nur mit der SDK-Version 1.20 oder höheren Versionen kompatibel. 
        ```python
        pip install azureml-synapse
        ```

## <a name="link-machine-learning-workspace-and-synapse-analytics-assets"></a>Verknüpfen des Machine Learning-Arbeitsbereichs mit Synapse Analytics-Ressourcen

Bevor Sie einen Apache Synapse Spark-Pool für die Datenaufbereitung anfügen können, muss der Azure Machine Learning-Arbeitsbereich mit Ihrem Azure Synapse Analytics-Arbeitsbereich verknüpft werden. 

Sie können Ihren Machine Learning-Arbeitsbereich und den Synapse Analytics-Arbeitsbereich über das [Python SDK](#link-sdk) oder [Azure Machine Learning Studio](#link-studio) verknüpfen. 

> [!IMPORTANT]
> Um erfolgreich eine Verknüpfung mit dem Azure Synapse Analytics-Arbeitsbereich herzustellen, benötigen Sie die Rolle **Besitzer** im Azure Synapse Analytics-Arbeitsbereich. Überprüfen Sie Ihren Zugriff im [Azure-Portal](https://ms.portal.azure.com/).
>
> Wenn Sie kein **Besitzer** des Azure Synapse Analytics-Arbeitsbereichs sind, aber einen vorhandenen verknüpften Dienst verwenden möchten, lesen Sie die Informationen unter [Abrufen eines vorhandenen verknüpften Diensts](#get-an-existing-linked-service).


<a name="link-sdk"></a>
### <a name="link-workspaces-with-the-python-sdk"></a>Verknüpfen von Arbeitsbereichen mit dem Python SDK

Der folgende Code verwendet die Klassen [`LinkedService`](/python/api/azureml-core/azureml.core.linked_service.linkedservice?preserve-view=true&view=azure-ml-py) und [`SynapseWorkspaceLinkedServiceConfiguration`](/python/api/azureml-core/azureml.core.linked_service.synapseworkspacelinkedserviceconfiguration?preserve-view=true&view=azure-ml-py). 

* Verknüpfen Sie Ihren Azure Machine Learning-Arbeitsbereich `ws` mit dem Azure Synapse Analytics-Arbeitsbereich. 
* Registrieren Sie Ihren Azure Synapse Analytics-Arbeitsbereich als verknüpften Dienst bei Azure Machine Learning.

``` python
import datetime  
from azureml.core import Workspace, LinkedService, SynapseWorkspaceLinkedServiceConfiguration

# Azure Machine Learning workspace
ws = Workspace.from_config()

#link configuration 
synapse_link_config = SynapseWorkspaceLinkedServiceConfiguration(
    subscription_id=ws.subscription_id,
    resource_group= 'your resource group',
    name='mySynapseWorkspaceName')

# Link workspaces and register Synapse workspace in Azure Machine Learning
linked_service = LinkedService.register(workspace = ws,              
                                            name = 'synapselink1',    
                                            linked_service_config = synapse_link_config)
```
> [!IMPORTANT] 
> Eine verwaltete Identität (`system_assigned_identity_principal_id`) wird für jeden verknüpften Dienst erstellt. Dieser verwalteten Identität muss die Rolle **Synapse Apache Spark-Administrator** des Azure Synapse Analytics-Arbeitsbereichs zugewiesen werden, bevor Sie die Apache Spark-Sitzung starten. [Weisen Sie die Rolle „Synapse Apache Spark-Administrator“ der verwalteten Identität in Synapse Studio zu.](../synapse-analytics/security/how-to-manage-synapse-rbac-role-assignments.md)
>
> Verwenden Sie `LinkedService.get('<your-mlworkspace-name>', '<linked-service-name>')` zum Ermitteln von `system_assigned_identity_principal_id` eines bestimmten verknüpften Diensts.

<a name="link-studio"></a>
### <a name="link-workspaces-via-studio"></a>Verknüpfen von Arbeitsbereichen über Studio

Verknüpfen Sie mit den folgenden Schritten den Azure Machine Learning-Arbeitsbereich und den Azure Synapse Analytics-Arbeitsbereich über Azure Machine Learning Studio: 

1. Melden Sie sich bei [Azure Machine Learning Studio](https://ml.azure.com/) an.
1. Wählen Sie im linken Bereich im Abschnitt **Verwalten** die Option **Verknüpfte Dienste** aus.
1. Wählen Sie **Integration hinzufügen** aus.
1. Füllen Sie die Felder im Formular **Arbeitsbereich verknüpfen** aus.

   |Feld| BESCHREIBUNG    
   |---|---
   |Name| Geben Sie einen Namen für Ihren verknüpften Dienst ein. Dieser Name wird verwendet, um auf diesen bestimmten verknüpften Dienst zu verweisen.
   |Abonnementname | Wählen Sie den Namen Ihres Abonnements aus, das Ihrem Machine Learning-Arbeitsbereich zugeordnet ist. 
   |Synapse-Arbeitsbereich | Wählen Sie den Synapse-Arbeitsbereich aus, mit dem Sie eine Verknüpfung herstellen möchten. 
   
1. Wählen Sie **Weiter** aus, um das Formular **Spark-Pools auswählen (optional)** zu öffnen. Wählen Sie in diesem Formular aus, welcher Synapse Apache Spark-Pool an Ihren Arbeitsbereich angefügt werden soll.

1. Wählen Sie **Weiter** aus, um das Formular **Überprüfung** zu öffnen und Ihre Auswahl zu überprüfen. 
1. Wählen Sie **Erstellen** aus, um die Erstellung des verknüpften Diensts abzuschließen.

## <a name="get-an-existing-linked-service"></a>Abrufen eines vorhandenen verknüpften Diensts

Zum Abrufen und Verwenden eines vorhandenen verknüpften Diensts sind Berechtigungen vom Typ **Benutzer oder Mitwirkender** für den Azure Synapse Analytics-Arbeitsbereich erforderlich.

In diesem Beispiel wird ein vorhandener verknüpfter Dienst (`synapselink1`) aus dem Arbeitsbereich `ws` mit der Methode [`get()`](/python/api/azureml-core/azureml.core.linkedservice?preserve-view=true&view=azure-ml-py#get-workspace--name-) abgerufen.
```python
linked_service = LinkedService.get(ws, 'synapselink1')
```

### <a name="manage-linked-services"></a>Verwalten von verknüpften Diensten

Verwenden Sie die Methode `unregister()`, um die Verknüpfung von Arbeitsbereichen aufzuheben.

``` python
linked_service.unregister()
```

Zeigen Sie alle verknüpften Dienste an, die Ihrem Machine Learning-Arbeitsbereich zugeordnet sind. 

```python
LinkedService.list(ws)
```
 
## <a name="attach-synapse-spark-pool-as-a-compute"></a>Anfügen eines Synapse Spark-Pools als Computeressource

Wenn Ihre Arbeitsbereiche verknüpft sind, fügen Sie einen Synapse Apache Spark-Pool als dedizierte Computeressource für Ihre Datenaufbereitungsaufgaben an. 

Sie können Apache Spark-Pools über folgende Komponenten anfügen:
* Azure Machine Learning Studio
* [Azure Resource Manager-Vorlagen](https://github.com/Azure/azure-quickstart-templates/blob/master/101-machine-learning-linkedservice-create/azuredeploy.json)
* Python SDK 

Führen Sie die folgenden Schritte aus, um einen Apache Spark-Pool mithilfe von Studio anzufügen: 

1. Melden Sie sich bei [Azure Machine Learning Studio](https://ml.azure.com/) an.
1. Wählen Sie im linken Bereich im Abschnitt **Verwalten** die Option **Verknüpfte Dienste** aus.
1. Wählen Sie Ihren Synapse-Arbeitsbereich aus.
1. Wählen Sie oben links **Angefügte Spark-Pools** aus. 
1. Wählen Sie **Anfügen** aus. 
1. Wählen Sie Ihren Apache Spark-Pool in der Liste aus, und geben Sie einen Namen an.  
    1. Diese Liste gibt Aufschluss über die verfügbaren Synapse Spark-Pools, die an Ihre Computeressource angefügt werden können. 
    1. Informationen zum Erstellen eines neuen Synapse Spark-Pools finden Sie unter [Erstellen eines Apache Spark-Pools mit Synapse Studio](../synapse-analytics/quickstart-create-apache-spark-pool-portal.md).
1. Wählen Sie **Attach selected** (Ausgewählte anfügen) aus. 


Sie können einen Apache Spark-Pool auch mithilfe des **Python SDK** anfügen. 

Mit dem unten genannten Code werden die folgenden Schritte ausgeführt: 
1. Konfigurieren von SynapseCompute mit

   1. dem verknüpften Dienst `linked_service`, den Sie im vorherigen Schritt erstellt oder abgerufen haben 
   1. dem Typ des Computeziels, das Sie anfügen möchten: `SynapseSpark`
   1. dem Namen des Apache Spark-Pools. Dieser muss einem vorhandenen Apache Spark-Pool entsprechen, der sich in Ihrem Azure Synapse Analytics-Arbeitsbereich befindet.
   
1. Erstellen eines Machine Learning-Elements vom Typ „ComputeTarget“ durch Übergabe 
   1. des Machine Learning-Arbeitsbereichs, den Sie verwenden möchten: `ws`
   1. des Namens, mit dem Sie im Azure Machine Learning-Arbeitsbereich auf die Computeressource verweisen möchten 
   1. der Anfügekonfiguration (attach_configuration), die Sie beim Konfigurieren der Synapse-Computeressource angegeben haben
       1. Der Aufruf von „ComputeTarget.attach()“ ist asynchron, daher wird das Beispiel blockiert, bis der Aufruf abgeschlossen ist.

```python
from azureml.core.compute import SynapseCompute, ComputeTarget

attach_config = SynapseCompute.attach_configuration(linked_service, #Linked synapse workspace alias
                                                    type='SynapseSpark', #Type of assets to attach
                                                    pool_name="<Synapse Spark pool name>") #Name of Synapse spark pool 

synapse_compute = ComputeTarget.attach(workspace= ws,                
                                       name='<Synapse Spark pool alias in Azure ML>', 
                                       attach_configuration=attach_config
                                      )

synapse_compute.wait_for_completion()
```

Überprüfen Sie, ob der Apache Spark-Pool angefügt wurde.

```python
ws.compute_targets['Synapse Spark pool alias']
```

## <a name="launch-synapse-spark-pool-for-data-preparation-tasks"></a>Starten des Synapse Spark-Pools für Datenaufbereitungsaufgaben

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

Geben Sie zum Starten der Datenaufbereitung mit dem Apache Spark-Pool den Namen des Apache Spark-Pools sowie Ihre Abonnement-ID, die Ressourcengruppe des Machine Learning-Arbeitsbereichs, den Namen des Machine Learning-Arbeitsbereichs und die während der Apache Spark-Sitzung zu verwendende Umgebung an. 

> [!IMPORTANT]
> Wenn Sie den Apache Spark-Pool weiterhin verwenden möchten, müssen Sie angeben, welche Computeressource in den Datenaufbereitungsaufgaben verwendet werden soll. Verwenden Sie `%synapse` für einzelne Codezeilen und `%%synapse` für mehrere Zeilen. 

```python
%synapse start -c SynapseSparkPoolAlias -s AzureMLworkspaceSubscriptionID -r AzureMLworkspaceResourceGroupName -w AzureMLworkspaceName -e myenv
```

Nach dem Start der Sitzung können Sie die Metadaten der Sitzung überprüfen.

```python
%synapse meta
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
sc._jsc.hadoopConfiguration().set("fs.azure.sas.<container name>.<storage account name>.blob.core.windows.net", "sas token")

# read from blob 
df = spark.read.option("header", "true").csv("wasbs://demo@dprepdata.blob.core.windows.net/Titanic.csv")
```

Der folgende Code veranschaulicht das Einlesen von Daten aus **Azure Data Lake Storage Generation 1 (ADLS Gen 1)** mit den Anmeldeinformationen Ihres Dienstprinzipals: 

```python

# setup service principal which has access of the data
sc._jsc.hadoopConfiguration().set("fs.adl.account.<storage account name>.oauth2.access.token.provider.type","ClientCredential")

sc._jsc.hadoopConfiguration().set("fs.adl.account.<storage account name>.oauth2.client.id", "<client id>")

sc._jsc.hadoopConfiguration().set("fs.adl.account.<storage account name>.oauth2.credential", "<client secret>")

sc._jsc.hadoopConfiguration().set("fs.adl.account.<storage account name>.oauth2.refresh.url",
https://login.microsoftonline.com/<tenant id>/oauth2/token)

df = spark.read.csv("adl://<storage account name>.azuredatalakestore.net/<path>")

```

Der folgende Code veranschaulicht das Einlesen von Daten aus **Azure Data Lake Storage Generation 2 (ADLS Gen 2)** mit den Anmeldeinformationen Ihres Dienstprinzipals: 

```python
# setup service principal which has access of the data
sc._jsc.hadoopConfiguration().set("fs.azure.account.auth.type.<storage account name>.dfs.core.windows.net","OAuth")
sc._jsc.hadoopConfiguration().set("fs.azure.account.oauth.provider.type.<storage account name>.dfs.core.windows.net", "org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider")
sc._jsc.hadoopConfiguration().set("fs.azure.account.oauth2.client.id.<storage account name>.dfs.core.windows.net", "<client id>")
sc._jsc.hadoopConfiguration().set("fs.azure.account.oauth2.client.secret.<storage account name>.dfs.core.windows.net", "<client secret>")
sc._jsc.hadoopConfiguration().set("fs.azure.account.oauth2.client.endpoint.<storage account name>.dfs.core.windows.net",
https://login.microsoftonline.com/<tenant id>/oauth2/token)


df = spark.read.csv("abfss://<container name>@<storage account>.dfs.core.windows.net/<path>")

```

### <a name="read-in-data-from-registered-datasets"></a>Einlesen von Daten aus registrierten Datasets

Sie können auch ein vorhandenes registriertes Dataset in Ihrem Arbeitsbereich abrufen und die Datenaufbereitung durchführen, indem Sie es in einen Spark-Datenrahmen konvertieren.  

Im folgenden Beispiel wird das registrierte Tabellendataset (TabularDataset) `blob_dset` abgerufen, das auf Dateien im Blobspeicher verweist, und in einen Spark-Datenrahmen konvertiert. Beim Konvertieren Ihrer Datasets in einen Spark-Datenrahmen können Sie die `pyspark`-Datenuntersuchung und -Vorbereitungsbibliotheken nutzen.  

``` python

%%synapse
from azureml.core import Workspace, Dataset

dset = Dataset.get_by_name(ws, "blob_dset")
spark_df = dset.to_spark_dataframe()
```

## <a name="perform-data-preparation-tasks"></a>Ausführen von Datenaufbereitungsaufgaben

Nach dem Abrufen und Untersuchen Ihrer Daten können Sie Datenaufbereitungsaufgaben ausführen.

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

## <a name="next-steps"></a>Nächste Schritte

* [Trainieren eines Modells](how-to-set-up-training-targets.md).
* [Trainieren mit Azure Machine Learning-Datasets](how-to-train-with-datasets.md)
* [Erstellen Sie ein Azure Machine Learning-Dataset](how-to-create-register-datasets.md).