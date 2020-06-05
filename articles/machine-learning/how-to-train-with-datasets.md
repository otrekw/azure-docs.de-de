---
title: Trainieren mit „azureml-datasets“
titleSuffix: Azure Machine Learning
description: Hier erfahren Sie, wie Sie Datasets beim Training einsetzen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sihhu
author: MayMSFT
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 04/20/2020
ms.openlocfilehash: cd72ce9fed7f821807b8604f68068c64a38293e3
ms.sourcegitcommit: 309a9d26f94ab775673fd4c9a0ffc6caa571f598
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2020
ms.locfileid: "82996656"
---
# <a name="train-with-datasets-in-azure-machine-learning"></a>Trainieren mit Datasets in Azure Machine Learning
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

In diesem Artikel erfahren Sie, wie Sie [Azure Machine Learning-Datasets](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset%28class%29?view=azure-ml-py) in Ihren Trainingsexperimenten einsetzen.  Sie können Datasets in Ihrem Lokal- oder Remotecomputeziel verwenden, ohne sich Gedanken über Verbindungszeichenfolgen oder Datenpfade machen zu müssten.

Azure Machine Learning-Datasets bieten eine nahtlose Integration in Azure Machine Learning-Trainingsprodukte wie [ScriptRun](https://docs.microsoft.com/python/api/azureml-core/azureml.core.scriptrun?view=azure-ml-py), [Estimator](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator?view=azure-ml-py), [HyperDrive](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.hyperdrive?view=azure-ml-py) und [Azure Machine Learning-Pipelines](how-to-create-your-first-pipeline.md).

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen Folgendes, um Datasets zu erstellen und für das Training zu nutzen:

* Ein Azure-Abonnement. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie die [kostenlose oder kostenpflichtige Version von Azure Machine Learning](https://aka.ms/AMLFree) noch heute aus.

* Ein [Azure Machine Learning-Arbeitsbereich](how-to-manage-workspace.md).

* Eine [Installation des Azure Machine Learning-SDK für Python](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py), in dem das Paket „azureml-datasets“ enthalten ist.

> [!Note]
> Einige Datasetklassen sind vom Paket [azureml-dataprep](https://docs.microsoft.com/python/api/azureml-dataprep/?view=azure-ml-py) abhängig. Für Linux-Benutzer werden diese Klassen nur unter den folgenden Distributionen unterstützt:  Red Hat Enterprise Linux, Ubuntu, Fedora und CentOS.

## <a name="access-and-explore-input-datasets"></a>Zugreifen auf und Untersuchen von Eingabedatasets

Sie können auf eine vorhandene TabularDataset-Klasse aus dem Trainingsskript eines Experiments in Ihrem Arbeitsbereich zugreifen und dieses Dataset in einen Pandas-Datenrahmen laden, um die lokale Umgebung genauer zu untersuchen.

Im folgenden Code wird die [`get_context()`]()-Methode in der [`Run`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py)-Klasse verwendet, um auf die vorhandene TabularDataset-Klasse (`titanic`) im Trainingsskript zuzugreifen. Anschließend wird die [`to_pandas_dataframe()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset#to-pandas-dataframe-on-error--null---out-of-range-datetime--null--)-Methode verwendet, um das Dataset in einen Pandas-Datenrahmen zu laden, um das Durchsuchen und die Vorbereitung der Daten vor dem Training zu vertiefen.

> [!Note]
> Wenn die ursprüngliche Datenquelle den Wert NaN, leere Zeichenfolgen oder leere Werte enthält, wenn Sie to_pandas_dataframe(), müssen diese durch *NULL-Werte* ersetzt werden. 

```Python
%%writefile $script_folder/train_titanic.py

from azureml.core import Dataset, Run

run = Run.get_context()
# get the input dataset by name
dataset = run.input_datasets['titanic']

# load the TabularDataset to pandas DataFrame
df = dataset.to_pandas_dataframe()
```

Wenn Sie die vorbereiteten Daten aus einem In-Memory-Pandas-Datenrahmen in ein neues Dataset laden müssen, schreiben Sie die Daten in eine lokale Datei, z. B. eine Parquet-Datei, und erstellen das neue Dataset aus dieser Datei. Sie können Datasets auch aus lokalen Dateien oder Pfaden in Datenspeichern erstellen. Erfahren Sie mehr über das [Erstellen von Datasets](how-to-create-register-datasets.md).

## <a name="use-datasets-directly-in-training-scripts"></a>Direktes Verwenden von Datasets in Trainingsskripts

Wenn Sie strukturierte Daten besitzen, die noch nicht als Dataset registriert wurden, können Sie eine TabularDataset-Klasse erstellen und diese direkt im Trainingsskript für das Lokal- oder Remoteexperiment verwenden.

In diesem Beispiel erstellen Sie eine nicht registrierte [TabularDataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py)-Klasse und verwenden diese als direkte Trainingseingabe für das `estimator`-Objekt. Wenn Sie diese TabularDataset-Klasse für andere Experimente in Ihrem Arbeitsbereich wiederverwenden möchten, finden Sie weitere Informationen unter [Registrieren von Datasets](how-to-create-register-datasets.md#register-datasets).

### <a name="create-a-tabulardataset"></a>Erstellen eines TabularDataset-Elements

Der folgende Code erstellt ein nicht registriertes TabularDataset-Element aus einer Web-URL.  

```Python
from azureml.core.dataset import Dataset

web_path ='https://dprepdata.blob.core.windows.net/demo/Titanic.csv'
titanic_ds = Dataset.Tabular.from_delimited_files(path=web_path)
```

TabularDataset-Objekte bieten die Möglichkeit, die TabularDataset-Daten in einen Pandas- oder Spark-Datenrahmen zu laden, damit Sie mit vertrauten Datenaufbereitungs- und Trainingsbibliotheken arbeiten können, ohne das Notebook zu verlassen. Informationen zur Nutzung dieser Funktion finden Sie unter [Zugreifen auf und Untersuchen von Eingabedatasets](#access-and-explore-input-datasets).

### <a name="configure-the-estimator"></a>Konfigurieren des Estimators

Ein [Estimator](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py)-Objekt wird verwendet, um die Experimentausführung zu übermitteln. Azure Machine Learning verfügt über vorkonfigurierte Estimators für gängige Frameworks für maschinelles Lernen sowie einen generischen Estimator.

Dieser Code erstellt ein generisches Estimator-Objekt (`est`) das Folgendes festlegt:

* Ein Skriptverzeichnis für Ihre Skripts. Alle Dateien in dieses Verzeichnis werden zur Ausführung in die Clusterknoten hochgeladen.
* Das Trainingsskript *train_titanic.py*
* Das Eingabedataset für das Training: `titanic_ds` `as_named_input()` ist erforderlich, damit der zugewiesene Name `titanic` in Ihrem Trainingsskript auf das Eingabedataset verweisen kann. 
* Das Computeziel für das Experiment
* Die Umgebungsdefinition für das Experiment

```Python
est = Estimator(source_directory=script_folder,
                entry_script='train_titanic.py',
                # pass dataset object as an input with name 'titanic'
                inputs=[titanic_ds.as_named_input('titanic')],
                compute_target=compute_target,
                environment_definition= conda_env)

# Submit the estimator as part of your experiment run
experiment_run = experiment.submit(est)
experiment_run.wait_for_completion(show_output=True)
```

## <a name="mount-files-to-remote-compute-targets"></a>Einbinden von Dateien in Remotecomputezielen

Wenn Sie unstrukturierte Daten besitzen, sollten Sie eine [FileDataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.filedataset?view=azure-ml-py)-Klasse erstellen und Ihre Datendateien einbinden oder herunterladen, damit diese vom Remotecomputeziel für das Training genutzt werden können. Lesen Sie nach, wann Sie Daten [einbinden oder herunterladen](#mount-vs-download) sollten, wenn Sie Remotetrainingsexperimente durchführen. 

Im folgenden Beispiel wird eine FileDataset-Klasse erstellt, und das Dataset wird in das Computeziel eingebunden, indem es als Argument in der Schätzung für das Training übergeben wird. 

### <a name="create-a-filedataset"></a>Erstellen eines FileDataset-Elements

Im folgenden Beispiel wird ein nicht registriertes FileDataset-Element aus Web-URLs erstellt. Erfahren Sie mehr über das [Erstellen von Datasets](https://aka.ms/azureml/howto/createdatasets) aus anderen Quellen.

```Python
from azureml.core.dataset import Dataset

web_paths = [
            'http://yann.lecun.com/exdb/mnist/train-images-idx3-ubyte.gz',
            'http://yann.lecun.com/exdb/mnist/train-labels-idx1-ubyte.gz',
            'http://yann.lecun.com/exdb/mnist/t10k-images-idx3-ubyte.gz',
            'http://yann.lecun.com/exdb/mnist/t10k-labels-idx1-ubyte.gz'
            ]
mnist_ds = Dataset.File.from_files(path = web_paths)
```

### <a name="configure-the-estimator"></a>Konfigurieren des Estimators

Es wird empfohlen, das Dataset bei der Einbindung als Argument zu übergeben. Neben dem Übergeben des Datasets über den `inputs`-Parameter im Estimator können Sie das Dataset auch über `script_params` übergeben und den Datenpfad (Bereitstellungspunkt) im Trainingsskript über Argumente abrufen. So können Sie dasselbe Trainingsskript für lokales Debugging und Remotetraining auf jeder Cloudplattform verwenden.

Ein [SKLearn](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.sklearn.sklearn?view=azure-ml-py)-Estimatorobjekt wird verwendet, um die Scikit-learn-Experimente zu übermitteln. Nach dem Übermitteln der Ausführung werden Datendateien, auf die das `mnist`-Dataset verweist, auf dem Computeziel eingebunden. Weitere Informationen zum Training mit der [SKlearn-Schätzung](how-to-train-scikit-learn.md).

```Python
from azureml.train.sklearn import SKLearn

script_params = {
    # mount the dataset on the remote compute and pass the mounted path as an argument to the training script
    '--data-folder': mnist_ds.as_named_input('mnist').as_mount(),
    '--regularization': 0.5
}

est = SKLearn(source_directory=script_folder,
              script_params=script_params,
              compute_target=compute_target,
              environment_definition=env,
              entry_script='train_mnist.py')

# Run the experiment
run = experiment.submit(est)
run.wait_for_completion(show_output=True)
```

### <a name="retrieve-the-data-in-your-training-script"></a>Abrufen der Daten im Trainingsskript

Der folgende Code zeigt, wie Sie die Daten in Ihrem Skript abrufen können.

```Python
%%writefile $script_folder/train_mnist.py

import argparse
import os
import numpy as np
import glob

from utils import load_data

# retrieve the 2 arguments configured through script_params in estimator
parser = argparse.ArgumentParser()
parser.add_argument('--data-folder', type=str, dest='data_folder', help='data folder mounting point')
parser.add_argument('--regularization', type=float, dest='reg', default=0.01, help='regularization rate')
args = parser.parse_args()

data_folder = args.data_folder
print('Data folder:', data_folder)

# get the file paths on the compute
X_train_path = glob.glob(os.path.join(data_folder, '**/train-images-idx3-ubyte.gz'), recursive=True)[0]
X_test_path = glob.glob(os.path.join(data_folder, '**/t10k-images-idx3-ubyte.gz'), recursive=True)[0]
y_train_path = glob.glob(os.path.join(data_folder, '**/train-labels-idx1-ubyte.gz'), recursive=True)[0]
y_test = glob.glob(os.path.join(data_folder, '**/t10k-labels-idx1-ubyte.gz'), recursive=True)[0]

# load train and test set into numpy arrays
X_train = load_data(X_train_path, False) / 255.0
X_test = load_data(X_test_path, False) / 255.0
y_train = load_data(y_train_path, True).reshape(-1)
y_test = load_data(y_test, True).reshape(-1)
```


## <a name="mount-vs-download"></a>Vergleich: Einbinden oder Herunterladen

Das Einbinden oder Herunterladen von Dateien beliebiger Formate aus Azure Blob Storage, Azure Files, Azure Data Lake Storage Gen1, Azure Data Lake Storage Gen2, Azure SQL-Datenbank und Azure Database for PostgreSQL wird für Datasets beliebiger Formate unterstützt. 

Wenn Sie ein Dataset einbinden, fügen Sie die Datei, auf die das Dataset verweist, an ein Verzeichnis (Bereitstellungspunkt) an und stellen sie auf dem Computeziel zur Verfügung. Einbinden wird für Linux-basierte Computeressourcen unterstützt, einschließlich Azure Machine Learning Compute, virtuelle Computer und HDInsight. 

Beim Herunterladen eines Datasets werden alle Dateien, auf die das Dataset verweist, auf das Computeziel heruntergeladen. Herunterladen wird für alle Computetypen unterstützt. 

Wenn Ihr Skript alle Dateien verarbeitet, auf die das Dataset verweist, und Ihr Computedatenträger über Platz für das vollständige Dataset verfügt, wird Herunterladen empfohlen, um den Mehraufwand das Streamen von Daten von Speicherdiensten zu vermeiden. Wenn der Umfang Ihrer Daten die Größe des Computedatenträgers übersteigt, ist das Herunterladen nicht möglich. Für dieses Szenario empfehlen wir das Einbinden, da nur die von Ihrem Skript verwendeten Datendateien zum Zeitpunkt der Verarbeitung geladen werden.

Der folgende Code bindet `dataset` in das temporäre Verzeichnis unter `mounted_path` ein.

```python
import tempfile
mounted_path = tempfile.mkdtemp()

# mount dataset onto the mounted_path of a Linux-based compute
mount_context = dataset.mount(mounted_path)

mount_context.start()

import os
print(os.listdir(mounted_path))
print (mounted_path)
```

## <a name="notebook-examples"></a>Notebook-Beispiele

Die [Dataset-Notebooks](https://aka.ms/dataset-tutorial) veranschaulichen und erläutern die in diesem Artikel enthaltenen Konzepte.

## <a name="next-steps"></a>Nächste Schritte

* [Automatisches Trainieren von Machine Learning-Modellen](how-to-auto-train-remote.md) mit TabularDatasets

* [Trainieren von Bildklassifizierungsmodellen](https://aka.ms/filedataset-samplenotebook) mit FileDatasets

* [Training mit Datasets mithilfe von Pipelines](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/work-with-data/datasets-tutorial/pipeline-with-datasets/pipeline-for-image-classification.ipynb)
