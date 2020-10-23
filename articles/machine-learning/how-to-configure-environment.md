---
title: Einrichten der Entwicklungsumgebung | Python
titleSuffix: Azure Machine Learning
description: Hier erfahren Sie, wie Sie eine Python-Entwicklungsumgebung für Azure Machine Learning einrichten. Verwenden Sie Conda-Umgebungen, erstellen Sie Konfigurationsdateien und konfigurieren Sie Ihren eigenen cloudbasierten Notebook-Server sowie Ihre eigenen Jupyter Notebooks, Azure Databricks, IDEs und Code-Editoren und die Data Science Virtual Machine.
services: machine-learning
author: rastala
ms.author: roastala
ms.service: machine-learning
ms.subservice: core
ms.reviewer: larryfr
ms.date: 09/30/2020
ms.topic: conceptual
ms.custom: how-to, devx-track-python, contperfq1
ms.openlocfilehash: 98bc108af22491c6ea0b64bc2e278b6b32f43a5b
ms.sourcegitcommit: 957c916118f87ea3d67a60e1d72a30f48bad0db6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2020
ms.locfileid: "92203091"
---
# <a name="set-up-a-development-environment-for-azure-machine-learning"></a>Einrichten einer Entwicklungsumgebung für Azure Machine Learning

Hier erfahren Sie, wie Sie eine Python-Entwicklungsumgebung für Azure Machine Learning konfigurieren.

In der folgenden Tabelle sind die einzelnen in diesem Artikel behandelten Entwicklungsumgebungen zusammen mit den jeweiligen Vor- und Nachteilen aufgeführt.

| Environment | Vorteile | Nachteile |
| --- | --- | --- |
| [Lokale Umgebung](#local) | Vollständige Kontrolle über Ihre Entwicklungsumgebung und die Abhängigkeiten. Ausführung mit einem Buildtool, einer Umgebung oder einer IDE Ihrer Wahl. | Der Einstieg dauert länger. Die erforderlichen SDK-Pakete müssen installiert werden. Außerdem muss eine Umgebung installiert werden, wenn noch keine vorhanden ist. |
| [Azure Machine Learning-Computeinstanz](#compute-instance) | Einfachste Methode für den Einstieg. Das gesamte SDK ist bereits auf der Arbeitsbereichs-VM installiert. Die Notebooktutorials sind vorab geklont und bereit zur Ausführung. | Keine Kontrolle über Ihre Entwicklungsumgebung und die Abhängigkeiten. Zusätzliche Kosten für den virtuellen Linux-Computer (virtueller Computer kann bei Nichtverwendung beendet werden, um Gebühren zu vermeiden). Preisdetails finden Sie [hier](https://azure.microsoft.com/pricing/details/virtual-machines/linux/). |
| [Azure Databricks](#aml-databricks) | Ideal für die Ausführung umfangreicher Workflows mit maschinellem Lernen auf der skalierbaren Apache Spark-Plattform. | Übermaß für experimentelles maschinelles Lernen oder kleinere Experimente und Workflows. Zusätzliche Kosten für Azure Databricks. Preisdetails finden Sie [hier](https://azure.microsoft.com/pricing/details/databricks/). |
| [Data Science Virtual Machine (DSVM)](#dsvm) | Vergleichbar mit der cloudbasierten Compute-Instanz (Python und das SDK sind vorinstalliert), aber mit zusätzlichen gängigen vorinstallierten Data Science- und Machine Learning-Tools. Einfache Skalierung und Kombination mit anderen benutzerdefinierten Tools und Workflows. | Langsamerer Einstieg im Vergleich zur cloudbasierten Compute-Instanz. |

Dieser Artikel enthält außerdem zusätzliche Anwendungstipps für die folgenden Tools:

* Jupyter Notebooks: Wenn Sie Jupyter Notebooks bereits verwenden, enthält das SDK einige zusätzliche Elemente, die Sie installieren sollten.

* Visual Studio Code: Wenn Sie Visual Studio Code verwenden, enthält die [Azure Machine Learning-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vscode-ai) eine umfassende Sprachunterstützung für Python sowie Features, mit denen die Arbeit mit Azure Machine Learning wesentlich bequemer und produktiver wird.

## <a name="prerequisites"></a>Voraussetzungen

* Einen Azure Machine Learning-Arbeitsbereich. Wenn Sie über keinen verfügen, können Sie einen Azure Machine Learning-Arbeitsbereich über das [Azure-Portal](how-to-manage-workspace.md), die [Azure CLI](how-to-manage-workspace-cli.md#create-a-workspace) oder über [Azure Resource Manager-Vorlagen (ARM)](how-to-create-workspace-template.md) erstellen.

### <a name="local-and-dsvm-only-create-a-workspace-configuration-file"></a><a id="workspace"></a> (Nur lokal und DSVM) Erstellen einer Arbeitsbereichskonfigurationsdatei

Die Arbeitsbereichs-Konfigurationsdatei ist eine JSON-Datei, die das SDK zur Kommunikation mit Ihrem Azure Machine Learning-Arbeitsbereich instruiert. Die Datei weist den Namen *config.json* und das folgende Format auf:

```json
{
    "subscription_id": "<subscription-id>",
    "resource_group": "<resource-group>",
    "workspace_name": "<workspace-name>"
}
```

Diese JSON-Datei muss sich in der Verzeichnisstruktur befinden, die Ihre Python-Skripts oder Jupyter Notebooks enthält. Sie kann sich im selben Verzeichnis, in einem Unterverzeichnis namens *.azureml* oder in einem übergeordneten Verzeichnis befinden.

Verwenden Sie die [`Workspace.from_config`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py#from-config-path-none--auth-none---logger-none---file-name-none-&preserve-view=true)-Methode, um diese Datei in Ihrem Code zu verwenden. Dieser Code lädt die Informationen aus der Datei und stellt eine Verbindung mit Ihrem Arbeitsbereich her.

Für das Erstellen einer Arbeitsbereichskonfigurationsdatei können Sie eine der folgenden beiden Methoden verwenden:

* Azure-Portal

    **Datei herunterladen**: Wählen Sie im [Azure-Portal](https://ms.portal.azure.com) im Abschnitt **Übersicht** Ihres Arbeitsbereichs **config.json herunterladen** aus.

    ![Azure-Portal](./media/how-to-configure-environment/configure.png)

* Python-SDK für Azure Machine Learning

    Erstellen Sie ein Skript, um eine Verbindung zu Ihrem Azure Machine Learning-Arbeitsbereich herzustellen, und verwenden Sie die [`write_config`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py#write-config-path-none--file-name-none-&preserve-view=true)-Methode, um Ihre Datei zu erstellen. Speichern Sie sie als *.azureml/config.json*. Achten Sie darauf, dass Sie `subscription_id`, `resource_group` und `workspace_name` durch Ihre eigenen Werte ersetzen.

    ```python
    from azureml.core import Workspace

    subscription_id = '<subscription-id>'
    resource_group  = '<resource-group>'
    workspace_name  = '<workspace-name>'

    try:
        ws = Workspace(subscription_id = subscription_id, resource_group = resource_group, workspace_name = workspace_name)
        ws.write_config()
        print('Library configuration succeeded')
    except:
        print('Workspace not found')
    ```

## <a name="local-computer"></a><a id="local"></a>Lokaler Computer

Konfigurieren einer lokalen Entwicklungsumgebung (dies könnte auch eine Remote-VM wie eine Azure Machine Learning-Compute-Instanz oder DSVM sein):

1. Erstellen Sie eine virtuelle Python-Umgebung (virtualenv, conda).

    > [!NOTE]
    > Obwohl es nicht erforderlich ist, wird empfohlen, dass Sie [Anaconda](https://www.anaconda.com/download/) oder [Miniconda](https://www.anaconda.com/download/) verwenden, um virtuelle Python-Umgebungen zu verwalten und Pakete zu installieren.

    > [!IMPORTANT]
    > Wenn Sie unter Linux oder macOS eine andere Shell als Bash verwenden (z.B. zsh), werden beim Ausführen einiger Befehle möglicherweise Fehler ausgegeben. Um dieses Problem zu umgehen, starten Sie mit dem Befehl `bash` eine neue Bash-Shell starten, und führen Sie die Befehle in dieser aus.

1. Aktivieren Sie Ihre neu erstellte virtuelle Python-Umgebung.
1. Installieren Sie das [Python SDK für Azure Machine Learning](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py&preserve-view=true).
1. [Erstellen Sie eine Arbeitsbereichskonfigurationsdatei](#workspace), oder verwenden Sie eine vorhandene, um Ihre lokale Umgebung so zu konfigurieren, dass Ihr Azure Machine Learning-Arbeitsbereich verwendet wird.

Da Sie nun Ihre lokale Umgebung eingerichtet haben, können Sie mit der Verwendung von Azure Machine Learning beginnen. Informationen zu den ersten Schritten finden Sie unter [Tutorial: Erste Schritte mit Azure Machine Learning in Ihrer Entwicklungsumgebung (Teil 1 von 4)](tutorial-1st-experiment-sdk-setup-local.md).

### <a name="jupyter-notebooks"></a><a id="jupyter"></a>Jupyter Notebooks

Wenn Sie einen lokalen Jupyter Notebook-Server betreiben, sollten Sie ein IPython-Kernel für Ihre virtuelle Python-Umgebung erstellen. Dies unterstützt Sie dabei, für das erwartete Kernel- und Paketimportverhalten zu sorgen.

1. Aktivieren umgebungsspezifischer IPython-Kernel

    ```bash
    conda install notebook ipykernel
    ```

1. Erstellen Sie ein Kernel für Ihre virtuelle Python-Umgebung. Ersetzen Sie `<myenv>` durch den Namen Ihrer virtuellen Python-Umgebung.

    ```bash
    ipython kernel install --user --name <myenv> --display-name "Python (myenv)"
    ```

1. Starten des Jupyter Notebook-Servers

Unter [Beispielnotebooks des Azure Machine Learning-Diensts](https://github.com/Azure/MachineLearningNotebooks) finden Sie Informationen zu den ersten Schritten mit Azure Machine Learning und Jupyter Notebook-Instanzen.

### <a name="visual-studio-code"></a><a id="vscode"></a>Visual Studio Code

So verwenden Visual Studio Code für die Entwicklung

1. Installieren Sie [Visual Studio Code](https://code.visualstudio.com/Download).
1. Installieren Sie die [Visual Studio Code-Erweiterung für Azure Machine Learning (Vorschau)](tutorial-setup-vscode-extension.md).

Sobald Sie die Visual Studio Code-Erweiterung installiert haben, können Sie Ihre [Azure Machine Learning-Ressourcen](how-to-manage-resources-vscode.md) verwalten, [Experimente ausführen und debuggen](how-to-debug-visual-studio-code.md) und [trainierte Modelle bereitstellen](tutorial-train-deploy-image-classification-model-vscode.md).

## <a name="azure-machine-learning-compute-instance"></a><a id="compute-instance"></a>Azure Machine Learning-Computeinstanz

Die Azure Machine Learning-[Compute-Instanz](concept-compute-instance.md) ist eine sichere, cloudbasierte Azure-Arbeitsstation, die wissenschaftlichen Fachkräften für Daten einen Jupyter Notebook-Server, JupyterLab und eine vollständige Machine Learning-Umgebung bereitstellt.

Für eine Compute-Instanz muss nichts installiert oder konfiguriert werden.  

In Ihrem Azure Machine Learning-Arbeitsbereich können Sie jederzeit eine Notebook-VM erstellen. Geben Sie einfach einen Namen und einen Azure-VM-Typ an. Sie die Einrichtung unter [Tutorial: Einrichten der Umgebung und des Arbeitsbereichs](tutorial-1st-experiment-sdk-setup.md).

Weitere Informationen zu Compute-Instanzen, einschließlich der Installation von Paketen, finden Sie unter [Compute-Instanzen](concept-compute-instance.md).

> [!TIP]
> [Halten Sie die Compute-Instanz an](tutorial-1st-experiment-bring-data.md#clean-up-resources), um wiederkehrende Kosten für eine nicht genutzte Compute-Instanz zu vermeiden.

Zusätzlich zu einem Jupyter Notebook-Server und JupyterLab können Sie Compute-Instanzen im [integrierten Notebook-Feature in Azure Machine Learning Studio](how-to-run-jupyter-notebooks.md) verwenden.

Sie können auch die Visual Studio Code-Erweiterung für Azure Machine Learning verwenden, um [eine Azure Machine Learning-Compute-Instanz als Remoteserver für Jupyter Notebook zu konfigurieren](how-to-set-up-vs-code-remote.md#configure-compute-instance-as-remote-notebook-server).

## <a name="data-science-virtual-machine"></a><a id="dsvm"></a>Data Science Virtual Machine

Die DSVM ist ein benutzerdefiniertes VM-Image. Sie ist für Data-Science-Aufgaben konzipiert und mit folgenden Tools und folgender Software vorab konfiguriert:

  - Pakete wie TensorFlow, PyTorch, Scikit-learn, XGBoost und das Azure Machine Learning SDK.
  - Beliebte Data Science-Tools wie Spark (eigenständig) und Drill.
  - Azure-Tools wie die Azure CLI, AzCopy und Storage-Explorer.
  - Integrierte Entwicklungsumgebungen (IDEs), z.B. Visual Studio Code und PyCharm.
  - Jupyter Notebook-Server

Eine umfassendere Liste der Tools finden Sie unter [Welche Tools sind in Azure Data Science Virtual Machine enthalten?](data-science-virtual-machine/tools-included.md)

> [!IMPORTANT]
> Wenn Sie eine DSVM-Instanz als [Computeziel](concept-compute-target.md) für Trainings- oder Rückschlussaufträge verwenden möchten, wird nur Ubuntu unterstützt.

Verwenden einer DSVM-Instanz als Entwicklungsumgebung

1. Erstellen Sie mithilfe einer der folgenden Methoden eine DSVM-Instanz:

    * Verwenden Sie das Azure-Portal, um eine DSVM-Instanz für [Ubuntu](data-science-virtual-machine/dsvm-ubuntu-intro.md) oder [Windows](data-science-virtual-machine/provision-vm.md) zu erstellen.
    * [Erstellen Sie eine DSVM-Instanz mithilfe von ARM-Vorlagen](data-science-virtual-machine/dsvm-tutorial-resource-manager.md).
    * Verwenden der Azure-CLI

        Verwenden Sie den folgenden Befehl, um eine DSVM-Instanz für Ubuntu zu erstellen:

        ```azurecli-interactive
        # create a Ubuntu DSVM in your resource group
        # note you need to be at least a contributor to the resource group in order to execute this command successfully
        # If you need to create a new resource group use: "az group create --name YOUR-RESOURCE-GROUP-NAME --location YOUR-REGION (For example: westus2)"
        az vm create --resource-group YOUR-RESOURCE-GROUP-NAME --name YOUR-VM-NAME --image microsoft-dsvm:linux-data-science-vm-ubuntu:linuxdsvmubuntu:latest --admin-username YOUR-USERNAME --admin-password YOUR-PASSWORD --generate-ssh-keys --authentication-type password
        ```

        Verwenden Sie den folgenden Befehl, um eine DSVM-Instanz für Windows zu erstellen:

        ```azurecli-interactive
        # create a Windows Server 2016 DSVM in your resource group
        # note you need to be at least a contributor to the resource group in order to execute this command successfully
        az vm create --resource-group YOUR-RESOURCE-GROUP-NAME --name YOUR-VM-NAME --image microsoft-dsvm:dsvm-windows:server-2016:latest --admin-username YOUR-USERNAME --admin-password YOUR-PASSWORD --authentication-type password
        ```

1. Aktivieren Sie die Conda-Umgebung, die das Azure Machine Learning SDK enthält.

    * Für die DSVM unter Ubuntu:

        ```bash
        conda activate py36
        ```

    * Für die DSVM unter Windows:

        ```bash
        conda activate AzureML
        ```

1. [Erstellen Sie eine Arbeitsbereichskonfigurationsdatei](#workspace), oder verwenden Sie eine vorhandene, um die DSVM-Instanz so zu konfigurieren, dass der Azure Machine Learning-Arbeitsbereich genutzt wird.

Ähnlich wie bei lokalen Umgebungen können Sie Visual Studio Code und die [Visual Studio Code-Erweiterung für Azure Machine Learning](#vscode) verwenden, um mit Azure Machine Learning zu interagieren.

Weitere Informationen finden Sie unter [Data Science Virtual Machines](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/).

## <a name="azure-databricks"></a><a name="aml-databricks"></a> Azure Databricks

Azure Databricks ist eine Apache Spark-basierte Umgebung in der Azure-Cloud. Sie stellt eine Notebook-basierte Umgebung mit CPU- oder GPU-basierten Computeclustern für die Zusammenarbeit bereit.

Azure Databricks kann auf folgende Weisen mit Azure Machine Learning verwendet werden:

+ Sie können mit Spark MLlib ein Modell trainieren und es über Azure Databricks in ACI/AKS bereitstellen.
+ Sie können mithilfe eines speziellen Azure ML SDK außerdem Features für [automatisiertes Machine Learning](concept-automated-ml.md) mit Azure Databricks verwenden.
+ Sie können Azure Databricks über eine [Azure Machine Learning-Pipeline](concept-ml-pipelines.md) als Computeziel verwenden.

### <a name="set-up-your-databricks-cluster"></a>Einrichten des Databricks-Clusters

Erstellen Sie einen [Databricks-Cluster](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal). Einige Einstellungen sind nur erforderlich, wenn Sie das SDK für automatisiertes Machine Learning in Databricks verwenden.
**Die Erstellung des Clusters dauert einige Minuten.**

Verwenden Sie die folgenden Einstellungen:

| Einstellung |Anwendungsbereich| Wert |
|----|---|---|
| Clustername |immer| IhrClustername |
| Databricks Runtime |immer|Nicht-ML-Runtime 7.1 (Scala 2.21, Spark 3.0.0) |
| Python-Version |immer| 3 |
| Worker |immer| 2 oder mehr |
| VM-Typen des Workerknotens <br>(bestimmt die maximale Anzahl gleichzeitiger Iterationen) |Automatisiertes maschinelles Lernen<br>Machine Learning| Arbeitsspeicheroptimierte VM bevorzugt |
| Automatische Skalierung aktivieren |Automatisiertes maschinelles Lernen<br>Machine Learning| Deaktivieren |

Warten Sie, bis der Cluster ausgeführt wird, bevor Sie fortfahren.

### <a name="install-the-correct-sdk-into-a-databricks-library"></a>Installieren des richtigen SDK in einer Databricks-Bibliothek

Erstellen Sie nach der Ausführung des Clusters [eine Bibliothek](https://docs.databricks.com/user-guide/libraries.html#create-a-library), um das entsprechende Azure Machine Learning SDK-Paket Ihrem Cluster anzufügen. Für automatisiertes maschinelles Lernen können Sie mit dem Abschnitt [SDK für Databricks mit automatisiertem maschinellen Lernen](#sdk-for-databricks-with-automated-machine-learning) fortfahren.

1. Klicken Sie mit der rechten Maustaste auf den aktuellen Arbeitsbereichsordner, in dem Sie die Bibliothek speichern möchten. Wählen Sie **Bibliothek** > **erstellen** aus.

1. Wählen Sie die folgende Option aus (weitere SDK-Installationen werden nicht unterstützt).

   |Zusatzkomponenten für &nbsp;SDK-Paket&nbsp;|`Source`|&nbsp;PyPi-Name&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|
   |----|---|---|
   |Für Databricks| Python Egg oder PyPI hochladen | azureml-sdk[databricks]|

   > [!Warning]
   > Sie können keine weiteren SDK-Zusatzkomponenten installieren. Wählen Sie nur die [`databricks`]-Option aus.

   * Wählen Sie nicht **Attach automatically to all clusters** (Automatisch an alle Cluster anfügen) aus.
   * Wählen Sie **Anfügen** neben dem Namen Ihres Clusters aus.

1. Der Status wird in **Angefügt** geändert. Dieser Vorgang kann einige Minuten in Anspruch nehmen. Überprüfen Sie währenddessen, ob Fehler auftreten.  Wenn bei diesem Schritt ein Fehler auftritt:

   Versuchen Sie, Ihren Cluster wie folgt neu zu starten:
   1. Wählen Sie im linken Bereich die Option **Cluster** aus.
   1. Wählen Sie in der Tabelle den Namen Ihres Clusters aus.
   1. Klicken Sie auf der Registerkarte **Bibliotheken** auf **Neu starten**.

   Berücksichtigen Sie außerdem Folgendes:
   + Fügen Sie in der AutoML-Konfiguration bei Verwendung von Azure Databricks die folgenden Parameter hinzu:
       1. ```max_concurrent_iterations``` basiert auf der Anzahl der Workerknoten in Ihrem Cluster.
        2. ```spark_context=sc``` basiert auf dem standardmäßigen Spark-Kontext.
   + Wenn Sie alternativ dazu eine alte SDK-Version nutzen, deaktivieren Sie diese in den installierten Bibliotheken des Clusters, und verschieben Sie sie in den Papierkorb. Installieren Sie die neue SDK-Version, und starten Sie den Cluster neu. Wenn nach dem Neustart ein Problem vorliegt, trennen Sie Ihren Cluster, und fügen Sie ihn wieder an.

Wenn die Installation erfolgreich war, sollte die importierte Bibliothek wie auf einem der folgenden Screenshots gezeigt aussehen:

#### <a name="sdk-for-databricks"></a>SDK für Databricks
![Azure Machine Learning SDK für Databricks](./media/how-to-configure-environment/amlsdk-withoutautoml.jpg)

#### <a name="sdk-for-databricks-with-automated-machine-learning"></a>SDK für Databricks mit automatisiertem maschinellen Lernen
Wenn der Cluster mit einer Nicht-ML-Runtime 7.1 oder höher für Databricks erstellt wurde, führen Sie den folgenden Befehl in der ersten Zelle Ihres Notebooks aus, um das AML SDK zu installieren.

```
%pip install --upgrade --force-reinstall -r https://aka.ms/automl_linux_requirements.txt
```
Für eine Nicht-ML-Runtime 7.0 oder niedriger für Databricks installieren Sie das AML SDK mithilfe des [init-Skripts](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/azure-databricks/automl/README.md).


### <a name="start-exploring"></a>Ausprobieren

So können Sie Azure Databricks testen:
+ Von den vielen verfügbaren Beispielnotebooks können **nur [ganz bestimmte](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/azure-databricks) mit Azure Databricks verwendet werden.**

+ Importieren Sie diese Beispiele direkt aus Ihrem Arbeitsbereich. Siehe unten: ![„Importieren“ auswählen](./media/how-to-configure-environment/azure-db-screenshot.png)
![Bereich für Importieren](./media/how-to-configure-environment/azure-db-import.png)

+ Erfahren Sie, wie Sie [mit Databricks als Computeziel für das Trainieren von Modellen eine Pipeline erstellen](how-to-create-your-first-pipeline.md).

## <a name="next-steps"></a>Nächste Schritte

- [Trainieren eines Modells](tutorial-train-models-with-aml.md) in Azure Machine Learning mit der MNIST-Datenbank
- Anzeigen der Referenz [Azure Machine Learning SDK für Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py&preserve-view=true)
