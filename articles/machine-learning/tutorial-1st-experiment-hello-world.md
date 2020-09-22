---
title: 'Tutorial: Ausführen des Python-Skripts „Hallo Welt“'
titleSuffix: Azure Machine Learning
description: Teil 2 der Azure ML-Einstiegsreihe zeigt, wie Sie ein triviales „Hallo Welt“-Python-Skript in die Cloud übermitteln.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: aminsaied
ms.author: amsaied
ms.reviewer: sgilley
ms.date: 09/15/2020
ms.custom: devx-track-python
ms.openlocfilehash: 98f4c30d03763e070b1bdc32a5e6e099556916ab
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90929495"
---
# <a name="tutorial-run-hello-world-python-script-part-2-of-4"></a>Tutorial: Ausführen des „Hallo Welt“-Python-Skripts (Teil 2 von 4)

In diesem Tutorial erfahren Sie, wie das Python SDK für Azure Machine Learning verwendet wird, um ein „Hallo Welt“-Python-Skript zu übermitteln und auszuführen.

Dieses Tutorial ist **Teil der vierteiligen Tutorialreihe**, in der Sie die Grundlagen von Azure Machine Learning kennenlernen und auftragsbasierte Machine Learning-Aufgaben in Azure ausführen. Dieses Tutorial baut auf der Arbeit auf, die Sie in [Tutorial Teil 1: Einrichten Ihres lokalen Computers für Azure Machine Learning](
tutorial-1st-experiment-sdk-setup-local.md) ausgeführt haben.

In diesem Lernprogramm führen Sie folgende Schritte aus:

> [!div class="checklist"]
> * Lokales Erstellen und Ausführen eines „Hallo Welt“-Python-Skripts
> * Erstellen eines Python-Steuerungsskripts zum Übermitteln von „Hallo Welt“ an Azure Machine Learning
> * Verstehen der Azure Machine Learning Konzepte im Steuerungsskript
> * Senden und Ausführen von „Hallo Welt“
> * Anzeigen Ihrer Codeausgabe in der Cloud

## <a name="prerequisites"></a>Voraussetzungen

- Schließen Sie [Teil 1 des Tutorials zur Einrichtung lokaler Computer](tutorial-1st-experiment-sdk-setup-local.md) ab, wenn Sie noch nicht über einen Azure Machine Learning-Arbeitsbereich verfügen.
- Einsteigerkenntnisse der Python-Sprache und der Machine Learning-Workflows.
- Lokale Entwicklungsumgebung. Dies schließt Visual Studio Code, Jupyter oder PyCharm ein, ist aber nicht auf diese beschränkt.
- Python (Version 3.5–3.7).

## <a name="create-and-run-a-python-script-locally"></a>Lokales Erstellen und Ausführen eines Python-Skripts

Erstellen Sie ein neues Unterverzeichnis mit dem Namen `src` unter dem Verzeichnis `tutorial` zur Speicherung von Code, den Sie auf einem Azure Machine Learning-Computecluster ausführen möchten. Erstellen Sie im `src`-Unterverzeichnis das Python-Skript `hello.py`:

```python
# src/hello.py
print("Hello world!")
```

Ihre Projektverzeichnisstruktur sieht nun wie folgt aus:

```Bash
tutorial
└──.azureml
|  └──config.json
└──src
|  └──hello.py
└──01-create-workspace.py
└──02-create-compute.py
```

### <a name="test-your-script-locally"></a>Lokales Testen des Skripts

Sie können Ihren Code lokal ausführen, was den Vorteil mit sich bringt, dass Sie den Code in Ihrer bevorzugten IDE oder einem Terminal interaktiv debuggen können:

```bash
cd <path/to/tutorial>
python ./src/hello.py
```

## <a name="create-a-control-script"></a>Erstellen eines Steuerungsskripts

Mithilfe eines *Steuerungsskripts* können Sie ihr `hello.py`-Skript in der Cloud ausführen.  Mit dem Steuerungsskript steuern Sie, wie und wo Ihr Machine Learning-Code ausgeführt wird.  

Erstellen Sie in Ihrem Tutorialverzeichnis eine neue Python-Datei mit dem Namen `03-run-hello.py`, kopieren Sie den unten abgebildeten Code, und fügen Sie ihn in diese Datei ein:

```python
# tutorial/03-run-hello.py
from azureml.core import Workspace, Experiment, Environment, ScriptRunConfig

ws = Workspace.from_config()
experiment = Experiment(workspace=ws, name='day1-experiment-hello')

config = ScriptRunConfig(source_directory='./src', script='hello.py', compute_target='cpu-cluster')

run = experiment.submit(config)
aml_url = run.get_portal_url()
print(aml_url)
```

### <a name="understand-the-code"></a>Grundlegendes zum Code

Eine kurze Beschreibung zur Funktionsweise des Steuerungsskripts:

:::row:::
   :::column span="":::
      `ws = Workspace.from_config()`
   :::column-end:::
   :::column span="2":::
      [Workspace](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py&preserve-view=true) stellt eine Verbindung mit Ihrem Azure Machine Learning-Arbeitsbereich her, so dass Sie mit Ihren Azure Machine Learning-Ressourcen kommunizieren können.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      `experiment =  Experiment( ... )`
   :::column-end:::
   :::column span="2":::
      [Experiment](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment?view=azure-ml-py&preserve-view=true) bietet eine einfache Möglichkeit, mehrere Ausführungen unter einem einzelnen Namen zu ordnen. Später können Sie sehen, wie Experimente den Vergleich von Metriken zwischen Dutzenden von Ausführungen einfach machen.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      `config = ScriptRunConfig( ... )` 
   :::column-end:::
   :::column span="2":::
      [ScriptRunConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.scriptrunconfig?view=azure-ml-py&preserve-view=true) umschließt Ihren `hello.py`-Code und übergibt ihn an Ihren Arbeitsbereich. Wie der Name schon nahelegt, können Sie diese Klasse verwenden, um die _Ausführung_ Ihres _Skripts_ in Azure Machine Learning zu _konfigurieren_. Außerdem wird angegeben, auf welchem Computeziel das Skript ausgeführt wird.  In diesem Code ist das Ziel der Computecluster, den Sie im [Setup-Tutorial](tutorial-1st-experiment-sdk-setup-local.md) erstellt haben.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      `run = experiment.submit(config)`
   :::column-end:::
   :::column span="2":::
       Übermittelt Ihr Skript. Diese Übermittlung wird als [Ausführung](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py&preserve-view=true) bezeichnet.  Eine Ausführung (Run) verkapselt eine einzelne Ausführung (Execution) Ihres Codes. Verwenden Sie eine Ausführung, um den Status Ihres Skripts zu überwachen, die Ausgabe zu erfassen, die Ergebnisse zu analysieren, die Metriken zu visualisieren und mehr.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      `aml_url = run.get_portal_url()` 
   :::column-end:::
   :::column span="2":::
        Das `run`-Objekt stellt ein Handle für die Ausführung Ihres Codes bereit. Überwachen Sie deren Status in Azure Machine Learning Studio mit der URL, die vom Python-Skript ausgegeben wird.  
   :::column-end:::
:::row-end:::

## <a name="submit-and-run-your-code-in-the-cloud"></a>Übermitteln und Ausführen Ihres Codes in der Cloud

Führen Sie Ihr Steuerungsskript aus, das seinerseits `hello.py` auf dem Computecluster ausführt, den Sie im [Setup-Tutorial](tutorial-1st-experiment-sdk-setup-local.md) erstellt haben.

```bash
python 03-run-hello.py
```

## <a name="monitor-your-code-in-the-cloud-using-studio"></a>Überwachen Ihres Codes in der Cloud mithilfe von Studio

Die Ausgabe enthält einen Link zum Azure Machine Learning-Studio, der etwa so aussieht: `https://ml.azure.com/experiments/hello-world/runs/<run-id>?wsid=/subscriptions/<subscription-id>/resourcegroups/<resource-group>/workspaces/<workspace-name>`.

Folgen Sie dem Link, und navigieren Sie zur Registerkarte **Ausgaben und Protokolle**. Dort sehen Sie eine Datei `70_driver_log.txt` etwa wie hier:

```txt
 1: [2020-08-04T22:15:44.407305] Entering context manager injector.
 2: [context_manager_injector.py] Command line Options: Namespace(inject=['ProjectPythonPath:context_managers.ProjectPythonPath', 'RunHistory:context_managers.RunHistory', 'TrackUserError:context_managers.TrackUserError', 'UserExceptions:context_managers.UserExceptions'], invocation=['hello.py'])
 3: Starting the daemon thread to refresh tokens in background for process with pid = 31263
 4: Entering Run History Context Manager.
 5: Preparing to call script [ hello.py ] with arguments: []
 6: After variable expansion, calling script [ hello.py ] with arguments: []
 7:
 8: Hello world!
 9: Starting the daemon thread to refresh tokens in background for process with pid = 31263
10:
11:
12: The experiment completed successfully. Finalizing run...
13: Logging experiment finalizing status in history service.
14: [2020-08-04T22:15:46.541334] TimeoutHandler __init__
15: [2020-08-04T22:15:46.541396] TimeoutHandler __enter__
16: Cleaning up all outstanding Run operations, waiting 300.0 seconds
17: 1 items cleaning up...
18: Cleanup took 0.1812913417816162 seconds
19: [2020-08-04T22:15:47.040203] TimeoutHandler __exit__
```

In Zeile 8 sehen Sie die Ausgabe „Hallo Welt!“ .

Die Datei `70_driver_log.txt` enthält die Standardausgabe einer Ausführung. Diese Datei kann zum Debuggen von Remoteausführungen in der Cloud nützlich sein.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie ein einfaches „Hallo Welt“-Skript in Azure ausgeführt. Sie haben erfahren, wie Sie die Verbindung mit Ihrem Azure Machine Learning-Arbeitsbereich herstellen, ein Experiment erstellen und Ihren `hello.py`-Code in die Cloud übermitteln.

Im nächsten Tutorial bauen Sie auf dem Gelernten auf, indem Sie etwas Interessanteres als `print("Hello world!")`ausführen.

> [!div class="nextstepaction"]
> [Tutorial 1: Trainieren eines Bildklassifizierungsmodells mit Azure Machine Learning](tutorial-1st-experiment-sdk-train.md)

>[!NOTE] 
> Wenn Sie die Tutorialreihe hier beenden und nicht mit dem nächsten Schritt fortfahren möchten, vergessen Sie bitte nicht, [Ihre Ressourcen zu bereinigen](tutorial-1st-experiment-bring-data.md#clean-up-resources)
