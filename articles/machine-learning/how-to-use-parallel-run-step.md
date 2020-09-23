---
title: Ausführen von Batchvorhersagen für Big Data
titleSuffix: Azure Machine Learning
description: Hier erfahren Sie, wie Sie mithilfe von ParallelRunStep in Azure Machine Learning asynchron Rückschlüsse für große Datenmengen erhalten. ParallelRunStep bietet vorgefertigte Parallelverarbeitungsfunktionen sowie Optimierungen für Fire-and-Forget-Rückschlüsse mit hohem Durchsatz in Big Data-Szenarien.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.reviewer: jmartens, larryfr
ms.author: tracych
author: tracychms
ms.date: 08/14/2020
ms.custom: Build2020, devx-track-python
ms.openlocfilehash: 8e8d0a13bc01e95311345154648ecb00b624c4bf
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90905611"
---
# <a name="run-batch-inference-on-large-amounts-of-data-by-using-azure-machine-learning"></a>Ausführen von Batchrückschlüssen für große Datenmengen mithilfe von Azure Machine Learning


In diesem Artikel wird veranschaulicht, wie Sie Ihr Azure Machine Learning-Modell parallel ausführen und große Datenmengen schnell auswerten können. 

Das Erlangen von Rückschlüssen für große Datasets oder bei komplizierten Modellen kann zeitaufwändig sein. Mit der Klasse `ParallelRunStep` können Sie die Verarbeitung parallel durchführen und die Gesamtergebnisse so ggf. schneller erhalten. Die Durchführung einer einzelnen Auswertung ist unter Umständen in relativ kurzer Zeit möglich, aber für viele Szenarien (Objekterkennung, Videoverarbeitung, Verarbeitung natürlicher Sprache usw.) wird eine größere Zahl von Auswertungen benötigt. 

Mit `ParallelRunStep` ist es leicht möglich, für große Cluster mit vielen Computern Rückschlüsse per Massenvorgang zu erlangen. Cluster dieser Art können mehrere Terabyte an strukturierten bzw. unstrukturierten Daten mit verbesserter Produktivität und optimierten Kosten verarbeiten.

In diesem Artikel lernen Sie Folgendes:

> 1. Einrichten von Machine Learning-Ressourcen.
> 1. Konfigurieren von Dateneingaben und -ausgaben für Batchrückschlüsse.
> 1. Vorbereiten des vortrainierten Bildklassifizierungsmodells, das auf dem [MNIST](https://publicdataset.azurewebsites.net/dataDetail/mnist/)-Dataset basiert. 
> 1.  Schreiben Ihres Rückschlussskripts.
> 1. Erstellen einer [Machine Learning-Pipeline](concept-ml-pipelines.md), die ParallelRunStep enthält, und Ausführen von Batchrückschluss für MNIST-Testbilder. 
> 1. Erneutes Übermitteln einer Batchrückschluss-Ausführung mit neuer Dateneingabe und neuen Parametern. 
> 1. Zeigen Sie die Ergebnisse an.

## <a name="prerequisites"></a>Voraussetzungen

* Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie die [kostenlose oder kostenpflichtige Version von Azure Machine Learning](https://aka.ms/AMLFree) aus.

* Absolvieren Sie das [Einrichtungstutorial](tutorial-1st-experiment-sdk-setup.md), falls Sie noch nicht über einen Azure Machine Learning-Arbeitsbereich verfügen. 

* Informationen zur Verwaltung Ihrer eigenen Umgebung und Abhängigkeiten finden Sie in der [Schrittanleitung](how-to-configure-environment.md) zum Konfigurieren Ihrer eigenen lokalen Umgebung.

## <a name="set-up-machine-learning-resources"></a>Einrichten von Machine Learning-Ressourcen

Die folgenden Aktionen dienen zum Einrichten der Machine Learning-Ressourcen, die zum Ausführen einer Batch-Rückschlusspipeline benötigt werden:

- Herstellen einer Verbindung mit einem Arbeitsbereich.
- Erstellen oder Anfügen einer vorhandenen Computeressource.

### <a name="configure-workspace"></a>Konfigurieren des Arbeitsbereichs

Erstellen Sie ein Arbeitsbereichsobjekt aus dem vorhandenen Arbeitsbereich. `Workspace.from_config()` liest die config.json-Datei und lädt die Details in ein Objekt mit dem Namen „ws“.

```python
from azureml.core import Workspace

ws = Workspace.from_config()
```

> [!IMPORTANT]
> In diesem Codeausschnitt wird davon ausgegangen, dass die Arbeitsbereichskonfiguration im aktuellen Verzeichnis oder in dessen übergeordnetem Verzeichnis gespeichert wird. Weitere Informationen zum Erstellen eines Arbeitsbereichs finden Sie unter [Erstellen und Verwalten von Azure Machine Learning-Arbeitsbereichen](how-to-manage-workspace.md). Weitere Informationen zum Speichern der Konfiguration in einer Datei finden Sie unter [Konfigurieren einer Entwicklungsumgebung für Azure Machine Learning](how-to-configure-environment.md#workspace).

### <a name="create-a-compute-target"></a>Erstellen eines Computeziels

In Azure Machine Learning bezieht sich *Compute* (oder *Computeziel*) auf die Computer oder Cluster, die die Berechnungsschritte in Ihrer Machine Learning-Pipeline durchführen. Führen Sie den folgenden Code aus, um ein CPU-basiertes Ziel vom Typ [AmlCompute](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute?view=azure-ml-py&preserve-view=true) zu erstellen:

```python
from azureml.core.compute import AmlCompute, ComputeTarget
from azureml.core.compute_target import ComputeTargetException

# choose a name for your cluster
compute_name = os.environ.get("AML_COMPUTE_CLUSTER_NAME", "cpucluster")
compute_min_nodes = os.environ.get("AML_COMPUTE_CLUSTER_MIN_NODES", 0)
compute_max_nodes = os.environ.get("AML_COMPUTE_CLUSTER_MAX_NODES", 4)

# This example uses CPU VM. For using GPU VM, set SKU to STANDARD_NC6
vm_size = os.environ.get("AML_COMPUTE_CLUSTER_SKU", "STANDARD_D2_V2")


if compute_name in ws.compute_targets:
    compute_target = ws.compute_targets[compute_name]
    if compute_target and type(compute_target) is AmlCompute:
        print('found compute target. just use it. ' + compute_name)
else:
    print('creating a new compute target...')
    provisioning_config = AmlCompute.provisioning_configuration(vm_size = vm_size,
                                                                min_nodes = compute_min_nodes, 
                                                                max_nodes = compute_max_nodes)

    # create the cluster
    compute_target = ComputeTarget.create(ws, compute_name, provisioning_config)
    
    # can poll for a minimum number of nodes and for a specific timeout. 
    # if no min node count is provided it will use the scale settings for the cluster
    compute_target.wait_for_completion(show_output=True, min_node_count=None, timeout_in_minutes=20)
    
     # For a more detailed view of current AmlCompute status, use get_status()
    print(compute_target.get_status().serialize())
```

[!INCLUDE [low-pri-note](../../includes/machine-learning-low-pri-vm.md)]

## <a name="configure-inputs-and-output"></a>Konfigurieren von Eingaben und -ausgaben

### <a name="create-a-datastore-with-sample-images"></a>Erstellen eines Datenspeichers mit Beispielbildern

Laden Sie den MNIST-Auswertungssatz aus dem öffentlichen Blobcontainer `sampledata` in ein Konto namens `pipelinedata` herunter. Erstellen Sie einen Datenspeicher mit dem Namen `mnist_datastore`, der auf diesen Container verweist. Durch Festlegen des Flags `overwrite` auf `True` im folgenden Aufruf von `register_azure_blob_container` wird jeglicher Datenspeicher überschrieben, der zuvor mit diesem Namen erstellt wurde. 

Sie können diesen Schritt ändern, um auf Ihren Blobcontainer zu verweisen, indem Sie eigene Werte für `datastore_name`, `container_name` und `account_name` angeben.

```python
from azureml.core import Datastore
from azureml.core import Workspace

mnist_blob = Datastore.register_azure_blob_container(ws, 
                      datastore_name="mnist_datastore", 
                      container_name="sampledata", 
                      account_name="pipelinedata",
                      overwrite=True)
```

Geben Sie als Nächstes den Standarddatenspeicher des Arbeitsbereichs als Ausgabedatenspeicher an. Er wird für die Rückschlussausgabe verwendet.

Wenn Sie Ihren Arbeitsbereich erstellen, werden standardmäßig [Azure Files](https://docs.microsoft.com/azure/storage/files/storage-files-introduction) und [Blob Storage](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction)  an den Arbeitsbereich angefügt. Azure Files ist der Standarddatenspeicher für einen Arbeitsbereich, aber Sie können auch Blob Storage als Datenspeicher verwenden. Weitere Informationen finden Sie unter [Azure-Speicheroptionen](https://docs.microsoft.com/azure/storage/common/storage-decide-blobs-files-disks).

```python
def_data_store = ws.get_default_datastore()
```

### <a name="create-the-data-inputs"></a>Erstellen der Dateneingaben

Die Eingaben für Batchrückschluss sind die Daten, die Sie für die Parallelverarbeitung partitionieren möchten. Eine Pipeline für Batchrückschluss akzeptiert Dateneingaben durch [`Dataset`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py&preserve-view=true).

`Dataset` dient der Untersuchung, Transformation und Verwaltung von Daten in Azure Machine Learning. Es gibt zwei Typen: [`TabularDataset`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py&preserve-view=true) und [`FileDataset`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.filedataset?view=azure-ml-py&preserve-view=true). In diesem Beispiel verwenden Sie `FileDataset` als Eingaben. `FileDataset` gibt Ihnen die Möglichkeit, die Dateien herunterzuladen oder in Ihrer Computeinstanz bereitzustellen. Durch Erstellen eines Datasets erstellen Sie einen Verweis auf den Speicherort der Datenquelle. Wenn Sie Unterklassen-Transformationen auf das Dataset angewendet haben, werden diese ebenfalls im Dataset gespeichert. Die Daten verbleiben an ihrem Speicherort, sodass keine zusätzlichen Speicherkosten anfallen.

Weitere Informationen zu Azure Machine Learning-Datasets finden Sie unter [Erstellen von und Zugreifen auf Datasets (Vorschauversion) in Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/how-to-create-register-datasets).

```python
from azureml.core.dataset import Dataset

path_on_datastore = mnist_blob.path('mnist/')
input_mnist_ds = Dataset.File.from_files(path=path_on_datastore, validate=False)
```

Um beim Ausführen der Batch-Rückschlusspipeline dynamische Dateneingaben zu verwenden, können Sie das Eingabe-`Dataset` als [`PipelineParameter`](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.graph.pipelineparameter?view=azure-ml-py&preserve-view=true) definieren. Sie können das Eingabedataset bei jeder erneuten Übermittlung einer Ausführung der Batch-Rückschlusspipeline angeben.

```python
from azureml.data.dataset_consumption_config import DatasetConsumptionConfig
from azureml.pipeline.core import PipelineParameter

pipeline_param = PipelineParameter(name="mnist_param", default_value=input_mnist_ds)
input_mnist_ds_consumption = DatasetConsumptionConfig("minist_param_config", pipeline_param).as_mount()
```

### <a name="create-the-output"></a>Erstellen der Ausgabe

Objekte vom Typ [`PipelineData`](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelinedata?view=azure-ml-py&preserve-view=true) dienen zur Übertragung von Zwischendaten zwischen Pipelineschritten. In diesem Beispiel wird es für Rückschlussausgaben verwendet.

```python
from azureml.pipeline.core import Pipeline, PipelineData

output_dir = PipelineData(name="inferences", datastore=def_data_store)
```

## <a name="prepare-the-model"></a>Vorbereiten des Modells

[Laden Sie das vortrainierte Bildklassifizierungsmodell herunter](https://pipelinedata.blob.core.windows.net/mnist-model/mnist-tf.tar.gz), und extrahieren Sie es im Verzeichnis `models`.

```python
import os
import tarfile
import urllib.request

model_dir = 'models'
if not os.path.isdir(model_dir):
    os.mkdir(model_dir)

url="https://pipelinedata.blob.core.windows.net/mnist-model/mnist-tf.tar.gz"
response = urllib.request.urlretrieve(url, "model.tar.gz")
tar = tarfile.open("model.tar.gz", "r:gz")
tar.extractall(model_dir)
```

Registrieren Sie das Modell anschließend bei Ihrem Arbeitsbereich, damit es für Ihre Computeressource verfügbar ist.

```python
from azureml.core.model import Model

# Register the downloaded model 
model = Model.register(model_path="models/",
                       model_name="mnist",
                       tags={'pretrained': "mnist"},
                       description="Mnist trained tensorflow model",
                       workspace=ws)
```

## <a name="write-your-inference-script"></a>Schreiben Ihres Rückschlussskripts

>[!Warning]
>Bei dem folgenden Code handelt es sich lediglich um ein Beispiel des [Beispielnotebooks](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/machine-learning-pipelines/parallel-run). Sie müssen ein eigenes Skript für Ihr Szenario erstellen.

Das Skript *muss zwei Funktionen enthalten*:
- `init()`: Verwenden Sie diese Funktion für alle aufwendigen oder allgemeinen Vorbereitungsmaßnahmen für den späteren Rückschluss. Ein Beispiel wäre etwa das Laden des Modells in ein globales Objekt. Diese Funktion wird nur einmal zu Beginn des Prozesses aufgerufen.
-  `run(mini_batch)`: Diese Funktion wird für jede Instanz vom Typ `mini_batch` ausgeführt.
    -  `mini_batch`: `ParallelRunStep` ruft die Run-Methode auf und übergibt entweder eine Liste oder einen Pandas-Datenrahmen (`DataFrame`) als Argument an die Methode. Jeder Eintrag in „mini_batch“ entspricht einem Dateipfad, wenn die Eingabe ein `FileDataset` ist, bzw. einem Pandas-Datenrahmen (`DataFrame`), wenn die Eingabe ein `TabularDataset` ist.
    -  `response`: Die Run-Methode sollte einen Pandas-Datenrahmen (`DataFrame`) oder ein Array zurückgeben. Bei Verwendung von „append_row output_action“ werden diese zurückgegebenen Elemente am Ende der allgemeinen Ausgabedatei hinzugefügt. Bei Verwendung von „summary_only“ wird der Inhalt der Elemente ignoriert. Bei allen Ausgabeaktionen geben die zurückgegebenen Ausgabeelemente jeweils eine erfolgreiche Ausführung für ein Eingabeelement aus dem Eingabeminibatch an. Stellen Sie sicher, dass das Ausführungsergebnis genügend Daten für eine Zuordnung zwischen der Eingabe und der Ausgabe des Ausführungsergebnisses enthält. Die Ausführungsausgabe wird in die Ausgabedatei geschrieben. Da hierbei nicht unbedingt die Reihenfolge eingehalten wird, müssen Sie einen Schlüssel in der Ausgabe verwenden, um sie der Eingabe zuzuordnen.

```python
%%writefile digit_identification.py
# Snippets from a sample script.
# Refer to the accompanying digit_identification.py
# (https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/machine-learning-pipelines/parallel-run)
# for the implementation script.

import os
import numpy as np
import tensorflow as tf
from PIL import Image
from azureml.core import Model


def init():
    global g_tf_sess

    # Pull down the model from the workspace
    model_path = Model.get_model_path("mnist")

    # Construct a graph to execute
    tf.reset_default_graph()
    saver = tf.train.import_meta_graph(os.path.join(model_path, 'mnist-tf.model.meta'))
    g_tf_sess = tf.Session()
    saver.restore(g_tf_sess, os.path.join(model_path, 'mnist-tf.model'))


def run(mini_batch):
    print(f'run method start: {__file__}, run({mini_batch})')
    resultList = []
    in_tensor = g_tf_sess.graph.get_tensor_by_name("network/X:0")
    output = g_tf_sess.graph.get_tensor_by_name("network/output/MatMul:0")

    for image in mini_batch:
        # Prepare each image
        data = Image.open(image)
        np_im = np.array(data).reshape((1, 784))
        # Perform inference
        inference_result = output.eval(feed_dict={in_tensor: np_im}, session=g_tf_sess)
        # Find the best probability, and add it to the result list
        best_result = np.argmax(inference_result)
        resultList.append("{}: {}".format(os.path.basename(image), best_result))

    return resultList
```

Wenn das Verzeichnis mit Ihrem Rückschlussskript noch andere Dateien oder Ordner enthält, können Sie einen Verweis darauf erstellen, indem Sie das aktuelle Arbeitsverzeichnis ermitteln.

```python
script_dir = os.path.realpath(os.path.join(__file__, '..',))
file_path = os.path.join(script_dir, "<file_name>")
```

## <a name="build-and-run-the-pipeline-containing-parallelrunstep"></a>Erstellen und Ausführen der Pipeline mit ParallelRunStep

Jetzt haben Sie alles, was Sie brauchen: die Dateneingaben, das Modell, die Ausgabe und Ihr Rückschlussskript. Erstellen wir also die Batch-Rückschlusspipeline, die ParallelRunStep enthält.

### <a name="prepare-the-environment"></a>Vorbereiten der Umgebung

Geben Sie zunächst die Abhängigkeiten für Ihr Skript an. Diese ermöglicht es Ihnen, sowohl PIP-Pakete zu installieren als auch die Umgebung zu konfigurieren.

Fügen Sie der Liste der pip-Pakete stets **azureml-core** und **azureml-dataset-runtime[pandas, fuse]** hinzu. Wenn Sie ein benutzerdefiniertes Docker-Image verwenden (user_managed_dependencies=True), sollte auf Ihrem System außerdem Conda installiert sein.

```python
from azureml.core.environment import Environment
from azureml.core.conda_dependencies import CondaDependencies
from azureml.core.runconfig import DEFAULT_GPU_IMAGE

batch_conda_deps = CondaDependencies.create(pip_packages=["tensorflow==1.15.2", "pillow", 
                                                          "azureml-core", "azureml-dataset-runtime[pandas, fuse]"])

batch_env = Environment(name="batch_environment")
batch_env.python.conda_dependencies = batch_conda_deps
batch_env.docker.enabled = True
batch_env.docker.base_image = DEFAULT_GPU_IMAGE
```

### <a name="specify-the-parameters-using-parallelrunconfig"></a>Angeben der Parameter mithilfe von ParallelRunConfig

`ParallelRunConfig` ist die Hauptkonfiguration für die `ParallelRunStep`-Instanz innerhalb der Azure Machine Learning-Pipeline. Sie wird verwendet, um Ihr Skript zu umschließen und die erforderlichen Parameter, einschließlich der folgenden Einträge, zu konfigurieren:
- `entry_script`: Ein Benutzerskript als lokaler Dateipfad, das parallel auf mehreren Knoten ausgeführt wird. Ist `source_directory` vorhanden, verwenden Sie einen relativen Pfad. Verwenden Sie andernfalls einen beliebigen Pfad, auf den auf dem Computer zugegriffen werden kann.
- `mini_batch_size`: Die Größe des Minibatchs, der an einen einzelnen Aufruf von `run()` übergeben wird (optional; Standardwerte sind `10` Dateien für `FileDataset` und `1MB` für `TabularDataset`).
    - Bei `FileDataset` handelt es sich um die Anzahl von Dateien (Mindestwert: `1`). Mehrere Dateien können zu einem Minibatch zusammengefasst werden.
    - Bei `TabularDataset` handelt es sich um die Datengröße. Beispielwerte: `1024`, `1024KB`, `10MB`, `1GB`. Empfohlener Wert: `1MB`. Der Minibatch auf der Grundlage von `TabularDataset` geht nie über Dateigrenzen hinaus. Ein Beispiel: Angenommen, Sie verfügen über unterschiedlich große CSV-Dateien. Die kleinste Datei hat eine Größe von 100 KB, die größte Datei ist 10 MB groß. Wenn Sie nun `mini_batch_size = 1MB` festlegen, werden Dateien mit einer Größe von weniger als 1 MB als einzelner Minibatch behandelt. Dateien mit einer Größe von mehr als 1 MB werden dagegen in mehrere Minibatches aufgeteilt.
- `error_threshold`: Die Anzahl von Datensatzfehlern für `TabularDataset` bzw. Dateifehlern für `FileDataset`, die während der Verarbeitung ignoriert werden sollen. Übersteigt die Fehleranzahl für die gesamte Eingabe diesen Wert, wird der Auftrag abgebrochen. Der Fehlerschwellenwert gilt für die gesamte Eingabe und nicht für den einzelnen Minibatch, der an die Methode `run()` gesendet wird. Zulässiger Bereich: `[-1, int.max]`. Der Teil `-1` gibt an, dass bei der Verarbeitung alle Fehler ignoriert werden sollen.
- `output_action`: Gibt an, wie die Ausgabe strukturiert werden soll:
    - `summary_only`: Das Benutzerskript speichert die Ausgabe. `ParallelRunStep` verwendet die Ausgabe nur zur Berechnung des Fehlerschwellenwerts.
    - `append_row`: Für alle Eingaben wird lediglich eine einzelne Datei im Ausgabeordner erstellt, in der alle Ausgaben angefügt werden (getrennt durch eine Zeile).
- `append_row_file_name`: Zum Anpassen des Namens der Ausgabedatei für append_row output_action (optional; der Standardwert ist `parallel_run_step.txt`).
- `source_directory`: Pfade zu Ordnern mit allen Dateien, die am Computeziel ausgeführt werden sollen (optional).
- `compute_target`: Nur `AmlCompute` wird unterstützt.
- `node_count`: Die Anzahl von Computeknoten, die zum Ausführen des Benutzerskripts verwendet werden sollen.
- `process_count_per_node`: Die Anzahl von Prozessen pro Knoten. Die bewährte Methode besteht im Festlegen auf die Anzahl der GPUs oder CPUs pro Knoten (optional; der Standardwert ist `1`).
- `environment`: Die Python-Umgebungsdefinition. Sie kann für die Verwendung einer vorhandenen Python-Umgebung oder für die Einrichtung einer temporären Umgebung konfiguriert werden. Die Definition kann auch zum Festlegen der erforderlichen Anwendungsabhängigkeiten verwendet werden (optional).
- `logging_level`: Die Ausführlichkeit des Protokolls. Mögliche Werte (mit zunehmender Ausführlichkeit): `WARNING`, `INFO`, `DEBUG`. (optional; Standardwert: `INFO`)
- `run_invocation_timeout`: Das Timeout für den Aufruf der Methode `run()` in Sekunden. (optional; Standardwert `60`)
- `run_max_try`: Maximale Anzahl der Versuche von `run()` für einen Minibatch. Ein `run()` ist fehlgeschlagen, wenn eine Ausnahme ausgelöst wird oder beim Erreichen von `run_invocation_timeout` nichts zurückgegeben wird (optional; der Standardwert ist `3`). 

Sie können `mini_batch_size`, `node_count`, `process_count_per_node`, `logging_level`, `run_invocation_timeout` und `run_max_try` als `PipelineParameter` festlegen, sodass Sie die Parameterwerte beim erneuten Senden einer Pipelineausführung optimieren können. In diesem Beispiel verwenden Sie `PipelineParameter` für `mini_batch_size` und `Process_count_per_node`, und Sie ändern diese Werte, wenn Sie später erneut eine Ausführung senden. 

In diesem Beispiel wird davon ausgegangen, dass Sie das `digit_identification.py`-Skript verwenden, das zuvor erläutert wurde. Wenn Sie ein eigenes Skript verwenden, ändern Sie die Parameter `source_directory` und `entry_script` entsprechend.

```python
from azureml.pipeline.core import PipelineParameter
from azureml.pipeline.steps import ParallelRunConfig

parallel_run_config = ParallelRunConfig(
    source_directory='.',
    entry_script="digit_identification.py",
    mini_batch_size=PipelineParameter(name="batch_size_param", default_value="5"),
    error_threshold=10,
    output_action="append_row",
    append_row_file_name="mnist_outputs.txt",
    environment=batch_env,
    compute_target=compute_target,
    process_count_per_node=PipelineParameter(name="process_count_param", default_value=2),
    node_count=2)
```

### <a name="create-the-parallelrunstep"></a>Erstellen von ParallelRunStep

Erstellen Sie ParallelRunStep mithilfe von Skript, Umgebungskonfiguration und Parametern. Geben Sie das Computeziel an, das Sie bereits als Ausführungsziel für Ihr Rückschlussskript an Ihren Arbeitsbereich angefügt haben. Verwenden Sie `ParallelRunStep`, um den Batchrückschluss-Pipelineschritt mit folgenden Parametern zu erstellen:
- `name`: Der Name des Schritts. Benennungseinschränkungen: eindeutig, 3–32 Zeichen und regulärer Ausdruck ^\[a-z\]([-a-z0-9]*[a-z0-9])?$.
- `parallel_run_config`: Ein Objekt vom Typ `ParallelRunConfig`, wie zuvor definiert.
- `inputs`: Mindestens ein Azure Machine Learning-Dataset mit einem einzelnen Typ, das für die Parallelverarbeitung partitioniert werden soll.
- `side_inputs`: Verweisdaten oder Datasets, die als Seiteneingabe verwendet werden und für die keine Partitionierung erforderlich ist.
- `output`: Ein Objekt vom Typ `PipelineData`, das dem Ausgabeverzeichnis entspricht.
- `arguments`: Eine Liste mit Argumenten, die an das Benutzerskript übergeben werden. Verwenden Sie „unknown_args“, um sie in Ihrem Einstiegsskript zu nutzen (optional).
- `allow_reuse`: Angabe, ob bei dem Schritt vorherige Ergebnisse wiederverwendet werden sollen, wenn er mit den gleichen Einstellungen/Eingaben ausgeführt wird. Ist der Parameter auf `False` festgelegt, wird für diesen Schritt bei der Pipelineausführung immer eine neue Ausführung generiert. (optional; Standardwert `True`)

```python
from azureml.pipeline.steps import ParallelRunStep

parallelrun_step = ParallelRunStep(
    name="predict-digits-mnist",
    parallel_run_config=parallel_run_config,
    inputs=[input_mnist_ds_consumption],
    output=output_dir,
    allow_reuse=True
)
```
### <a name="create-and-run-the-pipeline"></a>Erstellen und Ausführen der Pipeline

Führen Sie die Pipeline nun aus. Erstellen Sie zunächst ein Objekt vom Typ [`Pipeline`](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipeline%28class%29?view=azure-ml-py&preserve-view=true) mit einem Verweis auf Ihren Arbeitsbereich sowie mit dem erstellten Pipelineschritt. Der Parameter `steps` ist ein Array mit Schritten. In diesem Fall ist nur ein Schritt für den Batchrückschluss vorhanden. Wenn Sie Pipelines mit mehreren Schritten erstellen möchten, müssen Sie die Schritte in der gewünschten Reihenfolge in diesem Array platzieren.

Verwenden Sie als Nächstes die Funktion `Experiment.submit()`, um die Pipeline für die Ausführung zu übermitteln.

```python
from azureml.pipeline.core import Pipeline
from azureml.core.experiment import Experiment

pipeline = Pipeline(workspace=ws, steps=[parallelrun_step])
experiment = Experiment(ws, 'digit_identification')
pipeline_run = experiment.submit(pipeline)
```

## <a name="monitor-the-batch-inference-job"></a>Überwachen des Batchrückschlussauftrags

Ein Batchrückschlussauftrag kann sehr lange dauern. In diesem Beispiel wird der Fortschritt mithilfe eines Jupyter-Widgets überwacht. Weitere Möglichkeiten zum Überwachen des Auftragsstatus:

* Azure Machine Learning Studio 
* Konsolenausgabe des Objekts [`PipelineRun`](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.run.pipelinerun?view=azure-ml-py&preserve-view=true)

```python
from azureml.widgets import RunDetails
RunDetails(pipeline_run).show()

pipeline_run.wait_for_completion(show_output=True)
```

## <a name="resubmit-a-run-with-new-data-inputs-and-parameters"></a>Erneutes Senden eines Laufs mit neuen Dateneingaben und Parametern

Da Sie die Eingaben und mehrere Konfigurationen als `PipelineParameter` vorgenommen haben, können Sie einen Batch-Rückschlusslauf mit einer anderen Dataseteingabe erneut einreichen und dabei die Parameter optimieren, ohne eine ganz neue Pipeline erstellen zu müssen. Sie verwenden denselben Datenspeicher, aber nur ein einzelnes Bild als Dateneingabe.

```python
path_on_datastore = mnist_blob.path('mnist/0.png')
single_image_ds = Dataset.File.from_files(path=path_on_datastore, validate=False)

pipeline_run_2 = experiment.submit(pipeline, 
                                   pipeline_parameters={"mnist_param": single_image_ds, 
                                                        "batch_size_param": "1",
                                                        "process_count_param": 1}
)

pipeline_run_2.wait_for_completion(show_output=True)
```
## <a name="view-the-results"></a>Zeigen Sie die Ergebnisse an

Die Ergebnisse aus der obigen Ausführung werden in den `DataStore` geschrieben, der im `PipelineData`-Objekt als Ausgabedatenelement angegeben ist. In diesem Fall lautet der Name hierfür *inferences*. Die Ergebnisse werden im Standardblobcontainer gespeichert. Sie können zu Ihrem Speicherkonto navigieren und ihn über Storage-Explorer anzeigen. Der Dateipfad lautet „azureml-blobstore-*GUID*/azureml/*RunId*/*output_dir*“.

Sie können diese Daten auch herunterladen, um die Ergebnisse anzuzeigen. Nachfolgend sehen Sie den Beispielcode zum Anzeigen der ersten zehn Zeilen:

```python
import pandas as pd
import tempfile

batch_run = pipeline_run.find_step_run(parallelrun_step.name)[0]
batch_output = batch_run.get_output_data(output_dir.name)

target_dir = tempfile.mkdtemp()
batch_output.download(local_path=target_dir)
result_file = os.path.join(target_dir, batch_output.path_on_datastore, parallel_run_config.append_row_file_name)

df = pd.read_csv(result_file, delimiter=":", header=None)
df.columns = ["Filename", "Prediction"]
print("Prediction has ", df.shape[0], " rows")
df.head(10) 
```

## <a name="next-steps"></a>Nächste Schritte

Den gesamten Ablauf dieses Prozesses können Sie sich anhand des [Notebooks für Batchrückschlüsse](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/machine-learning-pipelines/parallel-run) ansehen. 

Informationen zum Debugging und zur Problembehandlung für ParallelRunStep finden Sie in der [Schrittanleitung](how-to-debug-parallel-run-step.md).

Informationen zum Debugging und zur Problembehandlung für Pipelines finden Sie in der [Schrittanleitung](how-to-debug-pipelines.md).

[!INCLUDE [aml-clone-in-azure-notebook](../../includes/aml-clone-for-examples.md)]

