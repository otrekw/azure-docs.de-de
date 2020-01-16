---
title: Zugreifen auf Daten in Azure Storage-Diensten
titleSuffix: Azure Machine Learning
description: Hier erfahren Sie, wie Sie mithilfe von Datenspeichern während des Trainings mit Azure Machine Learning eine sichere Verbindung zu Azure Storage-Diensten herstellen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: ylxiong
author: YLXiong1125
ms.reviewer: nibaccam
ms.date: 12/10/2019
ms.custom: seodec18
ms.openlocfilehash: ac6ef6341013ca13d5a9f27be8897365c1c2155d
ms.sourcegitcommit: ce4a99b493f8cf2d2fd4e29d9ba92f5f942a754c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/28/2019
ms.locfileid: "75535450"
---
# <a name="access-data-in-azure-storage-services"></a>Zugreifen auf Daten in Azure Storage-Diensten
[!INCLUDE [aml-applies-to-basic-enterprise-sku](../../includes/aml-applies-to-basic-enterprise-sku.md)]

In diesem Artikel erfahren Sie, wie Sie auf einfache Weise über Azure Machine Learning-Datenspeicher auf Ihre Daten in Azure Storage-Diensten zugreifen können. Datenspeicher werden zum Speichern von Verbindungsinformationen wie z. B. Ihrer Abonnement-ID und Tokenautorisierung verwendet. Mithilfe von Datenspeichern können Sie auf Ihren Speicher zugreifen, ohne die Verbindungsinformationen in Ihren Skripts hartcodieren zu müssen. 

Aus diesen [Azure Storage-Lösungen](#matrix) können Sie Datenspeicher erstellen. Für nicht unterstützte Speicherlösungen sowie um bei Machine Learning-Experimenten Kosten für ausgehende Daten zu sparen, empfiehlt es sich, [Ihre Daten in unterstützten Azure Storage-Lösungen zu verschieben](#move). 

## <a name="prerequisites"></a>Voraussetzungen
Sie benötigen Folgendes:
- ein Azure-Abonnement Wenn Sie kein Azure-Abonnement besitzen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie die [kostenlose oder kostenpflichtige Version von Azure Machine Learning](https://aka.ms/AMLFree) aus.

- Ein Azure-Speicherkonto mit einem [Azure-Blobcontainer](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-overview) oder einer [Azure-Dateifreigabe](https://docs.microsoft.com/azure/storage/files/storage-files-introduction).

- Das [Azure Machine Learning SDK für Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py), oder greifen Sie auf [Azure Machine Learning-Studio](https://ml.azure.com/) zu.

- Ein Azure Machine Learning-Arbeitsbereich.
  
  Sie können wahlweise [einen Azure Machine Learning-Arbeitsbereich erstellen](how-to-manage-workspace.md) oder mithilfe des Python SDKs einen vorhandenen verwenden:

   ```Python
   import azureml.core
   from azureml.core import Workspace, Datastore
        
   ws = Workspace.from_config()
   ```

<a name="access"></a>

## <a name="create-and-register-datastores"></a>Erstellen und Registrieren von Datenspeichern

Wenn Sie eine Azure Storage-Lösung als Datenspeicher registrieren, erstellen Sie diesen Datenspeicher automatisch in einem bestimmten Arbeitsbereich. Sie können Datenspeicher über das Python SDK oder über Azure Machine Learning-Studio in einem Arbeitsbereich registrieren.

### <a name="python-sdk"></a>Python SDK

Alle Registriermethoden befinden sich in der [`Datastore`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py)-Klasse und weisen das Format `register_azure_*` auf.

Die Informationen, die Sie zum Auffüllen der `register()`-Methode benötigen, finden Sie im [Azure-Portal](https://portal.azure.com):

1. Wählen Sie im linken Bereich **Speicherkonten** und dann das Speicherkonto aus, das Sie registrieren möchten. 
2. Informationen wie Kontoname, Container und Name der Dateifreigabe finden Sie auf der Seite **Übersicht**. Zum Abrufen von Authentifizierungsinformationen, wie dem Kontoschlüssel oder dem SAS-Token, navigieren Sie im Bereich **Einstellungen** zu **Kontoschlüssel**. 

> [!IMPORTANT]
> Wenn sich Ihr Speicherkonto in einem virtuellen Netzwerk befindet, wird nur die Erstellung eines Azure-Blobdatenspeichers unterstützt. Legen Sie den-Parameter `grant_workspace_access` auf `True` fest, um für Ihren Arbeitsbereich Zugriff auf Ihr Speicherkonto zu gewähren.

In den folgenden Beispielen wird gezeigt, wie Sie einen Azure-Blobcontainer, eine Azure-Dateifreigabe und Azure SQL-Daten als Datenspeicher registrieren können.

#### <a name="blob-container"></a>BLOB-Container

Um einen Azure-Blobcontainer als Datenspeicher zu registrieren, verwenden Sie [`register_azure_blob-container()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#register-azure-blob-container-workspace--datastore-name--container-name--account-name--sas-token-none--account-key-none--protocol-none--endpoint-none--overwrite-false--create-if-not-exists-false--skip-validation-false--blob-cache-timeout-none--grant-workspace-access-false--subscription-id-none--resource-group-none-).

Der folgende Code erstellt den Datenspeicher `blob_datastore_name` und registriert ihn im Arbeitsbereich `ws`. Dieser Datenspeicher greift auf den `my-container-name`-Blobcontainer im `my-account-name`-Speicherkonto zu und verwendet dazu den angegebenen Kontoschlüssel.

```Python
blob_datastore_name='azblobsdk' # Name of the datastore to workspace
container_name=os.getenv("BLOB_CONTAINER", "<my-container-name>") # Name of Azure blob container
account_name=os.getenv("BLOB_ACCOUNTNAME", "<my-account-name>") # Storage account name
account_key=os.getenv("BLOB_ACCOUNT_KEY", "<my-account-key>") # Storage account key

blob_datastore = Datastore.register_azure_blob_container(workspace=ws, 
                                                         datastore_name=blob_datastore_name, 
                                                         container_name=container_name, 
                                                         account_name=account_name,
                                                         account_key=account_key)
```

#### <a name="file-share"></a>Dateifreigabe

Um einen eine Azure-Dateifreigabe als Datenspeicher zu registrieren, verwenden Sie [`register_azure_file_share()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#register-azure-file-share-workspace--datastore-name--file-share-name--account-name--sas-token-none--account-key-none--protocol-none--endpoint-none--overwrite-false--create-if-not-exists-false--skip-validation-false-). 

Der folgende Code erstellt den Datenspeicher `file_datastore_name` und registriert ihn im Arbeitsbereich `ws`. Dieser Datenspeicher greift auf die `my-fileshare-name`-Dateifreigabe im `my-account-name`-Speicherkonto zu und verwendet dazu den angegebenen Kontoschlüssel.

```Python
file_datastore_name='azfilesharesdk' # Name of the datastore to workspace
file_share_name=os.getenv("FILE_SHARE_CONTAINER", "<my-fileshare-name>") # Name of Azure file share container
account_name=os.getenv("FILE_SHARE_ACCOUNTNAME", "<my-account-name>") # Storage account name
account_key=os.getenv("FILE_SHARE_ACCOUNT_KEY", "<my-account-key>") # Storage account key

file_datastore = Datastore.register_azure_file_share(workspace=ws,
                                                 datastore_name=file_datastore_name, 
                                                 file_share_name=file_share_name, 
                                                 account_name=account_name,
                                                 account_key=account_key)
```

#### <a name="sql-data"></a>SQL data

Bei einem Azure SQL-Datenspeicher verwenden Sie [register_azure_sql_database()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore.datastore?view=azure-ml-py#register-azure-sql-database-workspace--datastore-name--server-name--database-name--tenant-id-none--client-id-none--client-secret-none--resource-url-none--authority-url-none--endpoint-none--overwrite-false--username-none--password-none-) zum Registrieren eines Datenspeichers mit Anmeldeinformationen, der mit einer Azure SQL-Datenbank mit SQL-Authentifizierungs- oder Dienstprinzipalberechtigungen verbunden ist. 

Registrieren über die SQL-Authentifizierung:

```python
sql_datastore_name="azsqlsdksql"
server_name=os.getenv("SQL_SERVERNAME", "<my-server-name>") # Name of the Azure SQL server
database_name=os.getenv("SQL_DATBASENAME", "<my-database-name>") # Name of the Azure SQL database
username=os.getenv("SQL_USER_NAME", "<my-sql-user-name>") # Username of the database user to access the database
password=os.getenv("SQL_USER_PASSWORD", "<my-sql-user-password>") # Password of the database user to access the database

sql_datastore = Datastore.register_azure_sql_database(workspace=ws,
                                                  datastore_name=sql_datastore_name,
                                                  server_name=server_name,
                                                  database_name=database_name,
                                                  username=username,
                                                  password=password)

```

Registrieren über ein Dienstprinzipal:

```python 
sql_datastore_name="azsqlsdksp"
server_name=os.getenv("SQL_SERVERNAME", "<my-server-name>") # Name of the SQL server
database_name=os.getenv("SQL_DATBASENAME", "<my-database-name>") # Name of the SQL database
client_id=os.getenv("SQL_CLIENTNAME", "<my-client-id>") # Client ID of the service principal with permissions to access the database
client_secret=os.getenv("SQL_CLIENTSECRET", "<my-client-secret>") # Secret of the service principal
tenant_id=os.getenv("SQL_TENANTID", "<my-tenant-id>") # Tenant ID of the service principal

sql_datastore = Datastore.register_azure_sql_database(workspace=ws,
                                                      datastore_name=sql_datastore_name,
                                                      server_name=server_name,
                                                      database_name=database_name,
                                                      client_id=client_id,
                                                      client_secret=client_secret,
                                                      tenant_id=tenant_id)
```

#### <a name="storage-guidance"></a>Leitfaden für Speicher

Wir empfehlen einen Azure-Blobcontainer. Für Blobs stehen sowohl der Standard- als auch der Premiumspeicher zur Verfügung. Der Premiumspeicher ist zwar teurer, ermöglicht aber höhere Durchsätze. Dies kann sich vor allem bei einem Training mit einem großen Dataset positiv auf die Ausführungsgeschwindigkeit auswirken. Informationen zu den Kosten für Speicherkonten finden Sie unter [Azure-Preisrechner](https://azure.microsoft.com/pricing/calculator/?service=machine-learning-service).

### <a name="azure-machine-learning-studio"></a>Azure Machine Learning Studio 

Erstellen Sie einen neuen Datenspeicher in wenigen Schritten in Azure Machine Learning-Studio:

1. Melden Sie sich bei [Azure Machine Learning Studio](https://ml.azure.com/) an.
1. Wählen Sie im linken Bereich unter **Verwalten** die Option **Datenspeicher** aus.
1. Wählen Sie **+ Neuer Datenspeicher** aus.
1. Füllen Sie das Formular für einen neuen Datenspeicher aus. Das Formular aktualisiert sich ausgehend von den gewählten Optionen für den Azure Storage-Typ und den Authentifizierungstyp intelligent selbst.
  
Die Informationen, die Sie zum Auffüllen des Formulars benötigen, finden Sie im [Azure-Portal](https://portal.azure.com). Wählen Sie im linken Bereich **Speicherkonten** und dann das Speicherkonto aus, das Sie registrieren möchten. Die Seite **Übersicht** enthält Informationen wie den Kontonamen und den Namen des Containers oder der Dateifreigabe. Zum Abrufen von Authentifizierungselementen, wie dem Kontoschlüssel oder dem SAS-Token, navigieren Sie im Bereich **Einstellungen** zu **Kontoschlüssel**.

Das folgende Beispiel veranschaulicht das Aussehen des Formulars, wenn Sie einen Azure-Blobdatenspeicher erstellen: 
    
![Formular für einen neuen Datenspeicher](media/how-to-access-data/new-datastore-form.png)


<a name="get"></a>

## <a name="get-datastores-from-your-workspace"></a>Abrufen von Datenspeichern aus Ihrem Arbeitsbereich

Verwenden Sie die statische Methode [`get()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#get-workspace--datastore-name-) mit der `Datastore`-Klasse, um einen bestimmten Datenspeicher im aktuellen Arbeitsbereich zu registrieren:

```Python
# Get a named datastore from the current workspace
datastore = Datastore.get(ws, datastore_name='your datastore name')
```
Um die Liste der in einem bestimmten Arbeitsbereich registrierten Datenspeicher abzurufen, können Sie die [`datastores`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace%28class%29?view=azure-ml-py#datastores)-Eigenschaft für ein Arbeitsbereichsobjekt verwenden:

```Python
# List all datastores registered in the current workspace
datastores = ws.datastores
for name, datastore in datastores.items():
    print(name, datastore.datastore_type)
```

Wenn Sie einen Arbeitsbereich erstellen, werden ein Azure-Blobcontainer und eine Azure-Dateifreigabe automatisch im Arbeitsbereich registriert. Sie erhalten die Namen `workspaceblobstore` und `workspacefilestore`. Darin werden die Verbindungsinformationen des Blobcontainers und der Dateifreigabe, die im Speicherkonto bereitgestellt wird, das mit dem Arbeitsbereich verbunden ist, gespeichert. Der `workspaceblobstore`-Container wird als Standarddatenspeicher festgelegt.

Über diese Zeile rufen Sie den Standarddatenspeicher des Arbeitsbereichs ab:

```Python
datastore = ws.get_default_datastore()
```

Verwenden Sie die Methode [`set_default_datastore()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py#set-default-datastore-name-) mit dem Arbeitsbereichsobjekt, um einen anderen Standarddatenspeicher für den aktuellen Arbeitsbereich zu definieren:

```Python
# Define the default datastore for the current workspace
ws.set_default_datastore('your datastore name')
```

<a name="up-and-down"></a>
## <a name="upload-and-download-data"></a>Hochladen und Herunterladen von Daten

Die in den folgenden Beispielen beschriebenen [`upload()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py#upload-src-dir--target-path-none--overwrite-false--show-progress-true-)- und [`download()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py#download-target-path--prefix-none--overwrite-false--show-progress-true-)-Methoden sind für die [AzureBlobDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py)- und [AzureFileDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azurefiledatastore?view=azure-ml-py)-Klassen bestimmt und werden identisch ausgeführt.

### <a name="upload"></a>Upload

Laden Sie entweder ein Verzeichnis oder einzelne Dateien mit dem Python SDK in den Datenspeicher hoch:

```Python
datastore.upload(src_dir='your source directory',
                 target_path='your target path',
                 overwrite=True,
                 show_progress=True)
```

Der Parameter `target_path` gibt den Speicherort in der Dateifreigabe (oder im Blobcontainer) für den Upload an. Standardmäßig wird `None`verwendet, sodass die Daten in das Stammverzeichnis hochgeladen werden. Bei `overwrite=True` werden alle in `target_path` vorhandenen Daten überschrieben.

Alternativ können Sie eine Liste einzelner Dateien mithilfe der `upload_files()`-Methode in den Datenspeicher hochladen.

### <a name="download"></a>Download

Laden Sie die Daten aus einem Datenspeicher in Ihr lokales Dateisystem herunter.

```Python
datastore.download(target_path='your target path',
                   prefix='your prefix',
                   show_progress=True)
```

Der Parameter `target_path` ist der Speicherort des lokalen Verzeichnisses, in das die Daten heruntergeladen werden sollen. Um einen Pfad zu dem Ordner in der Dateifreigabe (oder im Blobcontainer) für den Download anzugeben, geben Sie diesen Pfad mit `prefix` an. Wenn `prefix` gleich `None` ist, werden alle Inhalte Ihrer Dateifreigabe (oder Ihres Blobcontainers) heruntergeladen.

<a name="train"></a>
## <a name="access-your-data-during-training"></a>Zugreifen auf Ihre Daten während des Trainings

> [!IMPORTANT]
> Wir empfehlen jetzt die Verwendung von [Azure Machine Learning-Datasets](how-to-create-register-datasets.md) für den Zugriff auf Ihre Daten im Training. Datasets stellen Funktionen bereit, die Tabellendaten Daten in einen Pandas-oder Spark-Datenrahmen laden. Mithilfe von Datasets können Sie auch Dateien beliebiger Formate aus Azure Blob Storage, Azure Files, Azure Data Lake Storage Gen1, Azure Data Lake Storage Gen2, Azure SQL-Datenbank und Azure Database for PostgreSQL herunterladen oder einbinden. [Erfahren Sie mehr über das Trainieren mit Datasets](how-to-train-with-datasets.md).

Die folgende Tabelle listet die Methoden auf, die dem Computeziel mitteilen, wie der Datenspeicher während der Ausführungen verwendet wird: 

Weg|Methode|BESCHREIBUNG|
----|-----|--------
Einbinden| [`as_mount()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.abstractazurestoragedatastore?view=azure-ml-py#as-mount--)| Wird zum Einbinden des Datenspeichers auf dem Computeziel verwendet. Wenn der Datenspeicher eingebunden ist, werden alle Dateien Ihres Datenspeicher für Ihr Computeziel zugänglich gemacht.
Download|[`as_download()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.abstractazurestoragedatastore?view=azure-ml-py#as-download-path-on-compute-none-)|Wird zum Herunterladen der Inhalte Ihres Datenspeichers in den unter `path_on_compute` angegebenen Speicherort verwendet. <br><br> Dieser Download findet vor der Ausführung statt.
Upload|[`as_upload()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.abstractazurestoragedatastore?view=azure-ml-py#as-upload-path-on-compute-none-)| Wird zum Hochladen einer Datei aus dem von `path_on_compute` angegebenen Speicherort in Ihren Datenspeicher verwendet. <br><br> Dieser Upload findet nach der Ausführung statt.

Um auf bestimmte Ordner oder Dateien in Ihrem Datenspeicher zu verweisen und sie im Computeziel verfügbar zu machen, verwenden Sie die Datenspeichermethode [`path()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.abstractazurestoragedatastore?view=azure-ml-py#path-path-none--data-reference-name-none-):

```Python
# To mount the full contents in your storage to the compute target
datastore.as_mount()

# To download the contents of only the `./bar` directory in your storage to the compute target
datastore.path('./bar').as_download()
```
> [!NOTE]
> Alle angegebenen `datastore` oder `datastore.path`-Objekts werden in den Namen einer Umgebungsvariablen des Formats `"$AZUREML_DATAREFERENCE_XXXX"`aufgelöst. Der Wert dieses Namens steht für den Pfad zum Einbinden/Herunterladen auf dem Computeziel. Der Datenspeicherpfad auf dem Computeziel stimmt möglicherweise nicht mit dem Ausführungspfad für das Trainingsskript überein.

### <a name="examples"></a>Beispiele 

Wir empfehlen die Verwendung der [`Estimator`](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py)-Klasse für den Zugriff auf Daten während des Trainings. 

Die `script_params`-Variable ist ein Wörterbuch, das Parameter für `entry_script` enthält. Verwenden Sie es, um einen Datenspeicher als Eingabe zu übergeben und zu beschreiben, wie Daten auf dem Computeziel verfügbar gemacht werden. Weitere Informationen finden Sie im End-to-End-[Tutorial](tutorial-train-models-with-aml.md).

```Python
from azureml.train.estimator import Estimator

# Notice that '/' is in front, which indicates the absolute path
script_params = {
    '--data_dir': datastore.path('/bar').as_mount()
}

est = Estimator(source_directory='your code directory',
                entry_script='train.py',
                script_params=script_params,
                compute_target=compute_target
                )
```

Sie können auch eine Liste von Datenspeichern an den Parameter `inputs` des `Estimator`-Konstruktors übergeben, um Daten in oder aus Ihrem Datenspeicher einzubinden oder zu kopieren. Dieses Codebeispiel:
* Lädt alle Inhalte in `datastore1` auf das Computeziel herunter, bevor das Trainingsskript `train.py` ausgeführt wird.
* Lädt den Ordner `'./foo'` in `datastore2` auf das Computeziel herunter, bevor `train.py` ausgeführt wird.
* Lädt die Datei `'./bar.pkl'` aus dem Computeziel in den `datastore3` hoch, nachdem Ihr Skript ausgeführt wurde.

```Python
est = Estimator(source_directory='your code directory',
                compute_target=compute_target,
                entry_script='train.py',
                inputs=[datastore1.as_download(), datastore2.path('./foo').as_download(), datastore3.as_upload(path_on_compute='./bar.pkl')])
```
Wenn Sie lieber ein `RunConfig`-Objekt für das Training verwenden möchten, müssen Sie ein [`DataReference`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py)-Objekt einrichten. 

Der folgende Code zeigt, wie Sie mit einem `DataReference`-Objekt in einer Schätzpipeline arbeiten. Ein vollständiges Beispiel finden Sie in diesem [Notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines/intro-to-pipelines/aml-pipelines-how-to-use-estimatorstep.ipynb).

```Python
from azureml.core import Datastore
from azureml.data.data_reference import DataReference
from azureml.pipeline.core import PipelineData

def_blob_store = Datastore(ws, "workspaceblobstore")

input_data = DataReference(
       datastore=def_blob_store,
       data_reference_name="input_data",
       path_on_datastore="20newsgroups/20news.pkl")

output = PipelineData("output", datastore=def_blob_store)
```
<a name="matrix"></a>

### <a name="compute-and-datastore-matrix"></a>Compute- und Datenspeichermatrix

Datenspeicher unterstützen derzeit das Speichern von Verbindungsinformationen in den Speicherdiensten, die in der folgenden Matrix aufgeführt sind. Diese Matrix zeigt die verfügbaren Datenzugriffsfunktionen für die verschiedenen Computeziele- und Datenspeicherszenarien. [Weitere Informationen zu den Computezielen für Azure Machine Learning.](how-to-set-up-training-targets.md#compute-targets-for-training)

|Compute|[AzureBlobDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py)                                       |[AzureFileDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azurefiledatastore?view=azure-ml-py)                                      |[AzureDataLakeDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_data_lake_datastore.azuredatalakedatastore?view=azure-ml-py) |[AzureDataLakeGen2Datastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_data_lake_datastore.azuredatalakegen2datastore?view=azure-ml-py) [AzurePostgreSqlDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_postgre_sql_datastore.azurepostgresqldatastore?view=azure-ml-py) [AzureSqlDatabaseDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_sql_database_datastore.azuresqldatabasedatastore?view=azure-ml-py) |
|--------------------------------|----------------------------------------------------------|----------------------------------------------------------|------------------------|----------------------------------------------------------------------------------------|
| Lokal|[as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-), [as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)|[as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-), [as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)|–         |–                                                                         |
| Azure Machine Learning Compute |[as_mount()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-mount--), [as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-), [as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-), [Machine Learning pipelines](concept-ml-pipelines.md)|[as_mount()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-mount--), [as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-), [as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-), [Machine Learning pipelines](concept-ml-pipelines.md)|–         |–                                                                         |
| Virtuelle Computer               |[as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-), [as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)                           | [as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-) [as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)                            |–         |–                                                                         |
| Azure HDInsight                      |[as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-) [as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)                            | [as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-) [as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)                            |–         |–                                                                         |
| Datenübertragung                  |[Machine Learning-Pipelines](concept-ml-pipelines.md)                                               |–                                           |[Machine Learning-Pipelines](concept-ml-pipelines.md)            |[Machine Learning-Pipelines](concept-ml-pipelines.md)                                                                            |
| Azure Databricks                     |[Machine Learning-Pipelines](concept-ml-pipelines.md)                                              |–                                           |[Machine Learning-Pipelines](concept-ml-pipelines.md)             |–                                                                         |
| Azure Batch                    |[Machine Learning-Pipelines](concept-ml-pipelines.md)                                               |–                                           |–         |–                                                                         |
| Azure Data Lake Analytics       |–                                           |–                                           |[Machine Learning-Pipelines](concept-ml-pipelines.md)             |–                                                                         |

> [!NOTE]
> Es gibt möglicherweise Szenarios, in denen äußerst iterative Datenverarbeitungsprozesse mit vielen Wiederholungen schneller mit `as_download()` anstelle von `as_mount()` ausgeführt werden. Dies können Sie in Experimenten überprüfen.

### <a name="accessing-source-code-during-training"></a>Zugreifen auf den Quellcode während des Trainings

Azure Blob Storage verfügt über eine höhere Durchsatzgeschwindigkeit als die Azure-Dateifreigabe und wird auf eine hohe Anzahl parallel gestarteter Aufträge skaliert. Aus diesem Grund empfiehlt es sich, Ausführungen so zu konfigurieren, dass für die Übertragung von Quellcodedateien Blobspeicher verwendet wird.

Im folgenden Codebeispiel wird in der Laufzeitkonfiguration angegeben, welcher Blobdatenspeicher für Quellcodeübertragungen verwendet werden soll.

```python 
# workspaceblobstore is the default blob storage
run_config.source_directory_data_store = "workspaceblobstore" 
```

## <a name="access-data-during-scoring"></a>Zugreifen auf Daten während der Bewertung

Azure Machine Learning bietet mehrere Möglichkeiten, Ihre Modelle zur Bewertung zu verwenden. Einige dieser Methoden bieten keinen Zugriff auf Datenspeicher. Verwenden Sie die folgende Tabelle, um zu verstehen, welche Methoden Ihnen ermöglichen, während der Bewertung auf Datenspeicher zuzugreifen:

| Methode | Datenspeicherzugriff | BESCHREIBUNG |
| ----- | :-----: | ----- |
| [Batchvorhersage](how-to-run-batch-predictions.md) | ✔ | Treffen Sie asynchron Vorhersagen für große Datenmengen. |
| [Webdienst](how-to-deploy-and-where.md) | &nbsp; | Stellen Sie Modelle als Webdienste bereit. |
| [Azure IoT Edge-Modul](how-to-deploy-and-where.md) | &nbsp; | Stellen Sie Modelle auf IoT Edge-Geräten bereit. |

Für Situationen, in denen das SDK keinen Zugriff auf Datenspeicher bietet, können Sie möglicherweise benutzerdefinierten Code mit dem entsprechenden Azure-SDK erstellen, um auf die Daten zuzugreifen. Das [Azure Storage SDK für Python](https://github.com/Azure/azure-storage-python) ist beispielsweise eine Clientbibliothek, mit der Sie auf in Blobs oder Dateien gespeicherte Daten zugreifen können.

<a name="move"></a>
## <a name="move-data-to-supported-azure-storage-solutions"></a>Verschieben von Daten in unterstützte Azure Storage-Lösungen

Azure Machine Learning unterstützt den Zugriff auf Daten aus Azure Blob Storage, Azure Files, Azure Data Lake Storage Gen1, Azure Data Lake Storage Gen2, einer Azure SQL-Datenbank und Azure Database for PostgreSQL. Wenn Sie einen nicht unterstützten Speicher verwenden, empfehlen wir Ihnen, Ihre Daten mithilfe der Azure Data Factory auf unterstützte Azure Storage-Lösungen zu verschieben. Durch das Verschieben von Daten auf einen unterstützten Speicher können Sie bei Machine Learning-Experimenten Kosten für die Datenausgabe sparen. 

Azure Data Factory bietet eine effiziente und robuste Datenübertragung mit mehr als 80 vorkonfigurierten Connectors ohne zusätzliche Kosten. Diese Connectors umfassen Azure-Datendienste, lokale Datenquellen, Amazon S3 und Redshift sowie Google BigQuery. [Befolgen Sie die Schritt-für-Schritt-Anleitung, um Ihre Daten mithilfe von Azure Data Factory zu verschieben](https://docs.microsoft.com/azure/data-factory/quickstart-create-data-factory-copy-data-tool).

## <a name="next-steps"></a>Nächste Schritte

* [Trainieren eines Modells](how-to-train-ml-models.md)
* [Bereitstellen eines Modells](how-to-deploy-and-where.md)
