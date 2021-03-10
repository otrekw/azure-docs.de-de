---
title: Migrieren von Schätzern zu ScriptRunConfig
titleSuffix: Azure Machine Learning
description: Migrationsleitfaden für die Migration von Schätzern zu ScriptRunConfig zum Konfigurieren von Trainingsaufträgen.
services: machine-learning
author: mx-iao
ms.author: minxia
ms.reviewer: sgilley
ms.service: machine-learning
ms.subservice: core
ms.date: 12/14/2020
ms.topic: conceptual
ms.custom: how-to, devx-track-python, contperf-fy21q1
ms.openlocfilehash: ae0623a11b940a4d142f6bfae02d4b20727a6f55
ms.sourcegitcommit: 956dec4650e551bdede45d96507c95ecd7a01ec9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2021
ms.locfileid: "102518871"
---
# <a name="migrating-from-estimators-to-scriptrunconfig"></a>Migrieren von Schätzern zu ScriptRunConfig

Bisher gab es mehrere Methoden zum Konfigurieren eines Trainingsauftrags in Azure Machine Learning über das SDK, darunter Schätzer, ScriptRunConfig und RunConfiguration auf niedrigerer Ebene.   Um diese Mehrdeutigkeit und Inkonsistenz zu beseitigen, vereinfachen wir den Prozess der Auftragskonfiguration in Azure ML.  Sie sollten jetzt ScriptRunConfig als empfohlene Option für die Konfiguration von Trainingsaufträgen verwenden. 

Schätzer sind mit der Version 1.19.0 des Python SDKs veraltet. Sie sollten es auch generell vermeiden, selbst explizit ein RunConfiguration-Objekt zu instanziieren, und stattdessen die Klasse „ScriptRunConfig“ verwenden, um Ihren Auftrag zu konfigurieren.

Dieser Artikel behandelt allgemeine Überlegungen zur Migration von Schätzern zu ScriptRunConfig.

> [!IMPORTANT]
> Stellen Sie sicher, dass Sie >= 1.15.0 des Python SDK verwenden, um von Schätzern zu ScriptRunConfig zu migrieren.

## <a name="scriptrunconfig-documentation-and-samples"></a>Dokumentation und Beispiele zu ScriptRunConfig
Die Azure Machine Learning-Dokumentation und -Beispiele wurden aktualisiert, um [ScriptRunConfig](/python/api/azureml-core/azureml.core.script_run_config.scriptrunconfig) für die Auftragskonfiguration und -übermittlung zu verwenden.

Informationen zur Verwendung von ScriptRunConfig finden Sie in der folgenden Dokumentation:
* [Übermitteln einer Trainingsausführung an ein Computeziel](how-to-set-up-training-targets.md)
* [Konfigurieren von PyTorch-Trainingsausführungen](how-to-train-pytorch.md)
* [Konfigurieren von TensorFlow-Trainingsausführungen](how-to-train-tensorflow.md)
* [Konfigurieren von scikit-learn-Trainingsausführungen](how-to-train-scikit-learn.md)

Beachten Sie außerdem die folgenden Beispiele und Tutorials:
* [Azure/MachineLearningNotebooks](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/ml-frameworks)
* [Azure/azureml-examples](https://github.com/Azure/azureml-examples)

## <a name="defining-the-training-environment"></a>Definieren der Trainingsumgebung
Während die verschiedenen Frameworkschätzer über vorkonfigurierte Umgebungen verfügen, die durch Docker-Images unterstützt werden, sind die Dockerfiles für diese Images privat.  Daher haben Sie keinen guten Einblick, was diese Umgebungen enthalten. Zusätzlich nehmen die Schätzer umgebungsbezogene Konfigurationen als individuelle Parameter (z. B. `pip_packages`, `custom_docker_image`) für ihre jeweiligen Konstruktoren auf.

Mithilfe von ScriptRunConfig werden alle umgebungsbezogenen Konfigurationen in dem `Environment`-Objekt gekapselt, das in den `environment`-Parameter des ScriptRunConfig-Konstruktors übergeben wird. Stellen Sie zum Konfigurieren eines Trainingsauftrags eine Umgebung bereit, die alle für Ihr Trainingsskript erforderlichen Abhängigkeiten aufweist. Wenn keine Umgebung angegeben wird, verwendet Azure ML eines der Azure ML-Basisimages, insbesondere das von `azureml.core.environment.DEFAULT_CPU_IMAGE` definierte, als Standardumgebung. Es gibt mehrere Möglichkeiten, eine Umgebung bereitzustellen:

* [Verwenden einer zusammengestellten Umgebung](how-to-use-environments.md#use-a-curated-environment) – Zusammengestellte Umgebungen sind vordefinierte Umgebungen, die standardmäßig in Ihrem Arbeitsbereich verfügbar sind. Es gibt eine entsprechende zusammengestellte Umgebung für jedes der vorkonfigurierten Docker-Images für Frameworks/Versionen, die die einzelnen Frameworkschätzer unterstützen.
* [Definieren einer eigenen benutzerdefinierten Umgebung](how-to-use-environments.md)

Hier ist ein Beispiel für die Verwendung der zusammengestellten PyTorch 1.6-Umgebung für das Training:

```python
from azureml.core import Workspace, ScriptRunConfig, Environment

curated_env_name = 'AzureML-PyTorch-1.6-GPU'
pytorch_env = Environment.get(workspace=ws, name=curated_env_name)

compute_target = ws.compute_targets['my-cluster']
src = ScriptRunConfig(source_directory='.',
                      script='train.py',
                      compute_target=compute_target,
                      environment=pytorch_env)
```

Wenn Sie **Umgebungsvariablen** angeben möchten, die für den Prozess festgelegt werden, in dem das Trainingsskript ausgeführt wird, verwenden Sie das Environment-Objekt:
```
myenv.environment_variables = {"MESSAGE":"Hello from Azure Machine Learning"}
```

Weitere Informationen zum Konfigurieren und Verwalten von Azure ML-Umgebungen finden Sie unter:
* [Verwenden von Umgebungen](how-to-use-environments.md)
* [Zusammengestellte Umgebungen](resource-curated-environments.md)
* [Trainieren mit einem benutzerdefinierten Docker-Image](how-to-train-with-custom-image.md)

## <a name="using-data-for-training"></a>Verwenden von Daten für das Training
### <a name="datasets"></a>Datasets
Wenn Sie ein Azure ML Dataset für das Training verwenden, übergeben Sie das Dataset mithilfe des Parameters `arguments` als Argument an Ihr Skript. Auf diese Weise erhalten Sie den Datenpfad (Bereitstellungspunkt oder Downloadpfad) im Trainingsskript über Argumente.

Das folgende Beispiel konfiguriert einen Trainingsauftrag, bei dem das FileDataset, `mnist_ds`, auf der entfernten Compute-Instanz eingebunden wird.
```python
src = ScriptRunConfig(source_directory='.',
                      script='train.py',
                      arguments=['--data-folder', mnist_ds.as_mount()], # or mnist_ds.as_download() to download
                      compute_target=compute_target,
                      environment=pytorch_env)
```

### <a name="datareference-old"></a>DataReference (alt)
Wir empfehlen zwar, Azure ML-Datasets anstatt der alten DataReference-Methode zu verwenden, aber wenn Sie aus irgendeinem Grund noch DataReferences verwenden, müssen Sie Ihren Auftrag wie folgt konfigurieren:
```python
# if you want to pass a DataReference object, such as the below:
datastore = ws.get_default_datastore()
data_ref = datastore.path('./foo').as_mount()

src = ScriptRunConfig(source_directory='.',
                      script='train.py',
                      arguments=['--data-folder', str(data_ref)], # cast the DataReference object to str
                      compute_target=compute_target,
                      environment=pytorch_env)
src.run_config.data_references = {data_ref.data_reference_name: data_ref.to_config()} # set a dict of the DataReference(s) you want to the `data_references` attribute of the ScriptRunConfig's underlying RunConfiguration object.
```

Weitere Informationen zum Verwenden von Daten für das Training finden Sie unter:
* [Trainieren mit Datasets in Azure ML](./how-to-train-with-datasets.md)

## <a name="distributed-training"></a>Verteiltes Training
Wenn Sie einen verteilten Auftrag für das Training konfigurieren müssen, geben Sie dazu den `distributed_job_config`-Parameter im ScriptRunConfig-Konstruktor an. Übergeben Sie eine [MpiConfiguration](/python/api/azureml-core/azureml.core.runconfig.mpiconfiguration), [PyTorchConfiguration](/python/api/azureml-core/azureml.core.runconfig.pytorchconfiguration) oder [TensorflowConfiguration](/python/api/azureml-core/azureml.core.runconfig.tensorflowconfiguration) für verteilte Aufträge der jeweiligen Typen.

Das folgende Beispiel konfiguriert einen PyTorch-Trainingsauftrag für die Verwendung von verteiltem Training mit MPI/Horovod:
```python
from azureml.core.runconfig import MpiConfiguration

src = ScriptRunConfig(source_directory='.',
                      script='train.py',
                      compute_target=compute_target,
                      environment=pytorch_env,
                      distributed_job_config=MpiConfiguration(node_count=2, process_count_per_node=2))
```

Weitere Informationen finden Sie unter
* [Verteiltes Training](how-to-train-pytorch.md#distributed-training) (PyTorch)
* [Verteiltes Training](how-to-train-tensorflow.md#distributed-training) (TensorFlow)

## <a name="miscellaneous"></a>Verschiedenes
Wenn Sie aus einem beliebigen Grund auf das zugrunde liegende RunConfiguration-Objekt für ein ScriptRunConfig zugreifen müssen, können Sie wie folgt vorgehen:
```
src.run_config
```

## <a name="next-steps"></a>Nächste Schritte

* [Übermitteln einer Trainingsausführung an ein Computeziel](how-to-set-up-training-targets.md)