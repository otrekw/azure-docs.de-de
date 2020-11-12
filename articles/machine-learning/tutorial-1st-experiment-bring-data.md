---
title: 'Tutorial: Verwenden eigener Daten'
titleSuffix: Azure Machine Learning
description: In Teil 4 der Azure Machine Learning-Einstiegsreihe erfahren Sie, wie Sie Ihre eigenen Daten in einem Remotetrainingslauf verwenden.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: aminsaied
ms.author: amsaied
ms.reviewer: sgilley
ms.date: 09/15/2020
ms.custom: tracking-python
ms.openlocfilehash: 123e55202de8a33bca88afcfd1f0dc0c7edeae77
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2020
ms.locfileid: "93320094"
---
# <a name="tutorial-use-your-own-data-part-4-of-4"></a>Tutorial: Verwenden eigener Daten (Teil 4 von 4)

In diesem Tutorial wird gezeigt, wie Sie Ihre eigenen Daten hochladen und in Azure Machine Learning zum Trainieren von Machine Learning-Modellen verwenden können.

Dieses Tutorial ist *Teil 4 einer vierteiligen Tutorialreihe* , in der Sie die Grundlagen von Azure Machine Learning kennenlernen und auftragsbasierte Machine Learning-Aufgaben in Azure ausführen. Dieses Tutorial basiert auf den Schritten aus [Teil 1: Einrichten](tutorial-1st-experiment-sdk-setup-local.md), [Teil 2: Ausführen von „Hello World!“](tutorial-1st-experiment-hello-world.md) und [Teil 3: Trainieren eines Modells](tutorial-1st-experiment-sdk-train.md).

In [Teil 3: Trainieren eines Modells](tutorial-1st-experiment-sdk-train.md) wurden Daten mithilfe der integrierten `torchvision.datasets.CIFAR10`-Methode in der PyTorch-API heruntergeladen. In vielen Fällen möchten Sie aber eigene Daten in einem Remotetrainingslauf verwenden. Dieser Artikel zeigt den Workflow, den Sie verwenden können, um in Azure Machine Learning mit eigenen Daten zu arbeiten.

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Konfigurieren eines Trainingsskripts für die Verwendung von Daten in einem lokalen Verzeichnis.
> * Lokales Testen des Trainingsskripts.
> * Hochladen von Daten in Azure.
> * Erstellen eines Steuerungsskripts.
> * Verstehen der neuen Azure Machine Learning-Konzepte (Übergeben von Parametern, Datasets, Datenspeichern).
> * Senden und Ausführen des Trainingsskripts
> * Anzeigen Ihrer Codeausgabe in der Cloud

## <a name="prerequisites"></a>Voraussetzungen

* Abschluss von [Teil 3](tutorial-1st-experiment-sdk-train.md) der Reihe.
* Kenntnisse der Python-Sprache und der Machine Learning-Workflows auf Einstiegsniveau.
* Lokale Entwicklungsumgebung, z. B. Visual Studio Code, Jupyter oder PyCharm.
* Python (Version 3.5 bis 3.7).

## <a name="adjust-the-training-script"></a>Anpassen des Trainingsskripts
Mittlerweile können Sie Ihr Trainingsskript (tutorial/src/train.py) in Azure Machine Learning ausführen und die Leistung des Modells überwachen. Wir können das Trainingsskript durch die Einführung von Argumenten parametrisieren. Mithilfe von Argumenten können Sie verschiedene Hyperparameter problemlos vergleichen.

Derzeit ist unser Trainingsskript zurzeit so konfiguriert, dass bei jeder Ausführung das CIFAR10-Dataset heruntergeladen wird. Der folgende Python-Code wurde angepasst, um die Daten aus einem Verzeichnis zu lesen.

>[!NOTE] 
> Die Verwendung von `argparse` parametrisiert das Skript.

```python
# tutorial/src/train.py
import os
import argparse
import torch
import torch.optim as optim
import torchvision
import torchvision.transforms as transforms

from model import Net
from azureml.core import Run

run = Run.get_context()

if __name__ == "__main__":
    parser = argparse.ArgumentParser()
    parser.add_argument('--data_path', type=str, help='Path to the training data')
    parser.add_argument('--learning_rate', type=float, default=0.001, help='Learning rate for SGD')
    parser.add_argument('--momentum', type=float, default=0.9, help='Momentum for SGD')
    args = parser.parse_args()
    
    print("===== DATA =====")
    print("DATA PATH: " + args.data_path)
    print("LIST FILES IN DATA PATH...")
    print(os.listdir(args.data_path))
    print("================")
    
    # prepare DataLoader for CIFAR10 data
    transform = transforms.Compose([transforms.ToTensor(), transforms.Normalize((0.5, 0.5, 0.5), (0.5, 0.5, 0.5))])
    trainset = torchvision.datasets.CIFAR10(
        root=args.data_path,
        train=True,
        download=False,
        transform=transform,
    )
    trainloader = torch.utils.data.DataLoader(trainset, batch_size=4, shuffle=True, num_workers=2)

    # define convolutional network
    net = Net()

    # set up pytorch loss /  optimizer
    criterion = torch.nn.CrossEntropyLoss()
    optimizer = optim.SGD(
        net.parameters(),
        lr=args.learning_rate,
        momentum=args.momentum,
    )

    # train the network
    for epoch in range(2):

        running_loss = 0.0
        for i, data in enumerate(trainloader, 0):
            # unpack the data
            inputs, labels = data

            # zero the parameter gradients
            optimizer.zero_grad()

            # forward + backward + optimize
            outputs = net(inputs)
            loss = criterion(outputs, labels)
            loss.backward()
            optimizer.step()

            # print statistics
            running_loss += loss.item()
            if i % 2000 == 1999:
                loss = running_loss / 2000
                run.log('loss', loss) # log loss metric to AML
                print(f'epoch={epoch + 1}, batch={i + 1:5}: loss {loss:.2f}')
                running_loss = 0.0

    print('Finished Training')
```

### <a name="understanding-the-code-changes"></a>Grundlegendes zu den Codeänderungen

Der Code in `train.py` hat die `argparse`-Bibliothek zum Einrichten von `data_path`, `learning_rate` und `momentum` verwendet.

```python
# .... other code
parser = argparse.ArgumentParser()
parser.add_argument('--data_path', type=str, help='Path to the training data')
parser.add_argument('--learning_rate', type=float, default=0.001, help='Learning rate for SGD')
parser.add_argument('--momentum', type=float, default=0.9, help='Momentum for SGD')
args = parser.parse_args()
# ... other code
```

Außerdem wurde das `train.py`-Skript angepasst, um den Optimierer für die Verwendung der benutzerdefinierten Parameter zu aktualisieren:

```python
optimizer = optim.SGD(
    net.parameters(),
    lr=args.learning_rate,     # get learning rate from command-line argument
    momentum=args.momentum,    # get momentum from command-line argument
)
```

## <a name="test-the-script-locally"></a>Lokales Testen des Skripts

Ihr Skript akzeptiert nun _data path_ als Argument. Testen Sie es zunächst lokal. Fügen Sie Ihrer Tutorial-Verzeichnisstruktur einen Ordner mit dem Namen `data`hinzu. Ihre Verzeichnisstruktur sollte wie folgt aussehen:

```txt
tutorial
└──.azureml
|  └──config.json
|  └──pytorch-env.yml
└──data
└──src
|  └──hello.py
|  └──model.py
|  └──train.py
└──01-create-workspace.py
└──02-create-compute.py
└──03-run-hello.py
└──04-run-pytorch.py
```

Wenn Sie `train.py` im vorherigen Tutorial nicht lokal ausgeführt haben, ist das `data/`-Verzeichnis nicht vorhanden. Führen Sie in diesem Fall die `torchvision.datasets.CIFAR10` -Methode lokal mit `download=True` im `train.py`-Skript aus.

Zum lokalen Ausführen des geänderten Trainingsskripts nehmen Sie den folgenden Aufruf vor:

```bash
python src/train.py --data_path ./data --learning_rate 0.003 --momentum 0.92
```

Sie können den Download des CIFAR10-Datasets vermeiden, indem Sie einen lokalen Pfad zu den Daten übergeben. Sie können auch mit verschiedenen Werten für die Hyperparameter _learning rate_ und _momentum_ experimentieren, ohne sie im Trainingsskript hartcodieren zu müssen.

## <a name="upload-the-data-to-azure"></a>Hochladen der Daten in Azure

Damit Sie dieses Skript in Azure Machine Learning ausführen können, müssen Sie Ihre Trainingsdaten in Azure verfügbar machen. Der Azure Machine Learning-Arbeitsbereich ist mit einem _Standarddatenspeicher_ ausgestattet. Dabei handelt es sich um ein Azure Blob Storage-Konto, in dem Sie Ihre Trainingsdaten speichern können.

>[!NOTE] 
> In Azure Machine Learning können Sie Verbindungen zu anderen cloudbasierten Datenspeichern herstellen, in denen Ihre Daten gespeichert sind. Weitere Details finden Sie in der [Dokumentation zu Datenspeichern](./concept-data.md).  

Erstellen Sie ein neues Python-Steuerungsskript mit dem Namen `05-upload-data.py` im `tutorial`-Verzeichnis:

```python
# tutorial/05-upload-data.py
from azureml.core import Workspace
ws = Workspace.from_config()
datastore = ws.get_default_datastore()
datastore.upload(src_dir='./data', target_path='datasets/cifar10', overwrite=True)
```

Der `target_path`-Wert gibt den Pfad im Datenspeicher an, in den die CIFAR10-Daten hochgeladen werden sollen.

>[!TIP] 
> Während Sie Azure Machine Learning zum Hochladen der Daten verwenden, können Sie [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/) zum Hochladen von Ad-hoc-Dateien verwenden. Wenn Sie ein ETL-Tool benötigen, können Sie [Azure Data Factory](../data-factory/introduction.md) für die Erfassung Ihrer Daten in Azure verwenden.

Führen Sie die Python-Datei aus, um die Daten hochzuladen. (Der Upload sollte schnell sein und weniger als 60 Sekunden dauern.)

```bash
python 05-upload-data.py
```
Es sollte die folgende Standardausgabe angezeigt werden:
```txt
Uploading ./data\cifar-10-batches-py\data_batch_2
Uploaded ./data\cifar-10-batches-py\data_batch_2, 4 files out of an estimated total of 9
.
.
Uploading ./data\cifar-10-batches-py\data_batch_5
Uploaded ./data\cifar-10-batches-py\data_batch_5, 9 files out of an estimated total of 9
Uploaded 9 files
```


## <a name="create-a-control-script"></a>Erstellen eines Steuerungsskripts

Erstellen Sie wie bereits zuvor ein neues Python-Steuerungsskript, dem Sie den Namen `06-run-pytorch-data.py` geben:

```python
# tutorial/06-run-pytorch-data.py
from azureml.core import Workspace
from azureml.core import Experiment
from azureml.core import Environment
from azureml.core import ScriptRunConfig
from azureml.core import Dataset

if __name__ == "__main__":
    ws = Workspace.from_config()
    
    datastore = ws.get_default_datastore()
    dataset = Dataset.File.from_files(path=(datastore, 'datasets/cifar10'))

    experiment = Experiment(workspace=ws, name='day1-experiment-data')

    config = ScriptRunConfig(
        source_directory='./src',
        script='train.py',
        compute_target='cpu-cluster',
        arguments=[
            '--data_path', dataset.as_named_input('input').as_mount(),
            '--learning_rate', 0.003,
            '--momentum', 0.92],
        )
    
    # set up pytorch environment
    env = Environment.from_conda_specification(name='pytorch-env',file_path='.azureml/pytorch-env.yml')
    config.run_config.environment = env

    run = experiment.submit(config)
    aml_url = run.get_portal_url()
    print("Submitted to an Azure Machine Learning compute cluster. Click on the link below")
    print("")
    print(aml_url)
```

### <a name="understand-the-code-changes"></a>Erläuterung der Codeänderungen

Das Steuerungsskript ähnelt dem Skript aus [Teil 3 dieser Reihe](tutorial-1st-experiment-sdk-train.md), enthält aber die folgenden neuen Zeilen:

:::row:::
   :::column span="":::
      `dataset = Dataset.File.from_files( ... )`
   :::column-end:::
   :::column span="2":::
      Ein [Dataset](/python/api/azureml-core/azureml.core.dataset.dataset?preserve-view=true&view=azure-ml-py) wird verwendet, um auf die Daten zu verweisen, die Sie in Azure Blob Storage hochgeladen haben. Datasets stellen eine Abstraktionsschicht über Ihren Daten dar, die eine höhere Zuverlässigkeit und Vertrauenswürdigkeit sicherstellt.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      `config = ScriptRunConfig(...)`
   :::column-end:::
   :::column span="2":::
      [ScriptRunConfig](/python/api/azureml-core/azureml.core.scriptrunconfig?preserve-view=true&view=azure-ml-py) wurde geändert und enthält jetzt eine Liste von Argumenten, die an `train.py` übergeben werden. Das `dataset.as_named_input('input').as_mount()`-Argument bedeutet, dass das angegebene Verzeichnis im Computeziel _eingebunden_ wird.
   :::column-end:::
:::row-end:::

## <a name="submit-the-run-to-azure-machine-learning"></a>Senden der Ausführung an Azure Machine Learning

Übermitteln Sie die Ausführung jetzt noch einmal, damit die neue Konfiguration verwendet wird:

```bash
python 06-run-pytorch-data.py
```

Dieser Code gibt eine URL für das Experiment in Azure Machine Learning Studio aus. Wenn Sie zu diesem Link navigieren, können Sie Ihren Code bei der Ausführung sehen.

### <a name="inspect-the-log-file"></a>Überprüfen der Protokolldatei

Wechseln Sie in Studio zum Experimentlauf (durch Auswählen der vorherigen URL-Ausgabe), gefolgt von **Ausgaben und Protokolle**. Wählen Sie die Datei `70_driver_log.txt` aus. Die folgende Ausgabe sollte angezeigt werden:

```txt
Processing 'input'.
Processing dataset FileDataset
{
  "source": [
    "('workspaceblobstore', 'datasets/cifar10')"
  ],
  "definition": [
    "GetDatastoreFiles"
  ],
  "registration": {
    "id": "XXXXX",
    "name": null,
    "version": null,
    "workspace": "Workspace.create(name='XXXX', subscription_id='XXXX', resource_group='X')"
  }
}
Mounting input to /tmp/tmp9kituvp3.
Mounted input to /tmp/tmp9kituvp3 as folder.
Exit __enter__ of DatasetContextManager
Entering Run History Context Manager.
Current directory:  /mnt/batch/tasks/shared/LS_root/jobs/dsvm-aml/azureml/tutorial-session-3_1600171983_763c5381/mounts/workspaceblobstore/azureml/tutorial-session-3_1600171983_763c5381
Preparing to call script [ train.py ] with arguments: ['--data_path', '$input', '--learning_rate', '0.003', '--momentum', '0.92']
After variable expansion, calling script [ train.py ] with arguments: ['--data_path', '/tmp/tmp9kituvp3', '--learning_rate', '0.003', '--momentum', '0.92']

Script type = None
===== DATA =====
DATA PATH: /tmp/tmp9kituvp3
LIST FILES IN DATA PATH...
['cifar-10-batches-py', 'cifar-10-python.tar.gz']
```

Beachten Sie Folgendes:

- Azure Machine Learning hat Blob Storage in den Computecluster automatisch eingebunden.
- ``dataset.as_named_input('input').as_mount()``, das im Steuerungsskript verwendet wird, wird in den Bereitstellungspunkt aufgelöst.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [aml-delete-resource-group](../../includes/aml-delete-resource-group.md)]

Sie können die Ressourcengruppe auch behalten und einen einzelnen Arbeitsbereich löschen. Zeigen Sie die Eigenschaften des Arbeitsbereichs an, und klicken Sie auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie Daten mithilfe eines `Datastore` in Azure hochgeladen werden. Der Datenspeicher diente als Cloudspeicher für Ihren Arbeitsbereich und als permanenter und flexibler Ort für die Aufbewahrung Ihrer Daten.

Sie haben gelernt, wie Sie Ihr Trainingsskript an der Befehlszeile so verändern, dass es einen Datenpfad annimmt. Durch Verwendung eines `Dataset` konnten Sie ein Verzeichnis für die Remoteausführung bereitstellen. 

Nachdem Sie nun über ein Modell verfügen, lernen Sie Folgendes:

* [Bereitstellen von Modellen mit Azure Machine Learning](how-to-deploy-and-where.md).