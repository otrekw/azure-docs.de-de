---
title: Ausführen von Jupyter Notebooks in Ihrem Arbeitsbereich
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie in Azure Machine Learning Studio ein Jupyter Notebook ausführen können, ohne Ihren Arbeitsbereich zu verlassen.
services: machine-learning
author: abeomor
ms.author: osomorog
ms.reviewer: sgilley
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.custom: how-to
ms.date: 01/19/2021
ms.openlocfilehash: 18ccadcf43d41c677a665ed068d093f51389b576
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101657331"
---
# <a name="run-jupyter-notebooks-in-your-workspace"></a>Ausführen von Jupyter Notebooks in Ihrem Arbeitsbereich

Erfahren Sie, wie Sie in Azure Machine Learning Studio Ihr Jupyter Notebook direkt in Ihrem Arbeitsbereich ausführen können. Sie können zwar [Jupyter](https://jupyter.org/) oder [JupyterLab](https://jupyterlab.readthedocs.io) starten, aber Sie können Ihre Notebooks auch bearbeiten und ausführen, ohne den Arbeitsbereich zu verlassen.

Informationen zum Erstellen und Verwalten von Dateien, einschließlich Notebooks, finden Sie unter [Erstellen und Verwalten von Dateien in Ihrem Arbeitsbereich](how-to-manage-files.md).

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://aka.ms/AMLFree) erstellen, bevor Sie beginnen.
* Ein Machine Learning-Arbeitsbereich. Weitere Informationen finden Sie unter [Erstellen eines Azure Machine Learning-Arbeitsbereichs](how-to-manage-workspace.md).

## <a name="edit-a-notebook"></a>Bearbeiten eines Notebooks

Öffnen Sie zum Bearbeiten eines Notebooks ein beliebiges Notebook, das sich im Abschnitt **Benutzerdateien** Ihres Arbeitsbereichs befindet. Klicken Sie auf die Zelle, die Sie bearbeiten möchten.  Wenn Sie in diesem Abschnitt über keine Notebooks verfügen, finden Sie weitere Informationen unter [Erstellen und Verwalten von Dateien in Ihrem Arbeitsbereich](how-to-manage-files.md).

Sie können das Notebook bearbeiten, ohne eine Verbindung zu einer Computeinstanz herzustellen.  Wenn Sie die Zellen im Notebook ausführen möchten, wählen oder erstellen Sie eine Computeinstanz.  Wenn Sie eine beendete Computeinstanz auswählen, wird sie automatisch gestartet, wenn Sie die erste Zelle ausführen.

Wenn eine Compute-Instanz ausgeführt wird, können Sie auch die Codevervollständigung mit [IntelliSense](https://code.visualstudio.com/docs/editor/intellisense) in jedem Python-Notebook verwenden.

Sie können Jupyter oder JupyterLab auch über die Notebook-Symbolleiste starten.  Azure Machine Learning stellt keine Updates und Fehlerbehebungen von Jupyter oder JupyterLab zur Verfügung, da es sich um Open Source-Produkte außerhalb der Zuständigkeit des Microsoft-Supports handelt.

## <a name="focus-mode"></a>Fokusmodus

Verwenden Sie den Fokusmodus, um Ihre aktuelle Ansicht zu erweitern, sodass Sie sich auf Ihre aktiven Registerkarten konzentrieren können. Der Fokusmodus blendet den Datei-Explorer für Notebooks aus.

1. Wählen Sie auf der Symbolleiste des Terminalfensters **Fokusmodus** aus, um den Fokusmodus zu aktivieren. Abhängig von Ihrer Fensterbreite befindet sich diese möglicherweise unter dem Menüpunkt **...** auf Ihrer Symbolleiste.
1. Kehren Sie im Fokusmodus zur Standardansicht zurück, indem Sie **Standardansicht** auswählen.

    :::image type="content" source="media/how-to-run-jupyter-notebooks/focusmode.gif" alt-text="Wechsel zwischen Fokusmodus/Standardansicht":::

## <a name="use-intellisense"></a>Verwenden von IntelliSense

[IntelliSense](https://code.visualstudio.com/docs/editor/intellisense) ist eine hilfreiche Anwendung zur Codevervollständigung mit Features wie: Auflisten von Elementen, Parameterinformationen, QuickInfo und Wort vervollständigen. Diese Features helfen Ihnen, mehr über den von Ihnen verwendeten Code zu erfahren, die von Ihnen eingegebenen Parameter zu verfolgen und Aufrufe von Eigenschaften und Methoden mit nur wenigen Tastatureingaben hinzuzufügen.  

Wenn Sie Code eingeben, verwenden Sie STRG+LEERTASTE, um IntelliSense auszulösen.

## <a name="clean-your-notebook-preview"></a>Bereinigen des Notebooks (Vorschau)

> [!IMPORTANT]
> Die Funktion „Sammeln“ ist zurzeit als öffentliche Vorschauversion verfügbar.
> Die Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Im Laufe der Erstellung eines Notebooks verfügen Sie in der Regel über Zellen, die Sie zum Durchsuchen von Daten oder zum Debuggen verwendet haben. Mithilfe der Funktion *Sammeln* können Sie ein sauberes Notebook ohne diese irrelevanten Zellen erstellen.

1. Führen Sie alle Notebookzellen aus.
1. Wählen Sie die Zelle aus mit dem Code aus, den das neue Notebook ausführen soll, beispielsweise Code zum Übermitteln eines Experiments oder vielleicht Code zum Registrieren eines Modells.
1. Wählen Sie das Symbol **Sammeln** aus, das auf der Symbolleiste für Zellen angezeigt wird.
    :::image type="content" source="media/how-to-run-jupyter-notebooks/gather.png" alt-text="Screenshot: Auswählen des Symbols „Sammeln“":::
1. Geben Sie den Namen für das neue „gesammelte“ Notebook ein.  

Das neue Notebook enthält nur Codezellen. Dabei müssen alle Zellen die gleichen Ergebnisse wie die Zelle generieren, die Sie für die Sammlung ausgewählt haben.

## <a name="save-and-checkpoint-a-notebook"></a>Speichern eines Notebooks und Hinzufügen eines Prüfpunkts

Azure Machine Learning erstellt eine Prüfpunktdatei, wenn Sie eine *IPYNB*-Datei erstellen.

Wählen Sie in der Notebook-Symbolleiste das Menü und dann **Datei&gt;Save and Checkpoint** (Speichern und Prüfpunkt), um das Notebook manuell zu speichern. Dadurch wird eine dem Notebook zugeordnete Prüfpunktdatei hinzugefügt.

:::image type="content" source="media/how-to-run-jupyter-notebooks/file-save.png" alt-text="Screenshot des Speichertools auf der Notebook-Symbolleiste":::

Jedes Notebook wird alle 30 Sekunden automatisch gespeichert. Die automatische Speicherung aktualisiert nur die ursprüngliche *IPYNB*-Datei, nicht die Prüfpunktdatei.
 
Wählen Sie **Checkpoints** (Prüfpunkte) im Notebook-Menü aus, um einen benannten Prüfpunkt zu erstellen und das Notebook auf einen gespeicherten Prüfpunkt zurückzusetzen.

## <a name="export-a-notebook"></a>Exportieren eines Notebooks

Wählen Sie in der Symbolleiste des Notebooks das Menü und dann **Exportieren als** aus, um das Notebook als einen der unterstützten Typen zu exportieren:

* Notebook
* Python
* HTML
* LaTeX

:::image type="content" source="media/how-to-run-jupyter-notebooks/export-notebook.png" alt-text="Exportieren eines Notebooks auf Ihren Computer":::

Die exportierte Datei wird auf dem Computer gespeichert.

## <a name="run-a-notebook-or-python-script"></a>Ausführen eines Notebooks oder Python-Skripts

Um ein Notebook oder ein Python-Skript auszuführen, stellen Sie zunächst eine Verbindung mit einer aktiven [Compute-Instanz](concept-compute-instance.md) her.

* Wenn Sie keine Computeinstanz besitzen, verwenden Sie die folgenden Schritte, um eine zu erstellen:

    1. Wählen Sie rechts neben der Dropdownliste „Compute“ auf der Symbolleiste des Notebooks oder Skripts die Option **+ New Compute** (Neue Computeressource) aus. Je nach Bildschirmgröße befindet sich dies möglicherweise unter einem **...** -Menü.
        :::image type="content" source="media/how-to-run-jupyter-notebooks/new-compute.png" alt-text="Erstellen einer neuen Computeressource":::
    1. Benennen Sie die Computeinstanz, und wählen Sie eine **VM-Größe** aus. 
    1. Klicken Sie auf **Erstellen**.
    1. Die Compute-Instanz wird automatisch mit der Datei verknüpft.  Sie können jetzt die Notebookzellen oder das Python-Skript mit dem Tool links neben der Compute-Instanz ausführen.

* Wenn Sie über eine angehaltene Compute-Instanz verfügen, wählen Sie rechts neben der Dropdownliste „Compute“ die Option **Start compute** (Compute starten) aus. Je nach Bildschirmgröße befindet sich dies möglicherweise unter einem **...** -Menü.

    :::image type="content" source="media/how-to-run-jupyter-notebooks/start-compute.png" alt-text="Starten einer Compute-Instanz":::

Nur Sie können die von Ihnen erstellten Computeinstanzen sehen und verwenden.  Ihre **Benutzerdateien** werden getrennt vom virtuellen Computer gespeichert und von allen Computeinstanzen im Arbeitsbereich gemeinsam genutzt.

### <a name="view-logs-and-output"></a>Anzeigen von Protokollen und Ausgaben

Verwenden Sie [Notebook-Widgets](/python/api/azureml-widgets/azureml.widgets?preserve-view=true&view=azure-ml-py), um den Fortschritt der Ausführung und die Protokolle anzuzeigen. Ein Widget ist asynchron und bietet Updates, bis das Training abgeschlossen ist. Azure Machine Learning-Widgets werden auch in Jupyter und JupterLab unterstützt.

:::image type="content" source="media/how-to-run-jupyter-notebooks/jupyter-widget.png" alt-text="Screenshot: Jupyter Notebook-Widget ":::

## <a name="explore-variables-in-the-notebook"></a>Untersuchen von Variablen im Notebook

Verwenden Sie auf der Notebook-Symbolleiste das Tool **Variablen-Explorer**, um den Namen, den Typ, die Länge und Stichprobenwerte aller Variablen anzuzeigen, die in Ihrem Notebook erstellt wurden.

:::image type="content" source="media/how-to-run-jupyter-notebooks/variable-explorer.png" alt-text="Screenshot: Variablen-Explorer-Tool":::

Wählen Sie das Tool aus, um das Fenster des Variablen-Explorers anzuzeigen.

:::image type="content" source="media/how-to-run-jupyter-notebooks/variable-explorer-window.png" alt-text="Screenshot: Fenster des Variablen-Explorers":::

## <a name="navigate-with-a-toc"></a>Navigieren mit einem Inhaltsverzeichnis

Verwenden Sie auf der Notebook-Symbolleiste das Tool **Inhaltsverzeichnis**, um das Inhaltsverzeichnis anzuzeigen oder auszublenden.  Beginnen Sie mit einer Markdown-Zelle mit einer Überschrift, um sie dem Inhaltsverzeichnis hinzuzufügen. Klicken Sie auf einen Eintrag in der Tabelle, um zu dieser Zelle im Notebook zu scrollen.  

:::image type="content" source="media/how-to-run-jupyter-notebooks/table-of-contents.png" alt-text="Screenshot: Inhaltsverzeichnis im Notebook":::

## <a name="change-the-notebook-environment"></a>Ändern der Notebookumgebung

Die Notebook-Symbolleiste ermöglicht Ihnen, die Umgebung zu ändern, in der das Notebook ausgeführt wird.  

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

## <a name="add-new-kernels"></a>Hinzufügen neuer Kernel

[Verwenden Sie das Terminal ](how-to-access-terminal.md#add-new-kernels), um neue Kernel zu erstellen und sie Ihrer Compute-Instanz hinzuzufügen. Das Notebook findet automatisch alle Jupyter-Kernel, die auf der verbundenen Compute-Instanz installiert sind.

Verwenden Sie die Kernel-Dropdownliste auf der rechten Seite, um zu einem der installierten Kernel zu wechseln.  


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

## <a name="troubleshooting"></a>Problembehandlung

* Wenn Sie keine Verbindung mit einem Notebook herstellen können, stellen Sie sicher, dass die WebSocket-Kommunikation **nicht** deaktiviert ist. Damit die Jupyter-Funktionen für Compute-Instanzen eingesetzt werden können, muss die WebSocket-Kommunikation aktiviert sein. Stellen Sie sicher, dass Ihr Netzwerk WebSocket-Verbindungen mit *.instances.azureml.net und *.instances.azureml.ms zulässt. 

* Wenn eine Compute-Instanz in einem Private Link-Arbeitsbereich bereitgestellt wird, kann nur im virtuellen Netzwerk darauf zugegriffen werden. Wenn Sie benutzerdefiniertes DNS oder HOSTS-Dateien verwenden, fügen Sie einen Eintrag für „<Instanzname>.<region>.instances.azureml.ms“ mit der privaten IP-Adresse des privaten Endpunkts des Arbeitsbereichs hinzu. Weitere Informationen finden Sie im Artikel [Benutzerdefiniertes DNS](./how-to-custom-dns.md?tabs=azure-cli).
    
## <a name="next-steps"></a>Nächste Schritte

* [Ausführen Ihres ersten Experiments](tutorial-1st-experiment-sdk-train.md)
* [Sichern des Dateispeichers mit Momentaufnahmen](../storage/files/storage-snapshots-files.md)
* [Arbeiten in sicheren Umgebungen](./how-to-secure-training-vnet.md#compute-instance)