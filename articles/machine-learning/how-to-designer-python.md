---
title: Python
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie Python in Azure Machine Learning-Designer zur Datentransformation verwenden können.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
author: peterclu
ms.author: peterlu
ms.date: 02/28/2020
ms.openlocfilehash: a2bd9845cd29c7d139e2042f39b4697847639207
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79455790"
---
# <a name="execute-python-code-in-azure-machine-learning-designer"></a>Ausführen von Python-Code in Azure Machine Learning-Designer

In diesem Artikel erfahren Sie, wie Sie das Modul [Ausführen von Python-Skripts](algorithm-module-reference/execute-python-script.md) verwenden, um benutzerdefinierte Logik zum Azure Machine Learning-Designer hinzuzufügen. In der folgenden Schrittanleitung verwenden Sie die Pandas-Bibliothek, um einfache Features zu entwickeln.

Sie können den integrierten Code-Editor verwenden, um schnell einfache Python-Logik hinzuzufügen. Wenn Sie komplexeren Code hinzufügen oder zusätzliche Python-Bibliotheken hochladen möchten, sollten Sie die Methode mit der ZIP-Datei verwenden.

Die standardmäßige Ausführungsumgebung verwendet die Anacondas-Distribution von Python. Eine vollständige Liste der vorinstallierten Pakete finden Sie auf der Seite für die [Modulreferenz zum Ausführen von Python-Skripts](algorithm-module-reference/execute-python-script.md).

![Eingabezuordnung zum Ausführen von Python-Code](media/how-to-designer-python/execute-python-map.png)

## <a name="execute-python-written-in-the-designer"></a>Führen Sie im Designer geschriebenen Python-Code aus.

### <a name="add-the-execute-python-script-module"></a>Fügen Sie das Modul „Ausführen von Python-Skripts“ hinzu.

1. Suchen Sie das Modul **Ausführen von Python-Skripts** in der Designerpalette. Es befindet sich im Abschnitt **Python (Programmiersprache)** .

1. Ziehen Sie das Modul per Drag & Drop auf die Pipelinecanvas.

### <a name="connect-input-datasets"></a>Verbinden von Eingabedatasets

In diesem Artikel wird das Beispieldataset **Automobile Price Data (Raw)** (Automobilpreisdaten (Rohdaten)) verwendet. 

1. Ziehen Sie Ihr Dataset per Drag & Drop auf die Pipelinecanvas.

1. Verbinden Sie den Ausgangsport des Datasets mit dem linken oberen Eingangsport des Moduls **Ausführen von Python-Skripts**. Der Designer stellt die Eingabe dem Einstiegspunktskript als Parameter zur Verfügung.
    
    Der rechte Eingangsport ist für gezippte Python-Bibliotheken reserviert.

    ![Verbinden von Datasets](media/how-to-designer-python/connect-dataset.png)
        

1. Notieren Sie sich, welchen Eingangsport Sie verwenden. Der Designer weist den linken Eingangsport der Variablen `dataset1` und den mittleren Eingangsport zu `dataset2` zu. 

Eingabemodule sind optional, da Sie Daten direkt im Modul **Ausführen von Python-Skripts** generieren oder importieren können.

### <a name="write-your-python-code"></a>Schreiben Sie Ihren Python-Code

Der Designer stellt Ihnen ein erstes Einstiegspunktskript zur Verfügung, mit dem Sie Ihren eigenen Python-Code bearbeiten und eingeben können. 

In diesem Beispiel verwenden Sie Pandas, um zwei Spalten aus dem Automobildataset, **Price** (Preis) und **Horsepower** (PS), zu kombinieren, um eine neue Spalte, **Dollars per horsepower** (US-Dollar pro PS), zu erstellen. Diese Spalte gibt an, wie viel Sie für jedes PS zahlen, was ein nützliches Feature sein könnte, um zu entscheiden, ob ein Auto ein gutes Geschäft für den Preis ist. 

1. Wählen Sie das Modul **Ausführen von Python-Skripts** aus.

1. Wählen Sie im Bereich rechts neben der Canvas das Textfeld **Python-Skript** aus.

1. Kopieren Sie den folgenden Code, und fügen Sie ihn in das Textfeld ein.

    ```python
    import pandas as pd
    
    def azureml_main(dataframe1 = None, dataframe2 = None):
        dataframe1['Dollar/HP'] = dataframe1.price / dataframe1.horsepower
        return dataframe1
    ```
    Ihre Pipeline sollte wie im folgenden Bild aussehen:
    
    ![Ausführen einer Python-Pipeline](media/how-to-designer-python/execute-python-pipeline.png)

    Das Einstiegspunktskript muss die Funktion `azureml_main` enthalten. Es gibt zwei Funktionsparameter, die den beiden Eingangsports für das Modul **Ausführen von Python-Skripts** zugeordnet werden.

    Der Rückgabewert muss ein Pandas-Datenrahmen sein. Sie können bis zu zwei Datenrahmen als Modulausgaben zurückgeben.
    
1. Übermitteln Sie die Pipeline.

Jetzt verfügen Sie über ein Dataset mit dem neuen Feature **Dollars/HP** (US-Dollar/PS), das beim Training eines Empfehlungssystems für Autos nützlich sein könnte. Dies ist ein Beispiel für die Featureextraktion und Verringerung der Dimensionalität. 

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie [ihre eigenen Daten](how-to-designer-import-data.md) in Azure Machine Learning-Designer importieren können.