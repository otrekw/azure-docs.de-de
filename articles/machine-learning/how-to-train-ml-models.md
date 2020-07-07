---
title: Trainieren von ML-Modellen mit einem Estimator
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie Trainings auf einem einzelnen Knoten und verteilte Trainings konventioneller Machine Learning- und Deep Learning-Modelle mithilfe der Estimator-Klasse von Azure Machine Learning ausführen.
ms.author: maxluk
author: maxluk
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.reviewer: sgilley
ms.date: 03/09/2020
ms.custom: seodec18
ms.openlocfilehash: 53a5a78f776982ec9bf21e5e45e3e89eb93e5cce
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84433839"
---
# <a name="train-models-with-azure-machine-learning-using-estimator"></a>Trainieren von Azure Machine Learning-Modellen mit einem Estimator
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Mit Azure Machine Learning können Sie Ihr Trainingsskript ganz einfach an [verschiedene Computeziele](how-to-set-up-training-targets.md#compute-targets-for-training) übermitteln, indem Sie ein [RunConfiguration-Objekt](how-to-set-up-training-targets.md#whats-a-run-configuration) und ein [ScriptRunConfig-Objekt](how-to-set-up-training-targets.md#submit) verwenden. Dieses Muster bietet Ihnen ein hohes Maß an Flexibilität und maximale Kontrolle.


Die Estimator-Klasse vereinfacht das Trainieren von Modellen mit Deep Learning und vertiefendem Lernen. Sie bietet eine Abstraktion auf hoher Ebene, die Ihnen das einfache Erstellen von Laufzeitkonfigurationen ermöglicht. Sie können einen allgemeinen [Estimator](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator?view=azure-ml-py) erstellen und verwenden, um ein Trainingsskript mit einem beliebigen Lernframework (z.B. scikit-learn) zu übermitteln, das Sie auf einem beliebigen, von Ihnen ausgewählten Computerziel unabhängig davon ausführen können, ob es sich um Ihren lokalen Computer, eine einzelne VM in Azure oder einen GPU-Cluster in Azure handelt. Für PyTorch-, TensorFlow- und Chainer-Aufgaben sowie Aufgaben für vertiefendes Lernen bietet Azure Machine Learning außerdem die passenden Schätzer für [PyTorch](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.pytorch?view=azure-ml-py), [TensorFlow](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py), [Chainer](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.chainer?view=azure-ml-py) und [vertiefendes Lernen](how-to-use-reinforcement-learning.md), um die Verwendung dieser Frameworks zu vereinfachen.

## <a name="train-with-an-estimator"></a>Trainieren mit einem Estimator

Nach der Erstellung Ihres [Arbeitsbereichs](concept-workspace.md) und Einrichten Ihrer [Entwicklungsumgebung](how-to-configure-environment.md) umfasst das Trainieren eines Modells in Azure Machine Learning die folgenden Schritte:  
1. Erstellen eines [Remotecomputeziels](how-to-set-up-training-targets.md) (Sie können auch einen lokalen Computer als Computeziel verwenden)
2. Hochladen Ihrer [Trainingsdaten](how-to-access-data.md) in den Datenspeicher (optional)
3. Erstellen Ihres [Trainingsskripts](tutorial-train-models-with-aml.md#create-a-training-script)
4. Erstellen eines `Estimator`-Objekts
5. Übermitteln des Estimators an ein Experimentobjekt im Arbeitsbereich

Der Schwerpunkt dieses Artikels liegt auf den Schritten 4-5. Für die Schritte 1 bis 3 finden Sie im [Trainieren von Modellen-Tutorial](tutorial-train-models-with-aml.md) ein Beispiel.

### <a name="single-node-training"></a>Training auf einem einzelnen Knoten

Verwenden Sie einen `Estimator` für die Ausführung eines Trainings auf einem Einzelknoten auf einem Remotecomputeziel in Azure für ein SciKit-Lernmodell. Das [Computeziel](how-to-set-up-training-targets.md#amlcompute)-Objekt `compute_target` und Ihr [FileDataset](how-to-create-register-datasets.md)-Objekt `ds` sollten Sie bereits erstellt haben.

```Python
from azureml.train.estimator import Estimator

script_params = {
    # to mount files referenced by mnist dataset
    '--data-folder': ds.as_named_input('mnist').as_mount(),
    '--regularization': 0.8
}

sk_est = Estimator(source_directory='./my-sklearn-proj',
                   script_params=script_params,
                   compute_target=compute_target,
                   entry_script='train.py',
                   conda_packages=['scikit-learn'])
```

Der Codeausschnitt gibt die folgenden Parameter für den `Estimator`-Konstruktor an.

Parameter | BESCHREIBUNG
--|--
`source_directory`| Lokales Verzeichnis, das den gesamten für den Trainingsauftrag erforderlichen Code enthält. Dieser Ordner wird von Ihrem lokalen Computer auf das Remotecomputeziel kopiert.
`script_params`| Wörterbuch, in dem die an Ihr Trainingsskript zu übergebenden Befehlszeilenargumente `entry_script` in Form von `<command-line argument, value>`-Paaren festgelegt sind. Um ein ausführliches Flag in `script_params` anzugeben, verwenden Sie `<command-line argument, "">`.
`compute_target`| Remotecomputeziel, auf dem Ihr Trainingsskript ausgeführt wird, in diesem Fall ein Azure Machine Learning Compute-Cluster ([AmlCompute](how-to-set-up-training-targets.md#amlcompute)). (Hinweis: Auch wenn der AmlCompute-Cluster das gängige Ziel ist, besteht auch die Möglichkeit, andere Computezieltypen wie Azure-VMs oder sogar lokale Computer auszuwählen.)
`entry_script`| Dateipfad des Trainingsskripts (relativ zu `source_directory`), das auf dem Remotecomputeziel ausgeführt werden soll. Diese Datei und alle von ihr abhängigen Dateien sollten sich in diesem Ordner befinden.
`conda_packages`| Liste der Python-Pakete, die über conda installiert werden und für Ihr Trainingsskript erforderlich sind.  

Im Konstruktor kann zusätzlich der Parameter `pip_packages` für alle erforderlichen pip-Pakete angegeben werden.

Nun haben Sie Ihr `Estimator`-Objekt erstellt und können den auszuführenden Trainingsauftrag auf dem Remotecomputeziel über einen Aufruf der `submit`-Funktion auf Ihrem [Experiment](concept-azure-machine-learning-architecture.md#experiments)-Objekt `experiment` ausführen. 

```Python
run = experiment.submit(sk_est)
print(run.get_portal_url())
```

> [!IMPORTANT]
> **Spezielle Ordner** Zwei Ordner, *outputs* und *logs*, erhalten eine besondere Behandlung durch Azure Machine Learning. Wenn Sie während des Trainings Dateien in die Ordner *outputs* und *logs* schreiben, die zum Stammverzeichnis relativ sind (`./outputs` und `./logs`), werden diese Dateien automatisch in Ihren Ausführungsverlauf hochgeladen, damit Sie Zugriff darauf haben, wenn die Ausführung abgeschlossen ist.
>
> Um während des Trainings Artefakte zu erstellen (z.B. Modelldateien, Prüfpunkte, Datendateien oder gezeichnete Bilder), schreiben Sie diese in den `./outputs`-Ordner.
>
> Auf ähnliche Weise können Sie alle Protokolle aus Ihrer Trainingsausführung in den `./logs`-Ordner schreiben. Um die [TensorBoard-Integration](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/export-run-history-to-tensorboard/export-run-history-to-tensorboard.ipynb) in Azure Machine Learning zu nutzen, stellen Sie sicher, dass Sie Ihre TensorBoard-Protokolle in diesen Ordner schreiben. Während Ihre Ausführung läuft, können Sie TensorBoard starten und diese Protokolle streamen.  Später können Sie auch die Protokolle aus Ihrer vorherigen Ausführungen wiederherstellen.
>
> So laden Sie beispielsweise eine Datei herunter, die nach Ihrer Remotetrainingausführung in den *outputs*-Ordner auf Ihrem lokalen Computer geschrieben wurde: `run.download_file(name='outputs/my_output_file', output_file_path='my_destination_path')`

### <a name="distributed-training-and-custom-docker-images"></a>Verteiltes Training und benutzerdefinierte Docker-Images

Es gibt zwei zusätzliche Trainingsszenarien, die Sie mit dem `Estimator` durchführen können:
* Verwenden eines benutzerdefinierten Docker-Images
* Verteiltes Training in einem Cluster mit mehreren Knoten

Der folgende Code zeigt, wie Sie ein verteiltes Training für ein Keras-Modell ausführen können. Darüber hinaus wird anstelle der standardmäßigen Azure Machine Learning-Images ein benutzerdefiniertes Docker-Image vom Docker Hub `continuumio/miniconda` für das Training angegeben.

Das [Computeziel](how-to-set-up-training-targets.md#amlcompute)-Objekt `compute_target` sollten Sie bereits erstellt haben. Sie können den Estimator wie folgt erstellen:

```Python
from azureml.train.estimator import Estimator
from azureml.core.runconfig import MpiConfiguration

estimator = Estimator(source_directory='./my-keras-proj',
                      compute_target=compute_target,
                      entry_script='train.py',
                      node_count=2,
                      process_count_per_node=1,
                      distributed_training=MpiConfiguration(),        
                      conda_packages=['tensorflow', 'keras'],
                      custom_docker_image='continuumio/miniconda')
```

Im obigen Code werden im `Estimator`-Konstruktor die folgenden neuen Parameter angegeben:

Parameter | BESCHREIBUNG | Standard
--|--|--
`custom_docker_image`| Der Name des zu verwendenden Images. Geben Sie nur Images an, die in öffentlichen Docker-Repositorys (in diesem Fall Docker Hub) verfügbar sind. Um ein Image aus einem privaten Docker-Repository zu verwenden, verwenden Sie stattdessen den `environment_definition`-Parameter des Konstruktors. [Siehe das Beispiel](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/how-to-use-estimator/how-to-use-estimator.ipynb). | `None`
`node_count`| Die Anzahl der Knoten, die für Ihren Trainingsauftrag verwendet werden sollen. | `1`
`process_count_per_node`| Die Anzahl der Prozesse (oder „Worker“), die auf jedem Knoten ausgeführt werden sollen. In diesem Fall verwenden Sie die auf jedem Knoten verfügbaren `2`-GPUs.| `1`
`distributed_training`| [MPIConfiguration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfig.mpiconfiguration?view=azure-ml-py)-Objekt zum Starten des verteilten Trainings mithilfe des MPI-Back-Ends.  | `None`


Übermitteln Sie abschließend den Trainingsauftrag:
```Python
run = experiment.submit(estimator)
print(run.get_portal_url())
```

## <a name="registering-a-model"></a>Registrieren eines Modells

Sobald Sie das Modell trainiert haben, können Sie es in Ihrem Arbeitsbereich speichern und registrieren. Die Modellregistrierung bietet die Möglichkeit, Ihre Modelle in Ihrem Arbeitsbereich zu speichern und zu versionieren, um die [Modellverwaltung und -bereitstellung](concept-model-management-and-deployment.md) zu vereinfachen.

Wenn Sie den folgenden Code ausführen, wird das Modell in Ihrem Arbeitsbereich registriert, und es kann in Remotecomputekontexten oder Bereitstellungsskripts anhand des Namens referenziert werden. Weitere Informationen und zusätzliche Parameter finden Sie [`register_model`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py#register-model-model-name--model-path-none--tags-none--properties-none--model-framework-none--model-framework-version-none--description-none--datasets-none--sample-input-dataset-none--sample-output-dataset-none--resource-configuration-none----kwargs-) in der Referenzdokumentation.

```python
model = run.register_model(model_name='sklearn-sample', model_path=None)
```

## <a name="github-tracking-and-integration"></a>GitHub-Nachverfolgung und -Integration

Wenn Sie eine Trainingsausführung starten, bei der das Quellverzeichnis ein lokales Git-Repository ist, werden Informationen über das Repository im Ausführungsverlauf gespeichert. Weitere Informationen finden Sie unter [Git-Integration für Azure Machine Learning](concept-train-model-git-integration.md).

## <a name="examples"></a>Beispiele
Ein Notebook, das die grundlegenden Einstellungen eines Estimator-Musters zeigt, finden Sie unter:
* [how-to-use-azureml/training-with-deep-learning/how-to-use-estimator](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/how-to-use-estimator/how-to-use-estimator.ipynb)

Ein Notebook zum Trainieren eines scikit-learn-Modells mit einem Estimator finden Sie unter:
* [tutorials/img-classification-part1-training.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/tutorials/image-classification-mnist-data/img-classification-part1-training.ipynb)

Notebooks zum Trainieren von Modellen mit bestimmten für „deep-learning-framework“ spezifischen Estimatoren finden Sie unter:

* [how-to-use-azureml/ml-frameworks](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/ml-frameworks)

[!INCLUDE [aml-clone-in-azure-notebook](../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Nächste Schritte

* [Erfassen einer Ausführungsmetrik während des Trainings](how-to-track-experiments.md)
* [Trainieren von PyTorch-Modellen](how-to-train-pytorch.md)
* [Trainieren von TensorFlow-Modellen](how-to-train-tensorflow.md)
* [Trainieren eines Deep Neural Network für vertiefendes Lernen](how-to-use-reinforcement-learning.md)
* [Optimieren von Hyperparametern](how-to-tune-hyperparameters.md)
* [Bereitstellen eines trainierten Modells](how-to-deploy-and-where.md)
* [Erstellen und Verwalten von Umgebungen für Training und Bereitstellung](how-to-use-environments.md)