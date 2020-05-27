---
title: Ausführen von Jupyter Notebooks in Ihrem Arbeitsbereich
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie im Azure Machine Learning Studio ein Jupyter Notebook ausführen können, ohne Ihren Arbeitsbereich zu verlassen.
services: machine-learning
author: abeomor
ms.author: osomorog
ms.reviewer: sgilley
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 04/21/2020
ms.openlocfilehash: 96e37afd8bf7d59eef4a4c0c831f535faa36d34d
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2020
ms.locfileid: "83681444"
---
# <a name="how-to-run-jupyter-notebooks-in-your-workspace"></a>Ausführen von Jupyter Notebooks in Ihrem Arbeitsbereich
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Erfahren Sie, wie Sie im Azure Machine Learning Studio Ihr Jupyter Notebook direkt in Ihrem Arbeitsbereich ausführen können. Sie können zwar [Jupyter](https://jupyter.org/) oder [JupyterLab](https://jupyterlab.readthedocs.io) starten, aber Sie können Ihre Notebooks auch bearbeiten und ausführen, ohne den Arbeitsbereich zu verlassen.

Erhalten Sie Informationen zu folgenden Themen:

* Erstellen von Jupyter Notebooks in Ihrem Arbeitsbereich
* Ausführen eines Experiments über ein Notebook
* Ändern der Notebookumgebung
* Suchen von Details zu den Computeinstanzen, die zum Ausführen Ihrer Notebooks verwendet werden

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://aka.ms/AMLFree) erstellen, bevor Sie beginnen.
* Ein Machine Learning-Arbeitsbereich. Weitere Informationen finden Sie unter [Erstellen eines Azure Machine Learning-Arbeitsbereichs](how-to-manage-workspace.md).

## <a name="create-notebooks"></a><a name="create"></a> Erstellen von Notebooks

Erstellen Sie in Ihrem Azure Machine Learning-Arbeitsbereich ein neues Jupyter Notebook, und beginnen Sie mit der Arbeit. Das neu erstellte Notebook wird im standardmäßigen Arbeitsbereichsspeicher gespeichert. Dieses Notebook kann für alle Personen mit Zugriff auf den Arbeitsbereich freigegeben werden. 

So erstellen Sie ein neues Notebook 

1. Öffnen Sie Ihren Arbeitsbereich in [Azure Machine Learning Studio](https://ml.azure.com).
1. Wählen Sie auf der linken Seite **Notebooks** aus. 
1. Wählen Sie das Symbol **Neue Datei erstellen** oberhalb der Liste **Benutzerdateien** im Abschnitt **Meine Dateien** aus.

    :::image type="content" source="media/how-to-run-jupyter-notebooks/create-new-file.png" alt-text="Erstellen einer neuen Datei":::

1. Benennen Sie die Datei. 
1. Für Jupyter Notebook-Dateien wählen Sie **Python Notebook** als Dateityp aus.
1. Wählen Sie ein Dateiverzeichnis aus.
1. Klicken Sie auf **Erstellen**.

> [!TIP]
> Sie können auch Textdateien erstellen.  Wählen Sie **Text** als Dateityp aus, und fügen Sie dem Namen die Erweiterung hinzu (z. B. „myfile.py“ oder „myfile.txt“)  

Sie können auch Ordner und Dateien, einschließlich Notebooks, mit den Tools am oberen Rand der Seite „Notebooks“ hochladen.  Notebooks und die meisten Textdateitypen werden im Vorschaubereich angezeigt.  Für die meisten anderen Dateitypen ist keine Vorschau verfügbar.

### <a name="clone-samples"></a>Klonen von Beispielen

Ihr Arbeitsbereich enthält einen Ordner mit **Beispielen** von Notebooks, die Ihnen helfen sollen, das SDK zu erkunden und als Beispiele für Ihre eigenen Projekte für maschinelles Lernen zu dienen.  Sie können diese Notebooks in Ihren eigenen Ordner im Speichercontainer Ihres Arbeitsbereichs klonen.  

Ein Beispiel finden Sie unter [Tutorial: Erstellen Ihres ersten ML-Experiments](tutorial-1st-experiment-sdk-setup.md#azure).

### <a name="use-files-from-git-and-version-my-files"></a><a name="terminal"></a> Verwenden von Dateien aus Git und Versionsverwaltung für meine Dateien

Sie können auf alle Git-Operationen über ein Terminalfenster zugreifen. Alle Git-Dateien und -Ordner werden in Ihrem Arbeitsbereichsdateisystem gespeichert.

> [!NOTE]
> Fügen Sie Ihre Dateien und Ordner irgendwo unter dem Ordner **~/cloudfiles/code/Users** hinzu, sodass sie in allen Ihren Jupyter-Umgebungen sichtbar sind.

So greifen Sie auf das Terminal zu

1. Öffnen Sie Ihren Arbeitsbereich in [Azure Machine Learning Studio](https://ml.azure.com).
1. Wählen Sie auf der linken Seite **Notebooks** aus.
1. Wählen Sie ein beliebiges Notebook aus, das sich im Abschnitt **Benutzerdateien** auf der linken Seite befindet.  Wenn sich dort keine Notebooks befinden, erstellen Sie zunächst ein [Notebook](#create).
1. Wählen Sie ein **Computeziel** aus, oder erstellen Sie ein neues Ziel und warten Sie, bis es ausgeführt wird.
1. Wählen Sie das Symbol **Terminal öffnen** aus.

    :::image type="content" source="media/how-to-run-jupyter-notebooks/open-terminal.png" alt-text="Öffnen Sie ein Terminal.":::

1. Wenn Sie das Symbol nicht sehen, wählen Sie **...** rechts neben dem Computeziel und dann **Terminal öffnen** aus.

    :::image type="content" source="media/how-to-run-jupyter-notebooks/alt-open-terminal.png" alt-text="Terminal öffnen über...":::


Erfahren Sie mehr über das [Klonen von Git-Repositorys in Ihrem Arbeitsbereichsdateisystem](concept-train-model-git-integration.md#clone-git-repositories-into-your-workspace-file-system).


### <a name="share-notebooks-and-other-files"></a>Freigeben von Notebooks und anderen Dateien

Kopieren Sie die URL und fügen Sie sie ein, um ein Notebook oder eine Datei freizugeben.  Nur andere Benutzer des Arbeitsbereichs können auf diese URL zugreifen.  Erfahren Sie mehr über das [Erteilen des Zugriffs auf Ihren Arbeitsbereich](how-to-assign-roles.md).

## <a name="edit-a-notebook"></a>Bearbeiten eines Notebooks

Öffnen Sie zum Bearbeiten eines Notebooks ein beliebiges Notebook, das sich im Abschnitt **Benutzerdateien** Ihres Arbeitsbereichs befindet. Klicken Sie auf die Zelle, die Sie bearbeiten möchten. 

Wenn eine Computeinstanz ausgeführt wird, können Sie auch die Codevervollständigung mit [Intellisense](https://code.visualstudio.com/docs/editor/intellisense) in jedem Python-Notebook verwenden.

Sie können Jupyter oder JupyterLab auch über die Notebook-Symbolleiste starten.  Azure Machine Learning stellt keine Updates und Fehlerbehebungen von Jupyter oder JupyterLab zur Verfügung, da es sich um Open Source-Produkte außerhalb der Zuständigkeit des Microsoft-Supports handelt.

### <a name="useful-keyboard-shortcuts"></a>Hilfreiche Tastenkombinationen

|Tastatur  |Aktion  |
|---------|---------|
|UMSCHALT+EINGABE     |  Ausführen einer Zelle       |
|STRG+M (Fenster)     |  Aktivieren/Deaktivieren des Abfangens von Registerkarten im Notebook       |
|STRG+UMSCHALT+M (Mac und Linux)     |    Aktivieren/Deaktivieren des Abfangens von Registerkarten im Notebook     |
|TAB (bei aktiviertem Abfangen von Registerkarten) | Hinzufügen eines „\t“-Zeichens (Einzug)
|TAB (bei deaktiviertem Abfangen von Registerkarten) | Ändern des Fokus auf das nächste Element, das den Fokus erhalten kann (Schaltfläche „Zelle löschen“, Schaltfläche „Ausführen“ usw.)

## <a name="delete-a-notebook"></a>Löschen eines Notebooks

Sie können die **Beispiel**-Notebooks *nicht* löschen.  Diese Notebooks sind Teil des Studios und werden jedes Mal aktualisiert, wenn ein neues SDK veröffentlicht wird.  

Sie *können* **Benutzerdateien**-Notebooks auf eine der folgenden Arten löschen:

* Wählen Sie im Studio die **...** (Auslassungspunkte) am Ende eines Ordners oder einer Datei aus.  Stellen Sie sicher, dass Sie einen unterstützten Browser (Microsoft Edge, Chrome oder Firefox) verwenden.
* Wählen Sie auf einer beliebigen Notebook-Symbolleiste [**Terminal öffnen**](#terminal) aus, um auf das Terminalfenster für die Computeinstanz zuzugreifen.
* Entweder in Jupyter oder JupyterLab mit den zugehörigen Tools.

## <a name="run-an-experiment"></a>Ausführen eines Experiments

Um ein Experiment von einem Notebook aus durchzuführen, stellen Sie zunächst eine Verbindung mit einer aktiven [Computeinstanz](concept-compute-instance.md) her. Wenn Sie keine Computeinstanz besitzen, verwenden Sie die folgenden Schritte, um eine zu erstellen: 

1. Wählen Sie **+** auf der Notebook-Symbolleiste aus. 
2. Benennen Sie die Computeinstanz, und wählen Sie eine **VM-Größe** aus. 
3. Klicken Sie auf **Erstellen**.
4. Die Computeinstanz wird automatisch mit dem Notebook verbunden, und Sie können jetzt Ihre Zellen ausführen.

Nur Sie können die von Ihnen erstellten Computeinstanzen sehen und verwenden.  Ihre **Benutzerdateien** werden getrennt vom virtuellen Computer gespeichert und von allen Computeinstanzen im Arbeitsbereich gemeinsam genutzt.

### <a name="view-logs-and-output"></a>Anzeigen von Protokollen und Ausgaben

Verwenden Sie [Notebook-Widgets](https://docs.microsoft.com/python/api/azureml-widgets/azureml.widgets?view=azure-ml-py), um den Fortschritt der Ausführung und der Protokolle anzuzeigen. Ein Widget ist asynchron und bietet Updates, bis das Training abgeschlossen ist. Azure Machine Learning-Widgets werden auch in Jupyter und JupterLab unterstützt.

## <a name="change-the-notebook-environment"></a>Ändern der Notebookumgebung

Die Notebook-Symbolleiste ermöglicht es Ihnen, die Umgebung zu ändern, in der das Notebook ausgeführt wird.  

Diese Aktionen ändern weder den Notebook-Zustand noch die Werte von Variablen im Notebook:

|Aktion  |Ergebnis  |
|---------|---------| --------|
|Beenden des Kernels     |  Beendet jede ausgeführte Zelle. Durch das Ausführen einer Zelle wird der Kernel automatisch neu gestartet. |
|Navigieren zu einem anderen Arbeitsbereichsabschnitt     |     Das Ausführen von Zellen wird beendet. |

Mit diesen Aktionen werden der Notebook-Zustand und alle Variablen im Notebook zurückgesetzt.

|Aktion  |Ergebnis  |
|---------|---------| --------|
| Ändern des Kernels | Das Notebook verwendet neuen Kernel. |
| Wechseln der Computeinstanz    |     Das Notebook verwendet automatisch die neue Computeinstanz. |
| Zurücksetzen der Computeinstanz | Wird erneut gestartet, wenn Sie versuchen, eine Zelle auszuführen. |
| Beenden der Computeinstanz     |    Es werden keine Zellen ausgeführt.  |
| Öffnen eines Notebooks in Jupyter oder JupyterLab     |    Das Notebook wurde auf einer neuen Registerkarte geöffnet.  |

### <a name="add-new-kernels"></a>Hinzufügen neuer Kernel

Das Notebook findet automatisch alle Jupyter-Kernel, die auf der verbundenen ComputeiInstanz installiert sind.  So fügen Sie einen Kernel zur Computeinstanz hinzu

1. Wählen Sie auf der Notebook-Symbolleiste [**Terminal öffnen**](#terminal) aus.
1. Verwenden Sie das Terminalfenster, um eine neue Umgebung zu erstellen.
1. Aktivieren Sie die Umgebung.  Beispielsweise nach dem Erstellen von `newenv`:

    ```shell
    source activate newenv
    python -m ipykernel install --user --name newenv --display-name "Python (newenv)"
    ```

Jeder der [verfügbaren Jupyter-Kernel](https://github.com/jupyter/jupyter/wiki/Jupyter-kernels) kann installiert werden.

### <a name="status-indicators"></a>Statusanzeigen

Ein Indikator neben der **Compute**-Dropdownliste zeigt seinen Status an.  Der Status wird auch in der Dropdownliste selbst angezeigt.  

|Color |Computestatus |
|---------|---------| 
| Grün | Compute wird ausgeführt |
| Red |Fehler bei Compute | 
| Schwarz | Compute beendet |
|  Hellblau |Erstellen, Starten, Neustarten und Einrichten von Compute |
|  Grau |Compute wird gelöscht, beendet |

Ein Indikator neben der **Kernel**-Dropdownliste zeigt seinen Status an.

|Color |Kernelstatus |
|---------|---------|
|  Grün |Kernel verbunden, im Leerlauf, ausgelastet|
|  Grau |Kernel nicht verbunden |

## <a name="find-compute-details"></a>Suchen von Computedetails 

Hier finden Sie Details zu ihren Computeinstanzen auf der **Compute**-Seite in [Studio](https://ml.azure.com).

## <a name="next-steps"></a>Nächste Schritte

* [Ausführen Ihres ersten Experiments](tutorial-1st-experiment-sdk-train.md)
* [Sichern des Dateispeichers mit Momentaufnahmen](../storage/files/storage-snapshots-files.md)
