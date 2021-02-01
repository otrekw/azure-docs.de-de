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
ms.openlocfilehash: d6832238b0c76059079e2a1330d31eed3212b242
ms.sourcegitcommit: b39cf769ce8e2eb7ea74cfdac6759a17a048b331
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2021
ms.locfileid: "98685577"
---
# <a name="how-to-run-jupyter-notebooks-in-your-workspace"></a>Ausführen von Jupyter Notebooks in Ihrem Arbeitsbereich

Erfahren Sie, wie Sie in Azure Machine Learning Studio Ihr Jupyter Notebook direkt in Ihrem Arbeitsbereich ausführen können. Sie können zwar [Jupyter](https://jupyter.org/) oder [JupyterLab](https://jupyterlab.readthedocs.io) starten, aber Sie können Ihre Notebooks auch bearbeiten und ausführen, ohne den Arbeitsbereich zu verlassen.

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
1. Für Jupyter Notebook-Dateien wählen Sie als Dateityp **Notebook** aus.
1. Wählen Sie ein Dateiverzeichnis aus.
1. Klicken Sie auf **Erstellen**.

Sie können auch Textdateien erstellen.  Wählen Sie **Text** als Dateityp aus, und fügen Sie dem Namen die Erweiterung hinzu (z. B. „myfile.py“ oder „myfile.txt“)  

Sie können auch Ordner und Dateien, einschließlich Notebooks, mit den Tools am oberen Rand der Seite „Notebooks“ hochladen.  Notebooks und die meisten Textdateitypen werden im Vorschaubereich angezeigt.  Für die meisten anderen Dateitypen ist keine Vorschau verfügbar.

> [!IMPORTANT]
> Inhalte in Notebooks und Skripts können möglicherweise Daten aus Ihren Sitzungen lesen und auf Daten zugreifen, ohne dass sich Ihre Organisation in Azure befindet.  Laden Sie nur Dateien aus vertrauenswürdigen Quellen. Weitere Informationen finden Sie unter [Bewährte Methoden für sicheren Code](concept-secure-code-best-practice.md#azure-ml-studio-notebooks).

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
1. Wählen Sie ein **Computeziel** aus, oder erstellen Sie ein neues Ziel, und warten Sie, bis es ausgeführt wird.
1. Wählen Sie das Symbol **Terminal öffnen** aus.

    :::image type="content" source="media/how-to-run-jupyter-notebooks/open-terminal.png" alt-text="Öffnen Sie ein Terminal.":::

1. Wird das Symbol nicht angezeigt, wählen Sie **...** rechts neben dem Computeziel und dann **Terminal öffnen** aus.

    :::image type="content" source="media/how-to-run-jupyter-notebooks/alt-open-terminal.png" alt-text="Terminal öffnen über...":::


Erfahren Sie mehr über das [Klonen von Git-Repositorys in Ihrem Arbeitsbereichsdateisystem](concept-train-model-git-integration.md#clone-git-repositories-into-your-workspace-file-system).

### <a name="copy-and-paste-in-terminal"></a>Kopieren und Einfügen in das Terminal

> * Windows: Drücken Sie `Ctrl-Insert` zum Kopieren und `Ctrl-Shift-v` oder `Shift-Insert` zum Einfügen.
> * Mac OS: Drücken Sie `Cmd-c` zum Kopieren und `Cmd-v` zum Einfügen.
> * FireFox/IE unterstützen die Berechtigungen für die Zwischenablage möglicherweise nicht ordnungsgemäß.

### <a name="share-notebooks-and-other-files"></a>Freigeben von Notebooks und anderen Dateien

Kopieren Sie die URL und fügen Sie sie ein, um ein Notebook oder eine Datei freizugeben.  Nur andere Benutzer des Arbeitsbereichs können auf diese URL zugreifen.  Erfahren Sie mehr über das [Erteilen des Zugriffs auf Ihren Arbeitsbereich](how-to-assign-roles.md).

## <a name="edit-a-notebook"></a>Bearbeiten eines Notebooks

Öffnen Sie zum Bearbeiten eines Notebooks ein beliebiges Notebook, das sich im Abschnitt **Benutzerdateien** Ihres Arbeitsbereichs befindet. Klicken Sie auf die Zelle, die Sie bearbeiten möchten. 

Sie können das Notebook bearbeiten, ohne eine Verbindung zu einer Computeinstanz herzustellen.  Wenn Sie die Zellen im Notebook ausführen möchten, wählen oder erstellen Sie eine Computeinstanz.  Wenn Sie eine beendete Computeinstanz auswählen, wird sie automatisch gestartet, wenn Sie die erste Zelle ausführen.

Wenn eine Compute-Instanz ausgeführt wird, können Sie auch die Codevervollständigung mit [IntelliSense](https://code.visualstudio.com/docs/editor/intellisense) in jedem Python-Notebook verwenden.

Sie können Jupyter oder JupyterLab auch über die Notebook-Symbolleiste starten.  Azure Machine Learning stellt keine Updates und Fehlerbehebungen von Jupyter oder JupyterLab zur Verfügung, da es sich um Open Source-Produkte außerhalb der Zuständigkeit des Microsoft-Supports handelt.

### <a name="focus-mode"></a>Fokusmodus

Verwenden Sie den Fokusmodus, um Ihre aktuelle Ansicht zu erweitern, sodass Sie sich auf Ihre aktiven Registerkarten konzentrieren können. Der Fokusmodus blendet den Datei-Explorer für Notebooks aus.

1. Wählen Sie auf der Symbolleiste des Terminalfensters **Fokusmodus** aus, um den Fokusmodus zu aktivieren. Abhängig von Ihrer Fensterbreite befindet sich diese möglicherweise unter dem Menüpunkt **...** auf Ihrer Symbolleiste.
1. Kehren Sie im Fokusmodus zur Standardansicht zurück, indem Sie **Standardansicht** auswählen.

    :::image type="content" source="media/how-to-run-jupyter-notebooks/focusmode.gif" alt-text="Wechsel zwischen Fokusmodus/Standardansicht":::


### <a name="use-intellisense"></a>Verwenden von IntelliSense

[IntelliSense](https://code.visualstudio.com/docs/editor/intellisense) ist eine hilfreiche Anwendung zur Codevervollständigung mit Features wie: Auflisten von Elementen, Parameterinformationen, QuickInfo und Wort vervollständigen. Diese Features helfen Ihnen, mehr über den von Ihnen verwendeten Code zu erfahren, die von Ihnen eingegebenen Parameter zu verfolgen und Aufrufe von Eigenschaften und Methoden mit nur wenigen Tastatureingaben hinzuzufügen.  

Wenn Sie Code eingeben, verwenden Sie STRG+LEERTASTE, um IntelliSense auszulösen.

### <a name="clean-your-notebook-preview"></a>Bereinigen des Notebooks (Vorschau)

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

### <a name="save-and-checkpoint-a-notebook"></a>Speichern eines Notebooks und Hinzufügen eines Prüfpunkts

Azure Machine Learning erstellt eine Prüfpunktdatei, wenn Sie eine *IPYNB*-Datei erstellen.

Wählen Sie in der Notebook-Symbolleiste das Menü und dann **Datei&gt;Save and Checkpoint** (Speichern und Prüfpunkt), um das Notebook manuell zu speichern. Dadurch wird eine dem Notebook zugeordnete Prüfpunktdatei hinzugefügt.

:::image type="content" source="media/how-to-run-jupyter-notebooks/file-save.png" alt-text="Screenshot des Speichertools auf der Notebook-Symbolleiste":::

Jedes Notebook wird alle 30 Sekunden automatisch gespeichert. Die automatische Speicherung aktualisiert nur die ursprüngliche *IPYNB*-Datei, nicht die Prüfpunktdatei.
 
Wählen Sie **Checkpoints** (Prüfpunkte) im Notebook-Menü aus, um einen benannten Prüfpunkt zu erstellen und das Notebook auf einen gespeicherten Prüfpunkt zurückzusetzen.

## <a name="delete-a-notebook"></a>Löschen eines Notebooks

Sie können die **Beispiel**-Notebooks *nicht* löschen.  Diese Notebooks sind Teil des Studios und werden jedes Mal aktualisiert, wenn ein neues SDK veröffentlicht wird.  

Sie *können* **Benutzerdateien**-Notebooks auf eine der folgenden Arten löschen:

* Wählen Sie im Studio die **...** (Auslassungspunkte) am Ende eines Ordners oder einer Datei aus.  Stellen Sie sicher, dass Sie einen unterstützten Browser (Microsoft Edge, Chrome oder Firefox) verwenden.
* Wählen Sie auf einer beliebigen Notebook-Symbolleiste [**Terminal öffnen**](#terminal) aus, um auf das Terminalfenster für die Compute-Instanz zuzugreifen.
* Entweder in Jupyter oder JupyterLab mit den zugehörigen Tools.

## <a name="run-a-notebook-or-python-script"></a>Ausführen eines Notebooks oder Python-Skripts

Um ein Notebook oder ein Python-Skript auszuführen, stellen Sie zunächst eine Verbindung mit einer aktiven [Compute-Instanz](concept-compute-instance.md) her. Wenn Sie keine Computeinstanz besitzen, verwenden Sie die folgenden Schritte, um eine zu erstellen: 

1. Wählen Sie auf der Notebook- oder Skript-Symbolleiste **+** aus. 
2. Benennen Sie die Computeinstanz, und wählen Sie eine **VM-Größe** aus. 
3. Klicken Sie auf **Erstellen**.
4. Die Compute-Instanz wird automatisch mit der Datei verknüpft.  Sie können jetzt die Notebookzellen oder das Python-Skript mit dem Tool links neben der Compute-Instanz ausführen.

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

### <a name="add-new-kernels"></a>Hinzufügen neuer Kernel

Das Notebook findet automatisch alle Jupyter-Kernel, die auf der verbundenen Compute-Instanz installiert sind.  So fügen Sie einen Kernel zur Computeinstanz hinzu

1. Wählen Sie auf der Notebook-Symbolleiste [**Terminal öffnen**](#terminal) aus.
1. Verwenden Sie das Terminalfenster, um eine neue Umgebung zu erstellen.  Beispielsweise erstellt der folgende Code `newenv`:
    ```shell
    conda create -y --name newenv
    ```
1. Aktivieren Sie die Umgebung.  Beispielsweise nach dem Erstellen von `newenv`:

    ```shell
    conda activate newenv
    ```
1. Installieren Sie das pip- und ipykernel-Paket in der neuen Umgebung, und erstellen Sie einen Kernel für diese conda-Umgebung.

    ```shell
    conda install -y pip
    conda install -y ipykernel
    python -m ipykernel install --user --name newenv --display-name "Python (newenv)"
    ```
1. Nachdem Sie den Kernel installiert haben, aktualisieren Sie die Seite, und öffnen Sie ein Notebook. Der neue Kernel wird nun in der Kernelliste angezeigt.

> [!NOTE]
> Für die Paketverwaltung innerhalb eines Notebooks verwenden Sie die magischen Funktionen **%pip** oder **%conda**, um Pakete automatisch in den **aktuell aktiven Kernel** zu installieren, anstatt **!pip** oder **!conda**, die sich auf alle Pakete beziehen (einschließlich der Pakete außerhalb des aktuell aktiven Kernels).

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

## <a name="shortcut-keys"></a>Tastenkombinationen
Ähnlich wie Jupyter Notebooks verfügen Azure Machine Learning-Notebooks über eine modale Benutzeroberfläche. Mit der Tastatur werden unterschiedliche Aktionen ausgeführt, je nachdem, in welchem Modus sich die Notebook-Zelle befindet. Azure Machine Learning-Notebooks unterstützen die folgenden zwei Modi für eine bestimmte Codezelle: Befehlsmodus und Bearbeitungsmodus.

### <a name="command-mode-shortcuts"></a>Tastenkombinationen im Befehlsmodus

Eine Zelle befindet sich im Befehlsmodus, wenn Sie kein Textcursor zur Eingabe auffordert. Wenn sich eine Zelle im Befehlsmodus befindet, können Sie das Notebook als Ganzes bearbeiten, aber keine Eingaben in einzelne Zellen vornehmen. Sie wechseln in den Befehlsmodus, indem Sie `ESC` drücken oder mit der Maus außerhalb des Editor-Bereichs einer Zelle klicken.  Der linke Rand der aktiven Zelle ist blau und durchgehend, und die zugehörige Schaltfläche **Ausführen** ist blau.

   :::image type="content" source="media/how-to-run-jupyter-notebooks/command-mode.png" alt-text="Notebookzelle im Befehlsmodus ":::

| Verknüpfung                      | Beschreibung                          |
| ----------------------------- | ------------------------------------|
| EINGABETASTE                         | Aktivieren des Bearbeitungsmodus             |        
| UMSCHALT + ENTER                 | Zelle ausführen, Zelle darunter auswählen         |     
| STRG/Befehlstaste + EINGABETASTE       | Run cell (Zelle ausführen)                            |
| ALT+EINGABETASTE                   | Zelle ausführen, Codezelle unten einfügen    |
| STRG/Befehlstaste + ALT + EINGABETASTE | Zelle ausführen, Markdownzelle unten einfügen|
| ALT + R                       | Run all (Alle ausführen)      |                       
| J                             | Zelle in Code konvertieren    |                         
| M                             | Zelle in Markdown konvertieren  |                       
| Nach oben/K                          | Zelle darüber auswählen    |               
| Nach unten/J                        | Zelle darunter auswählen    |               
| Ein                             | Codezelle oben einfügen  |            
| B                             | Codezelle unten einfügen   |           
| STRG/Befehlstaste + UMSCHALT + A   | Markdownzelle oben einfügen    |      
| STRG/Befehlstaste + UMSCHALT + B   | Markdownzelle unten einfügen   |       
| X                             | Ausgewählte Zelle ausschneiden    |               
| C                             | Ausgewählte Zelle kopieren   |               
| UMSCHALT + V                     | Ausgewählte Zelle oben einfügen           |
| V                             | Ausgewählte Zelle unten einfügen    |       
| D D                           | Ausgewählte Zelle löschen|                
| O                             | Toggle Output         |              
| UMSCHALT + O                     | Scrollen der Ausgabe umschalten   |          
| I I                           | Kernel unterbrechen |                   
| 0 0                           | Kernel neu starten |                     
| UMSCHALT + LEERTASTE                 | Bildlauf nach oben  |                         
| LeerZchn                         | Bildlauf nach unten|
| Registerkarte                           | Fokus auf das nächste Element ändern, das den Fokus erhalten kann (bei deaktivierter Fokusinitialisierung)|
| STRG/Befehlstaste + S           | Notebook speichern |                      
| 1                             | In h1 ändern|                       
| 2                             | In h2 ändern|                        
| 3                             | In h3 ändern|                        
| 4                             | In h4 ändern |                       
| 5                             | In h5 ändern |                       
| 6                             | In h6 ändern |                       

### <a name="edit-mode-shortcuts"></a>Tastenkombinationen im Bearbeitungsmodus

Der Bearbeitungsmodus wird durch einen Textcursor angezeigt, der Sie zur Eingabe im Editor-Bereich auffordert. Wenn sich eine Zelle im Bearbeitungsmodus befindet, können Sie etwas in die Zelle eingeben. Sie wechseln in den Bearbeitungsmodus, indem Sie `Enter` drücken oder mit der Maus auf den Editor-Bereich einer Zelle klicken. Der linke Rand der aktiven Zelle ist grün und gestrichelt, und die zugehörige Schaltfläche **Ausführen** ist grün. Außerdem wird im Bearbeitungsmodus der Cursor der Eingabeaufforderung in der Zelle angezeigt.

   :::image type="content" source="media/how-to-run-jupyter-notebooks/edit-mode.png" alt-text="Notebookzelle im Bearbeitungsmodus":::

Mithilfe der folgenden Tastenkombinationen können Sie in Azure Machine Learning-Notebooks einfacher navigieren und Code ausführen, während sie sich im Bearbeitungsmodus befinden.

| Verknüpfung                      | Beschreibung|                                     
| ----------------------------- | ----------------------------------------------- |
| Escape                        | Befehlsmodus aktivieren|  
| STRG/Befehlstaste + LEERTASTE       | Aktivieren von IntelliSense |
| UMSCHALT + ENTER                 | Zelle ausführen, Zelle darunter auswählen |                         
| STRG/Befehlstaste + EINGABETASTE       | Run cell (Zelle ausführen)  |                                      
| ALT+EINGABETASTE                   | Zelle ausführen, Codezelle unten einfügen  |              
| STRG/Befehlstaste + ALT + EINGABETASTE | Zelle ausführen, Markdownzelle unten einfügen  |          
| ALT + R                       | Ausführen aller Zellen     |                              
| Nach oben                            | Cursor nach oben oder in die vorherige Zelle bewegen    |             
| Nach unten                          | Cursor nach unten oder in die nächste Zelle bewegen |                  
| STRG/Befehlstaste + S           | Notebook speichern   |                                
| STRG/Befehlstaste + Nach-oben          | Zum Anfang der Zelle wechseln   |                             
| STRG/Befehlstaste + Nach-unten        | Zum Ende der Zelle wechseln |                                 
| Registerkarte                           | Codevervollständigung oder -einzug (bei aktivierter Fokusinitialisierung) |
| STRG/Befehlstaste + M           | Fokusinitialisierung aktivieren/deaktivieren  |                       
| STRG/Befehlstaste + ]           | Einziehen |                                         
| STRG/Befehlstaste + [           | Einzug entfernen  |                                        
| STRG/Befehlstaste + A           | Alle auswählen|                                      
| STRG/Befehlstaste + Z           | Rückgängig |                                           
| STRG/Befehlstaste + UMSCHALT + Z   | Wiederholen |                                           
| STRG/Befehlstaste + Y           | Wiederholen |                                           
| STRG/Befehlstaste + Pos1        | Zum Anfang der Zelle wechseln|                                
| STRG/Befehlstaste + Ende         | Zum Ende der Zelle wechseln   |                               
| STRG/Befehlstaste + Nach-links        | Ein Wort nach links wechseln |                               
| STRG/Befehlstaste + Nach-rechts       | Ein Wort nach rechts wechseln |                              
| STRG/Befehlstaste + RÜCKTASTE   | Wort davor löschen |                             
| STRG/Befehlstaste + Entf      | Wort danach löschen |                              
| STRG/Befehlstaste + /           | Kommentar in Zelle umschalten

## <a name="find-compute-details"></a>Suchen von Computedetails

Hier finden Sie Details zu ihren Computeinstanzen auf der **Compute**-Seite in [Studio](https://ml.azure.com).

## <a name="troubleshooting"></a>Problembehandlung

* Wenn Sie keine Verbindung mit einem Notebook herstellen können, stellen Sie sicher, dass die WebSocket-Kommunikation **nicht** deaktiviert ist. Damit die Jupyter-Funktionen für Compute-Instanzen eingesetzt werden können, muss die WebSocket-Kommunikation aktiviert sein. Stellen Sie sicher, dass Ihr Netzwerk WebSocket-Verbindungen mit *.instances.azureml.net und *.instances.azureml.ms zulässt. 

* Wenn eine Compute-Instanz in einem Private Link-Arbeitsbereich bereitgestellt wird, kann nur im virtuellen Netzwerk darauf zugegriffen werden. Wenn Sie benutzerdefiniertes DNS oder HOSTS-Dateien verwenden, fügen Sie einen Eintrag für „<Instanzname>.<region>.instances.azureml.ms“ mit der privaten IP-Adresse des privaten Endpunkts des Arbeitsbereichs hinzu. Weitere Informationen finden Sie im Artikel [Benutzerdefiniertes DNS](https://docs.microsoft.com/azure/machine-learning/how-to-custom-dns?tabs=azure-cli).
    
## <a name="next-steps"></a>Nächste Schritte

* [Ausführen Ihres ersten Experiments](tutorial-1st-experiment-sdk-train.md)
* [Sichern des Dateispeichers mit Momentaufnahmen](../storage/files/storage-snapshots-files.md)
* [Arbeiten in sicheren Umgebungen](https://docs.microsoft.com/azure/machine-learning/how-to-secure-training-vnet#compute-instance)
