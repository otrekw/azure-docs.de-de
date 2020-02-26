---
title: Was ist eine Azure Machine Learning-Compute-Instanz?
titleSuffix: Azure Machine Learning
description: Erfahren Sie mehr über die Azure Machine Learning-Compute-Instanz, eine vollständig verwaltete cloudbasierte Arbeitsstation.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sgilley
author: sdgilley
ms.date: 12/13/2019
ms.openlocfilehash: 280851b2fea0b8100a7d0f8ec8105109a41c8c83
ms.sourcegitcommit: 79cbd20a86cd6f516acc3912d973aef7bf8c66e4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2020
ms.locfileid: "77252075"
---
# <a name="what-is-an-azure-machine-learning-compute-instance"></a>Was ist eine Azure Machine Learning-Compute-Instanz?

Eine Azure Machine Learning-Compute-Instanz (Vorschau) ist eine vollständig verwaltete cloudbasierte Arbeitsstation für Datenanalysten. 

Compute-Instanzen vereinfachen den Einstieg in die Azure Machine Learning-Entwicklung und verhelfen IT-Administratoren zu Verwaltungs- und Unternehmensbereitschaftsfunktionen.  

Nutzen Sie eine Compute-Instanz als Ihre vollständig konfigurierte und verwaltete Entwicklungsumgebung in der Cloud.

Compute-Instanzen werden in der Regel als Entwicklungsumgebungen genutzt.  Sie können auch als Computeziel für Entwicklungs- und Testzwecke für Training und Rückschluss verwendet werden.  Bei umfangreichen Aufgaben ist ein [Azure Machine Learning-Computecluster](how-to-set-up-training-targets.md#amlcompute) mit Skalierungsmöglichkeiten dank mehrerer Knoten die bessere Wahl eines Computeziels.


## <a name="why-use-a-compute-instance"></a>Gründe für eine Compute-Instanz

Eine Compute-Instanz ist eine vollständig verwaltete cloudbasierte Arbeitsstation, die für Ihre Machine Learning-Entwicklungsumgebung optimiert ist. Ihnen bieten sich folgende Vorteile:

|Hauptvorteile||
|----|----|
|Produktivität|Data Scientists können mithilfe integrierter Notebooks und der folgenden Tools Modelle im Webbrowser erstellen und implementieren:<br/>-  Jupyter<br/>-  JupyterLab<br/>-  RStudio|
|Verwaltet und sicher|Verringern Sie Ihren Sicherheitsaufwand, und gewinnen Sie Compliance mit Anforderungen der Unternehmenssicherheit. Compute-Instanzen bieten verlässliche Verwaltungsrichtlinien und sichere Netzwerkkonfigurationen wie die folgenden:<br/><br/>- Automatisierte Bereitstellung über Resource Manager-Vorlagen oder das Azure Machine Learning SDK<br/>- [Rollenbasierte Zugriffssteuerung (RBAC)](/azure/role-based-access-control/overview)<br/>- [Unterstützung virtueller Netzwerke](how-to-enable-virtual-network.md#compute-instance)<br/>- SSH-Richtlinie zum Aktivieren/Deaktivieren des SSH-Zugriffs|
|Vorkonfiguriert&nbsp;oder&nbsp;ML|Sparen Sie Zeit bei der Einrichtung von Aufgaben mit vorkonfigurierten und aktuellen ML-Paketen, Deep Learning- Frameworks und GPU-Treibern.|
|Vollständig anpassbar|Umfassende Unterstützung für Azure-VM-Typen einschließlich GPUs und durchweg einfache Anpassungen wie die Installation von Paketen und Treibern machen erweiterte Szenarien zu einem Kinderspiel. |

## <a name="contents"></a>Tools und Umgebungen

Eine Azure Machine Learning-Compute-Instanz ermöglicht Ihnen das Erstellen, Trainieren und Bereitstellen von Modellen in einer vollständig integrierten Notebookumgebung in Ihrem Arbeitsbereich.


Diese Tools und Umgebungen sind in der Compute-Instanz installiert: 

|Allgemeine Tools und Umgebungen|Details|
|----|:----:|
|Treiber|`CUDA`</br>`cuDNN`</br>`NVIDIA`</br>`Blob FUSE` |
|Intel MPI Library||
|Azure-Befehlszeilenschnittstelle ||
|Azure Machine Learning-Beispiele ||
|Azure Machine Learning EDAT-Engine ||
|Docker||
|nginx||
|NCCL 2.0 ||
|Protobuf|| 

|**R**-Tools und -Umgebungen|Details|
|----|:----:|
|RStudio Server (Open Source Edition)||
|R-Kernel||
|Azure Machine Learning SDK für R|[azuremlsdk](https://azure.github.io/azureml-sdk-for-r/reference/index.html)</br>SDK-Beispiele|

|**PYTHON**-Tools und Umgebungen|Details|
|----|----|
|Anaconda Python||
|Jupyter und Erweiterungen||
|Jupyterlab und Erweiterungen||
|Visual Studio Code ||
[Azure Machine Learning SDK for Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)</br>aus PyPI|`azureml-sdk[notebooks,contrib,automl,explain]`</br>`azureml-contrib-datadrift`</br>`azureml-telemetry`</br>`azureml-tensorboard`</br>`azureml-contrib-opendatasets`</br>`azureml-opendatasets`</br>`azureml-contrib-reinforcementlearning`</br>`azureml-mlflow`</br>`azureml-contrib-interpret` |
|Andere PyPi-Pakete|`jupytext`</br>`jupyterlab-git`</br>`tensorboard`</br>`nbconvert`</br>`notebook`</br>`Pillow`|
|Conda-Pakete|`cython`</br>`numpy`</br>`ipykernel`</br>`scikit-learn`</br>`matplotlib`</br>`tqdm`</br>`joblib`</br>`nodejs`</br>`nb_conda_kernels`|
|Deep Learning-Pakete|`PyTorch`</br>`TensorFlow`</br>`Keras`</br>`Horovod`</br>`MLFlow`</br>`pandas-ml`</br>`scrapbook`|
|ONNX-Pakete|`keras2onnx`</br>`onnx`</br>`onnxconverter-common`</br>`skl2onnx`</br>`onnxmltools`|
|Python- und R-SDK-Beispiele für Azure Machine Learning||

Python-Pakete sind alle in der **Python 3.6 – AzureML**-Umgebung installiert.  

Compute-Instanzen werden in der Regel als Entwicklungsumgebungen genutzt.  Sie können auch als Computeziel für Entwicklungs- und Testzwecke für Training und Rückschluss verwendet werden.  Bei umfangreichen Aufgaben ist ein [Azure Machine Learning-Computecluster](how-to-set-up-training-targets.md#amlcompute) mit Skalierungsmöglichkeiten dank mehrerer Knoten die bessere Wahl eines Computeziels.

### <a name="installing-packages"></a>Installieren von Paketen

Sie können Pakete direkt in einem Jupyter-Notebook oder in RStudio installieren:

* Verwenden Sie die Registerkarte **Pakete** unten rechts oder die Registerkarte **Konsole** oben links.  
* Python: Fügen Sie Installationscode hinzu, und führen Sie ihn in einer Zelle im Jupyter-Notebook aus.

Sie können auch auf eine der folgenden Arten auf ein Terminalfenster zugreifen:

* RStudio: Wählen Sie die Registerkarte **Terminal** oben links aus.
* Jupyter Lab:  Wählen Sie die Kachel **Terminal** unter der Überschrift **Other** (Andere) auf der Registerkarte „Launcher“ aus.
* Jupyter:  Wählen Sie **Neu > Terminal** oben rechts auf der Registerkarte „Dateien“ aus.
* SSH-Verbindung mit dem Computer.  Installieren Sie dann Python-Pakete in der **Python 3.6 – AzureML**-Umgebung.  Installieren Sie R-Pakete in der **R**-Umgebung.

## <a name="accessing-files"></a>Zugreifen auf Dateien

Notebooks und R-Skripts werden im Standardspeicherkonto Ihres Arbeitsbereichs in der Azure-Dateifreigabe gespeichert.  Diese Dateien befinden sich im Verzeichnis „Benutzerdateien“. Dieser Speicher erleichtert die gemeinsame Nutzung von Notebooks zwischen Compute-Instanzen. Das Speicherkonto bewahrt auch Ihre Notebooks sicher auf, wenn Sie eine Compute-Instanz beenden oder löschen.

Das Azure-Dateifreigabekonto Ihres Arbeitsbereichs wird als Laufwerk in die Compute-Instanz eingebunden. Dieses Laufwerk ist das Standardarbeitsverzeichnis für Jupyter, Jupyter Labs und RStudio.

Auf die Dateien in der Dateifreigabe kann von allen Compute-Instanzen im gleichen Arbeitsbereich zugegriffen werden. Änderungen an diesen Dateien auf der Compute-Instanz werden zuverlässig dauerhaft in der Dateifreigabe zurückgespeichert.

Sie können auch die neuesten Azure Machine Learning-Beispiele in Ihren Ordner im Verzeichnis mit den Benutzerdateien in der Dateifreigabe des Arbeitsbereichs klonen.

Das Schreiben kleiner Dateien kann auf Netzlaufwerken langsamer sein als das Schreiben auf der VM selbst.  Wenn Sie viele kleine Dateien schreiben, sollten Sie versuchen, ein Verzeichnis direkt auf der Compute-Instanz zu verwenden, z. B. ein `/tmp`-Verzeichnis. Beachten Sie, dass auf diese Dateien von anderen Compute-Instanzen im Arbeitsbereich nicht zugegriffen werden kann.

## <a name="managing-a-compute-instance"></a>Verwalten einer Compute-Instanz

Wählen Sie Ihrem Arbeitsbereich in Azure Machine Learning Studio **Compute** aus, und wählen sie dann oben **Compute-Instanz** aus.

![Verwalten einer Compute-Instanz](./media/concept-compute-instance/manage-compute-instance.png)

Sie können folgende Aktionen ausführen:

* Erstellen einer Compute-Instanz. Geben Sie den Namen und den Azure-VM-Typ einschließlich GPUs an (dieser kann nach der Erstellung nicht mehr geändert werden), aktivieren bzw. deaktivieren Sie den SSH-Zugriff, und konfigurieren Sie optional Einstellungen für virtuelle Netzwerke. Sie können eine Instanz auch direkt anhand integrierter Notebooks, des Azure-Portals, einer Resource Manager-Vorlage oder des Azure Machine Learning SDK erstellen. Das Kontingent dedizierter Kerne pro Region, das für die Erstellung von Compute-Instanzen gilt, ist einheitlich und wird mit dem Kontingent für Azure Machine Learning-Computecluster gemeinsam genutzt.
* Aktualisieren der Registerkarte „Compute-Instanzen“
* Starten, Beenden und erneutes Starten einer Compute-Instanz
* Löschen einer Compute-Instanz

Für jede Compute-Instanz in Ihrem Arbeitsbereich können Sie folgende Aktionen ausführen:

* Zugreifen auf Jupyter, JupyterLab, RStudio auf der Computeinstanz
* SSH-Verbindung mit Compute-Instanz. Der SSH-Zugriff ist standardmäßig deaktiviert, kann aber zum Zeitpunkt der Erstellung der Compute-Instanz aktiviert werden. Der SSH-Zugriff erfolgt über einen Mechanismus mit öffentlichem/privatem Schlüssel. Auf der Registerkarte finden Sie Details zur SSH-Verbindung wie IP-Adresse, Benutzername und Portnummer.
* Informieren Sie sich über Details einer bestimmten Compute-Instanz wie IP-Adresse und Region.

Mithilfe von [RBAC](/azure/role-based-access-control/overview) können Sie steuern, welche Benutzer im Arbeitsbereich eine Compute-Instanz erstellen, löschen, starten, beenden und neu starten können. Alle Benutzer mit der Rolle „Mitwirkender“ und „Besitzer“ des Arbeitsbereichs können Compute-Instanzen im gesamten Arbeitsbereich erstellen, löschen, starten, beenden und neu starten. Allerdings darf nur der Ersteller einer bestimmten Compute-Instanz auf dieser Compute-Instanz auf Jupyter, JupyterLab und RStudio zugreifen. Die Compute-Instanz ist für ihren Ersteller dediziert, er besitzt Rootzugriff und kann Terminalverbindungen über Jupyter herstellen. Die Compute-Instanz weist die Einzelbenutzeranmeldung für den erstellenden Benutzer auf, und alle Aktionen verwenden die Identität dieses Benutzers für RBAC und die Zuordnung von Experimentausführungen. Der SSH-Zugriff wird über einen Mechanismus mit öffentlichem/privatem Schlüssel gesteuert.

Sie können auch eine Instanz
* direkt in der integrierten Notebookumgebung erstellen.
* Im Azure-Portal
* Über eine Azure Resource Manager-Vorlage
* Mit dem Azure Machine Learning SDK

Das Kontingent dedizierter Kerne pro Region, das für die Erstellung von Compute-Instanzen gilt, ist einheitlich und wird mit dem Kontingent für Azure Machine Learning-Trainingscluster geteilt. 

## <a name="compute-target"></a>Computeziel

Compute-Instanzen können als [Trainingscomputeziele](concept-compute-target.md#train) verwendet werden, ähnlich wie Azure Machine Learning-Computetrainingscluster. Stellen Sie eine Multi-GPU-VM bereit, um verteilte Trainingsaufträge mithilfe von TensorFlow-/PyTorch-Schätzfunktionen auszuführen. Sie können auch eine Ausführungskonfiguration erstellen und damit Ihr Experiment auf der Compute-Instanz ausführen. Sie können die Compute-Instanz als gefolgertes lokales Bereitstellungsziel für Test- und Debugszenarien verwenden.

## <a name="notebookvm"></a>Was ist mit der Notebook-VM passiert?

Compute-Instanzen ersetzen die Notebook-VM.  

Alle Notebookdateien, die in der Dateifreigabe des Arbeitsbereichs gespeichert sind, und Daten in den Datenspeichern des Arbeitsbereichs sind auf einer Compute-Instanz zugänglich. Allerdings müssen alle benutzerdefinierten Pakete, die zuvor auf einer Notebook-VM installiert waren, auf der Compute-Instanz neu installiert werden. Kontingentbeschränkungen, die für die Erstellung von Compute-Clustern gelten, gelten ebenso für die Erstellung von Compute-Instanzen. 

Neue Notebook-VMs können nicht erstellt werden. Sie können jedoch weiterhin auf die von Ihnen erstellten Notebook-VMs bei voller Funktionalität zugreifen und diese nutzen. Compute-Instanzen können im gleichen Arbeitsbereich wie die vorhandenen Notebook-VMs erstellt werden. 


## <a name="next-steps"></a>Nächste Schritte

 * [Tutorial: Trainieren Ihres ersten ML-Modells](tutorial-1st-experiment-sdk-train.md) zeigt, wie eine Compute-Instanz mit einem integrierten Notebook verwendet wird.
