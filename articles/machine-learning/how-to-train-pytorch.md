---
title: Trainieren von Deep Learning-PyTorch-Modellen
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie Ihre PyTorch-Trainingsskripts im Unternehmensumfang mit Azure Machine Learning ausführen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.author: minxia
author: mx-iao
ms.reviewer: peterlu
ms.date: 01/14/2020
ms.topic: how-to
ms.openlocfilehash: 1a261a972636021bec88244704b2b491ae8bcc61
ms.sourcegitcommit: 5ce88326f2b02fda54dad05df94cf0b440da284b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2021
ms.locfileid: "107888438"
---
# <a name="train-pytorch-models-at-scale-with-azure-machine-learning"></a>Bedarfsgerechtes Trainieren von PyTorch-Modellen mit Azure Machine Learning

In diesem Artikel erfahren Sie, wie Sie Ihre [PyTorch](https://pytorch.org/)-Trainingsskripts im Unternehmensumfang mit Azure Machine Learning ausführen.

Die Beispielskripts in diesem Artikel werden dazu verwendet, Hühner- und Truthahnbilder zu klassifizieren, um ein neuronales Deep Learning-Netz (DNN) aufzubauen, basierend auf dem [Tutorial](https://pytorch.org/tutorials/beginner/transfer_learning_tutorial.html) zum Transferlernen von PyTorch. Lerntransfer ist ein Verfahren, bei dem das bei der Lösung eines Problems gewonnene Wissen auf ein anderes, aber verwandtes Problem angewandt wird. Aufgrund des damit geringeren Aufwands an Daten, Zeit und Computeressourcen wird der Trainingsprozess rationalisiert. Weitere Informationen zum Lerntransfer finden Sie im Artikel [Deep Learning im Vergleich zu maschinellem Lernen](./concept-deep-learning-vs-machine-learning.md#what-is-transfer-learning).

Unabhängig davon, ob Sie ein PyTorch-Deep Learning-Modell von Grund auf trainieren, oder ob Sie ein vorhandenes Modell in die Cloud bringen, können Sie Azure Machine Learning zum Aufskalieren von Open-Source-Trainingsaufträgen mithilfe elastischer Cloud-Computeressourcen verwenden. Sie können produktionsgeeignete Modelle mit Azure Machine Learning erstellen, bereitstellen, überwachen sowie die Versionen verwalten. 

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie diesen Code in einer dieser Umgebungen aus:

- Azure Machine Learning-Compute-Instanz: keine Downloads oder Installationen erforderlich

    - Absolvieren Sie [Tutorial: Einrichten von Umgebung und Arbeitsbereich](tutorial-1st-experiment-sdk-setup.md), um einen dedizierten Notebookserver zu erstellen, auf dem das SDK und Beispielrepository vorinstalliert sind.
    - Suchen Sie im Deep Learning-Beispielordner auf dem Notebookserver ein fertiges und erweitertes Notebook. Dazu navigieren Sie zum folgenden Verzeichnis: **how-to-use-azureml > ml-frameworks > pytorch > train-hyperparameter-tune-deploy-with-pytorch**. 
 
 - Ihr eigener Jupyter Notebook-Server
    - [Installieren des Azure Machine Learning SDK](/python/api/overview/azure/ml/install) (1.15.0 oder höher).
    - [Erstellen Sie eine Konfigurationsdatei für den Arbeitsbereich.](how-to-configure-environment.md#workspace)
    - [Herunterladen der Beispielskriptdateien](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/ml-frameworks/pytorch/train-hyperparameter-tune-deploy-with-pytorch) `pytorch_train.py`
     
    Auf der GitHub-Seite mit Beispielen finden Sie außerdem eine fertige [Jupyter Notebook-Version](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/ml-frameworks/pytorch/train-hyperparameter-tune-deploy-with-pytorch/train-hyperparameter-tune-deploy-with-pytorch.ipynb) dieser Anleitung. Das Notebook umfasst erweiterte Abschnitte, in denen die intelligente Hyperparameteroptimierung, die Modellimplementierung und Notebook-Widgets behandelt werden.

## <a name="set-up-the-experiment"></a>Einrichten des Experiments

In diesem Abschnitt wird das Trainingsexperiment eingerichtet, indem die erforderlichen Python-Pakete geladen, ein Arbeitsbereich initialisiert, das Computeziel erstellt und die Trainingsumgebung definiert wird.

### <a name="import-packages"></a>Importieren von Paketen

Importieren Sie zunächst die erforderlichen Python-Bibliotheken.

```Python
import os
import shutil

from azureml.core.workspace import Workspace
from azureml.core import Experiment
from azureml.core import Environment

from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException
```

### <a name="initialize-a-workspace"></a>Initialisieren eines Arbeitsbereichs

Der [Azure Machine Learning-Arbeitsbereich](concept-workspace.md) ist die Ressource der obersten Ebene für den Dienst. Er stellt den zentralen Ort für die Arbeit mit allen erstellten Artefakten dar. Im Python SDK können Sie auf die Arbeitsbereichsartefakte zugreifen, indem Sie ein [`workspace`](/python/api/azureml-core/azureml.core.workspace.workspace)-Objekt erstellen.

Erstellen Sie ein Arbeitsbereichsobjekt aus der Datei `config.json`, die im [Abschnitt „Voraussetzungen“](#prerequisites) erstellt wurde.

```Python
ws = Workspace.from_config()
```

### <a name="get-the-data"></a>Abrufen von Daten

Das Dataset besteht aus etwa 120 Trainingsbildern, jeweils von Puten und Hühnern, mit 100 Validierungsbildern für jede Klasse. Im Rahmen unseres Trainingsskripts `pytorch_train.py` laden wir das Dataset herunter und extrahieren es. Die Bilder sind eine Teilmenge des [Open Images v5 Dataset](https://storage.googleapis.com/openimages/web/index.html).

### <a name="prepare-training-script"></a>Vorbereiten des Trainingsskripts

In diesem Tutorial ist das Trainingsskript `pytorch_train.py` bereits bereitgestellt. In der Praxis können Sie jedes benutzerdefinierte Trainingsskript, wie es ist, verwenden und mit Azure Machine Learning ausführen.

Erstellen Sie einen Ordner für Ihre Trainingsskripts.

```python
project_folder = './pytorch-birds'
os.makedirs(project_folder, exist_ok=True)
shutil.copy('pytorch_train.py', project_folder)
```

### <a name="create-a-compute-target"></a>Erstellen eines Computeziels

Erstellen Sie ein Computeziel, auf dem der PyTorch-Auftrag ausgeführt werden soll. In diesem Beispiel erstellen Sie einen GPU-fähigen Azure Machine Learning-Computercluster.

```Python
cluster_name = "gpu-cluster"

try:
    compute_target = ComputeTarget(workspace=ws, name=cluster_name)
    print('Found existing compute target')
except ComputeTargetException:
    print('Creating a new compute target...')
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_NC6', 
                                                           max_nodes=4)

    compute_target = ComputeTarget.create(ws, cluster_name, compute_config)

    compute_target.wait_for_completion(show_output=True, min_node_count=None, timeout_in_minutes=20)
```

[!INCLUDE [low-pri-note](../../includes/machine-learning-low-pri-vm.md)]

Weitere Informationen zu Computezielen finden Sie im Artikel [Was ist ein Computeziel?](concept-compute-target.md).

### <a name="define-your-environment"></a>Definieren der Umgebung

Um die Azure ML-[Umgebung](concept-environments.md) zu definieren, die die Abhängigkeiten Ihres Trainingsskripts kapselt, können Sie entweder eine benutzerdefinierte Umgebung definieren oder eine von Azure ML zusammengestellte Umgebung verwenden.

#### <a name="use-a-curated-environment"></a>Verwenden einer zusammengestellten Umgebung
Azure ML bietet vorgefertigte, zusammengestellte Umgebungen, falls Sie keine eigene Umgebung definieren möchten. Azure ML verfügt über mehrere unterschiedliche CPU- und GPU-Umgebungen für PyTorch, die verschiedenen Versionen von PyTorch entsprechen. Weitere Informationen finden Sie [hier](resource-curated-environments.md).

Wenn Sie eine zusammengestellte Umgebung verwenden möchten, können Sie stattdessen den folgenden Befehl ausführen:

```python
curated_env_name = 'AzureML-PyTorch-1.6-GPU'
pytorch_env = Environment.get(workspace=ws, name=curated_env_name)
```

Um die Pakete anzuzeigen, die in der zusammengestellten Umgebung enthalten sind, können Sie die Conda-Abhängigkeiten auf den Datenträger schreiben:
```python
pytorch_env.save_to_directory(path=curated_env_name)
```

Stellen Sie sicher, dass die zusammengestellte Umgebung alle Abhängigkeiten enthält, die von Ihrem Trainingsskript benötigt werden. Wenn dies nicht der Fall ist, müssen Sie die Umgebung so ändern, dass die fehlenden Abhängigkeiten eingeschlossen werden. Beachten Sie Folgendes: Wenn die Umgebung geändert wird, müssen Sie ihr einen neuen Namen geben, da das Präfix „AzureML“ für zusammengestellte Umgebungen reserviert ist. Wenn Sie die YAML-Datei für Conda-Abhängigkeiten geändert haben, können Sie daraus eine neue Umgebung mit einem neuen Namen erstellen, z. B.:
```python
pytorch_env = Environment.from_conda_specification(name='pytorch-1.6-gpu', file_path='./conda_dependencies.yml')
```

Wenn Sie das Objekt der zusammengestellten Umgebung stattdessen direkt geändert haben, können Sie diese Umgebung mit einem neuen Namen klonen:
```python
pytorch_env = pytorch_env.clone(new_name='pytorch-1.6-gpu')
```

#### <a name="create-a-custom-environment"></a>Erstellen einer benutzerdefinierten Umgebung

Sie können auch eine eigene Azure ML-Umgebung erstellen, die die Abhängigkeiten Ihres Trainingsskripts kapselt.

Definieren Sie zunächst Ihre Conda-Abhängigkeiten in einer YAML-Datei. In diesem Beispiel trägt die Datei den Namen `conda_dependencies.yml`.

```yaml
channels:
- conda-forge
dependencies:
- python=3.6.2
- pip:
  - azureml-defaults
  - torch==1.6.0
  - torchvision==0.7.0
  - future==0.17.1
  - pillow
```

Erstellen Sie anhand dieser Spezifikation der Conda-Umgebung eine Azure ML-Umgebung. Die Umgebung wird zur Laufzeit in einen Docker-Container gepackt.

Wenn kein Basisimage angegeben wird, verwendet Azure ML standardmäßig ein CPU-Image `azureml.core.environment.DEFAULT_CPU_IMAGE` als Basisimage. Da das Training in diesem Beispiel auf einem GPU-Cluster ausgeführt wird, müssen Sie ein GPU-Basisimage mit den erforderlichen GPU-Treibern und -Abhängigkeiten angeben. Azure ML verwaltet eine Reihe von Basisimages, die für Microsoft Container Registry (MCR) veröffentlicht werden, die Sie verwenden können. Weitere Informationen finden Sie im GitHub-Repository [Azure/AzureML-Containers](https://github.com/Azure/AzureML-Containers).

```python
pytorch_env = Environment.from_conda_specification(name='pytorch-1.6-gpu', file_path='./conda_dependencies.yml')

# Specify a GPU base image
pytorch_env.docker.enabled = True
pytorch_env.docker.base_image = 'mcr.microsoft.com/azureml/openmpi3.1.2-cuda10.1-cudnn7-ubuntu18.04'
```

> [!TIP]
> Optional können Sie einfach alle ihre Abhängigkeiten direkt in einem benutzerdefinierten Docker-Image oder in einer Dockerfile-Datei erfassen und Ihre Umgebung daraus erstellen. Weitere Informationen finden Sie unter [Trainieren mit einem benutzerdefinierten Image](how-to-train-with-custom-image.md).

Weitere Informationen zum Erstellen und Verwenden von Umgebungen finden Sie unter [Erstellen und Verwenden von Softwareumgebungen in Azure Machine Learning](how-to-use-environments.md).

## <a name="configure-and-submit-your-training-run"></a>Konfigurieren und Übermitteln Ihrer Trainingsausführung

### <a name="create-a-scriptrunconfig"></a>Erstellen eines ScriptRunConfig-Elements

Erstellen Sie ein [ScriptRunConfig-](/python/api/azureml-core/azureml.core.scriptrunconfig) Objekt, um die Konfigurationsdetails Ihres Trainingsauftrags anzugeben, einschließlich Ihres Trainingsskripts, der zu verwendenden Umgebung und des Computeziels für die Ausführung. Alle Argumente des Trainingsskripts werden über die Befehlszeile übergeben, wenn sie im `arguments` Parameter angegeben sind. 

```python
from azureml.core import ScriptRunConfig

src = ScriptRunConfig(source_directory=project_folder,
                      script='pytorch_train.py',
                      arguments=['--num_epochs', 30, '--output_dir', './outputs'],
                      compute_target=compute_target,
                      environment=pytorch_env)
```

> [!WARNING]
> Zum Ausführen von Trainingsskripts wird von Azure Machine Learning das gesamte Quellverzeichnis kopiert. Sind vertrauliche Daten vorhanden, die nicht hochgeladen werden sollen, verwenden Sie die [IGNORE-Datei](how-to-save-write-experiment-files.md#storage-limits-of-experiment-snapshots), oder platzieren Sie sie nicht im Quellverzeichnis. Greifen Sie stattdessen mit einem Azure ML-[Dataset](how-to-train-with-datasets.md) auf Ihre Daten zu.

Weitere Informationen zum Konfigurieren von Aufträgen mit ScriptRunConfig finden Sie unter [Konfigurieren und Übermitteln von Trainingsausführungen](how-to-set-up-training-targets.md).

> [!WARNING]
> Wenn Sie zuvor den PyTorch-Schätzer zum Konfigurieren Ihrer PyTorch-Trainingsaufträge verwendet haben, beachten Sie, dass der Schätzer mit der Veröffentlichung von Release 1.19.0 des SDK als veraltet eingestuft wurde. Mit dem Azure Machine Learning SDK 1.15.0 oder höher ist ScriptRunConfig die empfohlene Vorgehensweise zum Konfigurieren von Trainingsaufträgen, einschließlich derjenigen, die Deep Learning-Frameworks verwenden. Allgemeine Fragen zur Migration finden Sie im [Leitfaden zur Migration vom Schätzer zu ScriptRunConfig](how-to-migrate-from-estimators-to-scriptrunconfig.md).

## <a name="submit-your-run"></a>Übermitteln Ihrer Ausführung

Das [Run-Objekt](/python/api/azureml-core/azureml.core.run%28class%29) bildet die Schnittstelle zum Ausführungsverlauf, während der Auftrag ausgeführt wird und nachdem er abgeschlossen wurde.

```Python
run = Experiment(ws, name='Tutorial-pytorch-birds').submit(src)
run.wait_for_completion(show_output=True)
```

### <a name="what-happens-during-run-execution"></a>Was passiert während der Ausführung
Die Ausführung durchläuft die folgenden Phasen:

- **Vorbereitung**: Ein Docker-Image wird entsprechend der definierten Umgebung erstellt. Das Image wird in die Containerregistrierung des Arbeitsbereichs hochgeladen und für spätere Ausführungen zwischengespeichert. Darüber hinaus werden Protokolle in den Ausführungsverlauf gestreamt, mit deren Hilfe der Status überwacht werden kann. Wenn stattdessen eine zusammengestellte Umgebung angegeben wird, wird das zwischengespeicherte Image verwendet, das diese zusammengestellte Umgebung unterstützt.

- **Skalierung**: Der Cluster versucht ein Hochskalieren, wenn der Batch KI-Cluster mehr Knoten zur Ausführung benötigt, als derzeit verfügbar sind.

- **Wird ausgeführt:** Alle Skripts im Skriptordner werden auf das Computeziel hochgeladen, Datenspeicher werden bereitgestellt oder kopiert, und `script` wird ausgeführt. Ausgaben aus „stdout“ und dem Ordner **./logs** werden in den Ausführungsverlauf gestreamt und können zur Überwachung der Ausführung verwendet werden.

- **Nachbearbeitung**: Der Ordner **./outputs** der Ausführung wird in den Ausführungsverlauf kopiert.

## <a name="register-or-download-a-model"></a>Registrieren oder Herunterladen eines Modells

Sobald Sie das Modell trainiert haben, können Sie es in Ihrem Arbeitsbereich registrieren. Die Modellregistrierung bietet die Möglichkeit, Ihre Modelle in Ihrem Arbeitsbereich zu speichern und zu versionieren, um die [Modellverwaltung und -bereitstellung](concept-model-management-and-deployment.md) zu vereinfachen.

```Python
model = run.register_model(model_name='pytorch-birds', model_path='outputs/model.pt')
```

> [!TIP]
> Die Schrittanleitung zur Bereitstellung enthält einen Abschnitt zur Registrierung von Modellen, aber Sie können direkt zu [Erstellen eines Computeziels](how-to-deploy-and-where.md#choose-a-compute-target) für die Bereitstellung springen, da Sie bereits über ein registriertes Modell verfügen.

Mit dem Run-Objekt können Sie auch eine lokale Kopie des Modells herunterladen. Im Trainingsskript `pytorch_train.py` wird das Modell durch ein Speicherobjekt von PyTorch persistent in einem lokalen Ordner (lokal für das Computeziel) gespeichert. Sie können das Run-Objekt verwenden, um eine Kopie herunterzuladen.

```Python
# Create a model folder in the current directory
os.makedirs('./model', exist_ok=True)

# Download the model from run history
run.download_file(name='outputs/model.pt', output_file_path='./model/model.pt'), 
```

## <a name="distributed-training"></a>Verteiltes Training

Azure Machine Learning unterstützt auch verteilte PyTorch-Aufträge auf mehreren Knoten, sodass Sie Ihre Trainingworkloads skalieren können. Sie können ganz einfach verteilte PyTorch-Aufträge ausführen. Azure ML verwaltet die Orchestrierung für Sie.

Azure ML unterstützt die Ausführung verteilter PyTorch-Aufträge sowohl mit Horovod als auch mit dem integrierten DistributedDataParallel-Modul von PyTorch.

### <a name="horovod"></a>Horovod
[Horovod](https://github.com/uber/horovod) ist ein Open-Source-Allreduce-Framework, das von Uber für verteiltes Training entwickelt wurde. Es bietet einen einfachen Weg zum Schreiben von verteiltem PyTorch-Code für das Training.

Ihr Trainingscode muss mit Horovod für verteiltes Training instrumentiert werden. Weitere Informationen zur Verwendung von Horovod mit PyTorch finden Sie in der [Horovod-Dokumentation](https://horovod.readthedocs.io/en/stable/pytorch.html).

Stellen Sie außerdem sicher, dass Ihre Trainingsumgebung das **horovod**-Paket enthält. Wenn Sie eine zusammengestellte PyTorch-Umgebung verwenden, ist Horovod bereits als eine der Abhängigkeiten enthalten. Wenn Sie Ihre eigene Umgebung verwenden, stellen Sie sicher, dass die Horovod-Abhängigkeit enthalten ist, z. B.:

```yaml
channels:
- conda-forge
dependencies:
- python=3.6.2
- pip:
  - azureml-defaults
  - torch==1.6.0
  - torchvision==0.7.0
  - horovod==0.19.5
```

Um einen verteilten Auftrag mit MPI/Horovod in Azure ML auszuführen, müssen Sie eine [MpiConfiguration](/python/api/azureml-core/azureml.core.runconfig.mpiconfiguration) für den `distributed_job_config`-Parameter des ScriptRunConfig-Konstruktors angeben. Der folgende Code konfiguriert einen auf 2 Knoten verteilten Auftrag, der einen Prozess pro Knoten ausführt. Wenn Sie auch mehrere Prozesse pro Knoten ausführen möchten (wenn die Cluster-SKU also über mehrere GPUs verfügt), geben Sie zusätzlich den `process_count_per_node`-Parameter in MpiConfiguration an (der Standardwert ist `1`).

```python
from azureml.core import ScriptRunConfig
from azureml.core.runconfig import MpiConfiguration

src = ScriptRunConfig(source_directory=project_folder,
                      script='pytorch_horovod_mnist.py',
                      compute_target=compute_target,
                      environment=pytorch_env,
                      distributed_job_config=MpiConfiguration(node_count=2))
```

Ein vollständiges Tutorial zum Ausführen von verteiltem PyTorch mit Horovod in Azure ML finden Sie unter [Verteiltes PyTorch mit Horovod](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/ml-frameworks/pytorch/distributed-pytorch-with-horovod).

### <a name="distributeddataparallel"></a>DistributedDataParallel
Wenn Sie das in PyTorch integrierte [DistributedDataParallel](https://pytorch.org/tutorials/intermediate/ddp_tutorial.html)-Modul verwenden, das mit dem **torch.distributed**-Paket im Trainingscode erstellt wird, können Sie den verteilten Auftrag auch über Azure ML starten.

Sie haben zwei Möglichkeiten, bei Azure ML einen verteilten PyTorch-Auftrag zu starten:
1. Starten pro Prozess: Geben Sie die Gesamtzahl von Workerprozessen an, die Sie ausführen möchten. Azure ML führt dann das Starten der einzelnen Prozesse durch.
2. Starten pro Knoten mit `torch.distributed.launch`: Geben Sie den `torch.distributed.launch`-Befehl an, den Sie auf den einzelnen Knoten ausführen möchten. Das Hilfsprogramm für den Torch-Start übernimmt das Starten der Workerprozesse auf den einzelnen Knoten.

Zwischen diesen Startoptionen gibt es keine grundlegenden Unterschiede. Die Vorgehensweise hängt größtenteils von den Vorlieben des Benutzers oder den Konventionen der Frameworks/Bibliotheken ab, die basierend auf Vanilla PyTorch erstellt wurden (z. B. Lightning oder Hugging Face).

#### <a name="per-process-launch"></a>Starten pro Prozess
Gehen Sie wie folgt vor, um diese Option zum Ausführen eines verteilten PyTorch-Auftrags zu nutzen:
1. Geben Sie das Trainingsskript und die Argumente an.
2. Erstellen Sie eine [PyTorchConfiguration](/python/api/azureml-core/azureml.core.runconfig.pytorchconfiguration), und geben Sie die Werte für `process_count` und `node_count` an. `process_count` steht für die Gesamtzahl von Prozessen, die Sie für Ihren Auftrag ausführen möchten. Normalerweise sollte dieser Wert der Anzahl von GPUs pro Knoten multipliziert mit der Anzahl von Knoten entsprechen. Wenn `process_count` nicht angegeben ist, wird von Azure ML standardmäßig ein Prozess pro Knoten gestartet.

Von Azure ML werden die folgenden Umgebungsvariablen festgelegt:
* `MASTER_ADDR`: IP-Adresse des Computers, auf dem der Prozess mit dem Rang 0 gehostet wird.
* `MASTER_PORT`: Ein freier Port auf dem Computer, auf dem der Prozess mit dem Rang 0 gehostet wird.
* `NODE_RANK`: Der Rang des Knotens in Bezug auf das Training mit mehreren Knoten. Die möglichen Werte sind 0 bis „Gesamtknotenzahl - 1“.
* `WORLD_SIZE`: Gesamtzahl von Prozessen. Dieser Wert sollte der Gesamtzahl von Geräten (GPU) entsprechen, die für das verteilte Training verwendet werden.
* `RANK`: Der (globale) Rang des aktuellen Prozesses. Die möglichen Werte sind 0 bis „WORLD_SIZE - 1“.
* `LOCAL_RANK`: Der lokale (relative) Rang des Prozesses auf dem Knoten. Die möglichen Werte sind 0 bis „Anzahl von Prozessen auf dem Knoten - 1“.

Da die erforderlichen Umgebungsvariablen für Sie von Azure ML festgelegt werden, können Sie die [Standardmethode zum Initialisieren der Umgebungsvariable](https://pytorch.org/docs/stable/distributed.html#environment-variable-initialization) verwenden, um die Prozessgruppe in Ihrem Trainingscode zu initialisieren.

Mit dem folgenden Codeausschnitt wird ein PyTorch-Auftrag mit zwei Knoten und zwei Prozessen pro Knoten konfiguriert:
```python
from azureml.core import ScriptRunConfig
from azureml.core.runconfig import PyTorchConfiguration

curated_env_name = 'AzureML-PyTorch-1.6-GPU'
pytorch_env = Environment.get(workspace=ws, name=curated_env_name)
distr_config = PyTorchConfiguration(process_count=4, node_count=2)

src = ScriptRunConfig(
  source_directory='./src',
  script='train.py',
  arguments=['--epochs', 25],
  compute_target=compute_target,
  environment=pytorch_env,
  distributed_job_config=distr_config,
)

run = Experiment(ws, 'experiment_name').submit(src)
```

> [!WARNING]
> Zum Verwenden dieser Option für das Training mit mehreren Prozessen pro Knoten müssen Sie mindestens Azure ML Python SDK 1.22.0 nutzen, weil `process_count` in Version 1.22.0 eingeführt wurde.

> [!TIP]
> Wenn mit Ihrem Trainingsskript Informationen wie lokaler Rang oder Rang als Skriptargumente übergeben werden, können Sie in den Argumenten auf die Variablen verweisen: `arguments=['--epochs', 50, '--local_rank', $LOCAL_RANK]`.

#### <a name="per-node-launch-with-torchdistributedlaunch"></a>Starten pro Knoten mit `torch.distributed.launch`
Für PyTorch wird unter [torch.distributed.launch](https://pytorch.org/docs/stable/distributed.html#launch-utility) ein Hilfsprogramm für das Starten bereitgestellt, mit dem Benutzer mehrere Prozesse pro Knoten starten können. Mit dem Modul `torch.distributed.launch` werden auf den einzelnen Knoten jeweils mehrere Trainingsprozesse erzeugt.

In den folgenden Schritten wird veranschaulicht, wie Sie einen PyTorch-Auftrag mit einem Startvorgang pro Knoten in Azure ML konfigurieren, der zum gleichen Ergebnis wie beim Ausführen des folgenden Befehls führt:

```shell
python -m torch.distributed.launch --nproc_per_node <num processes per node> \
  --nnodes <num nodes> --node_rank $NODE_RANK --master_addr $MASTER_ADDR \
  --master_port $MASTER_PORT --use_env \
  <your training script> <your script arguments>
```

1. Geben Sie den Befehl `torch.distributed.launch` für den Parameter `command` des Konstruktors `ScriptRunConfig` an. Azure ML führt diesen Befehl auf allen Knoten Ihres Trainingsclusters aus. Der Wert `--nproc_per_node` sollte kleiner oder gleich der Anzahl von GPUs sein, die auf einem Knoten verfügbar sind. Da `MASTER_ADDR`, `MASTER_PORT` und `NODE_RANK` von Azure ML festgelegt werden, können Sie im Befehl einfach auf die Umgebungsvariablen verweisen. Von Azure ML wird `MASTER_PORT` auf 6105 festgelegt, aber wenn Sie möchten, können Sie auch einen anderen Wert an das Argument `--master_port` des Befehls `torch.distributed.launch` übergeben. (Mit dem Hilfsprogramm für das Starten werden die Umgebungsvariablen zurückgesetzt.)
2. Erstellen Sie ein `PyTorchConfiguration`-Element, und geben Sie den Wert für `node_count` an. Sie müssen `process_count` nicht festlegen, da von Azure ML standardmäßig ein Prozess pro Knoten gestartet wird, für den der von Ihnen angegebene Startbefehl ausgeführt wird.

```python
from azureml.core import ScriptRunConfig
from azureml.core.runconfig import PyTorchConfiguration

curated_env_name = 'AzureML-PyTorch-1.6-GPU'
pytorch_env = Environment.get(workspace=ws, name=curated_env_name)
distr_config = PyTorchConfiguration(node_count=2)
launch_cmd = "python -m torch.distributed.launch --nproc_per_node 2 --nnodes 2 --node_rank $NODE_RANK --master_addr $MASTER_ADDR --master_port $MASTER_PORT --use_env train.py --epochs 50".split()

src = ScriptRunConfig(
  source_directory='./src',
  command=launch_cmd,
  compute_target=compute_target,
  environment=pytorch_env,
  distributed_job_config=distr_config,
)

run = Experiment(ws, 'experiment_name').submit(src)
```

Ein vollständiges Tutorial zum Ausführen von verteiltem PyTorch in Azure ML finden Sie unter [Verteiltes PyTorch mit DistributedDataParallel](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/ml-frameworks/pytorch/distributed-pytorch-with-distributeddataparallel).

### <a name="troubleshooting"></a>Problembehandlung

* **Horovod wurde heruntergefahren**: Wenn „AbortedError: Horovod wurde heruntergefahren“ auftritt, ist meistens in einem der Prozesse eine Ausnahme aufgetreten, die Horovod zum Herunterfahren veranlasst hat. Jeder Rang im MPI-Auftrag erhält eine eigene dedizierte Protokolldatei in Azure ML. Diese Protokolle haben die Bezeichnung `70_driver_logs`. Im Falle von verteiltem Training werden die Protokollnamen durch das Suffix `_rank` ergänzt, um eine einfachere Unterscheidung der Protokolle zu ermöglichen. Um den genauen Fehler zu finden, der das Herunterfahren von Horovod verursacht hat, gehen Sie alle Protokolldateien durch, und suchen Sie am Ende der driver_log-Dateien nach `Traceback`. Eine dieser Dateien enthält die eigentliche zugrunde liegende Ausnahme. 

## <a name="export-to-onnx"></a>Exportieren nach ONNX

Um Rückschlüsse mit der [ONNX-Runtime](concept-onnx.md) zu optimieren, konvertieren Sie Ihr trainiertes PyTorch-Modell in dass ONNX-Format. Rückschlüsse oder Modellbewertungen stellen die Phase dar, in der das bereitgestellte Modell für die Vorhersage verwendet wird (meist für Produktionsdaten). Ein Beispiel finden Sie im [Tutorial](https://github.com/onnx/tutorials/blob/master/tutorials/PytorchOnnxExport.ipynb).

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie ein neuronales Deep Learning-Netz mithilfe von PyTorch in Azure Machine Learning trainiert und registriert. Um zu erfahren, wie Sie ein Modell bereitstellen, fahren Sie mit unserem Artikel zur Modellbereitstellung fort.

* [Wie und wo Modelle bereitgestellt werden](how-to-deploy-and-where.md)
* [Erfassen einer Ausführungsmetrik während des Trainings](how-to-log-view-metrics.md)
* [Optimieren von Hyperparametern](how-to-tune-hyperparameters.md)
* [Bereitstellen eines trainierten Modells](how-to-deploy-and-where.md)
* [Verteiltes Trainieren von Deep Learning-Modellen in Azure](/azure/architecture/reference-architectures/ai/training-deep-learning)
